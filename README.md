--// ============================================================================
--// IMPEL DOWN 1000% ORIGINAL TEST (GPO) - GEFIXT
--// ============================================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local workspaceService = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local localPlayer = player
local var = "HumanoidRootPart"

-- Fake Global Environment für die Dependencies
getgenv().auto_impel = false
getgenv().tween_speed = 1
getgenv().fish_kaitun = { Enabled = false }
getgenv().auto_santa = false
getgenv().baal_farm = false
getgenv().ship_farm = false

local tweenState = { running = false, target = nil, track_object = nil, cancel = false }
local floatHeight = 7
local rayStartHeight = 9000
local smoothness = 0.3
local currentTarget = nil
local tween_connection = nil
local noclipConnection = nil
local zonesReally = { Vector3.new(2951, 2081, -13983) }

--// ============================================================================
--// 1:1 KOPIERTE MOVEMENT & COMBAT DEPENDENCIES
--// ============================================================================

local getMaxSpeed = function()
    currentCharacter = game.Players.LocalPlayer.Character
    if not currentCharacter then
        return 160
    end
    local localPlayer = game:GetService("Players").LocalPlayer
    local playerStatsFolder = game:GetService("ReplicatedStorage"):FindFirstChild("Stats" .. localPlayer.Name)
    if not playerStatsFolder then
        return 160
    end
    local stats = playerStatsFolder:FindFirstChild("Stats")
    if not stats then
        return 160
    end
    local dfValue = stats:FindFirstChild("DF")
    local fightingStyleValue = stats:FindFirstChild("FightingStyle")
    local customizationFolder = playerStatsFolder:FindFirstChild("Customization")
    local raceValue = customizationFolder and customizationFolder:FindFirstChild("Race")
    if not dfValue or not fightingStyleValue or not raceValue then
        return 160
    end
    local maxSpeed = 160
    if dfValue.Value == "Buddha-Buddha" then
        maxSpeed = 160
    elseif fightingStyleValue.Value == "Rokushiki" or fightingStyleValue.Value == "Kamishiki" or currentCharacter:GetAttribute("GEAR2Active") or _G.soru_dashing == true or localPlayer:GetAttribute("soruDash") or currentCharacter:GetAttribute("evading") ~= nil then
        maxSpeed = 160
    elseif currentCharacter:GetAttribute("evading") == true then
        maxSpeed = 160
    elseif dfValue.Value == "Venom-Venom" then
        maxSpeed = 160
    elseif string.sub(string.lower(raceValue.Value), 1, 7) == "fishman" or _G.swimming then
        maxSpeed = 160
    elseif dfValue.Value == "Yomi-Yomi" then
        maxSpeed = 160
    end
    local previousEvasiveTick = currentCharacter:GetAttribute("previousEvasiveTick")
    if previousEvasiveTick and tick() - previousEvasiveTick <= 6 then
        maxSpeed = 78
    end
    local scaledSpeed = maxSpeed * math.clamp(currentCharacter:GetScale(), 1, 20)
    local speedBypass = currentCharacter:GetAttribute("SpeedBypass")
    if speedBypass and speedBypass > 0 then
        scaledSpeed = scaledSpeed + speedBypass
    end
    if scaledSpeed < 46 then
        scaledSpeed = 46
    end
    return scaledSpeed * (getgenv().tween_speed or 1)
end

local getExcludeList = function(characterModel)
    local excludeList = { characterModel }
    for _, otherPlayer in ipairs(Players:GetPlayers()) do
        if otherPlayer.Character then
            table.insert(excludeList, otherPlayer.Character)
        end
    end
    return excludeList
end

local isWithinDistance2 = function(target, distance, horizontalOnly)
    if not player or (not player.Character or not player.Character:FindFirstChild(var)) then
        return false
    end
    local rootPosition = player.Character.HumanoidRootPart.Position
    if typeof(target) ~= "Vector3" then
        if typeof(target) == "CFrame" then
            target = target.Position
        else
            if typeof(target) ~= "Instance" then
                return false
            end
            if target:IsA("Model") and target.PrimaryPart then
                target = target.PrimaryPart.Position
            else
                if not target:IsA("BasePart") then
                    return false
                end
                target = target.Position
            end
        end
    end
    if horizontalOnly then
        local deltaX = rootPosition.X - target.X
        local deltaZ = rootPosition.Z - target.Z
        return math.sqrt(deltaX * deltaX + deltaZ * deltaZ) <= distance
    end
    return (rootPosition - target).Magnitude <= distance
end

local snapToPosition = function(targetPosition)
    player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    player.Character.HumanoidRootPart.Velocity = Vector3.zero
end

tweenTeleportPro = function(target)
    local targetPosition = nil
    local targetInstance = nil
    local trackValue = typeof(target) == "table" and target.Track
    if typeof(target) == "Vector3" then
        targetPosition = target
    elseif typeof(target) == "table" then
        targetPosition = target.Position or target.Target
        if typeof(target.Target) == "Instance" then
            targetInstance = target.Target
        elseif typeof(target.Position) == "Instance" then
            targetInstance = target.Position
        end
    end
    if not targetPosition then
        return
    end
    local resolveTargetPosition = function()
        if targetInstance and targetInstance.Parent then
            if targetInstance:IsA("Model") then
                return targetInstance:GetPivot().Position
            end
            if targetInstance:IsA("BasePart") then
                return targetInstance.Position
            end
            if trackValue and targetInstance:IsA("ValueBase") then
                return targetInstance.Value
            end
        end
        return targetPosition
    end
    if currentTarget and tween_connection then
        local resolvedPosition = resolveTargetPosition()
        if (Vector3.new(resolvedPosition.X, 0, resolvedPosition.Z) - Vector3.new(currentTarget.X, 0, currentTarget.Z)).Magnitude < 1 then
            return
        end
    end
    local shouldContinue = typeof(target) == "table" and target.Stop or function()
        return true
    end
    local fixedY = typeof(target) == "table" and target.Y or nil
    if tween_connection then
        tween_connection:Disconnect()
    end
    if noclipConnection then
        noclipConnection:Disconnect()
    end
    currentTarget = resolveTargetPosition()
    local characterModel = player.Character
    local rootPart = characterModel and characterModel:FindFirstChild(var)
    if not characterModel or not rootPart then
        currentTarget = nil
        return
    end
    local raycastParams = RaycastParams.new()
    raycastParams.FilterType = Enum.RaycastFilterType.Exclude
    raycastParams.IgnoreWater = true
    raycastParams.FilterDescendantsInstances = getExcludeList(characterModel)
    local targetY
    if fixedY then
        targetY = fixedY
    else
        local raycastResult = workspaceService:Raycast(Vector3.new(currentTarget.X, rayStartHeight, currentTarget.Z), Vector3.new(0, -20000, 0), raycastParams)
        targetY = raycastResult and raycastResult.Position.Y + floatHeight or currentTarget.Y + floatHeight
    end
    local startY = math.max(rootPart.Position.Y, targetY, 5)
    rootPart.CFrame = CFrame.new(rootPart.Position.X, startY, rootPart.Position.Z)
    local currentY = startY
    noclipConnection = RunService.Stepped:Connect(function()
        if not characterModel or not characterModel.Parent then
            if noclipConnection then
                noclipConnection:Disconnect()
            end
            return
        end
        rootPart.Velocity = Vector3.zero
        rootPart.AssemblyLinearVelocity = Vector3.zero
        for _, descendant in ipairs(characterModel:GetDescendants()) do
            if descendant:IsA("BasePart") and descendant.CanCollide then
                descendant.CanCollide = false
            end
        end
    end)
    tween_connection = RunService.Heartbeat:Connect(function(deltaTime)
        if not characterModel or (not rootPart or not rootPart.Parent) then
            tween_connection:Disconnect()
            noclipConnection:Disconnect()
            currentTarget = nil
            return
        end
        if not shouldContinue() then
            tween_connection:Disconnect()
            noclipConnection:Disconnect()
            currentTarget = nil
            return
        end
        local resolvedTarget = resolveTargetPosition()
        currentTarget = resolvedTarget
        raycastParams.FilterDescendantsInstances = getExcludeList(characterModel)
        local currentPosition = rootPart.Position
        local offsetVector = Vector3.new(resolvedTarget.X, 0, resolvedTarget.Z) - Vector3.new(currentPosition.X, 0, currentPosition.Z)
        local horizontalDistance = offsetVector.Magnitude
        if horizontalDistance < 8 then
            local landingY = fixedY or resolvedTarget.Y
            if not fixedY then
                local raycastResult = workspaceService:Raycast(Vector3.new(resolvedTarget.X, rayStartHeight, resolvedTarget.Z), Vector3.new(0, -20000, 0), raycastParams)
                if raycastResult then
                    landingY = raycastResult.Position.Y + floatHeight
                end
            end
            local landingHeight = math.max(landingY, 5)
            rootPart.CFrame = CFrame.new(resolvedTarget.X, landingHeight, resolvedTarget.Z)
            tween_connection:Disconnect()
            noclipConnection:Disconnect()
            currentTarget = nil
            return
        end
        local direction = offsetVector.Unit
        local speed = getMaxSpeed() > 0 and getMaxSpeed() - 5 or 110
        local stepDistance = math.min(speed * deltaTime, horizontalDistance)
        local nextX = currentPosition.X + direction.X * stepDistance
        local nextZ = currentPosition.Z + direction.Z * stepDistance
        local nextY
        if fixedY then
            nextY = fixedY
        else
            local rayOrigin = Vector3.new(nextX, rayStartHeight, nextZ)
            local raycastResult = workspaceService:Raycast(rayOrigin, Vector3.new(0, -20000, 0), raycastParams)
            nextY = raycastResult and raycastResult.Position.Y + floatHeight or resolvedTarget.Y + floatHeight
        end
        local targetHeight = math.max(nextY, 5)
        currentY = currentY + (targetHeight - currentY) * (smoothness or 0.25)
        currentY = math.max(currentY, 5)
        rootPart.CFrame = CFrame.new(nextX, currentY, nextZ)
    end)
end

tweenToPos = function(target, speed, flightHeight)
    local moveSpeed = speed or getMaxSpeed() - 15
    local targetPosition = nil
    local trackObject = nil
    if typeof(target) == "Vector3" then
        targetPosition = target
    elseif typeof(target) == "Instance" then
        if target:IsA("ObjectValue") then
            trackObject = target
            if not target.Value then
                return
            end
            targetPosition = target.Value:GetPivot().Position
        else
            trackObject = target
            targetPosition = target:GetPivot().Position
        end
    end
    if not targetPosition then
        return
    end
    if tweenState.running and tweenState.track_object == trackObject and trackObject then
        return
    end
    if tweenState.running and tweenState.target and not trackObject and (Vector3.new(tweenState.target.X, 0, tweenState.target.Z) - Vector3.new(targetPosition.X, 0, targetPosition.Z)).Magnitude < 1 then
        return
    end
    if tweenState.running then
        tweenState.target = targetPosition
        tweenState.track_object = trackObject
        tweenState.cancel = true
        return
    end
    (function()
        local shouldStop = false
        local characterModel = player.Character
        local rootPart = characterModel and characterModel:FindFirstChild(var)
        if not rootPart then
            return
        end
        tweenState.target = targetPosition
        tweenState.track_object = trackObject
        tweenState.running = true
        tweenState.cancel = false
        getgenv().busy = true
        player.Character.Humanoid:UnequipTools()
        local flightY = flightHeight or rootPart.Position.Y + 600
        rootPart.CFrame = CFrame.new(rootPart.Position.X, flightY, rootPart.Position.Z)
        rootPart.Velocity = Vector3.zero
        rootPart.AssemblyLinearVelocity = Vector3.zero
        while true do
            while true do
                while true do
                    while true do
                        moveSpeed = getMaxSpeed() - 15
                        if tweenState.cancel then
                            break
                        end
                        if not getgenv().fish_kaitun.Enabled and not getgenv().auto_santa and not getgenv().auto_impel or getgenv().die then
                            shouldStop = true
                        end
                        if shouldStop then
                            break
                        end
                        if trackObject then
                            if trackObject:IsA("ObjectValue") then
                                if not trackObject.Value then
                                    shouldStop = true
                                end
                                if not shouldStop then
                                    targetPosition = trackObject.Value:GetPivot().Position
                                end
                            else
                                targetPosition = trackObject:GetPivot().Position
                            end
                        end
                        if shouldStop then
                            break
                        end
                        local deltaTime = task.wait()
                        local currentPosition = rootPart.Position
                        local currentXZ = Vector2.new(currentPosition.X, currentPosition.Z)
                        local targetXZ = Vector2.new(targetPosition.X, targetPosition.Z)
                        local distance = (targetXZ - currentXZ).Magnitude
                        if distance <= 5 then
                            rootPart.CFrame = CFrame.new(targetPosition)
                            rootPart.Velocity = Vector3.zero
                            rootPart.AssemblyLinearVelocity = Vector3.zero
                            shouldStop = true
                        end
                        if shouldStop then
                            break
                        end
                        local nextXZ = currentXZ + (targetXZ - currentXZ).Unit * math.min(moveSpeed * deltaTime, distance)
                        rootPart.CFrame = CFrame.new(nextXZ.X, flightY, nextXZ.Y)
                        rootPart.Velocity = Vector3.zero
                        rootPart.AssemblyLinearVelocity = Vector3.zero
                    end
                    if shouldStop then
                        break
                    end
                    if shouldStop then
                        break
                    end
                    if shouldStop then
                        break
                    end
                    tweenState.cancel = false
                    trackObject = tweenState.track_object
                    if trackObject then
                        break
                    end
                    targetPosition = tweenState.target
                end
                if shouldStop then
                    break
                end
                if shouldStop then
                    break
                end
                if shouldStop then
                    break
                end
                if trackObject:IsA("ObjectValue") then
                    break
                end
                targetPosition = trackObject:GetPivot().Position
            end
            if shouldStop then
                break
            end
            if shouldStop then
                break
            end
            if shouldStop then
                break
            end
            if not trackObject.Value then
                break
            end
            targetPosition = trackObject.Value:GetPivot().Position
        end
        shouldStop = false
        getgenv().busy = false
        tweenState.running = false
        tweenState.target = nil
        tweenState.track_object = nil
    end)()
end

tweenToPos2 = function(target, speed, flightHeight, maxHeight, usePathfinding, showWaypoints)
    local moveSpeed = speed or getMaxSpeed() - 15
    local rayUpOffset = flightHeight or 150
    local maxFlightY = maxHeight or 500
    if usePathfinding == nil then
        usePathfinding = true
    end
    if showWaypoints == nil then
        showWaypoints = true
    end
    local targetPosition = nil
    local trackObject = nil
    if typeof(target) == "Vector3" then
        targetPosition = target
    elseif typeof(target) == "Instance" then
        if target:IsA("ObjectValue") then
            trackObject = target
            if not target.Value then
                return
            end
            targetPosition = target.Value:GetPivot().Position
        else
            trackObject = target
            targetPosition = target:GetPivot().Position
        end
    end
    if not targetPosition then
        return
    end
    if tweenState.running and tweenState.track_object == trackObject and trackObject then
        return
    end
    if tweenState.running and tweenState.target and not trackObject and (Vector3.new(tweenState.target.X, 0, tweenState.target.Z) - Vector3.new(targetPosition.X, 0, targetPosition.Z)).Magnitude < 1 then
        return
    end
    if tweenState.running then
        tweenState.target = targetPosition
        tweenState.track_object = trackObject
        tweenState.cancel = true
        return
    end
    (function()
        local shouldStop = false
        local characterModel = player.Character
        local rootPart = characterModel and characterModel:FindFirstChild(var)
        local humanoid = characterModel and characterModel:FindFirstChild("Humanoid")
        if not rootPart or not humanoid then
            return
        end
        tweenState.target = targetPosition
        tweenState.track_object = trackObject
        tweenState.running = true
        tweenState.cancel = false
        getgenv().busy = true
        humanoid:UnequipTools()
        local pathfindingService = game:GetService("PathfindingService")
        local raycastParams = RaycastParams.new()
        raycastParams.FilterType = Enum.RaycastFilterType.Exclude
        raycastParams.FilterDescendantsInstances = { characterModel }
        local flightY = rootPart.Position.Y
        local verticalVelocity = 0
        local waypoints = {}
        local waypointIndex = 1
        local lastPathTarget = nil
        local timeSinceRepath = 0
        local visualParts = {}
        local clearVisuals = function()
            for _, part in ipairs(visualParts) do
                if part and part.Parent then
                    part:Destroy()
                end
            end
            visualParts = {}
        end
        local drawWaypoints = function(waypointList)
            clearVisuals()
            if not showWaypoints then
                return
            end
            for index, waypoint in ipairs(waypointList) do
                if index ~= 1 then
                    local waypointPart = Instance.new("Part")
                    waypointPart.Name = "WaypointVisual_" .. index
                    waypointPart.Anchored = true
                    waypointPart.CanCollide = false
                    waypointPart.CanQuery = false
                    waypointPart.CanTouch = false
                    waypointPart.Size = Vector3.new(1, 1, 1)
                    waypointPart.Shape = Enum.PartType.Ball
                    waypointPart.Material = Enum.Material.Neon
                    waypointPart.Color = waypoint.Action == Enum.PathWaypointAction.Jump and Color3.fromRGB(255, 100, 100) or Color3.fromRGB(100, 255, 100)
                    waypointPart.Position = waypoint.Position
                    waypointPart.Transparency = 0.3
                    waypointPart.Parent = workspace
                    table.insert(visualParts, waypointPart)
                    if index > 2 then
                        local previousWaypoint = waypointList[index - 1]
                        local segmentLength = (waypoint.Position - previousWaypoint.Position).Magnitude
                        local linePart = Instance.new("Part")
                        linePart.Name = "WaypointLine_" .. index
                        linePart.Anchored = true
                        linePart.CanCollide = false
                        linePart.CanQuery = false
                        linePart.CanTouch = false
                        linePart.Size = Vector3.new(0.2, 0.2, segmentLength)
                        linePart.Material = Enum.Material.Neon
                        linePart.Color = Color3.fromRGB(100, 200, 255)
                        linePart.CFrame = CFrame.lookAt(previousWaypoint.Position, waypoint.Position) * CFrame.new(0, 0, -segmentLength / 2)
                        linePart.Transparency = 0.5
                        linePart.Parent = workspace
                        table.insert(visualParts, linePart)
                    end
                end
            end
        end
        local findGroundY = function(positionXZ, baseY)
            local rayStartY = math.max(baseY + rayUpOffset, maxFlightY + rayUpOffset)
            local rayOrigin = Vector3.new(positionXZ.X, rayStartY, positionXZ.Y)
            local rayDirection = Vector3.new(0, -(rayStartY + 1000), 0)
            local raycastResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
            if raycastResult then
                local groundY = raycastResult.Position.Y + 3
                if groundY <= maxFlightY then
                    return groundY
                end
                local retryStartY = raycastResult.Position.Y - 1
                local retryOrigin = Vector3.new(positionXZ.X, retryStartY, positionXZ.Y)
                local retryResult = workspace:Raycast(retryOrigin, Vector3.new(0, -2000, 0), raycastParams)
                if retryResult then
                    local retryGroundY = retryResult.Position.Y + 3
                    if retryGroundY <= maxFlightY then
                        return retryGroundY
                    end
                end
            end
            return nil
        end
        local computePath = function(startPosition, endPosition)
            local path = pathfindingService:CreatePath({ AgentRadius = 2, AgentHeight = 5, AgentCanJump = true, AgentCanClimb = false, WaypointSpacing = 4 })
            local computeSuccess, computeError = pcall(function()
                path:ComputeAsync(startPosition, endPosition)
            end)
            if computeSuccess and path.Status == Enum.PathStatus.Success then
                return path:GetWaypoints()
            end
            return nil
        end
        local shouldRepath = function(targetPosition)
            if not usePathfinding then
                return false
            end
            if not lastPathTarget then
                return true
            end
            if (targetPosition - lastPathTarget).Magnitude > 10 then
                return true
            end
            if #waypoints == 0 then
                return true
            end
            if waypointIndex < #waypoints then
                return true
            end
            return false
        end
        local targetFlightY = flightY
        while true do
            while true do
                moveSpeed = getMaxSpeed() - 15
                if tweenState.cancel then
                    break
                end
                if not getgenv().fish_kaitun.Enabled and not getgenv().auto_santa and not getgenv().auto_impel then
                    shouldStop = true
                end
                if shouldStop then
                    break
                end
                if trackObject then
                    if trackObject:IsA("ObjectValue") then
                        if not trackObject.Value then
                            shouldStop = true
                        end
                        if not shouldStop then
                            targetPosition = trackObject.Value:GetPivot().Position
                        end
                    else
                        targetPosition = trackObject:GetPivot().Position
                    end
                end
                if shouldStop then
                    break
                end
                local deltaTime = task.wait()
                timeSinceRepath = timeSinceRepath + deltaTime
                local currentPosition = rootPart.Position
                if (Vector3.new(targetPosition.X, 0, targetPosition.Z) - Vector3.new(currentPosition.X, 0, currentPosition.Z)).Magnitude <= 5 then
                    rootPart.CFrame = CFrame.new(targetPosition)
                    rootPart.Velocity = Vector3.zero
                    rootPart.AssemblyLinearVelocity = Vector3.zero
                    shouldStop = true
                end
                if shouldStop then
                    break
                end
                if usePathfinding and (shouldRepath(targetPosition) or timeSinceRepath > 1) then
                    local newWaypoints = computePath(currentPosition, targetPosition)
                    if newWaypoints and #newWaypoints > 1 then
                        waypoints = newWaypoints
                        waypointIndex = 2
                        lastPathTarget = targetPosition
                        timeSinceRepath = 0
                        drawWaypoints(waypoints)
                    else
                        waypoints = {}
                        waypointIndex = 1
                        clearVisuals()
                    end
                end
                local moveTarget
                if usePathfinding and #waypoints > 0 and waypointIndex <= #waypoints then
                    moveTarget = waypoints[waypointIndex].Position
                    if (Vector3.new(moveTarget.X, 0, moveTarget.Z) - Vector3.new(currentPosition.X, 0, currentPosition.Z)).Magnitude <= 3 then
                        if showWaypoints and visualParts[waypointIndex - 1] then
                            visualParts[waypointIndex - 1].Color = Color3.fromRGB(150, 150, 150)
                            visualParts[waypointIndex - 1].Transparency = 0.7
                        end
                        waypointIndex = waypointIndex + 1
                        if #waypoints < waypointIndex then
                            moveTarget = targetPosition
                        else
                            moveTarget = waypoints[waypointIndex].Position
                        end
                    end
                else
                    moveTarget = targetPosition
                end
                local currentXZ = Vector2.new(currentPosition.X, currentPosition.Z)
                local deltaXZ = Vector2.new(moveTarget.X, moveTarget.Z) - currentXZ
                local distance = deltaXZ.Magnitude
                if not (distance < 0.1) then
                    local direction = deltaXZ.Unit
                    local nextXZ = currentXZ + direction * math.min(moveSpeed * deltaTime, distance)
                    local groundY = findGroundY(currentXZ + direction * math.min(moveSpeed * 0.25, math.max(distance, 0.1)), flightY)
                    if groundY then
                        targetFlightY = groundY
                    end
                    verticalVelocity = verticalVelocity + ((targetFlightY - flightY) * 200 - verticalVelocity * 25) * deltaTime
                    flightY = flightY + verticalVelocity * deltaTime
                    if maxFlightY < flightY then
                        flightY = maxFlightY
                        if verticalVelocity > 0 then
                            verticalVelocity = 0
                        end
                    end
                    local lookDirection = Vector3.new(direction.X, 0, direction.Y)
                    rootPart.CFrame = CFrame.lookAt(Vector3.new(nextXZ.X, flightY, nextXZ.Y), Vector3.new(nextXZ.X, flightY, nextXZ.Y) + lookDirection)
                    rootPart.Velocity = Vector3.zero
                    rootPart.AssemblyLinearVelocity = Vector3.zero
                end
            end
            if not shouldStop then
                if not shouldStop then
                    if not shouldStop then
                        tweenState.cancel = false
                        trackObject = tweenState.track_object
                    end
                end
            end
            if shouldStop or (shouldStop or (shouldStop or not not trackObject)) then
                if shouldStop or (shouldStop or (shouldStop or not not trackObject:IsA("ObjectValue"))) then
                    if shouldStop or (shouldStop or (shouldStop or not trackObject.Value)) then
                        shouldStop = false
                        clearVisuals()
                        getgenv().busy = false
                        tweenState.running = false
                        tweenState.target = nil
                        tweenState.track_object = nil
                        return
                    end
                    targetPosition = trackObject.Value:GetPivot().Position
                else
                    targetPosition = trackObject:GetPivot().Position
                end
            else
                targetPosition = tweenState.target
            end
            waypoints = {}
            waypointIndex = 1
            lastPathTarget = nil
            clearVisuals()
        end
    end)()
end

local fakeGeppo = function()
    local stats_folder = game:GetService("ReplicatedStorage"):FindFirstChild("Stats" .. game.Players.LocalPlayer.Name)
    if not stats_folder then return end
    if stats_folder.Skills:FindFirstChild("skyWalk") and stats_folder.Skills:FindFirstChild("skyWalk").Value == true then
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer("Sky Walk2", { char = game.Players.LocalPlayer.Character, cf = CFrame.new(-3065.980712890625, 200.7225799560547, -11824.01171875, 0, 1, -0.0, 0, -0.0, -1, -1, 0, -0.0) })
    else
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer("Geppo", { char = game.Players.LocalPlayer.Character, cf = CFrame.new(-3065.980712890625, 200.7225799560547, -11824.01171875, 0, 1, -0.0, 0, -0.0, -1, -1, 0, -0.0) })
    end
end

getgenv().fighting_style = "Auto"
getgenv().get_style = function()
    char = player.Character
    if getgenv().fighting_style == "Auto" then
        for _, tool in ipairs(char:GetChildren()) do
            if tool:FindFirstChild("SwordEquip") then
                return tool, "Sword"
            end
        end
        for _, tool in ipairs(localPlayer.Backpack:GetChildren()) do
            if tool:FindFirstChild("SwordEquip") then
                tool.Parent = char
                return tool, "Sword"
            end
        end
        for _, tool in ipairs(char:GetChildren()) do
            if tool:GetAttribute("MeleeTool") then
                return tool, "Melee"
            end
        end
        for _, tool in ipairs(localPlayer.Backpack:GetChildren()) do
            if tool:GetAttribute("MeleeTool") then
                tool.Parent = char
                return tool, "Melee"
            end
        end
    elseif getgenv().fighting_style == "Sword" then
        for _, tool in ipairs(char:GetChildren()) do
            if tool:FindFirstChild("SwordEquip") then
                return tool, "Sword"
            end
        end
        for _, tool in ipairs(localPlayer.Backpack:GetChildren()) do
            if tool:FindFirstChild("SwordEquip") then
                tool.Parent = char
                return tool, "Sword"
            end
        end
    elseif getgenv().fighting_style == "Melee" then
        for _, tool in ipairs(char:GetChildren()) do
            if tool:GetAttribute("MeleeTool") then
                return tool, "Melee"
            end
        end
        for _, tool in ipairs(localPlayer.Backpack:GetChildren()) do
            if tool:GetAttribute("MeleeTool") then
                tool.Parent = char
                return tool, "Melee"
            end
        end
    end
    return
end

getgenv().combat_state = { Combo = 1, NextAttack = 0, LastHitTime = 0, LastTarget = nil }
getgenv().global_hit = function(targets)
    if player.Character.HumanoidRootPart:FindFirstChild("knockback") then
        return
    end
    local combatState = getgenv().combat_state
    local now = tick()
    local characterModel = game.Players.LocalPlayer.Character
    if not characterModel then
        return
    end
    local humanoid = characterModel:FindFirstChild("Humanoid")
    local rootPart = characterModel:FindFirstChild(var)
    if not humanoid or humanoid.Health <= 0 or not rootPart then
        combatState.Combo = 1
        return
    end
    if now - (combatState.LastHitTime or 0) > 2.5 then
        combatState.Combo = 1
        combatState.LastTarget = nil
    end
    if now < (combatState.NextAttack or 0) then
        return
    end
    local findHumanoid = function(instance)
        if not instance then
            return nil
        end
        if instance:IsA("BasePart") then
            instance = instance.Parent
        end
        while instance do
            if instance:IsA("Model") then
                local humanoid = instance:FindFirstChild("Humanoid")
                if humanoid then
                    return humanoid
                end
            end
            instance = instance.Parent
        end
        return nil
    end
    local resolveHitPart = function(instance)
        if not instance then
            return nil
        end
        if instance:IsA("BasePart") then
            return instance
        end
        if instance:IsA("Model") then
            if instance:FindFirstChild(var) then
                return instance.HumanoidRootPart
            end
            if instance.PrimaryPart then
                return instance.PrimaryPart
            end
            return instance:FindFirstChildWhichIsA("BasePart", true)
        end
        return nil
    end
    local targetParts = {}
    local targetHumanoid = nil
    if typeof(targets) == "table" then
        for _, target in next, targets, nil do
            local hitPart = resolveHitPart(target)
            if hitPart then
                table.insert(targetParts, hitPart)
                if not targetHumanoid then
                    targetHumanoid = findHumanoid(hitPart)
                end
            end
        end
    else
        local hitPart = resolveHitPart(targets)
        if hitPart then
            table.insert(targetParts, hitPart)
            targetHumanoid = findHumanoid(hitPart)
        end
    end
    if #targetParts == 0 then
        return
    end
    if targetHumanoid and combatState.LastTarget and targetHumanoid ~= combatState.LastTarget then
        combatState.Combo = 1
    end
    if targetHumanoid and targetHumanoid.Health <= 0 then
        combatState.Combo = 1
    end
    combatState.LastTarget = targetHumanoid
    local tool, styleName = getgenv().get_style()
    if tool and tool.Parent ~= characterModel then
        characterModel.Humanoid:EquipTool(tool)
    end
    local fightingStyle = typeof(styleName) == "string" and styleName or "Melee"
    local combo = combatState.Combo or 1
    local ground = "Ground"
    local rootCFrame = rootPart.CFrame
    local combatRegister = game:GetService("ReplicatedStorage").Events.CombatRegister
    combatState.LastHitTime = now
    local attackDelay = 0.35
    if combo == 3 then
        attackDelay = 0.4
    elseif combo == 4 then
        attackDelay = 0.8
    elseif combo == 5 then
        attackDelay = 1.7
    end
    combatState.NextAttack = now + attackDelay
    task.spawn(function()
        pcall(function()
            combatRegister:InvokeServer({ "swingsfx", fightingStyle, combo, ground, false, nil, 1.75, 2, rootCFrame })
        end)
        task.wait(0.15)
        pcall(function()
            combatRegister:InvokeServer({ "damage", targetParts, fightingStyle, { combo, ground, fightingStyle }, true, rootCFrame, aircombo = ground })
        end)
    end)
    combatState.Combo = combo + 1
    if combatState.Combo > 5 then
        combatState.Combo = 1
    end
end

local lootChest = function()
    local closestChest = nil
    local closestDistance = math.huge
    for _, effect in next, workspaceService.Effects:GetChildren() do
        if effect:FindFirstChildWhichIsA("ProximityPrompt", true) and effect:IsA("Model") and effect:FindFirstChildWhichIsA("ProximityPrompt", true).Parent:IsA("MeshPart") then
            local distance2 = (effect:GetPivot().Position - player.Character:GetPivot().Position).Magnitude
            if distance2 < closestDistance then
                closestChest = effect
                closestDistance = distance2
            end
        end
    end
    if closestChest then
        if not isWithinDistance2(closestChest:GetPivot().Position, 10, true) then
            tweenToPos2(closestChest:GetPivot().Position, 50, 30, 2090)
        else
            local prompt = closestChest:FindFirstChild("ProximityPrompt", true)
            local currentCamera = workspaceService.CurrentCamera
            local rootPart = game.Players.LocalPlayer.Character.HumanoidRootPart
            local promptPosition = prompt.Parent.Position
            local savedCameraCFrame = currentCamera.CFrame
            rootPart.CFrame = CFrame.new(promptPosition.X, promptPosition.Y + 5, promptPosition.Z) * CFrame.Angles(-math.pi / 2, 0, 0)
            currentCamera.CameraSubject = prompt.Parent
            currentCamera.CFrame = CFrame.lookAt(rootPart.Position, promptPosition)
            task.wait()
            prompt:InputHoldBegin()
            task.wait(prompt.HoldDuration + 0.1)
            prompt:InputHoldEnd()
            currentCamera.CameraSubject = game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
            currentCamera.CFrame = savedCameraCFrame
        end
    end
end

local getStat = function(statName)
    local amount = 0
    for _, statFrame in next, player.PlayerGui.Statistics.Main.Stats:GetChildren() do
        local statLabel = statFrame:FindFirstChild("Stat", true)
        local amountLabel = statFrame:FindFirstChild("Amount", true)
        if statLabel and amountLabel and statLabel.Text == statName then
            amount = tonumber(amountLabel.Text)
        end
    end
    return amount
end

local getCurrentFloor = function()
    local floor = 1
    for _, timer in next, game:GetService("Players").LocalPlayer.PlayerGui.ImpelDownUI.Info.Timers:GetChildren() do
        if timer.Name:match("Floor") then
            local floorDigits = timer.Name:match("%d+")
            if floorDigits then
                floor = tonumber(floorDigits)
                return floor
            end
        end
    end
    return floor
end

local hasZone = function()
    for _, npc in next, workspaceService.NPCs:GetChildren() do
        if npc:IsA("Model") and npc:FindFirstChild("Humanoid") then
            for _, zone in next, zonesReally, nil do
                local targetDistance = (npc:GetPivot().Position - zone).Magnitude
                if targetDistance < 150 then
                    return zone
                end
            end
        end
    end
    return nil
end

local putMines = function()
    local minesRemote = ReplicatedStorage:FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Mines")
    if not minesRemote then
        task.spawn(function()
            ReplicatedStorage:FindFirstChild("Events"):FindFirstChild("Skill"):InvokeServer("Explosive Mines")
        end)
    else
        task.wait(1.5)
        minesRemote:InvokeServer({ cf = player.Character.HumanoidRootPart.CFrame })
    end
end

local HasRange = function()
    return
end


--// ============================================================================
--// IMPEL DOWN 1000% ORIGINAL LOGIC (MIT FIX)
--// ============================================================================

local function ImpelDownFarm(enabled)
    getgenv().auto_impel = enabled
    if not enabled then return end

    task.spawn(function()
        while getgenv().auto_impel and task.wait(1) do
            fakeGeppo()
            if player.Character:FindFirstChild("Bomb-Bomb") or player.Backpack:FindFirstChild("Bomb-Bomb") then
                if getStat("Devil Fruit") < 700 then
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("stats"):FireServer(unpack({ "DevilFruitMastery", nil, 700 }))
                end
                local leapRemote = game:GetService("ReplicatedStorage"):FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Leap")
                if not leapRemote then
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer(unpack({ "Explosive Leap" }))
                else
                    leapRemote:FireServer({ cf = CFrame.new(0, 0, 0), extra = 0 })
                end
            end
        end
    end)
    
    task.spawn(function()
        while getgenv().auto_impel and task.wait() do
            if game.PlaceId ~= 11424731604 then
                local impelEntrance = Vector3.new(5878, 9, -10214)
                if not isWithinDistance2(impelEntrance, 10, true) then
                    tweenTeleportPro({
                        Position = impelEntrance,
                        Speed = 50,
                        Stop = function()
                            return getgenv().auto_impel
                        end
                    })
                else
                    snapToPosition(impelEntrance)
                end
            elseif not player.PlayerGui:FindFirstChild("diffChooser") then
                
                if getCurrentFloor() == 1 then
                    -- 1. PRIORITÄT: Handschellen loswerden
                    if player.Character:FindFirstChild("LeftCuff") then
                        local keyModel = workspaceService.Effects:FindFirstChild("Key")
                        if keyModel then
                            if not isWithinDistance2(keyModel:GetPivot().Position, 10, true) then
                                tweenTeleportPro({
                                    Position = keyModel:GetPivot().Position,
                                    Y = keyModel:GetPivot().Position.Y, -- GEFIXT: Großes Y
                                    Speed = 50,
                                    Stop = function()
                                        return getgenv().auto_impel
                                    end
                                })
                            else
                                snapToPosition(keyModel:GetPivot().Position)
                                local prompt = keyModel:FindFirstChild("ProximityPrompt", true)
                                task.wait()
                                prompt:InputHoldBegin()
                                task.wait(prompt.HoldDuration)
                                prompt:InputHoldEnd()
                                task.wait()
                            end
                        end
                        
                    -- 2. PRIORITÄT: Bombe holen / essen
                    elseif not player.Backpack:FindFirstChild("Bomb-Bomb") then
                        if not player.Character:FindFirstChild("Bomb") then
                            if player.Backpack:FindFirstChild("Bomb") then
                                player.Backpack:FindFirstChild("Bomb").Parent = player.Character
                                task.wait(1)
                            elseif workspaceService.Effects:FindFirstChild("Bomb") then
                                if not isWithinDistance2(workspaceService.Effects:FindFirstChild("Bomb"):GetPivot().Position, 10, true) then
                                    tweenToPos2(workspaceService.Effects:FindFirstChild("Bomb"):GetPivot().Position, 50, 30, 2090)
                                else
                                    local bombPrompt = workspaceService.Effects:FindFirstChild("Bomb"):FindFirstChild("ProximityPrompt", true)
                                    bombPrompt:InputHoldBegin()
                                    task.wait(bombPrompt.HoldDuration + 0.1)
                                    bombPrompt:InputHoldEnd()
                                end
                            else
                                lootChest()
                            end
                        elseif not player.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                            player.Character:FindFirstChild("Bomb"):Activate()
                        else
                            firesignal(game:GetService("Players").LocalPlayer.PlayerGui.ConfirmationPrompt.Main.OptionsFrame.Accept.MouseButton1Click)
                        end
                        
                    -- 3. PRIORITÄT: Vera farmen (Boss)
                    elseif isWithinDistance2(Vector3.new(5906, 9, -10197), 300, true) then
                        local vera = workspaceService.NPCs:FindFirstChild("Vera")
                        if vera and not isWithinDistance2(vera:GetPivot().Position, 10, true) then
                            tweenTeleportPro({
                                Position = vera:GetPivot().Position,
                                Speed = 50,
                                Stop = function()
                                    return getgenv().auto_impel
                                end
                            })
                        elseif vera and isWithinDistance2(vera:GetPivot().Position, 10, true) then
                            snapToPosition(vera:GetPivot().Position)
                            task.spawn(function()
                                getgenv().global_hit(vera)
                            end)
                        end
                        
                    -- 4. PRIORITÄT: Weiterlaufen und Zonen einnehmen
                    else
                        local startBarrier = workspaceService.Islands["Impel Base - Floor 1"].Barriers:FindFirstChild("StartBarrier")
                        if startBarrier and startBarrier.CanCollide then
                            tweenToPos2(startBarrier.Position, 50, 30, 2090, false)
                        end
                        if hasZone() then
                            if not isWithinDistance2(hasZone(), 10, true) then
                                tweenToPos2(hasZone(), 50, 30, 2090, false)
                            else
                                snapToPosition(hasZone() + Vector3.new(0, 4, 0))
                                putMines()
                            end
                        else
                            local zoneOffset = Vector3.new(0, 0, -40)
                            if workspaceService.Effects.Zones:FindFirstChild("End") then
                                local endPosition = workspaceService.Effects.Zones:FindFirstChild("End").Position + zoneOffset
                                if not isWithinDistance2(endPosition, 10, true) then
                                    tweenToPos2(endPosition, 50, 3000)
                                else
                                    snapToPosition(endPosition + Vector3.new(0, 4, 0))
                                    putMines()
                                end
                            else
                                tweenToPos2(Vector3.new(2880, 2783, -14473) + zoneOffset, 50, 3000)
                            end
                        end
                    end
                elseif getCurrentFloor() == 2 then
                    if isWithinDistance2(Vector3.new(3199, 2405, -20056), 50, true) then
                        tweenToPos(Vector3.new(3201, 2384, -20276))
                    end
                    if not HasRange() then
                    end
                end
            end
        end
    end)
end


--// ============================================================================
--// SIMPLE DRAGGABLE TEST GUI
--// ============================================================================

local testGui = Instance.new("ScreenGui")
testGui.Name = "ImpelOriginal1000Test"
testGui.ResetOnSpawn = false
pcall(function() testGui.Parent = CoreGui end)

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 200, 0, 100)
mainFrame.Position = UDim2.new(0.5, -100, 0.8, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true 
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 8)
mainFrame.Parent = testGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "IMPEL DOWN 1:1 TEST"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.Parent = mainFrame

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0.8, 0, 0, 40)
toggleBtn.Position = UDim2.new(0.1, 0, 0, 45)
toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
toggleBtn.Text = "START IMPEL FARM"
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 14
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(0, 6)
toggleBtn.Parent = mainFrame

toggleBtn.MouseButton1Click:Connect(function()
    if getgenv().auto_impel then
        toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        toggleBtn.Text = "START IMPEL FARM"
        ImpelDownFarm(false)
    else
        toggleBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        toggleBtn.Text = "STOP IMPEL FARM"
        ImpelDownFarm(true)
    end
end)

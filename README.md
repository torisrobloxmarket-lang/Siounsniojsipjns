--// ==========================================
--// RYU HUB - TJS EDITION (FULL LOGIC INTEGRATION)
--// 100% MONOCHROME CLEAN TEMPLATE (FIXED NOTIFICATIONS)
--// ==========================================

local LPH_NO_VIRTUALIZE = function(f) return f end
if getgenv and getgenv().LPH_NO_VIRTUALIZE then
    LPH_NO_VIRTUALIZE = getgenv().LPH_NO_VIRTUALIZE
end

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local PathfindingService = game:GetService("PathfindingService")
local StarterGui = game:GetService("StarterGui")

local LocalPlayer = Players.LocalPlayer
local Player = LocalPlayer
local Mouse = Player:GetMouse()
local camera = Workspace.CurrentCamera

--// NATIVE NOTIFICATION SYSTEM (FIX FÜR DEN NIL VALUE ERROR)
local function SendNotification(title, text, duration)
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = title or "Ryu Hub",
            Text = text or "",
            Duration = duration or 3
        })
    end)
end

--// GUI PARENT RESOLVER
local guiParent
pcall(function()
    if type(gethui) == "function" then guiParent = gethui()
    elseif syn and syn.protect_gui then guiParent = CoreGui end
end)
if not guiParent then guiParent = LocalPlayer:WaitForChild("PlayerGui") end
for _, v in pairs(guiParent:GetChildren()) do if v.Name == "RyuHubUI" then v:Destroy() end end

--// THEME
local Theme = {
    Background = Color3.fromRGB(15, 15, 15),
    Sidebar = Color3.fromRGB(22, 22, 22),
    SectionBG = Color3.fromRGB(30, 30, 30),
    Text = Color3.fromRGB(255, 255, 255),
    SubText = Color3.fromRGB(150, 150, 150),
    Accent = Color3.fromRGB(255, 50, 50),
    ToggleOff = Color3.fromRGB(45, 45, 45),
    ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(60, 60, 60)
}

--// ==========================================
--// RYU CONFIG & STATE (WIRED TO GUI)
--// ==========================================
local RyuConfig = {
    BlackFlashEnabled = false,
    CameraLockEnabled = true,
    DashDistance = 15,
    FireDelay = 0.25,
    DashDuration = 0.35,
    LockTime = 0.1,
    DashEasingStyle = Enum.EasingStyle.Cubic,
    DashEasingDirection = Enum.EasingDirection.Out,
    
    ESPBox = false, ESPBoxColor = Color3.fromRGB(255, 65, 65),
    ESPCorner = false, ESPCornerColor = Color3.fromRGB(0, 255, 255),
    ESPOutline = false, ESPOutlineColor = Color3.fromRGB(255, 255, 255),
    ESPSkeleton = false, ESPSkeletonColor = Color3.fromRGB(190, 90, 255),
    ESPHeadDot = false, ESPHeadDotColor = Color3.fromRGB(255, 255, 0),
    ESPTracer = false, ESPTracerColor = Color3.fromRGB(85, 255, 85),
    ESPName = false, ESPNameColor = Color3.fromRGB(255, 255, 255),
    ESPCharacter = false, ESPCharacterColor = Color3.fromRGB(180, 200, 255),
    ESPDistance = false, ESPDistanceColor = Color3.fromRGB(120, 200, 255),
    ESPKill = false, ESPKillColor = Color3.fromRGB(255, 170, 0),
    ESPHPText = false, ESPHPBar = false, CooldownRevealer = false,

    LockKeybind = Enum.KeyCode.C,
    LockSpecificPlayer = "Auto",
    
    TeleportMode = "Tween",
    MinPlayersFilter = 0,
    MaxPingFilter = 999
}

local Logic = {
    TIME_WINDOW = 2, LastFiredTick = 0,
    TargetAnimations = { ["100962226150441"] = true, ["95852624447551"] = true, ["74145636023952"] = true, ["123171106092050"] = true },
    DashAnimLeft = Instance.new("Animation"), DashAnimRight = Instance.new("Animation"),
    DashAssistState = { Enabled = false, CameraLock = false, OnlyIfFacing = false, DetectionRange = 15, BehindDistance = 5, FlightDuration = 0.42, CurveStrength = 10, ArchHeight = 3, LockDuration = 0.35, DashAssistKeybind = Enum.KeyCode.J },
    Pathfinding = { Active = false, Speed = 350, VisualizeOn = true, CurrentTween = nil, VisualParts = {}, VisualFolder = nil },
    TargetRemote = nil, ESPObjects = {}, Connections = {}, ServerMap = {}, ServerList = {}, SelectedServerData = nil,
    ShadowColor = Color3.fromRGB(0, 0, 0),
    CooldownGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(20, 255, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 177, 255)) }),
    R15Bones = { {"Head","UpperTorso"}, {"UpperTorso","LowerTorso"}, {"UpperTorso","LeftUpperArm"}, {"LeftUpperArm","LeftLowerArm"}, {"LeftLowerArm","LeftHand"}, {"UpperTorso","RightUpperArm"}, {"RightUpperArm","RightLowerArm"},{"RightLowerArm","RightHand"}, {"LowerTorso","LeftUpperLeg"}, {"LeftUpperLeg","LeftLowerLeg"}, {"LeftLowerLeg","LeftFoot"}, {"LowerTorso","RightUpperLeg"}, {"RightUpperLeg","RightLowerLeg"},{"RightLowerLeg","RightFoot"} },
    R6Bones = { {"Head","Torso"}, {"Torso","Left Arm"}, {"Torso","Right Arm"}, {"Torso","Left Leg"}, {"Torso","Right Leg"} },
    LockSettings = { ESPEnabled = false, Sticky = false, Method = "Camera", TargetMode = "Closest", TargetPart = "HumanoidRootPart", Smoothness = 0, SideOffset = 1.75, MaxDistance = 500, WallCheck = false, ShowFOV = false, FOVRadius = 200, FOVFilled = false, FOVTransparency = 0.8, FOVSides = 60, Prediction = 0, ShowTargetInfo = false, LockNotification = true, AutoRetarget = false, TargetInfoPos = "Top" },
    LockState = { Enabled = false, LastTargetSearch = 0, WasLockedBody = false, ZoomDistance = 10, CurrentLockTarget = nil, CameraLocked = false, OrigCamType = Enum.CameraType.Custom, LockBodyGyro = nil, ESPGui = nil, FOVCircle = nil, TargetInfoName = nil, TargetInfoHP = nil, TargetInfoDist = nil, TargetInfoLine = nil, LastTargetName = nil }
}
Logic.DashAnimLeft.AnimationId = "rbxassetid://75203303352791"
Logic.DashAnimRight.AnimationId = "rbxassetid://117223862448096"

local TeleportLocations = { ["Under the Map"] = Vector3.new(-20.23, -61.53, -146.34), ["Unlicensed Studios"] = Vector3.new(196.86, 23.58, -37.27), ["Towers"] = Vector3.new(25.35, 183.08, 110.77), ["Train Button"] = Vector3.new(182.21, -9.33, 562.54), ["Bowling"] = Vector3.new(267.60, -59.89, -255.06), ["Restaurant"] = Vector3.new(-43.24, 23.63, -83.07), ["Storage House"] = Vector3.new(195.69, 23.58, 151.44), ["Sewers 1"] = Vector3.new(-148.14, -31.48, -127.22), ["Train Station"] = Vector3.new(185.27, -9.69, -97.17), ["Sewers 2"] = Vector3.new(60.84, -10.58, 167.47), ["Shenanigans Mall"] = Vector3.new(155.66, -26.38, -254.85), ["Rhythm Game"] = Vector3.new(12.23, -30.21, -315.03), ["Piano"] = Vector3.new(-86.38, 26.65, -252.48), ["Convenience Store"] = Vector3.new(-247.51, 26.96, -116.64), ["Court"] = Vector3.new(124.48, 23.78, -247.06), ["Graveyard"] = Vector3.new(228.55, 23.68, -130.48), ["Train Station Exit"] = Vector3.new(1.52, 24.72, 396.06), ["Tze's"] = Vector3.new(-55.30, 23.62, 245.42), ["Jail"] = Vector3.new(-243.84, 23.58, 126.97) }
local LocationNames = {}
for k, _ in pairs(TeleportLocations) do table.insert(LocationNames, k) end
table.sort(LocationNames)
local currentTPDest = LocationNames[1]

--// ==========================================
--// SYSTEM FUNCTIONS 
--// ==========================================

_G.MiscState = _G.MiscState or {}
local Invisibility = { Connections = {} }

function Invisibility.toggle(state, silent)
    local char = Player.Character
    if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local animator = hum and hum:FindFirstChildOfClass("Animator")
    if not (hum and hrp and animator) then return end
    local Camera = workspace.CurrentCamera

    if state then
        _G.MiscState.IsInvisible = true
        if not silent then hum.AutoRotate = false; hrp.Anchored = true end
        Invisibility.Connections["InvisNoclip"] = RunService.Stepped:Connect(function()
            if not _G.MiscState.IsInvisible then return end
            for _, part in ipairs(char:GetDescendants()) do if part:IsA("BasePart") then part.CanCollide = false end end
        end)
        _G.MiscState.InvisibleFakeTorso = Instance.new("Part")
        _G.MiscState.InvisibleFakeTorso.Name = "FakeTorso"; _G.MiscState.InvisibleFakeTorso.Size = Vector3.new(2,2,1); _G.MiscState.InvisibleFakeTorso.Transparency = 1; _G.MiscState.InvisibleFakeTorso.CanCollide = false; _G.MiscState.InvisibleFakeTorso.Anchored = true; _G.MiscState.InvisibleFakeTorso.Parent = workspace
        Camera.CameraType = Enum.CameraType.Custom
        Camera.CameraSubject = _G.MiscState.InvisibleFakeTorso

        if not silent then
            RunService:BindToRenderStep("MeditationFocus", Enum.RenderPriority.Camera.Value - 1, function()
                if not _G.MiscState.IsInvisible or not hrp or not hrp.Parent or not _G.MiscState.InvisibleFakeTorso then return end
                local camCF = Camera.CFrame
                local lookDir = Vector3.new(camCF.LookVector.X, 0, camCF.LookVector.Z)
                if lookDir.Magnitude > 0.001 then hrp.CFrame = CFrame.new(hrp.Position, hrp.Position + lookDir.Unit) end
                _G.MiscState.InvisibleFakeTorso.CFrame = CFrame.new(hrp.Position + Vector3.new(0, 1.5, 0))
            end)
        end
        pcall(function()
            local animObj = ReplicatedStorage.Modules.MVP.Meditation.Character
            _G.MiscState.InvisibleTrack = animator:LoadAnimation(animObj)
            _G.MiscState.InvisibleTrack.Priority = Enum.AnimationPriority.Action4
            _G.MiscState.InvisibleTrack:Play()
            task.wait(0.1); _G.MiscState.InvisibleTrack.TimePosition = 0.1; _G.MiscState.InvisibleTrack:AdjustSpeed(0)
            for _, t in ipairs(animator:GetPlayingAnimationTracks()) do if t ~= _G.MiscState.InvisibleTrack then t:Stop(0) end end
            Invisibility.Connections["InvisAnim"] = animator.AnimationPlayed:Connect(function(newTrack) if newTrack ~= _G.MiscState.InvisibleTrack then newTrack:Stop(0) end end)
        end)
        SendNotification("Invisibility", "Invisibility Enabled", 2)
    else
        _G.MiscState.IsInvisible = false
        if Invisibility.Connections["InvisNoclip"] then Invisibility.Connections["InvisNoclip"]:Disconnect(); Invisibility.Connections["InvisNoclip"] = nil end
        if Invisibility.Connections["InvisAnim"] then Invisibility.Connections["InvisAnim"]:Disconnect(); Invisibility.Connections["InvisAnim"] = nil end
        pcall(function() RunService:UnbindFromRenderStep("MeditationFocus") end)
        if char and char.Parent then for _, part in ipairs(char:GetDescendants()) do if part:IsA("BasePart") then part.CanCollide = true end end end
        if hrp and hrp.Parent then hrp.Anchored = false end
        if hum and hum.Parent then hum.AutoRotate = true; Camera.CameraSubject = hum end
        if _G.MiscState.InvisibleFakeTorso then _G.MiscState.InvisibleFakeTorso:Destroy(); _G.MiscState.InvisibleFakeTorso = nil end
        if _G.MiscState.InvisibleTrack then pcall(function() _G.MiscState.InvisibleTrack:Stop(0) end); pcall(function() _G.MiscState.InvisibleTrack:Destroy() end); _G.MiscState.InvisibleTrack = nil end
        SendNotification("Invisibility", "Invisibility Disabled", 2)
    end
end

local function clearPathVisualization()
    for _, p in ipairs(Logic.Pathfinding.VisualParts) do pcall(function() p:Destroy() end) end
    Logic.Pathfinding.VisualParts = {}
    if Logic.Pathfinding.VisualFolder and Logic.Pathfinding.VisualFolder.Parent then Logic.Pathfinding.VisualFolder:Destroy() end
    Logic.Pathfinding.VisualFolder = nil
end

local function visualizePath(waypoints)
    clearPathVisualization()
    if not Logic.Pathfinding.VisualizeOn then return end
    local folder = Instance.new("Folder"); folder.Name = "PathVisuals"; folder.Parent = workspace
    Logic.Pathfinding.VisualFolder = folder

    for i, wp in ipairs(waypoints) do
        local isJump = (wp.Action == Enum.PathWaypointAction.Jump)
        local node = Instance.new("Part"); node.Shape = Enum.PartType.Ball; node.Size = isJump and Vector3.new(1.4, 1.4, 1.4) or Vector3.new(0.9, 0.9, 0.9); node.Position = wp.Position; node.Anchored = true; node.CanCollide = false; node.Material = Enum.Material.Neon; node.Transparency = 0.25; node.Color = isJump and Color3.fromRGB(255, 180, 30) or Color3.fromRGB(30, 180, 255); node.Parent = folder
        table.insert(Logic.Pathfinding.VisualParts, node)

        if i > 1 then
            local prevPos = waypoints[i - 1].Position; local currPos = wp.Position; local mid = (prevPos + currPos) / 2; local dist = (currPos - prevPos).Magnitude
            local line = Instance.new("Part"); line.Anchored = true; line.CanCollide = false; line.Material = Enum.Material.Neon; line.Color = Color3.fromRGB(50, 100, 255); line.Transparency = 0.55; line.Size = Vector3.new(0.22, 0.22, dist); line.CFrame = CFrame.lookAt(mid, currPos); line.Parent = folder
            table.insert(Logic.Pathfinding.VisualParts, line)
        end
    end
end

local function batchWaypoints(waypoints, minSegmentLength)
    local batched = {}; local lastAdded = waypoints[1]; table.insert(batched, waypoints[1])
    for i = 2, #waypoints do
        local wp = waypoints[i]; local isJump = (wp.Action == Enum.PathWaypointAction.Jump); local isLast = (i == #waypoints); local distFromLast = (wp.Position - lastAdded.Position).Magnitude
        if isJump or isLast or distFromLast >= minSegmentLength then table.insert(batched, wp); lastAdded = wp end
    end
    return batched
end

local function tweenToPosition(hrp, targetPos, speed)
    local distance = (hrp.Position - targetPos).Magnitude
    if distance < 0.5 then return true end
    local duration = math.max(distance / speed, 0.001)
    local direction = (targetPos - hrp.Position); local flatDir = Vector3.new(direction.X, 0, direction.Z); local targetCF = flatDir.Magnitude > 0.1 and CFrame.new(targetPos, targetPos + flatDir.Unit) or CFrame.new(targetPos) * hrp.CFrame.Rotation

    local tween = TweenService:Create(hrp, TweenInfo.new(duration, Enum.EasingStyle.Linear), { CFrame = targetCF })
    Logic.Pathfinding.CurrentTween = tween; tween:Play()

    local done = false; local conn
    conn = tween.Completed:Connect(function() done = true; if conn then conn:Disconnect() end end)
    while not done and Logic.Pathfinding.Active do RunService.Heartbeat:Wait() end
    if not done and Logic.Pathfinding.CurrentTween == tween then tween:Cancel() end
    return done and Logic.Pathfinding.Active
end

local function stepToPosition(hrp, targetPos, speed)
    local startPos = hrp.Position; local direction = (targetPos - startPos); local distance = direction.Magnitude
    if distance < 0.5 then return true end
    local dirUnit = direction.Unit; local flatDir = Vector3.new(dirUnit.X, 0, dirUnit.Z); local lookCF = flatDir.Magnitude > 0.01 and CFrame.lookAt(Vector3.zero, flatDir) or CFrame.new()

    local traveled = 0
    while traveled < distance and Logic.Pathfinding.Active do
        local dt = RunService.Heartbeat:Wait(); local step = speed * dt; traveled = math.min(traveled + step, distance)
        hrp.CFrame = CFrame.new(startPos + dirUnit * traveled) * lookCF.Rotation
    end
    return traveled >= distance and Logic.Pathfinding.Active
end

local function stopPathfinding()
    Logic.Pathfinding.Active = false
    if _G.MiscState and _G.MiscState.IsInvisible then Invisibility.toggle(false, false) end
    if Logic.Pathfinding.CurrentTween then pcall(function() Logic.Pathfinding.CurrentTween:Cancel() end) end
    local hrp = Player.Character and Player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.Anchored = false end
    local hum = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end
    clearPathVisualization()
end

local function startPathfinding(targetPos)
    if Logic.Pathfinding.Active then stopPathfinding(); task.wait(0.15) end
    Logic.Pathfinding.Active = true

    task.spawn(function()
        while Logic.Pathfinding.Active do
            local speed = Logic.Pathfinding.Speed
            local path = PathfindingService:CreatePath({ AgentRadius = 3, AgentHeight = 6, AgentCanJump = true, AgentCanClimb = true, WaypointSpacing = 10 })
            local hrp = Player.Character and Player.Character:FindFirstChild("HumanoidRootPart")
            local humanoid = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")
            if not hrp or not humanoid then task.wait(1) continue end

            if (hrp.Position - targetPos).Magnitude < 6 then 
                SendNotification("Teleport", "Destination reached!", 3)
                stopPathfinding() 
                break 
            end

            local ok, _ = pcall(function() path:ComputeAsync(hrp.Position, targetPos) end)
            if not ok or path.Status == Enum.PathStatus.NoPath then 
                SendNotification("Teleport", "No path found! Retrying...", 2)
                task.wait(1.5) 
                continue 
            end

            local batchedWP = batchWaypoints(path:GetWaypoints(), math.max(speed * 0.05, 2))
            visualizePath(path:GetWaypoints())

            local pathBlocked = false; local blockedConn = path.Blocked:Connect(function() pathBlocked = true end)
            hrp.Anchored = true; humanoid.PlatformStand = true
            local completed = true

            for i = 2, #batchedWP do
                if not Logic.Pathfinding.Active or pathBlocked then completed = false break end
                local success
                if Logic.Pathfinding.Speed > 500 then success = stepToPosition(hrp, batchedWP[i].Position, Logic.Pathfinding.Speed)
                else success = tweenToPosition(hrp, batchedWP[i].Position, Logic.Pathfinding.Speed) end
                if not success then completed = false break end
            end

            if blockedConn then blockedConn:Disconnect() end
            if completed and Logic.Pathfinding.Active then 
                SendNotification("Teleport", "Destination reached!", 3)
                stopPathfinding() 
                break 
            end
            task.wait(0.3)
        end
    end)
end

local function getLockTarget()
    if tick() - Logic.LockState.LastTargetSearch < 0.5 then return nil end
    Logic.LockState.LastTargetSearch = tick()
    local char = Player.Character
    if not char then return nil end
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return nil end

    if RyuConfig.LockSpecificPlayer ~= "Auto" then
        local selectedName = RyuConfig.LockSpecificPlayer
        local isNPC = string.sub(selectedName, 1, 6) == "[NPC] "
        
        if isNPC then
            local npcName = string.sub(selectedName, 7)
            local charsFolder = workspace:FindFirstChild("Characters")
            if charsFolder then
                for _, model in ipairs(charsFolder:GetChildren()) do
                    if model.Name == npcName and model:IsA("Model") then
                        local tHum = model:FindFirstChildOfClass("Humanoid")
                        local tRoot = model:FindFirstChild("HumanoidRootPart") or model:FindFirstChild("Torso")
                        if tHum and tHum.Health > 0 and tRoot then
                            local dist = (root.Position - tRoot.Position).Magnitude
                            if dist <= Logic.LockSettings.MaxDistance then
                                if Logic.LockSettings.WallCheck then
                                    local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, model}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
                                    local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * dist, rayParams)
                                    if ray then return nil end
                                end
                                return model
                            end
                        end
                    end
                end
            end
            return nil
        else
            local targetPlayer = Players:FindFirstChild(selectedName)
            if targetPlayer and targetPlayer.Character then
                local tChar = targetPlayer.Character
                local tHum = tChar:FindFirstChildOfClass("Humanoid")
                local tRoot = tChar:FindFirstChild("HumanoidRootPart") or tChar:FindFirstChild("Torso")
                if tHum and tHum.Health > 0 and tRoot and tChar ~= char then
                    local dist = (root.Position - tRoot.Position).Magnitude
                    if dist <= Logic.LockSettings.MaxDistance then
                        if Logic.LockSettings.WallCheck then
                            local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, tChar}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
                            local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * dist, rayParams)
                            if ray then return nil end
                        end
                        return tChar
                    end
                end
            end
            return nil
        end
    end

    local best, shortest = nil, math.huge
    local charsFolder = workspace:FindFirstChild("Characters")
    local entities = charsFolder and charsFolder:GetChildren() or {}
    if #entities == 0 then for _, p in ipairs(Players:GetPlayers()) do if p.Character then table.insert(entities, p.Character) end end end

    for _, tChar in ipairs(entities) do
        if tChar:IsA("Model") and tChar ~= char then
            local tHum = tChar:FindFirstChildOfClass("Humanoid")
            local tRoot = tChar:FindFirstChild("HumanoidRootPart") or tChar:FindFirstChild("Torso")

            if tHum and tHum.Health > 0 and tRoot then
                local worldDist = (root.Position - tRoot.Position).Magnitude
                if worldDist > Logic.LockSettings.MaxDistance then continue end
                
                if Logic.LockSettings.WallCheck then
                    local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, tChar}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
                    local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * worldDist, rayParams)
                    if ray then continue end
                end

                if Logic.LockSettings.TargetMode == "Closest" then
                    if worldDist < shortest then shortest = worldDist; best = tChar end
                else
                    local pos, onScreen = camera:WorldToViewportPoint(tRoot.Position)
                    if onScreen then
                        local d = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(pos.X, pos.Y)).Magnitude
                        if Logic.LockSettings.ShowFOV and Logic.LockSettings.FOVRadius > 0 then if d > Logic.LockSettings.FOVRadius then continue end end
                        if d < shortest then shortest = d; best = tChar end
                    end
                end
            end
        end
    end
    return best
end

local function getClosestTarget(maxDist)
    local char = Player.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return nil end
    local closest = nil; local minDistance = math.huge
    local charactersFolder = workspace:FindFirstChild("Characters")
    local targetsToSearch = charactersFolder and charactersFolder:GetChildren() or workspace:GetChildren()

    for _, model in ipairs(targetsToSearch) do
        if model ~= char and model:IsA("Model") then
            local enemyRoot = model:FindFirstChild("HumanoidRootPart")
            local enemyHum = model:FindFirstChild("Humanoid")
            if enemyRoot and enemyHum and enemyHum.Health > 0 then
                local dist = (enemyRoot.Position - root.Position).Magnitude
                if dist < minDistance and dist <= maxDist then minDistance = dist; closest = model end
            end
        end
    end
    return closest
end

local isDashingArc = false
local function getBezierPoint(t, p0, p1, p2) return (1 - t)^2 * p0 + 2 * (1 - t) * t * p1 + t^2 * p2 end

local function executeDashArc(direction)
    if isDashingArc then return end; isDashingArc = true
    local character = Player.Character; if not character then isDashingArc = false return end
    local root = character:FindFirstChild("HumanoidRootPart"); local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not root or not humanoid then isDashingArc = false return end

    local targetChar = getClosestTarget(Logic.DashAssistState.DetectionRange)
    local target = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
    if not target then isDashingArc = false return end

    if Logic.DashAssistState.OnlyIfFacing then
        local toPlayer = (root.Position - target.Position).Unit
        if target.CFrame.LookVector:Dot(toPlayer) < -0.1 then isDashingArc = false return end
    end

    root.Anchored = true; humanoid.AutoRotate = false; root.AssemblyLinearVelocity = Vector3.zero
    for _, mover in pairs(root:GetChildren()) do if mover:IsA("BodyVelocity") or mover:IsA("LinearVelocity") or mover:IsA("AlignPosition") or mover:IsA("VectorForce") or mover:IsA("BodyPosition") then mover:Destroy() end end

    local animator = humanoid:FindFirstChildOfClass("Animator")
    local dashTrack = nil
    if animator then
        for _, track in pairs(animator:GetPlayingAnimationTracks()) do if track.Animation and (track.Animation.AnimationId:match("117223862448096") or track.Animation.AnimationId:match("75203303352791")) then track:Stop(0) end end
        dashTrack = animator:LoadAnimation((direction == "Left") and Logic.DashAnimLeft or Logic.DashAnimRight)
        dashTrack.Priority = Enum.AnimationPriority.Action4; dashTrack:Play(0.05, 1, 1 / Logic.DashAssistState.FlightDuration)
    end

    local p0 = root.Position; local sideMult = (direction == "Left") and -1 or 1
    local progress = Instance.new("NumberValue"); progress.Value = 0
    local dashName = "RyuHubFakeDash_" .. tostring(tick())

    local tween = TweenService:Create(progress, TweenInfo.new(Logic.DashAssistState.FlightDuration, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut), { Value = 1 })
    local Camera = workspace.CurrentCamera; local prevCamType = Camera.CameraType

    RunService:BindToRenderStep(dashName, 20000, function()
        if not target or not target.Parent or not root then
            RunService:UnbindFromRenderStep(dashName); if root then root.Anchored = false end; if humanoid then humanoid.AutoRotate = true end
            if dashTrack then dashTrack:Stop() end; if Logic.DashAssistState.CameraLock then Camera.CameraType = prevCamType end
            isDashingArc = false; return
        end

        root.Anchored = true; humanoid.AutoRotate = false; root.AssemblyLinearVelocity = Vector3.zero
        local val = progress.Value; local tPos = target.Position
        local tLook = target.CFrame.LookVector; local flatLook = Vector3.new(tLook.X, 0, tLook.Z)
        if flatLook.Magnitude > 0.001 then flatLook = flatLook.Unit else flatLook = Vector3.new(0, 0, -1) end

        local tRight = target.CFrame.RightVector; local flatRight = Vector3.new(tRight.X, 0, tRight.Z)
        if flatRight.Magnitude > 0.001 then flatRight = flatRight.Unit else flatRight = Vector3.new(1, 0, 0) end

        local p2 = tPos + (flatLook * -Logic.DashAssistState.BehindDistance)
        local midPoint = (p0 + p2) / 2
        local p1 = midPoint + (flatRight * (Logic.DashAssistState.CurveStrength * sideMult)) + Vector3.new(0, Logic.DashAssistState.ArchHeight, 0)

        local currentPos = getBezierPoint(val, p0, p1, p2)
        local lookPos = Vector3.new(tPos.X, currentPos.Y, tPos.Z)
        if (lookPos - currentPos).Magnitude > 0.1 then root.CFrame = CFrame.lookAt(currentPos, lookPos) else root.CFrame = CFrame.new(currentPos) end

        if Logic.DashAssistState.CameraLock then
            Camera.CameraType = Enum.CameraType.Scriptable; local dirToEnemy = (tPos - root.Position).Unit
            Camera.CFrame = Camera.CFrame:Lerp(CFrame.lookAt(root.Position - (dirToEnemy * 11) + Vector3.new(0, 4.5, 0), tPos), 0.35)
        end
    end)

    tween:Play()
    tween.Completed:Connect(function()
        RunService:UnbindFromRenderStep(dashName); progress:Destroy()
        if dashTrack then dashTrack:Stop(0.1) end
        if Logic.DashAssistState.CameraLock then Camera.CameraType = prevCamType end

        local lockStart = tick(); local lockName = "RyuHubDashLock_" .. tostring(lockStart)
        RunService:BindToRenderStep(lockName, 20000, function()
            if tick() - lockStart > Logic.DashAssistState.LockDuration or not target or not target.Parent or not root then
                RunService:UnbindFromRenderStep(lockName); if root then root.Anchored = false; root.AssemblyLinearVelocity = Vector3.zero end
                if humanoid then humanoid.AutoRotate = true end; isDashingArc = false return
            end
            root.Anchored = true; if humanoid then humanoid.AutoRotate = false end; root.AssemblyLinearVelocity = Vector3.zero
            local lockedPos = root.Position; local facePos = Vector3.new(target.Position.X, lockedPos.Y, target.Position.Z)
            if (facePos - lockedPos).Magnitude > 0.1 then root.CFrame = CFrame.lookAt(lockedPos, facePos) end
        end)
    end)
end

local function autoFireDivergentFist()
    local character = Player.Character
    if character and character:FindFirstChild("Moveset") then
        local divergentFistMove = character.Moveset:FindFirstChild("Divergent Fist")
        if divergentFistMove and Logic.TargetRemote then Logic.TargetRemote:FireServer(divergentFistMove, nil) end
    end
end

local function performDashLogic(target)
    local MAX_DASH_DISTANCE = RyuConfig.DashDistance; local FIRE_DELAY = RyuConfig.FireDelay; local DASH_DURATION = RyuConfig.DashDuration; local POST_DASH_LOCK_TIME = RyuConfig.LockTime
    local easingStyleEnum = RyuConfig.DashEasingStyle or Enum.EasingStyle.Cubic
    local easingDirectionEnum = RyuConfig.DashEasingDirection or Enum.EasingDirection.Out

    local char = Player.Character; local root = char and char:FindFirstChild("HumanoidRootPart"); local humanoid = char and char:FindFirstChild("Humanoid"); local enemyRoot = target:FindFirstChild("HumanoidRootPart")
    if not root or not enemyRoot then task.delay(FIRE_DELAY, autoFireDivergentFist) return end

    local initialEnemyCFrame = enemyRoot.CFrame; local initialEnemyPos = initialEnemyCFrame.Position; local startPos = root.Position
    local isBehind = initialEnemyCFrame:PointToObjectSpace(startPos).Z > 0; local distanceToEnemy = (startPos - initialEnemyPos).Magnitude

    local isDash = true; local dashType = "Arch"; local endPos = startPos; local controlPos = startPos

    if isBehind and distanceToEnemy <= 10 then isDash = false
    elseif isBehind and distanceToEnemy > 10 then dashType = "Straight"; endPos = (initialEnemyCFrame * CFrame.new(0, 0, 5)).Position
    else
        dashType = "Arch"; endPos = (initialEnemyCFrame * CFrame.new(0, 0, 4)).Position
        local perp = Vector3.new(-(endPos - startPos).Z, 0, (endPos - startPos).X)
        if perp.Magnitude > 0.001 then perp = perp.Unit else perp = Vector3.new(1, 0, 0) end
        local midPos = (startPos + endPos) / 2; local archWidth = math.clamp((endPos - startPos).Magnitude / 1.5, 5, 25)
        local cp1 = midPos + (perp * archWidth); local cp2 = midPos - (perp * archWidth)
        controlPos = (((startPos - initialEnemyPos):Dot(initialEnemyCFrame.RightVector) > 0) == ((cp1 - initialEnemyPos):Dot(initialEnemyCFrame.RightVector) > 0)) and cp1 or cp2
    end

    if humanoid then humanoid.AutoRotate = false end
    local startTime = tick(); local hasFired = false; local dashConn

    local function finalizeMovement(finalCFrame)
        if dashConn then dashConn:Disconnect() end
        if humanoid then humanoid.AutoRotate = true end
        if finalCFrame and root and root.Parent then root.CFrame = finalCFrame end
        if not hasFired then hasFired = true; autoFireDivergentFist() end
    end

    dashConn = RunService.Heartbeat:Connect(function()
        if not root or not root.Parent then return finalizeMovement(nil) end
        local elapsed = tick() - startTime
        if elapsed >= FIRE_DELAY and not hasFired then hasFired = true; autoFireDivergentFist() end

        local currentPos = startPos
        if isDash then
            local alpha = math.clamp(elapsed / DASH_DURATION, 0, 1); local easedAlpha = TweenService:GetValue(alpha, easingStyleEnum, easingDirectionEnum)
            if dashType == "Straight" then currentPos = startPos:Lerp(endPos, easedAlpha)
            elseif dashType == "Arch" then currentPos = (1 - easedAlpha)^2 * startPos + 2 * (1 - easedAlpha) * easedAlpha * controlPos + easedAlpha^2 * endPos end
        end
        root.CFrame = CFrame.lookAt(currentPos, initialEnemyPos)

        if RyuConfig.CameraLockEnabled then local cam = workspace.CurrentCamera; if cam then cam.CFrame = CFrame.lookAt(cam.CFrame.Position, initialEnemyPos) end end
        if elapsed >= (isDash and (DASH_DURATION + POST_DASH_LOCK_TIME) or (FIRE_DELAY + POST_DASH_LOCK_TIME)) then finalizeMovement(CFrame.lookAt(currentPos, initialEnemyPos)) end
    end)
end

local function estimateRegion(ping)
    if ping <= 0 then return "Unknown" elseif ping <= 40 then return "US East / Nearby" elseif ping <= 70 then return "US Central" elseif ping <= 100 then return "US West / Regional" elseif ping <= 140 then return "EU West" elseif ping <= 180 then return "EU East / Middle East" elseif ping <= 220 then return "South America" elseif ping <= 270 then return "East Asia" elseif ping <= 320 then return "Southeast Asia / Oceania" elseif ping <= 400 then return "Africa / South Asia" else return "Very Far" end
end

local function pingQualityIcon(ping)
    if ping <= 60 then return "[+++]" elseif ping <= 120 then return "[++ ]" elseif ping <= 200 then return "[+  ]" else return "[!  ]" end
end

local function fetchServers()
    Logic.ServerList = {}; Logic.ServerMap = {}
    local placeId = game.PlaceId; local cursor = ""; local maxPages = 10; local idx = 0
    SendNotification("Server Browser", "Fetching servers... This may take a moment.", 4)
    for page = 1, maxPages do
        local url = string.format("https://games.roblox.com/v1/games/%d/servers/0?sortOrder=2&excludeFullGames=false&limit=100%s", placeId, cursor ~= "" and ("&cursor=" .. cursor) or "")
        local ok, res = pcall(game.HttpGet, game, url)
        if not ok or not res then break end
        local ok2, data = pcall(HttpService.JSONDecode, HttpService, res)
        if not ok2 or not data or not data.data then break end
        for _, srv in ipairs(data.data) do
            idx += 1
            local entry = { JobId = srv.id or "", Playing = srv.playing or 0, MaxPlayers = srv.maxPlayers or 0, Ping = srv.ping or 0, FPS = srv.fps or 0, Index = idx, Region = estimateRegion(srv.ping or 0), IsCurrent = (srv.id == game.JobId) }
            table.insert(Logic.ServerList, entry)
        end
        cursor = data.nextPageCursor or ""
        if cursor == "" then break end; task.wait(0.4)
    end
    SendNotification("Server Browser", "Found " .. tostring(#Logic.ServerList) .. " servers!", 4)
    return Logic.ServerList
end

--// ==========================================
--// UI BUILDER & POPULATION
--// ==========================================

local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 50, 0, 50); ToggleBtn.Position = UDim2.new(0, 15, 0, 60); ToggleBtn.BackgroundColor3 = Theme.Sidebar; ToggleBtn.Text = ""; ToggleBtn.Parent = RyuHub
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
local btnStroke = Instance.new("UIStroke", ToggleBtn); btnStroke.Color = Theme.Accent; btnStroke.Thickness = 2; btnStroke.Transparency = 0.5
local Katana = Instance.new("Frame", ToggleBtn); Katana.Size = UDim2.new(1, 0, 1, 0); Katana.BackgroundTransparency = 1; Katana.Rotation = 45
local Blade = Instance.new("Frame", Katana); Blade.Size = UDim2.new(0, 2, 0, 24); Blade.Position = UDim2.new(0.5, -1, 0.5, -18); Blade.BackgroundColor3 = Theme.Text; Blade.BorderSizePixel = 0
local Guard = Instance.new("Frame", Katana); Guard.Size = UDim2.new(0, 12, 0, 2); Guard.Position = UDim2.new(0.5, -6, 0.5, 6); Guard.BackgroundColor3 = Theme.SubText; Guard.BorderSizePixel = 0
local Handle = Instance.new("Frame", Katana); Handle.Size = UDim2.new(0, 4, 0, 10); Handle.Position = UDim2.new(0.5, -2, 0.5, 8); Handle.BackgroundColor3 = Theme.Stroke; Handle.BorderSizePixel = 0
Instance.new("UICorner", Blade).CornerRadius = UDim.new(1, 0); Instance.new("UICorner", Guard).CornerRadius = UDim.new(1, 0); Instance.new("UICorner", Handle).CornerRadius = UDim.new(0, 1)

local function AddClickPop(element)
    local orig = element.Size
    element.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then pcall(function() TweenService:Create(element, TweenInfo.new(0.1), {Size = UDim2.new(orig.X.Scale, orig.X.Offset - 4, orig.Y.Scale, orig.Y.Offset - 4)}):Play() end) end end)
    element.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then pcall(function() TweenService:Create(element, TweenInfo.new(0.3), {Size = orig}):Play() end) end end)
end
AddClickPop(ToggleBtn)

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 0, 0, 0); MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0); MainFrame.BackgroundColor3 = Theme.Background; MainFrame.BorderSizePixel = 0; MainFrame.Visible = false; MainFrame.ClipsDescendants = true; MainFrame.Active = true; MainFrame.Parent = RyuHub
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)
local mainStroke = Instance.new("UIStroke", MainFrame); mainStroke.Color = Theme.Stroke; mainStroke.Thickness = 1.5
local DragText = Instance.new("TextLabel", MainFrame); DragText.Size = UDim2.new(1, 0, 1, 0); DragText.Position = UDim2.new(0, 0, 0, 0); DragText.BackgroundTransparency = 1; DragText.Text = "DISCORD.GG/RYUHUB"; DragText.Font = Enum.Font.GothamBlack; DragText.TextSize = 50; DragText.TextColor3 = Theme.Text; DragText.TextTransparency = 0.95; DragText.ZIndex = 0

ToggleBtn.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end)
        task.delay(0.3, function() MainFrame.Visible = false end)
    else
        MainFrame.Visible = true
        pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = MainSize, Position = UDim2.new(0.5, -MainSize.X.Offset / 2, 0.5, -MainSize.Y.Offset / 2)}):Play() end)
    end
end)

local ContentWrapper = Instance.new("Frame", MainFrame); ContentWrapper.Size = UDim2.new(1, 0, 1, 0); ContentWrapper.BackgroundTransparency = 1; ContentWrapper.BorderSizePixel = 0
local Topbar = Instance.new("Frame", ContentWrapper); Topbar.Size = UDim2.new(1, 0, 0, 60); Topbar.BackgroundTransparency = 1
local Title = Instance.new("TextLabel", Topbar); Title.Size = UDim2.new(0, 300, 1, 0); Title.Position = UDim2.new(0, 20, 0, 0); Title.BackgroundTransparency = 1; Title.Text = "RYU HUB"; Title.Font = Enum.Font.GothamBlack; Title.TextSize = 22; Title.TextXAlignment = Enum.TextXAlignment.Left; Title.TextColor3 = Theme.Text
local SubTitle = Instance.new("TextLabel", Topbar); SubTitle.Size = UDim2.new(0, 300, 0, 15); SubTitle.Position = UDim2.new(0, 20, 0, 38); SubTitle.BackgroundTransparency = 1; SubTitle.Text = "Jujutsu Shenanigans"; SubTitle.TextColor3 = Theme.SubText; SubTitle.Font = Enum.Font.Gotham; SubTitle.TextSize = 11; SubTitle.TextXAlignment = Enum.TextXAlignment.Left
local CloseBtn = Instance.new("TextButton", Topbar); CloseBtn.Size = UDim2.new(0, 28, 0, 28); CloseBtn.Position = UDim2.new(1, -40, 0, 15); CloseBtn.BackgroundColor3 = Theme.SectionBG; CloseBtn.Text = "X"; CloseBtn.TextColor3 = Theme.Text; CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.TextSize = 14
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6); Instance.new("UIStroke", CloseBtn).Color = Theme.Stroke
CloseBtn.MouseButton1Click:Connect(function() pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end); task.delay(0.3, function() MainFrame.Visible = false end) end)

local mDragging, mDragStart, mStartPos = false, nil, nil
Topbar.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then mDragging = true; mDragStart = input.Position; mStartPos = MainFrame.Position end end)
Topbar.InputChanged:Connect(function(input) if mDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then local delta = input.Position - mDragStart; MainFrame.Position = UDim2.new(mStartPos.X.Scale, mStartPos.X.Offset + delta.X, mStartPos.Y.Scale, mStartPos.Y.Offset + delta.Y) end end)
Topbar.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then mDragging = false end end)

local Line = Instance.new("Frame", ContentWrapper); Line.Size = UDim2.new(1, -40, 0, 1); Line.Position = UDim2.new(0, 20, 0, 65); Line.BackgroundColor3 = Theme.Stroke; Line.BorderSizePixel = 0
local Sidebar = Instance.new("ScrollingFrame", ContentWrapper); Sidebar.Size = UDim2.new(0, SidebarWidth, 1, -85); Sidebar.Position = UDim2.new(0, 10, 0, 75); Sidebar.BackgroundTransparency = 1; Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar); SideLayout.Padding = UDim.new(0, 6); SideLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left; SideLayout.SortOrder = Enum.SortOrder.LayoutOrder
local ContentContainer = Instance.new("Frame", ContentWrapper); ContentContainer.Size = UDim2.new(1, -(SidebarWidth + 25), 1, -85); ContentContainer.Position = UDim2.new(0, SidebarWidth + 15, 0, 75); ContentContainer.BackgroundTransparency = 1

local itemOrderCounter = 0
local Tabs = {}
local refreshPlayerDropdown -- Forward declaration
local LockPlayerDropdown -- Forward declaration

local function UpdateSidebarCanvas()
    local totalH = 10
    for _, t in pairs(Tabs) do totalH = totalH + 36 + 6; if t.IsOpen then totalH = totalH + t.SubLayout.AbsoluteContentSize.Y + 6 end end
    Sidebar.CanvasSize = UDim2.new(0, 0, 0, totalH)
end

local function CreateMainTab(name)
    local tabObj = { Btn = nil, SubContainer = nil, SubLayout = nil, IsOpen = false, SubTabs = {} }
    local tabBtn = Instance.new("TextButton", Sidebar); tabBtn.Size = UDim2.new(1, 0, 0, 36); tabBtn.BackgroundColor3 = Theme.Sidebar; tabBtn.Text = "  " .. string.upper(name); tabBtn.TextColor3 = Theme.SubText; tabBtn.Font = Enum.Font.GothamBlack; tabBtn.TextSize = 13; tabBtn.TextXAlignment = Enum.TextXAlignment.Left; Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8); tabObj.Btn = tabBtn
    local subContainer = Instance.new("Frame", Sidebar); subContainer.Size = UDim2.new(1, 0, 0, 0); subContainer.BackgroundTransparency = 1; subContainer.ClipsDescendants = true; tabObj.SubContainer = subContainer
    local subLayout = Instance.new("UIListLayout", subContainer); subLayout.Padding = UDim.new(0, 2); subLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left; subLayout.SortOrder = Enum.SortOrder.LayoutOrder; tabObj.SubLayout = subLayout

    tabBtn.MouseButton1Click:Connect(function()
        tabObj.IsOpen = not tabObj.IsOpen
        local targetSize = tabObj.IsOpen and UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) or UDim2.new(1, 0, 0, 0)
        pcall(function() TweenService:Create(subContainer, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = targetSize}):Play(); tabBtn.TextColor3 = tabObj.IsOpen and Theme.Text or Theme.SubText; tabBtn.BackgroundColor3 = tabObj.IsOpen and Theme.SectionBG or Theme.Sidebar end)
        task.delay(0.26, UpdateSidebarCanvas)
    end)
    subLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() if tabObj.IsOpen then subContainer.Size = UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) end end)
    table.insert(Tabs, tabObj)
    return tabObj
end

local function CreateSubTab(tabObj, subName)
    local subObj = { Btn = nil, Page = nil }
    local subBtn = Instance.new("TextButton", tabObj.SubContainer); subBtn.Size = UDim2.new(1, 0, 0, 28); subBtn.BackgroundTransparency = 1; subBtn.Text = "     " .. subName; subBtn.TextColor3 = Theme.SubText; subBtn.Font = Enum.Font.GothamMedium; subBtn.TextSize = 12; subBtn.TextXAlignment = Enum.TextXAlignment.Left; subObj.Btn = subBtn
    local page = Instance.new("ScrollingFrame", ContentContainer); page.Size = UDim2.new(1, 0, 1, 0); page.BackgroundTransparency = 1; page.ScrollBarThickness = 2; page.ScrollBarImageColor3 = Theme.Accent; page.Visible = false; subObj.Page = page
    local pageLayout = Instance.new("UIListLayout", page); pageLayout.Padding = UDim.new(0, 12); pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    pageLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() page.CanvasSize = UDim2.new(0, 0, 0, pageLayout.AbsoluteContentSize.Y + 20) end)

    subBtn.MouseButton1Click:Connect(function()
        for _, t in pairs(Tabs) do for _, st in pairs(t.SubTabs) do st.Page.Visible = false; st.Btn.TextColor3 = Theme.SubText end end
        page.Visible = true; subBtn.TextColor3 = Theme.Text
    end)
    table.insert(tabObj.SubTabs, subObj)
    return page
end

local function CreateSection(page, titleText)
    local section = Instance.new("Frame", page); section.Size = UDim2.new(0.98, 0, 0, 50); section.BackgroundColor3 = Theme.SectionBG; section.BackgroundTransparency = 0; Instance.new("UICorner", section).CornerRadius = UDim.new(0, 10); Instance.new("UIStroke", section).Color = Theme.Stroke
    local secLayout = Instance.new("UIListLayout", section); secLayout.Padding = UDim.new(0, 10); secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center; secLayout.SortOrder = Enum.SortOrder.LayoutOrder
    local secPadding = Instance.new("UIPadding", section); secPadding.PaddingTop = UDim.new(0, 12); secPadding.PaddingBottom = UDim.new(0, 12)
    local title = Instance.new("TextLabel", section); title.LayoutOrder = -1; title.Size = UDim2.new(0.92, 0, 0, 24); title.BackgroundTransparency = 1; title.Text = titleText; title.TextColor3 = Theme.Text; title.Font = Enum.Font.GothamBold; title.TextSize = 14; title.TextXAlignment = Enum.TextXAlignment.Left
    secLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() section.Size = UDim2.new(1, 0, 0, secLayout.AbsoluteContentSize.Y + 24) end)
    return section
end

local function CreateLabel(section, text)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 90); frame.BackgroundTransparency = 1
    local lbl = Instance.new("TextLabel", frame); lbl.Size = UDim2.new(1, 0, 1, 0); lbl.BackgroundTransparency = 1; lbl.Text = text; lbl.TextColor3 = Theme.SubText; lbl.Font = Enum.Font.GothamMedium; lbl.TextSize = 11; lbl.TextXAlignment = Enum.TextXAlignment.Left; lbl.TextWrapped = true
    return lbl
end

local function CreateToggle(section, text, defaultState, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 34); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(0.7, 0, 0, 34); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = defaultState and Theme.Text or Theme.SubText; label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    local tBtn = Instance.new("TextButton", frame); tBtn.Size = UDim2.new(0, 42, 0, 22); tBtn.Position = UDim2.new(1, -42, 0, 6); tBtn.BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff; tBtn.Text = ""; Instance.new("UICorner", tBtn).CornerRadius = UDim.new(1, 0)
    local circle = Instance.new("Frame", tBtn); circle.Size = UDim2.new(0, 16, 0, 16); circle.Position = defaultState and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8); circle.BackgroundColor3 = defaultState and Theme.Background or Color3.fromRGB(150, 150, 150); Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = defaultState or false
    tBtn.MouseButton1Click:Connect(function()
        isOn = not isOn
        pcall(function() TweenService:Create(tBtn, TweenInfo.new(0.2), {BackgroundColor3 = isOn and Theme.ToggleOn or Theme.ToggleOff}):Play(); TweenService:Create(circle, TweenInfo.new(0.2), {Position = isOn and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = isOn and Theme.Background or Color3.fromRGB(150, 150, 150)}):Play(); label.TextColor3 = isOn and Theme.Text or Theme.SubText end)
        if callback then pcall(function() callback(isOn) end) end
    end)
end

local function CreateSlider(section, text, min, max, default, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 50); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(1, -40, 0, 18); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Theme.SubText; label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    local valLabel = Instance.new("TextLabel", frame); valLabel.Size = UDim2.new(0, 40, 0, 18); valLabel.Position = UDim2.new(1, -40, 0, 0); valLabel.BackgroundTransparency = 1; valLabel.Text = tostring(default); valLabel.TextColor3 = Theme.Accent; valLabel.Font = Enum.Font.GothamBold; valLabel.TextSize = 13; valLabel.TextXAlignment = Enum.TextXAlignment.Right
    local sliderBg = Instance.new("Frame", frame); sliderBg.Size = UDim2.new(1, 0, 0, 4); sliderBg.Position = UDim2.new(0, 0, 0, 32); sliderBg.BackgroundColor3 = Theme.ToggleOff; Instance.new("UICorner", sliderBg).CornerRadius = UDim.new(1, 0)
    local percentage = math.clamp((default - min) / (max - min), 0, 1)
    local sliderFill = Instance.new("Frame", sliderBg); sliderFill.Size = UDim2.new(percentage, 0, 1, 0); sliderFill.BackgroundColor3 = Theme.Accent; Instance.new("UICorner", sliderFill).CornerRadius = UDim.new(1, 0)
    local knob = Instance.new("TextButton", sliderFill); knob.Size = UDim2.new(0, 14, 0, 14); knob.Position = UDim2.new(1, -7, 0.5, -7); knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255); knob.Text = ""; Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
    
    local dragging = false
    knob.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relative = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            local value = math.floor(min + (max - min) * relative)
            valLabel.Text = tostring(value); sliderFill.Size = UDim2.new(relative, 0, 1, 0)
            if callback then pcall(function() callback(value) end) end
        end
    end)
end

local function CreateButton(section, text, color, callback)
    if type(color) == "function" then callback = color; color = Theme.SectionBG end
    itemOrderCounter = itemOrderCounter + 1
    local btn = Instance.new("TextButton", section); btn.LayoutOrder = itemOrderCounter; btn.Size = UDim2.new(0.92, 0, 0, 34); btn.BackgroundColor3 = color; btn.Text = text; btn.TextColor3 = Color3.fromRGB(255, 255, 255); btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6); Instance.new("UIStroke", btn).Color = Theme.Stroke; AddClickPop(btn)
    
    if callback then btn.MouseButton1Click:Connect(function() pcall(callback) end) end
    return btn
end

local function CreateDropdown(section, headerText, itemsList, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 160); frame.BackgroundTransparency = 1
    local header = Instance.new("TextLabel", frame); header.Size = UDim2.new(1, 0, 0, 20); header.BackgroundTransparency = 1; header.Text = headerText .. ": None"; header.TextColor3 = Theme.SubText; header.Font = Enum.Font.GothamMedium; header.TextSize = 12; header.TextXAlignment = Enum.TextXAlignment.Left
    local scroll = Instance.new("ScrollingFrame", frame); scroll.Size = UDim2.new(1, 0, 0, 130); scroll.Position = UDim2.new(0, 0, 0, 25); scroll.BackgroundColor3 = Theme.Background; scroll.ScrollBarThickness = 4; Instance.new("UICorner", scroll).CornerRadius = UDim.new(0, 6)
    local listLayout = Instance.new("UIListLayout", scroll); listLayout.Padding = UDim.new(0, 4); listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    local function populate(list)
        for _, child in pairs(scroll:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        for _, itemName in ipairs(list) do
            local btn = Instance.new("TextButton", scroll); btn.Size = UDim2.new(0.94, 0, 0, 26); btn.BackgroundColor3 = Theme.SectionBG; btn.Text = "  " .. tostring(itemName); btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; btn.TextXAlignment = Enum.TextXAlignment.Left; Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
            btn.MouseButton1Click:Connect(function() header.Text = headerText .. ": " .. tostring(itemName); if callback then callback(itemName) end end)
        end
        task.defer(function() scroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 10) end)
    end
    populate(itemsList)
    return { Refresh = populate }
end

local function CreateKeybind(section, text, defaultKey, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 34); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(0.7, 0, 0, 34); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Theme.Text; label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    local btn = Instance.new("TextButton", frame); btn.Size = UDim2.new(0, 80, 0, 22); btn.Position = UDim2.new(1, -80, 0, 6); btn.BackgroundColor3 = Theme.ToggleOff; btn.Text = defaultKey.Name; btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    
    local waiting = false
    btn.MouseButton1Click:Connect(function() waiting = true; btn.Text = "..." end)
    UserInputService.InputBegan:Connect(function(input)
        if waiting and input.UserInputType == Enum.UserInputType.Keyboard then
            waiting = false; btn.Text = input.KeyCode.Name; if callback then pcall(function() callback(input.KeyCode) end) end
        end
    end)
end

--// TAB: COMBAT
local TabCombat = CreateMainTab("Combat")
local SubBlackFlash = CreateSubTab(TabCombat, "Black Flash")
local SecBlackFlash = CreateSection(SubBlackFlash, "Blackflash Chain")
CreateToggle(SecBlackFlash, "Enable Black Flash Chain", RyuConfig.BlackFlashEnabled, function(v) RyuConfig.BlackFlashEnabled = v end)
CreateToggle(SecBlackFlash, "Camera Lock During Dash", RyuConfig.CameraLockEnabled, function(v) RyuConfig.CameraLockEnabled = v end)
CreateSlider(SecBlackFlash, "Max Dash Distance", 5, 50, RyuConfig.DashDistance, function(v) RyuConfig.DashDistance = v end)
CreateSlider(SecBlackFlash, "Combo Fire Delay (s)", 0, 1, RyuConfig.FireDelay, function(v) RyuConfig.FireDelay = v end)
CreateSlider(SecBlackFlash, "Dash Duration (s)", 0, 1, RyuConfig.DashDuration, function(v) RyuConfig.DashDuration = v end)
CreateSlider(SecBlackFlash, "Post Dash Lock Time", 0, 1, RyuConfig.LockTime, function(v) RyuConfig.LockTime = v end)

local SubSideDash = CreateSubTab(TabCombat, "Side Dash")
local SecDashAssist = CreateSection(SubSideDash, "Side Dash Assist")
CreateToggle(SecDashAssist, "Enable Side Dash Assist", Logic.DashAssistState.Enabled, function(v) Logic.DashAssistState.Enabled = v end)
CreateToggle(SecDashAssist, "Lock Camera On Enemy", Logic.DashAssistState.CameraLock, function(v) Logic.DashAssistState.CameraLock = v end)
CreateToggle(SecDashAssist, "Dash Only If Facing Front", Logic.DashAssistState.OnlyIfFacing, function(v) Logic.DashAssistState.OnlyIfFacing = v end)
CreateKeybind(SecDashAssist, "Dash Keybind", Logic.DashAssistState.DashAssistKeybind, function(v) Logic.DashAssistState.DashAssistKeybind = v end)
CreateSlider(SecDashAssist, "Detection Range", 1, 50, Logic.DashAssistState.DetectionRange, function(v) Logic.DashAssistState.DetectionRange = v end)

local SecDashArc = CreateSection(SubSideDash, "Arc Modifiers")
CreateSlider(SecDashArc, "Behind Distance", 1, 15, Logic.DashAssistState.BehindDistance, function(v) Logic.DashAssistState.BehindDistance = v end)
CreateSlider(SecDashArc, "Curve Strength", 0, 25, Logic.DashAssistState.CurveStrength, function(v) Logic.DashAssistState.CurveStrength = v end)
CreateSlider(SecDashArc, "Arch Height", 0, 10, Logic.DashAssistState.ArchHeight, function(v) Logic.DashAssistState.ArchHeight = v end)

--// TAB: VISUALS
local TabVisuals = CreateMainTab("Visuals")
local SubESPOverlays = CreateSubTab(TabVisuals, "Overlays")
local SecESPBox = CreateSection(SubESPOverlays, "ESP Boxes & Lines")
CreateToggle(SecESPBox, "Box ESP", RyuConfig.ESPBox, function(v) RyuConfig.ESPBox = v end)
CreateToggle(SecESPBox, "Corner ESP", RyuConfig.ESPCorner, function(v) RyuConfig.ESPCorner = v end)
CreateToggle(SecESPBox, "Outline ESP", RyuConfig.ESPOutline, function(v) RyuConfig.ESPOutline = v end)
CreateToggle(SecESPBox, "Skeleton ESP", RyuConfig.ESPSkeleton, function(v) RyuConfig.ESPSkeleton = v end)
CreateToggle(SecESPBox, "Head Dot", RyuConfig.ESPHeadDot, function(v) RyuConfig.ESPHeadDot = v end)
CreateToggle(SecESPBox, "Tracer ESP", RyuConfig.ESPTracer, function(v) RyuConfig.ESPTracer = v end)

local SubESPInfo = CreateSubTab(TabVisuals, "Information")
local SecESPInfo = CreateSection(SubESPInfo, "Player Information")
CreateToggle(SecESPInfo, "Name ESP", RyuConfig.ESPName, function(v) RyuConfig.ESPName = v end)
CreateToggle(SecESPInfo, "Character Moveset ESP", RyuConfig.ESPCharacter, function(v) RyuConfig.ESPCharacter = v end)
CreateToggle(SecESPInfo, "Distance ESP", RyuConfig.ESPDistance, function(v) RyuConfig.ESPDistance = v end)
CreateToggle(SecESPInfo, "Kill ESP", RyuConfig.ESPKill, function(v) RyuConfig.ESPKill = v end)
CreateToggle(SecESPInfo, "HP Text", RyuConfig.ESPHPText, function(v) RyuConfig.ESPHPText = v end)
CreateToggle(SecESPInfo, "HP Bar", RyuConfig.ESPHPBar, function(v) RyuConfig.ESPHPBar = v end)
CreateToggle(SecESPInfo, "Cooldown Revealer", RyuConfig.CooldownRevealer, function(v) RyuConfig.CooldownRevealer = v end)

--// TAB: AIMBOT
local TabAimbot = CreateMainTab("Aimbot")
local SubTargetLock = CreateSubTab(TabAimbot, "Target Lock")
local SecLockMain = CreateSection(SubTargetLock, "Main Lock")
CreateToggle(SecLockMain, "Enable Lock", Logic.LockState.Enabled, function(v) Logic.LockState.Enabled = v; if v then SendNotification("Ryu Hub Lock", "Lock ENABLED - searching for targets...", 2) else SendNotification("Ryu Hub Lock", "Lock DISABLED", 2) end end)
CreateKeybind(SecLockMain, "Lock Keybind", RyuConfig.LockKeybind, function(v) RyuConfig.LockKeybind = v end)
CreateToggle(SecLockMain, "Lock ESP", Logic.LockSettings.ESPEnabled, function(v) Logic.LockSettings.ESPEnabled = v end)

-- Dropdown function def
refreshPlayerDropdown = function()
    local vals = {"Auto"}; local charsFolder = workspace:FindFirstChild("Characters")
    for _, p in ipairs(Players:GetPlayers()) do if p ~= Player then table.insert(vals, p.Name) end end
    if charsFolder then
        for _, model in ipairs(charsFolder:GetChildren()) do
            if model:IsA("Model") and not Players:GetPlayerFromCharacter(model) then
                local hum = model:FindFirstChildOfClass("Humanoid")
                if hum and hum.Health > 0 then
                    local npcName = "[NPC] " .. model.Name
                    if not table.find(vals, npcName) then table.insert(vals, npcName) end
                end
            end
        end
    end
    pcall(function()
        if LockPlayerDropdown then LockPlayerDropdown.Refresh(vals) end
        if not table.find(vals, RyuConfig.LockSpecificPlayer) then RyuConfig.LockSpecificPlayer = "Auto" end
    end)
    SendNotification("Ryu Hub Lock", "Player list refreshed! Found " .. (#vals - 1) .. " targets.", 2)
end

CreateButton(SecLockMain, "Refresh Player List", Theme.SectionBG, function() refreshPlayerDropdown() end)
LockPlayerDropdown = CreateDropdown(SecLockMain, "Target Player", {"Auto"}, function(v) RyuConfig.LockSpecificPlayer = v end)

local SecLockCfg = CreateSection(SubTargetLock, "Lock Config")
CreateToggle(SecLockCfg, "Wall Check", Logic.LockSettings.WallCheck, function(v) Logic.LockSettings.WallCheck = v end)
CreateToggle(SecLockCfg, "Auto Retarget", Logic.LockSettings.AutoRetarget, function(v) Logic.LockSettings.AutoRetarget = v end)
CreateSlider(SecLockCfg, "Max Lock Distance", 10, 2000, Logic.LockSettings.MaxDistance, function(v) Logic.LockSettings.MaxDistance = v end)
CreateSlider(SecLockCfg, "Prediction", 0, 1, Logic.LockSettings.Prediction, function(v) Logic.LockSettings.Prediction = v end)

--// TAB: WORLD
local TabWorld = CreateMainTab("World")
local SubTeleport = CreateSubTab(TabWorld, "Teleports")
local SecTeleport = CreateSection(SubTeleport, "Auto Teleport Settings")
CreateDropdown(SecTeleport, "Destination", LocationNames, function(v) currentTPDest = v end)
CreateSlider(SecTeleport, "Teleport Speed (Studs/s)", 30, 1500, Logic.Pathfinding.Speed, function(v) Logic.Pathfinding.Speed = v end)
CreateToggle(SecTeleport, "Show Path Visualization", Logic.Pathfinding.VisualizeOn, function(v) Logic.Pathfinding.VisualizeOn = v end)
CreateButton(SecTeleport, "Start Teleport", Theme.SectionBG, function() local destPos = TeleportLocations[currentTPDest]; if destPos then SendNotification("Teleport", "Pathfinding to " .. currentTPDest .. "...", 2); startPathfinding(destPos) end end)
CreateButton(SecTeleport, "Stop Teleport", Theme.SectionBG, function() stopPathfinding(); SendNotification("Teleport", "Teleport manually stopped.", 2) end)

local SubServers = CreateSubTab(TabWorld, "Servers")
local SecServerList = CreateSection(SubServers, "Server List")

local function refreshSrvDropdown()
    local vals = buildServerMap(Logic.ServerList)
    if SrvDropController then if #vals > 0 then SrvDropController.Refresh(vals) else SrvDropController.Refresh({ "No servers found" }) end end
    updateStatsLabel()
end

SrvDropController = CreateDropdown(SecServerList, "Select Server", {"Click 'Refresh' to load servers"}, function(v) Logic.SelectedServerData = Logic.ServerMap[v]; updateServerInfo(Logic.SelectedServerData) end)
CreateButton(SecServerList, "Refresh Server List", Theme.SectionBG, function() task.spawn(function() fetchServers(); refreshSrvDropdown() end) end)

local SecServerInfo = CreateSection(SubServers, "Server Details")
UI_SrvInfoLabel = CreateLabel(SecServerInfo, "No server selected.\nClick 'Refresh Server List' to load servers.")
UI_SrvStatsLabel = CreateLabel(SecServerInfo, "No data. Refresh to load servers.")
CreateButton(SecServerInfo, "Join Selected Server", Theme.SectionBG, function() if Logic.SelectedServerData and not Logic.SelectedServerData.IsCurrent then SendNotification("Servers", "Teleporting...", 2); TeleportService:TeleportToPlaceInstance(game.PlaceId, Logic.SelectedServerData.JobId, Player) end end)
CreateButton(SecServerInfo, "Join Lowest Ping Server", Theme.SectionBG, function()
    local best = nil
    for _, srv in ipairs(Logic.ServerList) do if not srv.IsCurrent and srv.Playing > 0 and srv.Ping > 0 then if not best or srv.Ping < best.Ping then best = srv end end end
    if best then SendNotification("Servers", "Joining best ping server (" .. best.Ping .. "ms)...", 2); TeleportService:TeleportToPlaceInstance(game.PlaceId, best.JobId, Player) end
end)

--// INIT UI
pcall(function()
    if Tabs[1] and Tabs[1].Btn then Tabs[1].IsOpen = true; Tabs[1].SubContainer.Size = UDim2.new(1, 0, 0, Tabs[1].SubLayout.AbsoluteContentSize.Y); Tabs[1].Btn.TextColor3 = Theme.Text; Tabs[1].Btn.BackgroundColor3 = Theme.SectionBG end
    if Tabs[1] and Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Page then Tabs[1].SubTabs[1].Page.Visible = true; Tabs[1].SubTabs[1].Btn.TextColor3 = Theme.Text end
    UpdateSidebarCanvas()
end)

task.spawn(function() task.wait(1); refreshPlayerDropdown() end)
task.spawn(function() task.wait(2); fetchServers(); refreshSrvDropdown() end)

--// ==========================================
--// BACKGROUND LOOPS & LOGIC HOOKS
--// ==========================================

-- Dash Hook
Logic.Connections.DashInput = UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if not Logic.DashAssistState.Enabled then return end
    if input.KeyCode == Logic.DashAssistState.DashAssistKeybind then
        local direction = "Right"; local char = Player.Character; local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum and hum.MoveDirection.Magnitude > 0 then
            local camRight = workspace.CurrentCamera.CFrame.RightVector * Vector3.new(1, 0, 1)
            if camRight.Magnitude > 0 and hum.MoveDirection:Dot(camRight.Unit) < -0.2 then direction = "Left" end
        end
        task.spawn(function() executeDashArc(direction) end)
    end
end)

-- Blackflash Remote Log
task.spawn(function()
    Logic.TargetRemote = ReplicatedStorage:WaitForChild("Knit"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DivergentFistService"):WaitForChild("RE"):WaitForChild("Activated")
end)

local oldNamecall
oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
    local method = getnamecallmethod()
    if not checkcaller() and method == "FireServer" and self == Logic.TargetRemote and RyuConfig.BlackFlashEnabled then
        Logic.LastFiredTick = tick()
    end
    return oldNamecall(self, ...)
end)

-- Lock & ESP Loop
local lockEspGui = Instance.new("BillboardGui"); lockEspGui.Name = "RyuHubLockESP"; lockEspGui.Size = UDim2.new(0, 150, 0, 150); lockEspGui.AlwaysOnTop = true; lockEspGui.LightInfluence = 0; lockEspGui.Enabled = false; lockEspGui.Adornee = nil
local mainImg = Instance.new("ImageLabel"); mainImg.Name = "Main"; mainImg.BackgroundTransparency = 1; mainImg.Image = "rbxassetid://119160428107030"; mainImg.Size = UDim2.new(0, 110, 0, 110); mainImg.Position = UDim2.new(0.5, 0, 0.5, 0); mainImg.AnchorPoint = Vector2.new(0.5, 0.5); mainImg.Parent = lockEspGui
local markerImg = Instance.new("ImageLabel"); markerImg.Name = "Marker"; markerImg.BackgroundTransparency = 1; markerImg.Image = "rbxassetid://85142373343991"; markerImg.Size = UDim2.new(0, 25, 0, 15); markerImg.Position = UDim2.new(0.5, 0, -0.05, 0); markerImg.AnchorPoint = Vector2.new(0.5, 0.5); markerImg.Parent = lockEspGui
pcall(function() lockEspGui.Parent = CoreGui end); if not lockEspGui.Parent then lockEspGui.Parent = LocalPlayer:WaitForChild("PlayerGui") end
Logic.LockState.ESPGui = lockEspGui

RunService:BindToRenderStep("RyuHubTargetLock", Enum.RenderPriority.Camera.Value + 5, function(dt)
    local char = Player.Character; local hum = char and char:FindFirstChildOfClass("Humanoid"); local root = char and char:FindFirstChild("HumanoidRootPart")
    local isAlive = (hum and hum.Health > 0 and root)

    if not Logic.LockState.Enabled or not isAlive then
        if Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end
        if Logic.LockState.CameraLocked then camera.CameraType = Enum.CameraType.Custom; if hum then camera.CameraSubject = hum end; Logic.LockState.CameraLocked = false end
        if Logic.LockState.WasLockedBody then if hum then hum.AutoRotate = true end; if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy(); Logic.LockState.LockBodyGyro = nil end; Logic.LockState.WasLockedBody = false end
        if not Logic.LockState.Enabled then Logic.LockState.CurrentLockTarget = nil end
        return
    end

    if Logic.LockState.CurrentLockTarget then
        local eHum = Logic.LockState.CurrentLockTarget:FindFirstChildOfClass("Humanoid")
        if not (Logic.LockState.CurrentLockTarget.Parent and eHum and eHum.Health > 0) then
            Logic.LockState.CurrentLockTarget = nil; Logic.LockState.LastTargetName = nil
        else
            local tRoot = Logic.LockState.CurrentLockTarget:FindFirstChild("HumanoidRootPart") or Logic.LockState.CurrentLockTarget:FindFirstChild("Torso")
            if tRoot and (root.Position - tRoot.Position).Magnitude > Logic.LockSettings.MaxDistance and not Logic.LockSettings.Sticky then Logic.LockState.CurrentLockTarget = nil end
        end
    end

    if not Logic.LockSettings.Sticky and Logic.LockState.CurrentLockTarget == nil then Logic.LockState.CurrentLockTarget = getLockTarget()
    elseif not Logic.LockState.CurrentLockTarget then Logic.LockState.CurrentLockTarget = getLockTarget() end

    if not Logic.LockState.CurrentLockTarget or not root or not hum then
        if Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end; return
    end

    local targetPart = Logic.LockState.CurrentLockTarget:FindFirstChild(Logic.LockSettings.TargetPart) or Logic.LockState.CurrentLockTarget:FindFirstChild("HumanoidRootPart")
    if not targetPart then if Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end; return end
    local targetPos = targetPart.Position
    if Logic.LockSettings.Prediction > 0 then local tRoot = Logic.LockState.CurrentLockTarget:FindFirstChild("HumanoidRootPart"); if tRoot and tRoot:IsA("BasePart") then targetPos = targetPos + (tRoot.AssemblyLinearVelocity * Logic.LockSettings.Prediction) end end

    if Logic.LockSettings.ESPEnabled and Logic.LockState.ESPGui then
        Logic.LockState.ESPGui.Enabled = true; Logic.LockState.ESPGui.Adornee = targetPart
        if mainImg then mainImg.Rotation = (tick() * 45) % 360 end
    else
        Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false
    end

    if Logic.LockSettings.Method == "Camera" then
        if Logic.LockState.WasLockedBody then hum.AutoRotate = true; if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy(); Logic.LockState.LockBodyGyro = nil end; Logic.LockState.WasLockedBody = false end
        if not Logic.LockState.CameraLocked then
            Logic.LockState.OrigCamType = camera.CameraType
            local dist = (camera.CFrame.Position - root.Position).Magnitude
            Logic.LockState.ZoomDistance = dist <= 50 and math.clamp(dist, 4, 50) or 10
            Logic.LockState.CameraLocked = true
        end
        camera.CameraType = Enum.CameraType.Scriptable
        local offsetDir = root.Position - targetPos; local flatDir = Vector3.new(offsetDir.X, 0, offsetDir.Z)
        if flatDir.Magnitude < 0.001 then flatDir = -root.CFrame.LookVector; flatDir = Vector3.new(flatDir.X, 0, flatDir.Z) end
        local camPos = root.Position + (flatDir.Unit * Logic.LockState.ZoomDistance) + Vector3.new(0, 2.5, 0)
        local lookCF = CFrame.lookAt(camPos, targetPos); camPos = camPos + (lookCF.RightVector * Logic.LockSettings.SideOffset)

        local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, Logic.LockState.CurrentLockTarget}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
        local rayStart = root.Position + Vector3.new(0, 1.5, 0); local rayResult = workspace:Raycast(rayStart, camPos - rayStart, rayParams)
        if rayResult then camPos = rayResult.Position + (lookCF.LookVector * 0.5) end
        
        local desiredCF = CFrame.lookAt(camPos, targetPos)
        if Logic.LockSettings.Smoothness <= 0 then camera.CFrame = desiredCF else local rate = 30 / Logic.LockSettings.Smoothness; camera.CFrame = camera.CFrame:Lerp(desiredCF, math.clamp(1 - math.exp(-rate * dt), 0, 1)) end
    end
end)

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == RyuConfig.LockKeybind then
        Logic.LockState.Enabled = not Logic.LockState.Enabled
    end
end)

Logic.Connections.ESP = RunService.Heartbeat:Connect(function()
    local cam = workspace.CurrentCamera
    if not cam then return end

    local entities = getEntities()
    local alive = {}

    for _, data in ipairs(entities) do
        local char = data.Model
        local plr = data.Player
        local dName = data.Name
        alive[char] = true

        if plr == Player then
            if Logic.ESPObjects[char] then
                hideAll(Logic.ESPObjects[char])
                if Logic.ESPObjects[char].HL then Logic.ESPObjects[char].HL.Enabled = false end
            end
            continue
        end

        local hrp = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
        local hum = char:FindFirstChildOfClass("Humanoid")
        local head = char:FindFirstChild("Head")

        if not Logic.ESPObjects[char] then Logic.ESPObjects[char] = createESP() end
        local e = Logic.ESPObjects[char]

        if not (hrp and hum and hum.Health > 0) then
            hideAll(e)
            if e.HL then e.HL.Enabled = false end
            continue
        end

        local dist = (cam.CFrame.Position - hrp.Position).Magnitude
        local box = getCharBounds(char)
        
        if RyuConfig.ESPBox and box then
            for i = 1, 4 do e.BoxL[i].Color = RyuConfig.ESPBoxColor end
            local tl = Vector2.new(box.X, box.Y); local tr = Vector2.new(box.X + box.W, box.Y)
            local bl = Vector2.new(box.X, box.Y + box.H); local br = Vector2.new(box.X + box.W, box.Y + box.H)
            setLinePair(e.BoxS[1], e.BoxL[1], tl, tr, true); setLinePair(e.BoxS[2], e.BoxL[2], tr, br, true)
            setLinePair(e.BoxS[3], e.BoxL[3], br, bl, true); setLinePair(e.BoxS[4], e.BoxL[4], bl, tl, true)
        else
            for i = 1, 4 do e.BoxS[i].Visible = false; e.BoxL[i].Visible = false end
        end
        
        if RyuConfig.ESPCorner and box then
            for i = 1, 8 do e.CorL[i].Color = RyuConfig.ESPCornerColor end
            local x, y, w, h = box.X, box.Y, box.W, box.H
            local len = math.clamp(math.min(w, h) * 0.25, 5, 9999)
            local tl = Vector2.new(x, y); local tr = Vector2.new(x + w, y)
            local bl = Vector2.new(x, y + h); local br = Vector2.new(x + w, y + h)
            local segs = { {tl, tl + Vector2.new(len, 0)}, {tl, tl + Vector2.new(0, len)}, {tr, tr + Vector2.new(-len, 0)}, {tr, tr + Vector2.new(0, len)}, {bl, bl + Vector2.new(len, 0)}, {bl, bl + Vector2.new(0, -len)}, {br, br + Vector2.new(-len, 0)}, {br, br + Vector2.new(0, -len)} }
            for i = 1, 8 do setLinePair(e.CorS[i], e.CorL[i], segs[i][1], segs[i][2], true) end
        else
            for i = 1, 8 do e.CorS[i].Visible = false; e.CorL[i].Visible = false end
        end

        if RyuConfig.ESPSkeleton then
            local bones = char:FindFirstChild("UpperTorso") and Logic.R15Bones or Logic.R6Bones
            local idx = 0
            for _, b in ipairs(bones) do
                idx += 1
                local p1 = char:FindFirstChild(b[1])
                local p2 = char:FindFirstChild(b[2])
                if p1 and p2 then
                    local s1, o1 = w2v(p1.Position)
                    local s2, o2 = w2v(p2.Position)
                    if o1 and o2 then
                        e.Skel[idx].Color = RyuConfig.ESPSkeletonColor
                        e.Skel[idx].From = s1; e.Skel[idx].To = s2; e.Skel[idx].Visible = true
                    else e.Skel[idx].Visible = false end
                else e.Skel[idx].Visible = false end
            end
            for i = idx + 1, 14 do e.Skel[i].Visible = false end
        else
            for i = 1, 14 do e.Skel[i].Visible = false end
        end

        if RyuConfig.ESPHeadDot and head then
            local hs, ho = w2v(head.Position)
            if ho then
                local headTop = w2v(head.Position + Vector3.new(0, head.Size.Y * 0.5, 0))
                local r = headTop and math.abs(hs.Y - headTop.Y) or 5
                r = math.clamp(r, 3, 50)
                e.HDot.Color = RyuConfig.ESPHeadDotColor
                e.HDotS.Position = hs; e.HDotS.Radius = r + 1; e.HDotS.Visible = true
                e.HDot.Position  = hs; e.HDot.Radius  = r;     e.HDot.Visible  = true
            else
                e.HDotS.Visible = false; e.HDot.Visible = false
            end
        else
            e.HDotS.Visible = false; e.HDot.Visible = false
        end

        if RyuConfig.ESPTracer then
            local fp, fo = w2v(hrp.Position - Vector3.new(0, 3, 0))
            if fo then
                local bot = Vector2.new(cam.ViewportSize.X / 2, cam.ViewportSize.Y)
                e.Trc.Color = RyuConfig.ESPTracerColor
                e.TrcS.From = bot; e.TrcS.To = fp; e.TrcS.Visible = true
                e.Trc.From  = bot; e.Trc.To  = fp; e.Trc.Visible  = true
            else
                e.TrcS.Visible = false; e.Trc.Visible = false
            end
        else
            e.TrcS.Visible = false; e.Trc.Visible = false
        end

        if RyuConfig.ESPHPText and box then
            local f = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
            e.HPTxt.Position = Vector2.new(box.X + box.W + 4, box.Y)
            e.HPTxt.Text = math.floor(hum.Health) .. "/" .. math.floor(hum.MaxHealth)
            e.HPTxt.Color = hpColor(f)
            e.HPTxt.Visible = true
        else
            e.HPTxt.Visible = false
        end

        if RyuConfig.ESPHPBar and box then
            local f = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
            local bx = box.X - 6
            local top = Vector2.new(bx, box.Y)
            local bot = Vector2.new(bx, box.Y + box.H)
            local ft = Vector2.new(bx, box.Y + box.H * (1 - f))

            e.BarO.From = top; e.BarO.To = bot; e.BarO.Visible = true
            e.BarBG.From = top; e.BarBG.To = bot; e.BarBG.Visible = true
            e.BarF.From = ft; e.BarF.To = bot; e.BarF.Color = hpColor(f); e.BarF.Visible = f > 0
        else
            e.BarO.Visible = false; e.BarBG.Visible = false; e.BarF.Visible = false
        end
        
        if RyuConfig.ESPOutline then
            if not e.HL or e.HL.Parent ~= char then
                safeRM(e.HL)
                local hl = Instance.new("Highlight")
                hl.FillTransparency = 1
                hl.OutlineColor = RyuConfig.ESPOutlineColor
                hl.OutlineTransparency = 0
                hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                hl.Parent = char
                e.HL = hl
            end
            e.HL.OutlineColor = RyuConfig.ESPOutlineColor
            e.HL.Enabled = true
        else
            if e.HL then e.HL.Enabled = false end
        end

        if RyuConfig.CooldownRevealer then
            if not e.CooldownCleanup then e.RevealGui, e.CooldownCleanup = CreateCooldownBillboard(char, plr, hrp) end
            if e.RevealGui then e.RevealGui.Enabled = true end
        else
            if e.CooldownCleanup then e.CooldownCleanup(); e.CooldownCleanup = nil; e.RevealGui = nil end
        end

        if box then
            local ay = box.Y - 2
            if RyuConfig.ESPCharacter then
                ay = ay - 14
                local ms = char:GetAttribute("Moveset") or (plr and plr:GetAttribute("Moveset")) or "Unknown"
                e.CharTxt.Color = RyuConfig.ESPCharacterColor
                e.CharTxt.Position = Vector2.new(box.CX, ay)
                e.CharTxt.Text = "[" .. tostring(ms) .. "]"
                e.CharTxt.Visible = true
            else
                e.CharTxt.Visible = false
            end

            if RyuConfig.ESPName then
                ay = ay - 16
                e.NameTxt.Color = RyuConfig.ESPNameColor
                e.NameTxt.Position = Vector2.new(box.CX, ay)
                e.NameTxt.Text = dName
                e.NameTxt.Visible = true
            else
                e.NameTxt.Visible = false
            end
            
            local by = box.Y + box.H + 2
            if RyuConfig.ESPDistance then
                e.DistTxt.Color = RyuConfig.ESPDistanceColor
                e.DistTxt.Position = Vector2.new(box.CX, by)
                e.DistTxt.Text = math.floor(dist) .. " studs"
                e.DistTxt.Visible = true
                by = by + 14
            else
                e.DistTxt.Visible = false
            end

            if RyuConfig.ESPKill then
                local kills = 0
                if plr then
                    local ls = plr:FindFirstChild("leaderstats")
                    if ls then
                        local ks = ls:FindFirstChild("Kills") or ls:FindFirstChild("kills") or ls:FindFirstChild("KOs") or ls:FindFirstChild("KO")
                        if ks then kills = ks.Value end
                    end
                end
                e.KillTxt.Color = RyuConfig.ESPKillColor
                e.KillTxt.Position = Vector2.new(box.CX, by)
                e.KillTxt.Text = "Kills: " .. tostring(kills)
                e.KillTxt.Visible = true
            else
                e.KillTxt.Visible = false
            end
        else
            e.NameTxt.Visible = false; e.CharTxt.Visible = false; e.DistTxt.Visible = false; e.KillTxt.Visible = false
        end
    end

    for char, e in pairs(Logic.ESPObjects) do
        if not alive[char] or not char.Parent then
            hideAll(e)
            if e.HL then e.HL.Enabled = false; safeRM(e.HL) end
            removeAll(e)
            Logic.ESPObjects[char] = nil
        end
    end
end)

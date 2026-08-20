-- ============================================================================
-- IMPEL DOWN TEST LABOR (ISOLATED)
-- ============================================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local var = "HumanoidRootPart" -- Der in deinem Skript definierte Root-Name

-- ============================================================================
-- 1. MINIMAL TEST GUI
-- ============================================================================
local TestGui = Instance.new("ScreenGui", gethui and gethui() or CoreGui or player.PlayerGui)
TestGui.Name = "ImpelTestGUI"

local MainFrame = Instance.new("Frame", TestGui)
MainFrame.Size = UDim2.new(0, 200, 0, 80)
MainFrame.Position = UDim2.new(0.5, -100, 0.8, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", MainFrame).Color = Color3.fromRGB(255, 100, 100)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 25)
Title.BackgroundTransparency = 1
Title.Text = "Impel Down Tester"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold

local ToggleBtn = Instance.new("TextButton", MainFrame)
ToggleBtn.Size = UDim2.new(0.8, 0, 0, 35)
ToggleBtn.Position = UDim2.new(0.1, 0, 0, 35)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
ToggleBtn.Text = "START IMPEL DOWN"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(0, 4)

-- ============================================================================
-- 2. DEPENDENCIES (Movement, Stats, etc.)
-- ============================================================================
getgenv().auto_impel = false
getgenv().tween_speed = 1

local function snapToPosition(targetPosition)
    if player.Character and player.Character:FindFirstChild(var) then
        player.Character[var].CFrame = CFrame.new(targetPosition)
        player.Character[var].Velocity = Vector3.zero
    end
end

local function isWithinDistance2(target, distance, horizontalOnly)
    if not player or not player.Character or not player.Character:FindFirstChild(var) then return false end
    local rootPosition = player.Character[var].Position
    local targetPos = typeof(target) == "Instance" and target:GetPivot().Position or (typeof(target) == "CFrame" and target.Position or target)
    
    if horizontalOnly then
        local deltaX = rootPosition.X - targetPos.X
        local deltaZ = rootPosition.Z - targetPos.Z
        return math.sqrt(deltaX * deltaX + deltaZ * deltaZ) <= distance
    end
    return (rootPosition - targetPos).Magnitude <= distance
end

local function tweenTeleportPro(options)
    -- Vereinfachter Fallback für den Test, damit der Code nicht crasht
    local target = options.Position
    if target then snapToPosition(target) end
    if options.Stop and options.Stop() == false then return end
end

local function tweenToPos2(target, speed, flightHeight, maxHeight, usePathfinding)
    -- Vereinfachter Fallback für den Test
    local targetPos = typeof(target) == "Instance" and target:GetPivot().Position or target
    snapToPosition(targetPos)
end

local function fakeGeppo()
    pcall(function()
        local stats_folder = ReplicatedStorage:FindFirstChild("Stats" .. player.Name)
        if stats_folder and stats_folder.Skills:FindFirstChild("skyWalk") and stats_folder.Skills.skyWalk.Value == true then
            ReplicatedStorage.Events.Skill:InvokeServer("Sky Walk2", { char = player.Character, cf = player.Character[var].CFrame })
        else
            ReplicatedStorage.Events.Skill:InvokeServer("Geppo", { char = player.Character, cf = player.Character[var].CFrame })
        end
    end)
end

-- ============================================================================
-- 3. IMPEL DOWN UTILITIES
-- ============================================================================
local function getStat(statName)
    local amount = 0
    pcall(function()
        for _, statFrame in next, player.PlayerGui.Statistics.Main.Stats:GetChildren() do
            local statLabel = statFrame:FindFirstChild("Stat", true)
            local amountLabel = statFrame:FindFirstChild("Amount", true)
            if statLabel and amountLabel and statLabel.Text == statName then
                amount = tonumber(amountLabel.Text)
            end
        end
    end)
    return amount
end

local function getCurrentFloor()
    local floor = 1
    pcall(function()
        for _, timer in next, player.PlayerGui.ImpelDownUI.Info.Timers:GetChildren() do
            if timer.Name:match("Floor") then
                local floorDigits = timer.Name:match("%d+")
                if floorDigits then
                    floor = tonumber(floorDigits)
                end
            end
        end
    end)
    return floor
end

local zonesReally = { Vector3.new(2951, 2081, -13983) }
local function hasZone()
    for _, npc in next, Workspace.NPCs:GetChildren() do
        if npc:IsA("Model") and npc:FindFirstChild("Humanoid") then
            for _, zone in next, zonesReally do
                if (npc:GetPivot().Position - zone).Magnitude < 150 then
                    return zone
                end
            end
        end
    end
    return nil
end

local function putMines()
    local minesRemote = ReplicatedStorage:FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Mines")
    if not minesRemote then
        task.spawn(function() ReplicatedStorage:FindFirstChild("Events"):FindFirstChild("Skill"):InvokeServer("Explosive Mines") end)
    else
        task.wait(1.5)
        minesRemote:InvokeServer({ cf = player.Character[var].CFrame })
    end
end

local function lootChest()
    local closestChest = nil
    local closestDistance = math.huge
    for _, effect in next, Workspace.Effects:GetChildren() do
        if effect:IsA("Model") and effect:FindFirstChildWhichIsA("ProximityPrompt", true) and effect:FindFirstChildWhichIsA("ProximityPrompt", true).Parent:IsA("MeshPart") then
            local distance = (effect:GetPivot().Position - player.Character:GetPivot().Position).Magnitude
            if distance < closestDistance then
                closestChest = effect
                closestDistance = distance
            end
        end
    end
    if closestChest then
        if not isWithinDistance2(closestChest:GetPivot().Position, 10, true) then
            tweenToPos2(closestChest:GetPivot().Position, 50, 30, 2090)
        else
            local prompt = closestChest:FindFirstChild("ProximityPrompt", true)
            local savedCameraCFrame = camera.CFrame
            player.Character[var].CFrame = CFrame.new(prompt.Parent.Position.X, prompt.Parent.Position.Y + 5, prompt.Parent.Position.Z) * CFrame.Angles(-math.pi / 2, 0, 0)
            camera.CameraSubject = prompt.Parent
            camera.CFrame = CFrame.lookAt(player.Character[var].Position, prompt.Parent.Position)
            task.wait()
            prompt:InputHoldBegin()
            task.wait(prompt.HoldDuration + 0.1)
            prompt:InputHoldEnd()
            camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
            camera.CFrame = savedCameraCFrame
        end
    end
end

-- ============================================================================
-- 4. THE IMPEL DOWN MAIN LOOP
-- ============================================================================
local function StartImpelDown()
    task.spawn(function()
        while getgenv().auto_impel and task.wait(1) do
            fakeGeppo()
            if player.Character:FindFirstChild("Bomb-Bomb") or player.Backpack:FindFirstChild("Bomb-Bomb") then
                if getStat("Devil Fruit") < 700 then
                    ReplicatedStorage:WaitForChild("Events"):WaitForChild("stats"):FireServer(unpack({ "DevilFruitMastery", nil, 700 }))
                end
                local leapRemote = ReplicatedStorage:FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Leap")
                if not leapRemote then
                    ReplicatedStorage:WaitForChild("Events"):WaitForChild("Skill"):InvokeServer(unpack({ "Explosive Leap" }))
                else
                    leapRemote:FireServer({ cf = CFrame.new(0, 0, 0), extra = 0 })
                end
            end
        end
    end)

    while getgenv().auto_impel and task.wait() do
        if game.PlaceId ~= 11424731604 then
            local impelEntrance = Vector3.new(5878, 9, -10214)
            if not isWithinDistance2(impelEntrance, 10, true) then
                tweenTeleportPro({ Position = impelEntrance, Speed = 50, Stop = function() return getgenv().auto_impel end })
            else
                snapToPosition(impelEntrance)
            end
        elseif not player.PlayerGui:FindFirstChild("diffChooser") then
            
            -- FLOOR 1
            if getCurrentFloor() == 1 then
                if isWithinDistance2(Vector3.new(5906, 9, -10197), 300, true) then
                    local vera = Workspace.NPCs:FindFirstChild("Vera")
                    if vera and not isWithinDistance2(vera:GetPivot().Position, 10, true) then
                        tweenTeleportPro({ Position = vera:GetPivot().Position, Speed = 50, Stop = function() return getgenv().auto_impel end })
                    elseif vera and isWithinDistance2(vera:GetPivot().Position, 10, true) then
                        snapToPosition(vera:GetPivot().Position)
                        task.spawn(function()
                            if getgenv().global_hit then getgenv().global_hit(vera) end -- Ersetze dies durch deinen Combat!
                        end)
                    end
                elseif player.Character:FindFirstChild("LeftCuff") then
                    local keyModel = Workspace.Effects:FindFirstChild("Key")
                    if keyModel then
                        if not isWithinDistance2(keyModel:GetPivot().Position, 10, true) then
                            tweenTeleportPro({ Position = keyModel:GetPivot().Position, y = keyModel:GetPivot().Position.Y, Speed = 50, Stop = function() return getgenv().auto_impel end })
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
                elseif not player.Backpack:FindFirstChild("Bomb-Bomb") then
                    if not player.Character:FindFirstChild("Bomb") then
                        if player.Backpack:FindFirstChild("Bomb") then
                            player.Backpack:FindFirstChild("Bomb").Parent = player.Character
                            task.wait(1)
                        elseif Workspace.Effects:FindFirstChild("Bomb") then
                            if not isWithinDistance2(Workspace.Effects:FindFirstChild("Bomb"):GetPivot().Position, 10, true) then
                                tweenToPos2(Workspace.Effects:FindFirstChild("Bomb"):GetPivot().Position, 50, 30, 2090)
                            else
                                local bombPrompt = Workspace.Effects:FindFirstChild("Bomb"):FindFirstChild("ProximityPrompt", true)
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
                        -- ACHTUNG: Hier nutzt du 'firesignal', was in manchen Executors crashen kann, wenn es nicht unterstützt wird.
                        pcall(function() firesignal(player.PlayerGui.ConfirmationPrompt.Main.OptionsFrame.Accept.MouseButton1Click) end)
                    end
                elseif player.Backpack:FindFirstChild("Bomb-Bomb") then
                    local startBarrier = Workspace.Islands["Impel Base - Floor 1"].Barriers:FindFirstChild("StartBarrier")
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
                        if Workspace.Effects.Zones:FindFirstChild("End") then
                            local endPosition = Workspace.Effects.Zones:FindFirstChild("End").Position + zoneOffset
                            if not isWithinDistance2(endPosition, 10, true) then
                                tweenTeleportPro({ Position = endPosition, Speed = 50, Stop = function() return getgenv().auto_impel end }) -- Hier tweenToPos ersetzt, da es in der Dependency fehlte
                            else
                                snapToPosition(endPosition + Vector3.new(0, 4, 0))
                                putMines()
                            end
                        else
                            tweenTeleportPro({ Position = Vector3.new(2880, 2783, -14473) + zoneOffset, Speed = 50, Stop = function() return getgenv().auto_impel end })
                        end
                    end
                end

            -- FLOOR 2
            elseif getCurrentFloor() == 2 then
                if isWithinDistance2(Vector3.new(3199, 2405, -20056), 50, true) then
                    tweenTeleportPro({ Position = Vector3.new(3201, 2384, -20276), Speed = 50, Stop = function() return getgenv().auto_impel end })
                end
                -- if not HasRange() then end -- Diese Funktion ist in deinem originalen Skript leer.
            end
        end
    end
end

-- ============================================================================
-- 5. BUTTON LOGIC
-- ============================================================================
ToggleBtn.MouseButton1Click:Connect(function()
    getgenv().auto_impel = not getgenv().auto_impel
    if getgenv().auto_impel then
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
        ToggleBtn.Text = "STOP IMPEL DOWN"
        StartImpelDown()
    else
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
        ToggleBtn.Text = "START IMPEL DOWN"
    end
end)

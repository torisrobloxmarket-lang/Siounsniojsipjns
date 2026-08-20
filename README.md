--// ============================================================================
--// IMPEL DOWN STANDALONE TEST SCRIPT (GPO)
--// ============================================================================

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local CoreGui = game:GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer
getgenv().auto_impel = false

--// ============================================================================
--// HELPER FUNCTIONS
--// ============================================================================

local function getRoot()
    local char = LocalPlayer.Character
    return char and char:FindFirstChild("HumanoidRootPart")
end

local function getHumanoid()
    local char = LocalPlayer.Character
    return char and char:FindFirstChildOfClass("Humanoid")
end

local function isWithinDistance(targetPos, maxDist)
    local root = getRoot()
    if not root then return false end
    return (Vector3.new(root.Position.X, 0, root.Position.Z) - Vector3.new(targetPos.X, 0, targetPos.Z)).Magnitude <= maxDist
end

local function snapToPosition(targetPos)
    local root = getRoot()
    if root then
        root.CFrame = CFrame.new(targetPos)
        root.Velocity = Vector3.zero
    end
end

-- Simpler Tween für das Test-Skript (ersetzt das riesige tweenTeleportPro)
local function tweenToPos(targetPos, speed)
    local root = getRoot()
    if not root then return end
    local dist = (root.Position - targetPos).Magnitude
    local timeToTravel = dist / (speed or 50)
    
    local tween = TweenService:Create(root, TweenInfo.new(timeToTravel, Enum.EasingStyle.Linear), {CFrame = CFrame.new(targetPos)})
    tween:Play()
    tween.Completed:Wait()
end

local function fakeGeppo()
    local char = LocalPlayer.Character
    local stats = ReplicatedStorage:FindFirstChild("Stats" .. LocalPlayer.Name)
    if not char or not stats then return end
    
    local skillEvent = ReplicatedStorage:WaitForChild("Events"):WaitForChild("Skill")
    if stats.Skills:FindFirstChild("skyWalk") and stats.Skills.skyWalk.Value == true then
        skillEvent:InvokeServer("Sky Walk2", { char = char, cf = char:GetPivot() })
    else
        skillEvent:InvokeServer("Geppo", { char = char, cf = char:GetPivot() })
    end
end

local function getStat(statName)
    local amount = 0
    pcall(function()
        for _, statFrame in pairs(LocalPlayer.PlayerGui.Statistics.Main.Stats:GetChildren()) do
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
        for _, timer in pairs(LocalPlayer.PlayerGui.ImpelDownUI.Info.Timers:GetChildren()) do
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
    for _, npc in pairs(Workspace.NPCs:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("Humanoid") then
            for _, zone in pairs(zonesReally) do
                if (npc:GetPivot().Position - zone).Magnitude < 150 then
                    return zone
                end
            end
        end
    end
    return nil
end

local function putMines()
    local minesRemote = ReplicatedStorage:FindFirstChild(LocalPlayer.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Mines")
    if not minesRemote then
        task.spawn(function()
            ReplicatedStorage.Events.Skill:InvokeServer("Explosive Mines")
        end)
    else
        task.wait(1.5)
        local root = getRoot()
        if root then
            minesRemote:InvokeServer({ cf = root.CFrame })
        end
    end
end

local function basicHit(target)
    if not target or not target:FindFirstChild("HumanoidRootPart") then return end
    local root = getRoot()
    if not root then return end
    
    local combatRegister = ReplicatedStorage.Events:FindFirstChild("CombatRegister")
    if combatRegister then
        pcall(function()
            combatRegister:InvokeServer({ "swingsfx", "Melee", 1, "Ground", false, nil, 1.75, 2, root.CFrame })
            task.wait(0.15)
            combatRegister:InvokeServer({ "damage", {target:FindFirstChild("HumanoidRootPart")}, "Melee", { 1, "Ground", "Melee" }, true, root.CFrame, aircombo = "Ground" })
        end)
    end
end

local function lootChest()
    local closestChest, closestDistance = nil, math.huge
    for _, effect in pairs(Workspace.Effects:GetChildren()) do
        local prompt = effect:FindFirstChildWhichIsA("ProximityPrompt", true)
        if prompt and effect:IsA("Model") and prompt.Parent:IsA("MeshPart") then
            local dist = (effect:GetPivot().Position - getRoot().Position).Magnitude
            if dist < closestDistance then
                closestChest = effect
                closestDistance = dist
            end
        end
    end
    
    if closestChest then
        if not isWithinDistance(closestChest:GetPivot().Position, 10) then
            tweenToPos(closestChest:GetPivot().Position, 50)
        else
            local prompt = closestChest:FindFirstChild("ProximityPrompt", true)
            snapToPosition(prompt.Parent.Position + Vector3.new(0, 5, 0))
            task.wait(0.1)
            prompt:InputHoldBegin()
            task.wait(prompt.HoldDuration + 0.1)
            prompt:InputHoldEnd()
        end
    end
end

--// ============================================================================
--// MAIN IMPEL DOWN LOGIC
--// ============================================================================

local function ImpelDownLoop()
    -- Sub-Thread für Geppo & Skills
    task.spawn(function()
        while getgenv().auto_impel do
            pcall(fakeGeppo)
            if LocalPlayer.Character:FindFirstChild("Bomb-Bomb") or LocalPlayer.Backpack:FindFirstChild("Bomb-Bomb") then
                if getStat("Devil Fruit") < 700 then
                    ReplicatedStorage.Events.stats:FireServer("DevilFruitMastery", nil, 700)
                end
                local leapRemote = ReplicatedStorage:FindFirstChild(LocalPlayer.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Leap")
                if not leapRemote then
                    ReplicatedStorage.Events.Skill:InvokeServer("Explosive Leap")
                else
                    leapRemote:FireServer({ cf = CFrame.new(0, 0, 0), extra = 0 })
                end
            end
            task.wait(1)
        end
    end)

    -- Haupt-Logik Loop
    while getgenv().auto_impel do
        task.wait(0.1)
        local root = getRoot()
        if not root then continue end

        -- Prüfe, ob wir im richtigen Spiel/Ort sind (Impel Down Place ID)
        if game.PlaceId ~= 11424731604 then
            local impelEntrance = Vector3.new(5878, 9, -10214)
            if not isWithinDistance(impelEntrance, 10) then
                tweenToPos(impelEntrance, 75)
            else
                snapToPosition(impelEntrance)
            end
        else
            -- Wenn diffChooser weg ist, startet der Run
            if not LocalPlayer.PlayerGui:FindFirstChild("diffChooser") then
                local currentFloor = getCurrentFloor()
                
                -- FLOOR 1
                if currentFloor == 1 then
                    if isWithinDistance(Vector3.new(5906, 9, -10197), 300) then
                        local vera = Workspace.NPCs:FindFirstChild("Vera")
                        if vera then
                            if not isWithinDistance(vera:GetPivot().Position, 10) then
                                tweenToPos(vera:GetPivot().Position, 50)
                            else
                                snapToPosition(vera:GetPivot().Position)
                                basicHit(vera)
                            end
                        end
                    elseif LocalPlayer.Character:FindFirstChild("LeftCuff") then
                        local keyModel = Workspace.Effects:FindFirstChild("Key")
                        if keyModel then
                            if not isWithinDistance(keyModel:GetPivot().Position, 10) then
                                tweenToPos(keyModel:GetPivot().Position, 50)
                            else
                                snapToPosition(keyModel:GetPivot().Position)
                                local prompt = keyModel:FindFirstChild("ProximityPrompt", true)
                                if prompt then
                                    prompt:InputHoldBegin()
                                    task.wait(prompt.HoldDuration)
                                    prompt:InputHoldEnd()
                                end
                            end
                        end
                    elseif not LocalPlayer.Backpack:FindFirstChild("Bomb-Bomb") then
                        if not LocalPlayer.Character:FindFirstChild("Bomb") then
                            if LocalPlayer.Backpack:FindFirstChild("Bomb") then
                                LocalPlayer.Backpack.Bomb.Parent = LocalPlayer.Character
                                task.wait(1)
                            elseif Workspace.Effects:FindFirstChild("Bomb") then
                                local bomb = Workspace.Effects.Bomb
                                if not isWithinDistance(bomb:GetPivot().Position, 10) then
                                    tweenToPos(bomb:GetPivot().Position, 50)
                                else
                                    local bombPrompt = bomb:FindFirstChild("ProximityPrompt", true)
                                    if bombPrompt then
                                        bombPrompt:InputHoldBegin()
                                        task.wait(bombPrompt.HoldDuration + 0.1)
                                        bombPrompt:InputHoldEnd()
                                    end
                                end
                            else
                                lootChest()
                            end
                        elseif not LocalPlayer.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                            LocalPlayer.Character.Bomb:Activate()
                        else
                            pcall(function()
                                -- Bestätigt den Dialog zum Essen der Bombe
                                firesignal(LocalPlayer.PlayerGui.ConfirmationPrompt.Main.OptionsFrame.Accept.MouseButton1Click)
                            end)
                        end
                    elseif LocalPlayer.Backpack:FindFirstChild("Bomb-Bomb") then
                        local startBarrier = Workspace.Islands["Impel Base - Floor 1"].Barriers:FindFirstChild("StartBarrier")
                        if startBarrier and startBarrier.CanCollide then
                            tweenToPos(startBarrier.Position, 50)
                        end
                        
                        local targetZone = hasZone()
                        if targetZone then
                            if not isWithinDistance(targetZone, 10) then
                                tweenToPos(targetZone, 50)
                            else
                                snapToPosition(targetZone + Vector3.new(0, 4, 0))
                                putMines()
                            end
                        else
                            local zoneOffset = Vector3.new(0, 0, -40)
                            if Workspace.Effects.Zones:FindFirstChild("End") then
                                local endPosition = Workspace.Effects.Zones.End.Position + zoneOffset
                                if not isWithinDistance(endPosition, 10) then
                                    tweenToPos(endPosition, 50)
                                else
                                    snapToPosition(endPosition + Vector3.new(0, 4, 0))
                                    putMines()
                                end
                            else
                                tweenToPos(Vector3.new(2880, 2783, -14473) + zoneOffset, 50)
                            end
                        end
                    end
                    
                -- FLOOR 2 (Aktuell im Original leer/unfertig, dient als Placeholder)
                elseif currentFloor == 2 then
                    if isWithinDistance(Vector3.new(3199, 2405, -20056), 50) then
                        tweenToPos(Vector3.new(3201, 2384, -20276), 50)
                    end
                end
            end
        end
    end
end

--// ============================================================================
--// SIMPLE TEST GUI
--// ============================================================================

local testGui = Instance.new("ScreenGui")
testGui.Name = "ImpelTestGUI"
testGui.ResetOnSpawn = false
pcall(function() testGui.Parent = CoreGui end)

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 200, 0, 100)
mainFrame.Position = UDim2.new(0.5, -100, 0.8, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true -- Einfaches Dragging für das Test-GUI
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 8)
mainFrame.Parent = testGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "IMPEL DOWN TEST"
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
    getgenv().auto_impel = not getgenv().auto_impel
    if getgenv().auto_impel then
        toggleBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        toggleBtn.Text = "STOP IMPEL FARM"
        task.spawn(ImpelDownLoop)
    else
        toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        toggleBtn.Text = "START IMPEL FARM"
    end
end)

print("✅ Impel Down Test GUI geladen!")

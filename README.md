--// ==========================================
--// RYU HUB: IMPEL DOWN ENGINE (CUSTOM UI & 100% PERFECT LOGIC)
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local HttpService = game:GetService("HttpService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local PathfindingService = game:GetService("PathfindingService")
local GuiService = game:GetService("GuiService")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

--// ANTI-CHEAT BYPASS (Adonis & ClientMovers)
task.spawn(function()
    pcall(function()
        for _, v in ipairs(game:GetDescendants()) do
            if v.Name:lower():match("adonis") or v.Name:match("ClientMover") or v.Name == "__FUNCTION" then
                v:Destroy()
            end
        end
    end)
end)

--// GUI CLEANUP
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end end)
for _, v in pairs(guiParent:GetChildren()) do if v.Name == "RyuHubGPO" then v:Destroy() end end

--// CONFIG SYSTEM
_G.RyuConfig = {
    AutoImpelDown = false,
    AntiAFK = false
}

--// NEW CUSTOM UI BUILDER
local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubGPO"
RyuHub.ResetOnSpawn = false
RyuHub.Parent = guiParent

local function MakeDraggable(gui)
    local dragging, dragInput, dragStart, startPos
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = gui.Position
        end
    end)
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    gui.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end)
end

-- Mobile Toggle Button
local OpenBtn = Instance.new("TextButton", RyuHub)
OpenBtn.Size = UDim2.new(0, 50, 0, 50)
OpenBtn.Position = UDim2.new(0, 20, 0, 20)
OpenBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
OpenBtn.Text = "RYU"
OpenBtn.TextColor3 = Color3.fromRGB(0, 150, 255)
OpenBtn.Font = Enum.Font.GothamBlack
OpenBtn.TextSize = 16
Instance.new("UICorner", OpenBtn).CornerRadius = UDim.new(1, 0)
Instance.new("UIStroke", OpenBtn).Color = Color3.fromRGB(0, 150, 255)
Instance.new("UIStroke", OpenBtn).Thickness = 2
MakeDraggable(OpenBtn)

-- Main Window
local MainFrame = Instance.new("Frame", RyuHub)
MainFrame.Size = UDim2.new(0, 320, 0, 400)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 18)
MainFrame.Visible = false
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", MainFrame).Color = Color3.fromRGB(40, 40, 50)
MakeDraggable(MainFrame)

-- Title
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "  RYU HUB | IMPEL DOWN"
Title.TextColor3 = Color3.fromRGB(0, 150, 255)
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 16
Title.TextXAlignment = Enum.TextXAlignment.Left
local Sep = Instance.new("Frame", MainFrame)
Sep.Size = UDim2.new(1, 0, 0, 1); Sep.Position = UDim2.new(0, 0, 0, 40); Sep.BackgroundColor3 = Color3.fromRGB(40, 40, 50)

-- Close Button
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30); CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundTransparency = 1; CloseBtn.Text = "X"; CloseBtn.TextColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.TextSize = 16
CloseBtn.MouseButton1Click:Connect(function() MainFrame.Visible = false end)

OpenBtn.MouseButton1Click:Connect(function() MainFrame.Visible = not MainFrame.Visible end)

-- Scrolling Content
local Scroll = Instance.new("ScrollingFrame", MainFrame)
Scroll.Size = UDim2.new(1, -10, 1, -50); Scroll.Position = UDim2.new(0, 5, 0, 45)
Scroll.BackgroundTransparency = 1; Scroll.ScrollBarThickness = 4
local Layout = Instance.new("UIListLayout", Scroll)
Layout.Padding = UDim.new(0, 8); Layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
Layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() Scroll.CanvasSize = UDim2.new(0, 0, 0, Layout.AbsoluteContentSize.Y + 10) end)

local function CreateLabel(text)
    local lbl = Instance.new("TextLabel", Scroll)
    lbl.Size = UDim2.new(0.95, 0, 0, 25); lbl.BackgroundTransparency = 1
    lbl.Text = text; lbl.TextColor3 = Color3.fromRGB(150, 150, 150)
    lbl.Font = Enum.Font.GothamBold; lbl.TextSize = 12; lbl.TextXAlignment = Enum.TextXAlignment.Left
end

local function CreateToggle(text, configKey, callback)
    local frame = Instance.new("Frame", Scroll)
    frame.Size = UDim2.new(0.95, 0, 0, 40); frame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    
    local lbl = Instance.new("TextLabel", frame)
    lbl.Size = UDim2.new(0.7, 0, 1, 0); lbl.Position = UDim2.new(0, 10, 0, 0)
    lbl.BackgroundTransparency = 1; lbl.Text = text; lbl.TextColor3 = Color3.fromRGB(230, 230, 230)
    lbl.Font = Enum.Font.GothamMedium; lbl.TextSize = 14; lbl.TextXAlignment = Enum.TextXAlignment.Left
    
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 40, 0, 20); btn.Position = UDim2.new(1, -50, 0.5, -10)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 50); btn.Text = ""
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)
    
    local circle = Instance.new("Frame", btn)
    circle.Size = UDim2.new(0, 16, 0, 16); circle.Position = UDim2.new(0, 2, 0.5, -8)
    circle.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = _G.RyuConfig[configKey] or false
    btn.MouseButton1Click:Connect(function()
        isOn = not isOn
        _G.RyuConfig[configKey] = isOn
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = isOn and Color3.fromRGB(0, 150, 255) or Color3.fromRGB(40, 40, 50)}):Play()
        TweenService:Create(circle, TweenInfo.new(0.2), {Position = isOn and UDim2.new(1, -18, 0.5, -8) or UDim2.new(0, 2, 0.5, -8)}):Play()
        if callback then callback(isOn) end
    end)
end

CreateLabel("-- IMPEL DOWN ENGINE --")
CreateToggle("Enable Auto Impel Down", "AutoImpelDown")
CreateLabel("-- UTILITY --")
CreateToggle("Anti-AFK Protection", "AntiAFK", function(state)
    if state then _G.AntiAfkConnection = LocalPlayer.Idled:Connect(function() game:GetService("VirtualUser"):CaptureController(); game:GetService("VirtualUser"):ClickButton2(Vector2.new()) end)
    else if _G.AntiAfkConnection then _G.AntiAfkConnection:Disconnect() end end
end)


--// ============================================================================
--// IMPEL DOWN AUTO FARM ENGINE LOGIC (PERFECTED)
--// ============================================================================

-- ANTI-STUN SYSTEM
local function AntiStunSetup(char)
    char.DescendantAdded:Connect(function(desc)
        if not _G.RyuConfig.AutoImpelDown then return end
        task.wait()
        if desc.Name == "Stun" or desc.Name == "StunFolder" then
            desc:Destroy()
        end
        if (desc:IsA("BodyPosition") or desc:IsA("BodyVelocity")) and desc.Name ~= "geppo" and desc.Name ~= "rolling" and desc.Name ~= "RyuHover" then
            desc:Destroy()
        end
    end)
end

if LocalPlayer.Character then AntiStunSetup(LocalPlayer.Character) end
LocalPlayer.CharacterAdded:Connect(function(char) AntiStunSetup(char) end)

-- PASSIVE STATS ALLOCATOR
task.spawn(function()
    while true do
        task.wait()
        if _G.RyuConfig.AutoImpelDown then
            pcall(function()
                local argsStr = {"Strength", nil, 1}
                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("stats"):FireServer(unpack(argsStr, 1, 3))
                
                local argsDef = {"Defense", nil, 1}
                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("stats"):FireServer(unpack(argsDef, 1, 3))
            end)
        end
    end
end)

-- PASSIVE SPIRIT ESSENCE HANDLER (Strict Green/Accept Match)
_G.SpiritEssenceUsed = false
task.spawn(function()
    local GuiService = game:GetService("GuiService")
    while true do
        task.wait(1)
        if not _G.RyuConfig.AutoImpelDown then continue end
        if _G.SpiritEssenceUsed then continue end
        if _G.ImpelState == "Init" or _G.ImpelState == "WaitForCutscene" then continue end 
        
        local char = LocalPlayer.Character
        if not char then continue end
        local hum = char:FindFirstChildOfClass("Humanoid")
        
        local essence = LocalPlayer.Backpack:FindFirstChild("Spirit Essence") or char:FindFirstChild("Spirit Essence")
        if essence and hum then
            char.PrimaryPart.Velocity = Vector3.new(0,0,0)
            
            pcall(function() ReplicatedStorage.Events.Tools:InvokeServer("equip", "Spirit Essence") end)
            task.wait(0.5)
            pcall(function() hum:EquipTool(essence) end)
            task.wait(0.5)
            
            pcall(function()
                local center = camera.ViewportSize / 2
                VirtualInputManager:SendMouseButtonEvent(center.X, center.Y, 0, true, game, 1)
                task.wait(0.1)
                VirtualInputManager:SendMouseButtonEvent(center.X, center.Y, 0, false, game, 1)
            end)
            task.wait(1.5)
            
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                local guiInset = GuiService:GetGuiInset()
                if pg then
                    local clicked = false
                    for _, v in pairs(pg:GetDescendants()) do
                        if clicked then break end
                        if v:IsA("TextButton") or v:IsA("ImageButton") then
                            local txt = ""
                            if v:IsA("TextLabel") or v:IsA("TextButton") then txt = string.lower(v.Text or "") end
                            local name = string.lower(v.Name)
                            
                            local isGreen = false
                            local isRed = false
                            if v.BackgroundColor3 then
                                isGreen = (v.BackgroundColor3.G > v.BackgroundColor3.R + 0.1 and v.BackgroundColor3.G > v.BackgroundColor3.B + 0.1)
                                isRed = (v.BackgroundColor3.R > v.BackgroundColor3.G + 0.1 and v.BackgroundColor3.R > v.BackgroundColor3.B + 0.1)
                            end
                            
                            if isRed or string.find(txt, "no") or string.find(txt, "decline") or string.find(txt, "ablehnen") or string.find(name, "no") or string.find(name, "decline") then
                                continue
                            end
                            
                            if (string.find(txt, "accept") or string.find(name, "accept") or string.find(txt, "yes") or string.find(name, "yes") or string.find(txt, "akzeptieren") or isGreen) then
                                local absPos = v.AbsolutePosition
                                local absSize = v.AbsoluteSize
                                local centerX = absPos.X + (absSize.X / 2)
                                local centerY = absPos.Y + (absSize.Y / 2) + guiInset.Y
                                
                                VirtualInputManager:SendMouseButtonEvent(centerX, centerY, 0, true, game, 1)
                                task.wait(0.05)
                                VirtualInputManager:SendMouseButtonEvent(centerX, centerY, 0, false, game, 1)
                                
                                pcall(function() v.Activated:Fire() end)
                                pcall(function() getsenv(v).Click() end)
                                pcall(function() v.MouseButton1Click:Fire() end)
                                
                                clicked = true
                                break
                            end
                        end
                    end
                end
            end)
            
            task.wait(1)
            pcall(function()
                local args = {"Buso"}
                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Haki"):FireServer(unpack(args, 1, 1))
            end)
            
            _G.SpiritEssenceUsed = true
        end
    end
end)

_G.ImpelState = "Init"
_G.VeraSeen = false

-- PASSIVE CAMERA TRACKING
task.spawn(function()
    while true do
        task.wait()
        if _G.RyuConfig.AutoImpelDown then
            pcall(function()
                local char = LocalPlayer.Character
                if char then
                    local hum = char:FindFirstChildOfClass("Humanoid")
                    if hum then
                        camera.CameraType = Enum.CameraType.Custom
                        camera.CameraSubject = hum
                    end
                end
            end)
        end
    end
end)

local currentComboIndex = 1
local lastSwing = 0

local function EquipTargetWeapon()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not hum then return false end
    
    local targetWep = nil
    for _, item in pairs(LocalPlayer.Backpack:GetChildren()) do
        if item:IsA("Tool") and (item:GetAttribute("MeleeTool") or item.Name:lower():find("combat") or item.Name:lower():find("sword")) then
            targetWep = item; break
        end
    end
    if not targetWep then
        for _, item in pairs(LocalPlayer.Backpack:GetChildren()) do
            if item:IsA("Tool") then targetWep = item break end
        end
    end
    
    if targetWep then
        pcall(function() ReplicatedStorage.Events.Tools:InvokeServer("equip", targetWep.Name) end)
        task.wait(0.1)
        hum:EquipTool(targetWep)
        return true
    end
    return false
end

local function PerformMeleeAttack(targets)
    pcall(function()
        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if not root then return end
        
        local now = tick()
        if now - lastSwing >= 0.5 then
            lastSwing = now
            task.spawn(function()
                local hitParts = {}
                if type(targets) == "table" then
                    for _, npc in ipairs(targets) do
                        local mRoot = npc:FindFirstChild("HumanoidRootPart")
                        local mHum = npc:FindFirstChildOfClass("Humanoid")
                        if mRoot and mHum and mHum.Health > 0 then table.insert(hitParts, mRoot) end
                    end
                end
                
                local animName = "Punch" .. currentComboIndex
                if currentComboIndex == 1 then animName = "Dash" end
                if currentComboIndex == 4 then animName = "GroundPunch4" end
                
                local animObj = ReplicatedStorage:FindFirstChild("CombatAnimations") and ReplicatedStorage.CombatAnimations:FindFirstChild("Melee") and ReplicatedStorage.CombatAnimations.Melee:FindFirstChild(animName)
                
                if animObj then
                    local argsAnim = {"swingsfx", "Melee", currentComboIndex, "Ground", currentComboIndex == 1, animObj, 2, 1.5}
                    if ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("CombatRegister") then
                        pcall(function() ReplicatedStorage.Events.CombatRegister:InvokeServer(argsAnim) end)
                    end
                end
                
                if #hitParts > 0 then
                    local argsDamage = {"damage", hitParts, "Melee", {currentComboIndex, "Ground", "Melee"}, true, root.CFrame, ["aircombo"] = "Ground"}
                    if ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("CombatRegister") then
                        pcall(function() ReplicatedStorage.Events.CombatRegister:InvokeServer(argsDamage) end)
                    end
                end
                
                currentComboIndex = currentComboIndex + 1
                if currentComboIndex > 4 then currentComboIndex = 1 end
            end)
        end
    end)
end

local function ToggleHover(state)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    if state then
        local bp = root:FindFirstChild("RyuHover")
        if not bp then
            bp = Instance.new("BodyPosition")
            bp.Name = "RyuHover"
            bp.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            bp.D = 500
            bp.P = 50000
            bp.Parent = root
        end
        bp.Position = root.Position
    else
        local bp = root:FindFirstChild("RyuHover")
        if bp then bp:Destroy() end
    end
end

local function PathTransport(targetPos, speed, timeout)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return false end

    ToggleHover(true)
    local startTime = tick()
    local timeoutLimit = timeout or 99999
    local tickCounter = 0
    local lastPos = root.Position
    local stuckTimer = 0

    while _G.RyuConfig.AutoImpelDown do
        if tick() - startTime > timeoutLimit then return false end

        local dist = (root.Position - targetPos).Magnitude
        if dist < 4 then break end
        
        tickCounter = tickCounter + 1
        if tickCounter % 15 == 0 then
            local tpCheck = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "tp check") or string.find(txt, "teleport check") or string.find(txt, "noclip") then
                                tpCheck = true; break
                            end
                        end
                    end
                end
            end)
            if tpCheck then
                root.Velocity = Vector3.new(0,0,0)
                task.wait(1.5)
                return false 
            end
        end

        local dt = RunService.Heartbeat:Wait()
        
        if (root.Position - lastPos).Magnitude < (speed * dt * 0.2) then
            stuckTimer = stuckTimer + dt
            if stuckTimer > 0.5 then return false end 
        else
            stuckTimer = 0
        end
        lastPos = root.Position

        local dir = (targetPos - root.Position).Unit
        local flatDir = Vector3.new(dir.X, 0, dir.Z).Unit
        if flatDir.Magnitude ~= flatDir.Magnitude then flatDir = Vector3.new(1,0,0) end

        local nextPos = root.Position + (dir * speed * dt)
        
        root.CFrame = CFrame.lookAt(nextPos, nextPos + flatDir)
        local bp = root:FindFirstChild("RyuHover")
        if bp then bp.Position = nextPos end
        root.Velocity = Vector3.new(0,0,0)
        root.RotVelocity = Vector3.new(0,0,0)
        
        pcall(function()
            local camPos = root.Position - (flatDir * 15) + Vector3.new(0, 7, 0)
            camera.CFrame = CFrame.lookAt(camPos, root.Position)
        end)
    end
    return true
end

local function HoldInteract(duration)
    local t = tick()
    while tick() - t < duration do
        if not _G.RyuConfig.AutoImpelDown then break end
        
        pcall(function() VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game) end)
        
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("ProximityPrompt") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                if (LocalPlayer.Character.HumanoidRootPart.Position - v.Parent.Position).Magnitude <= v.MaxActivationDistance + 5 then
                    if fireproximityprompt then fireproximityprompt(v, 1) else v:InputHoldBegin() end
                end
            end
        end
        
        task.wait(0.5)
        
        pcall(function() VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game) end)
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("ProximityPrompt") then pcall(function() v:InputHoldEnd() end) end
        end
        
        task.wait(0.1)
    end
end

local function CheckHPAndFailsafe(root, hum, safePos)
    if hum.Health / hum.MaxHealth < 0.3 then
        ToggleHover(true)
        pcall(function() ReplicatedStorage.Events.climb:InvokeServer(true) end)
        
        local bp = root:FindFirstChild("RyuHover")
        if bp then bp.Position = root.Position + Vector3.new(0, 20, 0) end
        
        while hum.Health / hum.MaxHealth < 0.8 do
            if not _G.RyuConfig.AutoImpelDown then break end
            pcall(function()
                if ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Block") then
                    ReplicatedStorage.Events.Block:InvokeServer(true, "Melee", true)
                end
            end)
            root.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
            task.wait(0.2)
        end
        
        pcall(function() ReplicatedStorage.Events.climb:InvokeServer(false) end)
        pcall(function()
            if ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Block") then
                ReplicatedStorage.Events.Block:InvokeServer(false, "Melee", true)
            end
        end)
        return true
    end
    return false
end

-- IMPEL DOWN MASTER LOOP
task.spawn(function()
    while true do
        task.wait(0.05)
        if not _G.RyuConfig.AutoImpelDown then continue end

        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if not root or not hum or hum.Health <= 0 then continue end

        -- 1. DIFF CHOOSER
        local diffChooser = LocalPlayer:FindFirstChild("PlayerGui") and LocalPlayer.PlayerGui:FindFirstChild("DiffChooser")
        if diffChooser and diffChooser.Enabled then
            pcall(function() diffChooser.Replication.RemoteEvent:FireServer("Nightmare", "check!") end)
            task.wait(0.5)
            continue 
        end

        -- 2. VERA COMBAT
        if _G.ImpelState == "Init" then
            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local vera = npcsFolder and npcsFolder:FindFirstChild("Vera")
            
            if vera then
                local vHum = vera:FindFirstChildOfClass("Humanoid")
                local vRoot = vera:FindFirstChild("HumanoidRootPart") or vera.PrimaryPart
                
                if not vHum or not vRoot then task.wait(0.1) continue end
                
                if vHum.Health > 0 then
                    local distToVera = (root.Position - vRoot.Position).Magnitude
                    if _G.VeraSeen and distToVera > 150 then ToggleHover(false) _G.ImpelState = "WaitForCutscene" continue end
                    if not _G.VeraSeen and distToVera > 50 then continue end

                    _G.VeraSeen = true
                    
                    if vRoot.Size.X < 15 then vRoot.Size = Vector3.new(15, 15, 15) vRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, vRoot.Position) then continue end
                    
                    if not _G.SmartHoverHeight then _G.SmartHoverHeight = 6.5 end
                    if not _G.PlayerLastHpVera then _G.PlayerLastHpVera = hum.Health end
                    if hum.Health < _G.PlayerLastHpVera then _G.DodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpVera = hum.Health
                    
                    if not _G.VeraLastHp then _G.VeraLastHp = vHum.Health end
                    if vHum.Health < _G.VeraLastHp then
                        _G.VeraLastHp = vHum.Health
                        _G.VeraLastHitTime = tick()
                    end
                    if tick() - (_G.VeraLastHitTime or tick()) > 1.5 then
                        _G.SmartHoverHeight = math.max(4.0, _G.SmartHoverHeight - 0.1)
                        _G.VeraLastHitTime = tick() 
                    end

                    local currentDodgeOffset = (tick() < (_G.DodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.SmartHoverHeight + currentDodgeOffset

                    local lookDir = vRoot.CFrame.LookVector
                    local attackPos = vRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local flatTarget = Vector3.new(vRoot.Position.X, attackPos.Y, vRoot.Position.Z)
                    local targetRot = CFrame.lookAt(attackPos, flatTarget) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = attackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack({vera})
                    continue 
                else
                    ToggleHover(false)
                    _G.ImpelState = "WaitForCutscene"
                end
            else
                if _G.VeraSeen then ToggleHover(false) _G.ImpelState = "WaitForCutscene" end
            end
            continue
        end

        -- 2.5 CUTSCENE / MESSAGE WAIT
        if _G.ImpelState == "WaitForCutscene" then
            if not _G.VeraDeadTime then _G.VeraDeadTime = tick() end
            
            local messageFound = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "floor 1") or string.find(txt, "stage 1") or string.find(txt, "your skill points have been reset") then
                                messageFound = true
                                break
                            end
                        end
                    end
                end
            end)

            if messageFound then
                ToggleHover(false)
                if hum then hum:Move(Vector3.new(0,0,0), false) end
                root.Velocity = Vector3.new(0, 0, 0)
                _G.ImpelState = "Key"
            else
                if tick() - _G.VeraDeadTime > 15 then _G.ImpelState = "Key" else task.wait(0.2) end
            end
            continue
        end

        -- 3. KEY PHASE
        if _G.ImpelState == "Key" then
            local keyPart = nil
            pcall(function()
                local effects = Workspace:FindFirstChild("Effects")
                if effects then
                    local kModel = effects:FindFirstChild("Key")
                    if kModel then
                        if kModel:IsA("BasePart") then keyPart = kModel elseif kModel:FindFirstChild("Key") then keyPart = kModel.Key end
                    end
                end
                if not keyPart then
                    local islands = Workspace:FindFirstChild("Islands")
                    if islands then
                        for _, isl in pairs(islands:GetChildren()) do
                            if string.find(string.lower(isl.Name), "impel base") then
                                local spawns = isl:FindFirstChild("KeySpawns")
                                if spawns then
                                    for _, k in pairs(spawns:GetChildren()) do
                                        if k.Name == "Key" and k:IsA("BasePart") and k.Transparency < 1 then 
                                            keyPart = k; break 
                                        end
                                    end
                                end
                            end
                        end
                    end
                end
            end)

            if keyPart then
                local reached = PathTransport(keyPart.Position, 43, 20)
                if reached then HoldInteract(2) end
                _G.ImpelState = "ChestRoute"
            else
                task.wait(0.5)
            end
            continue
        end

        -- 4. CHEST ROUTE
        if _G.ImpelState == "ChestRoute" then
            task.wait(2)
            local points = {
                {pos = Vector3.new(2952.66, 2075.45, -13461.08), action = "wait", time = 1},
                {pos = Vector3.new(3010.94, 2076.70, -13535.94), action = "chest", time = 5},
                {pos = Vector3.new(2991.08, 2076.70, -13583.22), action = "chest", time = 5},
                {pos = Vector3.new(2886.56, 2077.70, -13581.59), action = "chest", time = 5},
                {pos = Vector3.new(2860.68, 2084.70, -13604.73), action = "chest", time = 5},
                {pos = Vector3.new(3036.38, 2082.95, -13540.35), action = "chest", time = 5},
                {pos = Vector3.new(3090.27, 2080.05, -13512.88), action = "chest", time = 5},
                {pos = Vector3.new(3079.41, 2080.45, -13473.72), action = "chest", time = 5}
            }

            for _, pt in ipairs(points) do
                if not _G.RyuConfig.AutoImpelDown then break end
                local reached = PathTransport(pt.pos, 43, 20)
                if reached then
                    if pt.action == "wait" then task.wait(pt.time) elseif pt.action == "chest" then HoldInteract(pt.time) end
                end
            end
            _G.ImpelState = "Waypoints"
            continue
        end

        -- 5. WAYPOINTS TO GUARDS
        if _G.ImpelState == "Waypoints" then
            PathTransport(Vector3.new(2945.63, 2075.55, -13578.02), 30, 20)
            PathTransport(Vector3.new(2946.49, 2075.45, -13908.61), 30, 20)
            _G.ImpelState = "Guards"
            continue
        end

        -- 6. IMPEL GUARDS
        if _G.ImpelState == "Guards" then
            local npcsFolder = Workspace:FindFirstChild("NPCs")
            if not npcsFolder then continue end
            
            local guards = {}
            for _, v in pairs(npcsFolder:GetChildren()) do
                if string.find(string.lower(v.Name), "guard") then
                    local gHum = v:FindFirstChildOfClass("Humanoid")
                    local gRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                    if gHum and gHum.Health > 0 and gRoot then
                        if (root.Position - gRoot.Position).Magnitude <= 100 then
                            table.insert(guards, v)
                        end
                    end
                end
            end
            
            if #guards > 0 then
                _G.LastGuardSeenTime = tick()
                local target = guards[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 30, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.GuardHoverHeight then _G.GuardHoverHeight = 6.5 end
                    if not _G.PlayerLastHpGuard then _G.PlayerLastHpGuard = hum.Health end
                    if hum.Health < _G.PlayerLastHpGuard then _G.GuardDodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpGuard = hum.Health
                    
                    if not _G.GuardLastHp then _G.GuardLastHp = tHum.Health end
                    if tHum.Health < _G.GuardLastHp then
                        _G.GuardLastHp = tHum.Health
                        _G.GuardLastHitTime = tick()
                    end
                    if tick() - (_G.GuardLastHitTime or tick()) > 1.5 then
                        _G.GuardHoverHeight = math.max(4.0, _G.GuardHoverHeight - 0.1)
                        _G.GuardLastHitTime = tick()
                    end

                    local currentDodgeOffset = (tick() < (_G.GuardDodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.GuardHoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local flatTarget = Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)
                    local targetRot = CFrame.lookAt(finalAttackPos, flatTarget) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(guards) 
                end
                continue
            else
                if not _G.LastGuardSeenTime then _G.LastGuardSeenTime = tick() end
                if tick() - _G.LastGuardSeenTime > 5 then
                    ToggleHover(false)
                    _G.ImpelState = "LabyrinthStart"
                end
                continue
            end
        end

        -- 7. LABYRINTH BYPASS
        if _G.ImpelState == "LabyrinthStart" then
            local pos1 = Vector3.new(2951.33, 2075.45, -14048.78)
            PathTransport(pos1, 43, 20)
            
            local labyrinthTarget = Vector3.new(2660.54, 2075.45, -15403.33)
            while _G.RyuConfig.AutoImpelDown and (root.Position - labyrinthTarget).Magnitude > 15 do
                local path = PathfindingService:CreatePath({ AgentRadius = 4.5, AgentHeight = 6, AgentCanJump = true, WaypointSpacing = 4 })
                local success, err = pcall(function() path:ComputeAsync(root.Position, labyrinthTarget) end)
                
                if success and path.Status == Enum.PathStatus.Success then
                    local waypoints = path:GetWaypoints()
                    for i, waypoint in ipairs(waypoints) do
                        if not _G.RyuConfig.AutoImpelDown then break end
                        if waypoint.Action == Enum.PathWaypointAction.Jump then hum.Jump = true end
                        local reached = PathTransport(waypoint.Position, 43, 3) 
                        if not reached then break end
                    end
                else
                    PathTransport(labyrinthTarget, 43, 2)
                end
            end
            
            local pos3 = Vector3.new(2663.73, 2075.45, -15501.86)
            PathTransport(pos3, 43, 20)
            
            _G.LabGuardLastCombat = tick()
            _G.ImpelState = "LabyrinthGuards"
            continue
        end

        -- 8. LABYRINTH GUARDS
        if _G.ImpelState == "LabyrinthGuards" then
            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local guards = {}
            if npcsFolder then
                for _, v in pairs(npcsFolder:GetChildren()) do
                    if string.find(string.lower(v.Name), "guard") then
                        local gHum = v:FindFirstChildOfClass("Humanoid")
                        local gRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                        if gHum and gHum.Health > 0 and gRoot then
                            if (root.Position - gRoot.Position).Magnitude <= 100 then
                                table.insert(guards, v)
                            end
                        end
                    end
                end
            end
            
            if #guards > 0 then
                _G.LabGuardLastCombat = tick()
                local target = guards[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.GuardHoverHeight then _G.GuardHoverHeight = 6.5 end
                    if not _G.PlayerLastHpGuard then _G.PlayerLastHpGuard = hum.Health end
                    if hum.Health < _G.PlayerLastHpGuard then _G.GuardDodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpGuard = hum.Health
                    
                    if not _G.GuardLastHp then _G.GuardLastHp = tHum.Health end
                    if tHum.Health < _G.GuardLastHp then
                        _G.GuardLastHp = tHum.Health
                        _G.GuardLastHitTime = tick()
                    end
                    if tick() - (_G.GuardLastHitTime or tick()) > 1.5 then
                        _G.GuardHoverHeight = math.max(4.0, _G.GuardHoverHeight - 0.1)
                        _G.GuardLastHitTime = tick()
                    end

                    local currentDodgeOffset = (tick() < (_G.GuardDodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.GuardHoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local flatTarget = Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)
                    local targetRot = CFrame.lookAt(finalAttackPos, flatTarget) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(guards) 
                end
            else
                if not _G.LabGuardLastCombat then _G.LabGuardLastCombat = tick() end
                if tick() - _G.LabGuardLastCombat > 3 then
                    ToggleHover(false)
                    _G.ImpelState = "WaitingForNext"
                end
            end
            continue
        end

        -- 9. WAITING FOR NEXT ROOM (Floor 2 Detection)
        if _G.ImpelState == "WaitingForNext" then
            local floor2Found = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            if string.find(string.lower(v.Text), "floor 2") then
                                floor2Found = true
                                break
                            end
                        end
                    end
                end
            end)

            if floor2Found then _G.ImpelState = "Floor2Waypoints" end
            task.wait(0.5)
            continue
        end

        -- 10. FLOOR 2 WAYPOINTS (Room 1)
        if _G.ImpelState == "Floor2Waypoints" then
            local pts = {
                Vector3.new(3200.23, 2405.38, -20190.65),
                Vector3.new(3265.69, 2405.38, -20199.22),
                Vector3.new(3261.70, 2405.38, -20193.35),
                Vector3.new(3197.87, 2380.43, -20281.73)
            }
            for _, pt in ipairs(pts) do
                if not _G.RyuConfig.AutoImpelDown then break end
                PathTransport(pt, 43, 15)
                task.wait(0.1)
            end
            _G.ImpelState = "Floor2Combat"
            continue
        end

        -- 11. FLOOR 2 COMBAT (Room 1)
        if _G.ImpelState == "Floor2Combat" then
            local roomCleared = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "room cleared") or string.find(txt, "floor cleared") then
                                roomCleared = true
                                break
                            end
                        end
                    end
                end
            end)

            if roomCleared then
                ToggleHover(false)
                _G.ImpelState = "Floor2Waypoints2"
                task.wait(2)
                continue
            end

            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local enemies = {}
            if npcsFolder then
                for _, v in pairs(npcsFolder:GetChildren()) do
                    if v:FindFirstChildOfClass("Humanoid") and v.Name ~= LocalPlayer.Name and v.Name ~= "Vera" then
                        local eHum = v:FindFirstChildOfClass("Humanoid")
                        local eRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                        if eHum and eHum.Health > 0 and eRoot then
                            if (root.Position - eRoot.Position).Magnitude <= 150 then
                                table.insert(enemies, v)
                            end
                        end
                    end
                end
            end

            if #enemies > 0 then
                local target = enemies[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.F2HoverHeight then _G.F2HoverHeight = 6.5 end
                    if not _G.PlayerLastHpF2 then _G.PlayerLastHpF2 = hum.Health end
                    if hum.Health < _G.PlayerLastHpF2 then _G.F2DodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpF2 = hum.Health
                    
                    if not _G.F2LastHp then _G.F2LastHp = tHum.Health end
                    if tHum.Health < _G.F2LastHp then
                        _G.F2LastHp = tHum.Health
                        _G.F2LastHitTime = tick()
                    end
                    if tick() - (_G.F2LastHitTime or tick()) > 1.5 then
                        _G.F2HoverHeight = math.max(4.0, _G.F2HoverHeight - 0.1)
                        _G.F2LastHitTime = tick()
                    end

                    local currentDodgeOffset = (tick() < (_G.F2DodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.F2HoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local flatTarget = Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)
                    local targetRot = CFrame.lookAt(finalAttackPos, flatTarget) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(enemies) 
                end
            else
                task.wait(0.1)
            end
            continue
        end
        
        -- 12. FLOOR 2 WAYPOINTS (Room 2)
        if _G.ImpelState == "Floor2Waypoints2" then
            local pt = Vector3.new(3201.04, 2378.43, -20382.98)
            PathTransport(pt, 43, 20)
            _G.ImpelState = "Floor2Combat2"
            continue
        end

        -- 13. FLOOR 2 COMBAT (Room 2)
        if _G.ImpelState == "Floor2Combat2" then
            local roomCleared = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "room cleared") or string.find(txt, "floor cleared") then
                                roomCleared = true
                                break
                            end
                        end
                    end
                end
            end)

            if roomCleared then
                ToggleHover(false)
                _G.ImpelState = "Floor2Done"
                continue
            end

            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local enemies = {}
            if npcsFolder then
                for _, v in pairs(npcsFolder:GetChildren()) do
                    if v:FindFirstChildOfClass("Humanoid") and v.Name ~= LocalPlayer.Name and v.Name ~= "Vera" then
                        local eHum = v:FindFirstChildOfClass("Humanoid")
                        local eRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                        if eHum and eHum.Health > 0 and eRoot then
                            if (root.Position - eRoot.Position).Magnitude <= 150 then
                                table.insert(enemies, v)
                            end
                        end
                    end
                end
            end

            if #enemies > 0 then
                local target = enemies[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.F2H2HoverHeight then _G.F2H2HoverHeight = 6.5 end
                    if not _G.PlayerLastHpF2H2 then _G.PlayerLastHpF2H2 = hum.Health end
                    if hum.Health < _G.PlayerLastHpF2H2 then _G.F2H2DodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpF2H2 = hum.Health
                    
                    if not _G.F2H2LastHp then _G.F2H2LastHp = tHum.Health end
                    if tHum.Health < _G.F2H2LastHp then
                        _G.F2H2LastHp = tHum.Health
                        _G.F2H2LastHitTime = tick()
                    end
                    if tick() - (_G.F2H2LastHitTime or tick()) > 1.5 then
                        _G.F2H2HoverHeight = math.max(4.0, _G.F2H2HoverHeight - 0.1)
                        _G.F2H2LastHitTime = tick()
                    end

                    local currentDodgeOffset = (tick() < (_G.F2H2DodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.F2H2HoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local flatTarget = Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)
                    local targetRot = CFrame.lookAt(finalAttackPos, flatTarget) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(enemies) 
                end
            else
                task.wait(0.1)
            end
            continue
        end

        if _G.ImpelState == "Floor2Done" then
            task.wait(1)
            continue
        end

    end
end)

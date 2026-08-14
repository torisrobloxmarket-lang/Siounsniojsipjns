-- ryu hub - jujutsu shenanigans (tjs) v1
-- fully customized for tjs knit architecture

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local VirtualInputManager = game:GetService("VirtualInputManager")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

-- gui cleanup
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() 
    if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end 
end)

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "RyuHubTJS" then v:Destroy() end 
end

-- tjs knit remotes cache
local knitServices = ReplicatedStorage:WaitForChild("Knit", 5) and ReplicatedStorage.Knit:WaitForChild("Services")
local remotes = {
    BlockOn = knitServices and knitServices:FindFirstChild("BlockService") and knitServices.BlockService.RE:FindFirstChild("Activated"),
    BlockOff = knitServices and knitServices:FindFirstChild("BlockService") and knitServices.BlockService.RE:FindFirstChild("Deactivated"),
    Chase = knitServices and knitServices:FindFirstChild("ItadoriService") and knitServices.ItadoriService.RE:FindFirstChild("Chase"),
    Divergent = knitServices and knitServices:FindFirstChild("DivergentFistService") and knitServices.DivergentFistService.RE:FindFirstChild("Activated")
}

-- global config
local CONFIG_FILE = "RyuHub_TJS_Settings.json"
local RyuConfig = {
    -- player
    Speed = false, SpeedKey = Enum.KeyCode.Z, SpeedVal = 50,
    Fly = false, FlyKey = Enum.KeyCode.X,
    JumpHigh = false, JumpSpam = false,
    Noclip = false, Invisible = false,
    
    -- auto
    AutoBlock = false, BlockRange = 15,
    AutoBlackFlash = false,
    AutoDodge = false, DodgeRange = 20,
    
    -- ability
    LockOn = false, LockOnKey = Enum.KeyCode.C,
    Knockback = false, KnockbackVal = 50,
    DomainBypass = false, DashSpam = false,
    
    -- ai farm
    AIFarm = false, AIRange = 50, AutoUlt = false, HumanMode = false,
    S1 = false, S1R = 10, S2 = false, S2R = 10, S3 = false, S3R = 10, S4 = false, S4R = 10,
    
    -- target farm
    TargetPlayer = "", StayBehind = false, BehindDist = 3,
    
    -- money farm
    MoneyFarm = false, MFRole = "Farmer", MFVictim = "",
    
    -- config
    AutoRejoin = false, MinPlayers = 3, HighPop = true,
    TargetJoinUser = ""
}

-- save/load engine
local function SaveConfig()
    pcall(function()
        if writefile then writefile(CONFIG_FILE, HttpService:JSONEncode(RyuConfig)) end
    end)
end

pcall(function()
    if readfile and isfile and isfile(CONFIG_FILE) then
        local data = HttpService:JSONDecode(readfile(CONFIG_FILE))
        for k,v in pairs(data) do RyuConfig[k] = v end
    end
end)

-- theme & setup
local Theme = {
    Background = Color3.fromRGB(12, 12, 14), Sidebar = Color3.fromRGB(18, 18, 20),
    SectionBG = Color3.fromRGB(24, 24, 26), Text = Color3.fromRGB(250, 250, 250),
    SubText = Color3.fromRGB(130, 130, 135), Accent = Color3.fromRGB(255, 65, 65),
    ToggleOff = Color3.fromRGB(35, 35, 38), ToggleOn = Color3.fromRGB(255, 65, 65),
    Stroke = Color3.fromRGB(45, 45, 50), Warning = Color3.fromRGB(255, 180, 50)
}

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubTJS"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 650, 0, 420)
MainFrame.Position = UDim2.new(0.5, -325, 0.5, -210)
MainFrame.BackgroundColor3 = Theme.Background
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = RyuHub
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", MainFrame).Color = Theme.Stroke

local Topbar = Instance.new("Frame", MainFrame)
Topbar.Size = UDim2.new(1, 0, 0, 40)
Topbar.BackgroundTransparency = 1
local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 200, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "RYU HUB | TJS"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 18
Title.TextColor3 = Theme.Text
Title.TextXAlignment = Enum.TextXAlignment.Left

local Line = Instance.new("Frame", MainFrame)
Line.Size = UDim2.new(1, 0, 0, 1)
Line.Position = UDim2.new(0, 0, 0, 40)
Line.BackgroundColor3 = Theme.Stroke
Line.BorderSizePixel = 0

local Sidebar = Instance.new("ScrollingFrame", MainFrame)
Sidebar.Size = UDim2.new(0, 140, 1, -41)
Sidebar.Position = UDim2.new(0, 0, 0, 41)
Sidebar.BackgroundTransparency = 1
Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar)
SideLayout.SortOrder = Enum.SortOrder.LayoutOrder

local ContentContainer = Instance.new("Frame", MainFrame)
ContentContainer.Size = UDim2.new(1, -141, 1, -41)
ContentContainer.Position = UDim2.new(0, 141, 0, 41)
ContentContainer.BackgroundTransparency = 1

local Tabs = {}
local function CreateMainTab(name)
    local btn = Instance.new("TextButton", Sidebar)
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Theme.Sidebar
    btn.Text = "  " .. name
    btn.TextColor3 = Theme.SubText
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    btn.TextXAlignment = Enum.TextXAlignment.Left
    
    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = 2
    page.Visible = false
    local pl = Instance.new("UIListLayout", page)
    pl.Padding = UDim.new(0, 8)
    pl.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    btn.MouseButton1Click:Connect(function()
        for _, t in pairs(Tabs) do
            t.Page.Visible = false
            t.Btn.TextColor3 = Theme.SubText
            t.Btn.BackgroundColor3 = Theme.Sidebar
        end
        page.Visible = true
        btn.TextColor3 = Theme.Text
        btn.BackgroundColor3 = Theme.SectionBG
    end)
    
    table.insert(Tabs, {Btn = btn, Page = page, Layout = pl})
    return page
end

local function CreateSection(page, titleText)
    local sec = Instance.new("Frame", page)
    sec.Size = UDim2.new(0.96, 0, 0, 30)
    sec.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", sec).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", sec).Color = Theme.Stroke
    
    local lbl = Instance.new("TextLabel", sec)
    lbl.Size = UDim2.new(1, -20, 0, 25)
    lbl.Position = UDim2.new(0, 10, 0, 5)
    lbl.BackgroundTransparency = 1
    lbl.Text = titleText
    lbl.TextColor3 = Theme.Accent
    lbl.Font = Enum.Font.GothamBold
    lbl.TextSize = 14
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    
    local sl = Instance.new("UIListLayout", sec)
    sl.Padding = UDim.new(0, 5)
    sl.HorizontalAlignment = Enum.HorizontalAlignment.Center
    sl.SortOrder = Enum.SortOrder.LayoutOrder
    
    local pad = Instance.new("UIPadding", sec)
    pad.PaddingTop = UDim.new(0, 35)
    pad.PaddingBottom = UDim.new(0, 10)
    
    sl:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        sec.Size = UDim2.new(0.96, 0, 0, sl.AbsoluteContentSize.Y + 45)
        page.CanvasSize = UDim2.new(0,0,0, page:FindFirstChildOfClass("UIListLayout").AbsoluteContentSize.Y + 20)
    end)
    return sec
end

local function CreateToggle(sec, txt, state, cb)
    local fr = Instance.new("Frame", sec)
    fr.Size = UDim2.new(0.94, 0, 0, 25)
    fr.BackgroundTransparency = 1
    local lbl = Instance.new("TextLabel", fr)
    lbl.Size = UDim2.new(0.8, 0, 1, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = txt
    lbl.TextColor3 = Theme.Text
    lbl.Font = Enum.Font.GothamMedium
    lbl.TextSize = 12
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    
    local btn = Instance.new("TextButton", fr)
    btn.Size = UDim2.new(0, 30, 0, 16)
    btn.Position = UDim2.new(1, -30, 0.5, -8)
    btn.BackgroundColor3 = state and Theme.ToggleOn or Theme.ToggleOff
    btn.Text = ""
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1,0)
    
    btn.MouseButton1Click:Connect(function()
        state = not state
        btn.BackgroundColor3 = state and Theme.ToggleOn or Theme.ToggleOff
        if cb then pcall(function() cb(state) end) end
    end)
end

local function CreateSlider(sec, txt, min, max, def, cb)
    local fr = Instance.new("Frame", sec)
    fr.Size = UDim2.new(0.94, 0, 0, 35)
    fr.BackgroundTransparency = 1
    local lbl = Instance.new("TextLabel", fr)
    lbl.Size = UDim2.new(0.8, 0, 0, 15)
    lbl.BackgroundTransparency = 1
    lbl.Text = txt .. " : " .. tostring(def)
    lbl.TextColor3 = Theme.Text
    lbl.Font = Enum.Font.GothamMedium
    lbl.TextSize = 12
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    
    local bg = Instance.new("TextButton", fr)
    bg.Size = UDim2.new(1, 0, 0, 4)
    bg.Position = UDim2.new(0, 0, 1, -10)
    bg.BackgroundColor3 = Theme.ToggleOff
    bg.Text = ""
    Instance.new("UICorner", bg).CornerRadius = UDim.new(1,0)
    
    local fill = Instance.new("Frame", bg)
    fill.Size = UDim2.new(math.clamp((def-min)/(max-min),0,1), 0, 1, 0)
    fill.BackgroundColor3 = Theme.Accent
    Instance.new("UICorner", fill).CornerRadius = UDim.new(1,0)
    
    local drag = false
    bg.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then drag = true end end)
    UserInputService.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then drag = false end end)
    UserInputService.InputChanged:Connect(function(i)
        if drag and i.UserInputType == Enum.UserInputType.MouseMovement then
            local pct = math.clamp((i.Position.X - bg.AbsolutePosition.X) / bg.AbsoluteSize.X, 0, 1)
            fill.Size = UDim2.new(pct, 0, 1, 0)
            local val = math.floor(min + ((max - min) * pct))
            lbl.Text = txt .. " : " .. tostring(val)
            if cb then pcall(function() cb(val) end) end
        end
    end)
end

local function CreateButton(sec, txt, cb)
    local btn = Instance.new("TextButton", sec)
    btn.Size = UDim2.new(0.94, 0, 0, 25)
    btn.BackgroundColor3 = Theme.ToggleOff
    btn.Text = txt
    btn.TextColor3 = Theme.Text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,4)
    Instance.new("UIStroke", btn).Color = Theme.Stroke
    btn.MouseButton1Click:Connect(function() pcall(cb) end)
end

local function CreateLabel(sec, txt)
    local lbl = Instance.new("TextLabel", sec)
    lbl.Size = UDim2.new(0.94, 0, 0, 20)
    lbl.BackgroundTransparency = 1
    lbl.Text = txt
    lbl.TextColor3 = Theme.SubText
    lbl.Font = Enum.Font.Gotham
    lbl.TextSize = 11
    lbl.TextWrapped = true
    lbl.TextXAlignment = Enum.TextXAlignment.Left
end

local function CreateInput(sec, placeholder, cb)
    local box = Instance.new("TextBox", sec)
    box.Size = UDim2.new(0.94, 0, 0, 25)
    box.BackgroundColor3 = Theme.ToggleOff
    box.PlaceholderText = placeholder
    box.Text = ""
    box.TextColor3 = Theme.Text
    box.Font = Enum.Font.GothamMedium
    box.TextSize = 12
    Instance.new("UICorner", box).CornerRadius = UDim.new(0,4)
    box.FocusLost:Connect(function() if cb then pcall(function() cb(box.Text) end) end end)
end

-- ========================================================
-- TAB 1: COMBAT
-- ========================================================
local T_Combat = CreateMainTab("COMBAT")

-- PLAYER
local S_Player = CreateSection(T_Combat, "Player")
CreateToggle(S_Player, "Speed Hack", RyuConfig.Speed, function(v) RyuConfig.Speed = v end)
CreateSlider(S_Player, "Speed Value", 16, 200, RyuConfig.SpeedVal, function(v) RyuConfig.SpeedVal = v end)
CreateToggle(S_Player, "Fly", RyuConfig.Fly, function(v) RyuConfig.Fly = v end)
CreateToggle(S_Player, "Jump High", RyuConfig.JumpHigh, function(v) RyuConfig.JumpHigh = v end)
CreateToggle(S_Player, "Jump Spam (Inf Jump)", RyuConfig.JumpSpam, function(v) RyuConfig.JumpSpam = v end)
CreateToggle(S_Player, "Noclip", RyuConfig.Noclip, function(v) RyuConfig.Noclip = v end)
CreateToggle(S_Player, "Invisible (Local/Desync)", RyuConfig.Invisible, function(v) RyuConfig.Invisible = v end)
CreateButton(S_Player, "Dance (Visible to others)", function()
    pcall(function() game:GetService("Players"):Chat("/e dance") end)
end)

-- AUTO
local S_Auto = CreateSection(T_Combat, "Auto Features")
CreateToggle(S_Auto, "Auto Block", RyuConfig.AutoBlock, function(v) RyuConfig.AutoBlock = v end)
CreateSlider(S_Auto, "Block React Range", 5, 50, RyuConfig.BlockRange, function(v) RyuConfig.BlockRange = v end)
CreateToggle(S_Auto, "Auto Black Flash", RyuConfig.AutoBlackFlash, function(v) RyuConfig.AutoBlackFlash = v end)
CreateLabel(S_Auto, "Auto Combos: Join discord.gg/ryuhub and send me clips of combos.")
CreateToggle(S_Auto, "Auto Dodge (TP Back on DMG)", RyuConfig.AutoDodge, function(v) RyuConfig.AutoDodge = v end)
CreateSlider(S_Auto, "Dodge TP Distance", 5, 50, RyuConfig.DodgeRange, function(v) RyuConfig.DodgeRange = v end)

-- ABILITY
local S_Abil = CreateSection(T_Combat, "Abilities")
CreateToggle(S_Abil, "Lock On (Nearest)", RyuConfig.LockOn, function(v) RyuConfig.LockOn = v end)
CreateToggle(S_Abil, "Knockback M1s", RyuConfig.Knockback, function(v) RyuConfig.Knockback = v end)
CreateSlider(S_Abil, "Knockback Force", 10, 200, RyuConfig.KnockbackVal, function(v) RyuConfig.KnockbackVal = v end)
CreateButton(S_Abil, "TP All To Me (Visual/Bring)", function()
    pcall(function()
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.CFrame = hrp.CFrame * CFrame.new(0,0,-3)
            end
        end
    end)
end)
CreateToggle(S_Abil, "Domain Eraser (Bypass Domains)", RyuConfig.DomainBypass, function(v) RyuConfig.DomainBypass = v end)
CreateToggle(S_Abil, "No Cooldown Dash Spam", RyuConfig.DashSpam, function(v) RyuConfig.DashSpam = v end)
CreateLabel(S_Abil, "Suggest more in discord.gg/ryuhub")

-- ========================================================
-- TAB 2: FARM
-- ========================================================
local T_Farm = CreateMainTab("FARM")

-- AI FARM
local S_AIFarm = CreateSection(T_Farm, "AI Auto Farm")
CreateToggle(S_AIFarm, "Enable AI Farm", RyuConfig.AIFarm, function(v) RyuConfig.AIFarm = v end)
CreateSlider(S_AIFarm, "Chase Range", 10, 500, RyuConfig.AIRange, function(v) RyuConfig.AIRange = v end)
CreateToggle(S_AIFarm, "Auto Ultimate (G)", RyuConfig.AutoUlt, function(v) RyuConfig.AutoUlt = v end)
CreateToggle(S_AIFarm, "Human Mode (Strafe/Jump)", RyuConfig.HumanMode, function(v) RyuConfig.HumanMode = v end)
CreateToggle(S_AIFarm, "Use Skill 1", RyuConfig.S1, function(v) RyuConfig.S1 = v end)
CreateSlider(S_AIFarm, "S1 Range", 5, 100, RyuConfig.S1R, function(v) RyuConfig.S1R = v end)
CreateToggle(S_AIFarm, "Use Skill 2", RyuConfig.S2, function(v) RyuConfig.S2 = v end)
CreateSlider(S_AIFarm, "S2 Range", 5, 100, RyuConfig.S2R, function(v) RyuConfig.S2R = v end)
CreateToggle(S_AIFarm, "Use Skill 3", RyuConfig.S3, function(v) RyuConfig.S3 = v end)
CreateSlider(S_AIFarm, "S3 Range", 5, 100, RyuConfig.S3R, function(v) RyuConfig.S3R = v end)
CreateToggle(S_AIFarm, "Use Skill 4", RyuConfig.S4, function(v) RyuConfig.S4 = v end)
CreateSlider(S_AIFarm, "S4 Range", 5, 100, RyuConfig.S4R, function(v) RyuConfig.S4R = v end)

-- TARGET
local S_Target = CreateSection(T_Farm, "Target Settings")
CreateInput(S_Target, "Target Username...", function(v) RyuConfig.TargetPlayer = v end)
CreateToggle(S_Target, "Stay Behind Target", RyuConfig.StayBehind, function(v) RyuConfig.StayBehind = v end)
CreateSlider(S_Target, "Behind Distance", 1, 15, RyuConfig.BehindDist, function(v) RyuConfig.BehindDist = v end)

-- MONEY FARM
local S_MFarm = CreateSection(T_Farm, "Money Farm (Alt Method)")
CreateLabel(S_MFarm, "How to use:\n1. Main account should be set to Farmer\n2. Alts should be set to Victim\n3. Turn everything on.\n\nMissunderstandings:\nYes all of your alts need the script (use a roblox cloner)\nBefore you ask help in the discord make sure to check if you did everything right.\nAlso very importan: stay happy in life and never give up.")
CreateToggle(S_MFarm, "Enable Money Farm", RyuConfig.MoneyFarm, function(v) RyuConfig.MoneyFarm = v end)
CreateButton(S_MFarm, "Role: " .. RyuConfig.MFRole, function()
    if RyuConfig.MFRole == "Farmer" then RyuConfig.MFRole = "Victim" else RyuConfig.MFRole = "Farmer" end
    S_MFarm:GetChildren()[4].Text = "Role: " .. RyuConfig.MFRole
end)
CreateInput(S_MFarm, "Victim: Enter Farmer Name", function(v) RyuConfig.MFVictim = v end)

-- CONFIG
local S_Cfg = CreateSection(T_Farm, "Config & Server")
CreateToggle(S_Cfg, "Auto Rejoin", RyuConfig.AutoRejoin, function(v) RyuConfig.AutoRejoin = v end)
CreateSlider(S_Cfg, "Rejoin if players < X", 1, 10, RyuConfig.MinPlayers, function(v) RyuConfig.MinPlayers = v end)
CreateToggle(S_Cfg, "Find 80% Full Lobby", RyuConfig.HighPop, function(v) RyuConfig.HighPop = v end)
CreateInput(S_Cfg, "Alt Joiner: Enter Main Name", function(v) RyuConfig.TargetJoinUser = v end)
CreateButton(S_Cfg, "Join Main", function()
    -- snipe / join logic placeholder
    RyuNotify:Send("Alt Joiner", "Attempting to join " .. RyuConfig.TargetJoinUser, 3)
end)
CreateButton(S_Cfg, "Save Settings", function()
    SaveConfig()
    RyuNotify:Send("Config", "Settings saved!", 3)
end)

-- init logic
Tabs[1].Btn.TextColor3 = Theme.Text
Tabs[1].Btn.BackgroundColor3 = Theme.SectionBG
Tabs[1].Page.Visible = true

-- ========================================================
-- CORE ENGINE & LOOP
-- ========================================================
local bv = nil
local lastHealth = 100
local m1Spamming = false
local boxSpawned = false
local boxPart = nil

-- auto black flash variables
local lastFov = Workspace.CurrentCamera.FieldOfView
local flashFired = false

-- domain bypass
Workspace.ChildAdded:Connect(function(c)
    if RyuConfig.DomainBypass then
        task.wait(0.1)
        if c.Name:lower():find("domain") or c.Name:lower():find("sphere") or c:IsA("MeshPart") then
            c.Transparency = 1
            if c:IsA("Model") then
                for _, p in pairs(c:GetDescendants()) do
                    if p:IsA("BasePart") then p.Transparency = 1 end
                end
            end
        end
    end
end)

RunService.Stepped:Connect(function()
    pcall(function()
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local cam = Workspace.CurrentCamera
        
        if not char or not hrp or not hum then return end
        
        -- speed
        if RyuConfig.Speed and hum.MoveDirection.Magnitude > 0 then
            hrp.CFrame = hrp.CFrame + (hum.MoveDirection * (RyuConfig.SpeedVal / 100))
        end
        
        -- fly
        if RyuConfig.Fly then
            if not bv then
                bv = Instance.new("BodyVelocity", hrp)
                bv.MaxForce = Vector3.new(1e9,1e9,1e9)
            end
            hum.PlatformStand = true
            local dir = Vector3.zero
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then dir = dir + cam.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then dir = dir - cam.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then dir = dir - cam.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then dir = dir + cam.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then dir = dir - Vector3.new(0,1,0) end
            
            if dir.Magnitude > 0 then bv.Velocity = dir.Unit * 50 else bv.Velocity = Vector3.zero end
            
            -- fly anim spoof
            if hum:GetState() ~= Enum.HumanoidStateType.Freefall then
                hum:ChangeState(Enum.HumanoidStateType.Freefall)
            end
        else
            if bv then bv:Destroy(); bv = nil end
            hum.PlatformStand = false
        end
        
        -- noclip
        if RyuConfig.Noclip then
            for _, p in pairs(char:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
        
        -- invisible (löscht lokales right leg = r6 invis glitch oft, oder offset)
        if RyuConfig.Invisible then
            local lower = char:FindFirstChild("LowerTorso")
            if lower and lower.Transparency == 0 then
                for _, p in pairs(char:GetDescendants()) do
                    if p:IsA("BasePart") and p.Name ~= "HumanoidRootPart" then p.Transparency = 1 end
                end
            end
        end
        
        -- knockback
        if RyuConfig.Knockback then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local tHrp = p.Character.HumanoidRootPart
                    if (tHrp.Position - hrp.Position).Magnitude < 5 then
                        tHrp.Velocity = hrp.CFrame.LookVector * RyuConfig.KnockbackVal
                    end
                end
            end
        end
        
        -- lock on
        if RyuConfig.LockOn then
            local near = nil
            local d = math.huge
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local mag = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                    if mag < d then d = mag; near = p.Character.HumanoidRootPart end
                end
            end
            if near then cam.CFrame = CFrame.lookAt(cam.CFrame.Position, near.Position) end
        end
        
        -- auto black flash (fov detect)
        if RyuConfig.AutoBlackFlash then
            local cfov = cam.FieldOfView
            if cfov > lastFov and lastFov < 50 and not flashFired then
                flashFired = true
                VirtualInputManager:SendMouseButtonEvent(cam.ViewportSize.X/2, cam.ViewportSize.Y/2, 0, true, game, 0)
                task.wait(0.05)
                VirtualInputManager:SendMouseButtonEvent(cam.ViewportSize.X/2, cam.ViewportSize.Y/2, 0, false, game, 0)
                task.delay(1, function() flashFired = false end)
            end
            lastFov = cfov
        end
        
        -- auto dodge
        if RyuConfig.AutoDodge then
            if hum.Health < lastHealth then
                hrp.CFrame = hrp.CFrame * CFrame.new(0, 0, RyuConfig.DodgeRange)
            end
        end
        lastHealth = hum.Health
        
        -- auto block
        if RyuConfig.AutoBlock and remotes.BlockOn and remotes.BlockOff then
            local enemyAttacking = false
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    if (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude <= RyuConfig.BlockRange then
                        local eHum = p.Character:FindFirstChild("Humanoid")
                        if eHum and eHum:FindFirstChild("Animator") then
                            for _, t in pairs(eHum.Animator:GetPlayingAnimationTracks()) do
                                if t.Name:lower():find("attack") or t.Name:lower():find("m1") or t.Name:lower():find("punch") then
                                    enemyAttacking = true
                                    break
                                end
                            end
                        end
                    end
                end
            end
            if enemyAttacking then
                remotes.BlockOn:FireServer()
            else
                remotes.BlockOff:FireServer()
            end
        end
        
        -- no cooldown dash (spamming chase remote)
        if RyuConfig.DashSpam and remotes.Chase then
            if hum.MoveDirection.Magnitude > 0 then
                remotes.Chase:FireServer(false)
            end
        end
        
        -- stay behind target
        if RyuConfig.StayBehind and RyuConfig.TargetPlayer ~= "" then
            local tPlr = Players:FindFirstChild(RyuConfig.TargetPlayer)
            if tPlr and tPlr.Character and tPlr.Character:FindFirstChild("HumanoidRootPart") then
                hrp.CFrame = tPlr.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, RyuConfig.BehindDist)
            end
        end
        
        -- AI FARM
        if RyuConfig.AIFarm then
            local target = nil
            local minD = RyuConfig.AIRange
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChild("Humanoid").Health > 0 then
                    local d = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                    if d < minD then minD = d; target = p.Character end
                end
            end
            
            if target then
                local tHrp = target.HumanoidRootPart
                if RyuConfig.HumanMode then
                    hum:MoveTo(tHrp.Position)
                    if math.random(1, 100) > 95 then hum.Jump = true end
                else
                    hrp.CFrame = tHrp.CFrame * CFrame.new(0, 0, 3)
                end
                
                if not m1Spamming then
                    m1Spamming = true
                    task.spawn(function()
                        VirtualInputManager:SendMouseButtonEvent(0,0,0,true,game,0)
                        task.wait(0.1)
                        VirtualInputManager:SendMouseButtonEvent(0,0,0,false,game,0)
                        
                        if RyuConfig.S1 and minD <= RyuConfig.S1R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.One, false, game) end
                        if RyuConfig.S2 and minD <= RyuConfig.S2R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Two, false, game) end
                        if RyuConfig.S3 and minD <= RyuConfig.S3R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Three, false, game) end
                        if RyuConfig.S4 and minD <= RyuConfig.S4R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Four, false, game) end
                        if RyuConfig.AutoUlt then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.G, false, game) end
                        
                        task.wait(0.2)
                        m1Spamming = false
                    end)
                end
            end
        end
        
        -- MONEY FARM (BOX & ROLES)
        if RyuConfig.MoneyFarm then
            if not boxSpawned then
                boxPart = Instance.new("Part", Workspace)
                boxPart.Size = Vector3.new(100, 5, 100)
                boxPart.Position = Vector3.new(0, 50000, 0)
                boxPart.Anchored = true
                boxPart.Transparency = 0.5
                boxSpawned = true
            end
            
            if RyuConfig.MFRole == "Farmer" then
                if hrp.Position.Y < 49000 then hrp.CFrame = CFrame.new(0, 50005, 0) end
                RyuConfig.AIFarm = true -- force ai farm for farmer
            elseif RyuConfig.MFRole == "Victim" and RyuConfig.MFVictim ~= "" then
                local fPlr = nil
                for _, p in pairs(Players:GetPlayers()) do
                    if p.Name:lower():find(RyuConfig.MFVictim:lower()) or p.DisplayName:lower():find(RyuConfig.MFVictim:lower()) then
                        fPlr = p
                        break
                    end
                end
                if fPlr and fPlr.Character and fPlr.Character:FindFirstChild("HumanoidRootPart") then
                    local fHrp = fPlr.Character.HumanoidRootPart
                    local d = (hrp.Position - fHrp.Position).Magnitude
                    if fHrp.Position.Y > 49000 then
                        if d > 10 then
                            hrp.CFrame = fHrp.CFrame * CFrame.new(0, 0, -2)
                        else
                            hum:MoveTo(fHrp.Position)
                        end
                    end
                end
            end
        else
            if boxSpawned and boxPart then boxPart:Destroy(); boxSpawned = false end
        end
        
    end)
end)

-- jump overrides
UserInputService.JumpRequest:Connect(function()
    if RyuConfig.JumpSpam then
        local c = LocalPlayer.Character
        if c and c:FindFirstChild("HumanoidRootPart") then
            c.HumanoidRootPart.Velocity = Vector3.new(c.HumanoidRootPart.Velocity.X, 50, c.HumanoidRootPart.Velocity.Z)
        end
    elseif RyuConfig.JumpHigh then
        local c = LocalPlayer.Character
        if c and c:FindFirstChild("HumanoidRootPart") then
            c.HumanoidRootPart.Velocity = Vector3.new(c.HumanoidRootPart.Velocity.X, 150, c.HumanoidRootPart.Velocity.Z)
        end
    end
end)

-- auto rejoin logic
task.spawn(function()
    while task.wait(5) do
        if RyuConfig.AutoRejoin then
            if #Players:GetPlayers() <= RyuConfig.MinPlayers then
                local url = "https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Desc&limit=100"
                pcall(function()
                    local req = game:HttpGet(url)
                    local data = HttpService:JSONDecode(req)
                    for _, s in pairs(data.data) do
                        if s.playing < s.maxPlayers and s.id ~= game.JobId then
                            local ratio = s.playing / s.maxPlayers
                            if RyuConfig.HighPop and ratio >= 0.7 and ratio <= 0.9 then
                                TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id, LocalPlayer)
                                break
                            elseif not RyuConfig.HighPop and s.playing > 2 then
                                TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id, LocalPlayer)
                                break
                            end
                        end
                    end
                end)
            end
        end
    end
end)

--// ==========================================
--// RYU HUB - JUJUTSU SHENANIGANS EDITION (TJS)
--// ==========================================
-- ryu hub - jujutsu shenanigans (tjs) v1.2
-- fixed auto farm, lock on, pure monochrome ui, no toggle drag, proper block remotes

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
@@ -10,102 +9,80 @@ local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TeleportService = game:GetService("TeleportService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

--// 1. GUI SECURITY & CLEANUP
local guiParent = CoreGui
-- gui cleanup
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() 
    if type(gethui) == "function" then 
        local hui = gethui()
        if hui then guiParent = hui end
    elseif LocalPlayer:FindFirstChild("PlayerGui") then
        guiParent = LocalPlayer.PlayerGui
    end 
    if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end 
end)

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "RyuHubTJS" then 
        v:Destroy() 
    end 
    if v.Name == "RyuHubTJS" then v:Destroy() end 
end

--// 2. CONFIGURATION & SAVE SYSTEM
local CONFIG_FILE = "RyuHub_TJS_Config.json"
-- tjs knit remotes cache
local knitServices = ReplicatedStorage:WaitForChild("Knit", 5) and ReplicatedStorage.Knit:WaitForChild("Services")
local remotes = {
    BlockOn = knitServices and knitServices:FindFirstChild("BlockService") and knitServices.BlockService.RE:FindFirstChild("Activated"),
    BlockOff = knitServices and knitServices:FindFirstChild("BlockService") and knitServices.BlockService.RE:FindFirstChild("Deactivated"),
    Chase = knitServices and knitServices:FindFirstChild("ItadoriService") and knitServices.ItadoriService.RE:FindFirstChild("Chase"),
    ItadoriActivated = knitServices and knitServices:FindFirstChild("ItadoriService") and knitServices.ItadoriService.RE:FindFirstChild("Activated")
}

-- global config
local CONFIG_FILE = "RyuHub_TJS_Settings.json"
local RyuConfig = {
    -- Player
    SpeedHack = false, SpeedVal = 50,
    Speed = false, SpeedVal = 50,
Fly = false, FlyKey = Enum.KeyCode.X,
JumpHigh = false, JumpSpam = false,
Noclip = false, Invisible = false,

    -- Auto
AutoBlock = false, BlockRange = 15,
AutoBlackFlash = false,
AutoDodge = false, DodgeRange = 20,

    -- Abilities
LockOn = false, LockOnKey = Enum.KeyCode.C,
Knockback = false, KnockbackVal = 50,
DomainBypass = false, DashSpam = false,

    -- AI Farm
AIFarm = false, AIRange = 50, AutoUlt = false, HumanMode = false,
S1 = false, S1R = 10, S2 = false, S2R = 10, S3 = false, S3R = 10, S4 = false, S4R = 10,

    -- Target & Money Farm
TargetPlayer = "", StayBehind = false, BehindDist = 3,
MoneyFarm = false, MFRole = "Farmer", MFVictim = "",

    -- Config
AutoRejoin = false, MinPlayers = 3, HighPop = true,
    TargetJoinUser = "", AntiAFK = false,
    
    GuiColor = Color3.fromRGB(255, 65, 65)
    TargetJoinUser = "", AntiAFK = false
}

-- save/load engine
local function SaveConfig()
pcall(function()
        if type(writefile) == "function" then
            writefile(CONFIG_FILE, HttpService:JSONEncode(RyuConfig))
        end
        if writefile then writefile(CONFIG_FILE, HttpService:JSONEncode(RyuConfig)) end
end)
end

pcall(function()
    if type(readfile) == "function" and type(isfile) == "function" and isfile(CONFIG_FILE) then
    if readfile and isfile and isfile(CONFIG_FILE) then
local data = HttpService:JSONDecode(readfile(CONFIG_FILE))
        for k, v in pairs(data) do
            RyuConfig[k] = v
        end
    end
end)

--// 3. TJS REMOTES (KNIT)
local remotes = {}
pcall(function()
    local knitServices = ReplicatedStorage:WaitForChild("Knit", 5):WaitForChild("Services", 5)
    if knitServices then
        remotes.BlockOn = knitServices:FindFirstChild("BlockService") and knitServices.BlockService.RE:FindFirstChild("Activated")
        remotes.BlockOff = knitServices:FindFirstChild("BlockService") and knitServices.BlockService.RE:FindFirstChild("Deactivated")
        remotes.Chase = knitServices:FindFirstChild("ItadoriService") and knitServices.ItadoriService.RE:FindFirstChild("Chase")
        remotes.Divergent = knitServices:FindFirstChild("DivergentFistService") and knitServices.DivergentFistService.RE:FindFirstChild("Activated")
        for k,v in pairs(data) do RyuConfig[k] = v end
end
end)

--// 4. THEME & UI FRAMEWORK (EXACT COPY OF YOUR OVERLAY)
-- theme & setup (100% monochrome, no red)
local Theme = {
Background = Color3.fromRGB(12, 12, 14), Sidebar = Color3.fromRGB(18, 18, 20),
SectionBG = Color3.fromRGB(24, 24, 26), Text = Color3.fromRGB(250, 250, 250),
SubText = Color3.fromRGB(130, 130, 135), CloudLight = Color3.fromRGB(255, 255, 255),
    CloudDark = Color3.fromRGB(60, 60, 65), Accent = RyuConfig.GuiColor,
    ToggleOff = Color3.fromRGB(35, 35, 38), ToggleOn = RyuConfig.GuiColor,
    Stroke = Color3.fromRGB(45, 45, 50), Warning = Color3.fromRGB(255, 75, 75)
    CloudDark = Color3.fromRGB(60, 60, 65), Accent = Color3.fromRGB(255, 255, 255),
    ToggleOff = Color3.fromRGB(35, 35, 38), ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(45, 45, 50), Warning = Color3.fromRGB(200, 200, 200)
}

local MainSize = UDim2.new(0, math.min(750, camera.ViewportSize.X - 40), 0, math.min(480, camera.ViewportSize.Y - 40))
@@ -136,10 +113,10 @@ local function AddClickPop(element)
end)
end

-- KATANA TOGGLE BUTTON
-- FIXED TOP LEFT TOGGLE BUTTON (No dragging)
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
ToggleBtn.Position = UDim2.new(0, 25, 0, 25)
ToggleBtn.Position = UDim2.new(0, 15, 0, 15) -- FIXED STATIC POSITION
ToggleBtn.BackgroundColor3 = Theme.Sidebar
ToggleBtn.Text = ""
ToggleBtn.Parent = RyuHub
@@ -162,22 +139,8 @@ Instance.new("UICorner", Guard).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Handle).CornerRadius = UDim.new(0, 1)

AddClickPop(ToggleBtn)
local tDragStart, tStartPos, isDraggingBtn = nil, nil, false
ToggleBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDraggingBtn = false; tDragStart = input.Position; tStartPos = ToggleBtn.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if tDragStart and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - tDragStart
        if delta.Magnitude > 5 then 
            isDraggingBtn = true; ToggleBtn.Position = UDim2.new(tStartPos.X.Scale, tStartPos.X.Offset + delta.X, tStartPos.Y.Scale, tStartPos.Y.Offset + delta.Y) 
        end
    end
end)

-- MAIN WINDOW
-- main frame setup
local MainFrame = Instance.new("CanvasGroup")
MainFrame.Size = UDim2.new(0, 0, 0, 0)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
@@ -190,7 +153,7 @@ Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)
local mainStroke = Instance.new("UIStroke", MainFrame)
mainStroke.Color = Theme.Stroke; mainStroke.Transparency = 0.2; mainStroke.Thickness = 1.5

-- DISCORD BACKGROUND
-- background discord text
local DragText = Instance.new("TextLabel", RyuHub)
DragText.Size = UDim2.new(1, 0, 1, 0); DragText.Position = UDim2.new(0, 0, 0, 0); DragText.BackgroundTransparency = 1
DragText.Text = "DISCORD.GG/RYUHUB"; DragText.Font = Enum.Font.GothamBlack; DragText.TextSize = 50
@@ -217,6 +180,8 @@ Topbar.Size = UDim2.new(1, 0, 0, 60); Topbar.BackgroundTransparency = 1
local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 300, 1, 0); Title.Position = UDim2.new(0, 20, 0, 0); Title.BackgroundTransparency = 1
Title.Text = "RYU HUB"; Title.Font = Enum.Font.GothamBlack; Title.TextSize = 22; Title.TextXAlignment = Enum.TextXAlignment.Left
Title.TextColor3 = Theme.Text

local TitleGradient = Instance.new("UIGradient", Title)
TitleGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(180, 180, 185)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 255, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(180, 180, 185))}
TitleGradient.Offset = Vector2.new(-1, 0)
@@ -249,6 +214,7 @@ end)
local Line = Instance.new("Frame", ContentWrapper)
Line.Size = UDim2.new(1, -40, 0, 1); Line.Position = UDim2.new(0, 20, 0, 65); Line.BackgroundColor3 = Theme.Stroke; Line.BorderSizePixel = 0

-- UI Layout Framework
local Sidebar = Instance.new("ScrollingFrame", ContentWrapper)
Sidebar.Size = UDim2.new(0, SidebarWidth, 1, -85); Sidebar.Position = UDim2.new(0, 10, 0, 75); Sidebar.BackgroundTransparency = 1; Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar)
@@ -257,7 +223,6 @@ SideLayout.Padding = UDim.new(0, 6); SideLayout.HorizontalAlignment = Enum.Horiz
local ContentContainer = Instance.new("Frame", ContentWrapper)
ContentContainer.Size = UDim2.new(1, -(SidebarWidth + 25), 1, -85); ContentContainer.Position = UDim2.new(0, SidebarWidth + 15, 0, 75); ContentContainer.BackgroundTransparency = 1

--// 5. ACCORDEON SYSTEM
local Tabs = {}
local sidebarOrderCounter = 0
local itemOrderCounter = 0
@@ -434,14 +399,14 @@ local function CreateToggle(section, text, descText, defaultState, callback)
isOn = not isOn
pcall(function()
if isOn then
                TweenService:Create(tBtn, TweenInfo.new(0.25), {BackgroundColor3 = Theme.ToggleOn}):Play()
                TweenService:Create(circle, TweenInfo.new(0.25), {Position = UDim2.new(1, -19, 0.5, -8), BackgroundColor3 = Theme.Background}):Play()
                TweenService:Create(label, TweenInfo.new(0.25), {TextColor3 = Theme.Text}):Play()
                TweenService:Create(tBtn, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {BackgroundColor3 = Theme.ToggleOn}):Play()
                TweenService:Create(circle, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Position = UDim2.new(1, -19, 0.5, -8), BackgroundColor3 = Theme.Background}):Play()
                TweenService:Create(label, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {TextColor3 = Theme.Text}):Play()
bStroke.Color = Theme.ToggleOn
else
                TweenService:Create(tBtn, TweenInfo.new(0.25), {BackgroundColor3 = Theme.ToggleOff}):Play()
                TweenService:Create(circle, TweenInfo.new(0.25), {Position = UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = Color3.fromRGB(150, 150, 150)}):Play()
                TweenService:Create(label, TweenInfo.new(0.25), {TextColor3 = Theme.SubText}):Play()
                TweenService:Create(tBtn, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {BackgroundColor3 = Theme.ToggleOff}):Play()
                TweenService:Create(circle, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Position = UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = Color3.fromRGB(150, 150, 150)}):Play()
                TweenService:Create(label, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {TextColor3 = Theme.SubText}):Play()
bStroke.Color = Theme.Stroke
end
end)
@@ -527,7 +492,7 @@ local TabCombat = CreateMainTab("Combat")

local SubPlayer = CreateSubTab(TabCombat, "Player")
local SecPlayer = CreateSection(SubPlayer, "Movement Mods")
CreateToggle(SecPlayer, "Speed Hack", "Modifies character speed", RyuConfig.SpeedHack, function(v) RyuConfig.SpeedHack = v end)
CreateToggle(SecPlayer, "Speed Hack", "Modifies character speed", RyuConfig.Speed, function(v) RyuConfig.Speed = v end)
CreateSlider(SecPlayer, "Speed Value", 16, 150, RyuConfig.SpeedVal, function(v) RyuConfig.SpeedVal = v end)
CreateToggle(SecPlayer, "Fly", "Fly locally", RyuConfig.Fly, function(v) RyuConfig.Fly = v end)
CreateToggle(SecPlayer, "High Jump", "Jump extremely high", RyuConfig.JumpHigh, function(v) RyuConfig.JumpHigh = v end)
@@ -538,7 +503,7 @@ CreateButton(SecPlayer, "Dance (Visible to everyone)", Theme.ToggleOff, function

local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAutoBlock = CreateSection(SubAuto, "Defensive")
CreateToggle(SecAutoBlock, "Auto Block", "Reacts to enemy animations", RyuConfig.AutoBlock, function(v) RyuConfig.AutoBlock = v end)
CreateToggle(SecAutoBlock, "Auto Block (Perfect Counter)", "Reacts to enemy animations via ItadoriRemote", RyuConfig.AutoBlock, function(v) RyuConfig.AutoBlock = v end)
CreateSlider(SecAutoBlock, "Block React Range", 5, 50, RyuConfig.BlockRange, function(v) RyuConfig.BlockRange = v end)
CreateToggle(SecAutoBlock, "Auto Dodge (TP Back)", "Dodge backwards on damage", RyuConfig.AutoDodge, function(v) RyuConfig.AutoDodge = v end)
CreateSlider(SecAutoBlock, "Dodge Distance", 5, 50, RyuConfig.DodgeRange, function(v) RyuConfig.DodgeRange = v end)
@@ -549,7 +514,7 @@ CreateLabel(SecAutoHit, "Auto Combos: Join discord.gg/ryuhub and send clips of y

local SubAbil = CreateSubTab(TabCombat, "Abilities")
local SecAbil = CreateSection(SubAbil, "Combat Enhancements")
CreateToggle(SecAbil, "Lock On (Nearest)", "Auto aim camera", RyuConfig.LockOn, function(v) RyuConfig.LockOn = v end)
CreateToggle(SecAbil, "Lock On (Nearest Head)", "Auto aim camera precisely at head", RyuConfig.LockOn, function(v) RyuConfig.LockOn = v end)
CreateToggle(SecAbil, "Knockback M1s", "Pushes enemies away", RyuConfig.Knockback, function(v) RyuConfig.Knockback = v end)
CreateSlider(SecAbil, "Knockback Force", 10, 300, RyuConfig.KnockbackVal, function(v) RyuConfig.KnockbackVal = v end)
CreateToggle(SecAbil, "Domain Eraser", "Bypass domains (Local)", RyuConfig.DomainBypass, function(v) RyuConfig.DomainBypass = v end)
@@ -594,7 +559,7 @@ CreateSlider(SecTarget, "Distance Behind", 1, 15, RyuConfig.BehindDist, function

local SubMFarm = CreateSubTab(TabFarm, "Money Farm")
local SecMFarm = CreateSection(SubMFarm, "Alt Money Farm")
CreateLabel(SecMFarm, "How to use:\n1. Main account = Farmer\n2. Alts = Helper\n3. Turn everything on.\nAlts need script too!")
CreateLabel(SecMFarm, "How to use:\n1. Main account = Farmer\n2. Alts = Victim\n3. Turn everything on.\nAlts need script too!")
CreateToggle(SecMFarm, "Enable Money Farm", "Sky box farm", RyuConfig.MoneyFarm, function(v) RyuConfig.MoneyFarm = v end)

local RoleBtn
@@ -633,7 +598,6 @@ local flashFired = false
local lastFov = 70
local boxSpawned = false
local boxPart = nil
local m1Spamming = false
local bv = nil

-- Domain Eraser
@@ -653,17 +617,19 @@ Workspace.ChildAdded:Connect(function(c)
end
end)

-- Anti AFK
-- Anti AFK Engine
LocalPlayer.Idled:Connect(function()
if RyuConfig.AntiAFK then
pcall(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
            if VirtualUser then
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.new())
            end
end)
end
end)

-- Jump Request
-- Jump Request Engine
UserInputService.JumpRequest:Connect(function()
pcall(function()
if RyuConfig.JumpSpam then
@@ -680,7 +646,35 @@ UserInputService.JumpRequest:Connect(function()
end)
end)

-- Main Background Loop
-- Lock On Engine (Smoother inside RenderStepped)
RunService.RenderStepped:Connect(function()
    pcall(function()
        local cam = Workspace.CurrentCamera
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if RyuConfig.LockOn and hrp and cam then
            local near = nil
            local d = math.huge
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local ehum = p.Character:FindFirstChild("Humanoid")
                    if ehum and ehum.Health > 0 then
                        local mag = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if mag < d then 
                            d = mag 
                            near = p.Character:FindFirstChild("Head") or p.Character.HumanoidRootPart 
                        end
                    end
                end
            end
            if near then 
                cam.CFrame = CFrame.lookAt(cam.CFrame.Position, near.Position) 
            end
        end
    end)
end)

-- Main Background Loop (Physics & Combat Logic)
RunService.Stepped:Connect(function()
pcall(function()
local char = LocalPlayer.Character
@@ -691,7 +685,7 @@ RunService.Stepped:Connect(function()
if not char or not hrp or not hum then return end

-- SPEED HACK
        if RyuConfig.SpeedHack and hum.MoveDirection.Magnitude > 0 then
        if RyuConfig.Speed and hum.MoveDirection.Magnitude > 0 then
hrp.CFrame = hrp.CFrame + (hum.MoveDirection * (RyuConfig.SpeedVal / 100))
end

@@ -745,27 +739,16 @@ RunService.Stepped:Connect(function()
end
end

        -- LOCK ON
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
        
-- AUTO BLACK FLASH (FOV DETECT)
if RyuConfig.AutoBlackFlash then
local cfov = cam.FieldOfView
if cfov > lastFov and lastFov < 50 and not flashFired then
flashFired = true
                VirtualInputManager:SendMouseButtonEvent(cam.ViewportSize.X/2, cam.ViewportSize.Y/2, 0, true, game, 0)
                task.wait(0.05)
                VirtualInputManager:SendMouseButtonEvent(cam.ViewportSize.X/2, cam.ViewportSize.Y/2, 0, false, game, 0)
                if VirtualInputManager and type(VirtualInputManager.SendMouseButtonEvent) == "function" then
                    VirtualInputManager:SendMouseButtonEvent(cam.ViewportSize.X/2, cam.ViewportSize.Y/2, 0, true, game, 0)
                    task.wait(0.05)
                    VirtualInputManager:SendMouseButtonEvent(cam.ViewportSize.X/2, cam.ViewportSize.Y/2, 0, false, game, 0)
                end
task.delay(1, function() flashFired = false end)
end
lastFov = cfov
@@ -779,16 +762,17 @@ RunService.Stepped:Connect(function()
end
lastHealth = hum.Health

        -- AUTO BLOCK
        if RyuConfig.AutoBlock and remotes.BlockOn and remotes.BlockOff then
        -- AUTO BLOCK (ITADORI ACTIVATED REMOTE TRIGGER)
        if RyuConfig.AutoBlock then
local enemyAttacking = false
for _, p in pairs(Players:GetPlayers()) do
if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
if (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude <= RyuConfig.BlockRange then
local eHum = p.Character:FindFirstChild("Humanoid")
if eHum and eHum:FindFirstChild("Animator") then
for _, t in pairs(eHum.Animator:GetPlayingAnimationTracks()) do
                                if t.Name:lower():find("attack") or t.Name:lower():find("m1") or t.Name:lower():find("punch") then
                                local n = t.Name:lower()
                                if n:find("attack") or n:find("m1") or n:find("punch") or n:find("swing") then
enemyAttacking = true
break
end
@@ -797,10 +781,16 @@ RunService.Stepped:Connect(function()
end
end
end
            
if enemyAttacking then
                remotes.BlockOn:FireServer()
                pcall(function()
                    if remotes.ItadoriActivated then
                        remotes.ItadoriActivated:FireServer(false)
                    end
                end)
                if remotes.BlockOn then remotes.BlockOn:FireServer() end
else
                remotes.BlockOff:FireServer()
                if remotes.BlockOff then remotes.BlockOff:FireServer() end
end
end

@@ -825,46 +815,6 @@ RunService.Stepped:Connect(function()
end
end

        -- AI FARM
        if RyuConfig.AIFarm then
            local target = nil
            local minD = RyuConfig.AIRange
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
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
@@ -901,16 +851,64 @@ RunService.Stepped:Connect(function()
end
else
if boxSpawned and boxPart then boxPart:Destroy(); boxSpawned = false end
            if RyuConfig.MFRole == "Farmer" and RyuConfig.MoneyFarm == false then
                -- Disable forced AI farm when disabling Money Farm
                -- Only toggle off if user actually turned off MoneyFarm
            end
end
        
end)
end)

--// AUTO REJOIN LOGIC
--// AI FARMING ENGINE (DEDICATED LOOP TO PREVENT CRASHES)
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if not RyuConfig.AIFarm then return end
            
            local char = LocalPlayer.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if not char or not hrp or not hum or hum.Health <= 0 then return end
            
            local target = nil
            local minD = RyuConfig.AIRange
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local ehum = p.Character:FindFirstChild("Humanoid")
                    if ehum and ehum.Health > 0 then
                        local d = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if d < minD then 
                            minD = d
                            target = p.Character 
                        end
                    end
                end
            end
            
            if target then
                local tHrp = target.HumanoidRootPart
                if RyuConfig.HumanMode then
                    hum:MoveTo(tHrp.Position)
                    if math.random(1, 100) > 90 then hum.Jump = true end
                else
                    hrp.CFrame = tHrp.CFrame * CFrame.new(0, 0, 3)
                end
                
                -- Attack & Use Skills
                if VirtualInputManager and type(VirtualInputManager.SendMouseButtonEvent) == "function" then
                    local cx, cy = camera.ViewportSize.X/2, camera.ViewportSize.Y/2
                    VirtualInputManager:SendMouseButtonEvent(cx, cy, 0, true, game, 0)
                    task.wait(0.05)
                    VirtualInputManager:SendMouseButtonEvent(cx, cy, 0, false, game, 0)
                    
                    if RyuConfig.S1 and minD <= RyuConfig.S1R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.One, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.One, false, game) end
                    if RyuConfig.S2 and minD <= RyuConfig.S2R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Two, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Two, false, game) end
                    if RyuConfig.S3 and minD <= RyuConfig.S3R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Three, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Three, false, game) end
                    if RyuConfig.S4 and minD <= RyuConfig.S4R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Four, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Four, false, game) end
                    if RyuConfig.AutoUlt then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.G, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.G, false, game) end
                end
            end
        end)
    end
end)

-- auto rejoin logic
task.spawn(function()
while task.wait(5) do
if RyuConfig.AutoRejoin then

--// ============================================================================
--// RYU HUB - DOORS EDITION (ULTIMATE V1)
--// ============================================================================

if game.GameId ~= 2440500124 then return end

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lighting = game:GetService("Lighting")
local ProximityPromptService = game:GetService("ProximityPromptService")
local PathfindingService = game:GetService("PathfindingService")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")
local collision = character:WaitForChild("Collision", 3)

local gameData = ReplicatedStorage:WaitForChild("GameData")
local floor = gameData:WaitForChild("Floor")
local latestRoom = gameData:WaitForChild("LatestRoom")
local remotesFolder = ReplicatedStorage:WaitForChild("RemotesFolder")

--// RYU CONFIGURATION
local RyuConfig = {
    -- Movement
    Speed = false, SpeedBoost = 5, Noclip = false, NoAccel = false,
    Fly = false, FlySpeed = 1, FlyUpOffset = 0, FixExitDelay = false,
    -- Interaction
    DoorReach = false, PromptClip = false, PromptRange = 1,
    -- Exploits
    NoclipBypass = false, SpamTools = false,
    AntiDupe = false, AntiEyes = false, AntiSnare = false, AntiSeek = false,
    DeleteFigure = false, NoScreech = false, NoA90 = false,
    -- Visuals
    GhostBody = false, BodyTrans = 0.5, Fullbright = false, NoFog = false, FOV = 70,
    -- ESP
    ESPDoor = false, ESPEntity = false, ESPItem = false, ESPObjective = false, ESPPlayer = false, ESPGold = false,
    -- Automation
    AutoInteract = false, AutoPadlock = false, AutoBreaker = false
}

--// GUI CLEANUP
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() 
    if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end 
end)

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "RyuHubDoors" then v:Destroy() end 
end

--// UI SETUP (RYU HUB STYLE)
local Theme = {
    Background = Color3.fromRGB(12, 12, 14),
    Sidebar = Color3.fromRGB(18, 18, 20),
    SectionBG = Color3.fromRGB(24, 24, 26),
    Text = Color3.fromRGB(250, 250, 250),
    SubText = Color3.fromRGB(130, 130, 135),
    Accent = Color3.fromRGB(255, 255, 255),
    ToggleOff = Color3.fromRGB(35, 35, 38),
    ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(45, 45, 50)
}

local MainSize = UDim2.new(0, 600, 0, 400) 
local SidebarWidth = 160

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubDoors"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

local MainFrame = Instance.new("Frame", RyuHub)
MainFrame.Size = MainSize
MainFrame.Position = UDim2.new(0.5, -MainSize.X.Offset/2, 0.5, -MainSize.Y.Offset/2)
MainFrame.BackgroundColor3 = Theme.Background
MainFrame.Active = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

local Topbar = Instance.new("Frame", MainFrame)
Topbar.Size = UDim2.new(1, 0, 0, 60)
Topbar.BackgroundTransparency = 1

local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 300, 1, 0)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "RYU HUB : DOORS"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 22
Title.TextColor3 = Theme.Text
Title.TextXAlignment = Enum.TextXAlignment.Left

local ToggleBtn = Instance.new("TextButton", RyuHub)
ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
ToggleBtn.Position = UDim2.new(0, 25, 0, 25)
ToggleBtn.BackgroundColor3 = Theme.Sidebar
ToggleBtn.Text = "R"
ToggleBtn.Font = Enum.Font.GothamBlack
ToggleBtn.TextColor3 = Theme.Accent
ToggleBtn.TextSize = 20
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)

local isMenuOpen = true
ToggleBtn.Activated:Connect(function()
    isMenuOpen = not isMenuOpen
    MainFrame.Visible = isMenuOpen
end)

local ContentContainer = Instance.new("Frame", MainFrame)
ContentContainer.Size = UDim2.new(1, -(SidebarWidth + 25), 1, -85)
ContentContainer.Position = UDim2.new(0, SidebarWidth + 15, 0, 75)
ContentContainer.BackgroundTransparency = 1

local Sidebar = Instance.new("ScrollingFrame", MainFrame)
Sidebar.Size = UDim2.new(0, SidebarWidth, 1, -85)
Sidebar.Position = UDim2.new(0, 10, 0, 75)
Sidebar.BackgroundTransparency = 1
Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar)
SideLayout.Padding = UDim.new(0, 6)

local Tabs = {}
local function CreateMainTab(name)
    local tabObj = { Btn = nil, SubContainer = nil, SubLayout = nil, IsOpen = false, SubTabs = {} }
    
    local tabBtn = Instance.new("TextButton", Sidebar)
    tabBtn.Size = UDim2.new(1, 0, 0, 36)
    tabBtn.BackgroundColor3 = Theme.Sidebar
    tabBtn.Text = "  " .. string.upper(name)
    tabBtn.TextColor3 = Theme.SubText
    tabBtn.Font = Enum.Font.GothamBlack
    tabBtn.TextSize = 13
    tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8)
    tabObj.Btn = tabBtn

    local subContainer = Instance.new("Frame", Sidebar)
    subContainer.Size = UDim2.new(1, 0, 0, 0)
    subContainer.BackgroundTransparency = 1
    subContainer.ClipsDescendants = true
    tabObj.SubContainer = subContainer

    local subLayout = Instance.new("UIListLayout", subContainer)
    subLayout.Padding = UDim.new(0, 2)
    tabObj.SubLayout = subLayout

    tabBtn.Activated:Connect(function()
        tabObj.IsOpen = not tabObj.IsOpen
        TweenService:Create(subContainer, TweenInfo.new(0.25), {Size = tabObj.IsOpen and UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) or UDim2.new(1, 0, 0, 0)}):Play()
        tabBtn.TextColor3 = tabObj.IsOpen and Theme.Text or Theme.SubText
    end)
    
    subLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        if tabObj.IsOpen then subContainer.Size = UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) end
    end)
    
    table.insert(Tabs, tabObj)
    return tabObj
end

local function CreateSubTab(tabObj, subName)
    local subBtn = Instance.new("TextButton", tabObj.SubContainer)
    subBtn.Size = UDim2.new(1, 0, 0, 28)
    subBtn.BackgroundTransparency = 1
    subBtn.Text = "     " .. subName
    subBtn.TextColor3 = Theme.SubText
    subBtn.Font = Enum.Font.GothamMedium
    subBtn.TextSize = 12
    subBtn.TextXAlignment = Enum.TextXAlignment.Left
    
    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = 2
    page.Visible = false
    
    local pageLayout = Instance.new("UIListLayout", page)
    pageLayout.Padding = UDim.new(0, 12)
    pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    pageLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() 
        page.CanvasSize = UDim2.new(0, 0, 0, pageLayout.AbsoluteContentSize.Y + 20) 
    end)
    
    subBtn.Activated:Connect(function()
        for _, v in pairs(ContentContainer:GetChildren()) do v.Visible = false end
        page.Visible = true
    end)
    
    return page
end

local function CreateSection(page, titleText)
    local section = Instance.new("Frame", page)
    section.Size = UDim2.new(0.98, 0, 0, 50)
    section.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", section).CornerRadius = UDim.new(0, 10)
    local secLayout = Instance.new("UIListLayout", section)
    secLayout.Padding = UDim.new(0, 10)
    secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    Instance.new("UIPadding", section).PaddingTop = UDim.new(0, 12)
    Instance.new("UIPadding", section).PaddingBottom = UDim.new(0, 12)
    
    local title = Instance.new("TextLabel", section)
    title.Size = UDim2.new(0.92, 0, 0, 24)
    title.BackgroundTransparency = 1
    title.Text = titleText
    title.TextColor3 = Theme.Text
    title.Font = Enum.Font.GothamBold
    title.TextSize = 14
    title.TextXAlignment = Enum.TextXAlignment.Left
    
    secLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() 
        section.Size = UDim2.new(1, 0, 0, secLayout.AbsoluteContentSize.Y + 24) 
    end)
    return section
end

local function CreateToggle(section, text, defaultState, callback)
    local frame = Instance.new("Frame", section)
    frame.Size = UDim2.new(0.92, 0, 0, 34)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.7, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Theme.Text
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local tBtn = Instance.new("TextButton", frame)
    tBtn.Size = UDim2.new(0, 42, 0, 22)
    tBtn.Position = UDim2.new(1, -42, 0, 6)
    tBtn.BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff
    tBtn.Text = ""
    Instance.new("UICorner", tBtn).CornerRadius = UDim.new(1, 0)
    
    local isOn = defaultState
    tBtn.Activated:Connect(function() 
        isOn = not isOn
        tBtn.BackgroundColor3 = isOn and Theme.ToggleOn or Theme.ToggleOff
        if callback then callback(isOn) end 
    end)
end

local function CreateSlider(section, text, min, max, default, callback)
    local frame = Instance.new("Frame", section)
    frame.Size = UDim2.new(0.92, 0, 0, 50)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(1, 0, 0, 20)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Theme.SubText
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local valLabel = Instance.new("TextLabel", frame)
    valLabel.Size = UDim2.new(0, 40, 0, 20)
    valLabel.Position = UDim2.new(1, -40, 0, 0)
    valLabel.BackgroundTransparency = 1
    valLabel.Text = tostring(default)
    valLabel.TextColor3 = Theme.Accent
    valLabel.Font = Enum.Font.GothamBold
    valLabel.TextSize = 13
    valLabel.TextXAlignment = Enum.TextXAlignment.Right
    
    local sliderBg = Instance.new("Frame", frame)
    sliderBg.Size = UDim2.new(1, 0, 0, 4)
    sliderBg.Position = UDim2.new(0, 0, 0, 32)
    sliderBg.BackgroundColor3 = Theme.ToggleOff
    Instance.new("UICorner", sliderBg).CornerRadius = UDim.new(1, 0)
    
    local sliderFill = Instance.new("Frame", sliderBg)
    sliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    sliderFill.BackgroundColor3 = Theme.Accent
    Instance.new("UICorner", sliderFill).CornerRadius = UDim.new(1, 0)
    
    local knob = Instance.new("TextButton", sliderFill)
    knob.Size = UDim2.new(0, 14, 0, 14)
    knob.Position = UDim2.new(1, -7, 0.5, -7)
    knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    knob.Text = ""
    Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
    
    local dragging = false
    knob.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relative = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            local value = math.floor(min + (max - min) * relative)
            valLabel.Text = tostring(value)
            sliderFill.Size = UDim2.new(relative, 0, 1, 0)
            if callback then callback(value) end
        end
    end)
end

local function CreateButton(section, text, callback)
    local btn = Instance.new("TextButton", section)
    btn.Size = UDim2.new(0.92, 0, 0, 34)
    btn.BackgroundColor3 = Theme.SectionBG
    btn.Text = text
    btn.TextColor3 = Theme.Text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", btn).Color = Theme.Stroke
    btn.Activated:Connect(callback)
end

--// =======================
--// MENU ARCHITECTURE
--// =======================

-- 1. PLAYER TAB
local TabPlayer = CreateMainTab("Player")
local SubMovement = CreateSubTab(TabPlayer, "Movement")
local SubInteract = CreateSubTab(TabPlayer, "Interaction")
local SubMisc = CreateSubTab(TabPlayer, "Fun & Misc")

local SecSpeed = CreateSection(SubMovement, "Character Modifiers")
CreateToggle(SecSpeed, "Speed Boost", false, function(s) RyuConfig.Speed = s end)
CreateSlider(SecSpeed, "Boost Amount", 1, 25, 5, function(v) RyuConfig.SpeedBoost = v end)
CreateToggle(SecSpeed, "Noclip", false, function(s) RyuConfig.Noclip = s end)
CreateToggle(SecSpeed, "No Acceleration (Ice Skates off)", false, function(s) RyuConfig.NoAccel = s end)

local SecFly = CreateSection(SubMovement, "Flight")
local flyVelocity
CreateToggle(SecFly, "Enable Flight", false, function(s) 
    RyuConfig.Fly = s 
    if s then
        flyVelocity = Instance.new("BodyVelocity", rootPart)
        flyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
        flyVelocity.Velocity = Vector3.new(0,0,0)
    else
        if flyVelocity then flyVelocity:Destroy() end
    end
end)
CreateSlider(SecFly, "Fly Speed", 1, 5, 1, function(v) RyuConfig.FlySpeed = v end)

local SecReach = CreateSection(SubInteract, "Reach & Prompts")
CreateToggle(SecReach, "Door Reach", false, function(s) RyuConfig.DoorReach = s end)
CreateToggle(SecReach, "Prompt Clip (Through Walls)", false, function(s) RyuConfig.PromptClip = s end)
CreateSlider(SecReach, "Prompt Range Multiplier", 1, 3, 1, function(v) RyuConfig.PromptRange = v end)
CreateToggle(SecReach, "Fix Exit Delay", false, function(s) RyuConfig.FixExitDelay = s end)

local SecFun = CreateSection(SubMisc, "Actions")
CreateButton(SecFun, "Die", function() if humanoid then humanoid.Health = 0 end end)
CreateButton(SecFun, "Revive", function() remotesFolder.Revive:FireServer() end)
CreateButton(SecFun, "Play Again", function() remotesFolder.PlayAgain:FireServer() end)
CreateButton(SecFun, "Lobby", function() remotesFolder.Lobby:FireServer() end)

-- 2. EXPLOITS TAB
local TabExploits = CreateMainTab("Exploits")
local SubBypass = CreateSubTab(TabExploits, "Bypasses")
local SubEntity = CreateSubTab(TabExploits, "Anti-Entities")

local SecBypass = CreateSection(SubBypass, "Core Bypasses")
CreateToggle(SecBypass, "Noclip Bypass (Safe)", false, function(s) RyuConfig.NoclipBypass = s end)
CreateToggle(SecBypass, "Spam Tools", false, function(s) RyuConfig.SpamTools = s end)

local SecAnti = CreateSection(SubEntity, "Entity Protection")
CreateToggle(SecAnti, "Anti-Dupe (Fake Doors)", false, function(s) RyuConfig.AntiDupe = s end)
CreateToggle(SecAnti, "Anti-Eyes", false, function(s) RyuConfig.AntiEyes = s end)
CreateToggle(SecAnti, "Anti-Snare", false, function(s) RyuConfig.AntiSnare = s end)
CreateToggle(SecAnti, "Anti-Seek", false, function(s) RyuConfig.AntiSeek = s end)
CreateToggle(SecAnti, "Delete Figure (Room 50)", false, function(s) RyuConfig.DeleteFigure = s end)
CreateToggle(SecAnti, "No Screech", false, function(s) RyuConfig.NoScreech = s end)
CreateToggle(SecAnti, "No A-90", false, function(s) RyuConfig.NoA90 = s end)

-- 3. VISUALS TAB
local TabVisuals = CreateMainTab("Visuals")
local SubEnv = CreateSubTab(TabVisuals, "Environment")
local SubESP = CreateSubTab(TabVisuals, "ESP")

local SecLighting = CreateSection(SubEnv, "Lighting")
CreateToggle(SecLighting, "Fullbright", false, function(s) RyuConfig.Fullbright = s end)
CreateToggle(SecLighting, "No Fog", false, function(s) RyuConfig.NoFog = s end)
CreateSlider(SecLighting, "Field of View", 70, 120, 70, function(v) camera.FieldOfView = v end)

local SecGhost = CreateSection(SubEnv, "Ghost Body")
CreateToggle(SecGhost, "Enable Ghost Body", false, function(s) RyuConfig.GhostBody = s end)

local SecESPMain = CreateSection(SubESP, "Object ESP")
CreateToggle(SecESPMain, "Doors ESP", false, function(s) RyuConfig.ESPDoor = s end)
CreateToggle(SecESPMain, "Entity ESP", false, function(s) RyuConfig.ESPEntity = s end)
CreateToggle(SecESPMain, "Items ESP", false, function(s) RyuConfig.ESPItem = s end)
CreateToggle(SecESPMain, "Objectives ESP (Keys/Levers)", false, function(s) RyuConfig.ESPObjective = s end)
CreateToggle(SecESPMain, "Gold ESP", false, function(s) RyuConfig.ESPGold = s end)
CreateToggle(SecESPMain, "Players ESP", false, function(s) RyuConfig.ESPPlayer = s end)

-- 4. AUTOMATION TAB
local TabAuto = CreateMainTab("Automation")
local SubAuto = CreateSubTab(TabAuto, "Auto Tasks")

local SecAuto = CreateSection(SubAuto, "Game Automation")
CreateToggle(SecAuto, "Auto Interact (Insta Grab/Open)", false, function(s) RyuConfig.AutoInteract = s end)
CreateToggle(SecAuto, "Auto Padlock", false, function(s) RyuConfig.AutoPadlock = s end)
CreateToggle(SecAuto, "Auto Breaker Box", false, function(s) RyuConfig.AutoBreaker = s end)

--// =======================
--// BACKEND LOGIC LOOP
--// =======================

local oldAmbient = Lighting.Ambient
local oldFog = Lighting.FogEnd

RunService.RenderStepped:Connect(function()
    if not character or not rootPart or not humanoid then return end

    -- Visuals
    if RyuConfig.Fullbright then
        Lighting.Ambient = Color3.new(1, 1, 1)
    else
        Lighting.Ambient = oldAmbient
    end

    if RyuConfig.NoFog then
        Lighting.FogEnd = 100000
    else
        Lighting.FogEnd = oldFog
    end

    if RyuConfig.GhostBody then
        for _, p in pairs(character:GetChildren()) do
            if p:IsA("BasePart") and p.Name ~= "HumanoidRootPart" then
                p.Transparency = RyuConfig.BodyTrans
            end
        end
    end

    -- Movement
    if RyuConfig.Speed then
        humanoid.WalkSpeed = 15 + RyuConfig.SpeedBoost
    end

    if RyuConfig.Noclip then
        for _, p in pairs(character:GetChildren()) do
            if p:IsA("BasePart") then p.CanCollide = false end
        end
    end

    if RyuConfig.NoAccel then
        rootPart.CustomPhysicalProperties = PhysicalProperties.new(100, 0, 0, 0, 0)
    else
        rootPart.CustomPhysicalProperties = PhysicalProperties.new(0.7, 0.7, 0, 1, 1)
    end

    if RyuConfig.Fly and flyVelocity then
        local moveDir = Vector3.new(0,0,0)
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + camera.CFrame.RightVector end
        flyVelocity.Velocity = moveDir * (RyuConfig.FlySpeed * 50) + Vector3.new(0, RyuConfig.FlyUpOffset, 0)
    end

    -- Exploits / Automation
    if RyuConfig.DoorReach and Workspace.CurrentRooms:FindFirstChild(tostring(latestRoom.Value)) then
        local currentRoomFolder = Workspace.CurrentRooms[tostring(latestRoom.Value)]
        local door = currentRoomFolder:FindFirstChild("Door")
        if door and door:FindFirstChild("ClientOpen") then
            door.ClientOpen:FireServer()
        end
    end

    if RyuConfig.AntiEyes and Workspace:FindFirstChild("Eyes") then
        remotesFolder.MotorReplication:FireServer(0, -89, 0, false)
    end
end)

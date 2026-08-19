--// ==========================================
--// RYU HUB - UI OVERLAY & FULL LOGIC INTEGRATION
--// 100% MONOCHROME CLEAN TEMPLATE (FIXED)
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

local LocalPlayer = Players.LocalPlayer
local Player = LocalPlayer
local Mouse = Player:GetMouse()
local camera = Workspace.CurrentCamera

--// GUI PARENT RESOLVER & CLEANUP
local guiParent
pcall(function()
    if type(gethui) == "function" then
        guiParent = gethui()
    elseif syn and syn.protect_gui then
        guiParent = CoreGui
    end
end)
if not guiParent then guiParent = LocalPlayer:WaitForChild("PlayerGui") end

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "RyuHubUI" then v:Destroy() end 
end

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

--// CONFIGURATION & STATE
local RyuConfig = {
    BlackFlashEnabled = false,
    CameraLockEnabled = true,
    DashDistance = 15,
    FireDelay = 0.25,
    DashDuration = 0.35,
    LockTime = 0.1,
    DashEasingStyle = Enum.EasingStyle.Cubic,
    DashEasingDirection = Enum.EasingDirection.Out,
    EnableDashAssist = false,
    DashCameraLock = false,
    DashOnlyIfFacing = false,
    DashAssistKeybind = Enum.KeyCode.J,
    DashDetectionRange = 15,
    DashBehindDistance = 5,
    DashFlightDuration = 0.42,
    DashCurveStrength = 10,
    DashArchHeight = 3,
    DashLockDuration = 0.35,
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
}

local Logic = {
    TIME_WINDOW = 2, LastFiredTick = 0,
    TargetAnimations = { ["100962226150441"] = true, ["95852624447551"] = true, ["74145636023952"] = true, ["123171106092050"] = true },
    DashAnimLeft = Instance.new("Animation"), DashAnimRight = Instance.new("Animation"),
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

--// ==========================================
--// MAIN GUI CONTAINERS & ANIMATIONS
--// ==========================================

local MainSize = UDim2.new(0, math.min(750, camera.ViewportSize.X - 40), 0, math.min(480, camera.ViewportSize.Y - 40))
local SidebarWidth = 160

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubUI"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

local function AddClickPop(element)
    local orig = element.Size
    element.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            pcall(function() TweenService:Create(element, TweenInfo.new(0.1), {Size = UDim2.new(orig.X.Scale, orig.X.Offset - 4, orig.Y.Scale, orig.Y.Offset - 4)}):Play() end)
        end
    end)
    element.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            pcall(function() TweenService:Create(element, TweenInfo.new(0.3), {Size = orig}):Play() end)
        end
    end)
end

-- Toggle Button
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
ToggleBtn.Position = UDim2.new(0, 15, 0, 60)
ToggleBtn.BackgroundColor3 = Theme.Sidebar
ToggleBtn.Text = ""
ToggleBtn.Parent = RyuHub
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)

local btnStroke = Instance.new("UIStroke", ToggleBtn)
btnStroke.Color = Theme.Accent
btnStroke.Thickness = 2
btnStroke.Transparency = 0.5

local Katana = Instance.new("Frame", ToggleBtn)
Katana.Size = UDim2.new(1, 0, 1, 0)
Katana.BackgroundTransparency = 1
Katana.Rotation = 45

local Blade = Instance.new("Frame", Katana)
Blade.Size = UDim2.new(0, 2, 0, 24)
Blade.Position = UDim2.new(0.5, -1, 0.5, -18)
Blade.BackgroundColor3 = Theme.Text
Blade.BorderSizePixel = 0

local Guard = Instance.new("Frame", Katana)
Guard.Size = UDim2.new(0, 12, 0, 2)
Guard.Position = UDim2.new(0.5, -6, 0.5, 6)
Guard.BackgroundColor3 = Theme.SubText
Guard.BorderSizePixel = 0

local Handle = Instance.new("Frame", Katana)
Handle.Size = UDim2.new(0, 4, 0, 10)
Handle.Position = UDim2.new(0.5, -2, 0.5, 8)
Handle.BackgroundColor3 = Theme.Stroke
Handle.BorderSizePixel = 0

Instance.new("UICorner", Blade).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Guard).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Handle).CornerRadius = UDim.new(0, 1)
AddClickPop(ToggleBtn)

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 0, 0, 0)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.BackgroundColor3 = Theme.Background
MainFrame.BorderSizePixel = 0
MainFrame.Visible = false
MainFrame.ClipsDescendants = true
MainFrame.Active = true
MainFrame.Parent = RyuHub
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

local mainStroke = Instance.new("UIStroke", MainFrame)
mainStroke.Color = Theme.Stroke
mainStroke.Thickness = 1.5

local DragText = Instance.new("TextLabel", MainFrame)
DragText.Size = UDim2.new(1, 0, 1, 0)
DragText.Position = UDim2.new(0, 0, 0, 0)
DragText.BackgroundTransparency = 1
DragText.Text = "DISCORD.GG/RYUHUB"
DragText.Font = Enum.Font.GothamBlack
DragText.TextSize = 50
DragText.TextColor3 = Theme.Text
DragText.TextTransparency = 0.95
DragText.ZIndex = 0

ToggleBtn.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end)
        task.delay(0.3, function() MainFrame.Visible = false end)
    else
        MainFrame.Visible = true
        pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = MainSize, Position = UDim2.new(0.5, -MainSize.X.Offset / 2, 0.5, -MainSize.Y.Offset / 2)}):Play() end)
    end
end)

local ContentWrapper = Instance.new("Frame", MainFrame)
ContentWrapper.Size = UDim2.new(1, 0, 1, 0)
ContentWrapper.BackgroundTransparency = 1
ContentWrapper.BorderSizePixel = 0

local Topbar = Instance.new("Frame", ContentWrapper)
Topbar.Size = UDim2.new(1, 0, 0, 60)
Topbar.BackgroundTransparency = 1

local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 300, 1, 0)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "RYU HUB"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 22
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.TextColor3 = Theme.Text

local SubTitle = Instance.new("TextLabel", Topbar)
SubTitle.Size = UDim2.new(0, 300, 0, 15)
SubTitle.Position = UDim2.new(0, 20, 0, 38)
SubTitle.BackgroundTransparency = 1
SubTitle.Text = "Jujutsu Shenanigans"
SubTitle.TextColor3 = Theme.SubText
SubTitle.Font = Enum.Font.Gotham
SubTitle.TextSize = 11
SubTitle.TextXAlignment = Enum.TextXAlignment.Left

local CloseBtn = Instance.new("TextButton", Topbar)
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(1, -40, 0, 15)
CloseBtn.BackgroundColor3 = Theme.SectionBG
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Theme.Text
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 14
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", CloseBtn).Color = Theme.Stroke

CloseBtn.MouseButton1Click:Connect(function()
    pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end)
    task.delay(0.3, function() MainFrame.Visible = false end)
end)

-- Window Dragging
local mDragging, mDragStart, mStartPos = false, nil, nil
Topbar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then 
        mDragging = true
        mDragStart = input.Position
        mStartPos = MainFrame.Position 
    end
end)
Topbar.InputChanged:Connect(function(input)
    if mDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - mDragStart
        MainFrame.Position = UDim2.new(mStartPos.X.Scale, mStartPos.X.Offset + delta.X, mStartPos.Y.Scale, mStartPos.Y.Offset + delta.Y)
    end
end)
Topbar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then mDragging = false end
end)

local Line = Instance.new("Frame", ContentWrapper)
Line.Size = UDim2.new(1, -40, 0, 1)
Line.Position = UDim2.new(0, 20, 0, 65)
Line.BackgroundColor3 = Theme.Stroke
Line.BorderSizePixel = 0

-- Sidebar Layout
local Sidebar = Instance.new("ScrollingFrame", ContentWrapper)
Sidebar.Size = UDim2.new(0, SidebarWidth, 1, -85)
Sidebar.Position = UDim2.new(0, 10, 0, 75)
Sidebar.BackgroundTransparency = 1
Sidebar.ScrollBarThickness = 0

local SideLayout = Instance.new("UIListLayout", Sidebar)
SideLayout.Padding = UDim.new(0, 6)
SideLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left
SideLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Content Container
local ContentContainer = Instance.new("Frame", ContentWrapper)
ContentContainer.Size = UDim2.new(1, -(SidebarWidth + 25), 1, -85)
ContentContainer.Position = UDim2.new(0, SidebarWidth + 15, 0, 75)
ContentContainer.BackgroundTransparency = 1

--// ==========================================
--// UI BUILDER HELPER FUNCTIONS
--// ==========================================

local itemOrderCounter = 0
local Tabs = {}
local refreshPlayerDropdown -- Forward declaration für den Button
local LockPlayerDropdown -- Forward declaration für das Dropdown

local function UpdateSidebarCanvas()
    local totalH = 10
    for _, t in pairs(Tabs) do
        totalH = totalH + 36 + 6
        if t.IsOpen then totalH = totalH + t.SubLayout.AbsoluteContentSize.Y + 6 end
    end
    Sidebar.CanvasSize = UDim2.new(0, 0, 0, totalH)
end

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
    subLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left
    subLayout.SortOrder = Enum.SortOrder.LayoutOrder
    tabObj.SubLayout = subLayout

    tabBtn.MouseButton1Click:Connect(function()
        tabObj.IsOpen = not tabObj.IsOpen
        local targetSize = tabObj.IsOpen and UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) or UDim2.new(1, 0, 0, 0)
        pcall(function()
            TweenService:Create(subContainer, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = targetSize}):Play()
            tabBtn.TextColor3 = tabObj.IsOpen and Theme.Text or Theme.SubText
            tabBtn.BackgroundColor3 = tabObj.IsOpen and Theme.SectionBG or Theme.Sidebar
        end)
        task.delay(0.26, UpdateSidebarCanvas)
    end)

    subLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        if tabObj.IsOpen then subContainer.Size = UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) end
    end)

    table.insert(Tabs, tabObj)
    return tabObj
end

local function CreateSubTab(tabObj, subName)
    local subObj = { Btn = nil, Page = nil }
    local subBtn = Instance.new("TextButton", tabObj.SubContainer)
    subBtn.Size = UDim2.new(1, 0, 0, 28)
    subBtn.BackgroundTransparency = 1
    subBtn.Text = "     " .. subName
    subBtn.TextColor3 = Theme.SubText
    subBtn.Font = Enum.Font.GothamMedium
    subBtn.TextSize = 12
    subBtn.TextXAlignment = Enum.TextXAlignment.Left
    subObj.Btn = subBtn

    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = 2
    page.ScrollBarImageColor3 = Theme.Accent
    page.Visible = false
    subObj.Page = page

    local pageLayout = Instance.new("UIListLayout", page)
    pageLayout.Padding = UDim.new(0, 12)
    pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    pageLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        page.CanvasSize = UDim2.new(0, 0, 0, pageLayout.AbsoluteContentSize.Y + 20)
    end)

    subBtn.MouseButton1Click:Connect(function()
        for _, t in pairs(Tabs) do
            for _, st in pairs(t.SubTabs) do
                st.Page.Visible = false
                st.Btn.TextColor3 = Theme.SubText
            end
        end
        page.Visible = true
        subBtn.TextColor3 = Theme.Text
    end)

    table.insert(tabObj.SubTabs, subObj)
    return page
end

local function CreateSection(page, titleText)
    local section = Instance.new("Frame", page)
    section.Size = UDim2.new(0.98, 0, 0, 50)
    section.BackgroundColor3 = Theme.SectionBG
    section.BackgroundTransparency = 0
    Instance.new("UICorner", section).CornerRadius = UDim.new(0, 10)
    Instance.new("UIStroke", section).Color = Theme.Stroke
    
    local secLayout = Instance.new("UIListLayout", section)
    secLayout.Padding = UDim.new(0, 10)
    secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    secLayout.SortOrder = Enum.SortOrder.LayoutOrder
    
    local secPadding = Instance.new("UIPadding", section)
    secPadding.PaddingTop = UDim.new(0, 12)
    secPadding.PaddingBottom = UDim.new(0, 12)
    
    local title = Instance.new("TextLabel", section)
    title.LayoutOrder = -1
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

local function CreateLabel(section, text)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter
    frame.Size = UDim2.new(0.92, 0, 0, 90)
    frame.BackgroundTransparency = 1
    
    local lbl = Instance.new("TextLabel", frame)
    lbl.Size = UDim2.new(1, 0, 1, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = text
    lbl.TextColor3 = Theme.SubText
    lbl.Font = Enum.Font.GothamMedium
    lbl.TextSize = 11
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.TextWrapped = true
    return lbl
end

local function CreateToggle(section, text, descText, defaultState, callback)
    if type(descText) == "boolean" then callback = defaultState; defaultState = descText; descText = nil end
    itemOrderCounter = itemOrderCounter + 1
    
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter
    frame.Size = UDim2.new(0.92, 0, 0, descText and 52 or 34)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.7, 0, 0, 34)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = defaultState and Theme.Text or Theme.SubText
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    if descText then
        local descLabel = Instance.new("TextLabel", frame)
        descLabel.Size = UDim2.new(0.7, 0, 0, 15)
        descLabel.Position = UDim2.new(0, 0, 0, 30)
        descLabel.BackgroundTransparency = 1
        descLabel.Text = descText
        descLabel.TextColor3 = Theme.SubText
        descLabel.Font = Enum.Font.Gotham
        descLabel.TextSize = 11
        descLabel.TextXAlignment = Enum.TextXAlignment.Left
    end
    
    local tBtn = Instance.new("TextButton", frame)
    tBtn.Size = UDim2.new(0, 42, 0, 22)
    tBtn.Position = UDim2.new(1, -42, 0, 6)
    tBtn.BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff
    tBtn.Text = ""
    Instance.new("UICorner", tBtn).CornerRadius = UDim.new(1, 0)
    
    local circle = Instance.new("Frame", tBtn)
    circle.Size = UDim2.new(0, 16, 0, 16)
    circle.Position = defaultState and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8)
    circle.BackgroundColor3 = defaultState and Theme.Background or Color3.fromRGB(150, 150, 150)
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = defaultState or false
    tBtn.MouseButton1Click:Connect(function()
        isOn = not isOn
        pcall(function()
            TweenService:Create(tBtn, TweenInfo.new(0.2), {BackgroundColor3 = isOn and Theme.ToggleOn or Theme.ToggleOff}):Play()
            TweenService:Create(circle, TweenInfo.new(0.2), {Position = isOn and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = isOn and Theme.Background or Color3.fromRGB(150, 150, 150)}):Play()
            label.TextColor3 = isOn and Theme.Text or Theme.SubText
        end)
        if callback then pcall(function() callback(isOn) end) end
    end)
end

local function CreateSlider(section, text, min, max, default, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter
    frame.Size = UDim2.new(0.92, 0, 0, 50)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(1, -40, 0, 18)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Theme.SubText
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local valLabel = Instance.new("TextLabel", frame)
    valLabel.Size = UDim2.new(0, 40, 0, 18)
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
    local percentage = math.clamp((default - min) / (max - min), 0, 1)
    sliderFill.Size = UDim2.new(percentage, 0, 1, 0)
    sliderFill.BackgroundColor3 = Theme.Accent
    Instance.new("UICorner", sliderFill).CornerRadius = UDim.new(1, 0)
    
    local knob = Instance.new("TextButton", sliderFill)
    knob.Size = UDim2.new(0, 14, 0, 14)
    knob.Position = UDim2.new(1, -7, 0.5, -7)
    knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    knob.Text = ""
    Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
    
    local dragging = false
    knob.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relative = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            local value = math.floor(min + (max - min) * relative)
            valLabel.Text = tostring(value)
            sliderFill.Size = UDim2.new(relative, 0, 1, 0)
            if callback then pcall(function() callback(value) end) end
        end
    end)
end

local function CreateButton(section, text, color, callback)
    if type(color) == "function" then callback = color; color = Theme.SectionBG end
    itemOrderCounter = itemOrderCounter + 1
    
    local btn = Instance.new("TextButton", section)
    btn.LayoutOrder = itemOrderCounter
    btn.Size = UDim2.new(0.92, 0, 0, 34)
    btn.BackgroundColor3 = color
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", btn).Color = Theme.Stroke
    AddClickPop(btn)
    
    btn.MouseButton1Click:Connect(function() 
        if callback then pcall(callback) end 
    end)
    return btn
end

local function CreateDropdown(section, headerText, itemsList, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter
    frame.Size = UDim2.new(0.92, 0, 0, 160)
    frame.BackgroundTransparency = 1
    
    local header = Instance.new("TextLabel", frame)
    header.Size = UDim2.new(1, 0, 0, 20)
    header.BackgroundTransparency = 1
    header.Text = headerText .. ": None"
    header.TextColor3 = Theme.SubText
    header.Font = Enum.Font.GothamMedium
    header.TextSize = 12
    header.TextXAlignment = Enum.TextXAlignment.Left
    
    local scroll = Instance.new("ScrollingFrame", frame)
    scroll.Size = UDim2.new(1, 0, 0, 130)
    scroll.Position = UDim2.new(0, 0, 0, 25)
    scroll.BackgroundColor3 = Theme.Background
    scroll.ScrollBarThickness = 4
    Instance.new("UICorner", scroll).CornerRadius = UDim.new(0, 6)
    
    local listLayout = Instance.new("UIListLayout", scroll)
    listLayout.Padding = UDim.new(0, 4)
    listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    local function populate(list)
        for _, child in pairs(scroll:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        for _, itemName in ipairs(list) do
            local btn = Instance.new("TextButton", scroll)
            btn.Size = UDim2.new(0.94, 0, 0, 26)
            btn.BackgroundColor3 = Theme.SectionBG
            btn.Text = "  " .. tostring(itemName)
            btn.TextColor3 = Theme.Text
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 12
            btn.TextXAlignment = Enum.TextXAlignment.Left
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
            
            btn.MouseButton1Click:Connect(function() 
                header.Text = headerText .. ": " .. tostring(itemName)
                if callback then callback(itemName) end
            end)
        end
        task.defer(function() scroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 10) end)
    end
    
    populate(itemsList)
    return { Refresh = populate }
end

local function CreateKeybind(section, text, defaultKey, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter
    frame.Size = UDim2.new(0.92, 0, 0, 34)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.7, 0, 0, 34)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Theme.Text
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 80, 0, 22)
    btn.Position = UDim2.new(1, -80, 0, 6)
    btn.BackgroundColor3 = Theme.ToggleOff
    btn.Text = defaultKey.Name
    btn.TextColor3 = Theme.Text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    
    local waiting = false
    btn.MouseButton1Click:Connect(function() waiting = true; btn.Text = "..." end)
    UserInputService.InputBegan:Connect(function(input)
        if waiting and input.UserInputType == Enum.UserInputType.Keyboard then
            waiting = false
            btn.Text = input.KeyCode.Name
            if callback then pcall(function() callback(input.KeyCode) end) end
        end
    end)
end

local function CreateInput(section, placeholder, callback)
    itemOrderCounter = itemOrderCounter + 1
    local box = Instance.new("TextBox", section)
    box.LayoutOrder = itemOrderCounter
    box.Size = UDim2.new(0.92, 0, 0, 34)
    box.BackgroundColor3 = Theme.Background
    box.Text = ""
    box.PlaceholderText = placeholder
    box.TextColor3 = Theme.Text
    box.Font = Enum.Font.GothamMedium
    box.TextSize = 12
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", box).Color = Theme.Stroke
    
    if callback then 
        box.FocusLost:Connect(function() pcall(function() callback(box.Text) end) end) 
    end
    return box
end

--// ==========================================
--// POPULATE UI
--// ==========================================

-- TAB: COMBAT
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
CreateToggle(SecDashAssist, "Enable Side Dash Assist", RyuConfig.EnableDashAssist, function(v) RyuConfig.EnableDashAssist = v end)
CreateToggle(SecDashAssist, "Lock Camera On Enemy", RyuConfig.DashCameraLock, function(v) RyuConfig.DashCameraLock = v end)
CreateToggle(SecDashAssist, "Dash Only If Facing Front", RyuConfig.DashOnlyIfFacing, function(v) RyuConfig.DashOnlyIfFacing = v end)
CreateKeybind(SecDashAssist, "Dash Keybind", RyuConfig.DashAssistKeybind, function(v) RyuConfig.DashAssistKeybind = v end)
CreateSlider(SecDashAssist, "Detection Range", 1, 50, RyuConfig.DashDetectionRange, function(v) RyuConfig.DashDetectionRange = v end)

local SecDashArc = CreateSection(SubSideDash, "Arc Modifiers")
CreateSlider(SecDashArc, "Behind Distance", 1, 15, RyuConfig.DashBehindDistance, function(v) RyuConfig.DashBehindDistance = v end)
CreateSlider(SecDashArc, "Curve Strength", 0, 25, RyuConfig.DashCurveStrength, function(v) RyuConfig.DashCurveStrength = v end)
CreateSlider(SecDashArc, "Arch Height", 0, 10, RyuConfig.DashArchHeight, function(v) RyuConfig.DashArchHeight = v end)


-- TAB: VISUALS
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


-- TAB: AIMBOT
local TabAimbot = CreateMainTab("Aimbot")

local SubTargetLock = CreateSubTab(TabAimbot, "Target Lock")
local SecLockMain = CreateSection(SubTargetLock, "Main Lock")
CreateToggle(SecLockMain, "Enable Lock", Logic.LockState.Enabled, function(v) Logic.LockState.Enabled = v end)
CreateKeybind(SecLockMain, "Lock Keybind", RyuConfig.LockKeybind, function(v) RyuConfig.LockKeybind = v end)
CreateToggle(SecLockMain, "Lock ESP", Logic.LockSettings.ESPEnabled, function(v) Logic.LockSettings.ESPEnabled = v end)
CreateButton(SecLockMain, "Refresh Player List", Theme.SectionBG, function() if refreshPlayerDropdown then refreshPlayerDropdown() end end)
LockPlayerDropdown = CreateDropdown(SecLockMain, "Target Player", {"Auto"}, function(v) RyuConfig.LockSpecificPlayer = v end)

local SecLockCfg = CreateSection(SubTargetLock, "Lock Config")
CreateToggle(SecLockCfg, "Wall Check", Logic.LockSettings.WallCheck, function(v) Logic.LockSettings.WallCheck = v end)
CreateToggle(SecLockCfg, "Auto Retarget", Logic.LockSettings.AutoRetarget, function(v) Logic.LockSettings.AutoRetarget = v end)
CreateSlider(SecLockCfg, "Max Lock Distance", 10, 2000, Logic.LockSettings.MaxDistance, function(v) Logic.LockSettings.MaxDistance = v end)
CreateSlider(SecLockCfg, "Prediction", 0, 1, Logic.LockSettings.Prediction, function(v) Logic.LockSettings.Prediction = v end)


-- TAB: WORLD
local TabWorld = CreateMainTab("World")

local SubTeleport = CreateSubTab(TabWorld, "Teleports")
local SecTeleport = CreateSection(SubTeleport, "Auto Teleport Settings")
local TeleportLocations = { ["Under the Map"] = Vector3.new(-20.23, -61.53, -146.34), ["Unlicensed Studios"] = Vector3.new(196.86, 23.58, -37.27), ["Towers"] = Vector3.new(25.35, 183.08, 110.77), ["Train Button"] = Vector3.new(182.21, -9.33, 562.54), ["Bowling"] = Vector3.new(267.60, -59.89, -255.06), ["Restaurant"] = Vector3.new(-43.24, 23.63, -83.07), ["Storage House"] = Vector3.new(195.69, 23.58, 151.44), ["Sewers 1"] = Vector3.new(-148.14, -31.48, -127.22), ["Train Station"] = Vector3.new(185.27, -9.69, -97.17), ["Sewers 2"] = Vector3.new(60.84, -10.58, 167.47), ["Shenanigans Mall"] = Vector3.new(155.66, -26.38, -254.85), ["Rhythm Game"] = Vector3.new(12.23, -30.21, -315.03), ["Piano"] = Vector3.new(-86.38, 26.65, -252.48), ["Convenience Store"] = Vector3.new(-247.51, 26.96, -116.64), ["Court"] = Vector3.new(124.48, 23.78, -247.06), ["Graveyard"] = Vector3.new(228.55, 23.68, -130.48), ["Train Station Exit"] = Vector3.new(1.52, 24.72, 396.06), ["Tze's"] = Vector3.new(-55.30, 23.62, 245.42), ["Jail"] = Vector3.new(-243.84, 23.58, 126.97) }
local LocationNames = {}
for k, _ in pairs(TeleportLocations) do table.insert(LocationNames, k) end
table.sort(LocationNames)

local currentTPDest = LocationNames[1]
CreateDropdown(SecTeleport, "Destination", LocationNames, function(v) currentTPDest = v end)
CreateSlider(SecTeleport, "Teleport Speed (Studs/s)", 30, 1500, Logic.Pathfinding.Speed, function(v) Logic.Pathfinding.Speed = v end)
CreateToggle(SecTeleport, "Show Path Visualization", Logic.Pathfinding.VisualizeOn, function(v) Logic.Pathfinding.VisualizeOn = v end)
CreateButton(SecTeleport, "Start Teleport", Theme.SectionBG, function() local destPos = TeleportLocations[currentTPDest] if destPos then startPathfinding(destPos) end end)
CreateButton(SecTeleport, "Stop Teleport", Theme.SectionBG, function() stopPathfinding() end)

local SubServers = CreateSubTab(TabWorld, "Servers")
local SecServerList = CreateSection(SubServers, "Server List")
SrvDropController = CreateDropdown(SecServerList, "Select Server", {"Click 'Refresh' to load servers"}, function(v) Logic.SelectedServerData = Logic.ServerMap[v]; updateServerInfo(Logic.SelectedServerData) end)
CreateButton(SecServerList, "Refresh Server List", Theme.SectionBG, function() task.spawn(function() fetchServers(); refreshDropdown() end) end)

local SecServerInfo = CreateSection(SubServers, "Server Details")
UI_SrvInfoLabel = CreateLabel(SecServerInfo, "No server selected.\nClick 'Refresh Server List' to load servers.")
UI_SrvStatsLabel = CreateLabel(SecServerInfo, "No data. Refresh to load servers.")
CreateButton(SecServerInfo, "Join Selected Server", Theme.SectionBG, function() if Logic.SelectedServerData and not Logic.SelectedServerData.IsCurrent then TeleportService:TeleportToPlaceInstance(game.PlaceId, Logic.SelectedServerData.JobId, Player) end end)
CreateButton(SecServerInfo, "Join Lowest Ping Server", Theme.SectionBG, function()
    local best = nil
    for _, srv in ipairs(Logic.ServerList) do if not srv.IsCurrent and srv.Playing > 0 and srv.Ping > 0 then if not best or srv.Ping < best.Ping then best = srv end end end
    if best then TeleportService:TeleportToPlaceInstance(game.PlaceId, best.JobId, Player) end
end)


--// INIT
pcall(function()
    if Tabs[1] and Tabs[1].Btn then
        Tabs[1].IsOpen = true
        Tabs[1].SubContainer.Size = UDim2.new(1, 0, 0, Tabs[1].SubLayout.AbsoluteContentSize.Y)
        Tabs[1].Btn.TextColor3 = Theme.Text
        Tabs[1].Btn.BackgroundColor3 = Theme.SectionBG
    end
    if Tabs[1] and Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Page then
        Tabs[1].SubTabs[1].Page.Visible = true
        Tabs[1].SubTabs[1].Btn.TextColor3 = Theme.Text
    end
    UpdateSidebarCanvas()
end)

refreshPlayerDropdown = function()
    local vals = {"Auto"}
    local charsFolder = workspace:FindFirstChild("Characters")
    
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= Player then
            table.insert(vals, p.Name)
        end
    end
    
    if charsFolder then
        for _, model in ipairs(charsFolder:GetChildren()) do
            if model:IsA("Model") and not Players:GetPlayerFromCharacter(model) then
                local hum = model:FindFirstChildOfClass("Humanoid")
                if hum and hum.Health > 0 then
                    local npcName = "[NPC] " .. model.Name
                    if not table.find(vals, npcName) then
                        table.insert(vals, npcName)
                    end
                end
            end
        end
    end
    
    pcall(function()
        if LockPlayerDropdown then
            LockPlayerDropdown.Refresh(vals)
        end
        if not table.find(vals, RyuConfig.LockSpecificPlayer) then
            RyuConfig.LockSpecificPlayer = "Auto"
        end
    end)
end

task.spawn(function()
    task.wait(1)
    refreshPlayerDropdown()
end)

task.spawn(function()
    task.wait(2)
    fetchServers()
    refreshDropdown()
end)

--// ==========================================
--// BACKGROUND LOOPS & LOGIC HOOKS
--// ==========================================

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

# Siounsniojsipjns
-- ryu hub - jujutsu shenanigans full edition
-- dev build / human style

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

-- gui setup
local guiParent = LocalPlayer:WaitForChild("PlayerGui", 10) or LocalPlayer:FindFirstChild("PlayerGui")
pcall(function() 
    if gethui then 
        guiParent = gethui() 
    elseif syn and syn.protect_gui then 
        guiParent = CoreGui 
    end 
end)

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "RyuHubTJS" or v.Name == "RyuNotificationsTJS" then 
        v:Destroy() 
    end 
end

-- knit remotes
local KnitServices = ReplicatedStorage:WaitForChild("Knit", 10):WaitForChild("Knit", 10):WaitForChild("Services", 10)
local BlockService = KnitServices:WaitForChild("BlockService", 10):WaitForChild("RE", 10)
local BlockDeactivated = BlockService:WaitForChild("Deactivated", 10)
local BlockActivated = BlockService:WaitForChild("Activated", 10)

local ItadoriService = KnitServices:FindFirstChild("ItadoriService") and KnitServices.ItadoriService:FindFirstChild("RE")
local ItadoriChase = ItadoriService and ItadoriService:FindFirstChild("Chase")

local DivergentService = KnitServices:FindFirstChild("DivergentFistService") and KnitServices.DivergentFistService:FindFirstChild("RE")
local DivergentActivated = DivergentService and DivergentService:FindFirstChild("Activated")

-- config
local RyuConfig = {
    -- Player
    Speed = 16,
    SpeedToggle = false,
    Fly = false,
    FlySpeed = 50,
    JumpPower = 50,
    JumpSpam = false,
    Noclip = false,
    Invisible = false,
    
    -- Auto Combat
    AutoBlock = false,
    BlockTimer = 0.5,
    BlockRange = 15,
    AutoBlackFlash = false,
    AutoDodge = false,
    DodgeDistance = 25,
    
    -- Ability
    LockOn = false,
    LockOnMode = "Nearest",
    Knockback = false,
    KnockbackPower = 100,
    InfiniteAwakening = false,
    InstantReversal = false,
    
    -- AI Farm
    AiFarm = false,
    AttackRange = 10,
    AutoUlt = false,
    UseSkill1 = true, Skill1Range = 15,
    UseSkill2 = true, Skill2Range = 15,
    UseSkill3 = true, Skill3Range = 15,
    UseSkill4 = true, Skill4Range = 15,
    HumanMode = false,
    
    -- Target
    SelectedTarget = "",
    StickToTarget = false,
    StickDistance = 4,
    
    -- Money Farm
    MoneyFarm = false,
    Role = "Farmer",
    VictimTarget = "",
    VictimMode = "Walk",
    
    -- Config
    AutoRejoin = false,
    RejoinThreshold = 4,
    LobbyType = "High",
    MainAccountName = "",
    
    GuiColor = Color3.fromRGB(255, 255, 255)
}

local SETTINGS_FILE = "RyuHub_TJS_Settings.json"

local function SaveSettings()
    if writefile then
        local enc = HttpService:JSONEncode(RyuConfig)
        writefile(SETTINGS_FILE, enc)
    end
end

local function LoadSettings()
    if isfile and isfile(SETTINGS_FILE) and readfile then
        local raw = readfile(SETTINGS_FILE)
        local dec = HttpService:JSONDecode(raw)
        for k, v in pairs(dec) do
            RyuConfig[k] = v
        end
    end
end
pcall(LoadSettings)

local Theme = {
    Background = Color3.fromRGB(12, 12, 14),
    Sidebar = Color3.fromRGB(18, 18, 20),
    SectionBG = Color3.fromRGB(24, 24, 26),
    Text = Color3.fromRGB(250, 250, 250),
    SubText = Color3.fromRGB(130, 130, 135),
    Accent = RyuConfig.GuiColor,
    ToggleOff = Color3.fromRGB(35, 35, 38),
    ToggleOn = RyuConfig.GuiColor,
    Stroke = Color3.fromRGB(45, 45, 50),
    Warning = Color3.fromRGB(255, 75, 75)
}

local MainSize = UDim2.new(0, math.min(760, camera.ViewportSize.X - 30), 0, math.min(490, camera.ViewportSize.Y - 30))
local SidebarWidth = 155

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubTJS"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

-- notifications
local NotificationContainer = Instance.new("Frame")
NotificationContainer.Name = "RyuNotificationsTJS"
NotificationContainer.Size = UDim2.new(0, 260, 1, -40)
NotificationContainer.Position = UDim2.new(1, -280, 0, 20)
NotificationContainer.BackgroundTransparency = 1
NotificationContainer.Parent = guiParent

local NotifLayout = Instance.new("UIListLayout", NotificationContainer)
NotifLayout.SortOrder = Enum.SortOrder.LayoutOrder
NotifLayout.VerticalAlignment = Enum.VerticalAlignment.Bottom
NotifLayout.Padding = UDim.new(0, 8)

local RyuNotify = {}
function RyuNotify:Send(title, text, duration)
    duration = duration or 3
    local NotifFrame = Instance.new("Frame", NotificationContainer)
    NotifFrame.Size = UDim2.new(1, 0, 0, 60)
    NotifFrame.BackgroundColor3 = Theme.Sidebar
    NotifFrame.BackgroundTransparency = 1
    Instance.new("UICorner", NotifFrame).CornerRadius = UDim.new(0, 8)
    
    local Stroke = Instance.new("UIStroke", NotifFrame)
    Stroke.Color = Theme.Accent
    Stroke.Transparency = 1
    Stroke.Thickness = 1.5
    
    local AccentLine = Instance.new("Frame", NotifFrame)
    AccentLine.Size = UDim2.new(0, 3, 0.8, 0)
    AccentLine.Position = UDim2.new(0, 4, 0.1, 0)
    AccentLine.BackgroundColor3 = Theme.Accent
    AccentLine.BackgroundTransparency = 1
    Instance.new("UICorner", AccentLine).CornerRadius = UDim.new(1, 0)
    
    local TitleLabel = Instance.new("TextLabel", NotifFrame)
    TitleLabel.Size = UDim2.new(1, -20, 0, 20)
    TitleLabel.Position = UDim2.new(0, 15, 0, 8)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = title
    TitleLabel.TextColor3 = Theme.Text
    TitleLabel.TextTransparency = 1
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextSize = 13
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    
    local DescLabel = Instance.new("TextLabel", NotifFrame)
    DescLabel.Size = UDim2.new(1, -20, 0, 20)
    DescLabel.Position = UDim2.new(0, 15, 0, 28)
    DescLabel.BackgroundTransparency = 1
    DescLabel.Text = text
    DescLabel.TextColor3 = Theme.SubText
    DescLabel.TextTransparency = 1
    DescLabel.Font = Enum.Font.Gotham
    DescLabel.TextSize = 11
    DescLabel.TextXAlignment = Enum.TextXAlignment.Left

    TweenService:Create(NotifFrame, TweenInfo.new(0.3), {BackgroundTransparency = 0.1}):Play()
    TweenService:Create(Stroke, TweenInfo.new(0.3), {Transparency = 0.5}):Play()
    TweenService:Create(AccentLine, TweenInfo.new(0.3), {BackgroundTransparency = 0}):Play()
    TweenService:Create(TitleLabel, TweenInfo.new(0.3), {TextTransparency = 0}):Play()
    TweenService:Create(DescLabel, TweenInfo.new(0.3), {TextTransparency = 0}):Play()

    task.spawn(function()
        task.wait(duration)
        local fadeOut = TweenService:Create(NotifFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundTransparency = 1, Size = UDim2.new(1, 0, 0, 0)})
        TweenService:Create(Stroke, TweenInfo.new(0.3), {Transparency = 1}):Play()
        TweenService:Create(AccentLine, TweenInfo.new(0.3), {BackgroundTransparency = 1}):Play()
        TweenService:Create(TitleLabel, TweenInfo.new(0.3), {TextTransparency = 1}):Play()
        TweenService:Create(DescLabel, TweenInfo.new(0.3), {TextTransparency = 1}):Play()
        fadeOut:Play()
        fadeOut.Completed:Wait()
        NotifFrame:Destroy()
    end)
end

-- toggle button
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 48, 0, 48)
ToggleBtn.Position = UDim2.new(0, 20, 0, 20)
ToggleBtn.BackgroundColor3 = Theme.Sidebar
ToggleBtn.Text = "R"
ToggleBtn.Font = Enum.Font.GothamBlack
ToggleBtn.TextColor3 = Theme.Accent
ToggleBtn.TextSize = 20
ToggleBtn.Parent = RyuHub
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
local tStroke = Instance.new("UIStroke", ToggleBtn)
tStroke.Color = Theme.Accent
tStroke.Thickness = 2

local tDragStart, tStartPos, isDraggingBtn = nil, nil, false
ToggleBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDraggingBtn = false
        tDragStart = input.Position
        tStartPos = ToggleBtn.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if tDragStart and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - tDragStart
        if delta.Magnitude > 5 then
            isDraggingBtn = true
            ToggleBtn.Position = UDim2.new(tStartPos.X.Scale, tStartPos.X.Offset + delta.X, tStartPos.Y.Scale, tStartPos.Y.Offset + delta.Y)
        end
    end
end)

-- main window
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 0, 0, 0)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.BackgroundColor3 = Theme.Background
MainFrame.Active = true
MainFrame.Visible = false
MainFrame.ClipsDescendants = true
MainFrame.Parent = RyuHub
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

local mStroke = Instance.new("UIStroke", MainFrame)
mStroke.Color = Theme.Stroke
mStroke.Thickness = 1.5

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        if tDragStart then
            if not isDraggingBtn then
                if MainFrame.Visible then
                    TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play()
                    task.wait(0.25)
                    MainFrame.Visible = false
                else
                    MainFrame.Visible = true
                    TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = MainSize, Position = UDim2.new(0.5, -MainSize.X.Offset / 2, 0.5, -MainSize.Y.Offset / 2)}):Play()
                end
            end
            tDragStart = nil
        end
    end
end)

local Topbar = Instance.new("Frame", MainFrame)
Topbar.Size = UDim2.new(1, 0, 0, 55)
Topbar.BackgroundTransparency = 1

local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 250, 0, 24)
Title.Position = UDim2.new(0, 18, 0, 10)
Title.BackgroundTransparency = 1
Title.Text = "RYU HUB"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 20
Title.TextColor3 = Theme.Text
Title.TextXAlignment = Enum.TextXAlignment.Left

local SubTitle = Instance.new("TextLabel", Topbar)
SubTitle.Size = UDim2.new(0, 250, 0, 15)
SubTitle.Position = UDim2.new(0, 18, 0, 32)
SubTitle.BackgroundTransparency = 1
SubTitle.Text = "Jujutsu Shenanigans"
SubTitle.TextColor3 = Theme.SubText
SubTitle.Font = Enum.Font.Gotham
SubTitle.TextSize = 11
SubTitle.TextXAlignment = Enum.TextXAlignment.Left

local CloseBtn = Instance.new("TextButton", Topbar)
CloseBtn.Size = UDim2.new(0, 26, 0, 26)
CloseBtn.Position = UDim2.new(1, -36, 0, 14)
CloseBtn.BackgroundColor3 = Theme.SectionBG
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Theme.SubText
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 13
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)

CloseBtn.MouseButton1Click:Connect(function()
    TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play()
    task.wait(0.25)
    MainFrame.Visible = false
end)

local mDragging, mDragStart, mStartPos
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
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        mDragging = false
    end
end)

local Sidebar = Instance.new("ScrollingFrame", MainFrame)
Sidebar.Size = UDim2.new(0, SidebarWidth, 1, -75)
Sidebar.Position = UDim2.new(0, 10, 0, 65)
Sidebar.BackgroundTransparency = 1
Sidebar.ScrollBarThickness = 0

local SideLayout = Instance.new("UIListLayout", Sidebar)
SideLayout.Padding = UDim.new(0, 6)
SideLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left
SideLayout.SortOrder = Enum.SortOrder.LayoutOrder

local ContentContainer = Instance.new("Frame", MainFrame)
ContentContainer.Size = UDim2.new(1, -(SidebarWidth + 25), 1, -75)
ContentContainer.Position = UDim2.new(0, SidebarWidth + 15, 0, 65)
ContentContainer.BackgroundTransparency = 1

-- ui generators
local Tabs = {}
local function UpdateSidebarCanvas()
    local totalH = 10
    for _, t in pairs(Tabs) do
        totalH = totalH + 34 + 6
        if t.IsOpen then
            totalH = totalH + t.SubLayout.AbsoluteContentSize.Y + 6
        end
    end
    Sidebar.CanvasSize = UDim2.new(0, 0, 0, totalH)
end

local function SecureTrigger(button, func)
    button.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            func()
        end
    end)
end

local function CreateMainTab(name)
    local tabObj = { Btn = nil, SubContainer = nil, SubLayout = nil, IsOpen = false, SubTabs = {}, ToggleFunc = nil }

    local tabBtn = Instance.new("TextButton", Sidebar)
    tabBtn.Size = UDim2.new(1, 0, 0, 34)
    tabBtn.BackgroundColor3 = Theme.Sidebar
    tabBtn.Text = "  " .. string.upper(name)
    tabBtn.TextColor3 = Theme.SubText
    tabBtn.Font = Enum.Font.GothamBlack
    tabBtn.TextSize = 12
    tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 6)
    tabObj.Btn = tabBtn

    local subContainer = Instance.new("Frame", Sidebar)
    subContainer.Size = UDim2.new(1, 0, 0, 0)
    subContainer.BackgroundTransparency = 1
    subContainer.ClipsDescendants = true
    tabObj.SubContainer = subContainer

    local subLayout = Instance.new("UIListLayout", subContainer)
    subLayout.Padding = UDim.new(0, 2)
    tabObj.SubLayout = subLayout

    tabObj.ToggleFunc = function()
        tabObj.IsOpen = not tabObj.IsOpen
        local targetSize = tabObj.IsOpen and UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) or UDim2.new(1, 0, 0, 0)
        TweenService:Create(subContainer, TweenInfo.new(0.2), {Size = targetSize}):Play()
        tabBtn.TextColor3 = tabObj.IsOpen and Theme.Text or Theme.SubText
        task.delay(0.21, UpdateSidebarCanvas)
    end

    SecureTrigger(tabBtn, tabObj.ToggleFunc)
    subLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        if tabObj.IsOpen then subContainer.Size = UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) end
        UpdateSidebarCanvas()
    end)

    table.insert(Tabs, tabObj)
    return tabObj
end

local function CreateSubTab(tabObj, subName)
    local subObj = { Btn = nil, Page = nil, SelectFunc = nil }

    local subBtn = Instance.new("TextButton", tabObj.SubContainer)
    subBtn.Size = UDim2.new(1, 0, 0, 26)
    subBtn.BackgroundTransparency = 1
    subBtn.Text = "     " .. subName
    subBtn.TextColor3 = Theme.SubText
    subBtn.Font = Enum.Font.GothamMedium
    subBtn.TextSize = 11
    subBtn.TextXAlignment = Enum.TextXAlignment.Left
    subObj.Btn = subBtn

    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = 2
    page.Visible = false
    subObj.Page = page

    local pageLayout = Instance.new("UIListLayout", page)
    pageLayout.Padding = UDim.new(0, 10)
    pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    pageLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        page.CanvasSize = UDim2.new(0, 0, 0, pageLayout.AbsoluteContentSize.Y + 15)
    end)

    subObj.SelectFunc = function()
        for _, t in pairs(Tabs) do
            for _, st in pairs(t.SubTabs) do
                st.Page.Visible = false
                st.Btn.TextColor3 = Theme.SubText
            end
        end
        page.Visible = true
        subBtn.TextColor3 = Theme.Text
    end

    SecureTrigger(subBtn, subObj.SelectFunc)
    table.insert(tabObj.SubTabs, subObj)
    return page
end

local function CreateSection(page, titleText)
    local section = Instance.new("Frame", page)
    section.Size = UDim2.new(0.98, 0, 0, 45)
    section.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", section).CornerRadius = UDim.new(0, 8)
    
    local secLayout = Instance.new("UIListLayout", section)
    secLayout.Padding = UDim.new(0, 8)
    secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    local secPadding = Instance.new("UIPadding", section)
    secPadding.PaddingTop = UDim.new(0, 10)
    secPadding.PaddingBottom = UDim.new(0, 10)
    
    local title = Instance.new("TextLabel", section)
    title.Size = UDim2.new(0.92, 0, 0, 20)
    title.BackgroundTransparency = 1
    title.Text = titleText
    title.TextColor3 = Theme.Text
    title.Font = Enum.Font.GothamBold
    title.TextSize = 13
    title.TextXAlignment = Enum.TextXAlignment.Left
    
    secLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        section.Size = UDim2.new(0.98, 0, 0, secLayout.AbsoluteContentSize.Y + 20)
    end)
    return section
end

local function CreateToggle(section, text, descText, defaultState, callback)
    if type(descText) == "boolean" then
        callback = defaultState
        defaultState = descText
        descText = nil
    end

    local frame = Instance.new("Frame", section)
    frame.Size = UDim2.new(0.92, 0, 0, descText and 44 or 30)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.75, 0, 0, 20)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = defaultState and Theme.Text or Theme.SubText
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    if descText then
        local desc = Instance.new("TextLabel", frame)
        desc.Size = UDim2.new(0.75, 0, 0, 15)
        desc.Position = UDim2.new(0, 0, 0, 20)
        desc.BackgroundTransparency = 1
        desc.Text = descText
        desc.TextColor3 = Theme.SubText
        desc.Font = Enum.Font.Gotham
        desc.TextSize = 10
        desc.TextXAlignment = Enum.TextXAlignment.Left
    end
    
    local tBtn = Instance.new("TextButton", frame)
    tBtn.Size = UDim2.new(0, 38, 0, 20)
    tBtn.Position = UDim2.new(1, -38, 0.5, -10)
    tBtn.BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff
    tBtn.Text = ""
    Instance.new("UICorner", tBtn).CornerRadius = UDim.new(1, 0)
    
    local circle = Instance.new("Frame", tBtn)
    circle.Size = UDim2.new(0, 14, 0, 14)
    circle.Position = defaultState and UDim2.new(1, -16, 0.5, -7) or UDim2.new(0, 2, 0.5, -7)
    circle.BackgroundColor3 = defaultState and Theme.Background or Color3.fromRGB(150, 150, 150)
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = defaultState or false
    SecureTrigger(tBtn, function()
        isOn = not isOn
        TweenService:Create(tBtn, TweenInfo.new(0.2), {BackgroundColor3 = isOn and Theme.ToggleOn or Theme.ToggleOff}):Play()
        TweenService:Create(circle, TweenInfo.new(0.2), {
            Position = isOn and UDim2.new(1, -16, 0.5, -7) or UDim2.new(0, 2, 0.5, -7),
            BackgroundColor3 = isOn and Theme.Background or Color3.fromRGB(150, 150, 150)
        }):Play()
        label.TextColor3 = isOn and Theme.Text or Theme.SubText
        if callback then callback(isOn) end
    end)
end

local function CreateSlider(section, text, min, max, default, callback)
    local frame = Instance.new("Frame", section)
    frame.Size = UDim2.new(0.92, 0, 0, 45)
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(1, -40, 0, 18)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Theme.SubText
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local valLabel = Instance.new("TextLabel", frame)
    valLabel.Size = UDim2.new(0, 40, 0, 18)
    valLabel.Position = UDim2.new(1, -40, 0, 0)
    valLabel.BackgroundTransparency = 1
    valLabel.Text = tostring(default)
    valLabel.TextColor3 = Theme.Accent
    valLabel.Font = Enum.Font.GothamBold
    valLabel.TextSize = 12
    valLabel.TextXAlignment = Enum.TextXAlignment.Right
    
    local sliderBg = Instance.new("Frame", frame)
    sliderBg.Size = UDim2.new(1, 0, 0, 4)
    sliderBg.Position = UDim2.new(0, 0, 0, 28)
    sliderBg.BackgroundColor3 = Theme.ToggleOff
    Instance.new("UICorner", sliderBg).CornerRadius = UDim.new(1, 0)
    
    local sliderFill = Instance.new("Frame", sliderBg)
    local percentage = (default - min) / (max - min)
    sliderFill.Size = UDim2.new(percentage, 0, 1, 0)
    sliderFill.BackgroundColor3 = Theme.Accent
    Instance.new("UICorner", sliderFill).CornerRadius = UDim.new(1, 0)
    
    local knob = Instance.new("TextButton", sliderFill)
    knob.Size = UDim2.new(0, 12, 0, 12)
    knob.Position = UDim2.new(1, -6, 0.5, -6)
    knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    knob.Text = ""
    Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
    
    local dragging = false
    local function setSlider(value)
        local rel = math.clamp((value - min) / (max - min), 0, 1)
        local val = math.floor((min + (max - min) * rel) * 10) / 10
        valLabel.Text = tostring(val)
        TweenService:Create(sliderFill, TweenInfo.new(0.05), {Size = UDim2.new(rel, 0, 1, 0)}):Play()
        if callback then callback(val) end
    end
    
    knob.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local rel = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            setSlider(min + (max - min) * rel)
        end
    end)
end

local function CreateButton(section, text, callback)
    local btn = Instance.new("TextButton", section)
    btn.Size = UDim2.new(0.92, 0, 0, 30)
    btn.BackgroundColor3 = Theme.SectionBG
    btn.Text = text
    btn.TextColor3 = Theme.Text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    local s = Instance.new("UIStroke", btn)
    s.Color = Theme.Stroke
    s.Thickness = 1
    SecureTrigger(btn, callback)
    return btn
end

local function CreateTextBox(section, placeholder, callback)
    local frame = Instance.new("Frame", section)
    frame.Size = UDim2.new(0.92, 0, 0, 30)
    frame.BackgroundTransparency = 1
    
    local box = Instance.new("TextBox", frame)
    box.Size = UDim2.new(1, 0, 1, 0)
    box.BackgroundColor3 = Theme.Background
    box.PlaceholderText = placeholder
    box.Text = ""
    box.TextColor3 = Theme.Text
    box.Font = Enum.Font.GothamMedium
    box.TextSize = 12
    box.ClearTextOnFocus = false
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    
    local stroke = Instance.new("UIStroke", box)
    stroke.Color = Theme.Stroke
    stroke.Thickness = 1
    
    if callback then 
        box.FocusLost:Connect(function() 
            callback(box.Text) 
        end) 
    end
    return box
end

local function CreateDropdown(section, headerText, itemsList, defaultVal, callback)
    local frame = Instance.new("Frame", section)
    frame.Size = UDim2.new(0.92, 0, 0, 30)
    frame.BackgroundColor3 = Theme.ToggleOff
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(1, -30, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = headerText .. ": " .. tostring(defaultVal)
    label.TextColor3 = Theme.Text
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local isDropped = false
    local container = Instance.new("Frame", section)
    container.Size = UDim2.new(0.92, 0, 0, 0)
    container.BackgroundColor3 = Theme.Background
    container.ClipsDescendants = true
    Instance.new("UICorner", container).CornerRadius = UDim.new(0, 6)
    
    local list = Instance.new("UIListLayout", container)
    list.Padding = UDim.new(0, 2)
    
    local function populate()
        for _, c in pairs(container:GetChildren()) do
            if c:IsA("TextButton") then c:Destroy() end
        end
        local currentItems = type(itemsList) == "function" and itemsList() or itemsList
        for _, item in ipairs(currentItems) do
            local btn = Instance.new("TextButton", container)
            btn.Size = UDim2.new(1, 0, 0, 24)
            btn.BackgroundTransparency = 1
            btn.Text = "  " .. tostring(item)
            btn.TextColor3 = Theme.SubText
            btn.Font = Enum.Font.Gotham
            btn.TextSize = 11
            btn.TextXAlignment = Enum.TextXAlignment.Left
            btn.MouseButton1Click:Connect(function()
                label.Text = headerText .. ": " .. tostring(item)
                if callback then callback(item) end
                isDropped = false
                TweenService:Create(container, TweenInfo.new(0.2), {Size = UDim2.new(0.92, 0, 0, 0)}):Play()
            end)
        end
    end
    
    local clickArea = Instance.new("TextButton", frame)
    clickArea.Size = UDim2.new(1, 0, 1, 0)
    clickArea.BackgroundTransparency = 1
    clickArea.Text = ""
    SecureTrigger(clickArea, function()
        isDropped = not isDropped
        if isDropped then populate() end
        TweenService:Create(container, TweenInfo.new(0.2), {Size = isDropped and UDim2.new(0.92, 0, 0, list.AbsoluteContentSize.Y) or UDim2.new(0.92, 0, 0, 0)}):Play()
    end)
end

-- ============================================================================
-- FEATURE LOGICS
-- ============================================================================

-- 1. Movement Helpers
local flyBodyPos, flyBodyGyro
local function ToggleFly(state)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    if state then
        flyBodyPos = Instance.new("BodyPosition", root)
        flyBodyPos.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        flyBodyPos.Position = root.Position
        
        flyBodyGyro = Instance.new("BodyGyro", root)
        flyBodyGyro.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
        flyBodyGyro.CFrame = root.CFrame
        
        task.spawn(function()
            while RyuConfig.Fly and root and flyBodyPos and flyBodyGyro do
                local moveDir = Vector3.new(0,0,0)
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + camera.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - camera.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - camera.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + camera.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0, 1, 0) end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then moveDir = moveDir - Vector3.new(0, 1, 0) end
                
                flyBodyPos.Position = flyBodyPos.Position + (moveDir * (RyuConfig.FlySpeed * 0.05))
                flyBodyGyro.CFrame = camera.CFrame
                RunService.RenderStepped:Wait()
            end
        end)
    else
        if flyBodyPos then flyBodyPos:Destroy() end
        if flyBodyGyro then flyBodyGyro:Destroy() end
    end
end

-- Speed & Noclip loops
RunService.Stepped:Connect(function()
    local char = LocalPlayer.Character
    if char then
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum and RyuConfig.SpeedToggle then
            hum.WalkSpeed = RyuConfig.Speed
        end
        if RyuConfig.Noclip then
            for _, v in pairs(char:GetChildren()) do
                if v:IsA("BasePart") then v.CanCollide = false end
            end
        end
    end
end)

-- Jump Spam
RunService.Heartbeat:Connect(function()
    if RyuConfig.JumpSpam then
        local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
    end
end)

-- Auto Block / Parry & Auto Dodge
local lastDodgeTick = tick()
local lastHealth = 100

RunService.Heartbeat:Connect(function()
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not root or not hum then return end
    
    -- Auto Block
    if RyuConfig.AutoBlock then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local eRoot = p.Character.HumanoidRootPart
                local dist = (eRoot.Position - root.Position).Magnitude
                if dist <= RyuConfig.BlockRange then
                    pcall(function()
                        BlockActivated:FireServer()
                        task.delay(RyuConfig.BlockTimer, function()
                            BlockDeactivated:FireServer()
                        end)
                    end)
                    break
                end
            end
        end
    end
    
    -- Auto Dodge (Teleport backwards on hit)
    if RyuConfig.AutoDodge and hum.Health < lastHealth then
        if tick() - lastDodgeTick > 1.5 then
            lastDodgeTick = tick()
            root.CFrame = root.CFrame * CFrame.new(0, 0, RyuConfig.DodgeDistance)
            pcall(function()
                BlockActivated:FireServer()
                task.delay(0.2, function() BlockDeactivated:FireServer() end)
            end)
        end
    end
    lastHealth = hum.Health
end)

-- Auto Black Flash (Raycast FOV M1 Click)
task.spawn(function()
    while true do
        task.wait(0.1)
        if RyuConfig.AutoBlackFlash then
            local char = LocalPlayer.Character
            local root = char and char:FindFirstChild("HumanoidRootPart")
            if root then
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        local eRoot = p.Character.HumanoidRootPart
                        local dir = (eRoot.Position - root.Position).Unit
                        local dot = root.CFrame.LookVector:Dot(dir)
                        if dot > 0.75 and (eRoot.Position - root.Position).Magnitude < 12 then
                            -- Trigger Divergent / Chase & M1 Simulation
                            if DivergentActivated and char:FindFirstChild("Moveset") and char.Moveset:FindFirstChild("Divergent Fist") then
                                pcall(function() DivergentActivated:FireServer(char.Moveset["Divergent Fist"]) end)
                            end
                            if ItadoriChase then
                                pcall(function() ItadoriChase:FireServer(false) end)
                            end
                            pcall(function()
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
                                task.wait(0.05)
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
                            end)
                        end
                    end
                end
            end
        end
    end
end)

-- Stick / Orbit Target
RunService.RenderStepped:Connect(function()
    if RyuConfig.StickToTarget and RyuConfig.SelectedTarget ~= "" then
        local targetPlayer = Players:FindFirstChild(RyuConfig.SelectedTarget)
        local tRoot = targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart")
        local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if tRoot and root then
            root.CFrame = tRoot.CFrame * CFrame.new(0, 0, RyuConfig.StickDistance)
        end
    end
end)

-- AI Auto Farm Core
task.spawn(function()
    while true do
        task.wait(0.15)
        if RyuConfig.AiFarm then
            local char = LocalPlayer.Character
            local root = char and char:FindFirstChild("HumanoidRootPart")
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if not root or not hum then continue end
            
            local closestTarget = nil
            local minDistance = math.huge
            
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChildOfClass("Humanoid") and p.Character:FindFirstChildOfClass("Humanoid").Health > 0 then
                    local dist = (p.Character.HumanoidRootPart.Position - root.Position).Magnitude
                    if dist < minDistance then
                        minDistance = dist
                        closestTarget = p.Character.HumanoidRootPart
                    end
                end
            end
            
            if closestTarget then
                local dist = (closestTarget.Position - root.Position).Magnitude
                
                -- Move towards target
                if dist > RyuConfig.AttackRange then
                    hum:MoveTo(closestTarget.Position)
                    if RyuConfig.HumanMode and math.random(1, 4) == 1 then
                        hum:ChangeState(Enum.HumanoidStateType.Jumping)
                        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Q, false, game)
                        task.delay(0.05, function() VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Q, false, game) end)
                    end
                else
                    -- Spam Attacks & Skills
                    root.CFrame = CFrame.lookAt(root.Position, Vector3.new(closestTarget.Position.X, root.Position.Y, closestTarget.Position.Z))
                    
                    VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
                    task.wait(0.03)
                    VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
                    
                    local keys = {}
                    if RyuConfig.UseSkill1 and dist <= RyuConfig.Skill1Range then table.insert(keys, Enum.KeyCode.One) end
                    if RyuConfig.UseSkill2 and dist <= RyuConfig.Skill2Range then table.insert(keys, Enum.KeyCode.Two) end
                    if RyuConfig.UseSkill3 and dist <= RyuConfig.Skill3Range then table.insert(keys, Enum.KeyCode.Three) end
                    if RyuConfig.UseSkill4 and dist <= RyuConfig.Skill4Range then table.insert(keys, Enum.KeyCode.Four) end
                    if RyuConfig.AutoUlt then table.insert(keys, Enum.KeyCode.G) end
                    
                    if #keys > 0 then
                        local selectedKey = keys[math.random(1, #keys)]
                        VirtualInputManager:SendKeyEvent(true, selectedKey, false, game)
                        task.delay(0.05, function() VirtualInputManager:SendKeyEvent(false, selectedKey, false, game) end)
                    end
                end
            end
        end
    end
end)

-- Money Farm (Cage + Victim TP)
local moneyFarmCage
task.spawn(function()
    while true do
        task.wait(0.5)
        local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if not root then continue end
        
        if RyuConfig.MoneyFarm then
            if not moneyFarmCage then
                moneyFarmCage = Instance.new("Part", Workspace)
                moneyFarmCage.Size = Vector3.new(20, 1, 20)
                moneyFarmCage.Position = Vector3.new(0, 5000, 0)
                moneyFarmCage.Anchored = true
            end
            
            if RyuConfig.Role == "Farmer" then
                if (root.Position - moneyFarmCage.Position).Magnitude > 30 then
                    root.CFrame = moneyFarmCage.CFrame * CFrame.new(0, 4, 0)
                end
                RyuConfig.AiFarm = true
            elseif RyuConfig.Role == "Helper" and RyuConfig.VictimTarget ~= "" then
                local farmerPlayer = Players:FindFirstChild(RyuConfig.VictimTarget)
                local fRoot = farmerPlayer and farmerPlayer.Character and farmerPlayer.Character:FindFirstChild("HumanoidRootPart")
                if fRoot then
                    if RyuConfig.VictimMode == "Walk" then
                        LocalPlayer.Character.Humanoid:MoveTo(fRoot.Position)
                    else
                        if (fRoot.Position - root.Position).Magnitude > 10 then
                            root.CFrame = fRoot.CFrame * CFrame.new(0, 0, 3)
                        end
                    end
                end
            end
        else
            if moneyFarmCage then
                moneyFarmCage:Destroy()
                moneyFarmCage = nil
            end
        end
    end
end)

-- Server Hop & Alt Join
local function ServerHop(fullThreshold, isHigh)
    local placeId = game.PlaceId
    local serversApi = "https://games.roblox.com/v1/games/" .. placeId .. "/servers/Public?sortOrder=Desc&limit=100"
    pcall(function()
        local raw = game:HttpGet(serversApi)
        local data = HttpService:JSONDecode(raw)
        if data and data.data then
            for _, s in ipairs(data.data) do
                if s.playing and s.maxPlayers and s.id ~= game.JobId then
                    local fillRate = s.playing / s.maxPlayers
                    if isHigh and fillRate >= (fullThreshold / 100) and s.playing < s.maxPlayers then
                        TeleportService:TeleportToPlaceInstance(placeId, s.id, LocalPlayer)
                        break
                    elseif not isHigh and fillRate <= (fullThreshold / 100) then
                        TeleportService:TeleportToPlaceInstance(placeId, s.id, LocalPlayer)
                        break
                    end
                end
            end
        end
    end)
end

-- ============================================================================
-- UI TABS SETUP
-- ============================================================================

local function GetPlayerList()
    local list = {}
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then table.insert(list, p.Name) end
    end
    return list
end

-- TAB 1: COMBAT
local TabCombat = CreateMainTab("Combat")

-- Subtab: Player
local SubPlayer = CreateSubTab(TabCombat, "Player")
local SecPlayer = CreateSection(SubPlayer, "Movement & Modifications")
CreateToggle(SecPlayer, "Speed Modifier", "Applies custom walkspeed to character.", RyuConfig.SpeedToggle, function(v) RyuConfig.SpeedToggle = v end)
CreateSlider(SecPlayer, "Speed Amount", 16, 120, RyuConfig.Speed, function(v) RyuConfig.Speed = v end)
CreateToggle(SecPlayer, "Fly", "Fly freely in any direction using camera.", RyuConfig.Fly, function(v) RyuConfig.Fly = v; ToggleFly(v) end)
CreateSlider(SecPlayer, "Fly Speed", 20, 150, RyuConfig.FlySpeed, function(v) RyuConfig.FlySpeed = v end)
CreateSlider(SecPlayer, "Jump Power", 50, 200, RyuConfig.JumpPower, function(v)
    RyuConfig.JumpPower = v
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = v
    end
end)
CreateToggle(SecPlayer, "Jump Spam", "Spams jumping to ascend infinitely.", RyuConfig.JumpSpam, function(v) RyuConfig.JumpSpam = v end)
CreateToggle(SecPlayer, "Noclip", "Walk through all solid walls.", RyuConfig.Noclip, function(v) RyuConfig.Noclip = v end)
CreateToggle(SecPlayer, "Invisibility", "Turns character model client-side invisible.", RyuConfig.Invisible, function(v)
    RyuConfig.Invisible = v
    if LocalPlayer.Character then
        for _, p in pairs(LocalPlayer.Character:GetDescendants()) do
            if p:IsA("BasePart") or p:IsA("Decal") then p.Transparency = v and 1 or 0 end
        end
    end
end)
CreateButton(SecPlayer, "Play Dance (Sync)", function()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then
        local anim = Instance.new("Animation")
        anim.AnimationId = "rbxassetid://507771019"
        local track = hum:LoadAnimation(anim)
        track:Play()
    end
end)

-- Subtab: Auto
local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAuto = CreateSection(SubAuto, "Combat Automation")
CreateToggle(SecAuto, "Auto Block / Parry", "Automatically blocks enemy attacks in range.", RyuConfig.AutoBlock, function(v) RyuConfig.AutoBlock = v end)
CreateSlider(SecAuto, "Block Hold Time (s)", 0.1, 2, RyuConfig.BlockTimer, function(v) RyuConfig.BlockTimer = v end)
CreateSlider(SecAuto, "Block Range (Studs)", 5, 30, RyuConfig.BlockRange, function(v) RyuConfig.BlockRange = v end)
CreateToggle(SecAuto, "Auto Black Flash", "Raycasts opponent FOV and executes flash M1.", RyuConfig.AutoBlackFlash, function(v) RyuConfig.AutoBlackFlash = v end)
CreateToggle(SecAuto, "Auto Dodge (Backstep)", "Teleports backward instantly on damage.", RyuConfig.AutoDodge, function(v) RyuConfig.AutoDodge = v end)
CreateSlider(SecAuto, "Dodge Distance", 10, 60, RyuConfig.DodgeDistance, function(v) RyuConfig.DodgeDistance = v end)

local SecCombos = CreateSection(SubAuto, "Auto Combos")
local comboLabel = Instance.new("TextLabel", SecCombos)
comboLabel.Size = UDim2.new(0.92, 0, 0, 30); comboLabel.BackgroundTransparency = 1
comboLabel.Text = "Join discord.gg/ryuhub and send me clips of combos."; comboLabel.TextColor3 = Theme.SubText
comboLabel.Font = Enum.Font.Gotham; comboLabel.TextSize = 11; comboLabel.TextWrapped = true

-- Subtab: Ability
local SubAbility = CreateSubTab(TabCombat, "Ability")
local SecAbility = CreateSection(SubAbility, "Super Abilities")
CreateToggle(SecAbility, "Lock-On Target", "Locks camera orientation toward nearest foe.", RyuConfig.LockOn, function(v) RyuConfig.LockOn = v end)
CreateToggle(SecAbility, "Knockback Multiplier", "Flings opponents away upon receiving M1.", RyuConfig.Knockback, function(v) RyuConfig.Knockback = v end)
CreateSlider(SecAbility, "Knockback Force", 50, 300, RyuConfig.KnockbackPower, function(v) RyuConfig.KnockbackPower = v end)
CreateButton(SecAbility, "TP All Players To Me", function()
    local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.CFrame = root.CFrame * CFrame.new(0, 0, -3)
            end
        end
    end
end)
CreateToggle(SecAbility, "Infinite Awakening Gauge", "Simulates continuous ultimate state.", RyuConfig.InfiniteAwakening, function(v) RyuConfig.InfiniteAwakening = v end)
CreateButton(SecAbility, "Instant Reverse Cursed Technique", function()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.Health = hum.MaxHealth end
end)

local suggestLabel = Instance.new("TextLabel", SecAbility)
suggestLabel.Size = UDim2.new(0.92, 0, 0, 20); suggestLabel.BackgroundTransparency = 1
suggestLabel.Text = "suggest more in discord.gg/ryuhub"; suggestLabel.TextColor3 = Theme.SubText
suggestLabel.Font = Enum.Font.GothamBold; suggestLabel.TextSize = 11

-- TAB 2: FARM
local TabFarm = CreateMainTab("Farm")

-- Subtab: AI Auto Farm
local SubAiFarm = CreateSubTab(TabFarm, "AI Auto Farm")
local SecAi = CreateSection(SubAiFarm, "Automated Player Farming")
CreateToggle(SecAi, "Enable AI Auto Farm", "Tracks nearest player, spams M1s and skills.", RyuConfig.AiFarm, function(v) RyuConfig.AiFarm = v end)
CreateSlider(SecAi, "Attack Engagement Range", 5, 25, RyuConfig.AttackRange, function(v) RyuConfig.AttackRange = v end)
CreateToggle(SecAi, "Auto Ultimate (G)", "Triggers awakening immediately when ready.", RyuConfig.AutoUlt, function(v) RyuConfig.AutoUlt = v end)
CreateToggle(SecAi, "Use Skill 1", RyuConfig.UseSkill1, function(v) RyuConfig.UseSkill1 = v end)
CreateSlider(SecAi, "Skill 1 Range", 5, 30, RyuConfig.Skill1Range, function(v) RyuConfig.Skill1Range = v end)
CreateToggle(SecAi, "Use Skill 2", RyuConfig.UseSkill2, function(v) RyuConfig.UseSkill2 = v end)
CreateSlider(SecAi, "Skill 2 Range", 5, 30, RyuConfig.Skill2Range, function(v) RyuConfig.Skill2Range = v end)
CreateToggle(SecAi, "Use Skill 3", RyuConfig.UseSkill3, function(v) RyuConfig.UseSkill3 = v end)
CreateSlider(SecAi, "Skill 3 Range", 5, 30, RyuConfig.Skill3Range, function(v) RyuConfig.Skill3Range = v end)
CreateToggle(SecAi, "Use Skill 4", RyuConfig.UseSkill4, function(v) RyuConfig.UseSkill4 = v end)
CreateSlider(SecAi, "Skill 4 Range", 5, 30, RyuConfig.Skill4Range, function(v) RyuConfig.Skill4Range = v end)
CreateToggle(SecAi, "Human-Like Movements", "Adds strafes, jump dashes (Q), and natural steps.", RyuConfig.HumanMode, function(v) RyuConfig.HumanMode = v end)

-- Subtab: Target
local SubTarget = CreateSubTab(TabFarm, "Target")
local SecTarget = CreateSection(SubTarget, "Target Stalking")
CreateDropdown(SecTarget, "Select Player", GetPlayerList, "None", function(v) RyuConfig.SelectedTarget = v end)
CreateToggle(SecTarget, "Stick Behind Player", "Permanently positions character behind target.", RyuConfig.StickToTarget, function(v) RyuConfig.StickToTarget = v end)
CreateSlider(SecTarget, "Follow Offset Distance", 2, 15, RyuConfig.StickDistance, function(v) RyuConfig.StickDistance = v end)

-- Subtab: Money Farm
local SubMoney = CreateSubTab(TabFarm, "Money Farm")
local SecMoney = CreateSection(SubMoney, "Sky-Cage Automated Farm")
CreateToggle(SecMoney, "Enable Money Farm", "Spawns a 5k stud sky cage and farms alts.", RyuConfig.MoneyFarm, function(v) RyuConfig.MoneyFarm = v end)
CreateDropdown(SecMoney, "Account Role", {"Farmer", "Helper"}, RyuConfig.Role, function(v) RyuConfig.Role = v end)
CreateDropdown(SecMoney, "Helper Target", GetPlayerList, "None", function(v) RyuConfig.VictimTarget = v end)
CreateDropdown(SecMoney, "Helper Mode", {"Walk", "Teleport"}, RyuConfig.VictimMode, function(v) RyuConfig.VictimMode = v end)

local SecMoneyInfo = CreateSection(SubMoney, "Guide & Instructions")
local infoText = Instance.new("TextLabel", SecMoneyInfo)
infoText.Size = UDim2.new(0.92, 0, 0, 150); infoText.BackgroundTransparency = 1
infoText.Text = "How to use:\n1. Main account should be set to farmer\n2. Alts should be set to helper\n3. Turn everything on.\n\nMissunderstandings:\nYes all of your alts need the script (use a roblox cloner)\nBefore you ask help in the discord make sure to check if you did everything right.\n\nAlso very important: stay happy in life and never give up."
infoText.TextColor3 = Theme.SubText; infoText.Font = Enum.Font.Gotham; infoText.TextSize = 11
infoText.TextXAlignment = Enum.TextXAlignment.Left; infoText.TextWrapped = true

-- Subtab: Config
local SubConfig = CreateSubTab(TabFarm, "Config")
local SecConfig = CreateSection(SubConfig, "Server & Multi-Account Settings")
CreateToggle(SecConfig, "Auto Rejoin on Player Count", "Rejoins if lobby players drop below count.", RyuConfig.AutoRejoin, function(v) RyuConfig.AutoRejoin = v end)
CreateSlider(SecConfig, "Player Threshold", 1, 15, RyuConfig.RejoinThreshold, function(v) RyuConfig.RejoinThreshold = v end)
CreateDropdown(SecConfig, "Lobby Type", {"High", "Low"}, RyuConfig.LobbyType, function(v) RyuConfig.LobbyType = v end)
CreateButton(SecConfig, "Rejoin Matching Server", function()
    ServerHop(80, RyuConfig.LobbyType == "High")
end)
CreateTextBox(SecConfig, "Main Account Username", function(v) RyuConfig.MainAccountName = v end)
CreateButton(SecConfig, "Join Main Account", function()
    if RyuConfig.MainAccountName ~= "" then
        RyuNotify:Send("Alt Joiner", "Searching session for " .. RyuConfig.MainAccountName, 3)
    end
end)
CreateButton(SecConfig, "Save Settings", function()
    SaveSettings()
    RyuNotify:Send("Settings", "Config successfully saved!", 3)
end)

-- init
task.spawn(function()
    if Tabs[1] and Tabs[1].ToggleFunc then Tabs[1].ToggleFunc() end
    if Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].SelectFunc then Tabs[1].SubTabs[1].SelectFunc() end
    RyuNotify:Send("Ryu Hub", "Jujutsu Shenanigans Edition Loaded!", 3)
end)

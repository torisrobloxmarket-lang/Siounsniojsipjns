--// ==========================================
--// RYU HUB - TOWER OF HELL SUITE v5.1
--// MONOCHROME - AUTO WIN FIX & SKIN HISTORY
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local lp = LocalPlayer
local char = lp.Character or lp.CharacterAdded:Wait()
local root, hum

local function bindChar(c)
    char = c
    root = c:WaitForChild("HumanoidRootPart", 5)
    hum  = c:WaitForChild("Humanoid", 5)
end
bindChar(char)
lp.CharacterAdded:Connect(bindChar)

--// ── CONFIGURATION & SETTINGS ────────────────────────────────────────────────
local CFG = {
    speedBoost      = false,
    speedVal        = 48,
    superJump       = false,
    jumpVal         = 150,
    fly             = false,
    flySpeed        = 60,
    dashSpeed       = 140,
    dashCooldown    = 0.5,
    noclip          = true,
    invisible       = false,
    infiniteJump    = false,
    antiVoid        = false,
    antiVoidY       = -50,
    godmode         = false,
    spin            = false,
    spinSpeed       = 35,
    playerEsp       = false,
    autoWin         = false,
    autoWinInterval = 2, -- minutes
    freecam         = false,
    freecamSpeed    = 1,
    antiAfk         = false,
    fakeName        = "",
}

local savedCheckpoints = {}
local activeCheckpointSlot = 1
local skinHistory = {}
local NotificationContainer

--// 1. GUI CLEANUP & INJECTION
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
    if v.Name == "RyuHubToH" or v.Name == "RyuNotifications" then v:Destroy() end 
end

--// 2. THEME & UI FRAMEWORK (PURE MONOCHROME)
local Theme = {
    Background = Color3.fromRGB(15, 15, 15), Sidebar = Color3.fromRGB(22, 22, 22),
    SectionBG = Color3.fromRGB(30, 30, 30), Text = Color3.fromRGB(255, 255, 255),
    SubText = Color3.fromRGB(150, 150, 150), Accent = Color3.fromRGB(255, 255, 255),
    ToggleOff = Color3.fromRGB(45, 45, 45), ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(60, 60, 60), Warning = Color3.fromRGB(255, 255, 255)
}

local MainSize = UDim2.new(0, math.min(750, camera.ViewportSize.X - 40), 0, math.min(480, camera.ViewportSize.Y - 40))
local SidebarWidth = 160

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubToH"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

-- Notification System
NotificationContainer = Instance.new("Frame", RyuHub)
NotificationContainer.Name = "RyuNotifications"
NotificationContainer.Size = UDim2.new(0, 260, 1, -40)
NotificationContainer.Position = UDim2.new(1, -280, 0, 20)
NotificationContainer.BackgroundTransparency = 1

local NotifLayout = Instance.new("UIListLayout", NotificationContainer)
NotifLayout.SortOrder = Enum.SortOrder.LayoutOrder
NotifLayout.VerticalAlignment = Enum.VerticalAlignment.Bottom
NotifLayout.Padding = UDim.new(0, 8)

local function SendNotification(title, text, duration)
    duration = duration or 2
    local f = Instance.new("Frame", NotificationContainer)
    f.Size = UDim2.new(1, 0, 0, 50); f.BackgroundColor3 = Theme.Sidebar; f.BackgroundTransparency = 0.1
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 8)
    Instance.new("UIStroke", f).Color = Theme.Stroke
    
    local t = Instance.new("TextLabel", f)
    t.Size = UDim2.new(1, -15, 0, 20); t.Position = UDim2.new(0, 10, 0, 5); t.BackgroundTransparency = 1
    t.Text = title; t.TextColor3 = Theme.Text; t.Font = Enum.Font.GothamBold; t.TextSize = 12; t.TextXAlignment = Enum.TextXAlignment.Left
    
    local m = Instance.new("TextLabel", f)
    m.Size = UDim2.new(1, -15, 0, 20); m.Position = UDim2.new(0, 10, 0, 25); m.BackgroundTransparency = 1
    m.Text = text; m.TextColor3 = Theme.SubText; m.Font = Enum.Font.Gotham; m.TextSize = 11; m.TextXAlignment = Enum.TextXAlignment.Left
    
    task.spawn(function()
        task.wait(duration)
        TweenService:Create(f, TweenInfo.new(0.3), {BackgroundTransparency = 1, Size = UDim2.new(1,0,0,0)}):Play()
        task.wait(0.3)
        f:Destroy()
    end)
end

local function AddClickPop(element)
    local orig = element.Size
    element.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            pcall(function() TweenService:Create(element, TweenInfo.new(0.1), {Size = UDim2.new(orig.X.Scale, orig.X.Offset-4, orig.Y.Scale, orig.Y.Offset-4)}):Play() end)
        end
    end)
    element.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            pcall(function() TweenService:Create(element, TweenInfo.new(0.3), {Size = orig}):Play() end)
        end
    end)
end

-- FIXED TOP LEFT TOGGLE BUTTON
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
ToggleBtn.Position = UDim2.new(0, 15, 0, 60)
ToggleBtn.BackgroundColor3 = Theme.Sidebar
ToggleBtn.Text = ""
ToggleBtn.Parent = RyuHub
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
local btnStroke = Instance.new("UIStroke", ToggleBtn)
btnStroke.Color = Theme.Accent; btnStroke.Thickness = 2; btnStroke.Transparency = 0.5

local Katana = Instance.new("Frame", ToggleBtn)
Katana.Size = UDim2.new(1, 0, 1, 0); Katana.BackgroundTransparency = 1; Katana.Rotation = 45
local Blade = Instance.new("Frame", Katana)
Blade.Size = UDim2.new(0, 2, 0, 24); Blade.Position = UDim2.new(0.5, -1, 0.5, -18); Blade.BackgroundColor3 = Theme.Text; Blade.BorderSizePixel = 0
local Guard = Instance.new("Frame", Katana)
Guard.Size = UDim2.new(0, 12, 0, 2); Guard.Position = UDim2.new(0.5, -6, 0.5, 6); Guard.BackgroundColor3 = Theme.SubText; Guard.BorderSizePixel = 0
local Handle = Instance.new("Frame", Katana)
Handle.Size = UDim2.new(0, 4, 0, 10); Handle.Position = UDim2.new(0.5, -2, 0.5, 8); Handle.BackgroundColor3 = Theme.Stroke; Handle.BorderSizePixel = 0
Instance.new("UICorner", Blade).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Guard).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Handle).CornerRadius = UDim.new(0, 1)

AddClickPop(ToggleBtn)

-- MAIN WINDOW WITH WAVE ANIMATION
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
Instance.new("UIStroke", MainFrame).Color = Theme.Stroke; Instance.new("UIStroke", MainFrame).Thickness = 1.5

local DiscordWatermark = Instance.new("TextLabel", RyuHub)
DiscordWatermark.Size = UDim2.new(0, 300, 0, 30)
DiscordWatermark.Position = UDim2.new(0.5, -150, 0.5, -260)
DiscordWatermark.BackgroundTransparency = 1
DiscordWatermark.Text = "Discord.gg/ryuhub"
DiscordWatermark.Font = Enum.Font.GothamBlack
DiscordWatermark.TextSize = 16
DiscordWatermark.TextColor3 = Theme.Accent
DiscordWatermark.Visible = false
DiscordWatermark.ZIndex = 5

local WatermarkGradient = Instance.new("UIGradient", DiscordWatermark)
WatermarkGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(150, 150, 150)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 255, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(150, 150, 150))
}

task.spawn(function()
    while true do
        pcall(function()
            TweenService:Create(WatermarkGradient, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Offset = Vector2.new(1, 0)}):Play()
            task.wait(1.5)
            TweenService:Create(WatermarkGradient, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Offset = Vector2.new(-1, 0)}):Play()
            task.wait(1.5)
        end)
    end
end)

ToggleBtn.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        pcall(function() 
            TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() 
            TweenService:Create(DiscordWatermark, TweenInfo.new(0.2), {TextTransparency = 1}):Play()
        end)
        task.delay(0.3, function() MainFrame.Visible = false; DiscordWatermark.Visible = false end)
    else
        MainFrame.Visible = true
        DiscordWatermark.Visible = true
        DiscordWatermark.Position = UDim2.new(0.5, -150, 0.5, -(MainSize.Y.Offset/2) - 35)
        pcall(function()
            TweenService:Create(MainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = MainSize, Position = UDim2.new(0.5, -MainSize.X.Offset/2, 0.5, -MainSize.Y.Offset/2)}):Play()
            TweenService:Create(DiscordWatermark, TweenInfo.new(0.4), {TextTransparency = 0}):Play()
        end)
    end
end)

local ContentWrapper = Instance.new("Frame", MainFrame)
ContentWrapper.Size = UDim2.new(1, 0, 1, 0); ContentWrapper.Position = UDim2.new(0, 0, 0, 0); ContentWrapper.BackgroundTransparency = 1; ContentWrapper.BorderSizePixel = 0; ContentWrapper.ZIndex = 1

local Topbar = Instance.new("Frame", ContentWrapper)
Topbar.Size = UDim2.new(1, 0, 0, 60); Topbar.BackgroundTransparency = 1
local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 300, 1, 0); Title.Position = UDim2.new(0, 20, 0, 0); Title.BackgroundTransparency = 1
Title.Text = "RYU HUB"; Title.Font = Enum.Font.GothamBlack; Title.TextSize = 22; Title.TextXAlignment = Enum.TextXAlignment.Left
Title.TextColor3 = Theme.Text

local SubTitle = Instance.new("TextLabel", Topbar)
SubTitle.Size = UDim2.new(0, 300, 0, 15); SubTitle.Position = UDim2.new(0, 20, 0, 38); SubTitle.BackgroundTransparency = 1
SubTitle.Text = "Tower of Hell Suite"; SubTitle.TextColor3 = Theme.SubText; SubTitle.Font = Enum.Font.Gotham; SubTitle.TextSize = 11; SubTitle.TextXAlignment = Enum.TextXAlignment.Left

-- CLOSE BUTTON
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
    pcall(function() 
        TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() 
        TweenService:Create(DiscordWatermark, TweenInfo.new(0.2), {TextTransparency = 1}):Play()
    end)
    task.delay(0.3, function() MainFrame.Visible = false; DiscordWatermark.Visible = false end)
end)

local mDragging, mDragStart, mStartPos = false, nil, nil
Topbar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then 
        mDragging = true; mDragStart = input.Position; mStartPos = MainFrame.Position 
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
Line.Size = UDim2.new(1, -40, 0, 1); Line.Position = UDim2.new(0, 20, 0, 65); Line.BackgroundColor3 = Theme.Stroke; Line.BorderSizePixel = 0

local Sidebar = Instance.new("ScrollingFrame", ContentWrapper)
Sidebar.Size = UDim2.new(0, SidebarWidth, 1, -85); Sidebar.Position = UDim2.new(0, 10, 0, 75); Sidebar.BackgroundTransparency = 1; Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar)
SideLayout.Padding = UDim.new(0, 6); SideLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left; SideLayout.SortOrder = Enum.SortOrder.LayoutOrder

local ContentContainer = Instance.new("Frame", ContentWrapper)
ContentContainer.Size = UDim2.new(1, -(SidebarWidth + 25), 1, -85); ContentContainer.Position = UDim2.new(0, SidebarWidth + 15, 0, 75); ContentContainer.BackgroundTransparency = 1

local Tabs = {}
local itemOrderCounter = 0

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
    tabBtn.Size = UDim2.new(1, 0, 0, 36); tabBtn.BackgroundColor3 = Theme.Sidebar; tabBtn.Text = "  " .. string.upper(name)
    tabBtn.TextColor3 = Theme.SubText; tabBtn.Font = Enum.Font.GothamBlack; tabBtn.TextSize = 13; tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8)
    tabObj.Btn = tabBtn

    local subContainer = Instance.new("Frame", Sidebar)
    subContainer.Size = UDim2.new(1, 0, 0, 0); subContainer.BackgroundTransparency = 1; subContainer.ClipsDescendants = true
    tabObj.SubContainer = subContainer

    local subLayout = Instance.new("UIListLayout", subContainer)
    subLayout.Padding = UDim.new(0, 2); subLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left; subLayout.SortOrder = Enum.SortOrder.LayoutOrder
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
    subBtn.Size = UDim2.new(1, 0, 0, 28); subBtn.BackgroundTransparency = 1; subBtn.Text = "     " .. subName
    subBtn.TextColor3 = Theme.SubText; subBtn.Font = Enum.Font.GothamMedium; subBtn.TextSize = 12; subBtn.TextXAlignment = Enum.TextXAlignment.Left
    subObj.Btn = subBtn

    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0); page.BackgroundTransparency = 1; page.ScrollBarThickness = 2; page.ScrollBarImageColor3 = Theme.Accent; page.Visible = false
    subObj.Page = page

    local pageLayout = Instance.new("UIListLayout", page)
    pageLayout.Padding = UDim.new(0, 12); pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    pageLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() page.CanvasSize = UDim2.new(0, 0, 0, pageLayout.AbsoluteContentSize.Y + 20) end)

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
    section.Size = UDim2.new(0.98, 0, 0, 50); section.BackgroundColor3 = Theme.SectionBG; section.BackgroundTransparency = 0
    Instance.new("UICorner", section).CornerRadius = UDim.new(0, 10)
    Instance.new("UIStroke", section).Color = Theme.Stroke
    
    local secLayout = Instance.new("UIListLayout", section)
    secLayout.Padding = UDim.new(0, 10); secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center; secLayout.SortOrder = Enum.SortOrder.LayoutOrder
    Instance.new("UIPadding", section).PaddingTop = UDim.new(0, 12); Instance.new("UIPadding", section).PaddingBottom = UDim.new(0, 12)
    
    local title = Instance.new("TextLabel", section)
    title.LayoutOrder = -1; title.Size = UDim2.new(0.92, 0, 0, 24); title.BackgroundTransparency = 1; title.Text = titleText
    title.TextColor3 = Theme.Text; title.Font = Enum.Font.GothamBold; title.TextSize = 14; title.TextXAlignment = Enum.TextXAlignment.Left
    
    secLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() section.Size = UDim2.new(1, 0, 0, secLayout.AbsoluteContentSize.Y + 24) end)
    return section
end

local function CreateLabel(section, text)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 30); frame.BackgroundTransparency = 1
    local lbl = Instance.new("TextLabel", frame)
    lbl.Size = UDim2.new(1, 0, 1, 0); lbl.BackgroundTransparency = 1; lbl.Text = text; lbl.TextColor3 = Theme.SubText
    lbl.Font = Enum.Font.GothamMedium; lbl.TextSize = 11; lbl.TextXAlignment = Enum.TextXAlignment.Left; lbl.TextWrapped = true
    return lbl
end

local function CreateToggle(section, text, descText, defaultState, callback)
    if type(descText) == "boolean" then callback = defaultState; defaultState = descText; descText = nil end
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, descText and 52 or 34); frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.7, 0, 0, 34); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = defaultState and Theme.Text or Theme.SubText
    label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    
    if descText then
        local descLabel = Instance.new("TextLabel", frame)
        descLabel.Size = UDim2.new(0.7, 0, 0, 15); descLabel.Position = UDim2.new(0, 0, 0, 30); descLabel.BackgroundTransparency = 1
        descLabel.Text = descText; descLabel.TextColor3 = Theme.SubText; descLabel.Font = Enum.Font.Gotham; descLabel.TextSize = 11; descLabel.TextXAlignment = Enum.TextXAlignment.Left
    end
    
    local tBtn = Instance.new("TextButton", frame)
    tBtn.Size = UDim2.new(0, 42, 0, 22); tBtn.Position = UDim2.new(1, -42, 0, 6); tBtn.BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff; tBtn.Text = ""
    Instance.new("UICorner", tBtn).CornerRadius = UDim.new(1, 0)
    
    local circle = Instance.new("Frame", tBtn)
    circle.Size = UDim2.new(0, 16, 0, 16); circle.Position = defaultState and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8)
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
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 50); frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(1, -40, 0, 18); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Theme.SubText
    label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    
    local valLabel = Instance.new("TextLabel", frame)
    valLabel.Size = UDim2.new(0, 40, 0, 18); valLabel.Position = UDim2.new(1, -40, 0, 0); valLabel.BackgroundTransparency = 1; valLabel.Text = tostring(default)
    valLabel.TextColor3 = Theme.Accent; valLabel.Font = Enum.Font.GothamBold; valLabel.TextSize = 13; valLabel.TextXAlignment = Enum.TextXAlignment.Right
    
    local sliderBg = Instance.new("Frame", frame)
    sliderBg.Size = UDim2.new(1, 0, 0, 4); sliderBg.Position = UDim2.new(0, 0, 0, 32); sliderBg.BackgroundColor3 = Theme.ToggleOff
    Instance.new("UICorner", sliderBg).CornerRadius = UDim.new(1, 0)
    
    local sliderFill = Instance.new("Frame", sliderBg)
    local percentage = math.clamp((default - min) / (max - min), 0, 1)
    sliderFill.Size = UDim2.new(percentage, 0, 1, 0); sliderFill.BackgroundColor3 = Theme.Accent
    Instance.new("UICorner", sliderFill).CornerRadius = UDim.new(1, 0)
    
    local knob = Instance.new("TextButton", sliderFill)
    knob.Size = UDim2.new(0, 14, 0, 14); knob.Position = UDim2.new(1, -7, 0.5, -7); knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255); knob.Text = ""
    Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
    
    local dragging = false
    knob.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relative = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            local value = math.floor((min + (max - min) * relative) * 10) / 10
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
    btn.LayoutOrder = itemOrderCounter; btn.Size = UDim2.new(0.92, 0, 0, 34); btn.BackgroundColor3 = color
    btn.Text = text; btn.TextColor3 = Color3.fromRGB(255,255,255); btn.Font = Enum.Font.GothamBold; btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", btn).Color = Theme.Stroke
    AddClickPop(btn)
    btn.MouseButton1Click:Connect(function() pcall(callback) end)
    return btn
end

local function CreateInput(section, placeholder, callback)
    itemOrderCounter = itemOrderCounter + 1
    local box = Instance.new("TextBox", section)
    box.LayoutOrder = itemOrderCounter; box.Size = UDim2.new(0.92, 0, 0, 34); box.BackgroundColor3 = Theme.Background
    box.Text = ""; box.PlaceholderText = placeholder; box.TextColor3 = Theme.Text; box.Font = Enum.Font.GothamMedium; box.TextSize = 12
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", box).Color = Theme.Stroke
    if callback then box.FocusLost:Connect(function() pcall(function() callback(box.Text) end) end) end
    return box
end

--// ==========================================
--// TOWER OF HELL LOGIC (FIXED AUTO WIN & SKINS)
--// ==========================================

-- NOCLIP
RunService.Stepped:Connect(function()
    if not CFG.noclip or not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") then p.CanCollide = false end
    end
end)

-- FLY WITH ADJUSTABLE SPEED
local flyConn, flyBV
local flyKeys = {f=false,b=false,l=false,r=false,up=false,down=false}

local function setFly(on)
    CFG.fly = on
    if flyBV and flyBV.Parent then flyBV:Destroy() end
    if flyConn then flyConn:Disconnect() flyConn = nil end
    if not on then
        if hum then hum.PlatformStand = false end
        return
    end
    hum.PlatformStand = true
    flyBV = Instance.new("BodyVelocity", root)
    flyBV.MaxForce = Vector3.new(1e5,1e5,1e5)
    flyBV.Velocity = Vector3.zero
    flyConn = RunService.Heartbeat:Connect(function()
        if not CFG.fly or not root then return end
        local cf = camera.CFrame
        local dir = Vector3.zero
        if flyKeys.f then dir = dir + cf.LookVector end
        if flyKeys.b then dir = dir - cf.LookVector end
        if flyKeys.r then dir = dir + cf.RightVector end
        if flyKeys.l then dir = dir - cf.RightVector end
        if flyKeys.up then dir = dir + Vector3.new(0,1,0) end
        if flyKeys.down then dir = dir - Vector3.new(0,1,0) end
        flyBV.Velocity = dir.Magnitude > 0 and dir.Unit * CFG.flySpeed else Vector3.zero end
    end)
end

UserInputService.InputBegan:Connect(function(i, gp)
    if gp then return end
    local k = i.KeyCode
    if k == Enum.KeyCode.W then flyKeys.f = true end
    if k == Enum.KeyCode.S then flyKeys.b = true end
    if k == Enum.KeyCode.A then flyKeys.l = true end
    if k == Enum.KeyCode.D then flyKeys.r = true end
    if k == Enum.KeyCode.Space then flyKeys.up = true end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = true end
end)
UserInputService.InputEnded:Connect(function(i)
    local k = i.KeyCode
    if k == Enum.KeyCode.W then flyKeys.f = false end
    if k == Enum.KeyCode.S then flyKeys.b = false end
    if k == Enum.KeyCode.A then flyKeys.l = false end
    if k == Enum.KeyCode.D then flyKeys.r = false end
    if k == Enum.KeyCode.Space then flyKeys.up = false end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = false end
end)

-- DASH
local dashReady = true
local function doDash()
    if not dashReady or not root or not hum then return end
    dashReady = false
    local dir = (camera.CFrame.LookVector * Vector3.new(1,0,1)).Unit
    local bv = Instance.new("BodyVelocity", root)
    bv.MaxForce = Vector3.new(1e5, 0, 1e5)
    bv.Velocity = dir * CFG.dashSpeed
    task.delay(0.18, function() if bv and bv.Parent then bv:Destroy() end end)
    task.delay(CFG.dashCooldown, function() dashReady = true end)
end

UserInputService.InputBegan:Connect(function(i, gp)
    if not gp and i.KeyCode == Enum.KeyCode.Q then doDash() end
end)

-- SPEED & JUMP
local function setSpeed(on)
    CFG.speedBoost = on
    if hum then hum.WalkSpeed = on and CFG.speedVal or 16 end
end

local function setJump(on)
    CFG.superJump = on
    if hum then hum.JumpPower = on and CFG.jumpVal or 50 end
end

UserInputService.JumpRequest:Connect(function()
    if CFG.infiniteJump and hum then
        hum:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- INVISIBLE
local function setInvisible(on)
    CFG.invisible = on
    if not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") or p:IsA("Decal") then
            if p.Name ~= "HumanoidRootPart" then
                p.Transparency = on and 0.7 or 0
            end
        end
    end
end

-- ANTI-VOID
local antiVoidConn
local function setAntiVoid(on)
    CFG.antiVoid = on
    if antiVoidConn then antiVoidConn:Disconnect() antiVoidConn = nil end
    if not on then return end
    antiVoidConn = RunService.Heartbeat:Connect(function()
        if not root then return end
        if root.Position.Y < CFG.antiVoidY then
            root.CFrame = CFrame.new(root.Position.X, CFG.antiVoidY + 25, root.Position.Z)
        end
    end)
end

-- SPIN
local spinConn
local function setSpin(on)
    CFG.spin = on
    if spinConn then spinConn:Disconnect() spinConn = nil end
    if not on then return end
    spinConn = RunService.Heartbeat:Connect(function()
        if not root then return end
        root.CFrame = root.CFrame * CFrame.Angles(0, math.rad(CFG.spinSpeed), 0)
    end)
end

-- AUTO WIN (TP to finishes folder as shown in screenshot)
local function tpToFinishes()
    if not root then return end
    local finishesFolder = Workspace:FindFirstChild("finishes", true) or Workspace:FindFirstChild("Finishes", true)
    local targetPart = nil
    
    if finishesFolder then
        for _, child in ipairs(finishesFolder:GetChildren()) do
            if child:IsA("BasePart") then
                targetPart = child
                break
            end
        end
    end
    
    if targetPart then
        root.CFrame = targetPart.CFrame + Vector3.new(0, 3, 0)
    else
        -- Fallback search by name
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("BasePart") and (obj.Name:lower() == "finish" or obj.Name:lower() == "endzone") then
                targetPart = obj
                break
            end
        end
        if targetPart then root.CFrame = targetPart.CFrame + Vector3.new(0, 3, 0) else root.CFrame = root.CFrame + Vector3.new(0, 500, 0) end
    end
    
    pcall(function()
        if hum then hum.PlatformStand = true task.wait(0.6) hum.PlatformStand = false end
    end)
end

task.spawn(function()
    while task.wait(1) do
        if CFG.autoWin then
            tpToFinishes()
            SendNotification("Auto Win", "Teleported to finishes!", 2)
            task.wait(CFG.autoWinInterval * 60)
        end
    end
end)

-- FREECAM
local freecamConn, freecamPos = nil, Vector3.zero
local function setFreecam(on)
    CFG.freecam = on
    if freecamConn then freecamConn:Disconnect() freecamConn = nil end
    if on then
        freecamPos = camera.CFrame.Position
        camera.CameraType = Enum.CameraType.Scriptable
        freecamConn = RunService.RenderStepped:Connect(function(dt)
            local speed = CFG.freecamSpeed * 60 * dt
            local cf = camera.CFrame
            local moveDir = Vector3.zero
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + cf.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - cf.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - cf.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + cf.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.E) then moveDir = moveDir + Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.Q) then moveDir = moveDir - Vector3.new(0,1,0) end
            freecamPos = freecamPos + moveDir * speed
            camera.CFrame = CFrame.new(freecamPos, freecamPos + cf.LookVector)
        end)
    else
        camera.CameraType = Enum.CameraType.Custom
    end
end

--// ==========================================
--// BUILD TABS & SECTIONS
--// ==========================================

local TabBoost = CreateMainTab("Boost")

local SubMove = CreateSubTab(TabBoost, "Movement")
local SecMove = CreateSection(SubMove, "Speed & Jump")
CreateToggle(SecMove, "Speed Boost", false, function(s) setSpeed(s) end)
CreateSlider(SecMove, "Speed Value", 16, 200, 48, function(v) CFG.speedVal = v if CFG.speedBoost then hum.WalkSpeed = v end end)
CreateToggle(SecMove, "Super Jump", false, function(s) setJump(s) end)
CreateSlider(SecMove, "Jump Value", 50, 400, 150, function(v) CFG.jumpVal = v if CFG.superJump then hum.JumpPower = v end end)
CreateToggle(SecMove, "Infinite Jump", false, function(s) CFG.infiniteJump = s end)
CreateToggle(SecMove, "Fly", false, function(s) setFly(s) end)
CreateSlider(SecMove, "Fly Speed", 10, 200, 60, function(v) CFG.flySpeed = v end)
CreateButton(SecMove, "Dash (Q)", Theme.SectionBG, function() doDash() end)

local SubProt = CreateSubTab(TabBoost, "Protections")
local SecProt = CreateSection(SubProt, "Safety & Passives")
CreateToggle(SecProt, "Noclip", true, function(s) CFG.noclip = s end)
CreateToggle(SecProt, "Invisible (Visible Ghost)", false, function(s) setInvisible(s) end)
CreateToggle(SecProt, "Anti-Void", false, function(s) setAntiVoid(s) end)

-- TAB: FARM
local TabFarm = CreateMainTab("Farm")
local SubAutoWin = CreateSubTab(TabFarm, "Auto Win")
local SecAutoWin = CreateSection(SubAutoWin, "Win Automations")
CreateToggle(SecAutoWin, "Enable Auto Win (Ragdoll)", false, function(s) CFG.autoWin = s end)
CreateSlider(SecAutoWin, "Interval (Minutes)", 1, 10, 2, function(v) CFG.autoWinInterval = v end)
CreateButton(SecAutoWin, "Teleport to Finish Now", Theme.SectionBG, function() tpToFinishes() end)

-- TAB: VISUALS
local TabVisuals = CreateMainTab("Visuals")
local SubVis = CreateSubTab(TabVisuals, "Client Mods")
local SecVis = CreateSection(SubVis, "Visual Enhancements")
CreateToggle(SecVis, "Spin Bot", false, function(s) setSpin(s) end)
CreateSlider(SecVis, "Spin Speed", 10, 300, 35, function(v) CFG.spinSpeed = v end)
CreateToggle(SecVis, "Player ESP", false, function(s) CFG.playerEsp = s end)
CreateToggle(SecVis, "Freecam (WASD + Q/E)", false, function(s) setFreecam(s) end)
CreateSlider(SecVis, "Freecam Speed", 1, 10, 1, function(v) CFG.freecamSpeed = v end)

-- TAB: CHECKPOINTS
local TabCheck = CreateMainTab("Checkpoints")
local SubChk = CreateSubTab(TabCheck, "Savepoints")
local SecChk = CreateSection(SubChk, "Checkpoint System")

CreateButton(SecChk, "Save Checkpoint (C)", Theme.SectionBG, function()
    if root then
        savedCheckpoints[activeCheckpointSlot] = root.CFrame
        SendNotification("Saved!", "Checkpoint slot " .. activeCheckpointSlot .. " saved.", 1.5)
    end
end)
CreateButton(SecChk, "Teleport to Checkpoint (V)", Theme.SectionBG, function()
    if savedCheckpoints[activeCheckpointSlot] and root then
        root.CFrame = savedCheckpoints[activeCheckpointSlot]
    else
        SendNotification("Error", "No checkpoint saved in slot " .. activeCheckpointSlot, 1.5)
    end
end)
CreateSlider(SecChk, "Active Slot (1-5)", 1, 5, 1, function(v) activeCheckpointSlot = v end)

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.C then
        if root then
            savedCheckpoints[activeCheckpointSlot] = root.CFrame
            SendNotification("Saved!", "Slot " .. activeCheckpointSlot .. " updated.", 1.5)
        end
    elseif input.KeyCode == Enum.KeyCode.V then
        if savedCheckpoints[activeCheckpointSlot] and root then
            root.CFrame = savedCheckpoints[activeCheckpointSlot]
        end
    end
end)

-- TAB: TROLL (Copy Skin & History + Visual Name Changer)
local TabTroll = CreateMainTab("Troll")
local SubTrl = CreateSubTab(TabTroll, "Copy & Name")
local SecTrl = CreateSection(SubTrl, "Troll Suite")

local function applySkinByUsername(name)
    pcall(function()
        local targetPlr = Players:FindFirstChild(name)
        if targetPlr then
            local userId = targetPlr.UserId
            local desc = Players:GetHumanoidDescriptionFromUserId(userId)
            if desc and hum then
                hum:ApplyDescriptionReset(desc)
                -- Add to history (max 5)
                if not table.find(skinHistory, name) then
                    table.insert(skinHistory, 1, name)
                    if #skinHistory > 5 then table.remove(skinHistory, 6) end
                end
            end
        else
            SendNotification("Error", "Player not found!", 2)
        end
    end)
end

CreateInput(SecTrl, "Copy Skin (Username)...", function(name)
    applySkinByUsername(name)
end)

local SecHistory = CreateSection(SubTrl, "Skin History (Last 5)")
local historyContainer = Instance.new("Frame", SecHistory)
historyContainer.Size = UDim2.new(0.92, 0, 0, 110)
historyContainer.BackgroundTransparency = 1
local histLayout = Instance.new("UIListLayout", historyContainer)
histLayout.Padding = UDim.new(0, 4)

local function refreshSkinHistory()
    pcall(function()
        for _, c in ipairs(historyContainer:GetChildren()) do
            if c:IsA("TextButton") then c:Destroy() end
        end
        for i, username in ipairs(skinHistory) do
            local btn = Instance.new("TextButton", historyContainer)
            btn.Size = UDim2.new(1, 0, 0, 24)
            btn.BackgroundColor3 = Theme.Sidebar
            btn.TextColor3 = Theme.Text
            btn.Text = "Skin: " .. username
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 11
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
            btn.MouseButton1Click:Connect(function()
                applySkinByUsername(username)
                SendNotification("Skin Applied", "Loaded skin of " .. username, 1.5)
            end)
        end
    end)
end

-- Refresh history list periodically
task.spawn(function()
    while task.wait(2) do
        refreshSkinHistory()
    end
end)

CreateInput(SecTrl, "Name Changer (Visual Name)...", function(v)
    CFG.fakeName = v
    pcall(function()
        local head = char and char:FindFirstChild("Head")
        if head then
            local bg = head:FindFirstChild("RyuFakeName")
            if not bg and v ~= "" then
                bg = Instance.new("BillboardGui", head)
                bg.Name = "RyuFakeName"; bg.Size = UDim2.new(0, 200, 0, 50); bg.StudsOffset = Vector3.new(0, 2.5, 0); bg.AlwaysOnTop = true
                local tl = Instance.new("TextLabel", bg)
                tl.Size = UDim2.new(1,0,1,0); tl.BackgroundTransparency = 1; tl.Text = v; tl.TextColor3 = Color3.new(1,1,1); tl.TextStrokeTransparency = 0; tl.Font = Enum.Font.GothamBold; tl.TextSize = 14
            elseif bg then
                if v == "" then bg:Destroy() else bg.TextLabel.Text = v end
            end
        end
    end)
end)

-- TAB: SETTINGS
local TabSettings = CreateMainTab("Settings")
local SubSet = CreateSubTab(TabSettings, "Settings")
local SecSet = CreateSection(SubSet, "System Configuration")

CreateToggle(SecSet, "Anti-AFK Protection", false, function(v)
    CFG.antiAfk = v
    if v then
        LocalPlayer.Idled:Connect(function()
            if CFG.antiAfk and VirtualUser then
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.new())
            end
        end)
    end
end)

CreateLabel(SecSet, "Join Discord.gg/ryuhub to suggest more functions, scripts and more!")
CreateButton(SecSet, "Reset UI / Clean", Theme.SectionBG, function() RyuHub:Destroy() end)

--// INITIALIZE FIRST TAB
pcall(function() 
    if Tabs[1] and Tabs[1].Toggle then Tabs[1].Toggle() end
    if Tabs[1] and Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Open then Tabs[1].SubTabs[1].Open() end 
end)

print("[Ryu Hub] Tower of Hell Suite v5.1 successfully updated.")

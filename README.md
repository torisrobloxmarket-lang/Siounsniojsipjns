--// ==========================================
--// RYU HUB - TOWER OF HELL SUITE v7.2
--// MONOCHROME - LIVE SKIN APPLY & SAFE TP
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
local VirtualInputManager = game:GetService("VirtualInputManager")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local lp = LocalPlayer
local char = lp.Character or lp.CharacterAdded:Wait()
local root, hum
local lastSkinUsername = ""

-- Forward declaration
local applyGlobalSkin

local function bindChar(c)
    char = c
    root = c:WaitForChild("HumanoidRootPart", 5)
    hum  = c:WaitForChild("Humanoid", 5)
    
    -- Reapply perfect skin upon respawn if active
    if lastSkinUsername ~= "" then
        task.spawn(function()
            task.wait(0.5) -- Wait for character to fully build
            applyGlobalSkin(lastSkinUsername, true)
        end)
    end
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
    godmode         = true, 
    spin            = false,
    spinSpeed       = 35,
    playerEsp       = false,
    autoWin         = false,
    autoWinInterval = 2,
    freecam         = false,
    freecamSpeed    = 1,
    antiAfk         = false,
    fakeName        = "",
    FakeNameColor   = "White",
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
if not guiParent then pcall(function() guiParent = LocalPlayer:WaitForChild("PlayerGui") end) end
if not guiParent then guiParent = CoreGui end

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
        pcall(function()
            TweenService:Create(f, TweenInfo.new(0.3), {BackgroundTransparency = 1, Size = UDim2.new(1,0,0,0)}):Play()
            task.wait(0.3)
            f:Destroy()
        end)
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
        DiscordWatermark.Position = UDim2.new(0.5, -150, 0.5, MainFrame.Position.Y.Offset - 35)
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
    local tabObj = { Btn = nil, Arrow = nil, SubContainer = nil, SubLayout = nil, IsOpen = false, SubTabs = {} }
    
    local tabBtn = Instance.new("TextButton", Sidebar)
    tabBtn.Size = UDim2.new(1, 0, 0, 36); tabBtn.BackgroundColor3 = Theme.Sidebar; tabBtn.Text = "  " .. string.upper(name)
    tabBtn.TextColor3 = Theme.SubText; tabBtn.Font = Enum.Font.GothamBlack; tabBtn.TextSize = 13; tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8)
    tabObj.Btn = tabBtn

    local arrow = Instance.new("TextLabel", tabBtn)
    arrow.Size = UDim2.new(0, 20, 1, 0); arrow.Position = UDim2.new(1, -25, 0, 0); arrow.BackgroundTransparency = 1; arrow.Text = "v"
    arrow.TextColor3 = Theme.SubText; arrow.Font = Enum.Font.GothamBold; arrow.TextSize = 12
    tabObj.Arrow = arrow

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
    
    tBtn.MouseButton1Click:Connect(function()
        defaultState = not defaultState
        pcall(function()
            TweenService:Create(tBtn, TweenInfo.new(0.2), {BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff}):Play()
            TweenService:Create(circle, TweenInfo.new(0.2), {Position = defaultState and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = defaultState and Theme.Background or Color3.fromRGB(150, 150, 150)}):Play()
            label.TextColor3 = defaultState and Theme.Text or Theme.SubText
        end)
        if callback then pcall(function() callback(defaultState) end) end
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

local function CreateDropdown(section, headerText, itemsList, targetConfigKey, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 160); frame.BackgroundTransparency = 1
    local header = Instance.new("TextLabel", frame)
    header.Size = UDim2.new(1, 0, 0, 20); header.BackgroundTransparency = 1; header.Text = headerText .. ": " .. tostring(CFG[targetConfigKey] or "None")
    header.TextColor3 = Theme.SubText; header.Font = Enum.Font.GothamMedium; header.TextSize = 12; header.TextXAlignment = Enum.TextXAlignment.Left
    
    local scroll = Instance.new("ScrollingFrame", frame)
    scroll.Size = UDim2.new(1, 0, 0, 130); scroll.Position = UDim2.new(0, 0, 0, 25); scroll.BackgroundColor3 = Theme.Background; scroll.ScrollBarThickness = 4
    Instance.new("UICorner", scroll).CornerRadius = UDim.new(0, 6)
    local listLayout = Instance.new("UIListLayout", scroll)
    listLayout.Padding = UDim.new(0, 4); listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    local function populate(list)
        for _, child in pairs(scroll:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        for _, itemName in ipairs(list) do
            local btn = Instance.new("TextButton", scroll)
            btn.Size = UDim2.new(0.94, 0, 0, 26); btn.BackgroundColor3 = Theme.SectionBG; btn.Text = "  " .. tostring(itemName)
            btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; btn.TextXAlignment = Enum.TextXAlignment.Left
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
            btn.MouseButton1Click:Connect(function() 
                CFG[targetConfigKey] = itemName; header.Text = headerText .. ": " .. tostring(itemName)
                if callback then callback(itemName) end
            end)
        end
        task.defer(function() scroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 10) end)
    end
    listLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() scroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 10) end)
    populate(itemsList)
    return { Refresh = populate }
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
--// TABS & SECTIONS UI
--// ==========================================

local TabBoost = CreateMainTab("Boost")
local SubMove = CreateSubTab(TabBoost, "Speed & Jump")
local SecMove = CreateSection(SubMove, "Movement Adjustments")
CreateToggle(SecMove, "Speed Boost", false, function(s) CFG.speedBoost = s end)
CreateSlider(SecMove, "Speed Value", 16, 200, 48, function(v) CFG.speedVal = v end)
CreateToggle(SecMove, "Super Jump", false, function(s) CFG.superJump = s end)
CreateSlider(SecMove, "Jump Value", 50, 400, 150, function(v) CFG.jumpVal = v end)
CreateToggle(SecMove, "Fly", false, function(s) CFG.fly = s end)
CreateSlider(SecMove, "Fly Speed", 10, 200, 60, function(v) CFG.flySpeed = v end)
CreateToggle(SecMove, "Infinite Jump", false, function(s) CFG.infiniteJump = s end)
CreateButton(SecMove, "Dash (Q)", Theme.SectionBG, function()
    pcall(function()
        local c = LocalPlayer.Character
        local hrp = c and c:FindFirstChild("HumanoidRootPart")
        if hrp then
            local dir = (camera.CFrame.LookVector * Vector3.new(1,0,1)).Unit
            local bv = Instance.new("BodyVelocity", hrp)
            bv.MaxForce = Vector3.new(1e5, 0, 1e5); bv.Velocity = dir * CFG.dashSpeed
            task.delay(0.18, function() if bv then bv:Destroy() end end)
        end
    end)
end)

local SubProt = CreateSubTab(TabBoost, "Safety & Passives")
local SecProt = CreateSection(SubProt, "Protections")
CreateToggle(SecProt, "God Mode", "Permanent health refill", true, function(s) CFG.godmode = s end)
CreateToggle(SecProt, "Noclip", true, function(s) CFG.noclip = s end)
CreateToggle(SecProt, "Invisible (Local transparency)", false, function(s) CFG.invisible = s end)
CreateToggle(SecProt, "Anti-Void", false, function(s) CFG.antiVoid = s end)

local TabFarm = CreateMainTab("Auto Win")
local SubAutoWin = CreateSubTab(TabFarm, "Automation")
local SecAutoWin = CreateSection(SubAutoWin, "Win Settings")
CreateToggle(SecAutoWin, "Auto Win (Highest Point)", false, function(s) CFG.autoWin = s end)
CreateSlider(SecAutoWin, "Interval (Minutes)", 1, 10, 2, function(v) CFG.autoWinInterval = v end)

local function tpToHighestPoint()
    pcall(function()
        if not root then return end
        local highestY = -math.huge
        local highestPart = nil
        
        for _, obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") and not obj:IsDescendantOf(char) and obj.Transparency < 1 and obj.Position.Y < 15000 then
                if obj.Position.Y > highestY then
                    highestY = obj.Position.Y
                    highestPart = obj
                end
            end
        end
        
        if highestPart then
            root.CFrame = CFrame.new(highestPart.Position.X, highestY + 5, highestPart.Position.Z)
            root.Velocity = Vector3.zero
            
            local plat = Instance.new("Part", Workspace)
            plat.Size = Vector3.new(20, 2, 20)
            plat.Position = Vector3.new(highestPart.Position.X, highestY + 2, highestPart.Position.Z)
            plat.Anchored = true
            plat.Transparency = 0.5
            plat.Color = Color3.fromRGB(0, 255, 0)
            task.delay(2.5, function() if plat then plat:Destroy() end end)
            
            SendNotification("Auto Win", "Teleported to highest point!", 2)
        else
            root.CFrame = root.CFrame + Vector3.new(0, 350, 0)
            root.Velocity = Vector3.zero
            SendNotification("Auto Win", "Teleported 350 studs up!", 2)
        end
    end)
end

CreateButton(SecAutoWin, "Teleport to Highest Point Now", Theme.SectionBG, function() tpToHighestPoint() end)
CreateButton(SecAutoWin, "Rejoin Low Server", Theme.SectionBG, function()
    pcall(function()
        local servers = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Desc&limit=100"))
        for _, s in ipairs(servers.data) do
            if s.playing < s.maxPlayers and s.id ~= game.JobId then
                TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id, LocalPlayer)
                break
            end
        end
    end)
end)

local TabVisuals = CreateMainTab("Visuals")
local SubVis = CreateSubTab(TabVisuals, "Client Mods")
local SecVis = CreateSection(SubVis, "Visual Enhancements")
CreateToggle(SecVis, "Spin", false, function(s) CFG.spin = s end)
CreateSlider(SecVis, "Spin Speed", 10, 300, 35, function(v) CFG.spinSpeed = v end)
CreateToggle(SecVis, "Player ESP", false, function(s) CFG.playerEsp = s end)
CreateToggle(SecVis, "Freecam (WASD + Q/E)", false, function(s) CFG.freecam = s end)
CreateSlider(SecVis, "Freecam Speed", 1, 10, 1, function(v) CFG.freecamSpeed = v end)

local TabCheck = CreateMainTab("Checkpoints")
local SubChk = CreateSubTab(TabCheck, "Savepoints")
local SecChk = CreateSection(SubChk, "Checkpoint System")
local SaveLog = CreateLabel(SecChk, "Ready to save.")

CreateButton(SecChk, "Save Checkpoint (C)", Theme.SectionBG, function()
    pcall(function()
        if root then
            savedCheckpoints[activeCheckpointSlot] = root.CFrame
            SaveLog.Text = "Saved at slot " .. activeCheckpointSlot .. "!"
            task.delay(2, function() SaveLog.Text = "Ready to save." end)
        end
    end)
end)
CreateButton(SecChk, "Teleport to Checkpoint (V)", Theme.SectionBG, function()
    pcall(function()
        if savedCheckpoints[activeCheckpointSlot] and root then
            root.CFrame = savedCheckpoints[activeCheckpointSlot]
            root.Velocity = Vector3.zero
        else
            SendNotification("Error", "No checkpoint saved in slot " .. activeCheckpointSlot, 1.5)
        end
    end)
end)
CreateSlider(SecChk, "Active Slot (1-5)", 1, 5, 1, function(v) activeCheckpointSlot = v end)

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    pcall(function()
        if input.KeyCode == Enum.KeyCode.C then
            if root then
                savedCheckpoints[activeCheckpointSlot] = root.CFrame
                SaveLog.Text = "Saved at slot " .. activeCheckpointSlot .. "!"
                task.delay(2, function() SaveLog.Text = "Ready to save." end)
            end
        elseif input.KeyCode == Enum.KeyCode.V then
            if savedCheckpoints[activeCheckpointSlot] and root then
                root.CFrame = savedCheckpoints[activeCheckpointSlot]
                root.Velocity = Vector3.zero
            end
        end
    end)
end)

local TabTroll = CreateMainTab("Troll")
local SubTrl = CreateSubTab(TabTroll, "Copy & Name")
local SecTrl = CreateSection(SubTrl, "Troll Suite")

applyGlobalSkin = function(username, isRespawn)
    task.spawn(function()
        local userId
        local s1, _ = pcall(function() userId = Players:GetUserIdFromNameAsync(username) end)
        if s1 and userId then
            lastSkinUsername = username
            local c = LocalPlayer.Character
            local h = c and c:FindFirstChild("Humanoid")
            
            if c and h then
                -- Direct Live Apply without killing
                local descApplied = false
                pcall(function()
                    local desc = Players:GetHumanoidDescriptionFromUserId(userId)
                    if desc then 
                        h:ApplyDescription(desc) 
                        descApplied = true
                    end
                end)
                
                -- Fallback manual bypass if Roblox blocks local ApplyDescription
                if not descApplied then
                    local dummy
                    pcall(function() dummy = Players:CreateHumanoidModelFromUserId(userId) end)
                    if dummy then
                        for _, v in pairs(c:GetChildren()) do
                            if v:IsA("Accessory") or v:IsA("Shirt") or v:IsA("Pants") or v:IsA("ShirtGraphic") or v:IsA("CharacterMesh") or v:IsA("BodyColors") then
                                v:Destroy()
                            end
                        end
                        for _, v in pairs(dummy:GetChildren()) do
                            if v:IsA("Accessory") or v:IsA("Shirt") or v:IsA("Pants") or v:IsA("ShirtGraphic") or v:IsA("CharacterMesh") or v:IsA("BodyColors") then
                                v:Clone().Parent = c
                            end
                            if v:IsA("MeshPart") then
                                local myPart = c:FindFirstChild(v.Name)
                                if myPart and myPart:IsA("MeshPart") then
                                    pcall(function()
                                        myPart.MeshId = v.MeshId
                                        myPart.TextureID = v.TextureID
                                    end)
                                end
                            elseif v.Name == "Head" then
                                local myHead = c:FindFirstChild("Head")
                                if myHead then
                                    local dMesh = v:FindFirstChildOfClass("SpecialMesh")
                                    local mMesh = myHead:FindFirstChildOfClass("SpecialMesh")
                                    if dMesh and mMesh then
                                        mMesh.MeshId = dMesh.MeshId
                                        mMesh.TextureId = dMesh.TextureId
                                        mMesh.Scale = dMesh.Scale
                                    elseif dMesh and not mMesh then
                                        dMesh:Clone().Parent = myHead
                                    end
                                    local dFace = v:FindFirstChildOfClass("Decal")
                                    local mFace = myHead:FindFirstChildOfClass("Decal")
                                    if dFace then
                                        if mFace then mFace.Texture = dFace.Texture else dFace:Clone().Parent = myHead end
                                    end
                                end
                            end
                        end
                        dummy:Destroy()
                    end
                end
                
                if not isRespawn then
                    if not table.find(skinHistory, username) then
                        table.insert(skinHistory, 1, username)
                        if #skinHistory > 5 then table.remove(skinHistory, 6) end
                    end
                    SendNotification("Success", "Loaded global skin: " .. username, 2)
                end
            end
        else
            if not isRespawn then SendNotification("Error", "Player does not exist!", 2) end
        end
    end)
end

CreateInput(SecTrl, "Global Copy Skin (Username)...", function(name)
    applyGlobalSkin(name)
end)

local SecHistory = CreateSection(SubTrl, "Skin History (Last 5)")
local historyContainer = Instance.new("Frame", SecHistory)
historyContainer.Size = UDim2.new(0.92, 0, 0, 140)
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
            btn.Size = UDim2.new(1, 0, 0, 26)
            btn.BackgroundColor3 = Theme.Sidebar
            btn.TextColor3 = Theme.Text
            btn.Text = "Skin: " .. username
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 11
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
            btn.MouseButton1Click:Connect(function()
                applyGlobalSkin(username)
            end)
        end
    end)
end

task.spawn(function()
    while task.wait(2) do refreshSkinHistory() end
end)

CreateInput(SecTrl, "Name Changer (Visual Name)...", function(v) CFG.fakeName = v end)
CreateDropdown(SecTrl, "Name Color", {"White", "Red", "Blue", "Green", "Yellow", "Rainbow", "Neon Blue"}, "FakeNameColor", function(v) CFG.FakeNameColor = v end)

local TabSettings = CreateMainTab("Settings")
local SubSet = CreateSubTab(TabSettings, "Settings")
local SecSet = CreateSection(SubSet, "System Configuration")

CreateToggle(SecSet, "Anti-AFK Protection", false, function(v)
    CFG.antiAfk = v
    if v then
        LocalPlayer.Idled:Connect(function()
            pcall(function()
                if CFG.antiAfk and VirtualUser then
                    VirtualUser:CaptureController()
                    VirtualUser:ClickButton2(Vector2.new())
                end
            end)
        end)
    end
end)

CreateLabel(SecSet, "Join Discord.gg/ryuhub to suggest more functions, scripts and more!")
CreateButton(SecSet, "Reset Settings / Clean UI", Theme.Warning, function() pcall(function() RyuHub:Destroy() end) end)

--// INITIALIZE FIRST TAB
pcall(function() 
    if Tabs[1] and Tabs[1].Toggle then Tabs[1].Toggle() end
    if Tabs[1] and Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Open then Tabs[1].SubTabs[1].Open() end 
end)

--// 6. BACKGROUND PHYSICS & CONTINUOUS LOOPS
local flyBV
RunService.Stepped:Connect(function()
    pcall(function()
        local c = LocalPlayer.Character
        local hrp = c and c:FindFirstChild("HumanoidRootPart")
        local hum = c and c:FindFirstChildOfClass("Humanoid")
        
        if not c or not hrp or not hum then return end
        
        if CFG.godmode then hum.Health = hum.MaxHealth end
        
        if CFG.noclip then
            for _, p in ipairs(c:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
        
        if CFG.speedBoost then hum.WalkSpeed = CFG.speedVal else if hum.WalkSpeed > 30 then hum.WalkSpeed = 16 end end
        if CFG.superJump then hum.JumpPower = CFG.jumpVal else if hum.JumpPower > 60 then hum.JumpPower = 50 end end
        
        if CFG.invisible then
            for _, p in ipairs(c:GetDescendants()) do
                if p:IsA("BasePart") or p:IsA("Decal") then
                    if p.Name ~= "HumanoidRootPart" then p.Transparency = 0.3 end
                end
            end
        end
        
        if CFG.spin then
            hrp.CFrame = hrp.CFrame * CFrame.Angles(0, math.rad(CFG.spinSpeed), 0)
        end
        
        if CFG.antiVoid then
            if hrp.Position.Y < CFG.antiVoidY then
                hrp.CFrame = CFrame.new(hrp.Position.X, CFG.antiVoidY + 25, hrp.Position.Z)
                hrp.Velocity = Vector3.zero
            end
        end
        
        if CFG.autoWin and hrp.Velocity.Y < -50 then
            local ray = Workspace:Raycast(hrp.Position, Vector3.new(0, -1000, 0))
            if ray and ray.Instance and ray.Instance.CanCollide then
                hrp.CFrame = CFrame.new(ray.Position + Vector3.new(0, 4, 0))
                hrp.Velocity = Vector3.zero
            end
        end

        if CFG.fly then
            if not flyBV then
                flyBV = Instance.new("BodyVelocity", hrp)
                flyBV.MaxForce = Vector3.new(1e9,1e9,1e9)
            end
            hum.PlatformStand = true
            for _, t in pairs(hum:GetPlayingAnimationTracks()) do t:Stop() end
            
            local dir = Vector3.zero
            local cf = camera.CFrame
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then dir = dir + cf.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then dir = dir - cf.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then dir = dir - cf.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then dir = dir + cf.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then dir = dir - Vector3.new(0,1,0) end
            
            if dir.Magnitude > 0 then flyBV.Velocity = dir.Unit * CFG.flySpeed else flyBV.Velocity = Vector3.zero end
        else
            if flyBV then flyBV:Destroy(); flyBV = nil end
            if hum.PlatformStand then hum.PlatformStand = false end
        end
    end)
end)

-- FREECAM & ESP & VISUAL NAME LOOP
local freecamPos = Vector3.zero
RunService.RenderStepped:Connect(function(dt)
    pcall(function()
        if CFG.freecam then
            if camera.CameraType ~= Enum.CameraType.Scriptable then
                camera.CameraType = Enum.CameraType.Scriptable
                freecamPos = camera.CFrame.Position
            end
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
        else
            if camera.CameraType == Enum.CameraType.Scriptable then camera.CameraType = Enum.CameraType.Custom end
        end
        
        if CFG.playerEsp then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    local h = p.Character:FindFirstChild("Highlight") or Instance.new("Highlight", p.Character)
                    h.FillColor = Color3.fromRGB(255, 255, 255)
                end
            end
        else
            for _, p in pairs(Players:GetPlayers()) do
                if p.Character then
                    local h = p.Character:FindFirstChild("Highlight")
                    if h then h:Destroy() end
                end
            end
        end
        
        local c = LocalPlayer.Character
        if CFG.fakeName ~= "" and c and c:FindFirstChild("Head") then
            local head = c.Head
            local bg = head:FindFirstChild("RyuFakeName")
            if not bg then
                bg = Instance.new("BillboardGui", head)
                bg.Name = "RyuFakeName"; bg.Size = UDim2.new(0, 200, 0, 50); bg.StudsOffset = Vector3.new(0, 2.5, 0); bg.AlwaysOnTop = true
                local tl = Instance.new("TextLabel", bg)
                tl.Size = UDim2.new(1,0,1,0); tl.BackgroundTransparency = 1; tl.TextStrokeTransparency = 0; tl.Font = Enum.Font.GothamBold; tl.TextSize = 14
            end
            local lbl = bg:FindFirstChildOfClass("TextLabel")
            if lbl then
                lbl.Text = CFG.fakeName
                if CFG.FakeNameColor == "Rainbow" then lbl.TextColor3 = Color3.fromHSV((tick()%5)/5, 1, 1)
                elseif CFG.FakeNameColor == "Red" then lbl.TextColor3 = Color3.fromRGB(255, 50, 50)
                elseif CFG.FakeNameColor == "Blue" then lbl.TextColor3 = Color3.fromRGB(50, 100, 255)
                elseif CFG.FakeNameColor == "Green" then lbl.TextColor3 = Color3.fromRGB(50, 255, 50)
                elseif CFG.FakeNameColor == "Yellow" then lbl.TextColor3 = Color3.fromRGB(255, 255, 50)
                elseif CFG.FakeNameColor == "Neon Blue" then lbl.TextColor3 = Color3.fromRGB(0, 255, 255)
                else lbl.TextColor3 = Color3.fromRGB(255, 255, 255) end
            end
        else
            if c and c:FindFirstChild("Head") and c.Head:FindFirstChild("RyuFakeName") then c.Head.RyuFakeName:Destroy() end
        end
    end)
end)

task.spawn(function()
    while task.wait(1) do
        pcall(function()
            if CFG.autoWin then
                tpToHighestPoint()
                task.wait(CFG.autoWinInterval * 60)
            end
        end)
    end
end)

print("[Ryu Hub] ToH Suite v7.2 Successfully Loaded.")

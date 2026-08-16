--// ==========================================
--// RYU HUB - TOWER OF HELL EDITION (TJS) v4.1
--// FULL SUITE + ADVANCED FEATURES & TROLLS
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")
local Chat = game:GetService("Chat")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local cam = camera
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
    -- New requested features
    flySpeed        = 50,
    invincible      = false,
    floatSpeed      = 50,
    floatAscend     = 50,
    floatDescend    = 50,
    noclip          = true,
    invisible       = false,
    infiniteJump    = false,
    fallProtect     = false,
    jumpEsp         = false,
    playerEsp       = false,
    autoFarm        = false,
    autoFarmInterval= 1, -- minutes
    customJump      = 50,
    freecam         = false,
    freecamSpeed    = 1,
    
    -- Original / Troll features
    godmode       = false,
    fly           = false,
    dashSpeed     = 120,
    dashCooldown  = 0.6,
    elevStep      = 60,
    tpStepCap     = 100,
    farmDelay     = 0.04,
    farmStep      = 10,
    speedMult     = 3,
    jumpMult      = 3,
    antiVoid      = false,
    antiVoidY     = -50,
    flingForce    = 9e4,
    lowGravity    = false,
    defaultGrav   = 196.2,
    lowGravVal    = 40,
    freeze        = false,
    spinBot       = false,
    spinSpeed     = 10,
    reachMult     = 10,
    defaultSpeed  = 16,
    defaultJump   = 50,
    
    trollFollow       = false,
    trollChat         = false,
    trollCamLock      = false,
    trollFakeError    = false,
    trollGhostMode    = false,
    trollFloor        = false,
    trollChatSpam     = false,
    trollCopycat      = false,
    trollFakeAdmin    = false,
    trollSizeGrow     = false,
    trollSizeShrink   = false,
    trollHeadSize     = false,
    trollBobble       = false,
    trollShake        = false,
    trollYeet         = false,
}

local selectedTarget = nil
local savedCheckpoint = nil
local checkpointGhost = nil
local checkpointLabel = nil
local espHolders = {}

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
    if v.Name == "RyuHubToH" then v:Destroy() end 
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

-- MAIN WINDOW
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

local DragText = Instance.new("TextLabel", MainFrame)
DragText.Size = UDim2.new(1, 0, 1, 0); DragText.Position = UDim2.new(0, 0, 0, 0); DragText.BackgroundTransparency = 1
DragText.Text = "DISCORD.GG/RYUHUB"; DragText.Font = Enum.Font.GothamBlack; DragText.TextSize = 50
DragText.TextColor3 = Theme.Accent; DragText.TextTransparency = 0.95; DragText.ZIndex = 0

ToggleBtn.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end)
        task.delay(0.3, function() MainFrame.Visible = false end)
    else
        MainFrame.Visible = true
        pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = MainSize, Position = UDim2.new(0.5, -MainSize.X.Offset/2, 0.5, -MainSize.Y.Offset/2)}):Play() end)
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

-- CLOSE BUTTON (WHITE MONOCHROME)
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
            local value = math.floor(min + (max - min) * relative * 10) / 10
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

--// ==========================================
--// INTEGRATED TOWER OF HELL ENGINE
--// ==========================================

-- NOCLIP
RunService.Stepped:Connect(function()
    if not CFG.noclip or not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") then p.CanCollide = false end
    end
end)

-- GODMODE / INVINCIBILITY
local godConn
local function setGodmode(on)
    CFG.godmode = on
    CFG.invincible = on
    if godConn then godConn:Disconnect() godConn = nil end
    if on then
        godConn = RunService.Heartbeat:Connect(function()
            if hum then hum.Health = hum.MaxHealth end
        end)
    end
end

-- FLY WITH ADJUSTABLE SPEED
local flyConn
local flyBV, flyBA
local flyKeys = {f=false,b=false,l=false,r=false,up=false,down=false}

local function cleanFlyBodies()
    if flyBV and flyBV.Parent then flyBV:Destroy() end
    if flyBA and flyBA.Parent then flyBA:Destroy() end
end

local function setFly(on)
    CFG.fly = on
    cleanFlyBodies()
    if flyConn then flyConn:Disconnect() flyConn = nil end
    if not on then
        if hum then hum.PlatformStand = false end
        return
    end
    hum.PlatformStand = true
    flyBV = Instance.new("BodyVelocity", root)
    flyBV.MaxForce = Vector3.new(1e5,1e5,1e5)
    flyBV.Velocity  = Vector3.zero
    flyBA = Instance.new("BodyAngularVelocity", root)
    flyBA.MaxTorque = Vector3.new(1e5,1e5,1e5)
    flyBA.AngularVelocity = Vector3.zero
    flyConn = RunService.Heartbeat:Connect(function()
        if not CFG.fly or not root then return end
        local cf  = camera.CFrame
        local dir = Vector3.zero
        if flyKeys.f    then dir = dir + cf.LookVector        end
        if flyKeys.b    then dir = dir - cf.LookVector        end
        if flyKeys.r    then dir = dir + cf.RightVector       end
        if flyKeys.l    then dir = dir - cf.RightVector       end
        if flyKeys.up   then dir = dir + Vector3.new(0,1,0)   end
        if flyKeys.down then dir = dir - Vector3.new(0,1,0)   end
        flyBV.Velocity = dir.Magnitude > 0 and dir.Unit * CFG.flySpeed or Vector3.zero
    end)
end

UserInputService.InputBegan:Connect(function(i,gp)
    if gp then return end
    local k = i.KeyCode
    if k == Enum.KeyCode.W           then flyKeys.f    = true end
    if k == Enum.KeyCode.S           then flyKeys.b    = true end
    if k == Enum.KeyCode.A           then flyKeys.l    = true end
    if k == Enum.KeyCode.D           then flyKeys.r    = true end
    if k == Enum.KeyCode.Space       then flyKeys.up   = true end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = true end
end)
UserInputService.InputEnded:Connect(function(i)
    local k = i.KeyCode
    if k == Enum.KeyCode.W           then flyKeys.f    = false end
    if k == Enum.KeyCode.S           then flyKeys.b    = false end
    if k == Enum.KeyCode.A           then flyKeys.l    = false end
    if k == Enum.KeyCode.D           then flyKeys.r    = false end
    if k == Enum.KeyCode.Space       then flyKeys.up   = false end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = false end
end)

-- DASH
local dashReady = true
local function doDash(direction)
    if not dashReady or not root or not hum then return end
    dashReady = false
    local dir = direction or (camera.CFrame.LookVector * Vector3.new(1,0,1)).Unit
    local bv = Instance.new("BodyVelocity", root)
    bv.MaxForce = Vector3.new(1e5,0,1e5)
    bv.Velocity  = dir * CFG.dashSpeed
    task.delay(0.18, function() if bv and bv.Parent then bv:Destroy() end end)
    task.delay(CFG.dashCooldown, function() dashReady = true end)
end

local function doElevator()
    if not root then return end
    root.CFrame = root.CFrame + Vector3.new(0, CFG.elevStep, 0)
end

local function findTowerTop()
    local highY, topPart = -math.huge, nil
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(char) then
            if obj.Position.Y > highY then
                highY = obj.Position.Y
                topPart = obj
            end
        end
    end
    return topPart, highY
end

local function tpStep()
    if not root then return end
    local _, topY = findTowerTop()
    local cur     = root.Position.Y
    local target  = math.min(cur + CFG.tpStepCap, topY + 5)
    root.CFrame   = CFrame.new(root.Position.X, target, root.Position.Z)
end

-- AUTOCLIMB
local climbConn
local function autofarm()
    if climbConn then climbConn:Disconnect() climbConn = nil end
    local _, topY = findTowerTop()
    if not topY then return end
    local target = topY + 5
    climbConn = RunService.Heartbeat:Connect(function()
        if not root or hum.Health <= 0 then climbConn:Disconnect() return end
        if root.Position.Y >= target then climbConn:Disconnect() return end
        root.CFrame = root.CFrame + Vector3.new(0, CFG.farmStep, 0)
        task.wait(CFG.farmDelay)
    end)
end

local function setSpeed(on)
    if hum then hum.WalkSpeed = on and CFG.defaultSpeed * CFG.speedMult or CFG.defaultSpeed end
end

local function setJump(on)
    if hum then hum.JumpPower = on and CFG.defaultJump * CFG.jumpMult or CFG.defaultJump end
end

UserInputService.JumpRequest:Connect(function()
    if CFG.infiniteJump and hum then
        hum:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

local function setLowGrav(on)
    CFG.lowGravity = on
    workspace.Gravity = on and CFG.lowGravVal or CFG.defaultGrav
end

local function setInvisible(on)
    CFG.invisible = on
    if not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") or p:IsA("Decal") then
            p.Transparency = on and 1 or 0
        end
    end
end

local antiVoidConn
local function setAntiVoid(on)
    CFG.antiVoid = on
    if antiVoidConn then antiVoidConn:Disconnect() antiVoidConn = nil end
    if not on then return end
    antiVoidConn = RunService.Heartbeat:Connect(function()
        if not root then return end
        if root.Position.Y < CFG.antiVoidY then
            root.CFrame = CFrame.new(root.Position.X, CFG.antiVoidY + 20, root.Position.Z)
        end
    end)
end

local function setFreeze(on)
    CFG.freeze = on
    if root then root.Anchored = on end
end

local spinConn
local function setSpinBot(on)
    CFG.spinBot = on
    if spinConn then spinConn:Disconnect() spinConn = nil end
    if not on then return end
    spinConn = RunService.Heartbeat:Connect(function()
        if not root then return end
        root.CFrame = root.CFrame * CFG.Angles(0, math.rad(CFG.spinSpeed), 0)
    end)
end

local function tpToWin()
    if not root then return end
    local kws = {"EndZone","Finish","Win","Goal","End","Completed"}
    local target
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            for _, kw in ipairs(kws) do
                if obj.Name:lower():find(kw:lower()) then target = obj break end
            end
        end
        if target then break end
    end
    if target then
        root.CFrame = CFrame.new(target.Position + Vector3.new(0,5,0))
    else
        local _, topY = findTowerTop()
        root.CFrame = CFrame.new(root.Position.X, topY + 5, root.Position.Z)
    end
end

local function setHitbox(on)
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if hrp then
        hrp.Size = on and Vector3.new(4*CFG.reachMult*0.3, 4*CFG.reachMult*0.3, 4*CFG.reachMult*0.3) or Vector3.new(2,2,1)
    end
end

-- CHECKPOINT SYSTEM (C = Set, V = TP)
local function setCheckpoint()
    if not root then return end
    savedCheckpoint = root.CFrame
    if checkpointGhost then checkpointGhost:Destroy() end
    checkpointGhost = char:Clone()
    checkpointGhost.Name = "CheckpointGhost"
    for _, p in ipairs(checkpointGhost:GetDescendants()) do
        if p:IsA("BasePart") then p.Transparency = 0.5 p.Color = Color3.fromRGB(0,255,100) p.CanCollide = false end
    end
    checkpointGhost.Parent = Workspace
    checkpointGhost:SetPrimaryPartCFrame(savedCheckpoint)
end

local function tpToCheckpoint()
    if savedCheckpoint and root then
        root.CFrame = savedCheckpoint
    end
end

local function clearCheckpoint()
    savedCheckpoint = nil
    if checkpointGhost then checkpointGhost:Destroy() checkpointGhost = nil end
end

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.C then
        setCheckpoint()
    elseif input.KeyCode == Enum.KeyCode.V then
        tpToCheckpoint()
    end
end)

-- FREECAM
local freecamConn
local freecamPos = Vector3.zero
local function setFreecam(on)
    CFG.freecam = on
    if freecamConn then freecamConn:Disconnect() freecamConn = nil end
    if on then
        freecamPos = camera.CFrame.Position
        camera.CameraType = Enum.CameraType.Scriptable
        freecamConn = RunService.RenderStepped:Connect(function(dt)
            local speed = CFG.freecamSpeed * 50 * dt
            local cf = camera.CFrame
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then freecamPos = freecamPos + cf.LookVector * speed end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then freecamPos = freecamPos - cf.LookVector * speed end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then freecamPos = freecamPos - cf.RightVector * speed end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then freecamPos = freecamPos + cf.RightVector * speed end
            if UserInputService:IsKeyDown(Enum.KeyCode.E) then freecamPos = freecamPos + Vector3.new(0, 1, 0) * speed end
            if UserInputService:IsKeyDown(Enum.KeyCode.Q) then freecamPos = freecamPos - Vector3.new(0, 1, 0) * speed end
            camera.CFrame = CFrame.new(freecamPos, freecamPos + cf.LookVector)
        end)
    else
        camera.CameraType = Enum.CameraType.Custom
    end
end

-- AUTO FARM (Cycles tower finishes)
task.spawn(function()
    while task.wait(1) do
        if CFG.autoFarm then
            tpToWin()
            task.wait(CFG.autoFarmInterval * 60)
        end
    end
end)

-- FALL PROTECT & FALL ESP
RunService.RenderStepped:Connect(function()
    pcall(function()
        if not char or not root or not hum then return end
        
        -- Fall protect
        if CFG.fallProtect and hum:GetState() == Enum.HumanoidStateType.Freefall then
            if root.Position.Y < (savedCheckpoint and savedCheckpoint.Position.Y or 0) then
                if savedCheckpoint then root.CFrame = savedCheckpoint end
            end
        end
        
        -- Jump / Fall ESP highlighting
        if CFG.jumpEsp and (hum:GetState() == Enum.HumanoidStateType.Freefall or hum:GetState() == Enum.HumanoidStateType.Jumping) then
            for _, p in pairs(char:GetDescendants()) do
                if p:IsA("BasePart") then p.Color = Color3.fromRGB(255, 100, 255) end
            end
        end
        
        -- Player ESP
        if CFG.playerEsp then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= lp and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local h = p.Character:FindFirstChild("Highlight") or Instance.new("Highlight", p.Character)
                    h.FillColor = Color3.fromRGB(255, 255, 255)
                end
            end
        end
    end)
end)


--// ==========================================
--// BUILD TABS & SECTIONS
--// ==========================================

local TabCombat = CreateMainTab("Combat")

-- SubTab: Movement
local SubMove = CreateSubTab(TabCombat, "Movement")
local SecMove = CreateSection(SubMove, "Speed & Boosts")
CreateToggle(SecMove, "Speed Boost (x3)", false, function(s) setSpeed(s) end)
CreateToggle(SecMove, "Super Jump (x3)", false, function(s) setJump(s) end)
CreateSlider(SecMove, "Custom Jump Height", 0, 500, 50, function(v) CFG.customJump = v if hum then hum.JumpPower = v end end)
CreateToggle(SecMove, "Infinite Jump", false, function(s) CFG.infiniteJump = s end)
CreateToggle(SecMove, "Low Gravity", false, function(s) setLowGrav(s) end)
CreateToggle(SecMove, "Fly", false, function(s) setFly(s) end)
CreateSlider(SecMove, "Fly Speed", 1, 200, 60, function(v) CFG.flySpeed = v end)
CreateButton(SecMove, "Dash (Q)", Theme.SectionBG, function() doDash() end)

-- SubTab: Teleport / Tower
local SubTp = CreateSubTab(TabCombat, "Tower TP")
local SecTp = CreateSection(SubTp, "Vertical Progression")
CreateButton(SecTp, "Elevator (+60 Studs)", Theme.SectionBG, function() doElevator() end)
CreateButton(SecTp, "TP +100 Studs Step", Theme.SectionBG, function() tpStep() end)
CreateButton(SecTp, "TP to Win Zone", Theme.SectionBG, function() tpToWin() end)
CreateButton(SecTp, "Auto Climb Tower", Theme.SectionBG, function() autofarm() end)
CreateToggle(SecTp, "Auto Farm (Cycle Finishes)", false, function(s) CFG.autoFarm = s end)
CreateSlider(SecTp, "Auto Farm Interval (Min)", 0, 8, 1, function(v) CFG.autoFarmInterval = v end)

-- SubTab: Checkpoints
local SubChk = CreateSubTab(TabCombat, "Checkpoints")
local SecChk = CreateSection(SubChk, "Checkpoint System")
CreateButton(SecChk, "Set Checkpoint (Key: C)", Theme.SectionBG, function() setCheckpoint() end)
CreateButton(SecChk, "Teleport to Checkpoint (Key: V)", Theme.SectionBG, function() tpToCheckpoint() end)
CreateButton(SecChk, "Clear Checkpoint", Theme.SectionBG, function() clearCheckpoint() end)

-- SubTab: Freecam
local SubCam = CreateSubTab(TabCombat, "Freecam")
local SecCam = CreateSection(SubCam, "Free Camera Engine")
CreateToggle(SecCam, "Enable Freecam (WASD + Q/E)", false, function(s) setFreecam(s) end)
CreateSlider(SecCam, "Freecam Speed", 1, 10, 1, function(v) CFG.freecamSpeed = v end)

-- TAB: FARM (Protections)
local TabFarm = CreateMainTab("Farm")
local SubProt = CreateSubTab(TabFarm, "Protections")
local SecProt = CreateSection(SubProt, "Safety Suites")
CreateToggle(SecProt, "Noclip", true, function(s) CFG.noclip = s end)
CreateToggle(SecProt, "Godmode", false, function(s) setGodmode(s) end)
CreateToggle(SecProt, "Invincibility (Lava/Killparts)", false, function(s) setGodmode(s) end)
CreateToggle(SecProt, "Invisible", false, function(s) setInvisible(s) end)
CreateToggle(SecProt, "Anti-Void", false, function(s) setAntiVoid(s) end)
CreateToggle(SecProt, "Fall Protect", false, function(s) CFG.fallProtect = s end)
CreateToggle(SecProt, "Hitbox Expander", false, function(s) setHitbox(s) end)

-- TAB: VISUALS
local TabVisuals = CreateMainTab("Visuals")
local SubVis = CreateSubTab(TabVisuals, "Client Mods")
local SecVis = CreateSection(SubVis, "Client Visuals")
CreateToggle(SecVis, "Freeze Character", false, function(s) setFreeze(s) end)
CreateToggle(SecVis, "Spin Bot", false, function(s) setSpinBot(s) end)
CreateToggle(SecVis, "Jump / Fall ESP", false, function(s) CFG.jumpEsp = s end)
CreateToggle(SecVis, "Player ESP", false, function(s) CFG.playerEsp = s end)

-- TAB: SETTINGS & TROLL
local TabSettings = CreateMainTab("Settings")

local SubTroll = CreateSubTab(TabSettings, "Troll Suite")
local SecTargetPick = CreateSection(SubTroll, "Target Selection")
local targetBtn = CreateButton(SecTargetPick, "Target: (Tap to cycle)", Theme.SectionBG, function()
    local list = {}
    for _, p in ipairs(Players:GetPlayers()) do if p ~= lp then table.insert(list, p.Name) end end
    if #list == 0 then selectedTarget = nil return end
    local idx = 1
    for i, n in ipairs(list) do if n == selectedTarget then idx = i + 1 break end end
    if idx > #list then idx = 1 end
    selectedTarget = list[idx]
    targetBtn.Text = "Target: " .. selectedTarget
end)

local SecTrollActions = CreateSection(SubTroll, "Troll Actions")
CreateToggle(SecTrollActions, "Ghost Follow Target", false, function(s) setFollow(s) end)
CreateToggle(SecTrollActions, "Copycat Mirror", false, function(s) setCopycat(s) end)
CreateToggle(SecTrollActions, "Camera Lock Target", false, function(s) setCamLock(s) end)
CreateToggle(SecTrollActions, "Chat Spam", false, function(s) setChatSpam(s) end)
CreateToggle(SecTrollActions, "Ghost Mode", false, function(s) setGhostMode(s) end)
CreateToggle(SecTrollActions, "Size Grow", false, function(s) setSizeGrow(s) end)
CreateToggle(SecTrollActions, "Size Shrink", false, function(s) setSizeShrink(s) end)
CreateToggle(SecTrollActions, "Giant Head", false, function(s) setGiantHead(s) end)
CreateToggle(SecTrollActions, "Bobble Head", false, function(s) setBobble(s) end)
CreateToggle(SecTrollActions, "Disco Character", false, function(s) setDisco(s) end)
CreateButton(SecTrollActions, "Fake Admin Message", Theme.SectionBG, function() sendFakeAdmin() end)
CreateButton(SecTrollActions, "Fake BSOD Error", Theme.SectionBG, function() showFakeError(true) end)
CreateButton(SecTrollActions, "Screen Shake (4s)", Theme.SectionBG, function() doScreenShake() end)
CreateButton(SecTrollActions, "Yeet Self", Theme.SectionBG, function() doYeetSelf() end)
CreateButton(SecTrollActions, "Fake Floor Remove", Theme.SectionBG, function() fakeFloorRemove() end)

local SubSys = CreateSubTab(TabSettings, "System")
local SecSys = CreateSection(SubSys, "Utilities")
CreateInput(SecSys, "Impersonate Player (Username)", function(name)
    pcall(function()
        local targetPlr = Players:FindFirstChild(name)
        if targetPlr and targetPlr.Character and char then
            local desc = targetPlr.Character:FindFirstChildOfClass("HumanoidDescription")
            if desc and hum then
                hum:ApplyDescriptionReset(desc)
            end
        end
    end)
end)
CreateToggle(SecSys, "Anti-AFK Protection", false, function(v)
    if v then
        LocalPlayer.Idled:Connect(function()
            if VirtualUser then
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.new())
            end
        end)
    end
end)
CreateButton(SecSys, "Reset UI / Clean", Theme.SectionBG, function() RyuHub:Destroy() end)

--// INITIALIZE FIRST TAB
pcall(function() 
    if Tabs[1] and Tabs[1].Toggle then Tabs[1].Toggle() end
    if Tabs[1] and Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Open then Tabs[1].SubTabs[1].Open() end 
end)

print("[Ryu Hub] Tower of Hell Suite fully loaded with custom features.")

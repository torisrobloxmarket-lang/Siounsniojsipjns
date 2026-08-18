--// ==========================================
--// RYU HUB - UI OVERLAY (TJS EDITION)
--// 100% MONOCHROME CLEAN TEMPLATE
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local VirtualInputManager = game:GetService("VirtualInputManager")

local LocalPlayer = Players.LocalPlayer
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
    if v.Name == "RyuHubUI" or v.Name == "RyuBFMobileGui" then v:Destroy() end 
end

--// THEME
local Theme = {
    Background = Color3.fromRGB(15, 15, 15),
    Sidebar = Color3.fromRGB(22, 22, 22),
    SectionBG = Color3.fromRGB(30, 30, 30),
    Text = Color3.fromRGB(255, 255, 255),
    SubText = Color3.fromRGB(150, 150, 150),
    Accent = Color3.fromRGB(255, 255, 255),
    ToggleOff = Color3.fromRGB(45, 45, 45),
    ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(60, 60, 60)
}

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

--// TOGGLE BUTTON (STATIC UNDER ROBLOX LOGO)
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

--// MAIN CONTAINER
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
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then 
        mDragging = false 
    end
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

--// UI GENERATION FUNCTIONS
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
    frame.Size = UDim2.new(0.92, 0, 0, 30)
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
    
    -- Dynamische Größenanpassung für längere Texte
    lbl:GetPropertyChangedSignal("TextBounds"):Connect(function()
        if lbl.TextBounds.Y > 30 then
            frame.Size = UDim2.new(0.92, 0, 0, lbl.TextBounds.Y + 10)
        end
    end)
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
    
    btn.MouseButton1Click:Connect(function() pcall(callback) end)
    return btn
end

local function CreateDropdown(section, headerText, itemsList, targetConfigKey, callback)
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
--// CONFIG & GLOBAL VARIABLES
--// ==========================================
local RyuConfig = {
    AutoBFYuji = false,
    AutoBFMahito = false,
    
    AutoBFChain = false,
    BFChainKey = Enum.KeyCode.E,
    BFActionKey = Enum.KeyCode.R,
    BFNoDashBehind = false,
    BFSensitivity = 5,
    BFLockTime = 2,
    BFSmoothness = 0.9,
    BFRange = 13,
    BFDashDir = "Automatic",
    
    AutoBlock = false,
    BlockRange = 15,
    BlockDuration = 500,
}

-- Aus dem Leak: IDs für Movements (alles, was kein Angriff ist)
local KnownMovementAnims = {
    ["120133391090244"] = true, ["138196552148011"] = true, -- idle
    ["96489184596023"] = true, ["117941450906936"] = true, ["77705898607209"] = true, -- walk/walkL/walkR
    ["140491244934559"] = true, -- sprint base
    ["134343219970072"] = true, -- jump
    ["126572575938378"] = true, -- fall
    ["93938476274140"] = true, -- climb
    ["137199497329581"] = true, -- sit
    ["77992084875736"] = true, -- Gojo sprint
    ["135750035707554"] = true, -- Hakari sprint
    ["85570635517461"] = true, -- Mahoraga/Heian sprint
    ["85012092465916"] = true, -- Mahito sprint
    ["72509133503569"] = true, -- Charles sprint
    ["119619096808750"] = true, -- Sword sprint (Hiromi/Yuta/Haruta/Kurourushi)
    ["98616794135588"] = true, -- Nanami ult sprint
    ["97238189166310"] = true, -- Goku ult sprint
    ["125812953913280"] = true, -- Goku sprint
    ["77801551230831"] = true, -- Mokou sprint
    ["114113678077830"] = true, -- Chara sprint
}

--// ==========================================
--// BLACK FLASH / OFFENSIVE LOGIC (WITH 0.6s DELAY)
--// ==========================================
local function FireYujiBF()
    pcall(function()
        local char = LocalPlayer.Character
        if not char then return end
        local move = char:WaitForChild("Moveset", 3):WaitForChild("Divergent Fist", 3)
        if move then
            local args = { move }
            ReplicatedStorage:WaitForChild("Knit", 3):WaitForChild("Knit", 3):WaitForChild("Services", 3):WaitForChild("DivergentFistService", 3):WaitForChild("RE", 3):WaitForChild("Activated", 3):FireServer(unpack(args))
        end
    end)
end

local function FireMahitoBF()
    pcall(function()
        local char = LocalPlayer.Character
        if not char then return end
        local move = char:WaitForChild("Moveset", 3):WaitForChild("Focus Strike", 3)
        if move then
            local args = { move }
            ReplicatedStorage:WaitForChild("Knit", 3):WaitForChild("Knit", 3):WaitForChild("Services", 3):WaitForChild("FocusStrikeService", 3):WaitForChild("RE", 3):WaitForChild("Activated", 3):FireServer(unpack(args))
        end
    end)
end

pcall(function()
    local oldNamecall
    oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
        local method = getnamecallmethod()
        local args = {...}
        
        if method == "FireServer" and self.Name == "Activated" then
            if RyuConfig.AutoBFYuji and self.Parent and self.Parent.Parent and self.Parent.Parent.Name == "DivergentFistService" then
                if not _G.YujiBFDebounce then
                    _G.YujiBFDebounce = true
                    task.delay(0.6, function()
                        pcall(function() self:FireServer(unpack(args)) end)
                        task.wait(0.1)
                        _G.YujiBFDebounce = false
                    end)
                end
            end
            if RyuConfig.AutoBFMahito and self.Parent and self.Parent.Parent and self.Parent.Parent.Name == "FocusStrikeService" then
                if not _G.MahitoBFDebounce then
                    _G.MahitoBFDebounce = true
                    task.delay(0.6, function()
                        pcall(function() self:FireServer(unpack(args)) end)
                        task.wait(0.1)
                        _G.MahitoBFDebounce = false
                    end)
                end
            end
        end
        return oldNamecall(self, ...)
    end)
end)

--// ==========================================
--// BLACK FLASH CHAIN LOGIC (MOBILE BUTTON & STRICT CHARACTER LOCK)
--// ==========================================
local isMobilePlayer = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

local BFMobileGui = Instance.new("ScreenGui")
BFMobileGui.Name = "RyuBFMobileGui"
BFMobileGui.ResetOnSpawn = false
BFMobileGui.Parent = guiParent

local BFMobileBtn = Instance.new("TextButton")
BFMobileBtn.Size = UDim2.new(0, 60, 0, 60)
BFMobileBtn.Position = UDim2.new(1, -90, 1, -150)
BFMobileBtn.BackgroundColor3 = Theme.Sidebar
BFMobileBtn.Text = "BF"
BFMobileBtn.TextColor3 = Theme.Accent
BFMobileBtn.Font = Enum.Font.GothamBlack
BFMobileBtn.TextSize = 20
BFMobileBtn.Visible = false
Instance.new("UICorner", BFMobileBtn).CornerRadius = UDim.new(1, 0)
local bfBtnStroke = Instance.new("UIStroke", BFMobileBtn)
bfBtnStroke.Color = Theme.Stroke
bfBtnStroke.Thickness = 2
BFMobileBtn.Parent = BFMobileGui

local function TriggerBFAttack()
    local char = LocalPlayer.Character
    if not char then return end
    
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    local closest = nil
    local minDist = RyuConfig.BFRange
    
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local d = (p.Character.HumanoidRootPart.Position - root.Position).Magnitude
            if d <= minDist then
                minDist = d
                closest = p.Character
            end
        end
    end
    
    if closest then
        local enemyRoot = closest.HumanoidRootPart
        
        -- Locken auf den Gegner vor der Berechnung
        root.CFrame = CFrame.new(root.Position, Vector3.new(enemyRoot.Position.X, root.Position.Y, enemyRoot.Position.Z))
        
        -- Dot Product nutzen um herauszufinden ob wir HINTER ihm stehen
        local vectorToEnemy = (enemyRoot.Position - root.Position).Unit
        local enemyLookVector = enemyRoot.CFrame.LookVector
        local isFacingAway = enemyLookVector:Dot(vectorToEnemy) > 0.3
        
        local shouldDash = true
        if RyuConfig.BFNoDashBehind and isFacingAway then
            shouldDash = false
        end
        
        -- Wenn wir nicht hinter ihm stehen -> Kreisförmiger Dash nach Hinten
        if shouldDash and not isFacingAway then
            local dir = "Left"
            if RyuConfig.BFDashDir == "Automatic" then
                local rightVector = enemyRoot.CFrame.RightVector
                if rightVector:Dot(vectorToEnemy) > 0 then
                    dir = "Right"
                else
                    dir = "Left"
                end
            elseif RyuConfig.BFDashDir == "Right" then
                dir = "Right"
            end
            
            pcall(function()
                local args = { dir }
                ReplicatedStorage:WaitForChild("Knit"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("MovementService"):WaitForChild("RE"):WaitForChild("Dash"):FireServer(unpack(args))
            end)
            
            -- Da der RenderStepped Loop die Rotation hält, fliegen wir in einem Bogen in seinen Rücken
            task.wait(0.35) 
            
            if char:FindFirstChild("HumanoidRootPart") and closest:FindFirstChild("HumanoidRootPart") then
                root.CFrame = CFrame.new(root.Position, Vector3.new(enemyRoot.Position.X, root.Position.Y, enemyRoot.Position.Z))
            end
        end
        
        -- 2x Black Flash mit 0.6s Timer, nachdem wir hinter ihm sind
        FireYujiBF()
        task.wait(0.6)
        FireYujiBF()
    end
end

BFMobileBtn.MouseButton1Click:Connect(TriggerBFAttack)

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        if RyuConfig.AutoBFYuji then FireYujiBF() end
        if RyuConfig.AutoBFMahito then FireMahitoBF() end
    end
    
    if input.KeyCode == RyuConfig.BFActionKey then
        if RyuConfig.AutoBFChain or isMobilePlayer then
            TriggerBFAttack()
        end
    end
    
    if input.KeyCode == RyuConfig.BFChainKey and not isMobilePlayer then
        RyuConfig.AutoBFChain = not RyuConfig.AutoBFChain
    end
end)

-- Character STRICT Lock & WASD Override Loop
RunService.RenderStepped:Connect(function()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not char or not hum or not root then return end

    if (RyuConfig.AutoBFChain or isMobilePlayer) and not MovementState.Fly then
        local closest = nil
        local minDist = RyuConfig.BFRange
        
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local d = (p.Character.HumanoidRootPart.Position - root.Position).Magnitude
                if d <= minDist then
                    minDist = d
                    closest = p.Character
                end
            end
        end
        
        if closest then
            local enemyRoot = closest.HumanoidRootPart
            local lookPos = Vector3.new(enemyRoot.Position.X, root.Position.Y, enemyRoot.Position.Z)
            
            -- STRICT LOCK: Ignoriert Shiftlock und zwingt den Char zum Feind
            root.CFrame = CFrame.lookAt(root.Position, lookPos)
            hum.AutoRotate = false
            _G.WasBFLocked = true
            
            -- CUSTOM WASD OVERRIDE (zum Feind, weg vom Feind, kreisförmig)
            local isMoving = UserInputService:IsKeyDown(Enum.KeyCode.W) or UserInputService:IsKeyDown(Enum.KeyCode.S) or UserInputService:IsKeyDown(Enum.KeyCode.A) or UserInputService:IsKeyDown(Enum.KeyCode.D)
            if isMoving and not MovementState.Speed then
                local moveDir = Vector3.new(0, 0, 0)
                local toTarget = (lookPos - root.Position).Unit
                local rightTarget = root.CFrame.RightVector
                
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + toTarget end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - toTarget end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - rightTarget end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + rightTarget end
                
                if moveDir.Magnitude > 0 then
                    hum:Move(moveDir.Unit, false)
                end
            end
        else
            if _G.WasBFLocked then
                hum.AutoRotate = true
                _G.WasBFLocked = false
            end
        end
    else
        if _G.WasBFLocked then
            hum.AutoRotate = true
            _G.WasBFLocked = false
        end
    end
end)

--// ==========================================
--// AUTO BLOCK LOGIC (SMART ANIMATION DETECTION)
--// ==========================================
RunService.Heartbeat:Connect(function()
    if RyuConfig.AutoBlock then
        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if not root then return end
        
        local incomingAttack = false
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local eRoot = p.Character.HumanoidRootPart
                local dist = (eRoot.Position - root.Position).Magnitude
                if dist <= RyuConfig.BlockRange then
                    local eHum = p.Character:FindFirstChildOfClass("Humanoid")
                    if eHum then
                        local animator = eHum:FindFirstChildOfClass("Animator")
                        if animator then
                            pcall(function()
                                for _, track in pairs(animator:GetPlayingAnimationTracks()) do
                                    local isAttack = false
                                    
                                    if track.Animation and track.Animation.AnimationId then
                                        local animId = track.Animation.AnimationId:match("%d+")
                                        -- Filtert Idle, Lauf und Dash Animationen exakt über deine Leak-IDs
                                        if animId and KnownMovementAnims[animId] then
                                            -- Ist sicher, kein Blocken nötig
                                        else
                                            local name = string.lower(track.Name)
                                            if string.find(name, "punch") or string.find(name, "attack") or string.find(name, "strike") or string.find(name, "m1") then
                                                isAttack = true
                                            elseif track.Priority == Enum.AnimationPriority.Action or track.Priority == Enum.AnimationPriority.Action2 or track.Priority == Enum.AnimationPriority.Action3 or track.Priority == Enum.AnimationPriority.Action4 then
                                                isAttack = true
                                            end
                                        end
                                    end
                                    
                                    if isAttack then
                                        incomingAttack = true
                                        break
                                    end
                                end
                            end)
                        end
                    end
                end
            end
            if incomingAttack then break end
        end
        
        if incomingAttack then
            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
            task.wait(RyuConfig.BlockDuration / 1000)
            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.F, false, game)
        end
    end
end)


--// ==========================================
--// MOVEMENT LOGIC (NO MOMENTUM, INSTANT)
--// ==========================================
local MovementState = {
    Speed = false, SpeedValue = 50,
    Fly = false, FlySpeed = 50, FlyKey = Enum.KeyCode.X,
    HighJump = false, JumpPower = 150,
    InfJump = false, Noclip = false, Invis = false
}

local flyBodyVelocity
local flyBodyGyro

local function StartFly()
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    if flyBodyVelocity then flyBodyVelocity:Destroy() end
    if flyBodyGyro then flyBodyGyro:Destroy() end
    
    flyBodyVelocity = Instance.new("BodyVelocity")
    flyBodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    flyBodyVelocity.Velocity = Vector3.new(0, 0, 0)
    flyBodyVelocity.Parent = root
    
    flyBodyGyro = Instance.new("BodyGyro")
    flyBodyGyro.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
    flyBodyGyro.D = 100
    flyBodyGyro.P = 10000
    flyBodyGyro.CFrame = root.CFrame
    flyBodyGyro.Parent = root
end

local function StopFly()
    if flyBodyVelocity then flyBodyVelocity:Destroy(); flyBodyVelocity = nil end
    if flyBodyGyro then flyBodyGyro:Destroy(); flyBodyGyro = nil end
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end
end

-- Fly Keybind hook
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == MovementState.FlyKey then
        MovementState.Fly = not MovementState.Fly
        if MovementState.Fly then StartFly() else StopFly() end
    end
end)

-- Inf Jump & High Jump force
UserInputService.JumpRequest:Connect(function()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")
    
    if hum and root then
        if MovementState.InfJump or MovementState.HighJump then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
        if MovementState.HighJump then
            -- Setzt die vertikale Geschwindigkeit sofort, ohne Verzögerung
            root.Velocity = Vector3.new(root.Velocity.X, MovementState.JumpPower, root.Velocity.Z)
        end
    end
end)

-- FE Server-Sided Invis (Root Joint Break & Floor Drop Trick)
local InvisLoop
local function ToggleInvis(state)
    local char = LocalPlayer.Character
    if not char then return end
    
    local realRoot = char:FindFirstChild("RealRoot") or char:FindFirstChild("HumanoidRootPart")
    local torso = char:FindFirstChild("Torso") or char:FindFirstChild("LowerTorso")
    local hum = char:FindFirstChildOfClass("Humanoid")
    
    if not realRoot or not torso or not hum then return end
    
    if state then
        if not char:FindFirstChild("FakeRoot") then
            -- 1. Echten Root umbenennen, damit wir einen Fake aufbauen können
            realRoot.Name = "RealRoot"
            
            -- 2. FakeRoot klonen und dem Character zuweisen (Für Kamera & Lokale Bewegung)
            local fakeRoot = realRoot:Clone()
            fakeRoot.Name = "HumanoidRootPart"
            fakeRoot.Transparency = 1
            fakeRoot.Parent = char
            char.PrimaryPart = fakeRoot
            
            -- 3. Zerstöre den echten Joint -> Der Server trennt den Torso ab und lässt ihn fallen!
            local origJoint = realRoot:FindFirstChild("RootJoint") or torso:FindFirstChild("Root")
            if origJoint then
                origJoint:Destroy()
            end
            
            -- 4. Lokal verbinden wir Torso an den neuen FakeRoot
            local fakeJoint = fakeRoot:FindFirstChild("RootJoint") or fakeRoot:FindFirstChild("Root")
            if not fakeJoint then
                fakeJoint = Instance.new("Motor6D")
                fakeJoint.Name = realRoot:FindFirstChild("RootJoint") and "RootJoint" or "Root"
                fakeJoint.Parent = (fakeJoint.Name == "RootJoint") and fakeRoot or torso
            end
            fakeJoint.Part0 = fakeRoot
            fakeJoint.Part1 = torso
            
            -- 5. Kamera sicher auf den Humanoid binden
            Workspace.CurrentCamera.CameraSubject = hum
            
            -- 6. Lokaler Ghost Effekt
            for _, v in pairs(char:GetDescendants()) do
                if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" and v.Name ~= "RealRoot" and v.Transparency < 1 then
                    v:SetAttribute("OldTrans", v.Transparency)
                    v.Transparency = 0.5
                elseif (v:IsA("Decal") or v:IsA("Texture")) and v.Transparency < 1 then
                    v:SetAttribute("OldTrans", v.Transparency)
                    v.Transparency = 1
                end
            end
            
            -- 7. Halte den RealRoot unten versteckt fest
            InvisLoop = RunService.RenderStepped:Connect(function()
                if char and char:FindFirstChild("RealRoot") then
                    -- Positioniere den echten Root weit unter die Map
                    char.RealRoot.CFrame = CFrame.new(0, workspace.FallenPartsDestroyHeight + 5, 0)
                    char.RealRoot.Velocity = Vector3.new(0, 0, 0)
                end
            end)
        end
    else
        -- ALLES SAUBER ZURÜCKSETZEN
        if InvisLoop then InvisLoop:Disconnect(); InvisLoop = nil end
        
        local fakeRoot = char:FindFirstChild("HumanoidRootPart")
        
        if fakeRoot and realRoot then
            realRoot.Name = "HumanoidRootPart"
            char.PrimaryPart = realRoot
            
            -- Lösche den Fake-Joint
            local fakeJoint = fakeRoot:FindFirstChild("RootJoint") or torso:FindFirstChild("Root")
            if fakeJoint then fakeJoint:Destroy() end
            
            -- Erstelle den echten Joint wieder
            local newJoint = Instance.new("Motor6D")
            newJoint.Name = char:FindFirstChild("Torso") and "RootJoint" or "Root"
            newJoint.Part0 = realRoot
            newJoint.Part1 = torso
            newJoint.Parent = char:FindFirstChild("Torso") and realRoot or torso
            
            -- Teleportiere RealRoot zurück nach oben zu dir
            realRoot.CFrame = fakeRoot.CFrame
            fakeRoot:Destroy()
            
            Workspace.CurrentCamera.CameraSubject = hum
        end
        
        -- Visuals wiederherstellen
        for _, v in pairs(char:GetDescendants()) do
            if v:GetAttribute("OldTrans") then
                v.Transparency = v:GetAttribute("OldTrans")
                v:SetAttribute("OldTrans", nil)
            end
        end
    end
end

-- Render Loop for Speed (Integrated cleanly to not disrupt the Chain Lock)
RunService.RenderStepped:Connect(function()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not char or not hum or not root then return end

    if MovementState.Speed and not MovementState.Fly then
        if hum.MoveDirection.Magnitude > 0 then
            local flatDir = hum.MoveDirection
            root.Velocity = Vector3.new(flatDir.X * MovementState.SpeedValue, root.Velocity.Y, flatDir.Z * MovementState.SpeedValue)
        else
            root.Velocity = Vector3.new(0, root.Velocity.Y, 0)
        end
    end
end)


--// ==========================================
--// STRUCTURE SETUP
--// ==========================================

-- TAB 1: COMBAT
local TabCombat = CreateMainTab("Combat")

local SubPlayer = CreateSubTab(TabCombat, "Player")
local SecPlayer = CreateSection(SubPlayer, "Movement")
CreateToggle(SecPlayer, "Speed Hack", false, function(state) MovementState.Speed = state end)
CreateSlider(SecPlayer, "Speed Value", 16, 150, 50, function(val) MovementState.SpeedValue = val end)

CreateToggle(SecPlayer, "Fly", false, function(state) 
    MovementState.Fly = state 
    if state then StartFly() else StopFly() end
end)
CreateSlider(SecPlayer, "Fly Speed", 10, 200, 50, function(val) MovementState.FlySpeed = val end)
CreateKeybind(SecPlayer, "Fly Keybind", Enum.KeyCode.X, function(key) MovementState.FlyKey = key end)

CreateToggle(SecPlayer, "High Jump", false, function(state) MovementState.HighJump = state end)
CreateSlider(SecPlayer, "Jump Power", 50, 300, 150, function(val) MovementState.JumpPower = val end)

CreateToggle(SecPlayer, "Infinite Jump Spam", false, function(state) MovementState.InfJump = state end)
CreateToggle(SecPlayer, "Noclip", false, function(state) MovementState.Noclip = state end)
CreateToggle(SecPlayer, "Invisible (FE Server-Sided)", false, function(state) 
    MovementState.Invis = state
    ToggleInvis(state)
end)
CreateLabel(SecPlayer, "(If you know a way to make the player invisible please dm me on discord, ill gift you nitro.)")

local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAutoDef = CreateSection(SubAuto, "Defensive")
CreateToggle(SecAutoDef, "Auto Block", false, function(state) RyuConfig.AutoBlock = state end)
CreateSlider(SecAutoDef, "Block React Range", 5, 50, 15, function(val) RyuConfig.BlockRange = val end)
CreateSlider(SecAutoDef, "Block Hold Duration (ms)", 100, 1500, 500, function(val) RyuConfig.BlockDuration = val end)
CreateToggle(SecAutoDef, "Auto Dodge (TP Back)", false, function() end)
CreateSlider(SecAutoDef, "Dodge Distance", 5, 50, 20, function() end)

local SecAutoOff = CreateSection(SubAuto, "Offensive")
CreateToggle(SecAutoOff, "Auto Black Flash (Yuji)", false, function(state) RyuConfig.AutoBFYuji = state end)
CreateToggle(SecAutoOff, "Auto Black Flash (Mahito)", false, function(state) RyuConfig.AutoBFMahito = state end)
CreateToggle(SecAutoOff, "Auto Todo Slap", false, function() end)
CreateLabel(SecAutoOff, "Auto Combos: Join discord.gg/ryuhub and send clips of your combos!")

local SecBFChain = CreateSection(SubAuto, "Blackflash")
CreateToggle(SecBFChain, "Auto black flash chain", false, function(state) 
    RyuConfig.AutoBFChain = state 
    if state then
        if isMobilePlayer then
            BFMobileBtn.Visible = true
        end
    else
        BFMobileBtn.Visible = false
    end
end)
CreateKeybind(SecBFChain, "Chain Toggle Keybind", Enum.KeyCode.E, function(key) RyuConfig.BFChainKey = key end)
CreateKeybind(SecBFChain, "Action Keybind (Dash+BF)", Enum.KeyCode.R, function(key) RyuConfig.BFActionKey = key end)
CreateToggle(SecBFChain, "Dont side dash when behind", false, function(state) RyuConfig.BFNoDashBehind = state end)
CreateSlider(SecBFChain, "Sensitivity", 1, 10, 5, function(val) RyuConfig.BFSensitivity = val end)
CreateSlider(SecBFChain, "Lock time (s)", 1, 5, 2, function(val) RyuConfig.BFLockTime = val end)
CreateSlider(SecBFChain, "Smoothness", 1, 10, 9, function(val) RyuConfig.BFSmoothness = val/10 end)
CreateSlider(SecBFChain, "Range (Studs)", 13, 100, 13, function(val) RyuConfig.BFRange = val end)
CreateDropdown(SecBFChain, "Side dash direction", {"Automatic", "Left", "Right"}, "Automatic", function(val) RyuConfig.BFDashDir = val end)

local SecAutoUtils = CreateSection(SubAuto, "Utilities")
CreateToggle(SecAutoUtils, "Auto Train", false, function() end)
CreateToggle(SecAutoUtils, "Enable Auto Item", false, function() end)
CreateDropdown(SecAutoUtils, "Target Item", {"None", "Cursed Finger", "Bat"}, "TargetItem", function() end)

local SubAbil = CreateSubTab(TabCombat, "Abilities")
local SecAbil = CreateSection(SubAbil, "Combat Enhancements")
CreateToggle(SecAbil, "Lock On (Stationary Camera)", false, function() end)
CreateKeybind(SecAbil, "Lock On Keybind", Enum.KeyCode.C, function() end)
CreateSlider(SecAbil, "Lock On Y-Offset", -10, 10, 0, function() end)
CreateToggle(SecAbil, "Knockback M1s", false, function() end)
CreateSlider(SecAbil, "Knockback Force", 10, 300, 50, function() end)
CreateToggle(SecAbil, "Domain Eraser (Local)", false, function() end)
CreateToggle(SecAbil, "No Cooldown Dash", false, function() end)
CreateButton(SecAbil, "Teleport All to Me", Theme.SectionBG, function() end)

-- TAB 2: FARM
local TabFarm = CreateMainTab("Farm")

local SubAIFarm = CreateSubTab(TabFarm, "AI Auto Farm")
local SecAIFarm = CreateSection(SubAIFarm, "Auto Combat")
CreateToggle(SecAIFarm, "Enable AI Farm", false, function() end)
CreateSlider(SecAIFarm, "Chase Range", 10, 500, 15, function() end)
CreateToggle(SecAIFarm, "Auto Ultimate (G)", false, function() end)

local SecAISkills = CreateSection(SubAIFarm, "Choose Skills")
local function MakeSkillRow(name)
    local active = false
    local btn
    btn = CreateButton(SecAISkills, name, Theme.ToggleOff, function()
        active = not active
        btn.BackgroundColor3 = active and Theme.Accent or Theme.ToggleOff
        btn.TextColor3 = active and Theme.Background or Theme.Text
    end)
    CreateSlider(SecAISkills, name .. " Range", 5, 100, 10, function() end)
end
MakeSkillRow("Skill 1")
MakeSkillRow("Skill 2")
MakeSkillRow("Skill 3")
MakeSkillRow("Skill 4")

local SubTarget = CreateSubTab(TabFarm, "Target")
local SecTarget = CreateSection(SubTarget, "Specific Target Follow")
CreateDropdown(SecTarget, "Target Player", {"None", "Dummy1", "Player1"}, "TargetPlayer", function() end)
CreateToggle(SecTarget, "Enable Target Farm", false, function() end)
CreateSlider(SecTarget, "Distance Behind", 1, 15, 3, function() end)

local SubMFarm = CreateSubTab(TabFarm, "Money Farm")
local SecMFarm = CreateSection(SubMFarm, "Alt Money Farm")
CreateToggle(SecMFarm, "Enable Money Farm (Y=500 Box)", false, function() end)
CreateButton(SecMFarm, "Role: Farmer", Theme.ToggleOff, function() end)
CreateInput(SecMFarm, "Victim: Enter Farmer Name...", function() end)

local SubFarmConfig = CreateSubTab(TabFarm, "Config")
local SecServerConfig = CreateSection(SubFarmConfig, "Server & Connections")
CreateToggle(SecServerConfig, "Auto Rejoin Low Pop", false, function() end)
CreateSlider(SecServerConfig, "Rejoin if players < X", 1, 10, 3, function() end)
CreateToggle(SecServerConfig, "Find 80% Full Lobbys", false, function() end)
CreateInput(SecServerConfig, "Alt Joiner: Enter Main Username", function() end)

-- TAB 3: SETTINGS
local TabSettings = CreateMainTab("Settings")
local SubSettings = CreateSubTab(TabSettings, "Settings")

local SecCosmetics = CreateSection(SubSettings, "Cosmetics")
CreateInput(SecCosmetics, "Fake Name (Visual)", function() end)

local SecCfg = CreateSection(SubSettings, "System & Protection")
CreateToggle(SecCfg, "Anti-AFK Protection", false, function() end)
CreateButton(SecCfg, "Save Settings", Theme.SectionBG, function() end)
CreateButton(SecCfg, "Reset Settings", Theme.SectionBG, function() end)

-- Open first tab on load
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

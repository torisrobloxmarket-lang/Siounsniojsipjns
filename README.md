--// ==========================================
--// RYU HUB - UI OVERLAY (TJS EDITION)
--// RAZORBILL COMBAT ENGINE INTEGRATION
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local VirtualInputManager = game:GetService("VirtualInputManager")
local PathfindingService = game:GetService("PathfindingService")
local HttpService = game:GetService("HttpService")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

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
    if v.Name == "RyuHubUI" or v.Name == "RyuBFMobileGui" or v.Name == "RyuChainMobileGui" then v:Destroy() end 
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

--// TOGGLE BUTTON
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
            
            if max <= 2 then
                local rawValue = min + (max - min) * relative
                value = math.floor(rawValue * 100) / 100
            end
            
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
--// RAZORBILL LOGIC INTEGRATION + NEW RYU LOGIC
--// ==========================================
local RyuConfig = {
    -- Auto BF & Chain
    BlackFlashEnabled = false,
    AutoBFYuji = false,
    AutoBFMahito = false,
    
    DashDistance = 15,
    FireDelay = 0.25,
    DashDuration = 0.35,
    LockTime = 0.1,
    DashCameraLock = true,
    DashEasingStyle = "Cubic",
    DashEasingDirection = "Out",
    
    -- Side Dash Assist
    DashAssistEnabled = false,
    DashAssistCameraLock = false,
    DashAssistOnlyIfFacing = false,
    DashAssistKeybind = Enum.KeyCode.J,
    DashAssistDetectionRange = 60,
    DashAssistBehindDistance = 5,
    DashAssistFlightDuration = 0.42,
    DashAssistCurveStrength = 10,
    DashAssistArchHeight = 3,
    DashAssistLockDuration = 0.35,
    
    -- Target Lock
    LockEnabled = false,
    LockMethod = "Camera",
    LockTargetMode = "Closest",
    LockTargetPart = "HumanoidRootPart",
    LockMaxDistance = 500,
    LockPrediction = 0,
    LockSmoothness = 0,
    LockSideOffset = 1.75,
    LockWallCheck = false,
    
    -- Auto Block (SMART FACING)
    AutoBlock = false,
    BlockRange = 15,
    BlockDuration = 500,
    
    -- AI Auto Farm (PERFECTED)
    AutoFarm = false,
    FarmMode = "Combo",
    FarmRange = 500,
    FarmSkill1 = true,
    FarmSkill2 = true,
    FarmSkill3 = true,
    FarmSkill4 = true,
    FarmTween = false,
    
    -- Alt Farm (OP METHOD)
    AltFarmEnabled = false,
    AltRole = "None",
    AltMainName = "",
    GroupRejoin = false,
    GroupRejoinCount = 5,
}

local Logic = {
    LastFiredTick = 0,
    TIME_WINDOW = 2,
    TargetAnimations = {
        ["100962226150441"] = true,
        ["95852624447551"] = true,
        ["74145636023952"] = true,
        ["123171106092050"] = true
    },
    LockState = {
        Enabled = false,
        LastTargetSearch = 0,
        CurrentLockTarget = nil,
        CameraLocked = false,
        WasLockedBody = false,
        LockBodyGyro = nil,
        ZoomDistance = 10,
    },
    Pathfinding = {
        Active = false,
        Speed = 350,
        CurrentTween = nil,
    },
    TargetRemote = nil,
}

local DashAnimLeft = Instance.new("Animation")
DashAnimLeft.AnimationId = "rbxassetid://75203303352791"
local DashAnimRight = Instance.new("Animation")
DashAnimRight.AnimationId = "rbxassetid://117223862448096"

local KnownMovementAnims = {
    ["120133391090244"] = true, ["138196552148011"] = true, 
    ["96489184596023"] = true, ["117941450906936"] = true, ["77705898607209"] = true, 
    ["140491244934559"] = true, ["134343219970072"] = true, ["126572575938378"] = true, 
    ["93938476274140"] = true, ["137199497329581"] = true, ["77992084875736"] = true, 
    ["135750035707554"] = true, ["85570635517461"] = true, ["85012092465916"] = true, 
    ["72509133503569"] = true, ["119619096808750"] = true, ["98616794135588"] = true, 
    ["97238189166310"] = true, ["125812953913280"] = true, ["77801551230831"] = true, 
    ["114113678077830"] = true, 
}

local StraightAnimations = {
    ["123171106092050"] = true,
}

local isMobilePlayer = UserInputService.TouchEnabled and not UserInputService.MouseEnabled

-- Fetch Remote for Hooking
task.spawn(function()
    Logic.TargetRemote = ReplicatedStorage:WaitForChild("Knit")
        :WaitForChild("Knit")
        :WaitForChild("Services")
        :WaitForChild("DivergentFistService")
        :WaitForChild("RE")
        :WaitForChild("Activated")
end)

local oldNamecall
oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
    local method = getnamecallmethod()
    if not checkcaller() and method == "FireServer" and self == Logic.TargetRemote and RyuConfig.BlackFlashEnabled then
        Logic.LastFiredTick = tick()
    end
    return oldNamecall(self, ...)
end)

local function autoFireDivergentFist()
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("Moveset") then
        local move1 = character.Moveset:FindFirstChild("Divergent Fist")
        local move2 = character.Moveset:FindFirstChild("Focus Strike")
        if move1 and Logic.TargetRemote then
            Logic.TargetRemote:FireServer(move1, nil)
        elseif move2 then
            pcall(function()
                ReplicatedStorage.Knit.Knit.Services.FocusStrikeService.RE.Activated:FireServer(move2, nil)
            end)
        end
    end
end

-- Helpers
local function getHRP(character)
    return character and (character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("Torso") or character:FindFirstChild("UpperTorso"))
end

local function getClosestTarget(maxDist)
    local char = LocalPlayer.Character
    local root = getHRP(char)
    if not root then return nil end

    local closest = nil
    local minDistance = math.huge

    local charactersFolder = workspace:FindFirstChild("Characters")
    local targetsToSearch = charactersFolder and charactersFolder:GetChildren() or workspace:GetChildren()

    for _, model in ipairs(targetsToSearch) do
        if model ~= char and model:IsA("Model") then
            local enemyRoot = getHRP(model)
            local enemyHum = model:FindFirstChild("Humanoid")

            if enemyRoot and enemyHum and enemyHum.Health > 0 then
                local dist = (enemyRoot.Position - root.Position).Magnitude
                if dist < minDistance and dist <= maxDist then
                    minDistance = dist
                    closest = model
                end
            end
        end
    end
    return closest
end

local function getBezierPoint(t, p0, p1, p2)
    return (1 - t)^2 * p0 + 2 * (1 - t) * t * p1 + t^2 * p2
end

--// ==========================================
--// BLACK FLASH CHAIN (PERFORMDASHLOGIC)
--// ==========================================
local function performDashLogic(target)
    local MAX_DASH_DISTANCE = RyuConfig.DashDistance
    local FIRE_DELAY = RyuConfig.FireDelay
    local DASH_DURATION = RyuConfig.DashDuration
    local POST_DASH_LOCK_TIME = RyuConfig.LockTime

    local easingStyleEnum = Enum.EasingStyle[RyuConfig.DashEasingStyle] or Enum.EasingStyle.Cubic
    local easingDirectionEnum = Enum.EasingDirection[RyuConfig.DashEasingDirection] or Enum.EasingDirection.Out

    local char = LocalPlayer.Character
    local root = getHRP(char)
    local humanoid = char and char:FindFirstChildOfClass("Humanoid")
    local enemyRoot = getHRP(target)

    if not root or not enemyRoot then
        task.delay(FIRE_DELAY, autoFireDivergentFist)
        return
    end

    local initialEnemyCFrame = enemyRoot.CFrame
    local initialEnemyPos = initialEnemyCFrame.Position
    local startPos = root.Position

    local objectSpacePos = initialEnemyCFrame:PointToObjectSpace(startPos)
    local isBehind = objectSpacePos.Z > 0
    local distanceToEnemy = (startPos - initialEnemyPos).Magnitude

    local isDash = true
    local dashType = "Arch"
    local endPos = startPos
    local controlPos = startPos

    if isBehind and distanceToEnemy <= 10 then
        isDash = false
    elseif isBehind and distanceToEnemy > 10 then
        dashType = "Straight"
        endPos = (initialEnemyCFrame * CFrame.new(0, 0, 5)).Position
    else
        dashType = "Arch"
        endPos = (initialEnemyCFrame * CFrame.new(0, 0, 4)).Position

        local distance = (endPos - startPos).Magnitude
        local archWidth = math.clamp(distance / 1.5, 5, 25)

        local direction = endPos - startPos
        local perp = Vector3.new(-direction.Z, 0, direction.X)
        if perp.Magnitude > 0.001 then perp = perp.Unit else perp = Vector3.new(1, 0, 0) end

        local midPos = (startPos + endPos) / 2
        local cp1 = midPos + (perp * archWidth)
        local cp2 = midPos - (perp * archWidth)

        local enemyRightVector = initialEnemyCFrame.RightVector
        local playerIsOnRightSide = (startPos - initialEnemyPos):Dot(enemyRightVector) > 0
        local cp1IsOnRightSide = (cp1 - initialEnemyPos):Dot(enemyRightVector) > 0

        controlPos = (playerIsOnRightSide == cp1IsOnRightSide) and cp1 or cp2
    end

    if humanoid then humanoid.AutoRotate = false end

    local startTime = tick()
    local hasFired = false
    local dashConn

    local function finalizeMovement(finalCFrame)
        if dashConn then dashConn:Disconnect() end
        if humanoid then humanoid.AutoRotate = true end
        if finalCFrame and root and root.Parent then root.CFrame = finalCFrame end

        if not hasFired then
            hasFired = true
            autoFireDivergentFist()
        end
    end

    dashConn = RunService.Heartbeat:Connect(function()
        if not root or not root.Parent then return finalizeMovement(nil) end

        local elapsed = tick() - startTime

        if elapsed >= FIRE_DELAY and not hasFired then
            hasFired = true
            autoFireDivergentFist()
        end

        local currentPos = startPos
        if isDash then
            local alpha = math.clamp(elapsed / DASH_DURATION, 0, 1)
            local easedAlpha = TweenService:GetValue(alpha, easingStyleEnum, easingDirectionEnum)

            if dashType == "Straight" then
                currentPos = startPos:Lerp(endPos, easedAlpha)
            elseif dashType == "Arch" then
                currentPos = (1 - easedAlpha)^2 * startPos + 2 * (1 - easedAlpha) * easedAlpha * controlPos + easedAlpha^2 * endPos
            end
        end

        root.CFrame = CFrame.lookAt(currentPos, initialEnemyPos)

        if RyuConfig.DashCameraLock then
            local cam = workspace.CurrentCamera
            if cam then
                cam.CFrame = CFrame.lookAt(cam.CFrame.Position, initialEnemyPos)
            end
        end

        local totalWaitTime = isDash and (DASH_DURATION + POST_DASH_LOCK_TIME) or (FIRE_DELAY + POST_DASH_LOCK_TIME)

        if elapsed >= totalWaitTime then
            finalizeMovement(CFrame.lookAt(currentPos, initialEnemyPos))
        end
    end)
end

--// ==========================================
--// SIDE DASH ASSIST (EXECUTE DASH ARC)
--// ==========================================
local isDashingArc = false

local function executeDashArc(direction)
    if isDashingArc then return end 
    isDashingArc = true

    local character = LocalPlayer.Character
    if not character then isDashingArc = false return end
    local root = getHRP(character)
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not root or not humanoid then isDashingArc = false return end

    local targetChar = getClosestTarget(RyuConfig.DashAssistDetectionRange)
    local target = targetChar and getHRP(targetChar)

    if not target then isDashingArc = false return end

    if RyuConfig.DashAssistOnlyIfFacing then
        local toPlayer = (root.Position - target.Position).Unit
        local dot = target.CFrame.LookVector:Dot(toPlayer)
        if dot < -0.1 then isDashingArc = false return end
    end

    root.Anchored = true
    humanoid.AutoRotate = false
    root.AssemblyLinearVelocity = Vector3.zero
    for _, mover in pairs(root:GetChildren()) do
        if mover:IsA("BodyVelocity") or mover:IsA("LinearVelocity") or mover:IsA("AlignPosition") or mover:IsA("VectorForce") or mover:IsA("BodyPosition") then
            mover:Destroy()
        end
    end

    local animator = humanoid:FindFirstChildOfClass("Animator")
    local dashTrack = nil
    if animator then
        for _, track in pairs(animator:GetPlayingAnimationTracks()) do
            if track.Animation and (track.Animation.AnimationId:match("117223862448096") or track.Animation.AnimationId:match("75203303352791")) then
                track:Stop(0)
            end
        end
        local animToUse = (direction == "Left") and DashAnimLeft or DashAnimRight
        dashTrack = animator:LoadAnimation(animToUse)
        dashTrack.Priority = Enum.AnimationPriority.Action4
        dashTrack:Play(0.05, 1, 1 / RyuConfig.DashAssistFlightDuration)
    end

    local p0 = root.Position
    local sideMult = (direction == "Left") and -1 or 1

    local progress = Instance.new("NumberValue")
    progress.Value = 0
    local dashName = "RazorbillFakeDash_" .. tostring(tick())

    local tween = TweenService:Create(
        progress,
        TweenInfo.new(RyuConfig.DashAssistFlightDuration, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut),
        { Value = 1 }
    )

    local Camera = workspace.CurrentCamera
    local prevCamType = Camera.CameraType

    RunService:BindToRenderStep(dashName, 20000, function()
        if not target or not target.Parent or not root then
            RunService:UnbindFromRenderStep(dashName)
            if root then root.Anchored = false end
            if humanoid then humanoid.AutoRotate = true end
            if dashTrack then dashTrack:Stop() end
            if RyuConfig.DashAssistCameraLock then Camera.CameraType = prevCamType end
            isDashingArc = false
            return
        end

        root.Anchored = true
        humanoid.AutoRotate = false
        root.AssemblyLinearVelocity = Vector3.zero

        local val = progress.Value

        local tPos = target.Position
        local tLook = target.CFrame.LookVector
        local flatLook = Vector3.new(tLook.X, 0, tLook.Z)
        if flatLook.Magnitude > 0.001 then flatLook = flatLook.Unit else flatLook = Vector3.new(0, 0, -1) end

        local tRight = target.CFrame.RightVector
        local flatRight = Vector3.new(tRight.X, 0, tRight.Z)
        if flatRight.Magnitude > 0.001 then flatRight = flatRight.Unit else flatRight = Vector3.new(1, 0, 0) end

        local p2 = tPos + (flatLook * -RyuConfig.DashAssistBehindDistance)
        local midPoint = (p0 + p2) / 2
        local p1 = midPoint + (flatRight * (RyuConfig.DashAssistCurveStrength * sideMult)) + Vector3.new(0, RyuConfig.DashAssistArchHeight, 0)

        local currentPos = getBezierPoint(val, p0, p1, p2)

        local lookPos = Vector3.new(tPos.X, currentPos.Y, tPos.Z)
        if (lookPos - currentPos).Magnitude > 0.1 then
            root.CFrame = CFrame.lookAt(currentPos, lookPos)
        else
            root.CFrame = CFrame.new(currentPos)
        end

        if RyuConfig.DashAssistCameraLock then
            Camera.CameraType = Enum.CameraType.Scriptable
            local dirToEnemy = (tPos - root.Position).Unit
            local targetCamCF = CFrame.lookAt(root.Position - (dirToEnemy * 11) + Vector3.new(0, 4.5, 0), tPos)
            Camera.CFrame = Camera.CFrame:Lerp(targetCamCF, 0.35)
        end
    end)

    tween:Play()

    tween.Completed:Connect(function()
        RunService:UnbindFromRenderStep(dashName)
        progress:Destroy()
        if dashTrack then dashTrack:Stop(0.1) end

        if RyuConfig.DashAssistCameraLock then
            Camera.CameraType = prevCamType
        end

        local lockStart = tick()
        local lockName = "RazorbillDashLock_" .. tostring(lockStart)
        RunService:BindToRenderStep(lockName, 20000, function()
            if tick() - lockStart > RyuConfig.DashAssistLockDuration or not target or not target.Parent or not root then
                RunService:UnbindFromRenderStep(lockName)
                if root then
                    root.Anchored = false
                    root.AssemblyLinearVelocity = Vector3.zero
                end
                if humanoid then humanoid.AutoRotate = true end
                isDashingArc = false 
                return
            end
            root.Anchored = true
            if humanoid then humanoid.AutoRotate = false end
            root.AssemblyLinearVelocity = Vector3.zero

            local lockedPos = root.Position
            local facePos = Vector3.new(target.Position.X, lockedPos.Y, target.Position.Z)
            if (facePos - lockedPos).Magnitude > 0.1 then
                root.CFrame = CFrame.lookAt(lockedPos, facePos)
            end
        end)
    end)
end

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if not RyuConfig.DashAssistEnabled then return end

    if input.KeyCode == RyuConfig.DashAssistKeybind then
        local direction = "Right"
        local char = LocalPlayer.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum and hum.MoveDirection.Magnitude > 0 then
            local camRight = workspace.CurrentCamera.CFrame.RightVector * Vector3.new(1, 0, 1)
            if camRight.Magnitude > 0 then
                camRight = camRight.Unit
                if hum.MoveDirection:Dot(camRight) < -0.2 then direction = "Left" end
            end
        end
        task.spawn(function() executeDashArc(direction) end)
    end
end)

--// ==========================================
--// ANIMATION HOOK (AUTO BF & CHAIN)
--// ==========================================
local function onAnimationPlayed(animTrack)
    local animId = animTrack.Animation and animTrack.Animation.AnimationId or ""

    if RyuConfig.DashAssistEnabled and not isDashingArc then
        if animId:match("75203303352791") then
            task.spawn(function() executeDashArc("Left") end)
        elseif animId:match("117223862448096") then
            task.spawn(function() executeDashArc("Right") end)
        end
    end

    local idMatch = string.match(animId, "%d+")
    if idMatch and Logic.TargetAnimations[idMatch] then
        if RyuConfig.BlackFlashEnabled then
            if (tick() - Logic.LastFiredTick) <= Logic.TIME_WINDOW then
                Logic.LastFiredTick = 0

                local closestTarget = getClosestTarget(RyuConfig.DashDistance)
                if closestTarget then
                    performDashLogic(closestTarget)
                else
                    task.delay(RyuConfig.FireDelay, autoFireDivergentFist)
                end
            end
        elseif RyuConfig.AutoBFYuji or RyuConfig.AutoBFMahito then
            task.delay(0.19, autoFireDivergentFist)
        end
    end
end

local function setupCharacter(character)
    local humanoid = character:WaitForChild("Humanoid", 10)
    if humanoid then
        local animator = humanoid:WaitForChild("Animator", 10)
        if animator then
            animator.AnimationPlayed:Connect(onAnimationPlayed)
        end
    end
end

if LocalPlayer.Character then task.spawn(setupCharacter, LocalPlayer.Character) end
LocalPlayer.CharacterAdded:Connect(setupCharacter)

--// ==========================================
--// TARGET LOCK LOGIC
--// ==========================================
RunService:BindToRenderStep("RazorbillTargetLockExtracted", Enum.RenderPriority.Camera.Value + 5, function(dt)
    local char = LocalPlayer.Character
    local hum  = char and char:FindFirstChildOfClass("Humanoid")
    local root = getHRP(char)
    local isAlive = (hum and hum.Health > 0 and root)

    if not RyuConfig.LockEnabled or not isAlive then
        if Logic.LockState.CameraLocked then
            camera.CameraType = Enum.CameraType.Custom
            if hum then camera.CameraSubject = hum end
            Logic.LockState.CameraLocked = false
        end
        if Logic.LockState.WasLockedBody then
            if hum then hum.AutoRotate = true end
            if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy(); Logic.LockState.LockBodyGyro = nil end
            Logic.LockState.WasLockedBody = false
        end
        Logic.LockState.CurrentLockTarget = nil
        return
    end

    if Logic.LockState.CurrentLockTarget then
        local eHum = Logic.LockState.CurrentLockTarget:FindFirstChildOfClass("Humanoid")
        local targetAlive = Logic.LockState.CurrentLockTarget.Parent and eHum and eHum.Health > 0
        if not targetAlive then
            Logic.LockState.CurrentLockTarget = nil
        else
            local tRoot = getHRP(Logic.LockState.CurrentLockTarget)
            if tRoot and (root.Position - tRoot.Position).Magnitude > RyuConfig.LockMaxDistance then
                Logic.LockState.CurrentLockTarget = nil
            end
        end
    end

    if Logic.LockState.CurrentLockTarget == nil then
        if tick() - Logic.LockState.LastTargetSearch >= 0.5 then
            Logic.LockState.LastTargetSearch = tick()
            local best, shortest = nil, math.huge
            local charsFolder = workspace:FindFirstChild("Characters")
            local entities = charsFolder and charsFolder:GetChildren() or {}
            if #entities == 0 then
                for _, p in ipairs(Players:GetPlayers()) do
                    if p.Character then table.insert(entities, p.Character) end
                end
            end

            for _, tChar in ipairs(entities) do
                if tChar:IsA("Model") and tChar ~= char then
                    local tHum = tChar:FindFirstChildOfClass("Humanoid")
                    local tRoot = getHRP(tChar)

                    if tHum and tHum.Health > 0 and tRoot then
                        local worldDist = (root.Position - tRoot.Position).Magnitude
                        if worldDist <= RyuConfig.LockMaxDistance then
                            if RyuConfig.LockWallCheck then
                                local rayParams = RaycastParams.new()
                                rayParams.FilterDescendantsInstances = {char, tChar}
                                rayParams.FilterType = Enum.RaycastFilterType.Exclude
                                local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * worldDist, rayParams)
                                if ray then continue end
                            end

                            if RyuConfig.LockTargetMode == "Closest" then
                                if worldDist < shortest then shortest = worldDist; best = tChar end
                            else
                                local pos, onScreen = camera:WorldToViewportPoint(tRoot.Position)
                                if onScreen then
                                    local d = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(pos.X, pos.Y)).Magnitude
                                    if d < shortest then shortest = d; best = tChar end
                                end
                            end
                        end
                    end
                end
            end
            Logic.LockState.CurrentLockTarget = best
        end
    end
    
    if not Logic.LockState.CurrentLockTarget then return end
    local targetPart = Logic.LockState.CurrentLockTarget:FindFirstChild(RyuConfig.LockTargetPart) or getHRP(Logic.LockState.CurrentLockTarget)
    if not targetPart then return end

    local targetPos = targetPart.Position
    if RyuConfig.LockPrediction > 0 then
        local tRoot = getHRP(Logic.LockState.CurrentLockTarget)
        if tRoot and tRoot:IsA("BasePart") then
            targetPos = targetPos + (tRoot.AssemblyLinearVelocity * RyuConfig.LockPrediction)
        end
    end

    if RyuConfig.LockMethod == "Camera" then
        if Logic.LockState.WasLockedBody then
            hum.AutoRotate = true
            if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy(); Logic.LockState.LockBodyGyro = nil end
            Logic.LockState.WasLockedBody = false
        end

        if not Logic.LockState.CameraLocked then
            local dist = (camera.CFrame.Position - root.Position).Magnitude
            Logic.LockState.ZoomDistance = dist <= 50 and math.clamp(dist, 4, 50) or 10
            Logic.LockState.CameraLocked = true
        end

        camera.CameraType = Enum.CameraType.Scriptable
        local offsetDir = root.Position - targetPos
        local flatDir = Vector3.new(offsetDir.X, 0, offsetDir.Z)
        if flatDir.Magnitude < 0.001 then flatDir = -root.CFrame.LookVector; flatDir = Vector3.new(flatDir.X, 0, flatDir.Z) end
        flatDir = flatDir.Unit

        local camPos = root.Position + (flatDir * Logic.LockState.ZoomDistance) + Vector3.new(0, 2.5, 0)
        local lookCF = CFrame.lookAt(camPos, targetPos)
        camPos = camPos + (lookCF.RightVector * RyuConfig.LockSideOffset)

        local desiredCF = CFrame.lookAt(camPos, targetPos)
        if RyuConfig.LockSmoothness <= 0 then
            camera.CFrame = desiredCF
        else
            local rate  = 30 / RyuConfig.LockSmoothness
            local alpha = math.clamp(1 - math.exp(-rate * dt), 0, 1)
            camera.CFrame = camera.CFrame:Lerp(desiredCF, alpha)
        end
    else
        if Logic.LockState.CameraLocked then
            camera.CameraType = Enum.CameraType.Custom
            camera.CameraSubject = hum
            Logic.LockState.CameraLocked = false
        end
        hum.AutoRotate = false
        Logic.LockState.WasLockedBody = true

        if not Logic.LockState.LockBodyGyro or Logic.LockState.LockBodyGyro.Parent ~= root then
            if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy() end
            Logic.LockState.LockBodyGyro = Instance.new("BodyGyro")
            Logic.LockState.LockBodyGyro.MaxTorque = Vector3.new(0, 400000, 0)
            Logic.LockState.LockBodyGyro.P = 50000
            Logic.LockState.LockBodyGyro.D = 500
            Logic.LockState.LockBodyGyro.Parent = root
        end
        Logic.LockState.LockBodyGyro.CFrame = CFrame.lookAt(root.Position, Vector3.new(targetPos.X, root.Position.Y, targetPos.Z))
    end
end)


--// ==========================================
--// TWEEN & PATHFINDING TELEPORTS
--// ==========================================
local TeleportLocations = {
    ["Under the Map"]      = Vector3.new(-20.23, -61.53, -146.34),
    ["Unlicensed Studios"] = Vector3.new(196.86, 23.58, -37.27),
    ["Towers"]             = Vector3.new(25.35, 183.08, 110.77),
    ["Train Button"]       = Vector3.new(182.21, -9.33, 562.54),
    ["Bowling"]            = Vector3.new(267.60, -59.89, -255.06),
    ["Restaurant"]         = Vector3.new(-43.24, 23.63, -83.07),
    ["Storage House"]      = Vector3.new(195.69, 23.58, 151.44),
    ["Sewers 1"]           = Vector3.new(-148.14, -31.48, -127.22),
    ["Train Station"]      = Vector3.new(185.27, -9.69, -97.17),
    ["Sewers 2"]           = Vector3.new(60.84, -10.58, 167.47),
    ["Shenanigans Mall"]   = Vector3.new(155.66, -26.38, -254.85),
    ["Rhythm Game"]        = Vector3.new(12.23, -30.21, -315.03),
    ["Piano"]              = Vector3.new(-86.38, 26.65, -252.48),
    ["Convenience Store"]  = Vector3.new(-247.51, 26.96, -116.64),
    ["Court"]              = Vector3.new(124.48, 23.78, -247.06),
    ["Graveyard"]          = Vector3.new(228.55, 23.68, -130.48),
    ["Train Station Exit"] = Vector3.new(1.52, 24.72, 396.06),
    ["Tze's"]              = Vector3.new(-55.30, 23.62, 245.42),
    ["Jail"]               = Vector3.new(-243.84, 23.58, 126.97),
}

local LocationNames = {}
for k, _ in pairs(TeleportLocations) do table.insert(LocationNames, k) end
table.sort(LocationNames)

local function getWaypointSpacing(speed)
    if speed <= 30 then return 4
    elseif speed <= 100 then return 6
    elseif speed <= 300 then return 10
    elseif speed <= 600 then return 16
    else return 24 end
end

local function tweenToPosition(hrp, targetPos, speed)
    local distance = (hrp.Position - targetPos).Magnitude
    if distance < 0.5 then return true end
    local duration = math.max(distance / speed, 0.001)

    local direction = (targetPos - hrp.Position)
    local flatDir = Vector3.new(direction.X, 0, direction.Z)
    local targetCF = flatDir.Magnitude > 0.1 and CFrame.new(targetPos, targetPos + flatDir.Unit) or CFrame.new(targetPos) * hrp.CFrame.Rotation

    local tween = TweenService:Create(hrp, TweenInfo.new(duration, Enum.EasingStyle.Linear), { CFrame = targetCF })
    Logic.Pathfinding.CurrentTween = tween
    tween:Play()

    local done = false
    local conn
    conn = tween.Completed:Connect(function()
        done = true
        if conn then conn:Disconnect() end
    end)

    while not done and Logic.Pathfinding.Active do RunService.Heartbeat:Wait() end
    if not done and Logic.Pathfinding.CurrentTween == tween then tween:Cancel() end
    return done and Logic.Pathfinding.Active
end

local function stepToPosition(hrp, targetPos, speed)
    local startPos = hrp.Position
    local direction = (targetPos - startPos)
    local distance = direction.Magnitude
    if distance < 0.5 then return true end

    local dirUnit = direction.Unit
    local flatDir = Vector3.new(dirUnit.X, 0, dirUnit.Z)
    local lookCF = flatDir.Magnitude > 0.01 and CFrame.lookAt(Vector3.zero, flatDir) or CFrame.new()

    local traveled = 0
    while traveled < distance and Logic.Pathfinding.Active do
        local dt = RunService.Heartbeat:Wait()
        local step = speed * dt
        traveled = math.min(traveled + step, distance)
        hrp.CFrame = CFrame.new(startPos + dirUnit * traveled) * lookCF.Rotation
    end
    return traveled >= distance and Logic.Pathfinding.Active
end

local function batchWaypoints(waypoints, minSegmentLength)
    local batched = {}
    local lastAdded = waypoints[1]
    table.insert(batched, waypoints[1])
    for i = 2, #waypoints do
        local wp = waypoints[i]
        local isJump = (wp.Action == Enum.PathWaypointAction.Jump)
        local isLast = (i == #waypoints)
        local distFromLast = (wp.Position - lastAdded.Position).Magnitude
        if isJump or isLast or distFromLast >= minSegmentLength then
            table.insert(batched, wp)
            lastAdded = wp
        end
    end
    return batched
end

local function stopPathfinding()
    Logic.Pathfinding.Active = false
    if Logic.Pathfinding.CurrentTween then pcall(function() Logic.Pathfinding.CurrentTween:Cancel() end) end
    local hrp = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.Anchored = false end
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end
end

local function startPathfinding(targetPos)
    if Logic.Pathfinding.Active then stopPathfinding(); task.wait(0.15) end
    Logic.Pathfinding.Active = true

    task.spawn(function()
        while Logic.Pathfinding.Active do
            local speed = Logic.Pathfinding.Speed
            local path = PathfindingService:CreatePath({ AgentRadius = 3, AgentHeight = 6, AgentCanJump = true, AgentCanClimb = true, WaypointSpacing = getWaypointSpacing(speed) })
            local hrp = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if not hrp or not humanoid then task.wait(1) continue end

            if (hrp.Position - targetPos).Magnitude < 6 then
                stopPathfinding()
                break
            end

            local ok = pcall(function() path:ComputeAsync(hrp.Position, targetPos) end)
            if not ok or path.Status == Enum.PathStatus.NoPath then
                task.wait(1.5)
                continue
            end

            local batchedWP = batchWaypoints(path:GetWaypoints(), math.max(speed * 0.05, 2))
            local pathBlocked = false
            local blockedConn = path.Blocked:Connect(function() pathBlocked = true end)

            hrp.Anchored = true
            humanoid.PlatformStand = true

            local completed = true
            for i = 2, #batchedWP do
                if not Logic.Pathfinding.Active or pathBlocked then completed = false break end
                local success
                if speed > 500 then
                    success = stepToPosition(hrp, batchedWP[i].Position, speed)
                else
                    success = tweenToPosition(hrp, batchedWP[i].Position, speed)
                end
                if not success then completed = false break end
            end

            if blockedConn then blockedConn:Disconnect() end

            if completed and Logic.Pathfinding.Active then
                stopPathfinding()
                break
            end
            task.wait(0.3)
        end
    end)
end

--// ==========================================
--// AI AUTO FARM (PERFECTED LOGIC)
--// ==========================================
local m1Count = 0
local lastPos = Vector3.zero
local timeStuck = 0
local lastTick = tick()
local wHeld = false

RunService.Heartbeat:Connect(function()
    local char = LocalPlayer.Character
    local root = getHRP(char)
    local hum = char and char:FindFirstChildOfClass("Humanoid")

    if not RyuConfig.AutoFarm then
        if wHeld then
            if not isMobilePlayer then VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.W, false, game) end
            wHeld = false
        end
        return 
    end
    
    if not root or not hum or hum.Health <= 0 then return end
    
    local target = getClosestTarget(RyuConfig.FarmRange)
    if target then
        local tRoot = getHRP(target)
        if not tRoot then return end
        
        local dist = (root.Position - tRoot.Position).Magnitude
        
        -- Lock On permanent
        root.CFrame = CFrame.lookAt(root.Position, Vector3.new(tRoot.Position.X, root.Position.Y, tRoot.Position.Z))
        
        -- Movement (Tween or W+Walk)
        if RyuConfig.FarmTween then
            if wHeld then
                if not isMobilePlayer then VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.W, false, game) end
                wHeld = false
            end
            if dist > 5 then
                root.CFrame = root.CFrame:Lerp(CFrame.lookAt(root.Position + (tRoot.Position - root.Position).Unit * (50 * RunService.Heartbeat:Wait()), tRoot.Position), 1)
            end
        else
            if not wHeld and not isMobilePlayer then
                VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.W, false, game)
                wHeld = true
            end
            hum:MoveTo(tRoot.Position)
        end
        
        -- Q Dash Gap Closer
        if dist > 15 and dist < 40 and math.random() > 0.95 then
            if not isMobilePlayer then
                VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Q, false, game)
                task.wait(0.1)
                VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Q, false, game)
            end
        end
        
        -- Failsafe
        if tick() - lastTick >= 1 then
            lastTick = tick()
            if (root.Position - lastPos).Magnitude < 1.5 then
                timeStuck = timeStuck + 1
                if timeStuck > 2 then
                    hum.Jump = true
                    if not isMobilePlayer then
                        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 0)
                        task.wait(0.1)
                        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 0)
                    end
                end
                if timeStuck >= 120 then
                    hum.Health = 0
                    timeStuck = 0
                end
            else
                timeStuck = 0
            end
            lastPos = root.Position
        end
    else
        if wHeld then
            if not isMobilePlayer then VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.W, false, game) end
            wHeld = false
        end
    end
end)

-- Combat Loop
task.spawn(function()
    while task.wait(0.2) do
        if RyuConfig.AutoFarm then
            local char = LocalPlayer.Character
            local root = getHRP(char)
            if root then
                local target = getClosestTarget(RyuConfig.FarmRange)
                if target then
                    local tRoot = getHRP(target)
                    if tRoot and (root.Position - tRoot.Position).Magnitude < 8 then
                        local skills = {}
                        if RyuConfig.FarmSkill1 then table.insert(skills, Enum.KeyCode.One) end
                        if RyuConfig.FarmSkill2 then table.insert(skills, Enum.KeyCode.Two) end
                        if RyuConfig.FarmSkill3 then table.insert(skills, Enum.KeyCode.Three) end
                        if RyuConfig.FarmSkill4 then table.insert(skills, Enum.KeyCode.Four) end
                        
                        if RyuConfig.FarmMode == "Spam" then
                            if #skills > 0 and not isMobilePlayer then
                                local k = skills[math.random(1, #skills)]
                                VirtualInputManager:SendKeyEvent(true, k, false, game)
                                task.wait(0.05)
                                VirtualInputManager:SendKeyEvent(false, k, false, game)
                            end
                            if not isMobilePlayer then
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 0)
                                task.wait(0.05)
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 0)
                            end
                        else
                            if not isMobilePlayer then
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 0)
                                task.wait(0.05)
                                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 0)
                            end
                            m1Count = m1Count + 1
                            if m1Count >= 4 then
                                if #skills > 0 and not isMobilePlayer then
                                    local k = skills[math.random(1, #skills)]
                                    VirtualInputManager:SendKeyEvent(true, k, false, game)
                                    task.wait(0.1)
                                    VirtualInputManager:SendKeyEvent(false, k, false, game)
                                end
                                m1Count = 0
                                task.wait(0.4)
                            end
                        end
                    end
                end
            end
        end
    end
end)

--// ==========================================
--// ALT FARM (OP CASHER METHOD)
--// ==========================================
local AltPlatformPos = Vector3.new(9999, 800, 9999)

local function GetAltPlatform()
    local plat = workspace:FindFirstChild("RyuAltPlatform")
    if not plat then
        plat = Instance.new("Part")
        plat.Name = "RyuAltPlatform"
        plat.Size = Vector3.new(150, 5, 150)
        plat.Position = AltPlatformPos
        plat.Anchored = true
        plat.Parent = workspace
        
        local sg = Instance.new("SurfaceGui", plat)
        sg.Face = Enum.NormalId.Top
        local tl = Instance.new("TextLabel", sg)
        tl.Size = UDim2.new(1, 0, 1, 0)
        tl.BackgroundTransparency = 1
        tl.Text = "discord.gg/ryuhub"
        tl.TextScaled = true
        tl.TextColor3 = Color3.new(1,1,1)
        tl.Font = Enum.Font.GothamBlack
    end
    return plat
end

RunService.Heartbeat:Connect(function()
    if RyuConfig.AltFarmEnabled then
        local char = LocalPlayer.Character
        local root = getHRP(char)
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if not root or not hum then return end
        
        local plat = GetAltPlatform()
        
        if RyuConfig.AltRole == "Casher" then
            local mainPlr = Players:FindFirstChild(RyuConfig.AltMainName)
            local hpPercent = hum.Health / hum.MaxHealth
            
            if hpPercent <= 0.35 then
                plat.CanCollide = false
            else
                plat.CanCollide = true
                if mainPlr and mainPlr.Character and mainPlr.Character:FindFirstChild("HumanoidRootPart") then
                    root.CFrame = mainPlr.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -3)
                else
                    root.CFrame = CFrame.new(AltPlatformPos + Vector3.new(0, 5, 0))
                end
            end
        elseif RyuConfig.AltRole == "Main" then
            plat.CanCollide = true
            if (root.Position - AltPlatformPos).Magnitude > 100 then
                root.CFrame = CFrame.new(AltPlatformPos + Vector3.new(0, 5, 0))
            end
        end
    end
end)

-- Alt Main Combat
task.spawn(function()
    while task.wait(0.2) do
        if RyuConfig.AltFarmEnabled and RyuConfig.AltRole == "Main" then
            if not isMobilePlayer then
                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 0)
                task.wait(0.05)
                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 0)
                
                local keys = {Enum.KeyCode.One, Enum.KeyCode.Two, Enum.KeyCode.Three, Enum.KeyCode.Four}
                local k = keys[math.random(1, 4)]
                VirtualInputManager:SendKeyEvent(true, k, false, game)
                task.wait(0.05)
                VirtualInputManager:SendKeyEvent(false, k, false, game)
            end
        end
    end
end)

-- Auto Rejoin (Low Pop)
task.spawn(function()
    while task.wait(5) do
        if RyuConfig.GroupRejoin then
            if #Players:GetPlayers() <= RyuConfig.GroupRejoinCount then
                pcall(function()
                    local url = "https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/0?sortOrder=2&excludeFullGames=true&limit=10"
                    local req = game:HttpGet(url)
                    local data = HttpService:JSONDecode(req)
                    if data and data.data then
                        for _, srv in pairs(data.data) do
                            if srv.playing > 0 and srv.id ~= game.JobId then
                                game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, srv.id, LocalPlayer)
                                break
                            end
                        end
                    end
                end)
            end
        end
    end
end)

--// ==========================================
--// AUTO BLOCK LOGIC (STRICT FACING)
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
                    
                    local toPlayer = (root.Position - eRoot.Position).Unit
                    local lookDir = eRoot.CFrame.LookVector
                    
                    if lookDir:Dot(toPlayer) > 0.65 then
                        local eHum = p.Character:FindFirstChildOfClass("Humanoid")
                        if eHum then
                            local animator = eHum:FindFirstChildOfClass("Animator")
                            if animator then
                                pcall(function()
                                    for _, track in pairs(animator:GetPlayingAnimationTracks()) do
                                        local isAttack = false
                                        if track.Animation and track.Animation.AnimationId then
                                            local animId = track.Animation.AnimationId:match("%d+")
                                            if not (animId and KnownMovementAnims[animId]) then
                                                local name = string.lower(track.Name)
                                                if string.find(name, "punch") or string.find(name, "attack") or string.find(name, "strike") or string.find(name, "m1") then
                                                    isAttack = true
                                                elseif track.Priority == Enum.AnimationPriority.Action or track.Priority == Enum.AnimationPriority.Action2 or track.Priority == Enum.AnimationPriority.Action3 or track.Priority == Enum.AnimationPriority.Action4 then
                                                    isAttack = true
                                                end
                                            end
                                        end
                                        if isAttack then incomingAttack = true break end
                                    end
                                end)
                            end
                        end
                    end
                end
            end
            if incomingAttack then break end
        end
        
        if incomingAttack then
            pcall(function()
                ReplicatedStorage:WaitForChild("Knit"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("BlockService"):WaitForChild("RE"):WaitForChild("Activated"):FireServer()
            end)
            task.wait(RyuConfig.BlockDuration / 1000)
            pcall(function()
                ReplicatedStorage:WaitForChild("Knit"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("BlockService"):WaitForChild("RE"):WaitForChild("Deactivated"):FireServer()
            end)
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

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == MovementState.FlyKey then
        MovementState.Fly = not MovementState.Fly
        if MovementState.Fly then StartFly() else StopFly() end
    end
end)

UserInputService.JumpRequest:Connect(function()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")
    
    if hum and root then
        if MovementState.InfJump or MovementState.HighJump then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
        if MovementState.HighJump then
            root.Velocity = Vector3.new(root.Velocity.X, MovementState.JumpPower, root.Velocity.Z)
        end
    end
end)

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

local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAutoDef = CreateSection(SubAuto, "Defensive")
CreateToggle(SecAutoDef, "Auto Block", false, function(state) RyuConfig.AutoBlock = state end)
CreateSlider(SecAutoDef, "Block React Range", 5, 50, 15, function(val) RyuConfig.BlockRange = val end)
CreateSlider(SecAutoDef, "Block Hold Duration (ms)", 100, 1500, 500, function(val) RyuConfig.BlockDuration = val end)

local SecAutoOff = CreateSection(SubAuto, "Offensive")
CreateToggle(SecAutoOff, "Auto Black Flash (Yuji)", false, function(state) RyuConfig.AutoBFYuji = state end)
CreateToggle(SecAutoOff, "Auto Black Flash (Mahito)", false, function(state) RyuConfig.AutoBFMahito = state end)

local SecBFChain = CreateSection(SubAuto, "Blackflash Chain")
CreateToggle(SecBFChain, "Enable Black Flash Chain", false, function(state) 
    RyuConfig.BlackFlashEnabled = state 
    if state then
        if isMobilePlayer then dashBtn.Visible = true end
    else
        dashBtn.Visible = false
    end
end)
CreateToggle(SecBFChain, "Lock Mobile Dash Button", false, function(state) 
    mobileBtnLocked = state 
    if state then
        lockDot.BackgroundColor3 = Color3.fromRGB(255, 200, 50)
        dashStroke.Color = Color3.fromRGB(255, 200, 50)
    else
        lockDot.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
        dashStroke.Color = Color3.fromRGB(90, 90, 90)
    end
end)
CreateKeybind(SecBFChain, "Action Keybind (Dash+BF)", Enum.KeyCode.R, function(key) RyuConfig.BFActionKey = key end)
CreateSlider(SecBFChain, "Max Dash Distance", 5, 50, 15, function(val) RyuConfig.DashDistance = val end)
CreateSlider(SecBFChain, "Dash Duration (s)", 10, 100, 35, function(val) RyuConfig.DashDuration = val/100 end)
CreateSlider(SecBFChain, "Combo Fire Delay (s)", 10, 100, 25, function(val) RyuConfig.FireDelay = val/100 end)
CreateToggle(SecBFChain, "Camera Lock During Dash", true, function(state) RyuConfig.DashCameraLock = state end)
CreateDropdown(SecBFChain, "Dash Easing Style", {"Linear", "Sine", "Quad", "Cubic", "Quart", "Quint", "Expo", "Circ", "Elastic", "Back", "Bounce"}, "Cubic", function(val) RyuConfig.DashEasingStyle = val end)
CreateDropdown(SecBFChain, "Dash Easing Direction", {"In", "Out", "InOut"}, "Out", function(val) RyuConfig.DashEasingDirection = val end)

local SubDash = CreateSubTab(TabCombat, "Side Dash Assist")
local SecDash = CreateSection(SubDash, "Side Dash Settings")
CreateToggle(SecDash, "Enable Side Dash Assist", false, function(s) RyuConfig.DashAssistEnabled = s end)
CreateToggle(SecDash, "Lock Camera On Enemy", false, function(s) RyuConfig.DashAssistCameraLock = s end)
CreateToggle(SecDash, "Dash Only If Facing Front", false, function(s) RyuConfig.DashAssistOnlyIfFacing = s end)
CreateKeybind(SecDash, "Dash Keybind", Enum.KeyCode.J, function(k) RyuConfig.DashAssistKeybind = k end)
CreateSlider(SecDash, "Detection Range", 1, 50, 15, function(v) RyuConfig.DashAssistDetectionRange = v end)
CreateSlider(SecDash, "Behind Distance", 1, 15, 5, function(v) RyuConfig.DashAssistBehindDistance = v end)
CreateSlider(SecDash, "Flight Duration (s)", 10, 100, 42, function(v) RyuConfig.DashAssistFlightDuration = v/100 end)
local SecDashArc = CreateSection(SubDash, "Arc Modifiers")
CreateSlider(SecDashArc, "Curve Strength", 0, 25, 10, function(v) RyuConfig.DashAssistCurveStrength = v end)
CreateSlider(SecDashArc, "Arch Height", 0, 10, 3, function(v) RyuConfig.DashAssistArchHeight = v end)
CreateSlider(SecDashArc, "Lock Duration (s)", 10, 150, 35, function(v) RyuConfig.DashAssistLockDuration = v/100 end)

local SubLock = CreateSubTab(TabCombat, "Target Lock")
local SecLock = CreateSection(SubLock, "Lock Settings")
CreateToggle(SecLock, "Enable Target Lock", false, function(state) RyuConfig.LockEnabled = state end)
CreateDropdown(SecLock, "Lock Method", {"Camera", "Body"}, "Camera", function(val) RyuConfig.LockMethod = val end)
CreateDropdown(SecLock, "Target Mode", {"Closest", "Closest to Mouse"}, "Closest", function(val) RyuConfig.LockTargetMode = val end)
CreateDropdown(SecLock, "Target Part", {"HumanoidRootPart", "Head", "UpperTorso", "LowerTorso"}, "HumanoidRootPart", function(val) RyuConfig.LockTargetPart = val end)
CreateSlider(SecLock, "Lock Smoothness", 0, 20, 0, function(val) RyuConfig.LockSmoothness = val end)
CreateSlider(SecLock, "Lock Side Offset", -8, 8, 1, function(val) RyuConfig.LockSideOffset = val end)
CreateSlider(SecLock, "Max Lock Distance", 10, 2000, 500, function(val) RyuConfig.LockMaxDistance = val end)
CreateSlider(SecLock, "Prediction", 0, 100, 0, function(val) RyuConfig.LockPrediction = val/100 end)
CreateToggle(SecLock, "Wall Check", false, function(state) RyuConfig.LockWallCheck = state end)

-- TAB 2: FARM
local TabFarm = CreateMainTab("Farm")
local SubAIFarm = CreateSubTab(TabFarm, "AI Auto Farm")
local SecAIFarm = CreateSection(SubAIFarm, "Auto Combat")

CreateToggle(SecAIFarm, "Enable AI Farm", false, function(state) RyuConfig.AutoFarm = state end)
CreateToggle(SecAIFarm, "Use Tween Pathfinder", false, function(state) RyuConfig.FarmTween = state end)
CreateDropdown(SecAIFarm, "Combat Mode", {"Combo", "Spam"}, "Combo", function(state) RyuConfig.FarmMode = state end)
CreateSlider(SecAIFarm, "Chase Range", 10, 2000, 500, function(val) RyuConfig.FarmRange = val end)

local SecAISkills = CreateSection(SubAIFarm, "Choose Skills")
CreateToggle(SecAISkills, "Use Skill 1", true, function(state) RyuConfig.FarmSkill1 = state end)
CreateToggle(SecAISkills, "Use Skill 2", true, function(state) RyuConfig.FarmSkill2 = state end)
CreateToggle(SecAISkills, "Use Skill 3", true, function(state) RyuConfig.FarmSkill3 = state end)
CreateToggle(SecAISkills, "Use Skill 4", true, function(state) RyuConfig.FarmSkill4 = state end)

local SubMFarm = CreateSubTab(TabFarm, "Money Farm")
local SecMFarm = CreateSection(SubMFarm, "Alt Money Farm")
CreateToggle(SecMFarm, "Enable Alt Farm", false, function(state) RyuConfig.AltFarmEnabled = state end)
CreateDropdown(SecMFarm, "Role", {"None", "Main", "Casher"}, "None", function(state) RyuConfig.AltRole = state end)
CreateInput(SecMFarm, "Main Username", function(text) RyuConfig.AltMainName = text end)

local SecGroupRejoin = CreateSection(SubMFarm, "Group Rejoin")
CreateToggle(SecGroupRejoin, "Auto Rejoin Low Pop", false, function(state) RyuConfig.GroupRejoin = state end)
CreateSlider(SecGroupRejoin, "Rejoin if players < X", 3, 12, 5, function(val) RyuConfig.GroupRejoinCount = val end)

local SubTeleport = CreateSubTab(TabFarm, "Travel")
local SecTeleport = CreateSection(SubTeleport, "Destination Travel")

local selectedDest = LocationNames[1]
CreateDropdown(SecTeleport, "Destination", LocationNames, "Dest", function(val) selectedDest = val end)
CreateSlider(SecTeleport, "Speed (Studs/s)", 50, 1000, 350, function(val) Logic.Pathfinding.Speed = val end)
CreateButton(SecTeleport, "Start Teleport", Theme.SectionBG, function()
    local pos = TeleportLocations[selectedDest]
    if pos then startPathfinding(pos) end
end)
CreateButton(SecTeleport, "Stop Teleport", Theme.SectionBG, function()
    stopPathfinding()
end)

-- TAB 3: SETTINGS
local TabSettings = CreateMainTab("Settings")
local SubSettings = CreateSubTab(TabSettings, "Settings")
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

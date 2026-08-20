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

--// ==========================================
--// RAZORBILL COMBAT LOGIC STATE[cite: 1]
--// ==========================================
local RyuConfig = {
    -- Auto BF & Chain
    AutoBFChain = false,
    BFChainKey = Enum.KeyCode.E,
    BFActionKey = Enum.KeyCode.R,
    AutoBFYuji = false,
    AutoBFMahito = false,
    
    -- Target Lock[cite: 1]
    LockEnabled = false,
    LockMethod = "Camera", -- Camera / Body
    LockTargetMode = "Closest",
    LockTargetPart = "HumanoidRootPart",
    LockMaxDistance = 500,
    LockPrediction = 0,
    LockSmoothness = 0,
    LockSideOffset = 1.75,
    LockWallCheck = false,
    
    -- Dash Assist Arc Settings[cite: 1]
    DashDistance = 15,
    FireDelay = 0.25,
    DashDuration = 0.35,
    DashCameraLock = true,
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
}

local DashAnimLeft = Instance.new("Animation")
DashAnimLeft.AnimationId = "rbxassetid://75203303352791"
local DashAnimRight = Instance.new("Animation")
DashAnimRight.AnimationId = "rbxassetid://117223862448096"

local isMobilePlayer = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

--// ==========================================
--// TARGET LOCK LOGIC (RAZORBILL EXTRACTED)[cite: 1]
--// ==========================================
local function getHRP(character)
    return character and (character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("Torso") or character:FindFirstChild("UpperTorso"))
end

local function getLockTarget()
    if tick() - Logic.LockState.LastTargetSearch < 0.5 then return nil end
    Logic.LockState.LastTargetSearch = tick()

    local char = LocalPlayer.Character
    if not char then return nil end
    local root = getHRP(char)
    if not root then return nil end

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
                if worldDist > RyuConfig.LockMaxDistance then continue end
                
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
    return best
end

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
        Logic.LockState.CurrentLockTarget = getLockTarget()
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
        -- BODY GYRO LOCK[cite: 1]
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
--// BLACK FLASH / OFFENSIVE LOGIC[cite: 1]
--// ==========================================
local function fireActivatedRemote()
    pcall(function()
        local char = LocalPlayer.Character
        if not char then return end
        local moveset = char:FindFirstChild("Moveset")
        if not moveset then return end
        
        local move = moveset:FindFirstChild("Divergent Fist") or moveset:FindFirstChild("Focus Strike")
        if move then
            local re = ReplicatedStorage.Knit.Knit.Services.DivergentFistService.RE.Activated
            if re then re:FireServer(move) end
        end
    end)
end

local function triggerBlackFlash()
    if isMobilePlayer then
        fireActivatedRemote()
    else
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Three, false, game)
        task.wait()
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Three, false, game)
    end
end

-- Auto Black Flash Hook
local function setupCharacterBF(character)
    local humanoid = character:WaitForChild("Humanoid", 5)
    if not humanoid then return end
    local animator = humanoid:WaitForChild("Animator", 5)
    if not animator then return end
    
    animator.AnimationPlayed:Connect(function(track)
        local animId = track.Animation and track.Animation.AnimationId or ""
        
        -- Side Dash Assist Hook[cite: 1]
        if animId:match("75203303352791") then
            -- Optional side dash logic hook
        elseif animId:match("117223862448096") then
            -- Optional side dash logic hook
        end

        if not (RyuConfig.AutoBFYuji or RyuConfig.AutoBFMahito or RyuConfig.AutoBFChain) then return end
        local idMatch = string.match(animId, "%d+")
        
        if idMatch and Logic.TargetAnimations[idMatch] then
            if (tick() - Logic.LastFiredTick) <= Logic.TIME_WINDOW then
                Logic.LastFiredTick = 0
                local target = getClosestTarget(RyuConfig.DashDistance)
                if target and RyuConfig.AutoBFChain then
                    -- handled below
                else
                    task.delay(RyuConfig.FireDelay, triggerBlackFlash)
                end
            end
        end
    end)
end

if LocalPlayer.Character then setupCharacterBF(LocalPlayer.Character) end
LocalPlayer.CharacterAdded:Connect(function(char)
    task.wait(0.3)
    setupCharacterBF(char)
end)


--// ==========================================
--// BLACK FLASH CHAIN LOGIC (CURVE GLIDE & MOBILE UI)[cite: 1]
--// ==========================================
local ChainMobileGui = Instance.new("ScreenGui")
ChainMobileGui.Name = "RyuChainMobileGui"
ChainMobileGui.ResetOnSpawn = false
ChainMobileGui.Parent = guiParent

local BTN_SIZE = 62
local mobileBtnLocked = false
local mobileBtnDragging = false
local mobileBtnDragOff = Vector2.new(0, 0)

local dashBtn = Instance.new("TextButton")
dashBtn.Name = "DashButton"
dashBtn.Size = UDim2.new(0, BTN_SIZE, 0, BTN_SIZE)
dashBtn.Position = UDim2.new(1, -(BTN_SIZE + 20), 1, -(BTN_SIZE * 3 + 20))
dashBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
dashBtn.BackgroundTransparency = 0.25
dashBtn.Text = ""
dashBtn.BorderSizePixel = 0
dashBtn.Visible = false
dashBtn.ZIndex = 10
dashBtn.ClipsDescendants = false
dashBtn.Parent = ChainMobileGui
Instance.new("UICorner", dashBtn).CornerRadius = UDim.new(1, 0)

local dashStroke = Instance.new("UIStroke", dashBtn)
dashStroke.Color = Color3.fromRGB(90, 90, 90)
dashStroke.Thickness = 2

local dashIcon = Instance.new("TextLabel")
dashIcon.Size = UDim2.new(0, 36, 0, 36)
dashIcon.Position = UDim2.new(0.5, -18, 0.5, -22)
dashIcon.BackgroundTransparency = 1
dashIcon.Text = "✦"
dashIcon.TextColor3 = Color3.fromRGB(220, 220, 220)
dashIcon.TextSize = 22
dashIcon.Font = Enum.Font.GothamBold
dashIcon.TextXAlignment = Enum.TextXAlignment.Center
dashIcon.TextYAlignment = Enum.TextYAlignment.Center
dashIcon.ZIndex = 11
dashIcon.Parent = dashBtn

local dashLabel = Instance.new("TextLabel")
dashLabel.Size = UDim2.new(1, 0, 0, 13)
dashLabel.Position = UDim2.new(0, 0, 1, -16)
dashLabel.BackgroundTransparency = 1
dashLabel.Text = "DASH"
dashLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
dashLabel.TextSize = 9
dashLabel.Font = Enum.Font.Gotham
dashLabel.TextXAlignment = Enum.TextXAlignment.Center
dashLabel.ZIndex = 11
dashLabel.Parent = dashBtn

local lockDot = Instance.new("Frame")
lockDot.Size = UDim2.new(0, 10, 0, 10)
lockDot.Position = UDim2.new(1, -3, 0, -3)
lockDot.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
lockDot.BorderSizePixel = 0
lockDot.ZIndex = 12
lockDot.Parent = dashBtn
Instance.new("UICorner", lockDot).CornerRadius = UDim.new(1, 0)

dashBtn.MouseButton1Down:Connect(function()
    if not mobileBtnLocked then
        mobileBtnDragging = true
        local abs = dashBtn.AbsolutePosition
        local mpos = UserInputService:GetMouseLocation()
        mobileBtnDragOff = Vector2.new(mpos.X - abs.X, mpos.Y - abs.Y)
    end
end)
dashBtn.MouseButton1Up:Connect(function() mobileBtnDragging = false end)
UserInputService.InputChanged:Connect(function(input)
    if not mobileBtnDragging or mobileBtnLocked then return end
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        local pos = input.Position
        local ss = ChainMobileGui.AbsoluteSize
        local nx = math.clamp(pos.X - mobileBtnDragOff.X, 0, ss.X - BTN_SIZE)
        local ny = math.clamp(pos.Y - mobileBtnDragOff.Y, 0, ss.Y - BTN_SIZE)
        dashBtn.Position = UDim2.new(0, nx, 0, ny)
    end
end)

local function getClosestTarget(maxDist)
    local char = Player.Character
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

-- Bezier Curve Glide Dash[cite: 1]
local function performDashLogic(target)
    local char = LocalPlayer.Character
    local root = getHRP(char)
    local humanoid = char and char:FindFirstChildOfClass("Humanoid")
    local enemyRoot = getHRP(target)

    if not root or not enemyRoot then
        task.delay(RyuConfig.FireDelay, triggerBlackFlash)
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
            triggerBlackFlash()
        end
    end

    dashConn = RunService.Heartbeat:Connect(function()
        if not root or not root.Parent then return finalizeMovement(nil) end
        local elapsed = tick() - startTime

        if elapsed >= RyuConfig.FireDelay and not hasFired then
            hasFired = true
            triggerBlackFlash()
        end

        local currentPos = startPos
        if isDash then
            local alpha = math.clamp(elapsed / RyuConfig.DashDuration, 0, 1)
            local easedAlpha = TweenService:GetValue(alpha, Enum.EasingStyle.Cubic, Enum.EasingDirection.Out)

            if dashType == "Straight" then
                currentPos = startPos:Lerp(endPos, easedAlpha)
            elseif dashType == "Arch" then
                currentPos = (1 - easedAlpha)^2 * startPos + 2 * (1 - easedAlpha) * easedAlpha * controlPos + easedAlpha^2 * endPos
            end
        end

        root.CFrame = CFrame.lookAt(currentPos, initialEnemyPos)

        if RyuConfig.DashCameraLock then
            local cam = workspace.CurrentCamera
            if cam then cam.CFrame = CFrame.lookAt(cam.CFrame.Position, initialEnemyPos) end
        end

        local totalWaitTime = isDash and (RyuConfig.DashDuration + 0.1) or (RyuConfig.FireDelay + 0.1)
        if elapsed >= totalWaitTime then
            finalizeMovement(CFrame.lookAt(currentPos, initialEnemyPos))
        end
    end)
end

dashBtn.MouseButton1Click:Connect(function()
    if not RyuConfig.AutoBFChain then return end
    local target = getClosestTarget(RyuConfig.DashDistance)
    if target then
        performDashLogic(target)
    else
        task.delay(RyuConfig.FireDelay, triggerBlackFlash)
    end
end)

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == RyuConfig.BFActionKey then
        if RyuConfig.AutoBFChain then
            local target = getClosestTarget(RyuConfig.DashDistance)
            if target then performDashLogic(target) else task.delay(RyuConfig.FireDelay, triggerBlackFlash) end
        end
    end
end)

--// ==========================================
--// TWEEN & PATHFINDING TELEPORTS[cite: 1]
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

local SecAutoOff = CreateSection(SubAuto, "Offensive")
CreateToggle(SecAutoOff, "Auto Black Flash (Yuji)", false, function(state) RyuConfig.AutoBFYuji = state end)
CreateToggle(SecAutoOff, "Auto Black Flash (Mahito)", false, function(state) RyuConfig.AutoBFMahito = state end)

local SecBFChain = CreateSection(SubAuto, "Blackflash Chain")
CreateToggle(SecBFChain, "Enable Black Flash Chain", false, function(state) 
    RyuConfig.AutoBFChain = state 
    if state then
        if isMobilePlayer then
            dashBtn.Visible = true
        end
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


-- TAB 2: TELEPORTS
local TabTeleports = CreateMainTab("Teleports")
local SubTeleport = CreateSubTab(TabTeleports, "Travel")
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

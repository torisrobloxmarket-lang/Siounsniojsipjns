--// ==========================================
--// RYU HUB - UI OVERLAY (RAZORBILL EDITION)
--// 100% MONOCHROME CLEAN TEMPLATE
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local PathfindingService = game:GetService("PathfindingService")

local Player = Players.LocalPlayer
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
if not guiParent then guiParent = Player:WaitForChild("PlayerGui") end

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

--// NOTIFICATIONS (Replacing Library:Notify)
local NotificationContainer = Instance.new("Frame")
NotificationContainer.Name = "RyuNotifications"
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
    NotifFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 20)
    NotifFrame.BackgroundTransparency = 1
    NotifFrame.BorderSizePixel = 0
    Instance.new("UICorner", NotifFrame).CornerRadius = UDim.new(0, 8)
    
    local Stroke = Instance.new("UIStroke", NotifFrame)
    Stroke.Color = Color3.fromRGB(255, 255, 255); Stroke.Transparency = 1; Stroke.Thickness = 1.5
    
    local AccentLine = Instance.new("Frame", NotifFrame)
    AccentLine.Size = UDim2.new(0, 3, 0.8, 0); AccentLine.Position = UDim2.new(0, 4, 0.1, 0)
    AccentLine.BackgroundColor3 = Color3.fromRGB(255, 255, 255); AccentLine.BackgroundTransparency = 1
    Instance.new("UICorner", AccentLine).CornerRadius = UDim.new(1, 0)
    
    local TitleLabel = Instance.new("TextLabel", NotifFrame)
    TitleLabel.Size = UDim2.new(1, -20, 0, 20); TitleLabel.Position = UDim2.new(0, 15, 0, 8)
    TitleLabel.BackgroundTransparency = 1; TitleLabel.Text = title; TitleLabel.TextColor3 = Color3.fromRGB(250, 250, 250)
    TitleLabel.TextTransparency = 1; TitleLabel.Font = Enum.Font.GothamBold; TitleLabel.TextSize = 13; TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    
    local DescLabel = Instance.new("TextLabel", NotifFrame)
    DescLabel.Size = UDim2.new(1, -20, 0, 20); DescLabel.Position = UDim2.new(0, 15, 0, 28)
    DescLabel.BackgroundTransparency = 1; DescLabel.Text = text; DescLabel.TextColor3 = Color3.fromRGB(130, 130, 135)
    DescLabel.TextTransparency = 1; DescLabel.Font = Enum.Font.Gotham; DescLabel.TextSize = 11; DescLabel.TextXAlignment = Enum.TextXAlignment.Left

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
        fadeOut:Play(); fadeOut.Completed:Wait(); NotifFrame:Destroy()
    end)
end

local Library = {
    Notify = function(self, args)
        RyuNotify:Send(args.Title or "Notification", args.Description or "", args.Time or 3)
    end
}

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
ToggleBtn.Size = UDim2.new(0, 50, 0, 50); ToggleBtn.Position = UDim2.new(0, 15, 0, 60); ToggleBtn.BackgroundColor3 = Theme.Sidebar; ToggleBtn.Text = ""; ToggleBtn.Parent = RyuHub
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
local btnStroke = Instance.new("UIStroke", ToggleBtn); btnStroke.Color = Theme.Accent; btnStroke.Thickness = 2; btnStroke.Transparency = 0.5
local Katana = Instance.new("Frame", ToggleBtn); Katana.Size = UDim2.new(1, 0, 1, 0); Katana.BackgroundTransparency = 1; Katana.Rotation = 45
local Blade = Instance.new("Frame", Katana); Blade.Size = UDim2.new(0, 2, 0, 24); Blade.Position = UDim2.new(0.5, -1, 0.5, -18); Blade.BackgroundColor3 = Theme.Text; Blade.BorderSizePixel = 0
local Guard = Instance.new("Frame", Katana); Guard.Size = UDim2.new(0, 12, 0, 2); Guard.Position = UDim2.new(0.5, -6, 0.5, 6); Guard.BackgroundColor3 = Theme.SubText; Guard.BorderSizePixel = 0
local Handle = Instance.new("Frame", Katana); Handle.Size = UDim2.new(0, 4, 0, 10); Handle.Position = UDim2.new(0.5, -2, 0.5, 8); Handle.BackgroundColor3 = Theme.Stroke; Handle.BorderSizePixel = 0
Instance.new("UICorner", Blade).CornerRadius = UDim.new(1, 0); Instance.new("UICorner", Guard).CornerRadius = UDim.new(1, 0); Instance.new("UICorner", Handle).CornerRadius = UDim.new(0, 1)
AddClickPop(ToggleBtn)

--// MAIN CONTAINER
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 0, 0, 0); MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0); MainFrame.BackgroundColor3 = Theme.Background; MainFrame.BorderSizePixel = 0; MainFrame.Visible = false; MainFrame.ClipsDescendants = true; MainFrame.Active = true; MainFrame.Parent = RyuHub
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)
local mainStroke = Instance.new("UIStroke", MainFrame); mainStroke.Color = Theme.Stroke; mainStroke.Thickness = 1.5

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
ContentWrapper.Size = UDim2.new(1, 0, 1, 0); ContentWrapper.BackgroundTransparency = 1; ContentWrapper.BorderSizePixel = 0
local Topbar = Instance.new("Frame", ContentWrapper)
Topbar.Size = UDim2.new(1, 0, 0, 60); Topbar.BackgroundTransparency = 1
local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 300, 1, 0); Title.Position = UDim2.new(0, 20, 0, 0); Title.BackgroundTransparency = 1; Title.Text = "RYU HUB"; Title.Font = Enum.Font.GothamBlack; Title.TextSize = 22; Title.TextXAlignment = Enum.TextXAlignment.Left; Title.TextColor3 = Theme.Text
local SubTitle = Instance.new("TextLabel", Topbar)
SubTitle.Size = UDim2.new(0, 300, 0, 15); SubTitle.Position = UDim2.new(0, 20, 0, 38); SubTitle.BackgroundTransparency = 1; SubTitle.Text = "Razorbill Logic Edition"; SubTitle.TextColor3 = Theme.SubText; SubTitle.Font = Enum.Font.Gotham; SubTitle.TextSize = 11; SubTitle.TextXAlignment = Enum.TextXAlignment.Left
local CloseBtn = Instance.new("TextButton", Topbar)
CloseBtn.Size = UDim2.new(0, 28, 0, 28); CloseBtn.Position = UDim2.new(1, -40, 0, 15); CloseBtn.BackgroundColor3 = Theme.SectionBG; CloseBtn.Text = "X"; CloseBtn.TextColor3 = Theme.Text; CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.TextSize = 14
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6); Instance.new("UIStroke", CloseBtn).Color = Theme.Stroke
CloseBtn.MouseButton1Click:Connect(function()
    pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end)
    task.delay(0.3, function() MainFrame.Visible = false end)
end)

local mDragging, mDragStart, mStartPos = false, nil, nil
Topbar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then mDragging = true; mDragStart = input.Position; mStartPos = MainFrame.Position end
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
    tabBtn.Size = UDim2.new(1, 0, 0, 36); tabBtn.BackgroundColor3 = Theme.Sidebar; tabBtn.Text = "  " .. string.upper(name); tabBtn.TextColor3 = Theme.SubText; tabBtn.Font = Enum.Font.GothamBlack; tabBtn.TextSize = 13; tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8); tabObj.Btn = tabBtn

    local subContainer = Instance.new("Frame", Sidebar)
    subContainer.Size = UDim2.new(1, 0, 0, 0); subContainer.BackgroundTransparency = 1; subContainer.ClipsDescendants = true; tabObj.SubContainer = subContainer

    local subLayout = Instance.new("UIListLayout", subContainer)
    subLayout.Padding = UDim.new(0, 2); subLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left; subLayout.SortOrder = Enum.SortOrder.LayoutOrder; tabObj.SubLayout = subLayout

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
    subBtn.Size = UDim2.new(1, 0, 0, 28); subBtn.BackgroundTransparency = 1; subBtn.Text = "     " .. subName; subBtn.TextColor3 = Theme.SubText; subBtn.Font = Enum.Font.GothamMedium; subBtn.TextSize = 12; subBtn.TextXAlignment = Enum.TextXAlignment.Left; subObj.Btn = subBtn

    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0); page.BackgroundTransparency = 1; page.ScrollBarThickness = 2; page.ScrollBarImageColor3 = Theme.Accent; page.Visible = false; subObj.Page = page

    local pageLayout = Instance.new("UIListLayout", page)
    pageLayout.Padding = UDim.new(0, 12); pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
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
        page.Visible = true; subBtn.TextColor3 = Theme.Text
    end)
    table.insert(tabObj.SubTabs, subObj)
    return page
end

local function CreateSection(page, titleText)
    local section = Instance.new("Frame", page)
    section.Size = UDim2.new(0.98, 0, 0, 50); section.BackgroundColor3 = Theme.SectionBG; section.BackgroundTransparency = 0
    Instance.new("UICorner", section).CornerRadius = UDim.new(0, 10); Instance.new("UIStroke", section).Color = Theme.Stroke
    
    local secLayout = Instance.new("UIListLayout", section)
    secLayout.Padding = UDim.new(0, 10); secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center; secLayout.SortOrder = Enum.SortOrder.LayoutOrder
    local secPadding = Instance.new("UIPadding", section); secPadding.PaddingTop = UDim.new(0, 12); secPadding.PaddingBottom = UDim.new(0, 12)
    
    local title = Instance.new("TextLabel", section)
    title.LayoutOrder = -1; title.Size = UDim2.new(0.92, 0, 0, 24); title.BackgroundTransparency = 1; title.Text = titleText; title.TextColor3 = Theme.Text; title.Font = Enum.Font.GothamBold; title.TextSize = 14; title.TextXAlignment = Enum.TextXAlignment.Left
    secLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() section.Size = UDim2.new(1, 0, 0, secLayout.AbsoluteContentSize.Y + 24) end)
    return section
end

local function CreateLabel(section, text)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 30); frame.BackgroundTransparency = 1
    local lbl = Instance.new("TextLabel", frame); lbl.Size = UDim2.new(1, 0, 1, 0); lbl.BackgroundTransparency = 1; lbl.Text = text; lbl.TextColor3 = Theme.SubText; lbl.Font = Enum.Font.GothamMedium; lbl.TextSize = 11; lbl.TextXAlignment = Enum.TextXAlignment.Left; lbl.TextWrapped = true
    return lbl
end

local function CreateToggle(section, text, defaultState, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 34); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(0.7, 0, 0, 34); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = defaultState and Theme.Text or Theme.SubText; label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    
    local tBtn = Instance.new("TextButton", frame); tBtn.Size = UDim2.new(0, 42, 0, 22); tBtn.Position = UDim2.new(1, -42, 0, 6); tBtn.BackgroundColor3 = defaultState and Theme.ToggleOn or Theme.ToggleOff; tBtn.Text = ""; Instance.new("UICorner", tBtn).CornerRadius = UDim.new(1, 0)
    local circle = Instance.new("Frame", tBtn); circle.Size = UDim2.new(0, 16, 0, 16); circle.Position = defaultState and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8); circle.BackgroundColor3 = defaultState and Theme.Background or Color3.fromRGB(150, 150, 150); Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
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
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 50); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(1, -40, 0, 18); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Theme.SubText; label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    local valLabel = Instance.new("TextLabel", frame); valLabel.Size = UDim2.new(0, 40, 0, 18); valLabel.Position = UDim2.new(1, -40, 0, 0); valLabel.BackgroundTransparency = 1; valLabel.Text = tostring(default); valLabel.TextColor3 = Theme.Accent; valLabel.Font = Enum.Font.GothamBold; valLabel.TextSize = 13; valLabel.TextXAlignment = Enum.TextXAlignment.Right
    local sliderBg = Instance.new("Frame", frame); sliderBg.Size = UDim2.new(1, 0, 0, 4); sliderBg.Position = UDim2.new(0, 0, 0, 32); sliderBg.BackgroundColor3 = Theme.ToggleOff; Instance.new("UICorner", sliderBg).CornerRadius = UDim.new(1, 0)
    local percentage = math.clamp((default - min) / (max - min), 0, 1)
    local sliderFill = Instance.new("Frame", sliderBg); sliderFill.Size = UDim2.new(percentage, 0, 1, 0); sliderFill.BackgroundColor3 = Theme.Accent; Instance.new("UICorner", sliderFill).CornerRadius = UDim.new(1, 0)
    local knob = Instance.new("TextButton", sliderFill); knob.Size = UDim2.new(0, 14, 0, 14); knob.Position = UDim2.new(1, -7, 0.5, -7); knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255); knob.Text = ""; Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
    
    local dragging = false
    knob.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relative = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            local value = (min + (max - min) * relative)
            local isDecimal = (math.floor(min) ~= min or math.floor(max) ~= max)
            value = isDecimal and (math.floor(value * 100) / 100) or math.floor(value)
            valLabel.Text = tostring(value)
            sliderFill.Size = UDim2.new(relative, 0, 1, 0)
            if callback then pcall(function() callback(value) end) end
        end
    end)
end

local function CreateButton(section, text, color, callback)
    if type(color) == "function" then callback = color; color = Theme.SectionBG end
    itemOrderCounter = itemOrderCounter + 1
    local btn = Instance.new("TextButton", section); btn.LayoutOrder = itemOrderCounter; btn.Size = UDim2.new(0.92, 0, 0, 34); btn.BackgroundColor3 = color; btn.Text = text; btn.TextColor3 = Color3.fromRGB(255, 255, 255); btn.Font = Enum.Font.GothamBold; btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6); Instance.new("UIStroke", btn).Color = Theme.Stroke
    AddClickPop(btn)
    btn.MouseButton1Click:Connect(function() pcall(callback) end)
    return btn
end

local function CreateDropdown(section, headerText, itemsList, targetConfigKey, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 160); frame.BackgroundTransparency = 1
    local header = Instance.new("TextLabel", frame); header.Size = UDim2.new(1, 0, 0, 20); header.BackgroundTransparency = 1; header.Text = headerText .. ": None"; header.TextColor3 = Theme.SubText; header.Font = Enum.Font.GothamMedium; header.TextSize = 12; header.TextXAlignment = Enum.TextXAlignment.Left
    
    local scroll = Instance.new("ScrollingFrame", frame); scroll.Size = UDim2.new(1, 0, 0, 130); scroll.Position = UDim2.new(0, 0, 0, 25); scroll.BackgroundColor3 = Theme.Background; scroll.ScrollBarThickness = 4; Instance.new("UICorner", scroll).CornerRadius = UDim.new(0, 6)
    local listLayout = Instance.new("UIListLayout", scroll); listLayout.Padding = UDim.new(0, 4); listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    
    local function populate(list)
        for _, child in pairs(scroll:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        for _, itemName in ipairs(list) do
            local btn = Instance.new("TextButton", scroll); btn.Size = UDim2.new(0.94, 0, 0, 26); btn.BackgroundColor3 = Theme.SectionBG; btn.Text = "  " .. tostring(itemName); btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; btn.TextXAlignment = Enum.TextXAlignment.Left; Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
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
    local frame = Instance.new("Frame", section); frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 34); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(0.7, 0, 0, 34); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Theme.Text; label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    local btn = Instance.new("TextButton", frame); btn.Size = UDim2.new(0, 80, 0, 22); btn.Position = UDim2.new(1, -80, 0, 6); btn.BackgroundColor3 = Theme.ToggleOff; btn.Text = defaultKey.Name; btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    
    local waiting = false
    btn.MouseButton1Click:Connect(function() waiting = true; btn.Text = "..." end)
    UserInputService.InputBegan:Connect(function(input)
        if waiting and input.UserInputType == Enum.UserInputType.Keyboard then
            waiting = false; btn.Text = input.KeyCode.Name
            if callback then pcall(function() callback(input.KeyCode) end) end
        end
    end)
end

--// ==========================================
--// MOCK CONFIGURATION & STATE
--// ==========================================

local Toggles = {
    BlackFlashEnabled = {Value = false},
    CameraLockEnabled = {Value = true},
    ESPBox = {Value = false}, ESPCorner = {Value = false}, ESPOutline = {Value = false},
    ESPSkeleton = {Value = false}, ESPHeadDot = {Value = false}, ESPTracer = {Value = false},
    ESPName = {Value = false}, ESPCharacter = {Value = false}, ESPDistance = {Value = false},
    ESPKill = {Value = false}, ESPHPText = {Value = false}, ESPHPBar = {Value = false},
    CooldownRevealer = {Value = false}, EnableLock = {Value = false}, LockESP = {Value = false},
    LockWallCheck = {Value = false}, LockAutoRetarget = {Value = false}, LockNotification = {Value = true},
    EnableDashAssist = {Value = false}, DashCameraLock = {Value = false}, DashOnlyIfFacing = {Value = false},
    TeleportVisualize = {Value = true}
}

local Options = {
    DashDistance = {Value = 15}, FireDelay = {Value = 0.25}, DashDuration = {Value = 0.35}, LockTime = {Value = 0.1},
    DashEasingStyle = {Value = "Cubic"}, DashEasingDirection = {Value = "Out"},
    
    ESPBoxColor = {Value = Color3.fromRGB(255, 65, 65)}, ESPCornerColor = {Value = Color3.fromRGB(0, 255, 255)},
    ESPOutlineColor = {Value = Color3.fromRGB(255, 255, 255)}, ESPSkeletonColor = {Value = Color3.fromRGB(190, 90, 255)},
    ESPHeadDotColor = {Value = Color3.fromRGB(255, 255, 0)}, ESPTracerColor = {Value = Color3.fromRGB(85, 255, 85)},
    ESPNameColor = {Value = Color3.fromRGB(255, 255, 255)}, ESPCharacterColor = {Value = Color3.fromRGB(180, 200, 255)},
    ESPDistanceColor = {Value = Color3.fromRGB(120, 200, 255)}, ESPKillColor = {Value = Color3.fromRGB(255, 170, 0)},
    
    LockKeybind = {Value = Enum.KeyCode.C, GetState = function() return Toggles.EnableLock.Value end},
    LockSpecificPlayer = {Value = "Auto", SetValues = function(self, vals) if self._uiRefresh then self._uiRefresh(vals) end end, SetValue = function(self, val) self.Value = val end},
    LockMethod = {Value = "Camera"}, LockTargetMode = {Value = "Closest"}, LockTargetPart = {Value = "HumanoidRootPart"},
    LockSmoothness = {Value = 0}, LockSideOffset = {Value = 1.75}, LockMaxDistance = {Value = 500}, LockPrediction = {Value = 0},
    TargetInfoColor = {Value = Color3.fromRGB(255, 255, 255)},
    
    DashAssistKeybind = {Value = Enum.KeyCode.J}, DashDetectionRange = {Value = 15}, DashBehindDistance = {Value = 5},
    DashFlightDuration = {Value = 0.42}, DashCurveStrength = {Value = 10}, DashArchHeight = {Value = 3}, DashLockDuration = {Value = 0.35},
    
    TeleportDestination = {Value = "Town of Beginnings"}, TeleportMode = {Value = "Tween"}, TeleportSpeed = {Value = 350},
    ServerDropdown = {Value = "Click 'Refresh' to load servers", SetValues = function(self, vals) if self._uiRefresh then self._uiRefresh(vals) end end, SetValue = function(self, val) self.Value = val end},
    ServerSortBy = {Value = "Default (Roblox)"}, MinPlayersFilter = {Value = 0}, MaxPingFilter = {Value = 999}
}

--// ==========================================
--// RAZORBILL LOGIC ENGINE
--// ==========================================
local Logic = {
    TIME_WINDOW = 2, LastFiredTick = 0,
    TargetAnimations = { ["100962226150441"] = true, ["95852624447551"] = true, ["74145636023952"] = true, ["123171106092050"] = true },
    DashAnimLeft = Instance.new("Animation"), DashAnimRight = Instance.new("Animation"),
    DashAssistState = { Enabled = false, CameraLock = false, OnlyIfFacing = false, DetectionRange = 60, BehindDistance = 5, FlightDuration = 0.42, CurveStrength = 10, ArchHeight = 3, LockDuration = 0.35 }, 
    Pathfinding = { Active = false, Speed = 350, VisualizeOn = true, CurrentTween = nil, VisualParts = {}, VisualFolder = nil },
    TargetRemote = nil, ESPObjects = {}, Connections = {}, ServerMap = {},
    ShadowColor = Color3.fromRGB(0, 0, 0),
    CooldownGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(20, 255, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 177, 255)) }),
    R15Bones = { {"Head","UpperTorso"}, {"UpperTorso","LowerTorso"}, {"UpperTorso","LeftUpperArm"},  {"LeftUpperArm","LeftLowerArm"},  {"LeftLowerArm","LeftHand"}, {"UpperTorso","RightUpperArm"}, {"RightUpperArm","RightLowerArm"},{"RightLowerArm","RightHand"}, {"LowerTorso","LeftUpperLeg"},  {"LeftUpperLeg","LeftLowerLeg"},  {"LeftLowerLeg","LeftFoot"}, {"LowerTorso","RightUpperLeg"}, {"RightUpperLeg","RightLowerLeg"},{"RightLowerLeg","RightFoot"} },
    R6Bones = { {"Head","Torso"}, {"Torso","Left Arm"}, {"Torso","Right Arm"}, {"Torso","Left Leg"}, {"Torso","Right Leg"} },
    LockSettings = { ESPEnabled = false, Sticky = false, Method = "Camera", TargetMode = "Closest", TargetPart = "HumanoidRootPart", Smoothness = 0, SideOffset = 1.75, MaxDistance = 500, WallCheck = false, ShowFOV = false, FOVRadius = 200, FOVFilled = false, FOVTransparency = 0.8, FOVSides = 60, Prediction = 0, SelectedPlayer = "Auto", ShowTargetInfo = false, LockNotification = true, AutoRetarget = false, TargetInfoPos = "Top" },
    LockState = { Enabled = false, LastTargetSearch = 0, WasLockedBody = false, ZoomDistance = 10, CurrentLockTarget = nil, CameraLocked = false, OrigCamType = Enum.CameraType.Custom, LockBodyGyro = nil, ESPGui = nil, FOVCircle = nil, TargetInfoName = nil, TargetInfoHP = nil, TargetInfoDist = nil, TargetInfoLine = nil, LastTargetName = nil }
}
Logic.DashAnimLeft.AnimationId = "rbxassetid://75203303352791"
Logic.DashAnimRight.AnimationId = "rbxassetid://117223862448096"

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
Options.TeleportDestination.Value = LocationNames[1]

-- INVISIBILITY ENGINE
_G.MiscState = _G.MiscState or {}
local Invisibility = { Connections = {} }

function Invisibility.toggle(state, silent)
    local char = Player.Character
    if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local animator = hum and hum:FindFirstChildOfClass("Animator")
    if not (hum and hrp and animator) then return end
    local Camera = workspace.CurrentCamera

    if state then
        _G.MiscState.IsInvisible = true
        if not silent then
            hum.AutoRotate = false
            hrp.Anchored = true
        end

        Invisibility.Connections["InvisNoclip"] = RunService.Stepped:Connect(function()
            if not _G.MiscState.IsInvisible then return end
            for _, part in ipairs(char:GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end)

        _G.MiscState.InvisibleFakeTorso = Instance.new("Part")
        _G.MiscState.InvisibleFakeTorso.Name = "FakeTorso"
        _G.MiscState.InvisibleFakeTorso.Size = Vector3.new(2,2,1)
        _G.MiscState.InvisibleFakeTorso.Transparency = 1
        _G.MiscState.InvisibleFakeTorso.CanCollide = false
        _G.MiscState.InvisibleFakeTorso.Anchored = true
        _G.MiscState.InvisibleFakeTorso.Parent = workspace
        Camera.CameraType = Enum.CameraType.Custom
        Camera.CameraSubject = _G.MiscState.InvisibleFakeTorso

        if not silent then
            RunService:BindToRenderStep("MeditationFocus", Enum.RenderPriority.Camera.Value - 1, function()
                if not _G.MiscState.IsInvisible or not hrp or not hrp.Parent or not _G.MiscState.InvisibleFakeTorso then return end
                local camCF = Camera.CFrame
                local lookDir = Vector3.new(camCF.LookVector.X, 0, camCF.LookVector.Z)
                if lookDir.Magnitude > 0.001 then
                    hrp.CFrame = CFrame.new(hrp.Position, hrp.Position + lookDir.Unit)
                end
                _G.MiscState.InvisibleFakeTorso.CFrame = CFrame.new(hrp.Position + Vector3.new(0, 1.5, 0))
            end)
        end

        pcall(function()
            local animObj = ReplicatedStorage.Modules.MVP.Meditation.Character
            _G.MiscState.InvisibleTrack = animator:LoadAnimation(animObj)
            _G.MiscState.InvisibleTrack.Priority = Enum.AnimationPriority.Action4
            _G.MiscState.InvisibleTrack:Play()
            task.wait(0.1)
            _G.MiscState.InvisibleTrack.TimePosition = 0.1
            _G.MiscState.InvisibleTrack:AdjustSpeed(0)

            for _, t in ipairs(animator:GetPlayingAnimationTracks()) do
                if t ~= _G.MiscState.InvisibleTrack then t:Stop(0) end
            end

            Invisibility.Connections["InvisAnim"] = animator.AnimationPlayed:Connect(function(newTrack)
                if newTrack ~= _G.MiscState.InvisibleTrack then newTrack:Stop(0) end
            end)
        end)
        Library:Notify({ Title = "Invisibility", Description = "Invisibility Enabled", Time = 2 })
    else
        _G.MiscState.IsInvisible = false
        
        if Invisibility.Connections["InvisNoclip"] then 
            Invisibility.Connections["InvisNoclip"]:Disconnect() 
            Invisibility.Connections["InvisNoclip"] = nil 
        end
        if Invisibility.Connections["InvisAnim"] then 
            Invisibility.Connections["InvisAnim"]:Disconnect() 
            Invisibility.Connections["InvisAnim"] = nil 
        end
        pcall(function() RunService:UnbindFromRenderStep("MeditationFocus") end)

        if char and char.Parent then
            for _, part in ipairs(char:GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = true end
            end
        end
        if hrp and hrp.Parent then hrp.Anchored = false end
        if hum and hum.Parent then hum.AutoRotate = true; Camera.CameraSubject = hum end
        if _G.MiscState.InvisibleFakeTorso then 
            _G.MiscState.InvisibleFakeTorso:Destroy() 
            _G.MiscState.InvisibleFakeTorso = nil 
        end
        if _G.MiscState.InvisibleTrack then
            pcall(function() _G.MiscState.InvisibleTrack:Stop(0) end)
            pcall(function() _G.MiscState.InvisibleTrack:Destroy() end)
            _G.MiscState.InvisibleTrack = nil
        end
        Library:Notify({ Title = "Invisibility", Description = "Invisibility Disabled", Time = 2 })
    end
end

-- PATHFINDING ENGINE
local function getWaypointSpacing(speed)
    if speed <= 30 then return 4 elseif speed <= 100 then return 6 elseif speed <= 300 then return 10 elseif speed <= 600 then return 16 else return 24 end
end

local function clearPathVisualization()
    for _, p in ipairs(Logic.Pathfinding.VisualParts) do pcall(function() p:Destroy() end) end
    Logic.Pathfinding.VisualParts = {}
    if Logic.Pathfinding.VisualFolder and Logic.Pathfinding.VisualFolder.Parent then Logic.Pathfinding.VisualFolder:Destroy() end
    Logic.Pathfinding.VisualFolder = nil
end

local function visualizePath(waypoints)
    clearPathVisualization()
    if not Logic.Pathfinding.VisualizeOn then return end
    local folder = Instance.new("Folder"); folder.Name = "PathVisuals"; folder.Parent = workspace; Logic.Pathfinding.VisualFolder = folder
    for i, wp in ipairs(waypoints) do
        local isJump = (wp.Action == Enum.PathWaypointAction.Jump)
        local node = Instance.new("Part"); node.Shape = Enum.PartType.Ball; node.Size = isJump and Vector3.new(1.4, 1.4, 1.4) or Vector3.new(0.9, 0.9, 0.9); node.Position = wp.Position; node.Anchored = true; node.CanCollide = false; node.Material = Enum.Material.Neon; node.Transparency = 0.25; node.Color = isJump and Color3.fromRGB(255, 180, 30) or Color3.fromRGB(30, 180, 255); node.Parent = folder
        table.insert(Logic.Pathfinding.VisualParts, node)
        if i > 1 then
            local prevPos = waypoints[i - 1].Position; local currPos = wp.Position; local mid = (prevPos + currPos) / 2; local dist = (currPos - prevPos).Magnitude
            local line = Instance.new("Part"); line.Anchored = true; line.CanCollide = false; line.Material = Enum.Material.Neon; line.Color = Color3.fromRGB(50, 100, 255); line.Transparency = 0.55; line.Size = Vector3.new(0.22, 0.22, dist); line.CFrame = CFrame.lookAt(mid, currPos); line.Parent = folder
            table.insert(Logic.Pathfinding.VisualParts, line)
        end
    end
end

local function batchWaypoints(waypoints, minSegmentLength)
    local batched = {}; local lastAdded = waypoints[1]; table.insert(batched, waypoints[1])
    for i = 2, #waypoints do
        local wp = waypoints[i]; local isJump = (wp.Action == Enum.PathWaypointAction.Jump); local isLast = (i == #waypoints); local distFromLast = (wp.Position - lastAdded.Position).Magnitude
        if isJump or isLast or distFromLast >= minSegmentLength then table.insert(batched, wp); lastAdded = wp end
    end
    return batched
end

local function tweenToPosition(hrp, targetPos, speed)
    local distance = (hrp.Position - targetPos).Magnitude
    if distance < 0.5 then return true end
    local duration = math.max(distance / speed, 0.001)
    local direction = (targetPos - hrp.Position); local flatDir = Vector3.new(direction.X, 0, direction.Z); local targetCF = flatDir.Magnitude > 0.1 and CFrame.new(targetPos, targetPos + flatDir.Unit) or CFrame.new(targetPos) * hrp.CFrame.Rotation
    local tween = TweenService:Create(hrp, TweenInfo.new(duration, Enum.EasingStyle.Linear), { CFrame = targetCF })
    Logic.Pathfinding.CurrentTween = tween; tween:Play()
    local done = false; local conn; conn = tween.Completed:Connect(function() done = true; if conn then conn:Disconnect() end end)
    while not done and Logic.Pathfinding.Active do RunService.Heartbeat:Wait() end
    if not done and Logic.Pathfinding.CurrentTween == tween then tween:Cancel() end
    return done and Logic.Pathfinding.Active
end

local function stepToPosition(hrp, targetPos, speed)
    local startPos = hrp.Position; local direction = (targetPos - startPos); local distance = direction.Magnitude
    if distance < 0.5 then return true end
    local dirUnit = direction.Unit; local flatDir = Vector3.new(dirUnit.X, 0, dirUnit.Z); local lookCF = flatDir.Magnitude > 0.01 and CFrame.lookAt(Vector3.zero, flatDir) or CFrame.new()
    local traveled = 0
    while traveled < distance and Logic.Pathfinding.Active do
        local dt = RunService.Heartbeat:Wait(); local step = speed * dt; traveled = math.min(traveled + step, distance)
        hrp.CFrame = CFrame.new(startPos + dirUnit * traveled) * lookCF.Rotation
    end
    return traveled >= distance and Logic.Pathfinding.Active
end

local function stopPathfinding()
    Logic.Pathfinding.Active = false
    if _G.MiscState and _G.MiscState.IsInvisible then Invisibility.toggle(false, false) end
    if Logic.Pathfinding.CurrentTween then pcall(function() Logic.Pathfinding.CurrentTween:Cancel() end) end
    local hrp = Player.Character and Player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.Anchored = false end
    local hum = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end
    clearPathVisualization()
end

local function startPathfinding(targetPos)
    if Logic.Pathfinding.Active then stopPathfinding(); task.wait(0.15) end
    Logic.Pathfinding.Active = true
    task.spawn(function()
        while Logic.Pathfinding.Active do
            local speed = Logic.Pathfinding.Speed
            local agentParams = { AgentRadius = 3, AgentHeight = 6, AgentCanJump = true, AgentCanClimb = true, WaypointSpacing = getWaypointSpacing(speed) }
            local path = PathfindingService:CreatePath(agentParams)
            local hrp = Player.Character and Player.Character:FindFirstChild("HumanoidRootPart")
            local humanoid = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")
            if not hrp or not humanoid then task.wait(1) continue end
            if (hrp.Position - targetPos).Magnitude < 6 then Library:Notify({ Title = "Teleport", Description = "Destination reached!", Time = 3 }); stopPathfinding(); break end
            local ok, _ = pcall(function() path:ComputeAsync(hrp.Position, targetPos) end)
            if not ok or path.Status == Enum.PathStatus.NoPath then Library:Notify({ Title = "Teleport", Description = "No path found! Retrying...", Time = 2 }); task.wait(1.5); continue end
            local batchedWP = batchWaypoints(path:GetWaypoints(), math.max(speed * 0.05, 2))
            visualizePath(path:GetWaypoints())
            local pathBlocked = false; local blockedConn = path.Blocked:Connect(function() pathBlocked = true end)
            hrp.Anchored = true; humanoid.PlatformStand = true
            local completed = true
            for i = 2, #batchedWP do
                if not Logic.Pathfinding.Active or pathBlocked then completed = false break end
                local success
                if Logic.Pathfinding.Speed > 500 then success = stepToPosition(hrp, batchedWP[i].Position, Logic.Pathfinding.Speed)
                else success = tweenToPosition(hrp, batchedWP[i].Position, Logic.Pathfinding.Speed) end
                if not success then completed = false break end
            end
            if blockedConn then blockedConn:Disconnect() end
            if completed and Logic.Pathfinding.Active then Library:Notify({ Title = "Teleport", Description = "Destination reached!", Time = 3 }); stopPathfinding(); break end
            task.wait(0.3)
        end
    end)
end

-- ESP ENGINE
local function w2v(pos)
    local cam = workspace.CurrentCamera
    if not cam then return nil, false end
    local sp, on = cam:WorldToViewportPoint(pos)
    return Vector2.new(sp.X, sp.Y), on
end

local function hpColor(f)
    f = math.clamp(f, 0, 1)
    return Color3.fromRGB(255 * (1 - f), 255 * f, 50)
end

local function getCharBounds(char)
    local cam = workspace.CurrentCamera
    if not cam then return nil end
    local hrp = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
    if not hrp then return nil end
    local head = char:FindFirstChild("Head")
    local pos  = hrp.Position
    local topY = head and (head.Position.Y + 0.5) or (pos.Y + 2.8)
    local botY = pos.Y - 3.1
    local topWorld = Vector3.new(pos.X, topY, pos.Z)
    local botWorld = Vector3.new(pos.X, botY, pos.Z)
    local topSP, topOn = cam:WorldToViewportPoint(topWorld)
    local botSP, botOn = cam:WorldToViewportPoint(botWorld)
    if not topOn or not botOn then return nil end
    local h  = math.abs(botSP.Y - topSP.Y)
    local w  = h * 0.5
    local cx = (topSP.X + botSP.X) / 2
    local my = math.min(topSP.Y, botSP.Y)
    return { X = cx - w / 2, Y = my, W = w, H = h, CX = cx, CY = (topSP.Y + botSP.Y) / 2 }
end

local function getEntities()
    local out = {}
    local folder = workspace:FindFirstChild("Characters")
    local targetsToSearch = folder and folder:GetChildren() or workspace:GetChildren()
    for _, char in ipairs(targetsToSearch) do
        if char:IsA("Model") and char:FindFirstChildOfClass("Humanoid") then
            local plr = Players:GetPlayerFromCharacter(char)
            out[#out + 1] = { Model = char, Player = plr, Name = plr and plr.DisplayName or char.Name }
        end
    end
    return out
end

local function mkLine(thick)
    local l = Drawing.new("Line"); l.Visible = false; l.Thickness = thick or 1; return l
end

local function mkCircle(thick, filled)
    local c = Drawing.new("Circle"); c.Visible = false; c.Thickness = thick or 2; c.Filled = filled or false; c.NumSides = 30; return c
end

local function mkText(size)
    local t = Drawing.new("Text"); t.Visible = false; t.Size = size or 14; t.Center = true; t.Outline = true; t.OutlineColor = Color3.new(0, 0, 0); t.Font = 2; return t
end

local function CreateCooldownBillboard(character, player, rootPart)
    if not player or not rootPart then return nil, nil end
    local Reveal = Instance.new("BillboardGui"); Reveal.Name = "Reveal"; Reveal.SizeOffset = Vector2.new(1.1, 0); Reveal.StudsOffset = Vector3.new(0, 1, 0); Reveal.MaxDistance = 75; Reveal.ClipsDescendants = false; Reveal.AlwaysOnTop = true; Reveal.Size = UDim2.new(2, 0, 4, 0); Reveal.Adornee = rootPart; Reveal.Parent = rootPart
    local Moveset = Instance.new("Frame"); Moveset.Name = "Moveset"; Moveset.Visible = true; Moveset.ClipsDescendants = false; Moveset.BackgroundColor3 = Color3.fromRGB(255, 255, 255); Moveset.BackgroundTransparency = 1; Moveset.BorderSizePixel = 0; Moveset.Size = UDim2.new(0.45, 0, 1, 0); Moveset.Parent = Reveal
    local UIListLayout = Instance.new("UIListLayout"); UIListLayout.Name = "UIListLayout"; UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder; UIListLayout.Padding = UDim.new(0.04, 0); UIListLayout.Parent = Moveset
    local Ult = Instance.new("Frame"); Ult.Name = "Ult"; Ult.BackgroundColor3 = Color3.fromRGB(255, 255, 255); Ult.BackgroundTransparency = 0.5; Ult.Position = UDim2.new(0.55, 0, 0, 0); Ult.BorderSizePixel = 0; Ult.Size = UDim2.new(0.25, 0, 1, 0); Ult.Parent = Reveal
    local UltStroke = Instance.new("UIStroke"); UltStroke.Thickness = 2; UltStroke.Transparency = 0.5; UltStroke.Color = Color3.fromRGB(0, 0, 0); UltStroke.Parent = Ult
    local UltFill = Instance.new("Frame"); UltFill.Name = "Fill"; UltFill.AnchorPoint = Vector2.new(0, 1); UltFill.BackgroundColor3 = Color3.fromRGB(255, 255, 255); UltFill.BackgroundTransparency = 0.25; UltFill.Position = UDim2.new(0, 0, 1, 0); UltFill.BorderSizePixel = 0; UltFill.Size = UDim2.new(1, 0, 0, 0); UltFill.Parent = Ult
    local UltFillGradient = Instance.new("UIGradient"); UltFillGradient.Color = Logic.CooldownGradient; UltFillGradient.Parent = UltFill
    local Evasive = Instance.new("Frame"); Evasive.Name = "Evasive"; Evasive.AnchorPoint = Vector2.new(1, 0); Evasive.BackgroundColor3 = Color3.fromRGB(255, 255, 255); Evasive.BackgroundTransparency = 0.5; Evasive.Position = UDim2.new(0.95, 0, 0, 0); Evasive.BorderSizePixel = 0; Evasive.Size = UDim2.new(0.1, 0, 1, 0); Evasive.Parent = Reveal
    local EvasiveStroke = Instance.new("UIStroke"); EvasiveStroke.Thickness = 2; EvasiveStroke.Transparency = 0.5; EvasiveStroke.Color = Color3.fromRGB(0, 0, 0); EvasiveStroke.Parent = Evasive
    local EvasiveFill = Instance.new("Frame"); EvasiveFill.Name = "Fill"; EvasiveFill.AnchorPoint = Vector2.new(0, 1); EvasiveFill.BackgroundColor3 = Color3.fromRGB(0, 85, 127); EvasiveFill.BackgroundTransparency = 0; EvasiveFill.Position = UDim2.new(0, 0, 1, 0); EvasiveFill.BorderSizePixel = 0; EvasiveFill.Size = UDim2.new(1, 0, 0, 0); EvasiveFill.Parent = Evasive
    local EvasiveFillGradient = Instance.new("UIGradient"); EvasiveFillGradient.Color = Logic.CooldownGradient; EvasiveFillGradient.Parent = EvasiveFill

    local connections = {}; local moveFrames = {}

    local function addMoveCooldown(moveValue)
        if moveFrames[moveValue] then return end
        local Move = Instance.new("Frame"); Move.Name = moveValue.Name; Move.BackgroundColor3 = Color3.fromRGB(31, 31, 31); Move.BackgroundTransparency = 0.5; Move.BorderSizePixel = 0; Move.Size = UDim2.new(1, 0, 0.22, 0); Move.Parent = Moveset
        local MoveStroke = Instance.new("UIStroke"); MoveStroke.Thickness = 2; MoveStroke.Transparency = 0.5; MoveStroke.Color = Color3.fromRGB(0, 0, 0); MoveStroke.Parent = Move
        local MoveCooldown = Instance.new("Frame"); MoveCooldown.Name = "Cooldown"; MoveCooldown.AnchorPoint = Vector2.new(0.5, 1); MoveCooldown.BackgroundColor3 = Color3.fromRGB(255, 255, 255); MoveCooldown.BackgroundTransparency = 0.25; MoveCooldown.Position = UDim2.new(0.5, 0, 1, 0); MoveCooldown.BorderSizePixel = 0; MoveCooldown.Size = UDim2.new(1, 0, 0, 0); MoveCooldown.ZIndex = 1; MoveCooldown.Parent = Move
        local MoveGradient = Instance.new("UIGradient"); MoveGradient.Color = Logic.CooldownGradient; MoveGradient.Parent = MoveCooldown
        local MoveText = Instance.new("TextLabel"); MoveText.Name = "MoveNameText"; MoveText.Parent = Move; MoveText.Size = UDim2.new(1, -4, 1, -4); MoveText.Position = UDim2.new(0, 2, 0, 2); MoveText.BackgroundTransparency = 1; MoveText.Text = moveValue.Name; MoveText.TextColor3 = Color3.fromRGB(255, 255, 255); MoveText.TextScaled = true; MoveText.Font = Enum.Font.GothamBold; MoveText.ZIndex = 2 
        local TextStroke = Instance.new("UIStroke"); TextStroke.Parent = MoveText; TextStroke.Thickness = 1.2; TextStroke.Color = Color3.fromRGB(0, 0, 0)
        moveFrames[moveValue] = Move

        table.insert(connections, moveValue:GetAttributeChangedSignal("ReadyAt"):Connect(function()
            local maxCd = moveValue.Value or 0; local readyAt = moveValue:GetAttribute("ReadyAt")
            if maxCd <= 0 or not readyAt then if MoveCooldown then MoveCooldown.Size = UDim2.new(1, 0, 0, 0) end return end
            task.spawn(function()
                while MoveCooldown and MoveCooldown.Parent and Reveal and Reveal.Parent do
                    local currentTime = workspace:GetServerTimeNow(); local remaining = readyAt - currentTime
                    if remaining <= 0 then MoveCooldown.Size = UDim2.new(1, 0, 0, 0); break
                    else MoveCooldown.Size = UDim2.new(1, 0, math.clamp(remaining / maxCd, 0, 1), 0) end
                    task.wait()
                end
            end)
        end))
    end

    local function trackMoveset()
        local moveset = character:FindFirstChild("Moveset")
        if not moveset then return end
        for _, child in ipairs(moveset:GetChildren()) do if child:IsA("NumberValue") then addMoveCooldown(child) end end
        table.insert(connections, moveset.ChildAdded:Connect(function(child) if child:IsA("NumberValue") then addMoveCooldown(child) end end))
    end

    table.insert(connections, character.ChildAdded:Connect(function(child) if child.Name == "Moveset" then trackMoveset() end end))
    trackMoveset()

    local function updateUltimate()
        local ultValue = math.clamp((player:GetAttribute("Ultimate") or 0) / 100, 0, 1)
        if UltFill then UltFill.Size = UDim2.new(1, 0, ultValue, 0) end
    end
    local function updateEvade()
        local evadeValue = math.clamp((character:GetAttribute("Evade") or 0) / 50, 0, 1)
        if EvasiveFill then EvasiveFill.Size = UDim2.new(1, 0, evadeValue, 0) end
    end
    updateUltimate(); updateEvade()
    table.insert(connections, player:GetAttributeChangedSignal("Ultimate"):Connect(updateUltimate))
    table.insert(connections, character:GetAttributeChangedSignal("Evade"):Connect(updateEvade))
    
    local function Cleanup()
        if Reveal then Reveal:Destroy() end
        for _, conn in ipairs(connections) do conn:Disconnect() end
        table.clear(connections)
    end
    return Reveal, Cleanup
end

local function createESP()
    local e = {}; e.BoxS = {}; e.BoxL = {}
    for i = 1, 4 do e.BoxS[i] = mkLine(3.5); e.BoxS[i].Color = Logic.ShadowColor; e.BoxL[i] = mkLine(1.5) end
    e.CorS = {}; e.CorL = {}
    for i = 1, 8 do e.CorS[i] = mkLine(3.5); e.CorS[i].Color = Logic.ShadowColor; e.CorL[i] = mkLine(1.5) end
    e.Skel = {}; for i = 1, 14 do e.Skel[i] = mkLine(1.5) end
    e.HDotS = mkCircle(4, false); e.HDotS.Color = Logic.ShadowColor; e.HDot  = mkCircle(2, false)
    e.TrcS = mkLine(3); e.TrcS.Color = Logic.ShadowColor; e.Trc = mkLine(1.5)
    e.HPTxt = mkText(13); e.HPTxt.Center = false
    e.BarO = mkLine(7); e.BarO.Color = Logic.ShadowColor; e.BarBG = mkLine(5); e.BarBG.Color = Color3.fromRGB(40, 40, 40); e.BarF = mkLine(3)
    e.NameTxt = mkText(14); e.CharTxt = mkText(12); e.DistTxt = mkText(12); e.KillTxt = mkText(12)
    e.HL = nil; e.RevealGui = nil; e.CooldownCleanup = nil
    return e
end

local function hideAll(e)
    if not e then return end
    for i = 1, 4 do e.BoxS[i].Visible = false; e.BoxL[i].Visible = false end
    for i = 1, 8 do e.CorS[i].Visible = false; e.CorL[i].Visible = false end
    for i = 1, 14 do e.Skel[i].Visible = false end
    e.HDotS.Visible = false; e.HDot.Visible = false; e.TrcS.Visible = false; e.Trc.Visible = false; e.HPTxt.Visible = false; e.BarO.Visible = false; e.BarBG.Visible = false; e.BarF.Visible = false; e.NameTxt.Visible = false; e.CharTxt.Visible = false; e.DistTxt.Visible = false; e.KillTxt.Visible = false
    if e.RevealGui then e.RevealGui.Enabled = false end
end

local function safeRM(obj)
    if not obj then return end
    pcall(function() if typeof(obj) == "Instance" then obj:Destroy() elseif type(obj) == "userdata" and obj.Remove then obj:Remove() end end)
end

local function removeAll(e)
    if not e then return end
    for i = 1, 4 do safeRM(e.BoxS[i]); safeRM(e.BoxL[i]) end
    for i = 1, 8 do safeRM(e.CorS[i]); safeRM(e.CorL[i]) end
    for i = 1, 14 do safeRM(e.Skel[i]) end
    safeRM(e.HDotS); safeRM(e.HDot); safeRM(e.TrcS); safeRM(e.Trc); safeRM(e.HPTxt); safeRM(e.BarO); safeRM(e.BarBG); safeRM(e.BarF); safeRM(e.NameTxt); safeRM(e.CharTxt); safeRM(e.DistTxt); safeRM(e.KillTxt); safeRM(e.HL)
    if e.CooldownCleanup then e.CooldownCleanup() end
end

local function setLinePair(shadow, line, from, to, vis)
    shadow.From = from; shadow.To = to; shadow.Visible = vis
    line.From = from; line.To = to; line.Visible = vis
end

-- TARGET LOCK & DASH ASSIST ENGINE
task.spawn(function()
    local circle = Drawing.new("Circle"); circle.Visible = false; circle.Thickness = 1.5; circle.NumSides = 60; circle.Filled = false; circle.Transparency = 0.2; circle.Color = Color3.fromRGB(255, 255, 255); circle.Radius = 200; Logic.LockState.FOVCircle = circle
    local nameText = Drawing.new("Text"); nameText.Visible = false; nameText.Size = 18; nameText.Center = true; nameText.Outline = true; nameText.OutlineColor = Color3.new(0, 0, 0); nameText.Font = 2; nameText.Color = Color3.fromRGB(255, 255, 255); Logic.LockState.TargetInfoName = nameText
    local hpText = Drawing.new("Text"); hpText.Visible = false; hpText.Size = 15; hpText.Center = true; hpText.Outline = true; hpText.OutlineColor = Color3.new(0, 0, 0); hpText.Font = 2; Logic.LockState.TargetInfoHP = hpText
    local distText = Drawing.new("Text"); distText.Visible = false; distText.Size = 14; distText.Center = true; distText.Outline = true; distText.OutlineColor = Color3.new(0, 0, 0); distText.Font = 2; Logic.LockState.TargetInfoDist = distText
    local sepLine = Drawing.new("Line"); sepLine.Visible = false; sepLine.Thickness = 1; sepLine.Color = Color3.fromRGB(255, 255, 255); sepLine.Transparency = 0.5; Logic.LockState.TargetInfoLine = sepLine
end)

task.spawn(function()
    if Logic.LockState.ESPGui then pcall(function() Logic.LockState.ESPGui:Destroy() end) end
    local lockEspGui = Instance.new("BillboardGui"); lockEspGui.Name = "RazorbillLockESP"; lockEspGui.Size = UDim2.new(0, 150, 0, 150); lockEspGui.AlwaysOnTop = true; lockEspGui.LightInfluence = 0; lockEspGui.Enabled = false; lockEspGui.Adornee = nil
    local mainImg = Instance.new("ImageLabel"); mainImg.Name = "Main"; mainImg.BackgroundTransparency = 1; mainImg.Image = "rbxassetid://119160428107030"; mainImg.Size = UDim2.new(0, 110, 0, 110); mainImg.Position = UDim2.new(0.5, 0, 0.5, 0); mainImg.AnchorPoint = Vector2.new(0.5, 0.5); mainImg.Parent = lockEspGui
    local markerImg = Instance.new("ImageLabel"); markerImg.Name = "Marker"; markerImg.BackgroundTransparency = 1; markerImg.Image = "rbxassetid://85142373343991"; markerImg.Size = UDim2.new(0, 25, 0, 15); markerImg.Position = UDim2.new(0.5, 0, -0.05, 0); markerImg.AnchorPoint = Vector2.new(0.5, 0.5); markerImg.Parent = lockEspGui
    pcall(function() local coreGui = game:GetService("CoreGui"); if coreGui then lockEspGui.Parent = coreGui else lockEspGui.Parent = Player:WaitForChild("PlayerGui") end end)
    if not lockEspGui.Parent then lockEspGui.Parent = Player:WaitForChild("PlayerGui") end
    Logic.LockState.ESPGui = lockEspGui
end)

local function refreshPlayerDropdown()
    local vals = {"Auto"}
    local charsFolder = workspace:FindFirstChild("Characters")
    for _, p in ipairs(Players:GetPlayers()) do if p ~= Player then table.insert(vals, p.Name) end end
    if charsFolder then
        for _, model in ipairs(charsFolder:GetChildren()) do
            if model:IsA("Model") and not Players:GetPlayerFromCharacter(model) then
                local hum = model:FindFirstChildOfClass("Humanoid")
                if hum and hum.Health > 0 then
                    local npcName = "[NPC] " .. model.Name
                    if not table.find(vals, npcName) then table.insert(vals, npcName) end
                end
            end
        end
    end
    pcall(function() Options.LockSpecificPlayer:SetValues(vals); if not table.find(vals, Logic.LockSettings.SelectedPlayer) then Logic.LockSettings.SelectedPlayer = "Auto"; Options.LockSpecificPlayer:SetValue("Auto") end end)
end

Logic.Connections.PlayerAdded = Players.PlayerAdded:Connect(function() task.wait(1); refreshPlayerDropdown() end)
Logic.Connections.PlayerRemoving = Players.PlayerRemoving:Connect(function(plr)
    task.wait(0.5); refreshPlayerDropdown()
    if Logic.LockState.CurrentLockTarget then
        local targetPlayer = Players:GetPlayerFromCharacter(Logic.LockState.CurrentLockTarget)
        if targetPlayer == plr then
            Logic.LockState.CurrentLockTarget = nil; Logic.LockState.LastTargetName = nil
            if Logic.LockSettings.LockNotification then Library:Notify({ Title = "Target Lock", Description = plr.Name .. " left the game. Target lost.", Time = 3 }) end
        end
    end
end)

local function getLockTarget()
    if tick() - Logic.LockState.LastTargetSearch < 0.5 then return nil end
    Logic.LockState.LastTargetSearch = tick()
    local char = Player.Character; if not char then return nil end
    local root = char:FindFirstChild("HumanoidRootPart"); if not root then return nil end
    
    if Logic.LockSettings.SelectedPlayer ~= "Auto" then
        local selectedName = Logic.LockSettings.SelectedPlayer; local isNPC = string.sub(selectedName, 1, 6) == "[NPC] "
        if isNPC then
            local npcName = string.sub(selectedName, 7); local charsFolder = workspace:FindFirstChild("Characters")
            if charsFolder then
                for _, model in ipairs(charsFolder:GetChildren()) do
                    if model.Name == npcName and model:IsA("Model") then
                        local tHum = model:FindFirstChildOfClass("Humanoid"); local tRoot = model:FindFirstChild("HumanoidRootPart") or model:FindFirstChild("Torso")
                        if tHum and tHum.Health > 0 and tRoot then
                            local dist = (root.Position - tRoot.Position).Magnitude
                            if dist <= Logic.LockSettings.MaxDistance then
                                if Logic.LockSettings.WallCheck then
                                    local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, model}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
                                    local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * dist, rayParams)
                                    if ray then return nil end
                                end
                                return model
                            end
                        end
                    end
                end
            end
            return nil
        else
            local targetPlayer = Players:FindFirstChild(selectedName)
            if targetPlayer and targetPlayer.Character then
                local tChar = targetPlayer.Character; local tHum = tChar:FindFirstChildOfClass("Humanoid"); local tRoot = tChar:FindFirstChild("HumanoidRootPart") or tChar:FindFirstChild("Torso")
                if tHum and tHum.Health > 0 and tRoot and tChar ~= char then
                    local dist = (root.Position - tRoot.Position).Magnitude
                    if dist <= Logic.LockSettings.MaxDistance then
                        if Logic.LockSettings.WallCheck then
                            local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, tChar}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
                            local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * dist, rayParams)
                            if ray then return nil end
                        end
                        return tChar
                    end
                end
            end
            return nil
        end
    end

    local best, shortest = nil, math.huge
    local charsFolder = workspace:FindFirstChild("Characters")
    local entities = charsFolder and charsFolder:GetChildren() or {}
    if #entities == 0 then for _, p in ipairs(Players:GetPlayers()) do if p.Character then table.insert(entities, p.Character) end end end

    local Camera = workspace.CurrentCamera
    for _, tChar in ipairs(entities) do
        if tChar:IsA("Model") and tChar ~= char then
            local tHum = tChar:FindFirstChildOfClass("Humanoid"); local tRoot = tChar:FindFirstChild("HumanoidRootPart") or tChar:FindFirstChild("Torso")
            if tHum and tHum.Health > 0 and tRoot then
                local worldDist = (root.Position - tRoot.Position).Magnitude
                if worldDist > Logic.LockSettings.MaxDistance then continue end
                if Logic.LockSettings.WallCheck then
                    local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, tChar}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
                    local ray = workspace:Raycast(root.Position, (tRoot.Position - root.Position).Unit * worldDist, rayParams)
                    if ray then continue end
                end
                if Logic.LockSettings.TargetMode == "Closest" then
                    if worldDist < shortest then shortest = worldDist; best = tChar end
                else
                    local pos, onScreen = Camera:WorldToViewportPoint(tRoot.Position)
                    if onScreen then
                        local d = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(pos.X, pos.Y)).Magnitude
                        if Logic.LockSettings.ShowFOV and Logic.LockSettings.FOVRadius > 0 then
                            if d > Logic.LockSettings.FOVRadius then continue end
                        end
                        if d < shortest then shortest = d; best = tChar end
                    end
                end
            end
        end
    end
    return best
end

local function hideTargetInfoHUD()
    if Logic.LockState.TargetInfoName then Logic.LockState.TargetInfoName.Visible = false end
    if Logic.LockState.TargetInfoHP then Logic.LockState.TargetInfoHP.Visible = false end
    if Logic.LockState.TargetInfoDist then Logic.LockState.TargetInfoDist.Visible = false end
    if Logic.LockState.TargetInfoLine then Logic.LockState.TargetInfoLine.Visible = false end
end

local function getClosestTarget(maxDist)
    local char = Player.Character; local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return nil end
    local closest = nil; local minDistance = math.huge
    local charactersFolder = workspace:FindFirstChild("Characters")
    local targetsToSearch = charactersFolder and charactersFolder:GetChildren() or workspace:GetChildren()
    for _, model in ipairs(targetsToSearch) do
        if model ~= char and model:IsA("Model") then
            local enemyRoot = model:FindFirstChild("HumanoidRootPart"); local enemyHum = model:FindFirstChild("Humanoid")
            if enemyRoot and enemyHum and enemyHum.Health > 0 then
                local dist = (enemyRoot.Position - root.Position).Magnitude
                if dist < minDistance and dist <= maxDist then minDistance = dist; closest = model end
            end
        end
    end
    return closest
end

local isDashingArc = false
local function getBezierPoint(t, p0, p1, p2) return (1 - t)^2 * p0 + 2 * (1 - t) * t * p1 + t^2 * p2 end

local function executeDashArc(direction)
    if isDashingArc then return end; isDashingArc = true
    local character = Player.Character; if not character then isDashingArc = false return end
    local root = character:FindFirstChild("HumanoidRootPart"); local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not root or not humanoid then isDashingArc = false return end

    local targetChar = getClosestTarget(Logic.DashAssistState.DetectionRange); local target = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
    if not target then isDashingArc = false return end

    if Logic.DashAssistState.OnlyIfFacing then
        local toPlayer = (root.Position - target.Position).Unit; local dot = target.CFrame.LookVector:Dot(toPlayer)
        if dot < -0.1 then isDashingArc = false return end
    end
    
    root.Anchored = true; humanoid.AutoRotate = false; root.AssemblyLinearVelocity = Vector3.zero
    for _, mover in pairs(root:GetChildren()) do if mover:IsA("BodyVelocity") or mover:IsA("LinearVelocity") or mover:IsA("AlignPosition") or mover:IsA("VectorForce") or mover:IsA("BodyPosition") then mover:Destroy() end end

    local animator = humanoid:FindFirstChildOfClass("Animator"); local dashTrack = nil
    if animator then
        for _, track in pairs(animator:GetPlayingAnimationTracks()) do if track.Animation and (track.Animation.AnimationId:match("117223862448096") or track.Animation.AnimationId:match("75203303352791")) then track:Stop(0) end end
        local animToUse = (direction == "Left") and Logic.DashAnimLeft or Logic.DashAnimRight
        dashTrack = animator:LoadAnimation(animToUse); dashTrack.Priority = Enum.AnimationPriority.Action4; dashTrack:Play(0.05, 1, 1 / Logic.DashAssistState.FlightDuration)
    end

    local p0 = root.Position; local sideMult = (direction == "Left") and -1 or 1
    local progress = Instance.new("NumberValue"); progress.Value = 0
    local dashName = "RazorbillFakeDash_" .. tostring(tick())
    local tween = TweenService:Create(progress, TweenInfo.new(Logic.DashAssistState.FlightDuration, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut), { Value = 1 })

    local Camera = workspace.CurrentCamera; local prevCamType = Camera.CameraType

    RunService:BindToRenderStep(dashName, 20000, function()
        if not target or not target.Parent or not root then
            RunService:UnbindFromRenderStep(dashName); if root then root.Anchored = false end; if humanoid then humanoid.AutoRotate = true end; if dashTrack then dashTrack:Stop() end; if Logic.DashAssistState.CameraLock then Camera.CameraType = prevCamType end; isDashingArc = false return
        end
        root.Anchored = true; humanoid.AutoRotate = false; root.AssemblyLinearVelocity = Vector3.zero
        local val = progress.Value; local tPos = target.Position; local tLook = target.CFrame.LookVector; local flatLook = Vector3.new(tLook.X, 0, tLook.Z)
        if flatLook.Magnitude > 0.001 then flatLook = flatLook.Unit else flatLook = Vector3.new(0, 0, -1) end
        local tRight = target.CFrame.RightVector; local flatRight = Vector3.new(tRight.X, 0, tRight.Z)
        if flatRight.Magnitude > 0.001 then flatRight = flatRight.Unit else flatRight = Vector3.new(1, 0, 0) end
        local p2 = tPos + (flatLook * -Logic.DashAssistState.BehindDistance); local midPoint = (p0 + p2) / 2; local p1 = midPoint + (flatRight * (Logic.DashAssistState.CurveStrength * sideMult)) + Vector3.new(0, Logic.DashAssistState.ArchHeight, 0)
        local currentPos = getBezierPoint(val, p0, p1, p2); local lookPos = Vector3.new(tPos.X, currentPos.Y, tPos.Z)
        if (lookPos - currentPos).Magnitude > 0.1 then root.CFrame = CFrame.lookAt(currentPos, lookPos) else root.CFrame = CFrame.new(currentPos) end
        if Logic.DashAssistState.CameraLock then
            Camera.CameraType = Enum.CameraType.Scriptable; local dirToEnemy = (tPos - root.Position).Unit; local targetCamCF = CFrame.lookAt(root.Position - (dirToEnemy * 11) + Vector3.new(0, 4.5, 0), tPos); Camera.CFrame = Camera.CFrame:Lerp(targetCamCF, 0.35)
        end
    end)
    tween:Play()
    tween.Completed:Connect(function()
        RunService:UnbindFromRenderStep(dashName); progress:Destroy(); if dashTrack then dashTrack:Stop(0.1) end
        if Logic.DashAssistState.CameraLock then Camera.CameraType = prevCamType end
        local lockStart = tick(); local lockName = "RazorbillDashLock_" .. tostring(lockStart)
        RunService:BindToRenderStep(lockName, 20000, function()
            if tick() - lockStart > Logic.DashAssistState.LockDuration or not target or not target.Parent or not root then RunService:UnbindFromRenderStep(lockName); if root then root.Anchored = false; root.AssemblyLinearVelocity = Vector3.zero end; if humanoid then humanoid.AutoRotate = true end; isDashingArc = false return end
            root.Anchored = true; if humanoid then humanoid.AutoRotate = false end; root.AssemblyLinearVelocity = Vector3.zero
            local lockedPos = root.Position; local facePos = Vector3.new(target.Position.X, lockedPos.Y, target.Position.Z)
            if (facePos - lockedPos).Magnitude > 0.1 then root.CFrame = CFrame.lookAt(lockedPos, facePos) end
        end)
    end)
end

Logic.Connections.LockZoom = UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseWheel and Logic.LockState.CameraLocked then
        Logic.LockState.ZoomDistance = math.clamp(Logic.LockState.ZoomDistance - (input.Position.Z * 2), 4, 50)
    end
end)

RunService:BindToRenderStep("RazorbillTargetLock", Enum.RenderPriority.Camera.Value + 5, function(dt)
    local Camera = workspace.CurrentCamera; local char = Player.Character; local hum  = char and char:FindFirstChildOfClass("Humanoid"); local root = char and char:FindFirstChild("HumanoidRootPart")
    local isAlive = (hum and hum.Health > 0 and root)

    if Logic.LockState.FOVCircle then
        if Toggles.EnableLock.Value and Options.LockKeybind:GetState() and Logic.LockSettings.ShowFOV then
            Logic.LockState.FOVCircle.Position = Vector2.new(Mouse.X, Mouse.Y); Logic.LockState.FOVCircle.Radius = Logic.LockSettings.FOVRadius; Logic.LockState.FOVCircle.Color = Options.TargetInfoColor.Value; Logic.LockState.FOVCircle.Filled = Logic.LockSettings.FOVFilled; Logic.LockState.FOVCircle.Transparency = 1 - Logic.LockSettings.FOVTransparency; Logic.LockState.FOVCircle.NumSides = Logic.LockSettings.FOVSides; Logic.LockState.FOVCircle.Visible = true
        else Logic.LockState.FOVCircle.Visible = false end
    end

    if not Toggles.EnableLock.Value or not isAlive then
        if Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end
        if Logic.LockState.CameraLocked then Camera.CameraType = Enum.CameraType.Custom; if hum then Camera.CameraSubject = hum end; Logic.LockState.CameraLocked = false end
        if Logic.LockState.WasLockedBody then if hum then hum.AutoRotate = true end; if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy(); Logic.LockState.LockBodyGyro = nil end; Logic.LockState.WasLockedBody = false end
        Logic.LockState.CurrentLockTarget = nil; hideTargetInfoHUD(); return
    end

    if Logic.LockState.CurrentLockTarget then
        local eHum = Logic.LockState.CurrentLockTarget:FindFirstChildOfClass("Humanoid"); local targetAlive = Logic.LockState.CurrentLockTarget.Parent and eHum and eHum.Health > 0
        if not targetAlive then
            local deadName = Logic.LockState.LastTargetName; Logic.LockState.CurrentLockTarget = nil; Logic.LockState.LastTargetName = nil
            if Logic.LockSettings.AutoRetarget then Logic.LockState.LastTargetSearch = 0; if Logic.LockSettings.LockNotification and deadName then Library:Notify({ Title = "Target Lock", Description = deadName .. " eliminated. Retargeting...", Time = 2 }) end
            elseif Logic.LockSettings.Sticky then if Logic.LockSettings.LockNotification and deadName then Library:Notify({ Title = "Target Lock", Description = deadName .. " eliminated. Target lost.", Time = 2 }) end end
        else
            local tRoot = Logic.LockState.CurrentLockTarget:FindFirstChild("HumanoidRootPart") or Logic.LockState.CurrentLockTarget:FindFirstChild("Torso")
            if tRoot then
                local dist = (root.Position - tRoot.Position).Magnitude
                if dist > Logic.LockSettings.MaxDistance then
                    if not Logic.LockSettings.Sticky then Logic.LockState.CurrentLockTarget = nil; if Logic.LockSettings.LockNotification then Library:Notify({ Title = "Target Lock", Description = "Target out of range. Searching...", Time = 2 }) end end
                end
            end
        end
    end

    if not Logic.LockSettings.Sticky and Logic.LockState.CurrentLockTarget == nil then Logic.LockState.CurrentLockTarget = getLockTarget()
    elseif not Logic.LockState.CurrentLockTarget then Logic.LockState.CurrentLockTarget = getLockTarget() end
    
    if Logic.LockState.CurrentLockTarget then
        local newName = Logic.LockState.CurrentLockTarget.Name; local plrFromChar = Players:GetPlayerFromCharacter(Logic.LockState.CurrentLockTarget)
        if plrFromChar then newName = plrFromChar.DisplayName .. " (@" .. plrFromChar.Name .. ")" end
        if newName ~= Logic.LockState.LastTargetName then Logic.LockState.LastTargetName = newName; if Logic.LockSettings.LockNotification then Library:Notify({ Title = "Target Lock", Description = "Locked onto: " .. newName, Time = 2 }) end end
    end
    
    if not Logic.LockState.CurrentLockTarget or not root or not hum then 
        if Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end; hideTargetInfoHUD(); return 
    end

    local targetPart = Logic.LockState.CurrentLockTarget:FindFirstChild(Logic.LockSettings.TargetPart) or Logic.LockState.CurrentLockTarget:FindFirstChild("HumanoidRootPart")
    if not targetPart then if Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end; hideTargetInfoHUD(); return end

    local targetPos = targetPart.Position
    if Logic.LockSettings.Prediction > 0 then
        local tRoot = Logic.LockState.CurrentLockTarget:FindFirstChild("HumanoidRootPart")
        if tRoot and tRoot:IsA("BasePart") then targetPos = targetPos + (tRoot.AssemblyLinearVelocity * Logic.LockSettings.Prediction) end
    end

    if Logic.LockSettings.ESPEnabled and Logic.LockState.ESPGui then
        Logic.LockState.ESPGui.Enabled = true; Logic.LockState.ESPGui.Adornee = targetPart
        local espMain = Logic.LockState.ESPGui:FindFirstChild("Main"); if espMain then espMain.Rotation = (tick() * 45) % 360 end
        local espMarker = Logic.LockState.ESPGui:FindFirstChild("Marker"); if espMarker then espMarker.Position = UDim2.new(0.5, 0, -0.05, 0) end
    elseif Logic.LockState.ESPGui then Logic.LockState.ESPGui.Adornee = nil; Logic.LockState.ESPGui.Enabled = false end

    if Logic.LockSettings.ShowTargetInfo and Logic.LockState.TargetInfoName then
        local viewport = Camera.ViewportSize; local infoColor = Options.TargetInfoColor.Value; local eHum = Logic.LockState.CurrentLockTarget:FindFirstChildOfClass("Humanoid"); local tDist = (root.Position - targetPart.Position).Magnitude
        local baseX, baseY
        if Logic.LockSettings.TargetInfoPos == "Top Center" then baseX = viewport.X / 2; baseY = 35
        elseif Logic.LockSettings.TargetInfoPos == "Bottom Center" then baseX = viewport.X / 2; baseY = viewport.Y - 90
        elseif Logic.LockSettings.TargetInfoPos == "Top Right" then baseX = viewport.X - 150; baseY = 35
        else baseX = viewport.X / 2; baseY = 35 end
        
        local displayName = Logic.LockState.CurrentLockTarget.Name; local plrFromChar = Players:GetPlayerFromCharacter(Logic.LockState.CurrentLockTarget)
        if plrFromChar then displayName = plrFromChar.DisplayName end
        
        Logic.LockState.TargetInfoName.Text = "🎯 " .. displayName; Logic.LockState.TargetInfoName.Position = Vector2.new(baseX, baseY); Logic.LockState.TargetInfoName.Color = infoColor; Logic.LockState.TargetInfoName.Visible = true
        Logic.LockState.TargetInfoLine.From = Vector2.new(baseX - 60, baseY + 20); Logic.LockState.TargetInfoLine.To = Vector2.new(baseX + 60, baseY + 20); Logic.LockState.TargetInfoLine.Color = infoColor; Logic.LockState.TargetInfoLine.Visible = true
        
        if eHum then
            local hpFrac = math.clamp(eHum.Health / eHum.MaxHealth, 0, 1); local hpCol = Color3.fromRGB(255 * (1 - hpFrac), 255 * hpFrac, 50)
            Logic.LockState.TargetInfoHP.Text = string.format("HP: %d / %d (%.0f%%)", math.floor(eHum.Health), math.floor(eHum.MaxHealth), hpFrac * 100); Logic.LockState.TargetInfoHP.Position = Vector2.new(baseX, baseY + 24); Logic.LockState.TargetInfoHP.Color = hpCol; Logic.LockState.TargetInfoHP.Visible = true
        else Logic.LockState.TargetInfoHP.Visible = false end
        
        Logic.LockState.TargetInfoDist.Text = string.format("Distance: %d studs", math.floor(tDist)); Logic.LockState.TargetInfoDist.Position = Vector2.new(baseX, baseY + 40); Logic.LockState.TargetInfoDist.Color = infoColor; Logic.LockState.TargetInfoDist.Visible = true
    else hideTargetInfoHUD() end

    if Logic.LockSettings.Method == "Camera" then
        if Logic.LockState.WasLockedBody then hum.AutoRotate = true; if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy(); Logic.LockState.LockBodyGyro = nil end; Logic.LockState.WasLockedBody = false end
        if not Logic.LockState.CameraLocked then Logic.LockState.OrigCamType = Camera.CameraType; local dist = (Camera.CFrame.Position - root.Position).Magnitude; if dist <= 50 then Logic.LockState.ZoomDistance = math.clamp(dist, 4, 50) else Logic.LockState.ZoomDistance = 10 end; Logic.LockState.CameraLocked = true end
        Camera.CameraType = Enum.CameraType.Scriptable
        local offsetDir = root.Position - targetPos; local flatDir = Vector3.new(offsetDir.X, 0, offsetDir.Z)
        if flatDir.Magnitude < 0.001 then flatDir = -root.CFrame.LookVector; flatDir = Vector3.new(flatDir.X, 0, flatDir.Z) end
        flatDir = flatDir.Unit
        local baseHeight = Vector3.new(0, 2.5, 0); local camPos = root.Position + (flatDir * Logic.LockState.ZoomDistance) + baseHeight
        local lookCF = CFrame.lookAt(camPos, targetPos); camPos = camPos + (lookCF.RightVector * Logic.LockSettings.SideOffset)
        local rayParams = RaycastParams.new(); rayParams.FilterDescendantsInstances = {char, Logic.LockState.CurrentLockTarget}; rayParams.FilterType = Enum.RaycastFilterType.Exclude
        local rayStart = root.Position + Vector3.new(0, 1.5, 0); local rayResult = workspace:Raycast(rayStart, camPos - rayStart, rayParams)
        if rayResult then camPos = rayResult.Position + (lookCF.LookVector * 0.5) end
        local desiredCF = CFrame.lookAt(camPos, targetPos)
        if Logic.LockSettings.Smoothness <= 0 then Camera.CFrame = desiredCF else local rate  = 30 / Logic.LockSettings.Smoothness; local alpha = math.clamp(1 - math.exp(-rate * dt), 0, 1); Camera.CFrame = Camera.CFrame:Lerp(desiredCF, alpha) end
    else
        if Logic.LockState.CameraLocked then Camera.CameraType = Enum.CameraType.Custom; Camera.CameraSubject = hum; Logic.LockState.CameraLocked = false end
        hum.AutoRotate = false; Logic.LockState.WasLockedBody = true
        if not Logic.LockState.LockBodyGyro or Logic.LockState.LockBodyGyro.Parent ~= root then if Logic.LockState.LockBodyGyro then Logic.LockState.LockBodyGyro:Destroy() end; Logic.LockState.LockBodyGyro = Instance.new("BodyGyro"); Logic.LockState.LockBodyGyro.MaxTorque = Vector3.new(0, 400000, 0); Logic.LockState.LockBodyGyro.P = 50000; Logic.LockState.LockBodyGyro.D = 500; Logic.LockState.LockBodyGyro.Parent = root end
        Logic.LockState.LockBodyGyro.CFrame = CFrame.lookAt(root.Position, Vector3.new(targetPos.X, root.Position.Y, targetPos.Z))
    end
end)

-- SERVER BROWSER ENGINE
local function estimateRegion(ping)
    if ping <= 0 then return "Unknown" elseif ping <= 40 then return "US East / Nearby" elseif ping <= 70 then return "US Central" elseif ping <= 100 then return "US West / Regional" elseif ping <= 140 then return "EU West" elseif ping <= 180 then return "EU East / Middle East" elseif ping <= 220 then return "South America" elseif ping <= 270 then return "East Asia" elseif ping <= 320 then return "Southeast Asia / Oceania" elseif ping <= 400 then return "Africa / South Asia" else return "Very Far" end
end

local function pingQualityIcon(ping)
    if ping <= 60 then return "[+++]" elseif ping <= 120 then return "[++ ]" elseif ping <= 200 then return "[+  ]" else return "[!  ]" end
end

local function buildServerMap(serverList)
    Logic.ServerMap = {}; local vals = {}
    for i, entry in ipairs(serverList) do
        local label = string.format("%s #%d | %d/%d | %dms%s", pingQualityIcon(entry.Ping), i, entry.Playing, entry.MaxPlayers, entry.Ping, entry.IsCurrent and " [YOU]" or "")
        Logic.ServerMap[label] = entry; vals[#vals + 1] = label
    end
    return vals
end

local function updateServerInfo(entry)
    if not RyuHub:FindFirstChild("ServerDetailsLabel", true) then return end
    local lbl = RyuHub:FindFirstChild("ServerDetailsLabel", true)
    if not entry then lbl.Text = "No server selected.\n\nClick 'Refresh Server List' to load servers,\nthen select one from the dropdown."; return end
    local fillPercent = entry.MaxPlayers > 0 and math.floor((entry.Playing / entry.MaxPlayers) * 100) or 0
    local fillBar = string.rep("|", math.floor(fillPercent / 5)) .. string.rep(".", 20 - math.floor(fillPercent / 5))
    lbl.Text = string.format("Job ID:\n%s\n\nPlayers: %d / %d (%d%%)\n[%s]\n\nPing: %d ms  %s\nServer FPS: %.1f\n\nEstimated Region: %s\n%s", entry.JobId, entry.Playing, entry.MaxPlayers, fillPercent, fillBar, entry.Ping, pingQualityIcon(entry.Ping), entry.FPS, entry.Region, entry.IsCurrent and "\n>> This is YOUR current server <<" or "")
end

local function updateStatsLabel()
    local lbl = RyuHub:FindFirstChild("ServerStatsLabel", true)
    if not lbl then return end
    if #Logic.ServerList == 0 then lbl.Text = "No data. Refresh to load servers."; return end
    local totalPlayers, totalPing, minPing, maxPing = 0, 0, math.huge, 0; local fullServers, emptyServers = 0, 0
    for _, srv in ipairs(Logic.ServerList) do
        totalPlayers += srv.Playing; totalPing += srv.Ping
        if srv.Ping < minPing then minPing = srv.Ping end; if srv.Ping > maxPing then maxPing = srv.Ping end
        if srv.Playing >= srv.MaxPlayers then fullServers += 1 end; if srv.Playing == 0 then emptyServers += 1 end
    end
    local avgPing = math.floor(totalPing / #Logic.ServerList)
    lbl.Text = string.format("Total Servers: %d\nTotal Players: %d\n\nAvg Ping: %dms\nBest Ping: %dms\nWorst Ping: %dms\n\nFull Servers: %d\nEmpty Servers: %d", #Logic.ServerList, totalPlayers, avgPing, minPing, maxPing, fullServers, emptyServers)
end

local function fetchServers()
    Logic.ServerList = {}; Logic.ServerMap = {}; local placeId = game.PlaceId; local cursor = ""; local maxPages = 10; local idx = 0
    Library:Notify({ Title = "Server Browser", Description = "Fetching servers... This may take a moment.", Time = 4 })
    for page = 1, maxPages do
        local url = string.format("https://games.roblox.com/v1/games/%d/servers/0?sortOrder=2&excludeFullGames=false&limit=100%s", placeId, cursor ~= "" and ("&cursor=" .. cursor) or "")
        local ok, res = pcall(game.HttpGet, game, url)
        if not ok or not res then break end
        local ok2, data = pcall(HttpService.JSONDecode, HttpService, res)
        if not ok2 or not data or not data.data then break end
        for _, srv in ipairs(data.data) do
            idx += 1
            local entry = { JobId = srv.id or "", Playing = srv.playing or 0, MaxPlayers = srv.maxPlayers or 0, Ping = srv.ping or 0, FPS = srv.fps or 0, Index = idx, Region = estimateRegion(srv.ping or 0), IsCurrent = (srv.id == game.JobId) }
            table.insert(Logic.ServerList, entry)
        end
        cursor = data.nextPageCursor or ""; if cursor == "" then break end
        task.wait(0.4)
    end
    Library:Notify({ Title = "Server Browser", Description = "Found " .. tostring(#Logic.ServerList) .. " servers!", Time = 4 })
    return Logic.ServerList
end

-- BLACKFLASH ENGINE
task.spawn(function()
    Logic.TargetRemote = ReplicatedStorage:WaitForChild("Knit"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("DivergentFistService"):WaitForChild("RE"):WaitForChild("Activated")
end)

local oldNamecall
oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
    local method = getnamecallmethod()
    if not checkcaller() and method == "FireServer" and self == Logic.TargetRemote and Toggles.BlackFlashEnabled.Value then
        Logic.LastFiredTick = tick()
    end
    return oldNamecall(self, ...)
end)

local function autoFireDivergentFist()
    local character = Player.Character
    if character and character:FindFirstChild("Moveset") then
        local divergentFistMove = character.Moveset:FindFirstChild("Divergent Fist")
        if divergentFistMove and Logic.TargetRemote then
            Logic.TargetRemote:FireServer(divergentFistMove, nil)
        end
    end
end

local function performDashLogic(target)
    local MAX_DASH_DISTANCE = Options.DashDistance.Value; local FIRE_DELAY = Options.FireDelay.Value; local DASH_DURATION = Options.DashDuration.Value; local POST_DASH_LOCK_TIME = Options.LockTime.Value
    local easingStyleEnum = Enum.EasingStyle[Options.DashEasingStyle.Value] or Enum.EasingStyle.Cubic; local easingDirectionEnum = Enum.EasingDirection[Options.DashEasingDirection.Value] or Enum.EasingDirection.Out
    local char = Player.Character; local root = char and char:FindFirstChild("HumanoidRootPart"); local humanoid = char and char:FindFirstChild("Humanoid"); local enemyRoot = target:FindFirstChild("HumanoidRootPart")
    if not root or not enemyRoot then task.delay(FIRE_DELAY, autoFireDivergentFist); return end

    local initialEnemyCFrame = enemyRoot.CFrame; local initialEnemyPos = initialEnemyCFrame.Position; local startPos = root.Position
    local objectSpacePos = initialEnemyCFrame:PointToObjectSpace(startPos); local isBehind = objectSpacePos.Z > 0; local distanceToEnemy = (startPos - initialEnemyPos).Magnitude
    local isDash = true; local dashType = "Arch"; local endPos = startPos; local controlPos = startPos

    if isBehind and distanceToEnemy <= 10 then isDash = false
    elseif isBehind and distanceToEnemy > 10 then dashType = "Straight"; endPos = (initialEnemyCFrame * CFrame.new(0, 0, 5)).Position
    else
        dashType = "Arch"; endPos = (initialEnemyCFrame * CFrame.new(0, 0, 4)).Position
        local distance = (endPos - startPos).Magnitude; local archWidth = math.clamp(distance / 1.5, 5, 25)
        local direction = endPos - startPos; local perp = Vector3.new(-direction.Z, 0, direction.X)
        if perp.Magnitude > 0.001 then perp = perp.Unit else perp = Vector3.new(1, 0, 0) end
        local midPos = (startPos + endPos) / 2; local cp1 = midPos + (perp * archWidth); local cp2 = midPos - (perp * archWidth)
        local enemyRightVector = initialEnemyCFrame.RightVector; local playerIsOnRightSide = (startPos - initialEnemyPos):Dot(enemyRightVector) > 0; local cp1IsOnRightSide = (cp1 - initialEnemyPos):Dot(enemyRightVector) > 0
        controlPos = (playerIsOnRightSide == cp1IsOnRightSide) and cp1 or cp2
    end

    if humanoid then humanoid.AutoRotate = false end
    local startTime = tick(); local hasFired = false; local dashConn

    local function finalizeMovement(finalCFrame)
        if dashConn then dashConn:Disconnect() end
        if humanoid then humanoid.AutoRotate = true end
        if finalCFrame and root and root.Parent then root.CFrame = finalCFrame end
        if not hasFired then hasFired = true; autoFireDivergentFist() end
    end

    dashConn = RunService.Heartbeat:Connect(function()
        if not root or not root.Parent then return finalizeMovement(nil) end
        local elapsed = tick() - startTime
        if elapsed >= FIRE_DELAY and not hasFired then hasFired = true; autoFireDivergentFist() end
        local currentPos = startPos
        if isDash then
            local alpha = math.clamp(elapsed / DASH_DURATION, 0, 1)
            local easedAlpha = TweenService:GetValue(alpha, easingStyleEnum, easingDirectionEnum)
            if dashType == "Straight" then currentPos = startPos:Lerp(endPos, easedAlpha)
            elseif dashType == "Arch" then currentPos = (1 - easedAlpha)^2 * startPos + 2 * (1 - easedAlpha) * easedAlpha * controlPos + easedAlpha^2 * endPos end
        end
        root.CFrame = CFrame.lookAt(currentPos, initialEnemyPos)
        if Toggles.CameraLockEnabled.Value then local cam = workspace.CurrentCamera; if cam then cam.CFrame = CFrame.lookAt(cam.CFrame.Position, initialEnemyPos) end end
        local totalWaitTime = isDash and (DASH_DURATION + POST_DASH_LOCK_TIME) or (FIRE_DELAY + POST_DASH_LOCK_TIME)
        if elapsed >= totalWaitTime then finalizeMovement(CFrame.lookAt(currentPos, initialEnemyPos)) end
    end)
end

local function onAnimationPlayed(animTrack)
    local animId = animTrack.Animation and animTrack.Animation.AnimationId or ""
    if Logic.DashAssistState.Enabled and not isDashingArc then
        if animId:match("75203303352791") then task.spawn(function() executeDashArc("Left") end)
        elseif animId:match("117223862448096") then task.spawn(function() executeDashArc("Right") end) end
    end
    if Toggles.BlackFlashEnabled.Value then
        local idMatch = string.match(animId, "%d+")
        if idMatch and Logic.TargetAnimations[idMatch] then
            if (tick() - Logic.LastFiredTick) <= Logic.TIME_WINDOW then
                Logic.LastFiredTick = 0
                local closestTarget = getClosestTarget(Options.DashDistance.Value)
                if closestTarget then performDashLogic(closestTarget) else task.delay(Options.FireDelay.Value, autoFireDivergentFist) end
            end
        end
    end
end

local function setupCharacter(character)
    local humanoid = character:WaitForChild("Humanoid", 10)
    if humanoid then
        local animator = humanoid:WaitForChild("Animator", 10)
        if animator then animator.AnimationPlayed:Connect(onAnimationPlayed) end
    end
    
    -- Visualizer Loop ESP
    RunService.Heartbeat:Connect(function()
        local cam = workspace.CurrentCamera; if not cam then return end
        local entities = getEntities(); local alive = {}
        for _, data in ipairs(entities) do
            local char = data.Model; local plr = data.Player; local dName = data.Name; alive[char] = true
            if plr == Player then
                if Logic.ESPObjects[char] then hideAll(Logic.ESPObjects[char]); if Logic.ESPObjects[char].HL then Logic.ESPObjects[char].HL.Enabled = false end end
                continue
            end
            local hrp = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso"); local hum = char:FindFirstChildOfClass("Humanoid"); local head = char:FindFirstChild("Head")
            if not Logic.ESPObjects[char] then Logic.ESPObjects[char] = createESP() end
            local e = Logic.ESPObjects[char]
            if not (hrp and hum and hum.Health > 0) then hideAll(e); if e.HL then e.HL.Enabled = false end; continue end

            local dist = (cam.CFrame.Position - hrp.Position).Magnitude; local box = getCharBounds(char)

            if Toggles.ESPBox.Value and box then
                for i = 1, 4 do e.BoxL[i].Color = Options.ESPBoxColor.Value end
                local tl = Vector2.new(box.X, box.Y); local tr = Vector2.new(box.X + box.W, box.Y); local bl = Vector2.new(box.X, box.Y + box.H); local br = Vector2.new(box.X + box.W, box.Y + box.H)
                setLinePair(e.BoxS[1], e.BoxL[1], tl, tr, true); setLinePair(e.BoxS[2], e.BoxL[2], tr, br, true); setLinePair(e.BoxS[3], e.BoxL[3], br, bl, true); setLinePair(e.BoxS[4], e.BoxL[4], bl, tl, true)
            else for i = 1, 4 do e.BoxS[i].Visible = false; e.BoxL[i].Visible = false end end

            if Toggles.ESPCorner.Value and box then
                for i = 1, 8 do e.CorL[i].Color = Options.ESPCornerColor.Value end
                local x, y, w, h = box.X, box.Y, box.W, box.H; local len = math.clamp(math.min(w, h) * 0.25, 5, 9999); local tl = Vector2.new(x, y); local tr = Vector2.new(x + w, y); local bl = Vector2.new(x, y + h); local br = Vector2.new(x + w, y + h)
                local segs = { {tl, tl + Vector2.new(len, 0)}, {tl, tl + Vector2.new(0, len)}, {tr, tr + Vector2.new(-len, 0)}, {tr, tr + Vector2.new(0, len)}, {bl, bl + Vector2.new(len, 0)}, {bl, bl + Vector2.new(0, -len)}, {br, br + Vector2.new(-len, 0)}, {br, br + Vector2.new(0, -len)}, }
                for i = 1, 8 do setLinePair(e.CorS[i], e.CorL[i], segs[i][1], segs[i][2], true) end
            else for i = 1, 8 do e.CorS[i].Visible = false; e.CorL[i].Visible = false end end

            if Toggles.ESPSkeleton.Value then
                local bones = char:FindFirstChild("UpperTorso") and Logic.R15Bones or Logic.R6Bones; local idx = 0
                for _, b in ipairs(bones) do
                    idx += 1; local p1 = char:FindFirstChild(b[1]); local p2 = char:FindFirstChild(b[2])
                    if p1 and p2 then
                        local s1, o1 = w2v(p1.Position); local s2, o2 = w2v(p2.Position)
                        if o1 and o2 then e.Skel[idx].Color = Options.ESPSkeletonColor.Value; e.Skel[idx].From = s1; e.Skel[idx].To = s2; e.Skel[idx].Visible = true
                        else e.Skel[idx].Visible = false end
                    else e.Skel[idx].Visible = false end
                end
                for i = idx + 1, 14 do e.Skel[i].Visible = false end
            else for i = 1, 14 do e.Skel[i].Visible = false end end

            if Toggles.ESPHeadDot.Value and head then
                local hs, ho = w2v(head.Position)
                if ho then
                    local headTop = w2v(head.Position + Vector3.new(0, head.Size.Y * 0.5, 0)); local r = headTop and math.abs(hs.Y - headTop.Y) or 5; r = math.clamp(r, 3, 50)
                    e.HDot.Color = Options.ESPHeadDotColor.Value; e.HDotS.Position = hs; e.HDotS.Radius = r + 1; e.HDotS.Visible = true; e.HDot.Position = hs; e.HDot.Radius = r; e.HDot.Visible = true
                else e.HDotS.Visible = false; e.HDot.Visible = false end
            else e.HDotS.Visible = false; e.HDot.Visible = false end

            if Toggles.ESPTracer.Value then
                local fp, fo = w2v(hrp.Position - Vector3.new(0, 3, 0))
                if fo then
                    local bot = Vector2.new(cam.ViewportSize.X / 2, cam.ViewportSize.Y); e.Trc.Color = Options.ESPTracerColor.Value; e.TrcS.From = bot; e.TrcS.To = fp; e.TrcS.Visible = true; e.Trc.From = bot; e.Trc.To = fp; e.Trc.Visible = true
                else e.TrcS.Visible = false; e.Trc.Visible = false end
            else e.TrcS.Visible = false; e.Trc.Visible = false end

            if Toggles.ESPHPText.Value and box then
                local f = math.clamp(hum.Health / hum.MaxHealth, 0, 1); e.HPTxt.Position = Vector2.new(box.X + box.W + 4, box.Y); e.HPTxt.Text = math.floor(hum.Health) .. "/" .. math.floor(hum.MaxHealth); e.HPTxt.Color = hpColor(f); e.HPTxt.Visible = true
            else e.HPTxt.Visible = false end

            if Toggles.ESPHPBar.Value and box then
                local f = math.clamp(hum.Health / hum.MaxHealth, 0, 1); local bx = box.X - 6; local top = Vector2.new(bx, box.Y); local bot = Vector2.new(bx, box.Y + box.H); local ft = Vector2.new(bx, box.Y + box.H * (1 - f))
                e.BarO.From = top; e.BarO.To = bot; e.BarO.Visible = true; e.BarBG.From = top; e.BarBG.To = bot; e.BarBG.Visible = true; e.BarF.From = ft; e.BarF.To = bot; e.BarF.Color = hpColor(f); e.BarF.Visible = f > 0
            else e.BarO.Visible = false; e.BarBG.Visible = false; e.BarF.Visible = false end

            if Toggles.ESPOutline.Value then
                if not e.HL or e.HL.Parent ~= char then
                    safeRM(e.HL); local hl = Instance.new("Highlight"); hl.FillTransparency = 1; hl.OutlineColor = Options.ESPOutlineColor.Value; hl.OutlineTransparency = 0; hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop; hl.Parent = char; e.HL = hl
                end
                e.HL.OutlineColor = Options.ESPOutlineColor.Value; e.HL.Enabled = true
            else if e.HL then e.HL.Enabled = false end end

            if Toggles.CooldownRevealer.Value then
                if not e.CooldownCleanup then e.RevealGui, e.CooldownCleanup = CreateCooldownBillboard(char, plr, hrp) end
                if e.RevealGui then e.RevealGui.Enabled = true end
            else if e.CooldownCleanup then e.CooldownCleanup(); e.CooldownCleanup = nil; e.RevealGui = nil end end

            if box then
                local ay = box.Y - 2
                if Toggles.ESPCharacter.Value then ay = ay - 14; local ms = char:GetAttribute("Moveset") or (plr and plr:GetAttribute("Moveset")) or "Unknown"; e.CharTxt.Color = Options.ESPCharacterColor.Value; e.CharTxt.Position = Vector2.new(box.CX, ay); e.CharTxt.Text = "[" .. tostring(ms) .. "]"; e.CharTxt.Visible = true else e.CharTxt.Visible = false end
                if Toggles.ESPName.Value then ay = ay - 16; e.NameTxt.Color = Options.ESPNameColor.Value; e.NameTxt.Position = Vector2.new(box.CX, ay); e.NameTxt.Text = dName; e.NameTxt.Visible = true else e.NameTxt.Visible = false end
                local by = box.Y + box.H + 2
                if Toggles.ESPDistance.Value then e.DistTxt.Color = Options.ESPDistanceColor.Value; e.DistTxt.Position = Vector2.new(box.CX, by); e.DistTxt.Text = math.floor(dist) .. " studs"; e.DistTxt.Visible = true; by = by + 14 else e.DistTxt.Visible = false end
                if Toggles.ESPKill.Value then
                    local kills = 0
                    if plr then local ls = plr:FindFirstChild("leaderstats"); if ls then local ks = ls:FindFirstChild("Kills") or ls:FindFirstChild("kills") or ls:FindFirstChild("KOs") or ls:FindFirstChild("KO"); if ks then kills = ks.Value end end end
                    e.KillTxt.Color = Options.ESPKillColor.Value; e.KillTxt.Position = Vector2.new(box.CX, by); e.KillTxt.Text = "Kills: " .. tostring(kills); e.KillTxt.Visible = true
                else e.KillTxt.Visible = false end
            else
                e.NameTxt.Visible = false; e.CharTxt.Visible = false; e.DistTxt.Visible = false; e.KillTxt.Visible = false
            end
        end
        for charE, ev in pairs(Logic.ESPObjects) do
            if not alive[charE] or not charE.Parent then hideAll(ev); if ev.HL then ev.HL.Enabled = false; safeRM(ev.HL) end; removeAll(ev); Logic.ESPObjects[charE] = nil end
        end
    end)
end
if Player.Character then task.spawn(setupCharacter, Player.Character) end
Player.CharacterAdded:Connect(setupCharacter)

Logic.Connections.DashInput = UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if not Toggles.EnableDashAssist.Value then return end
    local assignedKey = Options.DashAssistKeybind.Value; local targetKeyCode
    if typeof(assignedKey) == "EnumItem" then targetKeyCode = assignedKey elseif typeof(assignedKey) == "string" then local ok, kc = pcall(function() return Enum.KeyCode[assignedKey] end); if ok then targetKeyCode = kc else return end else return end
    if input.KeyCode == targetKeyCode then
        local direction = "Right"; local char = Player.Character; local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum and hum.MoveDirection.Magnitude > 0 then
            local camRight = workspace.CurrentCamera.CFrame.RightVector * Vector3.new(1, 0, 1)
            if camRight.Magnitude > 0 then camRight = camRight.Unit; if hum.MoveDirection:Dot(camRight) < -0.2 then direction = "Left" end end
        end
        task.spawn(function() executeDashArc(direction) end)
    end
end)


--// ==========================================
--// UI TABS & SECTIONS CONSTRUCTION
--// ==========================================

-- TAB 1: COMBAT (Blackflash Chain)
local TabCombat = CreateMainTab("Blackflash Chain")
local SubCombat = CreateSubTab(TabCombat, "Combat Modules")
local SecCombat = CreateSection(SubCombat, "Black Flash Chain")

CreateToggle(SecCombat, "Enable Black Flash Chain", "Automatically target and dash behind enemies.", false, function(state) Toggles.BlackFlashEnabled.Value = state end)
CreateToggle(SecCombat, "Camera Lock During Dash", "Forces your camera to lock onto enemy back.", true, function(state) Toggles.CameraLockEnabled.Value = state end)
CreateSlider(SecCombat, "Max Dash Distance", 5, 50, 15, function(val) Options.DashDistance.Value = val end)
CreateSlider(SecCombat, "Combo Fire Delay (s)", 0.1, 1.0, 0.25, function(val) Options.FireDelay.Value = val end)
CreateSlider(SecCombat, "Dash Duration (s)", 0.1, 1.0, 0.35, function(val) Options.DashDuration.Value = val end)
CreateSlider(SecCombat, "Post Dash Lock Time", 0.0, 0.5, 0.1, function(val) Options.LockTime.Value = val end)
CreateDropdown(SecCombat, "Dash Easing Style", {"Linear", "Sine", "Quad", "Cubic", "Quart", "Quint", "Expo", "Circ", "Elastic", "Back", "Bounce"}, "DashEasingStyle", function(val) Options.DashEasingStyle.Value = val end)
CreateDropdown(SecCombat, "Dash Easing Direction", {"In", "Out", "InOut"}, "DashEasingDirection", function(val) Options.DashEasingDirection.Value = val end)

-- TAB 2: VISUALS
local TabVisuals = CreateMainTab("Visuals")
local SubESPBox = CreateSubTab(TabVisuals, "ESP Overlays")
local SecESPBox = CreateSection(SubESPBox, "Box & Shape ESP")

CreateToggle(SecESPBox, "Box ESP", false, function(state) Toggles.ESPBox.Value = state end)
CreateToggle(SecESPBox, "Corner ESP", false, function(state) Toggles.ESPCorner.Value = state end)
CreateToggle(SecESPBox, "Outline ESP", false, function(state) Toggles.ESPOutline.Value = state end)
CreateToggle(SecESPBox, "Skeleton ESP", false, function(state) Toggles.ESPSkeleton.Value = state end)
CreateToggle(SecESPBox, "Head Dot", false, function(state) Toggles.ESPHeadDot.Value = state end)
CreateToggle(SecESPBox, "Tracer ESP", false, function(state) Toggles.ESPTracer.Value = state end)

local SubESPInfo = CreateSubTab(TabVisuals, "ESP Information")
local SecESPInfo = CreateSection(SubESPInfo, "Text & Data ESP")

CreateToggle(SecESPInfo, "Name ESP", false, function(state) Toggles.ESPName.Value = state end)
CreateToggle(SecESPInfo, "Character Moveset ESP", false, function(state) Toggles.ESPCharacter.Value = state end)
CreateToggle(SecESPInfo, "Distance ESP", false, function(state) Toggles.ESPDistance.Value = state end)
CreateToggle(SecESPInfo, "Kill ESP", false, function(state) Toggles.ESPKill.Value = state end)
CreateToggle(SecESPInfo, "HP Text", "Shows actual Health values", false, function(state) Toggles.ESPHPText.Value = state end)
CreateToggle(SecESPInfo, "HP Bar", "Shows a graphical Health bar", false, function(state) Toggles.ESPHPBar.Value = state end)
CreateToggle(SecESPInfo, "Cooldown Revealer", "Shows enemy moveset cooldowns.", false, function(state) Toggles.CooldownRevealer.Value = state end)

-- TAB 3: LOCK
local TabLock = CreateMainTab("Lock")
local SubLockMain = CreateSubTab(TabLock, "Target Lock")
local SecLockMain = CreateSection(SubLockMain, "Lock Settings")

CreateToggle(SecLockMain, "Enable Lock", false, function(state) 
    Toggles.EnableLock.Value = state
    Logic.LockState.Enabled = state
    if Logic.LockSettings.LockNotification then
        if state then Library:Notify({ Title = "Target Lock", Description = "Lock ENABLED - searching for targets...", Time = 2 })
        else
            if Logic.LockState.LastTargetName then Library:Notify({ Title = "Target Lock", Description = "Unlocked from " .. tostring(Logic.LockState.LastTargetName), Time = 2 }) else Library:Notify({ Title = "Target Lock", Description = "Lock DISABLED", Time = 2 }) end
            Logic.LockState.LastTargetName = nil
        end
    end
end)
CreateKeybind(SecLockMain, "Lock Keybind", Enum.KeyCode.C, function(key) Options.LockKeybind.Value = key end)

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Options.LockKeybind.Value then
        Toggles.EnableLock.Value = not Toggles.EnableLock.Value
        Logic.LockState.Enabled = Toggles.EnableLock.Value
        if Logic.LockSettings.LockNotification then
            if Toggles.EnableLock.Value then Library:Notify({ Title = "Target Lock", Description = "Lock ENABLED", Time = 2 })
            else Library:Notify({ Title = "Target Lock", Description = "Lock DISABLED", Time = 2 }) end
        end
    end
end)

CreateToggle(SecLockMain, "Lock ESP", false, function(state) Toggles.LockESP.Value = state; Logic.LockSettings.ESPEnabled = state end)

local srvDropLock = CreateDropdown(SecLockMain, "Target Player", {"Auto"}, "LockSpecificPlayer", function(val) 
    Options.LockSpecificPlayer.Value = val; Logic.LockSettings.SelectedPlayer = val
    if val ~= "Auto" then Logic.LockState.CurrentLockTarget = nil; Logic.LockState.LastTargetSearch = 0 end
end)
Options.LockSpecificPlayer._uiRefresh = srvDropLock.Refresh

CreateButton(SecLockMain, "Refresh Player List", Theme.SectionBG, function() refreshPlayerDropdown(); Library:Notify({ Title = "Target Lock", Description = "Player list refreshed!", Time = 2 }) end)
CreateToggle(SecLockMain, "Wall Check", false, function(state) Logic.LockSettings.WallCheck = state end)
CreateToggle(SecLockMain, "Auto Retarget", false, function(state) Logic.LockSettings.AutoRetarget = state end)
CreateToggle(SecLockMain, "Lock Notifications", true, function(state) Logic.LockSettings.LockNotification = state end)

local SubLockConfig = CreateSubTab(TabLock, "Configuration")
local SecLockConfig = CreateSection(SubLockConfig, "Advanced Settings")
CreateDropdown(SecLockConfig, "Method", {"Camera", "Body"}, "LockMethod", function(val) Logic.LockSettings.Method = val end)
CreateDropdown(SecLockConfig, "Target Mode", {"Closest", "Closest to Mouse"}, "LockTargetMode", function(val) Logic.LockSettings.TargetMode = val end)
CreateDropdown(SecLockConfig, "Target Part", {"Head", "HumanoidRootPart", "UpperTorso", "LowerTorso"}, "LockTargetPart", function(val) Logic.LockSettings.TargetPart = val end)
CreateSlider(SecLockConfig, "Smoothness", 0, 20, 0, function(val) Logic.LockSettings.Smoothness = val end)
CreateSlider(SecLockConfig, "Camera Side Offset", -8, 8, 1.75, function(val) Logic.LockSettings.SideOffset = val end)
CreateSlider(SecLockConfig, "Max Lock Distance", 10, 2000, 500, function(val) Logic.LockSettings.MaxDistance = val end)
CreateSlider(SecLockConfig, "Prediction", 0, 1, 0, function(val) Logic.LockSettings.Prediction = val end)

-- TAB 4: TELEPORTS
local TabTeleports = CreateMainTab("Teleports")
local SubTeleports = CreateSubTab(TabTeleports, "Travel")
local SecTeleports = CreateSection(SubTeleports, "Auto Teleport Settings")

CreateDropdown(SecTeleports, "Destination", LocationNames, "TeleportDestination", function(val) Options.TeleportDestination.Value = val end)
CreateDropdown(SecTeleports, "Teleport Method", {"Tween", "Tween + Invis"}, "TeleportMode", function(val) Options.TeleportMode.Value = val end)
CreateSlider(SecTeleports, "Teleport Speed (Studs/s)", 30, 1500, 350, function(val) Logic.Pathfinding.Speed = val end)
CreateToggle(SecTeleports, "Show Path Visualization", true, function(state) Logic.Pathfinding.VisualizeOn = state end)

CreateButton(SecTeleports, "Start Teleport", Theme.SectionBG, function()
    local destName = Options.TeleportDestination.Value
    local destPos = TeleportLocations[destName]
    if destPos then
        if Options.TeleportMode.Value == "Tween + Invis" then Invisibility.toggle(true, false) end
        Library:Notify({ Title = "Teleport", Description = "Pathfinding to " .. destName .. "...", Time = 2 })
        startPathfinding(destPos)
    end
end)
CreateButton(SecTeleports, "Stop Teleport", Theme.SectionBG, function()
    stopPathfinding()
    Library:Notify({ Title = "Teleport", Description = "Teleport manually stopped.", Time = 2 })
end)

-- TAB 5: SIDE DASH
local TabSideDash = CreateMainTab("Side Dash")
local SubSideDash = CreateSubTab(TabSideDash, "Side Dash Assist")
local SecSideDash = CreateSection(SubSideDash, "Side Dash Settings")

CreateToggle(SecSideDash, "Enable Side Dash Assist", false, function(state) Toggles.EnableDashAssist.Value = state; Logic.DashAssistState.Enabled = state end)
CreateToggle(SecSideDash, "Lock Camera On Enemy", false, function(state) Logic.DashAssistState.CameraLock = state end)
CreateToggle(SecSideDash, "Dash Only If Facing Front", false, function(state) Logic.DashAssistState.OnlyIfFacing = state end)
CreateKeybind(SecSideDash, "Dash Keybind", Enum.KeyCode.J, function(key) Options.DashAssistKeybind.Value = key end)
CreateSlider(SecSideDash, "Detection Range", 1, 50, 15, function(val) Logic.DashAssistState.DetectionRange = val end)
CreateSlider(SecSideDash, "Behind Distance", 1, 15, 5, function(val) Logic.DashAssistState.BehindDistance = val end)
CreateSlider(SecSideDash, "Flight Duration", 0.1, 1.0, 0.42, function(val) Logic.DashAssistState.FlightDuration = val end)

local SubDashConfig = CreateSubTab(TabSideDash, "Arc Modifiers")
local SecDashConfig = CreateSection(SubDashConfig, "Arc Modifiers")
CreateSlider(SecDashConfig, "Curve Strength", 0, 25, 10, function(val) Logic.DashAssistState.CurveStrength = val end)
CreateSlider(SecDashConfig, "Arch Height", 0, 10, 3, function(val) Logic.DashAssistState.ArchHeight = val end)
CreateSlider(SecDashConfig, "Lock Duration", 0.1, 1.5, 0.35, function(val) Logic.DashAssistState.LockDuration = val end)

-- TAB 6: SERVERS
local TabServers = CreateMainTab("Servers")
local SubServerList = CreateSubTab(TabServers, "Server Browser")

local SecServerInfo = CreateSection(SubServerList, "Server Details")
local SrvInfoLabel = CreateLabel(SecServerInfo, "No server selected.\n\nClick 'Refresh Server List' to load servers,\nthen select one from the dropdown.")
SrvInfoLabel.Name = "ServerDetailsLabel"

CreateButton(SecServerInfo, "Join Selected Server", Theme.SectionBG, function()
    if not Logic.SelectedServerData then Library:Notify({ Title = "Servers", Description = "No server selected!", Time = 3 }); return end
    if Logic.SelectedServerData.IsCurrent then Library:Notify({ Title = "Servers", Description = "You're already in this server!", Time = 3 }); return end
    Library:Notify({ Title = "Servers", Description = "Teleporting...", Time = 3 })
    TeleportService:TeleportToPlaceInstance(game.PlaceId, Logic.SelectedServerData.JobId, Player)
end)
CreateButton(SecServerInfo, "Join Lowest Ping Server", Theme.SectionBG, function()
    if #Logic.ServerList == 0 then Library:Notify({ Title = "Servers", Description = "Refresh the server list first!", Time = 3 }); return end
    local best = nil; for _, srv in ipairs(Logic.ServerList) do if not srv.IsCurrent and srv.Playing > 0 and srv.Ping > 0 then if not best or srv.Ping < best.Ping then best = srv end end end
    if best then Library:Notify({ Title = "Servers", Description = "Joining best ping server (" .. best.Ping .. "ms, " .. best.Playing .. " players)...", Time = 3 }); TeleportService:TeleportToPlaceInstance(game.PlaceId, best.JobId, Player)
    else Library:Notify({ Title = "Servers", Description = "No valid server found!", Time = 3 }) end
end)
CreateButton(SecServerInfo, "Join Smallest Server", Theme.SectionBG, function()
    if #Logic.ServerList == 0 then Library:Notify({ Title = "Servers", Description = "Refresh the server list first!", Time = 3 }); return end
    local smallest = nil; for _, srv in ipairs(Logic.ServerList) do if not srv.IsCurrent and srv.Playing > 0 then if not smallest or srv.Playing < smallest.Playing then smallest = srv end end end
    if smallest then Library:Notify({ Title = "Servers", Description = "Joining smallest server (" .. smallest.Playing .. " players, " .. smallest.Ping .. "ms)...", Time = 3 }); TeleportService:TeleportToPlaceInstance(game.PlaceId, smallest.JobId, Player)
    else Library:Notify({ Title = "Servers", Description = "No valid server found!", Time = 3 }) end
end)
CreateButton(SecServerInfo, "Join Random Server", Theme.SectionBG, function()
    if #Logic.ServerList == 0 then Library:Notify({ Title = "Servers", Description = "Refresh the server list first!", Time = 3 }); return end
    local candidates = {}; for _, srv in ipairs(Logic.ServerList) do if not srv.IsCurrent then table.insert(candidates, srv) end end
    if #candidates > 0 then local pick = candidates[math.random(1, #candidates)]; Library:Notify({ Title = "Servers", Description = "Joining random server (" .. pick.Playing .. " players, " .. pick.Ping .. "ms)...", Time = 3 }); TeleportService:TeleportToPlaceInstance(game.PlaceId, pick.JobId, Player)
    else Library:Notify({ Title = "Servers", Description = "No other servers available!", Time = 3 }) end
end)
CreateButton(SecServerInfo, "Rejoin Current Server", Theme.SectionBG, function()
    Library:Notify({ Title = "Servers", Description = "Rejoining...", Time = 3 })
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, Player)
end)

local SecServerStats = CreateSection(SubServerList, "Server Statistics")
local SrvStatsLabel = CreateLabel(SecServerStats, "No data. Refresh to load servers.")
SrvStatsLabel.Name = "ServerStatsLabel"

local SecServerList = CreateSection(SubServerList, "Server List & Filters")
CreateButton(SecServerList, "Refresh Server List", Theme.SectionBG, function()
    task.spawn(function()
        local sList = fetchServers()
        local vals = buildServerMap(sList)
        if #vals > 0 then Options.ServerDropdown:SetValues(vals); Options.ServerDropdown.Value = vals[1] else Options.ServerDropdown:SetValues({ "No servers found" }) end
        updateStatsLabel()
    end)
end)

local srvDropList = CreateDropdown(SecServerList, "Select Server", {"Click 'Refresh' to load servers"}, "ServerDropdown", function(val)
    Options.ServerDropdown.Value = val
    Logic.SelectedServerData = Logic.ServerMap[val]
    updateServerInfo(Logic.SelectedServerData)
end)
Options.ServerDropdown._uiRefresh = srvDropList.Refresh

CreateDropdown(SecServerList, "Sort By", {"Default (Roblox)", "Most Players", "Least Players", "Lowest Ping", "Highest Ping", "Highest FPS"}, "ServerSortBy", function(Value)
    if #Logic.ServerList == 0 then return end
    local sorted = {}; for i, v in ipairs(Logic.ServerList) do sorted[i] = v end
    if Value == "Most Players" then table.sort(sorted, function(a, b) return a.Playing > b.Playing end)
    elseif Value == "Least Players" then table.sort(sorted, function(a, b) return a.Playing < b.Playing end)
    elseif Value == "Lowest Ping" then table.sort(sorted, function(a, b) return a.Ping < b.Ping end)
    elseif Value == "Highest Ping" then table.sort(sorted, function(a, b) return a.Ping > b.Ping end)
    elseif Value == "Highest FPS" then table.sort(sorted, function(a, b) return a.FPS > b.FPS end) end
    local vals = buildServerMap(sorted)
    Options.ServerDropdown:SetValues(vals)
end)
CreateSlider(SecServerList, "Min Players Filter", 0, 50, 0, function(val) Options.MinPlayersFilter.Value = val end)
CreateSlider(SecServerList, "Max Ping Filter", 10, 999, 999, function(val) Options.MaxPingFilter.Value = val end)
CreateButton(SecServerList, "Apply Filters", Theme.SectionBG, function()
    if #Logic.ServerList == 0 then Library:Notify({ Title = "Servers", Description = "Refresh the server list first!", Time = 3 }); return end
    local minP = Options.MinPlayersFilter.Value; local maxPing = Options.MaxPingFilter.Value
    local filtered = {}; for _, entry in ipairs(Logic.ServerList) do if entry.Playing >= minP and (maxPing >= 999 or entry.Ping <= maxPing) then table.insert(filtered, entry) end end
    local vals = buildServerMap(filtered)
    Options.ServerDropdown:SetValues(#vals > 0 and vals or { "No servers match filters" })
    Library:Notify({ Title = "Servers", Description = "Showing " .. #vals .. " servers after filtering.", Time = 3 })
end)

-- TAB 7: UI SETTINGS
local TabSettings = CreateMainTab("UI Settings")
local SubUI = CreateSubTab(TabSettings, "Configure UI")
local SecUI = CreateSection(SubUI, "UI Controls")
CreateButton(SecUI, "Unload UI", Theme.SectionBG, function()
    if Logic.Connections.ESP then Logic.Connections.ESP:Disconnect() end
    if Logic.Connections.LockZoom then Logic.Connections.LockZoom:Disconnect() end
    if Logic.Connections.PlayerAdded then Logic.Connections.PlayerAdded:Disconnect() end
    if Logic.Connections.PlayerRemoving then Logic.Connections.PlayerRemoving:Disconnect() end
    if Logic.Connections.DashInput then Logic.Connections.DashInput:Disconnect() end
    pcall(function() RunService:UnbindFromRenderStep("RazorbillTargetLock") end)
    if Logic.LockState.ESPGui then pcall(function() Logic.LockState.ESPGui:Destroy() end) end
    if Logic.LockState.LockBodyGyro then pcall(function() Logic.LockState.LockBodyGyro:Destroy() end) end
    if Logic.LockState.FOVCircle then pcall(function() Logic.LockState.FOVCircle:Remove() end) end
    if Logic.LockState.TargetInfoName then pcall(function() Logic.LockState.TargetInfoName:Remove() end) end
    if Logic.LockState.TargetInfoHP then pcall(function() Logic.LockState.TargetInfoHP:Remove() end) end
    if Logic.LockState.TargetInfoDist then pcall(function() Logic.LockState.TargetInfoDist:Remove() end) end
    if Logic.LockState.TargetInfoLine then pcall(function() Logic.LockState.TargetInfoLine:Remove() end) end
    stopPathfinding() 
    if workspace.CurrentCamera then workspace.CurrentCamera.CameraType = Enum.CameraType.Custom; local char = Player.Character; local hum = char and char:FindFirstChildOfClass("Humanoid"); if hum then workspace.CurrentCamera.CameraSubject = hum; hum.AutoRotate = true end end
    for _, e in pairs(Logic.ESPObjects) do hideAll(e); removeAll(e) end
    Logic.ESPObjects = {}
    RyuHub:Destroy()
end)

-- OPEN FIRST TAB ON LOAD
pcall(function()
    if Tabs[1] and Tabs[1].Btn then
        Tabs[1].IsOpen = true; Tabs[1].SubContainer.Size = UDim2.new(1, 0, 0, Tabs[1].SubLayout.AbsoluteContentSize.Y); Tabs[1].Btn.TextColor3 = Theme.Text; Tabs[1].Btn.BackgroundColor3 = Theme.SectionBG
    end
    if Tabs[1] and Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Page then
        Tabs[1].SubTabs[1].Page.Visible = true; Tabs[1].SubTabs[1].Btn.TextColor3 = Theme.Text
    end
    UpdateSidebarCanvas()
end)

-- AUTO-FETCH SERVERS
task.spawn(function()
    task.wait(2)
    local sList = fetchServers()
    local vals = buildServerMap(sList)
    if #vals > 0 then Options.ServerDropdown:SetValues(vals); Options.ServerDropdown.Value = vals[1] else Options.ServerDropdown:SetValues({ "No servers found" }) end
    updateStatsLabel()
end)

-- ryu hub - jujutsu shenanigans (tjs) v1.4
-- fixed ui, anticheat tp bypass, lock-on overhaul, new auto item & ai logic

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
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

-- gui cleanup
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
    if v.Name == "RyuHubTJS" then v:Destroy() end 
end

-- global config
local CONFIG_FILE = "RyuHub_TJS_Settings.json"
local RyuConfig = {
    Speed = false, SpeedVal = 50,
    Fly = false, FlySpeed = 50, FlyKey = Enum.KeyCode.X,
    JumpHigh = false, JumpPower = 150, JumpSpam = false,
    Noclip = false, Invisible = false,
    
    AutoBlock = false, BlockRange = 15, BlockTime = 500,
    AutoBlackFlash = false, AutoDodge = false, DodgeRange = 20,
    
    AutoItem = false, TargetItem = "None",
    
    LockOn = false, LockOnKey = Enum.KeyCode.C, LockOnY = 0,
    Knockback = false, KnockbackVal = 50,
    DomainBypass = false, DashSpam = false,
    
    AIFarm = false, AIRange = 50, AutoUlt = false,
    S1 = false, S2 = false, S3 = false, S4 = false,
    
    TargetPlayer = "None", TargetFarm = false, TargetDist = 3,
    MoneyFarm = false, MFRole = "Farmer", MFVictim = "",
    
    AutoRejoin = false, MinPlayers = 3, HighPop = true,
    TargetJoinUser = "", AntiAFK = false,
    
    GuiColor = Color3.fromRGB(255, 255, 255)
}

-- save/load
local function SaveConfig()
    pcall(function() if writefile then writefile(CONFIG_FILE, HttpService:JSONEncode(RyuConfig)) end end)
end
pcall(function()
    if readfile and isfile and isfile(CONFIG_FILE) then
        local data = HttpService:JSONDecode(readfile(CONFIG_FILE))
        for k,v in pairs(data) do RyuConfig[k] = v end
    end
end)

-- theme setup (monochrome aba style)
local Theme = {
    Background = Color3.fromRGB(12, 12, 14), Sidebar = Color3.fromRGB(18, 18, 20),
    SectionBG = Color3.fromRGB(24, 24, 26), Text = Color3.fromRGB(250, 250, 250),
    SubText = Color3.fromRGB(130, 130, 135), CloudLight = Color3.fromRGB(255, 255, 255),
    CloudDark = Color3.fromRGB(60, 60, 65), Accent = RyuConfig.GuiColor,
    ToggleOff = Color3.fromRGB(35, 35, 38), ToggleOn = RyuConfig.GuiColor,
    Stroke = Color3.fromRGB(45, 45, 50), Warning = Color3.fromRGB(255, 60, 60)
}

local MainSize = UDim2.new(0, math.min(750, camera.ViewportSize.X - 40), 0, math.min(480, camera.ViewportSize.Y - 40))
local SidebarWidth = 160

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubTJS"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

local function AddHoverEffect(element, def, hov)
    element.MouseEnter:Connect(function() pcall(function() TweenService:Create(element, TweenInfo.new(0.2), {BackgroundColor3 = hov}):Play() end) end)
    element.MouseLeave:Connect(function() pcall(function() TweenService:Create(element, TweenInfo.new(0.2), {BackgroundColor3 = def}):Play() end) end)
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

-- KNIT REMOTES
local remotes = {}
task.spawn(function()
    local ks = ReplicatedStorage:WaitForChild("Knit", 5)
    if ks then
        local serv = ks:WaitForChild("Services", 5)
        if serv then
            remotes.BlockOn = serv:FindFirstChild("BlockService") and serv.BlockService.RE:FindFirstChild("Activated")
            remotes.BlockOff = serv:FindFirstChild("BlockService") and serv.BlockService.RE:FindFirstChild("Deactivated")
            remotes.Chase = serv:FindFirstChild("ItadoriService") and serv.ItadoriService.RE:FindFirstChild("Chase")
            remotes.ItadoriActivated = serv:FindFirstChild("ItadoriService") and serv.ItadoriService.RE:FindFirstChild("Activated")
            remotes.Divergent = serv:FindFirstChild("DivergentFistService") and serv.DivergentFistService.RE:FindFirstChild("Activated")
            remotes.Teleport = serv:FindFirstChild("AntiCheatService") and serv.AntiCheatService.RE:FindFirstChild("Teleport")
        end
    end
end)

local function TJSTeleport(cframeObj)
    pcall(function()
        if remotes.Teleport then
            remotes.Teleport:FireServer(1786781036.3533041)
        end
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframeObj
    end)
end

-- FIXED TOP LEFT TOGGLE BUTTON (Under Roblox Logo)
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
Blade.Size = UDim2.new(0, 2, 0, 24); Blade.Position = UDim2.new(0.5, -1, 0.5, -18); Blade.BackgroundColor3 = Theme.CloudLight; Blade.BorderSizePixel = 0
local BladeGlow = Instance.new("UIStroke", Blade)
BladeGlow.Color = Theme.Accent; BladeGlow.Thickness = 1; BladeGlow.Transparency = 0.5
local Guard = Instance.new("Frame", Katana)
Guard.Size = UDim2.new(0, 12, 0, 2); Guard.Position = UDim2.new(0.5, -6, 0.5, 6); Guard.BackgroundColor3 = Theme.CloudDark; Guard.BorderSizePixel = 0
local Handle = Instance.new("Frame", Katana)
Handle.Size = UDim2.new(0, 4, 0, 10); Handle.Position = UDim2.new(0.5, -2, 0.5, 8); Handle.BackgroundColor3 = Color3.fromRGB(40, 45, 50); Handle.BorderSizePixel = 0
Instance.new("UICorner", Blade).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Guard).CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", Handle).CornerRadius = UDim.new(0, 1)

AddClickPop(ToggleBtn)

-- main frame setup
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
mainStroke.Color = Theme.Stroke; mainStroke.Transparency = 0.2; mainStroke.Thickness = 1.5

local DragText = Instance.new("TextLabel", MainFrame)
DragText.Size = UDim2.new(1, 0, 1, 0); DragText.Position = UDim2.new(0, 0, 0, 0); DragText.BackgroundTransparency = 1
DragText.Text = "DISCORD.GG/RYUHUB"; DragText.Font = Enum.Font.GothamBlack; DragText.TextSize = 50
DragText.TextColor3 = Color3.fromRGB(255, 255, 255); DragText.TextTransparency = 1; DragText.ZIndex = 0

local DragGradient = Instance.new("UIGradient", DragText)
DragGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(150, 150, 150)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 255, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(150, 150, 150))}

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

local TitleGradient = Instance.new("UIGradient", Title)
TitleGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(180, 180, 185)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 255, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(180, 180, 185))}
TitleGradient.Offset = Vector2.new(-1, 0)
task.spawn(function() pcall(function() TweenService:Create(TitleGradient, TweenInfo.new(2.0, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Offset = Vector2.new(1, 0)}):Play() end) end)

local SubTitle = Instance.new("TextLabel", Topbar)
SubTitle.Size = UDim2.new(0, 300, 0, 15); SubTitle.Position = UDim2.new(0, 20, 0, 38); SubTitle.BackgroundTransparency = 1
SubTitle.Text = "Jujutsu Shenanigans"; SubTitle.TextColor3 = Theme.SubText; SubTitle.Font = Enum.Font.Gotham; SubTitle.TextSize = 11; SubTitle.TextXAlignment = Enum.TextXAlignment.Left

-- RED CLOSE BUTTON
local CloseBtn = Instance.new("TextButton", Topbar)
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(1, -40, 0, 15)
CloseBtn.BackgroundColor3 = Theme.Warning
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 14
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)
CloseBtn.MouseButton1Click:Connect(function()
    pcall(function() TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play() end)
    task.delay(0.3, function() MainFrame.Visible = false end)
end)

local mDragging, mDragStart, mStartPos = false, nil, nil
Topbar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then 
        mDragging = true; mDragStart = input.Position; mStartPos = MainFrame.Position 
        pcall(function() TweenService:Create(DragText, TweenInfo.new(0.2), {TextTransparency = 0.1}):Play() end)
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
        pcall(function() TweenService:Create(DragText, TweenInfo.new(0.2), {TextTransparency = 1}):Play() end)
    end
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
local sidebarOrderCounter = 0
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
    local tabObj = { Btn = nil, Arrow = nil, SubContainer = nil, SubLayout = nil, IsOpen = false, Toggle = nil, SubTabs = {} }
    sidebarOrderCounter = sidebarOrderCounter + 1
    
    local tabBtn = Instance.new("TextButton", Sidebar)
    tabBtn.LayoutOrder = sidebarOrderCounter; tabBtn.Size = UDim2.new(1, 0, 0, 36); tabBtn.BackgroundColor3 = Theme.Sidebar; tabBtn.Text = "  " .. string.upper(name)
    tabBtn.TextColor3 = Theme.SubText; tabBtn.Font = Enum.Font.GothamBlack; tabBtn.TextSize = 13; tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 8)
    tabObj.Btn = tabBtn

    local arrow = Instance.new("TextLabel", tabBtn)
    arrow.Size = UDim2.new(0, 20, 1, 0); arrow.Position = UDim2.new(1, -25, 0, 0); arrow.BackgroundTransparency = 1; arrow.Text = "v"
    arrow.TextColor3 = Theme.SubText; arrow.Font = Enum.Font.GothamBold; arrow.TextSize = 12
    tabObj.Arrow = arrow

    sidebarOrderCounter = sidebarOrderCounter + 1
    local subContainer = Instance.new("Frame", Sidebar)
    subContainer.LayoutOrder = sidebarOrderCounter; subContainer.Size = UDim2.new(1, 0, 0, 0); subContainer.BackgroundTransparency = 1; subContainer.ClipsDescendants = true
    tabObj.SubContainer = subContainer

    local subLayout = Instance.new("UIListLayout", subContainer)
    subLayout.Padding = UDim.new(0, 2); subLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left; subLayout.SortOrder = Enum.SortOrder.LayoutOrder
    tabObj.SubLayout = subLayout

    local function toggleTab()
        tabObj.IsOpen = not tabObj.IsOpen
        local targetSize = tabObj.IsOpen and UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) or UDim2.new(1, 0, 0, 0)
        pcall(function()
            TweenService:Create(subContainer, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = targetSize}):Play()
            if tabObj.IsOpen then
                arrow.Text = "^"; TweenService:Create(tabBtn, TweenInfo.new(0.25), {TextColor3 = Theme.Text, BackgroundColor3 = Theme.SectionBG}):Play()
                TweenService:Create(arrow, TweenInfo.new(0.25), {TextColor3 = Theme.Text}):Play()
            else
                arrow.Text = "v"; TweenService:Create(tabBtn, TweenInfo.new(0.25), {TextColor3 = Theme.SubText, BackgroundColor3 = Theme.Sidebar}):Play()
                TweenService:Create(arrow, TweenInfo.new(0.25), {TextColor3 = Theme.SubText}):Play()
            end
        end)
        task.delay(0.26, UpdateSidebarCanvas)
    end

    tabBtn.MouseButton1Click:Connect(toggleTab)
    tabObj.Toggle = toggleTab

    subLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        if tabObj.IsOpen then subContainer.Size = UDim2.new(1, 0, 0, subLayout.AbsoluteContentSize.Y) end
        UpdateSidebarCanvas()
    end)

    table.insert(Tabs, tabObj)
    return tabObj
end

local function CreateSubTab(tabObj, subName)
    local subObj = { Btn = nil, Page = nil, Indicator = nil, Open = nil }

    local subBtn = Instance.new("TextButton", tabObj.SubContainer)
    subBtn.LayoutOrder = #tabObj.SubTabs + 1; subBtn.Size = UDim2.new(1, 0, 0, 28); subBtn.BackgroundTransparency = 1; subBtn.Text = "     " .. subName
    subBtn.TextColor3 = Theme.SubText; subBtn.Font = Enum.Font.GothamMedium; subBtn.TextSize = 12; subBtn.TextXAlignment = Enum.TextXAlignment.Left
    subObj.Btn = subBtn

    local indicator = Instance.new("Frame", subBtn)
    indicator.Size = UDim2.new(0, 16, 0, 2); indicator.Position = UDim2.new(0, 20, 1, -4); indicator.BackgroundColor3 = Theme.Accent; indicator.BorderSizePixel = 0; indicator.BackgroundTransparency = 1
    Instance.new("UICorner", indicator).CornerRadius = UDim.new(1, 0)
    subObj.Indicator = indicator

    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Size = UDim2.new(1, 0, 1, 0); page.BackgroundTransparency = 1; page.ScrollBarThickness = 2; page.ScrollBarImageColor3 = Theme.Accent; page.Visible = false
    subObj.Page = page

    local pageLayout = Instance.new("UIListLayout", page)
    pageLayout.Padding = UDim.new(0, 12); pageLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    pageLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() page.CanvasSize = UDim2.new(0, 0, 0, pageLayout.AbsoluteContentSize.Y + 20) end)

    local function openSubTab()
        pcall(function()
            for _, t in pairs(Tabs) do
                for _, st in pairs(t.SubTabs) do
                    st.Page.Visible = false
                    TweenService:Create(st.Btn, TweenInfo.new(0.2), {TextColor3 = Theme.SubText}):Play()
                    TweenService:Create(st.Indicator, TweenInfo.new(0.2), {BackgroundTransparency = 1}):Play()
                end
            end
            page.Visible = true
            TweenService:Create(subBtn, TweenInfo.new(0.2), {TextColor3 = Theme.Text}):Play()
            TweenService:Create(indicator, TweenInfo.new(0.2), {BackgroundTransparency = 0}):Play()
        end)
    end

    subBtn.MouseButton1Click:Connect(openSubTab)
    subObj.Open = openSubTab
    table.insert(tabObj.SubTabs, subObj)
    return page
end

local function CreateSection(page, titleText)
    local section = Instance.new("Frame", page)
    section.Size = UDim2.new(0.98, 0, 0, 50); section.BackgroundColor3 = Theme.SectionBG; section.BackgroundTransparency = 0
    Instance.new("UICorner", section).CornerRadius = UDim.new(0, 10)
    local sStroke = Instance.new("UIStroke", section); sStroke.Color = Theme.Stroke; sStroke.Transparency = 0.2
    
    local secLayout = Instance.new("UIListLayout", section)
    secLayout.Padding = UDim.new(0, 10); secLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center; secLayout.SortOrder = Enum.SortOrder.LayoutOrder
    local secPadding = Instance.new("UIPadding", section)
    secPadding.PaddingTop = UDim.new(0, 12); secPadding.PaddingBottom = UDim.new(0, 12)
    
    local title = Instance.new("TextLabel", section)
    title.LayoutOrder = -1; title.Size = UDim2.new(0.92, 0, 0, 24); title.BackgroundTransparency = 1; title.Text = titleText
    title.TextColor3 = Theme.Text; title.Font = Enum.Font.GothamBold; title.TextSize = 14; title.TextXAlignment = Enum.TextXAlignment.Left
    
    secLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() section.Size = UDim2.new(1, 0, 0, secLayout.AbsoluteContentSize.Y + 24) end)
    return section
end

local function CreateLabel(section, text, color)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 30); frame.BackgroundTransparency = 1
    local lbl = Instance.new("TextLabel", frame)
    lbl.Size = UDim2.new(1, 0, 1, 0); lbl.BackgroundTransparency = 1; lbl.Text = text; lbl.TextColor3 = color or Theme.SubText
    lbl.Font = Enum.Font.GothamMedium; lbl.TextSize = 11; lbl.TextXAlignment = Enum.TextXAlignment.Left; lbl.TextWrapped = true
    return lbl
end

local function CreateToggle(section, text, descText, defaultState, callback)
    if type(descText) == "boolean" then callback = defaultState; defaultState = descText; descText = nil end
    if type(defaultState) == "function" then callback = defaultState; defaultState = false end
    
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
    local bStroke = Instance.new("UIStroke", tBtn); bStroke.Color = defaultState and Theme.ToggleOn or Theme.Stroke; bStroke.Transparency = 0.2
    
    local circle = Instance.new("Frame", tBtn)
    circle.Size = UDim2.new(0, 16, 0, 16); circle.Position = defaultState and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8)
    circle.BackgroundColor3 = defaultState and Theme.Background or Color3.fromRGB(150, 150, 150)
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = defaultState or false
    local function fireToggle()
        isOn = not isOn
        pcall(function()
            if isOn then
                TweenService:Create(tBtn, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {BackgroundColor3 = Theme.ToggleOn}):Play()
                TweenService:Create(circle, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Position = UDim2.new(1, -19, 0.5, -8), BackgroundColor3 = Theme.Background}):Play()
                TweenService:Create(label, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {TextColor3 = Theme.Text}):Play()
                bStroke.Color = Theme.ToggleOn
            else
                TweenService:Create(tBtn, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {BackgroundColor3 = Theme.ToggleOff}):Play()
                TweenService:Create(circle, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Position = UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = Color3.fromRGB(150, 150, 150)}):Play()
                TweenService:Create(label, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {TextColor3 = Theme.SubText}):Play()
                bStroke.Color = Theme.Stroke
            end
        end)
        if callback then pcall(function() callback(isOn) end) end
    end
    tBtn.MouseButton1Click:Connect(fireToggle)
    return {Toggle = fireToggle, Set = function(v) if isOn ~= v then fireToggle() end end}
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
    local function setSlider(value)
        local relative = math.clamp((value - min) / (max - min), 0, 1)
        valLabel.Text = tostring(value)
        pcall(function() TweenService:Create(sliderFill, TweenInfo.new(0.08), {Size = UDim2.new(relative, 0, 1, 0)}):Play() end)
        if callback then pcall(function() callback(value) end) end
    end
    
    knob.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relative = math.clamp((input.Position.X - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
            setSlider(math.floor(min + (max - min) * relative))
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
    local strk = Instance.new("UIStroke", btn); strk.Color = Theme.Stroke; strk.Transparency = 0.5
    AddClickPop(btn)
    btn.MouseButton1Click:Connect(function() pcall(callback) end)
    return btn
end

local function CreateDropdown(section, headerText, itemsList, targetConfigKey, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 160); frame.BackgroundTransparency = 1
    local header = Instance.new("TextLabel", frame)
    header.Size = UDim2.new(1, 0, 0, 20); header.BackgroundTransparency = 1; header.Text = headerText .. ": " .. tostring(RyuConfig[targetConfigKey] or "None")
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
                RyuConfig[targetConfigKey] = itemName; header.Text = headerText .. ": " .. tostring(itemName)
                if callback then callback(itemName) end
            end)
        end
        task.defer(function() scroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 10) end)
    end
    
    listLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() scroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 10) end)
    populate(itemsList)
    return { Refresh = populate }
end

local function CreateKeybind(section, text, defaultKey, callback)
    itemOrderCounter = itemOrderCounter + 1
    local frame = Instance.new("Frame", section)
    frame.LayoutOrder = itemOrderCounter; frame.Size = UDim2.new(0.92, 0, 0, 34); frame.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.7, 0, 0, 34); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Theme.Text
    label.Font = Enum.Font.GothamMedium; label.TextSize = 13; label.TextXAlignment = Enum.TextXAlignment.Left
    
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 80, 0, 22); btn.Position = UDim2.new(1, -80, 0, 6); btn.BackgroundColor3 = Theme.ToggleOff; btn.Text = defaultKey.Name
    btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    local bStroke = Instance.new("UIStroke", btn); bStroke.Color = Theme.Stroke; bStroke.Transparency = 0.2
    
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
--// 5. TABS & FEATURES SETUP
--// ==========================================

-- TAB: COMBAT
local TabCombat = CreateMainTab("Combat")

local SubPlayer = CreateSubTab(TabCombat, "Player")
local SecPlayer = CreateSection(SubPlayer, "Movement Mods")
CreateToggle(SecPlayer, "Speed Hack", RyuConfig.Speed, function(v) RyuConfig.Speed = v end)
CreateSlider(SecPlayer, "Speed Value", 16, 150, RyuConfig.SpeedVal, function(v) RyuConfig.SpeedVal = v end)
CreateToggle(SecPlayer, "Fly", RyuConfig.Fly, function(v) RyuConfig.Fly = v end)
CreateSlider(SecPlayer, "Fly Speed", 10, 200, RyuConfig.FlySpeed, function(v) RyuConfig.FlySpeed = v end)
CreateKeybind(SecPlayer, "Fly Keybind", RyuConfig.FlyKey, function(v) RyuConfig.FlyKey = v end)

CreateToggle(SecPlayer, "High Jump", RyuConfig.JumpHigh, function(v) RyuConfig.JumpHigh = v end)
CreateSlider(SecPlayer, "Jump Power", 50, 300, RyuConfig.JumpPower, function(v) RyuConfig.JumpPower = v end)
CreateToggle(SecPlayer, "Infinite Jump Spam", RyuConfig.JumpSpam, function(v) RyuConfig.JumpSpam = v end)
CreateToggle(SecPlayer, "Noclip", RyuConfig.Noclip, function(v) RyuConfig.Noclip = v end)
CreateToggle(SecPlayer, "Invisible", RyuConfig.Invisible, function(v) RyuConfig.Invisible = v end)

local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAutoBlock = CreateSection(SubAuto, "Defensive")
CreateToggle(SecAutoBlock, "Auto Block", "Reacts to enemy hit animations", RyuConfig.AutoBlock, function(v) RyuConfig.AutoBlock = v end)
CreateSlider(SecAutoBlock, "Block React Range", 5, 50, RyuConfig.BlockRange, function(v) RyuConfig.BlockRange = v end)
CreateSlider(SecAutoBlock, "Block Hold Duration (ms)", 100, 1500, RyuConfig.BlockTime, function(v) RyuConfig.BlockTime = v end)
CreateToggle(SecAutoBlock, "Auto Dodge (TP Back)", RyuConfig.AutoDodge, function(v) RyuConfig.AutoDodge = v end)
CreateSlider(SecAutoBlock, "Dodge Distance", 5, 50, RyuConfig.DodgeRange, function(v) RyuConfig.DodgeRange = v end)

local SecAutoHit = CreateSection(SubAuto, "Offensive")
CreateToggle(SecAutoHit, "Auto Black Flash (Yuji)", "Spams Divergent & Activates M1", RyuConfig.AutoBlackFlash, function(v) RyuConfig.AutoBlackFlash = v end)
CreateLabel(SecAutoHit, "Auto Combos: Join discord.gg/ryuhub and send clips of your combos!")

local SecAutoItem = CreateSection(SubAuto, "Auto Item")
CreateToggle(SecAutoItem, "Enable Auto Item", RyuConfig.AutoItem, function(v) RyuConfig.AutoItem = v end)
local ItemDropdown = CreateDropdown(SecAutoItem, "Target Item", {"None"}, "TargetItem")

task.spawn(function()
    while task.wait(10) do
        pcall(function()
            local items = {"None"}
            local itemFolder = Workspace:FindFirstChild("Items")
            if itemFolder then
                for _,v in pairs(itemFolder:GetChildren()) do
                    if not table.find(items, v.Name) then table.insert(items, v.Name) end
                end
            end
            ItemDropdown:Refresh(items)
        end)
    end
end)

local SubAbil = CreateSubTab(TabCombat, "Abilities")
local SecAbil = CreateSection(SubAbil, "Combat Enhancements")
CreateToggle(SecAbil, "Lock On Target", RyuConfig.LockOn, function(v) RyuConfig.LockOn = v end)
CreateKeybind(SecAbil, "Lock On Keybind", RyuConfig.LockOnKey, function(v) RyuConfig.LockOnKey = v end)
CreateSlider(SecAbil, "Lock On Y-Offset", -10, 10, RyuConfig.LockOnY, function(v) RyuConfig.LockOnY = v end)

if isMobile then
    local lockBtn = Instance.new("TextButton", RyuHub)
    lockBtn.Size = UDim2.new(0, 60, 0, 60)
    lockBtn.Position = UDim2.new(1, -80, 0.5, 0)
    lockBtn.BackgroundColor3 = Theme.Sidebar
    lockBtn.Text = "LOCK"
    lockBtn.TextColor3 = Theme.Accent
    lockBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", lockBtn).CornerRadius = UDim.new(1, 0)
    lockBtn.MouseButton1Click:Connect(function()
        RyuConfig.LockOn = not RyuConfig.LockOn
        lockBtn.BackgroundColor3 = RyuConfig.LockOn and Theme.Accent or Theme.Sidebar
        lockBtn.TextColor3 = RyuConfig.LockOn and Theme.Background or Theme.Accent
    end)
end

CreateToggle(SecAbil, "Knockback M1s", RyuConfig.Knockback, function(v) RyuConfig.Knockback = v end)
CreateSlider(SecAbil, "Knockback Force", 10, 300, RyuConfig.KnockbackVal, function(v) RyuConfig.KnockbackVal = v end)
CreateToggle(SecAbil, "Domain Eraser (Local)", RyuConfig.DomainBypass, function(v) RyuConfig.DomainBypass = v end)
CreateButton(SecAbil, "Teleport All to Me", Theme.ToggleOff, function()
    pcall(function()
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.CFrame = hrp.CFrame * CFrame.new(0,0,-3)
            end
        end
    end)
end)
CreateLabel(SecAbil, "Suggest more abilities in discord.gg/ryuhub")

-- TAB: FARM
local TabFarm = CreateMainTab("Farm")

local SubAIFarm = CreateSubTab(TabFarm, "AI Auto Farm")
local SecAIFarm = CreateSection(SubAIFarm, "Auto Combat")
CreateToggle(SecAIFarm, "Enable AI Farm", "Aggressive camera, blocks, uses remotes", RyuConfig.AIFarm, function(v) RyuConfig.AIFarm = v end)
CreateSlider(SecAIFarm, "Chase Range", 10, 500, RyuConfig.AIRange, function(v) RyuConfig.AIRange = v end)
CreateToggle(SecAIFarm, "Auto Ultimate (G)", "Pops awakening", RyuConfig.AutoUlt, function(v) RyuConfig.AutoUlt = v end)

local SecAISkills = CreateSection(SubAIFarm, "Choose Skills")
local function makeSkillBtn(name, configKey)
    local btn = CreateButton(SecAISkills, name, RyuConfig[configKey] and Theme.Accent or Theme.ToggleOff, function() end)
    btn.MouseButton1Click:Connect(function()
        RyuConfig[configKey] = not RyuConfig[configKey]
        btn.BackgroundColor3 = RyuConfig[configKey] and Theme.Accent or Theme.ToggleOff
        btn.TextColor3 = RyuConfig[configKey] and Theme.Background or Theme.Text
    end)
end
makeSkillBtn("Skill 1", "S1")
makeSkillBtn("Skill 2", "S2")
makeSkillBtn("Skill 3", "S3")
makeSkillBtn("Skill 4", "S4")

local SubTarget = CreateSubTab(TabFarm, "Target")
local SecTarget = CreateSection(SubTarget, "Specific Target Follow")
local PlrDrop = CreateDropdown(SecTarget, "Target Player", {"None"}, "TargetPlayer")
CreateToggle(SecTarget, "Enable Target Farm", RyuConfig.TargetFarm, function(v) RyuConfig.TargetFarm = v end)

task.spawn(function()
    while task.wait(5) do
        pcall(function()
            local plrs = {"None"}
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer then table.insert(plrs, p.Name) end
            end
            PlrDrop:Refresh(plrs)
        end)
    end
end)

local SubMFarm = CreateSubTab(TabFarm, "Money Farm")
local SecMFarm = CreateSection(SubMFarm, "Alt Money Farm")
CreateToggle(SecMFarm, "Enable Money Farm", RyuConfig.MoneyFarm, function(v) RyuConfig.MoneyFarm = v end)
local RoleBtn
RoleBtn = CreateButton(SecMFarm, "Role: " .. RyuConfig.MFRole, Theme.ToggleOff, function()
    if RyuConfig.MFRole == "Farmer" then RyuConfig.MFRole = "Victim" else RyuConfig.MFRole = "Farmer" end
    RoleBtn.Text = "Role: " .. RyuConfig.MFRole
end)

-- TAB: SETTINGS
local TabSettings = CreateMainTab("Settings")
local SubSettings = CreateSubTab(TabSettings, "Settings")
local SecCfg = CreateSection(SubSettings, "System & Protection")
CreateToggle(SecCfg, "Anti-AFK Protection", "Prevents Roblox Kick", RyuConfig.AntiAFK, function(v) RyuConfig.AntiAFK = v end)
CreateButton(SecCfg, "Save Settings", Theme.SectionBG, function() SaveConfig() end)

-- init
pcall(function() 
    if Tabs[1] and Tabs[1].Toggle then Tabs[1].Toggle() end
    if Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Open then Tabs[1].SubTabs[1].Open() end 
end)

--// 6. TJS RUNTIME EXPLOITATION ENGINE
local lastHealth = 100
local bv = nil

-- Domain Eraser
Workspace.ChildAdded:Connect(function(c)
    if RyuConfig.DomainBypass then
        task.wait(0.1)
        pcall(function()
            if c.Name:lower():find("domain") or c.Name:lower():find("sphere") or c:IsA("MeshPart") then
                c.Transparency = 1
                if c:IsA("Model") then
                    for _, p in pairs(c:GetDescendants()) do
                        if p:IsA("BasePart") then p.Transparency = 1 end
                    end
                end
            end
        end)
    end
end)

-- Anti AFK
LocalPlayer.Idled:Connect(function()
    if RyuConfig.AntiAFK then
        pcall(function()
            if VirtualUser then
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.new())
            end
        end)
    end
end)

-- Jump Request
UserInputService.JumpRequest:Connect(function()
    pcall(function()
        if RyuConfig.JumpSpam then
            local c = LocalPlayer.Character
            if c and c:FindFirstChild("HumanoidRootPart") then
                c.HumanoidRootPart.Velocity = Vector3.new(c.HumanoidRootPart.Velocity.X, RyuConfig.JumpPower, c.HumanoidRootPart.Velocity.Z)
            end
        elseif RyuConfig.JumpHigh then
            local c = LocalPlayer.Character
            if c and c:FindFirstChild("HumanoidRootPart") then
                c.HumanoidRootPart.Velocity = Vector3.new(c.HumanoidRootPart.Velocity.X, RyuConfig.JumpPower, c.HumanoidRootPart.Velocity.Z)
            end
        end
    end)
end)

local lockOnActive = false
local lockOnTarget = nil
UserInputService.InputBegan:Connect(function(i, gp)
    if not gp and i.KeyCode == RyuConfig.LockOnKey then
        lockOnActive = not lockOnActive
        lockOnTarget = nil
    end
    if not gp and i.KeyCode == RyuConfig.FlyKey then
        RyuConfig.Fly = not RyuConfig.Fly
    end
end)

-- Lock On Camera
RunService.RenderStepped:Connect(function()
    pcall(function()
        local cam = Workspace.CurrentCamera
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if RyuConfig.LockOn and lockOnActive and hrp and cam then
            if not lockOnTarget then
                local near = nil
                local d = math.huge
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        local mag = (p.Character:FindFirstChild("Head").Position - hrp.Position).Magnitude
                        if mag < d then d = mag; near = p.Character:FindFirstChild("Head") end
                    end
                end
                lockOnTarget = near
            end
            if lockOnTarget and lockOnTarget.Parent then
                cam.CFrame = CFrame.lookAt(cam.CFrame.Position, lockOnTarget.Position + Vector3.new(0, RyuConfig.LockOnY, 0))
            end
        end
    end)
end)

-- Main Loop
local aiStuckPos = Vector3.zero
local aiStuckTick = os.clock()
local aiTarget = nil
local m1Combo = 0
local m1Tick = os.clock()
local blockWait = false

RunService.Stepped:Connect(function()
    pcall(function()
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local cam = Workspace.CurrentCamera
        
        if not char or not hrp or not hum then return end
        
        -- INVISIBLE
        if RyuConfig.Invisible then
            for _, p in pairs(char:GetDescendants()) do
                if p:IsA("BasePart") and p.Name ~= "HumanoidRootPart" then p.Transparency = 1 end
                if p:IsA("Decal") then p.Transparency = 1 end
            end
        end
        
        -- FLY
        if RyuConfig.Fly then
            if not bv then
                bv = Instance.new("BodyVelocity", hrp)
                bv.MaxForce = Vector3.new(1e9,1e9,1e9)
            end
            hum.PlatformStand = true
            local dir = Vector3.zero
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then dir = dir + cam.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then dir = dir - cam.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then dir = dir - cam.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then dir = dir + cam.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then dir = dir - Vector3.new(0,1,0) end
            if dir.Magnitude > 0 then bv.Velocity = dir.Unit * RyuConfig.FlySpeed else bv.Velocity = Vector3.zero end
        else
            if bv then bv:Destroy(); bv = nil end
            hum.PlatformStand = false
        end
        
        -- AUTO BLOCK
        if RyuConfig.AutoBlock and not blockWait then
            local attacking = false
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    if (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude <= RyuConfig.BlockRange then
                        local eHum = p.Character:FindFirstChild("Humanoid")
                        if eHum and eHum:FindFirstChild("Animator") then
                            for _, t in pairs(eHum.Animator:GetPlayingAnimationTracks()) do
                                local n = t.Name:lower()
                                if n:find("attack") or n:find("m1") or n:find("punch") then
                                    attacking = true; break
                                end
                            end
                        end
                    end
                end
            end
            if attacking then
                blockWait = true
                if remotes.ItadoriActivated then remotes.ItadoriActivated:FireServer(false) end
                if remotes.BlockOn then remotes.BlockOn:FireServer() end
                task.delay(RyuConfig.BlockTime / 1000, function()
                    if remotes.BlockOff then remotes.BlockOff:FireServer() end
                    blockWait = false
                end)
            end
        end
        
        -- AUTO BLACK FLASH
        if RyuConfig.AutoBlackFlash then
            local ms = char:FindFirstChild("Moveset")
            if ms and ms:FindFirstChild("Divergent Fist") then
                if remotes.Divergent then remotes.Divergent:FireServer(ms["Divergent Fist"]) end
            end
            if VirtualInputManager then
                VirtualInputManager:SendMouseButtonEvent(0,0,0,true,game,0)
                task.wait(0.01)
                VirtualInputManager:SendMouseButtonEvent(0,0,0,false,game,0)
            end
        end
        
        -- AUTO ITEM
        if RyuConfig.AutoItem and RyuConfig.TargetItem ~= "None" then
            local items = Workspace:FindFirstChild("Items")
            if items then
                local i = items:FindFirstChild(RyuConfig.TargetItem)
                if i and i:IsA("Model") and i.PrimaryPart then
                    TJSTeleport(i.PrimaryPart.CFrame)
                elseif i and i:IsA("BasePart") then
                    TJSTeleport(i.CFrame)
                end
            end
        end
        
        -- TARGET FARM
        if RyuConfig.TargetFarm and RyuConfig.TargetPlayer ~= "None" then
            local t = Players:FindFirstChild(RyuConfig.TargetPlayer)
            if t and t.Character and t.Character:FindFirstChild("HumanoidRootPart") then
                TJSTeleport(t.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, RyuConfig.TargetDist))
            end
        end
        
        -- AI FARM
        if RyuConfig.AIFarm then
            if not aiTarget or not aiTarget.Parent or not aiTarget:FindFirstChild("HumanoidRootPart") or aiTarget:FindFirstChild("Humanoid").Health <= 0 then
                local d = math.huge
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChild("Humanoid").Health > 0 then
                        local mag = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if mag < d then d = mag; aiTarget = p.Character end
                    end
                end
                aiStuckPos = hrp.Position
                aiStuckTick = os.clock()
            end
            
            if aiTarget then
                local tHrp = aiTarget.HumanoidRootPart
                
                -- Stuck detection (2 mins, <20 studs diff)
                if (hrp.Position - aiStuckPos).Magnitude < 20 then
                    if os.clock() - aiStuckTick > 120 then
                        aiTarget = nil -- find new target next frame
                        TJSTeleport(hrp.CFrame * CFrame.new(0, 50, 0))
                    end
                else
                    aiStuckPos = hrp.Position
                    aiStuckTick = os.clock()
                end
                
                -- Aggressive facing
                hrp.CFrame = CFrame.lookAt(hrp.Position, Vector3.new(tHrp.Position.X, hrp.Position.Y, tHrp.Position.Z))
                
                -- Movement
                local dist = (tHrp.Position - hrp.Position).Magnitude
                if dist > 4 then
                    TJSTeleport(tHrp.CFrame * CFrame.new(0,0,2))
                end
                
                -- Combat
                if os.clock() - m1Tick > 0.4 then
                    m1Tick = os.clock()
                    if VirtualInputManager then
                        VirtualInputManager:SendMouseButtonEvent(0,0,0,true,game,0)
                        task.wait(0.05)
                        VirtualInputManager:SendMouseButtonEvent(0,0,0,false,game,0)
                    end
                    m1Combo = m1Combo + 1
                    
                    if m1Combo >= 4 then
                        m1Combo = 0
                        if RyuConfig.S1 then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.One, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.One, false, game) end
                        if RyuConfig.S2 then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Two, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Two, false, game) end
                        if RyuConfig.S3 then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Three, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Three, false, game) end
                        if RyuConfig.S4 then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Four, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Four, false, game) end
                    end
                end
            end
        end
        
        -- MONEY FARM
        if RyuConfig.MoneyFarm then
            local boxCFrame = CFrame.new(0, 50000, 0)
            if RyuConfig.MFRole == "Farmer" then
                if hrp.Position.Y < 49000 then TJSTeleport(boxCFrame * CFrame.new(0,5,0)) end
                RyuConfig.AIFarm = true 
            elseif RyuConfig.MFRole == "Victim" and RyuConfig.MFVictim ~= "" then
                local fPlr = nil
                for _, p in pairs(Players:GetPlayers()) do
                    if p.Name:lower():find(RyuConfig.MFVictim:lower()) or p.DisplayName:lower():find(RyuConfig.MFVictim:lower()) then fPlr = p; break end
                end
                if fPlr and fPlr.Character and fPlr.Character:FindFirstChild("HumanoidRootPart") then
                    local fHrp = fPlr.Character.HumanoidRootPart
                    if fHrp.Position.Y > 49000 then
                        TJSTeleport(fHrp.CFrame * CFrame.new(0, 0, -2))
                    end
                end
            end
        end
        
    end)
end)

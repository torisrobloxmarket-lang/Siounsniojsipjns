--// ==========================================
--// RYU HUB - JUJUTSU SHENANIGANS EDITION (TJS) v1.5
--// ULTIMATE MONOCHROME FIX - 100% STABLE
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
local VirtualInputManager = game:GetService("VirtualInputManager")
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera
local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

--// 1. GUI SECURITY & CLEANUP
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

--// 2. CONFIGURATION & SAVE SYSTEM
local CONFIG_FILE = "RyuHub_TJS_Config.json"

local RyuConfig = {
    -- Player
    SpeedHack = false, SpeedVal = 50,
    Fly = false, FlySpeed = 50, FlyKey = Enum.KeyCode.X,
    JumpHigh = false, JumpPower = 150, JumpSpam = false,
    Noclip = false, Invisible = false, FakeName = "",
    
    -- Auto
    AutoBlock = false, BlockRange = 15, BlockTime = 500,
    AutoBlackFlash = false, AutoDodge = false, DodgeRange = 20,
    AutoItem = false, TargetItem = "None",
    
    -- Abilities
    LockOn = false, LockOnKey = Enum.KeyCode.C, LockOnY = 0,
    Knockback = false, KnockbackVal = 50, DomainBypass = false, DashSpam = false,
    
    -- AI Farm
    AIFarm = false, AIRange = 15, AutoUlt = false, HumanMode = false,
    S1 = false, S1R = 10, S2 = false, S2R = 10, S3 = false, S3R = 10, S4 = false, S4R = 10,
    
    -- Target & Money Farm
    TargetPlayer = "None", TargetFarm = false, TargetDist = 3,
    MoneyFarm = false, MFRole = "Farmer", MFVictim = "",
    
    -- Config
    AutoRejoin = false, MinPlayers = 3, HighPop = true,
    TargetJoinUser = "", AntiAFK = false,
    
    GuiColor = Color3.fromRGB(255, 255, 255) -- PURE WHITE
}

local function SaveConfig()
    pcall(function() if writefile then writefile(CONFIG_FILE, HttpService:JSONEncode(RyuConfig)) end end)
end

pcall(function()
    if readfile and isfile and isfile(CONFIG_FILE) then
        local data = HttpService:JSONDecode(readfile(CONFIG_FILE))
        for k, v in pairs(data) do RyuConfig[k] = v end
    end
end)

--// 3. TJS REMOTES (KNIT)
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
        if remotes.Teleport then remotes.Teleport:FireServer(1786781036.3533041) end
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframeObj
    end)
end

--// 4. THEME & UI FRAMEWORK (MONOCHROME)
local Theme = {
    Background = Color3.fromRGB(12, 12, 14), Sidebar = Color3.fromRGB(18, 18, 20),
    SectionBG = Color3.fromRGB(24, 24, 26), Text = Color3.fromRGB(250, 250, 250),
    SubText = Color3.fromRGB(130, 130, 135), CloudLight = Color3.fromRGB(255, 255, 255),
    CloudDark = Color3.fromRGB(100, 100, 100), Accent = RyuConfig.GuiColor,
    ToggleOff = Color3.fromRGB(35, 35, 38), ToggleOn = RyuConfig.GuiColor,
    Stroke = Color3.fromRGB(60, 60, 60), Warning = Color3.fromRGB(200, 200, 200)
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

-- FIXED TOP LEFT TOGGLE BUTTON (Under Roblox Logo)
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
ToggleBtn.Position = UDim2.new(0, 15, 0, 50) 
ToggleBtn.BackgroundColor3 = Theme.Sidebar
ToggleBtn.Text = ""
ToggleBtn.Parent = RyuHub
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
Instance.new("UIStroke", ToggleBtn).Color = Theme.Accent
Instance.new("UIStroke", ToggleBtn).Thickness = 2
Instance.new("UIStroke", ToggleBtn).Transparency = 0.5

local Katana = Instance.new("Frame", ToggleBtn)
Katana.Size = UDim2.new(1, 0, 1, 0); Katana.BackgroundTransparency = 1; Katana.Rotation = 45
local Blade = Instance.new("Frame", Katana)
Blade.Size = UDim2.new(0, 2, 0, 24); Blade.Position = UDim2.new(0.5, -1, 0.5, -18); Blade.BackgroundColor3 = Theme.CloudLight; Blade.BorderSizePixel = 0
local Guard = Instance.new("Frame", Katana)
Guard.Size = UDim2.new(0, 12, 0, 2); Guard.Position = UDim2.new(0.5, -6, 0.5, 6); Guard.BackgroundColor3 = Theme.CloudDark; Guard.BorderSizePixel = 0
local Handle = Instance.new("Frame", Katana)
Handle.Size = UDim2.new(0, 4, 0, 10); Handle.Position = UDim2.new(0.5, -2, 0.5, 8); Handle.BackgroundColor3 = Color3.fromRGB(80, 80, 80); Handle.BorderSizePixel = 0
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

local mainStroke = Instance.new("UIStroke", MainFrame)
mainStroke.Color = Theme.Stroke; mainStroke.Transparency = 0.2; mainStroke.Thickness = 1.5

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
SubTitle.Text = "Jujutsu Shenanigans"; SubTitle.TextColor3 = Theme.SubText; SubTitle.Font = Enum.Font.Gotham; SubTitle.TextSize = 11; SubTitle.TextXAlignment = Enum.TextXAlignment.Left

-- RED CLOSE BUTTON -> NOW WHITE
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
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then 
        mDragging = false 
    end
end)

local Line = Instance.new("Frame", ContentWrapper)
Line.Size = UDim2.new(1, -40, 0, 1); Line.Position = UDim2.new(0, 20, 0, 65); Line.BackgroundColor3 = Theme.Stroke; Line.BorderSizePixel = 0

-- UI Layout Framework
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
--// 5. TABS & FEATURES SETUP
--// ==========================================

-- TAB: COMBAT
local TabCombat = CreateMainTab("Combat")

local SubPlayer = CreateSubTab(TabCombat, "Player")
local SecPlayer = CreateSection(SubPlayer, "Movement Mods")
CreateToggle(SecPlayer, "Speed Hack (WalkSpeed)", "Enhances your WalkSpeed", RyuConfig.SpeedHack, function(v) RyuConfig.SpeedHack = v end)
CreateSlider(SecPlayer, "Speed Value", 16, 150, RyuConfig.SpeedVal, function(v) RyuConfig.SpeedVal = v end)
CreateToggle(SecPlayer, "Fly (Frozen Body)", "Fly locally without moving limbs", RyuConfig.Fly, function(v) RyuConfig.Fly = v end)
CreateSlider(SecPlayer, "Fly Speed", 10, 200, RyuConfig.FlySpeed, function(v) RyuConfig.FlySpeed = v end)
CreateKeybind(SecPlayer, "Fly Keybind", RyuConfig.FlyKey, function(v) RyuConfig.FlyKey = v end)

CreateToggle(SecPlayer, "High Jump", RyuConfig.JumpHigh, function(v) RyuConfig.JumpHigh = v end)
CreateSlider(SecPlayer, "Jump Power", 50, 300, RyuConfig.JumpPower, function(v) RyuConfig.JumpPower = v end)
CreateToggle(SecPlayer, "Infinite Jump Spam", RyuConfig.JumpSpam, function(v) RyuConfig.JumpSpam = v end)
CreateToggle(SecPlayer, "Noclip", "Walk through objects", RyuConfig.Noclip, function(v) RyuConfig.Noclip = v end)
CreateToggle(SecPlayer, "Invisible (FE Server-Sided)", "Destroys LowerTorso to turn invisible", RyuConfig.Invisible, function(v) RyuConfig.Invisible = v end)

local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAutoBlock = CreateSection(SubAuto, "Defensive")
CreateToggle(SecAutoBlock, "Auto Block", "Reacts to enemy hit animations", RyuConfig.AutoBlock, function(v) RyuConfig.AutoBlock = v end)
CreateSlider(SecAutoBlock, "Block React Range", 5, 50, RyuConfig.BlockRange, function(v) RyuConfig.BlockRange = v end)
CreateSlider(SecAutoBlock, "Block Hold Duration (ms)", 100, 1500, RyuConfig.BlockTime, function(v) RyuConfig.BlockTime = v end)
CreateToggle(SecAutoBlock, "Auto Dodge (TP Back)", "Dodge backwards on damage", RyuConfig.AutoDodge, function(v) RyuConfig.AutoDodge = v end)
CreateSlider(SecAutoBlock, "Dodge Distance", 5, 50, RyuConfig.DodgeRange, function(v) RyuConfig.DodgeRange = v end)

local SecAutoHit = CreateSection(SubAuto, "Offensive")
CreateToggle(SecAutoHit, "Auto Black Flash (Yuji)", "Waits for Divergent Fist/3rd punch", RyuConfig.AutoBlackFlash, function(v) RyuConfig.AutoBlackFlash = v end)
CreateLabel(SecAutoHit, "Auto Combos: Join discord.gg/ryuhub and send clips of your combos!")

local SecAutoItem = CreateSection(SubAuto, "Auto Item")
CreateToggle(SecAutoItem, "Enable Auto Item", RyuConfig.AutoItem, function(v) RyuConfig.AutoItem = v end)
local ItemDropdown = CreateDropdown(SecAutoItem, "Target Item", {"None"}, "TargetItem")

task.spawn(function()
    while task.wait(5) do
        pcall(function()
            local items = {"None"}
            local itemFolder = Workspace:FindFirstChild("Items") or Workspace
            for _,v in pairs(itemFolder:GetChildren()) do
                if v:IsA("Tool") or v.Name:find("Item") or v.Name:find("Object") then
                    if not table.find(items, v.Name) then table.insert(items, v.Name) end
                end
            end
            ItemDropdown:Refresh(items)
        end)
    end
end)

local SubAbil = CreateSubTab(TabCombat, "Abilities")
local SecAbil = CreateSection(SubAbil, "Combat Enhancements")

local lockBtnMobile
CreateToggle(SecAbil, "Lock On (Stationary Camera)", "Look at target without teleporting", RyuConfig.LockOn, function(v) 
    RyuConfig.LockOn = v 
    if isMobile and lockBtnMobile then lockBtnMobile.Visible = v end
end)
CreateKeybind(SecAbil, "Lock On Keybind", RyuConfig.LockOnKey, function(v) RyuConfig.LockOnKey = v end)
CreateSlider(SecAbil, "Lock On Y-Offset", -10, 10, RyuConfig.LockOnY, function(v) RyuConfig.LockOnY = v end)

if isMobile then
    lockBtnMobile = Instance.new("TextButton", RyuHub)
    lockBtnMobile.Size = UDim2.new(0, 60, 0, 60)
    lockBtnMobile.Position = UDim2.new(1, -80, 0.5, 0)
    lockBtnMobile.BackgroundColor3 = Theme.Sidebar
    lockBtnMobile.Text = "LOCK"
    lockBtnMobile.TextColor3 = Theme.Accent
    lockBtnMobile.Font = Enum.Font.GothamBold
    lockBtnMobile.Visible = RyuConfig.LockOn
    Instance.new("UICorner", lockBtnMobile).CornerRadius = UDim.new(1, 0)
    lockBtnMobile.MouseButton1Click:Connect(function()
        _G.RyuLockOnActive = not _G.RyuLockOnActive
        lockBtnMobile.BackgroundColor3 = _G.RyuLockOnActive and Theme.Accent or Theme.Sidebar
        lockBtnMobile.TextColor3 = _G.RyuLockOnActive and Theme.Background or Theme.Accent
    end)
end

CreateToggle(SecAbil, "Knockback M1s", "Velocity push", RyuConfig.Knockback, function(v) RyuConfig.Knockback = v end)
CreateSlider(SecAbil, "Knockback Force", 10, 300, RyuConfig.KnockbackVal, function(v) RyuConfig.KnockbackVal = v end)
CreateToggle(SecAbil, "Domain Eraser (Local)", "Bypass domains", RyuConfig.DomainBypass, function(v) RyuConfig.DomainBypass = v end)
CreateToggle(SecAbil, "No Cooldown Dash", "Spam chase remote", RyuConfig.DashSpam, function(v) RyuConfig.DashSpam = v end)
CreateButton(SecAbil, "Teleport All to Me (Physical Bring)", Theme.ToggleOff, function()
    pcall(function()
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local tHrp = p.Character.HumanoidRootPart
                tHrp.Velocity = (hrp.Position - tHrp.Position).Unit * 300
            end
        end
    end)
end)

-- TAB: FARM
local TabFarm = CreateMainTab("Farm")

local SubAIFarm = CreateSubTab(TabFarm, "AI Auto Farm")
local SecAIFarm = CreateSection(SubAIFarm, "Auto Combat")
CreateToggle(SecAIFarm, "Enable AI Farm", "Uses MoveTo and combats naturally", RyuConfig.AIFarm, function(v) RyuConfig.AIFarm = v end)
CreateSlider(SecAIFarm, "Chase Range", 10, 500, RyuConfig.AIRange, function(v) RyuConfig.AIRange = v end)
CreateToggle(SecAIFarm, "Auto Ultimate (G)", "Pops awakening", RyuConfig.AutoUlt, function(v) RyuConfig.AutoUlt = v end)
CreateToggle(SecAIFarm, "Human Mode", "Strafes & Jumps naturally", RyuConfig.HumanMode, function(v) RyuConfig.HumanMode = v end)

local SecAISkills = CreateSection(SubAIFarm, "Choose Skills")
local function makeSkillBtn(name, configKey, rangeKey)
    local btn = CreateButton(SecAISkills, name, RyuConfig[configKey] and Theme.Accent or Theme.ToggleOff, function() end)
    btn.MouseButton1Click:Connect(function()
        RyuConfig[configKey] = not RyuConfig[configKey]
        btn.BackgroundColor3 = RyuConfig[configKey] and Theme.Accent or Theme.ToggleOff
        btn.TextColor3 = RyuConfig[configKey] and Theme.Background or Theme.Text
    end)
    CreateSlider(SecAISkills, name .. " Range", 5, 100, RyuConfig[rangeKey], function(v) RyuConfig[rangeKey] = v end)
end
makeSkillBtn("Skill 1", "S1", "S1R")
makeSkillBtn("Skill 2", "S2", "S2R")
makeSkillBtn("Skill 3", "S3", "S3R")
makeSkillBtn("Skill 4", "S4", "S4R")

local SubTarget = CreateSubTab(TabFarm, "Target")
local SecTarget = CreateSection(SubTarget, "Specific Target Follow")
local PlrDrop = CreateDropdown(SecTarget, "Target Player", {"None"}, "TargetPlayer")
CreateToggle(SecTarget, "Enable Target Farm", "TPs to player", RyuConfig.TargetFarm, function(v) RyuConfig.TargetFarm = v end)
CreateSlider(SecTarget, "Distance Behind", 1, 15, RyuConfig.TargetDist, function(v) RyuConfig.TargetDist = v end)

task.spawn(function()
    while task.wait(3) do
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
CreateToggle(SecMFarm, "Enable Money Farm", "Y=500 Box", RyuConfig.MoneyFarm, function(v) RyuConfig.MoneyFarm = v end)
local RoleBtn
RoleBtn = CreateButton(SecMFarm, "Role: " .. RyuConfig.MFRole, Theme.ToggleOff, function()
    if RyuConfig.MFRole == "Farmer" then RyuConfig.MFRole = "Victim" else RyuConfig.MFRole = "Farmer" end
    RoleBtn.Text = "Role: " .. RyuConfig.MFRole
end)
CreateInput(SecMFarm, "Victim: Enter Farmer Name...", function(v) RyuConfig.MFVictim = v end)

local SubFarmConfig = CreateSubTab(TabFarm, "Config")
local SecServerConfig = CreateSection(SubFarmConfig, "Server & Connections")
CreateToggle(SecServerConfig, "Auto Rejoin Low Pop", "Rejoins if server dies", RyuConfig.AutoRejoin, function(v) RyuConfig.AutoRejoin = v end)
CreateSlider(SecServerConfig, "Rejoin if players < X", 1, 10, RyuConfig.MinPlayers, function(v) RyuConfig.MinPlayers = v end)
CreateToggle(SecServerConfig, "Find 80% Full Lobbys", "Joins active lobbies", RyuConfig.HighPop, function(v) RyuConfig.HighPop = v end)
CreateInput(SecServerConfig, "Alt Joiner: Enter Main Username", function(v) RyuConfig.TargetJoinUser = v end)

-- TAB: SETTINGS
local TabSettings = CreateMainTab("Settings")
local SubSettings = CreateSubTab(TabSettings, "Settings")

local SecCosmetics = CreateSection(SubSettings, "Cosmetics")
CreateInput(SecCosmetics, "Fake Name (Visual)", function(v) RyuConfig.FakeName = v end)

local SecCfg = CreateSection(SubSettings, "System & Protection")
CreateToggle(SecCfg, "Anti-AFK Protection", "Prevents Roblox Kick", RyuConfig.AntiAFK, function(v) RyuConfig.AntiAFK = v end)
CreateButton(SecCfg, "Save Settings", Theme.SectionBG, function() SaveConfig() end)
CreateButton(SecCfg, "Reset Settings", Theme.Warning, function() 
    pcall(function()
        if isfile(CONFIG_FILE) then delfile(CONFIG_FILE) end
        RyuHub:Destroy()
    end)
end)

-- init
pcall(function() 
    if Tabs[1] and Tabs[1].Toggle then Tabs[1].Toggle() end
    if Tabs[1].SubTabs[1] and Tabs[1].SubTabs[1].Open then Tabs[1].SubTabs[1].Open() end 
end)

--// 6. TJS RUNTIME EXPLOITATION ENGINE
local lastHealth = 100
local flashFired = false
local boxSpawned = false
local boxPart = nil
local bv = nil
_G.RyuLockOnActive = false

-- Fake Name Loop
RunService.RenderStepped:Connect(function()
    pcall(function()
        local char = LocalPlayer.Character
        if char then
            local head = char:FindFirstChild("Head")
            if head then
                if RyuConfig.FakeName ~= "" then
                    for _, v in pairs(head:GetChildren()) do
                        if v:IsA("BillboardGui") and v.Name ~= "RyuFakeName" then v.Enabled = false end
                    end
                    local bg = head:FindFirstChild("RyuFakeName")
                    if not bg then
                        bg = Instance.new("BillboardGui", head)
                        bg.Name = "RyuFakeName"
                        bg.Size = UDim2.new(0, 200, 0, 50)
                        bg.StudsOffset = Vector3.new(0, 2.5, 0)
                        bg.AlwaysOnTop = true
                        local tl = Instance.new("TextLabel", bg)
                        tl.Size = UDim2.new(1, 0, 1, 0)
                        tl.BackgroundTransparency = 1
                        tl.Text = RyuConfig.FakeName
                        tl.TextColor3 = Color3.new(1, 1, 1)
                        tl.TextStrokeTransparency = 0
                        tl.Font = Enum.Font.GothamBold
                        tl.TextSize = 14
                    else
                        bg:FindFirstChildOfClass("TextLabel").Text = RyuConfig.FakeName
                    end
                else
                    for _, v in pairs(head:GetChildren()) do
                        if v:IsA("BillboardGui") and v.Name ~= "RyuFakeName" then v.Enabled = true end
                    end
                    if head:FindFirstChild("RyuFakeName") then head.RyuFakeName:Destroy() end
                end
            end
        end
    end)
end)

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

-- Anti AFK Engine
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

-- Jump Request Engine
UserInputService.JumpRequest:Connect(function()
    pcall(function()
        local c = LocalPlayer.Character
        local hum = c and c:FindFirstChildOfClass("Humanoid")
        if RyuConfig.JumpSpam and hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        elseif RyuConfig.JumpHigh and c and c:FindFirstChild("HumanoidRootPart") then
            c.HumanoidRootPart.Velocity = Vector3.new(c.HumanoidRootPart.Velocity.X, RyuConfig.JumpPower, c.HumanoidRootPart.Velocity.Z)
        end
    end)
end)

local lockOnTarget = nil
UserInputService.InputBegan:Connect(function(i, gp)
    if not gp and i.KeyCode == RyuConfig.LockOnKey then
        _G.RyuLockOnActive = not _G.RyuLockOnActive
        lockOnTarget = nil
    end
    if not gp and i.KeyCode == RyuConfig.FlyKey then
        RyuConfig.Fly = not RyuConfig.Fly
    end
end)

-- Lock On Camera (Stationary, Look at Head)
RunService.RenderStepped:Connect(function()
    pcall(function()
        local cam = Workspace.CurrentCamera
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        
        if RyuConfig.LockOn and _G.RyuLockOnActive and hrp and cam then
            if not lockOnTarget or not lockOnTarget.Parent or lockOnTarget.Parent:FindFirstChild("Humanoid").Health <= 0 then
                local near = nil
                local d = math.huge
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        local ehum = p.Character:FindFirstChild("Humanoid")
                        if ehum and ehum.Health > 0 then
                            local head = p.Character:FindFirstChild("Head") or p.Character.HumanoidRootPart
                            local mag = (head.Position - hrp.Position).Magnitude
                            if mag < d then 
                                d = mag 
                                near = head 
                            end
                        end
                    end
                end
                lockOnTarget = near
            end
            if lockOnTarget and lockOnTarget.Parent then
                local tPos = lockOnTarget.Position + Vector3.new(0, RyuConfig.LockOnY, 0)
                cam.CFrame = CFrame.lookAt(cam.CFrame.Position, tPos)
            end
        else
            lockOnTarget = nil
        end
    end)
end)

-- Main Background Loop (Physics & Combat Logic)
local blockWait = false

RunService.Stepped:Connect(function()
    pcall(function()
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local cam = Workspace.CurrentCamera
        
        if not char or not hrp or not hum then return end
        
        -- SPEED HACK (WalkSpeed)
        if RyuConfig.SpeedHack then
            hum.WalkSpeed = RyuConfig.SpeedVal
        else
            if hum.WalkSpeed > 30 then hum.WalkSpeed = 16 end
        end
        
        -- FLY (Frozen Body)
        if RyuConfig.Fly then
            if not bv then
                bv = Instance.new("BodyVelocity", hrp)
                bv.MaxForce = Vector3.new(1e9,1e9,1e9)
            end
            hum.PlatformStand = true
            for _, t in pairs(hum:GetPlayingAnimationTracks()) do t:Stop() end
            
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
            if hum.PlatformStand then hum.PlatformStand = false end
        end
        
        -- NOCLIP
        if RyuConfig.Noclip then
            for _, p in pairs(char:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
        
        -- INVISIBLE (FE Server-Sided via Desync/LowerTorso)
        if RyuConfig.Invisible then
            local lower = char:FindFirstChild("LowerTorso")
            if lower then lower:Destroy() end -- breaks joints for R15, makes you invisible server side
        end
        
        -- KNOCKBACK
        if RyuConfig.Knockback then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local tHrp = p.Character.HumanoidRootPart
                    if (tHrp.Position - hrp.Position).Magnitude < 5 then
                        tHrp.Velocity = hrp.CFrame.LookVector * RyuConfig.KnockbackVal
                    end
                end
            end
        end
        
        -- AUTO BLACK FLASH (Waits for Divergent/3rd M1)
        if RyuConfig.AutoBlackFlash then
            local ms = char:FindFirstChild("Moveset")
            local hasDivergent = ms and ms:FindFirstChild("Divergent Fist")
            local isThirdPunch = false
            for _, t in pairs(hum:GetPlayingAnimationTracks()) do
                if t.Name:lower():find("3") and t.Name:lower():find("punch") then isThirdPunch = true break end
            end
            
            if (hasDivergent or isThirdPunch) and not flashFired then
                flashFired = true
                if remotes.Divergent and hasDivergent then remotes.Divergent:FireServer(ms["Divergent Fist"]) end
                task.delay(0.5, function()
                    if VirtualInputManager then
                        VirtualInputManager:SendMouseButtonEvent(0,0,0,true,game,0)
                        task.wait(0.05)
                        VirtualInputManager:SendMouseButtonEvent(0,0,0,false,game,0)
                    end
                    task.wait(1)
                    flashFired = false
                end)
            end
        end
        
        -- AUTO DODGE
        if RyuConfig.AutoDodge then
            if hum.Health < lastHealth then
                hrp.CFrame = hrp.CFrame * CFrame.new(0, 0, RyuConfig.DodgeRange)
            end
        end
        lastHealth = hum.Health
        
        -- AUTO BLOCK (ITADORI ACTIVATED REMOTE)
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
        
        -- NO COOLDOWN DASH
        if RyuConfig.DashSpam and remotes.Chase then
            if hum.MoveDirection.Magnitude > 0 then
                remotes.Chase:FireServer(false)
            end
        end
        
        -- AUTO ITEM
        if RyuConfig.AutoItem and RyuConfig.TargetItem ~= "None" then
            local itemFolder = Workspace:FindFirstChild("Items") or Workspace
            local item = itemFolder:FindFirstChild(RyuConfig.TargetItem)
            if item then
                if item:IsA("Model") and item.PrimaryPart then TJSTeleport(item.PrimaryPart.CFrame)
                elseif item:IsA("BasePart") then TJSTeleport(item.CFrame) end
            end
        end
        
        -- TARGET FARM
        if RyuConfig.TargetFarm and RyuConfig.TargetPlayer ~= "None" then
            local t = Players:FindFirstChild(RyuConfig.TargetPlayer)
            if t and t.Character and t.Character:FindFirstChild("HumanoidRootPart") then
                TJSTeleport(t.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, RyuConfig.TargetDist))
            end
        end
        
        -- MONEY FARM
        if RyuConfig.MoneyFarm then
            local boxCFrame = CFrame.new(0, 500, 0)
            if RyuConfig.MFRole == "Farmer" then
                if hrp.Position.Y < 490 then TJSTeleport(boxCFrame * CFrame.new(0,5,0)) end
                RyuConfig.AIFarm = true 
            elseif RyuConfig.MFRole == "Victim" and RyuConfig.MFVictim ~= "" then
                local fPlr = nil
                for _, p in pairs(Players:GetPlayers()) do
                    if p.Name:lower():find(RyuConfig.MFVictim:lower()) or p.DisplayName:lower():find(RyuConfig.MFVictim:lower()) then fPlr = p; break end
                end
                if fPlr and fPlr.Character and fPlr.Character:FindFirstChild("HumanoidRootPart") then
                    local fHrp = fPlr.Character.HumanoidRootPart
                    if fHrp.Position.Y > 490 then
                        TJSTeleport(fHrp.CFrame * CFrame.new(0, 0, -2))
                    end
                end
            end
        else
            if boxSpawned and boxPart then boxPart:Destroy(); boxSpawned = false end
        end
        
    end)
end)

--// AI FARMING ENGINE (DEDICATED LOOP TO PREVENT CRASHES)
local m1Combo = 0
local m1Tick = os.clock()

task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if not RyuConfig.AIFarm then return end
            
            local char = LocalPlayer.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if not char or not hrp or not hum or hum.Health <= 0 then return end
            
            local target = nil
            local minD = RyuConfig.AIRange
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local ehum = p.Character:FindFirstChild("Humanoid")
                    if ehum and ehum.Health > 0 then
                        local d = (p.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if d < minD then 
                            minD = d
                            target = p.Character 
                        end
                    end
                end
            end
            
            if target then
                local tHrp = target.HumanoidRootPart
                
                -- Aggressive facing (Shiftlock style)
                hrp.CFrame = CFrame.lookAt(hrp.Position, Vector3.new(tHrp.Position.X, hrp.Position.Y, tHrp.Position.Z))
                
                if RyuConfig.HumanMode then
                    hum:MoveTo(tHrp.Position)
                    if math.random(1, 100) > 90 then hum.Jump = true end
                else
                    hum:MoveTo(tHrp.Position)
                end
                
                if minD < 6 then
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
                            if VirtualInputManager then
                                if RyuConfig.S1 and minD <= RyuConfig.S1R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.One, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.One, false, game) end
                                if RyuConfig.S2 and minD <= RyuConfig.S2R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Two, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Two, false, game) end
                                if RyuConfig.S3 and minD <= RyuConfig.S3R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Three, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Three, false, game) end
                                if RyuConfig.S4 and minD <= RyuConfig.S4R then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Four, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Four, false, game) end
                                if RyuConfig.AutoUlt then VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.G, false, game) task.wait(0.05) VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.G, false, game) end
                            end
                        end
                    end
                end
            end
        end)
    end
end)

-- auto rejoin logic
task.spawn(function()
    while task.wait(5) do
        if RyuConfig.AutoRejoin then
            if #Players:GetPlayers() <= RyuConfig.MinPlayers then
                pcall(function()
                    local url = "https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Desc&limit=100"
                    local req = game:HttpGet(url)
                    local data = HttpService:JSONDecode(req)
                    for _, s in pairs(data.data) do
                        if s.playing < s.maxPlayers and s.id ~= game.JobId then
                            local ratio = s.playing / s.maxPlayers
                            if RyuConfig.HighPop and ratio >= 0.7 and ratio <= 0.9 then
                                TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id, LocalPlayer)
                                break
                            elseif not RyuConfig.HighPop and s.playing > 2 then
                                TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id, LocalPlayer)
                                break
                            end
                        end
                    end
                end)
            end
        end
    end
end)

--// ==========================================
--// RYU HUB - UI OVERLAY (TJS EDITION)
--// 100% MONOCHROME CLEAN TEMPLATE
--// FIXED: AddHoverEffect undefined, Theme.Warning missing,
--//        namecall hook guard, drag/click race, invis rewrite
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

--// PLATZHALTER-FUNKTION
local function Ryuhub()
    -- Für zukünftige Features
end

--// ANTI-ANNOYING MESSAGE & SOUND HIDER (CLIMB ERROR/CD)
task.spawn(function()
    local pg = LocalPlayer:WaitForChild("PlayerGui", 10)
    if pg then
        pg.DescendantAdded:Connect(function(descendant)
            if descendant:IsA("TextLabel") or descendant:IsA("TextButton") then
                task.delay(0.01, function()
                    pcall(function()
                        if descendant and descendant.Parent and descendant.Text then
                            local txt = string.lower(descendant.Text)
                            if txt:match("cd") or txt:match("cooldown") or txt:match("climb") or txt:match("error") then
                                descendant.Visible = false
                                descendant.TextTransparency = 1
                            end
                        end
                    end)
                end)
            elseif descendant:IsA("Sound") then
                task.delay(0.01, function()
                    pcall(function()
                        if descendant then
                            local sndName = string.lower(descendant.Name)
                            if sndName:match("error") or sndName:match("climb") then
                                descendant:Stop()
                                descendant.Volume = 0
                            end
                        end
                    end)
                end)
            end
        end)
    end
end)

--// QUEST PING TRACKER (__namecall Hook) — FIXED: guard was checking function as value
local lastQuestPing = tick()
pcall(function()
    local oldNamecall
    oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
        if typeof(getnamecallmethod) ~= "function" then
            return oldNamecall(self, ...)
        end

        local method = getnamecallmethod()
        if method == "InvokeServer"
            and typeof(self) == "Instance"
            and self.Name == "Quest"
        then
            local args = {...}
            if args[1] == "getNPCQuestLocations"
                or (type(args[1]) == "table" and args[1][1] == "getNPCQuestLocations")
            then
                lastQuestPing = tick()
            end
        end

        return oldNamecall(self, ...)
    end)
end)

--// GUI CLEANUP
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end end)
for _, v in pairs(guiParent:GetChildren()) do if v.Name == "RyuHubPremium" then v:Destroy() end end

--// DYNAMISCHER WORKSPACE SCANNER FÜR INSELN, NPCs & WAFFEN
local function GetDynamicLists()
    local islands = {}
    local islandsFolder = Workspace:FindFirstChild("Islands")
    if islandsFolder then
        for _, v in pairs(islandsFolder:GetChildren()) do
            table.insert(islands, v.Name)
        end
    else
        islands = {
            "???? Shrine", "A rock", "Coco Island", "Colosseum", "Colosseum of Arc",
            "Desert Kingdom", "Dokkan Island", "Fishman Cave", "Fishman Island",
            "Foro Island", "Impel Base", "Marine Base G-1", "Marine Fort F-1",
            "Mirror World", "Mysterious Cliff", "Mysterious Reef", "Orange Town",
            "Restaurant Baratie", "Reverse Mountain", "Roca Island", "Rose Kingdom",
            "Rovo Island", "Sakura Stronghold", "Sandora", "Sashi Island",
            "Sett's Arena", "Shark Park", "Shell's Town", "Sphinx Island",
            "Spirit Island", "Thriller Bark", "Town of Beginnings",
            "Turtleback Cave", "Umi Island", "Whole Cake Island"
        }
    end
    table.sort(islands)
    return islands
end

local function GetNPCNames()
    local names = {}
    local dict = {}
    if Workspace:FindFirstChild("NPCs") then
        for _, v in pairs(Workspace.NPCs:GetChildren()) do
            if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and not dict[v.Name] then
                dict[v.Name] = true
                table.insert(names, v.Name)
            end
        end
    end
    table.sort(names)
    return names
end

local function GetWeapons()
    local weapons = {}
    local dict = {}
    local function add(t)
        if t and t:IsA("Tool") and not dict[t.Name] then
            dict[t.Name] = true
            table.insert(weapons, t.Name)
        end
    end
    if LocalPlayer.Character then
        for _, v in pairs(LocalPlayer.Character:GetChildren()) do add(v) end
    end
    local bp = LocalPlayer:FindFirstChild("Backpack")
    if bp then
        for _, v in pairs(bp:GetChildren()) do add(v) end
    end
    if #weapons == 0 then return {"Melee"} end
    table.sort(weapons)
    return weapons
end

local InitIslands = GetDynamicLists()

--// RYU CONFIGURATION (GPO)
local RyuConfig = {
    TargetIsland = InitIslands[1] or "Town of Beginnings",
    IslandSpeed = 65,
    TweenWallDistance = 1,
    GuiColor = Color3.fromRGB(255, 255, 255),

    AutoFarm = false,
    FarmMode = "Solo",
    FarmTweenType = "FarmTween (Direct)",
    TargetNPC = "",
    TargetMob = "",
    FarmHoverHeight = 6.5,
    FarmHitDelay = 0.45,
    FarmComboDelay = 1.0,
    UseGun = false,
    FarmStyle = "Melee",

    AutoBuso = false,
    AutoGeppo = false,

    NoclipDash = false,

    EnableAutoStats = false,
    AutoStats = {
        Strength = 0,
        Stamina = 0,
        Defense = 0,
        SwordMastery = 0,
        GunMastery = 0
    },

    FakeName = ""
}

--// PREMIUM MONOCHROME THEME — FIXED: Added Warning color
local Theme = {
    Background = Color3.fromRGB(15, 15, 15),
    Sidebar = Color3.fromRGB(22, 22, 22),
    SectionBG = Color3.fromRGB(30, 30, 30),
    Text = Color3.fromRGB(255, 255, 255),
    SubText = Color3.fromRGB(150, 150, 150),
    Accent = Color3.fromRGB(255, 255, 255),
    ToggleOff = Color3.fromRGB(45, 45, 45),
    ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(60, 60, 60),
    Warning = Color3.fromRGB(220, 60, 60)
}

local isMobile = camera.ViewportSize.X < 850
local MainSize = UDim2.new(0, math.min(750, camera.ViewportSize.X - 40), 0, math.min(480, camera.ViewportSize.Y - 40))
local SidebarWidth = 160

local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubUI"
RyuHub.ResetOnSpawn = false
RyuHub.IgnoreGuiInset = true
RyuHub.Parent = guiParent

--// FIXED: AddHoverEffect defined before first use (was called on CloseBtn but never defined)
local function AddHoverEffect(element, normalColor, hoverColor)
    element.MouseEnter:Connect(function()
        pcall(function()
            TweenService:Create(element, TweenInfo.new(0.15), {BackgroundColor3 = hoverColor}):Play()
        end)
    end)
    element.MouseLeave:Connect(function()
        pcall(function()
            TweenService:Create(element, TweenInfo.new(0.15), {BackgroundColor3 = normalColor}):Play()
        end)
    end)
end

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

--// FIXED: Drag vs click race — isDraggingBtn resets after one frame via task.defer
local tDragStart, tStartPos, isDraggingBtn = nil, nil, false

ToggleBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch
    then
        isDraggingBtn = false
        tDragStart = input.Position
        tStartPos = ToggleBtn.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if tDragStart and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then
        local delta = input.Position - tDragStart
        if delta.Magnitude > 5 then
            isDraggingBtn = true
            ToggleBtn.Position = UDim2.new(
                tStartPos.X.Scale, tStartPos.X.Offset + delta.X,
                tStartPos.Y.Scale, tStartPos.Y.Offset + delta.Y
            )
        end
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch
    then
        tDragStart = nil
        task.defer(function() isDraggingBtn = false end)
    end
end)

ToggleBtn.MouseButton1Click:Connect(function()
    if isDraggingBtn then return end

    if MainFrame and MainFrame.Visible then
        pcall(function()
            TweenService:Create(MainFrame,
                TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
                {Size = UDim2.new(0,0,0,0), Position = UDim2.new(0.5,0,0.5,0)}
            ):Play()
        end)
        task.delay(0.3, function() if MainFrame then MainFrame.Visible = false end end)
    else
        if MainFrame then MainFrame.Visible = true end
        pcall(function()
            TweenService:Create(MainFrame,
                TweenInfo.new(0.35, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
                {Size = MainSize, Position = UDim2.new(0.5, -MainSize.X.Offset/2, 0.5, -MainSize.Y.Offset/2)}
            ):Play()
        end)
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

local TitleGradient = Instance.new("UIGradient", Title)
TitleGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(180, 180, 185)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 255, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(180, 180, 185))
}
TitleGradient.Offset = Vector2.new(-1, 0)

task.spawn(function()
    local tweenInfo = TweenInfo.new(2.0, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    pcall(function() TweenService:Create(TitleGradient, tweenInfo, {Offset = Vector2.new(1, 0)}):Play() end)
end)

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
AddHoverEffect(CloseBtn, Theme.SectionBG, Theme.Warning)

CloseBtn.MouseButton1Click:Connect(function()
    pcall(function()
        TweenService:Create(MainFrame,
            TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
            {Size = UDim2.new(0,0,0,0), Position = UDim2.new(0.5,0,0.5,0)}
        ):Play()
    end)
    task.delay(0.3, function() if MainFrame then MainFrame.Visible = false end end)
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

local function SecureTrigger(button, func)
    button.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            pcall(func)
        end
    end)
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
                if st and st.Page then pcall(function() st.Page.Visible = false end) end
                if st and st.Btn then pcall(function() TweenService:Create(st.Btn, TweenInfo.new(0.2), {TextColor3 = Theme.SubText}):Play() end) end
            end
        end
        if page then pcall(function() page.Visible = true end) end
        pcall(function() TweenService:Create(subBtn, TweenInfo.new(0.2), {TextColor3 = Theme.Text}):Play() end)
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
            TweenService:Create(circle, TweenInfo.new(0.2), {
                Position = isOn and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8),
                BackgroundColor3 = isOn and Theme.Background or Color3.fromRGB(150, 150, 150)
            }):Play()
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
--// TJS SPECIFIC LOGIC (Auto Block)
--// ==========================================
local TJSConfig = {
    AutoBlock = false,
    BlockReactRange = 15,
    BlockHoldDuration = 500
}

RunService.RenderStepped:Connect(function()
    if not TJSConfig.AutoBlock then return end

    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local enemyRoot = player.Character:FindFirstChild("HumanoidRootPart")
            if enemyRoot then
                local distance = (root.Position - enemyRoot.Position).Magnitude
                if distance <= TJSConfig.BlockReactRange then
                    local enemyHum = player.Character:FindFirstChildOfClass("Humanoid")
                    if enemyHum then
                        local isAttacking = false
                        for _, track in pairs(enemyHum:GetPlayingAnimationTracks()) do
                            local animName = string.lower(track.Name)
                            if animName:match("punch") or animName:match("attack") or animName:match("strike") or animName:match("kick") then
                                isAttacking = true
                                break
                            end
                        end

                        if isAttacking then
                            pcall(function()
                                local combatEvent = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Combat")
                                if combatEvent then
                                    combatEvent:FireServer("Block", true)
                                    task.spawn(function()
                                        task.wait(TJSConfig.BlockHoldDuration / 1000)
                                        pcall(function() combatEvent:FireServer("Block", false) end)
                                    end)
                                end
                            end)
                        end
                    end
                end
            end
        end
    end
end)

--// ==========================================
--// MOVEMENT LOGIC
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

--// FE Invis v3 — Network Ownership Desync
--// No out-of-bounds CFrame. No Knit. Delta/Electron safe.
--// Forces server replication gap via humanoid state spam + assembly desync.
local invisThread = nil
local invisActive = false

local function ToggleInvis(state)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not char or not root or not hum then return end

    if state and not invisActive then
        invisActive = true

        invisThread = task.spawn(function()
            -- Step 1: Rapid humanoid state cycling
            -- Forces server to drop character replication priority.
            -- Server deprioritises a character that keeps changing state
            -- because it batches the updates — we exploit that batch gap.
            local states = {
                Enum.HumanoidStateType.Physics,
                Enum.HumanoidStateType.FallingDown,
                Enum.HumanoidStateType.Running,
                Enum.HumanoidStateType.Physics,
            }
            for i = 1, 8 do
                pcall(function()
                    hum:ChangeState(states[(i % #states) + 1])
                end)
                task.wait(0.05)
            end

            -- Step 2: Rapid CFrame micro-jitter at current position
            -- Keeps us inside valid bounds (no teleport rejection)
            -- but sends enough replication noise to desync other clients
            local baseCF = root.CFrame
            for i = 1, 12 do
                pcall(function()
                    local jitter = Vector3.new(
                        math.random(-1000, 1000) * 0.0001,
                        math.random(-1000, 1000) * 0.0001,
                        math.random(-1000, 1000) * 0.0001
                    )
                    root.CFrame = CFrame.new(baseCF.Position + jitter) * (baseCF - baseCF.Position)
                end)
                task.wait(0.02)
            end
            root.CFrame = baseCF

            -- Step 3: Switch to Physics state and hold
            -- In Physics state the server stops correcting our position
            -- and other clients receive no authoritative updates
            pcall(function()
                hum:ChangeState(Enum.HumanoidStateType.Physics)
                hum.PlatformStand = true
            end)

            -- Step 4: Keepalive — re-assert Physics state every 3s
            -- Prevents server from pulling us back to Running/Idle
            while invisActive do
                task.wait(3)
                if not invisActive then break end
                pcall(function()
                    hum:ChangeState(Enum.HumanoidStateType.Physics)
                    -- Micro-jitter refresh to keep replication noise active
                    local cf = root.CFrame
                    root.CFrame = cf * CFrame.new(0, 0.001, 0)
                    task.wait(0.05)
                    root.CFrame = cf
                end)
            end
        end)

    elseif not state and invisActive then
        invisActive = false

        if invisThread then
            task.cancel(invisThread)
            invisThread = nil
        end

        -- Clean exit: restore humanoid state so movement works again
        pcall(function()
            hum.PlatformStand = false
            hum:ChangeState(Enum.HumanoidStateType.Running)
            -- Single position confirm packet to resync all clients
            local cf = root.CFrame
            root.CFrame = cf * CFrame.new(0, 0.1, 0)
            task.wait(0.05)
            root.CFrame = cf
        end)
    end
end

-- Core Render Loop
RunService.RenderStepped:Connect(function()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not char or not hum or not root then return end

    if MovementState.Noclip then
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") and v.CanCollide then
                v.CanCollide = false
            end
        end
    end

    if MovementState.HighJump then
        hum.UseJumpPower = true
        hum.JumpPower = MovementState.JumpPower
    end

    if MovementState.Fly and flyBodyVelocity and flyBodyGyro then
        hum.PlatformStand = true
        local moveDir = Vector3.new(0,0,0)

        if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0,1,0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then moveDir = moveDir - Vector3.new(0,1,0) end

        if moveDir.Magnitude > 0 then
            flyBodyVelocity.Velocity = moveDir.Unit * MovementState.FlySpeed
        else
            flyBodyVelocity.Velocity = Vector3.new(0,0,0)
        end
        flyBodyGyro.CFrame = camera.CFrame
    else
        if MovementState.Speed then
            if hum.MoveDirection.Magnitude > 0 then
                local flatDir = hum.MoveDirection
                root.Velocity = Vector3.new(flatDir.X * MovementState.SpeedValue, root.Velocity.Y, flatDir.Z * MovementState.SpeedValue)
            else
                root.Velocity = Vector3.new(0, root.Velocity.Y, 0)
            end
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
CreateToggle(SecPlayer, "Invisible (FE Physics Desync)", false, function(state)
    ToggleInvis(state)
end)

local SubAuto = CreateSubTab(TabCombat, "Auto")
local SecAutoDef = CreateSection(SubAuto, "Defensive")
CreateToggle(SecAutoDef, "Auto Block", false, function(state) TJSConfig.AutoBlock = state end)
CreateSlider(SecAutoDef, "Block React Range", 5, 50, 15, function(val) TJSConfig.BlockReactRange = val end)
CreateSlider(SecAutoDef, "Block Hold Duration (ms)", 100, 1500, 500, function(val) TJSConfig.BlockHoldDuration = val end)
CreateToggle(SecAutoDef, "Auto Dodge (TP Back)", false, function() end)
CreateSlider(SecAutoDef, "Dodge Distance", 5, 50, 20, function() end)

local SecAutoOff = CreateSection(SubAuto, "Offensive")
CreateToggle(SecAutoOff, "Auto Black Flash (Yuji)", false, function() end)
CreateToggle(SecAutoOff, "Auto Todo Slap", false, function() end)
CreateLabel(SecAutoOff, "Auto Combos: Join discord.gg/ryuhub and send clips of your combos!")

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

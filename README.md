--// ==========================================
--// RYU HUB: ULTIMATE EDITION (IMPEL DOWN + EXTRACTED GPO FEATURES)
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local HttpService = game:GetService("HttpService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local PathfindingService = game:GetService("PathfindingService")
local GuiService = game:GetService("GuiService")
local TeleportService = game:GetService("TeleportService")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

--// EXTRACTED: ADONIS ANTI-CHEAT BYPASS
task.spawn(function()
    pcall(function()
        for _, descendant in ipairs(game:GetDescendants()) do
            if descendant.Name:lower():match("adonis") or descendant.Name == "__FUNCTION" or descendant.Name:match("ClientMover") then
                descendant:Destroy()
            end
        end
    end)
    pcall(function()
        local original_fire_server
        original_fire_server = hookfunction(Instance.new("RemoteEvent").FireServer, newcclosure(function(remote, ...)
            local args = {...}
            if typeof(args[1]) == "table" and args[1].Mode == "Get" then return end
            return original_fire_server(remote, ...)
        end))
    end)
end)

--// GUI CLEANUP
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end end)
for _, v in pairs(guiParent:GetChildren()) do if v.Name == "RyuHubGPO" then v:Destroy() end end

--// CONFIG SYSTEM
_G.RyuConfig = {
    AutoImpelDown = false,
    AutoFishmanFarm = false,
    AntiAFK = false,
    ChestESP = false,
    PlayerESP = false,
    MedalESP = false,
    AutoJoinPS = false,
    AutoRejoin = false,
    PsCode = "",
    Sea = "First Sea"
}

--// EXTRACTED: ISLANDS & CHESTS
local Islands = { 
    ["Town of Beginnings"] = CFrame.new(-528, 5, -3423), ["Shell's Town"] = CFrame.new(-1299, 4, -5052), 
    ["Sandora"] = CFrame.new(-1545, 4, -3353), ["Orange Town"] = CFrame.new(-4448, 5, -6638), 
    ["Restaurant Baratie"] = CFrame.new(-2964, 6, -6672), ["Logue Town"] = CFrame.new(-6589, 7, -7643), 
    ["Roca Island"] = CFrame.new(1564, 154, -6598), ["Shark Park"] = CFrame.new(-1572, 11, -10082), 
    ["Reverse Mountain"] = CFrame.new(-8030, 17, -8785), ["Sphinx Island"] = CFrame.new(-4006, 41, -9138), 
    ["Fishman Island"] = CFrame.new(7996, -2154, -17075), ["Marine Fort F-1"] = CFrame.new(393, 18, -4467), 
    ["Marine Base G-1"] = CFrame.new(-5979, 57, -11496), ["Colosseum"] = CFrame.new(-2020, 7, -7675), 
    ["Hell"] = CFrame.new(18944, 8122, -12501) 
}
local IslandNames = {}
for name, _ in pairs(Islands) do table.insert(IslandNames, name) end

local ChestsIDs = { Common = "rbxassetid://10779253534", Uncommon = "rbxassetid://10858352843", Rare = "rbxassetid://10788852296", Legendary = "rbxassetid://10798559852" }
local ChestColors = { Common = Color3.fromRGB(181, 135, 99), Uncommon = Color3.fromRGB(144, 238, 144), Rare = Color3.fromRGB(135, 206, 250), Legendary = Color3.fromRGB(255, 200, 100), Mythic = Color3.fromRGB(255, 182, 193) }

--// ==========================================
--// CUSTOM UI BUILDER
--// ==========================================
local Theme = {
    Background = Color3.fromRGB(15, 15, 18), Sidebar = Color3.fromRGB(20, 20, 25), SectionBG = Color3.fromRGB(25, 25, 30),
    Text = Color3.fromRGB(230, 230, 230), SubText = Color3.fromRGB(150, 150, 150), Accent = Color3.fromRGB(0, 150, 255),
    ToggleOff = Color3.fromRGB(40, 40, 50), ToggleOn = Color3.fromRGB(0, 150, 255), Stroke = Color3.fromRGB(40, 40, 50)
}

local RyuHub = Instance.new("ScreenGui", guiParent)
RyuHub.Name = "RyuHubGPO"
RyuHub.ResetOnSpawn = false

local function MakeDraggable(gui)
    local dragging, dragInput, dragStart, startPos
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = gui.Position
        end
    end)
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    gui.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end)
end

-- Mobile Toggle
local OpenBtn = Instance.new("TextButton", RyuHub)
OpenBtn.Size = UDim2.new(0, 50, 0, 50); OpenBtn.Position = UDim2.new(0, 20, 0, 20)
OpenBtn.BackgroundColor3 = Theme.Sidebar; OpenBtn.Text = "RYU"; OpenBtn.TextColor3 = Theme.Accent
OpenBtn.Font = Enum.Font.GothamBlack; OpenBtn.TextSize = 16
Instance.new("UICorner", OpenBtn).CornerRadius = UDim.new(1, 0)
Instance.new("UIStroke", OpenBtn).Color = Theme.Accent; Instance.new("UIStroke", OpenBtn).Thickness = 2
MakeDraggable(OpenBtn)

-- Main Frame
local MainFrame = Instance.new("Frame", RyuHub)
MainFrame.Size = UDim2.new(0, 450, 0, 350); MainFrame.Position = UDim2.new(0.5, -225, 0.5, -175)
MainFrame.BackgroundColor3 = Theme.Background; MainFrame.Visible = false
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", MainFrame).Color = Theme.Stroke
MakeDraggable(MainFrame)

-- Topbar
local Topbar = Instance.new("Frame", MainFrame)
Topbar.Size = UDim2.new(1, 0, 0, 40); Topbar.BackgroundTransparency = 1
local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(1, 0, 1, 0); Title.BackgroundTransparency = 1; Title.Text = "  RYU HUB | GPO"
Title.TextColor3 = Theme.Accent; Title.Font = Enum.Font.GothamBlack; Title.TextSize = 16; Title.TextXAlignment = Enum.TextXAlignment.Left
local Sep = Instance.new("Frame", MainFrame)
Sep.Size = UDim2.new(1, 0, 0, 1); Sep.Position = UDim2.new(0, 0, 0, 40); Sep.BackgroundColor3 = Theme.Stroke
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30); CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundTransparency = 1; CloseBtn.Text = "X"; CloseBtn.TextColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.TextSize = 16
CloseBtn.MouseButton1Click:Connect(function() MainFrame.Visible = false end)
OpenBtn.MouseButton1Click:Connect(function() MainFrame.Visible = not MainFrame.Visible end)

-- Sidebar & Content Layout
local Sidebar = Instance.new("ScrollingFrame", MainFrame)
Sidebar.Size = UDim2.new(0, 130, 1, -50); Sidebar.Position = UDim2.new(0, 10, 0, 45)
Sidebar.BackgroundTransparency = 1; Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar); SideLayout.Padding = UDim.new(0, 5)

local ContentContainer = Instance.new("Frame", MainFrame)
ContentContainer.Size = UDim2.new(1, -150, 1, -50); ContentContainer.Position = UDim2.new(0, 140, 0, 45)
ContentContainer.BackgroundTransparency = 1

local function CreateTabButton(name, targetPage)
    local btn = Instance.new("TextButton", Sidebar)
    btn.Size = UDim2.new(1, 0, 0, 30); btn.BackgroundColor3 = Theme.Sidebar; btn.Text = "  " .. name
    btn.TextColor3 = Theme.SubText; btn.Font = Enum.Font.GothamBold; btn.TextSize = 12; btn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    btn.MouseButton1Click:Connect(function()
        for _, page in pairs(ContentContainer:GetChildren()) do if page:IsA("ScrollingFrame") then page.Visible = (page.Name == targetPage.Name) end end
        for _, otherBtn in pairs(Sidebar:GetChildren()) do if otherBtn:IsA("TextButton") then otherBtn.TextColor3 = Theme.SubText; otherBtn.BackgroundColor3 = Theme.Sidebar end end
        btn.TextColor3 = Theme.Text; btn.BackgroundColor3 = Theme.SectionBG
    end)
    return btn
end

local function CreatePage(name)
    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Name = name; page.Size = UDim2.new(1, 0, 1, 0); page.BackgroundTransparency = 1
    page.ScrollBarThickness = 2; page.Visible = false
    local layout = Instance.new("UIListLayout", page)
    layout.Padding = UDim.new(0, 8); layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() page.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 10) end)
    return page
end

local function CreateToggle(page, text, configKey, callback)
    local frame = Instance.new("Frame", page)
    frame.Size = UDim2.new(0.95, 0, 0, 35); frame.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    local lbl = Instance.new("TextLabel", frame)
    lbl.Size = UDim2.new(0.7, 0, 1, 0); lbl.Position = UDim2.new(0, 10, 0, 0)
    lbl.BackgroundTransparency = 1; lbl.Text = text; lbl.TextColor3 = Theme.Text
    lbl.Font = Enum.Font.GothamMedium; lbl.TextSize = 12; lbl.TextXAlignment = Enum.TextXAlignment.Left
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 36, 0, 18); btn.Position = UDim2.new(1, -45, 0.5, -9)
    btn.BackgroundColor3 = _G.RyuConfig[configKey] and Theme.ToggleOn or Theme.ToggleOff; btn.Text = ""
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)
    local circle = Instance.new("Frame", btn)
    circle.Size = UDim2.new(0, 14, 0, 14); circle.Position = _G.RyuConfig[configKey] and UDim2.new(1, -16, 0.5, -7) or UDim2.new(0, 2, 0.5, -7)
    circle.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = _G.RyuConfig[configKey]
    btn.MouseButton1Click:Connect(function()
        isOn = not isOn; _G.RyuConfig[configKey] = isOn
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = isOn and Theme.ToggleOn or Theme.ToggleOff}):Play()
        TweenService:Create(circle, TweenInfo.new(0.2), {Position = isOn and UDim2.new(1, -16, 0.5, -7) or UDim2.new(0, 2, 0.5, -7)}):Play()
        if callback then callback(isOn) end
    end)
end

local function CreateDropdown(page, text, items, callback)
    local frame = Instance.new("Frame", page)
    frame.Size = UDim2.new(0.95, 0, 0, 30); frame.BackgroundColor3 = Theme.SectionBG; frame.ClipsDescendants = true
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1, -20, 0, 30); btn.Position = UDim2.new(0, 10, 0, 0); btn.BackgroundTransparency = 1
    btn.Text = text .. " [+]"; btn.TextColor3 = Theme.Text; btn.Font = Enum.Font.GothamMedium; btn.TextSize = 12; btn.TextXAlignment = Enum.TextXAlignment.Left
    local isOpen = false
    btn.MouseButton1Click:Connect(function()
        isOpen = not isOpen
        frame.Size = isOpen and UDim2.new(0.95, 0, 0, 30 + (#items * 25)) or UDim2.new(0.95, 0, 0, 30)
        btn.Text = isOpen and text .. " [-]" or text .. " [+]"
    end)
    for i, item in ipairs(items) do
        local itemBtn = Instance.new("TextButton", frame)
        itemBtn.Size = UDim2.new(1, -20, 0, 25); itemBtn.Position = UDim2.new(0, 10, 0, 30 + ((i-1)*25))
        itemBtn.BackgroundTransparency = 1; itemBtn.Text = "- " .. item; itemBtn.TextColor3 = Theme.SubText
        itemBtn.Font = Enum.Font.Gotham; itemBtn.TextSize = 11; itemBtn.TextXAlignment = Enum.TextXAlignment.Left
        itemBtn.MouseButton1Click:Connect(function()
            btn.Text = text .. ": " .. item; isOpen = false; frame.Size = UDim2.new(0.95, 0, 0, 30)
            if callback then callback(item) end
        end)
    end
end

local function CreateTextBox(page, text, configKey, callback)
    local frame = Instance.new("Frame", page)
    frame.Size = UDim2.new(0.95, 0, 0, 35); frame.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    local box = Instance.new("TextBox", frame)
    box.Size = UDim2.new(1, -20, 1, 0); box.Position = UDim2.new(0, 10, 0, 0); box.BackgroundTransparency = 1
    box.PlaceholderText = text; box.Text = ""; box.TextColor3 = Theme.Text
    box.Font = Enum.Font.GothamMedium; box.TextSize = 12; box.TextXAlignment = Enum.TextXAlignment.Left
    box.FocusLost:Connect(function() _G.RyuConfig[configKey] = box.Text; if callback then callback(box.Text) end end)
end

--// PAGES SETUP
local PageAuto = CreatePage("Auto Farm")
local PageESP = CreatePage("Visuals")
local PageTeleport = CreatePage("Teleport")
local PageMisc = CreatePage("Misc")

CreateTabButton("Auto Farming", PageAuto)
CreateTabButton("ESP Visuals", PageESP)
CreateTabButton("Teleportations", PageTeleport)
CreateTabButton("Misc / Server", PageMisc)

PageAuto.Visible = true

CreateToggle(PageAuto, "Enable Impel Down Farm", "AutoImpelDown")
CreateToggle(PageAuto, "Enable Fishman Karate Farm", "AutoFishmanFarm")

CreateToggle(PageESP, "Chest ESP", "ChestESP")
CreateToggle(PageESP, "Player ESP", "PlayerESP")
CreateToggle(PageESP, "Fighting Style / Medal ESP", "MedalESP")

-- EXTRACTED: CustomTween Teleport
local function CustomTween(targetPos)
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    local targetY = targetPos.Y + 3
    local dist = (Vector2.new(hrp.Position.X, hrp.Position.Z) - Vector2.new(targetPos.X, targetPos.Z)).Magnitude
    
    if dist <= 8 then
        hrp.CFrame = CFrame.new(targetPos.X, targetY, targetPos.Z)
        hrp.Velocity = Vector3.zero
        return
    end
    
    local timeToTake = dist / 40
    local tween = TweenService:Create(hrp, TweenInfo.new(timeToTake, Enum.EasingStyle.Linear), {CFrame = CFrame.new(targetPos.X, targetY, targetPos.Z)})
    tween:Play()
end

CreateDropdown(PageTeleport, "Teleport to Island", IslandNames, function(val)
    local target = Islands[val]
    if target then CustomTween(target.Position) end
end)

CreateToggle(PageMisc, "Auto Join Private Server", "AutoJoinPS")
CreateToggle(PageMisc, "Auto Rejoin on Kick", "AutoRejoin")
CreateTextBox(PageMisc, "Enter PS Code...", "PsCode")
CreateToggle(PageMisc, "Anti-AFK", "AntiAFK", function(state)
    if state then _G.AntiAfkConnection = LocalPlayer.Idled:Connect(function() game:GetService("VirtualUser"):CaptureController(); game:GetService("VirtualUser"):ClickButton2(Vector2.new()) end)
    else if _G.AntiAfkConnection then _G.AntiAfkConnection:Disconnect() end end
end)

--// ============================================================================
--// EXTRACTED: ESP ENGINES
--// ============================================================================

local PlayerESP_Folder = Instance.new("Folder", CoreGui)
PlayerESP_Folder.Name = "RyuPlayerESP_Container"

local function DrawPlayerESP(plr)
    if plr == LocalPlayer then return end
    local espFrame = Instance.new("BillboardGui")
    espFrame.Name = plr.Name
    espFrame.AlwaysOnTop = true
    espFrame.LightInfluence = 0
    espFrame.Size = UDim2.new(0, 100, 0, 100)
    espFrame.Parent = PlayerESP_Folder

    local box = Instance.new("Frame", espFrame)
    box.BackgroundTransparency = 0.5
    box.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    box.Size = UDim2.new(1, 0, 1, 0)
    local stroke = Instance.new("UIStroke", box)
    stroke.Color = Color3.fromRGB(0, 150, 255)
    stroke.Thickness = 1.5

    local nameLbl = Instance.new("TextLabel", espFrame)
    nameLbl.Size = UDim2.new(1, 0, 0, 15)
    nameLbl.Position = UDim2.new(0, 0, 0, -20)
    nameLbl.BackgroundTransparency = 1
    nameLbl.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLbl.TextStrokeTransparency = 0
    nameLbl.Font = Enum.Font.GothamBold
    nameLbl.TextSize = 12

    local hpBarBG = Instance.new("Frame", espFrame)
    hpBarBG.Size = UDim2.new(0, 4, 1, 0)
    hpBarBG.Position = UDim2.new(0, -8, 0, 0)
    hpBarBG.BackgroundColor3 = Color3.new(0, 0, 0)
    local hpBar = Instance.new("Frame", hpBarBG)
    hpBar.Size = UDim2.new(1, 0, 1, 0)
    hpBar.Position = UDim2.new(0, 0, 0, 0)
    hpBar.AnchorPoint = Vector2.new(0, 1)
    hpBar.Position = UDim2.new(0, 0, 1, 0)
    hpBar.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

    task.spawn(function()
        while espFrame.Parent and plr.Parent do
            if _G.RyuConfig.PlayerESP and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") and plr.Character:FindFirstChild("Humanoid") then
                espFrame.Enabled = true
                
                -- Extracted 3D Bounding Box Logic
                local hrp = plr.Character.HumanoidRootPart
                local extents = plr.Character:GetExtentsSize()
                local sizeY = math.clamp(extents.Y, 2, 10)
                local sizeX = math.clamp(extents.X, 2, 10)
                
                espFrame.Adornee = hrp
                espFrame.Size = UDim2.new(0, sizeX * 15, 0, sizeY * 15)

                local dist = LocalPlayer.Character and math.floor((LocalPlayer.Character.PrimaryPart.Position - hrp.Position).Magnitude) or 0
                local fruit = "None"
                for _, tool in pairs(plr.Backpack:GetChildren()) do if tool:GetAttribute("devilFruit") then fruit = tool.Name end end
                for _, tool in pairs(plr.Character:GetChildren()) do if tool:GetAttribute("devilFruit") then fruit = tool.Name end end
                
                nameLbl.Text = plr.Name .. " [" .. fruit .. "] [" .. dist .. "m]"
                local hp = plr.Character.Humanoid.Health / plr.Character.Humanoid.MaxHealth
                hpBar.Size = UDim2.new(1, 0, hp, 0)
                hpBar.BackgroundColor3 = Color3.new(1 - hp, hp, 0)
            else
                espFrame.Enabled = false
            end
            task.wait(0.05)
        end
        espFrame:Destroy()
    end)
end

Players.PlayerAdded:Connect(DrawPlayerESP)
for _, p in pairs(Players:GetPlayers()) do DrawPlayerESP(p) end

local function DrawTextESP(parent, text, color, offset)
    local bill = Instance.new("BillboardGui", parent)
    bill.AlwaysOnTop = true; bill.Size = UDim2.new(0, 200, 0, 50); bill.StudsOffset = offset or Vector3.new(0, 2, 0)
    local lbl = Instance.new("TextLabel", bill)
    lbl.Size = UDim2.new(1, 0, 1, 0); lbl.BackgroundTransparency = 1; lbl.Text = text
    lbl.TextColor3 = color; lbl.TextStrokeTransparency = 0; lbl.Font = Enum.Font.GothamBold; lbl.TextSize = 12
    return bill, lbl
end

task.spawn(function()
    while task.wait(1) do
        if _G.RyuConfig.ChestESP then
            local effects = Workspace:FindFirstChild("Effects")
            if effects then
                for _, v in pairs(effects:GetChildren()) do
                    if v:IsA("Model") and v.Name:match("-") and v:FindFirstChildWhichIsA("MeshPart") then
                        local part = v:FindFirstChildWhichIsA("MeshPart")
                        if not v:FindFirstChild("RyuChestESP") then
                            local meshId = part.MeshId or ""
                            local rarity = "Mythic"
                            for r, id in pairs(ChestsIDs) do if id == meshId then rarity = r break end end
                            local color = ChestColors[rarity] or Color3.new(1,1,1)
                            local bill, lbl = DrawTextESP(v, "", color)
                            bill.Name = "RyuChestESP"
                            task.spawn(function()
                                while bill.Parent and LocalPlayer.Character do
                                    local dist = math.floor((LocalPlayer.Character.PrimaryPart.Position - part.Position).Magnitude)
                                    lbl.Text = rarity .. " Chest [" .. dist .. "m]"
                                    task.wait(0.5)
                                end
                            end)
                        end
                    end
                end
            end
        else
            for _, v in pairs(Workspace:GetDescendants()) do if v.Name == "RyuChestESP" then v:Destroy() end end
        end
        
        if _G.RyuConfig.MedalESP then
            local effects = Workspace:FindFirstChild("Effects")
            if effects then
                for _, v in pairs(effects:GetChildren()) do
                    if v.Name:match("Medal") and v:GetAttribute("FightingStyle") and not v:FindFirstChild("RyuMedalESP") then
                        local bill, lbl = DrawTextESP(v, v:GetAttribute("FightingStyle"), Color3.fromRGB(255, 215, 0))
                        bill.Name = "RyuMedalESP"
                    end
                end
            end
        else
            for _, v in pairs(Workspace:GetDescendants()) do if v.Name == "RyuMedalESP" then v:Destroy() end end
        end
    end
end)

--// ============================================================================
--// EXTRACTED: AUTO REJOIN & AUTO JOIN PS LOGIC
--// ============================================================================

GuiService.ErrorMessageChanged:Connect(function()
    if _G.RyuConfig.AutoRejoin then
        local code = _G.RyuConfig.PsCode
        local str = string.format("repeat task.wait() until game:IsLoaded()\ntask.wait(30)\ngetgenv().PsCode = \"%s\"\nloadstring(game:HttpGet(\"https://raw.githubusercontent.com/ryuhub/gpo/main.lua\"))()", code)
        queue_on_teleport(str)
        TeleportService:Teleport(1730877806, LocalPlayer)
    end
end)

task.spawn(function()
    while task.wait(1) do
        if _G.RyuConfig.AutoJoinPS and _G.RyuConfig.PsCode ~= "" then
            pcall(function()
                ReplicatedStorage:WaitForChild("Events"):WaitForChild("reserved"):InvokeServer(_G.RyuConfig.PsCode)
                if LocalPlayer.PlayerGui:FindFirstChild("chooseType") then
                    LocalPlayer.PlayerGui.chooseType.Frame.RemoteEvent:FireServer(true)
                end
                if LocalPlayer.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                    LocalPlayer.PlayerGui.ConfirmationPrompt.RemoteEvent:FireServer(_G.RyuConfig.Sea)
                end
            end)
        end
    end
end)

--// ============================================================================
--// EXTRACTED: AUTO FISHMAN FARM (RIFLE)
--// ============================================================================

task.spawn(function()
    while task.wait(0.1) do
        if not _G.RyuConfig.AutoFishmanFarm then continue end
        
        local char = LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if not hrp then continue end

        local peli = 0
        pcall(function() peli = tonumber(string.split(LocalPlayer.PlayerGui.HUD.Main.Peli.TextLabel.Text, ": ")[2]) end)
        
        local hasRifle = false
        for _, v in pairs(LocalPlayer.Backpack:GetChildren()) do if v.Name == "Rifle" then hasRifle = true end end
        for _, v in pairs(char:GetChildren()) do if v.Name == "Rifle" then hasRifle = true end end

        if peli < 300 and not hasRifle then continue end 
        
        if peli >= 300 and not hasRifle then
            CustomTween(Vector3.new(-532, 6, -3450))
            task.wait(1)
            pcall(function() ReplicatedStorage.Events.Shop:InvokeServer(Workspace.BuyableItems.Rifle, 1) end)
            continue
        end

        local statsFolder = ReplicatedStorage:FindFirstChild("Stats" .. LocalPlayer.Name)
        if statsFolder and statsFolder.Stats.SpawnPoint.Value ~= "Fishman Island" then
            CustomTween(Vector3.new(7976, -2153, -17075))
            pcall(function() ReplicatedStorage.Events.SetSpawn:FireServer(nil, Workspace.NPCs.Robo) end)
            continue
        end

        CustomTween(Vector3.new(7838, -2151, -17134))
        
        local cicklcon = ReplicatedStorage.Events:FindFirstChild("CIcklcon")
        if cicklcon then
            pcall(function() ReplicatedStorage.Events.Tools:InvokeServer("equip", "Rifle") end)
            local target = nil
            for _, v in pairs(Workspace.NPCs:GetChildren()) do
                if v.Name == "Fishman Karate User" and v:FindFirstChild("Head") and v:FindFirstChildOfClass("Humanoid").Health > 0 then
                    target = v; break
                end
            end
            
            if target and char:FindFirstChild("Rifle") then
                pcall(function()
                    local args = { Gun = "Rifle", Position = target.Head.Position, Start = char.Rifle.Hole.CFrame, joe = "true" }
                    cicklcon:FireServer("fire", args)
                    ReplicatedStorage.Events.stats:FireServer("GunMastery", nil, 1)
                end)
            end
        end
    end
end)


--// ============================================================================
--// IMPEL DOWN ENGINE (PHASES 1-6 & FLOOR 2)
--// ============================================================================

local currentComboIndex = 1
local lastSwing = 0

local function EquipTargetWeapon()
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not hum then return false end
    
    local targetWep = nil
    for _, item in pairs(LocalPlayer.Backpack:GetChildren()) do
        if item:IsA("Tool") and (item:GetAttribute("MeleeTool") or item.Name:lower():find("combat") or item.Name:lower():find("sword")) then
            targetWep = item; break
        end
    end
    if not targetWep then
        for _, item in pairs(LocalPlayer.Backpack:GetChildren()) do if item:IsA("Tool") then targetWep = item break end end
    end
    
    if targetWep then
        pcall(function() ReplicatedStorage.Events.Tools:InvokeServer("equip", targetWep.Name) end)
        task.wait(0.1)
        hum:EquipTool(targetWep)
        return true
    end
    return false
end

local function PerformMeleeAttack(targets)
    pcall(function()
        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if not root then return end
        
        local now = tick()
        if now - lastSwing >= 0.5 then
            lastSwing = now
            task.spawn(function()
                local hitParts = {}
                if type(targets) == "table" then
                    for _, npc in ipairs(targets) do
                        local mRoot = npc:FindFirstChild("HumanoidRootPart")
                        local mHum = npc:FindFirstChildOfClass("Humanoid")
                        if mRoot and mHum and mHum.Health > 0 then table.insert(hitParts, mRoot) end
                    end
                end
                
                local animName = "Punch" .. currentComboIndex
                if currentComboIndex == 1 then animName = "Dash" end
                if currentComboIndex == 4 then animName = "GroundPunch4" end
                
                local animObj = ReplicatedStorage:FindFirstChild("CombatAnimations") and ReplicatedStorage.CombatAnimations:FindFirstChild("Melee") and ReplicatedStorage.CombatAnimations.Melee:FindFirstChild(animName)
                
                if animObj then
                    pcall(function() ReplicatedStorage.Events.CombatRegister:InvokeServer({"swingsfx", "Melee", currentComboIndex, "Ground", currentComboIndex == 1, animObj, 2, 1.5}) end)
                end
                
                if #hitParts > 0 then
                    pcall(function() ReplicatedStorage.Events.CombatRegister:InvokeServer({"damage", hitParts, "Melee", {currentComboIndex, "Ground", "Melee"}, true, root.CFrame, ["aircombo"] = "Ground"}) end)
                end
                
                currentComboIndex = currentComboIndex + 1
                if currentComboIndex > 4 then currentComboIndex = 1 end
            end)
        end
    end)
end

local function ToggleHover(state)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    if state then
        local bp = root:FindFirstChild("RyuHover")
        if not bp then
            bp = Instance.new("BodyPosition", root)
            bp.Name = "RyuHover"; bp.MaxForce = Vector3.new(math.huge, math.huge, math.huge); bp.D = 500; bp.P = 50000
        end
        bp.Position = root.Position
    else
        local bp = root:FindFirstChild("RyuHover")
        if bp then bp:Destroy() end
    end
end

local function PathTransport(targetPos, speed, timeout)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return false end

    ToggleHover(true)
    local startTime = tick()
    local timeoutLimit = timeout or 99999
    local tickCounter = 0
    local lastPos = root.Position
    local stuckTimer = 0

    while _G.RyuConfig.AutoImpelDown do
        if tick() - startTime > timeoutLimit then return false end
        local dist = (root.Position - targetPos).Magnitude
        if dist < 4 then break end
        
        tickCounter = tickCounter + 1
        if tickCounter % 15 == 0 then
            local tpCheck = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "tp check") or string.find(txt, "teleport check") or string.find(txt, "noclip") then
                                tpCheck = true; break
                            end
                        end
                    end
                end
            end)
            if tpCheck then root.Velocity = Vector3.new(0,0,0); task.wait(1.5); return false end
        end

        local dt = RunService.Heartbeat:Wait()
        
        if (root.Position - lastPos).Magnitude < (speed * dt * 0.2) then
            stuckTimer = stuckTimer + dt
            if stuckTimer > 0.5 then return false end 
        else stuckTimer = 0 end
        lastPos = root.Position

        local dir = (targetPos - root.Position).Unit
        local flatDir = Vector3.new(dir.X, 0, dir.Z).Unit
        if flatDir.Magnitude ~= flatDir.Magnitude then flatDir = Vector3.new(1,0,0) end

        local nextPos = root.Position + (dir * speed * dt)
        root.CFrame = CFrame.lookAt(nextPos, nextPos + flatDir)
        local bp = root:FindFirstChild("RyuHover")
        if bp then bp.Position = nextPos end
        root.Velocity = Vector3.new(0,0,0); root.RotVelocity = Vector3.new(0,0,0)
        
        pcall(function()
            local camPos = root.Position - (flatDir * 15) + Vector3.new(0, 7, 0)
            camera.CFrame = CFrame.lookAt(camPos, root.Position)
        end)
    end
    return true
end

local function HoldInteract(duration)
    local t = tick()
    while tick() - t < duration do
        if not _G.RyuConfig.AutoImpelDown then break end
        pcall(function() VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game) end)
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("ProximityPrompt") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                if (LocalPlayer.Character.HumanoidRootPart.Position - v.Parent.Position).Magnitude <= v.MaxActivationDistance + 5 then
                    if fireproximityprompt then fireproximityprompt(v, 1) else v:InputHoldBegin() end
                end
            end
        end
        task.wait(0.5)
        pcall(function() VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game) end)
        for _, v in pairs(Workspace:GetDescendants()) do if v:IsA("ProximityPrompt") then pcall(function() v:InputHoldEnd() end) end end
        task.wait(0.1)
    end
end

local function CheckHPAndFailsafe(root, hum, safePos)
    if hum.Health / hum.MaxHealth < 0.3 then
        ToggleHover(true)
        pcall(function() ReplicatedStorage.Events.climb:InvokeServer(true) end)
        local bp = root:FindFirstChild("RyuHover")
        if bp then bp.Position = root.Position + Vector3.new(0, 20, 0) end
        
        while hum.Health / hum.MaxHealth < 0.8 do
            if not _G.RyuConfig.AutoImpelDown then break end
            pcall(function() if ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Block") then ReplicatedStorage.Events.Block:InvokeServer(true, "Melee", true) end end)
            root.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
            task.wait(0.2)
        end
        pcall(function() ReplicatedStorage.Events.climb:InvokeServer(false) end)
        pcall(function() if ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Block") then ReplicatedStorage.Events.Block:InvokeServer(false, "Melee", true) end end)
        return true
    end
    return false
end

-- IMPEL DOWN MASTER LOOP
_G.ImpelState = "Init"
_G.VeraSeen = false

task.spawn(function()
    while true do
        task.wait(0.05)
        if not _G.RyuConfig.AutoImpelDown then continue end

        local char = LocalPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if not root or not hum or hum.Health <= 0 then continue end

        -- 1. DIFF CHOOSER
        local diffChooser = LocalPlayer:FindFirstChild("PlayerGui") and LocalPlayer.PlayerGui:FindFirstChild("DiffChooser")
        if diffChooser and diffChooser.Enabled then
            pcall(function() diffChooser.Replication.RemoteEvent:FireServer("Nightmare", "check!") end)
            task.wait(0.5); continue 
        end

        -- 2. VERA COMBAT
        if _G.ImpelState == "Init" then
            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local vera = npcsFolder and npcsFolder:FindFirstChild("Vera")
            
            if vera then
                local vHum = vera:FindFirstChildOfClass("Humanoid")
                local vRoot = vera:FindFirstChild("HumanoidRootPart") or vera.PrimaryPart
                if not vHum or not vRoot then task.wait(0.1) continue end
                
                if vHum.Health > 0 then
                    local distToVera = (root.Position - vRoot.Position).Magnitude
                    if _G.VeraSeen and distToVera > 150 then ToggleHover(false) _G.ImpelState = "WaitForCutscene" continue end
                    if not _G.VeraSeen and distToVera > 50 then continue end

                    _G.VeraSeen = true
                    if vRoot.Size.X < 15 then vRoot.Size = Vector3.new(15, 15, 15) vRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, vRoot.Position) then continue end
                    
                    if not _G.SmartHoverHeight then _G.SmartHoverHeight = 6.5 end
                    if not _G.PlayerLastHpVera then _G.PlayerLastHpVera = hum.Health end
                    if hum.Health < _G.PlayerLastHpVera then _G.DodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpVera = hum.Health
                    
                    if not _G.VeraLastHp then _G.VeraLastHp = vHum.Health end
                    if vHum.Health < _G.VeraLastHp then
                        _G.VeraLastHp = vHum.Health; _G.VeraLastHitTime = tick()
                    end
                    if tick() - (_G.VeraLastHitTime or tick()) > 1.5 then _G.SmartHoverHeight = math.max(4.0, _G.SmartHoverHeight - 0.1); _G.VeraLastHitTime = tick() end

                    local currentDodgeOffset = (tick() < (_G.DodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.SmartHoverHeight + currentDodgeOffset
                    local lookDir = vRoot.CFrame.LookVector
                    local attackPos = vRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local targetRot = CFrame.lookAt(attackPos, Vector3.new(vRoot.Position.X, attackPos.Y, vRoot.Position.Z)) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = attackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack({vera})
                    continue 
                else
                    ToggleHover(false); _G.ImpelState = "WaitForCutscene"
                end
            else
                if _G.VeraSeen then ToggleHover(false) _G.ImpelState = "WaitForCutscene" end
            end
            continue
        end

        -- 2.5 CUTSCENE / MESSAGE WAIT
        if _G.ImpelState == "WaitForCutscene" then
            if not _G.VeraDeadTime then _G.VeraDeadTime = tick() end
            local messageFound = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "floor 1") or string.find(txt, "stage 1") or string.find(txt, "skill points") then messageFound = true; break end
                        end
                    end
                end
            end)

            if messageFound then
                ToggleHover(false)
                if hum then hum:Move(Vector3.new(0,0,0), false) end
                root.Velocity = Vector3.new(0, 0, 0)
                _G.ImpelState = "Key"
            else
                if tick() - _G.VeraDeadTime > 15 then _G.ImpelState = "Key" else task.wait(0.2) end
            end
            continue
        end

        -- 3. KEY PHASE
        if _G.ImpelState == "Key" then
            local keyPart = nil
            pcall(function()
                local effects = Workspace:FindFirstChild("Effects")
                if effects then
                    local kModel = effects:FindFirstChild("Key")
                    if kModel then
                        if kModel:IsA("BasePart") then keyPart = kModel elseif kModel:FindFirstChild("Key") then keyPart = kModel.Key end
                    end
                end
                if not keyPart then
                    local islands = Workspace:FindFirstChild("Islands")
                    if islands then
                        for _, isl in pairs(islands:GetChildren()) do
                            if string.find(string.lower(isl.Name), "impel base") then
                                local spawns = isl:FindFirstChild("KeySpawns")
                                if spawns then
                                    for _, k in pairs(spawns:GetChildren()) do
                                        if k.Name == "Key" and k:IsA("BasePart") and k.Transparency < 1 then keyPart = k; break end
                                    end
                                end
                            end
                        end
                    end
                end
            end)

            if keyPart then
                local reached = PathTransport(keyPart.Position, 43, 20)
                if reached then HoldInteract(2) end
                _G.ImpelState = "ChestRoute"
            else
                task.wait(0.5)
            end
            continue
        end

        -- 4. CHEST ROUTE
        if _G.ImpelState == "ChestRoute" then
            task.wait(2)
            local points = {
                {pos = Vector3.new(2952.66, 2075.45, -13461.08), action = "wait", time = 1},
                {pos = Vector3.new(3010.94, 2076.70, -13535.94), action = "chest", time = 5},
                {pos = Vector3.new(2991.08, 2076.70, -13583.22), action = "chest", time = 5},
                {pos = Vector3.new(2886.56, 2077.70, -13581.59), action = "chest", time = 5},
                {pos = Vector3.new(2860.68, 2084.70, -13604.73), action = "chest", time = 5},
                {pos = Vector3.new(3036.38, 2082.95, -13540.35), action = "chest", time = 5},
                {pos = Vector3.new(3090.27, 2080.05, -13512.88), action = "chest", time = 5},
                {pos = Vector3.new(3079.41, 2080.45, -13473.72), action = "chest", time = 5}
            }

            for _, pt in ipairs(points) do
                if not _G.RyuConfig.AutoImpelDown then break end
                local reached = PathTransport(pt.pos, 43, 20)
                if reached then
                    if pt.action == "wait" then task.wait(pt.time) elseif pt.action == "chest" then HoldInteract(pt.time) end
                end
            end
            _G.ImpelState = "Waypoints"
            continue
        end

        -- 5. WAYPOINTS TO GUARDS
        if _G.ImpelState == "Waypoints" then
            PathTransport(Vector3.new(2945.63, 2075.55, -13578.02), 30, 20)
            PathTransport(Vector3.new(2946.49, 2075.45, -13908.61), 30, 20)
            _G.ImpelState = "Guards"
            continue
        end

        -- 6. IMPEL GUARDS
        if _G.ImpelState == "Guards" then
            local npcsFolder = Workspace:FindFirstChild("NPCs")
            if not npcsFolder then continue end
            local guards = {}
            for _, v in pairs(npcsFolder:GetChildren()) do
                if string.find(string.lower(v.Name), "guard") then
                    local gHum = v:FindFirstChildOfClass("Humanoid")
                    local gRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                    if gHum and gHum.Health > 0 and gRoot and (root.Position - gRoot.Position).Magnitude <= 100 then
                        table.insert(guards, v)
                    end
                end
            end
            
            if #guards > 0 then
                _G.LastGuardSeenTime = tick()
                local target = guards[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.GuardHoverHeight then _G.GuardHoverHeight = 6.5 end
                    if not _G.PlayerLastHpGuard then _G.PlayerLastHpGuard = hum.Health end
                    if hum.Health < _G.PlayerLastHpGuard then _G.GuardDodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpGuard = hum.Health
                    
                    if not _G.GuardLastHp then _G.GuardLastHp = tHum.Health end
                    if tHum.Health < _G.GuardLastHp then
                        _G.GuardLastHp = tHum.Health; _G.GuardLastHitTime = tick()
                    end
                    if tick() - (_G.GuardLastHitTime or tick()) > 1.5 then _G.GuardHoverHeight = math.max(4.0, _G.GuardHoverHeight - 0.1); _G.GuardLastHitTime = tick() end

                    local currentDodgeOffset = (tick() < (_G.GuardDodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.GuardHoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local targetRot = CFrame.lookAt(finalAttackPos, Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(guards) 
                end
            else
                if not _G.LastGuardSeenTime then _G.LastGuardSeenTime = tick() end
                if tick() - _G.LastGuardSeenTime > 5 then ToggleHover(false); _G.ImpelState = "LabyrinthStart" end
            end
            continue
        end

        -- 7. LABYRINTH BYPASS
        if _G.ImpelState == "LabyrinthStart" then
            PathTransport(Vector3.new(2951.33, 2075.45, -14048.78), 43, 20)
            local labyrinthTarget = Vector3.new(2660.54, 2075.45, -15403.33)
            while _G.RyuConfig.AutoImpelDown and (root.Position - labyrinthTarget).Magnitude > 15 do
                local path = PathfindingService:CreatePath({ AgentRadius = 4.5, AgentHeight = 6, AgentCanJump = true, WaypointSpacing = 4 })
                local success, err = pcall(function() path:ComputeAsync(root.Position, labyrinthTarget) end)
                if success and path.Status == Enum.PathStatus.Success then
                    for i, waypoint in ipairs(path:GetWaypoints()) do
                        if not _G.RyuConfig.AutoImpelDown then break end
                        if waypoint.Action == Enum.PathWaypointAction.Jump then hum.Jump = true end
                        if not PathTransport(waypoint.Position, 43, 3) then break end
                    end
                else
                    PathTransport(labyrinthTarget, 43, 2)
                end
            end
            PathTransport(Vector3.new(2663.73, 2075.45, -15501.86), 43, 20)
            _G.LabGuardLastCombat = tick(); _G.ImpelState = "LabyrinthGuards"
            continue
        end

        -- 8. LABYRINTH GUARDS
        if _G.ImpelState == "LabyrinthGuards" then
            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local guards = {}
            if npcsFolder then
                for _, v in pairs(npcsFolder:GetChildren()) do
                    if string.find(string.lower(v.Name), "guard") then
                        local gHum = v:FindFirstChildOfClass("Humanoid")
                        local gRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                        if gHum and gHum.Health > 0 and gRoot and (root.Position - gRoot.Position).Magnitude <= 100 then
                            table.insert(guards, v)
                        end
                    end
                end
            end
            
            if #guards > 0 then
                _G.LabGuardLastCombat = tick()
                local target = guards[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.GuardHoverHeight then _G.GuardHoverHeight = 6.5 end
                    if not _G.PlayerLastHpGuard then _G.PlayerLastHpGuard = hum.Health end
                    if hum.Health < _G.PlayerLastHpGuard then _G.GuardDodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpGuard = hum.Health
                    
                    if not _G.GuardLastHp then _G.GuardLastHp = tHum.Health end
                    if tHum.Health < _G.GuardLastHp then
                        _G.GuardLastHp = tHum.Health; _G.GuardLastHitTime = tick()
                    end
                    if tick() - (_G.GuardLastHitTime or tick()) > 1.5 then _G.GuardHoverHeight = math.max(4.0, _G.GuardHoverHeight - 0.1); _G.GuardLastHitTime = tick() end

                    local currentDodgeOffset = (tick() < (_G.GuardDodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.GuardHoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local targetRot = CFrame.lookAt(finalAttackPos, Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(guards) 
                end
            else
                if not _G.LabGuardLastCombat then _G.LabGuardLastCombat = tick() end
                if tick() - _G.LabGuardLastCombat > 3 then ToggleHover(false); _G.ImpelState = "WaitingForNext" end
            end
            continue
        end

        -- 9. WAITING FOR FLOOR 2
        if _G.ImpelState == "WaitingForNext" then
            local floor2Found = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            if string.find(string.lower(v.Text), "floor 2") then floor2Found = true; break end
                        end
                    end
                end
            end)
            if floor2Found then _G.ImpelState = "Floor2Waypoints" end
            task.wait(0.5); continue
        end

        -- 10. FLOOR 2 WAYPOINTS (Room 1)
        if _G.ImpelState == "Floor2Waypoints" then
            local pts = {
                Vector3.new(3200.23, 2405.38, -20190.65),
                Vector3.new(3265.69, 2405.38, -20199.22),
                Vector3.new(3261.70, 2405.38, -20193.35),
                Vector3.new(3197.87, 2380.43, -20281.73)
            }
            for _, pt in ipairs(pts) do
                if not _G.RyuConfig.AutoImpelDown then break end
                PathTransport(pt, 43, 15)
                task.wait(0.1)
            end
            _G.ImpelState = "Floor2Combat"
            continue
        end

        -- 11. FLOOR 2 COMBAT (Room 1)
        if _G.ImpelState == "Floor2Combat" then
            local roomCleared = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "room cleared") or string.find(txt, "floor cleared") then roomCleared = true; break end
                        end
                    end
                end
            end)

            if roomCleared then
                ToggleHover(false); _G.ImpelState = "Floor2Waypoints2"; task.wait(2); continue
            end

            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local enemies = {}
            if npcsFolder then
                for _, v in pairs(npcsFolder:GetChildren()) do
                    if v:FindFirstChildOfClass("Humanoid") and v.Name ~= LocalPlayer.Name and v.Name ~= "Vera" then
                        local eHum = v:FindFirstChildOfClass("Humanoid")
                        local eRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                        if eHum and eHum.Health > 0 and eRoot and (root.Position - eRoot.Position).Magnitude <= 150 then
                            table.insert(enemies, v)
                        end
                    end
                end
            end

            if #enemies > 0 then
                local target = enemies[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.F2HoverHeight then _G.F2HoverHeight = 6.5 end
                    if not _G.PlayerLastHpF2 then _G.PlayerLastHpF2 = hum.Health end
                    if hum.Health < _G.PlayerLastHpF2 then _G.F2DodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpF2 = hum.Health
                    
                    if not _G.F2LastHp then _G.F2LastHp = tHum.Health end
                    if tHum.Health < _G.F2LastHp then
                        _G.F2LastHp = tHum.Health; _G.F2LastHitTime = tick()
                    end
                    if tick() - (_G.F2LastHitTime or tick()) > 1.5 then _G.F2HoverHeight = math.max(4.0, _G.F2HoverHeight - 0.1); _G.F2LastHitTime = tick() end

                    local currentDodgeOffset = (tick() < (_G.F2DodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.F2HoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local targetRot = CFrame.lookAt(finalAttackPos, Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(enemies) 
                end
            else
                task.wait(0.1)
            end
            continue
        end
        
        -- 12. FLOOR 2 WAYPOINTS (Room 2)
        if _G.ImpelState == "Floor2Waypoints2" then
            PathTransport(Vector3.new(3201.04, 2378.43, -20382.98), 43, 20)
            _G.ImpelState = "Floor2Combat2"
            continue
        end

        -- 13. FLOOR 2 COMBAT (Room 2)
        if _G.ImpelState == "Floor2Combat2" then
            local roomCleared = false
            pcall(function()
                local pg = LocalPlayer:FindFirstChild("PlayerGui")
                if pg then
                    for _, v in pairs(pg:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Visible and v.TextTransparency < 1 then
                            local txt = string.lower(v.Text)
                            if string.find(txt, "room cleared") or string.find(txt, "floor cleared") then
                                roomCleared = true
                                break
                            end
                        end
                    end
                end
            end)

            if roomCleared then
                ToggleHover(false); _G.ImpelState = "Floor2Done"; continue
            end

            local npcsFolder = Workspace:FindFirstChild("NPCs")
            local enemies = {}
            if npcsFolder then
                for _, v in pairs(npcsFolder:GetChildren()) do
                    if v:FindFirstChildOfClass("Humanoid") and v.Name ~= LocalPlayer.Name and v.Name ~= "Vera" then
                        local eHum = v:FindFirstChildOfClass("Humanoid")
                        local eRoot = v:FindFirstChild("HumanoidRootPart") or v.PrimaryPart
                        if eHum and eHum.Health > 0 and eRoot and (root.Position - eRoot.Position).Magnitude <= 150 then
                            table.insert(enemies, v)
                        end
                    end
                end
            end

            if #enemies > 0 then
                local target = enemies[1]
                local tRoot = target:FindFirstChild("HumanoidRootPart") or target.PrimaryPart
                local tHum = target:FindFirstChildOfClass("Humanoid")
                
                if tRoot and tHum then
                    local distToGuard = (root.Position - tRoot.Position).Magnitude
                    local lookDir = tRoot.CFrame.LookVector
                    local attackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, 6.5, 0)
                    
                    if distToGuard > 15 then PathTransport(attackPos, 40, 20) end
                    if tRoot.Size.X < 15 then tRoot.Size = Vector3.new(15, 15, 15) tRoot.CanCollide = false end
                    if CheckHPAndFailsafe(root, hum, tRoot.Position) then continue end
                    
                    if not _G.F2H2HoverHeight then _G.F2H2HoverHeight = 6.5 end
                    if not _G.PlayerLastHpF2H2 then _G.PlayerLastHpF2H2 = hum.Health end
                    if hum.Health < _G.PlayerLastHpF2H2 then _G.F2H2DodgeEndTime = tick() + 0.8 end
                    _G.PlayerLastHpF2H2 = hum.Health
                    
                    if not _G.F2H2LastHp then _G.F2H2LastHp = tHum.Health end
                    if tHum.Health < _G.F2H2LastHp then
                        _G.F2H2LastHp = tHum.Health; _G.F2H2LastHitTime = tick()
                    end
                    if tick() - (_G.F2H2LastHitTime or tick()) > 1.5 then _G.F2H2HoverHeight = math.max(4.0, _G.F2H2HoverHeight - 0.1); _G.F2H2LastHitTime = tick() end

                    local currentDodgeOffset = (tick() < (_G.F2H2DodgeEndTime or 0)) and 2 or 0
                    local actualHeight = _G.F2H2HoverHeight + currentDodgeOffset
                    local finalAttackPos = tRoot.Position - (lookDir * 3) + Vector3.new(0, actualHeight, 0)
                    local targetRot = CFrame.lookAt(finalAttackPos, Vector3.new(tRoot.Position.X, finalAttackPos.Y, tRoot.Position.Z)) * CFrame.Angles(math.rad(-60), 0, 0)
                    
                    ToggleHover(true)
                    local bp = root:FindFirstChild("RyuHover")
                    if bp then bp.Position = finalAttackPos end
                    local bg = root:FindFirstChild("RyuGyroVera")
                    if bg then bg.CFrame = targetRot end

                    EquipTargetWeapon()
                    PerformMeleeAttack(enemies) 
                end
            else
                task.wait(0.1)
            end
            continue
        end

        if _G.ImpelState == "Floor2Done" then
            task.wait(1)
        end
    end
end)

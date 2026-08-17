--// ==========================================
--// RYUHUB: GRAND PIECE ONLINE (CLEANED & PORTED)
--// ==========================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

--// GUI SECURITY & CLEANUP
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function() if gethui then guiParent = gethui() elseif syn and syn.protect_gui then guiParent = CoreGui end end)
for _, v in pairs(guiParent:GetChildren()) do if v.Name == "RyuHubGPO" then v:Destroy() end end

--// THEME SETTINGS
local Theme = {
    Background = Color3.fromRGB(12, 12, 14),
    Sidebar = Color3.fromRGB(18, 18, 20),
    SectionBG = Color3.fromRGB(24, 24, 26),
    Text = Color3.fromRGB(250, 250, 250),
    SubText = Color3.fromRGB(130, 130, 135),
    Accent = Color3.fromRGB(0, 150, 255),
    ToggleOff = Color3.fromRGB(35, 35, 38),
    ToggleOn = Color3.fromRGB(255, 255, 255),
    Stroke = Color3.fromRGB(45, 45, 50)
}

--// GPO DATA EXTRACTED FROM DECOMPILED SCRIPT
local Islands = { 
    ["Town of Beginnings"] = CFrame.new(-528, 5, -3423), 
    ["Shell's Town"] = CFrame.new(-1299, 4, -5052), 
    ["Sandora"] = CFrame.new(-1545, 4, -3353), 
    ["Orange Town"] = CFrame.new(-4448, 5, -6638), 
    ["Restaurant Baratie"] = CFrame.new(-2964, 6, -6672), 
    ["Logue Town"] = CFrame.new(-6589, 7, -7643), 
    ["Roca Island"] = CFrame.new(1564, 154, -6598), 
    ["Shark Park"] = CFrame.new(-1572, 11, -10082), 
    ["Reverse Mountain"] = CFrame.new(-8030, 17, -8785), 
    ["Sphinx Island"] = CFrame.new(-4006, 41, -9138), 
    ["Fishman Island"] = CFrame.new(7996, -2154, -17075), 
    ["Marine Fort F-1"] = CFrame.new(393, 18, -4467), 
    ["Marine Base G-1"] = CFrame.new(-5979, 57, -11496), 
    ["Colosseum"] = CFrame.new(-2020, 7, -7675), 
    ["Hell"] = CFrame.new(18944, 8122, -12501) 
}

local IslandNames = {}
for name, _ in pairs(Islands) do table.insert(IslandNames, name) end

getgenv().AntiStun = false
getgenv().ChestEspEnabled = false
getgenv().PlayerEspEnabled = false
getgenv().AutoJoinPS = false
getgenv().PsCode = ""

--// MAIN GUI BUILDER
local RyuHub = Instance.new("ScreenGui")
RyuHub.Name = "RyuHubGPO"
RyuHub.ResetOnSpawn = false
RyuHub.Parent = guiParent

local MainFrame = Instance.new("Frame", RyuHub)
MainFrame.Size = UDim2.new(0, 650, 0, 400)
MainFrame.Position = UDim2.new(0.5, -325, 0.5, -200)
MainFrame.BackgroundColor3 = Theme.Background
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", MainFrame).Color = Theme.Stroke

local Topbar = Instance.new("Frame", MainFrame)
Topbar.Size = UDim2.new(1, 0, 0, 50)
Topbar.BackgroundTransparency = 1

local Title = Instance.new("TextLabel", Topbar)
Title.Size = UDim2.new(0, 300, 1, 0)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "RYU HUB | GRAND PIECE ONLINE"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 18
Title.TextColor3 = Theme.Accent
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Dragging Logic
local dragging, dragInput, dragStart, startPos
Topbar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true; dragStart = input.Position; startPos = MainFrame.Position
    end
end)
Topbar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then dragInput = input end
end)
UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
end)

local Sidebar = Instance.new("ScrollingFrame", MainFrame)
Sidebar.Size = UDim2.new(0, 150, 1, -60)
Sidebar.Position = UDim2.new(0, 10, 0, 50)
Sidebar.BackgroundTransparency = 1
Sidebar.ScrollBarThickness = 0
local SideLayout = Instance.new("UIListLayout", Sidebar)
SideLayout.Padding = UDim.new(0, 5)

local ContentContainer = Instance.new("Frame", MainFrame)
ContentContainer.Size = UDim2.new(1, -180, 1, -60)
ContentContainer.Position = UDim2.new(0, 170, 0, 50)
ContentContainer.BackgroundTransparency = 1

--// UI ELEMENTS
local function CreateTabButton(name, targetPage)
    local btn = Instance.new("TextButton", Sidebar)
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Theme.Sidebar
    btn.Text = "  " .. name
    btn.TextColor3 = Theme.SubText
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    btn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    
    btn.MouseButton1Click:Connect(function()
        for _, page in pairs(ContentContainer:GetChildren()) do
            if page:IsA("ScrollingFrame") then page.Visible = (page.Name == targetPage.Name) end
        end
        for _, otherBtn in pairs(Sidebar:GetChildren()) do
            if otherBtn:IsA("TextButton") then
                otherBtn.TextColor3 = Theme.SubText
                otherBtn.BackgroundColor3 = Theme.Sidebar
            end
        end
        btn.TextColor3 = Theme.Text
        btn.BackgroundColor3 = Theme.SectionBG
    end)
    return btn
end

local function CreatePage(name)
    local page = Instance.new("ScrollingFrame", ContentContainer)
    page.Name = name
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = 4
    page.Visible = false
    local layout = Instance.new("UIListLayout", page)
    layout.Padding = UDim.new(0, 8)
    layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        page.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 10)
    end)
    return page
end

local function CreateToggle(page, text, state, callback)
    local frame = Instance.new("Frame", page)
    frame.Size = UDim2.new(0.95, 0, 0, 35)
    frame.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.7, 0, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Theme.Text
    label.Font = Enum.Font.GothamMedium
    label.TextSize = 13
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 40, 0, 20)
    btn.Position = UDim2.new(1, -50, 0.5, -10)
    btn.BackgroundColor3 = state and Theme.ToggleOn or Theme.ToggleOff
    btn.Text = ""
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)
    
    local circle = Instance.new("Frame", btn)
    circle.Size = UDim2.new(0, 14, 0, 14)
    circle.Position = state and UDim2.new(1, -17, 0.5, -7) or UDim2.new(0, 3, 0.5, -7)
    circle.BackgroundColor3 = Theme.Background
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    
    local isOn = state
    btn.MouseButton1Click:Connect(function()
        isOn = not isOn
        btn.BackgroundColor3 = isOn and Theme.ToggleOn or Theme.ToggleOff
        circle.Position = isOn and UDim2.new(1, -17, 0.5, -7) or UDim2.new(0, 3, 0.5, -7)
        if callback then callback(isOn) end
    end)
end

local function CreateTextBox(page, text, callback)
    local frame = Instance.new("Frame", page)
    frame.Size = UDim2.new(0.95, 0, 0, 35)
    frame.BackgroundColor3 = Theme.SectionBG
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    
    local box = Instance.new("TextBox", frame)
    box.Size = UDim2.new(1, -20, 1, 0)
    box.Position = UDim2.new(0, 10, 0, 0)
    box.BackgroundTransparency = 1
    box.PlaceholderText = text
    box.Text = ""
    box.TextColor3 = Theme.Text
    box.Font = Enum.Font.GothamMedium
    box.TextSize = 13
    box.TextXAlignment = Enum.TextXAlignment.Left
    
    box.FocusLost:Connect(function()
        if callback then callback(box.Text) end
    end)
end

local function CreateDropdown(page, text, items, callback)
    local frame = Instance.new("Frame", page)
    frame.Size = UDim2.new(0.95, 0, 0, 35)
    frame.BackgroundColor3 = Theme.SectionBG
    frame.ClipsDescendants = true
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1, -20, 0, 35)
    btn.Position = UDim2.new(0, 10, 0, 0)
    btn.BackgroundTransparency = 1
    btn.Text = text .. " (Click to open)"
    btn.TextColor3 = Theme.Text
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 13
    btn.TextXAlignment = Enum.TextXAlignment.Left
    
    local listLayout = Instance.new("UIListLayout", frame)
    listLayout.Padding = UDim.new(0, 2)
    listLayout.SortOrder = Enum.SortOrder.LayoutOrder
    
    local isOpen = false
    btn.MouseButton1Click:Connect(function()
        isOpen = not isOpen
        frame.Size = isOpen and UDim2.new(0.95, 0, 0, 35 + (#items * 25)) or UDim2.new(0.95, 0, 0, 35)
    end)
    
    for _, item in ipairs(items) do
        local itemBtn = Instance.new("TextButton", frame)
        itemBtn.Size = UDim2.new(1, -20, 0, 25)
        itemBtn.Position = UDim2.new(0, 10, 0, 0)
        itemBtn.BackgroundTransparency = 1
        itemBtn.Text = "- " .. item
        itemBtn.TextColor3 = Theme.SubText
        itemBtn.Font = Enum.Font.Gotham
        itemBtn.TextSize = 12
        itemBtn.TextXAlignment = Enum.TextXAlignment.Left
        
        itemBtn.MouseButton1Click:Connect(function()
            btn.Text = text .. ": " .. item
            isOpen = false
            frame.Size = UDim2.new(0.95, 0, 0, 35)
            if callback then callback(item) end
        end)
    end
end

--// PAGES & LOGIC BINDINGS
local PageMain = CreatePage("Main")
local PageESP = CreatePage("ESP")
local PageTeleport = CreatePage("Teleport")

CreateTabButton("Main / Farm", PageMain)
CreateTabButton("ESP Visuals", PageESP)
CreateTabButton("Teleports", PageTeleport)

-- Main Tab
CreateToggle(PageMain, "Enable Anti-Stun", false, function(state)
    getgenv().AntiStun = state
end)

CreateToggle(PageMain, "Auto Join Private Server", false, function(state)
    getgenv().AutoJoinPS = state
end)

CreateTextBox(PageMain, "Enter Private Server Code...", function(val)
    getgenv().PsCode = val
end)

-- Teleport Tab
CreateDropdown(PageTeleport, "Select Island to Teleport", IslandNames, function(islandName)
    local targetCFrame = Islands[islandName]
    if targetCFrame and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        -- Simple Tween Teleport from the original script
        local dist = (hrp.Position - targetCFrame.Position).Magnitude
        local tweenTime = dist / 150 -- Adjust speed as needed
        local tween = TweenService:Create(hrp, TweenInfo.new(tweenTime, Enum.EasingStyle.Linear), {CFrame = targetCFrame})
        tween:Play()
    end
end)

-- ESP Tab
CreateToggle(PageESP, "Chest ESP", false, function(state)
    getgenv().ChestEspEnabled = state
end)

CreateToggle(PageESP, "Player ESP", false, function(state)
    getgenv().PlayerEspEnabled = state
end)

-- Set first page visible
PageMain.Visible = true

--// GPO LOGIC LOOPS (Cleaned up from decompiled script)

-- Anti Stun Loop
LocalPlayer.CharacterAdded:Connect(function(char)
    char.DescendantAdded:Connect(function(desc)
        if not getgenv().AntiStun then return end
        task.wait()
        if desc.Name == "Stun" or desc.Name == "StunFolder" then
            desc:Destroy()
        end
        if (desc:IsA("BodyPosition") or desc:IsA("BodyVelocity")) and desc.Name ~= "geppo" and desc.Name ~= "rolling" then
            desc:Destroy()
        end
    end)
end)

if LocalPlayer.Character then
    LocalPlayer.Character.DescendantAdded:Connect(function(desc)
        if not getgenv().AntiStun then return end
        task.wait()
        if desc.Name == "Stun" or desc.Name == "StunFolder" then
            desc:Destroy()
        end
        if (desc:IsA("BodyPosition") or desc:IsA("BodyVelocity")) and desc.Name ~= "geppo" and desc.Name ~= "rolling" then
            desc:Destroy()
        end
    end)
end

-- Auto Join PS Loop
task.spawn(function()
    while task.wait(1) do
        if getgenv().AutoJoinPS and getgenv().PsCode ~= "" then
            pcall(function()
                local reserveEvent = ReplicatedStorage:WaitForChild("Events"):WaitForChild("reserved")
                reserveEvent:InvokeServer(getgenv().PsCode)
                
                if LocalPlayer.PlayerGui:FindFirstChild("chooseType") then
                    LocalPlayer.PlayerGui.chooseType.Frame.RemoteEvent:FireServer(true)
                end
                if LocalPlayer.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                    LocalPlayer.PlayerGui.ConfirmationPrompt.RemoteEvent:FireServer("First Sea")
                end
            end)
        end
    end
end)

-- Simple ESP Loop
task.spawn(function()
    while task.wait(1) do
        if getgenv().PlayerEspEnabled then
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                    if not player.Character.Head:FindFirstChild("RyuESP") then
                        local esp = Instance.new("BillboardGui")
                        esp.Name = "RyuESP"
                        esp.AlwaysOnTop = true
                        esp.Size = UDim2.new(0, 100, 0, 50)
                        esp.StudsOffset = Vector3.new(0, 2, 0)
                        esp.Parent = player.Character.Head
                        
                        local label = Instance.new("TextLabel", esp)
                        label.Size = UDim2.new(1, 0, 1, 0)
                        label.BackgroundTransparency = 1
                        label.TextColor3 = Color3.new(1, 0, 0)
                        label.TextStrokeTransparency = 0
                        label.Font = Enum.Font.GothamBold
                        label.TextSize = 14
                        
                        task.spawn(function()
                            while esp.Parent and player.Character do
                                label.Text = player.Name .. " | " .. math.floor((LocalPlayer.Character.HumanoidRootPart.Position - player.Character.Head.Position).Magnitude) .. "m"
                                task.wait(0.5)
                            end
                        end)
                    end
                end
            end
        else
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character and player.Character:FindFirstChild("Head") and player.Character.Head:FindFirstChild("RyuESP") then
                    player.Character.Head.RyuESP:Destroy()
                end
            end
        end
    end
end)

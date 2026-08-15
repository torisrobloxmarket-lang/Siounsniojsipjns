--// ============================================================================
--// MURASAKI 紫 - LITE EDITION (NO LOADSTRINGS / NO WEBHOOKS)
--// ============================================================================

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

--// 1. GUI CLEANUP & SETUP
local guiParent = CoreGui
pcall(function() 
    if type(gethui) == "function" then guiParent = gethui() 
    elseif syn and syn.protect_gui then guiParent = CoreGui 
    else guiParent = LocalPlayer:WaitForChild("PlayerGui") end 
end)

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "MurasakiLite" then v:Destroy() end 
end

local MurasakiUI = Instance.new("ScreenGui", guiParent)
MurasakiUI.Name = "MurasakiLite"
MurasakiUI.ResetOnSpawn = false

local MainFrame = Instance.new("Frame", MurasakiUI)
MainFrame.Size = UDim2.new(0, 350, 0, 420)
MainFrame.Position = UDim2.new(0.5, -175, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", MainFrame).Color = Color3.fromRGB(150, 0, 255)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "Murasaki 紫 - Lite"
Title.TextColor3 = Color3.fromRGB(200, 150, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

local Container = Instance.new("ScrollingFrame", MainFrame)
Container.Size = UDim2.new(1, -20, 1, -50)
Container.Position = UDim2.new(0, 10, 0, 40)
Container.BackgroundTransparency = 1
Container.ScrollBarThickness = 4
local Layout = Instance.new("UIListLayout", Container)
Layout.Padding = UDim.new(0, 8)
Layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

--// 2. UI HELPER FUNCTIONS
local function CreateToggle(text, callback)
    local btn = Instance.new("TextButton", Container)
    btn.Size = UDim2.new(1, -10, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    btn.Text = "  [OFF] " .. text
    btn.TextColor3 = Color3.fromRGB(200, 200, 200)
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 14
    btn.TextXAlignment = Enum.TextXAlignment.Left
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    
    local state = false
    btn.MouseButton1Click:Connect(function()
        state = not state
        btn.Text = (state and "  [ON] " or "  [OFF] ") .. text
        btn.BackgroundColor3 = state and Color3.fromRGB(100, 50, 200) or Color3.fromRGB(35, 35, 40)
        pcall(function() callback(state) end)
    end)
end

local function CreateTextBox(placeholder, callback)
    local box = Instance.new("TextBox", Container)
    box.Size = UDim2.new(1, -10, 0, 35)
    box.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
    box.PlaceholderText = placeholder
    box.Text = ""
    box.TextColor3 = Color3.fromRGB(255, 255, 255)
    box.Font = Enum.Font.Gotham
    box.TextSize = 14
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    Instance.new("UIStroke", box).Color = Color3.fromRGB(100, 50, 200)
    
    box.FocusLost:Connect(function()
        pcall(function() callback(box.Text) end)
    end)
end

--// ==========================================
--// 3. LOGIC VARIABLES
--// ==========================================
local Config = {
    Knockback = false,
    Hitsphere = false,
    Chams = false,
    Fly = false,
    AntiBlackHole = false,
    MoveBlackHole = false,
    Bring = false,
    TargetName = ""
}

--// ==========================================
--// 4. FEATURES & MODULES
--// ==========================================

-- A. Knockback Override
local knockbackVelocity = Vector3.new(300, 200, 300)
local maxForce = Vector3.new(10000, 10000, 10000)
local power = 10000

local function overrideKnockback(obj)
    if not Config.Knockback then return end
    if obj:IsA("BodyVelocity") and obj.Name == "KnockbackForce" then
        obj.Velocity = knockbackVelocity
        obj.MaxForce = maxForce
        obj.P = power
    end
end

Workspace.DescendantAdded:Connect(overrideKnockback)
RunService.Heartbeat:Connect(function()
    if Config.Knockback then
        for _, obj in ipairs(Workspace:GetDescendants()) do
            overrideKnockback(obj)
        end
    end
end)

-- B. Hitbox Expander (Hitsphere)
RunService.Heartbeat:Connect(function()
    if Config.Hitsphere then
        local effects = Workspace:FindFirstChild("Effects")
        if effects then
            for _, obj in ipairs(effects:GetDescendants()) do
                if obj.Name == "Hitsphere" then
                    obj.Size = Vector3.new(40, 40, 40)
                end
            end
        end
    end
end)

-- C. Item Chams
local chamFolder = Workspace:FindFirstChild("ItemChams") or Instance.new("Folder", Workspace)
chamFolder.Name = "ItemChams"
local activeChams = {}

local function clearAllChams()
    chamFolder:ClearAllChildren()
    activeChams = {}
end

RunService.RenderStepped:Connect(function()
    if not Config.Chams then 
        if #chamFolder:GetChildren() > 0 then clearAllChams() end
        return 
    end
    
    local items = Workspace:FindFirstChild("Items")
    if items then
        for _, part in ipairs(items:GetDescendants()) do
            if part:IsA("BasePart") then
                if not activeChams[part] then
                    local cham = Instance.new("BoxHandleAdornment")
                    cham.Name = "Cham"
                    cham.Adornee = part
                    cham.AlwaysOnTop = true
                    cham.ZIndex = 5
                    cham.Size = part.Size + Vector3.new(0.1, 0.1, 0.1)
                    cham.Transparency = 0.5
                    cham.Color3 = Color3.fromRGB(0, 255, 0)
                    cham.Parent = chamFolder
                    activeChams[part] = cham
                end
            end
        end
    end
end)

-- D. Flight System
local flying = false
local bodyVelocity, bodyGyro, flightConnection
local FLIGHT_SPEED = 60
local SMOOTHNESS = 0.12
local currentVelocity = Vector3.zero

local function getTargetVelocity()
    local moveVec = Vector3.zero
    local camCFrame = Workspace.CurrentCamera.CFrame
    if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveVec = moveVec + camCFrame.LookVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveVec = moveVec - camCFrame.LookVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveVec = moveVec - camCFrame.RightVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveVec = moveVec + camCFrame.RightVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveVec = moveVec + camCFrame.UpVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then moveVec = moveVec - camCFrame.UpVector end
    if moveVec.Magnitude > 0 then moveVec = moveVec.Unit * FLIGHT_SPEED end
    return moveVec
end

local function toggleFlight()
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end

    flying = not flying
    if flying then
        bodyVelocity = Instance.new("BodyVelocity", root)
        bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
        bodyVelocity.P = 1e4
        bodyVelocity.Velocity = Vector3.zero

        bodyGyro = Instance.new("BodyGyro", root)
        bodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
        bodyGyro.P = 2e4
        bodyGyro.CFrame = Workspace.CurrentCamera.CFrame

        currentVelocity = Vector3.zero
        flightConnection = RunService.RenderStepped:Connect(function(dt)
            local targetVel = getTargetVelocity()
            currentVelocity = currentVelocity:Lerp(targetVel, 1 - math.exp(-dt / SMOOTHNESS))
            bodyVelocity.Velocity = currentVelocity
            bodyGyro.CFrame = CFrame.new(root.Position, root.Position + Workspace.CurrentCamera.CFrame.LookVector)
        end)
    else
        if flightConnection then flightConnection:Disconnect() flightConnection = nil end
        if bodyVelocity then bodyVelocity:Destroy() bodyVelocity = nil end
        if bodyGyro then bodyGyro:Destroy() bodyGyro = nil end
    end
end

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.Y and Config.Fly then
        toggleFlight()
    end
end)

-- E. Target Bring (Teleport behind Target)
local teleportLooping = false
local teleportTarget = nil

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.One and Config.Bring then
        if not teleportLooping then
            -- Find Target by Name
            teleportTarget = nil
            for _, plr in pairs(Players:GetPlayers()) do
                if string.lower(plr.Name):find(string.lower(Config.TargetName)) or string.lower(plr.DisplayName):find(string.lower(Config.TargetName)) then
                    teleportTarget = plr
                    break
                end
            end
            
            if teleportTarget then
                teleportLooping = true
                task.delay(3, function() teleportLooping = false end)
            end
        end
    end
end)

task.spawn(function()
    while true do
        if teleportLooping and teleportTarget and teleportTarget.Character and teleportTarget.Character:FindFirstChild("HumanoidRootPart") then
            local myHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if myHRP then
                myHRP.CFrame = teleportTarget.Character.HumanoidRootPart.CFrame * CFrame.new(0, 1, -3)
            end
        end
        task.wait(0.1)
    end
end)

-- F. Anti Black Hole
local antiSuck = Instance.new("BodyVelocity")
antiSuck.Name = "AntiSuck"
antiSuck.MaxForce = Vector3.new(1e5, 0, 1e5)
antiSuck.P = 1250
antiSuck.Velocity = Vector3.zero

RunService.RenderStepped:Connect(function()
    local blackHole = Workspace:FindFirstChild("BlackHole", true)
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    
    if Config.AntiBlackHole and blackHole and root and hum then
        local distance = (root.Position - blackHole.Position).Magnitude
        if distance < 50 then
            antiSuck.Parent = root
            local desiredVelocity = hum.MoveDirection * (hum.WalkSpeed + 30)
            local actualVelocity = root.AssemblyLinearVelocity
            local horizontalActual = Vector3.new(actualVelocity.X, 0, actualVelocity.Z)
            local externalInfluence = horizontalActual - desiredVelocity
            antiSuck.Velocity = antiSuck.Velocity:Lerp(-externalInfluence, 0.25)
        else
            antiSuck.Parent = nil
        end
    else
        antiSuck.Parent = nil
    end
end)

-- G. Move During Black Hole (Garuda Rebound)
local blackholeFlying = false
local bhFlyConnection, bhGyro, bhVelocity
local bhSpeed = 50

local function bhStartFlying()
    local char = LocalPlayer.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not root or not hum or blackholeFlying then return end

    blackholeFlying = true
    hum.PlatformStand = true

    bhGyro = Instance.new("BodyGyro", root)
    bhGyro.P = 9e4; bhGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
    
    bhVelocity = Instance.new("BodyVelocity", root)
    bhVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)

    bhFlyConnection = RunService.RenderStepped:Connect(function()
        local moveDirection = Vector3.zero
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDirection = moveDirection + Vector3.new(0, 0, -1) end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDirection = moveDirection + Vector3.new(0, 0, 1) end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDirection = moveDirection + Vector3.new(-1, 0, 0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDirection = moveDirection + Vector3.new(1, 0, 0) end

        local cam = Workspace.CurrentCamera
        local direction = cam.CFrame:VectorToWorldSpace(moveDirection.Unit * bhSpeed)
        bhVelocity.Velocity = moveDirection.Magnitude > 0 and direction or Vector3.zero
        bhGyro.CFrame = cam.CFrame
    end)
end

local function bhStopFlying()
    if not blackholeFlying then return end
    blackholeFlying = false
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end

    if bhFlyConnection then bhFlyConnection:Disconnect() bhFlyConnection = nil end
    if bhGyro then bhGyro:Destroy() end
    if bhVelocity then bhVelocity:Destroy() end
end

UserInputService.InputBegan:Connect(function(input, processed)
    if not processed and input.KeyCode == Enum.KeyCode.G and Config.MoveBlackHole then
        local char = LocalPlayer.Character
        local moveset = char and char:FindFirstChild("Moveset")
        if moveset and moveset:FindFirstChild("Garuda Rebound") then
            -- Clean original BodyPosition applied by the game
            task.delay(0.1, function()
                local root = char:FindFirstChild("HumanoidRootPart")
                if root then
                    for _, v in pairs(root:GetChildren()) do
                        if v:IsA("BodyPosition") then v:Destroy() end
                    end
                end
            end)
            
            bhStartFlying()
            task.delay(1.5, function() bhStopFlying() end)
        end
    end
end)

--// ==========================================
--// 5. ATTACH TO UI
--// ==========================================
CreateToggle("Super Knockback (All Moves)", function(state) Config.Knockback = state end)
CreateToggle("Hitbox Expander (40x40x40)", function(state) Config.Hitsphere = state end)
CreateTextBox("Enter Bring Target Name...", function(text) Config.TargetName = text end)
CreateToggle("Enable Bring (Press 1 with Todo)", function(state) Config.Bring = state end)
CreateToggle("Flight (Press Y to toggle)", function(state) Config.Fly = state end)
CreateToggle("Move during Black Hole", function(state) Config.MoveBlackHole = state end)
CreateToggle("Anti Black Hole (Ignore Pull)", function(state) Config.AntiBlackHole = state end)
CreateToggle("Item Chams (ESP)", function(state) Config.Chams = state end)

-- Resize Container based on contents
task.delay(0.5, function()
    Container.CanvasSize = UDim2.new(0, 0, 0, Layout.AbsoluteContentSize.Y + 20)
end)

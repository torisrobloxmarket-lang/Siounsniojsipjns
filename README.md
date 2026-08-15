--// ==========================================
--// RYU HUB - INVISIBLE TEST GUI (10 VARIANTEN)
--// ==========================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

-- Altes Test-GUI löschen
local guiParent = LocalPlayer:FindFirstChild("PlayerGui")
if not guiParent then guiParent = game.CoreGui end
for _, v in pairs(guiParent:GetChildren()) do
    if v.Name == "InvisTestGui" then v:Destroy() end
end

-- GUI Setup
local sg = Instance.new("ScreenGui", guiParent)
sg.Name = "InvisTestGui"
sg.ResetOnSpawn = false

local frame = Instance.new("Frame", sg)
frame.Size = UDim2.new(0, 300, 0, 450)
frame.Position = UDim2.new(0, 50, 0.5, -225)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 8)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "Invisible Test (10 Variants)"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local scroll = Instance.new("ScrollingFrame", frame)
scroll.Size = UDim2.new(1, 0, 1, -40)
scroll.Position = UDim2.new(0, 0, 0, 40)
scroll.BackgroundTransparency = 1
scroll.ScrollBarThickness = 4
local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0, 5)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Helper zum Aufräumen von Loops
local activeConnections = {}
local function clearLoops()
    for _, conn in pairs(activeConnections) do conn:Disconnect() end
    activeConnections = {}
    print("Loops cleared.")
end

local function makeButton(name, func)
    local btn = Instance.new("TextButton", scroll)
    btn.Size = UDim2.new(0.9, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.Text = name
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    
    btn.MouseButton1Click:Connect(function()
        clearLoops()
        local char = LocalPlayer.Character
        if not char then return end
        print("Testing: " .. name)
        pcall(func, char)
    end)
end

--// ==========================================
--// DIE 10 INVISIBLE VARIANTEN
--// ==========================================

-- 1. CFrame Desync (Der Klassiker: Server sieht dich im Himmel, du siehst dich am Boden)
makeButton("1. CFrame Desync (Stepped/Render)", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local fakePos = root.CFrame * CFrame.new(0, 9999, 0)
    local realPos = root.CFrame
    
    table.insert(activeConnections, RunService.Stepped:Connect(function()
        root.CFrame = fakePos
    end))
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        root.CFrame = realPos
    end))
end)

-- 2. RootJoint Break (Trennt Unterkörper vom Root)
makeButton("2. RootJoint Detach", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    local lowerTorso = char:FindFirstChild("LowerTorso") or char:FindFirstChild("Torso")
    if root and lowerTorso then
        local joint = lowerTorso:FindFirstChild("Root") or lowerTorso:FindFirstChild("RootJoint")
        if joint then joint:Destroy() end
        root.CFrame = CFrame.new(0, 99999, 0)
    end
end)

-- 3. Velocity Desync (Verwirrt die Server-Physik)
makeButton("3. High Velocity Spam", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        local old = root.Velocity
        root.Velocity = Vector3.new(0, 9999999, 0)
        RunService.RenderStepped:Wait()
        root.Velocity = old
    end))
end)

-- 4. TJS Specific: Megumi Shadow Sink Remote
makeButton("4. TJS: Megumi Remote Spam", function(char)
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        pcall(function()
            ReplicatedStorage.Knit.Knit.Services.MegumiService.RE.RightActivated:FireServer()
        end)
    end))
end)

-- 5. TJS Specific: Attribute Spoofer (Dein Leak)
makeButton("5. TJS: HidePlayer Attribute", function(char)
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        LocalPlayer:SetAttribute("HidePlayer", true)
        char:SetAttribute("HidePlayer", true)
        char:SetAttribute("Hidden", true)
    end))
end)

-- 6. TJS Specific: Garuda Clone / Ragdoll State
makeButton("6. TJS: Ragdoll State Desync", function(char)
    char:SetAttribute("Ragdoll", 999)
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        char:SetAttribute("Ragdoll", 999)
    end))
end)

-- 7. Fake Character Clone (Vollständiger Client-Clone)
makeButton("7. Fake Character Clone", function(char)
    char.Archivable = true
    local clone = char:Clone()
    clone.Name = "LocalClone"
    clone.Parent = Workspace
    camera.CameraSubject = clone:FindFirstChild("Humanoid")
    
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then root.CFrame = CFrame.new(0, 99999, 0) end
end)

-- 8. Visual Deletion (Zerstört Meshes/Decals für den Server)
makeButton("8. Delete Meshes/Faces", function(char)
    for _, v in pairs(char:GetDescendants()) do
        if v:IsA("MeshPart") or v:IsA("Decal") or v:IsA("Texture") or v:IsA("Accessory") then
            v:Destroy()
        end
    end
end)

-- 9. FallenPartsDestroyHeight Bypass (Sitzt auf Höhe -499)
makeButton("9. Void Hover (-490 Y)", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        root.CFrame = CFrame.new(root.Position.X, -490, root.Position.Z)
        root.Velocity = Vector3.new(0,0,0)
    end))
end)

-- 10. Kombi: TJS Attributes + CFrame Desync
makeButton("10. Combo: Attr + CFrame", function(char)
    LocalPlayer:SetAttribute("HidePlayer", true)
    char:SetAttribute("HidePlayer", true)
    char:SetAttribute("Hidden", true)
    
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local fakePos = root.CFrame * CFrame.new(0, 9999, 0)
    local realPos = root.CFrame
    
    table.insert(activeConnections, RunService.Stepped:Connect(function()
        root.CFrame = fakePos
    end))
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        root.CFrame = realPos
    end))
end)

makeButton("--- RESET (Clear Loops) ---", function()
    clearLoops()
end)

-- Canvas Size anpassen
task.defer(function()
    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 20)
end)

-- Custom DOORS Hub (Clean Rewrite / No External Framework Dependencies)
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- Altes GUI entfernen, falls vorhanden
if CoreGui:FindFirstChild("CleanDoorsHub") then
    CoreGui.CleanDoorsHub:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CleanDoorsHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

-- Hauptfenster
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 320, 0, 390)
MainFrame.Position = UDim2.new(0.5, -160, 0.3, -195)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- Titelleiste
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 10)
TitleCorner.Parent = TitleBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -20, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "DOORS - Custom Built Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 14
Title.Font = Enum.Font.GothamBold
Title.Parent = TitleBar

-- Scrollbarer Inhaltsbereich
local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, -20, 1, -55)
ScrollingFrame.Position = UDim2.new(0, 10, 0, 48)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 450)
ScrollingFrame.ScrollBarThickness = 3
ScrollingFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Parent = ScrollingFrame

-- Helper-Funktionen für UI-Elemente
local function createToggle(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    btn.TextColor3 = Color3.fromRGB(220, 220, 220)
    btn.TextSize = 13
    btn.Font = Enum.Font.Gotham
    btn.Text = text .. ": OFF"
    btn.Parent = ScrollingFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    local state = false
    btn.MouseButton1Click:Connect(function()
        state = not state
        btn.Text = text .. (state and ": ON" or ": OFF")
        btn.BackgroundColor3 = state and Color3.fromRGB(0, 110, 200) or Color3.fromRGB(35, 35, 35)
        callback(state)
    end)
end

local function createButton(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 13
    btn.Font = Enum.Font.Gotham
    btn.Text = text
    btn.Parent = ScrollingFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    btn.MouseButton1Click:Connect(callback)
end

-- ==========================================
-- SAUBER NACHGEBAUTE FUNKTIONEN
-- ==========================================

-- 1. Fullbright
createToggle("Fullbright", function(value)
    if value then
        Lighting.Ambient = Color3.new(1, 1, 1)
    else
        Lighting.Ambient = Color3.fromRGB(67, 51, 56)
    end
end)

-- 2. Noclip
createToggle("Noclip", function(value)
    RunService.Stepped:Connect(function()
        local char = LocalPlayer.Character
        if char and value then
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end)
end)

-- 3. Speed Boost
createToggle("Speed Boost", function(value)
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then
        hum.WalkSpeed = value and 22 or 15
    end
end)

-- 4. Door Reach
createToggle("Door Reach", function(value)
    task.spawn(function()
        while value do
            task.wait(0.2)
            pcall(function()
                local latestRoom = ReplicatedStorage:FindFirstChild("GameData") and ReplicatedStorage.GameData:FindFirstChild("LatestRoom")
                if latestRoom and Workspace.CurrentRooms:FindFirstChild(latestRoom.Value) then
                    local door = Workspace.CurrentRooms[latestRoom.Value]:FindFirstChild("Door")
                    if door and door:FindFirstChild("ClientOpen") then
                        door.ClientOpen:FireServer()
                    end
                end
            end)
        end
    end)
end)

-- 5. Anti-Eyes
createToggle("Anti-Eyes", function(value)
    task.spawn(function()
        while value do
            task.wait(0.1)
            pcall(function()
                if Workspace:FindFirstChild("Eyes") or Workspace:FindFirstChild("Lookman") then
                    local remotesFolder = ReplicatedStorage:FindFirstChild("RemotesFolder")
                    if remotesFolder and remotesFolder:FindFirstChild("MotorReplication") then
                        remotesFolder.MotorReplication:FireServer(0, -89, 0, false)
                    end
                end
            end)
        end
    end)
end)

-- 6. Disable Snares Button
createButton("Disable All Snares", function()
    pcall(function()
        for _, room in pairs(Workspace.CurrentRooms:GetChildren()) do
            if room:FindFirstChild("Assets") then
                for _, snare in pairs(room.Assets:GetChildren()) do
                    if snare.Name == "Snare" and snare:FindFirstChild("Hitbox") then
                        snare.Hitbox.CanTouch = false
                    end
                end
            end
        end
    end)
end)

-- 7. Revive Button
createButton("Instant Revive", function()
    pcall(function()
        local remotesFolder = ReplicatedStorage:FindFirstChild("RemotesFolder")
        if remotesFolder and remotesFolder:FindFirstChild("Revive") then
            remotesFolder.Revive:FireServer()
        end
    end)
end)

-- 8. GUI Schließen
createButton("Close Hub", function()
    ScreenGui:Destroy()
end)

print("[+] Clean DOORS Hub Loaded Successfully.")

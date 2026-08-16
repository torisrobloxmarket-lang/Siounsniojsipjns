-- Custom All-in-One GUI Hub für DOORS (Extrahiert aus clutch.lua)
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer

-- Altes GUI löschen, falls bereits vorhanden
if CoreGui:FindFirstChild("DoorsCustomHub") then
    CoreGui.DoorsCustomHub:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "DoorsCustomHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

-- Hauptfenster
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 360, 0, 440)
MainFrame.Position = UDim2.new(0.5, -180, 0.3, -220)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

-- Titel
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 45)
Title.BackgroundTransparency = 1
Title.Text = "DOORS - Custom Clutch Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame

-- Scrollable Liste für Buttons/Toggles
local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, -20, 1, -60)
ScrollingFrame.Position = UDim2.new(0, 10, 0, 50)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 650)
ScrollingFrame.ScrollBarThickness = 4
ScrollingFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Parent = ScrollingFrame

local function createToggle(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
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
        btn.BackgroundColor3 = state and Color3.fromRGB(0, 120, 215) or Color3.fromRGB(40, 40, 40)
        callback(state)
    end)
end

local function createButton(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
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
-- INTEGRIERTE FUNKTIONEN AUS DEM SKRIPT
-- ==========================================

-- 1. Fullbright (Helligkeit)
createToggle("Fullbright", function(value)
    if value then
        Lighting.Ambient = Color3.new(1, 1, 1)
    else
        Lighting.Ambient = Color3.fromRGB(67, 51, 56)
    end
end)

-- 2. Noclip (Durch Wände gehen)
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

-- 3. Speed Boost (WalkSpeed Erhöhung)
createToggle("Speed Boost (WalkSpeed)", function(value)
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then
        hum.WalkSpeed = value and 25 or 15
    end
end)

-- 4. Door Reach (Türen automatisch öffnen/fernaktivieren)
createToggle("Door Reach (Auto Client Open)", function(value)
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

-- 5. Anti-Eyes (Automatisch wegschauen bei Augen-Monstern)
createToggle("Anti-Eyes (Auto-Lookaway)", function(value)
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

-- 6. Anti-Snare (Fallen unsichtbar/harmlos machen)
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

-- 7. Twerk Emote (Animation abspielen)
createToggle("Twerk Animation", function(value)
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not hum then return end
    
    local twerkAnim = Instance.new("Animation")
    twerkAnim.AnimationId = "rbxassetid://12874447851"
    
    if not _G.twerkTrack then
        _G.twerkTrack = hum:LoadAnimation(twerkAnim)
    end
    
    if value then
        _G.twerkTrack:Play()
    else
        _G.twerkTrack:Stop()
    end
end)

-- 8. Sofortige Wiederbelebung (Revive)
createButton("Instant Revive", function()
    pcall(function()
        local remotesFolder = ReplicatedStorage:FindFirstChild("RemotesFolder")
        if remotesFolder and remotesFolder:FindFirstChild("Revive") then
            remotesFolder.Revive:FireServer()
        end
    end)
end)

-- GUI Schließen
createButton("Close GUI", function()
    ScreenGui:Destroy()
end)

print("[+] DOORS Custom Hub Loaded Successfully!")

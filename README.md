-- Delta Executor GUI: Invisibility Test GUI
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Altes GUI löschen, falls es schon offen ist
if CoreGui:FindFirstChild("InvisibleTestGui") then
    CoreGui.InvisibleTestGui:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "InvisibleTestGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 220, 0, 180)
MainFrame.Position = UDim2.new(0.5, -110, 0.4, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "JJS Invisibility GUI"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame

-- Invisibility Logik Variablen
local toggleState = false
local currentConnections = {}
local currentTrack = nil
local partsToSet = { "Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg" }

local function setTransparency(value, character)
    for _, partName in ipairs(partsToSet) do
        local part = character:FindFirstChild(partName)
        if part and part:IsA("BasePart") then
            part.Transparency = value
        end
    end
end

local function cleanup()
    for _, conn in pairs(currentConnections) do
        if conn then conn:Disconnect() end
    end
    currentConnections = {}

    if currentTrack and currentTrack.IsPlaying then
        currentTrack:Stop()
    end
end

local function setup(character)
    local humanoid = character:WaitForChild("Humanoid")
    local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)

    local animation = Instance.new("Animation")
    animation.AnimationId = "rbxassetid://83555998356899"
    currentTrack = animator:LoadAnimation(animation)
    currentTrack.Priority = Enum.AnimationPriority.Action4

    if toggleState then
        setTransparency(0.5, character)

        table.insert(currentConnections, RunService.Heartbeat:Connect(function()
            if not currentTrack.IsPlaying then
                currentTrack:Play()
            end
            currentTrack:AdjustSpeed(0)
            currentTrack.TimePosition = 10
        end))

        table.insert(currentConnections, RunService.RenderStepped:Connect(function()
            if currentTrack.IsPlaying then
                currentTrack:Stop()
            end
        end))
    else
        setTransparency(0, character)
    end
end

-- Toggle Button erstellen
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0.8, 0, 0, 45)
ToggleButton.Position = UDim2.new(0.1, 0, 0, 55)
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.TextSize = 14
ToggleButton.Font = Enum.Font.Gotham
ToggleButton.Text = "Invisibility: OFF"
ToggleButton.Parent = MainFrame

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 6)
BtnCorner.Parent = ToggleButton

ToggleButton.MouseButton1Click:Connect(function()
    toggleState = not toggleState
    ToggleButton.Text = toggleState and "Invisibility: ON" or "Invisibility: OFF"
    ToggleButton.BackgroundColor3 = toggleState and Color3.fromRGB(0, 120, 215) or Color3.fromRGB(50, 50, 50)

    local char = LocalPlayer.Character
    if char then
        cleanup()
        setup(char)
    end
end)

-- Schließen Button
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0.8, 0, 0, 35)
CloseButton.Position = UDim2.new(0.1, 0, 0, 115)
CloseButton.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 14
CloseButton.Font = Enum.Font.Gotham
CloseButton.Text = "Close GUI"
CloseButton.Parent = MainFrame

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseButton

CloseButton.MouseButton1Click:Connect(function()
    cleanup()
    ScreenGui:Destroy()
end)

print("[+] Custom Invisibility GUI Loaded Successfully.")

-- Delta Executor GUI Framework: Invisibility & Desync Test Suite
-- This script builds a simple test panel with buttons for the 3 theoretical methods.

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

-- Create GUI Container
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "JJS_TestSuite"
ScreenGui.Parent = CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 220, 0, 260)
MainFrame.Position = UDim2.new(0.5, -110, 0.4, -130)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "Invisibility Test Suite"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.SourceSansBold
Title.Parent = MainFrame

-- Helper function to create test buttons
local function createButton(name, yPos, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.8, 0, 0, 40)
    btn.Position = UDim2.new(0.1, 0, 0, yPos)
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 14
    btn.Font = Enum.Font.SourceSans
    btn.Text = name
    btn.Parent = MainFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    btn.MouseButton1Click:Connect(callback)
end

-- Method 1: State/Transparency Mask Test
createButton("Test 1: Visual Mask", 55, function()
    local char = localPlayer.Character
    if char then
        for _, obj in char:GetDescendants() do
            if obj:IsA("BasePart") or obj:IsA("MeshPart") then
                obj.Transparency = 1
            end
        end
        print("[Test 1] Visual mask applied.")
    end
end)

-- Method 2: Position Offset / Stash Test
createButton("Test 2: Position Offset", 110, function()
    local char = localPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame + Vector3.new(0, -100, 0)
        print("[Test 2] Position offset triggered.")
    end
end)

-- Method 3: Humanoid State Force Test
createButton("Test 3: State Physics Force", 165, function()
    local char = localPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        char:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Physics)
        print("[Test 3] Humanoid state forced to Physics.")
    end
end)

-- Close GUI Button
createButton("Close GUI", 220, function()
    ScreenGui:Destroy()
end)

print("[+] Test Suite GUI Loaded.")

-- Delta Executor GUI Framework: Combined "Test" Hub
-- This script compiles all features from your provided script into a single clean GUI tab menu.

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TestHub_Gui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 400, 0, 320)
MainFrame.Position = UDim2.new(0.5, -200, 0.4, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
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
Title.Text = "Test Hub - All-in-One"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, -20, 1, -60)
ScrollingFrame.Position = UDim2.new(0, 10, 0, 50)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 500)
ScrollingFrame.ScrollBarThickness = 4
ScrollingFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Parent = ScrollingFrame

local function createButton(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 14
    btn.Font = Enum.Font.Gotham
    btn.Text = text
    btn.Parent = ScrollingFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    btn.MouseButton1Click:Connect(callback)
end

-- Variables logic mapping
local ohString11 = "Dash"
local ohBoolean22 = true
local ohTable33 = { ["DashDirection"] = "Front" }
local ohString1 = "Dash"
local ohBoolean2 = true
local ohTable3 = { ["DashDirection"] = "Left", ["Evasive"] = true }

createButton("No Front-Dash Cooldown (Fire)", function()
    pcall(function()
        LocalPlayer.Remotes.Combat.AttackInput:FireServer(ohString11, ohBoolean22, ohTable33)
    end)
end)

createButton("Ragdoll Cancel (Fire)", function()
    pcall(function()
        LocalPlayer.Remotes.Combat.AttackInput:FireServer(ohString1, ohBoolean2, ohTable3)
    end)
end)

createButton("Anti-Ragdoll (Destroy Remote)", function()
    pcall(function()
        game:GetService("ReplicatedStorage").Remotes.Combat.Ragdoll:Destroy()
    end)
end)

createButton("Toggle Local Invisibility (Transparency)", function()
    local char = LocalPlayer.Character
    if char then
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") or part:IsA("MeshPart") then
                part.Transparency = part.Transparency == 1 and 0 or 1
            end
        end
    end
end)

createButton("Teleport to Chara", function()
    pcall(function()
        local charamodel = workspace:WaitForChild("Models"):WaitForChild("Chara")
        local char = LocalPlayer.Character
        local hrp = char and char:WaitForChild("HumanoidRootPart")
        if charamodel and charamodel.PrimaryPart and hrp then
            hrp.CFrame = charamodel.PrimaryPart.CFrame
        end
    end)
end)

createButton("Rejoin Server", function()
    local TeleportService = game:GetService("TeleportService")
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, LocalPlayer)
end)

createButton("Close GUI", function()
    ScreenGui:Destroy()
end)

print("[+] Test Hub GUI successfully compiled and loaded.")

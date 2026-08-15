--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
local Players = game:GetService("Players")
local VirtualInputManager = game:GetService("VirtualInputManager")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local enabled = true
task.spawn(function()
    pcall(function()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Auto Black Flash",
            Text = "Loaded!\nClick button to toggle\nDrag by title • X to close (disables BF)",
            Duration = 6
        })
    end)
end)
local AnimationTriggers = {
    ["rbxassetid://100962226150441"] = 0.18,
    ["rbxassetid://95852624447551"] = 0.18,
    ["rbxassetid://74145636023952"] = 0.18,
    ["rbxassetid://72475960800126"] = 0.20,
}
local function pressKey(keyCode)
    VirtualInputManager:SendKeyEvent(true, keyCode, false, game)
    task.wait()
    VirtualInputManager:SendKeyEvent(false, keyCode, false, game)
end
local function setupCharacter(character)
    local humanoid = character:WaitForChild("Humanoid", 5)
    if not humanoid then return end
    local animator = humanoid:WaitForChild("Animator", 5)
    if not animator then return end
    animator.AnimationPlayed:Connect(function(track)
        if not enabled then return end
        local animId = track.Animation.AnimationId
        local delay = AnimationTriggers[animId]
        if delay then
            task.delay(delay, function()
                if humanoid.Health > 0 and enabled then
                    pressKey(Enum.KeyCode.Three)
                end
            end)
        end
    end)
end
if player.Character then
    setupCharacter(player.Character)
end
player.CharacterAdded:Connect(function(char)
    task.wait(0.3)
    setupCharacter(char)
end)
local screenGui = Instance.new("ScreenGui")
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 240, 0, 100)
frame.Position = UDim2.new(1, -260, 0, 20)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BackgroundTransparency = 0.1
frame.BorderSizePixel = 0
frame.Parent = screenGui
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 16)
corner.Parent = frame
local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(60, 60, 60)
stroke.Thickness = 1.5
stroke.Parent = frame
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -35, 0, 35)
title.Position = UDim2.new(0, 10, 0, 5)
title.BackgroundTransparency = 1
title.Text = "AUTO BLACK FLASH"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.Parent = frame
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0.88, 0, 0, 42)
toggleButton.Position = UDim2.new(0.06, 0, 0, 48)
toggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
toggleButton.Text = "ENABLED"
toggleButton.TextColor3 = Color3.fromRGB(0, 0, 0)
toggleButton.TextScaled = true
toggleButton.Font = Enum.Font.GothamBold
toggleButton.Parent = frame
local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 10)
btnCorner.Parent = toggleButton
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 28, 0, 28)
closeButton.Position = UDim2.new(1, -33, 0, 6)
closeButton.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextScaled = true
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = frame
local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 8)
closeCorner.Parent = closeButton
local function updateButton()
    if enabled then
        toggleButton.Text = "ENABLED ✅"
        toggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
    else
        toggleButton.Text = "DISABLED ❌"
        toggleButton.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
    end
end
toggleButton.MouseButton1Click:Connect(function()
    enabled = not enabled
    updateButton()
    local status = enabled and "ENABLED ✅" or "DISABLED ❌"
    pcall(function()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Auto Black Flash",
            Text = status,
            Duration = 2.5
        })
    end)
end)
closeButton.MouseButton1Click:Connect(function()
    enabled = false
    screenGui:Destroy()
end)
updateButton()
local dragging = false
local dragStart
local startPos
title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
--son im crine who even struggle to blackflash

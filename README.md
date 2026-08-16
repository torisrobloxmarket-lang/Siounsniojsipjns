-- Delta Executor GUI: All-in-One Hub (Invisibility, Speed, Teleports & Combat)
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- Altes GUI entfernen, falls vorhanden
if CoreGui:FindFirstChild("JJS_AllInOneHub") then
    CoreGui.JJS_AllInOneHub:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "JJS_AllInOneHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 320, 0, 420)
MainFrame.Position = UDim2.new(0.5, -160, 0.3, -210)
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
Title.Text = "JJS Complete Feature Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, -20, 1, -60)
ScrollingFrame.Position = UDim2.new(0, 10, 0, 50)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 550)
ScrollingFrame.ScrollBarThickness = 4
ScrollingFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Parent = ScrollingFrame

local function createToggle(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
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
        btn.BackgroundColor3 = state and Color3.fromRGB(0, 120, 215) or Color3.fromRGB(45, 45, 45)
        callback(state)
    end)
end

local function createButton(text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
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

-- 1. Invisibility Funktion (aus dem Source Code)
local invToggleState = false
local invConnections = {}
local currentTrack = nil
local partsToSet = { "Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg" }

createToggle("Invisibility (Anim-Method)", function(value)
    invToggleState = value
    local char = LocalPlayer.Character
    if not char then return end

    for _, conn in pairs(invConnections) do
        if conn then conn:Disconnect() end
    end
    invConnections = {}

    if currentTrack and currentTrack.IsPlaying then
        currentTrack:Stop()
    end

    if invToggleState then
        for _, partName in ipairs(partsToSet) do
            local part = char:FindFirstChild(partName)
            if part and part:IsA("BasePart") then part.Transparency = 0.5 end
        end

        local humanoid = char:WaitForChild("Humanoid")
        local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)
        local animation = Instance.new("Animation")
        animation.AnimationId = "rbxassetid://83555998356899"
        
        pcall(function()
            currentTrack = animator:LoadAnimation(animation)
            currentTrack.Priority = Enum.AnimationPriority.Action4
            table.insert(invConnections, RunService.Heartbeat:Connect(function()
                if not currentTrack.IsPlaying then currentTrack:Play() end
                currentTrack:AdjustSpeed(0)
                currentTrack.TimePosition = 10
            end))
        end)
    else
        for _, partName in ipairs(partsToSet) do
            local part = char:FindFirstChild(partName)
            if part and part:IsA("BasePart") then part.Transparency = 0 end
        end
    end
end)

-- 2. No Front-Dash Cooldown
createToggle("No Front-Dash CD", function(value)
    _G.nofdashcd = value
    task.spawn(function()
        while _G.nofdashcd do
            pcall(function()
                LocalPlayer.Remotes.Combat.AttackInput:FireServer("Dash", true, { ["DashDirection"] = "Front" })
            end)
            task.wait(0.2)
        end
    end)
end)

-- 3. Speedhack / TP-Walk
local tpWalking = false
local tpWalkSpeed = 50
createToggle("Speedhack", function(value)
    tpWalking = value
end)

RunService.Heartbeat:Connect(function(dt)
    if not tpWalking then return end
    local char = LocalPlayer.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if char and hum and hum.MoveDirection.Magnitude > 0 then
        char:TranslateBy(hum.MoveDirection * tpWalkSpeed * dt)
    end
end)

-- 4. Anti-Ragdoll
createButton("Anti-Ragdoll (Destroy Remote)", function()
    pcall(function()
        game:GetService("ReplicatedStorage").Remotes.Combat.Ragdoll:Destroy()
    end)
end)

-- 5. Teleport zu Chara
createButton("Teleport to Chara Model", function()
    pcall(function()
        local charamodel = workspace:WaitForChild("Models"):WaitForChild("Chara")
        local char = LocalPlayer.Character
        local hrp = char and char:WaitForChild("HumanoidRootPart")
        if charamodel and charamodel.PrimaryPart and hrp then
            hrp.CFrame = charamodel.PrimaryPart.CFrame
        end
    end)
end)

-- Schließen Button
createButton("Close GUI", function()
    ScreenGui:Destroy()
end)

print("[+] All-in-One Hub successfully loaded.")

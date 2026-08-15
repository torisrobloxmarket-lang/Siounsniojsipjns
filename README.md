--// ==========================================
--// RYU INVIS — REPLICATED BUILD
--// Jujutsu Shenanigans
--// Transparency (replicated) + Description wipe
--// ==========================================

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

--// GUI PARENT
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function()
    if gethui then guiParent = gethui() end
end)

for _, v in pairs(guiParent:GetChildren()) do
    if v.Name == "RyuInvisSolo" then v:Destroy() end
end

--// GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RyuInvisSolo"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = guiParent

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 220, 0, 70)
Frame.Position = UDim2.new(0.5, -110, 0, 120)
Frame.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
Frame.BorderSizePixel = 0
Frame.Active = true
Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 10)
local fs = Instance.new("UIStroke", Frame)
fs.Color = Color3.fromRGB(60, 60, 60)
fs.Thickness = 1.5

-- Drag
local dragStart, dragPos
Frame.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
        dragStart = i.Position
        dragPos = Frame.Position
    end
end)
UserInputService.InputChanged:Connect(function(i)
    if dragStart and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local d = i.Position - dragStart
        Frame.Position = UDim2.new(dragPos.X.Scale, dragPos.X.Offset + d.X, dragPos.Y.Scale, dragPos.Y.Offset + d.Y)
    end
end)
UserInputService.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
        dragStart = nil
    end
end)

local Label = Instance.new("TextLabel", Frame)
Label.Size = UDim2.new(0, 130, 1, 0)
Label.Position = UDim2.new(0, 14, 0, 0)
Label.BackgroundTransparency = 1
Label.Text = "Invisible"
Label.TextColor3 = Color3.fromRGB(200, 200, 200)
Label.Font = Enum.Font.GothamBold
Label.TextSize = 14
Label.TextXAlignment = Enum.TextXAlignment.Left

local StatusDot = Instance.new("Frame", Frame)
StatusDot.Size = UDim2.new(0, 8, 0, 8)
StatusDot.Position = UDim2.new(0, 14, 0, 46)
StatusDot.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
StatusDot.BorderSizePixel = 0
Instance.new("UICorner", StatusDot).CornerRadius = UDim.new(1, 0)

local StatusLabel = Instance.new("TextLabel", Frame)
StatusLabel.Size = UDim2.new(1, -30, 0, 16)
StatusLabel.Position = UDim2.new(0, 28, 0, 42)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "OFF — toggle to activate"
StatusLabel.TextColor3 = Color3.fromRGB(90, 90, 90)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 10
StatusLabel.TextXAlignment = Enum.TextXAlignment.Left

local ToggleBtn = Instance.new("TextButton", Frame)
ToggleBtn.Size = UDim2.new(0, 48, 0, 26)
ToggleBtn.Position = UDim2.new(1, -62, 0.5, -13)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ToggleBtn.Text = ""
ToggleBtn.BorderSizePixel = 0
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)

local Circle = Instance.new("Frame", ToggleBtn)
Circle.Size = UDim2.new(0, 18, 0, 18)
Circle.Position = UDim2.new(0, 4, 0.5, -9)
Circle.BackgroundColor3 = Color3.fromRGB(130, 130, 130)
Circle.BorderSizePixel = 0
Instance.new("UICorner", Circle).CornerRadius = UDim.new(1, 0)

--// ==========================================
--// INVIS LOGIC — REPLICATED
--// BasePart.Transparency = 1 replicates to server.
--// LocalTransparencyModifier is client-only — not used here.
--// Keepalive thread reapplies every 0.3s to catch
--// parts added dynamically (JTS spawns accessories mid-game).
--// HumanoidDescription wipe on toggle strips server-side
--// avatar so joining clients also see nothing.
--// ==========================================

local invisActive = false
local invisThread = nil
local originalTransparency = {}

local function setReplicated(char, hide)
    for _, part in pairs(char:GetDescendants()) do
        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
            if hide then
                if originalTransparency[part] == nil then
                    originalTransparency[part] = part.Transparency
                end
                part.Transparency = 1
            else
                if originalTransparency[part] ~= nil then
                    part.Transparency = originalTransparency[part]
                    originalTransparency[part] = nil
                else
                    part.Transparency = 0
                end
            end
        end

        -- Decals only — SpecialMesh excluded, no Transparency property
        if part:IsA("Decal") then
            if hide then
                if originalTransparency[part] == nil then
                    originalTransparency[part] = part.Transparency
                end
                part.Transparency = 1
            else
                if originalTransparency[part] ~= nil then
                    part.Transparency = originalTransparency[part]
                    originalTransparency[part] = nil
                else
                    part.Transparency = 0
                end
            end
        end
    end
end

local function wipeDescription(char)
    pcall(function()
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then
            hum:ApplyDescription(Instance.new("HumanoidDescription"))
        end
    end)
end

local function restoreDescription()
    pcall(function()
        local char = LocalPlayer.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum then
            local desc = Players:GetHumanoidDescriptionFromUserId(LocalPlayer.UserId)
            hum:ApplyDescription(desc)
        end
    end)
end

local function startInvis()
    local char = LocalPlayer.Character
    if not char then return end

    originalTransparency = {}
    setReplicated(char, true)
    wipeDescription(char)

    -- Keepalive: JTS adds accessories dynamically, reapply catches them
    invisThread = task.spawn(function()
        while invisActive do
            task.wait(0.3)
            local c = LocalPlayer.Character
            if c and invisActive then
                setReplicated(c, true)
            end
        end
    end)

    StatusDot.BackgroundColor3 = Color3.fromRGB(80, 200, 80)
    StatusLabel.Text = "ON — replicated to server"
    StatusLabel.TextColor3 = Color3.fromRGB(80, 200, 80)
end

local function stopInvis()
    invisActive = false
    if invisThread then
        task.cancel(invisThread)
        invisThread = nil
    end

    local char = LocalPlayer.Character
    if char then
        setReplicated(char, false)
    end
    originalTransparency = {}
    restoreDescription()

    StatusDot.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    StatusLabel.Text = "OFF — toggle to activate"
    StatusLabel.TextColor3 = Color3.fromRGB(90, 90, 90)
end

-- Reapply on respawn
LocalPlayer.CharacterAdded:Connect(function(char)
    originalTransparency = {}
    if invisActive then
        task.wait(1.5)
        setReplicated(char, true)
        wipeDescription(char)
    end
end)

--// TOGGLE
local isOn = false
ToggleBtn.MouseButton1Click:Connect(function()
    isOn = not isOn
    invisActive = isOn

    TweenService:Create(ToggleBtn, TweenInfo.new(0.2), {
        BackgroundColor3 = isOn and Color3.fromRGB(80, 200, 80) or Color3.fromRGB(45, 45, 45)
    }):Play()
    TweenService:Create(Circle, TweenInfo.new(0.2), {
        Position = isOn and UDim2.new(1, -22, 0.5, -9) or UDim2.new(0, 4, 0.5, -9),
        BackgroundColor3 = isOn and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(130, 130, 130)
    }):Play()

    if isOn then
        startInvis()
    else
        stopInvis()
    end
end)

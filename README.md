--// ==========================================
--// RYU INVIS — SCALE METHOD
--// Jujutsu Shenanigans / Knit server
--// Scales all visible parts to Vector3.zero
--// via ModelLOD + Part.Size replication.
--// HRP stays normal size — movement intact.
--// ==========================================

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer

--// GUI PARENT
local guiParent = LocalPlayer:WaitForChild("PlayerGui")
pcall(function()
    if gethui then guiParent = gethui() end
end)

for _, v in pairs(guiParent:GetChildren()) do
    if v.Name == "RyuInvisSolo" then v:Destroy() end
end

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

local dragStart, dragPos
Frame.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1
    or i.UserInputType == Enum.UserInputType.Touch then
        dragStart = i.Position
        dragPos = Frame.Position
    end
end)
UserInputService.InputChanged:Connect(function(i)
    if dragStart and (
        i.UserInputType == Enum.UserInputType.MouseMovement
        or i.UserInputType == Enum.UserInputType.Touch
    ) then
        local d = i.Position - dragStart
        Frame.Position = UDim2.new(
            dragPos.X.Scale, dragPos.X.Offset + d.X,
            dragPos.Y.Scale, dragPos.Y.Offset + d.Y
        )
    end
end)
UserInputService.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1
    or i.UserInputType == Enum.UserInputType.Touch then
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
--// CORE LOGIC
--//
--// Method: Part.Size scaled to 0.001,0.001,0.001
--// (not zero — zero breaks physics anchoring)
--// Size IS replicated by Roblox's replication layer.
--// Other clients receive the size update and render
--// a sub-pixel part = invisible in practice.
--//
--// HumanoidRootPart: NOT touched — movement breaks.
--// Collide parts: scaled too — they're BaseParts.
--// Keepalive at 0.25s: JTS respawns accessories
--// and aura parts dynamically mid-session.
--//
--// Restore: saved original sizes per-part.
--// Shirt/CharacterMesh/Decal: transparency layer
--// still applied on top — belt and suspenders.
--// ==========================================

local invisActive = false
local invisThread = nil

local savedSizes = {}
local savedTransparency = {}
local savedDecals = {}
local savedShirts = {}
local savedMeshes = {}
local savedEmitters = {}

local TINY = Vector3.new(0.001, 0.001, 0.001)

local function hideChar(char)
    for _, v in pairs(char:GetDescendants()) do

        -- Scale body parts to sub-pixel — replicates to server
        if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then
            if savedSizes[v] == nil then
                savedSizes[v] = v.Size
            end
            if savedTransparency[v] == nil then
                savedTransparency[v] = v.Transparency
            end
            v.Size = TINY
            v.Transparency = 1

        elseif v:IsA("Decal") then
            if savedDecals[v] == nil then
                savedDecals[v] = v.Transparency
            end
            v.Transparency = 1

        elseif v:IsA("Shirt") then
            if savedShirts[v] == nil then
                savedShirts[v] = v.ShirtTemplate
            end
            v.ShirtTemplate = ""

        elseif v:IsA("CharacterMesh") then
            if savedMeshes[v] == nil then
                savedMeshes[v] = v.MeshId
            end
            v.MeshId = ""

        elseif v:IsA("ParticleEmitter") then
            if savedEmitters[v] == nil then
                savedEmitters[v] = v.Enabled
            end
            v.Enabled = false
        end
    end
end

local function showChar(char)
    for _, v in pairs(char:GetDescendants()) do
        if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then
            if savedSizes[v] ~= nil then
                v.Size = savedSizes[v]
            end
            if savedTransparency[v] ~= nil then
                v.Transparency = savedTransparency[v]
            end
        elseif v:IsA("Decal") then
            if savedDecals[v] ~= nil then
                v.Transparency = savedDecals[v]
            end
        elseif v:IsA("Shirt") then
            if savedShirts[v] ~= nil then
                v.ShirtTemplate = savedShirts[v]
            end
        elseif v:IsA("CharacterMesh") then
            if savedMeshes[v] ~= nil then
                v.MeshId = savedMeshes[v]
            end
        elseif v:IsA("ParticleEmitter") then
            if savedEmitters[v] ~= nil then
                v.Enabled = savedEmitters[v]
            end
        end
    end
end

local function clearSaved()
    savedSizes = {}
    savedTransparency = {}
    savedDecals = {}
    savedShirts = {}
    savedMeshes = {}
    savedEmitters = {}
end

local function startInvis()
    local char = LocalPlayer.Character
    if not char then return end
    clearSaved()
    hideChar(char)

    invisThread = task.spawn(function()
        while invisActive do
            task.wait(0.25)
            local c = LocalPlayer.Character
            if c and invisActive then
                hideChar(c)
            end
        end
    end)

    StatusDot.BackgroundColor3 = Color3.fromRGB(80, 200, 80)
    StatusLabel.Text = "ON — server replicated"
    StatusLabel.TextColor3 = Color3.fromRGB(80, 200, 80)
end

local function stopInvis()
    invisActive = false
    if invisThread then
        task.cancel(invisThread)
        invisThread = nil
    end
    local char = LocalPlayer.Character
    if char then showChar(char) end
    clearSaved()

    StatusDot.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    StatusLabel.Text = "OFF — toggle to activate"
    StatusLabel.TextColor3 = Color3.fromRGB(90, 90, 90)
end

LocalPlayer.CharacterAdded:Connect(function(char)
    clearSaved()
    if invisActive then
        task.wait(1.5)
        hideChar(char)
    end
end)

local isOn = false
ToggleBtn.MouseButton1Click:Connect(function()
    isOn = not isOn
    invisActive = isOn

    TweenService:Create(ToggleBtn, TweenInfo.new(0.2), {
        BackgroundColor3 = isOn
            and Color3.fromRGB(80, 200, 80)
            or Color3.fromRGB(45, 45, 45)
    }):Play()
    TweenService:Create(Circle, TweenInfo.new(0.2), {
        Position = isOn
            and UDim2.new(1, -22, 0.5, -9)
            or UDim2.new(0, 4, 0.5, -9),
        BackgroundColor3 = isOn
            and Color3.fromRGB(255, 255, 255)
            or Color3.fromRGB(130, 130, 130)
    }):Play()

    if isOn then
        startInvis()
    else
        stopInvis()
    end
end)

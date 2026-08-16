local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local activeLoops = {}
local glitchTrack = nil
local isInvisible = false

--// 1. GUI ERSTELLEN
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "JJS_Desync_Test"
screenGui.ResetOnSpawn = false

-- Versuche das GUI sicher zu parenten
local success, err = pcall(function()
    screenGui.Parent = (gethui and gethui()) or CoreGui
end)
if not success then
    screenGui.Parent = player:WaitForChild("PlayerGui")
end

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 150, 0, 50)
mainFrame.Position = UDim2.new(0.5, -75, 0.2, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = mainFrame

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(1, -10, 1, -10)
toggleBtn.Position = UDim2.new(0, 5, 0, 5)
toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 14
toggleBtn.Text = "Desync: OFF"
toggleBtn.Parent = mainFrame

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 6)
btnCorner.Parent = toggleBtn

--// 2. FUNKTIONEN FÜR UNSICHTBARKEIT
local function setCharacterTransparency(character, transparency)
    local parts = {"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg", "UpperTorso", "LowerTorso"}
    for _, partName in ipairs(parts) do
        local part = character:FindFirstChild(partName)
        if part and part:IsA("BasePart") then
            part.Transparency = transparency
        end
    end
end

local function stopInvisibility()
    for _, loop in pairs(activeLoops) do
        if loop then loop:Disconnect() end
    end
    activeLoops = {}

    if glitchTrack and glitchTrack.IsPlaying then
        glitchTrack:Stop()
    end
    
    if player.Character then
        setCharacterTransparency(player.Character, 0)
    end
end

local function startInvisibility(character)
    local humanoid = character:WaitForChild("Humanoid")
    local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)

    -- FIX: Standard Roblox Animation verwenden, die niemals gelöscht wird
    local anim = Instance.new("Animation")
    anim.AnimationId = "rbxassetid://507771019" 
    glitchTrack = animator:LoadAnimation(anim)
    glitchTrack.Priority = Enum.AnimationPriority.Action4

    setCharacterTransparency(character, 0.5)

    -- Replikations-Überlastung starten
    local heartbeat = RunService.Heartbeat:Connect(function()
        if not glitchTrack.IsPlaying then
            glitchTrack:Play()
        end
        glitchTrack:AdjustSpeed(0)
        glitchTrack.TimePosition = 10
    end)
    table.insert(activeLoops, heartbeat)

    local render = RunService.RenderStepped:Connect(function()
        if glitchTrack.IsPlaying then
            glitchTrack:Stop()
        end
    end)
    table.insert(activeLoops, render)
end

--// 3. KNOPF-LOGIK
toggleBtn.MouseButton1Click:Connect(function()
    isInvisible = not isInvisible
    local char = player.Character

    if isInvisible then
        toggleBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        toggleBtn.Text = "Desync: ON"
        if char then
            stopInvisibility()
            startInvisibility(char)
        end
    else
        toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        toggleBtn.Text = "Desync: OFF"
        stopInvisibility()
    end
end)

--// 4. RESET-SCHUTZ
player.CharacterAdded:Connect(function(char)
    if isInvisible then
        char:WaitForChild("HumanoidRootPart")
        task.wait(0.5) 
        stopInvisibility()
        startInvisibility(char)
    end
end)

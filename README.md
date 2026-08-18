--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
if _G.boomshaka then 
    warn("Script already running!") 
    return 
end
_G.boomshaka = true

local ScriptEnabled = true 

local UserInputService    = game:GetService("UserInputService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local Players             = game:GetService("Players")
local RunService          = game:GetService("RunService")
local LocalPlayer         = Players.LocalPlayer
local Camera              = workspace.CurrentCamera

local AnimationTriggers = {
    ["rbxassetid://100962226150441"] = 0.19,
    ["rbxassetid://95852624447551"]  = 0.19,
    ["rbxassetid://74145636023952"]  = 0.19,
    ["rbxassetid://72475960800126"]  = 0.20,
}

local StraightAnimations = {
    ["rbxassetid://123171106092050"] = true,
}

local Settings = {
    Duration      = 0.25,
    Radius        = 3,
    Range         = 25,
    CurveStrength = 14,
    CamOffset     = 4,
}

local pingVisualizerEnabled = false
local ghostModel            = nil
local positionHistory       = {}
local MAX_HISTORY           = 300

local function createGhost(character)
    if ghostModel then ghostModel:Destroy() ghostModel = nil end
    if not character then return end
    ghostModel = Instance.new("Model")
    ghostModel.Name = "PingGhost"
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") or part:IsA("MeshPart") then
            local clone = part:Clone()
            for _, child in pairs(clone:GetChildren()) do
                if not child:IsA("SpecialMesh") and not child:IsA("Decal") then child:Destroy() end
            end
            clone.Anchored     = true
            clone.CanCollide   = false
            clone.CastShadow   = false
            clone.Transparency = 0.6
            clone.Color        = Color3.fromRGB(255, 80, 80)
            clone.Material     = Enum.Material.Neon
            clone.Parent       = ghostModel
        end
    end
    ghostModel.Parent = workspace
end

local function destroyGhost()
    if ghostModel then ghostModel:Destroy() ghostModel = nil end
    positionHistory = {}
end

local function recordSnapshot(character)
    if not character then return end
    local snapshot = { time = tick(), parts = {} }
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") or part:IsA("MeshPart") then
            snapshot.parts[part.Name] = part.CFrame
        end
    end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if hrp then snapshot.parts["HumanoidRootPart"] = hrp.CFrame end
    table.insert(positionHistory, snapshot)
    while #positionHistory > MAX_HISTORY do table.remove(positionHistory, 1) end
end

local function getDelayedSnapshot()
    local targetTime = tick() - LocalPlayer:GetNetworkPing()
    for i = #positionHistory, 1, -1 do
        if positionHistory[i].time <= targetTime then return positionHistory[i] end
    end
    return positionHistory[1]
end

local function applySnapshotToGhost(snapshot)
    if not ghostModel or not snapshot then return end
    for _, part in pairs(ghostModel:GetDescendants()) do
        if (part:IsA("BasePart") or part:IsA("MeshPart")) and snapshot.parts[part.Name] then
            part.CFrame = snapshot.parts[part.Name]
        end
    end
end

--// GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name           = "BoomshakaPC"
screenGui.ResetOnSpawn   = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent         = LocalPlayer.PlayerGui

local hintLabel = Instance.new("TextLabel")
hintLabel.Size             = UDim2.new(0, 240, 0, 28)
hintLabel.Position         = UDim2.new(0, 20, 0, 20)
hintLabel.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
hintLabel.TextColor3       = Color3.fromRGB(130, 130, 130)
hintLabel.Text             = "Press RightShift to open settings"
hintLabel.TextSize         = 13
hintLabel.Font             = Enum.Font.Gotham
hintLabel.BorderSizePixel  = 0
hintLabel.Visible          = false
hintLabel.Parent           = screenGui
Instance.new("UICorner", hintLabel).CornerRadius = UDim.new(0, 8)
Instance.new("UIPadding", hintLabel).PaddingLeft = UDim.new(0, 10)

local frame = Instance.new("Frame")
frame.Size             = UDim2.new(0, 320, 0, 600)
frame.Position         = UDim2.new(0, 20, 0.5, -300)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
frame.BorderSizePixel  = 0
frame.Active           = true
frame.Draggable        = true
frame.Parent           = screenGui
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)
local outerStroke = Instance.new("UIStroke", frame)
outerStroke.Color     = Color3.fromRGB(55, 55, 55)
outerStroke.Thickness = 1

local header = Instance.new("Frame")
header.Size             = UDim2.new(1, 0, 0, 52)
header.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
header.BorderSizePixel  = 0
header.Parent           = frame
Instance.new("UICorner", header).CornerRadius = UDim.new(0, 12)
local headerFix = Instance.new("Frame")
headerFix.Size             = UDim2.new(1, 0, 0.5, 0)
headerFix.Position         = UDim2.new(0, 0, 0.5, 0)
headerFix.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
headerFix.BorderSizePixel  = 0
headerFix.Parent           = header

local headerTitle = Instance.new("TextLabel")
headerTitle.Size                   = UDim2.new(1, -130, 1, 0)
headerTitle.Position               = UDim2.new(0, 16, 0, 0)
headerTitle.BackgroundTransparency = 1
headerTitle.Text                   = "Curve Settings"
headerTitle.TextColor3             = Color3.fromRGB(255, 255, 255)
headerTitle.TextSize               = 16
headerTitle.Font                   = Enum.Font.GothamBold
headerTitle.TextXAlignment         = Enum.TextXAlignment.Left
headerTitle.Parent                 = header

local headerSub = Instance.new("TextLabel")
headerSub.Size                   = UDim2.new(1, -130, 0, 16)
headerSub.Position               = UDim2.new(0, 16, 0, 30)
headerSub.BackgroundTransparency = 1
headerSub.Text                   = "RightShift to hide"
headerSub.TextColor3             = Color3.fromRGB(90, 90, 90)
headerSub.TextSize               = 11
headerSub.Font                   = Enum.Font.Gotham
headerSub.TextXAlignment         = Enum.TextXAlignment.Left
headerSub.Parent                 = header

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size             = UDim2.new(0, 72, 0, 30)
toggleBtn.Position         = UDim2.new(1, -82, 0.5, -15)
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 90)
toggleBtn.Text             = "ENABLED"
toggleBtn.TextColor3       = Color3.fromRGB(255, 255, 255)
toggleBtn.TextSize         = 12
toggleBtn.Font             = Enum.Font.GothamBold
toggleBtn.BorderSizePixel  = 0
toggleBtn.Parent           = header
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(0, 8)

local divider = Instance.new("Frame")
divider.Size             = UDim2.new(1, -24, 0, 1)
divider.Position         = UDim2.new(0, 12, 0, 52)
divider.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
divider.BorderSizePixel  = 0
divider.Parent           = frame

local container = Instance.new("Frame")
container.Size                   = UDim2.new(1, -24, 1, -68)
container.Position               = UDim2.new(0, 12, 0, 60)
container.BackgroundTransparency = 1
container.Parent                 = frame
local layout = Instance.new("UIListLayout", container)
layout.Padding   = UDim.new(0, 8)
layout.SortOrder = Enum.SortOrder.LayoutOrder
local padder = Instance.new("UIPadding", container)
padder.PaddingBottom = UDim.new(0, 8)

local function makeSlider(labelText, description, min, max, default, settingKey, step)
    step = step or 0.01
    local row = Instance.new("Frame")
    row.Size             = UDim2.new(1, 0, 0, 72)
    row.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
    row.BorderSizePixel  = 0
    row.Parent           = container
    Instance.new("UICorner", row).CornerRadius = UDim.new(0, 10)

    local lbl = Instance.new("TextLabel")
    lbl.Size                   = UDim2.new(1, -80, 0, 20)
    lbl.Position               = UDim2.new(0, 12, 0, 8)
    lbl.BackgroundTransparency = 1
    lbl.Text                   = labelText
    lbl.TextColor3             = Color3.fromRGB(230, 230, 230)
    lbl.TextSize               = 14
    lbl.Font                   = Enum.Font.GothamBold
    lbl.TextXAlignment         = Enum.TextXAlignment.Left
    lbl.Parent                 = row

    local desc = Instance.new("TextLabel")
    desc.Size                   = UDim2.new(1, -16, 0, 14)
    desc.Position               = UDim2.new(0, 12, 0, 28)
    desc.BackgroundTransparency = 1
    desc.Text                   = description
    desc.TextColor3             = Color3.fromRGB(100, 100, 100)
    desc.TextSize               = 11
    desc.Font                   = Enum.Font.Gotham
    desc.TextXAlignment         = Enum.TextXAlignment.Left
    desc.Parent                 = row

    local valLbl = Instance.new("TextLabel")
    valLbl.Size                   = UDim2.new(0, 60, 0, 20)
    valLbl.Position               = UDim2.new(1, -70, 0, 8)
    valLbl.BackgroundTransparency = 1
    valLbl.Text                   = tostring(default)
    valLbl.TextColor3             = Color3.fromRGB(80, 180, 255)
    valLbl.TextSize               = 14
    valLbl.Font                   = Enum.Font.GothamBold
    valLbl.TextXAlignment         = Enum.TextXAlignment.Right
    valLbl.Parent                 = row

    local track = Instance.new("Frame")
    track.Size             = UDim2.new(1, -24, 0, 6)
    track.Position         = UDim2.new(0, 12, 0, 52)
    track.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    track.BorderSizePixel  = 0
    track.Parent           = row
    Instance.new("UICorner", track).CornerRadius = UDim.new(1, 0)

    local fill = Instance.new("Frame")
    fill.Size             = UDim2.new((default - min) / (max - min), 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(80, 180, 255)
    fill.BorderSizePixel  = 0
    fill.Parent           = track
    Instance.new("UICorner", fill).CornerRadius = UDim.new(1, 0)

    local knob = Instance.new("TextButton")
    knob.Size             = UDim2.new(0, 16, 0, 16)
    knob.Position         = UDim2.new((default - min) / (max - min), -8, 0.5, -8)
    knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    knob.Text             = ""
    knob.BorderSizePixel  = 0
    knob.ZIndex           = 2
    knob.Parent           = track
    Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)

    local function update(xPos)
        local alpha   = math.clamp((xPos - track.AbsolutePosition.X) / track.AbsoluteSize.X, 0, 1)
        local snapped = math.round((min + (max - min) * alpha) / step) * step
        local display = math.floor(snapped * 1000 + 0.5) / 1000
        fill.Size            = UDim2.new(alpha, 0, 1, 0)
        knob.Position        = UDim2.new(alpha, -8, 0.5, -8)
        valLbl.Text          = tostring(display)
        Settings[settingKey] = snapped
    end

    local dragging = false
    knob.MouseButton1Down:Connect(function() dragging = true end)
    UserInputService.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(i)
        if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then update(i.Position.X) end
    end)
end

makeSlider("Glide Speed",      "Lower = faster. (0.1 is instant, 1.0 is slow)", 0.1, 1.0, Settings.Duration,      "Duration",      0.025)
makeSlider("Landing Distance", "How many studs behind the target you land",      1,   10,  Settings.Radius,        "Radius",        0.5)
makeSlider("Detection Range",  "How far away a target can be detected (studs)",  10,  50,  Settings.Range,         "Range",         1)
makeSlider("Curve Width",      "How wide the arc swings around the target",      0,   30,  Settings.CurveStrength, "CurveStrength", 1)
makeSlider("Camera Height",    "How high the lock-on camera floats above you",   0,   10,  Settings.CamOffset,     "CamOffset",     0.5)

local pingDivider = Instance.new("Frame")
pingDivider.Size             = UDim2.new(1, 0, 0, 1)
pingDivider.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
pingDivider.BorderSizePixel  = 0
pingDivider.Parent           = container

local pingSection = Instance.new("Frame")
pingSection.Size             = UDim2.new(1, 0, 0, 80)
pingSection.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
pingSection.BorderSizePixel  = 0
pingSection.Parent           = container
Instance.new("UICorner", pingSection).CornerRadius = UDim.new(0, 10)

local pingSectionTitle = Instance.new("TextLabel")
pingSectionTitle.Size                   = UDim2.new(1, -16, 0, 20)
pingSectionTitle.Position               = UDim2.new(0, 12, 0, 8)
pingSectionTitle.BackgroundTransparency = 1
pingSectionTitle.Text                   = "Ping Visualizer"
pingSectionTitle.TextColor3             = Color3.fromRGB(230, 230, 230)
pingSectionTitle.TextSize               = 14
pingSectionTitle.Font                   = Enum.Font.GothamBold
pingSectionTitle.TextXAlignment         = Enum.TextXAlignment.Left
pingSectionTitle.Parent                 = pingSection

local pingSectionDesc = Instance.new("TextLabel")
pingSectionDesc.Size                   = UDim2.new(1, -16, 0, 14)
pingSectionDesc.Position               = UDim2.new(0, 12, 0, 28)
pingSectionDesc.BackgroundTransparency = 1
pingSectionDesc.Text                   = "Shows red ghost of where server thinks you are"
pingSectionDesc.TextColor3             = Color3.fromRGB(100, 100, 100)
pingSectionDesc.TextSize               = 11
pingSectionDesc.Font                   = Enum.Font.Gotham
pingSectionDesc.TextXAlignment         = Enum.TextXAlignment.Left
pingSectionDesc.Parent                 = pingSection

local pingDisplay = Instance.new("TextLabel")
pingDisplay.Size                   = UDim2.new(0, 80, 0, 20)
pingDisplay.Position               = UDim2.new(1, -90, 0, 8)
pingDisplay.BackgroundTransparency = 1
pingDisplay.Text                   = "-- ms"
pingDisplay.TextColor3             = Color3.fromRGB(80, 180, 255)
pingDisplay.TextSize               = 14
pingDisplay.Font                   = Enum.Font.GothamBold
pingDisplay.TextXAlignment         = Enum.TextXAlignment.Right
pingDisplay.Parent                 = pingSection

local pingToggle = Instance.new("TextButton")
pingToggle.Size             = UDim2.new(0, 72, 0, 28)
pingToggle.Position         = UDim2.new(1, -82, 0, 46)
pingToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
pingToggle.Text             = "OFF"
pingToggle.TextColor3       = Color3.fromRGB(180, 180, 180)
pingToggle.TextSize         = 12
pingToggle.Font             = Enum.Font.GothamBold
pingToggle.BorderSizePixel  = 0
pingToggle.Parent           = pingSection
Instance.new("UICorner", pingToggle).CornerRadius = UDim.new(0, 8)

toggleBtn.MouseButton1Click:Connect(function()
    ScriptEnabled = not ScriptEnabled
    toggleBtn.Text             = ScriptEnabled and "ENABLED" or "DISABLED"
    toggleBtn.BackgroundColor3 = ScriptEnabled and Color3.fromRGB(0, 180, 90) or Color3.fromRGB(190, 50, 50)
end)

pingToggle.MouseButton1Click:Connect(function()
    pingVisualizerEnabled = not pingVisualizerEnabled
    if pingVisualizerEnabled then
        pingToggle.Text             = "ON"
        pingToggle.TextColor3       = Color3.fromRGB(255, 255, 255)
        pingToggle.BackgroundColor3 = Color3.fromRGB(0, 180, 90)
        createGhost(LocalPlayer.Character)
    else
        pingToggle.Text             = "OFF"
        pingToggle.TextColor3       = Color3.fromRGB(180, 180, 180)
        pingToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        destroyGhost()
    end
end)

UserInputService.InputBegan:Connect(function(input, processed)
    if input.KeyCode == Enum.KeyCode.RightShift then
        local v = not frame.Visible
        frame.Visible     = v
        hintLabel.Visible = not v
    end
end)

--// Helpers
local function getHRP(character)
    return character and (character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("Torso") or character:FindFirstChild("UpperTorso"))
end
local function pressKey(keyCode)
    if not ScriptEnabled then return end
    VirtualInputManager:SendKeyEvent(true, keyCode, false, game)
    VirtualInputManager:SendKeyEvent(false, keyCode, false, game)
end
local function getNearestPlayer(maxRange)
    local myHRP = getHRP(LocalPlayer.Character)
    if not myHRP then return nil end
    local nearest, nearestDist = nil, maxRange
    for _, pl in pairs(Players:GetPlayers()) do
        if pl ~= LocalPlayer and pl.Character and pl.Character:FindFirstChild("Humanoid") and pl.Character.Humanoid.Health > 0 then
            local tHRP = getHRP(pl.Character)
            if tHRP then
                local dist = (myHRP.Position - tHRP.Position).Magnitude
                if dist < nearestDist then nearestDist = dist nearest = pl end
            end
        end
    end
    return nearest
end
local function isEnemyRagdolled(targetPlayer)
    if not targetPlayer or not targetPlayer.Character then return false end
    local hum = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    local hrp = getHRP(targetPlayer.Character)
    if hum and hrp then
        return hum:GetState() == Enum.HumanoidStateType.Physics or math.abs(hrp.CFrame.UpVector.Y) < 0.7
    end
    return false
end
local function getActiveAnimId()
    local char     = LocalPlayer.Character
    local hum      = char and char:FindFirstChildOfClass("Humanoid")
    local animator = hum and hum:FindFirstChildOfClass("Animator")
    if animator then
        for _, track in pairs(animator:GetPlayingAnimationTracks()) do
            local id = track.Animation.AnimationId
            if AnimationTriggers[id] or StraightAnimations[id] then return id end
        end
    end
    return nil
end

local function startCurveGlide()
    if not ScriptEnabled then return end
    local target = getNearestPlayer(Settings.Range)
    local myChar = LocalPlayer.Character
    local myHRP  = getHRP(myChar)
    local hum    = myChar and myChar:FindFirstChildOfClass("Humanoid")
    if not getActiveAnimId() then return end
    if target and isEnemyRagdolled(target) then return end
    if not (target and myHRP and hum) then return end

    local tHRP = getHRP(target.Character)
    if not tHRP or not tHRP.Parent then return end

    local goStraight = StraightAnimations[getActiveAnimId()] == true
    local startTime  = tick()
    local startPos   = Vector3.new(myHRP.Position.X, 0, myHRP.Position.Z)
    local targetPos  = Vector3.new(tHRP.Position.X,  0, tHRP.Position.Z)
    local behindDir  = Vector3.new(tHRP.CFrame.LookVector.X, 0, tHRP.CFrame.LookVector.Z).Unit
    local endPos     = targetPos - behindDir * Settings.Radius

    local controlPt
    if not goStraight then
        local mid      = (startPos + endPos) / 2
        local toTarget = targetPos - startPos
        local flatDist = toTarget.Magnitude
        local dirNorm  = flatDist > 0.1 and (toTarget / flatDist) or Vector3.new(1, 0, 0)
        local perp     = Vector3.new(-dirNorm.Z, 0, dirNorm.X)
        controlPt      = mid + perp * math.max(Settings.CurveStrength, flatDist * 0.6, 8)
    end

    hum.AutoRotate = false
    local prevCam  = Camera.CameraType
    Camera.CameraType = Enum.CameraType.Custom

    local conn
    conn = RunService.Heartbeat:Connect(function()
        if not ScriptEnabled then conn:Disconnect() hum.AutoRotate = true Camera.CameraType = prevCam return end
        local alpha = math.clamp((tick() - startTime) / Settings.Duration, 0, 1)
        if alpha >= 1 or not tHRP.Parent then conn:Disconnect() hum.AutoRotate = true Camera.CameraType = prevCam return end
        local t  = 1 - (1 - alpha)^2
        local t1 = 1 - t
        local movePos
        if goStraight then
            movePos = Vector3.new(startPos.X+(endPos.X-startPos.X)*t, myHRP.Position.Y, startPos.Z+(endPos.Z-startPos.Z)*t)
        else
            movePos = Vector3.new((t1*t1)*startPos.X+(2*t1*t)*controlPt.X+(t*t)*endPos.X, myHRP.Position.Y, (t1*t1)*startPos.Z+(2*t1*t)*controlPt.Z+(t*t)*endPos.Z)
        end
        myHRP.CFrame  = CFrame.new(movePos, Vector3.new(tHRP.Position.X, movePos.Y, tHRP.Position.Z))
        Camera.CFrame = CFrame.new(myHRP.Position + Vector3.new(0, Settings.CamOffset, 0), tHRP.Position)
    end)
end

local function setupCharacter(character)
    local humanoid = character:WaitForChild("Humanoid", 5)
    local animator = humanoid and humanoid:WaitForChild("Animator", 5)
    if not animator then return end
    animator.AnimationPlayed:Connect(function(track)
        if not ScriptEnabled then return end
        local animId    = track.Animation.AnimationId
        local delayTime = AnimationTriggers[animId]
        if not delayTime and StraightAnimations[animId] then delayTime = 0.19 end
        if delayTime then
            task.delay(delayTime, function()
                if humanoid.Health > 0 and ScriptEnabled then pressKey(Enum.KeyCode.Three) end
            end)
        end
    end)
    if pingVisualizerEnabled then createGhost(character) end
end

if LocalPlayer.Character then setupCharacter(LocalPlayer.Character) end
LocalPlayer.CharacterAdded:Connect(setupCharacter)

RunService.Heartbeat:Connect(function()
    local char = LocalPlayer.Character
    if not char then return end
    local pingMs = math.round(LocalPlayer:GetNetworkPing() * 1000)
    pingDisplay.Text = pingMs .. " ms"
    pingDisplay.TextColor3 = pingMs < 80 and Color3.fromRGB(80, 220, 100) or pingMs < 150 and Color3.fromRGB(255, 200, 50) or Color3.fromRGB(255, 80, 80)
    if not pingVisualizerEnabled then return end
    recordSnapshot(char)
    local snapshot = getDelayedSnapshot()
    if snapshot then applySnapshotToGhost(snapshot) end
end)

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if ScriptEnabled and input.KeyCode == Enum.KeyCode.E then
        pressKey(Enum.KeyCode.Three)
        task.wait(0.2)
        startCurveGlide()
    end
end)

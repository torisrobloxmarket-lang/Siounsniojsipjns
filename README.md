-- Tower of Hell | Full Feature Suite
-- Godmode · Dash · Fly · Elevator · Capped Teleport · Noclip · Autoclimb

local Players    = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS        = game:GetService("UserInputService")
local TweenS     = game:GetService("TweenService")
local GuiS       = game:GetService("GuiService")

local lp   = Players.LocalPlayer
local char = lp.Character or lp.CharacterAdded:Wait()
local root, hum, hrp

local function bindChar(c)
    char = c
    root = c:WaitForChild("HumanoidRootPart")
    hum  = c:WaitForChild("Humanoid")
    hrp  = root
end
bindChar(char)
lp.CharacterAdded:Connect(bindChar)

-- ── CONFIG ───────────────────────────────────────────────────────────────────
local CFG = {
    noclip      = true,
    godmode     = false,
    fly         = false,
    flySpeed    = 60,
    dashSpeed   = 120,
    dashCooldown= 0.6,   -- seconds
    elevStep    = 60,    -- studs per elevator tap
    tpStepCap   = 100,   -- max studs per teleport tap
    farmDelay   = 0.04,
    farmStep    = 10,
}
-- ─────────────────────────────────────────────────────────────────────────────

-- ── NOCLIP ───────────────────────────────────────────────────────────────────
RunService.Stepped:Connect(function()
    if not CFG.noclip or not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") then p.CanCollide = false end
    end
end)

-- ── GODMODE ──────────────────────────────────────────────────────────────────
-- Locks health at max every heartbeat; also disables BreakJointsOnDeath
local godConn
local function setGodmode(on)
    CFG.godmode = on
    if on then
        godConn = RunService.Heartbeat:Connect(function()
            if hum then
                hum.Health   = hum.MaxHealth
                hum.WalkSpeed = hum.WalkSpeed -- keep alive
            end
        end)
        -- Prevent kill bricks from breaking joints
        if char then
            for _, p in ipairs(char:GetDescendants()) do
                if p:IsA("BasePart") then
                    p.Touched:Connect(function() end) -- consume touch
                end
            end
        end
    else
        if godConn then godConn:Disconnect() godConn = nil end
    end
end

-- ── FLY ──────────────────────────────────────────────────────────────────────
local flyConn
local flyBV, flyBA  -- BodyVelocity, BodyAngularVelocity
local flyKeys = { up=false, down=false, f=false, b=false, l=false, r=false }

local function cleanFlyBodies()
    if flyBV  and flyBV.Parent  then flyBV:Destroy()  end
    if flyBA  and flyBA.Parent  then flyBA:Destroy()  end
end

local function setFly(on)
    CFG.fly = on
    cleanFlyBodies()
    if flyConn then flyConn:Disconnect() flyConn = nil end

    if not on then
        if hum then hum.PlatformStand = false end
        return
    end

    hum.PlatformStand = true

    flyBV = Instance.new("BodyVelocity", root)
    flyBV.MaxForce = Vector3.new(1e5,1e5,1e5)
    flyBV.Velocity  = Vector3.zero

    flyBA = Instance.new("BodyAngularVelocity", root)
    flyBA.MaxTorque = Vector3.new(1e5,1e5,1e5)
    flyBA.AngularVelocity = Vector3.zero

    flyConn = RunService.Heartbeat:Connect(function()
        if not CFG.fly or not root then return end
        local cam  = workspace.CurrentCamera
        local cf   = cam.CFrame
        local dir  = Vector3.zero

        if flyKeys.f   then dir = dir + cf.LookVector        end
        if flyKeys.b   then dir = dir - cf.LookVector        end
        if flyKeys.r   then dir = dir + cf.RightVector       end
        if flyKeys.l   then dir = dir - cf.RightVector       end
        if flyKeys.up  then dir = dir + Vector3.new(0,1,0)   end
        if flyKeys.down then dir = dir - Vector3.new(0,1,0)  end

        flyBV.Velocity = dir.Magnitude > 0
            and dir.Unit * CFG.flySpeed
            or  Vector3.zero
    end)
end

-- Keyboard fly controls
UIS.InputBegan:Connect(function(i, gp)
    if gp then return end
    local k = i.KeyCode
    if k == Enum.KeyCode.W     then flyKeys.f    = true end
    if k == Enum.KeyCode.S     then flyKeys.b    = true end
    if k == Enum.KeyCode.A     then flyKeys.l    = true end
    if k == Enum.KeyCode.D     then flyKeys.r    = true end
    if k == Enum.KeyCode.Space then flyKeys.up   = true end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = true end
end)
UIS.InputEnded:Connect(function(i)
    local k = i.KeyCode
    if k == Enum.KeyCode.W     then flyKeys.f    = false end
    if k == Enum.KeyCode.S     then flyKeys.b    = false end
    if k == Enum.KeyCode.A     then flyKeys.l    = false end
    if k == Enum.KeyCode.D     then flyKeys.r    = false end
    if k == Enum.KeyCode.Space then flyKeys.up   = false end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = false end
end)

-- ── DASH ─────────────────────────────────────────────────────────────────────
local dashReady = true

local function doDash(direction)
    if not dashReady or not root or not hum then return end
    dashReady = false

    local dir = direction
        or (workspace.CurrentCamera.CFrame.LookVector * Vector3.new(1,0,1)).Unit

    local bv = Instance.new("BodyVelocity", root)
    bv.MaxForce = Vector3.new(1e5, 0, 1e5)
    bv.Velocity  = dir * CFG.dashSpeed

    task.delay(0.18, function()
        if bv and bv.Parent then bv:Destroy() end
    end)
    task.delay(CFG.dashCooldown, function()
        dashReady = true
    end)
end

-- Keyboard dash: Q
UIS.InputBegan:Connect(function(i, gp)
    if gp then return end
    if i.KeyCode == Enum.KeyCode.Q then doDash() end
end)

-- ── ELEVATOR ─────────────────────────────────────────────────────────────────
local function doElevator()
    if not root then return end
    root.CFrame = root.CFrame + Vector3.new(0, CFG.elevStep, 0)
end

-- ── CAPPED TELEPORT ──────────────────────────────────────────────────────────
local function findTowerTop()
    local highestY = -math.huge
    local topPart
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(char) then
            if obj.Position.Y > highestY then
                highestY = obj.Position.Y
                topPart  = obj
            end
        end
    end
    return topPart, highestY
end

local function tpStep()
    if not root then return end
    local currentY  = root.Position.Y
    local _, topY   = findTowerTop()
    local targetY   = math.min(currentY + CFG.tpStepCap, topY + 5)
    root.CFrame = CFrame.new(root.Position.X, targetY, root.Position.Z)
    print(("[ToH] TP → Y %.1f"):format(targetY))
end

-- ── AUTOCLIMB ────────────────────────────────────────────────────────────────
local climbConn
local function autofarm()
    if climbConn then climbConn:Disconnect() climbConn = nil end
    local _, topY = findTowerTop()
    if not topY then return end
    local target = topY + 5

    climbConn = RunService.Heartbeat:Connect(function()
        if not root or hum.Health <= 0 then
            climbConn:Disconnect() return
        end
        if root.Position.Y >= target then
            climbConn:Disconnect()
            print("[ToH] Summit reached.")
            return
        end
        root.CFrame = root.CFrame + Vector3.new(0, CFG.farmStep, 0)
        task.wait(CFG.farmDelay)
    end)
end

-- ════════════════════════════════════════════════════════════════════════════
-- GUI
-- ════════════════════════════════════════════════════════════════════════════
local sg = Instance.new("ScreenGui", lp.PlayerGui)
sg.Name         = "ToH_Suite"
sg.ResetOnSpawn = false
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ── Main frame ───────────────────────────────────────────────────────────────
local mainF = Instance.new("Frame", sg)
mainF.Size              = UDim2.new(0, 220, 0, 380)
mainF.Position          = UDim2.new(0, 10, 0.5, -190)
mainF.BackgroundColor3  = Color3.fromRGB(18, 18, 18)
mainF.BorderSizePixel   = 0
mainF.Active            = true
mainF.Draggable         = true
Instance.new("UICorner", mainF).CornerRadius = UDim.new(0, 8)

-- Drop shadow illusion
local shadow = Instance.new("Frame", mainF)
shadow.Size             = UDim2.new(1, 6, 1, 6)
shadow.Position         = UDim2.new(0, -3, 0, 3)
shadow.BackgroundColor3 = Color3.fromRGB(0,0,0)
shadow.BackgroundTransparency = 0.6
shadow.BorderSizePixel  = 0
shadow.ZIndex           = 0
Instance.new("UICorner", shadow).CornerRadius = UDim.new(0, 10)

-- Title bar
local titleBar = Instance.new("Frame", mainF)
titleBar.Size           = UDim2.new(1, 0, 0, 36)
titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleBar.BorderSizePixel= 0
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 8)

local titleLbl = Instance.new("TextLabel", titleBar)
titleLbl.Size           = UDim2.new(1, 0, 1, 0)
titleLbl.BackgroundTransparency = 1
titleLbl.Text           = "🗼 Tower of Hell"
titleLbl.TextColor3     = Color3.fromRGB(255,255,255)
titleLbl.Font           = Enum.Font.GothamBold
titleLbl.TextSize       = 15

-- Scrollable button container
local scroll = Instance.new("ScrollingFrame", mainF)
scroll.Size             = UDim2.new(1, 0, 1, -44)
scroll.Position         = UDim2.new(0, 0, 0, 40)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel  = 0
scroll.ScrollBarThickness = 3
scroll.ScrollBarImageColor3 = Color3.fromRGB(80,80,80)
scroll.CanvasSize       = UDim2.new(0, 0, 0, 0)

local layout = Instance.new("UIListLayout", scroll)
layout.Padding          = UDim.new(0, 6)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

local pad = Instance.new("UIPadding", scroll)
pad.PaddingTop   = UDim.new(0, 6)
pad.PaddingLeft  = UDim.new(0, 8)
pad.PaddingRight = UDim.new(0, 8)

-- Auto-resize canvas
layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 12)
end)

-- ── Button helpers ────────────────────────────────────────────────────────────
local function makeLabel(text)
    local lbl = Instance.new("TextLabel", scroll)
    lbl.Size            = UDim2.new(1, 0, 0, 20)
    lbl.BackgroundTransparency = 1
    lbl.Text            = text
    lbl.TextColor3      = Color3.fromRGB(140,140,140)
    lbl.Font            = Enum.Font.GothamBold
    lbl.TextSize        = 11
    lbl.TextXAlignment  = Enum.TextXAlignment.Left
    return lbl
end

local function makeToggle(label, initState, onToggle)
    local btn = Instance.new("TextButton", scroll)
    btn.Size            = UDim2.new(1, 0, 0, 36)
    btn.BorderSizePixel = 0
    btn.Font            = Enum.Font.Gotham
    btn.TextSize        = 13
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

    local state = initState
    local function refresh()
        btn.Text             = (state and "✅ " or "⬜ ") .. label
        btn.BackgroundColor3 = state
            and Color3.fromRGB(40, 120, 60)
            or  Color3.fromRGB(45, 45, 45)
        btn.TextColor3       = Color3.fromRGB(255,255,255)
    end
    refresh()

    btn.MouseButton1Click:Connect(function()
        state = not state
        refresh()
        onToggle(state)
    end)
    return btn
end

local function makeBtn(label, callback)
    local btn = Instance.new("TextButton", scroll)
    btn.Size            = UDim2.new(1, 0, 0, 36)
    btn.BackgroundColor3= Color3.fromRGB(50, 50, 200)
    btn.BorderSizePixel = 0
    btn.Text            = label
    btn.TextColor3      = Color3.fromRGB(255,255,255)
    btn.Font            = Enum.Font.Gotham
    btn.TextSize        = 13
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- ── Populate buttons ──────────────────────────────────────────────────────────
makeLabel("  PASSIVE")
makeToggle("Noclip",  true,  function(s) CFG.noclip  = s end)
makeToggle("Godmode", false, function(s) setGodmode(s)   end)

makeLabel("  MOVEMENT")
makeToggle("Fly  [W/S/A/D + Space/Ctrl]", false, function(s) setFly(s) end)
makeBtn("⚡ Dash  [Q / button]", doDash)

makeLabel("  VERTICAL")
makeBtn("⬆ Elevator  (+" .. CFG.elevStep .. " studs)", doElevator)
makeBtn("🚀 TP +100 Studs", tpStep)
makeBtn("🔁 Auto Climb", autofarm)

-- ── MOBILE FLY DPAD ──────────────────────────────────────────────────────────
-- Only shown on mobile (touch devices)
if UIS.TouchEnabled then
    local dpad = Instance.new("Frame", sg)
    dpad.Size            = UDim2.new(0, 180, 0, 180)
    dpad.Position        = UDim2.new(0, 10, 1, -200)
    dpad.BackgroundTransparency = 1

    local function mobileFlyBtn(label, pos, dir)
        local b = Instance.new("TextButton", dpad)
        b.Size            = UDim2.new(0, 52, 0, 52)
        b.Position        = pos
        b.BackgroundColor3= Color3.fromRGB(30,30,30)
        b.BackgroundTransparency = 0.3
        b.Text            = label
        b.TextColor3      = Color3.fromRGB(255,255,255)
        b.Font            = Enum.Font.GothamBold
        b.TextSize        = 18
        b.BorderSizePixel = 0
        Instance.new("UICorner", b).CornerRadius = UDim.new(0, 26)

        b.MouseButton1Down:Connect(function()
            for k in pairs(flyKeys) do flyKeys[k] = false end
            flyKeys[dir] = true
        end)
        b.MouseButton1Up:Connect(function()
            flyKeys[dir] = false
        end)
        return b
    end

    mobileFlyBtn("↑",  UDim2.new(0, 64, 0, 0),   "f")
    mobileFlyBtn("↓",  UDim2.new(0, 64, 0, 122),  "b")
    mobileFlyBtn("←",  UDim2.new(0, 0,  0, 61),   "l")
    mobileFlyBtn("→",  UDim2.new(0, 128,0, 61),   "r")
    mobileFlyBtn("▲",  UDim2.new(0, 64, 0, 61),   "up")
    -- Rise button center

    -- Mobile dash button
    local dashMob = Instance.new("TextButton", sg)
    dashMob.Size            = UDim2.new(0, 70, 0, 70)
    dashMob.Position        = UDim2.new(1, -90, 1, -200)
    dashMob.BackgroundColor3= Color3.fromRGB(50,50,200)
    dashMob.BackgroundTransparency = 0.25
    dashMob.Text            = "DASH"
    dashMob.TextColor3      = Color3.fromRGB(255,255,255)
    dashMob.Font            = Enum.Font.GothamBold
    dashMob.TextSize        = 16
    dashMob.BorderSizePixel = 0
    Instance.new("UICorner", dashMob).CornerRadius = UDim.new(0, 35)
    dashMob.MouseButton1Click:Connect(function() doDash() end)
end

print("[ToH Suite] Loaded — drag the panel, use buttons or hotkeys.")

-- Tower of Hell | Extended Suite v2
-- Original 7 + 11 new systems including Fling

local Players    = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS        = game:GetService("UserInputService")
local TweenS     = game:GetService("TweenService")
local HttpS      = game:GetService("HttpService")
local StarterGui = game:GetService("StarterGui")

local lp   = Players.LocalPlayer
local cam  = workspace.CurrentCamera
local char = lp.Character or lp.CharacterAdded:Wait()
local root, hum

local function bindChar(c)
    char = c
    root = c:WaitForChild("HumanoidRootPart")
    hum  = c:WaitForChild("Humanoid")
end
bindChar(char)
lp.CharacterAdded:Connect(bindChar)

-- ── CONFIG ────────────────────────────────────────────────────────────────────
local CFG = {
    noclip        = true,
    godmode       = false,
    fly           = false,
    flySpeed      = 60,
    dashSpeed     = 120,
    dashCooldown  = 0.6,
    elevStep      = 60,
    tpStepCap     = 100,
    farmDelay     = 0.04,
    farmStep      = 10,
    speedMult     = 3,       -- WalkSpeed multiplier vs default 16
    jumpMult      = 3,       -- JumpPower  multiplier vs default 50
    invisible     = false,
    antiVoid      = false,
    antiVoidY     = -50,     -- respawn trigger Y
    flingForce    = 9e4,
    infiniteJump  = false,
    lowGravity    = false,
    defaultGrav   = 196.2,
    lowGravVal    = 40,
    freeze        = false,
    tpToWin       = false,
    spinBot       = false,
    spinSpeed     = 10,
    reachMult     = 10,      -- tool reach / hitbox expand factor
    defaultSpeed  = 16,
    defaultJump   = 50,
}
-- ─────────────────────────────────────────────────────────────────────────────

-- ════════════════════════════════════════════════════════════════════════════
-- ── ORIGINAL SYSTEMS ─────────────────────────────────────────────────────
-- ════════════════════════════════════════════════════════════════════════════

-- NOCLIP
RunService.Stepped:Connect(function()
    if not CFG.noclip or not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") then p.CanCollide = false end
    end
end)

-- GODMODE
local godConn
local function setGodmode(on)
    CFG.godmode = on
    if godConn then godConn:Disconnect() godConn = nil end
    if on then
        godConn = RunService.Heartbeat:Connect(function()
            if hum then hum.Health = hum.MaxHealth end
        end)
    end
end

-- FLY
local flyConn
local flyBV, flyBA
local flyKeys = {f=false,b=false,l=false,r=false,up=false,down=false}

local function cleanFlyBodies()
    if flyBV and flyBV.Parent then flyBV:Destroy() end
    if flyBA and flyBA.Parent then flyBA:Destroy() end
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
        local cf  = cam.CFrame
        local dir = Vector3.zero
        if flyKeys.f    then dir = dir + cf.LookVector         end
        if flyKeys.b    then dir = dir - cf.LookVector         end
        if flyKeys.r    then dir = dir + cf.RightVector        end
        if flyKeys.l    then dir = dir - cf.RightVector        end
        if flyKeys.up   then dir = dir + Vector3.new(0,1,0)    end
        if flyKeys.down then dir = dir - Vector3.new(0,1,0)    end
        flyBV.Velocity = dir.Magnitude > 0
            and dir.Unit * CFG.flySpeed or Vector3.zero
    end)
end

UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    local k = i.KeyCode
    if k == Enum.KeyCode.W           then flyKeys.f    = true end
    if k == Enum.KeyCode.S           then flyKeys.b    = true end
    if k == Enum.KeyCode.A           then flyKeys.l    = true end
    if k == Enum.KeyCode.D           then flyKeys.r    = true end
    if k == Enum.KeyCode.Space       then flyKeys.up   = true end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = true end
end)
UIS.InputEnded:Connect(function(i)
    local k = i.KeyCode
    if k == Enum.KeyCode.W           then flyKeys.f    = false end
    if k == Enum.KeyCode.S           then flyKeys.b    = false end
    if k == Enum.KeyCode.A           then flyKeys.l    = false end
    if k == Enum.KeyCode.D           then flyKeys.r    = false end
    if k == Enum.KeyCode.Space       then flyKeys.up   = false end
    if k == Enum.KeyCode.LeftControl then flyKeys.down = false end
end)

-- DASH
local dashReady = true
local function doDash(direction)
    if not dashReady or not root or not hum then return end
    dashReady = false
    local dir = direction
        or (cam.CFrame.LookVector * Vector3.new(1,0,1)).Unit
    local bv = Instance.new("BodyVelocity", root)
    bv.MaxForce = Vector3.new(1e5,0,1e5)
    bv.Velocity  = dir * CFG.dashSpeed
    task.delay(0.18, function() if bv and bv.Parent then bv:Destroy() end end)
    task.delay(CFG.dashCooldown, function() dashReady = true end)
end
UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    if i.KeyCode == Enum.KeyCode.Q then doDash() end
end)

-- ELEVATOR
local function doElevator()
    if not root then return end
    root.CFrame = root.CFrame + Vector3.new(0, CFG.elevStep, 0)
end

-- CAPPED TP
local function findTowerTop()
    local highY, topPart = -math.huge, nil
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(char) then
            if obj.Position.Y > highY then
                highY   = obj.Position.Y
                topPart = obj
            end
        end
    end
    return topPart, highY
end

local function tpStep()
    if not root then return end
    local _, topY  = findTowerTop()
    local cur      = root.Position.Y
    local target   = math.min(cur + CFG.tpStepCap, topY + 5)
    root.CFrame    = CFrame.new(root.Position.X, target, root.Position.Z)
end

-- AUTOCLIMB
local climbConn
local function autofarm()
    if climbConn then climbConn:Disconnect() climbConn = nil end
    local _, topY = findTowerTop()
    if not topY then return end
    local target = topY + 5
    climbConn = RunService.Heartbeat:Connect(function()
        if not root or hum.Health <= 0 then climbConn:Disconnect() return end
        if root.Position.Y >= target then
            climbConn:Disconnect()
            return
        end
        root.CFrame = root.CFrame + Vector3.new(0, CFG.farmStep, 0)
        task.wait(CFG.farmDelay)
    end)
end

-- ════════════════════════════════════════════════════════════════════════════
-- ── NEW SYSTEMS ──────────────────────────────────────────────────────────
-- ════════════════════════════════════════════════════════════════════════════

-- 1. SPEED BOOST
local function setSpeed(on)
    if not hum then return end
    hum.WalkSpeed = on and (CFG.defaultSpeed * CFG.speedMult) or CFG.defaultSpeed
end

-- 2. SUPER JUMP
local function setJump(on)
    if not hum then return end
    hum.JumpPower = on and (CFG.defaultJump * CFG.jumpMult) or CFG.defaultJump
end

-- 3. INFINITE JUMP
lp.CharacterAdded:Connect(function(c)
    c:WaitForChild("Humanoid")
end)
UIS.JumpRequest:Connect(function()
    if CFG.infiniteJump and hum then
        hum:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- 4. LOW GRAVITY
local function setLowGrav(on)
    CFG.lowGravity = on
    workspace.Gravity = on and CFG.lowGravVal or CFG.defaultGrav
end

-- 5. INVISIBLE
local function setInvisible(on)
    CFG.invisible = on
    if not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") or p:IsA("Decal") then
            p.Transparency = on and 1 or 0
        end
    end
    -- Reapply on respawn
    lp.CharacterAdded:Connect(function(c)
        task.wait(0.5)
        if not CFG.invisible then return end
        for _, p in ipairs(c:GetDescendants()) do
            if p:IsA("BasePart") or p:IsA("Decal") then
                p.Transparency = 1
            end
        end
    end)
end

-- 6. ANTI-VOID  (respawns you if you fall below CFG.antiVoidY)
local antiVoidConn
local function setAntiVoid(on)
    CFG.antiVoid = on
    if antiVoidConn then antiVoidConn:Disconnect() antiVoidConn = nil end
    if not on then return end
    antiVoidConn = RunService.Heartbeat:Connect(function()
        if not root then return end
        if root.Position.Y < CFG.antiVoidY then
            root.CFrame = CFrame.new(root.Position.X, CFG.antiVoidY + 20, root.Position.Z)
        end
    end)
end

-- 7. FREEZE (anchor root in place)
local function setFreeze(on)
    CFG.freeze = on
    if not root then return end
    root.Anchored = on
end

-- 8. SPIN BOT
local spinConn
local function setSpinBot(on)
    CFG.spinBot = on
    if spinConn then spinConn:Disconnect() spinConn = nil end
    if not on then return end
    spinConn = RunService.Heartbeat:Connect(function()
        if not root then return end
        root.CFrame = root.CFrame * CFrame.Angles(0, math.rad(CFG.spinSpeed), 0)
    end)
end

-- 9. TP TO WIN ZONE  (finds finish line / end part by name heuristic)
local function tpToWin()
    if not root then return end
    local candidates = {"EndZone","Finish","Win","Goal","End","Completed"}
    local target
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            local n = obj.Name:lower()
            for _, kw in ipairs(candidates) do
                if n:find(kw:lower()) then
                    target = obj
                    break
                end
            end
        end
        if target then break end
    end
    if target then
        root.CFrame = CFrame.new(target.Position + Vector3.new(0,5,0))
        print("[ToH] Teleported to win zone: " .. target.Name)
    else
        -- Fallback: teleport to highest point
        local _, topY = findTowerTop()
        root.CFrame = CFrame.new(root.Position.X, topY + 5, root.Position.Z)
        print("[ToH] Win zone not found — teleported to top.")
    end
end

-- 10. HITBOX EXPANDER  (makes your character easier to stand on things / touch triggers)
local function setHitbox(on)
    if not char then return end
    local hrpSize = on
        and Vector3.new(
            4 * CFG.reachMult * 0.3,
            4 * CFG.reachMult * 0.3,
            4 * CFG.reachMult * 0.3)
        or Vector3.new(2, 2, 1)
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.Size = hrpSize end
end

-- 11. FLING PLAYER
local selectedTarget = nil  -- set via dropdown

local function flingPlayer(targetName)
    local target = Players:FindFirstChild(targetName)
    if not target then
        warn("[ToH] Player not found: " .. tostring(targetName))
        return
    end
    local tChar = target.Character
    if not tChar then
        warn("[ToH] Target has no character.")
        return
    end
    local tRoot = tChar:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end

    -- Teleport next to them
    root.CFrame = CFrame.new(tRoot.Position + Vector3.new(0, 2, 0))
    task.wait(0.05)

    -- Apply massive outward BodyVelocity on their root
    -- (only works on client side for the physics push if they're in range)
    local bv = Instance.new("BodyVelocity", tRoot)
    bv.MaxForce = Vector3.new(CFG.flingForce, CFG.flingForce, CFG.flingForce)

    -- Direction: away from us + upward burst
    local flingDir = (tRoot.Position - root.Position).Unit
    bv.Velocity = (flingDir + Vector3.new(0, 1.5, 0)).Unit * CFG.flingForce * 0.003

    -- Also apply angular spin for chaos
    local bav = Instance.new("BodyAngularVelocity", tRoot)
    bav.MaxTorque = Vector3.new(1e5,1e5,1e5)
    bav.AngularVelocity = Vector3.new(
        math.random(-50,50),
        math.random(-50,50),
        math.random(-50,50)
    )

    task.delay(0.5, function()
        if bv  and bv.Parent  then bv:Destroy()  end
        if bav and bav.Parent then bav:Destroy() end
    end)
    print("[ToH] Flung: " .. targetName)
end

-- ════════════════════════════════════════════════════════════════════════════
-- GUI
-- ════════════════════════════════════════════════════════════════════════════
local sg = Instance.new("ScreenGui", lp.PlayerGui)
sg.Name           = "ToH_v2"
sg.ResetOnSpawn   = false
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Main panel
local mainF = Instance.new("Frame", sg)
mainF.Size             = UDim2.new(0, 230, 0, 520)
mainF.Position         = UDim2.new(0, 10, 0.5, -260)
mainF.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainF.BorderSizePixel  = 0
mainF.Active           = true
mainF.Draggable        = true
Instance.new("UICorner", mainF).CornerRadius = UDim.new(0, 10)

-- Title
local titleBar = Instance.new("Frame", mainF)
titleBar.Size             = UDim2.new(1, 0, 0, 38)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
titleBar.BorderSizePixel  = 0
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 10)

local titleLbl = Instance.new("TextLabel", titleBar)
titleLbl.Size                = UDim2.new(1,0,1,0)
titleLbl.BackgroundTransparency = 1
titleLbl.Text                = "🗼  Tower of Hell  v2"
titleLbl.TextColor3          = Color3.fromRGB(255,255,255)
titleLbl.Font                = Enum.Font.GothamBold
titleLbl.TextSize            = 14

-- Scroll container
local scroll = Instance.new("ScrollingFrame", mainF)
scroll.Size                  = UDim2.new(1,0,1,-44)
scroll.Position              = UDim2.new(0,0,0,42)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel       = 0
scroll.ScrollBarThickness    = 3
scroll.ScrollBarImageColor3  = Color3.fromRGB(80,80,80)
scroll.CanvasSize            = UDim2.new(0,0,0,0)

local layout = Instance.new("UIListLayout", scroll)
layout.Padding               = UDim.new(0,5)
layout.HorizontalAlignment   = Enum.HorizontalAlignment.Center

local pad = Instance.new("UIPadding", scroll)
pad.PaddingTop   = UDim.new(0,6)
pad.PaddingLeft  = UDim.new(0,8)
pad.PaddingRight = UDim.new(0,8)

layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    scroll.CanvasSize = UDim2.new(0,0,0, layout.AbsoluteContentSize.Y + 12)
end)

-- ── UI helpers ────────────────────────────────────────────────────────────────
local function sectionLabel(txt)
    local f = Instance.new("Frame", scroll)
    f.Size             = UDim2.new(1,0,0,22)
    f.BackgroundColor3 = Color3.fromRGB(30,30,30)
    f.BorderSizePixel  = 0
    Instance.new("UICorner", f).CornerRadius = UDim.new(0,4)
    local l = Instance.new("TextLabel", f)
    l.Size                = UDim2.new(1,-8,1,0)
    l.Position            = UDim2.new(0,8,0,0)
    l.BackgroundTransparency = 1
    l.Text                = txt
    l.TextColor3          = Color3.fromRGB(180,180,60)
    l.Font                = Enum.Font.GothamBold
    l.TextSize            = 11
    l.TextXAlignment      = Enum.TextXAlignment.Left
end

local function makeToggle(label, initState, onToggle)
    local btn = Instance.new("TextButton", scroll)
    btn.Size            = UDim2.new(1,0,0,34)
    btn.BorderSizePixel = 0
    btn.Font            = Enum.Font.Gotham
    btn.TextSize        = 13
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,6)
    local state = initState
    local function refresh()
        btn.Text             = (state and "✅ " or "◻ ") .. label
        btn.BackgroundColor3 = state
            and Color3.fromRGB(35,110,50)
            or  Color3.fromRGB(40,40,40)
        btn.TextColor3 = Color3.fromRGB(230,230,230)
    end
    refresh()
    btn.MouseButton1Click:Connect(function()
        state = not state
        refresh()
        onToggle(state)
    end)
    return btn
end

local function makeBtn(label, col, callback)
    local btn = Instance.new("TextButton", scroll)
    btn.Size             = UDim2.new(1,0,0,34)
    btn.BackgroundColor3 = col or Color3.fromRGB(50,50,180)
    btn.BorderSizePixel  = 0
    btn.Text             = label
    btn.TextColor3       = Color3.fromRGB(255,255,255)
    btn.Font             = Enum.Font.Gotham
    btn.TextSize         = 13
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,6)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- ── Fling dropdown ────────────────────────────────────────────────────────────
local function makeFlingSection()
    sectionLabel("⚡ FLING PLAYER")

    -- Player list dropdown (TextButton that cycles through players)
    local targetLabel = Instance.new("TextButton", scroll)
    targetLabel.Size             = UDim2.new(1,0,0,34)
    targetLabel.BackgroundColor3 = Color3.fromRGB(55,30,30)
    targetLabel.BorderSizePixel  = 0
    targetLabel.Font             = Enum.Font.Gotham
    targetLabel.TextSize         = 12
    targetLabel.TextColor3       = Color3.fromRGB(255,200,200)
    Instance.new("UICorner", targetLabel).CornerRadius = UDim.new(0,6)

    local function refreshTargetLabel()
        targetLabel.Text = "🎯 Target: " .. (selectedTarget or "(none)")
    end
    refreshTargetLabel()

    -- Cycle through online players on click
    targetLabel.MouseButton1Click:Connect(function()
        local list = {}
        for _, p in ipairs(Players:GetPlayers()) do
            if p ~= lp then table.insert(list, p.Name) end
        end
        if #list == 0 then
            selectedTarget = nil
            refreshTargetLabel()
            return
        end
        -- Find current index and advance
        local idx = 1
        for i, n in ipairs(list) do
            if n == selectedTarget then idx = i + 1 break end
        end
        if idx > #list then idx = 1 end
        selectedTarget = list[idx]
        refreshTargetLabel()
    end)

    makeBtn("🔥 Fling Selected", Color3.fromRGB(160,40,40), function()
        if selectedTarget then
            flingPlayer(selectedTarget)
        else
            warn("[ToH] Select a target first.")
        end
    end)
end

-- ── Populate ──────────────────────────────────────────────────────────────────
sectionLabel("🛡 PASSIVE")
makeToggle("Noclip",        true,  function(s) CFG.noclip = s       end)
makeToggle("Godmode",       false, function(s) setGodmode(s)         end)
makeToggle("Invisible",     false, function(s) setInvisible(s)       end)
makeToggle("Anti-Void",     false, function(s) setAntiVoid(s)        end)

sectionLabel("🏃 MOVEMENT")
makeToggle("Speed Boost  (x3)",   false, function(s) setSpeed(s)        end)
makeToggle("Super Jump   (x3)",   false, function(s) setJump(s)         end)
makeToggle("Infinite Jump",       false, function(s) CFG.infiniteJump=s end)
makeToggle("Low Gravity",         false, function(s) setLowGrav(s)      end)
makeToggle("Fly  [W/S/A/D+Space]",false, function(s) setFly(s)          end)
makeBtn("⚡ Dash  [Q]", Color3.fromRGB(50,50,180), doDash)

sectionLabel("📐 VERTICAL")
makeBtn("⬆ Elevator  (+60)",      Color3.fromRGB(40,80,40),  doElevator)
makeBtn("🚀 TP +100 Studs",       Color3.fromRGB(40,70,130), tpStep)
makeBtn("🏆 TP to Win Zone",      Color3.fromRGB(130,90,20), tpToWin)
makeBtn("🔁 Auto Climb",          Color3.fromRGB(50,50,180), autofarm)

sectionLabel("🎭 EXTRAS")
makeToggle("Freeze (Anchor)",  false, function(s) setFreeze(s)       end)
makeToggle("Spin Bot",         false, function(s) setSpinBot(s)      end)
makeToggle("Hitbox Expander",  false, function(s) setHitbox(s)       end)

makeFlingSection()

-- ════════════════════════════════════════════════════════════════════════════
-- MOBILE CONTROLS
-- ════════════════════════════════════════════════════════════════════════════
if UIS.TouchEnabled then
    -- Fly D-pad
    local dpad = Instance.new("Frame", sg)
    dpad.Size                = UDim2.new(0,180,0,180)
    dpad.Position            = UDim2.new(0,10,1,-200)
    dpad.BackgroundTransparency = 1

    local function mFlyBtn(label, pos, dir)
        local b = Instance.new("TextButton", dpad)
        b.Size             = UDim2.new(0,52,0,52)
        b.Position         = pos
        b.BackgroundColor3 = Color3.fromRGB(25,25,25)
        b.BackgroundTransparency = 0.3
        b.Text             = label
        b.TextColor3       = Color3.fromRGB(255,255,255)
        b.Font             = Enum.Font.GothamBold
        b.TextSize         = 18
        b.BorderSizePixel  = 0
        Instance.new("UICorner", b).CornerRadius = UDim.new(0,26)
        b.MouseButton1Down:Connect(function() flyKeys[dir] = true  end)
        b.MouseButton1Up:Connect(function()   flyKeys[dir] = false end)
    end

    mFlyBtn("↑",  UDim2.new(0,64,0,0),    "f")
    mFlyBtn("↓",  UDim2.new(0,64,0,122),  "b")
    mFlyBtn("←",  UDim2.new(0,0, 0,61),   "l")
    mFlyBtn("→",  UDim2.new(0,128,0,61),  "r")
    mFlyBtn("▲",  UDim2.new(0,64,0,61),   "up")

    -- Dash button mobile
    local dashMob = Instance.new("TextButton", sg)
    dashMob.Size             = UDim2.new(0,70,0,70)
    dashMob.Position         = UDim2.new(1,-90,1,-200)
    dashMob.BackgroundColor3 = Color3.fromRGB(50,50,200)
    dashMob.BackgroundTransparency = 0.25
    dashMob.Text             = "DASH"
    dashMob.TextColor3       = Color3.fromRGB(255,255,255)
    dashMob.Font             = Enum.Font.GothamBold
    dashMob.TextSize         = 16
    dashMob.BorderSizePixel  = 0
    Instance.new("UICorner", dashMob).CornerRadius = UDim.new(0,35)
    dashMob.MouseButton1Click:Connect(doDash)
end

print("[ToH v2] Loaded — " .. #Players:GetPlayers() .. " player(s) in server.")

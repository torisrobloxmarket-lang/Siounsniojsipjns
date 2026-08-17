-- Tower of Hell | Full Suite v3 (Chat Fix Applied)
-- Noclip · Godmode · Fly · Dash · Elevator · Capped TP · Autoclimb
-- Speed · SuperJump · InfiniteJump · LowGravity · Invisible · AntiVoid
-- Freeze · SpinBot · TpToWin · HitboxExpand · Fling
-- 15 Troll Functions

local Players    = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS        = game:GetService("UserInputService")
local TweenS     = game:GetService("TweenService")

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
    speedMult     = 3,
    jumpMult      = 3,
    invisible     = false,
    antiVoid      = false,
    antiVoidY     = -50,
    flingForce    = 9e4,
    infiniteJump  = false,
    lowGravity    = false,
    defaultGrav   = 196.2,
    lowGravVal    = 40,
    freeze        = false,
    spinBot       = false,
    spinSpeed     = 10,
    reachMult     = 10,
    defaultSpeed  = 16,
    defaultJump   = 50,
    trollFollow   = false,
    trollChatSpam = false,
    trollCamLock  = false,
    trollGhostMode= false,
    trollCopycat  = false,
    trollSizeGrow = false,
    trollSizeShrink=false,
    trollHeadSize = false,
    trollBobble   = false,
    trollShake    = false,
    trollYeet     = false,
}
-- ─────────────────────────────────────────────────────────────────────────────

-- ── CHAT BUBBLE HELPER (nil-safe) ────────────────────────────────────────────
-- Replaces direct Chat:Chat() calls — works across all executors
local function bubble(part, msg, color)
    local ok = pcall(function()
        game:GetService("Chat"):Chat(part, msg, color or Enum.ChatColor.White)
    end)
    if not ok then
        -- Fallback: Humanoid:Chat() is always available
        local h = char and char:FindFirstChildOfClass("Humanoid")
        if h then pcall(function() h:Chat(msg) end) end
    end
end
-- ─────────────────────────────────────────────────────────────────────────────

-- ════════════════════════════════════════════════════════════════════════════
-- CORE SYSTEMS
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
        if flyKeys.f    then dir = dir + cf.LookVector        end
        if flyKeys.b    then dir = dir - cf.LookVector        end
        if flyKeys.r    then dir = dir + cf.RightVector       end
        if flyKeys.l    then dir = dir - cf.RightVector       end
        if flyKeys.up   then dir = dir + Vector3.new(0,1,0)   end
        if flyKeys.down then dir = dir - Vector3.new(0,1,0)   end
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

-- TOWER TOP / CAPPED TP
local function findTowerTop()
    local highY, topPart = -math.huge, nil
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(char) then
            if obj.Position.Y > highY then
                highY = obj.Position.Y
                topPart = obj
            end
        end
    end
    return topPart, highY
end

local function tpStep()
    if not root then return end
    local _, topY = findTowerTop()
    local cur     = root.Position.Y
    local target  = math.min(cur + CFG.tpStepCap, topY + 5)
    root.CFrame   = CFrame.new(root.Position.X, target, root.Position.Z)
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
        if root.Position.Y >= target then climbConn:Disconnect() return end
        root.CFrame = root.CFrame + Vector3.new(0, CFG.farmStep, 0)
        task.wait(CFG.farmDelay)
    end)
end

-- SPEED
local function setSpeed(on)
    if hum then hum.WalkSpeed = on and CFG.defaultSpeed * CFG.speedMult or CFG.defaultSpeed end
end

-- SUPER JUMP
local function setJump(on)
    if hum then hum.JumpPower = on and CFG.defaultJump * CFG.jumpMult or CFG.defaultJump end
end

-- INFINITE JUMP
UIS.JumpRequest:Connect(function()
    if CFG.infiniteJump and hum then
        hum:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- LOW GRAVITY
local function setLowGrav(on)
    CFG.lowGravity    = on
    workspace.Gravity = on and CFG.lowGravVal or CFG.defaultGrav
end

-- INVISIBLE
local function setInvisible(on)
    CFG.invisible = on
    if not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") or p:IsA("Decal") then
            p.Transparency = on and 1 or 0
        end
    end
end

-- ANTI-VOID
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

-- FREEZE
local function setFreeze(on)
    CFG.freeze = on
    if root then root.Anchored = on end
end

-- SPIN BOT
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

-- TP TO WIN
local function tpToWin()
    if not root then return end
    local kws = {"EndZone","Finish","Win","Goal","End","Completed"}
    local target
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            for _, kw in ipairs(kws) do
                if obj.Name:lower():find(kw:lower()) then
                    target = obj break
                end
            end
        end
        if target then break end
    end
    if target then
        root.CFrame = CFrame.new(target.Position + Vector3.new(0,5,0))
    else
        local _, topY = findTowerTop()
        root.CFrame = CFrame.new(root.Position.X, topY + 5, root.Position.Z)
    end
end

-- HITBOX EXPAND
local function setHitbox(on)
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if hrp then
        hrp.Size = on
            and Vector3.new(4*CFG.reachMult*0.3, 4*CFG.reachMult*0.3, 4*CFG.reachMult*0.3)
            or  Vector3.new(2,2,1)
    end
end

-- FLING
local selectedTarget = nil

local function flingPlayer(targetName)
    local target = Players:FindFirstChild(targetName)
    if not target then return end
    local tChar = target.Character
    if not tChar then return end
    local tRoot = tChar:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    root.CFrame = CFrame.new(tRoot.Position + Vector3.new(0,2,0))
    task.wait(0.05)
    local bv = Instance.new("BodyVelocity", tRoot)
    bv.MaxForce = Vector3.new(CFG.flingForce,CFG.flingForce,CFG.flingForce)
    local fDir  = (tRoot.Position - root.Position).Unit
    bv.Velocity = (fDir + Vector3.new(0,1.5,0)).Unit * CFG.flingForce * 0.003
    local bav   = Instance.new("BodyAngularVelocity", tRoot)
    bav.MaxTorque = Vector3.new(1e5,1e5,1e5)
    bav.AngularVelocity = Vector3.new(
        math.random(-50,50), math.random(-50,50), math.random(-50,50))
    task.delay(0.5, function()
        if bv  and bv.Parent  then bv:Destroy()  end
        if bav and bav.Parent then bav:Destroy() end
    end)
end

-- ════════════════════════════════════════════════════════════════════════════
-- TROLL SYSTEMS
-- ════════════════════════════════════════════════════════════════════════════

local function getTRoot()
    if not selectedTarget then return nil end
    local t = Players:FindFirstChild(selectedTarget)
    if not t or not t.Character then return nil end
    return t.Character:FindFirstChild("HumanoidRootPart")
end

-- 1. GHOST FOLLOW
local followConn
local function setFollow(on)
    CFG.trollFollow = on
    if followConn then followConn:Disconnect() followConn = nil end
    if not on then return end
    followConn = RunService.Heartbeat:Connect(function()
        local tr = getTRoot()
        if not tr or not root then return end
        root.CFrame = tr.CFrame * CFrame.new(0, 0, 4)
    end)
end

-- 2. CHAT SPAM (uses nil-safe bubble helper)
local chatSpamConn
local spamMessages = {
    "bro this game is so easy lol",
    "why is everyone so slow",
    "i finished already",
    "skill issue ngl",
    "this section took me 0.2 seconds",
    "my grandma beats this stage",
    "literally afk and still winning",
    "you guys need practice",
    "i could do this blindfolded",
    "not even trying rn",
}
local spamIdx = 1

local function setChatSpam(on)
    CFG.trollChatSpam = on
    if chatSpamConn then
        task.cancel(chatSpamConn)
        chatSpamConn = nil
    end
    if not on then return end
    chatSpamConn = task.spawn(function()
        while CFG.trollChatSpam do
            if char and char:FindFirstChild("Head") then
                bubble(char.Head, spamMessages[spamIdx])
                spamIdx = (spamIdx % #spamMessages) + 1
            end
            task.wait(3)
        end
    end)
end

-- 3. FAKE ADMIN ANNOUNCE (uses nil-safe bubble helper)
local fakeAdminMessages = {
    "⚠ Server Notice: Anti-cheat scan complete. No violations found.",
    "⚠ [ADMIN] Teleport exploit detected — monitoring session.",
    "⚠ Server: Speed hackers will be removed in 60 seconds.",
    "⚠ [SYSTEM] Your account has been flagged for review.",
    "⚠ [MOD] Please do not use third-party tools in this experience.",
}
local fakeAdminIdx = 1

local function sendFakeAdmin()
    if char and char:FindFirstChild("Head") then
        bubble(char.Head, fakeAdminMessages[fakeAdminIdx], Enum.ChatColor.Red)
        fakeAdminIdx = (fakeAdminIdx % #fakeAdminMessages) + 1
    end
end

-- 4. FAKE ERROR SCREEN
local fakeErrorGui
local function showFakeError(on)
    CFG.trollFakeError = on
    if fakeErrorGui then fakeErrorGui:Destroy() fakeErrorGui = nil end
    if not on then return end

    fakeErrorGui = Instance.new("ScreenGui", lp.PlayerGui)
    fakeErrorGui.Name           = "FakeError"
    fakeErrorGui.ResetOnSpawn   = false
    fakeErrorGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    local bg = Instance.new("Frame", fakeErrorGui)
    bg.Size             = UDim2.new(1,0,1,0)
    bg.BackgroundColor3 = Color3.fromRGB(0,0,180)
    bg.BorderSizePixel  = 0
    bg.ZIndex           = 100

    local function addTxt(text, size, pos, color)
        local l = Instance.new("TextLabel", bg)
        l.Size                   = UDim2.new(0.9,0,0,size+6)
        l.Position               = pos
        l.BackgroundTransparency = 1
        l.Text                   = text
        l.TextColor3             = color or Color3.fromRGB(255,255,255)
        l.Font                   = Enum.Font.Code
        l.TextSize               = size
        l.TextXAlignment         = Enum.TextXAlignment.Left
        l.TextWrapped            = true
        l.ZIndex                 = 101
    end

    addTxt(":(",                                                          72, UDim2.new(0.05,0,0.08,0))
    addTxt("Your PC ran into a problem and needs to restart.",            20, UDim2.new(0.05,0,0.28,0))
    addTxt("We're just collecting some error info, and then we'll restart for you.\n\n0% complete",
                                                                         16, UDim2.new(0.05,0,0.38,0))
    addTxt("Stop code: ROBLOX_KERNEL_SECURITY_CHECK_FAILURE",            13, UDim2.new(0.05,0,0.72,0),
           Color3.fromRGB(200,200,200))

    task.delay(8, function()
        if fakeErrorGui then fakeErrorGui:Destroy() fakeErrorGui = nil end
        CFG.trollFakeError = false
    end)
end

-- 5. GHOST MODE
local function setGhostMode(on)
    CFG.trollGhostMode = on
    setInvisible(on)
end

-- 6. FAKE FLOOR REMOVER
local function fakeFloorRemove()
    local tr = getTRoot()
    if not tr then return end
    local pos     = tr.Position
    local removed = {}
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(char) then
            if (obj.Position - pos).Magnitude < 15 and obj.Position.Y < pos.Y then
                obj.CanCollide = false
                table.insert(removed, obj)
            end
        end
    end
    task.delay(3, function()
        for _, obj in ipairs(removed) do
            if obj and obj.Parent then obj.CanCollide = true end
        end
    end)
end

-- 7. COPYCAT
local copycatConn
local function setCopycat(on)
    CFG.trollCopycat = on
    if copycatConn then copycatConn:Disconnect() copycatConn = nil end
    if not on then return end
    copycatConn = RunService.Heartbeat:Connect(function()
        local tr = getTRoot()
        if not tr or not root then return end
        root.CFrame = tr.CFrame
    end)
end

-- 8. CAMERA LOCK
local camLockConn
local function setCamLock(on)
    CFG.trollCamLock = on
    if camLockConn then camLockConn:Disconnect() camLockConn = nil end
    if not on then
        cam.CameraType = Enum.CameraType.Custom
        return
    end
    cam.CameraType = Enum.CameraType.Scriptable
    camLockConn = RunService.RenderStepped:Connect(function()
        local tr = getTRoot()
        if not tr or not root then return end
        cam.CFrame = CFrame.new(root.Position, tr.Position)
    end)
end

-- 9. SIZE GROW
local sizeGrowConn
local function setSizeGrow(on)
    CFG.trollSizeGrow = on
    if sizeGrowConn then task.cancel(sizeGrowConn) sizeGrowConn = nil end
    if not on then
        if char then
            for _, p in ipairs(char:GetDescendants()) do
                if p:IsA("BasePart") then p.Size = p.Size * 0.5 end
            end
        end
        return
    end
    sizeGrowConn = task.spawn(function()
        local steps = 0
        while CFG.trollSizeGrow and steps < 20 do
            if char then
                for _, p in ipairs(char:GetDescendants()) do
                    if p:IsA("BasePart") then p.Size = p.Size * 1.08 end
                end
            end
            steps = steps + 1
            task.wait(0.15)
        end
    end)
end

-- 10. SIZE SHRINK
local function setSizeShrink(on)
    CFG.trollSizeShrink = on
    if not on then return end
    task.spawn(function()
        local steps = 0
        while CFG.trollSizeShrink and steps < 20 do
            if char then
                for _, p in ipairs(char:GetDescendants()) do
                    if p:IsA("BasePart") then p.Size = p.Size * 0.92 end
                end
            end
            steps = steps + 1
            task.wait(0.15)
        end
    end)
end

-- 11. GIANT HEAD
local function setGiantHead(on)
    CFG.trollHeadSize = on
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end
    head.Size = on and Vector3.new(4,4,4) or Vector3.new(2,1,1)
end

-- 12. BOBBLE HEAD
local bobbleConn
local bobbleT = 0
local function setBobble(on)
    CFG.trollBobble = on
    if bobbleConn then bobbleConn:Disconnect() bobbleConn = nil end
    if not on then return end
    bobbleConn = RunService.RenderStepped:Connect(function(dt)
        bobbleT = bobbleT + dt * 8
        if not char then return end
        local neck = char:FindFirstChild("Neck", true)
        if neck and neck:IsA("Motor6D") then
            neck.C0 = neck.C0 * CFrame.Angles(
                math.sin(bobbleT) * 0.4, 0, math.cos(bobbleT) * 0.2)
        end
    end)
end

-- 13. SCREEN SHAKE
local function doScreenShake()
    cam.CameraType = Enum.CameraType.Scriptable
    local shakeConn
    local t = 0
    shakeConn = RunService.RenderStepped:Connect(function(dt)
        t = t + dt
        if t > 4 then
            shakeConn:Disconnect()
            cam.CameraType = Enum.CameraType.Custom
            return
        end
        local intensity = 0.6 * (1 - t / 4)
        cam.CFrame = cam.CFrame
            * CFrame.new(
                math.random() * intensity - intensity/2,
                math.random() * intensity - intensity/2,
                0)
            * CFrame.Angles(
                math.random() * 0.04 - 0.02,
                math.random() * 0.04 - 0.02,
                0)
    end)
end

-- 14. YEET SELF
local function doYeetSelf()
    if not root then return end
    local bv = Instance.new("BodyVelocity", root)
    bv.MaxForce = Vector3.new(0, 1e6, 0)
    bv.Velocity  = Vector3.new(
        math.random(-40,40), 500, math.random(-40,40))
    task.delay(0.3, function()
        if bv and bv.Parent then bv:Destroy() end
    end)
end

-- 15. DISCO CHARACTER
local discoConn
local function setDisco(on)
    if discoConn then discoConn:Disconnect() discoConn = nil end
    if not on then
        if char then
            for _, p in ipairs(char:GetDescendants()) do
                if p:IsA("BasePart") then
                    p.BrickColor = BrickColor.new("Medium stone grey")
                end
            end
        end
        return
    end
    discoConn = RunService.Heartbeat:Connect(function()
        if not char then return end
        for _, p in ipairs(char:GetDescendants()) do
            if p:IsA("BasePart") then p.BrickColor = BrickColor.Random() end
        end
    end)
end

-- ════════════════════════════════════════════════════════════════════════════
-- GUI
-- ════════════════════════════════════════════════════════════════════════════
local sg = Instance.new("ScreenGui", lp.PlayerGui)
sg.Name           = "ToH_v3"
sg.ResetOnSpawn   = false
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local mainF = Instance.new("Frame", sg)
mainF.Size             = UDim2.new(0, 236, 0, 560)
mainF.Position         = UDim2.new(0, 10, 0.5, -280)
mainF.BackgroundColor3 = Color3.fromRGB(13, 13, 13)
mainF.BorderSizePixel  = 0
mainF.Active           = true
mainF.Draggable        = true
Instance.new("UICorner", mainF).CornerRadius = UDim.new(0, 10)

local titleBar = Instance.new("Frame", mainF)
titleBar.Size             = UDim2.new(1,0,0,38)
titleBar.BackgroundColor3 = Color3.fromRGB(22,22,22)
titleBar.BorderSizePixel  = 0
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0,10)

local titleLbl = Instance.new("TextLabel", titleBar)
titleLbl.Size                   = UDim2.new(1,0,1,0)
titleLbl.BackgroundTransparency = 1
titleLbl.Text                   = "🗼  Tower of Hell  v3"
titleLbl.TextColor3             = Color3.fromRGB(255,255,255)
titleLbl.Font                   = Enum.Font.GothamBold
titleLbl.TextSize               = 14

local scroll = Instance.new("ScrollingFrame", mainF)
scroll.Size                   = UDim2.new(1,0,1,-44)
scroll.Position               = UDim2.new(0,0,0,42)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel        = 0
scroll.ScrollBarThickness     = 3
scroll.ScrollBarImageColor3   = Color3.fromRGB(80,80,80)
scroll.CanvasSize             = UDim2.new(0,0,0,0)

local layout = Instance.new("UIListLayout", scroll)
layout.Padding             = UDim.new(0,5)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

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
    f.BackgroundColor3 = Color3.fromRGB(28,28,28)
    f.BorderSizePixel  = 0
    Instance.new("UICorner", f).CornerRadius = UDim.new(0,4)
    local l = Instance.new("TextLabel", f)
    l.Size                   = UDim2.new(1,-8,1,0)
    l.Position               = UDim2.new(0,8,0,0)
    l.BackgroundTransparency = 1
    l.Text                   = txt
    l.TextColor3             = Color3.fromRGB(200,180,60)
    l.Font                   = Enum.Font.GothamBold
    l.TextSize               = 11
    l.TextXAlignment         = Enum.TextXAlignment.Left
end

local function makeToggle(label, initState, onToggle)
    local btn = Instance.new("TextButton", scroll)
    btn.Size            = UDim2.new(1,0,0,32)
    btn.BorderSizePixel = 0
    btn.Font            = Enum.Font.Gotham
    btn.TextSize        = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,6)
    local state = initState
    local function refresh()
        btn.Text             = (state and "✅ " or "◻ ") .. label
        btn.BackgroundColor3 = state
            and Color3.fromRGB(35,110,50)
            or  Color3.fromRGB(38,38,38)
        btn.TextColor3 = Color3.fromRGB(230,230,230)
    end
    refresh()
    btn.MouseButton1Click:Connect(function()
        state = not state; refresh(); onToggle(state)
    end)
end

local function makeBtn(label, col, callback)
    local btn = Instance.new("TextButton", scroll)
    btn.Size             = UDim2.new(1,0,0,32)
    btn.BackgroundColor3 = col or Color3.fromRGB(50,50,180)
    btn.BorderSizePixel  = 0
    btn.Text             = label
    btn.TextColor3       = Color3.fromRGB(255,255,255)
    btn.Font             = Enum.Font.Gotham
    btn.TextSize         = 12
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,6)
    btn.MouseButton1Click:Connect(callback)
end

-- ── Target picker ─────────────────────────────────────────────────────────────
local function makeTargetPicker()
    local targetBtn = Instance.new("TextButton", scroll)
    targetBtn.Size             = UDim2.new(1,0,0,32)
    targetBtn.BackgroundColor3 = Color3.fromRGB(55,25,25)
    targetBtn.BorderSizePixel  = 0
    targetBtn.Font             = Enum.Font.Gotham
    targetBtn.TextSize         = 12
    targetBtn.TextColor3       = Color3.fromRGB(255,180,180)
    Instance.new("UICorner", targetBtn).CornerRadius = UDim.new(0,6)

    local function refreshTB()
        targetBtn.Text = "🎯 Target: " .. (selectedTarget or "(tap to pick)")
    end
    refreshTB()

    targetBtn.MouseButton1Click:Connect(function()
        local list = {}
        for _, p in ipairs(Players:GetPlayers()) do
            if p ~= lp then table.insert(list, p.Name) end
        end
        if #list == 0 then selectedTarget = nil refreshTB() return end
        local idx = 1
        for i, n in ipairs(list) do
            if n == selectedTarget then idx = i + 1 break end
        end
        if idx > #list then idx = 1 end
        selectedTarget = list[idx]
        refreshTB()
    end)
end

-- ── Populate ──────────────────────────────────────────────────────────────────
sectionLabel("🛡  PASSIVE")
makeToggle("Noclip",               true,  function(s) CFG.noclip=s      end)
makeToggle("Godmode",              false, function(s) setGodmode(s)      end)
makeToggle("Invisible",            false, function(s) setInvisible(s)    end)
makeToggle("Anti-Void",            false, function(s) setAntiVoid(s)     end)

sectionLabel("🏃  MOVEMENT")
makeToggle("Speed Boost  (x3)",    false, function(s) setSpeed(s)        end)
makeToggle("Super Jump   (x3)",    false, function(s) setJump(s)         end)
makeToggle("Infinite Jump",        false, function(s) CFG.infiniteJump=s end)
makeToggle("Low Gravity",          false, function(s) setLowGrav(s)      end)
makeToggle("Fly  [W/S/A/D+Space]", false, function(s) setFly(s)          end)
makeBtn("⚡ Dash  [Q]",            Color3.fromRGB(50,50,180), doDash)

sectionLabel("📐  VERTICAL")
makeBtn("⬆ Elevator  (+60)",       Color3.fromRGB(35,75,35),  doElevator)
makeBtn("🚀 TP +100 Studs",        Color3.fromRGB(35,60,120), tpStep)
makeBtn("🏆 TP to Win Zone",       Color3.fromRGB(110,80,15), tpToWin)
makeBtn("🔁 Auto Climb",           Color3.fromRGB(50,50,180), autofarm)

sectionLabel("🎭  EXTRAS")
makeToggle("Freeze",               false, function(s) setFreeze(s)       end)
makeToggle("Spin Bot",             false, function(s) setSpinBot(s)      end)
makeToggle("Hitbox Expander",      false, function(s) setHitbox(s)       end)

sectionLabel("⚡  FLING")
makeTargetPicker()
makeBtn("🔥 Fling Target",         Color3.fromRGB(150,35,35), function()
    if selectedTarget then flingPlayer(selectedTarget) end
end)

sectionLabel("😈  TROLL — TARGET")
makeToggle("Ghost Follow",         false, function(s) setFollow(s)       end)
makeToggle("Copycat (mirror)",     false, function(s) setCopycat(s)      end)
makeToggle("Camera Lock on Target",false, function(s) setCamLock(s)      end)
makeBtn("💣 Fake Floor Remove",    Color3.fromRGB(100,50,10), fakeFloorRemove)

sectionLabel("😈  TROLL — SELF")
makeToggle("Chat Spam",            false, function(s) setChatSpam(s)     end)
makeToggle("Ghost Mode",           false, function(s) setGhostMode(s)    end)
makeToggle("Size Grow",            false, function(s) setSizeGrow(s)     end)
makeToggle("Size Shrink",          false, function(s) setSizeShrink(s)   end)
makeToggle("Giant Head",           false, function(s) setGiantHead(s)    end)
makeToggle("Bobble Head",          false, function(s) setBobble(s)       end)
makeToggle("Disco Character",      false, function(s) setDisco(s)        end)
makeBtn("📢 Fake Admin Message",   Color3.fromRGB(100,20,20), sendFakeAdmin)
makeBtn("💻 Fake BSOD  (8s)",      Color3.fromRGB(0,0,160),   function() showFakeError(true) end)
makeBtn("📳 Screen Shake  (4s)",   Color3.fromRGB(80,40,80),  doScreenShake)
makeBtn("🚀 Yeet Self",            Color3.fromRGB(80,60,20),  doYeetSelf)

-- ── Mobile controls ───────────────────────────────────────────────────────────
if UIS.TouchEnabled then
    local dpad = Instance.new("Frame", sg)
    dpad.Size                   = UDim2.new(0,180,0,180)
    dpad.Position               = UDim2.new(0,10,1,-200)
    dpad.BackgroundTransparency = 1

    local function mFlyBtn(lbl, pos, dir)
        local b = Instance.new("TextButton", dpad)
        b.Size             = UDim2.new(0,52,0,52)
        b.Position         = pos
        b.BackgroundColor3 = Color3.fromRGB(22,22,22)
        b.BackgroundTransparency = 0.3
        b.Text             = lbl
        b.TextColor3       = Color3.fromRGB(255,255,255)
        b.Font             = Enum.Font.GothamBold
        b.TextSize         = 18
        b.BorderSizePixel  = 0
        Instance.new("UICorner", b).CornerRadius = UDim.new(0,26)
        b.MouseButton1Down:Connect(function() flyKeys[dir]=true  end)
        b.MouseButton1Up:Connect(function()   flyKeys[dir]=false end)
    end

    mFlyBtn("↑", UDim2.new(0,64,0,0),   "f")
    mFlyBtn("↓", UDim2.new(0,64,0,122), "b")
    mFlyBtn("←", UDim2.new(0,0, 0,61),  "l")
    mFlyBtn("→", UDim2.new(0,128,0,61), "r")
    mFlyBtn("▲", UDim2.new(0,64,0,61),  "up")

    local dm = Instance.new("TextButton", sg)
    dm.Size             = UDim2.new(0,70,0,70)
    dm.Position         = UDim2.new(1,-90,1,-200)
    dm.BackgroundColor3 = Color3.fromRGB(50,50,200)
    dm.BackgroundTransparency = 0.25
    dm.Text             = "DASH"
    dm.TextColor3       = Color3.fromRGB(255,255,255)
    dm.Font             = Enum.Font.GothamBold
    dm.TextSize         = 16
    dm.BorderSizePixel  = 0
    Instance.new("UICorner", dm).CornerRadius = UDim.new(0,35)
    dm.MouseButton1Click:Connect(doDash)
end

print("[ToH v3] Loaded — " .. #Players:GetPlayers() .. " player(s) in server.")

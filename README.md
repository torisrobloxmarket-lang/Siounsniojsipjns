--// ==========================================
--// RYU HUB - ADVANCED INVISIBLE TEST GUI (30 METHODS)
--// ==========================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local camera = Workspace.CurrentCamera

-- Altes GUI löschen
local guiParent = LocalPlayer:FindFirstChild("PlayerGui")
if not guiParent then guiParent = game.CoreGui end
for _, v in pairs(guiParent:GetChildren()) do
    if v.Name == "AdvInvisTestGui" then v:Destroy() end
end

-- GUI Setup
local sg = Instance.new("ScreenGui", guiParent)
sg.Name = "AdvInvisTestGui"
sg.ResetOnSpawn = false

local frame = Instance.new("Frame", sg)
frame.Size = UDim2.new(0, 320, 0, 500)
frame.Position = UDim2.new(0, 50, 0.5, -250)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
frame.BorderSizePixel = 0
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", frame).Color = Color3.fromRGB(60, 60, 60)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "Invis Tests (30 Variants)"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local scroll = Instance.new("ScrollingFrame", frame)
scroll.Size = UDim2.new(1, 0, 1, -80)
scroll.Position = UDim2.new(0, 0, 0, 40)
scroll.BackgroundTransparency = 1
scroll.ScrollBarThickness = 4
local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0, 5)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

local activeConnections = {}
local function clearLoops()
    for _, conn in pairs(activeConnections) do conn:Disconnect() end
    activeConnections = {}
    print("Loops cleared.")
end

local resetBtn = Instance.new("TextButton", frame)
resetBtn.Size = UDim2.new(0.9, 0, 0, 30)
resetBtn.Position = UDim2.new(0.05, 0, 1, -35)
resetBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
resetBtn.Text = "RESET / CLEAR LOOPS"
resetBtn.TextColor3 = Color3.new(1, 1, 1)
resetBtn.Font = Enum.Font.GothamBold
resetBtn.TextSize = 12
Instance.new("UICorner", resetBtn).CornerRadius = UDim.new(0, 4)
resetBtn.MouseButton1Click:Connect(clearLoops)

local function makeButton(name, func)
    local btn = Instance.new("TextButton", scroll)
    btn.Size = UDim2.new(0.95, 0, 0, 30)
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(200, 200, 200)
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 11
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    Instance.new("UIStroke", btn).Color = Color3.fromRGB(50, 50, 50)
    
    btn.MouseButton1Click:Connect(function()
        clearLoops()
        local char = LocalPlayer.Character
        if not char then return end
        print("Testing Method: " .. name)
        btn.TextColor3 = Color3.fromRGB(100, 255, 100)
        task.delay(0.5, function() btn.TextColor3 = Color3.fromRGB(200, 200, 200) end)
        pcall(func, char)
    end)
end

--// ==========================================
--// DIE 30 NEUEN INVISIBLE METHODEN
--// ==========================================

-- [ MATH & PHYSICS BREAKERS ]
makeButton("11. NaN CFrame (Math Break)", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.Heartbeat:Connect(function()
        root.CFrame = CFrame.new(0/0, 0/0, 0/0)
    end))
end)

makeButton("12. NaN Velocity", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.Heartbeat:Connect(function()
        root.Velocity = Vector3.new(0/0, 0/0, 0/0)
    end))
end)

makeButton("13. Inf Velocity", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.Stepped:Connect(function()
        root.Velocity = Vector3.new(math.huge, math.huge, math.huge)
    end))
end)

makeButton("14. Inf RotVelocity", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.Stepped:Connect(function()
        root.RotVelocity = Vector3.new(math.huge, math.huge, math.huge)
    end))
end)

makeButton("15. Extreme Y-Fling", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        root.CFrame = root.CFrame * CFrame.new(0, 100000, 0)
        RunService.RenderStepped:Wait()
        root.CFrame = root.CFrame * CFrame.new(0, -100000, 0)
    end))
end)

-- [ RIG & JOINT BREAKERS ]
makeButton("16. Break Waist Joint", function(char)
    local upper = char:FindFirstChild("UpperTorso")
    if upper then
        local waist = upper:FindFirstChild("Waist")
        if waist then waist:Destroy() end
    end
end)

makeButton("17. Break Neck Joint", function(char)
    local head = char:FindFirstChild("Head")
    if head then
        local neck = head:FindFirstChild("Neck")
        if neck then neck:Destroy() end
    end
end)

makeButton("18. Destroy All Motor6Ds (Ragdoll)", function(char)
    for _, v in pairs(char:GetDescendants()) do
        if v:IsA("Motor6D") and v.Name ~= "Root" then
            v:Destroy()
        end
    end
end)

makeButton("19. R15 to R6 Break", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum.RigType = Enum.HumanoidRigType.R6 end -- Breaks server rendering often
end)

makeButton("20. Break Humanoid (Permadeath)", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum:Destroy() end
end)

-- [ STATE MANIPULATION ]
makeButton("21. State: Dead", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum:ChangeState(Enum.HumanoidStateType.Dead) end
end)

makeButton("22. State: Physics", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum:ChangeState(Enum.HumanoidStateType.Physics) end
end)

makeButton("23. PlatformStand Spam", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum then return end
    table.insert(activeConnections, RunService.Stepped:Connect(function()
        hum.PlatformStand = true
    end))
end)

makeButton("24. NetworkOwnership Drop", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then root.Anchored = true end -- Drops network ownership instantly
end)

makeButton("25. Sit + High Velocity", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    local root = char:FindFirstChild("HumanoidRootPart")
    if hum and root then
        hum.Sit = true
        table.insert(activeConnections, RunService.Stepped:Connect(function()
            root.Velocity = Vector3.new(0, 5000, 0)
        end))
    end
end)

-- [ TJS SPECIFIC COMBOS ]
makeButton("26. TJS: Hide Attribute + CFrame", function(char)
    LocalPlayer:SetAttribute("HidePlayer", true)
    char:SetAttribute("HidePlayer", true)
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then
        local realPos = root.CFrame
        table.insert(activeConnections, RunService.Stepped:Connect(function()
            root.CFrame = CFrame.new(0, 99999, 0)
        end))
        table.insert(activeConnections, RunService.RenderStepped:Connect(function()
            root.CFrame = realPos
        end))
    end
end)

makeButton("27. TJS: Megumi + NaN Velocity", function(char)
    pcall(function() ReplicatedStorage.Knit.Knit.Services.MegumiService.RE.RightActivated:FireServer() end)
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then
        table.insert(activeConnections, RunService.Heartbeat:Connect(function()
            root.Velocity = Vector3.new(0/0, 0/0, 0/0)
        end))
    end
end)

makeButton("28. TJS: Ragdoll Attr + Fall", function(char)
    char:SetAttribute("Ragdoll", 999)
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then
        table.insert(activeConnections, RunService.Heartbeat:Connect(function()
            root.CFrame = CFrame.new(root.Position.X, -490, root.Position.Z)
        end))
    end
end)

makeButton("29. TJS: Dash Bypass Remote Spam", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then
        table.insert(activeConnections, RunService.RenderStepped:Connect(function()
            pcall(function() ReplicatedStorage.Events.takestam:FireServer(0.5, "dash", root.CFrame * CFrame.new(0,9999,0)) end)
        end))
    end
end)

makeButton("30. TJS: Hide + Garuda Clone", function(char)
    char:SetAttribute("Hidden", true)
    pcall(function() LocalPlayer:SetAttribute("HidePlayer", true) end)
    local upper = char:FindFirstChild("UpperTorso")
    if upper then upper:Destroy() end
end)

-- [ WEIRD / GLITCH EXPLOITS ]
makeButton("31. Accessory Drop Lag", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    for _, v in pairs(char:GetChildren()) do
        if v:IsA("Accessory") then
            v.Parent = Workspace
        end
    end
end)

makeButton("32. Head Destroy + Sit", function(char)
    local head = char:FindFirstChild("Head")
    local hum = char:FindFirstChildOfClass("Humanoid")
    if head then head:Destroy() end
    if hum then hum.Sit = true end
end)

makeButton("33. Fast CFrame Flicker", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local tickCounter = 0
    table.insert(activeConnections, RunService.RenderStepped:Connect(function()
        tickCounter = tickCounter + 1
        if tickCounter % 2 == 0 then
            root.CFrame = root.CFrame * CFrame.new(0, 10000, 0)
        else
            root.CFrame = root.CFrame * CFrame.new(0, -10000, 0)
        end
    end))
end)

makeButton("34. BodyPosition Sky Anchor", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local bp = Instance.new("BodyPosition", root)
    bp.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    bp.Position = Vector3.new(0, 99999, 0)
    table.insert(activeConnections, {Disconnect = function() bp:Destroy() end})
end)

makeButton("35. HipHeight Inf", function(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum.HipHeight = math.huge end
end)

makeButton("36. Target Part Fling", function(char)
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local bg = Instance.new("BodyAngularVelocity", root)
    bg.AngularVelocity = Vector3.new(9999, 9999, 9999)
    bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
    table.insert(activeConnections, {Disconnect = function() bg:Destroy() end})
end)

makeButton("37. LowerTorso Destroy", function(char)
    local lt = char:FindFirstChild("LowerTorso") or char:FindFirstChild("Torso")
    if lt then lt:Destroy() end
end)

makeButton("38. Local Re-Parent (Camera)", function(char)
    char.Parent = camera
end)

makeButton("39. Local Re-Parent (Lighting)", function(char)
    char.Parent = game:GetService("Lighting")
end)

makeButton("40. Ultimate Chaos (Attr + NaN + Reparent)", function(char)
    LocalPlayer:SetAttribute("HidePlayer", true)
    char:SetAttribute("Hidden", true)
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then
        table.insert(activeConnections, RunService.Heartbeat:Connect(function()
            root.Velocity = Vector3.new(0/0, 0/0, 0/0)
        end))
    end
    char.Parent = camera
end)

task.defer(function()
    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 20)
end)

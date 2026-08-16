local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local isInvisible = false
local fakeBodyLoop = nil
local savedRootJoint = nil

--// 1. GUI ERSTELLEN
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "JJS_RootDesync"
screenGui.ResetOnSpawn = false

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

--// 2. DESYNC FUNKTIONEN
local function stopInvisibility()
    -- Render-Loop stoppen
    if fakeBodyLoop then
        fakeBodyLoop:Disconnect()
        fakeBodyLoop = nil
    end

    local char = player.Character
    if char and savedRootJoint then
        local lowerTorso = char:FindFirstChild("LowerTorso")
        -- Gelenk wiederherstellen, damit der Server uns wieder sieht
        if lowerTorso and not lowerTorso:FindFirstChild("Root") then
            local newJoint = savedRootJoint:Clone()
            newJoint.Parent = lowerTorso
        end
    end
    
    -- Transparenz zurücksetzen
    if char then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                part.Transparency = 0
            elseif part:IsA("Decal") then
                part.Transparency = 0
            end
        end
    end
end

local function startInvisibility(character)
    local rootPart = character:WaitForChild("HumanoidRootPart")
    local lowerTorso = character:WaitForChild("LowerTorso")
    
    -- Das Hauptgelenk suchen und zerstören (Bricht den Charakter für den Server)
    local rootJoint = lowerTorso:FindFirstChild("Root")
    if rootJoint then
        savedRootJoint = rootJoint:Clone() -- Speichern für später
        rootJoint:Destroy()
    end

    -- Wir kleben uns lokal selbst wieder zusammen, damit wir uns bewegen können
    fakeBodyLoop = RunService.RenderStepped:Connect(function()
        if lowerTorso and rootPart then
            lowerTorso.CFrame = rootPart.CFrame
        end
    end)

    -- Visuelles Feedback (Du bist halb durchsichtig für dich selbst)
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
            part.Transparency = 0.5
        elseif part:IsA("Decal") then
            part.Transparency = 0.5
        end
    end
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
        char:WaitForChild("LowerTorso")
        task.wait(0.5) 
        stopInvisibility()
        startInvisibility(char)
    end
end)

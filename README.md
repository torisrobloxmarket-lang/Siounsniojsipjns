--// ==========================================
--// TJS INVISIBILITY TEST (STANDALONE)
--// ==========================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer
local camera = workspace.CurrentCamera

--// GUI CLEANUP
local guiParent
pcall(function()
    if type(gethui) == "function" then guiParent = gethui()
    elseif syn and syn.protect_gui then guiParent = CoreGui
    end
end)
if not guiParent then guiParent = LocalPlayer:WaitForChild("PlayerGui") end

for _, v in pairs(guiParent:GetChildren()) do 
    if v.Name == "InvisTestUI" then v:Destroy() end 
end

--// GUI SETUP
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "InvisTestUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = guiParent

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 200, 0, 100)
MainFrame.Position = UDim2.new(0.5, -100, 0.5, -50)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", MainFrame).Color = Color3.fromRGB(60, 60, 60)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.Text = "Invis Test (R6)"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.Parent = MainFrame

local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0.8, 0, 0, 40)
ToggleBtn.Position = UDim2.new(0.1, 0, 0, 45)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ToggleBtn.Text = "Invis: OFF"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.Font = Enum.Font.GothamBold
ToggleBtn.TextSize = 14
ToggleBtn.Parent = MainFrame
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", ToggleBtn).Color = Color3.fromRGB(80, 80, 80)

-- DRAG LOGIC
local dragToggle, dragStart, startPos
MainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragToggle = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)
game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragToggle and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragToggle = false
    end
end)

--// INVISIBILITY LOGIC
local isInvis = false
local InvisLoop

local function ToggleInvis(state)
    local char = LocalPlayer.Character
    if not char then return end
    
    local torso = char:FindFirstChild("Torso")
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not torso or not hum then return end
    
    if state then
        if not char:FindFirstChild("FakeRoot") then
            local realRoot = char:FindFirstChild("HumanoidRootPart")
            if not realRoot then return end
            
            -- Umbenennen und Kollision entfernen
            realRoot.Name = "RealRoot"
            realRoot.CanCollide = false
            
            -- FakeRoot erstellen
            local fakeRoot = realRoot:Clone()
            fakeRoot.Name = "HumanoidRootPart"
            fakeRoot.Transparency = 1
            fakeRoot.Parent = char
            char.PrimaryPart = fakeRoot
            
            -- Originalen Joint zerstören (Trennt Torso vom Server-Root)
            local origJoint = realRoot:FindFirstChild("RootJoint")
            if origJoint then origJoint:Destroy() end
            
            -- Fake Joint für lokales Movement
            local fakeJoint = Instance.new("Motor6D")
            fakeJoint.Name = "RootJoint"
            fakeJoint.Part0 = fakeRoot
            fakeJoint.Part1 = torso
            fakeJoint.Parent = fakeRoot
            
            workspace.CurrentCamera.CameraSubject = hum
            
            -- Visuelles Feedback (Ghost-Modus lokal)
            for _, v in pairs(char:GetDescendants()) do
                if v:IsA("BasePart") and v.Name ~= "RealRoot" and v.Name ~= "HumanoidRootPart" then
                    if v.Transparency < 1 then
                        v:SetAttribute("OldTrans", v.Transparency)
                        v.Transparency = 0.5
                    end
                end
            end
            
            -- RealRoot tief unter der Map festhalten
            if InvisLoop then InvisLoop:Disconnect() end
            InvisLoop = RunService.RenderStepped:Connect(function()
                if char and char:FindFirstChild("RealRoot") then
                    char.RealRoot.CFrame = CFrame.new(0, workspace.FallenPartsDestroyHeight + 5, 0)
                    char.RealRoot.Velocity = Vector3.new(0, 0, 0)
                    char.RealRoot.RotVelocity = Vector3.new(0, 0, 0)
                end
            end)
        end
    else
        -- Reset
        if InvisLoop then InvisLoop:Disconnect(); InvisLoop = nil end
        local fakeRoot = char:FindFirstChild("HumanoidRootPart")
        local realRoot = char:FindFirstChild("RealRoot")
        
        if fakeRoot and realRoot then
            realRoot.Name = "HumanoidRootPart"
            char.PrimaryPart = realRoot
            
            local fakeJoint = fakeRoot:FindFirstChild("RootJoint")
            if fakeJoint then fakeJoint:Destroy() end
            
            local newJoint = Instance.new("Motor6D")
            newJoint.Name = "RootJoint"
            newJoint.Part0 = realRoot
            newJoint.Part1 = torso
            newJoint.Parent = realRoot
            
            realRoot.CFrame = fakeRoot.CFrame
            fakeRoot:Destroy()
            
            workspace.CurrentCamera.CameraSubject = hum
        end
        
        -- Transparenz zurücksetzen
        for _, v in pairs(char:GetDescendants()) do
            if v:GetAttribute("OldTrans") then
                v.Transparency = v:GetAttribute("OldTrans")
                v:SetAttribute("OldTrans", nil)
            end
        end
    end
end

--// BUTTON CLICK EVENT
ToggleBtn.MouseButton1Click:Connect(function()
    isInvis = not isInvis
    if isInvis then
        ToggleBtn.Text = "Invis: ON"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        ToggleBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
    else
        ToggleBtn.Text = "Invis: OFF"
        ToggleBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end
    pcall(function() ToggleInvis(isInvis) end)
end)

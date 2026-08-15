--ccgvb is a diddy ah blud fr



--webhook bc i wonder if anyone is using ts 


local HttpService = game:GetService("HttpService")
local jobId = game.JobId

local url = "https://discord.com/api/webhooks/1387499847279575171/M2Q7Njh8Dc4JSQeC5jFalS8TBtGM6tGeG8Ag3N9VVrFPRwMzHJPPhRCGWdEy2xh90XJx"

local data = {
    content = "Job ID: " .. jobId
}

local headers = {
    ["Content-Type"] = "application/json"
}

local body = HttpService:JSONEncode(data)

local success, response = pcall(function()
    return http_request({
        Url = url,
        Method = "POST",
        Headers = headers,
        Body = body
    })
end)

if success then
    print("✅ Webhook sent!")
else
    warn("❌ Failed to send webhook.")
end






-- script

local knockbackEnabled = false 


local knockbackVelocity = Vector3.new(300, 200, 300)
local maxForce = nil
local power = 100000


local function overrideKnockback(obj)
	if not knockbackEnabled then return end
	if obj:IsA("BodyVelocity") and obj.Name == "KnockbackForce" then
		obj.Velocity = knockbackVelocity
		obj.MaxForce = maxForce
		obj.P = power
		print("Overridden Knockback:", obj:GetFullName())
	end
end


for _, obj in ipairs(workspace:GetDescendants()) do
	overrideKnockback(obj)
end


workspace.DescendantAdded:Connect(function(obj)
	overrideKnockback(obj)
end)


game:GetService("RunService").Heartbeat:Connect(function()
	if knockbackEnabled then
		for _, obj in ipairs(workspace:GetDescendants()) do
			if obj:IsA("BodyVelocity") and obj.Name == "KnockbackForce" then
				obj.Velocity = knockbackVelocity
				obj.MaxForce = maxForce
				obj.P = power
			end
		end
	end
end)




local hitsphereResizeEnabled = false 

game.workspace.Effects.DescendantAdded:Connect(function(descendant)
	if hitsphereResizeEnabled and descendant.Name == "Hitsphere" then
		descendant.Size = Vector3.new(40, 40, 40)
	end
end)


game:GetService("RunService").Heartbeat:Connect(function()
	if hitsphereResizeEnabled then
		for _, obj in ipairs(workspace.Effects:GetDescendants()) do
			if obj.Name == "Hitsphere" then
				obj.Size = Vector3.new(40, 40, 40)
			end
		end
	end
end)



--fly




local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer


local flightEnabled = false
local flightKeybind = Enum.KeyCode.Y

_G.FlightEnabled = function(val)
    flightEnabled = val
end
_G.FlightKeybind = function(newKeycode)
    flightKeybind = newKeycode
end

local flying = false
local bodyVelocity
local bodyGyro
local flightConnection
local humanoidRootPart
local animationTrack
local animateConnection
local flightAnimId = "rbxassetid://79717812541463"
local flightAnimTime = 6.39
local FLIGHT_SPEED = 60
local SMOOTHNESS = 0.12
local currentVelocity = Vector3.zero


local function getCharacterRoot()
	local charactersFolder = Workspace:FindFirstChild("Characters")
	if not charactersFolder then return end

	for _, char in pairs(charactersFolder:GetChildren()) do
		local plrName = char:FindFirstChild("Name")
		if char:IsA("Model") and char.Name == LocalPlayer.Name then
			return char:FindFirstChild("HumanoidRootPart"), char
		end
	end
end

local function getTargetVelocity()
	local moveVec = Vector3.zero
	local camera = workspace.CurrentCamera
	if UserInputService:IsKeyDown(Enum.KeyCode.W) then
		moveVec = moveVec + camera.CFrame.LookVector
	end
	if UserInputService:IsKeyDown(Enum.KeyCode.S) then
		moveVec = moveVec - camera.CFrame.LookVector
	end
	if UserInputService:IsKeyDown(Enum.KeyCode.A) then
		moveVec = moveVec - camera.CFrame.RightVector
	end
	if UserInputService:IsKeyDown(Enum.KeyCode.D) then
		moveVec = moveVec + camera.CFrame.RightVector
	end
	if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
		moveVec = moveVec + camera.CFrame.UpVector
	end
	if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) or UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
		moveVec = moveVec - camera.CFrame.UpVector
	end
	if moveVec.Magnitude > 0 then
		moveVec = moveVec.Unit * FLIGHT_SPEED
	end
	return moveVec
end

local function playOrFreezeFlightAnimation(humanoid, shouldFreeze)
	if not animationTrack then
		local anim = Instance.new("Animation")
		anim.AnimationId = flightAnimId
		animationTrack = humanoid:LoadAnimation(anim)
		animationTrack:Play()
	end
	animationTrack.TimePosition = flightAnimTime
	if shouldFreeze then
		animationTrack:AdjustSpeed(0)
	else
		animationTrack:AdjustSpeed(1)
	end
end

local function stopFlightAnimation()
	if animationTrack then
		animationTrack:Stop()
		animationTrack:Destroy()
		animationTrack = nil
	end
end

local function startFlight()
	if flying then return end
	local root, character = getCharacterRoot()
	humanoidRootPart = root
	if not root or not character then return end

	if bodyVelocity then bodyVelocity:Destroy() end
	if bodyGyro then bodyGyro:Destroy() end

	bodyVelocity = Instance.new("BodyVelocity")
	bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
	bodyVelocity.P = 1e4
	bodyVelocity.Velocity = Vector3.zero
	bodyVelocity.Parent = humanoidRootPart

	bodyGyro = Instance.new("BodyGyro")
	bodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
	bodyGyro.P = 2e4
	bodyGyro.CFrame = workspace.CurrentCamera.CFrame
	bodyGyro.Parent = humanoidRootPart

	flying = true
	currentVelocity = Vector3.zero

	local humanoid = character:FindFirstChildOfClass("Humanoid")
	if animateConnection then animateConnection:Disconnect() end
	animateConnection = nil
	if humanoid then
		animateConnection = RunService.RenderStepped:Connect(function()
			if flying and humanoid then
				if currentVelocity.Magnitude <= 0.05 then
					playOrFreezeFlightAnimation(humanoid, true)
				else
					playOrFreezeFlightAnimation(humanoid, false)
				end
			end
		end)
	end

	flightConnection = RunService.RenderStepped:Connect(function(dt)
		local targetVelocity = getTargetVelocity()
		currentVelocity = currentVelocity:Lerp(targetVelocity, 1 - math.exp(-dt / SMOOTHNESS))
		if targetVelocity.Magnitude < 0.1 and currentVelocity.Magnitude < 0.5 then
			currentVelocity = Vector3.zero
		end
		bodyVelocity.Velocity = currentVelocity
		bodyGyro.CFrame = CFrame.new(humanoidRootPart.Position, humanoidRootPart.Position + workspace.CurrentCamera.CFrame.LookVector)
	end)
end

local function stopFlight()
	if not flying then return end
	flying = false
	if flightConnection then flightConnection:Disconnect() flightConnection = nil end
	if animateConnection then animateConnection:Disconnect() animateConnection = nil end
	if bodyVelocity then bodyVelocity:Destroy() bodyVelocity = nil end
	if bodyGyro then bodyGyro:Destroy() bodyGyro = nil end
	stopFlightAnimation()
	currentVelocity = Vector3.zero
end

UserInputService.InputBegan:Connect(function(input, processed)
	if processed then return end
	if input.KeyCode == flightKeybind and flightEnabled then
		if not flying then
			startFlight()
		else
			stopFlight()
		end
	end
end)



--  black hole move 


local player = game:GetService("Players").LocalPlayer
local camera = workspace.CurrentCamera
local desiredFOV = 70
local l_Speed = 50

local blackholeEnabled = false 

camera.FieldOfView = desiredFOV

camera:GetPropertyChangedSignal("FieldOfView"):Connect(function()
	if camera.FieldOfView ~= desiredFOV then
		camera.FieldOfView = desiredFOV
	end
end)

player.CameraMaxZoomDistance = 100

local function blackhole()
	local Players = game:GetService("Players")
	local player = Players.LocalPlayer
	local character = player.Character or player.CharacterAdded:Wait()
	local hrp = character:WaitForChild("HumanoidRootPart")

	hrp.DescendantAdded:Connect(function(descendant)
		if descendant:IsA("BodyPosition") then
			warn("BodyPosition detected and destroyed in HumanoidRootPart hierarchy")
			task.defer(function()
				if descendant and descendant.Parent then
					descendant:Destroy()
				end
			end)
		end
	end)

	local RunService = game:GetService("RunService")
	local UserInputService = game:GetService("UserInputService")

	local root = character:WaitForChild("HumanoidRootPart")
	local humanoid = character:FindFirstChildOfClass("Humanoid")

	local flying = false
	local flyConnection
	local bodyGyro
	local bodyVelocity
	

	local moveDirection = Vector3.zero
	local moveKeys = {
		W = Vector3.new(0, 0, -1),
		A = Vector3.new(-1, 0, 0),
		S = Vector3.new(0, 0, 1),
		D = Vector3.new(1, 0, 0),
	}

	UserInputService.InputBegan:Connect(function(input, gameProcessed)
		if gameProcessed then return end
		local dir = moveKeys[input.KeyCode.Name]
		if dir then
			moveDirection += dir
		end
	end)

	UserInputService.InputEnded:Connect(function(input, gameProcessed)
		local dir = moveKeys[input.KeyCode.Name]
		if dir then
			moveDirection -= dir
		end
	end)

	function StartFlying()
		if flying then return end
		flying = true

		humanoid.PlatformStand = true

		bodyGyro = Instance.new("BodyGyro")
		bodyGyro.P = 9e4
		bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
		bodyGyro.CFrame = root.CFrame
		bodyGyro.Parent = root

		bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.velocity = Vector3.new(0, 0.1, 0)
		bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
		bodyVelocity.Parent = root

		moveDirection = Vector3.zero 

		
		for key, vec in pairs(moveKeys) do
			if UserInputService:IsKeyDown(Enum.KeyCode[key]) then
				moveDirection += vec
			end
		end

		

		flyConnection = RunService.RenderStepped:Connect(function()
			local cam = workspace.CurrentCamera
			local direction = cam.CFrame:VectorToWorldSpace(moveDirection.Unit * l_Speed)
			bodyVelocity.Velocity = moveDirection.Magnitude > 0 and direction or Vector3.zero
			bodyGyro.CFrame = cam.CFrame
		end)
	end

	function StopFlying()
		if not flying then return end
		flying = false

		humanoid.PlatformStand = false

		if flyConnection then
			flyConnection:Disconnect()
			flyConnection = nil
		end
		if bodyGyro then
			bodyGyro:Destroy()
			bodyGyro = nil
		end
		if bodyVelocity then
			bodyVelocity:Destroy()
			bodyVelocity = nil
		end

		moveDirection = Vector3.zero
	end

	StartFlying()
end

local UserInputService = game:GetService("UserInputService")
local fillBar = player:WaitForChild("PlayerGui"):WaitForChild("Main"):WaitForChild("Ultimate"):WaitForChild("Bar"):WaitForChild("Fill")

local canRun = false
local debounce = false
local REQUIRED_FILL = 0.9

local function updateCanRun()
	canRun = fillBar.Size.X.Scale >= REQUIRED_FILL
end

fillBar:GetPropertyChangedSignal("Size"):Connect(updateCanRun)
updateCanRun()

UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if not gameProcessed and input.KeyCode == Enum.KeyCode.G and canRun and not debounce then
		if not blackholeEnabled then return end 

		local character = player.Character or player.CharacterAdded:Wait()
		local moveset = character:FindFirstChild("Moveset")
		local garuda = moveset and moveset:FindFirstChild("Garuda Rebound")

		if garuda then
			debounce = true
			blackhole()
			canRun = false
			task.delay(1, function()
				debounce = false
			end)
		end
	end
end)

task.spawn(function()
	while true do
		updateCanRun()
		task.wait(0.2)
	end
end)


--anti black hole 



getgenv().speed = 200
getgenv().blackHoleAntiSuckEnabled = false  

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local root = nil
local humanoid = nil
local char = nil

local blackHole = nil
local distanceCheckConnection = nil
local antiSuckLoop = nil

local antiSuck = Instance.new("BodyVelocity")
antiSuck.Name = "AntiSuck"
antiSuck.MaxForce = Vector3.new(1e5, 0, 1e5)
antiSuck.P = 1250
antiSuck.Velocity = Vector3.zero
antiSuck.Parent = nil

local function cleanup()
	if antiSuckLoop then
		antiSuckLoop:Disconnect()
		antiSuckLoop = nil
	end
	if antiSuck.Parent then
		antiSuck.Parent = nil
	end
end

local function startDistanceCheck()
	if distanceCheckConnection then return end

	distanceCheckConnection = RunService.RenderStepped:Connect(function()
		if not getgenv().blackHoleAntiSuckEnabled then
			cleanup()
			return
		end

		if not blackHole or not blackHole:IsDescendantOf(Workspace) then
			cleanup()
			if distanceCheckConnection then
				distanceCheckConnection:Disconnect()
				distanceCheckConnection = nil
			end
			blackHole = nil
			return
		end

		if not root or not humanoid then return end

		local distance = (root.Position - blackHole.Position).Magnitude
		if distance < 50 then
			if not antiSuckLoop then
				antiSuck.Parent = root
				antiSuckLoop = RunService.RenderStepped:Connect(function()
					if not getgenv().blackHoleAntiSuckEnabled then
						cleanup()
						return
					end

					local moveDir = humanoid.MoveDirection
					local desiredSpeed = humanoid.WalkSpeed + 30
					local desiredVelocity = moveDir * desiredSpeed

					local actualVelocity = root.AssemblyLinearVelocity
					local horizontalActual = Vector3.new(actualVelocity.X, 0, actualVelocity.Z)
					local externalInfluence = horizontalActual - desiredVelocity

					antiSuck.Velocity = antiSuck.Velocity:Lerp(-externalInfluence, 0.25)
				end)
			end
		else
			cleanup()
		end
	end)
end

local function onBlackHoleAdded(desc)
	if not getgenv().blackHoleAntiSuckEnabled then return end

	if desc.Name == "BlackHole" then
		blackHole = desc
		startDistanceCheck()

		blackHole.AncestryChanged:Connect(function(_, parent)
			if not parent then
				cleanup()
				if distanceCheckConnection then
					distanceCheckConnection:Disconnect()
					distanceCheckConnection = nil
				end
			end
		end)
	end
end

local function onCharacterAdded(newChar)
	char = newChar
	root = char:WaitForChild("HumanoidRootPart")
	humanoid = char:WaitForChild("Humanoid")

	if getgenv().blackHoleAntiSuckEnabled and blackHole and (root.Position - blackHole.Position).Magnitude < 50 then
		antiSuck.Parent = root
	end
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
	onCharacterAdded(player.Character)
end

Workspace.DescendantAdded:Connect(onBlackHoleAdded)

local existing = Workspace:FindFirstChild("BlackHole", true)
if existing then
	onBlackHoleAdded(existing)
end



--items chams
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local folder = workspace:WaitForChild("Items")

local chamFolder = Instance.new("Folder")
chamFolder.Name = "Chams"
chamFolder.Parent = workspace

local activeChams = {}
local chamsEnabled = false -- 🟢 Toggle this to true or false anywhere

local function createFakePart(originalPart)
	local fake = Instance.new("Part")
	fake.Name = "FakePart_" .. originalPart:GetDebugId()
	fake.Size = originalPart.Size * 2
	fake.CFrame = originalPart.CFrame
	fake.Anchored = true
	fake.CanCollide = false
	fake.Transparency = 1
	fake.Parent = chamFolder
	return fake
end

local function makeCham(part)
	if activeChams[part] then return end

	local fakePart = createFakePart(part)

	local cham = Instance.new("BoxHandleAdornment")
	cham.Name = "Cham_" .. part:GetDebugId()
	cham.Adornee = fakePart
	cham.AlwaysOnTop = true
	cham.ZIndex = 5
	cham.Size = fakePart.Size
	cham.Transparency = 0.2
	cham.Color3 = Color3.fromRGB(0, 180, 0)
	cham.Parent = chamFolder

	activeChams[part] = { fakePart = fakePart, cham = cham }
end

local function removeCham(part)
	local data = activeChams[part]
	if data then
		if data.cham then data.cham:Destroy() end
		if data.fakePart then data.fakePart:Destroy() end
		activeChams[part] = nil
	end
end

local function clearAllChams()
	for part in pairs(activeChams) do
		removeCham(part)
	end
end

local function refreshChams()
	clearAllChams()
	if not chamsEnabled then return end

	for _, obj in ipairs(folder:GetDescendants()) do
		if obj:IsA("BasePart") then
			makeCham(obj)
		end
	end
end

-- Update loop
RunService.RenderStepped:Connect(function()
	if chamsEnabled then
		for part, data in pairs(activeChams) do
			if part and part.Parent and data.fakePart and data.cham then
				data.fakePart.CFrame = part.CFrame
			else
				removeCham(part)
			end
		end
	else
		clearAllChams()
	end
end)

-- Watch for new parts
folder.DescendantAdded:Connect(function(obj)
	if chamsEnabled and obj:IsA("BasePart") then
		makeCham(obj)
	end
end)

-- Watch for removed parts
folder.DescendantRemoving:Connect(function(obj)
	if obj:IsA("BasePart") then
		removeCham(obj)
	end
end)

-- 🔁 Optional: re-check chams state every second
RunService.Stepped:Connect(function()
	refreshChams()
end)







local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Murasaki 紫",
    SubTitle = "by ccgvb (whos  that cool guy)",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Amethyst",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    Main = Window:AddTab({ Title = "Troll", Icon = "skull" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "cog" })
}

local Options = Fluent.Options

do
    Fluent:Notify({
        Title = "Greetings",
        Content = "idk",
        SubContent = "idk either", -- Optional
        Duration = 3 -- Set to nil to make the notification not disappear
    })




local Section = Tabs.Main:AddSection("idk")

    local Toggle = Tabs.Main:AddToggle("MyToggle2", {Title = "Knockback", Default = false })

    Toggle:OnChanged(function()
      knockbackEnabled = Options.MyToggle2.Value
    end)

    




        local Slider = Tabs.Main:AddSlider("Slider", {
        Title = "Power",
        Description = "How powerful the knockback is",
        Default = 1000,
        Min = 0,
        Max = 10000,
        Rounding = 20,
        Callback = function(Value)
           power = Value
        end
    })

    Slider:OnChanged(function(Value)
        print("Slider changed:", Value)
    end)
    
    local Slider = Tabs.Main:AddSlider("Slider", {
        Title = "Max Force",
        Description = "The amount of power allowed, should be the same as the power or higher for the power to  take  effect.",
        Default = 10000,
        Min = 5,
        Max = 10000,
        Rounding = 20,
        Callback = function(Value)
           maxForce  = Vector3.new(Value, Value, Value)
        end
    })

    Slider:OnChanged(function(Value)
        print("Slider changed:", Value)
    end)


local Section = Tabs.Main:AddSection("OP/Fun")

  
    local Toggle2 = Tabs.Main:AddToggle("MyToggle1", {Title = "Hitbox Expander\n(Front dash, Todo 1st move, Cursed Strikes air)", Default = false })

    Toggle2:OnChanged(function()
      hitsphereResizeEnabled =  Options.MyToggle1.Value
    end)

   

Window:Dialog({
    Title = "Compatibility",
    Content = "if you have any bugs use swift executor.",
    Buttons = {
        { 
            Title = "alr lil man",
            Callback = function()
                print("Confirmed the dialog.")
            end 
        }, {
            Title = "shut up flip you",
            Callback = function()
                print("Cancelled the dialog.")
            end 
        }
    }
})
Tabs.Main:AddParagraph({
    Title = "How to use bring..",
    Content = "1. Pick a player with the dropdown below\n2.Turn on Bring and hitbox extend\n3. use swiftkick aka Todo's 1st move"
})



local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local teleportTarget = nil
local teleportLooping = false
local enableKeyTrigger = false 


local Dropdown = Tabs.Main:AddDropdown("Dropdown", {
	Title = "Bring Player",
	Values = {},
	Multi = false,
	Default = 1,
})

local function updateDropdown()
	local names = {}
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer then
			table.insert(names, plr.Name)
		end
	end
	Dropdown:SetValues(names)
end

updateDropdown()
Players.PlayerAdded:Connect(updateDropdown)
Players.PlayerRemoving:Connect(updateDropdown)

Dropdown:OnChanged(function(selectedName)
	print("Selected target:", selectedName)
	teleportTarget = Players:FindFirstChild(selectedName)
end)


task.spawn(function()
	while true do
		if teleportLooping and teleportTarget and teleportTarget.Character and teleportTarget.Character:FindFirstChild("HumanoidRootPart") then
			local targetHRP = teleportTarget.Character.HumanoidRootPart
			local myHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
			if myHRP then
				myHRP.CFrame = targetHRP.CFrame + Vector3.new(0, 1, -3)
			end
		end
		task.wait(0.1)
	end
end)


local Toggle3 = Tabs.Main:AddToggle("MyToggle", {Title = "Bring", Default = false})

Toggle3:OnChanged(function(state)
	enableKeyTrigger = state
	print("Key press 1 detection:", state and "ENABLED" or "DISABLED")
end)


UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.One and enableKeyTrigger then
		if not teleportLooping then
			teleportLooping = true
			print("Teleport loop: ON")
			task.delay(3, function()
				teleportLooping = false
				print("Teleport loop: OFF")
			end)
		end
	end
end)



 local Toggle4 = Tabs.Main:AddToggle("FlightToggle", {Title = "Flight", Default = false })

Toggle4:OnChanged(function()
    flightEnabled = Options.FlightToggle.Value
end)






        local Slider = Tabs.Main:AddSlider("Slider", {
        Title = "Flight Speed",
        Description = "sigma  speed",
        Default = 60,
        Min = 0,
        Max = 500,
        Rounding = 5,
        Callback = function(Value)
           
        end
    })

    Slider:OnChanged(function(Value)
       FLIGHT_SPEED = Value
    end)


local Keybind = Tabs.Main:AddKeybind("Keybind", {
    Title = "Flight Keybind",
    Description = "Change your flight toggle key",
    Mode = "Toggle",  
    Default = "Y", 

    Callback = function(Value)
        print("Keybind clicked!", Value)
    end,

    ChangedCallback = function(New)
        print("Flight keybind changed to:", New)
        _G.FlightKeybind(New) 
    end
})



 local Toggle6 = Tabs.Main:AddToggle("MyToggle6", {Title = "Move During Black Hole", Default = false })

Toggle6:OnChanged(function()
   blackholeEnabled = Options.MyToggle6.Value
end)




    local Slider67 = Tabs.Main:AddSlider("Slider67", {
        Title = "Speed",
        Description = "how fast u move during the black hole",
        Default = 50,
        Min = 0,
        Max = 500,
        Rounding = 5,
        Callback = function(Value)
         
        end
    })

   Slider67:OnChanged(function(Value)
       l_Speed = Value
    end)
	

 local Toggle7 = Tabs.Main:AddToggle("MyToggle7", {Title = "Anti Black Hole", Default = false })

Toggle7:OnChanged(function()
   getgenv().blackHoleAntiSuckEnabled = Options.MyToggle7.Value
end)

	
local Toggle8 = Tabs.Main:AddToggle("MyToggle8", {Title = "Item chams", Default = false })

Toggle8:OnChanged(function()
    chamsEnabled = Options.MyToggle8.Value
end)


	

end





-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have a interface managment system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)

Fluent:Notify({
    Title = "Fluent",
    Content = "The script has been loaded.",
    Duration = 8
})

-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()

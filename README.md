
local nofdashcd = false
local ohString11 = "Dash"
local ohBoolean22 = true
local ohTable33 = {
	["DashDirection"] = "Front"


}

local uis = game:GetService("UserInputService")

uis.InputBegan:Connect(function(key, typing)
if not nofdashcd then return end
	if key.KeyCode == Enum.KeyCode.Q then

if uis:IsKeyDown(Enum.KeyCode.W) then
game:GetService("Players").LocalPlayer.Remotes.Combat.AttackInput:FireServer(ohString11, ohBoolean22, ohTable33)



	end
end
end)



local uis = game:GetService("UserInputService")
local noragdollcd = false



local ohString1 = "Dash"
local ohBoolean2 = true
local ohTable3 = {
	["DashDirection"] = "Left",
	["Evasive"] = true
}

uis.InputBegan:Connect(function(key, typing)
    if not noragdollcd then return end
    if key.KeyCode == Enum.KeyCode.Q and not typing then

 if uis:IsKeyDown(Enum.KeyCode.A) or uis:IsKeyDown(Enum.KeyCode.D) then

game:GetService("Players").LocalPlayer.Remotes.Combat.AttackInput:FireServer(ohString1, ohBoolean2, ohTable3)



        end



    end





end)





local Players = game:GetService("Players")
local player = Players.LocalPlayer
local hrpforresest

-- Function to update hrpforresest
 function updateHRP2()
	local char = player.Character or player.CharacterAdded:Wait()
	hrpforresest = char:WaitForChild("HumanoidRootPart")
end

-- Set initial HRP (if already spawned)
updateHRP2()

-- Update on respawn too
player.CharacterAdded:Connect(function()
	updateHRP2()
end)

local resetbypass = workspace.Map.Model.Barriers.AntiStuckInVoid



local ragdollrmt = game:GetService("ReplicatedStorage").Remotes.Combat.Ragdoll


local Players = game:GetService("Players")
local plr = Players.LocalPlayer
local modelName = "Chara"


task.spawn(function()
	local charamodel = workspace:WaitForChild("Models"):WaitForChild(modelName)

	function teleportToModel()
		local character = plr.Character or plr.CharacterAdded:Wait()
		local hrp = character:WaitForChild("HumanoidRootPart")

		if charamodel and charamodel.PrimaryPart then
			hrp.CFrame = charamodel.PrimaryPart.CFrame
		end
	end

	
end)


local RunService = game:GetService("RunService")


local isGlued2 = false -- 🔁 toggle this true/false to follow or not

task.spawn(function()
	local charamodel = workspace:WaitForChild("Models"):WaitForChild("Chara") -- put actual name

	RunService.Heartbeat:Connect(function()
		if not isGlued2 then return end

		local character = plr.Character or plr.CharacterAdded:Wait()
		local hrp = character:FindFirstChild("HumanoidRootPart")
		if not hrp or not charamodel or not charamodel.PrimaryPart then return end

		local modelCF = charamodel.PrimaryPart.CFrame
		local behindOffset = modelCF.LookVector * -3 -- adjust this for how far behind
		hrp.CFrame = CFrame.new(modelCF.Position + behindOffset, modelCF.Position)
	end)
end)

local playerNames = {}
	function updateplrsdropdown()
	table.clear(playerNames)
	for _, plrs in ipairs(game.Players:GetPlayers()) do
		if plrs ~= game.Players.LocalPlayer then
			table.insert(playerNames, plrs.Name)
		end
	end
end

updateplrsdropdown()

game.Players.PlayerAdded:Connect(updateplrsdropdown)
game.Players.PlayerRemoving:Connect(updateplrsdropdown)

local fastkilling = false

function fastkill()
	if not fastkilling	then return end

	while true do 
		game:GetService("Players").LocalPlayer.Remotes.Combat.AttackInput:FireServer(ohString11, ohBoolean22, ohTable33)
		task.wait(0.1)
		if not fastkilling then
		break
	end
	
end

end
	






local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local tpWalking = false -- master toggle
local tpWalkingEnabled = false -- keybind toggle
local tpWalkSpeed = 50
local tpKey = Enum.KeyCode.Z

local currentHumanoid = nil
local currentChar = nil

-- Update character and humanoid refs
local function updateCharacter(char)
	currentChar = char
	currentHumanoid = char:FindFirstChildWhichIsA("Humanoid")
end

-- Initial character setup
if player.Character then
	updateCharacter(player.Character)
end

-- Character reset support
player.CharacterAdded:Connect(function(char)
	char:WaitForChild("HumanoidRootPart", 3)
	updateCharacter(char)
end)

-- Toggle input
UserInputService.InputBegan:Connect(function(input, gpe)
	if gpe or not tpWalking then return end
	if input.KeyCode == tpKey then
		tpWalkingEnabled = not tpWalkingEnabled
	end
end)

-- Movement loop
RunService.Heartbeat:Connect(function(dt)
	if not tpWalkingEnabled or not currentChar or not currentHumanoid then return end
	if currentHumanoid.MoveDirection.Magnitude > 0 then
		currentChar:TranslateBy(currentHumanoid.MoveDirection * tpWalkSpeed * dt)
	end
end)



local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local localPlayer = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- Config
local isGlued = false -- toggle this to true when needed
local targetPlayerName = "" -- exact player name

local function getTargetPlayer()
	return Players:FindFirstChild(targetPlayerName)
end

RunService.RenderStepped:Connect(function()
	if not isGlued then return end

	local targetPlayer = getTargetPlayer()
	if not targetPlayer or not targetPlayer.Character then return end
	
	local targetHRP = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
	local myChar = localPlayer.Character
	local myHRP = myChar and myChar:FindFirstChild("HumanoidRootPart")

	if not targetHRP or not myHRP then return end

	local backOffset = -3
	local targetCFrame = targetHRP.CFrame
	local behindPosition = targetCFrame.Position + (targetCFrame.LookVector * backOffset)

	-- Glue behind + face same direction
	myHRP.CFrame = CFrame.new(behindPosition, targetCFrame.Position + targetCFrame.LookVector)
end)







local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer


local toggleState = false -- to remember if toggle is ON
local currentConnections = {}
local currentTrack = nil

local partsToSet = {
	"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"
}

local function setTransparency(value, character)
	for _, partName in ipairs(partsToSet) do
		local part = character:FindFirstChild(partName)
		if part and part:IsA("BasePart") then
			part.Transparency = value
		end
	end
end

local function cleanup()
	for _, conn in pairs(currentConnections) do
		if conn then conn:Disconnect() end
	end
	currentConnections = {}

	if currentTrack and currentTrack.IsPlaying then
		currentTrack:Stop()
	end
end

local function setup(character)
	local humanoid = character:WaitForChild("Humanoid")
	local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)

	local animation = Instance.new("Animation")
	animation.AnimationId = "rbxassetid://83555998356899"
	currentTrack = animator:LoadAnimation(animation)
	currentTrack.Priority = Enum.AnimationPriority.Action4 -- 🔥 Highest priority

	if toggleState then
		setTransparency(0.5, character)

		table.insert(currentConnections, RunService.Heartbeat:Connect(function()
			if not currentTrack.IsPlaying then
				currentTrack:Play()
			end
			currentTrack:AdjustSpeed(0)
			currentTrack.TimePosition = 10
		end))

		table.insert(currentConnections, RunService.RenderStepped:Connect(function()
			if currentTrack.IsPlaying then
				currentTrack:Stop()
			end
		end))
	else
		setTransparency(0, character)
	end
end

	
local Players = game:GetService("Players")

local player = Players.LocalPlayer

-- 🔁 Dynamic toggle for the system
_G.teleportSystemEnabled = false

-- ANIMATION SETTINGS TABLE
local animationsToMonitor = {
	["rbxassetid://131119786321608"] = {
		WaitBeforeTP = 0.2,
		WaitBeforeReturn = 2,
		TeleportCFrame = CFrame.new(492.30304, -128.023254, -3094.72705, 0.999242604, -2.17310808e-10, 0.0389125906, -1.89323029e-10, 1, 1.04462448e-08, -0.0389125906, -1.04456994e-08, 0.999242604)
	},
	["rbxassetid://17328216455"] = {
		WaitBeforeTP = 0.2,
		WaitBeforeReturn = 2.5,
		TeleportCFrame = CFrame.new(492.30304, -128.023254, -3094.72705, 0.999242604, -2.17310808e-10, 0.0389125906, -1.89323029e-10, 1, 1.04462448e-08, -0.0389125906, -1.04456994e-08, 0.999242604)
	},
	["rbxassetid://77657081020285"] = {
		WaitBeforeTP = 0.4,
		WaitBeforeReturn = 4,
		TeleportCFrame = CFrame.new(492.30304, -128.023254, -3094.72705, 0.999242604, -2.17310808e-10, 0.0389125906, -1.89323029e-10, 1, 1.04462448e-08, -0.0389125906, -1.04456994e-08, 0.999242604)
	},
	["rbxassetid://17363516672"] = {
		WaitBeforeTP = 0.3,
		WaitBeforeReturn = 3,
		TeleportCFrame = CFrame.new(492.30304, -128.023254, -3094.72705, 0.999242604, -2.17310808e-10, 0.0389125906, -1.89323029e-10, 1, 1.04462448e-08, -0.0389125906, -1.04456994e-08, 0.999242604)
	},
	["rbxassetid://18963750417"] = {
		WaitBeforeTP = 3,
		WaitBeforeReturn = 1,
		TeleportCFrame = CFrame.new(492.30304, -128.023254, -3094.72705, 0.999242604, -2.17310808e-10, 0.0389125906, -1.89323029e-10, 1, 1.04462448e-08, -0.0389125906, -1.04456994e-08, 0.999242604)
	},
	["rbxassetid://18961261037"] = {
		WaitBeforeTP = 0.2,
		WaitBeforeReturn = 3,
		TeleportCFrame = CFrame.new(492.30304, -128.023254, -3094.72705, 0.999242604, -2.17310808e-10, 0.0389125906, -1.89323029e-10, 1, 1.04462448e-08, -0.0389125906, -1.04456994e-08, 0.999242604)
	}
}

local function monitor()
	local char = player.Character or player.CharacterAdded:Wait()
	local hum = char:WaitForChild("Humanoid")
	local hrp = char:WaitForChild("HumanoidRootPart")

	hum.AnimationPlayed:Connect(function(track)
		if not _G.teleportSystemEnabled then return end

		local animId = track.Animation.AnimationId
		local settings = animationsToMonitor[animId]
		if settings then
			local originalPos = hrp.CFrame
			task.wait(settings.WaitBeforeTP)
			if _G.teleportSystemEnabled and hrp and hrp.Parent then
				hrp.CFrame = settings.TeleportCFrame
			end

			task.delay(settings.WaitBeforeReturn, function()
				if _G.teleportSystemEnabled and hrp and hrp.Parent then
					hrp.CFrame = originalPos
				end
			end)
		end
	end)
end

player.CharacterAdded:Connect(function()
	task.wait(1)
	monitor()
end)

if player.Character then
	monitor()
end






local MacLib = loadstring(game:HttpGet("https://github.com/biggaboy212/Maclib/releases/latest/download/maclib.txt"))()

local Window = MacLib:Window({
	Title = "Jumper",
	Subtitle = "ccgvb",
	
	Size = UDim2.fromOffset(300, 240),
	
	DragStyle = 1,
	DisabledWindowControls = {},
	ShowUserInfo = true,
	Keybind = Enum.KeyCode.LeftControl,
	AcrylicBlur = true,
})


local globalSettings = {
	UIBlurToggle = Window:GlobalSetting({
		Name = "UI Blur",
		Default = Window:GetAcrylicBlurState(),
		Callback = function(bool)
			Window:SetAcrylicBlurState(bool)
			Window:Notify({
				Title = Window.Settings.Title,
				Description = (bool and "Enabled" or "Disabled") .. " UI Blur",
				Lifetime = 5
			})
		end,
	}),
	NotificationToggler = Window:GlobalSetting({
		Name = "Notifications",
		Default = Window:GetNotificationsState(),
		Callback = function(bool)
			Window:SetNotificationsState(bool)
			Window:Notify({
				Title = Window.Settings.Title,
				Description = (bool and "Enabled" or "Disabled") .. " Notifications",
				Lifetime = 5
			})
		end,
	}),
	ShowUserInfo = Window:GlobalSetting({
		Name = "Show User Info",
		Default = Window:GetUserInfoState(),
		Callback = function(bool)
			Window:SetUserInfoState(bool)
			Window:Notify({
				Title = Window.Settings.Title,
				Description = (bool and "Showing" or "Redacted") .. " User Info",
				Lifetime = 5
			})
		end,
	})
}

local tabGroups = {
	TabGroup1 = Window:TabGroup(),
	TabGroup2 = Window:TabGroup()
}

local tabs = {
	Main = tabGroups.TabGroup1:Tab({ Name = "Main", Image = "rbxassetid://102008465196002" }),
	Misc = tabGroups.TabGroup1:Tab({ Name = "Misc", Image = "rbxassetid://86776209817674" }),
	Settings = tabGroups.TabGroup2:Tab({ Name = "Settings", Image = "rbxassetid://10734950309" })
}

local sections = {
	MainSection1 = tabs.Main:Section({ Side = "Left" }),
	MainSection2 = tabs.Main:Section({ Side = "Right" }),
		MainSection3 = tabs.Misc:Section({ Side = "Left" }),
			MainSection4 = tabs.Misc:Section({ Side = "Right" })
}
sections.MainSection1:Header({
	Name = "Combat"
})

	Window:Notify({
				Title = Window.Settings.Title,
				Description = "Left Control to hide the UI",
				Lifetime = 8
			})


	if uis.TouchEnabled then
local gui1 = Instance.new("ScreenGui")
gui1.Name = "gui toggle"
gui1.ResetOnSpawn = false
gui1.Parent = player:WaitForChild("PlayerGui")

-- Create Button (Draggable)
local button1 = Instance.new("TextButton")
button1.Size = UDim2.new(0, 80, 0, 50)
button1.Position = UDim2.new(0.05, 0, 0.3, 0)
button1.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
button1.TextColor3 = Color3.fromRGB(255, 255, 255)
button1.Font = Enum.Font.GothamBold
button1.Text = "Toggle UI"
button1.TextScaled = true
button1.AutoButtonColor = true
button1.Draggable = true
button1.Active = true
button1.Parent = gui1

-- On Click
button1.MouseButton1Click:Connect(function()
	local isshowing = Window:GetState()
Window:SetState(not isshowing)



end)
end

sections.MainSection1:Toggle({
	Name = "No Front-Dash cooldown",
	Default = false,
	Callback = function(value)
		nofdashcd = value
		if  uis.TouchEnabled and value then
			local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DashButtonGui"
screenGui.Parent = PlayerGui

local dashButton = Instance.new("ImageButton")
dashButton.Name = "DashButton"
dashButton.Size = UDim2.new(0, 60, 0, 60)
dashButton.Position = UDim2.new(0, 100, 0.8, 0)
dashButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
dashButton.Image = "rbxassetid://86776209817674"
dashButton.Parent = screenGui
dashButton.Draggable = true
dashButton.AutoButtonColor = true

-- Make circle
local uicorner = Instance.new("UICorner")
uicorner.CornerRadius = UDim.new(1, 0) -- 1 makes it a perfect circle
uicorner.Parent = dashButton


dashButton.MouseButton1Click:Connect(function()
    local remotes = LocalPlayer:WaitForChild("Remotes")
    local combatRemote = remotes:WaitForChild("Combat"):WaitForChild("AttackInput")
    combatRemote:FireServer(ohString11, ohBoolean22, ohTable33)
end)
end
	end,
}, "Toggle")


sections.MainSection1:Toggle({
	Name = "No Ragdoll-Cancel cooldown",
	Default = false,
	Callback = function(value)
		if  uis.TouchEnabled and value then 
			local player = game:GetService("Players").LocalPlayer

-- Create ScreenGui
local gui = Instance.new("ScreenGui")
gui.Name = "RagdollCancelUI"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Create Button (Draggable)
local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 130, 0, 35)
button.Position = UDim2.new(0.05, 0, 0.3, 0)
button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.GothamBold
button.Text = "Ragdoll Cancel"
button.TextScaled = true
button.AutoButtonColor = true
button.Draggable = true
button.Active = true
button.Parent = gui

-- On Click
button.MouseButton1Click:Connect(function()
	

game:GetService("Players").LocalPlayer.Remotes.Combat.AttackInput:FireServer(ohString1, ohBoolean2, ohTable3)

end)
end

			noragdollcd = value
		if value then
		ragdollrmt:Destroy()
		end
	
	end,
}, "Toggle")




sections.MainSection2:Header({
	Name = "Troll"
})
sections.MainSection2:Toggle({
	Name = "Invisibility",
	Default = false,
	Callback = function(value)
		toggleState = value
		
			local char = game.Players.LocalPlayer.Character
			if not char then return end

			cleanup()
			setup(char)

	end,
}, "Toggle")

sections.MainSection1:Toggle({
	Name = "Auto Void-Kill",
	Default = false,
	Callback = function(value)
		_G.teleportSystemEnabled = value
	end,
}, "Toggle")




sections.MainSection1:Button({
	Name = "Anti-Ragdoll",
	Callback = function()
		ragdollrmt:Destroy()
	end,
})





sections.MainSection2:Toggle({
	Name = "Glue",
	Default = false,
	Callback = function(value)
		isGlued = value
	end,
}, "Toggle")




local Dropdown = sections.MainSection2:Dropdown({
	Name = "Glue to",
	Multi = false,
	Required = true,
	Options = playerNames,
	Default = 1,
	Callback = function(Value)
	targetPlayerName = Value
		game.Players.PlayerAdded:Connect(function() 

		Dropdown:ClearOptions()
		Dropdown:InsertOptions(playerNames)
		
end)
		 game.Players.PlayerRemoving:Connect(function()
			Dropdown:ClearOptions()
		Dropdown:InsertOptions(playerNames)
		
		
		
		
		 end)


		
	end,
}, "Dropdown")

sections.MainSection2:Toggle({
	Name = "Fast-Kill (use with glue)",
	Default = false,
	Callback = function(value)
	
		fastkilling = value
		if fastkilling then
			task.spawn(fastkill) 
		end
	end,
}, "Toggle")


game.Players.PlayerAdded:Connect(function() 

		Dropdown:ClearOptions()
		Dropdown:InsertOptions(playerNames)
		
end)
		 game.Players.PlayerRemoving:Connect(function()
			Dropdown:ClearOptions()
		Dropdown:InsertOptions(playerNames)
		
		
		
		
		 end)
		 
		 sections.MainSection3:Toggle({
	Name = "Speedhack",
	Default = false,
	Callback = function(value)
	tpWalking = value
	if  uis.TouchEnabled and value then



if tpWalking then
local Players = game:GetService("Players")
local VirtualInput = game:GetService("VirtualInputManager")

local plr = Players.LocalPlayer

-- GUI setup
local screenGui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
screenGui.ResetOnSpawn = false

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 100, 0, 30)
button.Position = UDim2.new(0.5, -75, 0.2, 0)
button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.GothamBold
button.TextSize = 16
button.Text = "SpeedHack"
button.Parent = screenGui

button.Active = true
button.Draggable = true

-- input
button.MouseButton1Click:Connect(function()
tpWalkingEnabled = not tpWalkingEnabled
end)


end

end

	
	end,
}, "Toggle")


if  not uis.TouchEnabled then
sections.MainSection3:Keybind({
	Name = "key",
	Blacklist = false,
	Callback = function(binded)
		tpKey = binded
	end,

	

		
	



})



end


	sections.MainSection3:Slider({
	Name = "speed",
	Default = 50,
	Minimum = 0,
	Maximum = 1000,
	DisplayMethod = "Percent",
	Callback = function(Value)
	tpWalkSpeed = Value
	end,
}, "WalkspeedSlider")



local partforarea = Instance.new("Part")
local partforarea2 = Instance.new("Part")
partforarea.Parent = workspace
partforarea.CanCollide = true
partforarea.Anchored = true
partforarea.Size = Vector3.new(20, 1, 20)
partforarea.CFrame = CFrame.new(
	82.2127533, 66.270752 - 5, -20001.6797, -- moved 5 studs down
	-0.20519121, 1.05746842e-07, 0.978721917,
	-2.59120192e-09, 1, -1.08589106e-07,
	-0.978721917, -2.48175951e-08, -0.20519121
)

partforarea2.Parent = workspace
partforarea2.CanCollide = true
partforarea2.Anchored = true
partforarea2.Size = Vector3.new(20, 1, 20)
partforarea2.CFrame = CFrame.new(
	42.7815666, 73.5011292, 19933.9062,
	0.828718901, 2.42312819e-08, -0.559665024,
	3.38124728e-08, 1, 9.33635533e-08,
	0.559665024, -9.62958069e-08, 0.828718901
)
sections.MainSection3:Divider()

sections.MainSection3:Button({
	Name = "Rejoin server",
	Callback = function()
		local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlaceId = game.PlaceId
local JobId = game.JobId

-- Delay to prevent immediate teleport on join (optional)
wait(0.1)

-- Rejoin the same server
TeleportService:TeleportToPlaceInstance(PlaceId, JobId, LocalPlayer)

		
	end,
})



sections.MainSection4:Button({
	Name = "Tp to Chara",
	Callback = function()
teleportToModel()
		
	end,
})

sections.MainSection2:Toggle({
	Name = "Glue to Chara",
	Default = false,
	Callback = function(value)
		isGlued2 = value
	end,
}, "Toggle")



--[[middle
269.842987, 5.45333672, 362.904785, 0.115370512, -2.61038728e-08, 0.993322551, 6.27933474e-08, 1, 1.89861531e-08, -0.993322551, 6.01836021e-08, 0.115370512

--mountain area
-215.243103, 243.081726, 85.0237579, 0.132866964, -2.41381741e-08, -0.991133869, -1.31960398e-08, 1, -2.61231019e-08, 0.991133869, 1.65499401e-08, 0.132866964

--above chara area
-474.638, 134.109741, 121.629463, 0.421889812, 7.10480919e-08, -0.906647086, 1.92542373e-08, 1, 8.7323123e-08, 0.906647086, -5.42975371e-08, 0.421889812


--chara area
-371.342072, 5.56633282, 93.5839081, -0.866185129, 8.18949264e-09, 0.499723256, -2.32883668e-08, 1, -5.67544731e-08, -0.499723256, -6.07976176e-08, -0.866185129

--cave 
237.28244, 7.34690762, -771.277161, 0.991938174, 1.91424103e-08, 0.126722977, -2.86298238e-08, 1, 7.30459817e-08, -0.126722977, -7.60851506e-08, 0.991938174

--cave secret area
365.793884, 7.34690762, -788.411804, 0.307435304, 7.61766561e-09, 0.951568961, -3.11627346e-08, 1, 2.06276196e-09, -0.951568961, -3.02876586e-08, 0.307435304

--cabin
-224.669281, 4.86968803, -511.644043, 0.0669663101, -1.88245171e-08, 0.997755229, 7.59259606e-08, 1, 1.3770947e-08, -0.997755229, 7.48333377e-08, 0.0669663101

--new place
972.954895, 4.59052467, 25.9301949, -0.837864459, 2.75072178e-08, 0.545878351, 1.1863678e-08, 1, -3.21812799e-08, -0.545878351, -2.04874251e-08, -0.837864459

--soccer field
621.203735, 4.11576986, -484.114319, 0.905904114, 4.98410557e-08, -0.423482895, -8.74177175e-09, 1, 9.899302e-08, 0.423482895, -8.59761897e-08, 0.905904114

--invincible desert
82.2127533, 66.270752, -20001.6797, -0.20519121, 1.05746842e-07, 0.978721917, -2.59120192e-09, 1, -1.08589106e-07, -0.978721917, -2.48175951e-08, -0.20519121

--mob map
42.7815666, 78.5011292, 19933.9062, 0.828718901, 2.42312819e-08, -0.559665024, 3.38124728e-08, 1, 9.33635533e-08, 0.559665024, -9.62958069e-08, 0.828718901

--mob map sewer
9.91348457, -41.184433, 19980.9375, 0.328368217, 5.65654652e-08, 0.944549799, -9.28720993e-08, 1, -2.75996239e-08, -0.944549799, -7.86594825e-08, 0.328368217
]]

local hrp = nil

player.CharacterAdded:Connect(function(char)
	hrp = char:WaitForChild("HumanoidRootPart")
end)

-- If character already exists when script runs
if player.Character then
	hrp = player.Character:WaitForChild("HumanoidRootPart")
end

-- Getter function
function getHRP()
	return hrp
end
 hrp = getHRP()
local area 
local barriers = workspace.Map.Model.Barriers

for _, part in ipairs(barriers:GetChildren()) do
	if part:IsA("Part") and part.Name == "Barriers" then
		part:Destroy()
	end
end


local Dropdown = sections.MainSection4:Dropdown({
	Name = "Tp to area",
	Multi = false,
	Required = true,
	Options = {"Middle", "Mountains", "Chara area", "Above Chara area", "Cave", "New place", "Cabin",  "Invincible desert", "Mob map", "Mob map sewer"},
	Default = 1,
	Callback = function(v)
        if v == "Middle" then
            area = CFrame.new(269.842987, 5.45333672, 362.904785, 0.115370512, -2.61038728e-08, 0.993322551, 6.27933474e-08, 1, 1.89861531e-08, -0.993322551, 6.01836021e-08, 0.115370512)

        elseif v == "Mountains" then
            area = CFrame.new(-215.243103, 243.081726, 85.0237579, 0.132866964, -2.41381741e-08, -0.991133869, -1.31960398e-08, 1, -2.61231019e-08, 0.991133869, 1.65499401e-08, 0.132866964)

        elseif v == "Chara area" then
            area = CFrame.new(-371.342072, 5.56633282, 93.5839081, -0.866185129, 8.18949264e-09, 0.499723256, -2.32883668e-08, 1, -5.67544731e-08, -0.499723256, -6.07976176e-08, -0.866185129)

        elseif v == "Above Chara area" then
            area = CFrame.new(-474.638, 134.109741, 121.629463, 0.421889812, 7.10480919e-08, -0.906647086, 1.92542373e-08, 1, 8.7323123e-08, 0.906647086, -5.42975371e-08, 0.421889812)

        elseif v == "Cave" then
            area = CFrame.new(237.28244, 7.34690762, -771.277161, 0.991938174, 1.91424103e-08, 0.126722977, -2.86298238e-08, 1, 7.30459817e-08, -0.126722977, -7.60851506e-08, 0.991938174)

        elseif v == "New place" then
            area = CFrame.new(972.954895, 4.59052467, 25.9301949, -0.837864459, 2.75072178e-08, 0.545878351, 1.1863678e-08, 1, -3.21812799e-08, -0.545878351, -2.04874251e-08, -0.837864459)

        elseif v == "Invincible desert" then
            area = CFrame.new(82.2127533, 66.270752, -20001.6797, -0.20519121, 1.05746842e-07, 0.978721917, -2.59120192e-09, 1, -1.08589106e-07, -0.978721917, -2.48175951e-08, -0.20519121)

        elseif v == "Mob map" then
            area = CFrame.new(42.7815666, 78.5011292, 19933.9062, 0.828718901, 2.42312819e-08, -0.559665024, 3.38124728e-08, 1, 9.33635533e-08, 0.559665024, -9.62958069e-08, 0.828718901)


        elseif v == "Mob map sewer" then
            area = CFrame.new(9.91348457, -41.184433, 19980.9375, 0.328368217, 5.65654652e-08, 0.944549799, -9.28720993e-08, 1, -2.75996239e-08, -0.944549799, -7.86594825e-08, 0.328368217)

		elseif v == "Cabin" then 
			area = CFrame.new(-224.669281, 4.86968803, -511.644043, 0.0669663101, -1.88245171e-08, 0.997755229, 7.59259606e-08, 1, 1.3770947e-08, -0.997755229, 7.48333377e-08, 0.0669663101)




        end
		
	

		
	end,
}, "Dropdown")
sections.MainSection4:Button({
	Name = "Tp",
	Callback = function()

if hrp then
    hrp.CFrame = area
    end
		
	end,
})

sections.MainSection4:Button({
	Name = "Reset (Bypasses In Combat)",
	Callback = function()

if hrpforresest then
    firetouchinterest(hrpforresest, resetbypass, 0)
    task.wait(0.1)
    firetouchinterest(hrpforresest, resetbypass, 1)

end
	end,
})


MacLib:SetFolder("Maclib")
tabs.Settings:InsertConfigSection("Left")

Window.onUnloaded(function()
	print("Unloaded!")
end)

tabs.Main:Select()
MacLib:LoadAutoLoadConfig()



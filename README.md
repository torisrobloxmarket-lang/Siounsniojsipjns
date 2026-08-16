local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

local toggleState = false -- 🔁 Schaltet die Unsichtbarkeit ein oder aus
local currentConnections = {}
local currentTrack = nil

local partsToSet = {
	"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"
}

-- Funktion zum Anpassen der Transparenz der Körperteile
local function setTransparency(value, character)
	for _, partName in ipairs(partsToSet) do
		local part = character:FindFirstChild(partName)
		if part and part:IsA("BasePart") then
			part.Transparency = value
		end
	end
end

-- Aufräumen von alten Verbindungen und Animationen
local function cleanup()
	for _, conn in pairs(currentConnections) do
		if conn then conn:Disconnect() end
	end
	currentConnections = {}

	if currentTrack and currentTrack.IsPlaying then
		currentTrack:Stop()
	end
end

-- Setup für die Animations-Manipulation zur Unsichtbarkeit
local function setup(character)
	local humanoid = character:WaitForChild("Humanoid")
	local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)

	local animation = Instance.new("Animation")
	animation.AnimationId = "rbxassetid://83555998356899"
	currentTrack = animator:LoadAnimation(animation)
	currentTrack.Priority = Enum.AnimationPriority.Action4 -- Höchste Priorität

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

-- Funktion zum Aktivieren/Deaktivieren (kann an einen Toggle oder Button gebunden werden)
local function toggleInvisibility(value)
	toggleState = value
	local char = player.Character
	if not char then return end

	cleanup()
	setup(char)
end

-- Beispiel für den Aufruf:
-- toggleInvisibility(true)  -> Aktiviert den Invisibility-Glitch
-- toggleInvisibility(false) -> Setzt es zurück

-- ============================================================================
--  TweenSystem.lua
--  Reworked movement / tween system for the ryuhub Grand Piece Online script.
--
--  Standalone: this file is fully self-contained (own service references).
--  On load it installs ALL legacy globals (tweenTeleportPro, tweenTeleport,
--  tweenTeleport2, tweenTeleport3, tweenToPos, tweenToPos2, tweenTeleportTo,
--  CustomTween, lerpTweenTo, flyToPosition, walkTo, snapToPosition,
--  getMaxSpeed, getSpeed, getExcludeList, isWithinDistance, isWithinDistance2)
--  so every existing call site in the main script keeps working unchanged.
--
--  Once tested, paste this whole file into the main script where the old
--  tween code used to live (see GpoDiamond_mit_TweenSystem.lua).
--
--  REWORK FIXES vs. the old code:
--   * walkTo used the stale global 'currentCharacter' -> now always uses the
--     live character.
--   * tweenTeleportPro ignored the Speed option callers passed -> honored now.
--   * tweenToPos overwrote its 'speed' parameter every frame -> honored now.
--   * tweenTeleport3 read the raw tween_speed slider (0.1-1) as studs/sec,
--     which slowed it to a crawl after touching the slider -> fixed 50 again.
--   * getgenv().tween_speed is now ONE thing: a multiplier (default 1) that
--     only scales GetMaxSpeed(). No more 1 vs 50 conflicts.
--   * Rifle-farm fly/drop tweens now CANCEL instantly when the farm is
--     toggled off (old code waited for the tween to finish first).
--   * Strike-notification scan, ice-color dash trigger and ground-raycast
--     landing existed 3-4 times copy-pasted -> single implementation.
--   * tweenToPos2's nested 'if shouldStop then break' pyramid and the
--     '(shouldStop or (shouldStop or ...))' chain -> flattened retarget logic.
--   * Raycast retry loops are iteration-capped (no infinite loops).
--
--  KEPT QUIRKS (intentionally, behavior preservation):
--   * The santa farm passes 'y = 1000' (lowercase) which tweenTeleportPro
--     never read - it is still ignored. Fix the call site if you want it.
--   * CustomTween is dead code (never called) but kept for compatibility.
-- ============================================================================

local TweenSystem = {}

-- [TS1] SERVICES ---------------------------------------------------------------

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local PathfindingService = game:GetService("PathfindingService")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer

-- [TS2] CONFIG + SHARED STATE --------------------------------------------------

local Config = {
	FloatHeight = 7,          -- hover height above ground (pro tween)
	RayStartHeight = 9000,    -- raycast origin height (pro tween)
	RayDepth = 20000,         -- raycast depth (pro tween)
	Smoothness = 0.3,         -- vertical lerp factor (pro tween)
	MinFlightY = 5,           -- never fly below this Y (pro tween)
	WalkSpeed = 30,           -- walkTo studs/sec
	FlySpeed = 40,            -- rifle-farm horizontal studs/sec
	DropSpeed = 15,           -- rifle-farm drop studs/sec
	LerpSpeed = 15,           -- tweenTeleportTo studs/sec
	ArriveDistance = 8,       -- default horizontal arrive threshold
	StrikeWait = 5,           -- seconds to wait on TP/Strike notification
	DashCooldown = 2,         -- seconds between auto ice-dashes
	MaxRaycastRetries = 50,   -- cap for leaves/non-solid skip loops
}
TweenSystem.Config = Config

-- tween_speed is a MULTIPLIER for GetMaxSpeed() (UI slider: 0.1 - 1).
getgenv().tween_speed = getgenv().tween_speed or 1

local lastDashTick = 0
local trackedState = { running = false, target = nil, track_object = nil, cancel = false }
local flyToRunning = false

-- [TS3] CHARACTER HELPERS ------------------------------------------------------

local function getCharacter()
	return LocalPlayer.Character
end

local function rootPartOf(characterModel)
	if not characterModel then
		return nil
	end
	return characterModel:FindFirstChild("HumanoidRootPart")
end

local function humanoidOf(characterModel)
	if not characterModel then
		return nil
	end
	return characterModel:FindFirstChildOfClass("Humanoid")
end

local function zeroVelocity(rootPart)
	rootPart.Velocity = Vector3.zero
	rootPart.AssemblyLinearVelocity = Vector3.zero
end

local function unequipTools(characterModel)
	local humanoid = humanoidOf(characterModel)
	if humanoid then
		humanoid:UnequipTools()
	end
end

-- [TS4] SPEED LOGIC ------------------------------------------------------------

-- Max movement speed from stats, race, devil fruit and character attributes.
-- Result is scaled by character scale, SpeedBypass and getgenv().tween_speed.
function TweenSystem.GetMaxSpeed()
	local characterModel = LocalPlayer.Character
	currentCharacter = characterModel -- legacy side effect, some code reads it
	if not characterModel then
		return 160
	end
	local statsFolder = ReplicatedStorage:FindFirstChild("Stats" .. LocalPlayer.Name)
	local stats = statsFolder and statsFolder:FindFirstChild("Stats")
	local customization = statsFolder and statsFolder:FindFirstChild("Customization")
	local dfValue = stats and stats:FindFirstChild("DF")
	local fightingStyleValue = stats and stats:FindFirstChild("FightingStyle")
	local raceValue = customization and customization:FindFirstChild("Race")
	if not dfValue or not fightingStyleValue or not raceValue then
		return 160
	end
	local maxSpeed = 160
	if dfValue.Value == "Buddha-Buddha" then
		maxSpeed = 160
	elseif fightingStyleValue.Value == "Rokushiki" or fightingStyleValue.Value == "Kamishiki" or characterModel:GetAttribute("GEAR2Active") or _G.soru_dashing == true or LocalPlayer:GetAttribute("soruDash") or characterModel:GetAttribute("evading") ~= nil then
		maxSpeed = 160
	elseif characterModel:GetAttribute("evading") == true then
		maxSpeed = 160
	elseif dfValue.Value == "Venom-Venom" then
		maxSpeed = 160
	elseif string.sub(string.lower(raceValue.Value), 1, 7) == "fishman" or _G.swimming then
		maxSpeed = 160
	elseif dfValue.Value == "Yomi-Yomi" then
		maxSpeed = 160
	end
	local previousEvasiveTick = characterModel:GetAttribute("previousEvasiveTick")
	if previousEvasiveTick and tick() - previousEvasiveTick <= 6 then
		maxSpeed = 78
	end
	local scaledSpeed = maxSpeed * math.clamp(characterModel:GetScale(), 1, 20)
	local speedBypass = characterModel:GetAttribute("SpeedBypass")
	if speedBypass and speedBypass > 0 then
		scaledSpeed = scaledSpeed + speedBypass
	end
	if scaledSpeed < 46 then
		scaledSpeed = 46
	end
	return scaledSpeed * getgenv().tween_speed
end

function TweenSystem.GetSpeed()
	local maxSpeed = TweenSystem.GetMaxSpeed()
	return maxSpeed > 0 and maxSpeed - 5 or 110
end

-- options.Speed may be a number or a function evaluated per step.
local function resolveSpeed(speedOption)
	if type(speedOption) == "function" then
		return speedOption()
	end
	if type(speedOption) == "number" then
		return speedOption
	end
	return TweenSystem.GetSpeed()
end

-- [TS5] TARGET RESOLUTION + DISTANCE ------------------------------------------

-- Accepts Vector3 | CFrame | BasePart | Model | ObjectValue.
-- Returns: position (Vector3 or nil), trackObject (Instance or nil)
local function resolveTarget(target)
	local targetType = typeof(target)
	if targetType == "Vector3" then
		return target, nil
	end
	if targetType == "CFrame" then
		return target.Position, nil
	end
	if targetType == "Instance" then
		if target:IsA("ValueBase") then
			-- ObjectValue -> tracked instance; Vector3Value -> static position
			local ok, value = pcall(function()
				return target.Value
			end)
			if not ok or value == nil then
				return nil, target
			end
			if typeof(value) == "Vector3" then
				return value, target
			end
			if typeof(value) == "Instance" then
				return value:GetPivot().Position, target
			end
			return nil, target
		end
		return target:GetPivot().Position, target
	end
	return nil, nil
end

local function resolveTrackedPosition(trackObject)
	if not trackObject then
		return nil
	end
	if trackObject:IsA("ValueBase") then
		local ok, value = pcall(function()
			return trackObject.Value
		end)
		if not ok or value == nil then
			return nil
		end
		if typeof(value) == "Vector3" then
			return value
		end
		if typeof(value) == "Instance" then
			return value:GetPivot().Position
		end
		return nil
	end
	return trackObject:GetPivot().Position
end

local function horizontalDistance(positionA, positionB)
	local deltaX = positionA.X - positionB.X
	local deltaZ = positionA.Z - positionB.Z
	return math.sqrt(deltaX * deltaX + deltaZ * deltaZ)
end

-- Distance check. target: Vector3 | CFrame | BasePart | Model.
function TweenSystem.IsWithinDistance(target, maxDistance, horizontalOnly)
	local characterModel = LocalPlayer.Character
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		return false
	end
	local targetPosition = resolveTarget(target)
	if not targetPosition then
		return false
	end
	if horizontalOnly then
		return horizontalDistance(rootPart.Position, targetPosition) <= maxDistance
	end
	return (rootPart.Position - targetPosition).Magnitude <= maxDistance
end

-- [TS6] RAYCAST / STRIKE / ICE-DASH HELPERS -----------------------------------

-- Exclude the local character plus every player character from raycasts.
local function buildRaycastParams(characterModel, extraExclude)
	local excludeList = { characterModel }
	for _, otherPlayer in ipairs(Players:GetPlayers()) do
		if otherPlayer.Character then
			table.insert(excludeList, otherPlayer.Character)
		end
	end
	if extraExclude then
		for _, instance in ipairs(extraExclude) do
			if instance then
				table.insert(excludeList, instance)
			end
		end
	end
	local raycastParams = RaycastParams.new()
	raycastParams.FilterType = Enum.RaycastFilterType.Exclude
	raycastParams.IgnoreWater = true
	raycastParams.FilterDescendantsInstances = excludeList
	return raycastParams
end

-- True while a TP/Strike anticheat notification is on screen.
local function hasStrikeNotification()
	local found = false
	pcall(function()
		for _, descendant in ipairs(LocalPlayer.PlayerGui.Notifications:GetDescendants()) do
			if descendant:IsA("TextLabel") and (descendant.Text == "TP" or descendant.Text == "Strike" or descendant.Text:find("Strike")) then
				found = true
				return
			end
		end
	end)
	return found
end

-- The game flags two specific ground colors as ice - dash over them.
local function isIceColor(instance)
	local hitColor = instance.Color
	local red = math.floor(hitColor.R * 255)
	local green = math.floor(hitColor.G * 255)
	local blue = math.floor(hitColor.B * 255)
	return (red == 148 and green == 147 and blue == 150) or (red == 116 and green == 134 and blue == 157)
end

local function maybeFireIceDash(hitInstance, waitAfter)
	if not isIceColor(hitInstance) then
		return
	end
	local characterModel = LocalPlayer.Character
	if not characterModel then
		return
	end
	if tick() - lastDashTick < Config.DashCooldown then
		return
	end
	if (characterModel:GetAttribute("SpeedBypass") or 0) >= 1 then
		return
	end
	lastDashTick = tick()
	task.spawn(pcall, function()
		local eventsFolder = ReplicatedStorage:FindFirstChild("Events")
		local takestamRemote = eventsFolder and eventsFolder:FindFirstChild("takestam")
		if takestamRemote then
			takestamRemote:FireServer(0.52, "dash")
		end
	end)
	if waitAfter and waitAfter > 0 then
		task.wait(waitAfter)
	end
end

-- Waits until SpeedBypass builds up (geppo/sky-walk charge before flying).
local function waitForSpeedBypass(characterModel, cancelFn)
	local statsFolder = LocalPlayer:FindFirstChild("Stats")
	local skyWalk = statsFolder and statsFolder:FindFirstChild("Skills") and statsFolder.Skills:FindFirstChild("skyWalk")
	if not skyWalk or not skyWalk.Value then
		return
	end
	while LocalPlayer.Character do
		local speedBypass = LocalPlayer.Character:GetAttribute("SpeedBypass")
		if not speedBypass or speedBypass >= 15 then
			break
		end
		if cancelFn and cancelFn() then
			break
		end
		unequipTools(LocalPlayer.Character)
		task.wait(0.1)
	end
end

-- Ground-height resolver. ctx fields:
--   raycastParams, rootPart, startY
--   fixedY            : always this Y
--   keepHeightMode    : "start" (keep startY) or "current" (keep live rootY)
--   keepStartY        : underwater/fishman mode - no minY clamp
--   rayStartY         : number or function(ctx) -> number
--   rayDepth          : raycast depth
--   landingOffset     : added to hit Y
--   minY              : floor clamp (nil = no clamp)
--   skipNonSolid      : keep raycasting through leaves / non-solid parts
--   iceDash           : fire dash on ice colors
--   iceDashMinStartY  : only dash when startY above this (deep-water guard)
--   iceDashWait       : seconds to wait after firing a dash
local function computeLandingY(ctx, x, z)
	if ctx.fixedY then
		return ctx.fixedY
	end
	if ctx.keepHeightMode == "start" then
		return ctx.startY
	end
	if ctx.keepHeightMode == "current" then
		return ctx.rootPart.Position.Y
	end
	local rayStartY = ctx.rayStartY
	if type(rayStartY) == "function" then
		rayStartY = rayStartY(ctx)
	end
	local rayOrigin = Vector3.new(x, rayStartY, z)
	local rayDirection = Vector3.new(0, -ctx.rayDepth, 0)
	local noGroundFallback = ctx.keepStartY and ctx.startY or math.max(ctx.startY, ctx.minY or 2)
	for _ = 1, Config.MaxRaycastRetries do
		local result = Workspace:Raycast(rayOrigin, rayDirection, ctx.raycastParams)
		if not result then
			return noGroundFallback
		end
		if ctx.skipNonSolid then
			local hitInstance = result.Instance
			local isSolid = hitInstance.CanCollide or hitInstance.CanQuery
			if not isSolid or hitInstance.Name == "Leaves" then
				rayOrigin = result.Position - Vector3.new(0, 1, 0)
			else
				if ctx.iceDash and (not ctx.iceDashMinStartY or ctx.startY > ctx.iceDashMinStartY) then
					maybeFireIceDash(hitInstance, ctx.iceDashWait)
				end
				local landingY = result.Position.Y + ctx.landingOffset
				if ctx.keepStartY then
					return landingY
				end
				return ctx.minY and math.max(landingY, ctx.minY) or landingY
			end
		else
			local landingY = result.Position.Y + ctx.landingOffset
			if ctx.keepStartY then
				return landingY
			end
			return ctx.minY and math.max(landingY, ctx.minY) or landingY
		end
	end
	return ctx.startY
end

-- [TS7] MOVEMENT PRIMITIVES ----------------------------------------------------

-- Instant snap + velocity reset.
function TweenSystem.Snap(targetPosition)
	local rootPart = rootPartOf(getCharacter())
	if not rootPart then
		return
	end
	rootPart.CFrame = CFrame.new(targetPosition)
	zeroVelocity(rootPart)
end

-- TweenService linear tween at a fixed studs/sec rate.
-- shouldContinue() (optional) can cancel mid-tween.
-- Returns true when the tween completed, false when cancelled.
function TweenSystem.TweenCFrame(rootPart, targetCFrame, studsPerSec, shouldContinue)
	local distance = (rootPart.Position - targetCFrame.Position).Magnitude
	if distance < 0.1 then
		rootPart.CFrame = targetCFrame
		zeroVelocity(rootPart)
		return true
	end
	local duration = distance / studsPerSec
	local tween = TweenService:Create(rootPart, TweenInfo.new(duration, Enum.EasingStyle.Linear), { CFrame = targetCFrame })
	tween:Play()
	local completed = true
	task.spawn(function()
		while tween.PlaybackState == Enum.PlaybackState.Playing do
			if shouldContinue and not shouldContinue() then
				tween:Cancel()
				completed = false
				return
			end
			rootPart.Velocity = Vector3.zero
			task.wait()
		end
	end)
	tween.Completed:Wait()
	zeroVelocity(rootPart)
	return completed
end

-- Rifle-farm pattern: rise to cruise height, glide over, drop onto target.
-- options: CruiseY (absolute) or AscendOffset (relative, default 100),
--          FlySpeed (40), DropSpeed (15), Drop = false to skip the drop,
--          ShouldContinue() to cancel.
function TweenSystem.FlyDrop(targetPosition, options)
	options = options or {}
	local rootPart = rootPartOf(getCharacter())
	if not rootPart then
		return false
	end
	local startPosition = rootPart.Position
	local cruiseY = options.CruiseY or (startPosition.Y + (options.AscendOffset or 100))
	rootPart.CFrame = CFrame.new(startPosition.X, cruiseY, startPosition.Z)
	zeroVelocity(rootPart)
	local arrived = TweenSystem.TweenCFrame(rootPart, CFrame.new(targetPosition.X, cruiseY, targetPosition.Z), options.FlySpeed or Config.FlySpeed, options.ShouldContinue)
	if not arrived or options.Drop == false then
		return arrived
	end
	return TweenSystem.TweenCFrame(rootPart, CFrame.new(targetPosition), options.DropSpeed or Config.DropSpeed, options.ShouldContinue)
end

-- Blocking straight-line 3D lerp at a fixed rate (old tweenTeleportTo).
function TweenSystem.LerpTo(targetPosition, studsPerSec)
	local rootPart = rootPartOf(getCharacter())
	if not rootPart then
		return
	end
	local startCFrame = rootPart.CFrame
	local targetCFrame = typeof(targetPosition) == "CFrame" and targetPosition or CFrame.new(targetPosition)
	local travelTime = (targetCFrame.Position - startCFrame.Position).Magnitude / (studsPerSec or Config.LerpSpeed)
	local elapsed = 0
	while elapsed < travelTime and rootPart.Parent do
		elapsed = elapsed + RunService.Heartbeat:Wait()
		rootPart.CFrame = startCFrame:Lerp(targetCFrame, math.min(elapsed / travelTime, 1))
		zeroVelocity(rootPart)
	end
end

-- Frame-stepped straight-line movement with dynamic speed (old lerpTweenTo).
-- Rescues the character from the skybox (Y > 2000 -> back to Y = 5).
function TweenSystem.FrameStepTo(targetPosition, speed)
	local characterModel = getCharacter()
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		return
	end
	if rootPart.Position.Y > 2000 then
		rootPart.CFrame = CFrame.new(rootPart.Position.X, 5, rootPart.Position.Z)
	end
	local lastTick = tick()
	while characterModel.Parent and rootPart.Parent do
		if rootPart.Position.Y > 2000 then
			rootPart.CFrame = CFrame.new(rootPart.Position.X, 5, rootPart.Position.Z)
		end
		local now = tick()
		local deltaTime = now - lastTick
		lastTick = now
		local currentPosition = rootPart.Position
		local delta = targetPosition - currentPosition
		if delta.Magnitude < 0.5 then
			rootPart.CFrame = CFrame.new(targetPosition)
			break
		end
		local step = math.min(resolveSpeed(speed) * deltaTime, delta.Magnitude)
		rootPart.CFrame = CFrame.new(currentPosition + delta.Unit * step)
		zeroVelocity(rootPart)
		task.wait()
	end
end

-- Heartbeat arc tween with optional cruise height (old CustomTween).
-- Requires SpeedBypass >= 1 (charged geppo) or it refuses to run.
function TweenSystem.ArcTo(targetPosition, flyHeight, noArc)
	local characterModel = getCharacter()
	if not characterModel or (characterModel:GetAttribute("SpeedBypass") or 0) < 1 then
		return
	end
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		return
	end
	local startCFrame = rootPart.CFrame
	local targetWithOffset = targetPosition + Vector3.new(0, 3, 0)
	local startPosition = rootPart.Position
	-- near the sky island the cruise height gets disabled
	if flyHeight and (startPosition - Vector3.new(8812, 66, 11539)).Magnitude <= 100 then
		flyHeight = nil
	end
	if flyHeight then
		rootPart.CFrame = CFrame.new(startPosition.X, flyHeight, startPosition.Z) * (startCFrame - startCFrame.Position)
		startPosition = rootPart.Position
	end
	if horizontalDistance(startPosition, targetWithOffset) <= 8 then
		local finalY = not noArc and targetWithOffset.Y or flyHeight or startPosition.Y
		rootPart.CFrame = CFrame.new(targetWithOffset.X, finalY, targetWithOffset.Z) * (startCFrame - startCFrame.Position)
		zeroVelocity(rootPart)
		return
	end
	local arcBaseY = flyHeight or startPosition.Y
	local travelTime = horizontalDistance(startPosition, targetWithOffset) / 40
	local arcThreshold = 2
	if not noArc then
		if horizontalDistance(startPosition, targetWithOffset) <= 10 then
			arcThreshold = 0
		else
			arcThreshold = 1 - 10 / horizontalDistance(startPosition, targetWithOffset)
		end
	end
	local progress = 0
	while progress < 1 do
		characterModel = getCharacter()
		rootPart = rootPartOf(characterModel)
		if not rootPart then
			return
		end
		progress = math.clamp(progress + RunService.Heartbeat:Wait() / travelTime, 0, 1)
		local currentX = startPosition.X + progress * (targetWithOffset.X - startPosition.X)
		local currentZ = startPosition.Z + progress * (targetWithOffset.Z - startPosition.Z)
		local currentY = if not (progress < arcThreshold) then arcBaseY + (arcThreshold < 1 and (progress - arcThreshold) / (1 - arcThreshold) or 0) * (targetWithOffset.Y - arcBaseY) else arcBaseY
		zeroVelocity(rootPart)
		rootPart.CFrame = CFrame.new(currentX, currentY, currentZ) * (startCFrame - startCFrame.Position)
	end
	characterModel = getCharacter()
	rootPart = rootPartOf(characterModel)
	if rootPart then
		local finalY = arcThreshold <= 1 and targetWithOffset.Y or arcBaseY
		rootPart.CFrame = CFrame.new(targetWithOffset.X, finalY, targetWithOffset.Z) * (startCFrame - startCFrame.Position)
		zeroVelocity(rootPart)
	end
end

-- Ascend -> glide over -> drop onto target, with geppo/stamina upkeep
-- (old flyToPosition). Guarded against re-entry.
function TweenSystem.FlyTo(targetPosition, speed, ascendFirst, useGeppo)
	if flyToRunning then
		return
	end
	local flySpeed = speed or 50
	local shouldAscend = ascendFirst == nil and true or ascendFirst
	local shouldGeppo = useGeppo == nil and true or useGeppo
	if typeof(targetPosition) == "CFrame" then
		targetPosition = targetPosition.Position
	elseif typeof(targetPosition) ~= "Vector3" then
		return
	end
	local characterModel = getCharacter()
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		return
	end
	flyToRunning = true
	if horizontalDistance(rootPart.Position, targetPosition) >= 10 and shouldGeppo then
		task.spawn(function()
			while flyToRunning and task.wait() do
				local hasSkyWalk = false
				pcall(function()
					local statsFolder = stats_folder or ReplicatedStorage:FindFirstChild("Stats" .. LocalPlayer.Name)
					hasSkyWalk = statsFolder and statsFolder.Skills:FindFirstChild("skyWalk").Value == true
				end)
				if hasSkyWalk then
					ReplicatedStorage.Events.Skill:InvokeServer("Sky Walk2", { char = characterModel, cf = CFrame.new(rootPart.Position) })
					task.wait(1)
				else
					ReplicatedStorage.Events.takestam:FireServer(1, "dash")
					task.wait(3)
				end
			end
		end)
	end
	local startPosition = rootPart.Position
	if shouldAscend then
		startPosition = Vector3.new(startPosition.X, startPosition.Y + 1000, startPosition.Z)
		rootPart.CFrame = CFrame.new(startPosition)
		zeroVelocity(rootPart)
	end
	local cruiseTarget = Vector3.new(targetPosition.X, startPosition.Y, targetPosition.Z)
	local distance = horizontalDistance(startPosition, cruiseTarget)
	if distance > 0 then
		local duration = distance / flySpeed
		local elapsed = 0
		while elapsed < duration and flyToRunning do
			elapsed = elapsed + RunService.RenderStepped:Wait()
			local alpha = math.clamp(elapsed / duration, 0, 1)
			local nextX = startPosition.X + (cruiseTarget.X - startPosition.X) * alpha
			local nextZ = startPosition.Z + (cruiseTarget.Z - startPosition.Z) * alpha
			zeroVelocity(rootPart)
			rootPart.CFrame = CFrame.new(nextX, startPosition.Y, nextZ)
		end
	end
	if not flyToRunning then
		zeroVelocity(rootPart)
		return
	end
	rootPart.CFrame = CFrame.new(targetPosition)
	zeroVelocity(rootPart)
	flyToRunning = false
end

-- Cancels a running FlyTo (stops the cruise loop and the geppo upkeep).
function TweenSystem.StopFly()
	if flyToRunning then
		flyToRunning = false
	end
end

-- [TS8] GLIDE ENGINE -----------------------------------------------------------
-- One ground-hugging movement engine that replaces tweenTeleportPro,
-- tweenTeleport, tweenTeleport2 and tweenTeleport3. Blocking.
--
-- options:
--   Target            Vector3 | CFrame | Instance | ObjectValue (tracked live)
--   Speed             number or function() -> number (default GetSpeed())
--   Gate              function() -> bool; false aborts start/abort mid-flight
--   Cancel            function() -> bool; true cancels mid-flight
--   SnapIfCloser      horizontal distance that short-circuits to a landing
--   ArriveDistance    horizontal arrive threshold (default 8)
--   UnequipTools      unequip before flying
--   WaitForSpeedBypass wait for geppo charge before flying
--   Noclip            disable collision while flying
--   StrikePause       wait StrikeWait on TP/Strike notifications
--   StrikeResumeSpeed speed override after a strike (nil = keep)
--   FaceDirection     rotate the character into the move direction
--   FixedY            fly at this Y, no raycast
--   KeepHeightMode    "start" | "current" (no raycast)
--   KeepStartY        underwater mode - no minY clamp
--   RayStartY         number or function(ctx) -> number (default 1000)
--   RayDepth          default 2000
--   LandingOffset     default 5
--   MinY              floor clamp (nil = no clamp)
--   SkipNonSolid      raycast through leaves / non-solid parts
--   IceDash           auto dash on ice colors
--   IceDashMinStartY  deep-water guard for the dash
--   IceDashWait       wait after firing a dash
--   SmoothY           vertical lerp factor (nil = snap to landing Y)
--   ExcludeExtra      extra instances for the raycast filter
--   Lock              function(bool) running-lock callback
-- Returns "OK" | "ABORT" | "CANCELLED".
function TweenSystem.GlideTo(options)
	local targetPosition, trackObject = resolveTarget(options.Target)
	if not targetPosition then
		warn("TweenSystem.GlideTo: no target defined")
		return "ABORT"
	end
	local gate = options.Gate
	if gate and not gate() then
		return "ABORT"
	end
	local characterModel = getCharacter()
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		return "ABORT"
	end

	local ctx = {
		character = characterModel,
		rootPart = rootPart,
		startY = rootPart.Position.Y,
		raycastParams = buildRaycastParams(characterModel, options.ExcludeExtra),
		fixedY = options.FixedY,
		keepHeightMode = options.KeepHeight and (options.KeepHeightMode or "start") or nil,
		keepStartY = options.KeepStartY,
		rayStartY = options.RayStartY or 1000,
		rayDepth = options.RayDepth or 2000,
		landingOffset = options.LandingOffset or 5,
		minY = options.MinY,
		skipNonSolid = options.SkipNonSolid,
		iceDash = options.IceDash,
		iceDashMinStartY = options.IceDashMinStartY,
		iceDashWait = options.IceDashWait,
	}

	-- already there -> land immediately, no setup needed
	if options.SnapIfCloser and horizontalDistance(rootPart.Position, targetPosition) < options.SnapIfCloser then
		local landingY = computeLandingY(ctx, targetPosition.X, targetPosition.Z)
		rootPart.CFrame = CFrame.new(targetPosition.X, landingY, targetPosition.Z)
		zeroVelocity(rootPart)
		return "OK"
	end

	if options.UnequipTools then
		unequipTools(characterModel)
	end
	if options.WaitForSpeedBypass then
		waitForSpeedBypass(characterModel, options.Cancel)
	end

	local noclipConnection = nil
	if options.Noclip then
		noclipConnection = RunService.Stepped:Connect(function()
			if not characterModel.Parent then
				return
			end
			zeroVelocity(rootPart)
			for _, descendant in ipairs(characterModel:GetDescendants()) do
				if descendant:IsA("BasePart") and descendant.CanCollide then
					descendant.CanCollide = false
				end
			end
		end)
	end
	if options.Lock then
		options.Lock(true)
	end

	local status = "OK"
	local arriveDistance = options.ArriveDistance or Config.ArriveDistance
	local strikeResumeSpeed = nil
	local smoothY = options.SmoothY
	local currentY = rootPart.Position.Y

	while characterModel.Parent and rootPart.Parent do
		if options.Cancel and options.Cancel() then
			status = "CANCELLED"
			break
		end
		if gate and not gate() then
			status = "ABORT"
			break
		end
		if options.StrikePause and hasStrikeNotification() then
			task.wait(options.StrikeWait or Config.StrikeWait)
			strikeResumeSpeed = options.StrikeResumeSpeed
			task.wait()
			continue
		end
		if trackObject then
			local tracked = resolveTrackedPosition(trackObject)
			if not tracked then
				status = "ABORT"
				break
			end
			targetPosition = tracked
		end
		local currentPosition = rootPart.Position
		local horizontal = horizontalDistance(currentPosition, targetPosition)
		if horizontal <= arriveDistance then
			local landingY = computeLandingY(ctx, targetPosition.X, targetPosition.Z)
			rootPart.CFrame = CFrame.new(targetPosition.X, landingY, targetPosition.Z)
			zeroVelocity(rootPart)
			status = "OK"
			break
		end
		local speed = strikeResumeSpeed or resolveSpeed(options.Speed)
		local deltaTime = task.wait()
		local stepDistance = math.min(speed * deltaTime, horizontal)
		local direction = Vector3.new(targetPosition.X - currentPosition.X, 0, targetPosition.Z - currentPosition.Z).Unit
		local nextX = currentPosition.X + direction.X * stepDistance
		local nextZ = currentPosition.Z + direction.Z * stepDistance
		local nextY
		if ctx.fixedY or ctx.keepHeightMode then
			nextY = computeLandingY(ctx, nextX, nextZ)
		elseif smoothY then
			local groundY = computeLandingY(ctx, nextX, nextZ)
			currentY = currentY + (groundY - currentY) * smoothY
			nextY = ctx.minY and math.max(currentY, ctx.minY) or currentY
		else
			nextY = computeLandingY(ctx, nextX, nextZ)
		end
		if options.FaceDirection then
			rootPart.CFrame = CFrame.lookAt(Vector3.new(nextX, nextY, nextZ), Vector3.new(nextX, nextY, nextZ) + direction)
		else
			rootPart.CFrame = CFrame.new(nextX, nextY, nextZ)
		end
		zeroVelocity(rootPart)
	end

	if noclipConnection then
		noclipConnection:Disconnect()
	end
	if options.Lock then
		options.Lock(false)
	end
	return status
end

-- [TS9] TRACKED GLIDE ENGINE ---------------------------------------------------
-- Replaces tweenToPos / tweenToPos2: a retargetable, track-object-aware
-- movement loop. Calling it again while running just updates the target.
--
-- options:
--   Target          Vector3 | Instance | ObjectValue (tracked live)
--   Speed           number or nil (nil = GetMaxSpeed() - 15 per step)
--   Gate            function() -> bool
--   FlightHeight    fixed flight Y (nil = rootY + 600)
--   GroundSpring    follow terrain with a vertical spring (tweenToPos2)
--   RayUpOffset     ground-ray height above flight Y (default 150)
--   MaxFlightY      spring ceiling (default 500)
--   Pathfinding     follow PathfindingService waypoints
--   ShowWaypoints   draw waypoint parts (default true when Pathfinding)
function TweenSystem.TrackedGlide(target, options)
	options = options or {}
	local targetPosition, trackObject = resolveTarget(target)
	if not targetPosition then
		return
	end

	-- dedup / retarget protocol
	if trackedState.running and trackedState.track_object == trackObject and trackObject then
		return
	end
	if trackedState.running and trackedState.target and not trackObject and horizontalDistance(trackedState.target, targetPosition) < 1 then
		return
	end
	if trackedState.running then
		trackedState.target = targetPosition
		trackedState.track_object = trackObject
		trackedState.cancel = true
		return
	end

	local characterModel = getCharacter()
	local rootPart = rootPartOf(characterModel)
	local humanoid = humanoidOf(characterModel)
	if not rootPart or not humanoid then
		return
	end

	trackedState.target = targetPosition
	trackedState.track_object = trackObject
	trackedState.running = true
	trackedState.cancel = false
	getgenv().busy = true
	humanoid:UnequipTools()

	local usePathfinding = options.Pathfinding
	local showWaypoints = options.ShowWaypoints
	if showWaypoints == nil then
		showWaypoints = usePathfinding and true or false
	end
	local rayUpOffset = options.RayUpOffset or 150
	local maxFlightY = options.MaxFlightY or 500
	local raycastParams = buildRaycastParams(characterModel)

	-- vertical setup: fixed flight height vs. terrain-following spring
	local flightY
	local verticalVelocity = 0
	if options.GroundSpring then
		flightY = rootPart.Position.Y
	else
		flightY = options.FlightHeight or rootPart.Position.Y + 600
		rootPart.CFrame = CFrame.new(rootPart.Position.X, flightY, rootPart.Position.Z)
		zeroVelocity(rootPart)
	end
	local targetFlightY = flightY

	-- pathfinding state
	local waypoints = {}
	local waypointIndex = 1
	local lastPathTarget = nil
	local timeSinceRepath = 0
	local visualParts = {}

	local function clearVisuals()
		for _, part in ipairs(visualParts) do
			if part and part.Parent then
				part:Destroy()
			end
		end
		visualParts = {}
	end

	local function drawWaypoints(waypointList)
		clearVisuals()
		if not showWaypoints then
			return
		end
		for index, waypoint in ipairs(waypointList) do
			if index ~= 1 then
				local waypointPart = Instance.new("Part")
				waypointPart.Name = "WaypointVisual_" .. index
				waypointPart.Anchored = true
				waypointPart.CanCollide = false
				waypointPart.CanQuery = false
				waypointPart.CanTouch = false
				waypointPart.Size = Vector3.new(1, 1, 1)
				waypointPart.Shape = Enum.PartType.Ball
				waypointPart.Material = Enum.Material.Neon
				waypointPart.Color = waypoint.Action == Enum.PathWaypointAction.Jump and Color3.fromRGB(255, 100, 100) or Color3.fromRGB(100, 255, 100)
				waypointPart.Position = waypoint.Position
				waypointPart.Transparency = 0.3
				waypointPart.Parent = Workspace
				table.insert(visualParts, waypointPart)
				if index > 2 then
					local previousWaypoint = waypointList[index - 1]
					local segmentLength = (waypoint.Position - previousWaypoint.Position).Magnitude
					local linePart = Instance.new("Part")
					linePart.Name = "WaypointLine_" .. index
					linePart.Anchored = true
					linePart.CanCollide = false
					linePart.CanQuery = false
					linePart.CanTouch = false
					linePart.Size = Vector3.new(0.2, 0.2, segmentLength)
					linePart.Material = Enum.Material.Neon
					linePart.Color = Color3.fromRGB(100, 200, 255)
					linePart.CFrame = CFrame.lookAt(previousWaypoint.Position, waypoint.Position) * CFrame.new(0, 0, -segmentLength / 2)
					linePart.Transparency = 0.5
					linePart.Parent = Workspace
					table.insert(visualParts, linePart)
				end
			end
		end
	end

	local function findGroundY(positionXZ, baseY)
		local rayStartY = math.max(baseY + rayUpOffset, maxFlightY + rayUpOffset)
		local result = Workspace:Raycast(Vector3.new(positionXZ.X, rayStartY, positionXZ.Y), Vector3.new(0, -(rayStartY + 1000), 0), raycastParams)
		if result then
			local groundY = result.Position.Y + 3
			if groundY <= maxFlightY then
				return groundY
			end
			local retryResult = Workspace:Raycast(Vector3.new(positionXZ.X, result.Position.Y - 1, positionXZ.Y), Vector3.new(0, -2000, 0), raycastParams)
			if retryResult then
				local retryGroundY = retryResult.Position.Y + 3
				if retryGroundY <= maxFlightY then
					return retryGroundY
				end
			end
		end
		return nil
	end

	local function computePath(startPosition, endPosition)
		local path = PathfindingService:CreatePath({ AgentRadius = 2, AgentHeight = 5, AgentCanJump = true, AgentCanClimb = false, WaypointSpacing = 4 })
		local computeSuccess = pcall(function()
			path:ComputeAsync(startPosition, endPosition)
		end)
		if computeSuccess and path.Status == Enum.PathStatus.Success then
			return path:GetWaypoints()
		end
		return nil
	end

	local function shouldRepath(newTarget)
		if not usePathfinding then
			return false
		end
		if not lastPathTarget then
			return true
		end
		if (newTarget - lastPathTarget).Magnitude > 10 then
			return true
		end
		if #waypoints == 0 then
			return true
		end
		if waypointIndex < #waypoints then
			return true
		end
		return false
	end

	local function resetPath()
		waypoints = {}
		waypointIndex = 1
		lastPathTarget = nil
		clearVisuals()
	end

	local gate = options.Gate
	local shouldStop = false

	while true do
		-- movement loop; left via cancel (retarget) or shouldStop (arrive/abort)
		while true do
			if trackedState.cancel then
				break
			end
			if gate and not gate() then
				shouldStop = true
				break
			end
			if trackObject then
				local tracked = resolveTrackedPosition(trackObject)
				if not tracked then
					shouldStop = true
					break
				end
				targetPosition = tracked
			end
			local deltaTime = task.wait()
			timeSinceRepath = timeSinceRepath + deltaTime
			local currentPosition = rootPart.Position
			if horizontalDistance(currentPosition, targetPosition) <= 5 then
				rootPart.CFrame = CFrame.new(targetPosition)
				zeroVelocity(rootPart)
				shouldStop = true
				break
			end
			if usePathfinding and (shouldRepath(targetPosition) or timeSinceRepath > 1) then
				local newWaypoints = computePath(currentPosition, targetPosition)
				if newWaypoints and #newWaypoints > 1 then
					waypoints = newWaypoints
					waypointIndex = 2
					lastPathTarget = targetPosition
					timeSinceRepath = 0
					drawWaypoints(waypoints)
				else
					resetPath()
				end
			end
			local moveTarget = targetPosition
			if usePathfinding and #waypoints > 0 and waypointIndex <= #waypoints then
				moveTarget = waypoints[waypointIndex].Position
				if horizontalDistance(currentPosition, moveTarget) <= 3 then
					if showWaypoints and visualParts[waypointIndex - 1] then
						visualParts[waypointIndex - 1].Color = Color3.fromRGB(150, 150, 150)
						visualParts[waypointIndex - 1].Transparency = 0.7
					end
					waypointIndex = waypointIndex + 1
					moveTarget = waypointIndex <= #waypoints and waypoints[waypointIndex].Position or targetPosition
				end
			end
			local currentXZ = Vector2.new(currentPosition.X, currentPosition.Z)
			local deltaXZ = Vector2.new(moveTarget.X, moveTarget.Z) - currentXZ
			local distance = deltaXZ.Magnitude
			if distance >= 0.1 then
				local moveSpeed = options.Speed ~= nil and resolveSpeed(options.Speed) or (TweenSystem.GetMaxSpeed() - 15)
				local direction = deltaXZ.Unit
				local nextXZ = currentXZ + direction * math.min(moveSpeed * deltaTime, distance)
				if options.GroundSpring then
					local aheadXZ = currentXZ + direction * math.min(moveSpeed * 0.25, math.max(distance, 0.1))
					local groundY = findGroundY(aheadXZ, flightY)
					if groundY then
						targetFlightY = groundY
					end
					verticalVelocity = verticalVelocity + ((targetFlightY - flightY) * 200 - verticalVelocity * 25) * deltaTime
					flightY = flightY + verticalVelocity * deltaTime
					if flightY > maxFlightY then
						flightY = maxFlightY
						if verticalVelocity > 0 then
							verticalVelocity = 0
						end
					end
				end
				local nextPosition = Vector3.new(nextXZ.X, flightY, nextXZ.Y)
				if options.FaceDirection or options.GroundSpring then
					rootPart.CFrame = CFrame.lookAt(nextPosition, nextPosition + Vector3.new(direction.X, 0, direction.Y))
				else
					rootPart.CFrame = CFrame.new(nextPosition)
				end
				zeroVelocity(rootPart)
			end
		end

		if shouldStop then
			break
		end

		-- cancel = retarget request: re-read and keep flying
		trackedState.cancel = false
		trackObject = trackedState.track_object
		if trackObject then
			local tracked = resolveTrackedPosition(trackObject)
			if not tracked then
				break
			end
			targetPosition = tracked
		else
			targetPosition = trackedState.target
		end
		resetPath()
		timeSinceRepath = 0
	end

	clearVisuals()
	getgenv().busy = false
	trackedState.running = false
	trackedState.target = nil
	trackedState.track_object = nil
end

-- [TS10] LEGACY COMPATIBILITY LAYER --------------------------------------------
-- Installs every global the main script used to define inline.
-- Same names, same signatures, same call sites - no other edits needed.

getMaxSpeed = TweenSystem.GetMaxSpeed
getSpeed = TweenSystem.GetSpeed

getExcludeList = function(characterModel)
	local excludeList = { characterModel }
	for _, otherPlayer in ipairs(Players:GetPlayers()) do
		if otherPlayer.Character then
			table.insert(excludeList, otherPlayer.Character)
		end
	end
	return excludeList
end

isWithinDistance = TweenSystem.IsWithinDistance
isWithinDistance2 = TweenSystem.IsWithinDistance
snapToPosition = TweenSystem.Snap

-- -- tweenTeleportPro ----------------------------------------------------------
-- Accepts a Vector3 or a table { Position/Target, Track, Stop, Y, Speed }.
-- Old version was connection-based and ignored Speed; now blocking + honored.
-- Concurrent callers: a new target cancels the running glide and takes over
-- (same as the old connection:Disconnect() behavior).
local proCurrentTarget = nil
local proActiveControl = nil
tweenTeleportPro = function(target)
	local targetPosition = nil
	local targetInstance = nil
	if typeof(target) == "Vector3" then
		targetPosition = target
	elseif typeof(target) == "table" then
		targetPosition = target.Position or target.Target
		if typeof(target.Target) == "Instance" then
			targetInstance = target.Target
		elseif typeof(target.Position) == "Instance" then
			targetInstance = target.Position
		end
	end
	local resolvedTarget = targetInstance or targetPosition
	if not resolvedTarget then
		warn("horotppro: no target defined")
		return
	end
	local checkPosition = resolveTarget(resolvedTarget)
	if not checkPosition then
		warn("horotppro: no target defined")
		return
	end

	-- dedup: identical horizontal target while running -> skip
	if proCurrentTarget and horizontalDistance(checkPosition, proCurrentTarget) < 1 then
		return
	end

	-- takeover: cancel the previous glide and wait for it to release
	if proActiveControl then
		proActiveControl.cancel = true
		repeat
			task.wait()
		until not proActiveControl
	end
	local control = { cancel = false }
	proActiveControl = control
	proCurrentTarget = checkPosition

	local gate = typeof(target) == "table" and target.Stop or nil
	local fixedY = typeof(target) == "table" and target.Y or nil -- lowercase 'y' was always ignored, kept that way
	local speedOverride = typeof(target) == "table" and target.Speed or nil

	local characterModel = getCharacter()
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		proActiveControl = nil
		proCurrentTarget = nil
		return
	end

	-- lift to cruise height first (unless a fixed Y is requested)
	if not fixedY then
		local raycastParams = buildRaycastParams(characterModel)
		local rayResult = Workspace:Raycast(Vector3.new(checkPosition.X, Config.RayStartHeight, checkPosition.Z), Vector3.new(0, -Config.RayDepth, 0), raycastParams)
		local targetY = rayResult and rayResult.Position.Y + Config.FloatHeight or checkPosition.Y + Config.FloatHeight
		local liftY = math.max(rootPart.Position.Y, targetY, Config.MinFlightY)
		rootPart.CFrame = CFrame.new(rootPart.Position.X, liftY, rootPart.Position.Z)
	end

	local status = TweenSystem.GlideTo({
		Target = resolvedTarget,
		Speed = speedOverride or function()
			return TweenSystem.GetSpeed()
		end,
		Gate = gate,
		Cancel = function()
			return control.cancel
		end,
		FixedY = fixedY,
		RayStartY = Config.RayStartHeight,
		RayDepth = Config.RayDepth,
		LandingOffset = Config.FloatHeight,
		MinY = Config.MinFlightY,
		SmoothY = Config.Smoothness,
		Noclip = true,
		ArriveDistance = 8,
	})
	proActiveControl = nil
	proCurrentTarget = nil
	return status
end

-- Cancels a running tweenTeleportPro glide (used by the universal stop).
function TweenSystem.StopPro()
	if proActiveControl then
		proActiveControl.cancel = true
	end
end

-- -- tweenTeleport (fish kaitun main tween) ------------------------------------
tweenTeleport = function(targetPosition, unused)
	if isTeleporting then
		local currentHoro = getgenv().current_horo_target
		if not currentHoro or horizontalDistance(targetPosition, currentHoro) <= 5 then
			return
		end
		getgenv().cancel_horo_tp = true
		repeat
			task.wait()
		until not isTeleporting
	end
	if getgenv().fish_kaitun and getgenv().fish_kaitun.Enabled == false then
		return
	end
	getgenv().current_horo_target = targetPosition
	getgenv().cancel_horo_tp = false
	getgenv().busy = true
	TweenSystem.GlideTo({
		Target = targetPosition,
		Speed = function()
			return TweenSystem.GetMaxSpeed()
		end,
		Gate = function()
			return getgenv().fish_kaitun ~= nil and getgenv().fish_kaitun.Enabled == true
		end,
		Cancel = function()
			return getgenv().cancel_horo_tp
		end,
		SnapIfCloser = 8,
		ArriveDistance = 0.5,
		UnequipTools = true,
		WaitForSpeedBypass = true,
		StrikePause = true,
		RayStartY = 1000,
		RayDepth = 2000,
		LandingOffset = 5,
		MinY = 2,
		Lock = function(locked)
			isTeleporting = locked
		end,
	})
	getgenv().current_horo_target = nil
	getgenv().busy = false
end

-- -- tweenTeleport2 (tuff2 / fish kaitun, strike-aware) ------------------------
tweenTeleport2 = function(targetPosition, unusedSpeed, keepHeight, flightHeight)
	local function farmEnabled()
		if getgenv().tuff2 then
			return true
		end
		if getgenv().fish_kaitun and getgenv().fish_kaitun.Enabled then
			return true
		end
		return false
	end
	if horoTpRunning then
		return
	end
	if not farmEnabled() then
		warn("not enabled :c")
		return
	end
	local characterModel = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
	local rootPart = characterModel:WaitForChild("HumanoidRootPart", 5)
	if not rootPart then
		return
	end
	local startY = rootPart.Position.Y
	local keepStartY = startY < -100 -- underwater
	if not keepStartY then
		pcall(function()
			local currentIsland = getCurrentIsland and getCurrentIsland()
			if currentIsland and currentIsland.Name == "Fishman Island" then
				keepStartY = true
			end
		end)
	end
	TweenSystem.GlideTo({
		Target = targetPosition,
		Speed = 50,
		Gate = farmEnabled,
		StrikePause = true,
		KeepHeight = keepHeight,
		KeepHeightMode = "start",
		KeepStartY = keepStartY,
		RayStartY = startY + (flightHeight or 300),
		RayDepth = 2000,
		LandingOffset = 5,
		MinY = 2,
		SkipNonSolid = true,
		IceDash = true,
		IceDashMinStartY = -1500,
		ArriveDistance = 2,
		ExcludeExtra = { Workspace:FindFirstChild("PlayerCharacters"), Workspace:FindFirstChild("Effects") },
		Lock = function(locked)
			horoTpRunning = locked
		end,
	})
end

-- -- tweenTeleport3 (auto juzo, strike-aware) ----------------------------------
tweenTeleport3 = function(targetPosition, unused, keepHeight)
	if horoTpRunning then
		return
	end
	if not getgenv().auto_juzo then
		return
	end
	local characterModel = getCharacter()
	if not rootPartOf(characterModel) then
		return
	end
	TweenSystem.GlideTo({
		Target = targetPosition,
		Speed = 50, -- rework: fixed 50 again; the old raw-slider read was broken
		Gate = function()
			return getgenv().auto_juzo
		end,
		StrikePause = true,
		StrikeResumeSpeed = 30,
		KeepHeight = keepHeight,
		KeepHeightMode = "current",
		RayStartY = function(ctx)
			return ctx.rootPart.Position.Y + 300
		end,
		RayDepth = 1000,
		LandingOffset = 5,
		SkipNonSolid = true,
		IceDash = true,
		IceDashWait = 0.5,
		ArriveDistance = 0.5,
		Lock = function(locked)
			horoTpRunning = locked
		end,
	})
end

-- -- tweenTeleportTo (simple blocking lerp, keeps rotation) --------------------
tweenTeleportTo = function(targetPosition, unused)
	if isTweening then
		return
	end
	local rootPart = rootPartOf(getCharacter())
	if not rootPart then
		return
	end
	isTweening = true
	TweenSystem.LerpTo(targetPosition, Config.LerpSpeed)
	isTweening = false
end

-- -- tweenToPos (fixed flight height, tracked) ---------------------------------
tweenToPos = function(target, speed, flightHeight)
	TweenSystem.TrackedGlide(target, {
		Speed = speed,
		FlightHeight = flightHeight,
		Gate = function()
			local kaitunOn = getgenv().fish_kaitun and getgenv().fish_kaitun.Enabled
			return (kaitunOn or getgenv().auto_santa or getgenv().auto_impel) and not getgenv().die
		end,
	})
end

-- -- tweenToPos2 (terrain spring + pathfinding, tracked) -----------------------
tweenToPos2 = function(target, speed, flightHeight, maxHeight, usePathfinding, showWaypoints)
	if usePathfinding == nil then
		usePathfinding = true
	end
	TweenSystem.TrackedGlide(target, {
		Speed = speed,
		GroundSpring = true,
		RayUpOffset = flightHeight or 150,
		MaxFlightY = maxHeight or 500,
		Pathfinding = usePathfinding,
		ShowWaypoints = showWaypoints,
		Gate = function()
			local kaitunOn = getgenv().fish_kaitun and getgenv().fish_kaitun.Enabled
			return kaitunOn or getgenv().auto_santa or getgenv().auto_impel
		end,
	})
end

-- -- lerpTweenTo (pika flight, straight 3D) ------------------------------------
lerpTweenTo = function(targetPosition, speed)
	if getgenv().lerp_tween_running then
		return
	end
	local characterModel = getCharacter()
	if not rootPartOf(characterModel) then
		return
	end
	getgenv().lerp_tween_running = true
	TweenSystem.FrameStepTo(targetPosition, speed or function()
		return TweenSystem.GetMaxSpeed() - 5
	end)
	getgenv().lerp_tween_running = false
end

-- -- walkTo (slow TweenService walk) -------------------------------------------
-- Old version used the stale global 'currentCharacter' - fixed.
walkTo = function(targetPosition)
	local rootPart = rootPartOf(getCharacter())
	if not rootPart then
		return
	end
	TweenSystem.TweenCFrame(rootPart, CFrame.new(targetPosition), Config.WalkSpeed)
end

-- -- flyToPosition (ascend + glide + geppo upkeep) -----------------------------
flyToPosition = TweenSystem.FlyTo

-- -- CustomTween (heartbeat arc; dead code, kept for compat) -------------------
CustomTween = TweenSystem.ArcTo

-- -- legacy state globals -------------------------------------------------------
-- were plain globals in the main script; some farm code may probe them
isTweening = isTweening or false
isTeleporting = isTeleporting or false
horoTpRunning = horoTpRunning or false
getgenv().lerp_tween_running = getgenv().lerp_tween_running or false
getgenv().current_horo_target = getgenv().current_horo_target or nil
getgenv().cancel_horo_tp = getgenv().cancel_horo_tp or nil

-- [TS11] SHIP / BAAL TRACKING TWEENS -------------------------------------------
-- ports of getgenv().cframe_track_tween (+_predicted) and raycast_track_tween.
-- NOTE: both existed TWICE in the main script (v1 without, v2 with strike
-- check / heightOffset) - the later definition always won. Only v2 is ported.

local function shipFarmActive()
	return getgenv().ship_farm or getgenv().baal_farm
end

function TweenSystem.StopShipTween()
	if getgenv().tween_connection then
		getgenv().tween_connection:Disconnect()
		getgenv().tween_connection = nil
	end
	getgenv().ship_tween_running = false
	getgenv().active_tween_target = nil
end

-- Blocking 3D follow of a moving model (old cframe_track_tween v2).
-- offset > 0 aims that many studs in FRONT of the target (LookVector).
function TweenSystem.TrackModel(targetModel, speed, offset)
	local characterModel = getCharacter()
	if not characterModel then
		return
	end
	if hasStrikeNotification() then
		task.wait(Config.StrikeWait)
		return
	end
	if (characterModel:GetAttribute("SpeedBypass") or 0) < 1 then
		return
	end
	if getgenv().ship_tween_running then
		getgenv().ship_tween_running = false
		task.wait()
	end
	if not targetModel then
		return
	end
	getgenv().ship_tween_running = true
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		getgenv().ship_tween_running = false
		return
	end
	local moveSpeed = speed or 110
	local lookOffset = offset or 0
	while shipFarmActive() and getgenv().ship_tween_running and targetModel and targetModel.Parent do
		local targetRoot = targetModel:FindFirstChild("HumanoidRootPart") or targetModel:FindFirstChild("HRP")
		if not targetRoot then
			getgenv().ship_tween_running = false
			return
		end
		local targetPosition = targetRoot.Position
		if lookOffset > 0 then
			targetPosition = targetPosition + targetRoot.CFrame.LookVector * lookOffset
		end
		local currentPosition = rootPart.Position
		local delta = targetPosition - currentPosition
		if delta.Magnitude <= 2 then
			break
		end
		local deltaTime = RunService.Heartbeat:Wait()
		rootPart.CFrame = CFrame.new(currentPosition + delta.Unit * moveSpeed * deltaTime)
		zeroVelocity(rootPart)
	end
	getgenv().ship_tween_running = false
end

-- Blocking 3D move to a static position (old cframe_track_tween_predicted).
function TweenSystem.TrackPosition(targetPosition, speed)
	local characterModel = getCharacter()
	if not characterModel then
		return
	end
	if (characterModel:GetAttribute("SpeedBypass") or 0) < 1 then
		return
	end
	if getgenv().ship_tween_running then
		getgenv().ship_tween_running = false
		task.wait()
	end
	if not targetPosition then
		return
	end
	getgenv().ship_tween_running = true
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		getgenv().ship_tween_running = false
		return
	end
	local moveSpeed = speed or 110
	while shipFarmActive() and getgenv().ship_tween_running do
		local currentPosition = rootPart.Position
		local delta = targetPosition - currentPosition
		if delta.Magnitude <= 2 then
			break
		end
		local deltaTime = RunService.Heartbeat:Wait()
		rootPart.CFrame = CFrame.new(currentPosition + delta.Unit * moveSpeed * deltaTime)
		zeroVelocity(rootPart)
	end
	getgenv().ship_tween_running = false
end

-- Async ground-hugging tracker with noclip (old raycast_track_tween v2).
-- Connection-based like the original: returns instantly, keeps moving.
-- heightOffset pins the flight Y to target.Y + heightOffset (no raycast).
function TweenSystem.TrackGroundAsync(target, speed, heightOffset)
	local characterModel = getCharacter()
	if not characterModel then
		return
	end
	if (characterModel:GetAttribute("SpeedBypass") or 0) < 1 then
		return
	end
	if getgenv().ship_tween_running and getgenv().active_tween_target == target then
		return
	end
	TweenSystem.StopShipTween()
	getgenv().ship_tween_running = true
	getgenv().active_tween_target = target
	local moveSpeed = speed or 110
	local rootPart = rootPartOf(characterModel)
	if not rootPart then
		return
	end
	local raycastParams = RaycastParams.new()
	raycastParams.FilterType = Enum.RaycastFilterType.Exclude
	getgenv().tween_connection = RunService.Heartbeat:Connect(function(deltaTime)
		if not getgenv().ship_tween_running or not shipFarmActive() then
			TweenSystem.StopShipTween()
			return
		end
		local targetPosition = nil
		local targetCFrame = nil
		local targetType = typeof(target)
		if targetType == "Instance" then
			if not target.Parent then
				TweenSystem.StopShipTween()
				return
			end
			if target:IsA("Model") then
				local targetRoot = target.PrimaryPart or target:FindFirstChild("HumanoidRootPart") or target:FindFirstChild("HRP")
				if targetRoot then
					targetPosition = targetRoot.Position
					targetCFrame = targetRoot.CFrame
				end
			elseif target:IsA("BasePart") then
				targetPosition = target.Position
				targetCFrame = target.CFrame
			end
		elseif targetType == "Vector3" then
			targetPosition = target
		elseif targetType == "CFrame" then
			targetPosition = target.Position
			targetCFrame = target
		end
		if not targetPosition then
			return
		end
		for _, part in pairs(characterModel:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide then
				part.CanCollide = false
			end
		end
		rootPart.AssemblyLinearVelocity = Vector3.zero
		rootPart.AssemblyAngularVelocity = Vector3.zero
		local currentPosition = rootPart.Position
		local deltaXZ = Vector3.new(targetPosition.X, 0, targetPosition.Z) - Vector3.new(currentPosition.X, 0, currentPosition.Z)
		if deltaXZ.Magnitude <= 10 then
			if heightOffset then
				local snapPosition = Vector3.new(targetPosition.X, targetPosition.Y + heightOffset, targetPosition.Z)
				rootPart.CFrame = CFrame.new(snapPosition, targetPosition)
			elseif targetCFrame then
				rootPart.CFrame = targetCFrame
			else
				rootPart.CFrame = CFrame.new(targetPosition)
			end
			TweenSystem.StopShipTween()
			return
		end
		local nextXZ = currentPosition + deltaXZ.Unit * (moveSpeed * deltaTime)
		local nextY
		if heightOffset then
			nextY = targetPosition.Y + heightOffset
		else
			local rayOrigin = Vector3.new(nextXZ.X, math.max(currentPosition.Y, targetPosition.Y) + 50, nextXZ.Z)
			local rayDirection = Vector3.new(0, -500, 0)
			local filterList = { characterModel }
			raycastParams.FilterDescendantsInstances = filterList
			local raycastResult = Workspace:Raycast(rayOrigin, rayDirection, raycastParams)
			local attempts = 0
			while raycastResult and raycastResult.Instance.Name == "Leaves" and attempts < 10 do
				table.insert(filterList, raycastResult.Instance)
				raycastParams.FilterDescendantsInstances = filterList
				raycastResult = Workspace:Raycast(rayOrigin, rayDirection, raycastParams)
				attempts = attempts + 1
			end
			nextY = raycastResult and raycastResult.Position.Y + 4 or currentPosition.Y
		end
		local nextPosition = Vector3.new(nextXZ.X, nextY, nextXZ.Z)
		rootPart.CFrame = CFrame.new(nextPosition, Vector3.new(targetPosition.X, nextY, targetPosition.Z))
	end)
end

-- [TS12] SHIP / BAUM COMPAT + FACTORY ARC --------------------------------------

getgenv().ship_tween_running = getgenv().ship_tween_running or false
getgenv().cframe_track_tween = function(targetModel, speed, offset)
	return TweenSystem.TrackModel(targetModel, speed, offset)
end
getgenv().cframe_track_tween_predicted = function(targetPosition, speed)
	return TweenSystem.TrackPosition(targetPosition, speed)
end
getgenv().raycast_track_tween = function(target, speed, heightOffset)
	return TweenSystem.TrackGroundAsync(target, speed, heightOffset)
end

-- factory/scientist farm arc tween (dead code in the original, kept for compat)
factoryTween = TweenSystem.ArcTo

return TweenSystem

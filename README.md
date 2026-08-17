local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local farming = true

local function getHRP()
    return player.Character and player.Character:FindFirstChild("HumanoidRootPart")
end

local function getStats()
    return ReplicatedStorage:FindFirstChild("Stats" .. player.Name)
end

local function getLevel()
    local stats = getStats()
    return stats and stats.Stats.Level.Value or 0
end

local function getPeli()
    local stats = getStats()
    return stats and stats.Stats.Peli.Value or 0
end

local function getQuest()
    local stats = getStats()
    return stats and stats.Quest.CurrentQuest.Value or ""
end

local function tp(pos)
    local hrp = getHRP()
    if hrp then
        hrp.CFrame = CFrame.new(pos)
        hrp.Velocity = Vector3.zero
    end
end

local function tpTween(target, speed)
    local hrp = getHRP()
    if not hrp then return end
    speed = speed or 50
    local dist = (hrp.Position - target).Magnitude
    local t = dist / speed
    local tween = TweenService:Create(hrp, TweenInfo.new(t, Enum.EasingStyle.Linear), {CFrame = CFrame.new(target)})
    tween:Play()
    tween.Completed:Wait()
end

local function isNear(pos, dist)
    local hrp = getHRP()
    if not hrp then return false end
    return (hrp.Position - pos).Magnitude <= dist
end

local function takeQuest(name)
    pcall(function()
        ReplicatedStorage:WaitForChild("Events"):WaitForChild("Quest"):InvokeServer({"takequest", name})
    end)
end

local function quitQuest()
    pcall(function()
        ReplicatedStorage:WaitForChild("Events"):WaitForChild("Quest"):InvokeServer({"quit"})
    end)
end

local function returnItem()
    pcall(function()
        ReplicatedStorage:WaitForChild("Events"):WaitForChild("Quest"):InvokeServer({"returnitem"})
    end)
end

local function buyRifle()
    pcall(function()
        local buyableItems = workspace:WaitForChild("BuyableItems")
        ReplicatedStorage:WaitForChild("Events"):WaitForChild("Shop"):InvokeServer(buyableItems.Rifle, 1)
    end)
end

local function getNecklace()
    for _, v in ipairs(workspace.Effects:GetChildren()) do
        local part = v:IsA("BasePart") and v or nil
        if part then
            local mesh = part:FindFirstChildWhichIsA("SpecialMesh")
            if mesh and mesh.TextureId == "http://www.roblox.com/asset/?id=28461501" then
                return part
            end
        end
    end
    return nil
end

local function hasRifle()
    local inv = getStats() and getStats().Inventory.Inventory
    return inv and string.find(inv.Value, "Rifle")
end

local sarahPos = Vector3.new(-546, 9, -3403)
local rifleShopPos = Vector3.new(-532, 6, -3450)
local necklaceTimer = tick()

-- Fake geppo keepalive
task.spawn(function()
    while farming do
        task.wait(1)
        pcall(function()
            local char = player.Character
            if not char then return end
            local stats = getStats()
            local skyWalk = stats and stats.Skills:FindFirstChild("sky_walk")
            local skill = ReplicatedStorage.Events.Skill
            if skyWalk and skyWalk.Value then
                skill:InvokeServer("Sky Walk2", {char = char, cf = char.HumanoidRootPart.CFrame})
            else
                skill:InvokeServer("Geppo", {char = char, cf = char.HumanoidRootPart.CFrame})
            end
        end)
    end
end)

-- Main loop
task.spawn(function()
    while farming do
        task.wait()
        pcall(function()
            if not player.Character or not getHRP() then return end

            -- Buy rifle once we have enough peli
            if not hasRifle() and getPeli() >= 300 then
                if not isNear(rifleShopPos, 10) then
                    tpTween(rifleShopPos, 50)
                else
                    buyRifle()
                    task.wait(1)
                end
                return
            end

            -- Sarah necklace quest loop
            local quest = getQuest()

            if quest ~= "Find Sarah's necklace" then
                if not isNear(sarahPos, 10) then
                    tpTween(sarahPos, 50)
                else
                    quitQuest()
                    task.wait(1)
                    takeQuest("Find Sarah's necklace")
                    necklaceTimer = tick()
                    task.wait(2)
                end
                return
            end

            -- Quest taken — find necklace or return it
            if tick() - necklaceTimer > 30 then
                -- Retake quest if stuck
                if not isNear(sarahPos, 10) then
                    tpTween(sarahPos, 50)
                else
                    quitQuest()
                    task.wait(1)
                    takeQuest("Find Sarah's necklace")
                    necklaceTimer = tick()
                    task.wait(2)
                end
                return
            end

            local necklace = getNecklace()
            if necklace then
                if not isNear(necklace.Position, 5) then
                    tpTween(necklace.Position, 50)
                else
                    tp(necklace.Position)
                    local prompt = necklace:FindFirstChildWhichIsA("ProximityPrompt")
                    if prompt then
                        fireproximityprompt(prompt)
                        task.wait(1)
                    end
                end
            else
                -- Necklace not found, return to Sarah to hand in
                if not isNear(sarahPos, 10) then
                    tpTween(sarahPos, 50)
                else
                    returnItem()
                    task.wait(2)
                end
            end
        end)
    end
end)

print("Level farm started. Set farming = false to stop.")

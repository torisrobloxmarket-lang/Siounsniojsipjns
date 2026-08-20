local __lic="bf731cc2eb04c797";
-- JujutsuShenanigans.lua — Premium combat suite for Jujutsu Shenanigans
-- PlaceId: 9391468976  |  v2.1 (data-driven combos)
--
-- VERIFIED against the live game:
--  * Combat state = CHARACTER ATTRIBUTES. Canonical state names (ListData.States):
--      Stun, IFrame, NoM1, NoSprint, NoDash, DisableChase, Block, DirectionLock,
--      SpeedMultiplier, JumpMultiplier, Scale, InSkill   (+ Ragdoll, Burst, Evade, Dead)
--  * Character = `Moveset` attribute on the model (e.g. "Gojo").
--  * REAL move data: each char's Moveset FOLDER holds NumberValues:
--      .Name  = move name        .Value = cooldown (s)
--      attr Key      = skill slot key ("1".."4")
--      attr LastUse  = last-use timestamp        attr Service = handling service
--    Move names/ult/theme per character come from ReplicatedStorage.Modules.ListData.MoveList.
--  * Keybinds (InputContext "Combat"): Skill 1-8 = 1..8 | Melee = M1 | Dash = Q
--      Block = F | Awaken = G | Special = R | Sprint = LeftShift
--  * VirtualInputManager confirmed to drive the game's InputContext system.
--  * DEFENSE is EVENT-DRIVEN (v5): the game has ONLY M1 punches + abilities (skills 1-8), no M2. Each
--    enemy Moveset move's LastUse attr stamps when that ability fires (exact move name, no defer), and the
--    char's LastM1 attr changes on a punch. Both feed a distance-scaled parry window (rangeLead s/stud, so
--    far hits block LATER not early). Unblockable abilities (grab/pull/ragdoll, by effect OR name) -> dash.

local S = function(f) local ok, res = pcall(f); if ok then return res end end
local Players      = game:GetService('Players')
local RS           = game:GetService('ReplicatedStorage')
local RunService   = game:GetService('RunService')
local UIS          = game:GetService('UserInputService')
local VIM          = game:GetService('VirtualInputManager')
local TweenService = game:GetService('TweenService')
local Lighting     = game:GetService('Lighting')
local lp           = Players.LocalPlayer

----------------------------------------------------------------------
-- OvertimeUI loader
----------------------------------------------------------------------
local UI
do
    local ok, lib = pcall(function()
        return loadstring(game:HttpGet("https://raw.githubusercontent.com/overtimepog/OvertimeUI/main/OvertimeUI.lua"))()
    end)
    if ok then UI = lib else warn("[JJS] OvertimeUI failed:", lib); return end
end

----------------------------------------------------------------------
-- REAL PER-CHARACTER DATA
----------------------------------------------------------------------
local function C3(r,g,b) return Color3.new(r,g,b) end
local CHAR_DATA = {
    Gojo      ={ult='HONORED ONE',     col=C3(.666,1,1),          k={'Lapse Blue','Reversal Red','Rapid Punches','Twofold Kick'}},
    Itadori   ={ult='VESSEL',          col=C3(1,.607,.607),       k={'Cursed Strikes','Crushing Blow','Divergent Fist','Manji Kick'}},
    Hakari    ={ult='RESTLESS GAMBLER',col=C3(.666,1,.498),       k={'Reserve Balls','Shutter Doors','Rough Energy','Fever Breaker'}},
    Megumi    ={ult='TEN SHADOWS',     col=C3(.470,.470,.470),    k={'Rabbit Escape','Nue','Toad','Divine Dog: Totality'}},
    Mahoraga  ={ult='MAHORAGA',        col=C3(1,1,1),             k={'Divine Pummel','Ground Pitch','Earthquake','Takedown'}},
    Mahito    ={ult='PERFECTION',      col=C3(1,.666,1),          k={'Stockpile','Soul Fire','Focus Strike','Body Repel'}},
    Choso     ={ult='BLOOD MANIPULATOR',col=C3(.666,.294,.294),   k={'Piercing Blood','Flowing Red Scale','Supernova','Blood Edge'}},
    Todo      ={ult='SWITCHER',        col=C3(.666,1,1),          k={'Swift Kick','Brute Force','Pebble Throw','Elbow Drop'}},
    Locust    ={ult='LOCUST',          col=C3(.666,1,.498),       k={'Crushing Jaws','Clever','Wing Throw','Black Mucus'}},
    Hiromi    ={ult='DEFENSE ATTORNEY',col=C3(.450,.388,.301),    k={'Extended Swings','Justice Served',"Judgement's Reach",'Pressing Charges'}},
    Yuki      ={ult='STAR RAGE',       col=C3(.666,.666,1),       k={'Garuda Rebound','Rising Rage','Mass Breaker','Garuda Stab'}},
    Heian     ={ult='HEIAN',           col=C3(1,.333,0),          k={'Strong Dismantle','Open FURNACE','Cleave Rush','Kamutoke'}},
    Yuta      ={ult='CURSED PARTNERS', col=C3(1,.666,1),          k={'Severing Path','Resolute Slash','Veilstep','Second Wind'}},
    Charles   ={ult='ASPIRING MANGAKA',col=C3(1,.921,.921),       k={'Despair','Shut Up!','Eye Catching','Sacrilege'}},
    Mechamaru ={ult='PUPPET MASTER',   col=C3(.882,.039,.294),    k={'Ultra Spin','Boost On','Ultra Cannon','Heat Emission'}},
    Naoya     ={ult='HEAD OF THE HEI', col=C3(.501,.494,1),       k={'Projection Breaker','Bleedout','Decisive Strike','Cursory Impact'}},
    Nanami    ={ult='SALARYMAN',       col=C3(.368,.749,1),       k={'Cleaving Whirlwind','Severance Kick','Blunt Cut','Stabilize'}},
    Goku      ={ult='MONKEY KID',      col=C3(1,.082,.203),       k={'Kamehameha','Ki Spam','Staff Extend','Staff Uppercut'}},
    Haruta    ={ult='LUCKY COWARD',    col=C3(.654,.490,.796),    k={'Ambush','Backstab','Trip','Cheap Shot'}},
    MeiMei    ={ult='CROW CHARMER',    col=C3(.137,.156,.313),    k={'Impetus Updraft','Circling','Gliding Flight','Bird Control'}},
    Hanami    ={ult='DISASTER PLANTS', col=C3(.674,.796,.639),    k={'Root Swarm','Surging Thorns','Bud Shot','Defense Response'}},
    Ryu       ={ult='TRUE CANNON',     col=C3(.666,1,1),          k={'Granite Blast','Unsatisfied','Second Helping','Appetizer'}},
    Kurourushi={ult='BLACK DEATH',     col=C3(.396,.137,.172),    k={'Festering Strikes','Detach','Chokehold','Roach Swarm'}},
}
local SKILL_KEY = {['1']='One',['2']='Two',['3']='Three',['4']='Four',['5']='Five',['6']='Six',['7']='Seven',['8']='Eight'}

local COMBO_PRESETS  = {'ROUTE','SMART','1-2-3-4','4-3-2-1'}
local APPROACH_MODES = {'Off','Dash','Step'}
local TARGET_MODES   = {'Closest','Lowest HP'}

local COMBO_ROUTES = {
    Gojo    = {'M1','M1','M1','M1','3','4','M1','M1','M1','Q','2'},
    Itadori = {'1','M1','M1','M1','M1','2','M1','M1','M1','M1','1','M1','M1','M1'},
    Hakari  = {'M1','M1','M1','M1','2','M1','M1','M1','1','3','Q','M1','M1','M1','M1','M1'},
}

----------------------------------------------------------------------
-- STATE
----------------------------------------------------------------------
local VERSION = '5.0.3'
local BASE_ACCENT = Color3.fromRGB(150,99,255)
local ACCENT = BASE_ACCENT
local state = {
    autoCombo=false, autoBlock=true, autoDodge=true, autoEscape=true, antiStun=false, antiRagdoll=false, autoAwaken=false, aimTarget=true, glueTarget=false,
    perfectBlock=true, autoPunish=true, useSpecial=true,
    dodgeRange=34, dashCd=0.7, specialCd=6,
    parryM1=true,
    parryHold=0.45, parryLead=0.0, rangeLead=0.006,
    parryMargin=10, parryUnknown=45, dodgeLead=0.0,
    dashAssist=false, flankRange=30, flankCd=1.0,
    comboEscape=false, escapeReturn=0.45,
    autoBlackFlash=false, bfDelay=0.37,
    targetMode=1, aimKey=Enum.UserInputType.MouseButton2,
    comboKey=Enum.KeyCode.K,
    comboPreset=2, approachMode=3, engageDist=2, engageRange=200, comboRange=8, blockRange=14, auraRange=9, comboSpeed=0.30,
    lockBreak=50,
    esp=true, chams=false, fullbright=false, espRange=400,
    fly=false, flySpeed=70, noclip=false, infJump=false,
    walkSpeedOn=false, walkSpeed=16, jumpOn=false, jumpPower=50,
    targetLock=false, antiAFK=true, fov=70, rainbow=false, autoTheme=true,
}

----------------------------------------------------------------------
-- ORIGINAL STATE SNAPSHOT (restored on unload)
----------------------------------------------------------------------
local ORIG = {
    fov           = workspace.CurrentCamera.FieldOfView,
    brightness    = Lighting.Brightness,
    clockTime     = Lighting.ClockTime,
    fogEnd        = Lighting.FogEnd,
    globalShadows = Lighting.GlobalShadows,
    walkSpeed     = S(function() return lp.Character:FindFirstChildOfClass('Humanoid').WalkSpeed end) or 16,
    jumpPower     = S(function() return lp.Character:FindFirstChildOfClass('Humanoid').JumpPower end) or 50,
}

----------------------------------------------------------------------
-- BOOKKEEPING
----------------------------------------------------------------------
local conns = {}
local function track(c) conns[#conns+1]=c; return c end
local espObjs    = {}
local unloaded   = false
local blockUntil = 0
local blockFrom  = math.huge
local blockThreat
local blocking   = false
local genuineThreat   -- forward decl, filled after helpers
local validEnemy      -- forward decl
local learnedRange    = {}
local lastBlockedEnemy
local lastBlockedAt   = 0
local lastPunish      = 0
local prevBlocking    = false

----------------------------------------------------------------------
-- REAL ACTIVATION
----------------------------------------------------------------------
local COMBAT = (RS:FindFirstChild('Keybind') and RS.Keybind:FindFirstChild('Combat')) or nil
local hasGC = typeof(getconnections)=='function'
local function fireAction(name)
    if not (COMBAT and hasGC) then return false end
    local a=COMBAT:FindFirstChild(name); if not a then return false end
    local ok,cs=pcall(getconnections, a.Pressed); if not ok then return false end
    local fired=false
    for _,c in ipairs(cs) do pcall(function() c:Fire() end); fired=true end
    return fired
end
local function releaseAction(name)
    if not (COMBAT and hasGC) then return end
    local a=COMBAT:FindFirstChild(name); if not a then return end
    local ok,cs=pcall(getconnections, a.Released); if not ok then return end
    for _,c in ipairs(cs) do pcall(function() c:Fire() end) end
end
local function serverNow() return workspace:GetServerTimeNow() end

local m1Held=false
local function holdM1(on)
    on = on and true or false
    if on==m1Held then return end
    m1Held=on
    local cam=workspace.CurrentCamera; local vp=cam and cam.ViewportSize
    local x = vp and math.floor(vp.X/2) or 960
    local y = vp and math.floor(vp.Y/2) or 540
    S(function() VIM:SendMouseButtonEvent(x, y, 0, on, game, 0) end)
end
local function clickM1()
    if m1Held then holdM1(false) end
    local cam=workspace.CurrentCamera; local vp=cam and cam.ViewportSize
    local x = vp and math.floor(vp.X/2) or 960
    local y = vp and math.floor(vp.Y/2) or 540
    S(function() VIM:SendMouseButtonEvent(x, y, 0, true,  game, 0) end)
    task.wait(0.03)
    S(function() VIM:SendMouseButtonEvent(x, y, 0, false, game, 0) end)
end

local scriptLocked=false; local lastLockFire=0
local comboLockUntil=0
local function ensureLock(on)
    local cur = lp:GetAttribute('LockOn')==true
    if on then
        if not cur and tick()-lastLockFire>0.25 then fireAction('Lock On'); lastLockFire=tick() end
        scriptLocked=true
    elseif scriptLocked then
        if cur then
            if tick()-lastLockFire>0.25 then fireAction('Lock On'); lastLockFire=tick() end
        else scriptLocked=false end
    end
end

local KnitMC = S(function() return require(RS.Knit.Knit).GetController('MovementController') end)
local function gameDash()
    if KnitMC and KnitMC.DashRequest then return pcall(function() KnitMC:DashRequest() end) end
    return fireAction('Dash')
end

----------------------------------------------------------------------
-- CHARACTER HELPERS
----------------------------------------------------------------------
local function char()    return lp.Character end
local function hum()     local c=char(); return c and c:FindFirstChildOfClass('Humanoid') end
local function hrp()     local c=char(); return c and c:FindFirstChild('HumanoidRootPart') end
local function myName()  local c=char(); return c and tostring(c:GetAttribute('Moveset') or 'Unknown') end
local function active(v) return v==true or (type(v)=='number' and v>0) end
local function attr(o,n) return o and o:GetAttribute(n) end

local function stateOn(c, name)
    if not c then return false end
    if active(c:GetAttribute(name)) then return true end
    local info=c:FindFirstChild('Info')
    if info then
        local v=info:FindFirstChild(name)
        if v then if v:IsA('ValueBase') then return v.Value and v.Value~=false and true or false else return true end end
    end
    return false
end

local function liveMoveset(c)
    c = c or char()
    local ms = c and c:FindFirstChild('Moveset')
    local out = {}
    if ms then
        local now=serverNow()
        for _,mv in ipairs(ms:GetChildren()) do
            local key = mv:GetAttribute('Key')
            if key and SKILL_KEY[tostring(key)] then
                local cd=(mv:IsA('ValueBase') and mv.Value) or 0
                local lu=tonumber(mv:GetAttribute('LastUse')) or 0
                out[#out+1] = {key=tostring(key), name=mv.Name, cd=cd, lastUse=lu, inst=mv,
                               tip=tostring(mv:GetAttribute('Tip') or ''):upper(),
                               left=math.max(0, cd-(now-lu)), ready=((now-lu) >= cd)}
            end
        end
        table.sort(out, function(a,b) return a.key<b.key end)
    end
    return out
end

----------------------------------------------------------------------
-- INPUT (movement-only VIM helpers)
----------------------------------------------------------------------
local heldKeys = {}
local function setHold(name, down)
    if heldKeys[name]==down then return end
    heldKeys[name]=down
    local kc=Enum.KeyCode[name]; if kc then pcall(function() VIM:SendKeyEvent(down, kc, false, game) end) end
end

----------------------------------------------------------------------
-- OvertimeUI WINDOW
----------------------------------------------------------------------
local Window = UI:CreateWindow({
    Name      = "Jujutsu Shenanigans",
    Preset    = "Sleek",
    Accent    = Color3.fromRGB(150, 99, 255),
    AccentGlow= true,
    Layout    = "left",
    Roundness = 1.2,
    SaveFile  = "JujutsuShenanigans.cfg",
    AutoSave  = true,
})
if not Window then return end   -- re-run unloads old instance

Window:SetToggleKey("RightShift")
Window:SetDiscord("https://discord.com/invite/KrrT48SQ4g")

-- Dynamic label handles (updated in heartbeat)
local subLabel     -- character name + ult line
local skillLabels  = {}   -- [1..4] with :SetText()

----------------------------------------------------------------------
-- COMBAT TAB
----------------------------------------------------------------------
local combatTab   = Window:CreateTab("Combat")
local autoSection = combatTab:CreateSection("Automation")

local comboToggleHandle
comboToggleHandle = autoSection:CreateToggle({
    Name         = "Auto Combo (helper)",
    CurrentValue = state.autoCombo,
    SaveId       = "jjs_auto_combo",
    Callback     = function(v) state.autoCombo = v end,
})
comboToggleHandle:AddKeybind({
    CurrentKeybind = "K",
    Callback = function()
        state.autoCombo = not state.autoCombo
        comboToggleHandle:SetValue(state.autoCombo)
    end,
})

autoSection:CreateToggle({ Name="Auto Escape (anti-combo)",          CurrentValue=state.autoEscape,    SaveId="jjs_auto_escape",    Callback=function(v) state.autoEscape=v    end })
autoSection:CreateToggle({ Name="Combo Escape (dash out + punish)",  CurrentValue=state.comboEscape,   SaveId="jjs_combo_escape",   Callback=function(v) state.comboEscape=v   end })
autoSection:CreateToggle({ Name="Anti Stun",                         CurrentValue=state.antiStun,      SaveId="jjs_anti_stun",      Callback=function(v) state.antiStun=v      end })
autoSection:CreateToggle({ Name="Anti Ragdoll",                      CurrentValue=state.antiRagdoll,   SaveId="jjs_anti_ragdoll",   Callback=function(v) state.antiRagdoll=v   end })
autoSection:CreateToggle({ Name="Auto Block",                        CurrentValue=state.autoBlock,     SaveId="jjs_auto_block",     Callback=function(v) state.autoBlock=v     end })
autoSection:CreateToggle({ Name="Auto Counter (parry timing)",       CurrentValue=state.perfectBlock,  SaveId="jjs_auto_counter",   Callback=function(v) state.perfectBlock=v  end })
autoSection:CreateToggle({ Name="Auto Punish (combo after block)",   CurrentValue=state.autoPunish,    SaveId="jjs_auto_punish",    Callback=function(v) state.autoPunish=v    end })
autoSection:CreateToggle({ Name="Auto Dodge (dash unblockables)",    CurrentValue=state.autoDodge,     SaveId="jjs_auto_dodge",     Callback=function(v) state.autoDodge=v     end })
autoSection:CreateToggle({ Name="Block Enemy M1s",                   CurrentValue=state.parryM1,       SaveId="jjs_block_m1",       Callback=function(v) state.parryM1=v       end })
autoSection:CreateToggle({ Name="Dash Assist (flank behind target)", CurrentValue=state.dashAssist,    SaveId="jjs_dash_assist",    Callback=function(v) state.dashAssist=v    end })
autoSection:CreateToggle({ Name="Aim At Target (hold key)",          CurrentValue=state.aimTarget,     SaveId="jjs_aim_target",     Callback=function(v) state.aimTarget=v     end })
autoSection:CreateKeybind({ Name="Aim Key (hold)", CurrentKeybind="MouseButton2", Callback=function() end })
autoSection:CreateToggle({ Name="Auto Awaken",                       CurrentValue=state.autoAwaken,    SaveId="jjs_auto_awaken",    Callback=function(v) state.autoAwaken=v    end })
autoSection:CreateToggle({ Name="Use Special (R)",                   CurrentValue=state.useSpecial,    SaveId="jjs_use_special",    Callback=function(v) state.useSpecial=v    end })
autoSection:CreateToggle({ Name="Auto Black Flash (Itadori M1)",     CurrentValue=state.autoBlackFlash,SaveId="jjs_auto_black_flash",Callback=function(v) state.autoBlackFlash=v end })

local targetBtn   = autoSection:CreateButton({ Name="Target: Closest", Callback=function() state.targetMode  = state.targetMode  % #TARGET_MODES  + 1 end })
local presetBtn   = autoSection:CreateButton({ Name="Combo: ROUTE",    Callback=function() state.comboPreset  = state.comboPreset  % #COMBO_PRESETS  + 1 end })
local approachBtn = autoSection:CreateButton({ Name="Approach: Step",  Callback=function() state.approachMode = state.approachMode % #APPROACH_MODES + 1 end })

local function refreshPresetLabel()
    local p=COMBO_PRESETS[state.comboPreset]
    if p=='ROUTE' then p='ROUTE '..(COMBO_ROUTES[myName()] and 'OK' or '(no data->SMART)') end
    if presetBtn   and presetBtn.SetText   then presetBtn:SetText('Combo: '..p) end
    if approachBtn and approachBtn.SetText then approachBtn:SetText('Approach: '..APPROACH_MODES[state.approachMode]) end
    if targetBtn   and targetBtn.SetText   then targetBtn:SetText('Target: '..TARGET_MODES[state.targetMode]) end
end

-- Skills HUD
local skillsSection = combatTab:CreateSection("Skills HUD")
subLabel = skillsSection:CreateLabel({ Text="Loading character...", Color=Color3.fromRGB(180,180,200) })
for i=1,4 do
    skillLabels[i] = skillsSection:CreateLabel({ Text=tostring(i).."  --", Color=Color3.fromRGB(200,200,210) })
end

-- Glue Target
local glueSection = combatTab:CreateSection("Glue Target")
glueSection:CreateToggle({ Name="Glue Target (follow until dead)", CurrentValue=state.glueTarget, SaveId="jjs_glue_target", Callback=function(v) state.glueTarget=v end })
glueSection:CreateSlider({ Name="Engage Range",  Range={20,1500}, Increment=10,   CurrentValue=state.engageRange, Suffix="s", SaveId="jjs_engage_range", Callback=function(v) state.engageRange=v end })
glueSection:CreateSlider({ Name="Dash Cooldown", Range={0.3,2},   Increment=0.05, CurrentValue=state.dashCd,      Suffix="s", SaveId="jjs_dash_cd", Callback=function(v) state.dashCd=v      end })

-- Tuning
local tuningSection = combatTab:CreateSection("Tuning")
tuningSection:CreateSlider({ Name="Engage Dist",      Range={3,20},     Increment=0.5,  CurrentValue=state.engageDist,    Suffix="s", SaveId="jjs_engage_dist",    Callback=function(v) state.engageDist=v    end })
tuningSection:CreateSlider({ Name="Combo Speed",      Range={0.15,0.6}, Increment=0.01, CurrentValue=state.comboSpeed,    Suffix="s", SaveId="jjs_combo_speed",    Callback=function(v) state.comboSpeed=v   end })
tuningSection:CreateSlider({ Name="Black Flash Delay",Range={0.25,0.50},Increment=0.01, CurrentValue=state.bfDelay,       Suffix="s", SaveId="jjs_bf_delay",       Callback=function(v) state.bfDelay=v      end })
tuningSection:CreateSlider({ Name="Dodge/Block Range",Range={10,40},    Increment=1,    CurrentValue=state.dodgeRange,    Suffix="s", SaveId="jjs_dodge_range",    Callback=function(v) state.dodgeRange=v   end })
tuningSection:CreateSlider({ Name="Parry Hold",       Range={0.15,0.8}, Increment=0.01, CurrentValue=state.parryHold,     Suffix="s", SaveId="jjs_parry_hold",     Callback=function(v) state.parryHold=v    end })
tuningSection:CreateSlider({ Name="Flank Range",      Range={12,60},    Increment=1,    CurrentValue=state.flankRange,    Suffix="s", SaveId="jjs_flank_range",    Callback=function(v) state.flankRange=v   end })
tuningSection:CreateSlider({ Name="Flank Cooldown",   Range={0.4,3},    Increment=0.1,  CurrentValue=state.flankCd,       Suffix="s", SaveId="jjs_flank_cd",       Callback=function(v) state.flankCd=v      end })

----------------------------------------------------------------------
-- VISUALS TAB
----------------------------------------------------------------------
local visualTab = Window:CreateTab("Visuals")
local worldLeft = visualTab:CreateLeftGroupbox("World")
local colRight  = visualTab:CreateRightGroupbox("Colors")

worldLeft:CreateToggle({ Name="Name / HP ESP",     CurrentValue=state.esp,        SaveId="jjs_esp",        Callback=function(v) state.esp=v        end })
worldLeft:CreateToggle({ Name="Chams (Highlight)", CurrentValue=state.chams,      SaveId="jjs_chams",      Callback=function(v) state.chams=v      end })
worldLeft:CreateToggle({ Name="Fullbright",        CurrentValue=state.fullbright, SaveId="jjs_fullbright", Callback=function(v) state.fullbright=v end })
worldLeft:CreateSlider({ Name="ESP Range", Range={50,1000}, Increment=10, CurrentValue=state.espRange, Suffix="s", SaveId="jjs_esp_range", Callback=function(v) state.espRange=v end })

colRight:CreateToggle({ Name="Auto Theme (char color)", CurrentValue=state.autoTheme, SaveId="jjs_auto_theme", Callback=function(v) state.autoTheme=v end })
colRight:CreateToggle({ Name="Rainbow Accent",          CurrentValue=state.rainbow,   SaveId="jjs_rainbow",   Callback=function(v) state.rainbow=v   end })

----------------------------------------------------------------------
-- MOVE TAB
----------------------------------------------------------------------
local moveTab     = Window:CreateTab("Move")
local moveSection = moveTab:CreateSection("Teleport")

moveSection:CreateToggle({ Name="Fly (hold WASD)", CurrentValue=state.fly,         SaveId="jjs_fly",         Callback=function(v) state.fly=v         end })
moveSection:CreateSlider({ Name="Fly Speed", Range={20,250}, Increment=5, CurrentValue=state.flySpeed, SaveId="jjs_fly_speed", Callback=function(v) state.flySpeed=v end })
moveSection:CreateToggle({ Name="Noclip",          CurrentValue=state.noclip,      SaveId="jjs_noclip",      Callback=function(v) state.noclip=v      end })
moveSection:CreateToggle({ Name="Infinite Jump",   CurrentValue=state.infJump,     SaveId="jjs_inf_jump",     Callback=function(v) state.infJump=v     end })
moveSection:CreateToggle({ Name="WalkSpeed",       CurrentValue=state.walkSpeedOn, SaveId="jjs_walk_speed_on", Callback=function(v) state.walkSpeedOn=v end })
moveSection:CreateSlider({ Name="Speed", Range={16,120}, Increment=1, CurrentValue=state.walkSpeed, SaveId="jjs_walk_speed", Callback=function(v) state.walkSpeed=v end })
moveSection:CreateToggle({ Name="JumpPower",       CurrentValue=state.jumpOn,      SaveId="jjs_jump_on",      Callback=function(v) state.jumpOn=v      end })
moveSection:CreateSlider({ Name="Power", Range={50,250}, Increment=5, CurrentValue=state.jumpPower, SaveId="jjs_jump_power", Callback=function(v) state.jumpPower=v end })

----------------------------------------------------------------------
-- MISC TAB
----------------------------------------------------------------------
local miscTab       = Window:CreateTab("Misc")
local serverSection = miscTab:CreateSection("Server")

serverSection:CreateToggle({ Name="Target Lock (camera)", CurrentValue=state.targetLock, SaveId="jjs_target_lock", Callback=function(v) state.targetLock=v end })
serverSection:CreateToggle({ Name="Anti-AFK",             CurrentValue=state.antiAFK,    SaveId="jjs_anti_afk",    Callback=function(v) state.antiAFK=v    end })
serverSection:CreateSlider({ Name="FOV", Range={40,120}, Increment=1, CurrentValue=state.fov, SaveId="jjs_fov", Callback=function(v) state.fov=v end })
serverSection:CreateButton({ Name="Respawn Character", Callback=function() local h=hum(); if h then h.Health=0 end end })
serverSection:CreateButton({ Name="Rejoin Server",     Callback=function() S(function() game:GetService('TeleportService'):Teleport(game.PlaceId,lp) end) end })
serverSection:CreateButton({ Name="Unload Suite",      Callback=function() _G.__JJS_UNLOAD__() end })

local otherSection = miscTab:CreateSection("Other")
otherSection:CreateLabel({ Text="v"..VERSION, Color=Color3.fromRGB(150,99,255) })

Window:OnClose(function()
    if _G.__JJS_UNLOAD__ then _G.__JJS_UNLOAD__() end
end)

----------------------------------------------------------------------
-- ESP (raw BillboardGui, no library dependency)
----------------------------------------------------------------------
local function new(cls, props, parent)
    local o=Instance.new(cls); for k,v in pairs(props or {}) do o[k]=v end; if parent then o.Parent=parent end; return o
end
local function makeESP(c)
    local head=S(function() return c:FindFirstChild('Head') end); if not head then return end
    local bg=new('BillboardGui',{Name='__JJSESP__',Size=UDim2.new(0,210,0,42),StudsOffset=Vector3.new(0,3.2,0),AlwaysOnTop=true,MaxDistance=1000,Adornee=head},head)
    local nl=new('TextLabel',{Size=UDim2.new(1,0,0,15),BackgroundTransparency=1,Text=c.Name,TextColor3=Color3.new(1,1,1),Font=Enum.Font.GothamBold,TextSize=13,TextStrokeTransparency=0.5},bg)
    local hpBg=new('Frame',{Size=UDim2.new(0.8,0,0,5),Position=UDim2.new(0.1,0,0,17),BackgroundColor3=Color3.fromRGB(30,30,30),BorderSizePixel=0},bg)
    new('UICorner',{CornerRadius=UDim.new(0,2)},hpBg)
    local hp=new('Frame',{Size=UDim2.new(1,0,1,0),BackgroundColor3=Color3.fromRGB(60,220,80),BorderSizePixel=0},hpBg)
    new('UICorner',{CornerRadius=UDim.new(0,2)},hp)
    local dl=new('TextLabel',{Size=UDim2.new(1,0,0,13),Position=UDim2.new(0,0,0,24),BackgroundTransparency=1,Text='',TextColor3=Color3.fromRGB(190,190,200),Font=Enum.Font.Gotham,TextSize=11,TextStrokeTransparency=0.6},bg)
    espObjs[c.Name]={bg=bg,nl=nl,hp=hp,dl=dl,hl=nil,model=c}
end
local function clearESP(n)
    local e=espObjs[n]; if e then S(function() if e.bg then e.bg:Destroy() end end); S(function() if e.hl then e.hl:Destroy() end end); espObjs[n]=nil end
end

----------------------------------------------------------------------
-- COMBAT ACTIONS
----------------------------------------------------------------------
local function faceTarget(tr)
    local r=hrp(); if not (r and tr) then return end
    r.CFrame=CFrame.new(r.Position, Vector3.new(tr.Position.X, r.Position.Y, tr.Position.Z))
end
local function approachTarget(tr)
    if not state.glueTarget then return end
    for _=1,400 do
        if unloaded then break end
        local r=hrp()
        if not (r and tr and tr.Parent) then break end
        if (tr.Position-r.Position).Magnitude <= state.comboRange then break end
        task.wait(0.05)
    end
end
local comboFocus = nil
local engaged    = false
local MOVE_KEYS  = {'W','A','S','D'}
local function releaseMoveKeys() for _,k in ipairs(MOVE_KEYS) do setHold(k,false) end end
local function moveTowardPos(goal)
    local r=hrp(); if not (r and goal) then releaseMoveKeys(); return end
    local to=goal-r.Position; to=Vector3.new(to.X,0,to.Z)
    if to.Magnitude<0.5 then releaseMoveKeys(); return end
    to=to.Unit
    local cam=workspace.CurrentCamera
    local fwd=cam.CFrame.LookVector; fwd=Vector3.new(fwd.X,0,fwd.Z); fwd=fwd.Magnitude>0.1 and fwd.Unit or Vector3.new(0,0,-1)
    local rgt=cam.CFrame.RightVector; rgt=Vector3.new(rgt.X,0,rgt.Z); rgt=rgt.Magnitude>0.1 and rgt.Unit or Vector3.new(1,0,0)
    local f=to:Dot(fwd); local s=to:Dot(rgt)
    setHold('W', f>0.35); setHold('S', f<-0.35)
    setHold('D', s>0.35); setHold('A', s<-0.35)
end
local lastDashT=0
local function dashToward()
    if tick()-lastDashT < (state.dashCd or 0.7) then return end
    lastDashT=tick(); gameDash()
end
local function lungeAt(tr)
    local r=hrp(); local h=hum(); if not (r and h and tr) then return end
    local dir=tr.Position-r.Position; dir=Vector3.new(dir.X,0,dir.Z)
    if dir.Magnitude<0.1 then return end
    h:Move(dir.Unit, false); gameDash()
end
local PFS = game:GetService('PathfindingService')
local pathWPs, pathIdx, jumpWP = nil, 1, false
local function flatDist(a,b) return (Vector3.new(a.X,0,a.Z)-Vector3.new(b.X,0,b.Z)).Magnitude end
local function pathGoal(targetPos)
    local r=hrp(); if not (r and pathWPs) then return nil end
    while pathIdx <= #pathWPs and flatDist(pathWPs[pathIdx].Position, r.Position) < 6 do pathIdx=pathIdx+1 end
    if pathIdx > #pathWPs then return targetPos end
    local wp = pathWPs[pathIdx]; jumpWP = (wp.Action == Enum.PathWaypointAction.Jump)
    return wp.Position
end
local function trackTarget(tr) if tr then faceTarget(tr) end end

local function sampleRange(moveName, th, dist, base)
    if not (th and dist) then return end
    local c = th.Parent; local before = th.Health; local hadStun = c and stateOn(c,'Stun')
    task.spawn(function()
        task.wait(0.4)
        if not (th and th.Parent) then return end
        local hit = th.Health < before-2 or (stateOn(th.Parent,'Stun') and not hadStun) or active(th.Parent:GetAttribute('Ragdoll'))
        local cur = learnedRange[moveName]
        if hit then learnedRange[moveName] = math.max(cur or 0, dist)
        else local est = cur or base or dist; if dist <= est + 1 then learnedRange[moveName] = math.max(est*0.9, 6) end end
    end)
end

local M1_RANGE = 10
local function nameHas(n, list) n=n:lower() for _,kw in ipairs(list) do if n:find(kw,1,true) then return true end end return false end
local MELEE_KW  = {'kick','punch','fist','strike','rush','slam','blow','smash','grab','beat','claw','chop','flurry','barrage','stomp','knee','elbow','uppercut','dropkick','crush','bash','pummel','headbutt','combo'}
local RANGED_KW = {'blue','red','purple','arrow','beam','bullet','orb','ball','shot','wave','blast','bolt','fire','flame','lightning','void','ray','spear','pillar','plasma','blood','slash','dismantle','throw','poison','scale','wing','energy','reversal','hollow','cleave'}
local MOVE_DB = {
  ['Lapse Blue']={r=35,e='pull'}, ['Reversal Red']={r=40,e='knockback'}, ['Rapid Punches']={r=7,e='stun'}, ['Twofold Kick']={r=7,e='launch'},
  ['Lapse Blue MAX']={r=45,e='pull'}, ['Reversal Red MAX']={r=55,e='ragdoll'}, ['Hollow Purple']={r=60,e='knockback'}, ['Infinite Void']={r=80,e='stun'},
  ['Cursed Strikes']={r=10,e='stun'}, ['Crushing Blow']={r=8,e='launch'}, ['Divergent Fist']={r=8,e='launch'}, ['Manji Kick']={r=14,e='counter'},
  ['Dismantle']={r=30,e='knockback'}, ['Open']={r=50,e='knockback'}, ['Rush']={r=20,e='launch'}, ['Malevolent Shrine']={r=80,e='ragdoll'},
  ['Reserve Balls']={r=65,e='ragdoll'}, ['Shutter Doors']={r=8,e='stun'}, ['Rough Energy']={r=8,e='launch'}, ['Fever Breaker']={r=40,e='launch'},
  ['Lucky Volley']={r=8,e='stun'}, ['Lucky Rushdown']={r=70,e='knockback'}, ['Overwhelming Luck']={r=8,e='launch'}, ['Energy Surge']={r=8,e='launch'},
  ['Rabbit Escape']={r=10,e='stun'}, ['Nue']={r=50,e='knockback'}, ['Toad']={r=75,e='pull'}, ['Divine Dog: Totality']={r=60,e='knockback'},
  ['Max Elephant']={r=60,e='knockback'}, ['Great Serpent']={r=60,e='stun'}, ['Shadow Swarm']={r=10,e='launch'}, ['Mahoraga']={r=60,e='summon'},
  ['Divine Pummel']={r=8,e='launch'}, ['Ground Pitch']={r=20,e='knockback'}, ['Earthquake']={r=12,e='knockback'}, ['Takedown']={r=70,e='launch'},
  ['Adaptation']={r=10,e='counter'}, ['World Slash']={r=60,e='knockback'},
  ['Stockpile']={r=8,e='launch'}, ['Soul Fire']={r=50,e='knockback'}, ['Focus Strike']={r=8,e='stun'}, ['Body Repel']={r=70,e='knockback'},
  ['Idle Transfiguration']={r=40,e='stun'}, ['Body Disfigure']={r=8,e='launch'}, ['Spike Wrath']={r=25,e='stun'},
  ['Embodiment of Self Perfection']={r=80,e='stun'}, ['Widespread Strikes']={r=15,e='stun'}, ['Face Blitz']={r=8,e='knockback'}, ['Head Splitter']={r=14,e='counter'},
  ['Piercing Blood']={r=30,e='knockback'}, ['Flowing Red Scale']={r=8,e='launch'}, ['Supernova']={r=14,e='counter'}, ['Blood Edge']={r=8,e='knockback'},
  ['Slicing Exorcism']={r=50,e='knockback'}, ['Wing King']={r=8,e='pull'}, ['Blood Rain']={r=35,e='knockback'}, ['Plasma Wave']={r=120,e='knockback'},
  ['Boogie Woogie']={r=60,e='special'}, ['Swift Kick']={r=8,e='launch'}, ['Brute Force']={r=15,e='knockback'}, ['Pebble Throw']={r=45,e='knockback'},
  ['Elbow Drop']={r=8,e='knockback'}, ["Idol's Debut"]={r=30,e='knockback'}, ['Climax Jumping']={r=40,e='knockback'}, ['Dreams']={r=8,e='stun'}, ['Brothers']={r=14,e='counter'},
  ['No Escape']={r=50,e='knockback'}, ['Extended Swings']={r=8,e='knockback'}, ['Justice Served']={r=8,e='launch'}, ["Judgement's Reach"]={r=40,e='stun'},
  ['Pressing Charges']={r=8,e='knockback'}, ['Execution']={r=12,e='stun'}, ['Final Judgement']={r=14,e='stun'}, ['Verdict']={r=8,e='stun'}, ['Triple Sentence']={r=50,e='knockback'},
  ['Severing Path']={r=8,e='knockback'}, ['Resolute Slash']={r=35,e='knockback'}, ['Veilstep']={r=8,e='knockback'}, ['Revolve']={r=8,e='pull'}, ['Second Wind']={r=10,e='stun'}, ['Outburst']={r=12,e='knockback'},
  ['Rika Downslam']={r=10,e='knockback'}, ['Rika Launch']={r=12,e='launch'}, ['Rika Throw']={r=15,e='knockback'}, ['Rika Haymaker']={r=12,e='knockback'},
  ['Elbow Rush']={r=12,e='stun'}, ['True Love Beam']={r=80,e='knockback'}, ['Authentic Mutual Love']={r=80,e='knockback'}, ['Shrine']={r=80,e='knockback'},
  ['Ultra Spin']={r=8,e='knockback'}, ['Boost On']={r=8,e='launch'}, ['Ultra Cannon']={r=50,e='knockback'}, ['Heat Emission']={r=8,e='knockback'},
  ['Miracle Cannon']={r=50,e='knockback'}, ['Pigeon Viola']={r=250,e='knockback'}, ['Absolute Destruction']={r=15,e='knockback'}, ['Technique Charge']={r=50,e='knockback'},
  ['Projection Sorcery']={r=50,e='special'}, ['Projection Breaker']={r=8,e='knockback'}, ['Bleedout']={r=8,e='stun'}, ['Decisive Strike']={r=12,e='stun'},
  ['Cursory Impact']={r=8,e='knockback'}, ['Acceleration']={r=8,e='stun'}, ['Top Speed']={r=15,e='knockback'}, ['Flash Freezing']={r=15,e='stun'},
  ['Tendril Grab']={r=25,e='stun'}, ['Time Cell Moon Palace']={r=80,e='stun'},
  ['Cleaving Whirlwind']={r=8,e='knockback'}, ['Severance Kick']={r=8,e='stun'}, ['Blunt Cut']={r=8,e='knockback'}, ['Cross Cut']={r=8,e='knockback'},
  ['Stabilize']={r=8,e='stun'}, ['Ratio Breaker']={r=8,e='stun'}, ['Sharpen']={r=10,e='stun'}, ['Erosion']={r=8,e='knockback'}, ['Interrogate']={r=8,e='stun'}, ['Collapse']={r=15,e='launch'},
  ['Disaster Root']={r=60,e='pull'}, ['Flower Field']={r=25,e='stun'}, ['Root Swarm']={r=15,e='launch'}, ['Surging Thorns']={r=35,e='launch'},
  ['Bud Shot']={r=70,e='ragdoll'}, ['Defense Response']={r=8,e='knockback'},
  ['Granite Blast']={r=90,e='knockback'}, ['Unsatisfied']={r=8,e='knockback'}, ['Second Helping']={r=70,e='knockback'}, ['Appetizer']={r=50,e='ragdoll'},
  ['Every Last Drop.']={r=100,e='knockback'}, ['"What are you after?"']={r=15,e='knockback'}, ['"I had no idea..."']={r=8,e='knockback'},
  ['"This is what dessert is like!"']={r=15,e='knockback'}, ["\"You weren't invited.\""]={r=15,e='knockback'},
  ['Crushing Jaws']={r=8,e='ragdoll'}, ['Clever']={r=10,e='stun'}, ['Wing Throw']={r=8,e='launch'}, ['Black Mucus']={r=60,e='ragdoll'},
  ['Garuda Rebound']={r=50,e='summon'}, ['Rising Rage']={r=8,e='launch'}, ['Mass Breaker']={r=10,e='knockback'}, ['Garuda Stab']={r=12,e='knockback'},
  ['Strong Dismantle']={r=35,e='knockback'}, ['Open FURNACE']={r=50,e='knockback'}, ['Cleave Rush']={r=20,e='knockback'}, ['Kamutoke']={r=40,e='stun'},
  ['Despair']={r=8,e='launch'}, ['Shut Up!']={r=10,e='counter'}, ['Eye Catching']={r=14,e='counter'}, ['Sacrilege']={r=12,e='launch'},
  ['Kamehameha']={r=80,e='knockback'}, ['Ki Spam']={r=50,e='ragdoll'}, ['Staff Extend']={r=40,e='knockback'}, ['Staff Uppercut']={r=8,e='launch'},
  ['Ambush']={r=8,e='special'}, ['Backstab']={r=8,e='stun'}, ['Trip']={r=8,e='knockback'}, ['Cheap Shot']={r=40,e='knockback'},
  ['Ankle Cutter']={r=8,e='ragdoll'}, ['High Time']={r=8,e='launch'}, ['Dirty Play']={r=40,e='knockback'},
  ['Impetus Updraft']={r=8,e='launch'}, ['Circling']={r=12,e='knockback'}, ['Gliding Flight']={r=12,e='special'}, ['Bird Control']={r=80,e='knockback'},
  ['Festering Strikes']={r=8,e='stun'}, ['Detach']={r=30,e='knockback'}, ['Chokehold']={r=8,e='knockback'}, ['Roach Swarm']={r=40,e='knockback'},
}
local function baseRange(m)
    local n=m.name or ''
    if nameHas(n, MELEE_KW) then return 7 end
    if (m.tip or ''):find('TARGET') or nameHas(n, RANGED_KW) then return 38 end
    return 14
end
local function moveRange(m)
    local d=MOVE_DB[m.name]; if d then return d.r end
    local b=baseRange(m); return math.clamp(learnedRange[m.name] or b, 6, b*1.7)
end
local function moveEffect(m) local d=MOVE_DB[m.name]; return d and d.e or nil end
local function comboReach()
    local mr=M1_RANGE
    for _,m in ipairs(liveMoveset()) do mr=math.max(mr, moveRange(m)) end
    return mr
end

local function fireSkillHinted(m, tr, th)
    trackTarget(tr)
    local tip = m.tip or ''; local key = m.key
    local before = m.inst and (tonumber(m.inst:GetAttribute('LastUse')) or 0) or 0
    if tip:find('TARGET') then comboLockUntil=tick()+1.0 end
    if tip:find('HOLD') then
        fireAction('Skill '..key); task.wait(0.9); releaseAction('Skill '..key)
    else
        fireAction('Skill '..key)
        if m.inst then
            local t0=tick()
            repeat task.wait(0.03) until (tonumber(m.inst:GetAttribute('LastUse')) or 0)~=before or tick()-t0>0.35
            if (tonumber(m.inst:GetAttribute('LastUse')) or 0)==before then
                fireAction('Skill '..key)
                local t1=tick(); repeat task.wait(0.03) until (tonumber(m.inst:GetAttribute('LastUse')) or 0)~=before or tick()-t1>0.25
            end
        end
        if tip:find('TWICE') or tip:find('AGAIN') or tip:find('SPECIAL') then
            task.wait(state.bfDelay or 0.22); trackTarget(tr); fireAction('Skill '..key)
        end
    end
    local r=hrp(); sampleRange(m.name, th, (tr and r) and (tr.Position-r.Position).Magnitude or nil, baseRange(m))
    task.wait(state.comboSpeed)
end

local function doToken(tok, tr, th)
    trackTarget(tr)
    if tok=='M1' then fireAction('Melee'); return 0.24
    elseif tok=='Q' then fireAction('Dash'); return 0.16
    elseif tok=='R' then fireAction('Special'); return state.comboSpeed
    elseif tok=='SP' then local h=hum(); if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end; return 0.14
    elseif tok:match('^%d$') then
        local r=hrp(); local d=(tr and r) and (tr.Position-r.Position).Magnitude or nil
        fireAction('Skill '..tok); sampleRange('Skill '..tok, th, d, 18)
        return state.comboSpeed
    end
    return 0.1
end

local comboLock=false; local lastSpecial=0; local m1DownUntil=0
local function runCombo(target)
    if comboLock then return end; comboLock=true
    task.spawn(function()
      local ok,err = pcall(function()
        local tr=target and S(function() return target:FindFirstChild('HumanoidRootPart') end)
        local th=target and S(function() return target:FindFirstChildOfClass('Humanoid') end)
        if tr then approachTarget(tr); faceTarget(tr) end
        local preset=COMBO_PRESETS[state.comboPreset]; local route=COMBO_ROUTES[myName()]
        if preset=='ROUTE' and route then
            for _,tok in ipairs(route) do
                if unloaded then break end
                task.wait(doToken(tok, tr, th))
                if stateOn(char(),'Stun') or active(attr(char(),'Dead')) then break end
            end
        else
            local reverse = (preset=='4-3-2-1')
            local function inRangeOf(rng) local rr=hrp(); return rr and tr and tr.Parent and (tr.Position-rr.Position).Magnitude <= rng end
            local function maxRange() local mr=M1_RANGE for _,mv in ipairs(liveMoveset()) do mr=math.max(mr, moveRange(mv)) end return mr end
            local function tgtInRange()
                if unloaded or not tr or not tr.Parent or not validEnemy(target) then return false end
                return inRangeOf(maxRange()+4)
            end
            while tgtInRange() and not stateOn(char(),'Stun') and not active(attr(char(),'Dead')) do
                while blocking and not unloaded do holdM1(false); task.wait(0.05) end
                if unloaded then break end
                if state.autoBlock and genuineThreat(target) then break end
                trackTarget(tr)
                if state.autoBlackFlash and inRangeOf(M1_RANGE+2) then
                    holdM1(false)
                    for _=1,4 do
                        if unloaded or blocking or stateOn(char(),'Stun') or not inRangeOf(M1_RANGE+2) or not validEnemy(target) then break end
                        trackTarget(tr); clickM1(); task.wait(state.bfDelay or 0.37)
                    end
                elseif tick() >= m1DownUntil and inRangeOf(M1_RANGE+2)
                       and not active(attr(target,'Ragdoll')) and not stateOn(target,'Stun') then
                    holdM1(true)
                    local ms0=tick()
                    repeat task.wait(0.04) until unloaded or blocking or stateOn(char(),'Stun')
                        or (char():GetAttribute('CurrentM1') or 0) >= 4
                        or active(attr(target,'Ragdoll')) or stateOn(target,'Stun')
                        or not inRangeOf(M1_RANGE+4) or tick()-ms0>1.5
                    holdM1(false)
                    if (char():GetAttribute('CurrentM1') or 0) >= 4 or active(attr(target,'Ragdoll')) then m1DownUntil=tick()+1.9 end
                else holdM1(false) end
                local moves=liveMoveset(); local order={}; for i=1,#moves do order[i]=i end
                table.sort(order, function(a,b)
                    local ra,rb = moveRange(moves[a]), moveRange(moves[b])
                    if math.abs(ra-rb) > 2 then return ra < rb end
                    if reverse then return a>b end; return a<b
                end)
                for _,idx in ipairs(order) do
                    if not (tr and tr.Parent) or not validEnemy(target) or stateOn(char(),'Stun') then break end
                    local m = moves[idx]; local rng = moveRange(m)
                    local ok2 = m and m.ready and inRangeOf(rng)
                    if ok2 and moveEffect(m)=='pull' and inRangeOf(rng*0.45) then ok2=false end
                    if ok2 then holdM1(false); fireSkillHinted(m, tr, th); trackTarget(tr) end
                end
                if (active(attr(target,'Ragdoll')) or stateOn(target,'Stun')) and not stateOn(char(),'Stun') then
                    local w0=tick()
                    while tick()-w0 < 1.6 and validEnemy(target) and not stateOn(char(),'Stun') and not blocking and not unloaded do
                        if not active(attr(target,'Ragdoll')) then
                            local r=hrp(); local d = (r and tr and tr.Parent) and (tr.Position-r.Position).Magnitude or 999
                            if d > M1_RANGE and d <= 28 then lungeAt(tr) end
                            if d <= 28 then faceTarget(tr); m1DownUntil=tick(); holdM1(inRangeOf(M1_RANGE+3)) end
                            break
                        end
                        local didFire=false
                        for _,m in ipairs(liveMoveset()) do
                            if m.ready and inRangeOf(moveRange(m)) and moveEffect(m)~='pull' then
                                holdM1(false); fireSkillHinted(m, tr, th); didFire=true; break
                            end
                        end
                        if not didFire then task.wait(0.05) end
                    end
                end
                if state.useSpecial and tick()-lastSpecial > state.specialCd and inRangeOf(30)
                   and not stateOn(char(),'Stun') and validEnemy(target) then
                    lastSpecial=tick(); holdM1(false); fireAction('Special'); trackTarget(tr); task.wait(0.25)
                end
                task.wait(0.04)
            end
            holdM1(false)
        end
      end)
      if not ok then _G.__JJS_COMBOERR__ = tostring(err) end
      holdM1(false); task.wait(0.2); comboLock=false
    end)
end

task.spawn(function()
    while not unloaded do
        local ok=false
        if state.autoCombo and state.glueTarget and comboFocus and comboFocus.Parent then
            local r=hrp(); local tr=r and S(function() return comboFocus:FindFirstChild('HumanoidRootPart') end)
            if r and tr then
                local dist=(tr.Position-r.Position).Magnitude
                if dist>state.comboRange and dist<=state.engageRange then
                    local path=PFS:CreatePath({AgentRadius=2.5, AgentHeight=5, AgentCanJump=true, WaypointSpacing=6})
                    local cok=pcall(function() path:ComputeAsync(r.Position, tr.Position) end)
                    if cok and path.Status==Enum.PathStatus.Success then pathWPs=path:GetWaypoints(); pathIdx=2; ok=true end
                end
            end
        end
        if not ok then pathWPs=nil end
        task.wait(0.5)
    end
end)

track(RunService.Heartbeat:Connect(function()
    if not (state.autoCombo and state.glueTarget) or not comboFocus then releaseMoveKeys(); engaged=false; return end
    local t=comboFocus; if not t.Parent then releaseMoveKeys(); engaged=false; return end
    local r=hrp(); local tr=r and S(function() return t:FindFirstChild('HumanoidRootPart') end)
    if not tr then releaseMoveKeys(); return end
    local dist=(tr.Position-r.Position).Magnitude
    if dist > state.engageRange*1.5 then releaseMoveKeys(); return end
    if stateOn(char(),'Stun') or stateOn(char(),'Ragdoll') then releaseMoveKeys(); return end
    if dist > state.engageDist+1 then
        local goal = (dist > state.comboRange and pathGoal(tr.Position)) or tr.Position
        moveTowardPos(goal)
        if jumpWP then local h=hum(); if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end end
        if dist > 14 and not jumpWP then dashToward() end
    else releaseMoveKeys() end
end))

local awakenLock=false
local function tryAwaken()
    if awakenLock then return end; awakenLock=true; fireAction('Awaken')
    task.spawn(function() task.wait(5); awakenLock=false end)
end

local BLOCK_KEY       = Enum.KeyCode.F
local BlockSvc        = S(function() return RS.Knit.Knit.Services.BlockService end)
local BlockActivated  = BlockSvc and S(function() return BlockSvc:FindFirstChild('Activated',   true) end)
local BlockDeactivated= BlockSvc and S(function() return BlockSvc:FindFirstChild('Deactivated', true) end)
local lastBlockOn=0; local lastBlockFire=0
local function myBlockOn()
    local c=char(); if not c then return false end
    return stateOn(c,'Block') or active(c:GetAttribute('BlockTry'))
end
local function fireBlock(on)
    if BlockActivated and BlockDeactivated then S(function() (on and BlockActivated or BlockDeactivated):FireServer() end)
    else S(function() VIM:SendKeyEvent(on, BLOCK_KEY, false, game) end) end
end
local function setBlock(on)
    if on then
        if not blocking then blocking=true; lastBlockOn=tick() end
        if not myBlockOn() and tick()-lastBlockFire > 0.08 then lastBlockFire=tick(); fireBlock(true) end
    else if blocking then blocking=false; fireBlock(false) end end
end

local lastParry=0
S(function()
    local Knit=require(RS.Knit.Knit); local bs=Knit.GetService('BlockService')
    if bs and bs.Effects then
        track(bs.Effects:Connect(function(who, perfect)
            if perfect and who==char() then lastParry=tick(); _G.__JJS_PARRIES__=(_G.__JJS_PARRIES__ or 0)+1 end
        end))
    end
end)

local function burstEscapeAvail()
    local c=char(); if not c then return false end
    local info=c:FindFirstChild('Info')
    return active(c:GetAttribute('Burst')) and not (info and info:GetAttribute('Burst'))
end
local function kbCancelAvail()
    local c=char(); if not c then return false end
    local info=c:FindFirstChild('Info'); if not info then return false end
    if not (info:FindFirstChild('Knockback') or info:FindFirstChild('ForceEsc')) then return false end
    if (tonumber(c:GetAttribute('Evade')) or 0) < 50 then return false end
    for _,x in ipairs(info:GetChildren()) do if x:GetAttribute('Disable') then return false end end
    return true
end
local function canEscape() return burstEscapeAvail() or kbCancelAvail() end
local lastEscape=0
local function escapeStep()
    if tick()-lastEscape < 0.3 then return end; lastEscape=tick(); gameDash()
end

local DASH_CD=2.0; local lastDashFire=0
local function dashReady()
    if tick()-lastDashFire < DASH_CD then return false end
    local c=char(); if not c then return false end
    if stateOn(c,'NoDash') or stateOn(c,'Stun') or active(c:GetAttribute('Ragdoll')) then return false end
    return true
end

local lastComboEscape=0
local function doComboEscape(attacker)
    if tick()-lastComboEscape < 1.6 then return end
    local atkR = attacker and S(function() return attacker:FindFirstChild('HumanoidRootPart') end)
    if not atkR then return end
    if not (dashReady() or canEscape()) then return end
    lastComboEscape=tick()
    task.spawn(function()
        if canEscape() then escapeStep(); task.wait(0.06) end
        local r=hrp(); local h=hum()
        if r and h and atkR.Parent then
            local away=r.Position-atkR.Position; away=Vector3.new(away.X,0,away.Z)
            away = away.Magnitude>0.1 and away.Unit or Vector3.new(0,0,1)
            h:Move(away, false); lastDashFire=tick(); gameDash()
        end
        task.wait(state.escapeReturn or 0.45)
        if unloaded or not validEnemy(attacker) or not atkR.Parent then return end
        if stateOn(char(),'Stun') or active(attr(char(),'Ragdoll')) then return end
        lungeAt(atkR); task.wait(0.12); faceTarget(atkR)
        clickM1(); task.wait(0.18); clickM1()
    end)
end

local function realDodge(threat)
    if not dashReady() then return false end
    local r=hrp(); local h=hum(); local er=threat and S(function() return threat:FindFirstChild('HumanoidRootPart') end)
    if not (r and h and er) then return false end
    local away=r.Position-er.Position; away=Vector3.new(away.X,0,away.Z)
    away = away.Magnitude>0.1 and away.Unit or Vector3.new(0,0,1)
    local perp=Vector3.new(-away.Z,0,away.X); if math.random()<0.5 then perp=-perp end
    local dir=(perp*0.85 + away*0.35); dir = dir.Magnitude>0.1 and dir.Unit or away
    h:Move(dir, false); lastDashFire=tick(); gameDash()
    return true
end

local lastFlank=0
local function flat(v) return Vector3.new(v.X,0,v.Z) end
local function flankDash(target)
    if tick()-lastFlank < (state.flankCd or 1.0) then return false end
    if not dashReady() then return false end
    local r=hrp(); local h=hum(); local tr=target and S(function() return target:FindFirstChild('HumanoidRootPart') end)
    if not (r and h and tr) then return false end
    local committed = stateOn(target,'Stun') or active(attr(target,'Ragdoll')) or stateOn(target,'InSkill')
                      or (serverNow()-(tonumber(attr(target,'LastM1')) or 0) < 0.8)
    if not committed then return false end
    local gap=(flat(tr.Position)-flat(r.Position)).Magnitude
    if gap>(state.flankRange or 30) or gap<3 then return false end
    local look=flat(tr.CFrame.LookVector); look=look.Magnitude>0.1 and look.Unit or Vector3.new(0,0,1)
    local toMe=flat(r.Position-tr.Position); if toMe.Magnitude<0.1 then return false end
    if look:Dot(toMe.Unit) <= 0.2 then return false end
    local goal=tr.Position - look*4; local dir=flat(goal-r.Position); if dir.Magnitude<0.1 then return false end
    h:Move(dir.Unit, false); lastFlank=tick(); lastDashFire=tick(); gameDash()
    task.delay(0.12, function() if not unloaded then faceTarget(tr) end end)
    return true
end

local function nearestEnemy(maxDist)
    local origin=hrp(); if not origin then return end
    local best,bestD=nil,maxDist or math.huge
    local chars=workspace:FindFirstChild('Characters'); if not chars then return end
    for _,c in ipairs(chars:GetChildren()) do
        if c:IsA('Model') and c~=char() and c.Name~='Dummy' then
            local h=S(function() return c:FindFirstChildOfClass('Humanoid') end)
            local r=S(function() return c:FindFirstChild('HumanoidRootPart') end)
            if h and r and h.Health>0 and not active(c:GetAttribute('Dead')) then
                local d=(r.Position-origin.Position).Magnitude
                if d<bestD then best,bestD=c,d end
            end
        end
    end
    return best,bestD
end

function validEnemy(c)
    if not (c and c.Parent) or c==char() or c.Name=='Dummy' then return false end
    local h=S(function() return c:FindFirstChildOfClass('Humanoid') end)
    local r=S(function() return c:FindFirstChild('HumanoidRootPart') end)
    if not (h and r and h.Health>0) then return false end
    if active(c:GetAttribute('Dead')) or stateOn(c,'IFrame') then return false end
    return true,h,r
end
local function isAttacker(c, origin)
    local er=S(function() return c:FindFirstChild('HumanoidRootPart') end); if not er then return false end
    local toMe=origin.Position-er.Position; toMe=Vector3.new(toMe.X,0,toMe.Z)
    local facing = toMe.Magnitude<3 or (toMe.Magnitude>0 and er.CFrame.LookVector:Dot(toMe.Unit)>0.3)
    return facing and (stateOn(c,'InSkill') or (serverNow()-(tonumber(c:GetAttribute('LastM1')) or 0))<1.5)
end
local function pickTarget(range)
    local origin=hrp(); if not origin then return end
    local mode=TARGET_MODES[state.targetMode]
    local chars=workspace:FindFirstChild('Characters'); if not chars then return end
    local bestAtk,bestAtkD,best,bestScore
    for _,c in ipairs(chars:GetChildren()) do
        local ok,h,r=validEnemy(c)
        if ok then
            local d=(r.Position-origin.Position).Magnitude
            if d<=range then
                if isAttacker(c,origin) and (not bestAtkD or d<bestAtkD) then bestAtk,bestAtkD=c,d end
                local score=(mode=='Lowest HP') and h.Health or d
                if not bestScore or score<bestScore then best,bestScore=c,score end
            end
        end
    end
    return bestAtk or best
end

function genuineThreat(ignore)
    local r=hrp(); if not r then return nil end
    local chars=workspace:FindFirstChild('Characters'); if not chars then return nil end
    for _,c in ipairs(chars:GetChildren()) do
        if c:IsA('Model') and c~=char() and c~=ignore and c.Name~='Dummy' then
            local er=S(function() return c:FindFirstChild('HumanoidRootPart') end)
            local eh=S(function() return c:FindFirstChildOfClass('Humanoid') end)
            if er and eh and eh.Health>0 and not active(c:GetAttribute('Dead'))
               and not stateOn(c,'Stun') and not active(c:GetAttribute('Ragdoll')) then
                local d=(er.Position-r.Position).Magnitude
                if d<=state.blockRange then
                    local toUs=Vector3.new(r.Position.X-er.Position.X,0,r.Position.Z-er.Position.Z)
                    local facing = toUs.Magnitude<2 or er.CFrame.LookVector:Dot(toUs.Unit)>0.5
                    local m1age = serverNow()-(tonumber(c:GetAttribute('LastM1')) or 0)
                    local meleeImminent = facing and m1age<1.0 and d<=9
                    local skillImminent = facing and stateOn(c,'InSkill')
                    if meleeImminent or skillImminent then return c,d end
                end
            end
        end
    end
    return nil
end

local function enemyCastMove(c)
    local ms=c:FindFirstChild('Moveset'); if not ms then return nil end
    local now=serverNow(); local bestName,bestAge=nil,1.5
    for _,m in ipairs(ms:GetChildren()) do
        local lu=tonumber(m:GetAttribute('LastUse')) or 0
        if lu>0 then local age=now-lu; if age>=0 and age<bestAge then bestName,bestAge=m.Name,age end end
    end
    return bestName,bestAge
end

local DODGE_EFFECTS = { pull=true, ragdoll=true, special=true, grab=true }
local DODGE_KW = {'grab','throw','slam','pull','drag','chain','snare','root','trap','bind','tendril','jaw','mucus','sorcery','rebound','takedown','downslam','vacuum','suck','hook','net'}
local function isUnblockable(name, eff)
    if eff and DODGE_EFFECTS[eff] then return true end
    if name and nameHas(name, DODGE_KW) then return true end
    return false
end
local function rangeOfName(name)
    local d=MOVE_DB[name]; if d then return d.r end
    if not name then return state.parryUnknown or 45 end
    if nameHas(name, MELEE_KW) then return 7 end
    if nameHas(name, RANGED_KW) then return 38 end
    return 14
end
local function effectOfName(name) local d=MOVE_DB[name]; return d and d.e or nil end

local function flagThreat(dist, threat)
    local from = tick() + (state.parryLead or 0)
    local hold = (state.parryHold or 0.30) + math.clamp((dist or 0)*(state.rangeLead or 0.006), 0, 0.6)
    if blockUntil <= tick() then blockFrom = from else blockFrom = math.min(blockFrom, from) end
    blockUntil = math.max(blockUntil, from + hold)
    if threat then blockThreat = threat end
end
local function scheduleDodge(threat, dist)
    if not state.autoDodge then return end
    local lead = math.clamp((state.dodgeLead or 0) + (dist or 0)*(state.rangeLead or 0.006), 0, 0.6)
    task.delay(lead, function()
        if unloaded or not validEnemy(threat) then return end
        realDodge(threat)
    end)
end

local function handleMove(enemyChar, mvname)
    if unloaded then return end
    local h=hrp(); local eh=enemyChar and enemyChar:FindFirstChild('HumanoidRootPart')
    if not (h and eh and enemyChar.Parent) then return end
    local d=(eh.Position-h.Position).Magnitude
    local rng = rangeOfName(mvname); local eff = effectOfName(mvname)
    if d > rng + (state.parryMargin or 10) then return end
    if state.autoDodge and isUnblockable(mvname, eff) then scheduleDodge(enemyChar, d)
    elseif state.autoBlock then flagThreat(d, enemyChar) end
end

local watchedChars = setmetatable({}, {__mode='k'})
local function watchEnemyChar(plr, c)
    if not c or plr==lp or watchedChars[c] then return end
    watchedChars[c]=true
    task.delay(0.3, function()
        if not c.Parent then return end
        local function hookMove(mv)
            if not mv:IsA('ValueBase') then return end
            track(mv:GetAttributeChangedSignal('LastUse'):Connect(function() handleMove(c, mv.Name) end))
        end
        local function hookMoveset(ms)
            for _,mv in ipairs(ms:GetChildren()) do hookMove(mv) end
            track(ms.ChildAdded:Connect(hookMove))
        end
        local ms=c:FindFirstChild('Moveset')
        if ms then hookMoveset(ms)
        else track(c.ChildAdded:Connect(function(ch) if ch.Name=='Moveset' then hookMoveset(ch) end end)) end
        track(c.AttributeChanged:Connect(function(a)
            if unloaded or (a~='LastM1' and a~='CurrentM1') then return end
            local h=hrp(); local eh=c:FindFirstChild('HumanoidRootPart')
            if not (h and eh) then return end
            local d=(eh.Position-h.Position).Magnitude
            if state.autoBlock and state.parryM1 and d <= M1_RANGE + (state.parryMargin or 10) then flagThreat(d, c) end
        end))
    end)
end
local function watchEnemyPlayer(plr)
    if plr==lp then return end
    watchEnemyChar(plr, plr.Character)
    track(plr.CharacterAdded:Connect(function(c) watchEnemyChar(plr, c) end))
end
for _,p in ipairs(Players:GetPlayers()) do watchEnemyPlayer(p) end
track(Players.PlayerAdded:Connect(watchEnemyPlayer))

local function activeAttacker()
    local r=hrp(); if not r then return nil end
    local chars=workspace:FindFirstChild('Characters'); if not chars then return nil end
    local now=serverNow(); local best,bestD
    for _,c in ipairs(chars:GetChildren()) do
        if c:IsA('Model') and c~=char() and c.Name~='Dummy' then
            local er=S(function() return c:FindFirstChild('HumanoidRootPart') end)
            local eh=S(function() return c:FindFirstChildOfClass('Humanoid') end)
            if er and eh and eh.Health>0 and not active(c:GetAttribute('Dead'))
               and not stateOn(c,'Stun') and not active(c:GetAttribute('Ragdoll')) then
                local d=(er.Position-r.Position).Magnitude
                local toUs=Vector3.new(r.Position.X-er.Position.X,0,r.Position.Z-er.Position.Z)
                local facing = d<6 or (toUs.Magnitude>0 and er.CFrame.LookVector:Dot(toUs.Unit)>0.15)
                if facing then
                    local m1age = now-(tonumber(c:GetAttribute('LastM1')) or 0)
                    local threat = (m1age < 0.7 and d <= M1_RANGE + (state.parryMargin or 10))
                    if not threat and stateOn(c,'InSkill') then
                        local mv = enemyCastMove(c)
                        local rng = (mv and rangeOfName(mv)) or (state.parryUnknown or 45)
                        threat = d <= rng + (state.parryMargin or 10)
                    end
                    if threat and (not bestD or d<bestD) then best,bestD=c,d end
                end
            end
        end
    end
    return best,bestD
end

----------------------------------------------------------------------
-- MOVEMENT
----------------------------------------------------------------------
local flyBV,flyBG
local function startFly()
    local r,h=hrp(),hum(); if not (r and h) then return end; h.PlatformStand=true
    flyBV=new('BodyVelocity',{MaxForce=Vector3.new(1,1,1)*9e9,Velocity=Vector3.zero,P=1250},r)
    flyBG=new('BodyGyro',{MaxForce=Vector3.new(1,1,1)*9e9,P=9e4,D=500},r)
end
local function stopFly()
    if flyBV then flyBV:Destroy(); flyBV=nil end
    if flyBG then flyBG:Destroy(); flyBG=nil end
    local h=hum(); if h then h.PlatformStand=false end
end
track(RunService.RenderStepped:Connect(function()
    if state.fly then
        if not flyBV then startFly() end
        if flyBV and flyBG then
            local cam=workspace.CurrentCamera; local dir=Vector3.zero
            if UIS:IsKeyDown(Enum.KeyCode.W)           then dir+=cam.CFrame.LookVector end
            if UIS:IsKeyDown(Enum.KeyCode.S)           then dir-=cam.CFrame.LookVector end
            if UIS:IsKeyDown(Enum.KeyCode.A)           then dir-=cam.CFrame.RightVector end
            if UIS:IsKeyDown(Enum.KeyCode.D)           then dir+=cam.CFrame.RightVector end
            if UIS:IsKeyDown(Enum.KeyCode.Space)       then dir+=Vector3.new(0,1,0) end
            if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then dir-=Vector3.new(0,1,0) end
            flyBV.Velocity=(dir.Magnitude>0 and dir.Unit or Vector3.zero)*state.flySpeed
            flyBG.CFrame=cam.CFrame
        end
    elseif flyBV then stopFly() end
end))
track(RunService.Stepped:Connect(function()
    if state.noclip then local c=char(); if c then for _,p in ipairs(c:GetDescendants()) do if p:IsA('BasePart') and p.CanCollide then p.CanCollide=false end end end end
end))
track(UIS.JumpRequest:Connect(function() if state.infJump then local h=hum(); if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end end end))

----------------------------------------------------------------------
-- ANTI STUN / ANTI RAGDOLL
----------------------------------------------------------------------
local STUN_INFO  = {'Stun','NoM1','NoSprint','NoJump','NoDash','DirectionLock','Injury'}
local RAG_INFO   = {'Knockback','ForceEsc'}
local RAG_STATES = {[Enum.HumanoidStateType.Physics]=true,[Enum.HumanoidStateType.FallingDown]=true,[Enum.HumanoidStateType.Ragdoll]=true}
local ragRecover = 0
track(RunService.Heartbeat:Connect(function()
    if unloaded or not (state.antiStun or state.antiRagdoll) then return end
    local c=char(); if not c then return end
    local h=c:FindFirstChildOfClass('Humanoid'); local info=c:FindFirstChild('Info')
    if state.antiStun then
        if active(c:GetAttribute('Stun')) then pcall(function() c:SetAttribute('Stun', false) end) end
        if info then for _,nm in ipairs(STUN_INFO) do local v=info:FindFirstChild(nm)
            if v and v:IsA('ValueBase') and v.Value and v.Value~=false then pcall(function() v.Value=(nm=='Injury') and 0 or false end) end
        end end
    end
    if state.antiRagdoll then
        if active(c:GetAttribute('Ragdoll')) then pcall(function() c:SetAttribute('Ragdoll', 0) end); ragRecover=tick() end
        if h and ragRecover>0 and tick()-ragRecover < 1.2 then
            if RAG_STATES[h:GetState()] then pcall(function() h:ChangeState(Enum.HumanoidStateType.GettingUp) end) end
            if h.PlatformStand then pcall(function() h.PlatformStand=false end) end
        end
        if info then for _,nm in ipairs(RAG_INFO) do local v=info:FindFirstChild(nm)
            if v and v:IsA('ValueBase') and v.Value and v.Value~=false then pcall(function() v.Value=false end) end
        end end
    end
end))

----------------------------------------------------------------------
-- MAIN LOOP
----------------------------------------------------------------------
local nextRefresh,lastChar=0,nil
track(RunService.Heartbeat:Connect(function()
    local r=hrp(); if not r then return end
    local now=tick()

    if now>nextRefresh then
        nextRefresh=now+0.35
        local n=myName(); local data=CHAR_DATA[n]
        -- update character subtitle in Skills HUD section
        if subLabel and subLabel.SetText then
            subLabel:SetText(n..(data and ('  |  '..data.ult) or '')..'  v'..VERSION)
        end
        refreshPresetLabel()
        -- auto theme: update ACCENT variable (visual only; OvertimeUI does not expose live accent changes)
        if state.autoTheme and not state.rainbow and data and n~=lastChar then
            ACCENT=data.col; lastChar=n
        end
        -- live skills HUD labels
        local mv=liveMoveset()
        for i=1,4 do
            local lh=skillLabels[i]; local m=mv[i]
            if lh and lh.SetText then
                if m then
                    local tipTxt = (m.tip~='' and m.tip~='NIL') and ('  ['..m.tip..']') or ''
                    local cdTxt  = m.left>0.1 and string.format(' %.1fs',m.left) or ' READY'
                    lh:SetText(string.format('%d  %s%s  R%d%s', i, m.name, tipTxt, math.floor(moveRange(m)), cdTxt))
                else lh:SetText(tostring(i)..'  --') end
            end
        end
    end

    local h=hum()
    if h then
        if state.walkSpeedOn then h.WalkSpeed=state.walkSpeed end
        if state.jumpOn then h.UseJumpPower=true; h.JumpPower=state.jumpPower end
    end
    if state.fullbright then Lighting.Brightness=2; Lighting.ClockTime=14; Lighting.FogEnd=1e9; Lighting.GlobalShadows=false end
    workspace.CurrentCamera.FieldOfView=state.fov

    local seen={}
    local chars=workspace:FindFirstChild('Characters')
    if chars then
        for _,c in ipairs(chars:GetChildren()) do
            if c:IsA('Model') and c~=char() and c.Name~='Dummy' then
                local eh=S(function() return c:FindFirstChildOfClass('Humanoid') end)
                local er=S(function() return c:FindFirstChild('HumanoidRootPart') end)
                if eh and er then
                    seen[c.Name]=true
                    local dist=(er.Position-r.Position).Magnitude
                    local stun  =stateOn(c,'Stun'); local ragd=active(c:GetAttribute('Ragdoll'))
                    local iframe=stateOn(c,'IFrame'); local block=stateOn(c,'Block') or active(c:GetAttribute('BlockTry'))
                    local dead  =active(c:GetAttribute('Dead')) or eh.Health<=0
                    local cdata =CHAR_DATA[tostring(c:GetAttribute('Moveset'))]
                    if state.esp and not dead then
                        if not espObjs[c.Name] then makeESP(c) end
                        local e=espObjs[c.Name]
                        if e and e.bg then
                            local pct=math.clamp(eh.Health/math.max(eh.MaxHealth,1),0,1)
                            e.hp.Size=UDim2.new(pct,0,1,0)
                            e.hp.BackgroundColor3 = pct>0.6 and Color3.fromRGB(60,220,80) or pct>0.3 and Color3.fromRGB(235,215,60) or Color3.fromRGB(235,70,70)
                            e.dl.Text=string.format('%s  |  %dm',cdata and cdata.ult or tostring(c:GetAttribute('Moveset') or '?'),math.floor(dist))
                            e.nl.TextColor3 = stun and Color3.fromRGB(255,170,40) or ragd and Color3.fromRGB(255,70,70) or iframe and Color3.fromRGB(180,120,255) or block and Color3.fromRGB(90,160,255) or Color3.fromRGB(255,255,255)
                            e.bg.Enabled = dist<state.espRange
                        end
                    elseif espObjs[c.Name] then clearESP(c.Name) end
                    if state.chams and not dead then
                        local e=espObjs[c.Name]
                        if e and not e.hl then e.hl=new('Highlight',{Name='__JJSHL__',Adornee=c,FillColor=cdata and cdata.col or ACCENT,FillTransparency=0.55,OutlineColor=Color3.new(1,1,1),OutlineTransparency=0,DepthMode=Enum.HighlightDepthMode.AlwaysOnTop},c) end
                    else local e=espObjs[c.Name]; if e and e.hl then e.hl:Destroy(); e.hl=nil end end
                end
            end
        end
    end
    for n,_ in pairs(espObjs) do if not seen[n] then clearESP(n) end end

    local meStun = stateOn(char(),'Stun'); local meRagd = active(attr(char(),'Ragdoll'))
    local busy   = meStun or meRagd

    if state.autoEscape and canEscape() then escapeStep() end
    if state.comboEscape then
        local atk = (busy and (nearestEnemy(28)) or nil) or genuineThreat(nil)
        if atk then doComboEscape(atk) end
    end

    if not busy then
        local sel
        if state.autoCombo then
            if state.glueTarget then
                if comboFocus and validEnemy(comboFocus) then sel=comboFocus else sel=pickTarget(state.engageRange) end
            else sel=pickTarget(comboReach()) end
        end
        comboFocus = sel
        if state.autoBlock then
            local atk,ad = activeAttacker()
            if atk then flagThreat(ad, atk) end
        end
        local punishing = tick()-lastParry < 0.45
        local wantBlock = state.autoBlock and not punishing and tick()>=blockFrom and tick()<blockUntil
        if wantBlock then
            holdM1(false)
            if blockThreat and blockThreat.Parent then
                local tr=S(function() return blockThreat:FindFirstChild('HumanoidRootPart') end)
                if tr then faceTarget(tr) end
            end
        end
        setBlock(wantBlock)
        if wantBlock and blockThreat then lastBlockedEnemy=blockThreat; lastBlockedAt=tick() end
        if state.autoPunish and prevBlocking and not blocking and tick()-lastPunish > 0.7 then
            local atk = lastBlockedEnemy
            if atk and validEnemy(atk) and tick()-lastBlockedAt < 1.2 then
                local rr=hrp(); local ar=rr and S(function() return atk:FindFirstChild('HumanoidRootPart') end)
                if rr and ar and (ar.Position-rr.Position).Magnitude <= comboReach()+4 then
                    lastPunish=tick(); runCombo(atk)
                end
            end
        end
        if state.autoCombo and sel and not wantBlock then runCombo(sel) end
        if state.autoAwaken and not active(attr(char(),'InUlt')) then tryAwaken() end
    else setBlock(false) end
    prevBlocking = blocking
end))

----------------------------------------------------------------------
-- TARGET LOCK / ANTI-AFK / RAINBOW / AIM
----------------------------------------------------------------------
track(RunService.RenderStepped:Connect(function()
    if not state.targetLock then return end
    local cam=workspace.CurrentCamera; local tgt=nearestEnemy(150)
    local tr=tgt and S(function() return tgt:FindFirstChild('HumanoidRootPart') end)
    if tr then cam.CFrame=cam.CFrame:Lerp(CFrame.new(cam.CFrame.Position,tr.Position),0.25) end
end))
track(lp.Idled:Connect(function()
    if state.antiAFK then S(function() VIM:SendKeyEvent(true,Enum.KeyCode.Space,false,game); task.wait(0.05); VIM:SendKeyEvent(false,Enum.KeyCode.Space,false,game) end) end
end))
track(RunService.Heartbeat:Connect(function()
    if state.rainbow then
        ACCENT=Color3.fromHSV((tick()*0.15)%1,0.65,1)
        -- rainbow mode: visual only in-game (OvertimeUI does not expose live accent changes)
    end
end))
local function aimHeld()
    local k=state.aimKey; if not k then return false end
    if k.EnumType==Enum.KeyCode then return UIS:IsKeyDown(k) end
    if k.EnumType==Enum.UserInputType then return UIS:IsMouseButtonPressed(k) end
    return false
end
track(RunService.Heartbeat:Connect(function()
    if unloaded then return end
    local want = (state.aimTarget and aimHeld() and nearestEnemy(state.engageRange)~=nil)
              or (tick()<comboLockUntil and comboFocus~=nil and comboFocus.Parent~=nil)
    ensureLock(want and true or false)
end))
track(RunService.Heartbeat:Connect(function()
    if unloaded or not state.dashAssist or blocking then return end
    local ft = (comboFocus and validEnemy(comboFocus)) and comboFocus or nil
    if not ft and state.aimTarget and aimHeld() then ft = nearestEnemy(state.flankRange) end
    if ft and validEnemy(ft) and not genuineThreat(ft) then flankDash(ft) end
end))

track(lp.CharacterAdded:Connect(function() task.wait(0.6); flyBV,flyBG=nil,nil; state.fly=false; lastChar=nil; blocking=false end))

----------------------------------------------------------------------
-- UNLOAD
----------------------------------------------------------------------
_G.__JJS_UNLOAD__=function()
    unloaded=true
    for k,v in pairs(state) do if type(v)=='boolean' then state[k]=false end end
    for _,c in ipairs(conns) do S(function() c:Disconnect() end) end
    for k,_ in pairs(heldKeys) do setHold(k,false) end
    setBlock(false); holdM1(false); ensureLock(false)
    stopFly()
    local h=hum()
    if h then h.PlatformStand=false; S(function() h.WalkSpeed=ORIG.walkSpeed end); S(function() h.JumpPower=ORIG.jumpPower end) end
    local c=char()
    if c then for _,p in ipairs(c:GetDescendants()) do
        if p:IsA('BasePart') and p.Name~='HumanoidRootPart' and not p:FindFirstAncestorWhichIsA('Accessory') then
            S(function() p.CanCollide=true end)
        end
    end end
    for n,_ in pairs(espObjs) do clearESP(n) end; espObjs={}
    local chs=workspace:FindFirstChild('Characters')
    if chs then for _,d in ipairs(chs:GetDescendants()) do
        if d.Name=='__JJSESP__' or d.Name=='__JJSHL__' then S(function() d:Destroy() end) end
    end end
    S(function()
        Lighting.Brightness=ORIG.brightness; Lighting.ClockTime=ORIG.clockTime
        Lighting.FogEnd=ORIG.fogEnd; Lighting.GlobalShadows=ORIG.globalShadows
    end)
    S(function() workspace.CurrentCamera.FieldOfView=ORIG.fov end)
    if _G.__JJS_WATCH__ and _G.__JJS_WATCH__.conns then
        for _,wc in ipairs(_G.__JJS_WATCH__.conns) do S(function() wc:Disconnect() end) end
    end
    _G.__JJS_WATCH__=nil
    S(function() Window:Destroy() end)
    _G.__JJS_UNLOAD__=nil
end

_G.__JJS__ = {
    state = state,
    diag = function()
        local r = hrp(); local focus = comboFocus
        local fdist = (focus and r and focus:FindFirstChild('HumanoidRootPart'))
            and math.floor((focus.HumanoidRootPart.Position - r.Position).Magnitude) or nil
        local fhp = (focus and focus:FindFirstChildOfClass('Humanoid')) and math.floor(focus.Humanoid.Health) or nil
        return {
            autoCombo=state.autoCombo, autoBlock=state.autoBlock, skillCalls=_G.__JJS_SKC__ or 0,
            comboLock=comboLock, meStun=stateOn(char(),'Stun'), meRagd=active(attr(char(),'Ragdoll')),
            comboFocus=focus and focus.Name or nil, focusDist=fdist, focusHP=fhp, engageRange=state.engageRange,
            focusBlock=focus and (stateOn(focus,'Block') or active(attr(focus,'BlockTry'))) or nil,
            focusIFrame=focus and stateOn(focus,'IFrame') or nil, focusStun=focus and stateOn(focus,'Stun') or nil,
            myName=myName(), preset=COMBO_PRESETS[state.comboPreset], approach=APPROACH_MODES[state.approachMode],
            currentM1=char() and char():GetAttribute('CurrentM1') or nil,
            parries=_G.__JJS_PARRIES__ or 0, perfectBlock=state.perfectBlock, autoPunish=state.autoPunish,
            blocking=blocking, blockLatched=myBlockOn(),
            blockMethod=(BlockActivated and 'BlockService.Activated:FireServer' or 'VIM hold-F (fallback)'),
            lastBlocked=lastBlockedEnemy and lastBlockedEnemy.Name or nil,
            dashReady=dashReady(), evade=char() and char():GetAttribute('Evade') or nil, dashAssist=state.dashAssist,
            canEscape=canEscape(), blockUntil=math.max(0, blockUntil-tick()),
            blockWindowIn=(blockFrom==math.huge) and nil or math.max(0, blockFrom-tick()),
            blockThreat=blockThreat and blockThreat.Name or nil, parryM1=state.parryM1,
            parryHold=state.parryHold, rangeLead=state.rangeLead, parryMargin=state.parryMargin,
            readyKeys=(function() local t={} for _,m in ipairs(liveMoveset()) do if m.ready then t[#t+1]=m.key end end return table.concat(t,',') end)(),
        }
    end,
    forceCombo  = function() local sel=pickTarget(comboReach()); if sel then runCombo(sel) end return sel and sel.Name end,
    ranges      = function() local t={m1=M1_RANGE} for _,m in ipairs(liveMoveset()) do t[m.name]={key=m.key,tip=m.tip,range=moveRange(m),effect=moveEffect(m),inDB=(MOVE_DB[m.name]~=nil)} end t.reach=comboReach() return t end,
    dbCoverage  = function()
        local LD=S(function() return require(RS.Modules.ListData) end); if not LD then return 'no ListData' end
        local missing={}; local total,have=0,0
        for _,ch in ipairs(LD.MoveList) do
            if type(ch)=='table' then
                local cn=(type(ch[1])=='table' and ch[1][1]) or '?'
                for i=2,9 do local mv=ch[i]; if type(mv)=='string' then total=total+1
                    if MOVE_DB[mv] then have=have+1 else missing[#missing+1]=cn..': '..mv end end end
            end
        end
        return {total=total, have=have, missing=missing}
    end,
    testFire    = function(name) return fireAction(name) end,
    combatReady = (COMBAT~=nil), gcOK=hasGC,
    comboErr    = function() return _G.__JJS_COMBOERR__ end,
}

return {script='JujutsuShenanigans', status='deployed', version=VERSION,
    characters=23,
    activation='getconnections(InputAction.Pressed) -- verified to drive skills (VIM does not)',
    comboRoutes={'Gojo','Itadori','Hakari'},
    comboHints='reads each move in-game Tip (USE TWICE/HOLD/USE AGAIN/SPECIAL) live - works for all 23 chars',
    features={'RealActivation','RealCooldowns','HintAwareCombo','ComboRoutes','OvertimeUI','LiveSkillsHUD',
              'DataDrivenCombo','ComboPresets','AutoApproach','FaceTarget','RangeLearning','AutoTheme',
              'ESP','Chams','AutoCombo(reactive+wakeup-extend)','AutoBlock(remote)','AutoCounter(parry)',
              'AutoPunish','ParryDetect','SmartDodge(real-dash)','DashAssist(flank-behind)',
              'EvadeGatedEscape','ComboEscape(dash-out+punish)','AntiStun','AntiRagdoll',
              'AutoBlackFlash(Itadori)','EventDrivenDefense','DistanceScaledParry','AutoAwaken',
              'KillAura','Fly','Noclip','InfJump','WalkSpeed','JumpPower','TargetLock','Fullbright',
              'AntiAFK','Rainbow','Rejoin','Unload'}}

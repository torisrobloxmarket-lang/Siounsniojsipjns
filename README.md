--Decompiled with the Synapse X Decompiler.

if env_flag and getgenv().loaded then
    if guard_508(7715) ~= 37 then
        while true do
        end;
    end;
    warn("script already loaded");
    return ;
end;
getgenv().loaded = true;
local frame_class = "Frame"
local frame_class = "Frame"
local guard_502 = function(n) return n end
local guard_503 = 1353
local guard_506 = function(n) return n end
local guard_507 = function(n) return n end
local guard_508 = function(n) return n end
repeat
    if guard_506(3972) ~= 12156 then
        while true do
        end;
    end;
    task.wait();
until game:IsLoaded();
remove_adonis_anticheat = function()
    pcall(function()
        for index, descendant in ipairs(game:GetDescendants()) do
            if descendant.Name:lower():match("adonis") or descendant.Name == "__FUNCTION" or descendant.Name:match("ClientMover") then
                descendant:Destroy();
            end;
        end;
        return ;
    end);
    pcall(function()
        for nil_index, nil_instance in ipairs(getnilinstances()) do
            if nil_instance:IsA("RemoteEvent") or nil_instance:IsA("RemoteFunction") or nil_instance.Name:match("ClientMover") or nil_instance.Name == "__FUNCTION" then
                if guard_508(7972) ~= 564 then
                    while true do
                    end;
                end;
                nil_instance:Destroy();
            end;
        end;
        return ;
    end);
    pcall(function()
        if guard_503 + 413 ~= 1766 then
            while true do
            end;
        end;
        local original_fire_server = nil;
        original_fire_server = hookfunction(Instance.new("RemoteEvent").FireServer, (interpreter_proxy[1][34][5](function(remote, ...)
            if guard_506(7511) == 27263 then
                local args = { ... };
                if typeof(args[1]) == "table" and args[1].Mode == "Get" then
                    return ;
                end;
                return original_fire_server(remote, ...);
            end;
            while true do
            end;
        end)));
        return ;
    end);
    return true;
end;
remove_adonis_anticheat();
local show_loader = function()
    local tween_service = game:GetService("TweenService");
    local lighting = game:GetService("Lighting");
    if guard_503 + 396 ~= 1749 then
        while true do
        end;
    end;
    local run_service = game:GetService("RunService");
    local debris = game:GetService("Debris");
    local core_gui = game:GetService("CoreGui");
    local load_duration = 5;
    local title_text = "FERAL";
    local letter_spacing = 55;
    if guard_503 - 252 ~= 1101 then
        while true do
        end;
    end;
    local pink_color = Color3.fromRGB(255, 182, 193);
    local blue_color = Color3.fromRGB(160, 210, 255);
    local white_color = Color3.fromRGB(255, 255, 255);
    if guard_502 + 301 ~= 2745 then
        while true do
        end;
    end;
    local loader_gui = Instance.new("ScreenGui");
    loader_gui.Name = tostring(math.random(100000, 999999));
    loader_gui.IgnoreGuiInset = true;
    loader_gui.ResetOnSpawn = false;
    pcall(function()
        if guard_507(8998) >= 3248 and guard_507(8998) <= 3248 then
            loader_gui.Parent = core_gui;
            return ;
        end;
        while true do
        end;
    end);
    local blur_effect = Instance.new("BlurEffect");
    blur_effect.Name = tostring(math.random());
    blur_effect.Size = 0;
    blur_effect.Parent = lighting;
    local color_correction = Instance.new("ColorCorrectionEffect");
    color_correction.Name = tostring(math.random());
    color_correction.Saturation = 0;
    color_correction.TintColor = Color3.fromRGB(255, 255, 255);
    color_correction.Parent = lighting;
    local background_frame = Instance.new(frame_class);
    background_frame.Name = tostring(math.random());
    background_frame.Size = UDim2.new(1, 0, 1, 0);
    background_frame.BackgroundColor3 = Color3.fromRGB(255, 245, 250);
    background_frame.BackgroundTransparency = 1;
    background_frame.ZIndex = 0;
    background_frame.Parent = loader_gui;
    if guard_502 - 471 ~= 1973 then
        while true do
        end;
    end;
    local letter_container = Instance.new(frame_class);
    letter_container.Name = tostring(math.random());
    letter_container.Size = UDim2.new(0, #title_text * letter_spacing, 0, 100);
    letter_container.Position = UDim2.new(0.5, 0, 0.5, -30);
    letter_container.AnchorPoint = Vector2.new(0.5, 0.5);
    letter_container.BackgroundTransparency = 1;
    letter_container.Parent = loader_gui;
    local particle_container = Instance.new(frame_class);
    particle_container.Name = tostring(math.random());
    particle_container.Size = UDim2.new(1, 0, 1, 0);
    particle_container.BackgroundTransparency = 1;
    particle_container.Parent = loader_gui;
    local create_letter = function(char_2, char_index, char_count)
        local letter_holder = Instance.new(frame_class);
        letter_holder.Name = tostring(math.random());
        letter_holder.Size = UDim2.new(0, 60, 0, 80);
        if guard_502 - 2421 ~= 23 then
            while true do
            end;
        end;
        local x_offset = -(char_count * letter_spacing / 2) + letter_spacing / 2 + (char_index - 1) * letter_spacing;
        letter_holder.Position = UDim2.new(0.5, x_offset, 0.5, 50);
        letter_holder.AnchorPoint = Vector2.new(0.5, 0.5);
        letter_holder.BackgroundTransparency = 1;
        letter_holder.Parent = letter_container;
        local letter_font = Enum.Font.FredokaOne;
        if guard_502 + 2027 ~= 4471 then
            while true do
            end;
        end;
        local letter_size = 64;
        local shadow_label = Instance.new("TextLabel");
        shadow_label.Name = tostring(math.random());
        shadow_label.Text = char_2;
        shadow_label.Font = letter_font;
        shadow_label.TextSize = letter_size;
        shadow_label.TextColor3 = Color3.fromRGB(180, 140, 200);
        shadow_label.BackgroundTransparency = 1;
        shadow_label.Size = UDim2.new(1, 0, 1, 0);
        shadow_label.Position = UDim2.new(0, 3, 0, 3);
        shadow_label.TextTransparency = 1;
        shadow_label.ZIndex = 1;
        shadow_label.Parent = letter_holder;
        if guard_502 + 2250 ~= 4694 then
            while true do
            end;
        end;
        local main_label = Instance.new("TextLabel");
        main_label.Name = tostring(math.random());
        main_label.Text = char_2;
        main_label.Font = letter_font;
        main_label.TextSize = letter_size;
        main_label.TextColor3 = white_color;
        main_label.BackgroundTransparency = 1;
        main_label.Size = UDim2.new(1, 0, 1, 0);
        main_label.Position = UDim2.new(0, 0, 0, 0);
        main_label.TextTransparency = 1;
        main_label.ZIndex = 2;
        main_label.Parent = letter_holder;
        local letter_stroke = Instance.new("UIStroke");
        letter_stroke.Name = tostring(math.random());
        letter_stroke.Thickness = 3;
        letter_stroke.Color = pink_color;
        letter_stroke.Transparency = 1;
        letter_stroke.Parent = main_label;
        local stroke_gradient = Instance.new("UIGradient");
        stroke_gradient.Name = tostring(math.random());
        stroke_gradient.Color = ColorSequence.new({ ColorSequenceKeypoint.new(0, pink_color), ColorSequenceKeypoint.new(1, blue_color) });
        stroke_gradient.Rotation = 45;
        stroke_gradient.Parent = letter_stroke;
        return { holder = letter_holder, shadow = shadow_label, main = main_label, stroke = letter_stroke };
    end;
    local spawn_particle = function()
        local particle = Instance.new("ImageLabel");
        particle.Name = tostring(math.random());
        local particle_size = math.random(15, 25);
        particle.Size = UDim2.new(0, particle_size, 0, particle_size);
        particle.Position = UDim2.new(math.random(20, 80) / 100, 0, 1.1, 0);
        particle.BackgroundTransparency = 1;
        particle.Image = "rbxassetid://243097862";
        particle.ImageColor3 = math.random() > 0.5 and pink_color or blue_color;
        particle.ImageTransparency = 0.3;
        particle.Parent = particle_container;
        local particle_lifetime = math.random(3, 5);
        tween_service:Create(particle, TweenInfo.new(particle_lifetime, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), { Position = UDim2.new(particle.Position.X.Scale, math.random(-50, 50), -0.1, 0), ImageTransparency = 1, Rotation = math.random(-30, 30) }):Play();
        debris:AddItem(particle, particle_lifetime);
        return ;
    end;
    task.spawn(function()
        if guard_506(8481) <= 29073 and guard_506(8481) <= 29073 then
            local progress_bar = Instance.new(frame_class);
            progress_bar.Name = tostring(math.random());
            progress_bar.Size = UDim2.new(0, 220, 0, 6);
            progress_bar.Position = UDim2.new(0.5, 0, 0.5, 60);
            progress_bar.AnchorPoint = Vector2.new(0.5, 0.5);
            progress_bar.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
            progress_bar.BackgroundTransparency = 1;
            progress_bar.BorderSizePixel = 0;
            progress_bar.Parent = loader_gui;
            local bar_corner = Instance.new("UICorner");
            bar_corner.Name = tostring(math.random());
            bar_corner.CornerRadius = UDim.new(1, 0);
            bar_corner.Parent = progress_bar;
            local bar_fill = Instance.new(frame_class);
            bar_fill.Name = tostring(math.random());
            bar_fill.Size = UDim2.new(0, 0, 1, 0);
            bar_fill.BackgroundColor3 = pink_color;
            bar_fill.BorderSizePixel = 0;
            bar_fill.Parent = progress_bar;
            local fill_corner = Instance.new("UICorner");
            fill_corner.Name = tostring(math.random());
            fill_corner.CornerRadius = UDim.new(1, 0);
            fill_corner.Parent = bar_fill;
            local fill_gradient = Instance.new("UIGradient");
            fill_gradient.Name = tostring(math.random());
            fill_gradient.Color = ColorSequence.new({ ColorSequenceKeypoint.new(0, pink_color), ColorSequenceKeypoint.new(1, blue_color) });
            fill_gradient.Parent = bar_fill;
            if guard_503 - 623 ~= 730 then
                while true do
                end;
            end;
            local status_label = Instance.new("TextLabel");
            status_label.Name = tostring(math.random());
            status_label.Text = "initializing...";
            status_label.Font = Enum.Font.GothamMedium;
            status_label.TextSize = 14;
            status_label.TextColor3 = Color3.fromRGB(255, 255, 255);
            status_label.TextTransparency = 1;
            status_label.Size = UDim2.new(0, 200, 0, 20);
            status_label.Position = UDim2.new(0.5, 0, 0.5, 80);
            status_label.AnchorPoint = Vector2.new(0.5, 0.5);
            status_label.BackgroundTransparency = 1;
            status_label.Parent = loader_gui;
            local status_stroke = Instance.new("UIStroke");
            status_stroke.Name = tostring(math.random());
            status_stroke.Thickness = 1;
            status_stroke.Color = Color3.fromRGB(200, 150, 180);
            status_stroke.Transparency = 1;
            status_stroke.Parent = status_label;
            tween_service:Create(blur_effect, TweenInfo.new(1.5), { Size = 24 }):Play();
            tween_service:Create(background_frame, TweenInfo.new(1.5), { BackgroundTransparency = 0.8 }):Play();
            local letters = {};
            for i = 1, #title_text, 1 do
                local letter_char = string.sub(title_text, i, i);
                table.insert(letters, create_letter(letter_char, i, #title_text));
            end;
            for letter_index, letter in ipairs(letters) do
                task.wait(0.1);
                tween_service:Create(letter.main, TweenInfo.new(1), { TextTransparency = 0 }):Play();
                tween_service:Create(letter.shadow, TweenInfo.new(1), { TextTransparency = 0.5 }):Play();
                tween_service:Create(letter.stroke, TweenInfo.new(1), { Transparency = 0 }):Play();
                tween_service:Create(letter.holder, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), { Position = UDim2.new(letter.holder.Position.X.Scale, letter.holder.Position.X.Offset, 0.5, 0) }):Play();
            end;
            task.wait(0.5);
            tween_service:Create(progress_bar, TweenInfo.new(1), { BackgroundTransparency = 0.7 }):Play();
            tween_service:Create(status_label, TweenInfo.new(1), { TextTransparency = 0 }):Play();
            tween_service:Create(status_stroke, TweenInfo.new(1), { Transparency = 0.5 }):Play();
            tween_service:Create(bar_fill, TweenInfo.new(load_duration, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut), { Size = UDim2.new(1, 0, 1, 0) }):Play();
            local render_connection = run_service.RenderStepped:Connect(function()
                if math.random() > 0.92 then
                    spawn_particle();
                end;
                local clock_2 = os.clock();
                for wave_index, wave_letter in ipairs(letters) do
                    local wave_offset = math.sin(clock_2 * 3 + wave_index * 0.5) * 6;
                    wave_letter.holder.Position = UDim2.new(wave_letter.holder.Position.X.Scale, wave_letter.holder.Position.X.Offset, 0.5, wave_offset);
                end;
                return ;
            end);
            task.wait(load_duration * 0.3);
            status_label.Text = "bypassing...";
            task.wait(load_duration * 0.4);
            status_label.Text = "loading assets...";
            task.wait(load_duration * 0.2);
            status_label.Text = "success! <3";
            task.wait(0.5);
            render_connection:Disconnect();
            local fade_out_info = TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.In);
            for fade_index, fade_letter in ipairs(letters) do
                tween_service:Create(fade_letter.holder, fade_out_info, { Position = UDim2.new(fade_letter.holder.Position.X.Scale, fade_letter.holder.Position.X.Offset, 0.5, -100) }):Play();
                tween_service:Create(fade_letter.main, fade_out_info, { TextTransparency = 1 }):Play();
                tween_service:Create(fade_letter.shadow, fade_out_info, { TextTransparency = 1 }):Play();
                tween_service:Create(fade_letter.stroke, fade_out_info, { Transparency = 1 }):Play();
                task.wait(0.05);
            end;
            tween_service:Create(progress_bar, fade_out_info, { BackgroundTransparency = 1 }):Play();
            tween_service:Create(bar_fill, fade_out_info, { BackgroundTransparency = 1 }):Play();
            tween_service:Create(status_label, fade_out_info, { TextTransparency = 1 }):Play();
            tween_service:Create(status_stroke, fade_out_info, { Transparency = 1 }):Play();
            tween_service:Create(blur_effect, TweenInfo.new(1.5), { Size = 0 }):Play();
            tween_service:Create(background_frame, TweenInfo.new(1.5), { BackgroundTransparency = 1 }):Play();
            task.wait(1.5);
            loader_gui:Destroy();
            blur_effect:Destroy();
            color_correction:Destroy();
            return ;
        end;
        while true do
        end;
    end);
    return ;
end;
getgenv().loaded_anticheat_bypass = false;
task.spawn(pcall, function()
    if guard_507(8313) == 688 then
        if game.PlaceId ~= 1730877806 then
            if not (guard_508(1682) <= 8) or not (guard_508(1682) <= 8) then
                while true do
                end;
            end;
            local bypass_started = nil;
            repeat
                task.wait();
            until game:GetService("ReplicatedFirst"):FindFirstChild("paul greyrat");
            repeat
                task.wait();
            until #game:GetService("ReplicatedFirst")["paul greyrat"]:GetChildren() < 1;
            task.wait();
            show_loader();
            if game:GetService("ReplicatedFirst"):FindFirstChild("paul greyrat") then
                if guard_506(3240) ~= 11520 then
                    while true do
                    end;
                end;
                bypass_started = true;
                run_on_actor(game:GetService("ReplicatedFirst")["paul greyrat"], "\t\t\t\t\t\tlocal bypassed = true\n\t\t\t\t\t\tlocal Context = game:GetService('ScriptContext')\n\t\t\t\t\t\tfor i,v in next, getconnections(Context.Error) do \n\t\t\t\t\t\t\tif v.Function and debug.getinfo(v.Function).nups > 1 then \n\t\t\t\t\t\t\t\thookfunction(v.Function,function() end)\n\t\t\t\t\t\t\t\tbypassed = false\n\t\t\t\t\t\t\tend\n\t\t\t\t\t\tend\n\t\t\t\t\t\tif bypassed and game.PlaceId ~= 1730877806 then\n\t\t\t\t\t\t\tgame.Players.LocalPlayer:Kick('bypass failed')\n\t\t\t\t\t\tend\n\t\t\t\t\t");
            end;
            if not bypass_started and game.PlaceId ~= 1730877806 then
                game.Players.LocalPlayer:Kick("failed to load ac bypass please contact texchy or try rejoining");
            else
                task.wait(7);
                getgenv().loaded_anticheat_bypass = true;
            end;
        end;
        return ;
    end;
    while true do
    end;
end);
if env_flag and game.PlaceId ~= 1730877806 then
    repeat
        task.wait();
    until getgenv().loaded_anticheat_bypass;
end;
loadstring("\t\tfunction LPH_NO_UPVALUES(f) return f end;\n\t\t")();
loadstring("\t\tfunction LPH_JIT(f) return f end;\n\t\t")();
getgenv().script_key = script_key;
local http_service = game:GetService("HttpService");
local callbacks = {};
local connections = {};
local binds = {};
local element_registry = { toggles = {}, sliders = {}, dropdowns = {}, keybinds = {}, Boxes = {} };
local make_element_id = function(tab_name, section_name, element_name)
    if guard_507(6015) == 6640 then
        return tostring(tab_name) .. "||" .. tostring(section_name) .. "||" .. tostring(element_name);
    end;
    while true do
    end;
end;
requirements_tracker = {
    user_has = {},
    presets = {
        geppo = function()
            return requirements_tracker.user_has.geppo or false;
        end
    },
    active_connections = {}
};
requirements_tracker.update = function(self_value, key, value)
    self_value.user_has[key] = value;
    return ;
end;
requirements_tracker.check = function(self_value_2, preset_name)
    if self_value_2.presets[preset_name] then
        return self_value_2.presets[preset_name]();
    end;
    return false;
end;
requirements_tracker.add_preset = function(self_value_3, preset_name_2, preset_fn)
    self_value_3.presets[preset_name_2] = preset_fn;
    return ;
end;
get_ui = function()
    if getgenv().tvk and game.CoreGui:FindFirstChild("Feral GUI") then
        for gui_index, gui_child in ipairs(game.CoreGui:GetChildren()) do
            if gui_child.Name == "Feral GUI" then
                gui_child:Destroy();
            end;
        end;
    end;
    getgenv().tvk = true;
    getgenv().chon = true;
    local light_theme = { ["Border Color"] = Color3.fromRGB(131, 181, 255), ["Click Effect Color"] = Color3.fromRGB(230, 230, 230), ["Setting Icon Color"] = Color3.fromRGB(230, 230, 230), ["Logo Image"] = "rbxassetid://6248942117", ["Search Icon Color"] = Color3.fromRGB(255, 255, 255), ["Search Icon Highlight Color"] = Color3.fromRGB(131, 181, 255), ["GUI Text Color"] = Color3.fromRGB(230, 230, 230), [text_color_key] = Color3.fromRGB(230, 230, 230), ["Placeholder Text Color"] = Color3.fromRGB(178, 178, 178), ["Title Text Color"] = Color3.fromRGB(131, 181, 255), ["Background 1 Color"] = Color3.fromRGB(43, 43, 43), ["Background 1 Transparency"] = 0, ["Background 2 Color"] = Color3.fromRGB(90, 90, 90), ["Background 3 Color"] = Color3.fromRGB(53, 53, 53), ["Background Image"] = "", ["Page Selected Color"] = Color3.fromRGB(131, 181, 255), ["Section Text Color"] = Color3.fromRGB(131, 181, 255), ["Section Underline Color"] = Color3.fromRGB(131, 181, 255), ["Toggle Border Color"] = Color3.fromRGB(131, 181, 255), ["Toggle Checked Color"] = Color3.fromRGB(230, 230, 230), ["Toggle Desc Color"] = Color3.fromRGB(185, 185, 185), ["Button Color"] = Color3.fromRGB(131, 181, 255), ["Label Color"] = Color3.fromRGB(101, 152, 220), ["Dropdown Icon Color"] = Color3.fromRGB(230, 230, 230), ["Dropdown Selected Color"] = Color3.fromRGB(131, 181, 255), ["Textbox Highlight Color"] = Color3.fromRGB(131, 181, 255), ["Box Highlight Color"] = Color3.fromRGB(131, 181, 255), ["Slider Line Color"] = Color3.fromRGB(75, 75, 75), ["Slider Highlight Color"] = Color3.fromRGB(59, 82, 115), ["Tween Animation 1 Speed"] = 0.25, ["Tween Animation 2 Speed"] = 0.5, ["Tween Animation 3 Speed"] = 0.1 };
    local theme = { ["Border Color"] = Color3.fromRGB(40, 40, 40), ["Click Effect Color"] = Color3.fromRGB(60, 60, 60), ["Setting Icon Color"] = Color3.fromRGB(200, 200, 200), ["Logo Image"] = "rbxassetid://9327507243", ["Search Icon Color"] = Color3.fromRGB(200, 200, 200), ["Search Icon Highlight Color"] = Color3.fromRGB(90, 160, 255), ["GUI Text Color"] = Color3.fromRGB(220, 220, 220), [text_color_key] = Color3.fromRGB(220, 220, 220), ["Placeholder Text Color"] = Color3.fromRGB(150, 150, 150), ["Title Text Color"] = Color3.fromRGB(90, 160, 255), ["Background Main Color"] = Color3.fromRGB(20, 20, 20), ["Background 1 Color"] = Color3.fromRGB(30, 30, 30), ["Background 1 Transparency"] = 0, ["Background 2 Color"] = Color3.fromRGB(45, 45, 45), ["Background 3 Color"] = Color3.fromRGB(25, 25, 25), ["Background Image"] = "", ["Page Selected Color"] = Color3.fromRGB(90, 160, 255), ["Section Text Color"] = Color3.fromRGB(90, 160, 255), ["Section Underline Color"] = Color3.fromRGB(90, 160, 255), ["Toggle Border Color"] = Color3.fromRGB(90, 160, 255), ["Toggle Checked Color"] = Color3.fromRGB(220, 220, 220), ["Toggle Desc Color"] = Color3.fromRGB(180, 180, 180), ["Button Color"] = Color3.fromRGB(90, 160, 255), ["Label Color"] = Color3.fromRGB(90, 160, 255), ["Dropdown Icon Color"] = Color3.fromRGB(200, 200, 200), ["Dropdown Selected Color"] = Color3.fromRGB(90, 160, 255), ["Textbox Highlight Color"] = Color3.fromRGB(90, 160, 255), ["Box Highlight Color"] = Color3.fromRGB(90, 160, 255), ["Slider Line Color"] = Color3.fromRGB(60, 60, 60), ["Slider Highlight Color"] = Color3.fromRGB(70, 130, 200), ["Tween Animation 1 Speed"] = 0.25, ["Tween Animation 2 Speed"] = 0.5, ["Tween Animation 3 Speed"] = 0.1 };
    local theme_listeners = {};
    for theme_key, theme_value in pairs(theme) do
        if not (guard_506(9057) >= 28761) or not (guard_506(9057) >= 28761) then
            while true do
            end;
        end;
        theme_listeners[theme_key] = {};
    end;
    if guard_502 + 2287 ~= 4731 then
        while true do
        end;
    end;
    local custom_theme = {};
    for theme_key_2, theme_color in pairs(theme) do
        custom_theme[theme_key_2] = { Color = theme_color, Rainbow = false, Breathing = { Toggle = false, Color1 = Color3.new(), Color2 = Color3.new() } };
    end;
    local color_to_table = function(color_2)
        return { math.floor(color_2.r * 255), math.floor(color_2.g * 255), math.floor(color_2.b * 255), "Dit" };
    end;
    encode_theme = function(input_table)
        local output_table = {};
        for key_2, value_2 in pairs(input_table) do
            if guard_507(4755) ~= 2880 then
                while true do
                end;
            end;
            if typeof(value_2) == "Color3" then
                output_table[key_2] = color_to_table(value_2);
            elseif type(value_2) == "table" then
                output_table[key_2] = encode_theme(value_2);
            else
                output_table[key_2] = value_2;
            end;
        end;
        return output_table;
    end;
    decode_theme = function(input_table_2)
        local output_table_2 = {};
        for key_3, value_3 in pairs(input_table_2) do
            if type(value_3) == "table" and value_3[4] == "Dit" then
                output_table_2[key_3] = Color3.fromRGB(unpack(value_3));
            elseif type(value_3) == "table" then
                output_table_2[key_3] = decode_theme(value_3);
            else
                output_table_2[key_3] = value_3;
            end;
        end;
        return output_table_2;
    end;
    local http_service_2 = game:GetService("HttpService");
    local ui_settings_file = "!CustomUI.json";
    save_ui_settings = function()
        local http_service_3 = game:GetService("HttpService");
        if not isfolder("Feral") then
            makefolder("Feral");
        end;
        writefile("Feral/" .. ui_settings_file, http_service_3:JSONEncode(encode_theme(custom_theme)));
        return ;
    end;
    read_ui_settings = function()
        local read_ok, read_result = pcall(function()
            local http_service_4 = game:GetService("HttpService");
            if not isfolder("Feral") then
                makefolder("Feral");
            end;
            if guard_502 + 489 ~= 2933 then
                while true do
                end;
            end;
            local saved_theme = http_service_4:JSONDecode(readfile("Feral/" .. ui_settings_file));
            for theme_key_3, theme_entry in pairs(saved_theme) do
                if not (function()
                    if theme_entry.Color == nil then
                        return ;
                    end;
                    if theme_entry.Rainbow == nil then
                        return ;
                    end;
                    if theme_entry.Breathing == nil then
                        return ;
                    end;
                    if theme_entry.Breathing.Color1 == nil then
                        return ;
                    end;
                    if theme_entry.Breathing.Color2 == nil then
                        return ;
                    end;
                    return true;
                end)() then
                    save_ui_settings();
                    return read_ui_settings();
                end;
            end;
            return saved_theme;
        end);
        if read_ok then
            return read_result;
        end;
        if guard_508(1761) ~= 332 then
            while true do
            end;
        end;
        save_ui_settings();
        return read_ui_settings();
    end;
    local config_manager = {};
    if guard_503 + 1249 ~= 2602 then
        while true do
        end;
    end;
    local config_folder = "Feral/Configs";
    if guard_502 - 683 ~= 1761 then
        while true do
        end;
    end;
    local ensure_folders = function()
        if not isfolder("Feral") then
            makefolder("Feral");
        end;
        if env_ok and not isfolder(config_folder) then
            makefolder(config_folder);
        end;
        return ;
    end;
    config_manager.List = function()
        ensure_folders();
        local config_files = listfiles(config_folder);
        local config_names = {};
        for file_index, file_path in ipairs(config_files) do
            local config_name = file_path:match(".+[/\\](.+)%.json$");
            if config_name then
                table.insert(config_names, config_name);
            end;
        end;
        return config_names;
    end;
    config_manager.save = function(config_name_2)
        if not config_name_2 or config_name_2 == "" then
            return false, "No config name";
        end;
        ensure_folders();
        local config_data = { toggles = {}, sliders = {}, dropdowns = {}, keybinds = {}, Boxes = {}, UITheme = encode_theme(custom_theme) };
        for toggle_id, toggle in pairs(element_registry.toggles) do
            local get_ok, get_value = pcall(toggle.get);
            if get_ok then
                if guard_508(3904) ~= 130 then
                    while true do
                    end;
                end;
                config_data.toggles[toggle_id] = get_value;
            end;
        end;
        for slider_id, slider in pairs(element_registry.sliders) do
            local get_ok_2, get_value_2 = pcall(slider.get);
            if get_ok_2 then
                config_data.sliders[slider_id] = get_value_2;
            end;
        end;
        for dropdown_id, dropdown in pairs(element_registry.dropdowns) do
            if guard_507(6966) ~= 9216 then
                while true do
                end;
            end;
            local get_ok_3, get_value_3 = pcall(dropdown.get);
            if get_ok_3 then
                config_data.dropdowns[dropdown_id] = get_value_3;
            end;
        end;
        for keybind_id, keybind in pairs(element_registry.keybinds) do
            local get_ok_4, get_value_4 = pcall(keybind.get);
            if env_ok and get_ok_4 then
                config_data.keybinds[keybind_id] = get_value_4;
            end;
        end;
        for box_id, box in pairs(element_registry.Boxes) do
            if guard_503 + 704 ~= 2057 then
                while true do
                end;
            end;
            local get_ok_5, get_value_5 = pcall(box.get);
            if get_ok_5 then
                config_data.Boxes[box_id] = get_value_5;
            end;
        end;
        if guard_503 - 448 ~= 905 then
            while true do
            end;
        end;
        local save_ok, save_err = pcall(function()
            writefile(config_folder .. "/" .. config_name_2 .. ".json", game.HttpService:JSONEncode(config_data));
            return ;
        end);
        return save_ok, save_err;
    end;
    config_manager.load = function(config_name_3)
        if not config_name_3 or config_name_3 == "" then
            if guard_508(3995) ~= 85 then
                while true do
                end;
            end;
            return false, "No config name";
        end;
        ensure_folders();
        if guard_502 + 1775 ~= 4219 then
            while true do
            end;
        end;
        local config_path = config_folder .. "/" .. config_name_3 .. ".json";
        if env_ok and not isfile(config_path) then
            return false, "Config not found";
        end;
        local config_raw = readfile(config_path);
        local decode_ok, config_data_2 = pcall(function()
            return game.HttpService:JSONDecode(config_raw);
        end);
        if env_flag and (not decode_ok or type(config_data_2) ~= "table") then
            return false, "Invalid config data";
        end;
        local apply_config_values = function(element_type, registry, saved_values)
            if not saved_values then
                return ;
            end;
            for element_id, saved_value in pairs(saved_values) do
                local element = registry and registry[element_id];
                if element and element.set then
                    if not (guard_508(3356) >= 234) or not (guard_508(3356) >= 234) then
                        while true do
                        end;
                    end;
                    task.spawn(function()
                        local set_ok, set_err = pcall(element.set, saved_value);
                        if not set_ok then
                            warn("[Config]", element_type, "Set failed for id:", element_id, set_err);
                        end;
                        return ;
                    end);
                end;
            end;
            return ;
        end;
        if config_data_2.UITheme then
            task.spawn(function()
                local theme_ok, theme_err = pcall(function()
                    if guard_506(8065) == 26545 then
                        local decoded_theme = decode_theme(config_data_2.UITheme);
                        for theme_key_4, theme_entry_2 in pairs(decoded_theme) do
                            if type(theme_entry_2) == "table" and theme_entry_2.Color then
                                task.spawn(function()
                                    if guard_508(1412) >= 170 and guard_508(1412) >= 170 then
                                        local apply_ok, apply_err = pcall(function()
                                            if guard_508(6911) == 66 then
                                                getgenv().ui_color[theme_key_4] = theme_entry_2.Color;
                                                return ;
                                            end;
                                            while true do
                                            end;
                                        end);
                                        if not apply_ok then
                                            warn("[Config] UITheme apply failed for key:", theme_key_4, apply_err);
                                        end;
                                        return ;
                                    end;
                                    while true do
                                    end;
                                end);
                            end;
                        end;
                        return ;
                    end;
                    while true do
                    end;
                end);
                if not theme_ok then
                    if not (guard_507(4768) <= 2768) or not (guard_507(4768) <= 2768) then
                        while true do
                        end;
                    end;
                    warn("[Config] UITheme decoding failed:", theme_err);
                end;
                return ;
            end);
        end;
        task.spawn(function()
            apply_config_values("Toggle", element_registry.toggles, config_data_2.toggles);
            return ;
        end);
        task.spawn(function()
            apply_config_values("Slider", element_registry.sliders, config_data_2.sliders);
            return ;
        end);
        task.spawn(function()
            apply_config_values("Dropdown", element_registry.dropdowns, config_data_2.dropdowns);
            return ;
        end);
        task.spawn(function()
            apply_config_values("Keybind", element_registry.keybinds, config_data_2.keybinds);
            return ;
        end);
        task.spawn(function()
            apply_config_values("Box", element_registry.Boxes, config_data_2.Boxes);
            return ;
        end);
        return true;
    end;
    config_manager.delete = function(config_name_4)
        if not config_name_4 or config_name_4 == "" then
            if guard_507(6121) == 6496 then
                return false, "No config name";
            end;
            while true do
            end;
        end;
        ensure_folders();
        local config_path_2 = config_folder .. "/" .. config_name_4 .. ".json";
        if env_ok and not isfile(config_path_2) then
            return false, "Config not found";
        end;
        local del_ok, del_err = pcall(function()
            delfile(config_path_2);
            return ;
        end);
        return del_ok, del_err;
    end;
    getgenv().feral_config = config_manager;
    if not getgenv().chon then
        if not (guard_506(2773) <= 11269) or not (guard_506(2773) >= 11269) then
            while true do
            end;
        end;
        custom_theme = decode_theme(read_ui_settings());
        for theme_key_5, theme_entry_3 in pairs(custom_theme) do
            theme[theme_key_5] = theme_entry_3.Color;
        end;
    end;
    if not getgenv().ractvkretarddumb then
        if guard_507(9937) ~= 1312 then
            while true do
            end;
        end;
        spawn(function()
            while wait(1) do
                save_ui_settings();
            end;
            return ;
        end);
        getgenv().ractvkretarddumb = true;
    end;
    local ui_color_proxy = setmetatable({}, {
        __newindex = function(tbl_2, key_4, value_4)
            if key_4 == nil then
                warn("[Feral UI] UIColor __newindex got nil key, ignoring.");
                return ;
            end;
            rawset(theme, key_4, value_4);
            local listeners = theme_listeners[key_4];
            if listeners then
                for listener_id, listener in pairs(listeners) do
                    pcall(listener);
                end;
            end;
            if not custom_theme[key_4] then
                if guard_508(8915) ~= 148 then
                    while true do
                    end;
                end;
                custom_theme[key_4] = { Color = value_4, Rainbow = false, Breathing = { Toggle = false, Color1 = Color3.new(), Color2 = Color3.new() } };
            else
                custom_theme[key_4].Color = value_4;
            end;
            return ;
        end,
        __index = theme
    });
    getgenv().ui_color = ui_color_proxy;
    local rainbow_connections = {};
    local breathing_connections = {};
    local ui_library = {};
    local tween_service_2 = game:GetService("TweenService");
    local user_input_service = game:GetService("UserInputService");
    ui_library.button_effect = function()
        return ;
    end;
    ui_library.get_img = function(image_url)
        local temp_file = "SynAsset [";
        local asset_id = "";
        if string.find(image_url, "rbxassetid://") then
            if guard_506(4833) ~= 14529 then
                while true do
                end;
            end;
            asset_id = image_url;
        else
            pcall(function()
                if guard_508(7591) == 39 then
                    if image_url and type(image_url) == "string" and tostring(game:HttpGet(image_url)):find("PNG") then
                        for i_2 = 1, 5, 1 do
                            temp_file = tostring(temp_file .. string.char(math.random(65, 122)));
                        end;
                        temp_file = temp_file .. "].png";
                        writefile(temp_file, game:HttpGet(image_url));
                        spawn(function()
                            wait(5);
                            delfile(temp_file);
                            return ;
                        end);
                        asset_id = getsynasset(temp_file);
                    end;
                    return ;
                end;
                while true do
                end;
            end);
        end;
        return asset_id;
    end;
    ui_library.gui = Instance.new("ScreenGui");
    ui_library.gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
    ui_library.gui.Name = "Feral GUI";
    getgenv().ready_for_gui_loaded = false;
    spawn(function()
        ui_library.gui.Enabled = false;
        repeat
            wait();
        until getgenv().ready_for_gui_loaded;
        ui_library.gui.Enabled = true;
        return ;
    end);
    ui_library.noti_gui = Instance.new("ScreenGui");
    ui_library.noti_gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
    ui_library.noti_gui.Name = "Feral Notification";
    local noti_container = Instance.new(frame_class);
    local noti_list = Instance.new("UIListLayout");
    noti_container.Name = "NotiContainer";
    noti_container.Parent = ui_library.noti_gui;
    noti_container.AnchorPoint = Vector2.new(1, 1);
    noti_container.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
    noti_container.BackgroundTransparency = 1;
    noti_container.Position = UDim2.new(1, -5, 1, -5);
    noti_container.Size = UDim2.new(0, 350, 1, -10);
    noti_list.Name = "NotiList";
    noti_list.Parent = noti_container;
    noti_list.SortOrder = Enum.SortOrder.LayoutOrder;
    noti_list.VerticalAlignment = Enum.VerticalAlignment.Bottom;
    noti_list.Padding = UDim.new(0, 5);
    wait();
    getgenv().gui = ui_library.gui;
    setup_translator = function()
        if guard_506(6683) <= 25499 and guard_506(6683) <= 25499 then
            local target_language = "vi";
            local consent_token = isfile("googlev.txt") and readfile("googlev.txt") or "";
            local http_request_2 = request or syn.request;
            local wrap_logger = function(fn)
                return function(...)
                    return fn("[INLINE TRANSLATOR]", ...);
                end;
            end;
            if guard_508(3837) == 59 then
                google_consent = function(response_body)
                    local consent_fields = {};
                    for input_tag, input_match in response_body:gmatch("<input type=\"hidden\" name=\".-\" value=\".-\">") do
                        local field_name, field_value = input_tag:match("<input type=\"hidden\" name=\"(.-)\" value=\"(.-)\">");
                        consent_fields[field_name] = field_value;
                    end;
                    consent_token = consent_fields.v;
                    writefile("googlev.txt", consent_fields.v);
                    return ;
                end;
                local google_request = function(url, method, body)
                    local method_2 = method or "GET";
                    local response = http_request_2({ Url = url, Method = method_2, Headers = { cookie = "CONSENT=YES+" .. consent_token }, Body = body });
                    if response.Body:match("https://consent.google.com/s") then
                        google_consent(response.Body);
                        response = http_request_2({ Url = url, Method = "GET", Headers = { cookie = "CONSENT=YES+" .. consent_token } });
                    end;
                    return response;
                end;
                getgenv().languages = { auto = "Automatic", ["zh-cn"] = "Chinese Simplified", ["zh-tw"] = "Chinese Traditional", en = "English", fr = "French", de = "German", el = "Greek", hu = "Hungarian", id = "Indonesian", it = "Italian", ja = "Japanese", ko = "Korean", mg = "Malagasy", pl = "Polish", pt = "Portuguese", ru = "Russian", es = "Spanish", tr = "Turkish", vi = "Vietnamese" };
                local language_codes = {};
                for lang_code, lang_name in pairs(languages) do
                    table.insert(language_codes, lang_code);
                end;
                set_random_language = function()
                    target_language = language_codes[math.random(1, #language_codes)];
                    return ;
                end;
                find_language = function(query)
                    if guard_506(7464) <= 25752 and guard_506(7464) <= 25752 then
                        for lang_code_2, lang_name_2 in pairs(languages) do
                            if lang_code_2 == query or lang_name_2 == query then
                                if guard_506(8732) == 28556 then
                                    return lang_code_2;
                                end;
                                while true do
                                end;
                            end;
                        end;
                        return ;
                    end;
                    while true do
                    end;
                end;
                is_language_supported = function(query_2)
                    return find_language(query_2) and true or false;
                end;
                local resolve_language = function(query_3)
                    return (find_language(query_3));
                end;
                stringify_query = function(params)
                    local query_string = "";
                    for param_key, param_value in pairs(params) do
                        if type(param_value) == "table" then
                            for sub_key, sub_value in pairs(param_value) do
                                query_string = query_string .. ("&%s=%s"):format(game.HttpService:UrlEncode(param_key), game.HttpService:UrlEncode(sub_value));
                            end;
                        else
                            query_string = query_string .. ("&%s=%s"):format(game.HttpService:UrlEncode(param_key), game.HttpService:UrlEncode(param_value));
                        end;
                    end;
                    return (query_string:sub(2));
                end;
                local request_id = math.random(1000, 9999);
                local rpc_id = "MkEWBc";
                local translate_url = "https://translate.google.com/_/TranslateWebserverUi/data/batchexecute";
                local session_id = nil;
                local build_label = nil;
                json_encode = function(value_5)
                    return http_service:JSONEncode(value_5);
                end;
                json_decode = function(value_6)
                    return http_service:JSONDecode(value_6);
                end;
                local translate_raw = function(text_2, target_lang, source_lang)
                    request_id = request_id + 10000;
                    local source_lang_2 = source_lang and resolve_language(source_lang) or "auto";
                    local target_lang_2 = target_lang and resolve_language(target_lang) or "en";
                    local payload_2 = { { { rpc_id, json_encode({ { text_2, source_lang_2, target_lang_2, true }, { nil } }), nil, "generic" } } };
                    local request_url = translate_url .. "?" .. stringify_query({ rpcids = rpc_id, ["f.sid"] = session_id, bl = build_label, hl = "en", _reqid = request_id - 10000, rt = "c" });
                    local request_body = stringify_query({ ["f.req"] = json_encode(payload_2) });
                    local response_2 = google_request(request_url, "POST", request_body);
                    local outer_json = json_decode(response_2.Body:match("%[.-%]\n"));
                    local result_json = json_decode(outer_json[1][3]);
                    local result = { text = "", from = { language = "", text = "" }, raw = "", raw = result_json, text = result_json[2][1][1][6][1][1] };
                    result.from.language = result_json[3];
                    result.from.text = result_json[2][5][1];
                    return result;
                end;
                translate_from = function(text_3)
                    local raw_result = translate_raw(text_3, target_language);
                    local translated = nil;
                    if raw_result.from.language ~= target_language then
                        translated = raw_result.text;
                    end;
                    return { translated, raw_result.from.language };
                end;
                translate = function(text_4)
                    local translate_ok, translate_result = pcall(function()
                        return translate_from(text_4)[1];
                    end);
                    if translate_ok then
                        return translate_result;
                    end;
                    return ;
                end;
                return ;
            end;
            while true do
            end;
        end;
        while true do
        end;
    end;
    getgenv().translate_cache = {};
    if not isfolder("Feral") then
        makefolder("Feral");
    end;
    local cache_ok, cache_data = pcall(function()
        return game.HttpService:JSONDecode(readfile("Feral/!UIText.json"));
    end);
    save_translate_cache = function()
        pcall(function()
            writefile("Feral/!UIText.json", game.HttpService:JSONEncode(getgenv().translate_cache));
            return ;
        end);
        return ;
    end;
    is_number = function()
        return ;
    end;
    local ready = false;
    if cache_ok and cache_data then
        getgenv().translate_cache = cache_data;
    end;
    spawn(function()
        local var_2 = tick();
        while wait(2) do
            if tick() - var_2 > 30 then
                if ready then
                    save_translate_cache();
                    ready = false;
                end;
            else
                save_translate_cache();
            end;
        end;
        return ;
    end);
    ui_library.gui.Parent = game:GetService("CoreGui");
    ui_library.noti_gui.Parent = game:GetService("CoreGui");
    ui_library.get_color = function(arg)
        return { math.floor(arg.r * 255), math.floor(arg.g * 255), math.floor(arg.b * 255) };
    end;
    breathing_connections.create_noti = function(arg_2)
        getgenv().title_name_noti = arg_2.title or "";
        local var_3 = arg_2.desc;
        local var_4 = arg_2.show_time or 10;
        local var_5 = Instance.new(frame_class);
        local var_6 = Instance.new(frame_class);
        local var_7 = Instance.new("UICorner");
        local var_8 = Instance.new(frame_class);
        local var_9 = Instance.new("ImageLabel");
        local var_10 = Instance.new("UICorner");
        local var_11 = Instance.new("TextLabel");
        local var_12 = Instance.new(frame_class);
        local var_13 = Instance.new("ImageLabel");
        local var_14 = Instance.new("TextButton");
        local var_15 = Instance.new("TextLabel");
        var_5.Name = "NotiFrame";
        var_5.Parent = noti_container;
        var_5.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_5.BackgroundTransparency = 1;
        var_5.ClipsDescendants = true;
        var_5.Position = UDim2.new(0, 0, 0, 0);
        var_5.Size = UDim2.new(1, 0, 0, 0);
        var_5.AutomaticSize = Enum.AutomaticSize.Y;
        var_6.Name = "Noticontainer";
        var_6.Parent = var_5;
        var_6.Position = UDim2.new(1, 0, 0, 0);
        var_6.Size = UDim2.new(1, 0, 1, 6);
        var_6.AutomaticSize = Enum.AutomaticSize.Y;
        var_6.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
        table.insert(theme_listeners["Background 3 Color"], function()
            var_6.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
            return ;
        end);
        var_7.CornerRadius = UDim.new(0, 4);
        var_7.Parent = var_6;
        var_8.Name = "Topnoti";
        var_8.Parent = var_6;
        var_8.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_8.BackgroundTransparency = 1;
        var_8.Position = UDim2.new(0, 0, 0, 5);
        var_8.Size = UDim2.new(1, 0, 0, 25);
        var_9.Name = "Ruafimg";
        var_9.Parent = var_8;
        var_9.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_9.BackgroundTransparency = 1;
        var_9.Position = UDim2.new(0, 10, 0, 0);
        var_9.Size = UDim2.new(0, 25, 0, 25);
        var_9.Image = getgenv().ui_color["Logo Image"];
        table.insert(theme_listeners["Logo Image"], function()
            var_9.Image = ui_library.get_img(getgenv().ui_color["Logo Image"]);
            return ;
        end);
        var_10.CornerRadius = UDim.new(1, 0);
        var_10.Name = "RuafimgCorner";
        var_10.Parent = var_9;
        var_11.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().title_name_noti;
        table.insert(theme_listeners["Title Text Color"], function()
            var_11.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().title_name_noti;
            return ;
        end);
        var_11.Name = "TextLabelNoti";
        var_11.Parent = var_8;
        var_11.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_11.BackgroundTransparency = 1;
        var_11.Position = UDim2.new(0, 40, 0, 0);
        var_11.Size = UDim2.new(1, -40, 1, 0);
        var_11.Font = Enum.Font.GothamBold;
        var_11.TextSize = 14;
        var_11.TextWrapped = true;
        var_11.TextXAlignment = Enum.TextXAlignment.Left;
        var_11.RichText = true;
        var_11.TextColor3 = getgenv().ui_color["GUI Text Color"];
        table.insert(theme_listeners["GUI Text Color"], function()
            var_11.TextColor3 = getgenv().ui_color["GUI Text Color"];
            return ;
        end);
        var_12.Name = "CloseContainer";
        var_12.Parent = var_8;
        var_12.AnchorPoint = Vector2.new(1, 0.5);
        var_12.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_12.BackgroundTransparency = 1;
        var_12.Position = UDim2.new(1, -4, 0.5, 0);
        var_12.Size = UDim2.new(0, 22, 0, 22);
        var_13.Name = "CloseImage";
        var_13.Parent = var_12;
        var_13.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_13.BackgroundTransparency = 1;
        var_13.Size = UDim2.new(1, 0, 1, 0);
        var_13.Image = "rbxassetid://3926305904";
        var_13.ImageRectOffset = Vector2.new(284, 4);
        var_13.ImageRectSize = Vector2.new(24, 24);
        var_13.ImageColor3 = getgenv().ui_color["Search Icon Color"];
        table.insert(theme_listeners["Search Icon Color"], function()
            var_13.ImageColor3 = getgenv().ui_color["Search Icon Color"];
            return ;
        end);
        var_14.Parent = var_12;
        var_14.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_14.BackgroundTransparency = 1;
        var_14.Size = UDim2.new(1, 0, 1, 0);
        var_14.Font = Enum.Font.SourceSans;
        var_14.Text = "";
        var_14.TextColor3 = Color3.fromRGB(0, 0, 0);
        var_14.TextSize = 14;
        if var_3 then
            var_15.Name = "TextColor";
            var_15.Parent = var_6;
            var_15.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_15.BackgroundTransparency = 1;
            var_15.Position = UDim2.new(0, 10, 0, 35);
            var_15.Size = UDim2.new(1, -15, 0, 0);
            var_15.Font = Enum.Font.GothamBold;
            var_15.Text = var_3;
            var_15.TextSize = 14;
            var_15.TextXAlignment = Enum.TextXAlignment.Left;
            var_15.RichText = true;
            var_15.TextColor3 = getgenv().ui_color[text_color_key];
            var_15.AutomaticSize = Enum.AutomaticSize.Y;
            var_15.TextWrapped = true;
            table.insert(theme_listeners[text_color_key], function()
                var_15.TextColor3 = getgenv().ui_color[text_color_key];
                return ;
            end);
        end;
        local func = function()
            tween_service_2:Create(var_6, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Position = UDim2.new(1, 0, 0, 0) }):Play();
            wait(0.25);
            var_5:Destroy();
            return ;
        end;
        tween_service_2:Create(var_6, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Position = UDim2.new(0, 0, 0, 0) }):Play();
        var_14.MouseEnter:Connect(function()
            tween_service_2:Create(var_13, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { ImageColor3 = getgenv().ui_color["Search Icon Highlight Color"] }):Play();
            return ;
        end);
        var_14.MouseLeave:Connect(function()
            tween_service_2:Create(var_13, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { ImageColor3 = getgenv().ui_color["Search Icon Color"] }):Play();
            return ;
        end);
        var_14.MouseButton1Click:Connect(function()
            spawn(function()
                ui_library.button_effect();
                return ;
            end);
            wait(0.25);
            func();
            return ;
        end);
        spawn(function()
            wait(var_4);
            func();
            return ;
        end);
        return ;
    end;
    breathing_connections.create_main = function(arg_3)
        local str = tostring(arg_3.title) or "Feral";
        getgenv().main_desc = arg_3.desc or "";
        local flag = false;
        cac = false;
        local func_2 = function(arg_4, arg_5)
            local flag_2 = nil;
            local flag_3 = nil;
            local flag_4 = nil;
            local flag_5 = nil;
            arg_4.InputBegan:Connect(function(arg_6)
                if arg_6.UserInputType == Enum.UserInputType.MouseButton1 or arg_6.UserInputType == Enum.UserInputType.Touch then
                    flag_2 = true;
                    flag_4 = arg_6.Position;
                    flag_5 = arg_5.Position;
                    arg_6.Changed:Connect(function()
                        if arg_6.UserInputState == Enum.UserInputState.End then
                            flag_2 = false;
                        end;
                        return ;
                    end);
                end;
                return ;
            end);
            arg_4.InputChanged:Connect(function(arg_7)
                if arg_7.UserInputType == Enum.UserInputType.MouseMovement or arg_7.UserInputType == Enum.UserInputType.Touch then
                    flag_3 = arg_7;
                end;
                return ;
            end);
            user_input_service.InputChanged:Connect(function(arg_8)
                if arg_8 == flag_3 and flag_2 then
                    local var_16 = arg_8.Position - flag_4;
                    if not flag and cac then
                        tween_service_2:Create(arg_5, TweenInfo.new(0.35, Enum.EasingStyle.Linear, Enum.EasingDirection.Out), { Position = UDim2.new(flag_5.X.Scale, flag_5.X.Offset + var_16.X, flag_5.Y.Scale, flag_5.Y.Offset + var_16.Y) }):Play();
                    elseif not flag and not cac then
                        arg_5.Position = UDim2.new(flag_5.X.Scale, flag_5.X.Offset + var_16.X, flag_5.Y.Scale, flag_5.Y.Offset + var_16.Y);
                    end;
                end;
                return ;
            end);
            return ;
        end;
        local var_17 = Instance.new(frame_class);
        local var_18 = Instance.new("ImageLabel");
        local var_19 = Instance.new("UICorner");
        local var_20 = Instance.new(frame_class);
        local var_21 = Instance.new("ImageLabel");
        local var_22 = Instance.new("TextLabel");
        local var_23 = Instance.new(frame_class);
        local var_24 = Instance.new("UICorner");
        local var_25 = Instance.new("ScrollingFrame");
        local var_26 = Instance.new("UIListLayout");
        local var_27 = Instance.new("TextLabel");
        local var_28 = Instance.new(frame_class);
        local var_29 = Instance.new("UIPageLayout");
        local var_30 = Instance.new(frame_class);
        local var_31 = Instance.new("TextButton");
        local var_32 = Instance.new("ImageLabel");
        local var_33 = Instance.new(frame_class);
        local var_34 = Instance.new(frame_class);
        local var_35 = Instance.new(frame_class);
        local var_36 = Instance.new("UIPageLayout");
        var_17.Name = "Main";
        var_17.Parent = ui_library.gui;
        var_17.BackgroundColor3 = Color3.fromRGB(42, 42, 42);
        var_17.BackgroundTransparency = 1;
        var_17.Position = UDim2.new(0.5, 0, 0.5, 0);
        var_17.AnchorPoint = Vector2.new(0.5, 0.5);
        var_17.Size = UDim2.new(0, 629, 0, 359);
        func_2(var_17, var_17);
        var_18.Name = "maingui";
        var_18.Parent = var_17;
        var_18.AnchorPoint = Vector2.new(0.5, 0.5);
        var_18.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_18.BackgroundTransparency = 1;
        var_18.Position = UDim2.new(0.5, 0, 0.5, 0);
        var_18.Selectable = true;
        var_18.Size = UDim2.new(1, 30, 1, 30);
        var_18.Image = "rbxassetid://8068653048";
        var_18.ScaleType = Enum.ScaleType.Slice;
        var_18.SliceCenter = Rect.new(15, 15, 175, 175);
        var_18.SliceScale = 1.3;
        var_18.ImageColor3 = getgenv().ui_color["Border Color"];
        table.insert(theme_listeners["Border Color"], function()
            var_18.ImageColor3 = getgenv().ui_color["Border Color"];
            return ;
        end);
        ui_library.reload_main = function(arg_9)
            var_18.ImageColor3 = getgenv().ui_color["Title Text Color"];
            var_22.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().main_desc;
            table.insert(theme_listeners["Title Text Color"], function()
                var_18.ImageColor3 = getgenv().ui_color["Title Text Color"];
                var_22.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().main_desc;
                return ;
            end);
            local flag_6 = nil;
            if arg_9 ~= "" and (type(arg_9) == "string" and string.find(arg_9:lower(), ".webm") and pcall(function()
                writefile("seahub.webm", syn.request({ Url = arg_9 }).Body);
                return ;
            end)) then
                wait(0.25);
                local file_data = isfile("seahub.webm");
                wait(0.25);
                if file_data then
                    flag_6 = Instance.new("VideoFrame");
                    flag_6.Name = "MainContainer";
                    flag_6.Parent = var_17;
                    flag_6.BackgroundColor3 = getgenv().ui_color["Background Main Color"];
                    flag_6.Size = UDim2.new(1, 0, 1, 0);
                    flag_6.Video = getsynasset("seahub.webm");
                    flag_6.Looped = true;
                    flag_6:Play();
                    wait(0.5);
                    delfile("seahub.webm");
                end;
            else
                flag_6 = Instance.new("ImageLabel");
                flag_6.Name = "MainContainer";
                flag_6.Parent = var_17;
                flag_6.BackgroundColor3 = getgenv().ui_color["Background Main Color"];
                flag_6.Size = UDim2.new(1, 0, 1, 0);
                flag_6.Image = ui_library.get_img(arg_9);
            end;
            main_corner = Instance.new("UICorner");
            main_corner.CornerRadius = UDim.new(0, 4);
            main_corner.Name = "MainCorner";
            main_corner.Parent = flag_6;
            for var_37, var_38 in next, var_17:GetChildren() do
                if var_38.Name == "MainContainer" then
                    for var_39, var_40 in next, var_38:GetChildren() do
                        var_40.Parent = flag_6;
                    end;
                    wait();
                    var_38:Destroy();
                    break;
                end;
            end;
            table.insert(theme_listeners["Background 3 Color"], function()
                flag_6.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                return ;
            end);
            return ;
        end;
        var_18.ImageColor3 = getgenv().ui_color["Title Text Color"];
        var_22.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().main_desc;
        table.insert(theme_listeners["Title Text Color"], function()
            var_18.ImageColor3 = getgenv().ui_color["Title Text Color"];
            var_22.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().main_desc;
            return ;
        end);
        local flag_7 = nil;
        local var_41 = getgenv().ui_color["Background Image"];
        if var_41 ~= "" and (type(var_41) == "string" and string.find(var_41:lower(), ".webm") and pcall(function()
            writefile("seahub.webm", syn.request({ Url = var_41 }).Body);
            return ;
        end)) then
            wait(0.25);
            local file_data_2 = isfile("seahub.webm");
            wait(0.25);
            if file_data_2 then
                flag_7 = Instance.new("VideoFrame");
                flag_7.Name = "MainContainer";
                flag_7.Parent = var_17;
                flag_7.BackgroundColor3 = getgenv().ui_color["Background Main Color"];
                flag_7.Size = UDim2.new(1, 0, 1, 0);
                flag_7.Video = getsynasset("seahub.webm");
                flag_7.Looped = true;
                flag_7:Play();
                wait(0.5);
                delfile("seahub.webm");
            end;
        else
            flag_7 = Instance.new("ImageLabel");
            flag_7.Name = "MainContainer";
            flag_7.Parent = var_17;
            flag_7.BackgroundColor3 = getgenv().ui_color["Background Main Color"];
            flag_7.Size = UDim2.new(1, 0, 1, 0);
            flag_7.Image = ui_library.get_img(var_41);
        end;
        table.insert(theme_listeners["Background 3 Color"], function()
            flag_7.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
            return ;
        end);
        getgenv().ready_for_gui_loaded = true;
        var_19.CornerRadius = UDim.new(0, 4);
        var_19.Name = "MainCorner";
        var_19.Parent = flag_7;
        var_33.Name = "Concacontainer";
        var_33.Parent = flag_7;
        var_33.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_33.BackgroundTransparency = 1;
        var_33.ClipsDescendants = true;
        var_33.Position = UDim2.new(0, 0, 0, 30);
        var_33.Size = UDim2.new(1, 0, 1, -30);
        var_34.Name = "Concacmain";
        var_34.Parent = var_33;
        var_34.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_34.BackgroundTransparency = 1;
        var_34.Selectable = true;
        var_34.Size = UDim2.new(1, 0, 1, 0);
        var_35.Name = "Background1";
        var_35.Parent = var_33;
        var_35.LayoutOrder = 1;
        var_35.Selectable = true;
        var_35.Size = UDim2.new(1, 0, 1, 0);
        var_35.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
        table.insert(theme_listeners["Background 1 Transparency"], function()
            var_35.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
            return ;
        end);
        var_35.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
        table.insert(theme_listeners["Background 1 Color"], function()
            var_35.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
            return ;
        end);
        var_36.Name = "Concacpage";
        var_36.Parent = var_33;
        var_36.SortOrder = Enum.SortOrder.LayoutOrder;
        var_36.EasingDirection = Enum.EasingDirection.InOut;
        var_36.EasingStyle = Enum.EasingStyle.Quad;
        var_36.TweenTime = getgenv().ui_color["Tween Animation 1 Speed"];
        table.insert(theme_listeners["Tween Animation 1 Speed"], function()
            var_36.TweenTime = getgenv().ui_color["Tween Animation 1 Speed"];
            return ;
        end);
        var_20.Name = "TopMain";
        var_20.Parent = flag_7;
        var_20.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_20.BackgroundTransparency = 1;
        var_20.Size = UDim2.new(1, 0, 0, 25);
        var_21.Name = "Ruafimg";
        var_21.Parent = var_20;
        var_21.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_21.BackgroundTransparency = 1;
        var_21.Position = UDim2.new(0, 5, 0, 0);
        var_21.Size = UDim2.new(0, 25, 0, 25);
        var_21.Image = getgenv().ui_color["Logo Image"];
        table.insert(theme_listeners["Logo Image"], function()
            var_21.Image = getgenv().ui_color["Logo Image"];
            return ;
        end);
        var_22.Name = "TextLabelMain";
        var_22.Parent = var_20;
        var_22.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_22.BackgroundTransparency = 1;
        var_22.Position = UDim2.new(0, 35, 0, 0);
        var_22.Size = UDim2.new(1, -35, 1, 0);
        var_22.Font = Enum.Font.GothamBold;
        var_22.RichText = true;
        var_22.TextSize = 16;
        var_22.TextWrapped = true;
        var_22.TextXAlignment = Enum.TextXAlignment.Left;
        var_22.TextColor3 = getgenv().ui_color["GUI Text Color"];
        table.insert(theme_listeners["GUI Text Color"], function()
            var_22.TextColor3 = getgenv().ui_color["GUI Text Color"];
            return ;
        end);
        var_22.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().main_desc;
        table.insert(theme_listeners["Title Text Color"], function()
            var_22.Text = "<font color=\"rgb(" .. (tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[1]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[2]) .. "," .. tostring(ui_library.get_color(getgenv().ui_color["Title Text Color"])[3])) .. ")\">Feral</font> " .. getgenv().main_desc;
            return ;
        end);
        var_30.Name = "SettionMain";
        var_30.Parent = var_20;
        var_30.AnchorPoint = Vector2.new(1, 0);
        var_30.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_30.BackgroundTransparency = 1;
        var_30.Position = UDim2.new(1, 0, 0, 0);
        var_30.Size = UDim2.new(0, 30, 0, 30);
        var_31.Name = "SettionButton";
        var_31.Parent = var_30;
        var_31.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_31.BackgroundTransparency = 1;
        var_31.BorderColor3 = Color3.fromRGB(27, 42, 53);
        var_31.Size = UDim2.new(1, 0, 1, 0);
        var_31.Font = Enum.Font.SourceSans;
        var_31.Text = "";
        var_31.TextColor3 = Color3.fromRGB(0, 0, 0);
        var_31.TextSize = 14;
        var_31.Visible = true;
        var_32.Name = "SettingIcon";
        var_32.Parent = var_30;
        var_32.AnchorPoint = Vector2.new(0.5, 0.5);
        var_32.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_32.BackgroundTransparency = 1;
        var_32.Position = UDim2.new(0.5, 0, 0.5, 0);
        var_32.Size = UDim2.new(1, -10, 1, -10);
        var_32.Image = "rbxassetid://7397332215";
        var_32.Visible = true;
        var_32.ImageColor3 = getgenv().ui_color["Setting Icon Color"];
        table.insert(theme_listeners["Setting Icon Color"], function()
            var_32.ImageColor3 = getgenv().ui_color["Setting Icon Color"];
            return ;
        end);
        var_23.Name = "Background1";
        var_23.Parent = var_34;
        var_23.Position = UDim2.new(0, 5, 0, 0);
        var_23.Size = UDim2.new(0, 180, 0, 325);
        var_23.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
        table.insert(theme_listeners["Background 1 Transparency"], function()
            var_23.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
            return ;
        end);
        var_23.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
        table.insert(theme_listeners["Background 1 Color"], function()
            var_23.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
            return ;
        end);
        local var_42 = Instance.new("UIStroke");
        var_42.Color = getgenv().ui_color["Border Color"];
        var_42.Thickness = 1.5;
        var_42.Transparency = 0.5;
        var_42.ApplyStrokeMode = Enum.ApplyStrokeMode.Border;
        var_42.Parent = var_23;
        table.insert(theme_listeners["Border Color"], function()
            var_42.Color = getgenv().ui_color["Border Color"];
            return ;
        end);
        local var_43 = Instance.new("ImageLabel");
        var_43.Name = "Glow";
        var_43.Parent = var_23;
        var_43.AnchorPoint = Vector2.new(0.5, 0.5);
        var_43.BackgroundTransparency = 1;
        var_43.Position = UDim2.new(0.5, 0, 0.5, 0);
        var_43.Size = UDim2.new(1, 20, 1, 20);
        var_43.ZIndex = -1;
        var_43.Image = "rbxassetid://8068653048";
        var_43.ImageColor3 = getgenv().ui_color["Border Color"];
        var_43.ImageTransparency = 0.8;
        var_43.ScaleType = Enum.ScaleType.Slice;
        var_43.SliceCenter = Rect.new(15, 15, 175, 175);
        table.insert(theme_listeners["Border Color"], function()
            var_43.ImageColor3 = getgenv().ui_color["Border Color"];
            return ;
        end);
        var_24.CornerRadius = UDim.new(0, 4);
        var_24.Parent = var_23;
        var_25.Name = "ControlList";
        var_25.Parent = var_23;
        var_25.Active = true;
        var_25.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_25.BackgroundTransparency = 1;
        var_25.BorderColor3 = Color3.fromRGB(27, 42, 53);
        var_25.BorderSizePixel = 0;
        var_25.Position = UDim2.new(0, 0, 0, 30);
        var_25.Size = UDim2.new(1, -5, 1, -30);
        var_25.BottomImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
        var_25.CanvasSize = UDim2.new(0, 0, 0, 0);
        var_25.ScrollBarThickness = 5;
        var_25.TopImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
        var_26.Parent = var_25;
        var_26.SortOrder = Enum.SortOrder.LayoutOrder;
        var_26.Padding = UDim.new(0, 5);
        var_26.SortOrder = Enum.SortOrder.LayoutOrder;
        var_26.Padding = UDim.new(0, 5);
        var_27.Name = "GUITextColor";
        var_27.Parent = var_23;
        var_27.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_27.BackgroundTransparency = 1;
        var_27.Position = UDim2.new(0, 5, 0, 0);
        var_27.Size = UDim2.new(1, 0, 0, 25);
        var_27.Font = Enum.Font.GothamBold;
        var_27.Text = str;
        var_27.TextSize = 14;
        var_27.TextXAlignment = Enum.TextXAlignment.Left;
        var_27.TextColor3 = getgenv().ui_color["GUI Text Color"];
        table.insert(theme_listeners["GUI Text Color"], function()
            var_27.TextColor3 = getgenv().ui_color["GUI Text Color"];
            return ;
        end);
        var_28.Name = "MainPage";
        var_28.Parent = var_34;
        var_28.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
        var_28.BackgroundTransparency = 1;
        var_28.ClipsDescendants = true;
        var_28.Position = UDim2.new(0, 190, 0, 0);
        var_28.Size = UDim2.new(0, 435, 0, 325);
        var_29.Name = "UIPage";
        var_29.Parent = var_28;
        var_29.FillDirection = Enum.FillDirection.Vertical;
        var_29.SortOrder = Enum.SortOrder.LayoutOrder;
        var_29.EasingDirection = Enum.EasingDirection.InOut;
        var_29.EasingStyle = Enum.EasingStyle.Quart;
        var_29.Padding = UDim.new(0, 10);
        var_29.TweenTime = getgenv().ui_color["Tween Animation 1 Speed"];
        var_29.ScrollWheelInputEnabled = false;
        table.insert(theme_listeners["Tween Animation 1 Speed"], function()
            var_29.TweenTime = getgenv().ui_color["Tween Animation 1 Speed"];
            return ;
        end);
        var_26:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            var_25.CanvasSize = UDim2.new(0, 0, 0, var_26.AbsoluteContentSize.Y + 5);
            return ;
        end);
        local flag_8 = false;
        var_31.MouseButton1Click:Connect(function()
            ui_library.button_effect();
            return ;
        end);
        var_31.MouseButton1Click:Connect(function()
            flag_8 = not flag_8;
            local var_44 = flag_8 and var_35 or var_34;
            local var_45 = flag_8 and 180 or 0;
            var_36:JumpTo(var_44);
            game.TweenService:Create(var_32, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Rotation = var_45 }):Play();
            return ;
        end);
        local var_46 = Instance.new("ScrollingFrame");
        local var_47 = Instance.new("UIListLayout");
        var_46.Name = "CustomList";
        var_46.Parent = var_35;
        var_46.Active = true;
        var_46.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_46.BackgroundTransparency = 1;
        var_46.BorderColor3 = Color3.fromRGB(27, 42, 53);
        var_46.BorderSizePixel = 0;
        var_46.Position = UDim2.new(0, 5, 0, 30);
        var_46.Size = UDim2.new(1, -10, 1, -30);
        var_46.BottomImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
        var_46.ScrollBarThickness = 5;
        var_46.TopImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
        var_47.Name = "CustomListLayout";
        var_47.Parent = var_46;
        var_47.SortOrder = Enum.SortOrder.LayoutOrder;
        var_47.Padding = UDim.new(0, 5);
        var_47:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            var_46.CanvasSize = UDim2.new(0, 0, 0, var_47.AbsoluteContentSize.Y + 5);
            return ;
        end);
        local var_48 = Instance.new("TextLabel");
        var_48.Name = "GUITextColor";
        var_48.Parent = var_35;
        var_48.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_48.BackgroundTransparency = 1;
        var_48.Position = UDim2.new(0, 15, 0, 0);
        var_48.Size = UDim2.new(1, -15, 0, 25);
        var_48.Font = Enum.Font.GothamBold;
        var_48.Text = "";
        var_48.TextSize = 16;
        var_48.TextXAlignment = Enum.TextXAlignment.Left;
        var_48.TextColor3 = getgenv().ui_color["GUI Text Color"];
        table.insert(theme_listeners["GUI Text Color"], function()
            var_48.TextColor3 = getgenv().ui_color["GUI Text Color"];
            return ;
        end);
        local var_49 = Instance.new(frame_class);
        local var_50 = Instance.new("UICorner");
        local var_51 = Instance.new(frame_class);
        local var_52 = Instance.new("ImageLabel");
        local var_53 = Instance.new("TextButton");
        local var_54 = Instance.new("TextBox");
        var_49.Name = "Background2";
        var_49.Parent = var_35;
        var_49.AnchorPoint = Vector2.new(1, 0);
        var_49.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
        var_49.ClipsDescendants = true;
        var_49.Position = UDim2.new(1, -5, 0, 5);
        var_49.Size = UDim2.new(0, 20, 0, 20);
        var_49.ClipsDescendants = true;
        table.insert(theme_listeners["Background 2 Color"], function()
            var_49.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
            return ;
        end);
        var_50.CornerRadius = UDim.new(0, 2);
        var_50.Name = "PageSearchCorner";
        var_50.Parent = var_49;
        var_51.Name = "SearchFrame";
        var_51.Parent = var_49;
        var_51.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_51.BackgroundTransparency = 1;
        var_51.Size = UDim2.new(0, 20, 0, 20);
        var_52.Name = "SearchIcon";
        var_52.Parent = var_51;
        var_52.AnchorPoint = Vector2.new(0.5, 0.5);
        var_52.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_52.BackgroundTransparency = 1;
        var_52.Position = UDim2.new(0.5, 0, 0.5, 0);
        var_52.Size = UDim2.new(0, 16, 0, 16);
        var_52.Image = "rbxassetid://8154282545";
        var_52.ImageColor3 = getgenv().ui_color["Search Icon Color"];
        table.insert(theme_listeners["Search Icon Color"], function()
            var_52.ImageColor3 = getgenv().ui_color["Search Icon Color"];
            return ;
        end);
        var_53.Name = "active";
        var_53.Parent = var_51;
        var_53.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_53.BackgroundTransparency = 1;
        var_53.Size = UDim2.new(1, 0, 1, 0);
        var_53.Font = Enum.Font.SourceSans;
        var_53.Text = "";
        var_53.TextColor3 = Color3.fromRGB(0, 0, 0);
        var_53.TextSize = 14;
        var_54.Name = "TextColorPlaceholder";
        var_54.Parent = var_49;
        var_54.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
        var_54.BackgroundTransparency = 1;
        var_54.Position = UDim2.new(0, 30, 0, 0);
        var_54.Size = UDim2.new(1, -30, 1, 0);
        var_54.Font = Enum.Font.GothamBold;
        var_54.Text = "";
        var_54.TextSize = 14;
        var_54.TextXAlignment = Enum.TextXAlignment.Left;
        var_54.PlaceholderText = "Search Section name";
        var_54.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
        var_54.TextColor3 = getgenv().ui_color[text_color_key];
        table.insert(theme_listeners["Placeholder Text Color"], function()
            var_54.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
            return ;
        end);
        table.insert(theme_listeners[text_color_key], function()
            var_54.TextColor3 = getgenv().ui_color[text_color_key];
            return ;
        end);
        local flag_9 = false;
        var_53.MouseEnter:Connect(function()
            tween_service_2:Create(var_52, TweenInfo.new(getgenv().ui_color["Tween Animation 3 Speed"]), { ImageColor3 = getgenv().ui_color["Search Icon Highlight Color"] }):Play();
            return ;
        end);
        var_53.MouseLeave:Connect(function()
            tween_service_2:Create(var_52, TweenInfo.new(getgenv().ui_color["Tween Animation 3 Speed"]), { ImageColor3 = getgenv().ui_color["Search Icon Color"] }):Play();
            return ;
        end);
        var_53.MouseButton1Click:Connect(function()
            ui_library.button_effect();
            return ;
        end);
        var_54.Focused:Connect(function()
            ui_library.button_effect();
            return ;
        end);
        var_53.MouseButton1Click:Connect(function()
            flag_9 = not flag_9;
            local var_55 = flag_9 and UDim2.new(0, 175, 0, 20) or UDim2.new(0, 20, 0, 20);
            game.TweenService:Create(var_49, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Size = var_55 }):Play();
            return ;
        end);
        local func_3 = function()
            for var_56, var_57 in next, var_46:GetChildren() do
                if not var_57:IsA("UIListLayout") then
                    var_57.Visible = false;
                end;
            end;
            return ;
        end;
        local func_4 = function()
            for key_5, value_7 in pairs(var_46:GetChildren()) do
                if not value_7:IsA("UIListLayout") and string.find(string.lower(value_7.Name), string.lower(var_54.Text)) then
                    value_7.Visible = true;
                end;
            end;
            return ;
        end;
        var_54:GetPropertyChangedSignal("Text"):Connect(function()
            func_3();
            func_4();
            return ;
        end);
        breathing_connections.CreateCustomColor = function(arg_10)
            var_48.Text = arg_10 or "Custom GUI";
            return {
                create_section = function(arg_11)
                    local var_58 = Instance.new(frame_class);
                    local var_59 = Instance.new("UICorner");
                    local var_60 = Instance.new(frame_class);
                    local var_61 = Instance.new("TextLabel");
                    local var_62 = Instance.new(frame_class);
                    local var_63 = Instance.new("UIGradient");
                    local var_64 = Instance.new("UIListLayout");
                    if arg_11 then
                    end;
                    var_58.Name = arg_11 .. "Section";
                    var_58.Parent = var_46;
                    var_58.Size = UDim2.new(1, 0, 0, 285);
                    var_58.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                    table.insert(theme_listeners["Background 3 Color"], function()
                        var_58.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                        return ;
                    end);
                    var_58.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                    table.insert(theme_listeners["Background 1 Transparency"], function()
                        var_58.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                        return ;
                    end);
                    var_59.CornerRadius = UDim.new(0, 4);
                    var_59.Parent = var_58;
                    var_60.Name = "Topsec";
                    var_60.Parent = var_58;
                    var_60.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                    var_60.BackgroundTransparency = 1;
                    var_60.Size = UDim2.new(1, 0, 0, 27);
                    var_61.Name = "Sectiontitle";
                    var_61.Parent = var_60;
                    var_61.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                    var_61.BackgroundTransparency = 1;
                    var_61.Size = UDim2.new(1, 0, 1, 0);
                    var_61.Font = Enum.Font.GothamBold;
                    var_61.Text = arg_11;
                    var_61.TextSize = 14;
                    var_61.TextColor3 = getgenv().ui_color["Section Text Color"];
                    table.insert(theme_listeners["Section Text Color"], function()
                        var_61.TextColor3 = getgenv().ui_color["Section Text Color"];
                        return ;
                    end);
                    var_62.Name = "Linesec";
                    var_62.Parent = var_60;
                    var_62.AnchorPoint = Vector2.new(0.5, 1);
                    var_62.BorderSizePixel = 0;
                    var_62.Position = UDim2.new(0.5, 0, 1, -2);
                    var_62.Size = UDim2.new(1, -10, 0, 2);
                    var_62.BackgroundColor3 = getgenv().ui_color["Section Underline Color"];
                    table.insert(theme_listeners["Section Underline Color"], function()
                        var_62.BackgroundColor3 = getgenv().ui_color["Section Underline Color"];
                        return ;
                    end);
                    var_63.Transparency = NumberSequence.new({ NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5, 0), NumberSequenceKeypoint.new(0.51, 0.02), NumberSequenceKeypoint.new(1, 1) });
                    var_63.Parent = var_62;
                    var_64.Name = "SectionList";
                    var_64.Parent = var_58;
                    var_64.SortOrder = Enum.SortOrder.LayoutOrder;
                    var_64.Padding = UDim.new(0, 5);
                    var_64:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
                        var_58.Size = UDim2.new(1, 0, 0, var_64.AbsoluteContentSize.Y + 5);
                        return ;
                    end);
                    return {
                        create_color_picker = function(arg_12)
                            local var_65 = setmetatable({}, {
                                __index = function(arg_13, arg_14)
                                    if arg_14 == "Cungroi" then
                                        return custom_theme[arg_12.type].Rainbow;
                                    end;
                                    return ;
                                end,
                                __newindex = function(arg_15, arg_16, arg_17)
                                    if arg_16 == "Cungroi" then
                                        custom_theme[arg_12.type].Rainbow = arg_17;
                                    end;
                                    return ;
                                end
                            });
                            local var_66 = arg_12.title or "Color Picker";
                            if not custom_theme[arg_12.type].Color then
                                local color_3 = Color3.fromRGB(255, 255, 255);
                            end;
                            local var_67 = arg_12.type;
                            local var_68 = Instance.new(frame_class);
                            local var_69 = Instance.new("UICorner");
                            local var_70 = Instance.new(frame_class);
                            local var_71 = Instance.new("UICorner");
                            local var_72 = Instance.new("TextLabel");
                            local var_73 = Instance.new(frame_class);
                            local var_74 = Instance.new("UICorner");
                            local var_75 = Instance.new("TextButton");
                            local var_76 = Instance.new(frame_class);
                            local var_77 = Instance.new("UIGradient");
                            local var_78 = Instance.new(frame_class);
                            local var_79 = Instance.new("UICorner");
                            local var_80 = Instance.new(frame_class);
                            local var_81 = Instance.new(frame_class);
                            local var_82 = Instance.new("TextLabel");
                            local var_83 = Instance.new("TextBox");
                            local var_84 = Instance.new(frame_class);
                            local var_85 = Instance.new("TextLabel");
                            local var_86 = Instance.new("TextBox");
                            local var_87 = Instance.new(frame_class);
                            local var_88 = Instance.new("TextLabel");
                            local var_89 = Instance.new("TextBox");
                            local var_90 = Instance.new("UIListLayout");
                            local var_91 = Instance.new(frame_class);
                            local var_92 = Instance.new("TextLabel");
                            local var_93 = Instance.new("TextBox");
                            local var_94 = Instance.new(frame_class);
                            local var_95 = Instance.new("UIGradient");
                            local var_96 = Instance.new(frame_class);
                            local var_97 = Instance.new(frame_class);
                            local var_98 = Instance.new("TextLabel");
                            local var_99 = Instance.new("ImageLabel");
                            local var_100 = Instance.new("ImageLabel");
                            local var_101 = Instance.new("TextButton");
                            local var_102 = Instance.new("ImageLabel");
                            local var_103 = Instance.new(frame_class);
                            local var_104 = Instance.new("UICorner");
                            local var_105 = Instance.new(frame_class);
                            local var_106 = Instance.new(frame_class);
                            local var_107 = Instance.new("TextLabel");
                            local var_108 = Instance.new("ImageLabel");
                            local var_109 = Instance.new("ImageLabel");
                            local var_110 = Instance.new("TextButton");
                            local var_111 = Instance.new(frame_class);
                            local var_112 = Instance.new("UIListLayout");
                            local var_113 = Instance.new(frame_class);
                            local var_114 = Instance.new("UICorner");
                            local var_115 = Instance.new("TextButton");
                            local var_116 = Instance.new(frame_class);
                            local var_117 = Instance.new("UICorner");
                            local var_118 = Instance.new("TextButton");
                            var_68.Name = "ColorPick";
                            var_68.Parent = var_58;
                            var_68.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_68.BackgroundTransparency = 1;
                            var_68.ClipsDescendants = true;
                            var_68.Position = UDim2.new(0, 0, 0.112280704, 0);
                            var_68.Size = UDim2.new(1, 0, 0, 35);
                            var_69.CornerRadius = UDim.new(0, 4);
                            var_69.Name = "ColorPickCorner";
                            var_69.Parent = var_68;
                            var_70.Name = "Background1";
                            var_70.Parent = var_68;
                            var_70.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_70.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            var_70.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_70.Size = UDim2.new(1, -10, 1, 0);
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_70.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_70.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_70.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_71.CornerRadius = UDim.new(0, 4);
                            var_71.Name = "ColorpickBGCorner";
                            var_71.Parent = var_70;
                            var_72.Name = "TextColor";
                            var_72.Parent = var_70;
                            var_72.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_72.BackgroundTransparency = 1;
                            var_72.Position = UDim2.new(0, 10, 0, 0);
                            var_72.Size = UDim2.new(1, -10, 0, 35);
                            var_72.Font = Enum.Font.GothamBlack;
                            var_72.Text = var_66;
                            var_72.TextSize = 14;
                            var_72.TextXAlignment = Enum.TextXAlignment.Left;
                            var_72.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_72.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_73.Name = "ColorVal";
                            var_73.Parent = var_68;
                            var_73.AnchorPoint = Vector2.new(1, 0);
                            var_73.BackgroundColor3 = custom_theme[var_67].Color;
                            var_73.Position = UDim2.new(1, -10, 0, 5);
                            var_73.Size = UDim2.new(0, 150, 0, 25);
                            var_74.CornerRadius = UDim.new(0, 4);
                            var_74.Name = "ColorValCorner";
                            var_74.Parent = var_73;
                            var_75.Name = "ColorValButton";
                            var_75.Parent = var_73;
                            var_75.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_75.BackgroundTransparency = 1;
                            var_75.Size = UDim2.new(1, 0, 1, 0);
                            var_75.Font = Enum.Font.SourceSans;
                            var_75.Text = "";
                            var_75.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_75.TextSize = 14;
                            var_76.Name = "Hue";
                            var_76.Parent = var_68;
                            var_76.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_76.BorderSizePixel = 0;
                            var_76.Position = UDim2.new(0, 460, 0, 40);
                            var_76.Size = UDim2.new(0, 25, 0, 200);
                            var_77.Color = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 4)), ColorSequenceKeypoint.new(0.17, Color3.fromRGB(235, 7, 255)), ColorSequenceKeypoint.new(0.33, Color3.fromRGB(0, 9, 189)), ColorSequenceKeypoint.new(0.49, Color3.fromRGB(0, 193, 196)), ColorSequenceKeypoint.new(0.66, Color3.fromRGB(0, 255, 0)), ColorSequenceKeypoint.new(0.84, Color3.fromRGB(255, 247, 0)), ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 0)) });
                            var_77.Rotation = 90;
                            var_77.Name = "HueGra";
                            var_77.Parent = var_76;
                            var_78.Parent = var_76;
                            var_78.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_78.Position = UDim2.new(0, 0, 1, 0);
                            var_78.Size = UDim2.new(1, 0, 0, 2);
                            var_79.CornerRadius = UDim.new(0, 4);
                            var_79.Parent = var_76;
                            var_80.Name = "Concac";
                            var_80.Parent = var_68;
                            var_80.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_80.BackgroundTransparency = 1;
                            var_80.Position = UDim2.new(0, 495, 0, 40);
                            var_80.Size = UDim2.new(0, 115, 0, 100);
                            var_81.Name = "RFrame";
                            var_81.Parent = var_80;
                            var_81.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_81.BackgroundTransparency = 1;
                            var_81.Size = UDim2.new(1, 0, 0, 25);
                            var_81.LayoutOrder = 0;
                            var_82.Name = "RText";
                            var_82.Parent = var_81;
                            var_82.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_82.BackgroundTransparency = 1;
                            var_82.Size = UDim2.new(0, 25, 0, 25);
                            var_82.Font = Enum.Font.GothamBold;
                            var_82.Text = "R:";
                            var_82.TextColor3 = Color3.fromRGB(115, 115, 115);
                            var_82.TextSize = 14;
                            var_82.TextXAlignment = Enum.TextXAlignment.Left;
                            var_83.Name = "RBox";
                            var_83.Parent = var_81;
                            var_83.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_83.BackgroundTransparency = 1;
                            var_83.Position = UDim2.new(0, 25, 0, 0);
                            var_83.Size = UDim2.new(1, -25, 1, 0);
                            var_83.ClearTextOnFocus = false;
                            var_83.Font = Enum.Font.GothamBold;
                            var_83.Text = "255";
                            var_83.TextColor3 = Color3.fromRGB(255, 255, 255);
                            var_83.TextSize = 14;
                            var_83.TextXAlignment = Enum.TextXAlignment.Left;
                            var_84.Name = "GFrame";
                            var_84.Parent = var_80;
                            var_84.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_84.BackgroundTransparency = 1;
                            var_84.Size = UDim2.new(1, 0, 0, 25);
                            var_84.LayoutOrder = 1;
                            var_85.Name = "GText";
                            var_85.Parent = var_84;
                            var_85.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_85.BackgroundTransparency = 1;
                            var_85.Size = UDim2.new(0, 25, 0, 25);
                            var_85.Font = Enum.Font.GothamBold;
                            var_85.Text = "G:";
                            var_85.TextColor3 = Color3.fromRGB(115, 115, 115);
                            var_85.TextSize = 14;
                            var_85.TextXAlignment = Enum.TextXAlignment.Left;
                            var_86.Name = "GBox";
                            var_86.Parent = var_84;
                            var_86.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_86.BackgroundTransparency = 1;
                            var_86.Position = UDim2.new(0, 25, 0, 0);
                            var_86.Size = UDim2.new(1, -25, 1, 0);
                            var_86.ClearTextOnFocus = false;
                            var_86.Font = Enum.Font.GothamBold;
                            var_86.Text = "255";
                            var_86.TextColor3 = Color3.fromRGB(255, 255, 255);
                            var_86.TextSize = 14;
                            var_86.TextXAlignment = Enum.TextXAlignment.Left;
                            var_87.Name = "BFrame";
                            var_87.Parent = var_80;
                            var_87.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_87.BackgroundTransparency = 1;
                            var_87.Size = UDim2.new(1, 0, 0, 25);
                            var_87.LayoutOrder = 2;
                            var_88.Name = "BText";
                            var_88.Parent = var_87;
                            var_88.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_88.BackgroundTransparency = 1;
                            var_88.Size = UDim2.new(0, 25, 0, 25);
                            var_88.Font = Enum.Font.GothamBold;
                            var_88.Text = "B:";
                            var_88.TextColor3 = Color3.fromRGB(115, 115, 115);
                            var_88.TextSize = 14;
                            var_88.TextXAlignment = Enum.TextXAlignment.Left;
                            var_89.Name = "BBox";
                            var_89.Parent = var_87;
                            var_89.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_89.BackgroundTransparency = 1;
                            var_89.Position = UDim2.new(0, 25, 0, 0);
                            var_89.Size = UDim2.new(1, -25, 1, 0);
                            var_89.ClearTextOnFocus = false;
                            var_89.Font = Enum.Font.GothamBold;
                            var_89.Text = "255";
                            var_89.TextColor3 = Color3.fromRGB(255, 255, 255);
                            var_89.TextSize = 14;
                            var_89.TextXAlignment = Enum.TextXAlignment.Left;
                            var_90.Parent = var_80;
                            var_90.SortOrder = Enum.SortOrder.LayoutOrder;
                            var_91.Name = "HexFrame";
                            var_91.Parent = var_80;
                            var_91.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_91.BackgroundTransparency = 1;
                            var_91.Size = UDim2.new(1, 0, 0, 25);
                            var_91.LayoutOrder = 3;
                            var_92.Name = "HexText";
                            var_92.Parent = var_91;
                            var_92.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_92.BackgroundTransparency = 1;
                            var_92.Size = UDim2.new(0, 25, 0, 25);
                            var_92.Font = Enum.Font.GothamBold;
                            var_92.Text = "#";
                            var_92.TextColor3 = Color3.fromRGB(115, 115, 115);
                            var_92.TextSize = 14;
                            var_92.TextXAlignment = Enum.TextXAlignment.Left;
                            var_93.Name = "HexBox";
                            var_93.Parent = var_91;
                            var_93.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_93.BackgroundTransparency = 1;
                            var_93.Position = UDim2.new(0, 25, 0, 0);
                            var_93.Size = UDim2.new(1, -25, 1, 0);
                            var_93.ClearTextOnFocus = false;
                            var_93.Font = Enum.Font.GothamBold;
                            var_93.Text = "FFFFFF";
                            var_93.TextColor3 = Color3.fromRGB(255, 255, 255);
                            var_93.TextSize = 14;
                            var_93.TextXAlignment = Enum.TextXAlignment.Left;
                            var_94.Name = "Linesec";
                            var_94.Parent = var_80;
                            var_94.AnchorPoint = Vector2.new(0.5, 1);
                            var_94.BorderSizePixel = 0;
                            var_94.Position = UDim2.new(0.5, 0, 1, -2);
                            var_94.Size = UDim2.new(1, -10, 0, 2);
                            var_94.LayoutOrder = 4;
                            var_94.BackgroundColor3 = getgenv().ui_color["Section Underline Color"];
                            table.insert(theme_listeners["Section Underline Color"], function()
                                var_94.BackgroundColor3 = getgenv().ui_color["Section Underline Color"];
                                return ;
                            end);
                            var_95.Transparency = NumberSequence.new({ NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0.25), NumberSequenceKeypoint.new(0.7, 0.25), NumberSequenceKeypoint.new(1, 1) });
                            var_95.Parent = var_94;
                            var_96.Name = "CungroiF";
                            var_96.Parent = var_68;
                            var_96.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_96.BackgroundTransparency = 1;
                            var_96.Position = UDim2.new(0, 495, 0, 145);
                            var_96.Size = UDim2.new(0, 115, 0, 25);
                            var_97.Name = "CungroiFF";
                            var_97.Parent = var_96;
                            var_97.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_97.BackgroundTransparency = 1;
                            var_97.Size = UDim2.new(1, 0, 0, 25);
                            var_97.LayoutOrder = 4;
                            var_98.Name = "TextColor";
                            var_98.Parent = var_97;
                            var_98.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_98.BackgroundTransparency = 1;
                            var_98.Size = UDim2.new(0, 85, 0, 25);
                            var_98.Font = Enum.Font.GothamBold;
                            var_98.Text = "Rainbow";
                            var_98.TextSize = 14;
                            var_98.TextXAlignment = Enum.TextXAlignment.Left;
                            var_98.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_98.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_99.Name = "Setting_checkbox";
                            var_99.Parent = var_97;
                            var_99.AnchorPoint = Vector2.new(1, 0.5);
                            var_99.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_99.BackgroundTransparency = 1;
                            var_99.Position = UDim2.new(1, -5, 0.5, 0);
                            var_99.Size = UDim2.new(0, 25, 0, 25);
                            var_99.Image = "rbxassetid://4552505888";
                            var_99.ImageColor3 = getgenv().ui_color["Toggle Border Color"];
                            table.insert(theme_listeners["Toggle Border Color"], function()
                                var_99.ImageColor3 = getgenv().ui_color["Toggle Border Color"];
                                return ;
                            end);
                            var_100.Name = "Setting_check";
                            var_100.Parent = var_99;
                            var_100.AnchorPoint = Vector2.new(0, 1);
                            var_100.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_100.BackgroundTransparency = 1;
                            var_100.Position = UDim2.new(0, 0, 1, 0);
                            var_100.Image = "rbxassetid://4555411759";
                            var_100.ImageColor3 = getgenv().ui_color["Toggle Checked Color"];
                            table.insert(theme_listeners["Toggle Checked Color"], function()
                                var_100.ImageColor3 = getgenv().ui_color["Toggle Checked Color"];
                                return ;
                            end);
                            var_101.Name = "Cungroitog";
                            var_101.Parent = var_97;
                            var_101.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_101.BackgroundTransparency = 1;
                            var_101.Size = UDim2.new(1, 0, 1, 0);
                            var_101.Font = Enum.Font.SourceSans;
                            var_101.Text = "";
                            var_101.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_101.TextSize = 14;
                            var_102.Name = "Color";
                            var_102.Parent = var_68;
                            var_102.BackgroundColor3 = Color3.fromRGB(255, 0, 0);
                            var_102.BorderSizePixel = 0;
                            var_102.Position = UDim2.new(0, 10, 0, 40);
                            var_102.Size = UDim2.new(0, 440, 0, 200);
                            var_102.Image = "rbxassetid://4155801252";
                            var_103.Name = "SelectorColor";
                            var_103.Parent = var_102;
                            var_103.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_103.BackgroundColor3 = Color3.fromRGB(203, 203, 203);
                            var_103.BorderColor3 = Color3.fromRGB(70, 70, 70);
                            var_103.Position = UDim2.new(1, 0, 0, 0);
                            var_103.Size = UDim2.new(0, 4, 0, 4);
                            var_104.CornerRadius = UDim.new(0, 4);
                            var_104.Parent = var_102;
                            var_105.Name = "HoithoF";
                            var_105.Parent = var_68;
                            var_105.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_105.BackgroundTransparency = 1;
                            var_105.Position = UDim2.new(0, 495, 0, 175);
                            var_105.Size = UDim2.new(0, 115, 0, 25);
                            var_105.LayoutOrder = 5;
                            var_106.Name = "HoithoF";
                            var_106.Parent = var_105;
                            var_106.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_106.BackgroundTransparency = 1;
                            var_106.Size = UDim2.new(1, 0, 1, 25);
                            var_107.Name = "TextColor";
                            var_107.Parent = var_106;
                            var_107.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_107.BackgroundTransparency = 1;
                            var_107.Size = UDim2.new(0, 85, 0, 25);
                            var_107.Font = Enum.Font.GothamBold;
                            var_107.Text = "Breathing";
                            var_107.TextSize = 14;
                            var_107.TextXAlignment = Enum.TextXAlignment.Left;
                            var_107.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_107.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_108.Name = "setting_checkbox";
                            var_108.Parent = var_106;
                            var_108.AnchorPoint = Vector2.new(1, 0);
                            var_108.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_108.BackgroundTransparency = 1;
                            var_108.Position = UDim2.new(1, -5, 0, 0);
                            var_108.Size = UDim2.new(0, 25, 0, 25);
                            var_108.Image = "rbxassetid://4552505888";
                            var_108.ImageColor3 = Color3.fromRGB(131, 181, 255);
                            var_108.ImageColor3 = getgenv().ui_color["Toggle Border Color"];
                            table.insert(theme_listeners["Toggle Border Color"], function()
                                var_108.ImageColor3 = getgenv().ui_color["Toggle Border Color"];
                                return ;
                            end);
                            var_109.Name = "setting_check";
                            var_109.Parent = var_108;
                            var_109.AnchorPoint = Vector2.new(0, 1);
                            var_109.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_109.BackgroundTransparency = 1;
                            var_109.Position = UDim2.new(0, 0, 1, 0);
                            var_109.Image = "rbxassetid://4555411759";
                            var_109.ImageColor3 = getgenv().ui_color["Toggle Checked Color"];
                            table.insert(theme_listeners["Toggle Checked Color"], function()
                                var_109.ImageColor3 = getgenv().ui_color["Toggle Checked Color"];
                                return ;
                            end);
                            var_110.Name = "Hoithoitog";
                            var_110.Parent = var_106;
                            var_110.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_110.BackgroundTransparency = 1;
                            var_110.Size = UDim2.new(1, 0, 0, 25);
                            var_110.Font = Enum.Font.SourceSans;
                            var_110.Text = "";
                            var_110.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_110.TextSize = 14;
                            var_111.Parent = var_106;
                            var_111.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_111.BackgroundTransparency = 1;
                            var_111.Position = UDim2.new(0, 0, 0, 30);
                            var_111.Size = UDim2.new(1, 0, 0, 25);
                            var_112.Parent = var_111;
                            var_112.FillDirection = Enum.FillDirection.Horizontal;
                            var_112.SortOrder = Enum.SortOrder.LayoutOrder;
                            var_112.Padding = UDim.new(0, 5);
                            var_113.Name = "Cor1";
                            var_113.Parent = var_111;
                            var_113.BackgroundColor3 = custom_theme[var_67].Breathing.Color1;
                            var_113.Selectable = true;
                            var_113.Size = UDim2.new(0, 25, 0, 25);
                            var_114.CornerRadius = UDim.new(1, 0);
                            var_114.Parent = var_113;
                            var_115.Name = "BCor1";
                            var_115.Parent = var_113;
                            var_115.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_115.BackgroundTransparency = 1;
                            var_115.Size = UDim2.new(1, 0, 1, 0);
                            var_115.Font = Enum.Font.SourceSans;
                            var_115.Text = "";
                            var_115.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_115.TextSize = 14;
                            var_116.Name = "Cor2";
                            var_116.Parent = var_111;
                            var_116.BackgroundColor3 = custom_theme[var_67].Breathing.Color2;
                            var_116.Selectable = true;
                            var_116.Size = UDim2.new(0, 25, 0, 25);
                            var_117.CornerRadius = UDim.new(1, 0);
                            var_117.Parent = var_116;
                            var_118.Name = "BCor2";
                            var_118.Parent = var_116;
                            var_118.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_118.BackgroundTransparency = 1;
                            var_118.Size = UDim2.new(1, 0, 1, 0);
                            var_118.Font = Enum.Font.SourceSans;
                            var_118.Text = "";
                            var_118.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_118.TextSize = 14;
                            local flag_10 = false;
                            var_75.MouseButton1Click:Connect(function()
                                flag_10 = not flag_10;
                                local var_119 = flag_10 and UDim2.new(1, 0, 0, 255) or UDim2.new(1, 0, 0, 35);
                                tween_service_2:Create(var_68, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Size = var_119 }):Play();
                                return ;
                            end);
                            var_75.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            local game_2 = game:GetService("UserInputService");
                            local game_3 = game:GetService("RunService");
                            local local_player = game.Players.LocalPlayer:GetMouse();
                            local flag_11 = nil;
                            local flag_12 = nil;
                            local flag_13 = true;
                            local num = 0;
                            local func_5 = function(...)
                                if flag_13 then
                                    return wait(...);
                                end;
                                wait();
                                return false;
                            end;
                            local func_6 = function(arg_18)
                                return { math.floor(arg_18.r * 255), math.floor(arg_18.g * 255), math.floor(arg_18.b * 255) };
                            end;
                            local func_7 = function(arg_19)
                                local var_120 = arg_19:gsub("#", ""):upper():gsub("0X", "");
                                return Color3.fromRGB(tonumber(var_120:sub(1, 2), 16), tonumber(var_120:sub(3, 4), 16), tonumber(var_120:sub(5, 6), 16));
                            end;
                            local func_8 = function(arg_20)
                                local str_2 = string.format("%X", math.floor(arg_20.R * 255));
                                local str_3 = string.format("%X", math.floor(arg_20.G * 255));
                                local str_4 = string.format("%X", math.floor(arg_20.B * 255));
                                if #str_2 < 2 then
                                    str_2 = "0" .. str_2;
                                end;
                                if #str_3 < 2 then
                                    str_3 = "0" .. str_3;
                                end;
                                if #str_4 < 2 then
                                    str_4 = "0" .. str_4;
                                end;
                                return string.format("%s%s%s", str_2, str_3, str_4);
                            end;
                            local num_2 = 1;
                            local num_3 = 1;
                            local num_4 = 1;
                            local func_9 = function(arg_21, arg_22)
                                if arg_22 < arg_21 then
                                    return arg_21, arg_22;
                                end;
                                return arg_22, arg_21;
                            end;
                            local func_10 = function(arg_23, arg_24)
                                if arg_23 + arg_24 > 255 then
                                    local var_121, var_122 = func_9(arg_23, arg_24);
                                    local num_5 = 255 - var_121;
                                    local var_123, var_124 = func_9(num_5, var_122);
                                    return var_123 - var_124;
                                end;
                                return arg_23 + arg_24;
                            end;
                            cong_color = function(arg_25, arg_26)
                                local num_6 = math.sqrt;
                                local tbl_3 = { R = 255 - num_6(((255 - arg_25.R) ^ 2 + (255 - arg_26.R) ^ 2) / 2), G = 255 - num_6(((255 - arg_25.G) ^ 2 + (255 - arg_26.G) ^ 2) / 2), B = 255 - num_6(((255 - arg_25.B) ^ 2 + (255 - arg_26.B) ^ 2) / 2) };
                                return Color3.new(tbl_3.R, tbl_3.G, tbl_3.B);
                            end;
                            local func_11 = function(arg_27)
                                local var_125 = arg_27 or Color3.fromHSV(num_2, num_3, num_4);
                                if arg_27 then
                                    local var_126, var_127, var_128 = Color3.toHSV(arg_27);
                                    num_2 = var_126;
                                    num_3 = var_127;
                                    num_4 = var_128;
                                end;
                                var_93.Text = func_8(var_125);
                                var_102.BackgroundColor3 = Color3.fromHSV(num_2, 1, 1);
                                local var_129, var_130, var_131 = Color3.toHSV(var_125);
                                var_103.Position = UDim2.fromScale(var_130, 1 - var_131);
                                local num_7 = 1 - select(1, Color3.toHSV(var_125));
                                if math.abs(var_78.Position.Y.Scale - num_7) > 0.0001 then
                                    var_78.Position = UDim2.fromScale(0, num_7);
                                end;
                                local var_132, var_133, var_134 = table.unpack(func_6(var_125));
                                var_83.Text = var_132;
                                var_86.Text = var_133;
                                var_89.Text = var_134;
                                var_73.BackgroundColor3 = var_125;
                                getgenv().ui_color[var_67] = var_125;
                                return ;
                            end;
                            func_11(custom_theme[var_67].Color);
                            local func_12 = function(arg_28)
                                if flag_11 then
                                    flag_11 = flag_11:Disconnect() and nil or nil;
                                end;
                                if flag_12 then
                                    flag_12 = flag_12:Disconnect() and nil or nil;
                                end;
                                if arg_28 then
                                    pcall(function()
                                        local num_8 = 0.00392156862745098;
                                        while func_5() and var_65.Cungroi do
                                            num = num_8 + num;
                                            if num > 1 then
                                                num = 0;
                                            end;
                                            num_2 = num;
                                            func_11(Color3.fromHSV(num, 1, 1));
                                        end;
                                        return ;
                                    end);
                                end;
                                return ;
                            end;
                            local var_135 = var_65.Cungroi and UDim2.new(1, -4, 1, -4) or UDim2.new(0, 0, 0, 0);
                            local var_136 = var_65.Cungroi and UDim2.new(0.5, 0, 0.5, 0) or UDim2.new(0, 0, 1, 0);
                            local var_137 = var_65.Cungroi and Vector2.new(0.5, 0.5) or Vector2.new(0, 1);
                            var_100.Size = var_135;
                            var_100.Position = var_136;
                            var_100.AnchorPoint = var_137;
                            spawn(function()
                                func_12(var_65.Cungroi);
                                return ;
                            end);
                            var_101.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_101.MouseButton1Click:Connect(function()
                                var_65.Cungroi = not var_65.Cungroi;
                                var_135 = var_65.Cungroi and UDim2.new(1, -4, 1, -4) or UDim2.new(0, 0, 0, 0);
                                var_136 = var_65.Cungroi and UDim2.new(0.5, 0, 0.5, 0) or UDim2.new(0, 0, 1, 0);
                                var_137 = var_65.Cungroi and Vector2.new(0.5, 0.5) or Vector2.new(0, 1);
                                game.TweenService:Create(var_100, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Size = var_135, Position = var_136, AnchorPoint = var_137 }):Play();
                                func_12(var_65.Cungroi);
                                return ;
                            end);
                            var_93.FocusLost:Connect(function()
                                if #var_93.Text > 5 then
                                    local var_138, var_139 = pcall(func_7, var_93.Text);
                                    func_11(var_138 and var_139);
                                end;
                                return ;
                            end);
                            var_83.FocusLost:Connect(function()
                                if tonumber(var_83.Text) > 255 then
                                    var_83.Text = 255;
                                elseif tonumber(var_83.Text) < 0 then
                                    var_83.Text = 0;
                                end;
                                local var_140, var_141 = pcall(Color3.fromRGB, tonumber(var_83.Text), tonumber(var_89.Text), tonumber(var_86.Text));
                                func_11(var_140 and var_141);
                                return ;
                            end);
                            var_86.FocusLost:Connect(function()
                                if tonumber(var_86.Text) > 255 then
                                    var_86.Text = 255;
                                elseif tonumber(var_86.Text) < 0 then
                                    var_86.Text = 0;
                                end;
                                local var_142, var_143 = pcall(Color3.fromRGB, tonumber(var_83.Text), tonumber(var_89.Text), tonumber(var_86.Text));
                                func_11(var_142 and var_143);
                                return ;
                            end);
                            var_89.FocusLost:Connect(function()
                                if tonumber(var_89.Text) > 255 then
                                    var_89.Text = 255;
                                elseif tonumber(var_89.Text) < 0 then
                                    var_89.Text = 0;
                                end;
                                local var_144, var_145 = pcall(Color3.fromRGB, tonumber(var_83.Text), tonumber(var_89.Text), tonumber(var_86.Text));
                                func_11(var_144 and var_145);
                                return ;
                            end);
                            num_2 = 1 - math.clamp(var_76.Frame.AbsolutePosition.Y - var_76.AbsolutePosition.Y, 0, var_76.AbsoluteSize.Y) / var_76.AbsoluteSize.Y;
                            num_3 = math.clamp(var_102.SelectorColor.AbsolutePosition.X - var_102.AbsolutePosition.X, 0, var_102.AbsoluteSize.X) / var_102.AbsoluteSize.X;
                            num_4 = 1 - math.clamp(var_102.SelectorColor.AbsolutePosition.Y - var_102.AbsolutePosition.Y, 0, var_102.AbsoluteSize.Y) / var_102.AbsoluteSize.Y;
                            var_102.InputBegan:Connect(function(arg_29)
                                if arg_29.UserInputType == Enum.UserInputType.MouseButton1 then
                                    if flag_11 then
                                        flag_11:Disconnect();
                                    end;
                                    flag = true;
                                    flag_11 = game_3.RenderStepped:Connect(function()
                                        local num_9 = math.clamp(local_player.X - var_102.AbsolutePosition.X, 0, var_102.AbsoluteSize.X) / var_102.AbsoluteSize.X;
                                        local num_10 = math.clamp(local_player.Y - var_102.AbsolutePosition.Y, 0, var_102.AbsoluteSize.Y) / var_102.AbsoluteSize.Y;
                                        var_103.Position = UDim2.fromScale(num_9, num_10);
                                        num_3 = num_9;
                                        num_4 = 1 - num_10;
                                        func_11();
                                        return ;
                                    end);
                                end;
                                return ;
                            end);
                            var_102.InputEnded:Connect(function(arg_30)
                                if arg_30.UserInputType == Enum.UserInputType.MouseButton1 and flag_11 then
                                    flag = false;
                                    flag_11:Disconnect();
                                end;
                                return ;
                            end);
                            var_76.InputBegan:Connect(function(arg_31)
                                if arg_31.UserInputType == Enum.UserInputType.MouseButton1 then
                                    if flag_12 then
                                        flag_12:Disconnect();
                                    end;
                                    flag = true;
                                    flag_12 = game_3.RenderStepped:Connect(function()
                                        local num_11 = math.clamp(local_player.Y - var_76.AbsolutePosition.Y, 0, var_76.AbsoluteSize.Y) / var_76.AbsoluteSize.Y;
                                        var_76.Frame.Position = UDim2.fromScale(0, num_11);
                                        num_2 = 1 - num_11;
                                        func_11();
                                        return ;
                                    end);
                                end;
                                return ;
                            end);
                            var_76.InputEnded:Connect(function(arg_32)
                                if arg_32.UserInputType == Enum.UserInputType.MouseButton1 and flag_12 then
                                    flag = false;
                                    flag_12:Disconnect();
                                end;
                                return ;
                            end);
                            var_115.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_118.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_115.MouseButton1Click:Connect(function()
                                var_113.BackgroundColor3 = var_73.BackgroundColor3;
                                custom_theme[var_67].Breathing.Color1 = var_73.BackgroundColor3;
                                return ;
                            end);
                            var_118.MouseButton1Click:Connect(function()
                                var_116.BackgroundColor3 = var_73.BackgroundColor3;
                                custom_theme[var_67].Breathing.Color2 = var_73.BackgroundColor3;
                                return ;
                            end);
                            var_110.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            spawn(function()
                                while wait() do
                                    if custom_theme[var_67].Breathing.Toggle then
                                        func_11(var_73.BackgroundColor3);
                                    end;
                                end;
                                return ;
                            end);
                            local func_13 = function()
                                local var_146 = var_116.BackgroundColor3;
                                local var_147 = var_113.BackgroundColor3;
                                local var_148 = custom_theme[var_67].Breathing.Toggle and UDim2.new(1, -4, 1, -4) or UDim2.new(0, 0, 0, 0);
                                local var_149 = custom_theme[var_67].Breathing.Toggle and UDim2.new(0.5, 0, 0.5, 0) or UDim2.new(0, 0, 1, 0);
                                local var_150 = custom_theme[var_67].Breathing.Toggle and Vector2.new(0.5, 0.5) or Vector2.new(0, 1);
                                game.TweenService:Create(var_109, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Size = var_148, Position = var_149, AnchorPoint = var_150 }):Play();
                                if custom_theme[var_67].Breathing.Toggle then
                                    local tween = game.TweenService:Create(var_73, TweenInfo.new(2), { BackgroundColor3 = var_147 });
                                    local tween_2 = game.TweenService:Create(var_102, TweenInfo.new(2), { BackgroundColor3 = var_147 });
                                    tween:Play();
                                    tween_2:Play();
                                    tween.Completed:Connect(function()
                                        if custom_theme[var_67].Breathing.Toggle then
                                            local tween_3 = game.TweenService:Create(var_73, TweenInfo.new(2), { BackgroundColor3 = var_146 });
                                            local tween_4 = game.TweenService:Create(var_102, TweenInfo.new(2), { BackgroundColor3 = var_146 });
                                            tween_3:Play();
                                            tween_4:Play();
                                            if custom_theme[var_67].Breathing.Toggle then
                                                tween_3.Completed:Connect(function()
                                                    tween:Play();
                                                    tween_2:Play();
                                                    return ;
                                                end);
                                            end;
                                        end;
                                        return ;
                                    end);
                                end;
                                return ;
                            end;
                            spawn(function()
                                func_13();
                                return ;
                            end);
                            var_110.MouseButton1Click:Connect(function()
                                custom_theme[var_67].Breathing.Toggle = not custom_theme[var_67].Breathing.Toggle;
                                func_13();
                                return ;
                            end);
                            return ;
                        end,
                        create_box = function(arg_33)
                            local str_5 = tostring(arg_33.title) or "";
                            local str_6 = tostring(arg_33.placeholder) or "";
                            local var_151 = arg_33.type;
                            local var_152 = var_151 and getgenv().ui_color[var_151] or "";
                            local var_153 = Instance.new(frame_class);
                            local var_154 = Instance.new("UICorner");
                            local var_155 = Instance.new(frame_class);
                            local var_156 = Instance.new("UICorner");
                            local var_157 = Instance.new("TextLabel");
                            local var_158 = Instance.new(frame_class);
                            local var_159 = Instance.new("UICorner");
                            local var_160 = Instance.new("TextBox");
                            local var_161 = Instance.new(frame_class);
                            var_153.Name = "BoxFrame";
                            var_153.Parent = var_58;
                            var_153.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_153.BackgroundTransparency = 1;
                            var_153.Position = UDim2.new(0, 0, 0.208333328, 0);
                            var_153.Size = UDim2.new(1, 0, 0, 60);
                            var_154.CornerRadius = UDim.new(0, 4);
                            var_154.Name = "BoxCorner";
                            var_154.Parent = var_153;
                            var_155.Name = "Background1";
                            var_155.Parent = var_153;
                            var_155.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_155.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_155.Size = UDim2.new(1, -10, 1, 0);
                            var_155.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_155.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_155.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_155.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_156.CornerRadius = UDim.new(0, 4);
                            var_156.Name = "ButtonCorner";
                            var_156.Parent = var_155;
                            var_157.Name = "TextColor";
                            var_157.Parent = var_155;
                            var_157.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_157.BackgroundTransparency = 1;
                            var_157.Position = UDim2.new(0, 10, 0, 0);
                            var_157.Size = UDim2.new(1, -10, 0.5, 0);
                            var_157.Font = Enum.Font.GothamBlack;
                            var_157.Text = str_5;
                            var_157.TextSize = 14;
                            var_157.TextXAlignment = Enum.TextXAlignment.Left;
                            var_157.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_157.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_158.Name = "Background2";
                            var_158.Parent = var_155;
                            var_158.AnchorPoint = Vector2.new(1, 0.5);
                            var_158.ClipsDescendants = true;
                            var_158.Position = UDim2.new(1, -5, 0, 40);
                            var_158.Size = UDim2.new(1, -10, 0, 25);
                            var_158.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_158.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_159.CornerRadius = UDim.new(0, 4);
                            var_159.Name = "ButtonCorner";
                            var_159.Parent = var_158;
                            var_160.Name = "TextColorPlaceholder";
                            var_160.Parent = var_158;
                            var_160.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
                            var_160.BackgroundTransparency = 1;
                            var_160.Position = UDim2.new(0, 5, 0, 0);
                            var_160.Size = UDim2.new(1, -5, 1, 0);
                            var_160.Font = Enum.Font.GothamBold;
                            var_160.PlaceholderText = str_6;
                            var_160.Text = "";
                            var_160.TextSize = 14;
                            var_160.TextXAlignment = Enum.TextXAlignment.Left;
                            var_160.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                            var_160.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners["Placeholder Text Color"], function()
                                var_160.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                                return ;
                            end);
                            table.insert(theme_listeners[text_color_key], function()
                                var_160.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_161.Name = "Setting_Lineeeee";
                            var_161.Parent = var_158;
                            var_161.BackgroundTransparency = 1;
                            var_161.Position = UDim2.new(0, 0, 1, -2);
                            var_161.Size = UDim2.new(1, 0, 0, 6);
                            var_161.BackgroundColor3 = getgenv().ui_color["Textbox Highlight Color"];
                            table.insert(theme_listeners["Textbox Highlight Color"], function()
                                var_161.BackgroundColor3 = getgenv().ui_color["Textbox Highlight Color"];
                                return ;
                            end);
                            var_160.Focused:Connect(function()
                                tween_service_2:Create(var_161, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundTransparency = 0 }):Play();
                                return ;
                            end);
                            var_160.Focused:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_160.FocusLost:Connect(function()
                                tween_service_2:Create(var_161, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundTransparency = 1 }):Play();
                                local var_162 = var_160.Text;
                                if var_162 ~= "" and var_151 then
                                    getgenv().ui_color[var_151] = var_162;
                                    if var_151 == "Background Image" then
                                        ui_library.reload_main(var_162);
                                    end;
                                end;
                                return ;
                            end);
                            local tbl_4 = {};
                            if var_151 and var_152 ~= "" then
                                var_160.Text = tostring(var_152);
                            end;
                            tbl_4.set_value = function(arg_34)
                                local str_7 = tostring(arg_34 or "");
                                var_160.Text = str_7;
                                if var_151 then
                                    getgenv().ui_color[var_151] = str_7;
                                    if var_151 == "Background Image" then
                                        ui_library.reload_main(str_7);
                                    end;
                                end;
                                return ;
                            end;
                            return tbl_4;
                        end,
                        create_slider = function(arg_35)
                            local str_8 = tostring(arg_35.title) or "";
                            local num_12 = tonumber(arg_35.min) or 0;
                            local num_13 = tonumber(arg_35.max) or 100;
                            local var_163 = arg_35.precise or false;
                            local var_164 = getgenv().ui_color[arg_35.type] or 0;
                            local func_14 = function(arg_36)
                                getgenv().ui_color[arg_35.type] = arg_36;
                                return ;
                            end;
                            local num_14 = 600;
                            local var_165 = Instance.new(frame_class);
                            local var_166 = Instance.new("UICorner");
                            local var_167 = Instance.new(frame_class);
                            local var_168 = Instance.new("UICorner");
                            local var_169 = Instance.new("TextLabel");
                            local var_170 = Instance.new(frame_class);
                            local var_171 = Instance.new("TextButton");
                            local var_172 = Instance.new("UICorner");
                            local var_173 = Instance.new(frame_class);
                            local var_174 = Instance.new("UICorner");
                            local var_175 = Instance.new(frame_class);
                            local var_176 = Instance.new("UICorner");
                            local var_177 = Instance.new("TextBox");
                            var_165.Name = str_8 .. "buda";
                            var_165.Parent = var_58;
                            var_165.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_165.BackgroundTransparency = 1;
                            var_165.Position = UDim2.new(0, 0, 0.208333328, 0);
                            var_165.Size = UDim2.new(1, 0, 0, 50);
                            var_166.CornerRadius = UDim.new(0, 4);
                            var_166.Name = "SliderCorner";
                            var_166.Parent = var_165;
                            var_167.Name = "Background1";
                            var_167.Parent = var_165;
                            var_167.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_167.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_167.Size = UDim2.new(1, -10, 1, 0);
                            var_167.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_167.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_167.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_167.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_168.CornerRadius = UDim.new(0, 4);
                            var_168.Name = "SliderBGCorner";
                            var_168.Parent = var_167;
                            var_169.Name = "TextColor";
                            var_169.Parent = var_167;
                            var_169.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_169.BackgroundTransparency = 1;
                            var_169.Position = UDim2.new(0, 10, 0, 0);
                            var_169.Size = UDim2.new(1, -10, 0, 25);
                            var_169.Font = Enum.Font.GothamBlack;
                            var_169.Text = str_8;
                            var_169.TextSize = 14;
                            var_169.TextXAlignment = Enum.TextXAlignment.Left;
                            var_169.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_169.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_170.Name = "SliderBar";
                            var_170.Parent = var_165;
                            var_170.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_170.Position = UDim2.new(0.5, 0, 0.5, 14);
                            var_170.Size = UDim2.new(0, 600, 0, 6);
                            var_170.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_170.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_171.Name = "SliderButton ";
                            var_171.Parent = var_170;
                            var_171.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_171.BackgroundTransparency = 1;
                            var_171.Size = UDim2.new(1, 0, 1, 0);
                            var_171.Font = Enum.Font.GothamBold;
                            var_171.Text = "";
                            var_171.TextColor3 = Color3.fromRGB(230, 230, 230);
                            var_171.TextSize = 14;
                            var_172.CornerRadius = UDim.new(1, 0);
                            var_172.Name = "SliderBarCorner";
                            var_172.Parent = var_170;
                            var_173.Name = "Bar";
                            var_173.BorderSizePixel = 0;
                            var_173.Parent = var_170;
                            var_173.Size = UDim2.new(0, 0, 1, 0);
                            var_173.BackgroundColor3 = getgenv().ui_color["Slider Line Color"];
                            table.insert(theme_listeners["Slider Line Color"], function()
                                var_173.BackgroundColor3 = getgenv().ui_color["Slider Line Color"];
                                return ;
                            end);
                            var_174.CornerRadius = UDim.new(1, 0);
                            var_174.Name = "BarCorner";
                            var_174.Parent = var_173;
                            var_175.Name = "Background2";
                            var_175.Parent = var_165;
                            var_175.AnchorPoint = Vector2.new(1, 0);
                            var_175.Position = UDim2.new(1, -10, 0, 5);
                            var_175.Size = UDim2.new(0, 150, 0, 25);
                            var_175.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_175.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_176.CornerRadius = UDim.new(0, 4);
                            var_176.Name = "Sliderbox";
                            var_176.Parent = var_175;
                            var_177.Name = "TextColor";
                            var_177.Parent = var_175;
                            var_177.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_177.BackgroundTransparency = 1;
                            var_177.Size = UDim2.new(1, 0, 1, 0);
                            var_177.Font = Enum.Font.GothamBold;
                            var_177.Text = "";
                            var_177.TextSize = 14;
                            var_177.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_177.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_171.MouseEnter:Connect(function()
                                tween_service_2:Create(var_173, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundColor3 = getgenv().ui_color["Slider Highlight Color"] }):Play();
                                return ;
                            end);
                            var_171.MouseLeave:Connect(function()
                                tween_service_2:Create(var_173, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundColor3 = getgenv().ui_color["Slider Line Color"] }):Play();
                                return ;
                            end);
                            local local_player_2 = game.Players.LocalPlayer:GetMouse();
                            if var_164 then
                                if var_164 <= num_12 then
                                    var_164 = num_12;
                                elseif num_13 <= var_164 then
                                    var_164 = num_13;
                                end;
                                var_173.Size = UDim2.new(1 - (num_13 - var_164) / (num_13 - num_12), 0, 0, 6);
                                var_177.Text = var_164;
                                func_14(var_164);
                            end;
                            var_171.MouseButton1Down:Connect(function()
                                local var_178 = var_163 and tonumber(string.format("%.1f", (tonumber(num_13) - tonumber(num_12)) / num_14 * var_173.AbsoluteSize.X + tonumber(num_12))) or math.floor((tonumber(num_13) - tonumber(num_12)) / num_14 * var_173.AbsoluteSize.X + tonumber(num_12));
                                pcall(function()
                                    func_14(var_178);
                                    var_177.Text = var_178;
                                    return ;
                                end);
                                var_173.Size = UDim2.new(0, math.clamp(local_player_2.X - var_173.AbsolutePosition.X, 0, num_14), 0, 6);
                                moveconnection = local_player_2.Move:Connect(function()
                                    local var_179 = var_163 and tonumber(string.format("%.1f", (tonumber(num_13) - tonumber(num_12)) / num_14 * var_173.AbsoluteSize.X + tonumber(num_12))) or math.floor((tonumber(num_13) - tonumber(num_12)) / num_14 * var_173.AbsoluteSize.X + tonumber(num_12));
                                    pcall(function()
                                        func_14(var_179);
                                        var_177.Text = var_179;
                                        return ;
                                    end);
                                    var_173.Size = UDim2.new(0, math.clamp(local_player_2.X - var_173.AbsolutePosition.X, 0, num_14), 0, 6);
                                    return ;
                                end);
                                releaseconnection = user_input_service.InputEnded:Connect(function(arg_37)
                                    if arg_37.UserInputType == Enum.UserInputType.MouseButton1 then
                                        local var_180 = var_163 and tonumber(string.format("%.1f", (tonumber(num_13) - tonumber(num_12)) / num_14 * var_173.AbsoluteSize.X + tonumber(num_12))) or math.floor((tonumber(num_13) - tonumber(num_12)) / num_14 * var_173.AbsoluteSize.X + tonumber(num_12));
                                        pcall(function()
                                            func_14(var_180);
                                            var_177.Text = var_180;
                                            return ;
                                        end);
                                        var_173.Size = UDim2.new(0, math.clamp(local_player_2.X - var_173.AbsolutePosition.X, 0, num_14), 0, 6);
                                        moveconnection:Disconnect();
                                        releaseconnection:Disconnect();
                                    end;
                                    return ;
                                end);
                                return ;
                            end);
                            local func_15 = function(arg_38)
                                if tonumber(arg_38) <= num_12 then
                                    var_173.Size = UDim2.new(0, 0 * num_14, 0, 6);
                                    var_177.Text = num_12;
                                    func_14(tonumber(num_12));
                                elseif tonumber(arg_38) <= num_13 then
                                    var_173.Size = UDim2.new(0, num_13 / num_13 * num_14, 0, 6);
                                    var_177.Text = num_13;
                                    func_14(tonumber(num_13));
                                else
                                    var_173.Size = UDim2.new(1 - (num_13 - arg_38) / (num_13 - num_12), 0, 0, 6);
                                    func_14(tonumber(arg_38));
                                end;
                                return ;
                            end;
                            var_177.FocusLost:Connect(function()
                                func_15(var_177.Text);
                                return ;
                            end);
                            return {
                                set_value = function(arg_39)
                                    func_15(arg_39);
                                    return ;
                                end
                            };
                        end
                    };
                end
            };
        end;
        local var_181 = breathing_connections.CreateCustomColor();
        local var_182 = var_181.create_section("Main");
        var_182.create_color_picker({ title = "Border Color", type = "Border Color" });
        var_182.create_color_picker({ title = "Click Effect Color", type = "Click Effect Color" });
        var_182.create_color_picker({ title = "Setting Icon Color", type = "Setting Icon Color" });
        var_182.create_box({ title = "Logo Image", placeholder = "URL Here (PNG only), Recommended 128x128", type = "Logo Image" });
        local var_183 = var_181.create_section("Search");
        var_183.create_color_picker({ title = "Search Icon Color", type = "Search Icon Color" });
        var_183.create_color_picker({ title = "Search Icon Highlight Color", type = "Search Icon Highlight Color" });
        local var_184 = var_181.create_section("Text");
        var_184.create_color_picker({ title = "GUI Text Color", type = "GUI Text Color" });
        var_184.create_color_picker({ title = text_color_key, type = text_color_key });
        var_184.create_color_picker({ title = "Placeholder Text Color", type = "Placeholder Text Color" });
        var_184.create_color_picker({ title = "Title Text Color", type = "Title Text Color" });
        local var_185 = var_181.create_section("Background");
        var_185.create_color_picker({ title = "Background 1 Color", type = "Background 1 Color" });
        var_185.create_slider({ title = "Background 1 Transparency", type = "Background 1 Transparency", min = 0, max = 1, default = 0, precise = true });
        var_185.create_color_picker({ title = "Background 2 Color", type = "Background 2 Color" });
        var_185.create_color_picker({ title = "Background 3 Color", type = "Background 3 Color" });
        var_185.create_box({ title = "Background Image", placeholder = "URL Here (WEBM / PNG only), Recommended 1280x720", type = "Background Image" });
        var_181.create_section("Page").create_color_picker({ title = "Page Selected Color", type = "Page Selected Color" });
        local var_186 = var_181.create_section("Section");
        var_186.create_color_picker({ title = "Section Text Color", type = "Section Text Color" });
        var_186.create_color_picker({ title = "Section Underline Color", type = "Section Underline Color" });
        local var_187 = var_181.create_section("Toggle");
        var_187.create_color_picker({ title = "Toggle Border Color", type = "Toggle Border Color" });
        var_187.create_color_picker({ title = "Toggle Checked Color", type = "Toggle Checked Color" });
        var_187.create_color_picker({ title = "Toggle Desc Color", type = "Toggle Desc Color" });
        var_181.create_section("Button").create_color_picker({ title = "Button Color", type = "Button Color" });
        var_181.create_section("Label").create_color_picker({ title = "Label Color", type = "Label Color" });
        local var_188 = var_181.create_section("Dropdown");
        var_188.create_color_picker({ title = "Dropdown Icon Color", type = "Dropdown Icon Color" });
        var_188.create_color_picker({ title = "Dropdown Selected Color", type = "Dropdown Selected Color" });
        var_181.create_section("Textbox").create_color_picker({ title = "Textbox Highlight Color", type = "Textbox Highlight Color" });
        var_181.create_section("Box").create_color_picker({ title = "Box Highlight Color", type = "Box Highlight Color" });
        local var_189 = var_181.create_section("Slider");
        var_189.create_color_picker({ title = "Slider Line Color", type = "Slider Line Color" });
        var_189.create_color_picker({ title = "Slider Highlight Color", type = "Slider Highlight Color" });
        local var_190 = var_181.create_section("Animation");
        var_190.create_slider({ title = "Tween Animation 1 Speed", type = "Tween Animation 1 Speed", min = 0, max = 0.75, default = 0.25, precise = true });
        var_190.create_slider({ title = "Tween Animation 2 Speed", type = "Tween Animation 2 Speed", min = 0, max = 1, default = 0.5, precise = true });
        var_190.create_slider({ title = "Tween Animation 3 Speed", type = "Tween Animation 3 Speed", min = 0, max = 0.5, default = 0.1, precise = true });
        local tbl_5 = {};
        local num_15 = -1;
        local num_16 = -1;
        local num_17 = 1;
        tbl_5.create_page = function(arg_40)
            local str_9 = tostring(arg_40.page_name);
            local var_191 = str_9;
            local str_10 = tostring(arg_40.page_title);
            num_16 = num_16 + 1;
            num_15 = num_15 + 1;
            local var_192 = Instance.new(frame_class);
            local var_193 = Instance.new(frame_class);
            local var_194 = Instance.new("UICorner");
            local var_195 = Instance.new(frame_class);
            local var_196 = Instance.new(frame_class);
            local var_197 = Instance.new("UICorner");
            local var_198 = Instance.new(frame_class);
            local var_199 = Instance.new("TextLabel");
            local var_200 = Instance.new("TextButton");
            var_192.Name = str_9 .. "_Control";
            var_192.Parent = var_25;
            var_192.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_192.BackgroundTransparency = 1;
            var_192.Size = UDim2.new(1, -10, 0, 25);
            var_192.LayoutOrder = num_15;
            var_193.Parent = var_192;
            var_193.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_193.BackgroundTransparency = 1;
            var_193.Position = UDim2.new(0, 5, 0, 0);
            var_193.Size = UDim2.new(1, -5, 1, 0);
            var_194.CornerRadius = UDim.new(0, 4);
            var_194.Name = "TabNameCorner";
            var_194.Parent = var_193;
            var_195.Name = "Line";
            var_195.Parent = var_193;
            var_195.AnchorPoint = Vector2.new(0, 0.5);
            var_195.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_195.BackgroundTransparency = 1;
            var_195.Position = UDim2.new(0, 0, 0.5, 0);
            var_195.Size = UDim2.new(0, 14, 1, 0);
            var_196.Name = "PageInLine";
            var_196.Parent = var_195;
            var_196.AnchorPoint = Vector2.new(0.5, 0.5);
            var_196.BorderSizePixel = 0;
            var_196.Position = UDim2.new(0.5, 0, 0.5, 0);
            var_196.Size = UDim2.new(1, -10, 0, 0);
            var_196.BackgroundColor3 = getgenv().ui_color["Page Selected Color"];
            table.insert(theme_listeners["Page Selected Color"], function()
                var_196.BackgroundColor3 = getgenv().ui_color["Page Selected Color"];
                return ;
            end);
            var_197.Name = "LineCorner";
            var_197.Parent = var_196;
            var_198.Name = "TabTitleContainer";
            var_198.Parent = var_193;
            var_198.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_198.BackgroundTransparency = 1;
            var_198.Position = UDim2.new(0, 15, 0, 0);
            var_198.Size = UDim2.new(1, -15, 1, 0);
            var_199.Name = "GUITextColor";
            var_199.Parent = var_198;
            var_199.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_199.BackgroundTransparency = 1;
            var_199.Size = UDim2.new(1, 0, 1, 0);
            var_199.Font = Enum.Font.GothamBold;
            var_199.Text = str_9;
            var_199.TextColor3 = Color3.fromRGB(230, 230, 230);
            var_199.TextSize = 14;
            var_199.TextXAlignment = Enum.TextXAlignment.Left;
            var_199.TextColor3 = getgenv().ui_color["GUI Text Color"];
            table.insert(theme_listeners["GUI Text Color"], function()
                var_199.TextColor3 = getgenv().ui_color["GUI Text Color"];
                return ;
            end);
            var_200.Name = "PageButton";
            var_200.Parent = var_192;
            var_200.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_200.BackgroundTransparency = 1;
            var_200.Size = UDim2.new(1, 0, 1, 0);
            var_200.Font = Enum.Font.SourceSans;
            var_200.Text = "";
            var_200.TextColor3 = Color3.fromRGB(0, 0, 0);
            var_200.TextSize = 14;
            local var_201 = Instance.new(frame_class);
            local var_202 = Instance.new("UICorner");
            local var_203 = Instance.new("TextLabel");
            local var_204 = Instance.new("ScrollingFrame");
            local var_205 = Instance.new("UIListLayout");
            local var_206 = num_17;
            num_17 = num_17 + 1;
            var_201.Name = "Page" .. var_206;
            var_201.Parent = var_28;
            var_201.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
            var_201.Position = UDim2.new(0, 190, 0, 30);
            var_201.Size = UDim2.new(0, 435, 0, 325);
            var_201.LayoutOrder = num_16;
            table.insert(theme_listeners["Background 1 Color"], function()
                var_201.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                return ;
            end);
            var_201.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
            table.insert(theme_listeners["Background 1 Transparency"], function()
                var_201.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                return ;
            end);
            var_202.CornerRadius = UDim.new(0, 4);
            var_202.Parent = var_201;
            var_203.Name = "GUITextColor";
            var_203.Parent = var_201;
            var_203.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_203.BackgroundTransparency = 1;
            var_203.Position = UDim2.new(0, 5, 0, 0);
            var_203.Size = UDim2.new(1, 0, 0, 25);
            var_203.Font = Enum.Font.GothamBold;
            var_203.Text = str_10;
            var_203.TextSize = 16;
            var_203.TextXAlignment = Enum.TextXAlignment.Left;
            var_203.TextColor3 = getgenv().ui_color["GUI Text Color"];
            table.insert(theme_listeners["GUI Text Color"], function()
                var_203.TextColor3 = getgenv().ui_color["GUI Text Color"];
                return ;
            end);
            var_204.Name = "PageList";
            var_204.Parent = var_201;
            var_204.Active = true;
            var_204.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
            var_204.BackgroundTransparency = 1;
            var_204.BorderColor3 = Color3.fromRGB(27, 42, 53);
            var_204.BorderSizePixel = 0;
            var_204.Position = UDim2.new(0, 5, 0, 30);
            var_204.Size = UDim2.new(1, -10, 1, -30);
            var_204.BottomImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
            var_204.ScrollBarThickness = 5;
            var_204.TopImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
            var_205.Name = "Pagelistlayout";
            var_205.Parent = var_204;
            var_205.SortOrder = Enum.SortOrder.LayoutOrder;
            var_205.Padding = UDim.new(0, 5);
            var_205:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
                var_204.CanvasSize = UDim2.new(0, 0, 0, var_205.AbsoluteContentSize.Y + 5);
                return ;
            end);
            local var_207 = Instance.new(frame_class);
            local var_208 = Instance.new("UICorner");
            local var_209 = Instance.new(frame_class);
            local var_210 = Instance.new("ImageLabel");
            local var_211 = Instance.new("TextButton");
            local var_212 = Instance.new("TextBox");
            var_207.Name = "Background2";
            var_207.Parent = var_201;
            var_207.AnchorPoint = Vector2.new(1, 0);
            var_207.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
            var_207.Position = UDim2.new(1, -5, 0, 5);
            var_207.Size = UDim2.new(0, 20, 0, 20);
            var_207.ClipsDescendants = true;
            table.insert(theme_listeners["Background 2 Color"], function()
                var_207.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                return ;
            end);
            var_208.CornerRadius = UDim.new(0, 2);
            var_208.Name = "PageSearchCorner";
            var_208.Parent = var_207;
            var_209.Name = "SearchFrame";
            var_209.Parent = var_207;
            var_209.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
            var_209.BackgroundTransparency = 1;
            var_209.Size = UDim2.new(0, 20, 0, 20);
            var_210.Name = "SearchIcon";
            var_210.Parent = var_209;
            var_210.AnchorPoint = Vector2.new(0.5, 0.5);
            var_210.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
            var_210.BackgroundTransparency = 1;
            var_210.Position = UDim2.new(0.5, 0, 0.5, 0);
            var_210.Size = UDim2.new(0, 16, 0, 16);
            var_210.Image = "rbxassetid://8154282545";
            var_210.ImageColor3 = getgenv().ui_color["Search Icon Color"];
            table.insert(theme_listeners["Search Icon Color"], function()
                var_210.ImageColor3 = getgenv().ui_color["Search Icon Color"];
                return ;
            end);
            var_211.Name = "active";
            var_211.Parent = var_209;
            var_211.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
            var_211.BackgroundTransparency = 1;
            var_211.Size = UDim2.new(1, 0, 1, 0);
            var_211.Font = Enum.Font.SourceSans;
            var_211.Text = "";
            var_211.TextColor3 = Color3.fromRGB(0, 0, 0);
            var_211.TextSize = 14;
            var_212.Name = "TextColorPlaceholder";
            var_212.Parent = var_207;
            var_212.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
            var_212.BackgroundTransparency = 1;
            var_212.Position = UDim2.new(0, 30, 0, 0);
            var_212.Size = UDim2.new(1, -30, 1, 0);
            var_212.Font = Enum.Font.GothamBold;
            var_212.Text = "";
            var_212.TextSize = 14;
            var_212.TextXAlignment = Enum.TextXAlignment.Left;
            var_212.PlaceholderText = "Search Section name";
            var_212.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
            var_212.TextColor3 = getgenv().ui_color[text_color_key];
            table.insert(theme_listeners["Placeholder Text Color"], function()
                var_212.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                return ;
            end);
            table.insert(theme_listeners[text_color_key], function()
                var_212.TextColor3 = getgenv().ui_color[text_color_key];
                return ;
            end);
            local flag_14 = false;
            var_211.MouseEnter:Connect(function()
                tween_service_2:Create(var_210, TweenInfo.new(getgenv().ui_color["Tween Animation 3 Speed"]), { ImageColor3 = getgenv().ui_color["Search Icon Highlight Color"] }):Play();
                return ;
            end);
            var_211.MouseLeave:Connect(function()
                tween_service_2:Create(var_210, TweenInfo.new(getgenv().ui_color["Tween Animation 3 Speed"]), { ImageColor3 = getgenv().ui_color["Search Icon Color"] }):Play();
                return ;
            end);
            var_211.MouseButton1Click:Connect(function()
                ui_library.button_effect();
                return ;
            end);
            var_212.Focused:Connect(function()
                ui_library.button_effect();
                return ;
            end);
            var_211.MouseButton1Click:Connect(function()
                flag_14 = not flag_14;
                local var_213 = flag_14 and UDim2.new(0, 175, 0, 20) or UDim2.new(0, 20, 0, 20);
                game.TweenService:Create(var_207, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Size = var_213 }):Play();
                return ;
            end);
            local func_16 = function()
                for var_214, var_215 in next, var_204:GetChildren() do
                    if not var_215:IsA("UIListLayout") then
                        var_215.Visible = false;
                    end;
                end;
                return ;
            end;
            local func_17 = function()
                for key_6, value_8 in pairs(var_204:GetChildren()) do
                    if not value_8:IsA("UIListLayout") and string.find(string.lower(value_8.Name), string.lower(var_212.Text)) then
                        value_8.Visible = true;
                    end;
                end;
                return ;
            end;
            var_212:GetPropertyChangedSignal("Text"):Connect(function()
                func_16();
                func_17();
                return ;
            end);
            for key_7, value_9 in pairs(var_25:GetChildren()) do
                if not value_9:IsA("UIListLayout") and key_7 == 2 then
                    value_9.Frame.Line.PageInLine.Size = UDim2.new(1, -10, 1, -10);
                    oldlay = value_9.LayoutOrder;
                    oldobj = value_9;
                end;
            end;
            var_200.MouseButton1Click:Connect(function()
                ui_library.button_effect();
                if tostring(var_29.CurrentPage) == var_201.Name then
                    return ;
                end;
                local var_216 = getgenv().ui_color["Tween Animation 1 Speed"] or 0.25;
                local tween_info = TweenInfo.new(var_216, Enum.EasingStyle.Quad, Enum.EasingDirection.Out);
                for key_8, value_10 in ipairs(var_25:GetChildren()) do
                    if value_10:IsA(frame_class) and value_10:FindFirstChild(frame_class) then
                        local child = value_10.Frame:FindFirstChild("Line");
                        if child and child:FindFirstChild("PageInLine") then
                            local var_217 = child.PageInLine;
                            tween_service_2:Create(var_217, tween_info, { Size = UDim2.new(1, -10, 0, 0), Position = UDim2.new(0.5, 0, 1, 0), AnchorPoint = Vector2.new(0.5, 1) }):Play();
                        end;
                    end;
                end;
                tween_service_2:Create(var_196, tween_info, { Size = UDim2.new(1, -10, 1, -10), Position = UDim2.new(0.5, 0, 0.5, 0), AnchorPoint = Vector2.new(0.5, 0.5) }):Play();
                for key_9, value_11 in ipairs(var_28:GetChildren()) do
                    if value_11:IsA(frame_class) then
                        value_11.Visible = value_11 == var_201;
                    end;
                end;
                local child_2 = var_201:FindFirstChild("PageList");
                if child_2 and child_2:IsA("ScrollingFrame") then
                    child_2.CanvasPosition = Vector2.new(0, 0);
                end;
                var_29:JumpTo(var_201);
                return ;
            end);
            return {
                create_section = function(arg_41, arg_42)
                    local str_11 = tostring(arg_41);
                    local flag_15 = true;
                    if arg_42 ~= nil then
                        flag_15 = game.PlaceId == 11424731604;
                    end;
                    if not flag_15 then
                        return {
                            create_toggle = function()
                                return {
                                    set_stage = function()
                                        return ;
                                    end,
                                    set_keybind = function()
                                        return ;
                                    end,
                                    get_keybind = function()
                                        return nil;
                                    end
                                };
                            end,
                            create_button = function()
                                return ;
                            end,
                            create_label = function()
                                return {
                                    set_text = function()
                                        return ;
                                    end,
                                    set_color = function()
                                        return ;
                                    end
                                };
                            end,
                            create_dropdown = function()
                                return {
                                    clear_text = function()
                                        return ;
                                    end,
                                    get_new_list = function()
                                        return ;
                                    end,
                                    rf = function()
                                        return ;
                                    end
                                };
                            end,
                            create_bind = function()
                                return ;
                            end,
                            create_box = function()
                                return {
                                    set_value = function()
                                        return ;
                                    end
                                };
                            end,
                            create_slider = function()
                                return {
                                    set_value = function()
                                        return ;
                                    end
                                };
                            end
                        };
                    end;
                    local var_218 = Instance.new(frame_class);
                    local var_219 = Instance.new("UICorner");
                    local var_220 = Instance.new(frame_class);
                    local var_221 = Instance.new("TextLabel");
                    local var_222 = Instance.new(frame_class);
                    local var_223 = Instance.new("UIGradient");
                    local var_224 = Instance.new("UIListLayout");
                    var_218.Name = arg_41 .. "_Dot";
                    var_218.Parent = var_204;
                    var_218.Size = UDim2.new(0, 415, 0, 100);
                    var_218.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                    table.insert(theme_listeners["Background 3 Color"], function()
                        var_218.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                        return ;
                    end);
                    var_218.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                    table.insert(theme_listeners["Background 1 Transparency"], function()
                        var_218.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                        return ;
                    end);
                    var_219.CornerRadius = UDim.new(0, 4);
                    var_219.Parent = var_218;
                    var_220.Name = "Topsec";
                    var_220.Parent = var_218;
                    var_220.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                    var_220.BackgroundTransparency = 1;
                    var_220.Size = UDim2.new(0, 415, 0, 30);
                    var_221.Name = "Sectiontitle";
                    var_221.Parent = var_220;
                    var_221.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                    var_221.BackgroundTransparency = 1;
                    var_221.Size = UDim2.new(1, 0, 1, 0);
                    var_221.Font = Enum.Font.GothamBold;
                    var_221.Text = arg_41;
                    var_221.TextSize = 14;
                    var_221.TextColor3 = getgenv().ui_color["Section Text Color"];
                    table.insert(theme_listeners["Section Text Color"], function()
                        var_221.TextColor3 = getgenv().ui_color["Section Text Color"];
                        return ;
                    end);
                    var_222.Name = "Linesec";
                    var_222.Parent = var_220;
                    var_222.AnchorPoint = Vector2.new(0.5, 1);
                    var_222.BorderSizePixel = 0;
                    var_222.Position = UDim2.new(0.5, 0, 1, -2);
                    var_222.Size = UDim2.new(1, -10, 0, 2);
                    var_222.BackgroundColor3 = getgenv().ui_color["Section Underline Color"];
                    table.insert(theme_listeners["Section Underline Color"], function()
                        var_222.BackgroundColor3 = getgenv().ui_color["Section Underline Color"];
                        return ;
                    end);
                    var_223.Transparency = NumberSequence.new({ NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5, 0), NumberSequenceKeypoint.new(0.51, 0.02), NumberSequenceKeypoint.new(1, 1) });
                    var_223.Parent = var_222;
                    var_224.Name = "SectionList";
                    var_224.Parent = var_218;
                    var_224.SortOrder = Enum.SortOrder.LayoutOrder;
                    var_224.Padding = UDim.new(0, 5);
                    var_224:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
                        var_218.Size = UDim2.new(1, -5, 0, var_224.AbsoluteContentSize.Y + 5);
                        return ;
                    end);
                    return {
                        create_toggle = function(arg_43, arg_44)
                            local str_12 = tostring(arg_43.title);
                            local var_225 = arg_43.desc;
                            local var_226 = arg_43.default;
                            local var_227 = arg_43.Keybind or false;
                            local var_228 = arg_43.DefaultKey or nil;
                            local var_229 = arg_43.Textbox or false;
                            local var_230 = arg_43.TextboxPlaceholder or "Enter value...";
                            local var_231 = arg_43.TextboxDefault or "";
                            local var_232 = arg_43.TextboxCallback or function()
                                return ;
                            end;
                            local var_233 = arg_43.Requirements ~= nil;
                            local var_234 = arg_43.Requirements or {};
                            local var_235 = arg_43.RequirementUpdateInterval or 1;
                            local var_236 = arg_44 or function()
                                return ;
                            end;
                            local var_237 = Instance.new(frame_class);
                            local var_238 = Instance.new(frame_class);
                            local var_239 = Instance.new("ImageLabel");
                            local var_240 = Instance.new("ImageLabel");
                            local var_241 = Instance.new("TextLabel");
                            local var_242 = Instance.new("TextLabel");
                            local var_243 = Instance.new(frame_class);
                            local var_244 = Instance.new("UICorner");
                            local var_245 = Instance.new("TextButton");
                            local var_246 = Instance.new("UIListLayout");
                            local var_247 = Instance.new(frame_class);
                            local var_248 = Instance.new("TextButton");
                            local var_249 = Instance.new("UICorner");
                            local var_250 = Instance.new("UIStroke");
                            local var_251 = Instance.new(frame_class);
                            local var_252 = Instance.new("TextBox");
                            local var_253 = Instance.new("UICorner");
                            local var_254 = Instance.new("UIStroke");
                            local var_255 = Instance.new(frame_class);
                            local var_256 = Instance.new(frame_class);
                            local var_257 = Instance.new("UIListLayout");
                            local tbl_6 = {};
                            var_237.Name = "ToggleFrame";
                            var_237.Parent = var_218;
                            var_237.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_237.BackgroundTransparency = 1;
                            var_237.Position = UDim2.new(0, 0, 0.300000012, 0);
                            var_237.Size = UDim2.new(1, 0, 0, 0);
                            var_237.AutomaticSize = Enum.AutomaticSize.Y;
                            var_238.Name = "TogFrame1";
                            var_238.Parent = var_237;
                            var_238.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_238.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_238.BackgroundTransparency = 1;
                            var_238.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_238.Size = UDim2.new(1, -10, 0, 0);
                            var_238.AutomaticSize = Enum.AutomaticSize.Y;
                            var_239.Name = "checkbox";
                            var_239.Parent = var_238;
                            var_239.AnchorPoint = Vector2.new(1, 0.5);
                            var_239.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_239.BackgroundTransparency = 1;
                            var_239.Position = UDim2.new(1, -5, 0.5, 3);
                            var_239.Size = UDim2.new(0, 25, 0, 25);
                            var_239.Image = "rbxassetid://4552505888";
                            var_239.ImageColor3 = getgenv().ui_color["Toggle Border Color"];
                            var_239.ZIndex = 3;
                            table.insert(theme_listeners["Toggle Border Color"], function()
                                var_239.ImageColor3 = getgenv().ui_color["Toggle Border Color"];
                                return ;
                            end);
                            var_240.Name = "check";
                            var_240.Parent = var_239;
                            var_240.AnchorPoint = Vector2.new(0, 1);
                            var_240.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_240.BackgroundTransparency = 1;
                            var_240.Position = UDim2.new(0, 0, 1, 0);
                            var_240.Image = "rbxassetid://4555411759";
                            var_240.ImageColor3 = getgenv().ui_color["Toggle Checked Color"];
                            var_240.ZIndex = 3;
                            table.insert(theme_listeners["Toggle Checked Color"], function()
                                var_240.ImageColor3 = getgenv().ui_color["Toggle Checked Color"];
                                return ;
                            end);
                            if var_227 then
                                var_247.Name = "KeybindFrame";
                                var_247.Parent = var_238;
                                var_247.AnchorPoint = Vector2.new(1, 0.5);
                                var_247.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                var_247.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                var_247.Position = UDim2.new(1, -35, 0.5, 3);
                                var_247.Size = UDim2.new(0, 45, 0, 20);
                                var_247.ZIndex = 2;
                                table.insert(theme_listeners["Background 1 Color"], function()
                                    var_247.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                    return ;
                                end);
                                table.insert(theme_listeners["Background 1 Transparency"], function()
                                    var_247.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                    return ;
                                end);
                                var_249.CornerRadius = UDim.new(0, 4);
                                var_249.Parent = var_247;
                                var_250.Name = "KeybindBorder";
                                var_250.Parent = var_247;
                                var_250.Color = getgenv().ui_color["Toggle Border Color"];
                                var_250.Thickness = 1;
                                var_250.ApplyStrokeMode = Enum.ApplyStrokeMode.Border;
                                table.insert(theme_listeners["Toggle Border Color"], function()
                                    var_250.Color = getgenv().ui_color["Toggle Border Color"];
                                    return ;
                                end);
                                var_248.Name = "KeybindButton";
                                var_248.Parent = var_247;
                                var_248.BackgroundTransparency = 1;
                                var_248.Size = UDim2.new(1, 0, 1, 0);
                                var_248.Font = Enum.Font.GothamBold;
                                var_248.Text = var_228 or "...";
                                var_248.TextColor3 = getgenv().ui_color[text_color_key];
                                var_248.TextSize = 10;
                                var_248.TextWrapped = true;
                                var_248.ZIndex = 2;
                                table.insert(theme_listeners[text_color_key], function()
                                    var_248.TextColor3 = getgenv().ui_color[text_color_key];
                                    return ;
                                end);
                            end;
                            if var_229 then
                                var_251.Name = "TextboxFrame";
                                var_251.Parent = var_238;
                                var_251.AnchorPoint = Vector2.new(1, 0.5);
                                var_251.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                var_251.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                var_251.Position = UDim2.new(1, var_227 and -85 or -35, 0.5, 3);
                                var_251.Size = UDim2.new(0, 60, 0, 20);
                                var_251.ZIndex = 2;
                                table.insert(theme_listeners["Background 1 Color"], function()
                                    var_251.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                    return ;
                                end);
                                table.insert(theme_listeners["Background 1 Transparency"], function()
                                    var_251.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                    return ;
                                end);
                                var_253.CornerRadius = UDim.new(0, 4);
                                var_253.Parent = var_251;
                                var_254.Name = "TextboxBorder";
                                var_254.Parent = var_251;
                                var_254.Color = getgenv().ui_color["Toggle Border Color"];
                                var_254.Thickness = 1;
                                var_254.ApplyStrokeMode = Enum.ApplyStrokeMode.Border;
                                table.insert(theme_listeners["Toggle Border Color"], function()
                                    var_254.Color = getgenv().ui_color["Toggle Border Color"];
                                    return ;
                                end);
                                var_252.Name = "TextboxInput";
                                var_252.Parent = var_251;
                                var_252.BackgroundTransparency = 1;
                                var_252.Position = UDim2.new(0, 6, 0, 0);
                                var_252.Size = UDim2.new(1, -12, 1, 0);
                                var_252.Font = Enum.Font.Gotham;
                                var_252.PlaceholderText = var_230;
                                var_252.Text = var_231;
                                var_252.TextColor3 = getgenv().ui_color[text_color_key];
                                var_252.PlaceholderColor3 = getgenv().ui_color["Toggle Desc Color"];
                                var_252.TextSize = 12;
                                var_252.TextXAlignment = Enum.TextXAlignment.Left;
                                var_252.ClearTextOnFocus = false;
                                var_252.ZIndex = 2;
                                table.insert(theme_listeners[text_color_key], function()
                                    var_252.TextColor3 = getgenv().ui_color[text_color_key];
                                    return ;
                                end);
                                table.insert(theme_listeners["Toggle Desc Color"], function()
                                    var_252.PlaceholderColor3 = getgenv().ui_color["Toggle Desc Color"];
                                    return ;
                                end);
                                var_252.FocusLost:Connect(function(arg_45)
                                    if arg_45 then
                                        var_232(var_252.Text);
                                    end;
                                    return ;
                                end);
                            end;
                            local num_18 = 5;
                            if var_225 then
                                num_18 = 0;
                                var_241.Name = "ToggleDesc";
                                var_241.Parent = var_238;
                                var_241.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                                var_241.BackgroundTransparency = 1;
                                var_241.Position = UDim2.new(0, 15, 0, 20);
                                var_241.Size = UDim2.new(1, (var_227 and -90 or -50) - (var_229 and 70 or 0), 0, 0);
                                var_241.Font = Enum.Font.GothamBlack;
                                var_241.Text = var_225;
                                var_241.TextSize = 13;
                                var_241.TextWrapped = true;
                                var_241.TextXAlignment = Enum.TextXAlignment.Left;
                                var_241.RichText = true;
                                var_241.AutomaticSize = Enum.AutomaticSize.Y;
                                var_241.TextColor3 = getgenv().ui_color["Toggle Desc Color"];
                                table.insert(theme_listeners["Toggle Desc Color"], function()
                                    var_241.TextColor3 = getgenv().ui_color["Toggle Desc Color"];
                                    return ;
                                end);
                            else
                                var_241.Text = "";
                            end;
                            var_242.Name = "TextColor";
                            var_242.Parent = var_238;
                            var_242.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_242.BackgroundTransparency = 1;
                            var_242.Position = UDim2.new(0, 10, 0, num_18);
                            var_242.Size = UDim2.new(1, -10, 0, 20);
                            var_242.Font = Enum.Font.GothamBlack;
                            var_242.Text = str_12;
                            var_242.TextSize = 14;
                            var_242.TextXAlignment = Enum.TextXAlignment.Left;
                            var_242.TextYAlignment = Enum.TextYAlignment.Center;
                            var_242.RichText = true;
                            var_242.AutomaticSize = Enum.AutomaticSize.Y;
                            var_242.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_242.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_243.Name = "Background1";
                            var_243.Parent = var_238;
                            var_243.Size = UDim2.new(1, 0, 1, 6);
                            var_243.ZIndex = 0;
                            var_243.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_243.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_243.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_243.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_244.CornerRadius = UDim.new(0, 4);
                            var_244.Name = "ToggleCorner";
                            var_244.Parent = var_243;
                            var_245.Name = "ToggleButton";
                            var_245.Parent = var_238;
                            var_245.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_245.BackgroundTransparency = 1;
                            var_245.Size = UDim2.new(1, 0, 1, 6);
                            var_245.Font = Enum.Font.SourceSans;
                            var_245.Text = "";
                            var_245.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_245.TextSize = 14;
                            var_246.Name = "ToggleList";
                            var_246.Parent = var_237;
                            var_246.HorizontalAlignment = Enum.HorizontalAlignment.Center;
                            var_246.SortOrder = Enum.SortOrder.LayoutOrder;
                            var_246.VerticalAlignment = Enum.VerticalAlignment.Center;
                            var_246.Padding = UDim.new(0, 0);
                            local flag_16 = true;
                            local flag_17 = nil;
                            if var_233 and #var_234 > 0 then
                                var_244.CornerRadius = UDim.new(0, 4);
                                var_255.Name = "RequirementsContainer";
                                var_255.Parent = var_238;
                                var_255.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                var_255.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                var_255.Size = UDim2.new(1, 0, 0, 0);
                                var_255.AutomaticSize = Enum.AutomaticSize.Y;
                                var_255.Position = UDim2.new(0, 0, 1, 4);
                                var_255.ZIndex = 0;
                                table.insert(theme_listeners["Background 1 Color"], function()
                                    var_255.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                    return ;
                                end);
                                table.insert(theme_listeners["Background 1 Transparency"], function()
                                    var_255.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                    return ;
                                end);
                                local var_258 = Instance.new("UICorner");
                                var_258.CornerRadius = UDim.new(0, 4);
                                var_258.Parent = var_255;
                                var_256.Name = "RequirementsInner";
                                var_256.Parent = var_255;
                                var_256.BackgroundColor3 = Color3.fromRGB(0, 0, 0);
                                var_256.BackgroundTransparency = 0.85;
                                var_256.Size = UDim2.new(1, -12, 0, 0);
                                var_256.AutomaticSize = Enum.AutomaticSize.Y;
                                var_256.Position = UDim2.new(0, 6, 0, 6);
                                var_256.ZIndex = 1;
                                local var_259 = Instance.new("UICorner");
                                var_259.CornerRadius = UDim.new(0, 3);
                                var_259.Parent = var_256;
                                local var_260 = Instance.new("UIPadding");
                                var_260.PaddingTop = UDim.new(0, 6);
                                var_260.PaddingBottom = UDim.new(0, 6);
                                var_260.PaddingLeft = UDim.new(0, 8);
                                var_260.PaddingRight = UDim.new(0, 8);
                                var_260.Parent = var_256;
                                local var_261 = Instance.new("UIPadding");
                                var_261.PaddingBottom = UDim.new(0, 6);
                                var_261.Parent = var_255;
                                var_257.Name = "RequirementsLayout";
                                var_257.Parent = var_256;
                                var_257.FillDirection = Enum.FillDirection.Vertical;
                                var_257.HorizontalAlignment = Enum.HorizontalAlignment.Left;
                                var_257.SortOrder = Enum.SortOrder.LayoutOrder;
                                var_257.Padding = UDim.new(0, 4);
                                for key_10, value_12 in ipairs(var_234) do
                                    local var_262 = Instance.new(frame_class);
                                    var_262.Name = "Req_" .. value_12;
                                    var_262.Parent = var_256;
                                    var_262.BackgroundTransparency = 1;
                                    var_262.Size = UDim2.new(1, 0, 0, 16);
                                    var_262.LayoutOrder = key_10;
                                    local var_263 = Instance.new("ImageLabel");
                                    var_263.Name = "Icon";
                                    var_263.Parent = var_262;
                                    var_263.BackgroundTransparency = 1;
                                    var_263.Size = UDim2.new(0, 14, 0, 14);
                                    var_263.Position = UDim2.new(0, 0, 0.5, 0);
                                    var_263.AnchorPoint = Vector2.new(0, 0.5);
                                    var_263.Image = "rbxassetid://7072725342";
                                    var_263.ImageColor3 = Color3.fromRGB(255, 85, 85);
                                    var_263.ZIndex = 2;
                                    local var_264 = Instance.new("TextLabel");
                                    var_264.Name = "Text";
                                    var_264.Parent = var_262;
                                    var_264.BackgroundTransparency = 1;
                                    var_264.Size = UDim2.new(1, -22, 1, 0);
                                    var_264.Position = UDim2.new(0, 20, 0, 0);
                                    var_264.Font = Enum.Font.Gotham;
                                    var_264.Text = value_12;
                                    var_264.TextSize = 11;
                                    var_264.TextXAlignment = Enum.TextXAlignment.Left;
                                    var_264.TextColor3 = getgenv().ui_color["Toggle Desc Color"];
                                    var_264.ZIndex = 2;
                                    table.insert(theme_listeners["Toggle Desc Color"], function()
                                        var_264.TextColor3 = getgenv().ui_color["Toggle Desc Color"];
                                        return ;
                                    end);
                                    tbl_6[value_12] = { Frame = var_262, Icon = var_263, Text = var_264, Met = false };
                                end;
                                local func_18 = function()
                                    local flag_18 = true;
                                    for key_11, value_13 in pairs(tbl_6) do
                                        local var_265 = requirements_tracker:check(key_11);
                                        value_13.Met = var_265;
                                        if var_265 then
                                            value_13.Icon.Image = "rbxassetid://7072706620";
                                            value_13.Icon.ImageColor3 = Color3.fromRGB(85, 255, 127);
                                        else
                                            value_13.Icon.Image = "rbxassetid://7072725342";
                                            value_13.Icon.ImageColor3 = Color3.fromRGB(255, 85, 85);
                                            flag_18 = false;
                                        end;
                                    end;
                                    flag_16 = flag_18;
                                    if not flag_18 then
                                        var_239.ImageTransparency = 0.5;
                                        var_240.ImageTransparency = 0.5;
                                    else
                                        var_239.ImageTransparency = 0;
                                        var_240.ImageTransparency = 0;
                                    end;
                                    return ;
                                end;
                                func_18();
                                flag_17 = task.spawn(function()
                                    while true do
                                        task.wait(var_235);
                                        if not var_237 or not var_237.Parent then
                                            break;
                                        end;
                                        func_18();
                                    end;
                                    return ;
                                end);
                            end;
                            local var_266 = var_228;
                            local flag_19 = false;
                            local func_19 = function(arg_46)
                                if var_233 and (#var_234 > 0 and arg_46 and not flag_16) then
                                    return ;
                                end;
                                local var_267 = arg_46 and UDim2.new(1, -4, 1, -4) or UDim2.new(0, 0, 0, 0);
                                local var_268 = arg_46 and UDim2.new(0.5, 0, 0.5, 0) or UDim2.new(0, 0, 1, 0);
                                local var_269 = arg_46 and Vector2.new(0.5, 0.5) or Vector2.new(0, 1);
                                game.TweenService:Create(var_240, TweenInfo.new(getgenv().ui_color["Tween Animation 1 Speed"]), { Size = var_267, Position = var_268, AnchorPoint = var_269 }):Play();
                                var_236(arg_46);
                                return ;
                            end;
                            if var_236 then
                                func_19(var_226);
                            end;
                            var_245.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_245.MouseButton1Down:Connect(function()
                                if var_233 and (#var_234 > 0 and not var_226 and not flag_16) then
                                    return ;
                                end;
                                var_226 = not var_226;
                                func_19(var_226);
                                return ;
                            end);
                            if var_227 then
                                local game_4 = game:GetService("UserInputService");
                                var_248.MouseButton1Click:Connect(function()
                                    if not flag_19 then
                                        flag_19 = true;
                                        var_248.Text = "...";
                                        local flag_20 = nil;
                                        flag_20 = game_4.InputBegan:Connect(function(arg_47, arg_48)
                                            if not arg_48 and arg_47.UserInputType == Enum.UserInputType.Keyboard then
                                                local var_270 = arg_47.KeyCode.Name;
                                                var_266 = var_270;
                                                var_248.Text = var_270;
                                                flag_19 = false;
                                                flag_20:Disconnect();
                                            end;
                                            return ;
                                        end);
                                    end;
                                    return ;
                                end);
                                game_4.InputBegan:Connect(function(arg_49, arg_50)
                                    if not arg_50 and (not flag_19 and arg_49.UserInputType == Enum.UserInputType.Keyboard and var_266 and arg_49.KeyCode.Name == var_266) then
                                        if var_233 and #var_234 > 0 and not var_226 and not flag_16 then
                                            return ;
                                        end;
                                        var_226 = not var_226;
                                        func_19(var_226);
                                    end;
                                    return ;
                                end);
                            end;
                            local tbl_8 = {
                                set_stage = function(arg_51)
                                    func_19(arg_51);
                                    return ;
                                end,
                                set_keybind = function(arg_52)
                                    if var_227 then
                                        var_266 = arg_52;
                                        var_248.Text = arg_52 or "NONE";
                                    end;
                                    return ;
                                end,
                                get_keybind = function()
                                    return var_266;
                                end,
                                set_textbox_value = function(arg_53)
                                    if var_229 then
                                        var_252.Text = arg_53;
                                    end;
                                    return ;
                                end,
                                get_textbox_value = function()
                                    if var_229 then
                                        return var_252.Text;
                                    end;
                                    return nil;
                                end,
                                are_requirements_met = function()
                                    return flag_16;
                                end,
                                get_requirements = function()
                                    local tbl_7 = {};
                                    for key_12, value_14 in pairs(tbl_6) do
                                        tbl_7[key_12] = value_14.Met;
                                    end;
                                    return tbl_7;
                                end,
                                Destroy = function()
                                    if flag_17 then
                                        task.cancel(flag_17);
                                    end;
                                    var_237:Destroy();
                                    return ;
                                end
                            };
                            local var_271 = make_element_id(var_191, str_11, str_12);
                            element_registry.toggles[var_271] = {
                                get = function()
                                    return var_226;
                                end,
                                set = function(arg_54)
                                    local var_272 = not not arg_54;
                                    if var_226 == var_272 then
                                        return ;
                                    end;
                                    if var_233 and #var_234 > 0 and var_272 and not flag_16 then
                                        return ;
                                    end;
                                    var_226 = var_272;
                                    func_19(var_226);
                                    return ;
                                end
                            };
                            return tbl_8;
                        end,
                        create_account_table = function(arg_55, arg_56)
                            local str_13 = tostring(arg_55.title) or "Account Manager";
                            local var_273 = arg_55.default or {};
                            local var_274 = arg_56 or function()
                                return ;
                            end;
                            local var_275 = arg_55.enable_config_binding ~= false;
                            local var_276 = arg_55.on_account_select;
                            local var_277 = make_element_id(var_191, str_11, str_13);
                            local tbl_9 = {};
                            if type(var_273) == "table" then
                                for key_13, value_15 in pairs(var_273) do
                                    if type(value_15) == "table" then
                                        table.insert(tbl_9, { username = key_13, link = value_15.link or "", config = value_15.config or "" });
                                    else
                                        table.insert(tbl_9, { username = key_13, link = value_15, config = "" });
                                    end;
                                end;
                            end;
                            local game_5 = game:GetService("TweenService");
                            local var_278 = getgenv().ui_color["Tween Animation 2 Speed"];
                            local var_279 = Instance.new(frame_class);
                            local var_280 = Instance.new("UICorner");
                            var_279.Name = str_13 .. "_Wrapper";
                            var_279.Parent = var_218;
                            var_279.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_279.BackgroundTransparency = 1;
                            var_279.Position = UDim2.new(0, 0, 0, 0);
                            var_279.Size = UDim2.new(1, 0, 0, 280);
                            var_279.ClipsDescendants = true;
                            local var_281 = Instance.new(frame_class);
                            local var_282 = Instance.new("UICorner");
                            var_281.Name = "Background1";
                            var_281.Parent = var_279;
                            var_281.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_281.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_281.Size = UDim2.new(1, -10, 1, 0);
                            var_281.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            var_281.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_281.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_281.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_282.CornerRadius = UDim.new(0, 4);
                            var_282.Parent = var_281;
                            local var_283 = Instance.new("TextLabel");
                            local var_284 = Instance.new("TextButton");
                            var_283.Name = "Title";
                            var_283.Parent = var_281;
                            var_283.BackgroundTransparency = 1;
                            var_283.Position = UDim2.new(0, 10, 0, 5);
                            var_283.Size = UDim2.new(1, -40, 0, 25);
                            var_283.Font = Enum.Font.GothamBlack;
                            var_283.Text = str_13;
                            var_283.TextSize = 14;
                            var_283.TextXAlignment = Enum.TextXAlignment.Left;
                            var_283.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_283.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_284.Name = "Minimize";
                            var_284.Parent = var_281;
                            var_284.BackgroundTransparency = 1;
                            var_284.Position = UDim2.new(1, -30, 0, 5);
                            var_284.Size = UDim2.new(0, 25, 0, 25);
                            var_284.Font = Enum.Font.GothamBold;
                            var_284.Text = "-";
                            var_284.TextSize = 18;
                            var_284.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_284.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            local var_285 = Instance.new(frame_class);
                            local var_286 = Instance.new("UICorner");
                            var_285.Name = "ContentArea";
                            var_285.Parent = var_281;
                            var_285.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                            var_285.Position = UDim2.new(0, 5, 0, 35);
                            var_285.Size = UDim2.new(1, -10, 1, -40);
                            table.insert(theme_listeners["Background 3 Color"], function()
                                var_285.BackgroundColor3 = getgenv().ui_color["Background 3 Color"];
                                return ;
                            end);
                            var_286.CornerRadius = UDim.new(0, 4);
                            var_286.Parent = var_285;
                            local var_287 = Instance.new("ScrollingFrame");
                            local var_288 = Instance.new("UIListLayout");
                            var_287.Name = "List";
                            var_287.Parent = var_285;
                            var_287.Active = true;
                            var_287.BackgroundTransparency = 1;
                            var_287.BorderSizePixel = 0;
                            var_287.Position = UDim2.new(0, 5, 0, 5);
                            var_287.Size = UDim2.new(1, -10, 1, -80);
                            var_287.ScrollBarThickness = 2;
                            var_287.CanvasSize = UDim2.new(0, 0, 0, 0);
                            var_287.BottomImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
                            var_287.TopImage = "rbxasset://textures/ui/Scroll/scroll-middle.png";
                            var_288.Parent = var_287;
                            var_288.SortOrder = Enum.SortOrder.LayoutOrder;
                            var_288.Padding = UDim.new(0, 4);
                            var_288:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
                                var_287.CanvasSize = UDim2.new(0, 0, 0, var_288.AbsoluteContentSize.Y + 5);
                                return ;
                            end);
                            local var_289 = Instance.new(frame_class);
                            var_289.Parent = var_285;
                            var_289.BackgroundTransparency = 1;
                            var_289.AnchorPoint = Vector2.new(0, 1);
                            var_289.Position = UDim2.new(0, 5, 1, -5);
                            var_289.Size = UDim2.new(1, -10, 0, 65);
                            local func_20 = function(arg_57, arg_58, arg_59, arg_60)
                                local var_290 = Instance.new(frame_class);
                                local var_291 = Instance.new("UICorner");
                                local var_292 = Instance.new("TextBox");
                                local var_293 = Instance.new(frame_class);
                                var_290.Name = arg_57 .. frame_class;
                                var_290.Parent = var_289;
                                var_290.BackgroundColor3 = getgenv().ui_color["Background Main Color"];
                                var_290.Position = arg_59;
                                var_290.Size = arg_60;
                                table.insert(theme_listeners["Background Main Color"], function()
                                    var_290.BackgroundColor3 = getgenv().ui_color["Background Main Color"];
                                    return ;
                                end);
                                var_291.CornerRadius = UDim.new(0, 4);
                                var_291.Parent = var_290;
                                var_292.Parent = var_290;
                                var_292.BackgroundTransparency = 1;
                                var_292.Size = UDim2.new(1, -10, 1, 0);
                                var_292.Position = UDim2.new(0, 5, 0, 0);
                                var_292.Font = Enum.Font.GothamBold;
                                var_292.Text = "";
                                var_292.PlaceholderText = arg_58;
                                var_292.TextSize = 12;
                                var_292.TextXAlignment = Enum.TextXAlignment.Left;
                                var_292.TextColor3 = getgenv().ui_color[text_color_key];
                                var_292.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                                table.insert(theme_listeners[text_color_key], function()
                                    var_292.TextColor3 = getgenv().ui_color[text_color_key];
                                    return ;
                                end);
                                table.insert(theme_listeners["Placeholder Text Color"], function()
                                    var_292.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                                    return ;
                                end);
                                var_293.Parent = var_290;
                                var_293.BackgroundColor3 = getgenv().ui_color["Box Highlight Color"];
                                var_293.BackgroundTransparency = 1;
                                var_293.Position = UDim2.new(0, 0, 1, -2);
                                var_293.Size = UDim2.new(1, 0, 0, 2);
                                table.insert(theme_listeners["Box Highlight Color"], function()
                                    var_293.BackgroundColor3 = getgenv().ui_color["Box Highlight Color"];
                                    return ;
                                end);
                                var_292.Focused:Connect(function()
                                    ui_library.button_effect();
                                    game_5:Create(var_293, TweenInfo.new(0.3), { BackgroundTransparency = 0 }):Play();
                                    return ;
                                end);
                                var_292.FocusLost:Connect(function()
                                    game_5:Create(var_293, TweenInfo.new(0.3), { BackgroundTransparency = 1 }):Play();
                                    return ;
                                end);
                                return var_292;
                            end;
                            local var_294 = func_20("User", "Username", UDim2.new(0, 0, 0, 0), UDim2.new(0.35, 0, 0, 30));
                            local var_295 = func_20("Link", "PS Code", UDim2.new(0.35, 5, 0, 0), UDim2.new(0.65, -45, 0, 30));
                            local var_296 = func_20("Config", "Config Name (optional)", UDim2.new(0, 0, 0, 35), UDim2.new(1, -45, 0, 30));
                            local var_297 = Instance.new("TextButton");
                            local var_298 = Instance.new("UICorner");
                            var_297.Name = "AddBtn";
                            var_297.Parent = var_289;
                            var_297.AnchorPoint = Vector2.new(1, 0);
                            var_297.Position = UDim2.new(1, 0, 0, 0);
                            var_297.Size = UDim2.new(0, 35, 0, 65);
                            var_297.BackgroundColor3 = getgenv().ui_color["Button Color"];
                            var_297.Text = "+";
                            var_297.Font = Enum.Font.GothamBold;
                            var_297.TextSize = 18;
                            var_297.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners["Button Color"], function()
                                var_297.BackgroundColor3 = getgenv().ui_color["Button Color"];
                                return ;
                            end);
                            table.insert(theme_listeners[text_color_key], function()
                                var_297.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_298.CornerRadius = UDim.new(0, 4);
                            var_298.Parent = var_297;
                            var_297.MouseEnter:Connect(function()
                                game_5:Create(var_297, TweenInfo.new(0.2), { BackgroundTransparency = 0.2 }):Play();
                                return ;
                            end);
                            var_297.MouseLeave:Connect(function()
                                game_5:Create(var_297, TweenInfo.new(0.2), { BackgroundTransparency = 0 }):Play();
                                return ;
                            end);
                            local func_21 = function(arg_61, arg_62, arg_63, arg_64)
                                local var_299 = Instance.new(frame_class);
                                local var_300 = Instance.new("UICorner");
                                local var_301 = Instance.new("TextLabel");
                                local var_302 = Instance.new("TextLabel");
                                local var_303 = Instance.new("TextLabel");
                                local var_304 = Instance.new("TextButton");
                                local var_305 = Instance.new("UICorner");
                                local var_306 = Instance.new("TextButton");
                                local var_307 = Instance.new("UICorner");
                                var_299.Name = arg_61;
                                var_299.Parent = var_287;
                                var_299.LayoutOrder = arg_64;
                                var_299.Size = UDim2.new(1, 0, 0, 50);
                                var_299.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                table.insert(theme_listeners["Background 1 Color"], function()
                                    var_299.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                    return ;
                                end);
                                var_300.CornerRadius = UDim.new(0, 4);
                                var_300.Parent = var_299;
                                var_301.Parent = var_299;
                                var_301.BackgroundTransparency = 1;
                                var_301.Position = UDim2.new(0, 8, 0, 4);
                                var_301.Size = UDim2.new(1, -80, 0, 16);
                                var_301.Font = Enum.Font.GothamBold;
                                var_301.Text = arg_61;
                                var_301.TextSize = 13;
                                var_301.TextXAlignment = Enum.TextXAlignment.Left;
                                var_301.TextColor3 = getgenv().ui_color[text_color_key];
                                table.insert(theme_listeners[text_color_key], function()
                                    var_301.TextColor3 = getgenv().ui_color[text_color_key];
                                    return ;
                                end);
                                var_302.Parent = var_299;
                                var_302.BackgroundTransparency = 1;
                                var_302.Position = UDim2.new(0, 8, 0, 20);
                                var_302.Size = UDim2.new(0.5, -10, 0, 14);
                                var_302.Font = Enum.Font.Gotham;
                                var_302.Text = arg_62 and arg_62 ~= "" and "PS: " .. arg_62 or "No PS Code";
                                var_302.TextSize = 10;
                                var_302.TextXAlignment = Enum.TextXAlignment.Left;
                                var_302.TextColor3 = getgenv().ui_color["Toggle Desc Color"];
                                table.insert(theme_listeners["Toggle Desc Color"], function()
                                    var_302.TextColor3 = getgenv().ui_color["Toggle Desc Color"];
                                    return ;
                                end);
                                var_303.Parent = var_299;
                                var_303.BackgroundTransparency = 1;
                                var_303.Position = UDim2.new(0, 8, 0, 34);
                                var_303.Size = UDim2.new(0.6, -10, 0, 14);
                                var_303.Font = Enum.Font.Gotham;
                                var_303.Text = arg_63 and arg_63 ~= "" and "Config: " .. arg_63 or "No Config";
                                var_303.TextSize = 10;
                                var_303.TextXAlignment = Enum.TextXAlignment.Left;
                                var_303.TextColor3 = arg_63 and arg_63 ~= "" and Color3.fromRGB(100, 200, 100) or getgenv().ui_color["Toggle Desc Color"];
                                if var_275 and (arg_63 and arg_63 ~= "") then
                                    var_306.Parent = var_299;
                                    var_306.AnchorPoint = Vector2.new(1, 0.5);
                                    var_306.Position = UDim2.new(1, -40, 0.5, 0);
                                    var_306.Size = UDim2.new(0, 30, 0, 30);
                                    var_306.BackgroundColor3 = Color3.fromRGB(60, 140, 60);
                                    var_306.Text = "+";
                                    var_306.Font = Enum.Font.GothamBold;
                                    var_306.TextSize = 16;
                                    var_306.TextColor3 = Color3.fromRGB(255, 255, 255);
                                    var_307.CornerRadius = UDim.new(0, 4);
                                    var_307.Parent = var_306;
                                    var_306.MouseEnter:Connect(function()
                                        game_5:Create(var_306, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(80, 180, 80) }):Play();
                                        return ;
                                    end);
                                    var_306.MouseLeave:Connect(function()
                                        game_5:Create(var_306, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(60, 140, 60) }):Play();
                                        return ;
                                    end);
                                    var_306.MouseButton1Click:Connect(function()
                                        ui_library.button_effect();
                                        if getgenv().feral_config and getgenv().feral_config.load then
                                            local var_308, var_309 = getgenv().feral_config.load(arg_63);
                                            if var_308 then
                                                if breathing_connections and breathing_connections.create_noti then
                                                    breathing_connections.create_noti({ title = "Config", desc = "Loaded \"" .. arg_63 .. "\" for " .. arg_61, show_time = 4 });
                                                end;
                                            elseif breathing_connections and breathing_connections.create_noti then
                                                breathing_connections.create_noti({ title = "Config", desc = "Failed to load: " .. tostring(var_309), show_time = 4 });
                                            end;
                                        end;
                                        if var_276 then
                                            var_276(arg_61, arg_62, arg_63);
                                        end;
                                        return ;
                                    end);
                                end;
                                var_304.Parent = var_299;
                                var_304.AnchorPoint = Vector2.new(1, 0.5);
                                var_304.Position = UDim2.new(1, -5, 0.5, 0);
                                var_304.Size = UDim2.new(0, 30, 0, 30);
                                var_304.BackgroundColor3 = Color3.fromRGB(200, 60, 60);
                                var_304.Text = "X";
                                var_304.Font = Enum.Font.GothamBold;
                                var_304.TextSize = 14;
                                var_304.TextColor3 = Color3.fromRGB(255, 255, 255);
                                var_305.CornerRadius = UDim.new(0, 4);
                                var_305.Parent = var_304;
                                var_304.MouseEnter:Connect(function()
                                    game_5:Create(var_304, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(230, 80, 80) }):Play();
                                    return ;
                                end);
                                var_304.MouseLeave:Connect(function()
                                    game_5:Create(var_304, TweenInfo.new(0.2), { BackgroundColor3 = Color3.fromRGB(200, 60, 60) }):Play();
                                    return ;
                                end);
                                var_304.MouseButton1Click:Connect(function()
                                    ui_library.button_effect();
                                    var_299:Destroy();
                                    for key_14, value_16 in ipairs(tbl_9) do
                                        if value_16.username == arg_61 then
                                            table.remove(tbl_9, key_14);
                                            break;
                                        end;
                                    end;
                                    local tbl_10 = {};
                                    for key_15, value_17 in ipairs(tbl_9) do
                                        tbl_10[value_17.username] = { link = value_17.link, config = value_17.config };
                                    end;
                                    var_274(tbl_10);
                                    return ;
                                end);
                                return ;
                            end;
                            local func_22 = function()
                                for key_16, value_18 in pairs(var_287:GetChildren()) do
                                    if value_18:IsA(frame_class) then
                                        value_18:Destroy();
                                    end;
                                end;
                                table.sort(tbl_9, function(arg_65, arg_66)
                                    return arg_65.username:lower() < arg_66.username:lower();
                                end);
                                for key_17, value_19 in ipairs(tbl_9) do
                                    func_21(value_19.username, value_19.link, value_19.config, key_17);
                                end;
                                return ;
                            end;
                            var_297.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                local var_310 = var_294.Text;
                                local var_311 = var_295.Text;
                                local var_312 = var_296.Text;
                                if var_310:gsub(" ", "") ~= "" then
                                    for key_18, value_20 in ipairs(tbl_9) do
                                        if value_20.username == var_310 then
                                            return ;
                                        end;
                                    end;
                                    table.insert(tbl_9, { username = var_310, link = var_311, config = var_312 });
                                    func_22();
                                    var_294.Text = "";
                                    var_295.Text = "";
                                    var_296.Text = "";
                                    local tbl_11 = {};
                                    for key_19, value_21 in ipairs(tbl_9) do
                                        tbl_11[value_21.username] = { link = value_21.link, config = value_21.config };
                                    end;
                                    var_274(tbl_11);
                                end;
                                return ;
                            end);
                            local flag_21 = false;
                            local udim = UDim2.new(1, 0, 0, 280);
                            local udim_2 = UDim2.new(1, 0, 0, 35);
                            var_284.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                flag_21 = not flag_21;
                                var_284.Text = flag_21 and "+" or "-";
                                local var_313 = flag_21 and udim_2 or udim;
                                game_5:Create(var_279, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), { Size = var_313 }):Play();
                                if flag_21 then
                                    var_285.Visible = false;
                                else
                                    var_285.Visible = true;
                                end;
                                return ;
                            end);
                            var_284.MouseEnter:Connect(function()
                                game_5:Create(var_284, TweenInfo.new(0.2), { TextColor3 = getgenv().ui_color["Button Color"] }):Play();
                                return ;
                            end);
                            var_284.MouseLeave:Connect(function()
                                game_5:Create(var_284, TweenInfo.new(0.2), { TextColor3 = getgenv().ui_color[text_color_key] }):Play();
                                return ;
                            end);
                            func_22();
                            if var_275 then
                                task.spawn(function()
                                    task.wait(0.5);
                                    local game_6 = game:GetService("Players").LocalPlayer;
                                    if game_6 then
                                        for key_20, value_22 in ipairs(tbl_9) do
                                            if value_22.username == game_6.Name and value_22.config and value_22.config ~= "" then
                                                if getgenv().feral_config and getgenv().feral_config.load then
                                                    local var_314, var_315 = getgenv().feral_config.load(value_22.config);
                                                    if var_314 and breathing_connections and breathing_connections.create_noti then
                                                        breathing_connections.create_noti({ title = "Config", desc = "Auto-loaded \"" .. value_22.config .. "\" for " .. value_22.username, show_time = 4 });
                                                    end;
                                                end;
                                                return ;
                                            end;
                                        end;
                                    end;
                                    return ;
                                end);
                            end;
                            element_registry.toggles[var_277] = {
                                get = function()
                                    local tbl_12 = {};
                                    for key_21, value_23 in ipairs(tbl_9) do
                                        tbl_12[value_23.username] = { link = value_23.link, config = value_23.config };
                                    end;
                                    return tbl_12;
                                end,
                                set = function(arg_67)
                                    if type(arg_67) == "table" then
                                        tbl_9 = {};
                                        for key_22, value_24 in pairs(arg_67) do
                                            if type(value_24) == "table" then
                                                table.insert(tbl_9, { username = key_22, link = value_24.link or "", config = value_24.config or "" });
                                            else
                                                table.insert(tbl_9, { username = key_22, link = value_24, config = "" });
                                            end;
                                        end;
                                        func_22();
                                    end;
                                    return ;
                                end
                            };
                            return {
                                add = function(arg_68, arg_69, arg_70)
                                    table.insert(tbl_9, { username = arg_68, link = arg_69, config = arg_70 or "" });
                                    func_22();
                                    return ;
                                end,
                                get = function()
                                    return tbl_9;
                                end,
                                get_account_config = function(arg_71)
                                    for key_23, value_25 in ipairs(tbl_9) do
                                        if value_25.username == arg_71 then
                                            return value_25.config;
                                        end;
                                    end;
                                    return nil;
                                end,
                                load_config_for_current_player = function()
                                    local game_7 = game:GetService("Players").LocalPlayer;
                                    if game_7 then
                                        for key_24, value_26 in ipairs(tbl_9) do
                                            if value_26.username == game_7.Name and value_26.config and value_26.config ~= "" and getgenv().feral_config and getgenv().feral_config.load then
                                                return getgenv().feral_config.load(value_26.config);
                                            end;
                                        end;
                                    end;
                                    return false, "No config found for current player";
                                end
                            };
                        end,
                        create_button = function(arg_72, arg_73)
                            local var_316 = arg_72.title;
                            local var_317 = arg_73 or function()
                                return ;
                            end;
                            local var_318 = Instance.new(frame_class);
                            local var_319 = Instance.new(frame_class);
                            local var_320 = Instance.new("UICorner");
                            local var_321 = Instance.new("TextLabel");
                            local var_322 = Instance.new("TextButton");
                            var_318.Name = var_316 .. "dot";
                            var_318.Parent = var_218;
                            var_318.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_318.BackgroundTransparency = 1;
                            var_318.Position = UDim2.new(0, 0, 0.300000012, 0);
                            var_318.Size = UDim2.new(1, 0, 0, 25);
                            var_319.Name = "ButtonBG";
                            var_319.Parent = var_318;
                            var_319.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_319.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_319.Size = UDim2.new(1, -10, 1, 0);
                            var_319.BackgroundColor3 = getgenv().ui_color["Button Color"];
                            table.insert(theme_listeners["Button Color"], function()
                                var_319.BackgroundColor3 = getgenv().ui_color["Button Color"];
                                return ;
                            end);
                            var_319.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_319.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_320.CornerRadius = UDim.new(0, 4);
                            var_320.Name = "ButtonCorner";
                            var_320.Parent = var_319;
                            var_321.Name = "TextColor";
                            var_321.Parent = var_319;
                            var_321.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_321.BackgroundTransparency = 1;
                            var_321.Position = UDim2.new(0, 10, 0, 0);
                            var_321.Size = UDim2.new(1, -10, 1, 0);
                            var_321.Font = Enum.Font.GothamBlack;
                            var_321.Text = var_316;
                            var_321.TextSize = 14;
                            var_321.TextXAlignment = Enum.TextXAlignment.Left;
                            var_321.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_321.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_322.Name = "Button";
                            var_322.Parent = var_319;
                            var_322.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_322.BackgroundTransparency = 1;
                            var_322.Size = UDim2.new(1, 0, 1, 0);
                            var_322.Font = Enum.Font.SourceSans;
                            var_322.Text = "";
                            var_322.TextColor3 = Color3.fromRGB(0, 0, 0);
                            var_322.TextSize = 14;
                            var_322.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_322.MouseButton1Down:Connect(function()
                                var_317();
                                return ;
                            end);
                            return ;
                        end,
                        create_label = function(arg_74)
                            local str_14 = tostring(arg_74.title);
                            local var_323 = Instance.new(frame_class);
                            local var_324 = Instance.new(frame_class);
                            local var_325 = Instance.new("UICorner");
                            local var_326 = Instance.new("TextLabel");
                            var_323.Name = "LabelFrame";
                            var_323.Parent = var_218;
                            var_323.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_323.BackgroundTransparency = 1;
                            var_323.Position = UDim2.new(0, 0, 0, 0);
                            var_323.Size = UDim2.new(1, 0, 0, 0);
                            var_323.AutomaticSize = Enum.AutomaticSize.Y;
                            var_324.Name = "LabelBG";
                            var_324.Parent = var_323;
                            var_324.AnchorPoint = Vector2.new(0.5, 0);
                            var_324.Position = UDim2.new(0.5, 0, 0, 0);
                            var_324.Size = UDim2.new(1, -10, 0, -10);
                            var_324.BackgroundColor3 = getgenv().ui_color["Label Color"];
                            var_324.AutomaticSize = Enum.AutomaticSize.Y;
                            table.insert(theme_listeners["Label Color"], function()
                                var_324.BackgroundColor3 = getgenv().ui_color["Label Color"];
                                return ;
                            end);
                            var_324.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_324.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_325.CornerRadius = UDim.new(0, 4);
                            var_325.Name = "LabelCorner";
                            var_325.Parent = var_324;
                            var_326.Name = "TextColor";
                            var_326.Parent = var_324;
                            var_326.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_326.BackgroundTransparency = 1;
                            var_326.Position = UDim2.new(0, 10, 0, 3);
                            var_326.Size = UDim2.new(1, -20, 1, 0);
                            var_326.Font = Enum.Font.GothamBlack;
                            var_326.Text = str_14;
                            var_326.TextSize = 14;
                            var_326.TextXAlignment = Enum.TextXAlignment.Left;
                            var_326.AutomaticSize = Enum.AutomaticSize.Y;
                            var_326.TextWrapped = true;
                            var_326.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_326.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            return {
                                set_text = function(arg_75)
                                    var_326.Text = arg_75;
                                    return ;
                                end,
                                set_color = function(arg_76)
                                    var_326.TextColor3 = arg_76;
                                    return ;
                                end
                            };
                        end,
                        create_dropdown = function(arg_77, arg_78)
                            local str_15 = tostring(arg_77.title);
                            local var_327 = arg_77.List or {};
                            local var_328 = arg_77.search or false;
                            local var_329 = arg_77.selected or false;
                            local var_330 = arg_77.default;
                            local var_331 = arg_78 or function()
                                return ;
                            end;
                            local func_23 = function(arg_79)
                                if type(arg_79) ~= "table" then
                                    return false;
                                end;
                                local num_19 = 0;
                                for key_25, value_27 in pairs(arg_79) do
                                    if type(key_25) ~= "number" then
                                        return false;
                                    end;
                                    num_19 = num_19 + 1;
                                end;
                                return num_19 == #arg_79;
                            end;
                            local var_332 = Instance.new(frame_class);
                            local var_333 = Instance.new(frame_class);
                            local var_334 = Instance.new("UICorner");
                            local var_335 = Instance.new(frame_class);
                            local var_336 = Instance.new("UICorner");
                            local var_337 = Instance.new("ImageLabel");
                            local var_338 = Instance.new("TextButton");
                            local var_339 = Instance.new(frame_class);
                            local var_340 = Instance.new("ScrollingFrame");
                            local var_341 = Instance.new(frame_class);
                            local var_342 = Instance.new("UIListLayout");
                            local var_343;
                            if var_328 then
                                var_343 = Instance.new("TextBox");
                                var_338.Visible = false;
                            else
                                var_343 = Instance.new("TextLabel");
                            end;
                            var_332.Name = str_15 .. "DropdownFrame";
                            var_332.Parent = var_218;
                            var_332.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_332.BackgroundTransparency = 1;
                            var_332.Position = UDim2.new(0, 0, 0.473684222, 0);
                            var_332.Size = UDim2.new(1, 0, 0, 25);
                            var_333.Name = "Background1";
                            var_333.Parent = var_332;
                            var_333.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_333.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_333.Size = UDim2.new(1, -10, 1, 0);
                            var_333.ClipsDescendants = true;
                            var_333.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_333.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_333.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_333.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_334.CornerRadius = UDim.new(0, 4);
                            var_334.Name = "Dropdowncorner";
                            var_334.Parent = var_333;
                            var_335.Name = "Background2";
                            var_335.Parent = var_333;
                            var_335.Size = UDim2.new(1, 0, 0, 25);
                            var_335.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_335.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_335.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_335.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_336.CornerRadius = UDim.new(0, 4);
                            var_336.Parent = var_335;
                            var_343.Name = "TextColorPlaceholder";
                            var_343.Parent = var_335;
                            var_343.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_343.BackgroundTransparency = 1;
                            var_343.Position = UDim2.new(0, 10, 0, 0);
                            var_343.Size = UDim2.new(1, -40, 1, 0);
                            var_343.Font = Enum.Font.GothamBlack;
                            var_343.Text = "";
                            var_343.TextSize = 14;
                            var_343.TextXAlignment = Enum.TextXAlignment.Left;
                            var_343.ClipsDescendants = true;
                            var_343.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_343.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            if var_328 then
                                var_343.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                                table.insert(theme_listeners["Placeholder Text Color"], function()
                                    var_343.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                                    return ;
                                end);
                            end;
                            var_337.Name = "ImgDrop";
                            var_337.Parent = var_335;
                            var_337.AnchorPoint = Vector2.new(1, 0.5);
                            var_337.BackgroundTransparency = 1;
                            var_337.Position = UDim2.new(1, -6, 0.5, 0);
                            var_337.Size = UDim2.new(0, 15, 0, 15);
                            var_337.Image = "rbxassetid://6954383209";
                            var_337.ImageColor3 = getgenv().ui_color["Dropdown Icon Color"];
                            table.insert(theme_listeners["Dropdown Icon Color"], function()
                                var_337.ImageColor3 = getgenv().ui_color["Dropdown Icon Color"];
                                return ;
                            end);
                            var_338.Name = "DropdownButton";
                            var_338.Parent = var_335;
                            var_338.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_338.BackgroundTransparency = 1;
                            var_338.Size = UDim2.new(1, 0, 1, 0);
                            var_338.Font = Enum.Font.GothamBold;
                            var_338.Text = "";
                            var_338.TextColor3 = Color3.fromRGB(230, 230, 230);
                            var_338.TextSize = 14;
                            var_339.Name = "Dropdownlisttt";
                            var_339.Parent = var_333;
                            var_339.BackgroundTransparency = 1;
                            var_339.BorderSizePixel = 0;
                            var_339.Position = UDim2.new(0, 0, 0, 25);
                            var_339.Size = UDim2.new(1, 0, 0, 25);
                            var_340.Name = "DropdownScroll";
                            var_340.Parent = var_339;
                            var_340.Active = true;
                            var_340.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_340.BackgroundTransparency = 1;
                            var_340.BorderSizePixel = 0;
                            var_340.Size = UDim2.new(1, 0, 1, 0);
                            var_340.ScrollBarThickness = 5;
                            var_341.Name = "ScrollContainer";
                            var_341.Parent = var_340;
                            var_341.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_341.BackgroundTransparency = 1;
                            var_341.Position = UDim2.new(0, 5, 0, 5);
                            var_341.Size = UDim2.new(1, -15, 1, -5);
                            var_342.Name = "ScrollContainerList";
                            var_342.Parent = var_341;
                            var_342.SortOrder = Enum.SortOrder.LayoutOrder;
                            var_342:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
                                var_340.CanvasSize = UDim2.new(0, 0, 0, 10 + var_342.AbsoluteContentSize.Y + 5);
                                return ;
                            end);
                            local flag_22 = false;
                            local tbl_13 = {};
                            local tbl_14 = {};
                            local var_344 = var_327;
                            local flag_23 = nil;
                            if var_329 then
                                if func_23(var_344) then
                                    local tbl_15 = {};
                                    for key_26, value_28 in ipairs(var_344) do
                                        tbl_15[value_28] = false;
                                    end;
                                    var_344 = tbl_15;
                                else
                                    for key_27, value_29 in pairs(var_344) do
                                        var_344[key_27] = not not value_29;
                                    end;
                                end;
                            end;
                            local func_24;
                            func_24 = function()
                                for key_28, value_30 in ipairs(var_341:GetChildren()) do
                                    if value_30:IsA(frame_class) then
                                        value_30:Destroy();
                                    end;
                                end;
                                tbl_14 = {};
                                if not var_329 then
                                    for key_29, value_31 in ipairs(var_344) do
                                        local str_16 = tostring(value_31);
                                        local var_345 = str_16:lower();
                                        table.insert(tbl_14, var_345);
                                        local var_346 = Instance.new(frame_class);
                                        local var_347 = Instance.new("UICorner");
                                        local var_348 = Instance.new(frame_class);
                                        local var_349 = Instance.new(frame_class);
                                        local var_350 = Instance.new("UICorner");
                                        local var_351 = Instance.new(frame_class);
                                        local var_352 = Instance.new("TextButton");
                                        var_346.Name = var_345;
                                        var_346.Parent = var_341;
                                        var_346.BackgroundTransparency = 1;
                                        var_346.Size = UDim2.new(1, 0, 0, 25);
                                        var_347.CornerRadius = UDim.new(0, 4);
                                        var_347.Parent = var_346;
                                        var_348.Name = "Line";
                                        var_348.Parent = var_346;
                                        var_348.AnchorPoint = Vector2.new(0, 0.5);
                                        var_348.BackgroundTransparency = 1;
                                        var_348.Position = UDim2.new(0, 0, 0.5, 0);
                                        var_348.Size = UDim2.new(0, 14, 1, 0);
                                        var_349.Name = "InLine";
                                        var_349.Parent = var_348;
                                        var_349.AnchorPoint = Vector2.new(0.5, 0.5);
                                        var_349.BorderSizePixel = 0;
                                        var_349.Position = UDim2.new(0.5, 0, 0.5, 0);
                                        var_349.Size = UDim2.new(1, -10, 1, -10);
                                        var_349.BackgroundTransparency = flag_23 == str_16 and 0 or 1;
                                        var_349.BackgroundColor3 = getgenv().ui_color["Dropdown Selected Color"];
                                        table.insert(theme_listeners["Dropdown Selected Color"], function()
                                            var_349.BackgroundColor3 = getgenv().ui_color["Dropdown Selected Color"];
                                            return ;
                                        end);
                                        var_350.CornerRadius = UDim.new(0, 4);
                                        var_350.Parent = var_349;
                                        var_351.Name = "Dropvalcontainer";
                                        var_351.Parent = var_346;
                                        var_351.BackgroundTransparency = 1;
                                        var_351.Position = UDim2.new(0, 15, 0, 0);
                                        var_351.Size = UDim2.new(1, -15, 1, 0);
                                        var_352.Name = "TextColor";
                                        var_352.Parent = var_351;
                                        var_352.BackgroundTransparency = 1;
                                        var_352.Size = UDim2.new(1, 0, 1, 0);
                                        var_352.Font = Enum.Font.GothamBold;
                                        var_352.Text = str_16;
                                        var_352.TextSize = 14;
                                        var_352.TextXAlignment = Enum.TextXAlignment.Left;
                                        var_352.TextColor3 = getgenv().ui_color[text_color_key];
                                        table.insert(theme_listeners[text_color_key], function()
                                            var_352.TextColor3 = getgenv().ui_color[text_color_key];
                                            return ;
                                        end);
                                        var_352.MouseButton1Click:Connect(function()
                                            ui_library.button_effect();
                                            flag_23 = str_16;
                                            if var_328 then
                                                var_343.PlaceholderText = str_15 .. ": " .. str_16;
                                            else
                                                var_343.Text = str_15 .. ": " .. str_16;
                                            end;
                                            func_24();
                                            pcall(var_331, str_16);
                                            return ;
                                        end);
                                    end;
                                else
                                    for key_30, value_32 in pairs(var_344) do
                                        local str_17 = tostring(key_30);
                                        local var_353 = str_17:lower();
                                        table.insert(tbl_14, var_353);
                                        local var_354 = Instance.new(frame_class);
                                        local var_355 = Instance.new("UICorner");
                                        local var_356 = Instance.new(frame_class);
                                        local var_357 = Instance.new(frame_class);
                                        local var_358 = Instance.new("UICorner");
                                        local var_359 = Instance.new(frame_class);
                                        local var_360 = Instance.new("TextButton");
                                        var_354.Name = var_353;
                                        var_354.Parent = var_341;
                                        var_354.BackgroundTransparency = 1;
                                        var_354.Size = UDim2.new(1, 0, 0, 25);
                                        var_355.CornerRadius = UDim.new(0, 4);
                                        var_355.Parent = var_354;
                                        var_356.Name = "Line";
                                        var_356.Parent = var_354;
                                        var_356.AnchorPoint = Vector2.new(0, 0.5);
                                        var_356.BackgroundTransparency = 1;
                                        var_356.Position = UDim2.new(0, 0, 0.5, 0);
                                        var_356.Size = UDim2.new(0, 14, 1, 0);
                                        var_357.Name = "InLine";
                                        var_357.Parent = var_356;
                                        var_357.AnchorPoint = Vector2.new(0.5, 0.5);
                                        var_357.BorderSizePixel = 0;
                                        var_357.Position = UDim2.new(0.5, 0, 0.5, 0);
                                        var_357.Size = UDim2.new(1, -10, 1, -10);
                                        var_357.BackgroundTransparency = value_32 and 0 or 1;
                                        var_357.BackgroundColor3 = getgenv().ui_color["Dropdown Selected Color"];
                                        table.insert(theme_listeners["Dropdown Selected Color"], function()
                                            var_357.BackgroundColor3 = getgenv().ui_color["Dropdown Selected Color"];
                                            return ;
                                        end);
                                        var_358.CornerRadius = UDim.new(0, 4);
                                        var_358.Parent = var_357;
                                        var_359.Name = "Dropvalcontainer";
                                        var_359.Parent = var_354;
                                        var_359.BackgroundTransparency = 1;
                                        var_359.Position = UDim2.new(0, 15, 0, 0);
                                        var_359.Size = UDim2.new(1, -15, 1, 0);
                                        var_360.Name = "TextColor";
                                        var_360.Parent = var_359;
                                        var_360.BackgroundTransparency = 1;
                                        var_360.Size = UDim2.new(1, 0, 1, 0);
                                        var_360.Font = Enum.Font.GothamBold;
                                        var_360.Text = str_17;
                                        var_360.TextSize = 14;
                                        var_360.TextXAlignment = Enum.TextXAlignment.Left;
                                        var_360.TextColor3 = getgenv().ui_color[text_color_key];
                                        table.insert(theme_listeners[text_color_key], function()
                                            var_360.TextColor3 = getgenv().ui_color[text_color_key];
                                            return ;
                                        end);
                                        var_360.MouseButton1Click:Connect(function()
                                            ui_library.button_effect();
                                            var_344[key_30] = not var_344[key_30];
                                            var_357.BackgroundTransparency = var_344[key_30] and 0 or 1;
                                            pcall(var_331, key_30, var_344[key_30]);
                                            return ;
                                        end);
                                    end;
                                end;
                                return ;
                            end;
                            if not var_329 then
                                if var_330 ~= nil then
                                    local str_18 = tostring(var_330);
                                    flag_23 = str_18;
                                    if var_328 then
                                        var_343.PlaceholderText = str_15 .. ": " .. str_18;
                                    else
                                        var_343.Text = str_15 .. ": " .. str_18;
                                    end;
                                    pcall(var_331, str_18);
                                elseif var_328 then
                                    var_343.PlaceholderText = str_15 .. ": ";
                                else
                                    var_343.Text = str_15 .. ": ";
                                end;
                            else
                                if type(var_330) == "table" then
                                    if func_23(var_330) then
                                        for key_31, value_33 in ipairs(var_330) do
                                            if var_344[value_33] ~= nil then
                                                var_344[value_33] = true;
                                                pcall(var_331, value_33, true);
                                            end;
                                        end;
                                    else
                                        for key_32, value_34 in pairs(var_330) do
                                            if var_344[key_32] ~= nil then
                                                var_344[key_32] = not not value_34;
                                                pcall(var_331, key_32, var_344[key_32]);
                                            end;
                                        end;
                                    end;
                                end;
                                var_343.Text = str_15 .. ": ";
                            end;
                            func_24();
                            if var_328 then
                                var_343.Changed:Connect(function()
                                    local var_361 = var_343.Text:lower();
                                    for key_33, value_35 in ipairs(var_341:GetChildren()) do
                                        if value_35:IsA(frame_class) then
                                            value_35.Visible = var_361 == "" or value_35.Name:find(var_361, 1, true);
                                        end;
                                    end;
                                    return ;
                                end);
                            end;
                            local func_25 = function()
                                flag_22 = not flag_22;
                                local var_362 = flag_22 and UDim2.new(1, 0, 0, 170) or UDim2.new(1, 0, 0, 0);
                                local var_363 = flag_22 and UDim2.new(1, 0, 0, 200) or UDim2.new(1, 0, 0, 25);
                                local var_364 = flag_22 and 90 or 0;
                                tween_service_2:Create(var_339, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Size = var_362 }):Play();
                                tween_service_2:Create(var_332, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Size = var_363 }):Play();
                                tween_service_2:Create(var_337, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Rotation = var_364 }):Play();
                                return ;
                            end;
                            var_338.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                func_25();
                                return ;
                            end);
                            if var_328 then
                                var_343.Focused:Connect(function()
                                    ui_library.button_effect();
                                    func_25();
                                    return ;
                                end);
                            end;
                            local tbl_17 = {
                                rf = func_24,
                                clear_text = function(arg_80)
                                    if not var_329 then
                                        if var_328 then
                                            var_343.PlaceholderText = str_15 .. ": ";
                                        else
                                            var_343.Text = str_15 .. ": ";
                                        end;
                                        flag_23 = nil;
                                    else
                                        var_343.Text = str_15 .. ": ";
                                        for key_34, value_36 in pairs(var_344) do
                                            var_344[key_34] = false;
                                        end;
                                        func_24();
                                    end;
                                    return ;
                                end,
                                get_new_list = function(arg_81, arg_82)
                                    flag_22 = false;
                                    tween_service_2:Create(var_339, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Size = UDim2.new(1, 0, 0, 0) }):Play();
                                    tween_service_2:Create(var_332, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Size = UDim2.new(1, 0, 0, 25) }):Play();
                                    tween_service_2:Create(var_337, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { Rotation = 0 }):Play();
                                    if var_329 then
                                        if func_23(arg_82) then
                                            local tbl_16 = {};
                                            for key_35, value_37 in ipairs(arg_82) do
                                                tbl_16[value_37] = false;
                                            end;
                                            var_344 = tbl_16;
                                        else
                                            for key_36, value_38 in pairs(arg_82) do
                                                arg_82[key_36] = false;
                                            end;
                                            var_344 = arg_82;
                                        end;
                                    else
                                        var_344 = arg_82 or {};
                                    end;
                                    flag_23 = nil;
                                    func_24();
                                    return ;
                                end
                            };
                            local var_365 = make_element_id(var_191, str_11, str_15);
                            if not var_329 then
                                element_registry.dropdowns[var_365] = {
                                    get = function()
                                        return flag_23;
                                    end,
                                    set = function(arg_83)
                                        if not arg_83 then
                                            return ;
                                        end;
                                        local str_19 = tostring(arg_83);
                                        local flag_24 = false;
                                        for key_37, value_39 in ipairs(var_344) do
                                            if tostring(value_39) == str_19 then
                                                flag_24 = true;
                                                break;
                                            end;
                                        end;
                                        if not flag_24 then
                                            return ;
                                        end;
                                        flag_23 = str_19;
                                        if var_328 then
                                            var_343.PlaceholderText = str_15 .. ": " .. str_19;
                                        else
                                            var_343.Text = str_15 .. ": " .. str_19;
                                        end;
                                        func_24();
                                        pcall(var_331, str_19);
                                        return ;
                                    end
                                };
                            else
                                element_registry.dropdowns[var_365] = {
                                    get = function()
                                        local tbl_18 = {};
                                        for key_38, value_40 in pairs(var_344) do
                                            tbl_18[key_38] = not not value_40;
                                        end;
                                        return tbl_18;
                                    end,
                                    set = function(arg_84)
                                        if type(arg_84) ~= "table" then
                                            return ;
                                        end;
                                        for key_39, value_41 in pairs(arg_84) do
                                            if var_344[key_39] ~= nil then
                                                var_344[key_39] = not not value_41;
                                            end;
                                        end;
                                        func_24();
                                        for key_40, value_42 in pairs(var_344) do
                                            pcall(var_331, key_40, value_42);
                                        end;
                                        return ;
                                    end
                                };
                            end;
                            return tbl_17;
                        end,
                        create_bind = function(arg_85, arg_86)
                            local str_20 = tostring(arg_85.title) or "";
                            local var_366 = arg_85.Key;
                            local var_367 = arg_85.default or arg_85.Key;
                            local str_21 = tostring(var_367):match("UserInputType") and "UserInputType" or "KeyCode";
                            local var_368 = arg_86 or function()
                                return ;
                            end;
                            local str_22 = tostring(var_366):gsub("Enum.UserInputType.", "");
                            local str_23 = tostring(str_22):gsub("Enum.KeyCode.", "");
                            local var_369 = Instance.new(frame_class);
                            local var_370 = Instance.new("UICorner");
                            local var_371 = Instance.new(frame_class);
                            local var_372 = Instance.new("UICorner");
                            local var_373 = Instance.new("TextLabel");
                            local var_374 = Instance.new("TextButton");
                            local var_375 = Instance.new(frame_class);
                            local var_376 = Instance.new("UICorner");
                            local var_377 = Instance.new("TextButton");
                            var_369.Name = str_20 .. "bguvl";
                            var_369.Parent = var_218;
                            var_369.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_369.BackgroundTransparency = 1;
                            var_369.Position = UDim2.new(0, 0, 0.208333328, 0);
                            var_369.Size = UDim2.new(1, 0, 0, 35);
                            var_370.CornerRadius = UDim.new(0, 4);
                            var_370.Name = "BindCorner";
                            var_370.Parent = var_369;
                            var_371.Name = "Background1";
                            var_371.Parent = var_369;
                            var_371.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_371.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_371.Size = UDim2.new(1, -10, 1, 0);
                            var_371.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_371.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_371.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_371.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_372.CornerRadius = UDim.new(0, 4);
                            var_372.Name = "ButtonCorner";
                            var_372.Parent = var_371;
                            var_373.Name = "TextColor";
                            var_373.Parent = var_371;
                            var_373.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_373.BackgroundTransparency = 1;
                            var_373.Position = UDim2.new(0, 10, 0, 0);
                            var_373.Size = UDim2.new(1, -10, 1, 0);
                            var_373.Font = Enum.Font.GothamBlack;
                            var_373.Text = str_20;
                            var_373.TextSize = 14;
                            var_373.TextXAlignment = Enum.TextXAlignment.Left;
                            var_373.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_373.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_375.Name = "Background2";
                            var_375.Parent = var_371;
                            var_375.AnchorPoint = Vector2.new(1, 0.5);
                            var_375.Position = UDim2.new(1, -5, 0.5, 0);
                            var_375.Size = UDim2.new(0, 150, 0, 25);
                            var_375.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_375.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_376.CornerRadius = UDim.new(0, 4);
                            var_376.Name = "ButtonCorner";
                            var_376.Parent = var_375;
                            var_377.Name = "Bindkey";
                            var_377.Parent = var_375;
                            var_377.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_377.BackgroundTransparency = 1;
                            var_377.Size = UDim2.new(1, 0, 1, 0);
                            var_377.Font = Enum.Font.GothamBold;
                            var_377.Text = tostring(var_367):gsub("Enum.KeyCode.", "");
                            var_377.TextSize = 14;
                            var_377.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_377.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            local tbl_19 = { [Enum.UserInputType.MouseButton1] = "Mouse1", [Enum.UserInputType.MouseButton2] = "Mouse2", [Enum.UserInputType.MouseButton3] = "Mouse3" };
                            var_377.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_377.MouseButton1Click:Connect(function()
                                local flag_25 = nil;
                                var_377.Text = "...";
                                flag_25 = game:GetService("UserInputService").InputBegan:Connect(function(arg_87)
                                    if tbl_19[arg_87.UserInputType] then
                                        var_377.Text = tbl_19[arg_87.UserInputType];
                                        spawn(function()
                                            wait(0.1);
                                            var_367 = arg_87.UserInputType;
                                            str_21 = "UserInputType";
                                            return ;
                                        end);
                                    elseif arg_87.KeyCode ~= Enum.KeyCode.Unknown then
                                        var_377.Text = tostring(arg_87.KeyCode):gsub("Enum.KeyCode.", "");
                                        spawn(function()
                                            wait(0.1);
                                            var_367 = arg_87.KeyCode;
                                            str_21 = "KeyCode";
                                            return ;
                                        end);
                                    end;
                                    flag_25:Disconnect();
                                    return ;
                                end);
                                return ;
                            end);
                            game:GetService("UserInputService").InputBegan:Connect(function(arg_88)
                                if var_367 == arg_88.UserInputType or var_367 == arg_88.KeyCode then
                                    var_368(var_367);
                                end;
                                return ;
                            end);
                            return ;
                        end,
                        create_box = function(arg_89, arg_90)
                            local str_24 = tostring(arg_89.title) or "";
                            local str_25 = tostring(arg_89.placeholder) or "";
                            local var_378 = arg_89.default or false;
                            local var_379 = arg_89.Number or false;
                            local var_380 = arg_90 or function()
                                return ;
                            end;
                            local var_381 = Instance.new(frame_class);
                            local var_382 = Instance.new("UICorner");
                            local var_383 = Instance.new(frame_class);
                            local var_384 = Instance.new("UICorner");
                            local var_385 = Instance.new("TextLabel");
                            local var_386 = Instance.new(frame_class);
                            local var_387 = Instance.new("UICorner");
                            local var_388 = Instance.new("TextBox");
                            local var_389 = Instance.new(frame_class);
                            local var_390 = Instance.new("UICorner");
                            var_381.Name = "BoxFrame";
                            var_381.Parent = var_218;
                            var_381.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_381.BackgroundTransparency = 1;
                            var_381.Position = UDim2.new(0, 0, 0.208333328, 0);
                            var_381.Size = UDim2.new(1, 0, 0, 60);
                            var_382.CornerRadius = UDim.new(0, 4);
                            var_382.Name = "BoxCorner";
                            var_382.Parent = var_381;
                            var_383.Name = "Background1";
                            var_383.Parent = var_381;
                            var_383.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_383.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_383.Size = UDim2.new(1, -10, 1, 0);
                            var_383.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_383.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_383.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_383.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_384.CornerRadius = UDim.new(0, 4);
                            var_384.Name = "ButtonCorner";
                            var_384.Parent = var_383;
                            var_385.Name = "TextColor";
                            var_385.Parent = var_383;
                            var_385.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_385.BackgroundTransparency = 1;
                            var_385.Position = UDim2.new(0, 10, 0, 0);
                            var_385.Size = UDim2.new(1, -10, 0.5, 0);
                            var_385.Font = Enum.Font.GothamBlack;
                            var_385.Text = str_24;
                            var_385.TextSize = 14;
                            var_385.TextXAlignment = Enum.TextXAlignment.Left;
                            var_385.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_385.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_386.Name = "Background2";
                            var_386.Parent = var_383;
                            var_386.AnchorPoint = Vector2.new(1, 0.5);
                            var_386.ClipsDescendants = true;
                            var_386.Position = UDim2.new(1, -5, 0, 40);
                            var_386.Size = UDim2.new(1, -10, 0, 25);
                            var_386.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_386.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_387.CornerRadius = UDim.new(0, 4);
                            var_387.Name = "ButtonCorner";
                            var_387.Parent = var_386;
                            var_388.Name = "TextColorPlaceholder";
                            var_388.Parent = var_386;
                            var_388.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_388.BackgroundTransparency = 1;
                            var_388.Position = UDim2.new(0, 5, 0, 0);
                            var_388.Size = UDim2.new(1, -5, 1, 0);
                            var_388.Font = Enum.Font.GothamBold;
                            var_388.PlaceholderText = str_25;
                            var_388.Text = "";
                            var_388.TextSize = 14;
                            var_388.TextXAlignment = Enum.TextXAlignment.Left;
                            var_388.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                            var_388.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners["Placeholder Text Color"], function()
                                var_388.PlaceholderColor3 = getgenv().ui_color["Placeholder Text Color"];
                                return ;
                            end);
                            table.insert(theme_listeners[text_color_key], function()
                                var_388.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_389.Name = "TextNSBoxLineeeee";
                            var_389.Parent = var_386;
                            var_389.BackgroundTransparency = 1;
                            var_389.Position = UDim2.new(0, 0, 1, -2);
                            var_389.Size = UDim2.new(1, 0, 0, 6);
                            var_389.BackgroundColor3 = getgenv().ui_color["Box Highlight Color"];
                            table.insert(theme_listeners["Box Highlight Color"], function()
                                var_389.BackgroundColor3 = getgenv().ui_color["Box Highlight Color"];
                                return ;
                            end);
                            var_390.CornerRadius = UDim.new(1, 0);
                            var_390.Parent = var_389;
                            var_388.Focused:Connect(function()
                                tween_service_2:Create(var_389, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundTransparency = 0 }):Play();
                                return ;
                            end);
                            var_388.Focused:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            if var_379 then
                                var_388:GetPropertyChangedSignal("Text"):Connect(function()
                                    if not tonumber(var_388.Text) then
                                        var_388.PlaceholderText = str_25;
                                        var_388.Text = "";
                                    end;
                                    return ;
                                end);
                            end;
                            var_388.FocusLost:Connect(function()
                                tween_service_2:Create(var_389, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundTransparency = 1 }):Play();
                                if var_388.Text ~= "" then
                                    var_380(var_388.Text);
                                end;
                                return ;
                            end);
                            local tbl_20 = {};
                            if var_378 then
                                var_388.Text = var_378;
                                var_380(var_378);
                            end;
                            tbl_20.set_value = function(arg_91)
                                var_388.Text = arg_91;
                                var_380(arg_91);
                                return ;
                            end;
                            local var_391 = make_element_id(var_191, str_11, str_24);
                            element_registry.Boxes[var_391] = {
                                get = function()
                                    return var_388.Text;
                                end,
                                set = function(arg_92)
                                    var_388.Text = tostring(arg_92 or "");
                                    if var_388.Text ~= "" then
                                        var_380(var_388.Text);
                                    end;
                                    return ;
                                end
                            };
                            return tbl_20;
                        end,
                        create_slider = function(arg_93, arg_94)
                            local str_26 = tostring(arg_93.title) or "";
                            local num_20 = tonumber(arg_93.min) or 0;
                            local num_21 = tonumber(arg_93.max) or 100;
                            local var_392 = arg_93.precise or false;
                            local num_22 = tonumber(arg_93.default) or 0;
                            local num_23 = 400;
                            local game_8 = game:GetService("UserInputService");
                            local var_393 = tween_service_2 or game:GetService("TweenService");
                            local local_player_3 = game.Players.LocalPlayer:GetMouse();
                            local var_394 = typeof(arg_94) == "function" and arg_94 or function()
                                return ;
                            end;
                            local func_26 = function(arg_95)
                                task.spawn(var_394, arg_95);
                                return ;
                            end;
                            local var_395 = Instance.new(frame_class);
                            local var_396 = Instance.new("UICorner");
                            local var_397 = Instance.new(frame_class);
                            local var_398 = Instance.new("UICorner");
                            local var_399 = Instance.new("TextLabel");
                            local var_400 = Instance.new(frame_class);
                            local var_401 = Instance.new("TextButton");
                            local var_402 = Instance.new("UICorner");
                            local var_403 = Instance.new(frame_class);
                            local var_404 = Instance.new("UICorner");
                            local var_405 = Instance.new(frame_class);
                            local var_406 = Instance.new("UICorner");
                            local var_407 = Instance.new("TextBox");
                            var_395.Name = str_26 .. "buda";
                            var_395.Parent = var_218;
                            var_395.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_395.BackgroundTransparency = 1;
                            var_395.Position = UDim2.new(0, 0, 0.208333328, 0);
                            var_395.Size = UDim2.new(1, 0, 0, 50);
                            var_396.CornerRadius = UDim.new(0, 4);
                            var_396.Name = "SliderCorner";
                            var_396.Parent = var_395;
                            var_397.Name = "Background1";
                            var_397.Parent = var_395;
                            var_397.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_397.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_397.Size = UDim2.new(1, -10, 1, 0);
                            var_397.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_397.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_397.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_397.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_398.CornerRadius = UDim.new(0, 4);
                            var_398.Name = "SliderBGCorner";
                            var_398.Parent = var_397;
                            var_399.Name = "TextColor";
                            var_399.Parent = var_397;
                            var_399.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_399.BackgroundTransparency = 1;
                            var_399.Position = UDim2.new(0, 10, 0, 0);
                            var_399.Size = UDim2.new(1, -10, 0, 25);
                            var_399.Font = Enum.Font.GothamBlack;
                            var_399.Text = str_26;
                            var_399.TextSize = 14;
                            var_399.TextXAlignment = Enum.TextXAlignment.Left;
                            var_399.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_399.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_400.Name = "SliderBar";
                            var_400.Parent = var_395;
                            var_400.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_400.Position = UDim2.new(0.5, 0, 0.5, 14);
                            var_400.Size = UDim2.new(0, 400, 0, 6);
                            var_400.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_400.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_401.Name = "SliderButton";
                            var_401.Parent = var_400;
                            var_401.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_401.BackgroundTransparency = 1;
                            var_401.Size = UDim2.new(1, 0, 1, 0);
                            var_401.Font = Enum.Font.GothamBold;
                            var_401.Text = "";
                            var_401.TextColor3 = Color3.fromRGB(230, 230, 230);
                            var_401.TextSize = 14;
                            var_402.CornerRadius = UDim.new(1, 0);
                            var_402.Name = "SliderBarCorner";
                            var_402.Parent = var_400;
                            var_403.Name = "Bar";
                            var_403.BorderSizePixel = 0;
                            var_403.Parent = var_400;
                            var_403.Size = UDim2.new(0, 0, 1, 0);
                            var_403.BackgroundColor3 = getgenv().ui_color["Slider Line Color"];
                            table.insert(theme_listeners["Slider Line Color"], function()
                                var_403.BackgroundColor3 = getgenv().ui_color["Slider Line Color"];
                                return ;
                            end);
                            var_404.CornerRadius = UDim.new(1, 0);
                            var_404.Name = "BarCorner";
                            var_404.Parent = var_403;
                            var_405.Name = "Background2";
                            var_405.Parent = var_395;
                            var_405.AnchorPoint = Vector2.new(1, 0);
                            var_405.Position = UDim2.new(1, -10, 0, 5);
                            var_405.Size = UDim2.new(0, 150, 0, 25);
                            var_405.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_405.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_406.CornerRadius = UDim.new(0, 4);
                            var_406.Name = "Sliderbox";
                            var_406.Parent = var_405;
                            var_407.Name = "TextColor";
                            var_407.Parent = var_405;
                            var_407.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_407.BackgroundTransparency = 1;
                            var_407.Size = UDim2.new(1, 0, 1, 0);
                            var_407.Font = Enum.Font.GothamBold;
                            var_407.Text = "";
                            var_407.TextSize = 14;
                            var_407.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_407.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_401.MouseEnter:Connect(function()
                                var_393:Create(var_403, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundColor3 = getgenv().ui_color["Slider Highlight Color"] }):Play();
                                return ;
                            end);
                            var_401.MouseLeave:Connect(function()
                                var_393:Create(var_403, TweenInfo.new(getgenv().ui_color["Tween Animation 2 Speed"]), { BackgroundColor3 = getgenv().ui_color["Slider Line Color"] }):Play();
                                return ;
                            end);
                            if num_22 then
                                if num_22 <= num_20 then
                                    num_22 = num_20;
                                elseif num_21 <= num_22 then
                                    num_22 = num_21;
                                end;
                                var_403.Size = UDim2.new(1 - (num_21 - num_22) / (num_21 - num_20), 0, 0, 6);
                                var_407.Text = num_22;
                                func_26(num_22);
                            end;
                            local flag_26 = nil;
                            local flag_27 = nil;
                            var_401.MouseButton1Down:Connect(function()
                                local var_408 = var_392 and tonumber(string.format("%.1f", (num_21 - num_20) / num_23 * var_403.AbsoluteSize.X + num_20)) or math.floor((num_21 - num_20) / num_23 * var_403.AbsoluteSize.X + num_20);
                                var_407.Text = var_408;
                                func_26(var_408);
                                var_403.Size = UDim2.new(0, math.clamp(local_player_3.X - var_403.AbsolutePosition.X, 0, num_23), 0, 6);
                                flag_26 = local_player_3.Move:Connect(function()
                                    local var_409 = var_392 and tonumber(string.format("%.1f", (num_21 - num_20) / num_23 * var_403.AbsoluteSize.X + num_20)) or math.floor((num_21 - num_20) / num_23 * var_403.AbsoluteSize.X + num_20);
                                    var_407.Text = var_409;
                                    func_26(var_409);
                                    var_403.Size = UDim2.new(0, math.clamp(local_player_3.X - var_403.AbsolutePosition.X, 0, num_23), 0, 6);
                                    return ;
                                end);
                                flag_27 = game_8.InputEnded:Connect(function(arg_96)
                                    if arg_96.UserInputType == Enum.UserInputType.MouseButton1 then
                                        local var_410 = var_392 and tonumber(string.format("%.1f", (num_21 - num_20) / num_23 * var_403.AbsoluteSize.X + num_20)) or math.floor((num_21 - num_20) / num_23 * var_403.AbsoluteSize.X + num_20);
                                        var_407.Text = var_410;
                                        func_26(var_410);
                                        var_403.Size = UDim2.new(0, math.clamp(local_player_3.X - var_403.AbsolutePosition.X, 0, num_23), 0, 6);
                                        if flag_26 then
                                            flag_26:Disconnect();
                                        end;
                                        if flag_27 then
                                            flag_27:Disconnect();
                                        end;
                                    end;
                                    return ;
                                end);
                                return ;
                            end);
                            local func_27 = function(arg_97)
                                local num_24 = tonumber(arg_97);
                                if not num_24 then
                                    return ;
                                end;
                                if num_24 <= num_20 then
                                    var_403.Size = UDim2.new(0, 0 * num_23, 0, 6);
                                    var_407.Text = num_20;
                                    func_26(num_20);
                                elseif num_21 <= num_24 then
                                    var_403.Size = UDim2.new(0, num_21 / num_21 * num_23, 0, 6);
                                    var_407.Text = num_21;
                                    func_26(num_21);
                                else
                                    var_403.Size = UDim2.new(1 - (num_21 - num_24) / (num_21 - num_20), 0, 0, 6);
                                    var_407.Text = num_24;
                                    func_26(num_24);
                                end;
                                return ;
                            end;
                            var_407.FocusLost:Connect(function()
                                func_27(var_407.Text);
                                return ;
                            end);
                            local tbl_21 = {
                                set_value = function(arg_98)
                                    func_27(arg_98);
                                    return ;
                                end
                            };
                            local var_411 = make_element_id(var_191, str_11, str_26);
                            element_registry.sliders[var_411] = {
                                get = function()
                                    return tonumber(var_407.Text) or num_20;
                                end,
                                set = function(arg_99)
                                    func_27(tonumber(arg_99) or num_20);
                                    return ;
                                end
                            };
                            return tbl_21;
                        end,
                        create_keybind = function(arg_100, arg_101)
                            local str_27 = tostring(arg_100.title) or "Keybind";
                            local var_412 = arg_100.default or Enum.KeyCode.E;
                            local str_28 = tostring(var_412):match("UserInputType") and "UserInputType" or "KeyCode";
                            local var_413 = arg_101 or function()
                                return ;
                            end;
                            local var_414 = Instance.new(frame_class);
                            local var_415 = Instance.new("UICorner");
                            local var_416 = Instance.new(frame_class);
                            local var_417 = Instance.new("UICorner");
                            local var_418 = Instance.new("TextLabel");
                            local var_419 = Instance.new(frame_class);
                            local var_420 = Instance.new("UICorner");
                            local var_421 = Instance.new("TextButton");
                            var_414.Name = str_27 .. "KeybindFrame";
                            var_414.Parent = var_218;
                            var_414.BackgroundColor3 = Color3.fromRGB(60, 60, 60);
                            var_414.BackgroundTransparency = 1;
                            var_414.Position = UDim2.new(0, 0, 0.208333328, 0);
                            var_414.Size = UDim2.new(1, 0, 0, 35);
                            var_415.CornerRadius = UDim.new(0, 4);
                            var_415.Name = "KeybindCorner";
                            var_415.Parent = var_414;
                            var_416.Name = "Background1";
                            var_416.Parent = var_414;
                            var_416.AnchorPoint = Vector2.new(0.5, 0.5);
                            var_416.Position = UDim2.new(0.5, 0, 0.5, 0);
                            var_416.Size = UDim2.new(1, -10, 1, 0);
                            var_416.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                            table.insert(theme_listeners["Background 1 Color"], function()
                                var_416.BackgroundColor3 = getgenv().ui_color["Background 1 Color"];
                                return ;
                            end);
                            var_416.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                            table.insert(theme_listeners["Background 1 Transparency"], function()
                                var_416.BackgroundTransparency = getgenv().ui_color["Background 1 Transparency"];
                                return ;
                            end);
                            var_417.CornerRadius = UDim.new(0, 4);
                            var_417.Name = "KeybindBGCorner";
                            var_417.Parent = var_416;
                            var_418.Name = "TextColor";
                            var_418.Parent = var_416;
                            var_418.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_418.BackgroundTransparency = 1;
                            var_418.Position = UDim2.new(0, 10, 0, 0);
                            var_418.Size = UDim2.new(1, -10, 1, 0);
                            var_418.Font = Enum.Font.GothamBlack;
                            var_418.Text = str_27;
                            var_418.TextSize = 14;
                            var_418.TextXAlignment = Enum.TextXAlignment.Left;
                            var_418.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_418.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            var_419.Name = "Background2";
                            var_419.Parent = var_416;
                            var_419.AnchorPoint = Vector2.new(1, 0.5);
                            var_419.Position = UDim2.new(1, -5, 0.5, 0);
                            var_419.Size = UDim2.new(0, 150, 0, 25);
                            var_419.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                            table.insert(theme_listeners["Background 2 Color"], function()
                                var_419.BackgroundColor3 = getgenv().ui_color["Background 2 Color"];
                                return ;
                            end);
                            var_420.CornerRadius = UDim.new(0, 4);
                            var_420.Name = "KeybindButtonCorner";
                            var_420.Parent = var_419;
                            var_421.Name = "KeybindButton";
                            var_421.Parent = var_419;
                            var_421.BackgroundColor3 = Color3.fromRGB(230, 230, 230);
                            var_421.BackgroundTransparency = 1;
                            var_421.Size = UDim2.new(1, 0, 1, 0);
                            var_421.Font = Enum.Font.GothamBold;
                            var_421.Text = tostring(var_412):gsub("Enum.KeyCode.", ""):gsub("Enum.UserInputType.", "");
                            var_421.TextSize = 14;
                            var_421.TextColor3 = getgenv().ui_color[text_color_key];
                            table.insert(theme_listeners[text_color_key], function()
                                var_421.TextColor3 = getgenv().ui_color[text_color_key];
                                return ;
                            end);
                            local tbl_22 = { [Enum.UserInputType.MouseButton1] = "Mouse1", [Enum.UserInputType.MouseButton2] = "Mouse2", [Enum.UserInputType.MouseButton3] = "Mouse3" };
                            var_421.MouseButton1Click:Connect(function()
                                ui_library.button_effect();
                                return ;
                            end);
                            var_421.MouseButton1Click:Connect(function()
                                local flag_28 = nil;
                                var_421.Text = "...";
                                flag_28 = game:GetService("UserInputService").InputBegan:Connect(function(arg_102)
                                    if tbl_22[arg_102.UserInputType] then
                                        var_421.Text = tbl_22[arg_102.UserInputType];
                                        spawn(function()
                                            wait(0.1);
                                            var_412 = arg_102.UserInputType;
                                            str_28 = "UserInputType";
                                            return ;
                                        end);
                                    elseif arg_102.KeyCode ~= Enum.KeyCode.Unknown then
                                        var_421.Text = tostring(arg_102.KeyCode):gsub("Enum.KeyCode.", "");
                                        spawn(function()
                                            wait(0.1);
                                            var_412 = arg_102.KeyCode;
                                            str_28 = "KeyCode";
                                            return ;
                                        end);
                                    end;
                                    flag_28:Disconnect();
                                    return ;
                                end);
                                return ;
                            end);
                            game:GetService("UserInputService").InputBegan:Connect(function(arg_103)
                                if var_412 == arg_103.UserInputType or var_412 == arg_103.KeyCode then
                                    var_413(var_412);
                                end;
                                return ;
                            end);
                            local tbl_23 = {
                                set_key = function(arg_104)
                                    var_412 = arg_104;
                                    var_421.Text = tostring(arg_104):gsub("Enum.KeyCode.", ""):gsub("Enum.UserInputType.", "");
                                    return ;
                                end,
                                get_key = function()
                                    return var_412;
                                end
                            };
                            local var_422 = make_element_id(var_191, str_11, str_27);
                            element_registry.keybinds[var_422] = {
                                get = function()
                                    return tostring(var_412);
                                end,
                                set = function(arg_105)
                                    if not arg_105 then
                                        return ;
                                    end;
                                    local str_29 = tostring(arg_105);
                                    for key_41, value_43 in ipairs(Enum.KeyCode:GetEnumItems()) do
                                        if value_43.Name == str_29 or "Enum.KeyCode." .. value_43.Name == str_29 then
                                            var_412 = value_43;
                                            var_421.Text = value_43.Name;
                                            return ;
                                        end;
                                    end;
                                    for key_42, value_44 in ipairs(Enum.UserInputType:GetEnumItems()) do
                                        if value_44.Name == str_29 or "Enum.UserInputType." .. value_44.Name == str_29 then
                                            var_412 = value_44;
                                            var_421.Text = value_44.Name;
                                            return ;
                                        end;
                                    end;
                                    return ;
                                end
                            };
                            return tbl_23;
                        end
                    };
                end
            };
        end;
        return tbl_5;
    end;
    return breathing_connections;
end;
standalone = function()
    library = get_ui();
    local var_423 = library.create_main({ title = "Grand Piece Online", desc = "" });
    local var_424 = var_423.create_page({ page_name = "Main", page_title = "Main Tab" });
    local var_425 = var_423.create_page({ page_name = "Config", page_title = "Config Tab" });
    local var_426 = var_425.create_section("Config");
    local var_427 = var_425.create_section("Other");
    local var_428 = var_424.create_section("Private Server");
    connections = library;
    getgenv().sea = "First Sea";
    getgenv().ps_code = "";
    getgenv().queued = false;
    getgenv().sea = "First Sea";
    getgenv().queued = false;
    http_service = game:GetService("HttpService");
    Players = game:GetService("Players");
    LocalPlayer = Players.LocalPlayer;
    folder_name = "Feral";
    account_file_path = folder_name .. "/PSAccounts.json";
    if not isfolder(folder_name) then
        makefolder(folder_name);
    end;
    get_player_ps_code = function()
        if not isfile(account_file_path) then
            return "";
        end;
        local var_429, var_430 = pcall(function()
            return http_service:JSONDecode(readfile(account_file_path));
        end);
        if not var_429 or type(var_430) ~= "table" then
            return "";
        end;
        local var_431 = (var_430.accounts or {})[LocalPlayer.Name];
        if not var_431 then
            return "";
        end;
        if type(var_431) == "string" then
            return var_431;
        end;
        if type(var_431) == "table" then
            return var_431.link or var_431.code or var_431.Code or var_431.ps or var_431.PsCode or "";
        end;
        return "";
    end;
    saved_accounts = {};
    if isfile(account_file_path) then
        local var_432, var_433 = pcall(function()
            return http_service:JSONDecode(readfile(account_file_path));
        end);
        if var_432 and type(var_433) == "table" then
            saved_accounts = var_433.accounts or {};
        end;
    end;
    script_key = script_key or "";
    local str_30 = string.format("\t\t\tgetgenv().Loaded=false\n\t\t\ttask.wait(30)\n\t\t\tscript_key = '%s'\n\t\t\tloadstring(game:HttpGet(\"https://api.luarmor.net/files/v3/loaders/3dbb9943ee4a2c8986099c40d714d81e.lua\"))()\n\t\t", script_key);
    local str_31 = string.format("\t\t\tgetgenv().Loaded=false\n\t\t\ttask.wait(10)\n\t\t\tscript_key = '%s'\n\t\t\tloadstring(game:HttpGet(\"https://api.luarmor.net/files/v3/loaders/3dbb9943ee4a2c8986099c40d714d81e.lua\"))()\n\t\t", script_key);
    var_428.create_toggle({ title = "Auto Join PS", desc = "Enable whether to auto join ps or not", default = false }, function(arg_106)
        getgenv().auto_join = arg_106;
        if not arg_106 then
            return ;
        end;
        local var_434, var_435;
        repeat
            task.wait();
            var_434 = get_player_ps_code();
            var_435 = var_434 ~= nil and var_434 ~= "" and var_434 or getgenv().ps_code or "";
        until var_435 and var_435 ~= "" or not getgenv().auto_join;
        warn("PLAYER CODE: " .. var_434);
        warn("Real Code: " .. var_435);
        local flag_29 = nil;
        pcall(function()
            flag_29 = player.PlayerGui.Settings.Main.Code.Text;
            return ;
        end);
        if game.PlaceId ~= 1730877806 then
            repeat
                task.wait();
                pcall(function()
                    flag_29 = player.PlayerGui.Settings.Main.Code.Text;
                    return ;
                end);
            until flag_29 and flag_29 ~= "";
        end;
        if game.PlaceId ~= 1730877806 and flag_29 ~= var_435 then
            warn("Incorrect PlaceId Moving To Menu Chat Tuff");
            if not getgenv().queued then
                queue_on_teleport(str_31);
                getgenv().queued = true;
                task.wait(1);
            end;
            task.wait(5);
            game:GetService("TeleportService"):Teleport(1730877806, game.Players.LocalPlayer);
            connections.create_noti({ title = "Going To Menu", desc = "yeah going to menu chat..", show_time = 3 });
            return ;
        end;
        if flag_29 == var_435 then
            warn("We matched code so gg");
            return ;
        end;
        while getgenv().auto_join and task.wait(1) do
            task.spawn(pcall, function()
                var_434 = get_player_ps_code();
                var_435 = var_434 ~= nil and var_434 ~= "" and var_434 or getgenv().ps_code or "";
                warn("PLAYER CODE: " .. var_434);
                warn("Real Code: " .. var_435);
                return ;
            end);
            task.spawn(function()
                if not getgenv().queued then
                    queue_on_teleport(str_30);
                    getgenv().queued = true;
                    task.wait(1);
                end;
                return ;
            end);
            local var_436, var_437 = pcall(function()
                task.spawn(pcall, function()
                    if not game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("chooseType") and not game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                        warn("trying to invoke " .. var_435);
                        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("reserved"):InvokeServer(var_435);
                        warn("invoked");
                    end;
                    return ;
                end);
                pcall(function()
                    warn("choosetype part");
                    if game:GetService("Players").LocalPlayer.PlayerGui.chooseType.Frame.RemoteEvent then
                        warn("invoked choosetype");
                        game:GetService("Players").LocalPlayer.PlayerGui.chooseType.Frame.RemoteEvent:FireServer(true);
                    end;
                    return ;
                end);
                pcall(function()
                    warn("confirmation thing");
                    if game:GetService("Players").LocalPlayer.PlayerGui.ConfirmationPrompt then
                        warn("invoked confirmation");
                        game:GetService("Players").LocalPlayer.PlayerGui.ConfirmationPrompt.RemoteEvent:FireServer(getgenv().sea);
                    end;
                    return ;
                end);
                return ;
            end);
            if not var_436 and var_437 then
                warn("something errored chat" .. var_437);
            end;
        end;
        return ;
    end);
    getgenv().autorejoin = false;
    var_428.create_toggle({ title = "Auto Rejoin", desc = "Auto Rejoins When Kicked", default = false }, function(arg_107)
        getgenv().autorejoin = arg_107;
        return ;
    end);
    game:GetService("GuiService").ErrorMessageChanged:Connect(function()
        if getgenv().autorejoin then
            local str_32 = tostring(getgenv().ps_code or "");
            local str_33 = tostring(getgenv().script_key or "");
            local str_34 = string.format("repeat task.wait() until game:IsLoaded()\ntask.wait(30)\ngetgenv().PsCode = \"%s\"\ngetgenv().script_key = \"%s\"\ngetgenv().quu = true\nloadstring(game:HttpGet(\"https://api.luarmor.net/files/v3/loaders/3dbb9943ee4a2c8986099c40d714d81e.lua\"))()\nprint(\"loaded\")", str_32, str_33);
            queue_on_teleport(str_34);
            game:GetService("TeleportService"):Teleport(1730877806, game.Players.LocalPlayer);
        end;
        return ;
    end);
    local var_438 = "Feral/Configs";
    local var_439 = var_438 .. "/autoload.txt";
    local var_440 = var_438 .. "/autoload_config.txt";
    local func_28 = function()
        if not isfolder("Feral") then
            makefolder("Feral");
        end;
        if not isfolder(var_438) then
            makefolder(var_438);
        end;
        return ;
    end;
    local func_29 = function()
        local var_441, var_442 = pcall(function()
            if isfile(var_439) then
                return readfile(var_439);
            end;
            return ;
        end);
        if var_441 and type(var_442) == "string" then
            local var_443 = var_442:lower();
            return var_443 == "true" or var_443 == "1" or var_443 == "yes";
        end;
        return false;
    end;
    local func_30 = function(arg_108)
        func_28();
        writefile(var_439, arg_108 and "true" or "false");
        return ;
    end;
    local func_31 = function()
        local var_444, var_445 = pcall(function()
            if isfile(var_440) then
                return readfile(var_440);
            end;
            return ;
        end);
        if var_444 and (type(var_445) == "string" and var_445 ~= "") then
            return var_445;
        end;
        return "default";
    end;
    local func_32 = function(arg_109)
        func_28();
        writefile(var_440, arg_109 ~= "" and arg_109 or "default");
        return ;
    end;
    local var_446 = func_29();
    local var_447 = func_31();
    local var_448 = var_447 ~= "" and var_447 or "default";
    local var_449 = var_426.create_toggle({ title = "Auto-Load Config", desc = "If enabled, on execute auto-loads selected config", default = var_446 }, function(arg_110)
        var_446 = arg_110 and true or false;
        func_30(var_446);
        if var_447 ~= "" and var_447 then
        end;
        return ;
    end);
    local var_450 = var_426.create_box({ title = "Config Name", placeholder = "Enter config name", default = var_448 }, function(arg_111)
        if arg_111 ~= "" then
            var_448 = arg_111;
        end;
        return ;
    end);
    local flag_30 = nil;
    local func_33 = function()
        local tbl_24 = {};
        local var_451, var_452 = pcall(function()
            return getgenv().feral_config and getgenv().feral_config.List() or {};
        end);
        if var_451 and typeof(var_452) == "table" then
            tbl_24 = var_452;
        end;
        if flag_30 then
            flag_30:get_new_list(tbl_24);
        else
            flag_30 = var_426.create_dropdown({ title = "Existing Configs", List = tbl_24, search = false, selected = false, default = "" }, function(arg_112)
                if arg_112 and arg_112 ~= "" then
                    var_448 = arg_112;
                    var_450.set_value(arg_112);
                end;
                return ;
            end);
        end;
        return ;
    end;
    func_33();
    var_426.create_button({ title = "Save Config" }, function()
        local var_453 = var_448 ~= "" and var_448 or "default";
        local var_454, var_455 = getgenv().feral_config.save(var_453);
        if var_454 then
            var_447 = var_453;
            func_32(var_453);
            connections.create_noti({ title = "Config", desc = "Saved as \"" .. var_453 .. "\"", show_time = 5 });
            func_33();
        else
            connections.create_noti({ title = "Config", desc = "Save failed: " .. tostring(var_455), show_time = 5 });
        end;
        return ;
    end);
    var_426.create_button({ title = "Load Config" }, function()
        local var_456 = var_448 ~= "" and var_448 or "default";
        local var_457, var_458 = getgenv().feral_config.load(var_456);
        if var_457 then
            var_447 = var_456;
            func_32(var_456);
            connections.create_noti({ title = "Config", desc = "Loaded \"" .. var_456 .. "\"", show_time = 5 });
        else
            connections.create_noti({ title = "Config", desc = "Load failed: " .. tostring(var_458), show_time = 5 });
        end;
        return ;
    end);
    var_426.create_button({ title = "Delete Config" }, function()
        local var_459 = var_448;
        if not var_459 or var_459 == "" then
            connections.create_noti({ title = "Config", desc = "No config name selected.", show_time = 5 });
            return ;
        end;
        local var_460, var_461 = getgenv().feral_config.delete(var_459);
        if var_460 then
            connections.create_noti({ title = "Config", desc = "Deleted \"" .. var_459 .. "\"", show_time = 5 });
            var_448 = "default";
            var_450.set_value(var_448);
            if var_447 == var_459 then
                var_447 = "default";
                func_32(var_447);
            end;
            func_33();
        else
            connections.create_noti({ title = "Config", desc = "Delete failed: " .. tostring(var_461), show_time = 5 });
        end;
        return ;
    end);
    var_426.create_button({ title = "Refresh Config List" }, function()
        func_33();
        connections.create_noti({ title = "Config", desc = "Config list refreshed.", show_time = 3 });
        return ;
    end);
    var_428.create_box({ title = "Private Server Code", placeholder = "", default = "", Number = false }, function(arg_113)
        getgenv().ps_code = arg_113;
        return ;
    end);
    var_428.create_dropdown({ title = "Teleport Sea", List = { "First Sea", "Second Sea" }, default = "First Sea", search = false, selected = false }, function(arg_114, arg_115)
        getgenv().sea = arg_114;
        return ;
    end);
    local var_462 = var_427.create_keybind({ title = "Toggle UI", default = Enum.KeyCode.RightShift }, function(arg_116)
        local child_3 = game.CoreGui:FindFirstChild("Feral GUI");
        if child_3 then
            child_3.Enabled = not child_3.Enabled;
        end;
        return ;
    end);
    var_427.create_account_table({
        title = "Account Table [PS]",
        default = saved_accounts,
        enable_config_binding = true,
        on_account_select = function(arg_117, arg_118, arg_119)
            return ;
        end
    }, function(arg_120)
        writefile(account_file_path, http_service:JSONEncode({ accounts = arg_120 }));
        return ;
    end);
    task.spawn(function()
        task.wait(0.5);
        local game_9 = game:GetService("Players").LocalPlayer;
        local game_10 = game:GetService("HttpService");
        local flag_31 = false;
        if isfile(account_file_path) then
            local var_463, var_464 = pcall(function()
                return game_10:JSONDecode(readfile(account_file_path));
            end);
            if var_463 and type(var_464) == "table" then
                local var_465 = (var_464.accounts or {})[game_9.Name];
                if var_465 then
                    local flag_32 = nil;
                    if type(var_465) == "table" and var_465.config and var_465.config ~= "" then
                        flag_32 = var_465.config;
                    end;
                    if flag_32 then
                        local var_466, var_467 = getgenv().feral_config.load(flag_32);
                        if var_466 then
                            library.create_noti({ title = "Config", desc = "Auto-loaded account config \"" .. flag_32 .. "\" for " .. game_9.Name, show_time = 4 });
                            flag_31 = true;
                        else
                            library.create_noti({ title = "Config", desc = "Account config load failed: " .. tostring(var_467), show_time = 4 });
                        end;
                    end;
                end;
            end;
        end;
        if not flag_31 then
            if not var_446 then
                return ;
            end;
            if not func_29() then
                return ;
            end;
            local var_468 = func_31();
            local var_469, var_470 = getgenv().feral_config.load(var_468);
            if var_469 then
                library.create_noti({ title = "Config", desc = "Auto-loaded \"" .. var_468 .. "\"", show_time = 4 });
            else
                library.create_noti({ title = "Config", desc = "Auto-load failed: " .. tostring(var_470), show_time = 4 });
            end;
        end;
        return ;
    end);
    return ;
end;
if game.PlaceId == 1730877806 then
    standalone();
    return ;
end;
library = get_ui();
Islands = { ["Town of Beginnings"] = CFrame.new(-528, 5, -3423), ["Shell's Town"] = CFrame.new(-1299, 4, -5052), Sandora = CFrame.new(-1545, 4, -3353), ["Orange Town"] = CFrame.new(-4448, 5, -6638), ["Restaurant Baratie"] = CFrame.new(-2964, 6, -6672), ["Logue Town"] = CFrame.new(-6589, 7, -7643), ["Roca Island"] = CFrame.new(1564, 154, -6598), ["Shark Park"] = CFrame.new(-1572, 11, -10082), ["Reverse Mountain"] = CFrame.new(-8030, 17, -8785), ["Sphinx Island"] = CFrame.new(-4006, 41, -9138), ["World Scroll"] = CFrame.new(-7350.17431640625, 4.758918762207031, -14949.48828125), ["Mysterious Cliff"] = CFrame.new(83, 413, -8286), ["Kori Island"] = CFrame.new(-4267, 169, -2974), ["A rock"] = CFrame.new(2539, 5, -8363), ["Coco Island"] = CFrame.new(-3096, 96, -11762), ["Fishman Cave"] = CFrame.new(1838, 4, -12173), ["Fishman Island"] = CFrame.new(7996, -2154, -17075), ["Marine Fort F-1"] = CFrame.new(393, 18, -4467), ["Marine Base G-1"] = CFrame.new(-5979, 57, -11496), Spooksville = CFrame.new(-7427, 26, -793), Colosseum = CFrame.new(-2020, 7, -7675), ["Land of the Sky"] = CFrame.new(3449, 1438, -9094), ["Island Of Zou"] = CFrame.new(-3070, 9, -5258), Transylvania = CFrame.new(-9619, 29, -1875), Hell = CFrame.new(18944, 8122, -12501) };
island_names = {};
for var_471, var_472 in next, Islands, nil do
    table.insert(island_names, var_471);
end;
Players = game:GetService("Players");
ReplicatedStorage = game:GetService("ReplicatedStorage");
workspace_service = game:GetService("Workspace");
RunService = game:GetService("RunService");
tween_service = game:GetService("TweenService");
user_input_service = game:GetService("UserInputService");
marketplace_service = game:GetService("MarketplaceService");
script_context = game:GetService("ScriptContext");
player = Players.LocalPlayer;
character = player.Character;
player_character = player.Character;
player.CharacterAdded:Connect(function()
    repeat
        task.wait();
    until player.Character:FindFirstChild("Humanoid");
    local var_473;
    player_character, var_473 = player.Character, player.Character;
    character = var_473;
    return ;
end);
task.spawn(pcall, function()
    camera = workspace_service.CurrentCamera;
    stats_folder = ReplicatedStorage:FindFirstChild("Stats" .. player.Name);
    inventory_value = stats_folder:FindFirstChild("Inventory"):FindFirstChild("Inventory");
    virtual_input_manager = cloneref(Instance.new("VirtualInputManager"));
    events = ReplicatedStorage:WaitForChild("Events");
    quest_event = events:FindFirstChild("Quest");
    tools_event = events.Tools;
    shop_event = events.Shop;
    set_spawn_event = events.SetSpawn;
    knocked_out_event = events.KnockedOut;
    stats_event = events.stats;
    skill_event = events:FindFirstChild("Skill");
    buyable_items = workspace_service:WaitForChild("BuyableItems");
    return ;
end);
npcs = { Daph = Vector3.new(-575, 5, -3431), Noah = Vector3.new(-1709, 4, -3377), Robert = Vector3.new(-1441, 10, -5102), Zen = Vector3.new(-3172, 12, -5227), Gonny = Vector3.new(-4248, 42, -8928), Sarah = Vector3.new(-547, 9, -3402), max = Vector3.new(-4270, 42, -8941), Zhen = Vector3.new(4094, 1819, -9831) };
chests = { Common = "rbxassetid://10779253534", Uncommon = "rbxassetid://10858352843", Rare = "rbxassetid://10788852296", Legendary = "rbxassetid://10798559852" };
rarity_colors = { Common = Color3.fromRGB(181, 135, 99), Uncommon = Color3.fromRGB(144, 238, 144), Rare = Color3.fromRGB(135, 206, 250), Legendary = Color3.fromRGB(255, 200, 100), Mythic = Color3.fromRGB(255, 182, 193) };
selected_chests = { Common = true, Uncommon = true, Rare = true, Legendary = true, Mythic = true };
config = { maxDistance = 2500, max_distance = 2500, studs_offset = Vector3.new(0, 3, 0), font = Enum.Font.GothamBold, text_size = 16, dot_threshold = 0.05 };
getgenv().chest_esp_enabled = false;
getgenv().medal_esp_enabled = false;
getgenv().anti_stun = true;
getgenv().infinite_jump = false;
getgenv().tp_behind_enabled = false;
getgenv().autofish = false;
getgenv().farm = false;
getgenv().break_state = false;
task.wait();
local tbl_25 = { enabled = false, lookup = {}, cons = { heartbeat = nil, effects_child_added = nil, effects_added = nil } };
local tbl_26 = { enabled = false, lookup = {}, cons = {} };
local tbl_27 = { Enabled = false };
local func_34 = function(arg_121)
    local var_474 = arg_121.Backpack;
    local var_475 = arg_121.Character;
    for var_476, var_477 in next, var_474:GetChildren() do
        if var_477:IsA("Tool") and var_477:GetAttribute("devilFruit") then
            return var_477.Name;
        end;
    end;
    for var_478, var_479 in next, var_475:GetChildren() do
        if var_479:IsA("Tool") and var_479:GetAttribute("devilFruit") then
            return var_479.Name;
        end;
    end;
    return nil;
end;
local tbl_28 = { Enabled = false, Settings = { Enabled = false, DevilFruit = false, Boxes = true, Healthbar = true, Names = true, TeamCheck = false, max_distance = 2000, UseDisplayName = true, Thickness = 2, BoxColor = Color3.fromRGB(90, 176, 255), TextColor = Color3.fromRGB(220, 235, 255), HealthbarWidth = 10, HealthbarOffset = 8, HealthbarOutlineThickness = 1, HealthbarSide = "Left", HealthbarOutside = true, MinBoxSize = 2, BoxPadding = 2, NameHeight = 18, BoxFillTransparency = 0.45, BoxGradientEnabled = true, BoxGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(12, 26, 58)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(28, 92, 184)), ColorSequenceKeypoint.new(1, Color3.fromRGB(90, 176, 255)) }), BoxGradientRotation = 90, BoxStrokeGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(60, 140, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(130, 200, 255)) }), NameGradientEnabled = true, NameGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(160, 200, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 255, 255)) }), NameGradientRotation = 0, HealthFillGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(16, 42, 96)), ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 195, 255)) }), HealthStrokeGradient = ColorSequence.new({ ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 120, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(130, 210, 255)) }) } };
local flag_33 = nil;
local tbl_29 = {};
local tbl_30 = {};
local flag_34 = false;
local num_25 = 50;
local func_35 = function(arg_122)
    local var_480 = inventory_value and inventory_value.Value;
    if not var_480 then
        warn("Inventory not found or not loaded");
        return nil;
    end;
    local var_481, var_482 = pcall(function()
        return game:GetService("HttpService"):JSONDecode(var_480);
    end);
    if not var_481 then
        warn("Failed to decode inventory");
        return nil;
    end;
    return var_482[arg_122] or 0;
end;
local func_36 = function()
    return stats_folder.Stats.Peli.Value;
end;
local func_37 = function()
    return stats_folder.Inventory.Halloween25Candy.Value;
end;
local func_38 = function()
    return stats_folder.Stats.Level.Value;
end;
local func_39 = function()
    return stats_folder.Stamina.Value;
end;
local func_40 = function()
    return stats_folder.Quest.CurrentQuest.Value;
end;
local func_41 = function(arg_123)
    return arg_123 and arg_123:IsA("Model") and arg_123.Name:match("-") and arg_123:FindFirstChildWhichIsA("MeshPart");
end;
local func_42 = function(arg_124)
    local var_483 = arg_124 and arg_124:FindFirstChildWhichIsA("MeshPart");
    if not var_483 then
        return "Mythic";
    end;
    local var_484, var_485 = pcall(function()
        return gethiddenproperty(var_483, "MeshID");
    end);
    if not var_484 or not var_485 then
        local var_486, var_487 = pcall(function()
            return var_483.MeshId;
        end);
        var_485 = var_486 and var_487 or nil;
    end;
    local var_488 = "Mythic";
    if var_485 then
        for key_43, value_45 in pairs(chests) do
            if value_45 == var_485 then
                var_488 = key_43;
                return var_488;
            end;
        end;
    end;
    return var_488;
end;
local func_43 = function(arg_125)
    if arg_125.PrimaryPart then
        return arg_125.PrimaryPart;
    end;
    return arg_125:FindFirstChildWhichIsA("BasePart");
end;
local func_44 = function(arg_126)
    return string.format("rgb(%d, %d, %d)", math.floor(arg_126.R * 255 + 0.5), math.floor(arg_126.G * 255 + 0.5), math.floor(arg_126.B * 255 + 0.5));
end;
local func_45 = function(arg_127, arg_128, arg_129)
    local var_489 = Instance.new("BillboardGui");
    var_489.Name = "ChestESP_" .. arg_128;
    var_489.AlwaysOnTop = true;
    var_489.LightInfluence = 0;
    var_489.Size = UDim2.fromOffset(180, 20);
    var_489.StudsOffsetWorldSpace = config.studs_offset;
    var_489.max_distance = config.maxDistance;
    var_489.Adornee = arg_127;
    var_489.ResetOnSpawn = false;
    var_489.Enabled = true;
    var_489.Parent = arg_127;
    local var_490 = Instance.new("TextLabel");
    var_490.BackgroundTransparency = 1;
    var_490.Size = UDim2.new(1, 0, 1, 0);
    var_490.Font = config.font;
    var_490.TextSize = config.text_size;
    var_490.TextXAlignment = Enum.TextXAlignment.Center;
    var_490.TextYAlignment = Enum.TextYAlignment.Center;
    var_490.TextColor3 = Color3.fromRGB(255, 255, 255);
    var_490.TextStrokeTransparency = 0.6;
    var_490.TextStrokeColor3 = Color3.new(0, 0, 0);
    var_490.RichText = true;
    var_490.Parent = var_489;
    return var_489, var_490;
end;
tbl_25.make_esp = function(arg_130, arg_131)
    if arg_130.lookup[arg_131] then
        return ;
    end;
    local var_491 = func_43(arg_131);
    if not var_491 then
        return ;
    end;
    local var_492 = func_42(arg_131);
    if not selected_chests[var_492] then
        return ;
    end;
    local var_493 = rarity_colors[var_492];
    if not var_493 then
        return ;
    end;
    local var_494, var_495 = func_45(var_491, var_492, var_493);
    arg_130.lookup[arg_131] = { gui = var_494, label = var_495, adornee = var_491, rarity = var_492, color = var_493 };
    return ;
end;
tbl_25.remove_esp = function(arg_132, arg_133)
    local var_496 = arg_132.lookup[arg_133];
    if not var_496 then
        return ;
    end;
    if var_496.gui then
        var_496.gui:Destroy();
    end;
    arg_132.lookup[arg_133] = nil;
    return ;
end;
tbl_25.destroy_all = function(arg_134)
    for key_44, value_46 in pairs(arg_134.lookup) do
        if value_46.gui then
            value_46.gui:Destroy();
        end;
    end;
    arg_134.lookup = {};
    return ;
end;
tbl_25.update_all = function(arg_135)
    local var_497 = player.Character;
    local var_498 = var_497 and var_497:FindFirstChild(var);
    if not var_498 then
        for key_45, value_47 in pairs(arg_135.lookup) do
            if value_47.gui then
                value_47.gui.Enabled = false;
            end;
        end;
        return ;
    end;
    local var_499 = camera.CFrame;
    local var_500 = var_499.Position;
    local var_501 = var_499.LookVector;
    local tbl_31 = {};
    for key_46, value_48 in pairs(arg_135.lookup) do
        local var_502 = value_48.adornee;
        local var_503 = value_48.gui;
        local var_504 = value_48.label;
        if not key_46 or not key_46.Parent or not var_502 or not var_502.Parent or not var_503 or not var_504 then
            table.insert(tbl_31, key_46);
        else
            local var_505 = var_502.Position + config.studs_offset;
            local var_506 = (var_505 - var_498.Position).Magnitude;
            local num_26 = math.floor(var_506 + 0.5);
            local var_507 = var_505 - var_500;
            local var_508 = var_507.Magnitude > 0.001 and var_507.Unit:Dot(var_501) > config.dot_threshold;
            local var_509, var_510 = camera:WorldToViewportPoint(var_505);
            local var_511 = var_510 and var_509.Z > 0;
            local var_512 = var_506 <= config.maxDistance;
            var_503.Enabled = var_508 and var_511 and var_512;
            if var_503.Enabled then
                var_504.Text = string.format("<font color=\"%s\">%s</font> [ %d ]", func_44(value_48.color), string.upper(value_48.rarity), num_26);
            end;
        end;
    end;
    for key_47, value_49 in ipairs(tbl_31) do
        arg_135:remove_esp(value_49);
    end;
    return ;
end;
tbl_25.scan_existing = function(arg_136)
    local child_4 = workspace_service:FindFirstChild("Effects");
    if not child_4 then
        return ;
    end;
    for key_48, value_50 in ipairs(child_4:GetChildren()) do
        if func_41(value_50) then
            arg_136:make_esp(value_50);
        end;
    end;
    return ;
end;
tbl_25.bind_effects = function(arg_137)
    local child_5 = workspace_service:FindFirstChild("Effects");
    if child_5 and not arg_137.cons.effects_child_added then
        arg_137.cons.effects_child_added = child_5.ChildAdded:Connect(function(arg_138)
            if arg_137.enabled and func_41(arg_138) then
                arg_137:make_esp(arg_138);
            end;
            return ;
        end);
    end;
    if not child_5 and not arg_137.cons.effects_added then
        arg_137.cons.effects_added = workspace_service.ChildAdded:Connect(function(arg_139)
            if arg_139.Name == "Effects" then
                if arg_137.cons.effects_added then
                    arg_137.cons.effects_added:Disconnect();
                    arg_137.cons.effects_added = nil;
                end;
                if not arg_137.cons.effects_child_added then
                    arg_137.cons.effects_child_added = arg_139.ChildAdded:Connect(function(arg_140)
                        if arg_137.enabled and func_41(arg_140) then
                            arg_137:make_esp(arg_140);
                        end;
                        return ;
                    end);
                end;
                arg_137:scan_existing();
            end;
            return ;
        end);
    end;
    return ;
end;
tbl_25.start = function(arg_141)
    if arg_141.enabled then
        return ;
    end;
    arg_141.enabled = true;
    arg_141:scan_existing();
    if not arg_141.cons.heartbeat then
        arg_141.cons.heartbeat = RunService.Heartbeat:Connect(function()
            if arg_141.enabled then
                arg_141:update_all();
            end;
            return ;
        end);
    end;
    arg_141:bind_effects();
    return ;
end;
tbl_25.stop = function(arg_142)
    if not arg_142.enabled then
        return ;
    end;
    arg_142.enabled = false;
    for key_49, value_51 in pairs(arg_142.cons) do
        if value_51 then
            value_51:Disconnect();
            arg_142.cons[key_49] = nil;
        end;
    end;
    arg_142:destroy_all();
    return ;
end;
tbl_25.apply_filter = function(arg_143)
    for key_50, value_52 in pairs(arg_143.lookup) do
        if not selected_chests[value_52.rarity] then
            arg_143:remove_esp(key_50);
        end;
    end;
    arg_143:scan_existing();
    return ;
end;
local func_46 = function(arg_144)
    return arg_144 and arg_144.Name:match("Medal") and arg_144:GetAttribute("FightingStyle");
end;
local func_47 = function(arg_145, arg_146)
    local var_513 = Instance.new("BillboardGui");
    var_513.Name = "MedalESP";
    var_513.AlwaysOnTop = true;
    var_513.LightInfluence = 0;
    var_513.Size = UDim2.fromOffset(180, 20);
    var_513.StudsOffsetWorldSpace = config.studs_offset;
    var_513.max_distance = config.max_distance;
    var_513.Adornee = arg_145;
    var_513.ResetOnSpawn = false;
    var_513.Parent = arg_145;
    local var_514 = Instance.new("TextLabel");
    var_514.BackgroundTransparency = 1;
    var_514.Size = UDim2.new(1, 0, 1, 0);
    var_514.Font = config.font;
    var_514.TextSize = config.text_size;
    var_514.TextXAlignment = Enum.TextXAlignment.Center;
    var_514.TextColor3 = Color3.fromRGB(255, 215, 0);
    var_514.TextStrokeTransparency = 0.6;
    var_514.TextStrokeColor3 = Color3.new(0, 0, 0);
    var_514.Parent = var_513;
    return var_513, var_514;
end;
tbl_26.make_esp = function(arg_147, arg_148)
    if arg_147.lookup[arg_148] then
        return ;
    end;
    local var_515 = arg_148:IsA("BasePart") and arg_148 or arg_148:FindFirstChildWhichIsA("BasePart");
    if not var_515 then
        return ;
    end;
    local var_516 = arg_148:GetAttribute("FightingStyle");
    if not var_516 then
        return ;
    end;
    local var_517, var_518 = func_47(var_515, var_516);
    arg_147.lookup[arg_148] = { gui = var_517, label = var_518, adornee = var_515, style = var_516 };
    return ;
end;
tbl_26.remove_esp = function(arg_149, arg_150)
    local var_519 = arg_149.lookup[arg_150];
    if var_519 and var_519.gui then
        var_519.gui:Destroy();
    end;
    arg_149.lookup[arg_150] = nil;
    return ;
end;
tbl_26.update_all = function(arg_151)
    local var_520 = player.Character and player.Character:FindFirstChild(var);
    if not var_520 then
        return ;
    end;
    local var_521 = camera.CFrame;
    local var_522 = var_521.Position;
    local var_523 = var_521.LookVector;
    for key_51, value_53 in pairs(arg_151.lookup) do
        if not key_51.Parent or not value_53.adornee.Parent then
            arg_151:remove_esp(key_51);
        else
            local var_524 = value_53.adornee.Position + config.studs_offset;
            local var_525 = (var_524 - var_520.Position).Magnitude;
            local var_526 = var_524 - var_522;
            local var_527 = var_526.Magnitude > 0.001 and var_526.Unit:Dot(var_523) > config.dot_threshold;
            local var_528, var_529 = camera:WorldToViewportPoint(var_524);
            value_53.gui.Enabled = var_527 and var_529 and var_525 <= config.max_distance;
            if value_53.gui.Enabled then
                value_53.label.Text = string.format("%s [%d]", value_53.style, math.floor(var_525));
            end;
        end;
    end;
    return ;
end;
tbl_26.start = function(arg_152)
    if arg_152.enabled then
        return ;
    end;
    arg_152.enabled = true;
    local child_6 = workspace_service:FindFirstChild("Effects");
    if child_6 then
        for key_52, value_54 in ipairs(child_6:GetChildren()) do
            if func_46(value_54) then
                arg_152:make_esp(value_54);
            end;
        end;
        arg_152.cons.child_added = child_6.ChildAdded:Connect(function(arg_153)
            if arg_152.enabled and func_46(arg_153) then
                arg_152:make_esp(arg_153);
            end;
            return ;
        end);
    end;
    arg_152.cons.heartbeat = RunService.Heartbeat:Connect(function()
        if arg_152.enabled then
            arg_152:update_all();
        end;
        return ;
    end);
    return ;
end;
tbl_26.stop = function(arg_154)
    if not arg_154.enabled then
        return ;
    end;
    arg_154.enabled = false;
    for key_53, value_55 in pairs(arg_154.cons) do
        if value_55 then
            value_55:Disconnect();
        end;
    end;
    arg_154.cons = {};
    for key_54, value_56 in pairs(arg_154.lookup) do
        arg_154:remove_esp(key_54);
    end;
    return ;
end;
local var_530 = player;
local var_531 = var_530.Idled;
for key_55, value_57 in ipairs(getconnections(var_531)) do
    value_57:Disable();
end;
var_531:Connect(function()
    return false;
end);
tbl_27.Enable = function()
    local flag_35 = nil;
    pcall(function()
        flag_35 = game:GetService("ReplicatedStorage").Events.CIcklcon;
        return ;
    end);
    tbl_27.Enabled = true;
    local var_532 = player.Idled;
    for key_56, value_58 in ipairs(getconnections(var_532)) do
        value_58:Disable();
    end;
    var_532:Connect(function()
        return false;
    end);
    local func_48 = function()
        return player.PlayerGui.Quest.Enabled;
    end;
    local func_49 = function()
        local var_534, var_535 = pcall(function()
            local var_533 = player.PlayerGui.HUD.Main.Peli.TextLabel.Text;
            local str_35 = string.split(var_533, ": ");
            return tonumber(str_35[2]);
        end);
        return var_534 and var_535 or 0;
    end;
    local flag_36 = false;
    if func_49() < 300 and not string.find(inventory_value.Value, "Rifle") then
        repeat
            task.wait();
            if not tbl_27.Enabled or string.find(inventory_value.Value, "Rifle") then
                break;
            end;
            for key_57, value_59 in pairs(workspace_service.Env:GetChildren()) do
                if not tbl_27.Enabled then
                    break;
                end;
                if (value_59.Name == "Part" or value_59.Name == "Chest") and value_59:FindFirstChild("ProximityPrompt") then
                    local var_536 = value_59:GetPivot().Position;
                    if not (math.abs(var_536.Y) > 60) then
                        local var_537 = player.Character.HumanoidRootPart.Position;
                        if not ((var_537 - var_536).Magnitude > 300) then
                            player.Character.HumanoidRootPart.CFrame = CFrame.new(var_537.X, 25, var_537.Z);
                            local var_538 = (Vector3.new(var_537.X, 0, var_537.Z) - Vector3.new(var_536.X, 0, var_536.Z)).Magnitude / 40;
                            local tween_5 = tween_service:Create(player.Character.HumanoidRootPart, TweenInfo.new(var_538, Enum.EasingStyle.Linear), { CFrame = CFrame.new(var_536.X, 25, var_536.Z) });
                            tween_5:Play();
                            task.spawn(function()
                                while tween_5.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                                    player.Character.HumanoidRootPart.Velocity = Vector3.zero;
                                    task.wait();
                                end;
                                return ;
                            end);
                            tween_5.Completed:Wait();
                            if not tbl_27.Enabled then
                                break;
                            end;
                            local num_27 = math.abs(25 - var_536.Y) / 15;
                            local tween_6 = tween_service:Create(player.Character.HumanoidRootPart, TweenInfo.new(num_27, Enum.EasingStyle.Linear), { CFrame = CFrame.new(var_536.X, var_536.Y, var_536.Z) });
                            tween_6:Play();
                            task.spawn(function()
                                while tween_6.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                                    player.Character.HumanoidRootPart.Velocity = Vector3.zero;
                                    task.wait();
                                end;
                                return ;
                            end);
                            tween_6.Completed:Wait();
                            if not tbl_27.Enabled then
                                break;
                            end;
                            repeat
                                task.wait();
                                if not tbl_27.Enabled then
                                    break;
                                end;
                                pcall(function()
                                    fireproximityprompt(value_59.ProximityPrompt);
                                    return ;
                                end);
                            until not value_59:FindFirstChild("ProximityPrompt") or not value_59.ProximityPrompt.Enabled or not tbl_27.Enabled;
                            if func_49() >= 300 or string.find(inventory_value.Value, "Rifle") then
                                flag_36 = true;
                                break;
                            end;
                        end;
                    end;
                end;
            end;
        until flag_36 or func_49() >= 300 or string.find(inventory_value.Value, "Rifle") or not tbl_27.Enabled;
    end;
    if not tbl_27.Enabled then
        return ;
    end;
    if func_49() >= 300 and not string.find(inventory_value.Value, "Rifle") then
        local vector = Vector3.new(-532, 6, -3450);
        local var_539 = player.Character.HumanoidRootPart.Position;
        player.Character.HumanoidRootPart.CFrame = CFrame.new(var_539.X, 25, var_539.Z);
        local var_540 = (Vector3.new(var_539.X, 0, var_539.Z) - Vector3.new(vector.X, 0, vector.Z)).Magnitude / 40;
        local tween_7 = tween_service:Create(player.Character.HumanoidRootPart, TweenInfo.new(var_540, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector.X, 25, vector.Z) });
        tween_7:Play();
        task.spawn(function()
            while tween_7.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                player.Character.HumanoidRootPart.Velocity = Vector3.zero;
                task.wait();
            end;
            return ;
        end);
        tween_7.Completed:Wait();
    end;
    if not tbl_27.Enabled then
        return ;
    end;
    if func_49() >= 300 and not string.find(inventory_value.Value, "Rifle") then
        repeat
            task.wait();
            if not tbl_27.Enabled then
                break;
            end;
            shop_event:InvokeServer(buyable_items.Rifle, 1);
        until string.find(inventory_value.Value, "Rifle") or not tbl_27.Enabled;
    end;
    if not tbl_27.Enabled then
        return ;
    end;
    local func_50 = function(arg_155)
        player:RequestStreamAroundAsync(arg_155);
        player.Character.HumanoidRootPart.CFrame = CFrame.new(arg_155);
        for i_3 = 1, 10, 1 do
            if not tbl_27.Enabled then
                break;
            end;
            player.Character.HumanoidRootPart.CFrame = CFrame.new(arg_155);
            task.wait(0.05);
        end;
        knocked_out_event:FireServer("self");
        return ;
    end;
    if stats_folder.Stats.SpawnPoint.Value ~= "Fishman Island" then
        repeat
            task.wait(1);
            if not tbl_27.Enabled then
                break;
            end;
            pcall(function()
                func_50(Vector3.new(7976, -2153, -17075));
                quest_event:InvokeServer({ "npcChat", true });
                set_spawn_event:FireServer(nil, workspace_service.NPCs.Robo);
                return ;
            end);
        until stats_folder.Stats.SpawnPoint.Value == "Fishman Island" or not tbl_27.Enabled;
    end;
    if not tbl_27.Enabled then
        return ;
    end;
    repeat
        task.wait();
        if not tbl_27.Enabled then
            return ;
        end;
    until player.Character.Humanoid.Health > 25;
    task.wait(1);
    if not tbl_27.Enabled then
        return ;
    end;
    repeat
        task.wait();
        if not tbl_27.Enabled then
            return ;
        end;
    until player.Character.Humanoid.Health > 25;
    task.wait(1);
    if not tbl_27.Enabled then
        return ;
    end;
    local vector_2 = Vector3.new(7838, -2151, -17134);
    local var_541 = player.Character.HumanoidRootPart;
    local var_542 = var_541.Position;
    local var_543 = var_542.Y + 100;
    var_541.CFrame = CFrame.new(var_542.X, var_543, var_542.Z);
    local var_544 = (Vector3.new(var_542.X, 0, var_542.Z) - Vector3.new(vector_2.X, 0, vector_2.Z)).Magnitude / 40;
    local tween_8 = tween_service:Create(var_541, TweenInfo.new(var_544, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector_2.X, var_543, vector_2.Z) });
    tween_8:Play();
    task.spawn(function()
        while tween_8.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
            var_541.Velocity = Vector3.zero;
            task.wait();
        end;
        return ;
    end);
    tween_8.Completed:Wait();
    if not tbl_27.Enabled then
        return ;
    end;
    local num_28 = math.abs(var_543 - vector_2.Y) / 15;
    local tween_9 = tween_service:Create(var_541, TweenInfo.new(num_28, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector_2.X, vector_2.Y, vector_2.Z) });
    tween_9:Play();
    task.spawn(function()
        while tween_9.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
            var_541.Velocity = Vector3.zero;
            task.wait();
        end;
        return ;
    end);
    tween_9.Completed:Wait();
    if not tbl_27.Enabled then
        return ;
    end;
    tools_event:InvokeServer("equip", "Rifle");
    task.wait(0.5);
    if not tbl_27.Enabled then
        return ;
    end;
    if not player.Character:FindFirstChild("Rifle") then
        player.Character.Humanoid:EquipTool(player.Backpack.Rifle);
    end;
    rifle = require(ReplicatedStorage.Modules.GunHandle).New("Rifle", player.Character:FindFirstChild("Rifle"));
    while tbl_27.Enabled and task.wait(0.1) do
        pcall(function()
            if not tbl_27.Enabled then
                return ;
            end;
            if not func_48() and stats_folder.Stats.Level.Value >= 190 then
                local vector_3 = Vector3.new(7733, -2176, -17222);
                local var_545 = player.Character.HumanoidRootPart;
                local var_546 = var_545.Position;
                local var_547 = var_546.Y + 100;
                var_545.CFrame = CFrame.new(var_546.X, var_547, var_546.Z);
                local var_548 = (Vector3.new(var_546.X, 0, var_546.Z) - Vector3.new(vector_3.X, 0, vector_3.Z)).Magnitude / 40;
                local tween_10 = tween_service:Create(var_545, TweenInfo.new(var_548, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector_3.X, var_547, vector_3.Z) });
                tween_10:Play();
                task.spawn(function()
                    while tween_10.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                        var_545.Velocity = Vector3.zero;
                        task.wait();
                    end;
                    return ;
                end);
                tween_10.Completed:Wait();
                if not tbl_27.Enabled then
                    return ;
                end;
                local num_29 = math.abs(var_547 - vector_3.Y) / 15;
                local tween_11 = tween_service:Create(var_545, TweenInfo.new(num_29, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector_3.X, vector_3.Y, vector_3.Z) });
                tween_11:Play();
                task.spawn(function()
                    while tween_11.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                        var_545.Velocity = Vector3.zero;
                        task.wait();
                    end;
                    return ;
                end);
                tween_11.Completed:Wait();
                if not tbl_27.Enabled then
                    return ;
                end;
                task.wait(1);
                if not tbl_27.Enabled then
                    return ;
                end;
                repeat
                    task.wait();
                    if not tbl_27.Enabled then
                        break;
                    end;
                    quest_event:InvokeServer({ "takequest", "Help becky" });
                until func_48() or not tbl_27.Enabled;
                if not tbl_27.Enabled then
                    return ;
                end;
                local vector_4 = Vector3.new(7838, -2151, -17134);
                local var_549 = player.Character.HumanoidRootPart;
                local var_550 = var_549.Position;
                local var_551 = var_550.Y + 100;
                var_549.CFrame = CFrame.new(var_550.X, var_551, var_550.Z);
                local var_552 = (Vector3.new(var_550.X, 0, var_550.Z) - Vector3.new(vector_4.X, 0, vector_4.Z)).Magnitude / 40;
                local tween_12 = tween_service:Create(var_549, TweenInfo.new(var_552, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector_4.X, var_551, vector_4.Z) });
                tween_12:Play();
                task.spawn(function()
                    while tween_12.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                        var_549.Velocity = Vector3.zero;
                        task.wait();
                    end;
                    return ;
                end);
                tween_12.Completed:Wait();
                if not tbl_27.Enabled then
                    return ;
                end;
                local num_30 = math.abs(var_551 - vector_4.Y) / 15;
                local tween_13 = tween_service:Create(var_549, TweenInfo.new(num_30, Enum.EasingStyle.Linear), { CFrame = CFrame.new(vector_4.X, vector_4.Y, vector_4.Z) });
                tween_13:Play();
                task.spawn(function()
                    while tween_13.PlaybackState == Enum.PlaybackState.Playing and tbl_27.Enabled do
                        var_549.Velocity = Vector3.zero;
                        task.wait();
                    end;
                    return ;
                end);
                tween_13.Completed:Wait();
            end;
            if not tbl_27.Enabled then
                return ;
            end;
            local var_553 = player.Character;
            local var_554 = var_553 and var_553:FindFirstChild(var);
            if not var_554 then
                return ;
            end;
            local flag_37 = nil;
            local num_31 = math.huge;
            for key_58, value_60 in pairs(workspace_service.NPCs:GetChildren()) do
                if value_60.Name == "Fishman Karate User" and value_60:FindFirstChild(var) and not value_60:FindFirstChild("Blocking") then
                    local var_555 = (var_554.Position - value_60.HumanoidRootPart.Position).Magnitude;
                    if var_555 < num_31 then
                        flag_37 = value_60;
                        num_31 = var_555;
                    end;
                end;
            end;
            if flag_37 then
                if not var_553:FindFirstChild("Rifle") then
                    return ;
                end;
                if not rifle.Reloaded then
                    rifle:Reload();
                end;
                pcall(function()
                    local var_556 = var_553.RifleGun.Hole;
                    local tbl_32 = { Gun = rifle.Name, Position = flag_37.Head.Position, Start = var_556.CFrame, joe = "true" };
                    if flag_37.Parent and flag_37:FindFirstChild(var) then
                        flag_35:FireServer("fire", tbl_32);
                    end;
                    rifle.Reloaded = false;
                    rifle.Equiped = true;
                    rifle.IsAiming = true;
                    rifle.Ready = true;
                    stats_event:FireServer("GunMastery", nil, 1);
                    return ;
                end);
            end;
            return ;
        end);
    end;
    return ;
end;
tbl_27.Disable = function()
    tbl_27.Enabled = false;
    return ;
end;
local func_51 = function(arg_156)
    return math.floor(arg_156 + 0.5);
end;
local func_52 = function(arg_157)
    if not arg_157.Character then
        return true;
    end;
    local var_557 = arg_157.Character:FindFirstChildOfClass("Humanoid");
    local child_7 = arg_157.Character:FindFirstChild(var);
    if not var_557 or not child_7 then
        return true;
    end;
    if var_557.Health <= 0 then
        return true;
    end;
    if not child_7:IsA("BasePart") then
        return true;
    end;
    if not arg_157.Character.Parent then
        return true;
    end;
    if tbl_28.Settings.TeamCheck and arg_157.Team ~= nil and arg_157.Team == player.Team then
        return true;
    end;
    if (camera.CFrame.Position - child_7.Position).Magnitude < tbl_28.Settings.max_distance then
        return true;
    end;
    return false;
end;
local func_55 = function(arg_158, arg_159)
    local child_8 = arg_158:FindFirstChild(var);
    if not child_8 or not child_8:IsA("BasePart") then
        return nil;
    end;
    local var_558 = arg_158:FindFirstChildOfClass("Humanoid");
    if not var_558 or var_558.Health <= 0 then
        return nil;
    end;
    if not arg_158.Parent then
        return nil;
    end;
    local var_559 = arg_158:GetExtentsSize();
    if var_559.X > 100 or var_559.Y > 100 or var_559.Z > 100 then
        return nil;
    end;
    if var_559.X < 0.1 or var_559.Y < 0.1 or var_559.Z < 0.1 then
        return nil;
    end;
    local var_560 = var_559.X * 0.5;
    local var_561 = var_559.Y * 0.5;
    local var_562 = var_559.Z * 0.5;
    local var_563 = child_8.CFrame;
    local tbl_33 = { var_563 * Vector3.new(-var_560, -var_561, -var_562), var_563 * Vector3.new(-var_560, -var_561, var_562), var_563 * Vector3.new(-var_560, var_561, -var_562), var_563 * Vector3.new(-var_560, var_561, var_562), var_563 * Vector3.new(var_560, -var_561, -var_562), var_563 * Vector3.new(var_560, -var_561, var_562), var_563 * Vector3.new(var_560, var_561, -var_562), var_563 * Vector3.new(var_560, var_561, var_562) };
    local tbl_34 = { { 1, 2 }, { 1, 3 }, { 1, 5 }, { 2, 4 }, { 2, 6 }, { 3, 4 }, { 3, 7 }, { 4, 8 }, { 5, 6 }, { 5, 7 }, { 6, 8 }, { 7, 8 } };
    local var_564 = camera.ViewportSize;
    local num_32 = math.huge;
    local num_33 = math.huge;
    local var_565 = -math.huge;
    local var_566 = -math.huge;
    local num_34 = 0;
    local num_35 = 0.03;
    local func_53 = function(arg_160)
        return -camera.CFrame:PointToObjectSpace(arg_160).Z;
    end;
    local func_54 = function(arg_161)
        local var_567 = camera:WorldToViewportPoint(arg_161);
        if var_567.Z <= 0 then
            return ;
        end;
        if var_567.X ~= var_567.X or var_567.Y ~= var_567.Y then
            return ;
        end;
        num_32 = math.min(num_32, var_567.X);
        num_33 = math.min(num_33, var_567.Y);
        var_565 = math.max(var_565, var_567.X);
        var_566 = math.max(var_566, var_567.Y);
        num_34 = num_34 + 1;
        return ;
    end;
    for i_4 = 1, 8, 1 do
        if num_35 < func_53(tbl_33[i_4]) then
            func_54(tbl_33[i_4]);
        end;
    end;
    for key_59, value_61 in ipairs(tbl_34) do
        local var_568 = tbl_33[value_61[1]];
        local var_569 = tbl_33[value_61[2]];
        local var_570 = func_53(var_568);
        local var_571 = func_53(var_569);
        if var_570 > num_35 ~= (var_571 > num_35) then
            local var_572 = (num_35 - var_570) / (var_571 - var_570);
            local num_36 = math.clamp(var_572, 0, 1);
            func_54(var_568 + (var_569 - var_568) * num_36);
        end;
    end;
    if num_34 == 0 then
        if arg_159 and arg_159.LastRect and time() - (arg_159.LastSeen or 0) < 0.15 then
            return table.unpack(arg_159.LastRect);
        end;
        return nil;
    end;
    num_32 = math.clamp(num_32, 0, var_564.X);
    var_565 = math.clamp(var_565, 0, var_564.X);
    num_33 = math.clamp(num_33, 0, var_564.Y);
    var_566 = math.clamp(var_566, 0, var_564.Y);
    local num_37 = math.max(2, var_565 - num_32);
    local num_38 = math.max(2, var_566 - num_33);
    if num_37 < tbl_28.Settings.MinBoxSize or num_38 < tbl_28.Settings.MinBoxSize then
        if arg_159 and arg_159.LastRect and time() - (arg_159.LastSeen or 0) < 0.15 then
            return table.unpack(arg_159.LastRect);
        end;
        return nil;
    end;
    local var_573 = func_51(num_32);
    local var_574 = func_51(num_33);
    local var_575 = func_51(num_37);
    local var_576 = func_51(num_38);
    if arg_159 then
        arg_159.LastRect = { var_573, var_574, var_575, var_576 };
        arg_159.LastSeen = time();
    end;
    return var_573, var_574, var_575, var_576;
end;
local func_56 = function()
    if flag_33 and flag_33.Parent then
        return flag_33;
    end;
    flag_33 = Instance.new("ScreenGui");
    flag_33.Name = "PlayerESP";
    flag_33.IgnoreGuiInset = true;
    flag_33.ResetOnSpawn = false;
    flag_33.ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
    flag_33.Parent = player:WaitForChild("PlayerGui");
    return flag_33;
end;
local func_57 = function()
    local var_577 = Instance.new(frame_class);
    var_577.Name = "Holder";
    var_577.BackgroundTransparency = 1;
    var_577.BorderSizePixel = 0;
    var_577.Visible = false;
    var_577.ZIndex = 1;
    var_577.Parent = func_56();
    local var_578 = Instance.new(frame_class);
    var_578.Name = "Container";
    var_578.BackgroundTransparency = 1;
    var_578.BorderSizePixel = 0;
    var_578.Size = UDim2.fromScale(1, 1);
    var_578.ZIndex = 1;
    var_578.Parent = var_577;
    local var_579 = Instance.new(frame_class);
    var_579.Name = "Box";
    var_579.BackgroundTransparency = 1;
    var_579.BorderSizePixel = 0;
    var_579.ZIndex = 2;
    var_579.Parent = var_578;
    local var_580 = Instance.new(frame_class);
    var_580.Name = "BG";
    var_580.BackgroundTransparency = tbl_28.Settings.BoxFillTransparency;
    var_580.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
    var_580.BorderSizePixel = 0;
    var_580.Size = UDim2.fromScale(1, 1);
    var_580.ZIndex = 1;
    var_580.Parent = var_579;
    local var_581 = Instance.new("UIGradient");
    var_581.Rotation = tbl_28.Settings.BoxGradientRotation;
    var_581.Color = tbl_28.Settings.BoxGradient;
    var_581.Enabled = tbl_28.Settings.BoxGradientEnabled;
    var_581.Parent = var_580;
    local var_582 = Instance.new("UIStroke");
    var_582.Thickness = tbl_28.Settings.Thickness;
    var_582.Color = tbl_28.Settings.BoxColor;
    var_582.ApplyStrokeMode = Enum.ApplyStrokeMode.Border;
    var_582.Parent = var_579;
    local var_583 = Instance.new("UIGradient");
    var_583.Color = tbl_28.Settings.BoxStrokeGradient;
    var_583.Rotation = 90;
    var_583.Parent = var_582;
    local var_584 = Instance.new(frame_class);
    var_584.Name = "Health";
    var_584.BackgroundColor3 = Color3.fromRGB(12, 26, 58);
    var_584.BackgroundTransparency = 0;
    var_584.BorderSizePixel = 0;
    var_584.ClipsDescendants = true;
    var_584.ZIndex = 3;
    var_584.Parent = var_578;
    local var_585 = Instance.new("UIStroke");
    var_585.Thickness = tbl_28.Settings.HealthbarOutlineThickness;
    var_585.Color = Color3.fromRGB(0, 0, 0);
    var_585.ApplyStrokeMode = Enum.ApplyStrokeMode.Border;
    var_585.Parent = var_584;
    local var_586 = Instance.new("UIGradient");
    var_586.Color = tbl_28.Settings.HealthStrokeGradient;
    var_586.Rotation = 90;
    var_586.Parent = var_585;
    local var_587 = Instance.new(frame_class);
    var_587.Name = "Fill";
    var_587.BorderSizePixel = 0;
    var_587.BackgroundColor3 = Color3.fromRGB(255, 255, 255);
    var_587.AnchorPoint = Vector2.new(0, 1);
    var_587.Position = UDim2.new(0, 0, 1, 0);
    var_587.Size = UDim2.new(1, 0, 0, 0);
    var_587.ZIndex = 4;
    var_587.Parent = var_584;
    local var_588 = Instance.new("UIGradient");
    var_588.Rotation = 90;
    var_588.Color = tbl_28.Settings.HealthFillGradient;
    var_588.Enabled = true;
    var_588.Parent = var_587;
    local var_589 = Instance.new("TextLabel");
    var_589.Name = "Name";
    var_589.BackgroundTransparency = 1;
    var_589.BorderSizePixel = 0;
    var_589.TextColor3 = tbl_28.Settings.TextColor;
    var_589.TextStrokeColor3 = Color3.fromRGB(0, 0, 0);
    var_589.TextStrokeTransparency = 0.2;
    var_589.Font = Enum.Font.GothamSemibold;
    var_589.TextSize = 14;
    var_589.AnchorPoint = Vector2.new(0.5, 1);
    var_589.ZIndex = 5;
    var_589.Parent = var_578;
    local var_590 = Instance.new("UIGradient");
    var_590.Rotation = tbl_28.Settings.NameGradientRotation;
    var_590.Color = tbl_28.Settings.NameGradient;
    var_590.Enabled = tbl_28.Settings.NameGradientEnabled;
    var_590.Parent = var_589;
    return { Holder = var_577, Container = var_578, Box = var_579, BoxBG = var_580, BoxGrad = var_581, Stroke = var_582, BoxStrokeGrad = var_583, HB = var_584, HBS = var_585, HBSGrad = var_586, HBF = var_587, HBG = var_588, Name = var_589, NameGrad = var_590, LastRect = nil, LastSeen = 0 };
end;
local func_58 = function(arg_162)
    if arg_162 then
        if arg_162.Connections then
            for key_60, value_62 in pairs(arg_162.Connections) do
                if value_62 and value_62.Disconnect then
                    value_62:Disconnect();
                end;
            end;
        end;
        for key_61, value_63 in pairs(arg_162) do
            if typeof(value_63) == "Instance" and value_63.Destroy then
                value_63:Destroy();
            end;
        end;
    end;
    return ;
end;
local func_59 = function(arg_163, arg_164, arg_165)
    local var_591 = tbl_28.Settings;
    local var_592 = var_591.BoxPadding;
    local var_593 = var_591.Names and var_591.NameHeight or 0;
    local var_594 = arg_164 + var_592 * 2;
    local var_595 = arg_165 + var_592 * 2;
    local var_596 = var_591.Healthbar and var_591.HealthbarWidth or 0;
    local var_597 = var_591.Healthbar and var_591.HealthbarOutside and var_591.HealthbarOffset or 0;
    local num_39 = 0;
    local num_40 = 0;
    if var_591.Healthbar and var_591.HealthbarOutside then
        if var_591.HealthbarSide == "Left" then
            num_39 = var_596 + var_597;
        else
            num_40 = var_596 + var_597;
        end;
    end;
    local var_598 = num_39 + var_594 + num_40;
    local var_599 = var_593 + var_595;
    arg_163.Box.Position = UDim2.fromOffset(num_39, var_593);
    arg_163.Box.Size = UDim2.fromOffset(var_594, var_595);
    arg_163.BoxBG.BackgroundTransparency = var_591.BoxFillTransparency;
    arg_163.BoxBG.Visible = var_591.Boxes;
    arg_163.BoxGrad.Color = var_591.BoxGradient;
    arg_163.BoxGrad.Rotation = var_591.BoxGradientRotation;
    arg_163.BoxGrad.Enabled = var_591.BoxGradientEnabled;
    arg_163.Stroke.Thickness = var_591.Thickness;
    arg_163.Stroke.Color = var_591.BoxColor;
    arg_163.Stroke.Transparency = var_591.Boxes and 0 or 1;
    arg_163.BoxStrokeGrad.Color = var_591.BoxStrokeGradient;
    arg_163.HB.Visible = var_591.Healthbar;
    if var_591.Healthbar then
        if var_591.HealthbarOutside then
            local var_600 = if var_591.HealthbarSide ~= "Left" then num_39 + var_594 + (var_597 > 0 and var_597 or 0) - var_596 else 0;
            arg_163.HB.Position = UDim2.fromOffset(var_600, var_593);
            arg_163.HB.Size = UDim2.fromOffset(var_596, var_595);
        else
            local var_601 = num_39 + (var_591.HealthbarSide == "Left" and var_592 or var_594 - var_592 - var_596);
            arg_163.HB.Position = UDim2.fromOffset(var_601, var_593 + var_592);
            arg_163.HB.Size = UDim2.fromOffset(var_596, var_595 - var_592 * 2);
        end;
        arg_163.HBS.Thickness = var_591.HealthbarOutlineThickness;
        arg_163.HBS.Transparency = 0;
        arg_163.HBSGrad.Color = var_591.HealthStrokeGradient;
    end;
    arg_163.Name.Visible = var_591.Names;
    if var_591.Names then
        arg_163.Name.Position = UDim2.fromOffset(num_39 + math.floor(var_594 * 0.5), var_593 - 2);
        arg_163.Name.Size = UDim2.fromOffset(math.max(70, arg_164), var_591.NameHeight);
    end;
    return var_598, var_599, num_39, var_593;
end;
local func_60 = function(arg_166, arg_167)
    if not arg_167 then
        return ;
    end;
    if not tbl_28.Settings.Enabled or arg_166 == player or func_52(arg_166) then
        if arg_167.Holder then
            arg_167.Holder.Visible = false;
        end;
        return ;
    end;
    local var_602, var_603, var_604, var_605 = func_55(arg_166.Character, arg_167);
    if not var_602 then
        if arg_167.Holder then
            arg_167.Holder.Visible = false;
        end;
        return ;
    end;
    local var_606, var_607, var_608, var_609 = func_59(arg_167, var_604, var_605);
    arg_167.Holder.Size = UDim2.fromOffset(var_606, var_607);
    arg_167.Holder.Position = UDim2.fromOffset(func_51(var_602 - var_608), func_51(var_603 - var_609));
    arg_167.Holder.Visible = true;
    if tbl_28.Settings.Healthbar then
        local var_610 = arg_166.Character:FindFirstChildOfClass("Humanoid");
        local var_611 = var_610 and var_610.Health or 0;
        local var_612 = var_610 and math.max(1, var_610.MaxHealth) or 1;
        local num_41 = math.clamp(var_611 / var_612, 0, 1);
        arg_167.HBF.Size = UDim2.new(1, 0, num_41, 0);
        arg_167.HBF.Position = UDim2.new(0, 0, 1, 0);
        arg_167.HBG.Color = tbl_28.Settings.HealthFillGradient;
    end;
    if tbl_28.Settings.Names then
        local child_9 = arg_166.Character:FindFirstChild(var);
        local var_613 = tbl_28.Settings.UseDisplayName and arg_166.DisplayName or arg_166.Name;
        local var_614 = child_9 and (camera.CFrame.Position - child_9.Position).Magnitude or 0;
        local num_42 = math.floor(var_614 + 0.5);
        local var_615 = var_613 .. " [" .. tostring(num_42) .. "]";
        if tbl_28.Settings.DevilFruit then
            var_615 = var_613 .. " | Fruit : " .. (func_34(arg_166) or "None") .. " [" .. tostring(num_42) .. "]";
        end;
        arg_167.Name.TextColor3 = tbl_28.Settings.TextColor;
        arg_167.Name.Text = var_615;
        arg_167.NameGrad.Color = tbl_28.Settings.NameGradient;
        arg_167.NameGrad.Rotation = tbl_28.Settings.NameGradientRotation;
        arg_167.NameGrad.Enabled = tbl_28.Settings.NameGradientEnabled;
    end;
    return ;
end;
local func_61 = function()
    for key_62, value_64 in pairs(tbl_30) do
        func_60(key_62, value_64);
    end;
    return ;
end;
local func_62 = function(arg_168)
    if arg_168 == player then
        return ;
    end;
    if tbl_30[arg_168] then
        return ;
    end;
    local var_616 = func_57();
    tbl_30[arg_168] = var_616;
    var_616.Connections = {};
    var_616.Connections.CharacterAdded = arg_168.CharacterAdded:Connect(function()
        return ;
    end);
    return ;
end;
local func_63 = function(arg_169)
    local var_617 = tbl_30[arg_169];
    if var_617 then
        func_58(var_617);
        tbl_30[arg_169] = nil;
    end;
    return ;
end;
tbl_28.Enable = function()
    if tbl_28.Settings.Enabled then
        return ;
    end;
    tbl_28.Settings.Enabled = true;
    func_56();
    for key_63, value_65 in ipairs(Players:GetPlayers()) do
        if value_65 ~= player then
            func_62(value_65);
        end;
    end;
    tbl_29.PlayerAdded = Players.PlayerAdded:Connect(function(arg_170)
        func_62(arg_170);
        return ;
    end);
    tbl_29.PlayerRemoving = Players.PlayerRemoving:Connect(function(arg_171)
        func_63(arg_171);
        return ;
    end);
    tbl_29.Stepped = RunService.RenderStepped:Connect(function()
        if workspace_service.CurrentCamera ~= camera then
            camera = workspace_service.CurrentCamera;
        end;
        func_61();
        return ;
    end);
    return ;
end;
tbl_28.Disable = function()
    if not tbl_28.Settings.Enabled then
        return ;
    end;
    tbl_28.Settings.Enabled = false;
    for key_64, value_66 in pairs(tbl_29) do
        if value_66 and value_66.Disconnect then
            value_66:Disconnect();
        end;
    end;
    tbl_29 = {};
    for key_65, value_67 in pairs(tbl_30) do
        if value_67 and value_67.Holder then
            value_67.Holder.Visible = false;
        end;
    end;
    return ;
end;
tbl_28.Configure = function(arg_172)
    for key_66, value_68 in pairs(arg_172 or {}) do
        if tbl_28.Settings[key_66] ~= nil then
            tbl_28.Settings[key_66] = value_68;
        end;
    end;
    return ;
end;
local var_618 = player.Character or player.CharacterAdded:Wait();
local child_10 = var_618:WaitForChild("Humanoid");
local child_11 = var_618:WaitForChild(var);
local tbl_35 = { "geppo", "rolling" };
player.CharacterAdded:Connect(function(arg_173)
    var_618 = arg_173;
    child_10 = arg_173:WaitForChild("Humanoid");
    child_11 = arg_173:WaitForChild(var);
    return ;
end);
var_618.DescendantAdded:Connect(function(arg_174)
    if not getgenv().anti_stun then
        return ;
    end;
    task.wait();
    if arg_174.Name == "Stun" or arg_174.Name == "StunFolder" then
        arg_174:Destroy();
    end;
    if arg_174:IsA("BodyPosition") or arg_174:IsA("BodyVelocity") and not table.find(tbl_35, arg_174.Name) then
        arg_174:Destroy();
    end;
    return ;
end);
player.CharacterAdded:Connect(function(arg_175)
    repeat
        task.wait();
    until player.Character:FindFirstChild("Humanoid");
    var_618 = arg_175;
    var_618.DescendantAdded:Connect(function(arg_176)
        if not getgenv().anti_stun then
            return ;
        end;
        task.wait();
        if arg_176.Name == "Stun" or arg_176.Name == "StunFolder" then
            arg_176:Destroy();
        end;
        if arg_176:IsA("BodyPosition") or arg_176:IsA("BodyVelocity") and not table.find(tbl_35, arg_176.Name) then
            arg_176:Destroy();
        end;
        return ;
    end);
    return ;
end);
RunService.Heartbeat:Connect(function(arg_177)
    if not flag_34 then
        return ;
    end;
    if not child_10 or not child_11 then
        return ;
    end;
    local var_619 = child_10.MoveDirection;
    if var_619.Magnitude > 0 then
        local var_620 = child_11.AssemblyLinearVelocity;
        local var_621 = var_619 * (child_10.WalkSpeed + num_25);
        child_11.AssemblyLinearVelocity = Vector3.new(var_621.X, var_620.Y, var_621.Z);
    end;
    return ;
end);
reset_teleport_aura = function(arg_178)
    warn("teleporting");
    if player.Character:FindFirstChildWhichIsA("ForceField", true) then
        return ;
    end;
    warn("trying");
    player.Character.HumanoidRootPart.CFrame = CFrame.new(arg_178);
    warn("tped once");
    local num_43 = math.random(4, 8);
    for i_5 = 1, 10, 1 do
        if i_5 == num_43 then
            knocked_out_event:FireServer("self");
        end;
        player.Character.HumanoidRootPart.CFrame = CFrame.new(arg_178);
        task.wait(0.05);
    end;
    warn("we did ts 10 times");
    warn("done");
    local var_622 = player.Character.Humanoid;
    return ;
end;
CustomTween = function(arg_179, arg_180, arg_181)
    if player.Character:GetAttribute("SpeedBypass") < 1 then
        return ;
    end;
    local var_623 = player.Character;
    if not var_623 then
        return ;
    end;
    local child_12 = var_623:FindFirstChild(var);
    if not child_12 then
        return ;
    end;
    local var_624 = child_12.CFrame;
    local var_625 = arg_179 + Vector3.new(0, 3, 0);
    local var_626 = child_12.Position;
    local vector_5 = Vector3.new(8812, 66, 11539);
    local var_627 = (child_12.Position - vector_5).Magnitude;
    if arg_180 and var_627 <= 100 then
        arg_180 = nil;
    end;
    if arg_180 then
        child_12.CFrame = CFrame.new(child_12.Position.X, arg_180, child_12.Position.Z) * (var_624 - var_624.Position);
        var_626 = child_12.Position;
    end;
    local var_628 = (Vector2.new(var_626.X, var_626.Z) - Vector2.new(var_625.X, var_625.Z)).Magnitude;
    if var_628 <= 8 then
        local var_629 = not arg_181 and var_625.Y or arg_180 or var_626.Y;
        child_12.CFrame = CFrame.new(var_625.X, var_629, var_625.Z) * (var_624 - var_624.Position);
        child_12.Velocity = Vector3.zero;
        return ;
    end;
    local var_630 = arg_180 or var_626.Y;
    local var_631 = var_628 / 40;
    local num_44 = 2;
    if not arg_181 then
        if var_628 <= 10 then
            num_44 = 0;
        else
            num_44 = 1 - 10 / var_628;
        end;
    end;
    local num_45 = 0;
    while num_45 < 1 do
        if not player.Character or not player.Character:FindFirstChild(var) then
            return ;
        end;
        local var_632 = num_45 + RunService.Heartbeat:Wait() / var_631;
        num_45 = math.clamp(var_632, 0, 1);
        local var_633 = var_626.X + num_45 * (var_625.X - var_626.X);
        local var_634 = var_626.Z + num_45 * (var_625.Z - var_626.Z);
        local var_635 = if not (num_45 < num_44) then var_630 + (num_44 < 1 and (num_45 - num_44) / (1 - num_44) or 0) * (var_625.Y - var_630) else var_630;
        local vector_6 = Vector3.new(var_633, var_635, var_634);
        child_12.Velocity = Vector3.zero;
        child_12.CFrame = CFrame.new(vector_6) * (var_624 - var_624.Position);
    end;
    if player.Character and player.Character:FindFirstChild(var) then
        local var_636 = num_44 <= 1 and var_625.Y or var_630;
        child_12.CFrame = CFrame.new(Vector3.new(var_625.X, var_636, var_625.Z)) * (var_624 - var_624.Position);
        child_12.Velocity = Vector3.zero;
    end;
    return ;
end;
get_closest_mob = function(arg_182)
    if not player.Character or not player.Character:FindFirstChild(var) then
        return { agroed = {}, non_agroed = {} };
    end;
    local var_637 = player.Character.HumanoidRootPart;
    local tbl_36 = {};
    local tbl_37 = {};
    for var_638, var_639 in next, workspace_service.NPCs:GetChildren() do
        if var_639.Name == arg_182 and var_639:FindFirstChild("Head") then
            local var_640 = (var_639:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
            local child_13 = var_639:FindFirstChild("Info");
            local flag_38 = false;
            if child_13 then
                local child_14 = child_13:FindFirstChild("Target");
                if child_14 and child_14:IsA("ObjectValue") and child_14.Value == var_637 then
                    flag_38 = true;
                end;
            end;
            local tbl_38 = { Mob = var_639, target_distance = var_640 };
            if flag_38 then
                table.insert(tbl_36, tbl_38);
            else
                table.insert(tbl_37, tbl_38);
            end;
        end;
    end;
    table.sort(tbl_36, function(arg_183, arg_184)
        return arg_183.Distance < arg_184.Distance;
    end);
    table.sort(tbl_37, function(arg_185, arg_186)
        return arg_185.Distance < arg_186.Distance;
    end);
    return { agroed = tbl_36, non_agroed = tbl_37 };
end;
island = function(arg_187, arg_188)
    if stats_folder.Stats.SpawnPoint.Value == arg_187 then
        return true;
    end;
    if stats_folder.Stats.SpawnPoint.Value ~= arg_187 then
        repeat
            task.wait();
            reset_teleport_aura(arg_188);
            repeat
                task.wait();
            until player.Character.Humanoid.Health / player.Character.Humanoid.MaxHealth > 0.3;
            quest_event:InvokeServer({ "npcChat", true });
            if workspace_service.NPCs:FindFirstChild("Robo") then
                set_spawn_event:FireServer(nil, workspace_service.NPCs.Robo);
            end;
        until stats_folder.Stats.SpawnPoint.Value == arg_187 or not getgenv().farm;
    end;
    return ;
end;
IsPlayingAnimation = function(arg_189, arg_190)
    local var_641 = arg_190:lower();
    for key_67, value_69 in ipairs(arg_189:GetPlayingAnimationTracks()) do
        local var_642 = value_69.Animation.AnimationId:match("%d+");
        if var_642 and marketplace_service:GetProductInfo(tonumber(var_642)).Name:lower():match(var_641) then
            return true;
        end;
    end;
    return false;
end;
buy = function(arg_191, arg_192)
    local var_643 = arg_192 or 1;
    if typeof(arg_191) == "string" then
        arg_191 = workspace.BuyableItems:FindFirstChild(arg_191);
    end;
    shop_event:InvokeServer(arg_191, var_643);
    return ;
end;
GetChest = function()
    local flag_39 = nil;
    local num_46 = 300;
    for key_68, value_70 in ipairs(workspace_service.Env:GetChildren()) do
        if value_70.Name == "Chest" and value_70.PrimaryPart and value_70:GetPivot().Position.Y < 50 then
            local var_644 = (value_70:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
            if var_644 < num_46 then
                flag_39 = value_70;
                num_46 = var_644;
            end;
        end;
    end;
    return flag_39;
end;
get_prompt = function(arg_193)
    for var_645, var_646 in next, workspace_service.Env:GetChildren() do
        if var_646.Name == "Part" and var_646:IsA("BasePart") and (var_646.Position - arg_193).Magnitude < 10 then
            return var_646.ProximityPrompt;
        end;
    end;
    return ;
end;
get_current_island = function()
    local flag_40 = nil;
    local num_47 = math.huge;
    for var_647, var_648 in next, workspace.Islands:GetChildren() do
        local var_649 = (var_648:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
        if var_649 < num_47 then
            flag_40 = var_648;
            num_47 = var_649;
        end;
    end;
    return flag_40;
end;
Loop = nil;
Fish = function()
    local var_650 = player.Character;
    if not var_650 then
        return ;
    end;
    local child_15 = ReplicatedStorage:WaitForChild("Fishing"):WaitForChild("Remotes"):WaitForChild("Action");
    local child_16 = player.Backpack:FindFirstChild("Fishing Rod") or var_650:FindFirstChild("Fishing Rod");
    if not child_16 then
        return ;
    end;
    if not var_650:FindFirstChild("Fishing Rod") then
        local child_17 = player.Backpack:FindFirstChild("Fishing Rod");
        if child_17 then
            child_17.Parent = var_650;
        end;
        task.wait(0.2);
    end;
    local var_651 = get_current_island();
    if not var_651 then
        return ;
    end;
    if not var_651:FindFirstChild("Dock", true) then
        return ;
    end;
    local child_18 = workspace_service.Effects:FindFirstChild(player.Name .. "'s hook");
    if not child_18 then
        if not pcall(function()
            child_15:InvokeServer({ Action = "Throw", Goal = Vector3.new(-1269.864501953125, -99.21800994873047, -4819.37646484375), Bait = "Common Fish Bait" });
            task.wait(0.3);
            child_15:InvokeServer({ Action = "Landed" });
            return ;
        end) then
            return ;
        end;
        local var_652 = tick() + 5;
        repeat
            task.wait(0.1);
            child_18 = workspace_service.Effects:FindFirstChild(player.Name .. "'s hook");
        until child_18 or var_652 < tick() or not getgenv().farm;
        if not child_18 then
            return ;
        end;
    end;
    local flag_41 = true;
    task.spawn(function()
        while flag_41 and child_18 and child_18.Parent and getgenv().farm do
            pcall(function()
                if var_650 and var_650:FindFirstChild(var) then
                    child_18.CFrame = CFrame.new(var_650.HumanoidRootPart.Position);
                end;
                return ;
            end);
            task.wait();
        end;
        return ;
    end);
    task.spawn(function()
        local child_19 = ReplicatedStorage:WaitForChild("FishingShopRemote");
        local var_653 = require(ReplicatedStorage.Modules.ToolDesc.ToolDescUtils).GetAllFishes();
        for key_69, value_71 in pairs(var_653) do
            pcall(function()
                child_19:InvokeServer({ Method = "SellFish", Fish = key_69, All = true });
                return ;
            end);
            task.wait(0.05);
        end;
        return ;
    end);
    local flag_42 = false;
    local var_654 = tick();
    while child_18 and child_18.Parent and getgenv().farm and tick() - var_654 < 30 do
        task.wait(0.1);
        local flag_43 = false;
        for key_70, value_72 in ipairs(workspace_service.Effects:GetChildren()) do
            if value_72:IsA("Model") and value_72 ~= child_18 and (value_72:GetPivot().Position - child_18:GetPivot().Position).Magnitude < 10 then
                flag_43 = true;
                break;
            end;
        end;
        if flag_43 then
            flag_41 = false;
            child_16.Parent = player.Backpack;
            task.wait(0.5);
            if getgenv().farm and var_650 then
                child_16.Parent = var_650;
            end;
            break;
        end;
        if not flag_42 then
            local flag_44 = false;
            for key_71, value_73 in ipairs(workspace_service.Effects:GetChildren()) do
                if value_73.Name == "Splash" then
                    local vector_7 = Vector3.new(child_18:GetPivot().Position.X, 0, child_18:GetPivot().Position.Z);
                    if (Vector3.new(value_73.Position.X, 0, value_73.Position.Z) - vector_7).Magnitude < 10 then
                        flag_44 = true;
                        break;
                    end;
                end;
            end;
            if flag_44 then
                flag_42 = true;
                task.spawn(function()
                    task.wait(math.random(60, 70) / 10);
                    pcall(function()
                        child_15:InvokeServer({ Action = "Reel" });
                        return ;
                    end);
                    task.wait(1);
                    flag_42 = false;
                    return ;
                end);
            end;
        end;
    end;
    flag_41 = false;
    return ;
end;
fake_geppo = function()
    if stats_folder.Skills:FindFirstChild("skyWalk").Value == true then
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer("Sky Walk2", { char = game.Players.LocalPlayer.Character, cf = CFrame.new(-3065.980712890625, 200.7225799560547, -11824.01171875, 0, 1, -0.0, 0, -0.0, -1, -1, 0, -0.0) });
    else
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer("Geppo", { char = game.Players.LocalPlayer.Character, cf = CFrame.new(-3065.980712890625, 200.7225799560547, -11824.01171875, 0, 1, -0.0, 0, -0.0, -1, -1, 0, -0.0) });
    end;
    return ;
end;
has_geppo = function()
    return stats_folder.Skills:FindFirstChild("skyWalk").Value == true or stats_folder.Skills:FindFirstChild("Rokushiki") and stats_folder.Skills:FindFirstChild("Rokushiki").Value > 0;
end;
local func_64 = function(arg_194)
    quest_event:InvokeServer({ "quit" });
    task.wait(1.5);
    quest_event:InvokeServer({ "takequest", arg_194 });
    return ;
end;
local func_65 = function(arg_195)
    local tween_14 = tween_service:Create(var_618.HumanoidRootPart, TweenInfo.new((var_618.HumanoidRootPart.Position - arg_195).Magnitude / 30, Enum.EasingStyle.Linear), { CFrame = CFrame.new(arg_195) });
    tween_14:Play();
    tween_14.Completed:Wait();
    return ;
end;
local func_66 = function()
    local flag_45 = nil;
    local num_48 = 50;
    for key_72, value_74 in pairs(Players:GetPlayers()) do
        if value_74 ~= player and value_74.Character and value_74.Character:FindFirstChild(var) and player.Character and player.Character:FindFirstChild(var) then
            local var_655 = (player.Character.HumanoidRootPart.Position - value_74.Character.HumanoidRootPart.Position).Magnitude;
            if var_655 < num_48 then
                flag_45 = value_74;
                num_48 = var_655;
            end;
        end;
    end;
    return flag_45;
end;
main_tab = library.create_main({ title = "Grand Piece Online", desc = "" });
page_1 = main_tab.create_page({ page_name = "Main", page_title = "Main Tab" });
page_2 = main_tab.create_page({ page_name = "Esp", page_title = "ESP Tab" });
page_4 = main_tab.create_page({ page_name = "Combat", page_title = "Combat Tab" });
page_3 = main_tab.create_page({ page_name = "Farm", page_title = "Farm Tab" });
page_5 = main_tab.create_page({ page_name = "Misc", page_title = "Misc Tab" });
config_tab = main_tab.create_page({ page_name = "Config", page_title = "Config Tab" });
config_section = config_tab.create_section("Config");
othersection = config_tab.create_section("Other");
try_2 = config_tab.create_section("Auto Rejoin");
chest_esp = page_2.create_section("Chest Esp", true);
medal_esp = page_2.create_section("Medal Esp", true);
player_esp = page_2.create_section("Player Esp");
ps_section = page_1.create_section("Private Server");
speed_section = page_1.create_section("Speed");
level_section = page_3.create_section("Level Farm");
halloween_section = page_3.create_section("Halloween Farm");
juzo_section = page_3.create_section("Juzo");
santa_section = page_3.create_section("Santa");
impel_section = page_3.create_section("Impel");
ship_section = page_3.create_section("Ship Farm");
baal_section = page_3.create_section("Baal Farm");
pica_section = page_3.create_section("Pica Farm");
gkk_section = page_3.create_section("Gkk Stack");
stats_section = page_3.create_section("Auto Stats");
player_section = page_4.create_section("Player Combat");
movement_section = page_5.create_section("Movement");
qol = page_5.create_section("Quality Of Life");
island_tp = page_5.create_section("Island");
market_section = page_5.create_section("Market");
exploit_section = page_5.create_section("Local Player");
global_settings = page_5.create_section("Global Settings");
task.spawn(function()
    while true do
        task.wait(1);
        task.spawn(pcall, function()
            requirements_tracker:update("Geppo", stats_folder.Skills.sky_walk.Value);
            return ;
        end);
    end;
end);
chest_esp.create_toggle({ title = "Chest Esp", desc = "Enable or disable Chest Esp", default = false }, function(arg_196)
    getgenv().chest_esp_enabled = arg_196;
    if arg_196 then
        tbl_25:start();
    else
        tbl_25:stop();
    end;
    return ;
end);
chest_esp.create_slider({ title = "Max Distance", min = 0, max = 10000, default = 2500, precise = true }, function(arg_197)
    config.max_distance = arg_197;
    config.maxDistance = arg_197;
    return ;
end);
chest_esp.create_dropdown({ title = "Enabled Chests", List = { "Common", "Uncommon", "Rare", "Legendary", "Mythic" }, default = { "Common", "Uncommon", "Rare", "Legendary", "Mythic" }, search = false, selected = true }, function(arg_198, arg_199)
    selected_chests[arg_198] = arg_199;
    tbl_25:apply_filter();
    return ;
end);
medal_esp.create_toggle({ title = "Medal Esp", desc = "Enable or disable Medal Esp", default = false }, function(arg_200)
    getgenv().medal_esp_enabled = arg_200;
    if arg_200 then
        tbl_26:start();
    else
        tbl_26:stop();
    end;
    return ;
end);
medal_esp.create_slider({ title = "Max Distance", min = 0, max = 10000, default = 2500, precise = true }, function(arg_201)
    config.max_distance = arg_201;
    config.maxDistance = arg_201;
    return ;
end);
player_esp.create_toggle({ title = "Enable Player Esp", desc = "Enables player visuals", default = false }, function(arg_202)
    if arg_202 then
        tbl_28.Enable();
    else
        tbl_28.Disable();
    end;
    return ;
end);
player_esp.create_toggle({ title = "Enable Name", desc = "Enables showing name for esp", default = true }, function(arg_203)
    tbl_28.Settings.Names = arg_203;
    return ;
end);
player_esp.create_toggle({ title = "Enable Box", desc = "Enables Boxes for esp", default = true }, function(arg_204)
    tbl_28.Settings.Boxes = arg_204;
    return ;
end);
player_esp.create_toggle({ title = "Enable Devil Fruit", desc = "Enables Devil Fruit for esp", default = true }, function(arg_205)
    tbl_28.Settings.DevilFruit = arg_205;
    return ;
end);
player_esp.create_toggle({ title = "Enable HealthBar", desc = "Enables Healthbar for esp", default = true }, function(arg_206)
    tbl_28.Settings.Healthbar = arg_206;
    return ;
end);
speed_section.create_toggle({ title = "Speed", desc = "Increases your velocity", default = false }, function(arg_207)
    flag_34 = arg_207;
    return ;
end);
speed_section.create_slider({ title = "Speed Boost", min = 0, max = 250, default = 50, precise = true }, function(arg_208)
    num_25 = arg_208;
    return ;
end);
movement_section.create_toggle({ title = "No Stun", desc = "Removes stun when attacked", default = false }, function(arg_209)
    getgenv().anti_stun = arg_209;
    return ;
end);
movement_section.create_toggle({ title = "Infinite Jump", desc = "Lets you jump forever", default = false }, function(arg_210)
    getgenv().infinite_jump = arg_210;
    getgenv().last_jump = getgenv().last_jump or 0;
    if arg_210 then
        if not getgenv().inf_jump_connection then
            getgenv().inf_jump_connection = user_input_service.JumpRequest:Connect(function()
                if getgenv().infinite_jump and tick() - getgenv().last_jump >= 0.1 then
                    getgenv().last_jump = tick();
                    local var_656 = player.Character;
                    local var_657 = var_656 and var_656:FindFirstChildOfClass("Humanoid");
                    local var_658 = var_656 and var_656:FindFirstChild(var);
                    if var_657 and var_658 then
                        if var_657.JumpPower == 0 then
                            var_658.CFrame = var_658.CFrame + Vector3.new(0, 7, 0);
                            var_658.Velocity = Vector3.zero;
                        else
                            var_658.AssemblyLinearVelocity = Vector3.new(var_658.AssemblyLinearVelocity.X, 50, var_658.AssemblyLinearVelocity.Z);
                        end;
                    end;
                end;
                return ;
            end);
        end;
    elseif getgenv().inf_jump_connection then
        getgenv().inf_jump_connection:Disconnect();
        getgenv().inf_jump_connection = nil;
    end;
    return ;
end);
movement_section.create_toggle({ title = "Geppo Loop", desc = "Helps bypass some speed checks", default = false }, function(arg_211)
    getgenv().gp = arg_211;
    while getgenv().gp and task.wait(1) do
        fake_geppo();
    end;
    return ;
end);
qol.create_toggle({ title = "Auto Enable Buso", desc = "Enables Buso For You When At 100%", default = false }, function(arg_212)
    getgenv().ab = arg_212;
    if not arg_212 then
        return ;
    end;
    if not stats_folder:FindFirstChild("BusoBar") then
        library.create_noti({ title = "Uh Oh!", desc = "You need buso haki to use this!", show_time = 4 });
    end;
    while getgenv().ab and task.wait(1) do
        if stats_folder.BusoBar.Value == stats_folder.BusoBar.MaxValue then
            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Haki"):FireServer("Buso");
        end;
    end;
    return ;
end);
Players = game:GetService("Players");
RunService = game:GetService("RunService");
ReplicatedStorage = game:GetService("ReplicatedStorage");
player = Players.LocalPlayer;
last_dash_trigger = 0;
tp_connection = nil;
running_flag = false;
current_target = nil;
local func_67 = function()
    var_618 = game.Players.LocalPlayer.Character;
    if not var_618 then
        return 160;
    end;
    local game_11 = game:GetService("Players").LocalPlayer;
    local game_12 = game:GetService("ReplicatedStorage"):FindFirstChild("Stats" .. game_11.Name);
    if not game_12 then
        return 160;
    end;
    local child_20 = game_12:FindFirstChild("Stats");
    if not child_20 then
        return 160;
    end;
    local child_21 = child_20:FindFirstChild("DF");
    local child_22 = child_20:FindFirstChild("FightingStyle");
    local child_23 = game_12:FindFirstChild("Customization");
    local var_659 = child_23 and child_23:FindFirstChild("Race");
    if not child_21 or not child_22 or not var_659 then
        return 160;
    end;
    local num_49 = 26;
    if child_21.Value == "Buddha-Buddha" then
        num_49 = 160;
    elseif child_22.Value == "Rokushiki" or child_22.Value == "Kamishiki" or var_618:GetAttribute("GEAR2Active") or _G.soru_dashing == true or game_11:GetAttribute("soruDash") or var_618:GetAttribute("evading") ~= nil then
        num_49 = 78;
    elseif var_618:GetAttribute("evading") == true then
        num_49 = 70;
    elseif child_21.Value == "Venom-Venom" then
        num_49 = 50;
    elseif string.sub(string.lower(var_659.Value), 1, 7) == "fishman" or _G.swimming then
        num_49 = 36;
    elseif child_21.Value == "Yomi-Yomi" then
        num_49 = 36;
    end;
    local var_660 = var_618:GetAttribute("previousEvasiveTick");
    if var_660 and tick() - var_660 <= 6 then
        num_49 = 78;
    end;
    local var_661 = num_49 * math.clamp(var_618:GetScale(), 1, 20);
    local var_662 = var_618:GetAttribute("SpeedBypass");
    if var_662 and var_662 > 0 then
        var_661 = var_661 + var_662;
    end;
    if var_661 < 46 then
        var_661 = 46;
    end;
    return var_661 * getgenv().tween_speed;
end;
loadstring("    function LPH_JIT(f) return f end;\n    function LPH_JIT_MAX(f) return f end;\n    function LPH_JIT_ULTRA(f) return f end;\n    function LPH_NO_VIRTUALIZE(f) return f end;\n    function LPH_NO_UPVALUES(f) return f end;\n    function LPH_CRASH() return end;\n")();
Players = game:GetService("Players");
RunService = game:GetService("RunService");
workspace_service = game:GetService("Workspace");
player = Players.LocalPlayer;
tween_connection = nil;
noclip_connection = nil;
current_target = nil;
float_height = 7;
ray_start_height = 9000;
smoothness = 0.3;
get_speed = interpreter_proxy[1][34][5](function()
    return func_67() > 0 and func_67() - 5 or 35;
end);
get_exclude_list = function(arg_213)
    local tbl_39 = { arg_213 };
    for key_73, value_75 in ipairs(Players:GetPlayers()) do
        if value_75.Character then
            table.insert(tbl_39, value_75.Character);
        end;
    end;
    return tbl_39;
end;
local func_68 = function(arg_214, arg_215, arg_216)
    if not player or (not player.Character or not player.Character:FindFirstChild(var)) then
        return false;
    end;
    local var_663 = player.Character.HumanoidRootPart.Position;
    if arg_216 then
        local var_664 = var_663.X - arg_214.X;
        local var_665 = var_663.Z - arg_214.Z;
        return math.sqrt(var_664 * var_664 + var_665 * var_665) <= arg_215;
    end;
    return (var_663 - arg_214).Magnitude <= arg_215;
end;
horo_tp_pro = function(arg_217)
    local flag_46 = nil;
    local flag_47 = nil;
    local var_666 = typeof(arg_217) == "table" and arg_217.Track;
    if typeof(arg_217) == "Vector3" then
        flag_46 = arg_217;
    elseif typeof(arg_217) == "table" then
        flag_46 = arg_217.Position or arg_217.Target;
        if typeof(arg_217.Target) == "Instance" then
            flag_47 = arg_217.Target;
        elseif typeof(arg_217.Position) == "Instance" then
            flag_47 = arg_217.Position;
        end;
    end;
    if not flag_46 then
        warn("HoroTpPro: No target defined");
        return ;
    end;
    local func_69 = function()
        if flag_47 and flag_47.Parent then
            if flag_47:IsA("Model") then
                return flag_47:GetPivot().Position;
            end;
            if flag_47:IsA("BasePart") then
                return flag_47.Position;
            end;
            if var_666 and flag_47:IsA("ValueBase") then
                return flag_47.Value;
            end;
        end;
        return flag_46;
    end;
    if current_target and tween_connection then
        local var_667 = func_69();
        if (Vector3.new(var_667.X, 0, var_667.Z) - Vector3.new(current_target.X, 0, current_target.Z)).Magnitude < 1 then
            return ;
        end;
    end;
    local var_668 = typeof(arg_217) == "table" and arg_217.Stop or function()
        return true;
    end;
    local var_669 = typeof(arg_217) == "table" and arg_217.Y or nil;
    if tween_connection then
        tween_connection:Disconnect();
    end;
    if noclip_connection then
        noclip_connection:Disconnect();
    end;
    current_target = func_69();
    local var_670 = player.Character;
    local var_671 = var_670 and var_670:FindFirstChild(var);
    if not var_670 or not var_671 then
        current_target = nil;
        return ;
    end;
    local var_672 = RaycastParams.new();
    var_672.FilterType = Enum.RaycastFilterType.Exclude;
    var_672.IgnoreWater = true;
    var_672.FilterDescendantsInstances = get_exclude_list(var_670);
    local var_673;
    if var_669 then
        var_673 = var_669;
    else
        local var_674 = workspace_service:Raycast(Vector3.new(current_target.X, ray_start_height, current_target.Z), Vector3.new(0, -20000, 0), var_672);
        var_673 = var_674 and var_674.Position.Y + float_height or current_target.Y + float_height;
    end;
    local num_50 = math.max(var_671.Position.Y, var_673, 5);
    var_671.CFrame = CFrame.new(var_671.Position.X, num_50, var_671.Position.Z);
    local var_675 = num_50;
    noclip_connection = RunService.Stepped:Connect(function()
        if not var_670 or not var_670.Parent then
            if noclip_connection then
                noclip_connection:Disconnect();
            end;
            return ;
        end;
        var_671.Velocity = Vector3.zero;
        var_671.AssemblyLinearVelocity = Vector3.zero;
        for key_74, value_76 in ipairs(var_670:GetDescendants()) do
            if value_76:IsA("BasePart") and value_76.CanCollide then
                value_76.CanCollide = false;
            end;
        end;
        return ;
    end);
    tween_connection = RunService.Heartbeat:Connect(function(arg_218)
        if not var_670 or (not var_671 or not var_671.Parent) then
            tween_connection:Disconnect();
            noclip_connection:Disconnect();
            current_target = nil;
            return ;
        end;
        if not var_668() then
            tween_connection:Disconnect();
            noclip_connection:Disconnect();
            current_target = nil;
            return ;
        end;
        local var_676 = func_69();
        current_target = var_676;
        var_672.FilterDescendantsInstances = get_exclude_list(var_670);
        local var_677 = var_671.Position;
        local vector_8 = Vector3.new(var_676.X, 0, var_676.Z) - Vector3.new(var_677.X, 0, var_677.Z);
        local var_678 = vector_8.Magnitude;
        if var_678 < 8 then
            local var_679 = var_669 or var_676.Y;
            if not var_669 then
                local var_680 = workspace_service:Raycast(Vector3.new(var_676.X, ray_start_height, var_676.Z), Vector3.new(0, -20000, 0), var_672);
                if var_680 then
                    var_679 = var_680.Position.Y + float_height;
                end;
            end;
            local num_51 = math.max(var_679, 5);
            var_671.CFrame = CFrame.new(var_676.X, num_51, var_676.Z);
            tween_connection:Disconnect();
            noclip_connection:Disconnect();
            current_target = nil;
            return ;
        end;
        local var_681 = vector_8.Unit;
        local var_682 = get_speed();
        local num_52 = math.min(var_682 * arg_218, var_678);
        local var_683 = var_677.X + var_681.X * num_52;
        local var_684 = var_677.Z + var_681.Z * num_52;
        local var_685;
        if var_669 then
            var_685 = var_669;
        else
            local vector_9 = Vector3.new(var_683, ray_start_height, var_684);
            local var_686 = workspace_service:Raycast(vector_9, Vector3.new(0, -20000, 0), var_672);
            var_685 = var_686 and var_686.Position.Y + float_height or var_676.Y + float_height;
        end;
        local num_53 = math.max(var_685, 5);
        var_675 = var_675 + (num_53 - var_675) * (smoothness or 0.25);
        var_675 = math.max(var_675, 5);
        var_671.CFrame = CFrame.new(var_683, var_675, var_684);
        return ;
    end);
    return ;
end;
local func_70 = function(arg_219)
    player.Character.HumanoidRootPart.CFrame = CFrame.new(arg_219);
    player.Character.HumanoidRootPart.Velocity = Vector3.zero;
    return ;
end;
is_tweening = false;
tp_tween = function(arg_220, arg_221)
    if is_tweening then
        return ;
    end;
    local var_687 = player.Character and player.Character:FindFirstChild(var);
    if not var_687 then
        return ;
    end;
    is_tweening = true;
    local var_688 = var_687.CFrame;
    local var_689 = typeof(arg_220) == "CFrame" and arg_220 or CFrame.new(arg_220);
    local var_690 = (var_689.Position - var_688.Position).Magnitude / 15;
    local num_54 = 0;
    while num_54 < var_690 do
        num_54 = num_54 + game:GetService("RunService").Heartbeat:Wait();
        var_687.CFrame = var_688:Lerp(var_689, (math.min(num_54 / var_690, 1)));
        var_687.Velocity = Vector3.zero;
        var_687.AssemblyLinearVelocity = Vector3.zero;
    end;
    is_tweening = false;
    return ;
end;
touch = function(arg_222)
    if player.Character and player.Character:FindFirstChild(var) then
        firetouchinterest(player.Character.HumanoidRootPart, arg_222, 0);
        firetouchinterest(player.Character.HumanoidRootPart, arg_222, 1);
    end;
    return ;
end;
local func_71 = function(arg_223)
    for var_691, var_692 in next, player.Backpack:GetChildren() do
        if var_692.Name == arg_223 then
            return var_692;
        end;
    end;
    for var_693, var_694 in next, player.Character:GetChildren() do
        if var_694.Name == arg_223 then
            return var_694;
        end;
    end;
    return nil;
end;
get_inventory_tool = function(arg_224)
    local var_695, var_696 = pcall(function()
        return http_service:JSONDecode(inventory_value.Value);
    end);
    if var_695 and var_696 then
        return var_696[arg_224] ~= nil;
    end;
    return false;
end;
local var_697 = getgenv();
local var_698 = getgenv();
local var_699 = getgenv();
var_697.Did = false;
var_698.Did2 = false;
var_699.Open = false;
qol.create_toggle({ title = "Auto Second Sea", desc = "Auto Gets Scroll And Goes To Second Sea", default = false }, function(arg_225)
    getgenv().ss2 = arg_225;
    task.spawn(function()
        while getgenv().ss2 and task.wait(1) do
            fake_geppo();
        end;
        return ;
    end);
    while getgenv().ss2 and task.wait() do
        local vector_10 = Vector3.new(-7350.17431640625, 4.758918762207031, -14949.48828125);
        if not getgenv().did then
            if not func_68(vector_10, 10, true) then
                horo_tp_pro({
                    Position = vector_10,
                    Speed = 50,
                    Stop = function()
                        return getgenv().ss2;
                    end
                });
            else
                func_70(vector_10);
                local child_24 = workspace.Effects:FindFirstChild("World Scroll");
                if child_24 then
                    repeat
                        task.wait();
                        func_70(vector_10);
                        local var_700 = child_24:FindFirstChildWhichIsA("ProximityPrompt");
                        var_700:InputHoldBegin();
                        task.wait(var_700.HoldDuration);
                        var_700:InputHoldEnd();
                    until not child_24 or not child_24.Parent or not getgenv().ss2 or func_71("World Scroll") or get_inventory_tool("World Scroll");
                    getgenv().did = true;
                else
                    getgenv().did = true;
                end;
            end;
        elseif not getgenv().did2 then
            if not func_68(Vector3.new(-8077, 18, -10787), 10, true) then
                horo_tp_pro({
                    Position = Vector3.new(-8077, 18, -10787),
                    Speed = 50,
                    Stop = function()
                        return getgenv().ss2;
                    end
                });
            else
                getgenv().did2 = true;
            end;
        elseif not func_68(Vector3.new(-8007, 52, -8540), 10, true) then
            horo_tp_pro({
                Position = Vector3.new(-8007, 52, -8540),
                Speed = 50,
                Stop = function()
                    return getgenv().ss2;
                end
            });
        else
            func_70(Vector3.new(-8007, 52, -8540));
            if not getgenv().open then
                repeat
                    task.wait();
                    if not player.PlayerGui:FindFirstChild("NPCCHAT") then
                        fireproximityprompt(workspace.NPCs["Gatekeeper Puff"].UpperTorso.Prompt);
                    else
                        local flag_48 = nil;
                        pcall(function()
                            flag_48 = game:GetService("Players").LocalPlayer.PlayerGui.NPCCHAT.Frame.ImageLabel.desc.Line001.Container.Group001.TextColor3 == Color3.fromRGB(255, 102, 102);
                            return ;
                        end);
                        if flag_48 then
                            getgenv().open = true;
                        else
                            firesignal(game:GetService("Players").LocalPlayer.PlayerGui.NPCCHAT.Frame.endChat.MouseButton1Click);
                        end;
                    end;
                until getgenv().open;
            else
                touch(workspace.Effects["2ndSeaTeleport"]);
                task.wait(1);
            end;
        end;
    end;
    return ;
end);
qol.create_toggle({ title = "Reveal Fruits", desc = "Reveals Fruits In HotBar And Inventory", default = false }, function(arg_226)
    getgenv().fah = arg_226;
    while getgenv().fah and task.wait() do
        pcall(function()
            local func_72 = function(arg_227)
                local child_25 = arg_227:FindFirstChild("ToolImage", true);
                local child_26 = arg_227:FindFirstChild("ToolObject", true);
                if child_25 and (child_25.Image == "rbxassetid://2766648781" and child_26 and child_26.Value) then
                    if child_25.Parent:FindFirstChild("ToolViewport") then
                        return ;
                    end;
                    local var_701 = child_26.Value;
                    child_25.Visible = false;
                    local var_702 = Instance.new("ViewportFrame");
                    var_702.Name = "ToolViewport";
                    var_702.Size = UDim2.new(1, 0, 1, 0);
                    var_702.BackgroundTransparency = 1;
                    var_702.Parent = child_25.Parent;
                    local var_703 = Instance.new("Model");
                    for var_704, var_705 in var_701:GetDescendants() do
                        if var_705:IsA("BasePart") then
                            var_705:Clone().Parent = var_703;
                        end;
                    end;
                    var_703.Parent = var_702;
                    local var_706 = Instance.new("Camera");
                    var_706.Parent = var_702;
                    var_702.CurrentCamera = var_706;
                    local var_707, var_708 = var_703:GetBoundingBox();
                    var_706.CFrame = var_707 * CFrame.new(0, 0, math.max(var_708.X, var_708.Y, var_708.Z) * 0.8);
                end;
                return ;
            end;
            local game_13 = game:GetService("Players").LocalPlayer.PlayerGui.BackpackGui;
            for var_709, var_710 in game_13.Hotbar:GetChildren() do
                func_72(var_710);
            end;
            for var_711, var_712 in game_13.Backpack.Inventory.SpecialInventory:GetChildren() do
                func_72(var_712);
            end;
            return ;
        end);
    end;
    return ;
end);
qol.create_toggle({ title = "Auto Watch Xp Ads", desc = "Watches Ads When You Don't Have The Boost.", default = false }, function(arg_228)
    getgenv().abc = arg_228;
    if not arg_228 then
        return ;
    end;
    if func_38() < 126 then
        library.create_noti({ title = "Uh Oh!", desc = "You need lvl 125 to unlock ads!", show_time = 4 });
    end;
    while getgenv().abc and task.wait(1) do
        if player.PlayerGui.HUD.Main.Boosts.Exp.Visible ~= true then
            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ADShop"):FireServer(3457788725);
            task.wait(10);
        end;
    end;
    return ;
end);
qol.create_toggle({ title = "Auto Watch Drop Rate Ads", desc = "Watches Ads When You Don't Have The Boost.", default = false }, function(arg_229)
    getgenv().abc2 = arg_229;
    if not arg_229 then
        return ;
    end;
    if func_38() < 126 then
        library.create_noti({ title = "Uh Oh!", desc = "You need lvl 125 to unlock ads!", show_time = 4 });
    end;
    while getgenv().abc2 and task.wait(1) do
        if player.PlayerGui.HUD.Main.Boosts.Drop.Visible ~= true then
            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ADShop"):FireServer(3457789187);
            task.wait(10);
        end;
    end;
    return ;
end);
local func_73 = function()
    for var_713, var_714 in next, workspace.Effects:GetChildren() do
        if var_714:IsA("BasePart") then
            local child_27 = var_714:FindFirstChild("Mesh");
            if child_27 and child_27.TextureId == "http://www.roblox.com/asset/?id=28461501" then
                warn("found");
                return var_714;
            end;
        end;
    end;
    return nil;
end;
local child_28 = game.ReplicatedStorage:FindFirstChild("FishingShopRemote");
getgenv().dont_sell_legendary = false;
sell_fish = function()
    local game_14 = game.ReplicatedStorage;
    local var_715 = require(game_14.Modules.ToolDesc);
    local game_15 = game:GetService("HttpService");
    for key_75, value_77 in pairs(game_15:JSONDecode(game_14["Stats" .. game.Players.LocalPlayer.Name].Inventory.Inventory.Value)) do
        local var_716 = var_715[key_75];
        if var_716 and var_716.type == "Fish" then
            if getgenv().fish_kaitun.LegendaryBait and var_716.Rare == "Legendary" and func_35("Legendary Fish Bait") < 300 then
                return ;
            end;
            if getgenv().fish_kaitun.RareBait and var_716.Rare == "Rare" and func_35("Rare Fish Bait") < 300 then
                return ;
            end;
            if not getgenv().buying then
                if func_36() >= 1000000 then
                    return ;
                end;
                local var_717 = getgenv().fish_kaitun and getgenv().fish_kaitun.sell_rarities;
                if var_717 and table.find(var_717, var_716.Rare) then
                    if func_36() >= 1000000 then
                        return ;
                    end;
                    child_28:InvokeServer({ Method = "SellFish", Fish = key_75, All = false });
                    task.wait(0.5);
                end;
            end;
        end;
    end;
    return ;
end;
get_rarity = function(arg_230)
    local var_718 = require(game.ReplicatedStorage.Modules.ToolDesc)[arg_230];
    return var_718 and var_718.Rare or nil;
end;
local flag_49 = false;
getgenv().tween_speed = 1;
local func_75 = function(arg_231, arg_232, arg_233, arg_234)
    if flag_49 then
        return ;
    end;
    local var_719 = arg_232 or tonumber(getgenv().tween_speed);
    local var_720 = arg_233 == nil and true or arg_233;
    local var_721 = arg_234 == nil and true or arg_234;
    if typeof(arg_231) == "CFrame" then
        arg_231 = arg_231.Position;
    elseif typeof(arg_231) ~= "Vector3" then
        return ;
    end;
    local game_16 = game:GetService("Players").LocalPlayer.Character;
    if not game_16 then
        return ;
    end;
    local child_29 = game_16:FindFirstChild(var);
    if not child_29 then
        return ;
    end;
    flag_49 = true;
    local game_17 = game:GetService("RunService");
    if (Vector3.new(child_29.Position.X, 0, child_29.Position.Z) - Vector3.new(arg_231.X, 0, arg_231.Z)).Magnitude >= 10 and var_721 then
        task.spawn(function()
            while flag_49 and task.wait() do
                if stats_folder.Skills:FindFirstChild("skyWalk").Value == true then
                    game:GetService("ReplicatedStorage").Events.Skill:InvokeServer("Sky Walk2", { char = game_16, cf = CFrame.new(child_29.Position) });
                    task.wait(1);
                else
                    game:GetService("ReplicatedStorage").Events.takestam:FireServer(1, "dash");
                    task.wait(3);
                end;
            end;
            return ;
        end);
    end;
    local func_74 = function(arg_235, arg_236)
        local var_722 = (Vector2.new(arg_235.X, arg_235.Z) - Vector2.new(arg_236.X, arg_236.Z)).Magnitude;
        if var_722 == 0 then
            return arg_236;
        end;
        local var_723 = var_722 / var_719;
        local num_55 = 0;
        while num_55 < var_723 do
            if not flag_49 then
                return "STOP";
            end;
            num_55 = num_55 + game_17.RenderStepped:Wait();
            local num_56 = math.clamp(num_55 / var_723, 0, 1);
            local var_724 = arg_235.X + (arg_236.X - arg_235.X) * num_56;
            local var_725 = arg_235.Z + (arg_236.Z - arg_235.Z) * num_56;
            child_29.Velocity = Vector3.zero;
            child_29.CFrame = CFrame.new(Vector3.new(var_724, arg_235.Y, var_725));
        end;
        child_29.Velocity = Vector3.zero;
        child_29.CFrame = CFrame.new(arg_236);
        return "OK";
    end;
    local var_726 = child_29.Position;
    local var_727 = var_726.Y + 1000;
    if var_720 then
        var_726 = Vector3.new(var_726.X, var_727, var_726.Z);
        child_29.CFrame = CFrame.new(var_726);
        child_29.Velocity = Vector3.zero;
    end;
    if func_74(var_726, (Vector3.new(arg_231.X, var_726.Y, arg_231.Z))) == "STOP" then
        return ;
    end;
    child_29.CFrame = CFrame.new(arg_231);
    child_29.Velocity = Vector3.zero;
    flag_49 = false;
    return ;
end;
task.spawn(pcall, function()
    local game_18 = game:GetService("ReplicatedStorage"):FindFirstChild("Fishing"):FindFirstChild("Remotes"):FindFirstChild("Action");
    return ;
end);
local num_57 = 0;
horo_tp_running = false;
getgenv().tuff2 = true;
horo_tp2 = function(arg_237, arg_238, arg_239, arg_240)
    local func_76 = function()
        if getgenv().tuff2 then
            return true;
        end;
        if getgenv().fish_kaitun and getgenv().fish_kaitun.Enabled then
            return true;
        end;
        return false;
    end;
    if horo_tp_running then
        return ;
    end;
    if not func_76() then
        warn("not enabled :C");
        return ;
    end;
    local num_58 = 50;
    local var_728 = arg_240 or 300;
    local game_19 = game:GetService("Players").LocalPlayer;
    local var_729 = game_19.Character or game_19.CharacterAdded:Wait();
    local child_30 = var_729:WaitForChild(var, 5);
    if not child_30 then
        return ;
    end;
    horo_tp_running = true;
    local game_20 = game:GetService("RunService");
    local var_730 = child_30.Position.Y + var_728;
    local var_731 = child_30.Position;
    local var_732 = var_731.Y < -100;
    local flag_50 = false;
    pcall(function()
        local var_733 = get_current_island();
        if var_733 and var_733.Name == "Fishman Island" then
            flag_50 = true;
        end;
        return ;
    end);
    local var_734 = var_732 or flag_50;
    local var_735 = RaycastParams.new();
    var_735.FilterType = Enum.RaycastFilterType.Exclude;
    var_735.FilterDescendantsInstances = { var_729, workspace:FindFirstChild("PlayerCharacters"), workspace:FindFirstChild("Effects") };
    local func_77 = function()
        local flag_51 = false;
        pcall(function()
            for var_736, var_737 in game_19.PlayerGui.Notifications:GetDescendants() do
                if var_737:IsA("TextLabel") and (var_737.Text == "TP" or var_737.Text == "Strike" or var_737.Text:find("Strike")) then
                    flag_51 = true;
                    return ;
                end;
            end;
            return ;
        end);
        return flag_51;
    end;
    local func_78 = function(arg_241, arg_242)
        if not func_76() then
            return var_731.Y;
        end;
        if arg_239 then
            return var_731.Y;
        end;
        local vector_11 = Vector3.new(arg_241, var_730, arg_242);
        local vector_12 = Vector3.new(0, -2000, 0);
        local workspace_2 = workspace:Raycast(vector_11, vector_12, var_735);
        while workspace_2 and (not workspace_2.Instance.CanCollide and not workspace_2.Instance.CanQuery or workspace_2.Instance.Name == "Leaves") do
            local var_738 = workspace_2.Position - Vector3.new(0, 0.1, 0);
            workspace_2 = workspace:Raycast(var_738, vector_12, var_735);
        end;
        if not workspace_2 then
            return var_734 and var_731.Y or math.max(var_731.Y, 2);
        end;
        if var_731.Y > -1500 then
            local var_739 = workspace_2.Instance.Color;
            local num_59 = math.floor(var_739.R * 255);
            local num_60 = math.floor(var_739.G * 255);
            local num_61 = math.floor(var_739.B * 255);
            if num_59 == 148 and num_60 == 147 and num_61 == 150 or num_59 == 116 and num_60 == 134 and num_61 == 157 then
                local var_740 = game_19.Character;
                if var_740 then
                    local var_741 = var_740:GetAttribute("SpeedBypass") or 0;
                    if tick() - (num_57 or 0) >= 2 and var_741 < 1 then
                        num_57 = tick();
                        task.spawn(pcall, function()
                            local game_21 = game:GetService("ReplicatedStorage"):FindFirstChild("Events");
                            if game_21 then
                                local child_31 = game_21:FindFirstChild("takestam");
                                if child_31 then
                                    child_31:FireServer(0.52, "dash");
                                end;
                            end;
                            return ;
                        end);
                    end;
                end;
            end;
        end;
        local var_742 = workspace_2.Position.Y + 5;
        return var_734 and var_742 or math.max(var_742, 2);
    end;
    local func_79;
    func_79 = function()
        local var_743 = (Vector2.new(var_731.X, var_731.Z) - Vector2.new(arg_237.X, arg_237.Z)).Magnitude;
        if var_743 < 2 then
            local var_744 = func_78(arg_237.X, arg_237.Z);
            child_30.CFrame = CFrame.new(arg_237.X, var_744, arg_237.Z);
            horo_tp_running = false;
            return ;
        end;
        local var_745 = var_743 / num_58;
        local num_62 = 0;
        while num_62 < var_745 and func_76() do
            if func_77() then
                task.wait(5);
                var_731 = child_30.Position;
                func_79();
                return ;
            end;
            num_62 = num_62 + game_20.RenderStepped:Wait();
            local num_63 = math.clamp(num_62 / var_745, 0, 1);
            local var_746 = var_731.X + (arg_237.X - var_731.X) * num_63;
            local var_747 = var_731.Z + (arg_237.Z - var_731.Z) * num_63;
            local var_748 = func_78(var_746, var_747);
            child_30.Velocity = Vector3.zero;
            child_30.CFrame = CFrame.new(var_746, var_748, var_747);
        end;
        local var_749 = func_78(arg_237.X, arg_237.Z);
        child_30.CFrame = CFrame.new(arg_237.X, var_749, arg_237.Z);
        horo_tp_running = false;
        return ;
    end;
    func_79();
    return ;
end;
horo_tp3 = function(arg_243, arg_244, arg_245)
    if horo_tp_running then
        return ;
    end;
    if not getgenv().auto_juzo then
        return ;
    end;
    local num_64 = tonumber(getgenv().tween_speed) or 50;
    local game_22 = game:GetService("Players").LocalPlayer;
    local var_750 = game_22.Character;
    if not var_750 then
        return ;
    end;
    local child_32 = var_750:FindFirstChild(var);
    if not child_32 then
        return ;
    end;
    horo_tp_running = true;
    local game_23 = game:GetService("RunService");
    local var_751 = RaycastParams.new();
    var_751.FilterType = Enum.RaycastFilterType.Exclude;
    var_751.FilterDescendantsInstances = { var_750 };
    local func_80 = function()
        local flag_52 = false;
        pcall(function()
            for var_752, var_753 in next, game_22.PlayerGui.Notifications:GetDescendants() do
                if var_753:IsA("TextLabel") and (var_753.Text == "TP" or var_753.Text == "Strike" or var_753.Text:match("Strike")) then
                    flag_52 = true;
                    return ;
                end;
            end;
            return ;
        end);
        return flag_52;
    end;
    local func_81 = function(arg_246, arg_247)
        if not getgenv().auto_juzo then
            return child_32.Position.Y;
        end;
        if arg_245 then
            return child_32.Position.Y;
        end;
        local num_65 = math.min(child_32.Position.Y + 300, child_32.Position.Y + 500);
        local vector_13 = Vector3.new(arg_246, num_65, arg_247);
        local vector_14 = Vector3.new(0, -1000, 0);
        while getgenv().auto_juzo do
            local workspace_3 = workspace:Raycast(vector_13, vector_14, var_751);
            if not workspace_3 then
                return child_32.Position.Y;
            end;
            local var_754 = workspace_3.Instance;
            local var_755 = var_754.CanCollide == true or var_754.CanQuery == true;
            if var_754.Name ~= "Leaves" and var_755 then
                local var_756 = var_754.Color;
                local num_66 = math.floor(var_756.R * 255);
                local num_67 = math.floor(var_756.G * 255);
                local num_68 = math.floor(var_756.B * 255);
                if (num_66 == 148 and num_67 == 147 and num_68 == 150 or num_66 == 116 and num_67 == 134 and num_68 == 157) and tick() - num_57 >= 2 and game_22.Character:GetAttribute("SpeedBypass") < 1 then
                    num_57 = tick();
                    game:GetService("ReplicatedStorage").Events.takestam:FireServer(unpack({ 0.52, "dash" }));
                    task.wait(0.5);
                end;
                return workspace_3.Position.Y + 5;
            end;
            vector_13 = workspace_3.Position - Vector3.new(0, 1, 0);
        end;
        return child_32.Position.Y;
    end;
    local func_82;
    func_82 = function(arg_248, arg_249, arg_250)
        if not getgenv().auto_juzo then
            return "STOP";
        end;
        local var_757 = (Vector2.new(arg_248.X, arg_248.Z) - Vector2.new(arg_249.X, arg_249.Z)).Magnitude;
        if var_757 == 0 then
            return "OK";
        end;
        local var_758 = var_757 / arg_250;
        local num_69 = 0;
        while num_69 < var_758 do
            if not getgenv().auto_juzo then
                return "STOP";
            end;
            if func_80() then
                wait(5);
                local var_759 = child_32.Position;
                return func_82(var_759, arg_249, 30);
            end;
            num_69 = num_69 + game_23.RenderStepped:Wait();
            local num_70 = math.clamp(num_69 / var_758, 0, 1);
            local var_760 = arg_248.X + (arg_249.X - arg_248.X) * num_70;
            local var_761 = arg_248.Z + (arg_249.Z - arg_248.Z) * num_70;
            local var_762 = func_81(var_760, var_761);
            child_32.Velocity = Vector3.zero;
            child_32.CFrame = CFrame.new(var_760, var_762, var_761);
        end;
        if not getgenv().auto_juzo then
            return "STOP";
        end;
        local var_763 = func_81(arg_249.X, arg_249.Z);
        child_32.Velocity = Vector3.zero;
        child_32.CFrame = CFrame.new(arg_249.X, var_763, arg_249.Z);
        return "OK";
    end;
    func_82(child_32.Position, arg_243, num_64);
    horo_tp_running = false;
    return ;
end;
getgenv().delay = false;
getgenv().buying = false;
local flag_53 = false;
getgenv().tween_speed = 50;
getgenv().current_horo_target = nil;
getgenv().cancel_horo_tp = nil;
horo_tp = function(arg_251, arg_252)
    if flag_53 then
        if not getgenv().current_horo_target or not ((Vector2.new(arg_251.X, arg_251.Z) - Vector2.new(getgenv().current_horo_target.X, getgenv().current_horo_target.Z)).Magnitude > 5) then
            return ;
        end;
        getgenv().cancel_horo_tp = true;
        repeat
            task.wait();
        until not flag_53;
    end;
    getgenv().current_horo_target = arg_251;
    getgenv().cancel_horo_tp = false;
    local var_764 = func_67();
    local game_24 = game:GetService("Players").LocalPlayer;
    local var_765 = game_24.Character;
    if not var_765 then
        return ;
    end;
    local child_33 = var_765:FindFirstChild(var);
    if not child_33 then
        return ;
    end;
    if getgenv().fish_kaitun and getgenv().fish_kaitun.Enabled == false then
        return ;
    end;
    local func_83 = function()
        local var_766 = RaycastParams.new();
        var_766.FilterType = Enum.RaycastFilterType.Exclude;
        local tbl_40 = { var_765 };
        for var_767, var_768 in game:GetService("Players"):GetPlayers() do
            if var_768.Character then
                table.insert(tbl_40, var_768.Character);
            end;
        end;
        var_766.FilterDescendantsInstances = tbl_40;
        return var_766;
    end;
    local func_84 = function()
        local var_769 = var_765:FindFirstChildOfClass("Humanoid");
        if var_769 then
            var_769:UnequipTools();
        end;
        return ;
    end;
    local func_85 = function()
        local child_34 = game_24:FindFirstChild("Stats");
        if not child_34 or not child_34:FindFirstChild("Skills") then
            return ;
        end;
        local child_35 = child_34.Skills:FindFirstChild("skyWalk");
        if not child_35 or not child_35.Value then
            return ;
        end;
        while game_24.Character and game_24.Character:GetAttribute("SpeedBypass") and game_24.Character:GetAttribute("SpeedBypass") < 15 do
            func_84();
            task.wait(0.1);
        end;
        return ;
    end;
    if (Vector2.new(arg_251.X, arg_251.Z) - Vector2.new(child_33.Position.X, child_33.Position.Z)).Magnitude < 8 then
        local var_770 = func_83();
        local vector_15 = Vector3.new(arg_251.X, 1000, arg_251.Z);
        local workspace_4 = workspace:Raycast(vector_15, Vector3.new(0, -2000, 0), var_770);
        local var_771 = workspace_4 and math.max(workspace_4.Position.Y + 5, 2) or math.max(child_33.Position.Y, 2);
        child_33.CFrame = CFrame.new(arg_251.X, var_771, arg_251.Z);
        child_33.Velocity = Vector3.zero;
        getgenv().current_horo_target = nil;
        return ;
    end;
    getgenv().busy = true;
    func_84();
    func_85();
    flag_53 = true;
    local var_772 = func_83();
    local func_86 = function()
        local flag_54 = false;
        pcall(function()
            for var_773, var_774 in next, game:GetService("Players").LocalPlayer.PlayerGui.Notifications:GetDescendants() do
                if var_774:IsA("TextLabel") and (var_774.Text == "TP" or var_774.Text == "Strike" or var_774.Text:match("Strike")) then
                    flag_54 = true;
                    return ;
                end;
            end;
            return ;
        end);
        return flag_54;
    end;
    local func_87 = function(arg_253, arg_254)
        local vector_16 = Vector3.new(arg_253, 1000, arg_254);
        local vector_17 = Vector3.new(0, -2000, 0);
        local workspace_5 = workspace:Raycast(vector_16, vector_17, var_772);
        if workspace_5 then
            local var_775 = workspace_5.Position.Y + 5;
            return math.max(var_775, 2);
        end;
        return math.max(child_33.Position.Y, 2);
    end;
    (function(arg_255)
        while var_765.Parent and child_33.Parent do
            if getgenv().cancel_horo_tp then
                return "CANCELLED";
            end;
            if not getgenv().fish_kaitun or not getgenv().fish_kaitun.Enabled then
                flag_53 = false;
                return "ABORT";
            end;
            if func_86() then
                task.wait(5);
            else
                func_85();
                local var_776 = child_33.CFrame.Position;
                local vector_18 = Vector2.new(arg_255.X, arg_255.Z) - Vector2.new(var_776.X, var_776.Z);
                local var_777 = vector_18.Magnitude;
                if var_777 < 0.5 then
                    local var_778 = func_87(arg_255.X, arg_255.Z);
                    child_33.CFrame = CFrame.new(arg_255.X, var_778, arg_255.Z);
                    child_33.Velocity = Vector3.zero;
                    return "OK";
                end;
                local var_779 = func_67();
                local var_780 = task.wait();
                local num_71 = math.min(var_779 * var_780, var_777);
                local vector_19 = Vector3.new(vector_18.X, 0, vector_18.Y).Unit;
                local var_781 = var_776.X + vector_19.X * num_71;
                local var_782 = var_776.Z + vector_19.Z * num_71;
                local var_783 = func_87(var_781, var_782);
                child_33.CFrame = CFrame.new(var_781, var_783, var_782);
                child_33.Velocity = Vector3.zero;
            end;
        end;
        return "ABORT";
    end)(arg_251);
    flag_53 = false;
    getgenv().current_horo_target = nil;
    getgenv().busy = false;
    return ;
end;
getgenv().rifle = nil;
local func_88 = function()
    local child_36 = player.Backpack:FindFirstChild("Rifle") or player.Character:FindFirstChild("Rifle");
    if not child_36 then
        tools_event:InvokeServer("equip", "Rifle");
        task.wait(0.5);
    else
        child_36.Parent = player.Character;
        if not getgenv().rifle then
            local var_784 = require(game.ReplicatedStorage.Modules.GunHandle);
            getgenv().rifle = var_784.New("Rifle", var_530.Character:FindFirstChild("Rifle"));
        end;
        rifle = getgenv().rifle;
        if rifle then
            pcall(function()
                CIcklconEvent = game:GetService("ReplicatedStorage").Events.CIcklcon;
                return ;
            end);
            pcall(function()
                char = player.Character;
                local var_785 = char and char:FindFirstChild(var);
                if not var_785 then
                    return ;
                end;
                local flag_55 = nil;
                local num_72 = math.huge;
                for key_76, value_78 in pairs(workspace.NPCs:GetChildren()) do
                    if value_78.Name == "Fishman Karate User" and value_78:FindFirstChild(var) and not value_78:FindFirstChild("Blocking") then
                        local var_786 = (var_785.Position - value_78.HumanoidRootPart.Position).Magnitude;
                        if var_786 < num_72 then
                            flag_55 = value_78;
                            num_72 = var_786;
                        end;
                    end;
                end;
                if flag_55 then
                    if not char:FindFirstChild("Rifle") or not char:FindFirstChild("RifleGun") then
                        return ;
                    end;
                    if not rifle.Reloaded then
                        rifle:Reload();
                    end;
                    task.spawn(function()
                        local tbl_41 = { "reload", { Gun = "Rifle" } };
                        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("CIcklcon"):WaitForChild("gunFunctions"):InvokeServer(unpack(tbl_41));
                        return ;
                    end);
                    local var_787 = char.RifleGun.Hole;
                    local tbl_42 = { Gun = "Rifle", Position = flag_55.Head.Position, Start = var_787.CFrame, joe = "true" };
                    if flag_55.Parent and flag_55:FindFirstChild(var) then
                        game:GetService("ReplicatedStorage").Events.CIcklcon:FireServer("fire", tbl_42);
                    end;
                    rifle.Reloaded = false;
                    rifle.Equiped = true;
                    rifle.IsAiming = true;
                    rifle.Ready = true;
                    game:GetService("ReplicatedStorage").Events.stats:FireServer(table.unpack({ "GunMastery", nil, 1 }));
                    local tbl_43 = { "reload", { Gun = "Rifle" } };
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("CIcklcon"):WaitForChild("gunFunctions"):InvokeServer(unpack(tbl_43));
                end;
                return ;
            end);
        end;
    end;
    return ;
end;
level_section.create_toggle({ title = "Level Farm Unpatched (Beta)", desc = "Level farm 0-625 lvl", default = false }, function(arg_256)
    getgenv().tuff2 = arg_256;
    if arg_256 then
        pcall(function()
            workspace.Islands["Town of Beginnings"].ImportantBuildings.BoatShop:Destroy();
            return ;
        end);
    end;
    if not arg_256 then
        return ;
    end;
    task.spawn(function()
        local var_788 = tick();
        while getgenv().tuff2 and task.wait() do
            local str_36 = string.find(inventory_value.Value, "Rifle");
            local str_37 = string.find(inventory_value.Value, "Rowboat");
            if not str_36 then
                if func_36() >= 300 then
                    if (Vector3.new(-532, 6, -3450) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                        horo_tp2(Vector3.new(-532, 6, -3450));
                    else
                        game:GetService("ReplicatedStorage").Events.Shop:InvokeServer(workspace.BuyableItems.Rifle, 1);
                    end;
                elseif func_38() >= 3 then
                    if func_40() ~= "Find Sarah's necklace" then
                        var_788 = tick();
                        if (Vector3.new(-546, 9, -3403) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                            horo_tp2(Vector3.new(-546, 9, -3403));
                        else
                            func_64("Find Sarah's necklace");
                            task.wait(2);
                        end;
                    elseif tick() - var_788 > 30 then
                        if (Vector3.new(-546, 9, -3403) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                            horo_tp2(Vector3.new(-546, 9, -3403));
                        else
                            func_64("Find Sarah's necklace");
                            var_788 = tick();
                            task.wait(2);
                        end;
                    else
                        local flag_56 = nil;
                        pcall(function()
                            flag_56 = func_73();
                            return ;
                        end);
                        if flag_56 then
                            horo_tp2(flag_56.Position);
                            pcall(function()
                                fireproximityprompt(flag_56:FindFirstChildWhichIsA("ProximityPrompt"), true);
                                return ;
                            end);
                        elseif (Vector3.new(-546, 9, -3403) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                            horo_tp2(Vector3.new(-546, 9, -3403));
                        else
                            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Quest"):InvokeServer({ "returnitem" });
                            task.wait(2);
                        end;
                    end;
                elseif workspace.Env:FindFirstChild("Chest") then
                    pcall(function()
                        local flag_57 = nil;
                        local num_73 = math.huge;
                        local flag_58 = nil;
                        for var_789, var_790 in next, workspace.Env:GetChildren() do
                            if var_790.Name == "Chest" then
                                local var_791 = var_790:GetPivot().Position;
                                local var_792 = (var_791 - player.Character.HumanoidRootPart.Position).Magnitude;
                                if var_792 < 500 and var_791.Y < 50 then
                                    local var_793 = get_prompt(var_791);
                                    if var_793 and var_792 < num_73 then
                                        flag_57 = var_790;
                                        flag_58 = var_793;
                                        num_73 = var_792;
                                    end;
                                end;
                            end;
                        end;
                        if flag_57 and flag_58 then
                            pcall(function()
                                horo_tp2(flag_57:GetPivot().Position);
                                fireproximityprompt(flag_58);
                                return ;
                            end);
                        end;
                        return ;
                    end);
                elseif workspace.NPCs:FindFirstChild("Bandit") then
                    if func_40() ~= "Help Daph" then
                        if not func_68(Vector3.new(-576, 5, -3431), 10) then
                            horo_tp_pro({
                                Position = Vector3.new(-576, 5, -3431),
                                Speed = 30,
                                Stop = function()
                                    return getgenv().tuff2;
                                end
                            });
                        else
                            func_64("Help Daph");
                        end;
                    else
                        local child_37 = workspace.NPCs:FindFirstChild("Bandit");
                        if child_37 then
                            player.Character.HumanoidRootPart.Velocity = Vector3.zero;
                            horo_tp2(child_37:GetPivot().Position);
                            task.spawn(function()
                                getgenv().hit(child_37);
                                return ;
                            end);
                        end;
                    end;
                end;
            elseif get_current_island().Name ~= "Fishman Island" then
                if (Vector3.new(1838, 4, -12173) - player.Character.HumanoidRootPart.Position).Magnitude > 300 then
                    horo_tp2(Vector3.new(1838, 4, -12173));
                elseif (Vector3.new(workspace.AreaTeleporters.FirstSea.Fishman.Part.Position.X, 0, workspace.AreaTeleporters.FirstSea.Fishman.Part.Position.Z) - Vector3.new(player.Character.HumanoidRootPart.Position.X, 0, player.Character.HumanoidRootPart.Position.Z)).Magnitude < 10 then
                    repeat
                        task.wait();
                    until func_39() > 10;
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("takestam"):FireServer(unpack({ 0.525, "dash" }));
                    task.wait(0.5);
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(workspace.AreaTeleporters.FirstSea.Fishman.Part.Position);
                    task.wait(10);
                else
                    horo_tp2(workspace.AreaTeleporters.FirstSea.Fishman.Part.Position);
                end;
            elseif get_current_island().Name == "Fishman Island" then
                if stats_folder.Stats.SpawnPoint.Value ~= "Fishman Island" then
                    horo_tp2(Vector3.new(7977, -2153, -17074), 30, true);
                    if workspace.NPCs:FindFirstChild("Robo") then
                        quest_event:InvokeServer({ "npcChat", true });
                        set_spawn_event:FireServer(nil, workspace_service.NPCs.Robo);
                    end;
                elseif (Vector3.new(7839, -2151, -17135) - player.Character.HumanoidRootPart.Position).Magnitude > 10 then
                    horo_tp2(Vector3.new(7839, -2151, -17135), 30, false, 100);
                elseif func_40() ~= "Help becky" and func_38() >= 190 then
                    horo_tp2(Vector3.new(7733, -2176, -17218), 30, false, 30);
                    task.wait(1);
                    func_64("Help becky");
                    task.wait(1);
                else
                    pcall(function()
                        func_88();
                        return ;
                    end);
                    task.wait(0.2);
                end;
            end;
        end;
        return ;
    end);
    return ;
end);
getgenv().hit_delay = getgenv().hit_delay or 0.45;
hit = function(arg_257)
    if typeof(arg_257) ~= "table" or #arg_257 == 0 then
        return ;
    end;
    local local_player_4 = game.Players.LocalPlayer;
    local game_25 = game:GetService("ReplicatedStorage");
    local var_794 = local_player_4.Character;
    local var_795 = var_794 and var_794:FindFirstChildOfClass("Humanoid");
    if not var_794 or not var_795 then
        return ;
    end;
    getgenv().hit_delay = getgenv().hit_delay or 0.45;
    getgenv().swing_delay = getgenv().swing_delay or 0.05;
    getgenv().combo_reset_delay = getgenv().combo_reset_delay or 1.2;
    getgenv().throttle = getgenv().throttle or 0.05;
    getgenv().last_hit_tick = getgenv().last_hit_tick or 0;
    getgenv().next_hit_time = getgenv().next_hit_time or 0;
    getgenv().combo = getgenv().combo or 1;
    local var_796 = tick();
    if var_796 < getgenv().next_hit_time or var_796 - getgenv().last_hit_tick < getgenv().throttle then
        return ;
    end;
    local flag_59 = nil;
    for var_797, var_798 in next, var_794:GetChildren() do
        if var_798:GetAttribute("MeleeTool") then
            flag_59 = var_798;
            break;
        end;
    end;
    for var_799, var_800 in next, local_player_4.Backpack:GetChildren() do
        if var_800:GetAttribute("MeleeTool") then
            flag_59 = var_800;
            break;
        end;
    end;
    if not flag_59 then
        return ;
    end;
    if flag_59.Parent ~= var_794 then
        flag_59.Parent = var_794;
    end;
    local var_801 = select(2, getgenv().get_style());
    local child_38 = game_25:FindFirstChild("CombatAnimations"):FindFirstChild(var_801);
    if not child_38 then
        return ;
    end;
    local var_802 = getgenv().combo;
    local child_39 = child_38:FindFirstChild("Punch" .. var_802) or child_38:FindFirstChild("GroundPunch" .. var_802) or child_38:FindFirstChild("Punch1");
    if not child_39 then
        return ;
    end;
    local child_40 = var_794:FindFirstChild(var);
    if not child_40 then
        return ;
    end;
    local var_803 = child_40.Position;
    local num_74 = 0;
    local num_75 = 0;
    local num_76 = 0;
    for var_804, var_805 in next, arg_257, nil do
        if typeof(var_805) == "Instance" and var_805:IsA("BasePart") and var_805.Parent then
            num_74 = num_74 + var_805.Position.X;
            num_75 = num_75 + var_805.Position.Z;
            num_76 = num_76 + 1;
        end;
    end;
    if num_76 == 0 then
        return ;
    end;
    local vector_20 = Vector3.new(num_74 / num_76, var_803.Y, num_75 / num_76);
    local cframe = CFrame.new(var_803, vector_20);
    local var_806 = var_795:FindFirstChildOfClass("Animator");
    local var_807 = var_806 and var_806:LoadAnimation(child_39);
    local var_808 = local_player_4:GetAttribute("Ping") and math.clamp(local_player_4:GetAttribute("Ping"), 0, 0.35) or 0.05;
    local var_809 = (var_807 and var_807.Length or 0.35) / (var_807 and var_807.Speed ~= 0 and var_807.Speed or 1) * 0.4 - var_808;
    if var_809 < 0.03 then
        var_809 = 0.03;
    end;
    if var_807 then
        var_807:Stop();
        var_807:Destroy();
    end;
    getgenv().last_hit_tick = var_796;
    getgenv().next_hit_time = var_796 + var_809 + getgenv().swing_delay;
    pcall(function()
        game_25.Events.CombatRegister:InvokeServer({ "swingsfx", var_801, var_802, "Ground", false, child_39, 1.75, 2, cframe });
        return ;
    end);
    task.delay(var_809 + getgenv().hit_delay, function()
        local tbl_44 = {};
        for var_810, var_811 in next, arg_257, nil do
            if typeof(var_811) == "Instance" and var_811.Parent then
                table.insert(tbl_44, var_811);
            end;
        end;
        if #tbl_44 == 0 then
            return ;
        end;
        local cframe_2 = CFrame.new(var_803, vector_20);
        pcall(function()
            game_25.Events.CombatRegister:InvokeServer({ "damage", tbl_44, var_801, { var_802, "Ground", var_801 }, true, cframe_2 });
            return ;
        end);
        return ;
    end);
    getgenv().combo = getgenv().combo + 1;
    if getgenv().combo > 5 then
        getgenv().combo = 1;
        getgenv().next_hit_time = var_796 + var_809 + getgenv().combo_reset_delay;
    end;
    return ;
end;
local func_89 = function(arg_258, arg_259)
    local var_812 = arg_258:FindFirstChildWhichIsA("Animator", true);
    if not var_812 then
        return false;
    end;
    local str_38 = tostring(arg_259);
    for var_813, var_814 in next, var_812:GetPlayingAnimationTracks() do
        if var_814.Animation and var_814.Animation.AnimationId == "rbxassetid://" .. str_38 then
            return true;
        end;
    end;
    return false;
end;
local func_90 = function(arg_260)
    local flag_60 = nil;
    local num_77 = math.huge;
    for var_815, var_816 in next, workspace.NPCs:GetChildren() do
        if var_816.Name == arg_260 then
            distance = (var_816:GetPivot().Position - player.Character.HumanoidRootPart.Position).Magnitude;
            if distance < num_77 then
                num_77 = distance;
                flag_60 = var_816;
            end;
        end;
    end;
    return flag_60;
end;
player_section.create_toggle({ title = "Tp Behind Player", desc = "Teleports behind the closest player", default = false, Keybind = true, DefaultKey = "N" }, function(arg_261)
    getgenv().tp_behind_enabled = arg_261;
    while getgenv().tp_behind_enabled do
        local var_817 = func_66();
        if var_817 and var_817.Character and var_817.Character:FindFirstChild(var) and player.Character and player.Character:FindFirstChild(var) then
            local var_818 = var_817.Character.HumanoidRootPart;
            local var_819 = var_818.Position + var_818.CFrame.LookVector * -5;
            player.Character.HumanoidRootPart.CFrame = CFrame.new(var_819, var_818.Position + Vector3.new(0, 3, 0));
        end;
        task.wait();
    end;
    return ;
end);
sell_rarities = { "Legendary", "Rare", "Common", "Uncommon", "Epic" };
getgenv().last_position = 0;
task.spawn(pcall, function()
    Action = game:GetService("ReplicatedStorage"):FindFirstChild("Fishing"):FindFirstChild("Remotes"):FindFirstChild("Action");
    return ;
end);
getgenv().fish_kaitun = { Enabled = false, Merchant = false, Bait = "Common Fish Bait", Buyables = {}, SetSpawn = false, WebhookFruits = { "Mythical", "Legendary", "Rare", "Epic", "Common", "Uncommon" }, DropFruits = { "Mythical", "Legendary", "Rare", "Epic", "Common", "Uncommon" }, Bait = "Best", EquipTitle = true, LegendaryBait = false, RareBait = false, BaitBuyAmount = 50, StoreFruit = true, sell_rarities = { "Legendary", "Rare", "Epic", "Common", "Uncommon" }, AutoSell = true, Webhook = "" };
getgenv().loop = false;
tiers = { ["Devil Fruit Rod"] = 8, ["ODM Rod"] = 7, ["Jack-O'Rod"] = 6, ["Angler Rod"] = 5, ["Merchants Banana Rod"] = 4, ["Epic Fishing Rod"] = 3, ["Rare Fishing Rod"] = 2, ["Fishing Rod"] = 1 };
baits = { ["Legendary Fish Bait"] = 1, ["Rare Fish Bait"] = 2, ["Common Fish Bait"] = 3 };
best_bait = function()
    if getgenv().fish_kaitun.Bait == "Best" then
        local flag_61 = nil;
        local num_78 = math.huge;
        local flag_62 = nil;
        for var_820, var_821 in next, baits, nil do
            if func_35(var_820) > 0 and var_821 < num_78 then
                flag_61 = var_820;
                num_78 = var_821;
            end;
        end;
        if flag_61 == "Legendary Fish Bait" then
            flag_62 = "legendary";
        elseif flag_61 == "Rare Fish Bait" then
            flag_62 = "rare";
        elseif flag_61 == "Common Fish Bait" then
            flag_62 = "common";
        end;
        return flag_61, flag_62;
    end;
    local flag_63 = nil;
    local var_822 = func_35(getgenv().fish_kaitun.Bait) > 0 and getgenv().fish_kaitun.Bait or "Common Fish Bait";
    if var_822 == "Legendary Fish Bait" then
        flag_63 = "legendary";
    elseif var_822 == "Rare Fish Bait" then
        flag_63 = "rare";
    elseif var_822 == "Common Fish Bait" then
        flag_63 = "common";
    end;
    return var_822, flag_63;
end;
get_stock = function(arg_262)
    local game_26 = game:GetService("Players");
    local game_27 = game:GetService("HttpService");
    local var_823 = game_26.LocalPlayer;
    if not var_823 then
        return 0;
    end;
    local child_41 = var_823:WaitForChild("PlayerGui"):FindFirstChild("MerchentShop");
    if not child_41 then
        return 0;
    end;
    local var_824 = child_41:GetAttribute("Prices");
    if not var_824 or var_824 == "" then
        return 0;
    end;
    local var_825, var_826 = pcall(game_27.JSONDecode, game_27, var_824);
    if not var_825 or type(var_826) ~= "table" then
        return 0;
    end;
    local var_827 = var_826[arg_262];
    if not var_827 then
        return 0;
    end;
    return var_827.remaining or 0;
end;
is_in_stock = function(arg_263)
    local game_28 = game:GetService("Players");
    local game_29 = game:GetService("HttpService");
    local var_828 = game_28.LocalPlayer;
    if not var_828 then
        return false;
    end;
    local child_42 = var_828:WaitForChild("PlayerGui"):FindFirstChild("MerchentShop");
    if not child_42 then
        return false;
    end;
    local var_829 = child_42:GetAttribute("Prices");
    if not var_829 or var_829 == "" then
        return false;
    end;
    local var_830, var_831 = pcall(game_29.JSONDecode, game_29, var_829);
    if not var_830 or type(var_831) ~= "table" then
        return false;
    end;
    local var_832 = var_831[arg_263];
    if not var_832 then
        return false;
    end;
    if (var_832.remaining or 0) <= 0 then
        return false;
    end;
    local var_833 = var_832.price or 0;
    if var_833 <= 0 then
        return false;
    end;
    local var_834 = func_36();
    if type(var_834) ~= "number" then
        return false;
    end;
    return var_834 >= var_833;
end;
time_left = function(arg_264)
    return math.max(0, arg_264 - os.time());
end;
tbl = { "Race Reroll", "SP Reset Essence", "Dark Root", "Spirit Color Essence", "Rare Fish Bait", "Golden Staff", "Golden Hook", "Tropical Parrot", "Blossom Skirt", "Raylo's Outfit", "Desert Merchant Outfit", "Sea-Breeze Skirt", "Tari's Karoo Coat", "Sea-Breeze Haori", "Crimson Nightcoat", "Powderpunk Outfit", "Shadowrunner Mask", "Shadowrunner's Outfit", "Street Enforcer Outfit", "Wastelander Goggles", "Special Tailor Token", "Karoo Mount", "Gravity Blade", "Jitte", "Sorcerer Hunter Costume", "Ulquiorra Outfit", "Bomi's Log Pose", "Rare Fruit Chest", "Spare Fruit Bag", "Thrilled Ship", "Raiui", "Kessui", "Bisento", "Crab Cutlass", "Knight's Gauntlet", "Merchants Banana Rod", "Legendary Fish Bait", "Hoverboard", "Striker", "Coffin Boat", "Legendary Fruit Chest", "Mythical Fruit Chest", "Turkey Head", "Cute Turkey Bucket Hat", "Pilgrim Hat", "Turkey Costume" };
get_inventory_data = function()
    if not inventory_value or inventory_value.Value == "" then
        return nil;
    end;
    local var_835, var_836 = pcall(function()
        return http_service:JSONDecode(inventory_value.Value);
    end);
    if var_835 and type(var_836) == "table" then
        return var_836;
    end;
    return nil;
end;
get_rod_inventory = function()
    local var_837 = get_inventory_data();
    if not var_837 then
        return nil;
    end;
    local flag_64 = nil;
    local var_838 = -math.huge;
    for var_839, var_840 in next, tiers, nil do
        if var_837[var_839] and var_838 < var_840 then
            var_838 = var_840;
            flag_64 = var_839;
        end;
    end;
    return flag_64;
end;
ensure_best_rod_equipped = function()
    local var_841 = get_rod_instance();
    local var_842 = var_841 and tiers[var_841.Name] or -math.huge;
    local var_843 = get_rod_inventory();
    if var_843 and var_842 < (tiers[var_843] or -math.huge) then
        tools_event:InvokeServer("equip", var_843);
        task.wait(2);
        var_841 = get_rod_instance();
    end;
    return var_841;
end;
get_rod_instance = function()
    local flag_65 = nil;
    local var_844 = -math.huge;
    local flag_66 = nil;
    for var_845, var_846 in next, tiers, nil do
        local var_847 = func_71(var_845);
        if var_847 and var_844 < var_846 then
            var_844 = var_846;
            flag_66 = var_845;
            flag_65 = var_847;
        end;
        if not var_847 and get_inventory_tool(var_845) and var_844 < var_846 then
            var_844 = var_846;
            flag_65 = nil;
            flag_66 = var_845;
        end;
    end;
    if flag_66 and not flag_65 then
        tools_event:InvokeServer("equip", flag_66);
        task.wait(0.5);
        flag_65 = func_71(flag_66);
    end;
    return flag_65;
end;
local tbl_45 = {};
local func_91 = function()
    local var_848 = get_rod_instance();
    if not var_848 then
        local var_849 = get_rod_inventory();
        if var_849 then
            warn("equipping");
            tools_event:InvokeServer("equip", var_849);
            task.wait(2);
            local var_850 = get_rod_instance();
        elseif func_36() > 60 then
            if (Vector3.new(-1340, 2, -4978) - player.Character.HumanoidRootPart.Position).Magnitude > 10 then
                func_70(Vector3.new(-1340, 2, -4978));
            else
                buy(buyable_items:FindFirstChild("Fishing Rod"));
            end;
        else
            for var_851, var_852 in next, workspace.Env:GetChildren() do
                if var_852.Name == "Chest" then
                    local var_853 = get_prompt(var_852:GetPivot().Position);
                    pcall(function()
                        func_70(var_853.Parent.Position);
                        fireproximityprompt(var_853);
                        return ;
                    end);
                end;
            end;
        end;
    else
        if var_848.Parent ~= player.Character then
            player.Character.Humanoid:EquipTool(var_848);
            task.wait(2);
            return ;
        end;
        if func_35("Common Fish Bait") <= 1 then
            if (Vector3.new(-1340, 2, -4978) - player.Character.HumanoidRootPart.Position).Magnitude > 10 then
                func_70(Vector3.new(-1340, 2, -4978));
            elseif func_36() >= 500 then
                repeat
                    task.wait();
                    local var_854 = func_35("Common Fish Bait");
                    buy(buyable_items:FindFirstChild("Common Fish Bait"));
                until var_854 >= 50 or not getgenv().fish_kaitun.Enabled;
            else
                repeat
                    task.wait();
                    for var_855, var_856 in next, workspace.Env:GetChildren() do
                        if var_856.Name == "Chest" then
                            local var_857 = get_prompt(var_856:GetPivot().Position);
                            pcall(function()
                                func_70(var_857.Parent.Position);
                                fireproximityprompt(var_857);
                                return ;
                            end);
                        end;
                    end;
                until func_36() >= 500 or not getgenv().fish_kaitun.Enabled;
            end;
        else
            local child_43 = workspace.Effects:FindFirstChild(player.Name .. "'s hook") or nil;
            if not child_43 then
                Action:InvokeServer({ Goal = Vector3.new(0, 0, 0), Action = "Throw", Bait = best_bait() });
                task.wait(0.5);
                Action:InvokeServer({ Action = "Landed" });
                local child_44 = workspace.Effects:FindFirstChild(player.Name .. "'s hook");
            else
                warn("here");
                child_43.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position);
                if child_43:GetAttribute("Caught") and not getgenv().loop then
                    warn("caught");
                    getgenv().loop = true;
                    task.spawn(function()
                        repeat
                            task.wait(0.5);
                            Action:InvokeServer({ Action = "Reel" });
                        until not child_43:GetAttribute("Caught") or not getgenv().fish_kaitun.Enabled;
                        getgenv().loop = false;
                        return ;
                    end);
                elseif child_43:GetAttribute("MoveMultiplier") and not child_43:GetAttribute("Caught") then
                    warn("here unequipping");
                    var_848.Parent = player.Backpack;
                    task.wait(0.2);
                    var_848.Parent = player.Character;
                end;
            end;
            if func_36() < 1000000 and not getgenv().buying then
                sell_fish();
            end;
            if getgenv().fish_kaitun.Merchant then
                local game_30 = game:GetService("ReplicatedStorage").CompassGuider:FindFirstChild("Traveling Merchant");
                if getgenv().last_position ~= game_30.Value then
                    getgenv().last_position = game_30.Value;
                    if game_30 and game:GetService("Players").LocalPlayer.PlayerGui.Compass.Compass.Guiders:FindFirstChild("Traveling Merchant").Visible then
                        warn("here teleporting gng?");
                        horo_tp(game_30.Value);
                        task.wait(2);
                        getgenv().buying = true;
                        repeat
                            task.wait(1);
                            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("TravelingMerchentRemote"):InvokeServer("OpenShop");
                        until player.PlayerGui:FindFirstChild("MerchentShop") or not getgenv().fish_kaitun.Enabled;
                        for var_858, var_859 in next, getgenv().fish_kaitun.Buyables, nil do
                            if is_in_stock(var_859) then
                                for i_6 = 1, 10, 1 do
                                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("TravelingMerchentRemote"):InvokeServer(var_859, game:GetService("Players").LocalPlayer.PlayerGui.MerchentShop:GetAttribute("Seed"));
                                end;
                                task.wait();
                            end;
                        end;
                        getgenv().buying = false;
                        local child_45 = time_left(player.PlayerGui:FindFirstChild("MerchentShop"):GetAttribute("NextRefresh"));
                        task.delay(child_45, function()
                            getgenv().last_position = 0;
                            return ;
                        end);
                    end;
                end;
            end;
            local local_player_5 = game.Players.LocalPlayer;
            for var_860, var_861 in next, local_player_5.Backpack:GetChildren() do
                if var_861:GetAttribute("verifiedFruit") and not tbl_45[var_861] then
                    local num_79 = 0;
                    repeat
                        task.wait(0.1);
                        num_79 = num_79 + 0.1;
                        if var_861.Parent ~= local_player_5.Character then
                            local_player_5.Character.Humanoid:EquipTool(var_861);
                        end;
                        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("FruitStorage"):InvokeServer(true);
                    until not var_861 or not var_861.Parent or num_79 > 2;
                    if var_861 and var_861.Parent then
                        tbl_45[var_861] = true;
                    end;
                end;
            end;
            for var_862, var_863 in next, local_player_5.Character:GetChildren() do
                if var_863:GetAttribute("verifiedFruit") then
                    local num_80 = 0;
                    repeat
                        task.wait(0.1);
                        num_80 = num_80 + 0.1;
                        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("FruitStorage"):InvokeServer(true);
                    until not var_863 or not var_863.Parent or num_80 > 2;
                    if var_863 and var_863.Parent then
                        tbl_45[var_863] = true;
                    end;
                end;
            end;
        end;
    end;
    return ;
end;
getgenv().halloween_settings = { Enabled = false };
local var_864 = player;
local tbl_46 = {};
local tbl_47 = {};
local tbl_48 = {};
getgenv().config = { KAITUN = { ENABLED = false }, Halloween = {}, Player = {} };
local func_92 = function()
    local flag_67 = nil;
    local num_81 = math.huge;
    for var_865, var_866 in next, workspace.Islands.Spooksville.Building:GetChildren() do
        if not table.find(tbl_48, var_866) then
            local var_867 = (var_866:GetPivot().Position - player_character:GetPivot().Position).Magnitude;
            if var_867 < num_81 then
                flag_67 = var_866;
                num_81 = var_867;
            end;
        end;
    end;
    return flag_67;
end;
local func_93 = function()
    local flag_68 = nil;
    local num_82 = math.huge;
    for var_868, var_869 in next, workspace.Islands:GetChildren() do
        local var_870 = (var_869:GetPivot().Position - player_character:GetPivot().Position).Magnitude;
        if var_870 < num_82 then
            flag_68 = var_869;
            num_82 = var_870;
        end;
    end;
    return flag_68;
end;
local tbl_49 = { ["Candy Corn Basket"] = 250, ["Pumpkin Basket"] = 100, ["Pumpkin Bag"] = 0 };
local func_94 = function(arg_265)
    for var_871, var_872 in next, var_864.Backpack:GetChildren() do
        if var_872.Name == arg_265 then
            return var_872;
        end;
    end;
    if player_character then
        for var_873, var_874 in next, player_character:GetChildren() do
            if var_874.Name == arg_265 then
                return var_874;
            end;
        end;
    end;
    return nil;
end;
local func_95 = function()
    local flag_69 = nil;
    local var_875 = -math.huge;
    local flag_70 = nil;
    for key_77, value_79 in pairs(tbl_49) do
        local var_876 = func_94(key_77);
        local flag_71 = false;
        if not var_876 then
            local child_46 = game.ReplicatedStorage:FindFirstChild("Stats" .. var_864.Name);
            if child_46 and string.find(child_46.Inventory.Inventory.Value, key_77) then
                flag_71 = true;
            end;
        end;
        if (var_876 or flag_71) and var_875 < value_79 then
            var_875 = value_79;
            flag_70 = key_77;
            flag_69 = var_876;
        end;
    end;
    if flag_70 and not flag_69 then
        game:GetService("ReplicatedStorage").Events.Tools:InvokeServer("equip", flag_70);
        task.wait(0.5);
        flag_69 = func_94(flag_70);
    end;
    return flag_69;
end;
if workspace:FindFirstChild("P") then
    workspace:FindFirstChild("P"):Destroy();
end;
local func_96 = function(arg_266)
    getgenv().platform = arg_266;
    if not player_character then
        return ;
    end;
    if not player_character:FindFirstChild(var) then
        return ;
    end;
    if not getgenv().config.Player.USERPLATFORM then
        local var_877 = Instance.new("Part");
        var_877.Parent = workspace;
        var_877.Anchored = true;
        var_877.Name = "P";
        var_877.Material = Enum.Material.ForceField;
        var_877.Size = Vector3.new(10, 0.1, 10);
        var_877.CanCollide = false;
        getgenv().config.Player.USERPLATFORM = var_877;
    end;
    if arg_266 then
        getgenv().config.Player.USERPLATFORM.CanCollide = true;
        tbl_47.Platform = task.spawn(function()
            while task.wait() do
                local child_47 = player_character:FindFirstChild(var);
                if child_47 then
                    getgenv().config.Player.USERPLATFORM.CFrame = CFrame.new(child_47.Position.X, 34, child_47.Position.Z);
                    child_47.CFrame = CFrame.new(getgenv().config.Player.USERPLATFORM.Position + Vector3.new(0, 3, 0));
                end;
            end;
            return ;
        end);
    else
        getgenv().config.Player.USERPLATFORM.CanCollide = false;
        if tbl_47.Platform then
            task.cancel(tbl_47.Platform);
            tbl_47.Platform = nil;
        end;
    end;
    return ;
end;
local game_31 = game:GetService("TweenService");
local func_97 = function(arg_267, arg_268, arg_269)
    tbl_47.Tween = task.spawn(function()
        local var_878 = player_character and player_character:FindFirstChild(var);
        local var_879 = player_character and player_character:FindFirstChild("Humanoid");
        if not var_878 or not var_879 then
            return ;
        end;
        if tbl_47.Tween then
            task.cancel(tbl_47.Tween);
            tbl_47.Tween = nil;
        end;
        local var_880 = not arg_269 and arg_267 or CFrame.new(arg_267.Position.X, 36, arg_267.Position.Z);
        local var_881 = (var_878.Position - var_880.Position).Magnitude / (arg_268 or 40);
        local tween_15 = game_31:Create(var_878, TweenInfo.new(var_881, Enum.EasingStyle.Linear), { CFrame = var_880 });
        tween_15:Play();
        tween_15.Completed:Wait();
        return ;
    end);
    return ;
end;
tbl2 = { ["Race Reroll x5"] = 25, ["Legendary Fruit Chest Blueprint"] = 100, ["Mummy Wrappings"] = 100, ["Custom Spirit Color"] = 50, ["Devil Fruit Journal"] = 125, ["Devil Fruit Remover"] = 25, ["Joker Costume"] = 100, Lantern = 50, ["Blood Scythe"] = 500, ["Plague Doctor Costume"] = 100, ["Trading Sign"] = 100, ["SP Reset Essence"] = 10, ["Fruit Bag"] = 250, ["Ghost Face Costume"] = 100, ["Rare Fruit Chest"] = 250 };
tblnames = {};
for var_882, var_883 in next, tbl2, nil do
    table.insert(tblnames, var_882);
end;
halloween_section.create_toggle({ title = "Auto Farm Halloween", desc = "Auto Knocks on doors to farm candy (fully automatic)", default = false }, function(arg_270)
    getgenv().halloween_settings.Enabled = arg_270;
    task.spawn(function()
        while getgenv().halloween_settings.Enabled and task.wait(1) do
            fake_geppo();
        end;
        return ;
    end);
    if arg_270 and not has_geppo() then
        library.create_noti({ title = "Uh Oh!", desc = "You need geppo to use this!", show_time = 4 });
        return ;
    end;
    if not arg_270 then
        func_96(false);
        return ;
    end;
    pcall(function()
        repeat
            task.wait();
            if player.Character.Humanoid.Health / player.Character.Humanoid.MaxHealth < 0.3 then
                repeat
                    task.wait();
                until player.Character.Humanoid.Health / player.Character.Humanoid.MaxHealth > 0.3;
            end;
            local var_884 = func_95();
            if var_884 and var_884.Parent ~= player.Character then
                player.Character.Humanoid:EquipTool(var_884);
            end;
            if func_93().Name ~= "Spooksville" then
                func_96(false);
                horo_tp_pro({
                    Position = Vector3.new(-7465.7002, 100, -589.25),
                    Speed = 40,
                    Stop = function()
                        return getgenv().halloween_settings.Enabled;
                    end
                });
                task.wait(10);
            else
                func_96(true);
                local child_48 = game.ReplicatedStorage:FindFirstChild("Stats" .. player.Name) and game.ReplicatedStorage["Stats" .. player.Name].Inventory:FindFirstChild("Halloween25Candy") and game.ReplicatedStorage["Stats" .. player.Name].Inventory.Halloween25Candy.Value;
                local var_885 = var_884 and require(game.ReplicatedStorage.Modules.ToolDesc)[var_884.Name].MaxCandy or 0;
                local var_886, var_887 = tonumber(child_48), tonumber(var_885);
                local var_888 = var_887;
                pcall(function()
                    if var_888 <= var_886 then
                        for var_889, var_890 in next, player.PlayerGui.HalloweenShop.Main.List.Bundles:GetChildren() do
                            if var_890:IsA("ImageButton") then
                                local child_49 = var_890:FindFirstChild("Price", true).Text;
                                if tonumber(child_49) <= var_888 then
                                    game:GetService("ReplicatedStorage"):WaitForChild("HalloweenShopRemote"):InvokeServer(var_890.Name);
                                end;
                            end;
                        end;
                        task.wait(3);
                        return ;
                    end;
                    return ;
                end);
                local var_891 = func_92();
                if not var_891 then
                    tbl_48 = {};
                else
                    local child_50 = var_891:FindFirstChild("eventDoor");
                    if (child_50:GetPivot().Position - player.Character:GetPivot().Position).Magnitude > 200 then
                        horo_tp_pro({
                            Position = child_50:GetPivot().Position + Vector3.new(0, 5, 0),
                            Speed = 40,
                            Stop = function()
                                return getgenv().halloween_settings.Enabled;
                            end
                        });
                    else
                        func_97(CFrame.new(child_50:GetPivot().Position), 30, true);
                    end;
                    local var_892 = child_50:GetPivot();
                    local var_893 = child_50:FindFirstChildWhichIsA("ProximityPrompt", true);
                    repeat
                        task.wait();
                    until (child_50:GetPivot().Position - player.Character:GetPivot().Position).Magnitude < 15 or not getgenv().halloween_settings.Enabled;
                    repeat
                        task.wait(0.5);
                        if var_893.Enabled then
                            fireproximityprompt(var_893);
                        end;
                    until child_50:GetPivot() ~= var_892 or not getgenv().halloween_settings.Enabled or player.PlayerGui.Notifications.Frame:FindFirstChild("TextLabel");
                    task.wait(1.5);
                    if workspace.CurrentCamera.CameraType == Enum.CameraType.Scriptable then
                        repeat
                            task.wait();
                        until workspace.CurrentCamera.CameraType ~= Enum.CameraType.Scriptable;
                    end;
                    for var_894, var_895 in next, player.PlayerGui.Notifications.Frame:GetChildren() do
                        if var_895:IsA("TextLabel") then
                            var_895:Destroy();
                        end;
                    end;
                    table.insert(tbl_48, var_891);
                end;
            end;
        until not getgenv().halloween_settings.Enabled or not player.Character:FindFirstChild(var);
        func_96(false);
        return ;
    end);
    return ;
end);
local tbl_50 = {};
local func_98 = function()
    for var_896, var_897 in next, workspace.Islands.Spooksville.Building:GetChildren() do
        if var_897:FindFirstChild("eventDoor", true) and not tbl_50[var_897] then
            return var_897:FindFirstChild("eventDoor", true);
        end;
    end;
    return nil;
end;
local func_99 = function(arg_271)
    local var_898 = func_95();
    local child_51 = game.ReplicatedStorage:FindFirstChild("Stats" .. var_864.Name) and game.ReplicatedStorage["Stats" .. var_864.Name].Inventory:FindFirstChild("Halloween25Candy") and game.ReplicatedStorage["Stats" .. var_864.Name].Inventory.Halloween25Candy.Value;
    local var_899 = var_898 and require(game.ReplicatedStorage.Modules.ToolDesc)[var_898.Name].MaxCandy or 0;
    local var_900, var_901 = tonumber(child_51), tonumber(var_899);
    if var_901 <= var_900 then
        for var_902, var_903 in next, var_864.PlayerGui.HalloweenShop.Main.List.Bundles:GetChildren() do
            if var_903:IsA("ImageButton") then
                local child_52 = var_903:FindFirstChild("Price", true).Text;
                if var_901 <= tonumber(child_52) then
                    game:GetService("ReplicatedStorage"):WaitForChild("HalloweenShopRemote"):InvokeServer(var_903.Name);
                end;
            end;
        end;
        task.wait(1);
    end;
    return ;
end;
local func_100 = function()
    return workspace.Islands["Rose Kingdom"].Factory.FrontDoor.Model.Center.CFrame.Position.Y > 120;
end;
getgenv().halloween_buyables = {};
halloween_section.create_toggle({ title = "Auto Buy Halloween", desc = "Automaticlaly purchases selected items with candy", default = false }, function(arg_272)
    getgenv().auto_buy = arg_272;
    while getgenv().auto_buy and task.wait() do
        for var_904, var_905 in next, getgenv().halloween_buyables, nil do
            if tbl2[var_905] <= func_37() then
                game:GetService("ReplicatedStorage"):WaitForChild("HalloweenShopRemote"):InvokeServer(var_905);
            end;
        end;
    end;
    return ;
end);
halloween_section.create_dropdown({ title = "Buyables", List = tblnames, default = {}, search = false, selected = true }, function(arg_273, arg_274)
    local var_906 = getgenv().halloween_buyables;
    if arg_274 then
        if not table.find(var_906, arg_273) then
            table.insert(var_906, arg_273);
        end;
    else
        for key_78, value_80 in ipairs(var_906) do
            if value_80 == arg_273 then
                table.remove(var_906, key_78);
                return ;
            end;
        end;
    end;
    return ;
end);
getgenv().auto_stats_table = {};
local tbl_51 = {};
task.spawn(pcall, function()
    for var_907, var_908 in next, game:GetService("Players").LocalPlayer.PlayerGui.Statistics.Main.Stats:GetChildren() do
        if var_908:IsA("ImageLabel") then
            table.insert(tbl_51, var_908:FindFirstChild("Stat", true).Text);
        end;
    end;
    return ;
end);
getgenv().auto_stats = {};
getgenv().auto_stats_table = {};
for var_909, var_910 in next, tbl_51, nil do
    getgenv().auto_stats[var_910] = 800;
end;
task.spawn(pcall, function()
    for var_911, var_912 in next, tbl_51, nil do
        stats_section.create_toggle({
            title = "Auto " .. var_912 .. " Stat",
            desc = "Automatically Puts Stat Points Into " .. var_912,
            default = false,
            Textbox = true,
            TextboxPlaceholder = "Max Stat",
            TextboxDefault = tostring(getgenv().auto_stats[var_912]),
            TextboxCallback = function(arg_275)
                local num_83 = tonumber(arg_275);
                if num_83 then
                    getgenv().auto_stats[var_912] = num_83;
                    warn(var_912 .. " stat set to:", num_83);
                else
                    warn("Invalid number for " .. var_912);
                end;
                return ;
            end
        }, function(arg_276)
            if arg_276 then
                table.insert(getgenv().auto_stats_table, var_912);
                warn("Auto " .. var_912 .. " enabled");
            else
                local var_913 = table.find(getgenv().auto_stats_table, var_912);
                if var_913 then
                    table.remove(getgenv().auto_stats_table, var_913);
                    warn("Auto " .. var_912 .. " disabled");
                end;
            end;
            return ;
        end);
    end;
    return ;
end);
getgenv().stat_amount = 1;
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            local game_32 = game:GetService("Players").LocalPlayer.PlayerGui.Statistics.Main.TopOptions.AvailableSP;
            if (tonumber(string.match(game_32.Text, "%d+")) or 0) > 0 then
                for var_914, var_915 in next, game:GetService("Players").LocalPlayer.PlayerGui.Statistics.Main.Stats:GetChildren() do
                    if var_915:IsA("ImageLabel") then
                        local child_53 = var_915:FindFirstChild("Stat", true);
                        local child_54 = var_915:FindFirstChild("Amount", true);
                        if child_53 and child_54 then
                            local var_916 = child_53.Text;
                            local num_84 = tonumber(child_54.Text) or 0;
                            if table.find(getgenv().auto_stats_table, var_916) and num_84 < (getgenv().auto_stats[var_916] or 800) then
                                firesignal(var_915:FindFirstChild("Add").MouseButton1Click);
                            end;
                        end;
                    end;
                end;
            end;
            return ;
        end);
    end;
    return ;
end);
stats_section.create_slider({ title = "Stat Amount", min = 1, max = 100, default = 1, precise = true }, function(arg_277)
    getgenv().stat_amount = arg_277;
    return ;
end);
getgenv().no_dash = false;
if not getgenv().namecall then
    getgenv().namecall = true;
    local flag_72 = nil;
    flag_72 = hookmetamethod(game, "__namecall", (interpreter_proxy[1][34][5](function(arg_278, ...)
        if not checkcaller() and (getnamecallmethod() == "FireServer" and arg_278.Name == "takestam" and getgenv().no_dash) then
            return ;
        end;
        return flag_72(arg_278, ...);
    end)));
end;
local func_101 = function(arg_279)
    local var_917 = inventory_value and inventory_value.Value;
    if not var_917 then
        return 0;
    end;
    local var_918, var_919 = pcall(function()
        return game:GetService("HttpService"):JSONDecode(var_917);
    end);
    if not var_918 then
        return 0;
    end;
    return var_919[arg_279] or 0;
end;
get_stock = function(arg_280)
    local game_33 = game:GetService("Players").LocalPlayer.PlayerGui.MerchentShop.Main.List.Redeemables;
    if game_33:FindFirstChild(arg_280) then
        local child_55 = game_33:FindFirstChild(arg_280):FindFirstChild("Amount", true).Text;
        return tonumber(child_55:match("(%d+)/%d+"));
    end;
    return 0;
end;
has_bag = function()
    local var_920 = func_101("Fruit Bag");
    if func_101("Spare Fruit Bag") > 0 and var_920 < 1 then
        return true;
    end;
    return false;
end;
equip_shit = function(arg_281)
    if getgenv().busy then
        return ;
    end;
    if func_71(arg_281) and func_71(arg_281).Parent ~= player.Character then
        player.Character.Humanoid:UnequipTools();
        task.wait(1);
        func_71(arg_281).Parent = player.Character;
        task.wait(1);
    end;
    return ;
end;
exploit_section.create_toggle({ title = "Auto Use/Buy Fruit Bag", desc = "Uses/Buys Spare Fruit Bag When Available", default = false }, function(arg_282)
    getgenv().auto_use = arg_282;
    while getgenv().auto_use and task.wait() do
        if not getgenv().fish_kaitun.Enabled then
            if has_bag() then
                getgenv().busy = true;
                repeat
                    task.wait();
                    local var_921 = func_71("Spare Fruit Bag");
                    if var_921 then
                        if var_921.Parent ~= player.Character then
                            player.Character.Humanoid:UnequipTools();
                            task.wait(0.3);
                            var_921.Parent = player.Character;
                            task.wait(0.3);
                        elseif not player.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                            var_921:Activate();
                            if var_921:FindFirstChild("ConfirmationPrompt") then
                                var_921:FindFirstChild("ConfirmationPrompt").Parent = player.PlayerGui;
                            end;
                        else
                            game:GetService("GuiService").SelectedObject = player.PlayerGui:FindFirstChild("ConfirmationPrompt").Main.Accept;
                            virtual_input_manager:SendKeyEvent(true, Enum.KeyCode.Return, false, nil);
                            virtual_input_manager:SendKeyEvent(false, Enum.KeyCode.Return, false, nil);
                        end;
                    else
                        tools_event:InvokeServer("equip", "Spare Fruit Bag");
                    end;
                until not var_921 or not var_921.Parent or not getgenv().fish_kaitun.Enabled or func_101("Fruit Bag") > 0;
                game:GetService("GuiService").SelectedObject = nil;
                getgenv().busy = false;
            elseif has_merchant() and stats_folder.Skills:FindFirstChild("skyWalk") and stats_folder.Skills.sky_walk.Value then
                if not func_68(has_merchant(), 10, true) then
                    func_75(has_merchant());
                elseif not player.PlayerGui:FindFirstChild("MerchentShop") then
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("TravelingMerchentRemote"):InvokeServer("OpenShop");
                elseif get_stock("Spare Fruit Bag") and get_stock("Spare Fruit Bag") > 0 then
                    events.TravelingMerchentRemote:InvokeServer("Spare Fruit Bag", player.PlayerGui:FindFirstChild("MerchentShop"):GetAttribute("Seed"));
                end;
            end;
        end;
    end;
    return ;
end);
exploit_section.create_toggle({ title = "No Dash Take Stamina", desc = "Prevents the game from taking your stamina on dash", default = false }, function(arg_283)
    getgenv().no_dash = arg_283;
    return ;
end);
local flag_73 = false;
exploit_section.create_toggle({ title = "Fake Geppo", desc = "Gives you fake geppo (Anticheat still can flag you)", default = false }, function(arg_284)
    if arg_284 then
        if stats_folder.Skills.sky_walk.Value ~= true then
            flag_73 = true;
            stats_folder.Skills.sky_walk.Value = true;
        end;
    elseif flag_73 then
        stats_folder.Skills.sky_walk.Value = false;
    end;
    return ;
end);
exploit_section.create_toggle({ title = "Streamer Mode", desc = "Replaces usernames", default = false }, function(arg_285)
    getgenv().replace = arg_285;
    if not arg_285 then
        pcall(function()
            if game:GetService("Players").LocalPlayer.PlayerGui.Playerlist.Main.ScrollingFrame.Pirate.Container:FindFirstChild(player.Name) then
                game:GetService("Players").LocalPlayer.PlayerGui.Playerlist.Main.ScrollingFrame.Pirate.Container:FindFirstChild(player.Name).Visible = true;
            end;
            game:GetService("Players").LocalPlayer.PlayerGui.Display.Enabled = true;
            return ;
        end);
    end;
    while getgenv().replace and task.wait(1) do
        pcall(function()
            if player.PlayerGui.Playerlist and player.PlayerGui.Playerlist.Enabled then
                player.PlayerGui.Playerlist.Enabled = false;
            end;
            player.PlayerGui.HealthBars:FindFirstChild(player.Name).NameT.Text = "gg/feral";
            return ;
        end);
        pcall(function()
            if game:GetService("Players").LocalPlayer.PlayerGui.Playerlist.Main.ScrollingFrame.Pirate.Container:FindFirstChild(player.Name) then
                game:GetService("Players").LocalPlayer.PlayerGui.Playerlist.Main.ScrollingFrame.Pirate.Container:FindFirstChild(player.Name).Visible = false;
            end;
            return ;
        end);
        pcall(function()
            if game:GetService("Players").LocalPlayer.PlayerGui.Display.Enabled then
                game:GetService("Players").LocalPlayer.PlayerGui.Display.Enabled = false;
            end;
            return ;
        end);
    end;
    return ;
end);
exploit_section.create_toggle({ title = "Optimize Game", desc = "Applies optimizations :)", default = false }, function(arg_286)
    getgenv().optimize = arg_286;
    if arg_286 then
        local game_34 = game:GetService("Lighting");
        local game_35 = game:GetService("Workspace");
        local game_36 = game:GetService("RunService");
        local game_37 = game:GetService("Players");
        local game_38 = game:GetService("StarterGui");
        local game_39 = game:GetService("ReplicatedStorage");
        local game_40 = game:GetService("ReplicatedFirst");
        local game_41 = game:GetService("SoundService");
        local game_42 = game:GetService("CoreGui");
        local game_43 = game:GetService("Debris");
        local game_44 = game:GetService("TweenService");
        local var_922 = game_35:FindFirstChildOfClass("Terrain");
        local var_923 = game_35.CurrentCamera;
        local var_924 = game_37.LocalPlayer;
        local var_925 = sethiddenproperty or set_hidden_property or set_hidden_prop;
        local var_926 = gethiddenproperty or get_hidden_property or get_hidden_prop;
        local var_927 = getgc or get_gc;
        local var_928 = getconnections or get_connections;
        local var_929 = getnilinstances or get_nil_instances;
        local var_930 = getinstances or get_instances;
        if not getloadedmodules then
            local var_931 = get_loaded_modules;
        end;
        if not getscripts then
            local var_932 = get_scripts;
        end;
        if not getrunningscripts then
            local var_933 = get_running_scripts;
        end;
        if not firesignal then
            local var_934 = fire_signal;
        end;
        if not fireclickdetector then
            local var_935 = fire_click_detector;
        end;
        local var_936 = setrenderproperty or set_render_property;
        if not getrenderproperty then
            local var_937 = get_render_property;
        end;
        if not cloneref then
            local var_938 = clone_ref;
        end;
        if not gethui then
            local var_939 = get_hidden_ui;
        end;
        if not isreadonly then
            local var_940 = is_readonly;
        end;
        if not setreadonly then
            local var_941 = set_readonly;
        end;
        if not getrawmetatable then
            local var_942 = get_raw_metatable;
        end;
        if not setrawmetatable then
            local var_943 = set_raw_metatable;
        end;
        if not hookfunction and not hookfunc and not replaceclosure then
            local var_944 = detour_function;
        end;
        if not newcclosure then
            local var_945 = new_cclosure;
        end;
        if not islclosure then
            local var_946 = is_lclosure;
        end;
        local var_947 = getinfo or debug.getinfo;
        if not getconstants and not debug.getconstants then
            local var_948 = get_constants;
        end;
        if not getupvalues and not debug.getupvalues then
            local var_949 = get_upvalues;
        end;
        if not setupvalue and not debug.setupvalue then
            local var_950 = set_upvalue;
        end;
        if not getgenv().optimize_data then
            getgenv().optimize_data = { Lighting = {}, Terrain = {}, workspace_service = {}, camera = {}, Effects = {}, Parts = {}, Meshes = {}, Textures = {}, Decals = {}, SurfaceAppearances = {}, Materials = {}, Connections = {}, Sounds = {}, Guis = {}, NilParented = {}, Scripts = {}, Humanoids = {} };
        end;
        local var_951 = getgenv().optimize_data;
        local tbl_52 = { falling_particle = true, Lines = true, Lines2 = true };
        local func_102 = function(arg_287)
            if not arg_287 then
                return true;
            end;
            if tbl_52[arg_287.Name] then
                return true;
            end;
            local var_952 = arg_287.Parent;
            while var_952 do
                if tbl_52[var_952.Name] then
                    return true;
                end;
                var_952 = var_952.Parent;
            end;
            return false;
        end;
        var_951.Lighting = { GlobalShadows = game_34.GlobalShadows, FogEnd = game_34.FogEnd, FogStart = game_34.FogStart, FogColor = game_34.FogColor, Brightness = game_34.Brightness, Ambient = game_34.Ambient, OutdoorAmbient = game_34.OutdoorAmbient, ClockTime = game_34.ClockTime, GeographicLatitude = game_34.GeographicLatitude, EnvironmentDiffuseScale = game_34.EnvironmentDiffuseScale, EnvironmentSpecularScale = game_34.EnvironmentSpecularScale, ShadowSoftness = game_34.ShadowSoftness, ColorShift_Bottom = game_34.ColorShift_Bottom, ColorShift_Top = game_34.ColorShift_Top, ExposureCompensation = game_34.ExposureCompensation };
        game_34.GlobalShadows = false;
        game_34.FogEnd = 9000000000;
        game_34.FogStart = 9000000000;
        game_34.Brightness = 2;
        game_34.ClockTime = 14;
        game_34.GeographicLatitude = 0;
        game_34.EnvironmentDiffuseScale = 0;
        game_34.EnvironmentSpecularScale = 0;
        game_34.ShadowSoftness = 0;
        game_34.ExposureCompensation = 0;
        game_34.Ambient = Color3.fromRGB(178, 178, 178);
        game_34.OutdoorAmbient = Color3.fromRGB(178, 178, 178);
        pcall(function()
            var_951.Lighting.Technology = var_926(game_34, "Technology");
            var_925(game_34, "Technology", 2);
            return ;
        end);
        pcall(function()
            var_951.Lighting.GlobalIllumination = var_926(game_34, "GlobalIllumination");
            var_925(game_34, "GlobalIllumination", 0);
            return ;
        end);
        pcall(function()
            var_925(game_34, "LegacyOutlines", false);
            return ;
        end);
        pcall(function()
            var_925(game_34, "Outlines", false);
            return ;
        end);
        for key_79, value_81 in ipairs(game_34:GetChildren()) do
            if value_81:IsA("BlurEffect") or value_81:IsA("BloomEffect") or value_81:IsA("SunRaysEffect") or value_81:IsA("ColorCorrectionEffect") or value_81:IsA("DepthOfFieldEffect") or value_81:IsA("Atmosphere") or value_81:IsA("Clouds") or value_81:IsA("Sky") then
                var_951.Effects[value_81] = { Parent = game_34, ClassName = value_81.ClassName };
                value_81.Parent = nil;
                table.insert(var_951.NilParented, value_81);
            end;
        end;
        if var_922 then
            var_951.Terrain = { WaterWaveSize = var_922.WaterWaveSize, WaterWaveSpeed = var_922.WaterWaveSpeed, WaterReflectance = var_922.WaterReflectance, WaterTransparency = var_922.WaterTransparency, WaterColor = var_922.WaterColor };
            var_922.WaterWaveSize = 0;
            var_922.WaterWaveSpeed = 0;
            var_922.WaterReflectance = 0;
            var_922.WaterTransparency = 1;
            var_922.WaterColor = Color3.new(0.3, 0.3, 0.3);
            pcall(function()
                var_951.Terrain.Decoration = var_926(var_922, "Decoration");
                var_925(var_922, "Decoration", false);
                return ;
            end);
            pcall(function()
                var_951.Terrain.GrassLength = var_926(var_922, "GrassLength");
                var_925(var_922, "GrassLength", 0);
                return ;
            end);
            pcall(function()
                var_925(var_922, "MaterialColors", "");
                return ;
            end);
        end;
        for key_80, value_82 in ipairs({ "InterpolationThrottling", "PhysicsSteppingMethod", "HumanoidOnlySetCollisionsOnStateChange", "MeshPartHeadsAndAccessories", "ClientAnimatorThrottling", "StreamingMinRadius", "StreamingTargetRadius", "StreamingIntegrityMode", "ExpSolverEnabled_Replicate", "RejectCharacterDeletions", "TouchesUseCollisionGroups" }) do
            pcall(function()
                var_951.Workspace[value_82] = var_926(game_35, value_82);
                return ;
            end);
        end;
        pcall(function()
            var_925(game_35, "InterpolationThrottling", 0);
            return ;
        end);
        pcall(function()
            var_925(game_35, "PhysicsSteppingMethod", 0);
            return ;
        end);
        pcall(function()
            var_925(game_35, "HumanoidOnlySetCollisionsOnStateChange", 2);
            return ;
        end);
        pcall(function()
            var_925(game_35, "MeshPartHeadsAndAccessories", 1);
            return ;
        end);
        pcall(function()
            var_925(game_35, "ClientAnimatorThrottling", 0);
            return ;
        end);
        if game_35.StreamingEnabled then
            pcall(function()
                var_925(game_35, "StreamingMinRadius", 16);
                return ;
            end);
            pcall(function()
                var_925(game_35, "StreamingTargetRadius", 64);
                return ;
            end);
        end;
        pcall(function()
            var_951.Sounds.Service = { AmbientReverb = game_41.AmbientReverb, DistanceFactor = game_41.DistanceFactor, DopplerScale = game_41.DopplerScale, RolloffScale = game_41.RolloffScale, VolumetricAudio = game_41.VolumetricAudio };
            game_41.AmbientReverb = Enum.ReverbType.NoReverb;
            game_41.DopplerScale = 0;
            game_41.RolloffScale = 0;
            game_41.VolumetricAudio = Enum.VolumetricAudio.Disabled;
            return ;
        end);
        local flag_74 = nil;
        flag_74 = function(arg_288)
            if not arg_288 or not arg_288.Parent then
                return ;
            end;
            if func_102(arg_288) then
                return ;
            end;
            local result_2 = pcall(function()
                local var_953 = arg_288.ClassName;
                if var_953 == "Texture" then
                    if not var_951.Textures[arg_288] then
                        var_951.Textures[arg_288] = { Parent = arg_288.Parent, Texture = arg_288.Texture, Transparency = arg_288.Transparency, StudsPerTileU = arg_288.StudsPerTileU, StudsPerTileV = arg_288.StudsPerTileV, OffsetStudsU = arg_288.OffsetStudsU, OffsetStudsV = arg_288.OffsetStudsV, Color3 = arg_288.Color3, Face = arg_288.Face };
                    end;
                    arg_288.Texture = "";
                    arg_288.Transparency = 1;
                    arg_288.StudsPerTileU = 9000000000;
                    arg_288.StudsPerTileV = 9000000000;
                    pcall(function()
                        var_925(arg_288, "LocalTransparencyModifier", 1);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "Shiny", 0);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "Specular", 0);
                        return ;
                    end);
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Decal" then
                    if not var_951.Decals[arg_288] then
                        var_951.Decals[arg_288] = { Parent = arg_288.Parent, Texture = arg_288.Texture, Transparency = arg_288.Transparency, Color3 = arg_288.Color3, Face = arg_288.Face, ZIndex = arg_288.ZIndex };
                    end;
                    arg_288.Texture = "";
                    arg_288.Transparency = 1;
                    pcall(function()
                        var_925(arg_288, "LocalTransparencyModifier", 1);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "Shiny", 0);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "Specular", 0);
                        return ;
                    end);
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "SurfaceAppearance" then
                    if not var_951.SurfaceAppearances[arg_288] then
                        var_951.SurfaceAppearances[arg_288] = { Parent = arg_288.Parent, ColorMap = arg_288.ColorMap, MetalnessMap = arg_288.MetalnessMap, NormalMap = arg_288.NormalMap, RoughnessMap = arg_288.RoughnessMap, TexturePack = arg_288.TexturePack, AlphaMode = arg_288.AlphaMode };
                    end;
                    arg_288.ColorMap = "";
                    arg_288.MetalnessMap = "";
                    arg_288.NormalMap = "";
                    arg_288.RoughnessMap = "";
                    arg_288.TexturePack = "";
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "MaterialVariant" then
                    if not var_951.Materials[arg_288] then
                        var_951.Materials[arg_288] = { Parent = arg_288.Parent, ColorMap = arg_288.ColorMap, MetalnessMap = arg_288.MetalnessMap, NormalMap = arg_288.NormalMap, RoughnessMap = arg_288.RoughnessMap };
                    end;
                    arg_288.ColorMap = "";
                    arg_288.MetalnessMap = "";
                    arg_288.NormalMap = "";
                    arg_288.RoughnessMap = "";
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "ParticleEmitter" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Rate = arg_288.Rate, Texture = arg_288.Texture, Lifetime = arg_288.Lifetime, Speed = arg_288.Speed, Size = arg_288.Size, Transparency = arg_288.Transparency, LightEmission = arg_288.LightEmission, LightInfluence = arg_288.LightInfluence, SpreadAngle = arg_288.SpreadAngle, Drag = arg_288.Drag, Acceleration = arg_288.Acceleration, RotSpeed = arg_288.RotSpeed, Rotation = arg_288.Rotation, LockedToPart = arg_288.LockedToPart };
                    end;
                    arg_288.Enabled = false;
                    arg_288.Rate = 0;
                    arg_288.Texture = "";
                    arg_288.Lifetime = NumberRange.new(0);
                    arg_288.Speed = NumberRange.new(0);
                    arg_288.Size = NumberSequence.new(0);
                    arg_288.Transparency = NumberSequence.new(1);
                    arg_288.LightEmission = 0;
                    arg_288.LightInfluence = 0;
                    arg_288.Drag = 9999;
                    pcall(function()
                        var_925(arg_288, "LocalTransparencyModifier", 1);
                        return ;
                    end);
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Trail" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Lifetime = arg_288.Lifetime, Texture = arg_288.Texture, LightEmission = arg_288.LightEmission, LightInfluence = arg_288.LightInfluence, Transparency = arg_288.Transparency, Color = arg_288.Color, WidthScale = arg_288.WidthScale, MinLength = arg_288.MinLength, MaxLength = arg_288.MaxLength, FaceCamera = arg_288.FaceCamera };
                    end;
                    arg_288.Enabled = false;
                    arg_288.Lifetime = 0;
                    arg_288.Texture = "";
                    arg_288.LightEmission = 0;
                    arg_288.LightInfluence = 0;
                    arg_288.Transparency = NumberSequence.new(1);
                    arg_288.MinLength = 9000000000;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Beam" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Texture = arg_288.Texture, LightEmission = arg_288.LightEmission, LightInfluence = arg_288.LightInfluence, Transparency = arg_288.Transparency, Color = arg_288.Color, Width0 = arg_288.Width0, Width1 = arg_288.Width1, FaceCamera = arg_288.FaceCamera, Segments = arg_288.Segments, TextureLength = arg_288.TextureLength, TextureSpeed = arg_288.TextureSpeed };
                    end;
                    arg_288.Enabled = false;
                    arg_288.Texture = "";
                    arg_288.LightEmission = 0;
                    arg_288.LightInfluence = 0;
                    arg_288.Transparency = NumberSequence.new(1);
                    arg_288.Width0 = 0;
                    arg_288.Width1 = 0;
                    arg_288.Segments = 1;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Fire" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Size = arg_288.Size, Heat = arg_288.Heat, Color = arg_288.Color, SecondaryColor = arg_288.SecondaryColor, TimeScale = arg_288.TimeScale };
                    end;
                    arg_288.Enabled = false;
                    arg_288.Size = 0;
                    arg_288.Heat = 0;
                    arg_288.TimeScale = 0;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Smoke" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Size = arg_288.Size, RiseVelocity = arg_288.RiseVelocity, Opacity = arg_288.Opacity, Color = arg_288.Color, TimeScale = arg_288.TimeScale };
                    end;
                    arg_288.Enabled = false;
                    arg_288.Size = 0;
                    arg_288.Opacity = 0;
                    arg_288.RiseVelocity = 0;
                    arg_288.TimeScale = 0;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Sparkles" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, SparkleColor = arg_288.SparkleColor, TimeScale = arg_288.TimeScale };
                    end;
                    arg_288.Enabled = false;
                    arg_288.TimeScale = 0;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Highlight" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, FillTransparency = arg_288.FillTransparency, OutlineTransparency = arg_288.OutlineTransparency, FillColor = arg_288.FillColor, OutlineColor = arg_288.OutlineColor, DepthMode = arg_288.DepthMode };
                    end;
                    arg_288.Enabled = false;
                    arg_288.FillTransparency = 1;
                    arg_288.OutlineTransparency = 1;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "ForceField" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Visible = arg_288.Visible };
                    end;
                    arg_288.Visible = false;
                elseif var_953 == "Explosion" then
                    arg_288.BlastPressure = 0;
                    arg_288.BlastRadius = 0;
                    arg_288.Visible = false;
                    arg_288.TimeScale = 0;
                elseif arg_288:IsA("Light") then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Brightness = arg_288.Brightness, Color = arg_288.Color, Shadows = arg_288.Shadows };
                        if arg_288:IsA("PointLight") or arg_288:IsA("SpotLight") then
                            var_951.Effects[arg_288].Range = arg_288.Range;
                        end;
                        if arg_288:IsA("SpotLight") then
                            var_951.Effects[arg_288].Angle = arg_288.Angle;
                            var_951.Effects[arg_288].Face = arg_288.Face;
                        end;
                        if arg_288:IsA("SurfaceLight") then
                            var_951.Effects[arg_288].Angle = arg_288.Angle;
                            var_951.Effects[arg_288].Face = arg_288.Face;
                            var_951.Effects[arg_288].Range = arg_288.Range;
                        end;
                    end;
                    arg_288.Enabled = false;
                    arg_288.Brightness = 0;
                    arg_288.Shadows = false;
                    if arg_288:IsA("PointLight") or arg_288:IsA("SpotLight") or arg_288:IsA("SurfaceLight") then
                        arg_288.Range = 0;
                    end;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "MeshPart" then
                    if not var_951.Meshes[arg_288] then
                        var_951.Meshes[arg_288] = { RenderFidelity = arg_288.RenderFidelity, CastShadow = arg_288.CastShadow, DoubleSided = arg_288.DoubleSided, Material = arg_288.Material, Reflectance = arg_288.Reflectance, TextureID = arg_288.TextureID };
                    end;
                    arg_288.RenderFidelity = Enum.RenderFidelity.Performance;
                    arg_288.CastShadow = false;
                    arg_288.DoubleSided = false;
                    arg_288.Reflectance = 0;
                    arg_288.TextureID = "";
                    if arg_288.Material == Enum.Material.Glass or arg_288.Material == Enum.Material.Neon or arg_288.Material == Enum.Material.ForceField or arg_288.Material == Enum.Material.Water then
                        arg_288.Material = Enum.Material.SmoothPlastic;
                    end;
                    pcall(function()
                        var_951.Meshes[arg_288].CollisionFidelity = var_926(arg_288, "CollisionFidelity");
                        var_925(arg_288, "CollisionFidelity", 2);
                        return ;
                    end);
                    pcall(function()
                        var_951.Meshes[arg_288].FluidFidelity = var_926(arg_288, "FluidFidelity");
                        var_925(arg_288, "FluidFidelity", 0);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "LocalTransparencyModifier", 0);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "HasJointOffset", false);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "HasSkinnedMesh", false);
                        return ;
                    end);
                    for key_81, value_83 in ipairs(arg_288:GetChildren()) do
                        if (value_83:IsA("SurfaceAppearance") or value_83:IsA("Texture") or value_83:IsA("Decal")) and not func_102(value_83) then
                            flag_74(value_83);
                        end;
                    end;
                elseif var_953 == "UnionOperation" or var_953 == "NegateOperation" or var_953 == "IntersectOperation" then
                    if not var_951.Meshes[arg_288] then
                        var_951.Meshes[arg_288] = { CastShadow = arg_288.CastShadow, Material = arg_288.Material, Reflectance = arg_288.Reflectance, UsePartColor = arg_288.UsePartColor };
                    end;
                    arg_288.CastShadow = false;
                    arg_288.Reflectance = 0;
                    if arg_288.Material == Enum.Material.Glass or arg_288.Material == Enum.Material.Neon then
                        arg_288.Material = Enum.Material.SmoothPlastic;
                    end;
                    pcall(function()
                        var_925(arg_288, "FluidFidelity", 0);
                        return ;
                    end);
                elseif arg_288:IsA("BasePart") and not arg_288:IsA("Terrain") then
                    if not var_951.Parts[arg_288] then
                        var_951.Parts[arg_288] = { CastShadow = arg_288.CastShadow, Material = arg_288.Material, Reflectance = arg_288.Reflectance };
                    end;
                    arg_288.CastShadow = false;
                    arg_288.Reflectance = 0;
                    if arg_288.Material == Enum.Material.Glass or arg_288.Material == Enum.Material.Neon or arg_288.Material == Enum.Material.ForceField or arg_288.Material == Enum.Material.Water then
                        arg_288.Material = Enum.Material.SmoothPlastic;
                    end;
                    pcall(function()
                        var_925(arg_288, "LocalTransparencyModifier", 0);
                        return ;
                    end);
                    pcall(function()
                        var_925(arg_288, "FluidFidelity", 0);
                        return ;
                    end);
                    for key_82, value_84 in ipairs(arg_288:GetChildren()) do
                        if (value_84:IsA("Texture") or value_84:IsA("Decal") or value_84:IsA("SurfaceAppearance")) and not func_102(value_84) then
                            flag_74(value_84);
                        end;
                    end;
                elseif arg_288:IsA("DataModelMesh") then
                    if not var_951.Meshes[arg_288] then
                        var_951.Meshes[arg_288] = { Scale = arg_288.Scale, Offset = arg_288.Offset, VertexColor = arg_288.VertexColor };
                        if arg_288:IsA("SpecialMesh") or arg_288:IsA("FileMesh") then
                            var_951.Meshes[arg_288].TextureId = arg_288.TextureId;
                        end;
                    end;
                    if arg_288:IsA("SpecialMesh") or arg_288:IsA("FileMesh") then
                        arg_288.TextureId = "";
                    end;
                    arg_288.VertexColor = Vector3.one;
                elseif var_953 == "Sound" then
                    if not arg_288:IsDescendantOf(var_924) then
                        if not var_951.Sounds[arg_288] then
                            var_951.Sounds[arg_288] = { RollOffMaxDistance = arg_288.RollOffMaxDistance, RollOffMinDistance = arg_288.RollOffMinDistance, Volume = arg_288.Volume, EmitterSize = arg_288.EmitterSize };
                        end;
                        arg_288.RollOffMaxDistance = 30;
                        arg_288.RollOffMinDistance = 5;
                        arg_288.EmitterSize = 5;
                        arg_288.Volume = math.min(arg_288.Volume, 0.3);
                    end;
                elseif arg_288:IsA("SoundEffect") then
                    if not var_951.Sounds[arg_288] then
                        var_951.Sounds[arg_288] = { Parent = arg_288.Parent, Enabled = arg_288.Enabled, Priority = arg_288.Priority };
                    end;
                    arg_288.Enabled = false;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "BillboardGui" then
                    if not arg_288:IsDescendantOf(var_924) and not arg_288:IsDescendantOf(game_42) then
                        if not var_951.Guis[arg_288] then
                            var_951.Guis[arg_288] = { Enabled = arg_288.Enabled, max_distance = arg_288.max_distance, LightInfluence = arg_288.LightInfluence, Size = arg_288.Size, StudsOffset = arg_288.StudsOffset, AlwaysOnTop = arg_288.AlwaysOnTop };
                        end;
                        arg_288.max_distance = 15;
                        arg_288.LightInfluence = 0;
                        arg_288.Size = UDim2.new(0, 50, 0, 50);
                    end;
                elseif var_953 == "SurfaceGui" then
                    if not arg_288:IsDescendantOf(var_924) then
                        if not var_951.Guis[arg_288] then
                            var_951.Guis[arg_288] = { Enabled = arg_288.Enabled, max_distance = arg_288.max_distance, LightInfluence = arg_288.LightInfluence, PixelsPerStud = arg_288.PixelsPerStud, Brightness = arg_288.Brightness };
                        end;
                        arg_288.max_distance = 15;
                        arg_288.LightInfluence = 0;
                        arg_288.PixelsPerStud = 5;
                        arg_288.Brightness = 0;
                    end;
                elseif var_953 == "Humanoid" then
                    if not var_951.Humanoids[arg_288] then
                        var_951.Humanoids[arg_288] = { DisplayDistanceType = arg_288.DisplayDistanceType, HealthDisplayDistance = arg_288.HealthDisplayDistance, NameDisplayDistance = arg_288.NameDisplayDistance, HealthDisplayType = arg_288.HealthDisplayType, NameOcclusion = arg_288.NameOcclusion };
                    end;
                    arg_288.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None;
                    arg_288.HealthDisplayDistance = 0;
                    arg_288.NameDisplayDistance = 0;
                    arg_288.HealthDisplayType = Enum.HumanoidHealthDisplayType.AlwaysOff;
                    pcall(function()
                        var_925(arg_288, "DisplayName", "");
                        return ;
                    end);
                elseif var_953 == "Animator" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { PreferLodEnabled = arg_288.PreferLodEnabled };
                    end;
                    arg_288.PreferLodEnabled = true;
                elseif var_953 == "ProximityPrompt" then
                    if not var_951.Guis[arg_288] then
                        var_951.Guis[arg_288] = { MaxActivationDistance = arg_288.MaxActivationDistance };
                    end;
                    arg_288.MaxActivationDistance = math.min(arg_288.MaxActivationDistance, 8);
                elseif arg_288:IsA("Constraint") then
                    if arg_288:IsA("RopeConstraint") or arg_288:IsA("RodConstraint") or arg_288:IsA("SpringConstraint") then
                        if not var_951.Effects[arg_288] then
                            var_951.Effects[arg_288] = { Visible = arg_288.Visible, Color = arg_288.Color };
                        end;
                        arg_288.Visible = false;
                    end;
                elseif var_953 == "Wire" then
                    if not var_951.Effects[arg_288] then
                        var_951.Effects[arg_288] = { Parent = arg_288.Parent };
                    end;
                    arg_288.Parent = nil;
                    table.insert(var_951.NilParented, arg_288);
                elseif var_953 == "Attachment" then
                    pcall(function()
                        arg_288.Visible = false;
                        return ;
                    end);
                elseif var_953 == "VideoFrame" then
                    if not var_951.Guis[arg_288] then
                        var_951.Guis[arg_288] = { Playing = arg_288.Playing, Volume = arg_288.Volume };
                    end;
                    arg_288.Playing = false;
                    arg_288.Volume = 0;
                elseif var_953 == "ViewportFrame" and not arg_288:IsDescendantOf(var_924) then
                    if not var_951.Guis[arg_288] then
                        var_951.Guis[arg_288] = { Ambient = arg_288.Ambient, LightColor = arg_288.LightColor, LightDirection = arg_288.LightDirection };
                    end;
                    arg_288.Ambient = Color3.new(1, 1, 1);
                    arg_288.LightColor = Color3.new(0, 0, 0);
                end;
                return ;
            end);
            return ;
        end;
        local tbl_53 = { game_35, game_39, game_40, game_34 };
        local tbl_54 = {};
        for key_83, value_85 in ipairs(tbl_53) do
            for key_84, value_86 in ipairs(value_85:GetDescendants()) do
                table.insert(tbl_54, value_86);
            end;
        end;
        local num_85 = 750;
        for i_7 = 1, #tbl_54, num_85 do
            for i_8 = i_7, math.min(i_7 + num_85 - 1, #tbl_54), 1 do
                flag_74(tbl_54[i_8]);
            end;
            task.wait();
        end;
        if var_927 then
            task.spawn(function()
                local var_954 = var_927(true);
                for key_85, value_87 in ipairs(var_954) do
                    if typeof(value_87) == "Instance" and not func_102(value_87) then
                        pcall(function()
                            if value_87:IsA("ParticleEmitter") then
                                value_87.Enabled = false;
                                value_87.Rate = 0;
                            elseif value_87:IsA("Trail") or value_87:IsA("Beam") then
                                value_87.Enabled = false;
                            elseif value_87:IsA("Fire") or value_87:IsA("Smoke") or value_87:IsA("Sparkles") then
                                value_87.Enabled = false;
                            elseif value_87:IsA("Light") then
                                value_87.Enabled = false;
                            elseif value_87:IsA("Decal") or value_87:IsA("Texture") then
                                value_87.Transparency = 1;
                            elseif value_87:IsA("Sound") and value_87.IsPlaying and not value_87:IsDescendantOf(var_924) then
                                value_87.Volume = 0;
                            end;
                            return ;
                        end);
                    elseif typeof(value_87) == "table" and (rawget(value_87, "Particles") or rawget(value_87, "Effects") or rawget(value_87, "Trails")) then
                        pcall(function()
                            for key_86, value_88 in pairs(value_87) do
                                if typeof(value_88) == "Instance" and not func_102(value_88) and (value_88:IsA("ParticleEmitter") or value_88:IsA("Trail") or value_88:IsA("Beam")) then
                                    value_88.Enabled = false;
                                end;
                            end;
                            return ;
                        end);
                    end;
                end;
                return ;
            end);
        end;
        if var_929 then
            task.spawn(function()
                local var_955 = var_929();
                for key_87, value_89 in ipairs(var_955) do
                    if not func_102(value_89) then
                        pcall(function()
                            if value_89:IsA("Sound") and value_89.Playing then
                                value_89:Stop();
                                value_89.Volume = 0;
                            elseif value_89:IsA("ParticleEmitter") then
                                value_89.Enabled = false;
                                value_89.Rate = 0;
                            elseif value_89:IsA("Trail") or value_89:IsA("Beam") then
                                value_89.Enabled = false;
                            elseif value_89:IsA("Fire") or value_89:IsA("Smoke") or value_89:IsA("Sparkles") then
                                value_89.Enabled = false;
                            elseif value_89:IsA("VideoFrame") then
                                value_89.Playing = false;
                            end;
                            return ;
                        end);
                    end;
                end;
                return ;
            end);
        end;
        if var_930 then
            task.spawn(function()
                local var_956 = var_930();
                for key_88, value_90 in ipairs(var_956) do
                    if not func_102(value_90) then
                        pcall(function()
                            if value_90:IsA("ParticleEmitter") and value_90.Enabled then
                                value_90.Enabled = false;
                                value_90.Rate = 0;
                            elseif value_90:IsA("Trail") and value_90.Enabled then
                                value_90.Enabled = false;
                            elseif value_90:IsA("Beam") and value_90.Enabled then
                                value_90.Enabled = false;
                            end;
                            return ;
                        end);
                    end;
                end;
                return ;
            end);
        end;
        if var_928 then
            task.spawn(function()
                pcall(function()
                    local var_957 = var_928(game_36.RenderStepped);
                    for key_89, value_91 in ipairs(var_957) do
                        if value_91.Function then
                            local var_958 = var_947 and var_947(value_91.Function);
                            local var_959 = var_958 and var_958.source or "";
                            if not var_959:find("CoreScripts") and not var_959:find("Camera") and not var_959:find("PlayerModule") then
                                local str_39 = tostring(value_91.Function):lower();
                                if str_39:find("particle") or str_39:find("effect") or str_39:find("trail") or str_39:find("vfx") or str_39:find("beam") or str_39:find("smoke") then
                                    table.insert(var_951.Connections, value_91);
                                    value_91:Disable();
                                end;
                            end;
                        end;
                    end;
                    return ;
                end);
                pcall(function()
                    local var_960 = var_928(game_36.Heartbeat);
                    for key_90, value_92 in ipairs(var_960) do
                        if value_92.Function then
                            local var_961 = var_947 and var_947(value_92.Function);
                            local var_962 = var_961 and var_961.source or "";
                            if not var_962:find("CoreScripts") and not var_962:find("Camera") then
                                local str_40 = tostring(value_92.Function):lower();
                                if str_40:find("particle") or str_40:find("effect") then
                                    table.insert(var_951.Connections, value_92);
                                    value_92:Disable();
                                end;
                            end;
                        end;
                    end;
                    return ;
                end);
                return ;
            end);
        end;
        if var_936 then
            pcall(function()
                var_936("GuiNearClip", 0.1);
                return ;
            end);
            pcall(function()
                var_936("ExposureCompensation", 0);
                return ;
            end);
        end;
        getgenv().optimize_conns = {};
        table.insert(getgenv().optimize_conns, game_35.DescendantAdded:Connect(function(arg_289)
            if getgenv().optimize and not func_102(arg_289) then
                task.defer(flag_74, arg_289);
            end;
            return ;
        end));
        table.insert(getgenv().optimize_conns, game_39.DescendantAdded:Connect(function(arg_290)
            if getgenv().optimize and not func_102(arg_290) then
                task.defer(flag_74, arg_290);
            end;
            return ;
        end));
        table.insert(getgenv().optimize_conns, game_34.ChildAdded:Connect(function(arg_291)
            if getgenv().optimize and (not func_102(arg_291) and (arg_291:IsA("PostEffect") or arg_291:IsA("Atmosphere") or arg_291:IsA("Clouds") or arg_291:IsA("Sky"))) then
                var_951.Effects[arg_291] = { Parent = game_34 };
                arg_291.Parent = nil;
                table.insert(var_951.NilParented, arg_291);
            end;
            return ;
        end));
        local func_103 = function(arg_292)
            if not arg_292 then
                return ;
            end;
            for key_91, value_93 in ipairs(arg_292:GetDescendants()) do
                if not func_102(value_93) then
                    pcall(function()
                        if value_93:IsA("ParticleEmitter") or (value_93:IsA("Trail") or value_93:IsA("Beam") or value_93:IsA("Fire") or value_93:IsA("Smoke") or value_93:IsA("Sparkles")) then
                            value_93.Enabled = false;
                        elseif value_93:IsA("MeshPart") then
                            value_93.CastShadow = false;
                            value_93.TextureID = "";
                        elseif value_93:IsA("BasePart") then
                            value_93.CastShadow = false;
                        elseif value_93:IsA("Decal") or value_93:IsA("Texture") then
                            pcall(function()
                                var_925(value_93, "LocalTransparencyModifier", 0.3);
                                return ;
                            end);
                        end;
                        return ;
                    end);
                end;
            end;
            return ;
        end;
        for key_92, value_94 in ipairs(game_37:GetPlayers()) do
            if value_94.Character then
                func_103(value_94.Character);
            end;
            table.insert(getgenv().optimize_conns, value_94.CharacterAdded:Connect(func_103));
        end;
        table.insert(getgenv().optimize_conns, game_37.PlayerAdded:Connect(function(arg_293)
            table.insert(getgenv().optimize_conns, arg_293.CharacterAdded:Connect(func_103));
            return ;
        end));
    else
        if getgenv().optimize_conns then
            for key_93, value_95 in ipairs(getgenv().optimize_conns) do
                pcall(function()
                    value_95:Disconnect();
                    return ;
                end);
            end;
            getgenv().optimize_conns = nil;
        end;
        local var_963 = getgenv().optimize_data;
        if not var_963 then
            return ;
        end;
        local game_45 = game:GetService("Lighting");
        local game_46 = game:GetService("Workspace");
        local game_47 = game:GetService("SoundService");
        local var_964 = game_46:FindFirstChildOfClass("Terrain");
        for key_94, value_96 in pairs(var_963.Lighting or {}) do
            pcall(function()
                if key_94 == "Technology" or key_94 == "GlobalIllumination" then
                    sethiddenproperty(game_45, key_94, value_96);
                else
                    game_45[key_94] = value_96;
                end;
                return ;
            end);
        end;
        if var_964 and var_963.Terrain then
            for key_95, value_97 in pairs(var_963.Terrain) do
                pcall(function()
                    if key_95 == "Decoration" or key_95 == "GrassLength" then
                        sethiddenproperty(var_964, key_95, value_97);
                    else
                        var_964[key_95] = value_97;
                    end;
                    return ;
                end);
            end;
        end;
        for key_96, value_98 in pairs(var_963.Workspace or {}) do
            pcall(function()
                sethiddenproperty(game_46, key_96, value_98);
                return ;
            end);
        end;
        if var_963.Sounds and var_963.Sounds.Service then
            for key_97, value_99 in pairs(var_963.Sounds.Service) do
                pcall(function()
                    game_47[key_97] = value_99;
                    return ;
                end);
            end;
        end;
        for key_98, value_100 in ipairs(var_963.NilParented or {}) do
            pcall(function()
                if var_963.Effects[value_100] and var_963.Effects[value_100].Parent then
                    value_100.Parent = var_963.Effects[value_100].Parent;
                elseif var_963.Textures[value_100] and var_963.Textures[value_100].Parent then
                    value_100.Parent = var_963.Textures[value_100].Parent;
                elseif var_963.Decals[value_100] and var_963.Decals[value_100].Parent then
                    value_100.Parent = var_963.Decals[value_100].Parent;
                elseif var_963.SurfaceAppearances[value_100] and var_963.SurfaceAppearances[value_100].Parent then
                    value_100.Parent = var_963.SurfaceAppearances[value_100].Parent;
                elseif var_963.Materials[value_100] and var_963.Materials[value_100].Parent then
                    value_100.Parent = var_963.Materials[value_100].Parent;
                end;
                return ;
            end);
        end;
        for key_99, value_101 in pairs(var_963.Textures or {}) do
            pcall(function()
                key_99.Texture = value_101.Texture;
                key_99.Transparency = value_101.Transparency;
                key_99.StudsPerTileU = value_101.StudsPerTileU;
                key_99.StudsPerTileV = value_101.StudsPerTileV;
                return ;
            end);
        end;
        for key_100, value_102 in pairs(var_963.Decals or {}) do
            pcall(function()
                key_100.Texture = value_102.Texture;
                key_100.Transparency = value_102.Transparency;
                return ;
            end);
        end;
        for key_101, value_103 in pairs(var_963.SurfaceAppearances or {}) do
            pcall(function()
                key_101.ColorMap = value_103.ColorMap;
                key_101.MetalnessMap = value_103.MetalnessMap;
                key_101.NormalMap = value_103.NormalMap;
                key_101.RoughnessMap = value_103.RoughnessMap;
                key_101.TexturePack = value_103.TexturePack;
                return ;
            end);
        end;
        for key_102, value_104 in pairs(var_963.Materials or {}) do
            pcall(function()
                key_102.ColorMap = value_104.ColorMap;
                key_102.MetalnessMap = value_104.MetalnessMap;
                key_102.NormalMap = value_104.NormalMap;
                key_102.RoughnessMap = value_104.RoughnessMap;
                return ;
            end);
        end;
        for key_103, value_105 in pairs(var_963.Effects or {}) do
            pcall(function()
                for key_104, value_106 in pairs(value_105) do
                    if key_104 ~= "Parent" and key_104 ~= "ClassName" then
                        key_103[key_104] = value_106;
                    end;
                end;
                return ;
            end);
        end;
        for key_105, value_107 in pairs(var_963.Parts or {}) do
            pcall(function()
                for key_106, value_108 in pairs(value_107) do
                    key_105[key_106] = value_108;
                end;
                return ;
            end);
        end;
        for key_107, value_109 in pairs(var_963.Meshes or {}) do
            pcall(function()
                for key_108, value_110 in pairs(value_109) do
                    if key_108 == "CollisionFidelity" or key_108 == "FluidFidelity" then
                        sethiddenproperty(key_107, key_108, value_110);
                    elseif key_108 ~= "RenderFidelity" or not key_107:IsA("UnionOperation") and not key_107:IsA("NegateOperation") and not key_107:IsA("IntersectOperation") then
                        key_107[key_108] = value_110;
                    end;
                end;
                return ;
            end);
        end;
        for key_109, value_111 in pairs(var_963.Sounds or {}) do
            if typeof(value_111) == "table" and value_111 ~= var_963.Sounds.Service then
                pcall(function()
                    for key_110, value_112 in pairs(value_111) do
                        key_109[key_110] = value_112;
                    end;
                    return ;
                end);
            end;
        end;
        for key_111, value_113 in pairs(var_963.Guis or {}) do
            pcall(function()
                for key_112, value_114 in pairs(value_113) do
                    key_111[key_112] = value_114;
                end;
                return ;
            end);
        end;
        for key_113, value_115 in pairs(var_963.Humanoids or {}) do
            pcall(function()
                for key_114, value_116 in pairs(value_115) do
                    key_113[key_114] = value_116;
                end;
                return ;
            end);
        end;
        for key_115, value_117 in ipairs(var_963.Connections or {}) do
            pcall(function()
                value_117:Enable();
                return ;
            end);
        end;
        getgenv().optimize_data = nilw;
    end;
    return ;
end);
exploit_section.create_toggle({ title = "Hide Accounts", desc = "Hides other accounts (this can't be undone)", default = false }, function(arg_294)
    getgenv().fa = arg_294;
    while getgenv().fa and task.wait() do
        for var_965, var_966 in next, game:GetService("Players"):GetPlayers() do
            if var_966.Character and var_966 ~= player then
                var_966.Character:Destroy();
            end;
        end;
    end;
    return ;
end);
exploit_section.create_button({ title = "Remove Fog + Fix Lightning" }, function(arg_295)
    while task.wait() do
        for var_967, var_968 in next, game:GetService("Lighting"):GetChildren() do
            var_968:Destroy();
        end;
        game:GetService("Lighting").FogEnd = 100000;
        game:GetService("Lighting").ClockTime = 12;
    end;
    return ;
end);
exploit_section.create_button({ title = "Desync" }, function(arg_296)
    setfflag("NextGenReplicatorEnabledWrite4", "true");
    return ;
end);
getgenv().target_nearest = true;
GetNearest = function(arg_297)
    local local_player_6 = game.Players.LocalPlayer;
    local var_969 = local_player_6.Character;
    if not var_969 or not var_969:FindFirstChild(var) then
        return ;
    end;
    if getgenv().target_nearest then
        local flag_75 = nil;
        local num_86 = math.huge;
        for var_970, var_971 in next, game.Players:GetPlayers() do
            if var_971 ~= local_player_6 and var_971.Character and var_971.Character:FindFirstChild(var) and var_971.Character:FindFirstChildOfClass("Humanoid") and var_971.Character:FindFirstChildOfClass("Humanoid").Health > 0 then
                local var_972 = (var_971.Character.HumanoidRootPart.Position - var_969.HumanoidRootPart.Position).Magnitude;
                if var_972 < num_86 and var_972 <= 100 then
                    flag_75 = var_971.Character;
                    num_86 = var_972;
                end;
            end;
        end;
        if not flag_75 then
            for var_973, var_974 in next, workspace.NPCs:GetChildren() do
                if var_974:IsA("Model") and var_974:FindFirstChild(var) and var_974:FindFirstChildOfClass("Humanoid") and var_974:FindFirstChildOfClass("Humanoid").Health > 0 and var_974 ~= var_969 then
                    local var_975 = (var_974.HumanoidRootPart.Position - var_969.HumanoidRootPart.Position).Magnitude;
                    if var_975 < num_86 then
                        flag_75 = var_974;
                        num_86 = var_975;
                    end;
                end;
            end;
        end;
        if arg_297 then
            return flag_75;
        end;
        return flag_75 and flag_75:FindFirstChild(var).CFrame or var_969.HumanoidRootPart.CFrame;
    end;
    return var_969.HumanoidRootPart.CFrame;
end;
selected = "Coco Island";
island_tp.create_dropdown({ title = "Island Tp", List = island_names, default = "Coco Island", search = true, selected = false }, function(arg_298)
    selected = arg_298;
    return ;
end);
island_tp.create_button({ title = "Teleport" }, function(arg_299)
    if not selected then
        return ;
    end;
    func_75(Islands[selected], 50, true);
    return ;
end);
local func_104 = function(arg_300)
    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("learnStyle"):FireServer(arg_300);
    return ;
end;
local tbl_55 = { "skyWalkTrainer", "Cyborg", "Vampire", "Rokushiki" };
local var_976 = "skyWalkTrainer";
market_section.create_dropdown({ title = "Select Value", List = tbl_55, default = "skyWalkTrainer", search = false, selected = false }, function(arg_301)
    var_976 = arg_301;
    return ;
end);
market_section.create_button({ title = "Purchase" }, function(arg_302)
    if table.find(tbl_55, var_976) then
        func_104(var_976);
    end;
    return ;
end);
local tbl_56 = { ws = nil, ws_connected = false, registered = false, accountId = nil, last_inventory_snapshot = {}, auto_reconnect = false, update_loop_running = false, inventory_loop_running = false, inventory_connection = nil, notification_connection = nil, backpack_connection = nil, stats_folder = nil, inventory_value = nil, HttpService = nil, workspace_service = nil };
connect = function()
    if tbl_56.ws and tbl_56.ws_connected then
        return ;
    end;
    tbl_56.auto_reconnect = true;
    local tbl_57 = { BASE_URL = "wss://feral-backend-production.up.railway.app", LICENSE_KEY = script_key, UPDATE_INTERVAL = 1, INVENTORY_UPDATE_INTERVAL = 3 };
    local game_48 = game:GetService("Players");
    local game_49 = game:GetService("ReplicatedStorage");
    local game_50 = game:GetService("HttpService");
    local game_51 = game:GetService("Workspace");
    local game_52 = game:GetService("RunService");
    local var_977 = game_48.LocalPlayer;
    local child_56 = game_49:FindFirstChild("Stats" .. var_977.Name);
    local child_57 = child_56:FindFirstChild("Inventory"):FindFirstChild("Inventory");
    tbl_56.stats_folder = child_56;
    tbl_56.inventory_value = child_57;
    tbl_56.HttpService = game_50;
    tbl_56.Workspace = game_51;
    local func_105 = function()
        return tostring(var_977.UserId) .. "_" .. tostring(math.floor(tick() * 1000));
    end;
    local func_106 = function()
        local tbl_58 = { name = var_977.Name, level = 0, peli = 0, fps = 60, ping = 0 };
        pcall(function()
            tbl_58.level = child_56.Stats.Level.Value;
            tbl_58.peli = child_56.Stats.Peli.Value;
            return ;
        end);
        pcall(function()
            tbl_58.fps = math.floor(1 / game_52.RenderStepped:Wait());
            return ;
        end);
        pcall(function()
            tbl_58.ping = math.floor(var_977:GetNetworkPing() * 1000);
            return ;
        end);
        return tbl_58;
    end;
    local func_107 = function()
        local tbl_59 = {};
        pcall(function()
            if not child_57 or child_57.Value == "" then
                return ;
            end;
            local var_978, var_979 = pcall(function()
                return game_50:JSONDecode(child_57.Value);
            end);
            if var_978 and type(var_979) == "table" then
                for key_116, value_118 in pairs(var_979) do
                    local num_87 = 1;
                    if type(value_118) == "table" then
                        num_87 = value_118.Count or 1;
                    end;
                    table.insert(tbl_59, { name = key_116, count = num_87 });
                end;
            end;
            return ;
        end);
        return tbl_59;
    end;
    local func_108 = function(arg_303)
        if not tbl_56.ws then
            return false;
        end;
        return (pcall(function()
            tbl_56.ws:Send(game_50:JSONEncode(arg_303));
            return ;
        end));
    end;
    local func_109 = function(arg_304, arg_305)
        if not tbl_56.ws or not tbl_56.registered then
            return ;
        end;
        func_108({ type = arg_304, payload = arg_305 });
        return ;
    end;
    local func_110 = function(arg_306, arg_307, arg_308, arg_309)
        if not tbl_56.registered then
            return ;
        end;
        local flag_76 = nil;
        pcall(function()
            local var_980 = require(game.ReplicatedStorage.Modules.ToolDesc)[arg_306];
            flag_76 = var_980 and var_980.Rare or nil;
            return ;
        end);
        func_109("bought_item", { itemName = arg_306, cost = arg_307 or 0, costType = arg_308 or "Peli", rarity = flag_76, amount = arg_309 or 1 });
        return ;
    end;
    local func_111 = function(arg_310, arg_311)
        if not tbl_56.registered then
            return ;
        end;
        func_109("update_bait", { baitName = arg_310, baitType = arg_311 or "common" });
        return ;
    end;
    local func_112 = function(arg_312)
        if not tbl_56.registered then
            return ;
        end;
        func_109("update_fishing_state", { state = arg_312 or "idle" });
        return ;
    end;
    local func_113 = function(arg_313, arg_314)
        if not tbl_56.registered then
            return ;
        end;
        func_109("caught_fruit", { fruitName = arg_313, rarity = arg_314 or "Common" });
        return ;
    end;
    local tbl_60 = { { name = "Anglerfish", rarity = "Legendary", keywords = { "angler", "fish" } }, { name = "Dark Skeletal Shark", rarity = "Legendary", keywords = { "dark", "skeletal", "shark" } }, { name = "Jack-O'-Bite", rarity = "Legendary", keywords = { "jack", "bite", "jacko" } }, { name = "Swordfish", rarity = "Legendary", keywords = { "sword", "fish" } }, { name = "Golden Polka Puffer", rarity = "Legendary", keywords = { "golden", "polka", "puffer" } }, { name = "Golden Ribbon Angelfish", rarity = "Legendary", keywords = { "golden", "ribbon", "angel" } }, { name = "Golden Tigerfin", rarity = "Legendary", keywords = { "golden", "tiger" } }, { name = "Skeletal Shark", rarity = "Epic", keywords = { "skeletal", "shark", "halloween" } }, { name = "Candy Corn Squid", rarity = "Rare", keywords = { "candy", "corn", "squid" } }, { name = "Crimson Polka Puffer", rarity = "Rare", keywords = { "crimson", "polka", "puffer" } }, { name = "Crimson Snapper", rarity = "Rare", keywords = { "crimson", "snapper" } }, { name = "Exotic Tigerfin", rarity = "Rare", keywords = { "exotic", "tiger" } }, { name = "Fangfish", rarity = "Rare", keywords = { "fang", "fish" } }, { name = "Zebra Ribbon Angelfish", rarity = "Rare", keywords = { "zebra", "ribbon", "angel" } }, { name = "Blue-Lip Grouper", rarity = "Common", keywords = { "blue", "lip", "grouper" } }, { name = "Tigerfin", rarity = "Common", keywords = { "tiger", "fin" } } };
    local tbl_61 = { { name = "Kilo Kilo no Mi", rarity = "Common", keywords = { "kilo", "pound" } }, { name = "Suke Suke no Mi", rarity = "Common", keywords = { "suke", "clear" } }, { name = "Guru Guru no Mi", rarity = "Common", keywords = { "guru", "spin" } }, { name = "Chiyu Chiyu no Mi", rarity = "Common", keywords = { "chiyu", "heal" } }, { name = "Bari Bari no Mi", rarity = "Rare", keywords = { "bari", "barrier" } }, { name = "Mero Mero no Mi", rarity = "Rare", keywords = { "mero", "love" } }, { name = "Horo Horo no Mi", rarity = "Rare", keywords = { "horo", "hollow" } }, { name = "Gomu Gomu no Mi", rarity = "Rare", keywords = { "gomu", "rubber" } }, { name = "Bomu Bomu no Mi", rarity = "Rare", keywords = { "bomu", "bomb" } }, { name = "Yomi Yomi no Mi", rarity = "Epic", keywords = { "yomi", "revive" } }, { name = "Bane Bane no Mi", rarity = "Epic", keywords = { "bane", "spring" } }, { name = "Kira Kira no Mi", rarity = "Epic", keywords = { "kira", "diamond" } }, { name = "Zushi Zushi no Mi", rarity = "Legendary", keywords = { "zushi", "gravity" } }, { name = "Gura Gura no Mi", rarity = "Legendary", keywords = { "gura", "tremor", "quake" } }, { name = "Suna Suna no Mi", rarity = "Legendary", keywords = { "suna", "sand" } }, { name = "Hie Hie no Mi", rarity = "Legendary", keywords = { "hie", "ice" } }, { name = "Ito Ito no Mi", rarity = "Legendary", keywords = { "ito", "string" } }, { name = "Goro Goro no Mi", rarity = "Legendary", keywords = { "goro", "rumble", "lightning" } }, { name = "Nikyu Nikyu no Mi", rarity = "Legendary", keywords = { "nikyu", "paw" } }, { name = "Mera Mera no Mi", rarity = "Legendary", keywords = { "mera", "flame", "fire" } }, { name = "Kage Kage no Mi", rarity = "Legendary", keywords = { "kage", "shadow" } }, { name = "Magu Magu no Mi", rarity = "Legendary", keywords = { "magu", "magma" } }, { name = "Pika Pika no Mi", rarity = "Legendary", keywords = { "pika", "light" } }, { name = "Yami Yami no Mi", rarity = "Legendary", keywords = { "yami", "darkness" } }, { name = "Yuki Yuki no Mi", rarity = "Legendary", keywords = { "yuki", "snow" } }, { name = "Goru Goru no Mi", rarity = "Legendary", keywords = { "goru", "gold" } }, { name = "Moku Moku no Mi", rarity = "Legendary", keywords = { "moku", "smoke" } }, { name = "Tori Tori no Mi, Model: Phoenix", rarity = "Mythical", keywords = { "tori", "phoenix", "bird" } }, { name = "Mochi Mochi no Mi", rarity = "Mythical", keywords = { "mochi" } }, { name = "Ope Ope no Mi", rarity = "Mythical", keywords = { "ope", "operation" } }, { name = "Doku Doku no Mi", rarity = "Mythical", keywords = { "doku", "venom", "poison" } }, { name = "Hito Hito no Mi, Model: Daibutsu", rarity = "Mythical", keywords = { "hito", "buddha", "daibutsu" } }, { name = "Ryu Ryu no Mi, Model: Pteranodon", rarity = "Mythical", keywords = { "ryu", "ptero", "pteranodon" } } };
    local func_114 = function(arg_315, arg_316)
        local var_981 = arg_315:lower();
        local flag_77 = nil;
        local num_88 = 0;
        for key_117, value_119 in ipairs(arg_316) do
            local num_89 = 0;
            for key_118, value_120 in ipairs(value_119.keywords) do
                if var_981:find(value_120:lower()) then
                    num_89 = num_89 + 1;
                end;
            end;
            if num_88 < num_89 then
                flag_77 = value_119;
                num_88 = num_89;
            end;
        end;
        if flag_77 and num_88 > 0 then
            return flag_77;
        end;
        for key_119, value_121 in ipairs(arg_316) do
            local var_982 = value_121.name:lower();
            if var_982:find(var_981) or var_981:find(var_982) then
                return value_121;
            end;
        end;
        return nil;
    end;
    local func_115 = function(arg_317)
        local var_983 = func_114(arg_317, tbl_60);
        if var_983 then
            func_113(var_983.name, var_983.rarity);
            return true;
        end;
        local var_984 = func_114(arg_317, tbl_61);
        if var_984 then
            func_113(var_984.name, var_984.rarity);
            return true;
        end;
        return false;
    end;
    local tbl_62 = { "Anglerfish", "Dark Skeletal Shark", "Jack-O'-Bite", "Swordfish", "Golden Polka Puffer", "Golden Ribbon Angelfish", "Golden Tigerfin", "Skeletal Shark", "Candy Corn Squid", "Crimson Polka Puffer", "Crimson Snapper", "Exotic Tigerfin", "Fangfish", "Zebra Ribbon Angelfish", "Blue-Lip Grouper", "Tigerfin", "Kilo Kilo no Mi", "Suke Suke no Mi", "Guru Guru no Mi", "Chiyu Chiyu no Mi", "Bari Bari no Mi", "Mero Mero no Mi", "Horo Horo no Mi", "Gomu Gomu no Mi", "Bomu Bomu no Mi", "Yomi Yomi no Mi", "Bane Bane no Mi", "Kira Kira no Mi", "Zushi Zushi no Mi", "Gura Gura no Mi", "Suna Suna no Mi", "Hie Hie no Mi", "Ito Ito no Mi", "Goro Goro no Mi", "Nikyu Nikyu no Mi", "Mera Mera no Mi", "Kage Kage no Mi", "Magu Magu no Mi", "Pika Pika no Mi", "Yami Yami no Mi", "Yuki Yuki no Mi", "Goru Goru no Mi", "Moku Moku no Mi", "Tori Tori no Mi, Model: Phoenix", "Mochi Mochi no Mi", "Ope Ope no Mi", "Doku Doku no Mi", "Hito Hito no Mi, Model: Daibutsu", "Ryu Ryu no Mi, Model: Pteranodon" };
    local func_116 = function()
        pcall(function()
            local var_985 = var_977;
            if not var_985 or not var_985:FindFirstChild("PlayerGui") then
                return ;
            end;
            local child_58 = var_985.PlayerGui:WaitForChild("Notifications", 5);
            if not child_58 then
                return ;
            end;
            tbl_56.notification_connection = child_58.DescendantAdded:Connect(function(arg_318)
                if arg_318:IsA("TextLabel") then
                    local var_986 = arg_318.Parent;
                    if var_986 then
                        local flag_78 = false;
                        for key_120, value_122 in pairs(var_986:GetChildren()) do
                            if value_122:IsA("TextLabel") and value_122.Text:match("New") then
                                flag_78 = true;
                                break;
                            end;
                        end;
                        if flag_78 then
                            local var_987 = arg_318.Text:gsub("[<>]", "");
                            if table.find(tbl_62, var_987) then
                                func_115(var_987);
                            end;
                        end;
                    end;
                end;
                return ;
            end);
            return ;
        end);
        return ;
    end;
    local func_117 = function()
        pcall(function()
            local var_988 = var_977;
            if not var_988 or not var_988:FindFirstChild("Backpack") then
                return ;
            end;
            tbl_56.backpack_connection = var_988.Backpack.ChildAdded:Connect(function(arg_319)
                if arg_319:IsA("Tool") then
                    local var_989 = arg_319.Name;
                    local var_990 = func_114(var_989, tbl_61);
                    if var_990 then
                        func_113(var_990.name, var_990.rarity);
                    end;
                end;
                return ;
            end);
            return ;
        end);
        return ;
    end;
    local func_118 = function(arg_320)
        local var_991, var_992 = pcall(function()
            return game_50:JSONDecode(arg_320);
        end);
        if not var_991 then
            return ;
        end;
        if var_992.type == "connected" or var_992.type == "auth_success" then
            tbl_56.ws_connected = true;
            tbl_56.accountId = func_105();
            local var_993 = func_106();
            local var_994 = func_107();
            local tbl_63 = { type = "register_game", payload = { userKey = tbl_57.LICENSE_KEY, accountId = tbl_56.accountId, name = var_993.name, level = var_993.level, peli = var_993.peli, fps = var_993.fps, ping = var_993.ping, inventory = var_994 } };
            func_108(tbl_63);
        elseif var_992.type == "register_success" then
            tbl_56.registered = true;
        elseif var_992.type == "register_error" then
            tbl_56.registered = false;
        elseif var_992.type ~= "pong" then
        end;
        return ;
    end;
    local func_119;
    func_119 = function()
        local var_995 = tbl_57.BASE_URL .. "/" .. tbl_57.LICENSE_KEY;
        if not pcall(function()
            if not WebSocket then
                error("WebSocket not available in this executor");
            end;
            tbl_56.ws = WebSocket.connect(var_995);
            if not tbl_56.ws then
                error("Failed to create WebSocket connection");
            end;
            tbl_56.ws.OnMessage:Connect(function(arg_321)
                func_118(arg_321);
                return ;
            end);
            tbl_56.ws.OnClose:Connect(function()
                tbl_56.ws_connected = false;
                tbl_56.registered = false;
                if tbl_56.auto_reconnect then
                    task.wait(5);
                    func_119();
                end;
                return ;
            end);
            return ;
        end) and tbl_56.auto_reconnect then
            task.wait(5);
            func_119();
        end;
        return ;
    end;
    local func_120 = function()
        if tbl_56.update_loop_running then
            return ;
        end;
        tbl_56.update_loop_running = true;
        task.spawn(function()
            while tbl_56.auto_reconnect do
                task.wait(tbl_57.UPDATE_INTERVAL);
                if tbl_56.registered then
                    local var_996 = func_106();
                    func_109("update_account", { level = var_996.level, peli = var_996.peli, fps = var_996.fps, ping = var_996.ping });
                end;
            end;
            tbl_56.update_loop_running = false;
            return ;
        end);
        return ;
    end;
    local func_121 = function()
        if tbl_56.inventory_loop_running then
            return ;
        end;
        tbl_56.inventory_loop_running = true;
        task.spawn(function()
            while tbl_56.auto_reconnect do
                task.wait(tbl_57.INVENTORY_UPDATE_INTERVAL);
                if tbl_56.registered then
                    local var_997 = func_107();
                    local var_998 = func_101("Rare Fruit Chest") or 0;
                    local var_999 = func_101("Legendary Fruit Chest") or 0;
                    local var_1000 = func_101("Mythic Fruit Chest") or 0;
                    local var_1001 = func_101("Legendary Fish Bait") or 0;
                    func_109("update_inventory", { inventory = var_997, chestStats = { rareChest = var_998, legendaryChest = var_999, mythicChest = var_1000 }, legendaryFishBait = var_1001 });
                end;
            end;
            tbl_56.inventory_loop_running = false;
            return ;
        end);
        return ;
    end;
    func_119();
    func_120();
    func_121();
    func_116();
    func_117();
    print("we managed to connect!");
    return ;
end;
disconnect = function()
    tbl_56.auto_reconnect = false;
    if tbl_56.ws and tbl_56.HttpService then
        pcall(function()
            if tbl_56.registered then
                tbl_56.ws:Send(tbl_56.HttpService:JSONEncode({ type = "game_disconnect", payload = {} }));
            end;
            tbl_56.ws:Close();
            return ;
        end);
    end;
    if tbl_56.notification_connection then
        tbl_56.notification_connection:Disconnect();
        tbl_56.notification_connection = nil;
    end;
    if tbl_56.backpack_connection then
        tbl_56.backpack_connection:Disconnect();
        tbl_56.backpack_connection = nil;
    end;
    tbl_56.ws_connected = false;
    tbl_56.registered = false;
    tbl_56.ws = nil;
    tbl_56.last_inventory_snapshot = {};
    tbl_56.stats_folder = nil;
    tbl_56.inventory_value = nil;
    tbl_56.HttpService = nil;
    tbl_56.Workspace = nil;
    return ;
end;
local func_122 = function(arg_322, arg_323, arg_324, arg_325)
    if not tbl_56.registered or (not tbl_56.ws or not tbl_56.HttpService) then
        return ;
    end;
    local var_1002 = get_rarity(arg_322);
    pcall(function()
        tbl_56.ws:Send(tbl_56.HttpService:JSONEncode({ type = "bought_item", payload = { itemName = arg_322, cost = arg_323 or 0, costType = arg_324 or "Peli", rarity = var_1002, amount = arg_325 or 1 } }));
        return ;
    end);
    return ;
end;
local func_123 = function(arg_326, arg_327)
    if not tbl_56.registered or (not tbl_56.ws or not tbl_56.HttpService) then
        return ;
    end;
    pcall(function()
        tbl_56.ws:Send(tbl_56.HttpService:JSONEncode({ type = "update_bait", payload = { baitName = arg_326, baitType = arg_327 or "common" } }));
        return ;
    end);
    return ;
end;
local func_124 = function(arg_328)
    if not tbl_56.registered or (not tbl_56.ws or not tbl_56.HttpService) then
        return ;
    end;
    pcall(function()
        tbl_56.ws:Send(tbl_56.HttpService:JSONEncode({ type = "update_fishing_state", payload = { state = arg_328 or "idle" } }));
        return ;
    end);
    return ;
end;
local func_125 = function(arg_329, arg_330)
    if not tbl_56.registered or (not tbl_56.ws or not tbl_56.HttpService) then
        return ;
    end;
    pcall(function()
        tbl_56.ws:Send(tbl_56.HttpService:JSONEncode({ type = "caught_fruit", payload = { fruitName = arg_329, rarity = arg_330 or "Common" } }));
        return ;
    end);
    return ;
end;
_G.FeralClient = {
    connect = connect,
    disconnect = disconnect,
    is_connected = function()
        return tbl_56.registered;
    end,
    send_bought_item = func_122,
    update_bait = func_123,
    update_fishing_state = func_124,
    send_caught_fruit = func_125,
    get_peli = func_36,
    get_candy = func_37,
    get_level = func_38,
    get_camera = get_camera,
    get_item_count = func_101,
    get_inventory_data = get_inventory_data,
    get_rarity = get_rarity
};
fishing_titles = { [1] = "Godly Fisherman", [2] = "Master Fisherman", [3] = "Skilled Fisherman", [4] = "Novice Fisherman", [5] = "Fruit Fisher" };
get_best_title = function()
    local flag_79 = nil;
    local num_90 = math.huge;
    local var_1003 = stats_folder.Titles.AllTitles;
    local game_53 = game:GetService("HttpService"):JSONDecode(var_1003.Value);
    for var_1004, var_1005 in next, fishing_titles, nil do
        for var_1006, var_1007 in next, game_53, nil do
            if var_1006 == var_1005 and var_1004 < num_90 then
                flag_79 = var_1005;
                num_90 = var_1004;
            end;
        end;
    end;
    return flag_79;
end;
webhook_cooldowns = {};
cooldown_time = 60;
global_webhook_cooldown = 1;
last_webhook_time = 0;
local func_126 = function(arg_331)
    if not tbl_56.registered or (not tbl_56.ws or not tbl_56.HttpService) then
        return ;
    end;
    pcall(function()
        tbl_56.ws:Send(tbl_56.HttpService:JSONEncode({ type = "update_merchant_stock", payload = arg_331 }));
        return ;
    end);
    return ;
end;
local func_127 = function()
    local game_54 = game:GetService("Players").LocalPlayer;
    if game_54 then
        local child_59 = game_54:FindFirstChild("PlayerGui");
        if child_59 then
            local child_60 = child_59:FindFirstChild("MerchentShop");
            if child_60 then
                local var_1008 = child_60:GetAttribute("Prices");
                if var_1008 and var_1008 ~= "" and var_1008 ~= last_sent_data then
                    local var_1009, var_1010 = pcall(function()
                        return tbl_56.HttpService:JSONDecode(var_1008);
                    end);
                    if var_1009 and var_1010 then
                        func_126(var_1010);
                        last_sent_data = var_1008;
                    end;
                end;
            end;
        end;
    end;
    return ;
end;
send_webhook = function(arg_332, arg_333, arg_334)
    local var_1011 = getgenv().fish_kaitun.Webhook;
    if not var_1011 or var_1011 == "" then
        return ;
    end;
    if arg_334 == nil then
        arg_334 = true;
    end;
    local var_1012 = type(arg_332) == "table" and arg_332.Name or type(arg_332) == "string" and arg_332 or "Unknown";
    local var_1013 = tick();
    if var_1013 - last_webhook_time < global_webhook_cooldown then
        return ;
    end;
    if webhook_cooldowns[var_1012] and var_1013 - webhook_cooldowns[var_1012] < cooldown_time then
        return ;
    end;
    webhook_cooldowns[var_1012] = var_1013;
    last_webhook_time = var_1013;
    local var_1014 = request or http_request or syn and syn.request or fluxus and fluxus.request or krnl and krnl.request;
    if not var_1014 then
        warn("No supported HTTP request function found.");
        return ;
    end;
    local game_55 = game:GetService("Players");
    local game_56 = game:GetService("HttpService");
    local var_1015 = game_55.LocalPlayer;
    if not var_1015 then
        warn("LocalPlayer not found.");
        return ;
    end;
    local var_1016 = var_1015.Name;
    local var_1017 = var_1015.UserId;
    if type(arg_332) == "table" and arg_332.Quantity then
    end;
    local num_91 = os.date("!%Y-%m-%dT%H:%M:%S.000Z");
    local var_1018 = get_rarity(arg_332);
    local var_1019 = var_1012 == "Mythical Fruit Chest" or arg_333 and (var_1018 == "Mythical" or var_1018 == "Mythic");
    local num_92 = 5814783;
    local flag_80 = nil;
    local var_1020, var_1021;
    if arg_333 and table.find(getgenv().fish_kaitun.WebhookFruits, var_1018) then
        if arg_334 then
            var_1020 = "Devil Fruit Log";
            var_1021 = "Stored Devil Fruit: " .. tostring(var_1012);
            if var_1019 then
                num_92 = 16776960;
            end;
        else
            var_1020 = "Devil Fruit Log";
            var_1021 = "Failed to store " .. tostring(var_1012) .. " devil fruit";
            num_92 = 16711680;
        end;
    else
        var_1020 = "Purchase Log";
        var_1021 = "Purchased x" .. tostring(get_stock(var_1012)) .. " " .. tostring(var_1012);
        local child_61 = var_1015.PlayerGui:FindFirstChild("MerchentShop");
        if child_61 then
            local var_1022 = child_61:GetAttribute("Prices");
            if var_1022 and var_1022 ~= "" then
                local var_1023, var_1024 = pcall(game_56.JSONDecode, game_56, var_1022);
                if var_1023 and type(var_1024) == "table" then
                    local var_1025 = var_1024[var_1012];
                    if var_1025 then
                        local var_1026 = var_1025.remaining or 0;
                        local var_1027 = var_1025.price or 0;
                        local var_1028 = func_36();
                        if type(var_1028) == "number" and var_1027 > 0 then
                            local num_93 = math.floor(var_1028 / var_1027);
                            var_1021 = (var_1021 .. "\nStock Remaining: " .. tostring(var_1026)) .. "\nCan Afford: " .. tostring(num_93) .. " more";
                        end;
                    end;
                end;
            end;
        end;
        if var_1019 then
            num_92 = 16776960;
        end;
    end;
    if var_1019 then
        flag_80 = "@everyone";
    end;
    local var_1029 = var_1021 .. "\nPlayer: " .. var_1016 .. " (" .. tostring(var_1017) .. ")";
    local tbl_64 = { username = "Feral | Purchase Logs", avatar_url = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQsLa1v6TE3iNJ3WESfDIPVwEji6FTcp6RGfg&s", embeds = { { title = var_1020, description = var_1029, color = num_92, timestamp = num_91, footer = { text = "Feral" } } } };
    if flag_80 then
        tbl_64.content = flag_80;
    end;
    local var_1030 = game_56:JSONEncode(tbl_64);
    local var_1031, var_1032 = pcall(function()
        var_1014({ Url = var_1011, Method = "POST", Headers = { ["Content-Type"] = "application/json" }, Body = var_1030 });
        return ;
    end);
    if not var_1031 then
        warn("Failed to send webhook: " .. tostring(var_1032));
    end;
    return ;
end;
local var_1033 = require(game:GetService("ReplicatedStorage").Modules.ToolDesc);
local tbl_65 = { "Katana", "Katana2", "Gun", "Core", "Hammer", "Chest", "Grip", "FishingRod", "Mount", "Ship", "Baloon", "CandyBasket", "Cap", "Head", "Cape", "Waist", "Jaw", "Back", "Back2", "Glasses", "Headband", "Ears", "TorsoScarf", "Shoulder", "Shoulder2", "Outfit", "CustomOutfit", "ASESlot", "Book", "Artifact", "Pet", "Key", "Item" };
local func_128 = function(arg_335)
    if var_1033[arg_335] and (var_1033[arg_335].type and table.find(tbl_65, var_1033[arg_335].type) and func_101(arg_335) > 0) then
        return true;
    end;
    return false;
end;
local var_1034 = page_3.create_section("Fish Kaitun - Main");
local child_62 = workspace:FindFirstChild("Uhhh");
if not child_62 then
    child_62 = Instance.new("Part");
    child_62.Parent = workspace;
    child_62.Name = "Uhhh";
    child_62.Position = Vector3.new(0, 15000, 0);
    child_62.Size = Vector3.new(20, 0.1, 20);
    child_62.Material = Enum.Material.ForceField;
    child_62.Color = Color3.fromRGB(255, 0, 0);
    child_62.Anchored = true;
end;
local func_129 = function()
    local child_63 = game.Players.LocalPlayer.PlayerGui:FindFirstChild("MerchentShop");
    if not child_63 then
        return {};
    end;
    local var_1035 = child_63:GetAttribute("Prices");
    if not var_1035 or var_1035 == "" then
        return {};
    end;
    local game_57 = game:GetService("HttpService");
    local var_1036, var_1037 = pcall(game_57.JSONDecode, game_57, var_1035);
    if not var_1036 or type(var_1037) ~= "table" then
        return {};
    end;
    local tbl_66 = {};
    for var_1038, var_1039 in next, getgenv().fish_kaitun.Buyables, nil do
        if var_1037[var_1039] then
            table.insert(tbl_66, { name = var_1039, price = var_1037[var_1039].price or 0 });
        end;
    end;
    table.sort(tbl_66, function(arg_336, arg_337)
        return arg_336.price > arg_337.price;
    end);
    return tbl_66;
end;
getgenv().task_queue = getgenv().task_queue or {};
getgenv().task_running = getgenv().task_running or {};
getgenv().delayed = false;
getgenv().last_merchant = 0;
getgenv().equip_state = getgenv().equip_state or { IsEquipping = false, LastEquip = 0, LastReequip = 0 };
getgenv().blacklist = getgenv().blacklist or {};
getgenv().last_hook_state = nil;
getgenv().storing_fruit = false;
getgenv().throw_attempts = 0;
getgenv().reel_attempts = 0;
local tbl_67 = {
    queue = function(arg_338, arg_339)
        if not getgenv().task_running[arg_338] then
            getgenv().task_running[arg_338] = true;
            task.spawn(function()
                pcall(arg_339);
                getgenv().task_running[arg_338] = false;
                return ;
            end);
        end;
        return ;
    end,
    is_running = function(arg_340)
        return getgenv().task_running[arg_340] == true;
    end
};
if not getgenv().freaky then
    getgenv().freaky = game:GetService("Players").LocalPlayer.PlayerGui.Notifications.DescendantAdded:Connect(function(arg_341)
        if arg_341:IsA("TextLabel") and arg_341.Text == "shark" then
            local local_player_7 = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid");
            if local_player_7 then
                local_player_7:UnequipTools();
            end;
        end;
        return ;
    end);
end;
if not getgenv().fish_caught then
    getgenv().fish_caught = game:GetService("Players").LocalPlayer.PlayerGui.Notifications.DescendantAdded:Connect(function(arg_342)
        if arg_342:IsA("TextLabel") and (arg_342.Parent and arg_342.Parent.Name == "Container") then
            getgenv().reel_attempts = 0;
            getgenv().throw_attempts = 0;
        end;
        return ;
    end);
end;
craft_bait = function(arg_343)
    if not inventory_value or not inventory_value.Value then
        return ;
    end;
    local var_1042, var_1043 = pcall(function()
        local var_1040 = require(game:GetService("ReplicatedStorage").Modules.ToolDesc);
        local game_58 = game:GetService("HttpService"):JSONDecode(inventory_value.Value);
        for key_121, value_123 in pairs(game_58) do
            local var_1041 = var_1040[key_121];
            if var_1041 and var_1041.type == "Fish" then
                if arg_343 == "Rare" and var_1041.Rare == "Rare" then
                    game:GetService("ReplicatedStorage").CraftingRemote:InvokeServer({ BlueprintItem = "Rare Fish Bait", Method = "Craft", ExtraData = { ["Rare Fish"] = key_121 } });
                    return ;
                end;
                if arg_343 == "Legendary" and var_1041.Rare == "Legendary" then
                    game:GetService("ReplicatedStorage").CraftingRemote:InvokeServer({ BlueprintItem = "Legendary Fish Bait", Method = "Craft", ExtraData = { ["Legendary Fish"] = key_121 } });
                    return ;
                end;
            end;
        end;
        return ;
    end);
    return ;
end;
can_craft = function(arg_344)
    if not inventory_value or not inventory_value.Value then
        return false;
    end;
    local var_1046, var_1047 = pcall(function()
        local var_1044 = require(game:GetService("ReplicatedStorage").Modules.ToolDesc);
        local game_59 = game:GetService("HttpService"):JSONDecode(inventory_value.Value);
        for key_122, value_124 in pairs(game_59) do
            local var_1045 = var_1044[key_122];
            if var_1045 and var_1045.type == "Fish" then
                if arg_344 == "Rare" and var_1045.Rare == "Rare" then
                    return true;
                end;
                if arg_344 == "Legendary" and var_1045.Rare == "Legendary" and func_101("Legendary Fish Bait") < 300 then
                    return true;
                end;
            end;
        end;
        return ;
    end);
    return var_1046 and var_1047 or false;
end;
has_fruit = function()
    if not player or (not player.Character or not player.Backpack) then
        return false;
    end;
    for var_1048, var_1049 in next, player.Backpack:GetChildren() do
        if var_1049:GetAttribute("verifiedFruit") and not getgenv().blacklist[var_1049] then
            return var_1049;
        end;
    end;
    for var_1050, var_1051 in next, player.Character:GetChildren() do
        if var_1051:GetAttribute("verifiedFruit") and not getgenv().blacklist[var_1051] then
            return var_1051;
        end;
    end;
    return false;
end;
local tbl_68 = { Vector3.new(3389.60742, 1442.66956, -9062.29297), Vector3.new(7797.435546875, -2018.90625, -17452.791015625), Vector3.new(1836.715087890625, -14.500030517578125, -12258.0048828125) };
local func_130 = function()
    local game_60 = game:GetService("ReplicatedStorage").CompassGuider:FindFirstChild("Traveling Merchant");
    if not game_60 or not game_60.Value then
        return false;
    end;
    for var_1052, var_1053 in next, tbl_68, nil do
        if (var_1053 - game_60.Value).Magnitude < 2000 then
            return true;
        end;
    end;
    return false;
end;
local func_131 = function(arg_345)
    if getgenv().storing_fruit then
        return false;
    end;
    if not arg_345 and (tbl_67.is_running("EquipRod") or os.clock() - getgenv().equip_state.LastEquip < 2) then
        return false;
    end;
    tbl_67.queue("EquipRod", function()
        if getgenv().storing_fruit then
            return ;
        end;
        getgenv().equip_state.LastEquip = os.clock();
        local var_1054 = get_rod_instance();
        if not var_1054 then
            local var_1055 = get_rod_inventory();
            if var_1055 then
                tools_event:InvokeServer("equip", var_1055);
                for i_9 = 1, 20, 1 do
                    if getgenv().storing_fruit then
                        return ;
                    end;
                    var_1054 = get_rod_instance();
                    if var_1054 then
                        break;
                    end;
                    task.wait(0.1);
                end;
            end;
        end;
        if not getgenv().storing_fruit and var_1054 and var_1054.Parent ~= player.Character then
            var_1054.Parent = player.Character;
        end;
        return ;
    end);
    return true;
end;
reequip_rod = function()
    if getgenv().storing_fruit then
        return ;
    end;
    local num_94 = os.time();
    if num_94 - getgenv().equip_state.LastReequip < 1 then
        return ;
    end;
    getgenv().equip_state.LastReequip = num_94;
    getgenv().throw_attempts = 0;
    getgenv().reel_attempts = 0;
    if not player.Character then
        return ;
    end;
    local child_64 = player.Character:FindFirstChild("Humanoid");
    if child_64 then
        child_64:UnequipTools();
    end;
    task.wait(0.1);
    if getgenv().storing_fruit then
        return ;
    end;
    local var_1056 = get_rod_instance();
    if var_1056 then
        var_1056.Parent = player.Character;
    end;
    return ;
end;
ensure_only_fruit_equipped = function(arg_346)
    if not player.Character then
        return ;
    end;
    for key_123, value_125 in pairs(player.Character:GetChildren()) do
        if value_125:IsA("Tool") and value_125.Name ~= arg_346 then
            local child_65 = player.Character:FindFirstChild("Humanoid");
            if child_65 then
                child_65:UnequipTools();
            end;
            return ;
        end;
    end;
    return ;
end;
farm_peli = function()
    local game_61 = game:GetService("Players").LocalPlayer;
    local var_1057 = (game_61.Character or game_61.CharacterAdded:Wait()):WaitForChild(var);
    if not getgenv().necklace_timer then
        getgenv().necklace_timer = 0;
    end;
    local func_132 = function(arg_347, arg_348)
        return (Vector3.new(arg_347.X, 0, arg_347.Z) - Vector3.new(arg_348.X, 0, arg_348.Z)).Magnitude;
    end;
    local num_95 = 0;
    pcall(function()
        num_95 = func_38();
        return ;
    end);
    if num_95 >= 3 then
        local var_1058 = func_40();
        local vector_21 = Vector3.new(-546, 9, -3403);
        if var_1058 ~= "Find Sarah's necklace" then
            if func_132(var_1057.Position, vector_21) > 10 then
                horo_tp_pro({ Position = vector_21, Speed = 50 });
            else
                var_1057.CFrame = CFrame.new(vector_21);
                func_64("Find Sarah's necklace");
                getgenv().necklace_timer = tick();
                task.wait(1);
            end;
        elseif tick() - getgenv().necklace_timer > 30 then
            if func_132(var_1057.Position, vector_21) > 10 then
                horo_tp_pro({ Position = vector_21, Speed = 50 });
            else
                var_1057.CFrame = CFrame.new(vector_21);
                func_64("Find Sarah's necklace");
                getgenv().necklace_timer = tick();
                task.wait(1);
            end;
        else
            local flag_81 = nil;
            pcall(function()
                flag_81 = func_73();
                return ;
            end);
            if flag_81 then
                if func_132(var_1057.Position, flag_81.Position) > 10 then
                    horo_tp_pro({ Position = flag_81.Position, Speed = 50 });
                else
                    var_1057.CFrame = flag_81.CFrame;
                end;
                pcall(function()
                    fireproximityprompt(flag_81:FindFirstChildWhichIsA("ProximityPrompt"), true);
                    return ;
                end);
            elseif func_132(var_1057.Position, vector_21) > 10 then
                horo_tp_pro({ Position = vector_21, Speed = 50 });
            else
                var_1057.CFrame = CFrame.new(vector_21);
                game:GetService("ReplicatedStorage").Events.Quest:InvokeServer({ "returnitem" });
                task.wait(1);
            end;
        end;
    else
        pcall(function()
            local flag_82 = nil;
            local num_96 = math.huge;
            local flag_83 = nil;
            for var_1059, var_1060 in next, workspace.Env:GetChildren() do
                if var_1060.Name == "Chest" then
                    local var_1061 = var_1060:GetPivot().Position;
                    local var_1062 = (var_1061 - var_1057.Position).Magnitude;
                    if var_1062 < 500 and var_1061.Y < 50 then
                        local flag_84 = nil;
                        if get_prompt then
                            flag_84 = get_prompt(var_1061);
                        else
                            for key_124, value_126 in pairs(var_1060:GetDescendants()) do
                                if value_126:IsA("ProximityPrompt") then
                                    flag_84 = value_126;
                                    break;
                                end;
                            end;
                        end;
                        if flag_84 and var_1062 < num_96 then
                            flag_82 = var_1060;
                            flag_83 = flag_84;
                            num_96 = var_1062;
                        end;
                    end;
                end;
            end;
            if flag_82 and flag_83 then
                local var_1063 = flag_82:GetPivot().Position;
                if func_132(var_1057.Position, var_1063) > 10 then
                    horo_tp_pro({ Position = var_1063, Speed = 50 });
                else
                    var_1057.CFrame = CFrame.new(var_1063);
                end;
                fireproximityprompt(flag_83);
            end;
            return ;
        end);
    end;
    return ;
end;
getgenv().fish_last_action = getgenv().fish_last_action or 0;
getgenv().fish_reeling = getgenv().fish_reeling or false;
getgenv().fish_stuck_count = getgenv().fish_stuck_count or 0;
getgenv().fish_last_action = getgenv().fish_last_action or 0;
getgenv().delayed = getgenv().delayed or false;
getgenv().reel_attempts = getgenv().reel_attempts or 0;
getgenv().last_hook_state = getgenv().last_hook_state or nil;
pcall(function()
    local child_66 = ReplicatedStorage:FindFirstChild("Fishing"):FindFirstChild("Remotes"):FindFirstChild("Action");
    return ;
end);
animations = { Idle = "rbxassetid://107207137324194", Catch = "rbxassetid://127450553330242", Reel = "rbxassetid://136623058564703", Throw = "rbxassetid://140322334422224" };
loaded_animations = {};
load_anim = function(arg_349)
    if not loaded_animations[arg_349] then
        local var_1064 = Instance.new("Animation");
        var_1064.AnimationId = animations[arg_349];
        loaded_animations[arg_349] = game.Players.LocalPlayer.Character.Humanoid.Animator:LoadAnimation(var_1064);
    end;
    return loaded_animations[arg_349];
end;
play = function(arg_350)
    local var_1065 = load_anim(arg_350);
    if not var_1065.IsPlaying then
        var_1065:Play();
    end;
    return var_1065;
end;
Stop = function(arg_351)
    if loaded_animations[arg_351] and loaded_animations[arg_351].IsPlaying then
        loaded_animations[arg_351]:Stop();
    end;
    return ;
end;
stop_all = function()
    for key_125, value_127 in pairs(loaded_animations) do
        if value_127.IsPlaying then
            value_127:Stop();
        end;
    end;
    return ;
end;
get_animation = function(arg_352)
    return loaded_animations[arg_352];
end;
fish_once = function()
    if getgenv().delayed then
        return ;
    end;
    local game_62 = game:GetService("Players").LocalPlayer;
    local var_1066 = game_62.Character;
    if not var_1066 then
        return ;
    end;
    local child_67 = var_1066:FindFirstChild(var);
    if not child_67 then
        return ;
    end;
    local func_133 = function()
        local child_68 = var_1066:FindFirstChild("Humanoid");
        if child_68 then
            child_68:UnequipTools();
            task.wait(0.2);
            local child_69 = game_62.Backpack:FindFirstChild("Fishing Rod");
            if child_69 then
                child_69.Parent = var_1066;
            end;
        end;
        return ;
    end;
    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Tools"):InvokeServer(unpack({ "equip", "Fishing Rod" }));
    local child_70 = var_1066:FindFirstChild("Fishing Rod") or game_62.Backpack:FindFirstChild("Fishing Rod");
    if not child_70 then
        return ;
    end;
    if child_70.Parent ~= var_1066 then
        local child_71 = var_1066:FindFirstChild("Humanoid");
        if child_71 then
            child_71:EquipTool(child_70);
            getgenv().fish_last_action = tick() + 0.5;
            return ;
        end;
    end;
    local child_72 = workspace.Effects:FindFirstChild(game_62.Name .. "'s hook");
    if not child_72 then
        local workspace_6 = workspace.Env.WaterStuff.Ocean;
        local var_1067 = workspace_6.CFrame;
        local var_1068 = workspace_6.Size;
        local var_1069 = var_1067:PointToObjectSpace(child_67.Position);
        local var_1070 = var_1067:PointToWorldSpace((Vector3.new(math.clamp(var_1069.X, -var_1068.X / 2, var_1068.X / 2), var_1068.Y / 2, math.clamp(var_1069.Z, -var_1068.Z / 2, var_1068.Z / 2))));
        local var_1071 = RaycastParams.new();
        var_1071.FilterType = Enum.RaycastFilterType.Include;
        var_1071.FilterDescendantsInstances = { workspace.Env.WaterStuff };
        local var_1072 = child_67.Position;
        if var_1072.Y < 0 then
            var_1072 = Vector3.new(var_1072.X, 10, var_1072.Z);
        end;
        local workspace_7 = workspace:Raycast(var_1072, Vector3.new(0, -2000, 0), var_1071);
        local var_1073 = workspace_7 and workspace_7.Position + Vector3.new(0, 7, 0) or var_1070 + Vector3.new(0, 7, 0);
        local vector_22 = Vector3.new(var_1073.X, 0, var_1073.Z);
        local var_1074 = vector_22;
        getgenv().watersigma = vector_22;
        play("Throw");
        pcall(function()
            Action:InvokeServer({ Goal = vector_22, Action = "Throw", Bait = best_bait() });
            return ;
        end);
        task.wait(0.5);
        Action:InvokeServer({ Action = "Landed" });
    else
        child_72.CFrame = CFrame.new(getgenv().watersigma);
        if child_72:GetAttribute("Caught") and not getgenv().delayed then
            play("Reel");
            getgenv().delayed = true;
            local num_97 = math.random(7.5, 10);
            task.delay(num_97, function()
                if not getgenv().delayed then
                    return ;
                end;
                pcall(function()
                    Action:InvokeServer({ Action = "Reel" });
                    return ;
                end);
                play("Reel");
                getgenv().reel_attempts = (getgenv().reel_attempts or 0) + 1;
                if getgenv().reel_attempts >= 3 then
                    task.wait(0.3);
                    func_133();
                else
                    task.wait(0.2);
                end;
                getgenv().delayed = false;
                return ;
            end);
        elseif child_72:GetAttribute("MoveMultiplier") and not child_72:GetAttribute("Caught") then
            getgenv().reel_attempts = 0;
            pcall(function()
                play("Catch");
                Action:InvokeServer({ Action = "HookReturning" });
                Action:InvokeServer({ Action = "Cancel" });
                child_72:Destroy();
                return ;
            end);
            getgenv().fish_last_action = tick() + 0.5;
        end;
    end;
    return ;
end;
GetChestClose = function(arg_353)
    local flag_85 = nil;
    local flag_86 = nil;
    for var_1075, var_1076 in next, workspace.Env:GetChildren() do
        if var_1076.Name == "Chest" then
            local var_1077 = (var_1076:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
            local var_1078 = get_prompt(var_1076:GetPivot().Position);
            if var_1078 and var_1077 < arg_353 then
                flag_85 = var_1076;
                flag_86 = var_1078;
                arg_353 = var_1077;
            end;
        end;
    end;
    return flag_85, flag_86;
end;
player_pos = function()
    return player.Character.HumanoidRootPart.Position;
end;
get_distance = function(arg_354)
    if typeof(arg_354) == "Instance" then
        return (player_pos() - arg_354:GetPivot().Position).Magnitude;
    end;
    if typeof(arg_354) == "Vector3" then
        return (player_pos() - arg_354).Magnitude;
    end;
    if typeof(arg_354) == "CFrame" then
        return (player_pos() - arg_354.Position).Magnitude;
    end;
    return nil;
end;
getgenv().last_merchant = 0;
tbl_68 = { Vector3.new(-2964, 6, -6672) };
local func_134 = function()
    local game_63 = game:GetService("ReplicatedStorage").CompassGuider:FindFirstChild("Traveling Merchant");
    if not game_63 or not game_63.Value then
        return false;
    end;
    for var_1079, var_1080 in next, tbl_68, nil do
        if (var_1080 - game_63.Value).Magnitude < 500 then
            return true;
        end;
    end;
    return false;
end;
has_merchant = function()
    if func_134() then
        warn("merchant is at a blacklisted location.");
        return false;
    end;
    local game_64 = game:GetService("ReplicatedStorage").CompassGuider["Traveling Merchant"];
    if game_64 and game_64:GetAttribute("isEnabled") and getgenv().last_merchant ~= game_64.Value then
        return game_64.Value;
    end;
    return false;
end;
merchnatposition = function()
    local game_65 = game:GetService("ReplicatedStorage").CompassGuider["Traveling Merchant"];
    if game_65 then
        return game_65.Value;
    end;
    return false;
end;
get_price = function(arg_355)
    local game_66 = game:GetService("Players").LocalPlayer.PlayerGui.MerchentShop.Main.List.Redeemables;
    if game_66:FindFirstChild(arg_355) then
        local child_73 = game_66:FindFirstChild(arg_355):FindFirstChild("Price", true).Text;
        return tonumber((child_73:gsub("[^%d]", "")));
    end;
    return ;
end;
can_afford_stock = function(arg_356)
    local var_1081 = get_price(arg_356);
    local var_1082 = get_stock(arg_356);
    if var_1082 == 0 then
        return false;
    end;
    return func_36() >= var_1081 * var_1082;
end;
get_time = function()
    local var_1083, var_1084 = game:GetService("Players").LocalPlayer.PlayerGui.MerchentShop.Main.List.RefreshFrame.RefreshTimer.Text:match("Refresh in: (%d+):(%d+)");
    return tonumber(var_1083) * 60 + tonumber(var_1084);
end;
equip_tool = function(arg_357)
    player.Character.Humanoid:EquipTool(arg_357);
    return ;
end;
getgenv().blacklisted_buyables = {};
getgenv().tuff_delay = false;
getgenv().busy = false;
getgenv().last_most = nil;
wait_for_char = function()
    local var_1085 = player.Character;
    if not var_1085 or (not var_1085:FindFirstChild("Humanoid") or var_1085.Humanoid.Health <= 0) then
        var_1085 = player.CharacterAdded:Wait();
    end;
    var_1085:WaitForChild("Humanoid");
    var_1085:WaitForChild(var);
    return var_1085;
end;
get_char = function()
    local var_1086 = player.Character;
    if not var_1086 or (not var_1086:FindFirstChild("Humanoid") or var_1086.Humanoid.Health <= 0) then
        return wait_for_char();
    end;
    return var_1086;
end;
get_hrp = function()
    return get_char():WaitForChild(var);
end;
hum = function()
    return get_char():WaitForChild("Humanoid");
end;
getgenv().attempts = 0;
if not getgenv().added then
    getgenv().added = player.CharacterAdded:Connect(function()
        getgenv().attempts = 0;
        getgenv().zipl = false;
        getgenv().busy = false;
        return ;
    end);
end;
local tbl_69 = { running = false, target = nil, track_object = nil, cancel = false };
twunu = function(arg_358, arg_359, arg_360)
    local var_1087 = arg_359 or func_67() - 15;
    local flag_87 = nil;
    local flag_88 = nil;
    if typeof(arg_358) == "Vector3" then
        flag_87 = arg_358;
    elseif typeof(arg_358) == "Instance" then
        if arg_358:IsA("ObjectValue") then
            flag_88 = arg_358;
            if not arg_358.Value then
                return ;
            end;
            flag_87 = arg_358.Value:GetPivot().Position;
        else
            flag_88 = arg_358;
            flag_87 = arg_358:GetPivot().Position;
        end;
    end;
    if not flag_87 then
        return ;
    end;
    if tbl_69.running and tbl_69.track_object == flag_88 and flag_88 then
        return ;
    end;
    if tbl_69.running and tbl_69.target and not flag_88 and (Vector3.new(tbl_69.target.X, 0, tbl_69.target.Z) - Vector3.new(flag_87.X, 0, flag_87.Z)).Magnitude < 1 then
        return ;
    end;
    if tbl_69.running then
        tbl_69.target = flag_87;
        tbl_69.track_object = flag_88;
        tbl_69.cancel = true;
        return ;
    end;
    (function()
        local flag_89 = false;
        local var_1088 = player.Character;
        local var_1089 = var_1088 and var_1088:FindFirstChild(var);
        if not var_1089 then
            return ;
        end;
        tbl_69.target = flag_87;
        tbl_69.track_object = flag_88;
        tbl_69.running = true;
        tbl_69.cancel = false;
        getgenv().busy = true;
        player.Character.Humanoid:UnequipTools();
        local var_1090 = arg_360 or var_1089.Position.Y + 600;
        var_1089.CFrame = CFrame.new(var_1089.Position.X, var_1090, var_1089.Position.Z);
        var_1089.Velocity = Vector3.zero;
        var_1089.AssemblyLinearVelocity = Vector3.zero;
        while true do
            while true do
                while true do
                    while true do
                        var_1087 = func_67() - 15;
                        if tbl_69.cancel then
                            break;
                        end;
                        if not getgenv().fish_kaitun.Enabled and not getgenv().auto_santa and not getgenv().auto_impel or getgenv().die then
                            flag_89 = true;
                        end;
                        if flag_89 then
                            break;
                        end;
                        if flag_88 then
                            if flag_88:IsA("ObjectValue") then
                                if not flag_88.Value then
                                    flag_89 = true;
                                end;
                                if not flag_89 then
                                    flag_87 = flag_88.Value:GetPivot().Position;
                                end;
                            else
                                flag_87 = flag_88:GetPivot().Position;
                            end;
                        end;
                        if flag_89 then
                            break;
                        end;
                        local var_1091 = task.wait();
                        local var_1092 = var_1089.Position;
                        local vector_23 = Vector2.new(var_1092.X, var_1092.Z);
                        local vector_24 = Vector2.new(flag_87.X, flag_87.Z);
                        local var_1093 = (vector_24 - vector_23).Magnitude;
                        if var_1093 <= 5 then
                            var_1089.CFrame = CFrame.new(flag_87);
                            var_1089.Velocity = Vector3.zero;
                            var_1089.AssemblyLinearVelocity = Vector3.zero;
                            flag_89 = true;
                        end;
                        if flag_89 then
                            break;
                        end;
                        local var_1094 = vector_23 + (vector_24 - vector_23).Unit * math.min(var_1087 * var_1091, var_1093);
                        var_1089.CFrame = CFrame.new(var_1094.X, var_1090, var_1094.Y);
                        var_1089.Velocity = Vector3.zero;
                        var_1089.AssemblyLinearVelocity = Vector3.zero;
                    end;
                    if flag_89 then
                        break;
                    end;
                    if flag_89 then
                        break;
                    end;
                    if flag_89 then
                        break;
                    end;
                    tbl_69.cancel = false;
                    flag_88 = tbl_69.track_object;
                    if flag_88 then
                        break;
                    end;
                    flag_87 = tbl_69.target;
                end;
                if flag_89 then
                    break;
                end;
                if flag_89 then
                    break;
                end;
                if flag_89 then
                    break;
                end;
                if flag_88:IsA("ObjectValue") then
                    break;
                end;
                flag_87 = flag_88:GetPivot().Position;
            end;
            if flag_89 then
                break;
            end;
            if flag_89 then
                break;
            end;
            if flag_89 then
                break;
            end;
            if not flag_88.Value then
                break;
            end;
            flag_87 = flag_88.Value:GetPivot().Position;
        end;
        flag_89 = false;
        getgenv().busy = false;
        tbl_69.running = false;
        tbl_69.target = nil;
        tbl_69.track_object = nil;
        return ;
    end)();
    return ;
end;
var_1034.create_toggle({ title = "Fish Kaitun Enabled", desc = "Enable fish kaitun farming or disable", default = false }, function(arg_361)
    getgenv().fish_kaitun.Enabled = arg_361;
    task.spawn(function()
        while getgenv().fish_kaitun.Enabled and task.wait(1) do
            local var_1095, var_1096 = best_bait();
            _G.FeralClient.update_bait(var_1095, var_1096);
            if getgenv().fish_kaitun.AutoSell then
                sell_fish();
            end;
            if player.PlayerGui:FindFirstChild("ScreenGui") then
                hum():UnequipTools();
                task.wait(1);
            else
                fake_geppo();
                if getgenv().fish_kaitun.EquipTitle then
                    local var_1097 = get_best_title();
                    local var_1098 = stats_folder and stats_folder.Titles and stats_folder.Titles.EquipedTitle;
                    if var_1098 and var_1098.Value ~= var_1097 then
                        warn(var_1097);
                        events.Titles:InvokeServer(var_1097);
                    end;
                end;
            end;
        end;
        return ;
    end);
    if arg_361 then
        for var_1099, var_1100 in next, workspace.ZipLines:GetDescendants() do
            if var_1100.Name == "ZiplineRails" then
                for var_1101, var_1102 in next, var_1100:GetChildren() do
                    var_1102:Destroy();
                end;
            end;
        end;
    end;
    task.spawn(function()
        while getgenv().fish_kaitun.Enabled and task.wait() do
            if player.PlayerGui:FindFirstChild("ScreenGui") then
                _G.FeralClient.update_fishing_state("Black Screen Unequipping..");
                hum():UnequipTools();
                task.wait(1);
            else
                local var_1103 = get_rod_instance();
                if has_geppo() and var_1103 and not getgenv().busy then
                    if var_1103.Parent ~= get_char() then
                        _G.FeralClient.update_fishing_state("Equipped Rod");
                        equip_tool(var_1103);
                        task.wait(1);
                    else
                        local var_1104 = player.Character.HumanoidRootPart;
                        local workspace_8 = workspace.Env.WaterStuff.Ocean;
                        local var_1105 = workspace_8.CFrame;
                        local var_1106 = workspace_8.Size;
                        local var_1107 = var_1105:PointToObjectSpace(var_1104.Position);
                        local var_1108 = var_1105:PointToWorldSpace((Vector3.new(math.clamp(var_1107.X, -var_1106.X / 2, var_1106.X / 2), var_1106.Y / 2, math.clamp(var_1107.Z, -var_1106.Z / 2, var_1106.Z / 2))));
                        local var_1109 = RaycastParams.new();
                        var_1109.FilterType = Enum.RaycastFilterType.Include;
                        var_1109.FilterDescendantsInstances = { workspace.Env.WaterStuff };
                        local var_1110 = var_1104.Position;
                        if var_1110.Y < 0 then
                            var_1110 = Vector3.new(var_1110.X, 10, var_1110.Z);
                        end;
                        local workspace_9 = workspace:Raycast(var_1110, Vector3.new(0, -2000, 0), var_1109);
                        local var_1111 = workspace_9 and workspace_9.Position + Vector3.new(0, 7, 0) or var_1108 + Vector3.new(0, 7, 0);
                        local vector_25 = Vector3.new(var_1111.X, 0, var_1111.Z);
                        getgenv().waterpos = vector_25;
                        local child_74 = workspace.Effects:FindFirstChild(player.Name .. "'s hook") or nil;
                        if not child_74 then
                            play("Throw");
                            pcall(function()
                                Action:InvokeServer({ Goal = vector_25, Action = "Throw", Bait = best_bait() });
                                return ;
                            end);
                            task.wait(0.5);
                            Action:InvokeServer({ Action = "Landed" });
                            _G.FeralClient.update_fishing_state("Threw Bait");
                            local child_75 = workspace.Effects:FindFirstChild(player.Name .. "'s hook");
                        else
                            if player.Character.HumanoidRootPart.Position.Y > 0 and player.Character.HumanoidRootPart.Position.Y < 800 then
                                child_74.CFrame = CFrame.new(getgenv().waterpos);
                            end;
                            _G.FeralClient.update_fishing_state("Fishing / Idling");
                            if child_74:GetAttribute("Caught") and not getgenv().loop then
                                play("Reel");
                                warn("before reel");
                                getgenv().loop = true;
                                task.delay(7.5, function()
                                    warn("reel thing");
                                    pcall(function()
                                        Action:InvokeServer({ Action = "Reel" });
                                        return ;
                                    end);
                                    play("Reel");
                                    _G.FeralClient.update_fishing_state("Reeled Fish");
                                    getgenv().loop = false;
                                    warn("reel finish");
                                    return ;
                                end);
                            elseif child_74:GetAttribute("MoveMultiplier") and not child_74:GetAttribute("Caught") then
                                pcall(function()
                                    play("Catch");
                                    Action:InvokeServer({ Action = "HookReturning" });
                                    Action:InvokeServer({ Action = "Cancel" });
                                    child_74:Destroy();
                                    return ;
                                end);
                            elseif child_74:GetAttribute("Seed") and not child_74:GetAttribute("MoveMultiplier") and not child_74:GetAttribute("Caught") and func_68(merchnatposition(), 5, true) then
                                repeat
                                    task.wait();
                                until not has_merchant();
                                player.Character.Humanoid:UnequipTools();
                                task.wait();
                                local var_1112;
                                if func_68(Vector3.new(-7427, 26, -793), 1500, true) then
                                    var_1112 = Vector3.new(-1299, 4, -5052);
                                else
                                    var_1112 = workspace.NPCs:FindFirstChild("Robo") and workspace.NPCs:FindFirstChild("Robo"):GetPivot().Position + Vector3.new(-5, 0, 0) or Vector3.new(-1299, 4, -5052);
                                end;
                                repeat
                                    task.wait();
                                    if getgenv().attempts < 5 then
                                        twunu(var_1112);
                                    else
                                        horo_tp(var_1112);
                                    end;
                                until func_68(var_1112, 10, true) or not getgenv().fish_kaitun.Enabled or not player.Character:FindFirstChild("Humanoid");
                                pcall(function()
                                    var_1103.Parent = player.Backpack;
                                    task.wait(0.2);
                                    var_1103.Parent = get_char();
                                    return ;
                                end);
                            end;
                        end;
                    end;
                end;
            end;
        end;
        return ;
    end);
    while task.wait() and getgenv().fish_kaitun.Enabled do
        if not player.Character or not player.Character:FindFirstChild("Humanoid") then
            task.wait(10);
        end;
        local var_1113 = get_rod_instance();
        local var_1114 = has_geppo();
        if not var_1113 then
            if func_36() < 70 then
                if func_38() < 3 then
                    warn("level less than 3 and peli less than 70");
                    local var_1115, var_1116 = GetChestClose(250);
                    if var_1115 then
                        if (var_1115:GetPivot().Position - player_pos()).Magnitude > 5 then
                            _G.FeralClient.update_fishing_state("Moving To Chest");
                            horo_tp2(var_1115:GetPivot().Position);
                        else
                            pcall(function()
                                fireproximityprompt(var_1116);
                                return ;
                            end);
                            _G.FeralClient.update_fishing_state("Opening Chest");
                        end;
                    else
                        warn("bandit?");
                    end;
                else
                    warn("sarah?");
                    if func_40() ~= "Find Sarah's necklace" then
                        necklace_timer = tick();
                        if (Vector3.new(-546, 9, -3403) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                            horo_tp2(Vector3.new(-546, 9, -3403));
                        else
                            func_64("Find Sarah's necklace");
                            task.wait(2);
                        end;
                    elseif tick() - necklace_timer > 30 then
                        if (Vector3.new(-546, 9, -3403) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                            horo_tp2(Vector3.new(-546, 9, -3403));
                        else
                            func_64("Find Sarah's necklace");
                            necklace_timer = tick();
                            task.wait(2);
                        end;
                    else
                        local flag_90 = nil;
                        pcall(function()
                            flag_90 = func_73();
                            return ;
                        end);
                        if flag_90 then
                            horo_tp2(flag_90.Position);
                            if func_68(flag_90.Position, 5, true) then
                                func_70(flag_90.Position);
                                pcall(function()
                                    fireproximityprompt(flag_90:FindFirstChildWhichIsA("ProximityPrompt"), true);
                                    return ;
                                end);
                            end;
                        elseif (Vector3.new(-546, 9, -3403) - player.Character.HumanoidRootPart.Position).Magnitude > 5 then
                            horo_tp2(Vector3.new(-546, 9, -3403));
                        else
                            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Quest"):InvokeServer({ "returnitem" });
                            task.wait(2);
                        end;
                    end;
                end;
            elseif get_distance(Vector3.new(-1340, 3, -4980)) > 5 then
                horo_tp(Vector3.new(-1340, 3, -4980));
            else
                buy("Fishing Rod");
                task.wait();
                for i_10 = 1, 2, 1 do
                    buy("Common Fish Bait");
                end;
            end;
        elseif not var_1114 then
            if get_distance(Vector3.new(-1340, 3, -4980)) > 5 then
                horo_tp(Vector3.new(-1340, 3, -4980));
            else
                task.wait();
                if func_101("Common Fish Bait") < 1 then
                    buy("Common Fish Bait");
                end;
            end;
            if func_36() < 50000 then
                fish_once();
            else
                func_104("skyWalkTrainer");
            end;
        elseif not getgenv().zipl then
            local vector_26 = Vector3.new(-1290, 11, -5089);
            if not func_68(vector_26, 10, true) then
                horo_tp(vector_26);
            elseif player.Character:GetAttribute("SpeedBypass") < 250 then
                if getgenv().attempts < 5 then
                    getgenv().busy = true;
                    player.Character.Humanoid:UnequipTools();
                    func_70(vector_26);
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ZiplineRemote"):InvokeServer(unpack({ "Exit" }));
                    task.wait(1);
                    local tbl_70 = { "Enter", -1, Vector3.new(-1290.2841796875, 18.644325256347656, -5087.14990234375) };
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ZiplineRemote"):InvokeServer(unpack(tbl_70));
                    task.wait(3);
                    getgenv().attempts = getgenv().attempts + 1;
                else
                    getgenv().busy = false;
                    getgenv().zipl = true;
                end;
            elseif player.Character:GetAttribute("SpeedBypass") >= 250 then
                getgenv().busy = false;
                getgenv().zipl = true;
            end;
        elseif has_merchant() and getgenv().fish_kaitun.Merchant then
            _G.FeralClient.update_fishing_state("At Merchant");
            pcall(function()
                local var_1117 = get_distance(has_merchant());
                child_62.Position = has_merchant() + Vector3.new(0, 5, 0);
                if not func_68(has_merchant(), 10, true) then
                    if getgenv().attempts < 5 then
                        twunu(has_merchant());
                    else
                        horo_tp(has_merchant());
                    end;
                else
                    warn("teleporting");
                    get_hrp().CFrame = CFrame.new(child_62.Position + Vector3.new(0, 2.5, 0));
                    if not player.PlayerGui:FindFirstChild("MerchentShop") then
                        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("TravelingMerchentRemote"):InvokeServer("OpenShop");
                    else
                        func_127();
                        if not getgenv().tuff_delay then
                            getgenv().tuff_delay = true;
                            task.delay(get_time(), function()
                                task.wait(20);
                                warn("so is he refreshed?");
                                getgenv().last_merchant = 0;
                                getgenv().last_most = nil;
                                getgenv().tuff_delay = false;
                                return ;
                            end);
                        end;
                        local num_98 = math.huge;
                        local flag_91 = nil;
                        if get_stock("Mythical Fruit Chest") and get_stock("Mythical Fruit Chest") > 0 then
                            flag_91 = "Mythical Fruit Chest";
                        end;
                        if not flag_91 then
                            for var_1118, var_1119 in next, getgenv().fish_kaitun.Buyables, nil do
                                local var_1120 = get_stock(var_1119);
                                local var_1121 = get_price(var_1119);
                                if can_afford_stock(var_1119) and var_1120 > 0 and var_1121 < num_98 and not getgenv().blacklisted_buyables[var_1119] then
                                    flag_91 = var_1119;
                                    num_98 = var_1121;
                                end;
                            end;
                        end;
                        warn(getgenv().last_most, flag_91);
                        if flag_91 ~= getgenv().last_most and flag_91 ~= nil then
                            getgenv().last_most = flag_91;
                            warn("this part");
                            task.spawn(function()
                                send_webhook(flag_91);
                                return ;
                            end);
                            warn("webhook?");
                            _G.FeralClient.send_bought_item(flag_91, get_price(flag_91), "Peli", get_stock(flag_91));
                            warn("sent?");
                        end;
                        if flag_91 then
                            local var_1122 = get_stock(flag_91);
                            for i_11 = 1, var_1122, 1 do
                                get_hrp().CFrame = CFrame.new(child_62.Position + Vector3.new(0, 2.5, 0));
                                task.wait();
                                events.TravelingMerchentRemote:InvokeServer(flag_91, player.PlayerGui:FindFirstChild("MerchentShop"):GetAttribute("Seed"));
                                task.wait();
                                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("TravelingMerchentRemote"):InvokeServer("OpenShop");
                                if get_stock(flag_91) == var_1122 then
                                    getgenv().blacklisted_buyables[flag_91] = true;
                                end;
                                flag_91 = nil;
                            end;
                        else
                            getgenv().last_merchant = game:GetService("ReplicatedStorage").CompassGuider:FindFirstChild("Traveling Merchant").Value;
                        end;
                    end;
                end;
                return ;
            end);
            if not has_merchant() or not getgenv().fish_kaitun.Merchant then
            end;
        elseif getgenv().fish_kaitun.LegendaryBait and can_craft("Legendary") then
            _G.FeralClient.update_fishing_state("Crafting Legendary Bait");
            if get_distance(Vector3.new(-1377, 4, -5063)) < 10 then
                for i_12 = 1, 5, 1 do
                    craft_bait("Legendary");
                end;
            elseif getgenv().attempts < 5 then
                twunu(Vector3.new(-1377, 4, -5063));
            else
                horo_tp(Vector3.new(-1377, 4, -5063));
            end;
        elseif getgenv().fish_kaitun.RareBait and can_craft("Rare") then
            _G.FeralClient.update_fishing_state("Crafting Rare Bait");
            if get_distance(Vector3.new(-1377, 4, -5063)) < 10 then
                for i_13 = 1, 5, 1 do
                    craft_bait("Rare");
                end;
            elseif getgenv().attempts < 5 then
                twunu(Vector3.new(-1377, 4, -5063));
            else
                horo_tp(Vector3.new(-1377, 4, -5063));
            end;
        elseif func_101("Common Fish Bait") < 1 then
            _G.FeralClient.update_fishing_state("Buying Common Fish Bait");
            local game_67 = game.PlaceId == 6811831486 and Vector3.new(102, 10, -55) or Vector3.new(-1340, 3, -4980);
            if get_distance(game_67) > 5 then
                if getgenv().attempts < 5 then
                    twunu(game_67);
                else
                    horo_tp(game_67);
                end;
            else
                buy("Common Fish Bait", getgenv().fish_kaitun.BaitBuyAmount);
            end;
        elseif has_fruit() then
            getgenv().busy = true;
            local var_1123 = has_fruit();
            if func_101(var_1123.Name) > 0 and table.find(getgenv().fish_kaitun.DropFruits, get_rarity(var_1123.Name)) then
                local var_1124 = if not (get_rarity(has_fruit().Name) ~= "Legendary" or Raritiy ~= "Mythic") and Raritiy == "Mythical" then Vector3.new(-1401, 4, -5041) else Vector3.new(-1340, 4, -5035);
                if get_distance(var_1124) > 5 then
                    if getgenv().attempts < 5 then
                        twunu(var_1124);
                    else
                        horo_tp(var_1124);
                    end;
                else
                    events.Tools:InvokeServer("drop", var_1123);
                    _G.FeralClient.update_fishing_state("Dropping Fruit");
                    getgenv().busy = false;
                end;
            else
                local num_99 = 0;
                repeat
                    task.wait(1);
                    if func_101(var_1123.Name) > 0 then
                        break;
                    end;
                    num_99 = num_99 + 1;
                    hum():UnequipTools();
                    task.wait();
                    equip_tool(var_1123);
                    task.wait(0.5);
                    events.FruitStorage:InvokeServer(true);
                    task.wait(1);
                until not var_1123 or num_99 >= 5 or not getgenv().fish_kaitun.Enabled;
                if num_99 >= 5 then
                    warn("blacklisting attempt");
                    _G.FeralClient.update_fishing_state("Blacklisting Fruit");
                    getgenv().blacklist[var_1123] = true;
                end;
                if table.find(getgenv().fish_kaitun.WebhookFruits, get_rarity(var_1123.Name)) then
                    if num_99 >= 5 then
                        send_webhook(var_1123.Name, true, false);
                    else
                        send_webhook(var_1123.Name, true, true);
                    end;
                end;
                getgenv().busy = false;
            end;
        elseif has_bag() then
            getgenv().busy = true;
            repeat
                task.wait();
                local var_1125 = func_71("Spare Fruit Bag");
                if var_1125 then
                    if var_1125.Parent ~= player.Character then
                        player.Character.Humanoid:UnequipTools();
                        task.wait(0.3);
                        var_1125.Parent = player.Character;
                        task.wait(0.3);
                    elseif not player.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                        var_1125:Activate();
                        if var_1125:FindFirstChild("ConfirmationPrompt") then
                            var_1125:FindFirstChild("ConfirmationPrompt").Parent = player.PlayerGui;
                        end;
                    else
                        game:GetService("GuiService").SelectedObject = player.PlayerGui:FindFirstChild("ConfirmationPrompt").Main.Accept;
                        virtual_input_manager:SendKeyEvent(true, Enum.KeyCode.Return, false, nil);
                        virtual_input_manager:SendKeyEvent(false, Enum.KeyCode.Return, false, nil);
                    end;
                else
                    tools_event:InvokeServer("equip", "Spare Fruit Bag");
                end;
            until not var_1125 or not var_1125.Parent or not getgenv().fish_kaitun.Enabled or func_101("Fruit Bag") > 0;
            game:GetService("GuiService").SelectedObject = nil;
            getgenv().busy = false;
        elseif getgenv().fish_kaitun.SetSpawn and stats_folder.Stats.SpawnPoint.Value ~= "Shell's Town" then
            local vector_27 = Vector3.new(-1297, 4, -5058);
            if not func_68(vector_27, 10, true) then
                if getgenv().attempts < 5 then
                    twunu(vector_27);
                else
                    horo_tp(vector_27);
                end;
            else
                func_70(vector_27);
                quest_event:InvokeServer({ "npcChat", true });
                if workspace_service.NPCs:FindFirstChild("Robo") then
                    set_spawn_event:FireServer(nil, workspace_service.NPCs.Robo);
                end;
            end;
        end;
    end;
    return ;
end);
var_1034.create_toggle({ title = "Auto Sell Fish", desc = "Enable whether to sell fish or not.", default = true }, function(arg_362)
    getgenv().fish_kaitun.AutoSell = arg_362;
    return ;
end);
var_1034.create_toggle({ title = "Auto Equip Titles", desc = "Enables wether to equip titles or not.", default = true }, function(arg_363)
    getgenv().fish_kaitun.EquipTitle = arg_363;
    return ;
end);
var_1034.create_toggle({ title = "Auto Set Spawn", desc = "Sets spawn at Shell's Town", default = false }, function(arg_364)
    getgenv().fish_kaitun.SetSpawn = arg_364;
    return ;
end);
var_1034.create_dropdown({ title = "Bait", List = { "Best", "Legendary Fish Bait", "Rare Fish Bait", "Common Fish Bait" }, default = "Best", search = false, selected = false }, function(arg_365, arg_366)
    getgenv().fish_kaitun.Bait = arg_365;
    return ;
end);
var_1034.create_dropdown({ title = "Sell Rarities", List = sell_rarities, default = sell_rarities, search = false, selected = true }, function(arg_367, arg_368)
    local var_1126 = getgenv().fish_kaitun.sell_rarities;
    if arg_368 then
        if not table.find(var_1126, arg_367) then
            table.insert(var_1126, arg_367);
        end;
    else
        for key_126, value_128 in ipairs(var_1126) do
            if value_128 == arg_367 then
                table.remove(var_1126, key_126);
                return ;
            end;
        end;
    end;
    return ;
end);
local var_1127 = page_3.create_section("Fish Kaitun - Bait & Crafting");
var_1127.create_toggle({ title = "Auto Craft Legendary Bait", desc = "Auto Crafts Legendary Bait When You Have Legendary Fish", default = false }, function(arg_369)
    getgenv().fish_kaitun.LegendaryBait = arg_369;
    return ;
end);
var_1127.create_toggle({ title = "Auto Craft Rare Bait", desc = "Auto Crafts Rare Bait When You Have Rare Fish", default = false }, function(arg_370)
    getgenv().fish_kaitun.RareBait = arg_370;
    return ;
end);
var_1127.create_box({ title = "Buy Bait Amount (Common)", placeholder = "50", default = "50", Number = false }, function(arg_371)
    getgenv().fish_kaitun.BaitBuyAmount = arg_371;
    return ;
end);
local var_1128 = page_3.create_section("Fish Kaitun - Merchant & Items");
var_1128.create_toggle({ title = "Auto Buy From Merchant", desc = "Auto buys from the merchant when available", default = false }, function(arg_372)
    getgenv().fish_kaitun.Merchant = arg_372;
    return ;
end);
var_1128.create_toggle({ title = "Auto Store Fruits", desc = "Enable whether to store fruits or not.", default = true }, function(arg_373)
    getgenv().fish_kaitun.StoreFruit = arg_373;
    return ;
end);
getgenv().connected_ball = nil;
var_1128.create_toggle({ title = "Connect Websocket", desc = "Connectes you to the websocket server", default = false }, function(arg_374)
    if getgenv().connected_ball then
        return ;
    end;
    if arg_374 then
        warn("attempting to connect");
        connect();
    else
        disconnect();
    end;
    return ;
end);
var_1128.create_box({ title = "Webhook", placeholder = "https://discord.com/api/webhooks/...", default = "", Number = false }, function(arg_375)
    getgenv().fish_kaitun.Webhook = arg_375;
    return ;
end);
var_1128.create_dropdown({ title = "Drop Fruit Rarities", List = { "Mythical", "Legendary", "Rare", "Epic", "Common", "Uncommon" }, default = { "Mythical", "Legendary", "Rare", "Epic", "Common", "Uncommon" }, search = false, selected = true }, function(arg_376, arg_377)
    local var_1129 = getgenv().fish_kaitun.DropFruits;
    if arg_377 then
        if not table.find(var_1129, arg_376) then
            table.insert(var_1129, arg_376);
        end;
    else
        for key_127, value_129 in ipairs(var_1129) do
            if value_129 == arg_376 then
                table.remove(var_1129, key_127);
                return ;
            end;
        end;
    end;
    return ;
end);
var_1128.create_dropdown({ title = "Webhook Fruit Rarities", List = { "Mythical", "Legendary", "Rare", "Epic", "Common", "Uncommon" }, default = { "Mythical", "Legendary", "Rare", "Epic", "Common", "Uncommon" }, search = false, selected = true }, function(arg_378, arg_379)
    local var_1130 = getgenv().fish_kaitun.WebhookFruits;
    if arg_379 then
        if not table.find(var_1130, arg_378) then
            table.insert(var_1130, arg_378);
        end;
    else
        for key_128, value_130 in ipairs(var_1130) do
            if value_130 == arg_378 then
                table.remove(var_1130, key_128);
                return ;
            end;
        end;
    end;
    return ;
end);
var_1128.create_dropdown({ title = "Buyables", List = tbl, default = {}, search = true, selected = true }, function(arg_380, arg_381)
    local var_1131 = getgenv().fish_kaitun.Buyables;
    if arg_381 then
        if not table.find(var_1131, arg_380) then
            table.insert(var_1131, arg_380);
        end;
    else
        for key_129, value_131 in ipairs(var_1131) do
            if value_131 == arg_380 then
                table.remove(var_1131, key_129);
                return ;
            end;
        end;
    end;
    return ;
end);
local func_135 = function(arg_382)
    if not arg_382 then
        return 0;
    end;
    local var_1132 = arg_382:GetPivot().Position;
    local child_76 = workspace:WaitForChild("Effects");
    local tbl_71 = { ["Zombie Arm"] = true, ["Zombie Leg"] = true, ["Wandering Soul"] = true };
    local num_100 = 10;
    local var_1133 = num_100 * num_100;
    local num_101 = 0;
    for key_130, value_132 in ipairs(child_76:GetChildren()) do
        if value_132:IsA("Model") and value_132.PrimaryPart and (value_132.PrimaryPart.Position - var_1132).Magnitude ^ 2 <= var_1133 then
            for key_131, value_133 in pairs(tbl_71) do
                if value_132:FindFirstChild(key_131) then
                    num_101 = num_101 + 1;
                    break;
                end;
            end;
        end;
    end;
    return num_101;
end;
local func_136 = function(arg_383)
    if not arg_383 or #arg_383 == 0 then
        return ;
    end;
    local local_player_8 = game.Players.LocalPlayer;
    local var_1134 = local_player_8.Character;
    if not var_1134 then
        return ;
    end;
    local var_1135 = var_1134:FindFirstChildOfClass("Humanoid");
    if not var_1135 then
        return ;
    end;
    local flag_92 = nil;
    for var_1136, var_1137 in next, local_player_8.Backpack:GetChildren() do
        if var_1137:GetAttribute("MeleeTool") then
            flag_92 = var_1137;
            break;
        end;
    end;
    for var_1138, var_1139 in next, var_1134:GetChildren() do
        if var_1139:GetAttribute("MeleeTool") then
            flag_92 = var_1139;
            break;
        end;
    end;
    if not flag_92 then
        return ;
    end;
    if flag_92.Parent ~= var_1134 then
        flag_92.Parent = var_1134;
    end;
    if not getgenv().hit_state then
        getgenv().hit_state = { combo = 1, last_tick = 0, next_hit_time = 0 };
    end;
    local var_1140 = getgenv().hit_state;
    local var_1141 = tick();
    if var_1141 < var_1140.next_hit_time or var_1141 - var_1140.last_tick < 0.05 then
        return ;
    end;
    task.spawn(function()
        local var_1142 = tick();
        if var_1142 < var_1140.next_hit_time or var_1142 - var_1140.last_tick < 0.05 then
            return ;
        end;
        var_1140.next_hit_time = var_1142 + 0.5;
        var_1140.last_tick = var_1142;
        local game_68 = game:GetService("ReplicatedStorage");
        local tbl_72 = { "Rokushiki", "Electro" };
        local var_1143 = select(2, getgenv().get_style());
        local child_77 = game_68.CombatAnimations:FindFirstChild(var_1143);
        if not child_77 then
            return ;
        end;
        local child_78 = child_77:FindFirstChild("GroundPunch" .. var_1140.combo) or child_77:FindFirstChild("Punch" .. var_1140.combo) or child_77:FindFirstChild("Punch1");
        if not child_78 then
            return ;
        end;
        local var_1144 = var_1134.PrimaryPart;
        local var_1145 = var_1144 and var_1144.Position or Vector3.zero;
        local num_102 = 0;
        local num_103 = 0;
        local num_104 = 0;
        for var_1146, var_1147 in next, arg_383, nil do
            if typeof(var_1147) == "Instance" and var_1147:IsA("BasePart") and var_1147.Parent then
                num_102, num_103, num_104 = num_102 + var_1147.Position.X, num_103 + var_1147.Position.Z, num_104 + 1;
            end;
        end;
        local var_1148 = num_104 > 0 and CFrame.new(var_1145, Vector3.new(num_102 / num_104, var_1145.Y, num_103 / num_104)) or CFrame.new(var_1145, (workspace.CurrentCamera.CFrame * CFrame.new(0, 0, -1000)).Position);
        local var_1149 = local_player_8:GetAttribute("Ping");
        local var_1150 = typeof(var_1149) == "number" and math.clamp(var_1149, 0, 0.35) or 0.05;
        local var_1151 = var_1135:FindFirstChildOfClass("Animator");
        local var_1152 = var_1151 and var_1151:LoadAnimation(child_78);
        local var_1153 = (var_1152 and var_1152.Length or 0.35) / (var_1152 and var_1152.Speed or 1) * 0.4 - var_1150;
        if var_1153 < 0.03 then
            var_1153 = 0.03;
        end;
        if var_1152 then
            var_1152:Stop();
            var_1152:Destroy();
        end;
        pcall(function()
            game_68.Events.CombatRegister:InvokeServer({ "swingsfx", var_1143, var_1140.combo, "Ground", false, child_78, 1.75, 2, var_1148 });
            return ;
        end);
        task.delay(var_1153, function()
            local tbl_73 = {};
            for var_1154, var_1155 in next, arg_383, nil do
                if typeof(var_1155) == "Instance" and var_1155.Parent then
                    table.insert(tbl_73, var_1155);
                end;
            end;
            if #tbl_73 == 0 then
                return ;
            end;
            local cframe_3 = CFrame.new(var_1145, Vector3.new(num_102 / num_104, var_1145.Y, num_103 / num_104));
            local tbl_74 = { "damage", tbl_73, var_1143, { var_1140.combo, "Ground", var_1143 }, true, aircombo = "Ground", [6] = cframe_3 };
            local var_1156 = game_68.Events.CombatRegister;
            if not pcall(var_1156.InvokeServer, var_1156, tbl_74) then
                task.wait(0.03);
                pcall(var_1156.InvokeServer, var_1156, tbl_74);
            end;
            var_1140.last_tick = tick();
            return ;
        end);
        var_1140.combo = var_1140.combo + 1;
        if var_1140.combo == 4 then
            var_1140.next_hit_time = var_1142 + var_1153 + 0.65;
        elseif var_1140.combo > 5 then
            var_1140.combo = 1;
            var_1140.next_hit_time = var_1142 + var_1153 + 1.5;
        end;
        return ;
    end);
    return ;
end;
local func_137 = function()
    local local_player_9 = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid");
    if not local_player_9 then
        return false;
    end;
    for key_132, value_134 in ipairs(local_player_9:GetPlayingAnimationTracks()) do
        if value_134.Animation and value_134.Animation.AnimationId:match("761034697") then
            return true;
        end;
    end;
    return false;
end;
local var_1157 = getgenv().m1_combo_state or { combo = 1, last_tick = 0, next_hit_time = 0 };
getgenv().m1_combo_state = var_1157;
getgenv().m1_throttle = getgenv().m1_throttle or 0.05;
getgenv().m1_base_next = getgenv().m1_base_next or 0.5;
getgenv().m1_combo3_delay = getgenv().m1_combo3_delay or 0.65;
getgenv().m1_combo_reset_delay = getgenv().m1_combo_reset_delay or 1.5;
local func_138 = function(arg_384)
    local var_1158 = arg_384:GetAttribute("Ping");
    if typeof(var_1158) == "number" then
        return math.clamp(var_1158, 0, 0.35);
    end;
    return 0.05;
end;
local func_139 = function(arg_385, arg_386)
    local var_1159 = type(_G.m1_hit_delay) == "number" and _G.m1_hit_delay or 0.4;
    local var_1160 = arg_385 and arg_385.Length or 0.35;
    local var_1161 = arg_385 and arg_385.Speed or 1;
    local var_1162 = (var_1161 ~= 0 and var_1160 / var_1161 or var_1160) * var_1159 - func_138(arg_386);
    if var_1162 < 0.03 then
        var_1162 = 0.03;
    end;
    return var_1162;
end;
local func_140 = function(arg_387, arg_388)
    local var_1163 = arg_387 and arg_387.PrimaryPart;
    if not var_1163 then
        return CFrame.new();
    end;
    local var_1164 = var_1163.Position;
    local num_105 = 0;
    local num_106 = 0;
    local num_107 = 0;
    if typeof(arg_388) == "Instance" then
        if arg_388:IsA("BasePart") then
            num_105, num_106 = arg_388.Position.X, arg_388.Position.Z;
            num_107 = 1;
        end;
    elseif typeof(arg_388) == "table" then
        for var_1165, var_1166 in next, arg_388, nil do
            if typeof(var_1166) == "Instance" and var_1166:IsA("BasePart") and var_1166.Parent then
                num_105 = num_105 + var_1166.Position.X;
                num_106 = num_106 + var_1166.Position.Z;
                num_107 = num_107 + 1;
            end;
        end;
    end;
    if num_107 == 0 then
        local workspace_10 = workspace.CurrentCamera;
        return CFrame.new(var_1164, (workspace_10.CFrame * CFrame.new(0, 0, -1000)).Position);
    end;
    local vector_28 = Vector3.new(num_105 / num_107, var_1164.Y, num_106 / num_107);
    return CFrame.new(var_1164, vector_28);
end;
getgenv().hit = function(arg_389)
    local game_69 = game:GetService("Players");
    local game_70 = game:GetService("ReplicatedStorage");
    local var_1167 = game_69.LocalPlayer;
    if not var_1167 then
        return ;
    end;
    local var_1168 = var_1167.Character or var_1167.CharacterAdded:Wait();
    if not var_1168 then
        return ;
    end;
    local var_1169 = var_1168:FindFirstChildOfClass("Humanoid");
    local child_79 = var_1168:FindFirstChild(var);
    if not var_1169 or not child_79 then
        return ;
    end;
    local child_80 = game_70:FindFirstChild("Events");
    local var_1170 = child_80 and child_80:FindFirstChild("CombatRegister");
    if not var_1170 then
        return ;
    end;
    if typeof(arg_389) ~= "table" then
        arg_389 = { arg_389 };
    end;
    if #arg_389 == 0 then
        return ;
    end;
    local tbl_75 = {};
    for var_1171, var_1172 in next, arg_389, nil do
        if typeof(var_1172) == "Instance" then
            if var_1172:IsA("Model") then
                local child_81 = var_1172:FindFirstChild(var) or var_1172:FindFirstChild("Torso") or var_1172:FindFirstChild("UpperTorso");
                if child_81 and child_81:IsA("BasePart") then
                    table.insert(tbl_75, child_81);
                end;
            elseif var_1172:IsA("BasePart") then
                table.insert(tbl_75, var_1172);
            end;
        end;
    end;
    if #tbl_75 == 0 then
        return ;
    end;
    local var_1173 = tick();
    local var_1174 = getgenv().m1_throttle;
    if var_1173 < var_1157.next_hit_time then
        return ;
    end;
    if var_1173 - var_1157.last_tick < var_1174 then
        return ;
    end;
    for var_1175, var_1176 in next, var_1167.Backpack:GetChildren() do
        if var_1176:GetAttribute("MeleeTool") then
            local var_1177 = var_1176;
            break;
        end;
    end;
    for var_1178, var_1179 in next, var_1168:GetChildren() do
        if var_1179:GetAttribute("MeleeTool") then
            local var_1180 = var_1179;
            break;
        end;
    end;
    local var_1181 = select(2, getgenv().get_style());
    if not game_70:WaitForChild("CombatAnimations"):FindFirstChild(var_1181) then
        return ;
    end;
    local var_1182 = var_1157.combo;
    local num_108 = 0.1;
    local var_1183 = "Ground";
    local var_1184 = func_140(var_1168, tbl_75);
    local var_1185 = tick();
    var_1157.last_tick = var_1185;
    local var_1186 = getgenv().m1_base_next;
    local var_1187 = getgenv().m1_combo3_delay;
    local var_1188 = getgenv().m1_combo_reset_delay;
    local var_1189 = var_1185 + var_1186;
    local var_1190 = var_1182 + 1;
    if var_1190 == 4 then
        var_1189 = var_1185 + num_108 + var_1187;
    elseif var_1190 > 5 then
        var_1190 = 1;
        var_1189 = var_1185 + num_108 + var_1188;
    end;
    var_1157.combo = var_1190;
    var_1157.next_hit_time = var_1189;
    pcall(function()
        var_1170:InvokeServer({ "swingsfx", var_1181, var_1182, var_1183, false, anim, 1.75, 2, var_1184 });
        return ;
    end);
    task.delay(num_108, function()
        local var_1191 = var_1167.Character;
        local var_1192 = var_1191 and var_1191:FindFirstChildOfClass("Humanoid");
        if not var_1191 or (not var_1192 or var_1192.Health <= 0 or var_1192:GetState() == Enum.HumanoidStateType.Dead) then
            return ;
        end;
        local tbl_76 = {};
        for var_1193, var_1194 in next, tbl_75, nil do
            if typeof(var_1194) == "Instance" and var_1194:IsA("BasePart") and var_1194.Parent then
                table.insert(tbl_76, var_1194);
            end;
        end;
        if #tbl_76 == 0 then
            return ;
        end;
        local var_1195 = func_140(var_1191, tbl_76);
        local tbl_77 = { "damage", tbl_76, var_1181, { var_1182, var_1183, var_1181 }, true, aircombo = var_1183, [6] = var_1195 };
        local var_1196, var_1197 = pcall(var_1170.InvokeServer, var_1170, tbl_77);
        if not var_1196 or var_1197 == nil then
            task.wait(0.03);
            pcall(var_1170.InvokeServer, var_1170, tbl_77);
        end;
        var_1157.last_tick = tick();
        return ;
    end);
    return ;
end;
getgenv().cast_soul_unbound = function(arg_390)
    local game_71 = game.ReplicatedStorage.Events.Skill;
    local var_1198, var_1199 = pcall(game_71.InvokeServer, game_71, "Soul Unbound", arg_390 or CFrame.new(-9638, 172, -1954), true);
    if var_1198 and (type(var_1199) == "table" and typeof(var_1199[1]) == "Instance" and var_1199[1]:IsA("RemoteFunction")) then
        getgenv().horo_return_rf = var_1199[1];
        getgenv().horo_start = tick();
    end;
    return ;
end;
task.spawn(pcall, function()
    player = game.Players.LocalPlayer;
    replicated_storage = game:GetService("ReplicatedStorage");
    NPCs = workspace:FindFirstChild("NPCs");
    return ;
end);
local num_109 = 1;
local num_110 = 0;
local num_111 = 0;
local func_141 = function(arg_391)
    return arg_391 and arg_391:FindFirstChild(var);
end;
local func_142 = function(arg_392, arg_393)
    local var_1200 = arg_392.X - arg_393.X;
    local var_1201 = arg_392.Z - arg_393.Z;
    return var_1200 * var_1200 + var_1201 * var_1201;
end;
local func_143 = function(arg_394, arg_395, arg_396)
    local tbl_78 = {};
    local var_1202 = (typeof(arg_395) == "Vector3" or typeof(arg_395) == "CFrame") and typeof(arg_396) == "number";
    local flag_93 = nil;
    local flag_94 = nil;
    if var_1202 then
        flag_93 = typeof(arg_395) == "CFrame" and arg_395.Position or arg_395;
        flag_94 = arg_396 * arg_396;
    end;
    for key_133, value_135 in ipairs(NPCs:GetChildren()) do
        if value_135.Name == arg_394 then
            local child_82 = value_135:FindFirstChild(var);
            local child_83 = value_135:FindFirstChild("Head");
            if child_82 and child_83 then
                if var_1202 then
                    local var_1203 = child_82.Position - flag_93;
                    if var_1203.X * var_1203.X + var_1203.Y * var_1203.Y + var_1203.Z * var_1203.Z <= flag_94 then
                        tbl_78[#tbl_78 + 1] = value_135;
                    end;
                else
                    tbl_78[#tbl_78 + 1] = value_135;
                end;
            end;
        end;
    end;
    return tbl_78;
end;
local func_144 = function(arg_397, arg_398, arg_399)
    local tbl_79 = {};
    local var_1204 = arg_399 * arg_399;
    for key_134, value_136 in ipairs(arg_397) do
        local var_1205 = func_141(value_136);
        if var_1205 then
            local var_1206 = var_1205.Position.X - arg_398.X;
            local var_1207 = var_1205.Position.Y - arg_398.Y;
            local var_1208 = var_1205.Position.Z - arg_398.Z;
            if var_1206 * var_1206 + var_1207 * var_1207 + var_1208 * var_1208 <= var_1204 then
                tbl_79[#tbl_79 + 1] = value_136;
            end;
        end;
    end;
    return tbl_79;
end;
local func_145 = function(arg_400, arg_401)
    local child_84 = arg_400:FindFirstChild("Info");
    local var_1209 = child_84 and child_84:FindFirstChild("Target");
    local var_1210 = arg_401 and arg_401:FindFirstChild(var);
    return var_1209 and var_1210 and var_1209.Value == var_1210;
end;
local func_146 = function(arg_402, arg_403)
    if #arg_402 == 0 then
        return false;
    end;
    for key_135, value_137 in ipairs(arg_402) do
        if not func_145(value_137, arg_403) then
            return false;
        end;
    end;
    return true;
end;
local func_147 = function(arg_404, arg_405, arg_406)
    local tbl_80 = {};
    local var_1211 = arg_406 * arg_406;
    for key_136, value_138 in ipairs(arg_404) do
        local var_1212 = func_141(value_138);
        if var_1212 and func_142(var_1212.Position, arg_405) <= var_1211 then
            tbl_80[#tbl_80 + 1] = var_1212;
        end;
    end;
    return tbl_80;
end;
local func_148 = function(arg_407)
    for key_137, value_139 in ipairs(arg_407:GetChildren()) do
        if value_139:FindFirstChild("SwordEquip") then
            return value_139, "Sword";
        end;
    end;
    for key_138, value_140 in ipairs(player.Backpack:GetChildren()) do
        if value_140:FindFirstChild("SwordEquip") then
            value_140.Parent = arg_407;
            return value_140, "Sword";
        end;
    end;
    for key_139, value_141 in ipairs(arg_407:GetChildren()) do
        if value_141:GetAttribute("MeleeTool") then
            return value_141;
        end;
    end;
    for key_140, value_142 in ipairs(player.Backpack:GetChildren()) do
        if value_142:GetAttribute("MeleeTool") then
            value_142.Parent = arg_407;
            return value_142;
        end;
    end;
    return ;
end;
local func_149 = function(arg_408, arg_409)
    if not arg_408 then
        return ;
    end;
    if arg_409 then
        return arg_409;
    end;
    if arg_408.Name == "Rokushiki" or arg_408.Name == "Electro" or arg_408.Name == "Vampire" or arg_408:GetAttribute("devilFruit") then
        return "Melee";
    end;
    return arg_408.Name;
end;
local func_150 = function()
    local var_1213 = player:GetAttribute("Ping");
    if typeof(var_1213) ~= "number" then
        return 0.05;
    end;
    if var_1213 < 0 then
        return 0;
    end;
    if var_1213 > 0.35 then
        return 0.35;
    end;
    return var_1213;
end;
local func_151 = function(arg_410)
    local var_1214 = type(_G.m1_hit_delay) == "number" and _G.m1_hit_delay or 0.4;
    local var_1215 = arg_410 and arg_410.Length or 0.35;
    local var_1216 = arg_410 and arg_410.Speed or 1;
    local var_1217 = (var_1216 ~= 0 and var_1215 / var_1216 or var_1215) * var_1214 - func_150();
    if var_1217 < 0.03 then
        var_1217 = 0.03;
    end;
    return var_1217;
end;
local func_152 = function(arg_411, arg_412)
    local var_1218 = arg_411 and arg_411.PrimaryPart;
    if not var_1218 then
        return CFrame.new();
    end;
    local var_1219 = var_1218.Position.X;
    local var_1220 = var_1218.Position.Y;
    local var_1221 = var_1218.Position.Z;
    local num_112 = 0;
    local num_113 = 0;
    local num_114 = 0;
    for key_141, value_143 in ipairs(arg_412) do
        if typeof(value_143) == "Instance" and value_143:IsA("BasePart") and value_143.Parent then
            num_112 = num_112 + value_143.Position.X;
            num_113 = num_113 + value_143.Position.Z;
            num_114 = num_114 + 1;
        end;
    end;
    local vector_29 = Vector3.new(var_1219, var_1220, var_1221);
    if num_114 == 0 then
        local workspace_11 = workspace.CurrentCamera;
        return CFrame.new(vector_29, (workspace_11.CFrame * CFrame.new(0, 0, -1000)).Position);
    end;
    local var_1222 = num_112 / num_114;
    local var_1223 = num_113 / num_114;
    local vector_30 = Vector3.new(var_1222, var_1220, var_1223) - vector_29;
    if vector_30.Magnitude < 0.001 then
        vector_30 = Vector3.new(0, 0, -1);
    end;
    local var_1224 = vector_30.Unit;
    local var_1225 = var_1224 * 1.5;
    local var_1226 = vector_29 + var_1224;
    return CFrame.new(vector_29 + var_1225, var_1226);
end;
local func_153 = function(arg_413, arg_414)
    local var_1227 = arg_413 and arg_413.PrimaryPart;
    if not var_1227 then
        return CFrame.new();
    end;
    local var_1228 = var_1227.Position;
    local num_115 = 0;
    local num_116 = 0;
    local num_117 = 0;
    if typeof(arg_414) == "Instance" then
        if arg_414:IsA("BasePart") then
            num_115, num_116 = arg_414.Position.X, arg_414.Position.Z;
            num_117 = 1;
        end;
    elseif typeof(arg_414) == "table" then
        for var_1229, var_1230 in next, arg_414, nil do
            if typeof(var_1230) == "Instance" and var_1230:IsA("BasePart") and var_1230.Parent then
                num_115 = num_115 + var_1230.Position.X;
                num_116 = num_116 + var_1230.Position.Z;
                num_117 = num_117 + 1;
            end;
        end;
    end;
    if num_117 == 0 then
        local workspace_12 = workspace.CurrentCamera;
        return CFrame.new(var_1228, (workspace_12.CFrame * CFrame.new(0, 0, -1000)).Position);
    end;
    local var_1231 = num_115 / num_117;
    local var_1232 = num_116 / num_117;
    local vector_31 = Vector3.new(var_1231, var_1228.Y, var_1232) - var_1228;
    if vector_31.Magnitude < 0.001 then
        vector_31 = Vector3.new(0, 0, -1);
    end;
    local var_1233 = vector_31.Unit;
    local var_1234 = var_1233 * 1.5;
    local var_1235 = var_1228 + var_1233;
    return CFrame.new(var_1228 + var_1234, var_1235);
end;
local func_154 = function(arg_415, arg_416, arg_417, arg_418, arg_419)
    if not arg_415 or (not arg_417 or not arg_418 or #arg_419 == 0) then
        return ;
    end;
    local var_1236 = tick();
    if var_1236 < num_111 or var_1236 - num_110 < 0.05 then
        return ;
    end;
    local var_1237 = func_149(arg_415, arg_416);
    if not var_1237 then
        return ;
    end;
    local child_85 = replicated_storage:FindFirstChild("Events");
    local var_1238 = child_85 and child_85:FindFirstChild("CombatRegister");
    if not var_1238 then
        return ;
    end;
    local var_1239 = num_109;
    local var_1240 = "Ground";
    local var_1241 = func_152(arg_418, arg_419);
    local num_118 = 0.1;
    num_110 = var_1236;
    local var_1242 = var_1239 + 1;
    local var_1243 = var_1236 + 0.5;
    if var_1242 == 4 then
        var_1243 = var_1236 + num_118 + 0.65;
    elseif var_1242 > 5 then
        var_1242 = 1;
        var_1243 = var_1236 + num_118 + 1.5;
    end;
    num_109 = var_1242;
    num_111 = var_1243;
    pcall(function()
        var_1238:InvokeServer({ "swingsfx", var_1237, var_1239, var_1240, false, nil, 1.75, 2, var_1241 });
        return ;
    end);
    task.delay(num_118, function()
        if not getgenv().auto_baal then
            return ;
        end;
        local var_1244 = player.Character;
        local var_1245 = var_1244 and var_1244:FindFirstChildOfClass("Humanoid");
        if not var_1244 or not var_1245 or var_1245.Health <= 0 or var_1245:GetState() == Enum.HumanoidStateType.Dead then
            return ;
        end;
        local tbl_81 = {};
        for key_142, value_144 in ipairs(arg_419) do
            if typeof(value_144) == "Instance" and value_144:IsA("BasePart") and value_144.Parent then
                tbl_81[#tbl_81 + 1] = value_144;
            end;
        end;
        if #tbl_81 == 0 then
            return ;
        end;
        local var_1246 = func_152(var_1244, tbl_81);
        local tbl_82 = { "damage", tbl_81, var_1237, { var_1239, var_1240, var_1237 }, true, aircombo = var_1240, [6] = var_1246 };
        pcall(function()
            var_1238:InvokeServer(tbl_82);
            return ;
        end);
        num_110 = tick();
        return ;
    end);
    return ;
end;
task.spawn(pcall, function()
    player = game.Players.LocalPlayer;
    replicated_storage = game:GetService("ReplicatedStorage");
    NPCs = workspace:FindFirstChild("NPCs");
    return ;
end);
func_141 = function(arg_420)
    return arg_420 and arg_420:FindFirstChild(var);
end;
func_142 = function(arg_421, arg_422)
    local var_1247 = arg_421.X - arg_422.X;
    local var_1248 = arg_421.Z - arg_422.Z;
    return var_1247 * var_1247 + var_1248 * var_1248;
end;
local func_155 = function(arg_423)
    local tbl_83 = {};
    local local_player_10 = game.Players.LocalPlayer.Character;
    local var_1249 = local_player_10 and local_player_10:FindFirstChild(var);
    if not var_1249 then
        return tbl_83;
    end;
    for key_143, value_145 in ipairs(NPCs:GetChildren()) do
        if value_145.Name == arg_423 then
            local child_86 = value_145:FindFirstChild(var);
            local child_87 = value_145:FindFirstChild("Head");
            if child_86 and child_87 and (child_86.Position - var_1249.Position).Magnitude <= 1000 then
                tbl_83[#tbl_83 + 1] = value_145;
            end;
        end;
    end;
    return tbl_83;
end;
local func_156 = function(arg_424, arg_425, arg_426)
    local tbl_84 = {};
    local var_1250 = arg_426 * arg_426;
    for key_144, value_146 in ipairs(arg_424) do
        local var_1251 = func_141(value_146);
        if var_1251 and func_142(var_1251.Position, arg_425) <= var_1250 then
            tbl_84[#tbl_84 + 1] = value_146;
        end;
    end;
    return tbl_84;
end;
func_145 = function(arg_427, arg_428)
    local child_88 = arg_427:FindFirstChild("Info");
    local var_1252 = child_88 and child_88:FindFirstChild("Target");
    local var_1253 = arg_428 and arg_428:FindFirstChild(var);
    return var_1252 and var_1253 and var_1252.Value == var_1253;
end;
local func_157 = function(arg_429, arg_430)
    if #arg_429 == 0 then
        return false;
    end;
    for key_145, value_147 in ipairs(arg_429) do
        if not func_145(value_147, arg_430) then
            return false;
        end;
    end;
    return true;
end;
local func_158 = function(arg_431, arg_432, arg_433)
    local tbl_85 = {};
    local var_1254 = arg_433 * arg_433;
    for key_146, value_148 in ipairs(arg_431) do
        local var_1255 = func_141(value_148);
        if var_1255 and func_142(var_1255.Position, arg_432) <= var_1254 then
            tbl_85[#tbl_85 + 1] = var_1255;
        end;
    end;
    return tbl_85;
end;
local func_159 = function(arg_434)
    for key_147, value_149 in ipairs(arg_434:GetChildren()) do
        if value_149:FindFirstChild("SwordEquip") then
            return value_149, "Sword";
        end;
    end;
    for key_148, value_150 in ipairs(player.Backpack:GetChildren()) do
        if value_150:FindFirstChild("SwordEquip") then
            value_150.Parent = arg_434;
            return value_150, "Sword";
        end;
    end;
    for key_149, value_151 in ipairs(arg_434:GetChildren()) do
        if value_151:GetAttribute("MeleeTool") then
            return value_151;
        end;
    end;
    for key_150, value_152 in ipairs(player.Backpack:GetChildren()) do
        if value_152:GetAttribute("MeleeTool") then
            value_152.Parent = arg_434;
            return value_152;
        end;
    end;
    return ;
end;
func_149 = function(arg_435, arg_436)
    if not arg_435 then
        return ;
    end;
    if arg_436 then
        return arg_436;
    end;
    if arg_435.Name == "Rokushiki" or arg_435.Name == "Electro" or arg_435.Name == "Vampire" or arg_435:GetAttribute("devilFruit") then
        return "Melee";
    end;
    return arg_435.Name;
end;
local var_1256 = getgenv().m1_combo_state or { combo = 1, last_tick = 0, next_hit_time = 0 };
getgenv().m1_combo_state = var_1256;
getgenv().m1_throttle = getgenv().m1_throttle or 0.05;
getgenv().m1_base_next = getgenv().m1_base_next or 0.5;
getgenv().m1_combo3_delay = getgenv().m1_combo3_delay or 0.65;
getgenv().m1_combo_reset_delay = getgenv().m1_combo_reset_delay or 1.5;
local func_160 = function(arg_437)
    local var_1257 = arg_437:GetAttribute("Ping");
    if typeof(var_1257) == "number" then
        return math.clamp(var_1257, 0, 0.35);
    end;
    return 0.05;
end;
local func_161 = function(arg_438, arg_439)
    return func_153(arg_438, arg_439);
end;
getgenv().hit2 = function(arg_440)
    local game_72 = game:GetService("Players");
    local game_73 = game:GetService("ReplicatedStorage");
    local var_1258 = game_72.LocalPlayer;
    if not var_1258 then
        return ;
    end;
    local var_1259 = var_1258.Character or var_1258.CharacterAdded:Wait();
    if not var_1259 then
        return ;
    end;
    local var_1260 = var_1259:FindFirstChildOfClass("Humanoid");
    local child_89 = var_1259:FindFirstChild(var);
    if not var_1260 or not child_89 then
        return ;
    end;
    local child_90 = game_73:FindFirstChild("Events");
    local var_1261 = child_90 and child_90:FindFirstChild("CombatRegister");
    if not var_1261 then
        return ;
    end;
    if typeof(arg_440) ~= "table" then
        arg_440 = { arg_440 };
    end;
    if #arg_440 == 0 then
        return ;
    end;
    local tbl_86 = {};
    for var_1262, var_1263 in next, arg_440, nil do
        if typeof(var_1263) == "Instance" then
            if var_1263:IsA("Model") then
                local child_91 = var_1263:FindFirstChild(var) or var_1263:FindFirstChild("Torso") or var_1263:FindFirstChild("UpperTorso");
                if child_91 and child_91:IsA("BasePart") then
                    table.insert(tbl_86, child_91);
                end;
            elseif var_1263:IsA("BasePart") then
                table.insert(tbl_86, var_1263);
            end;
        end;
    end;
    if #tbl_86 == 0 then
        return ;
    end;
    local var_1264 = tick();
    local var_1265 = getgenv().m1_throttle;
    if var_1264 < var_1256.next_hit_time then
        return ;
    end;
    if var_1264 - var_1256.last_tick < var_1265 then
        return ;
    end;
    local var_1266, var_1267 = func_159(var_1259);
    if not var_1266 then
        return ;
    end;
    if var_1266:FindFirstChild("SwordEquip") then
        var_1267 = "Sword";
    elseif not var_1267 then
        if table.find({ "Rokushiki", "Electro", "Vampire" }, var_1266.Name) or var_1266:GetAttribute("devilFruit") then
            var_1267 = "Melee";
        else
            var_1267 = var_1266.Name;
        end;
    end;
    local child_92 = game_73:FindFirstChild("CombatAnimations");
    local var_1268 = var_1256.combo;
    local num_119 = 0.1;
    if num_119 < 0.03 then
        num_119 = 0.03;
    end;
    local var_1269 = "Ground";
    local var_1270 = func_161(var_1259, tbl_86);
    local var_1271 = tick();
    var_1256.last_tick = var_1271;
    local var_1272 = getgenv().m1_base_next;
    local var_1273 = getgenv().m1_combo3_delay;
    local var_1274 = getgenv().m1_combo_reset_delay;
    local var_1275 = var_1271 + var_1272;
    local var_1276 = var_1268 + 1;
    if var_1276 == 4 then
        var_1275 = var_1271 + num_119 + var_1273;
    elseif var_1276 > 5 then
        var_1276 = 1;
        var_1275 = var_1271 + num_119 + var_1274;
    end;
    var_1256.combo = var_1276;
    var_1256.next_hit_time = var_1275;
    pcall(function()
        var_1261:InvokeServer({ "swingsfx", var_1267, var_1268, var_1269, false, anim, 1.75, 2, var_1270 });
        return ;
    end);
    task.delay(num_119, function()
        local var_1277 = var_1258.Character;
        local var_1278 = var_1277 and var_1277:FindFirstChildOfClass("Humanoid");
        if not var_1277 or (not var_1278 or var_1278.Health <= 0 or var_1278:GetState() == Enum.HumanoidStateType.Dead) then
            return ;
        end;
        local tbl_87 = {};
        for var_1279, var_1280 in next, tbl_86, nil do
            if typeof(var_1280) == "Instance" and var_1280:IsA("BasePart") and var_1280.Parent then
                table.insert(tbl_87, var_1280);
            end;
        end;
        if #tbl_87 == 0 then
            return ;
        end;
        local var_1281 = func_161(var_1277, tbl_87);
        local tbl_88 = { "damage", tbl_87, var_1267, { var_1268, var_1269, var_1267 }, true, aircombo = var_1269, [6] = var_1281 };
        local var_1282, var_1283 = pcall(var_1261.InvokeServer, var_1261, tbl_88);
        if not var_1282 or var_1283 == nil then
            task.wait(0.03);
            pcall(var_1261.InvokeServer, var_1261, tbl_88);
        end;
        var_1256.last_tick = tick();
        return ;
    end);
    return ;
end;
bad = { "fire", "Alert" };
baal_attack_animations = { ["Roar + Fireballs"] = "rbxassetid://15171985195", Roar = "rbxassetid://15171447546", ["Skeleton Grab"] = "rbxassetid://15172484665", ["Fire breath"] = "rbxassetid://15171535144", ["Rapid Stomp"] = "rbxassetid://15171628089", ultimate = "rbxassetid://15172004593", slam = "rbxassetid://15171272212" };
baal_attack_ids = {};
for key_151, value_153 in pairs(baal_attack_animations) do
    local str_41 = tostring(value_153):match("%d+");
    if str_41 then
        baal_attack_ids[str_41] = key_151;
    end;
end;
last_log = 0;
log_cooldown = 0.5;
log_once = function(arg_441)
    local var_1284 = tick();
    if var_1284 - last_log <= log_cooldown then
        last_log = var_1284;
        print(arg_441);
    end;
    return ;
end;
is_bad = function()
    for var_1285, var_1286 in next, workspace.Effects:GetChildren() do
        if table.find(bad, var_1286.Name) then
            return true;
        end;
    end;
    local child_93 = workspace:FindFirstChild("NPCs");
    if child_93 then
        local child_94 = child_93:FindFirstChild("True Demon Ba'al");
        if child_94 then
            local var_1287 = child_94:FindFirstChildOfClass("Humanoid");
            if var_1287 then
                local var_1288 = var_1287:FindFirstChildOfClass("Animator") or var_1287:FindFirstChild("Animator");
                if var_1288 then
                    for key_152, value_154 in ipairs(var_1288:GetPlayingAnimationTracks()) do
                        if value_154.Animation and value_154.Animation.AnimationId then
                            local str_42 = tostring(value_154.Animation.AnimationId):match("%d+");
                            if str_42 and baal_attack_ids[str_42] then
                                local var_1289 = baal_attack_ids[str_42];
                                return true;
                            end;
                        end;
                    end;
                end;
            end;
        end;
    end;
    return false;
end;
has_bad = function(arg_442)
    hasbad = false;
    for var_1290, var_1291 in next, arg_442:GetChildren() do
        if var_1291:IsA("TextLabel") and var_1291.TextColor == Color3.fromRGB(255, 102, 102) then
            hasbad = true;
        end;
    end;
    return hasbad;
end;
getgenv().webhook_baal = "";
local tbl_89 = {};
local num_120 = 60;
send_baal_webhook = function(arg_443)
    local var_1292 = getgenv().webhook_baal;
    if not var_1292 or var_1292 == "" then
        return ;
    end;
    local var_1293 = request or http_request or syn and syn.request or fluxus and fluxus.request or krnl and krnl.request;
    if not var_1293 then
        return ;
    end;
    local game_74 = game:GetService("Players");
    local game_75 = game:GetService("HttpService");
    local var_1294 = game_74.LocalPlayer;
    if not var_1294 then
        return ;
    end;
    local var_1295 = var_1294.Name;
    local var_1296 = var_1294.UserId;
    local var_1297 = type(arg_443) == "table" and arg_443.Name or type(arg_443) == "string" and arg_443 or "Unknown";
    local var_1298 = tick();
    if tbl_89[var_1297] and var_1298 - tbl_89[var_1297] < num_120 then
        return ;
    end;
    tbl_89[var_1297] = var_1298;
    local num_121 = os.date("!%Y-%m-%dT%H:%M:%S.000Z");
    local var_1299 = game_75:JSONEncode({ username = "Feral | Baal Farm Logs", avatar_url = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQsLa1v6TE3iNJ3WESfDIPVwEji6FTcp6RGfg&s", embeds = { { title = "Baal Farm Log", description = "Baal Farm Kaitun Received x1 " .. tostring(var_1297) .. " from Baal" .. "\nPlayer: " .. var_1295 .. " (" .. tostring(var_1296) .. ")", color = 5814783, timestamp = num_121, footer = { text = "Feral" } } } });
    var_1293({ Url = var_1292, Method = "POST", Headers = { ["Content-Type"] = "application/json" }, Body = var_1299 });
    return ;
end;
getgenv().webhook_santa = nil;
santa_webhook = function(arg_444)
    local var_1300 = getgenv().webhook_santa;
    if not var_1300 or var_1300 == "" then
        warn("here");
        return ;
    end;
    local var_1301 = request or http_request or syn and syn.request or fluxus and fluxus.request or krnl and krnl.request;
    if not var_1301 then
        return ;
    end;
    local game_76 = game:GetService("Players");
    local game_77 = game:GetService("HttpService");
    local var_1302 = game_76.LocalPlayer;
    if not var_1302 then
        return ;
    end;
    local var_1303 = var_1302.Name;
    local var_1304 = var_1302.UserId;
    local var_1305 = type(arg_444) == "table" and arg_444.Name or type(arg_444) == "string" and arg_444 or "Unknown";
    local var_1306 = tick();
    if tbl_89[var_1305] and var_1306 - tbl_89[var_1305] < num_120 then
        return ;
    end;
    tbl_89[var_1305] = var_1306;
    local num_122 = os.date("!%Y-%m-%dT%H:%M:%S.000Z");
    local var_1307 = game_77:JSONEncode({ username = "Feral | Santa Farm Logs \240\159\142\133", avatar_url = "https://static.wikia.nocookie.net/grand-piece-online/images/8/8a/SantaSleigh.png", embeds = { { title = "\240\159\142\132 Santa Farm Log \240\159\142\129", description = "**Santa dropped a present!**\n\n" .. "\240\159\142\129 **Item:** " .. tostring(var_1305) .. "\n\240\159\145\164 **Player:** " .. var_1303 .. " (" .. tostring(var_1304) .. ")", color = 15548997, timestamp = num_122, footer = { text = "\240\159\142\133 Feral Santa Farm" } } } });
    var_1301({ Url = var_1300, Method = "POST", Headers = { ["Content-Type"] = "application/json" }, Body = var_1307 });
    return ;
end;
true_demon_baal_drops = { "True Ba'al's Guard", "Hollow's Great Sword", "True Ba'al's Snake Head", "True Ba'al's Snake Fire", "True Ba'al's Horns", "Hollow's World Ender", "Prestige World Ender", "Endbringer Wings", "Endbringer Armor" };
santa_drops = { "SP Reset Essence", "Dark Root", "Festive Merry Dress", "Frosty Festive Dress", "Iceborn Daggers", "Iceborn Rapier", "Iceborn Blade", "Candy Cane" };
juzo_items = { "Mythical Fruit Chest", "Turtleback Helmet", "Turtleback Armor", "Kira kira no Mi" };
local func_162 = function(arg_445)
    for key_153, value_155 in ipairs(player.Character:GetDescendants()) do
        if value_155:IsA("BasePart") then
            value_155.CanCollide = not arg_445;
        end;
    end;
    return ;
end;
getgenv().webhook_juzo = nil;
local func_163 = function(arg_446)
    local var_1308 = getgenv().webhook_juzo;
    if not var_1308 or var_1308 == "" then
        return ;
    end;
    local var_1309 = request or http_request or syn and syn.request or fluxus and fluxus.request or krnl and krnl.request;
    if not var_1309 then
        return ;
    end;
    local game_78 = game:GetService("Players");
    local game_79 = game:GetService("HttpService");
    local var_1310 = game_78.LocalPlayer;
    if not var_1310 then
        return ;
    end;
    local var_1311 = var_1310.Name;
    local var_1312 = var_1310.UserId;
    local var_1313 = type(arg_446) == "table" and arg_446.Name or type(arg_446) == "string" and arg_446 or "Unknown";
    local var_1314 = tick();
    if tbl_89[var_1313] and var_1314 - tbl_89[var_1313] < num_120 then
        return ;
    end;
    tbl_89[var_1313] = var_1314;
    local num_123 = os.date("!%Y-%m-%dT%H:%M:%S.000Z");
    local var_1315 = game_79:JSONEncode({ username = "Feral | Juzo Farm Logs", avatar_url = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQsLa1v6TE3iNJ3WESfDIPVwEji6FTcp6RGfg&s", embeds = { { title = "Juzo Farm Log", description = "Juzo Farm Kaitun Received x1 " .. tostring(var_1313) .. " from Juzo" .. "\nPlayer: " .. var_1311 .. " (" .. tostring(var_1312) .. ")", color = 5814783, timestamp = num_123, footer = { text = "Feral" } } } });
    var_1309({ Url = var_1308, Method = "POST", Headers = { ["Content-Type"] = "application/json" }, Body = var_1315 });
    return ;
end;
getgenv().last_hit = 0;
getgenv().combo = 1;
getgenv().next_hit_time = 0;
getgenv().combat_state = { Combo = 1, NextAttack = 0, LastHitTime = 0, LastTarget = nil };
getgenv().global_hit = function(arg_447)
    if player.Character.HumanoidRootPart:FindFirstChild("knockback") then
        return ;
    end;
    local var_1316 = getgenv().combat_state;
    local var_1317 = tick();
    local local_player_11 = game.Players.LocalPlayer.Character;
    if not local_player_11 then
        return ;
    end;
    local child_95 = local_player_11:FindFirstChild("Humanoid");
    local child_96 = local_player_11:FindFirstChild(var);
    if not child_95 or child_95.Health <= 0 or not child_96 then
        var_1316.Combo = 1;
        return ;
    end;
    if var_1317 - (var_1316.LastHitTime or 0) > 2.5 then
        var_1316.Combo = 1;
        var_1316.LastTarget = nil;
    end;
    if var_1317 < (var_1316.NextAttack or 0) then
        return ;
    end;
    local func_164 = function(arg_448)
        if not arg_448 then
            return nil;
        end;
        if arg_448:IsA("BasePart") then
            arg_448 = arg_448.Parent;
        end;
        while arg_448 do
            if arg_448:IsA("Model") then
                local child_97 = arg_448:FindFirstChild("Humanoid");
                if child_97 then
                    return child_97;
                end;
            end;
            arg_448 = arg_448.Parent;
        end;
        return nil;
    end;
    local func_165 = function(arg_449)
        if not arg_449 then
            return nil;
        end;
        if arg_449:IsA("BasePart") then
            return arg_449;
        end;
        if arg_449:IsA("Model") then
            if arg_449:FindFirstChild(var) then
                return arg_449.HumanoidRootPart;
            end;
            if arg_449.PrimaryPart then
                return arg_449.PrimaryPart;
            end;
            return arg_449:FindFirstChildWhichIsA("BasePart", true);
        end;
        return nil;
    end;
    local tbl_90 = {};
    local flag_95 = nil;
    if typeof(arg_447) == "table" then
        for var_1318, var_1319 in next, arg_447, nil do
            local var_1320 = func_165(var_1319);
            if var_1320 then
                table.insert(tbl_90, var_1320);
                if not flag_95 then
                    flag_95 = func_164(var_1320);
                end;
            end;
        end;
    else
        local var_1321 = func_165(arg_447);
        if var_1321 then
            table.insert(tbl_90, var_1321);
            flag_95 = func_164(var_1321);
        end;
    end;
    if #tbl_90 == 0 then
        return ;
    end;
    if flag_95 and var_1316.LastTarget and flag_95 ~= var_1316.LastTarget then
        var_1316.Combo = 1;
    end;
    if flag_95 and flag_95.Health <= 0 then
        var_1316.Combo = 1;
    end;
    var_1316.LastTarget = flag_95;
    local var_1322, var_1323 = getgenv().get_style();
    if var_1322 and var_1322.Parent ~= local_player_11 then
        local_player_11.Humanoid:EquipTool(var_1322);
    end;
    local var_1324 = typeof(var_1323) == "string" and var_1323 or "Melee";
    local var_1325 = var_1316.Combo or 1;
    local var_1326 = "Ground";
    local var_1327 = child_96.CFrame;
    local game_80 = game:GetService("ReplicatedStorage").Events.CombatRegister;
    var_1316.LastHitTime = var_1317;
    local num_124 = 0.35;
    if var_1325 == 3 then
        num_124 = 0.4;
    elseif var_1325 == 4 then
        num_124 = 0.8;
    elseif var_1325 == 5 then
        num_124 = 1.7;
    end;
    var_1316.NextAttack = var_1317 + num_124;
    task.spawn(function()
        pcall(function()
            game_80:InvokeServer({ "swingsfx", var_1324, var_1325, var_1326, false, nil, 1.75, 2, var_1327 });
            return ;
        end);
        task.wait(0.15);
        pcall(function()
            game_80:InvokeServer({ "damage", tbl_90, var_1324, { var_1325, var_1326, var_1324 }, true, var_1327, aircombo = var_1326 });
            return ;
        end);
        return ;
    end);
    var_1316.Combo = var_1325 + 1;
    if var_1316.Combo > 5 then
        var_1316.Combo = 1;
    end;
    return ;
end;
getgenv().fighting_style = "Auto";
getgenv().get_style = function()
    char = player.Character;
    if getgenv().fighting_style == "Auto" then
        for key_154, value_156 in ipairs(char:GetChildren()) do
            if value_156:FindFirstChild("SwordEquip") then
                return value_156, "Sword";
            end;
        end;
        for key_155, value_157 in ipairs(var_530.Backpack:GetChildren()) do
            if value_157:FindFirstChild("SwordEquip") then
                value_157.Parent = char;
                return value_157, "Sword";
            end;
        end;
        for key_156, value_158 in ipairs(char:GetChildren()) do
            if value_158:GetAttribute("MeleeTool") then
                return value_158, "Melee";
            end;
        end;
        for key_157, value_159 in ipairs(var_530.Backpack:GetChildren()) do
            if value_159:GetAttribute("MeleeTool") then
                value_159.Parent = char;
                return value_159, "Melee";
            end;
        end;
    elseif getgenv().fighting_style == "Sword" then
        for key_158, value_160 in ipairs(char:GetChildren()) do
            if value_160:FindFirstChild("SwordEquip") then
                return value_160, "Sword";
            end;
        end;
        for key_159, value_161 in ipairs(var_530.Backpack:GetChildren()) do
            if value_161:FindFirstChild("SwordEquip") then
                value_161.Parent = char;
                return value_161, "Sword";
            end;
        end;
    elseif getgenv().fighting_style == "Melee" then
        for key_160, value_162 in ipairs(char:GetChildren()) do
            if value_162:GetAttribute("MeleeTool") then
                return value_162, "Melee";
            end;
        end;
        for key_161, value_163 in ipairs(var_530.Backpack:GetChildren()) do
            if value_163:GetAttribute("MeleeTool") then
                value_163.Parent = char;
                return value_163, "Melee";
            end;
        end;
    end;
    return ;
end;
if not getgenv().tracked_effects then
    getgenv().tracked_effects = {};
end;
if not getgenv().effects_connection then
    local child_98 = workspace:FindFirstChild("Effects");
    if child_98 then
        getgenv().effects_connection = child_98.ChildAdded:Connect(function(arg_450)
            getgenv().tracked_effects[arg_450] = false;
            return ;
        end);
    end;
end;
local func_166 = function()
    return workspace.Islands["Rose Kingdom"].Factory.FrontDoor.Model.Center.CFrame.Position.Y > 120;
end;
local func_167 = function(arg_451)
    local tbl_91 = {};
    for var_1328, var_1329 in next, workspace.NPCs:GetChildren() do
        if (var_1329.Name == "Scientist" or var_1329.Name == "Devil Fruit Scientist") and var_1329:FindFirstChild(var) and var_1329.HumanoidRootPart.Position.Y < arg_451 then
            table.insert(tbl_91, var_1329);
        end;
    end;
    return tbl_91;
end;
local func_168 = function(arg_452)
    if arg_452:FindFirstChild("Info") and arg_452.Info.Target.Value == player.Character.HumanoidRootPart then
        return true;
    end;
    return false;
end;
stage1_wait = Vector3.new(8785, 68, 11891);
stage2_wait = Vector3.new(8785, 290, 11891);
local func_169 = function(arg_453, arg_454, arg_455)
    if player.Character:GetAttribute("SpeedBypass") < 1 then
        return ;
    end;
    local var_1330 = player.Character;
    if not var_1330 then
        return ;
    end;
    local child_99 = var_1330:FindFirstChild(var);
    if not child_99 then
        return ;
    end;
    local var_1331 = child_99.CFrame;
    local var_1332 = arg_453 + Vector3.new(0, 3, 0);
    local var_1333 = child_99.Position;
    local vector_32 = Vector3.new(8812, 66, 11539);
    local var_1334 = (child_99.Position - vector_32).Magnitude;
    if arg_454 and var_1334 <= 100 then
        arg_454 = nil;
    end;
    if arg_454 then
        child_99.CFrame = CFrame.new(child_99.Position.X, arg_454, child_99.Position.Z) * (var_1331 - var_1331.Position);
        var_1333 = child_99.Position;
    end;
    local var_1335 = (Vector2.new(var_1333.X, var_1333.Z) - Vector2.new(arg_453.X, arg_453.Z)).Magnitude;
    local var_1336 = arg_454 or var_1333.Y;
    local var_1337 = var_1335 / 40;
    local num_125 = 2;
    if not arg_455 and var_1335 > 0 then
        num_125 = 1 - 10 / var_1335;
        if var_1335 <= 10 then
            num_125 = 0;
        end;
    end;
    local num_126 = 0;
    while num_126 < 1 do
        if not player.Character or not player.Character:FindFirstChild(var) then
            return ;
        end;
        local var_1338 = num_126 + RunService.Heartbeat:Wait() / var_1337;
        num_126 = math.clamp(var_1338, 0, 1);
        local var_1339 = var_1333.X + num_126 * (var_1332.X - var_1333.X);
        local var_1340 = var_1333.Z + num_126 * (var_1332.Z - var_1333.Z);
        local var_1341 = if not (num_126 < num_125) then var_1336 + (num_125 < 1 and (num_126 - num_125) / (1 - num_125) or 0) * (var_1332.Y - var_1336) else var_1336;
        local vector_33 = Vector3.new(var_1339, var_1341, var_1340);
        child_99.Velocity = Vector3.zero;
        child_99.CFrame = CFrame.new(vector_33) * (var_1331 - var_1331.Position);
    end;
    if player.Character and player.Character:FindFirstChild(var) then
        local var_1342 = num_125 <= 1 and var_1332.Y or var_1336;
        child_99.CFrame = CFrame.new(Vector3.new(var_1332.X, var_1342, var_1332.Z)) * (var_1331 - var_1331.Position);
        child_99.Velocity = Vector3.zero;
    end;
    return ;
end;
local func_170 = function(arg_456, arg_457, arg_458)
    local var_1343 = arg_458 or 20;
    local flag_96 = true;
    for var_1344, var_1345 in next, arg_456, nil do
        if var_1343 < (var_1345:GetPivot().Position - arg_457).Magnitude then
            flag_96 = false;
        end;
    end;
    return flag_96;
end;
connections = library;
local var_1346 = "Feral/Configs";
local var_1347 = var_1346 .. "/autoload.txt";
local var_1348 = var_1346 .. "/autoload_config.txt";
local func_171 = function()
    if not isfolder("Feral") then
        makefolder("Feral");
    end;
    if not isfolder(var_1346) then
        makefolder(var_1346);
    end;
    return ;
end;
local func_172 = function()
    local var_1349, var_1350 = pcall(function()
        if isfile(var_1347) then
            return readfile(var_1347);
        end;
        return ;
    end);
    if var_1349 and type(var_1350) == "string" then
        local var_1351 = var_1350:lower();
        return var_1351 == "true" or var_1351 == "1" or var_1351 == "yes";
    end;
    return false;
end;
local func_173 = function(arg_459)
    func_171();
    writefile(var_1347, arg_459 and "true" or "false");
    return ;
end;
local func_174 = function()
    local var_1352, var_1353 = pcall(function()
        if isfile(var_1348) then
            return readfile(var_1348);
        end;
        return ;
    end);
    if var_1352 and (type(var_1353) == "string" and var_1353 ~= "") then
        return var_1353;
    end;
    return "default";
end;
local func_175 = function(arg_460)
    func_171();
    writefile(var_1348, arg_460 ~= "" and arg_460 or "default");
    return ;
end;
local var_1354 = func_172();
local var_1355 = func_174();
local var_1356 = var_1355 ~= "" and var_1355 or "default";
local var_1357 = config_section.create_toggle({ title = "Auto-Load Config", desc = "If enabled, on execute auto-loads selected config", default = var_1354 }, function(arg_461)
    var_1354 = arg_461 and true or false;
    func_173(var_1354);
    if var_1355 ~= "" and var_1355 then
    end;
    return ;
end);
local var_1358 = config_section.create_box({ title = "Config Name", placeholder = "Enter config name", default = var_1356 }, function(arg_462)
    if arg_462 ~= "" then
        var_1356 = arg_462;
    end;
    return ;
end);
local flag_97 = nil;
local func_176 = function()
    local tbl_92 = {};
    local var_1359, var_1360 = pcall(function()
        return getgenv().feral_config and getgenv().feral_config.List() or {};
    end);
    if var_1359 and typeof(var_1360) == "table" then
        tbl_92 = var_1360;
    end;
    if flag_97 then
        flag_97:get_new_list(tbl_92);
    else
        flag_97 = config_section.create_dropdown({ title = "Existing Configs", List = tbl_92, search = false, selected = false, default = "" }, function(arg_463)
            if arg_463 and arg_463 ~= "" then
                var_1356 = arg_463;
                var_1358.set_value(arg_463);
            end;
            return ;
        end);
    end;
    return ;
end;
func_176();
config_section.create_button({ title = "Save Config" }, function()
    local var_1361 = var_1356 ~= "" and var_1356 or "default";
    local var_1362, var_1363 = getgenv().feral_config.save(var_1361);
    if var_1362 then
        var_1355 = var_1361;
        func_175(var_1361);
        connections.create_noti({ title = "Config", desc = "Saved as \"" .. var_1361 .. "\"", show_time = 5 });
        func_176();
    else
        connections.create_noti({ title = "Config", desc = "Save failed: " .. tostring(var_1363), show_time = 5 });
    end;
    return ;
end);
config_section.create_button({ title = "Load Config" }, function()
    local var_1364 = var_1356 ~= "" and var_1356 or "default";
    local var_1365, var_1366 = getgenv().feral_config.load(var_1364);
    if var_1365 then
        var_1355 = var_1364;
        func_175(var_1364);
        connections.create_noti({ title = "Config", desc = "Loaded \"" .. var_1364 .. "\"", show_time = 5 });
    else
        connections.create_noti({ title = "Config", desc = "Load failed: " .. tostring(var_1366), show_time = 5 });
    end;
    return ;
end);
config_section.create_button({ title = "Delete Config" }, function()
    local var_1367 = var_1356;
    if not var_1367 or var_1367 == "" then
        connections.create_noti({ title = "Config", desc = "No config name selected.", show_time = 5 });
        return ;
    end;
    local var_1368, var_1369 = getgenv().feral_config.delete(var_1367);
    if var_1368 then
        connections.create_noti({ title = "Config", desc = "Deleted \"" .. var_1367 .. "\"", show_time = 5 });
        var_1356 = "default";
        var_1358.set_value(var_1356);
        if var_1355 == var_1367 then
            var_1355 = "default";
            func_175(var_1355);
        end;
        func_176();
    else
        connections.create_noti({ title = "Config", desc = "Delete failed: " .. tostring(var_1369), show_time = 5 });
    end;
    return ;
end);
config_section.create_button({ title = "Refresh Config List" }, function()
    func_176();
    connections.create_noti({ title = "Config", desc = "Config list refreshed.", show_time = 3 });
    return ;
end);
getgenv().sea = "First Sea";
getgenv().ps_code = "";
getgenv().queued = false;
getgenv().sea = "First Sea";
getgenv().queued = false;
http_service = game:GetService("HttpService");
Players = game:GetService("Players");
LocalPlayer = Players.LocalPlayer;
folder_name = "Feral";
account_file_path = folder_name .. "/PSAccounts.json";
if not isfolder(folder_name) then
    makefolder(folder_name);
end;
get_player_ps_code = function()
    if not isfile(account_file_path) then
        return "";
    end;
    local var_1370, var_1371 = pcall(function()
        return http_service:JSONDecode(readfile(account_file_path));
    end);
    if not var_1370 or type(var_1371) ~= "table" then
        return "";
    end;
    local var_1372 = (var_1371.accounts or {})[LocalPlayer.Name];
    if not var_1372 then
        return "";
    end;
    if type(var_1372) == "string" then
        return var_1372;
    end;
    if type(var_1372) == "table" then
        return var_1372.link or var_1372.code or var_1372.Code or var_1372.ps or var_1372.PsCode or "";
    end;
    return "";
end;
saved_accounts = {};
if isfile(account_file_path) then
    local var_1373, var_1374 = pcall(function()
        return http_service:JSONDecode(readfile(account_file_path));
    end);
    if var_1373 and type(var_1374) == "table" then
        saved_accounts = var_1374.accounts or {};
    end;
end;
script_key = script_key or "";
local str_43 = string.format("\t\tgetgenv().Loaded=false\n\t\ttask.wait(30)\n\t\tscript_key = '%s'\n\t\tloadstring(game:HttpGet(\"https://api.luarmor.net/files/v3/loaders/3dbb9943ee4a2c8986099c40d714d81e.lua\"))()\n\t", script_key);
local str_44 = string.format("\t\tgetgenv().Loaded=false\n\t\ttask.wait(10)\n\t\tscript_key = '%s'\n\t\tloadstring(game:HttpGet(\"https://api.luarmor.net/files/v3/loaders/3dbb9943ee4a2c8986099c40d714d81e.lua\"))()\n\t", script_key);
ps_section.create_toggle({ title = "Auto Join PS", desc = "Enable whether to auto join ps or not", default = false }, function(arg_464)
    getgenv().auto_join = arg_464;
    if not arg_464 then
        return ;
    end;
    local var_1375, var_1376;
    repeat
        task.wait();
        var_1375 = get_player_ps_code();
        var_1376 = var_1375 ~= nil and var_1375 ~= "" and var_1375 or getgenv().ps_code or "";
    until var_1376 and var_1376 ~= "" or not getgenv().auto_join;
    warn("PLAYER CODE: " .. var_1375);
    warn("Real Code: " .. var_1376);
    local flag_98 = nil;
    pcall(function()
        flag_98 = player.PlayerGui.Settings.Main.Code.Text;
        return ;
    end);
    if game.PlaceId ~= 1730877806 then
        repeat
            task.wait();
            pcall(function()
                flag_98 = player.PlayerGui.Settings.Main.Code.Text;
                return ;
            end);
        until flag_98 and flag_98 ~= "";
    end;
    if game.PlaceId ~= 1730877806 and flag_98 ~= var_1376 then
        warn("Incorrect PlaceId Moving To Menu Chat Tuff");
        if not getgenv().queued then
            queue_on_teleport(str_44);
            getgenv().queued = true;
            task.wait(1);
        end;
        task.wait(5);
        game:GetService("TeleportService"):Teleport(1730877806, game.Players.LocalPlayer);
        connections.create_noti({ title = "Going To Menu", desc = "yeah going to menu chat..", show_time = 3 });
        return ;
    end;
    if flag_98 == var_1376 then
        warn("We matched code so gg");
        return ;
    end;
    while getgenv().auto_join and task.wait(1) do
        task.spawn(pcall, function()
            var_1375 = get_player_ps_code();
            var_1376 = var_1375 ~= nil and var_1375 ~= "" and var_1375 or getgenv().ps_code or "";
            warn("PLAYER CODE: " .. var_1375);
            warn("Real Code: " .. var_1376);
            return ;
        end);
        task.spawn(function()
            if not getgenv().queued then
                queue_on_teleport(str_43);
                getgenv().queued = true;
                task.wait(1);
            end;
            return ;
        end);
        local var_1377, var_1378 = pcall(function()
            task.spawn(pcall, function()
                if not game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("chooseType") and not game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                    warn("trying to invoke " .. var_1376);
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("reserved"):InvokeServer(var_1376);
                    warn("invoked");
                end;
                return ;
            end);
            pcall(function()
                warn("choosetype part");
                if game:GetService("Players").LocalPlayer.PlayerGui.chooseType.Frame.RemoteEvent then
                    warn("invoked choosetype");
                    game:GetService("Players").LocalPlayer.PlayerGui.chooseType.Frame.RemoteEvent:FireServer(true);
                end;
                return ;
            end);
            pcall(function()
                warn("confirmation thing");
                if game:GetService("Players").LocalPlayer.PlayerGui.ConfirmationPrompt then
                    warn("invoked confirmation");
                    game:GetService("Players").LocalPlayer.PlayerGui.ConfirmationPrompt.RemoteEvent:FireServer(getgenv().sea);
                end;
                return ;
            end);
            return ;
        end);
        if not var_1377 and var_1378 then
            warn("something errored chat" .. var_1378);
        end;
    end;
    return ;
end);
getgenv().autorejoin = false;
ps_section.create_toggle({ title = "Auto Rejoin", desc = "Auto Rejoins When Kicked", default = false }, function(arg_465)
    getgenv().autorejoin = arg_465;
    return ;
end);
local func_177 = function()
    local tbl_93 = {};
    for var_1379, var_1380 in next, workspace.NPCs:GetChildren() do
        if var_1380.Name == "Kraken" then
            local flag_99 = nil;
            if var_1380:IsA("Model") then
                flag_99 = var_1380:GetPivot().Position;
            elseif var_1380:IsA("BasePart") then
                flag_99 = var_1380.Position;
            end;
            if flag_99 then
                tbl_93[#tbl_93 + 1] = flag_99;
            end;
        end;
    end;
    if #tbl_93 < 2 then
        return false;
    end;
    for i_14 = 1, #tbl_93, 1 do
        local num_127 = math.huge;
        for i_15 = 1, #tbl_93, 1 do
            if i_14 ~= i_15 then
                local var_1381 = (tbl_93[i_14] - tbl_93[i_15]).Magnitude;
                if var_1381 < num_127 then
                    num_127 = var_1381;
                end;
            end;
        end;
        if num_127 > 100 then
            return true;
        end;
    end;
    return false;
end;
game:GetService("GuiService").ErrorMessageChanged:Connect(function()
    if getgenv().autorejoin then
        local str_45 = tostring(getgenv().ps_code or "");
        local str_46 = tostring(getgenv().script_key or "");
        local str_47 = string.format("repeat task.wait() until game:IsLoaded()\ntask.wait(30)\ngetgenv().PsCode = \"%s\"\ngetgenv().script_key = \"%s\"\ngetgenv().quu = true\nloadstring(game:HttpGet(\"https://api.luarmor.net/files/v3/loaders/3dbb9943ee4a2c8986099c40d714d81e.lua\"))()\nprint(\"loaded\")", str_45, str_46);
        queue_on_teleport(str_47);
        game:GetService("TeleportService"):Teleport(1730877806, game.Players.LocalPlayer);
    end;
    return ;
end);
ps_section.create_box({ title = "Private Server Code", placeholder = "", default = "", Number = false }, function(arg_466)
    getgenv().ps_code = arg_466;
    return ;
end);
ps_section.create_dropdown({ title = "Teleport Sea", List = { "First Sea", "Second Sea" }, default = "First Sea", search = false, selected = false }, function(arg_467, arg_468)
    getgenv().sea = arg_467;
    return ;
end);
local var_1382 = othersection.create_keybind({ title = "Toggle UI", default = Enum.KeyCode.RightShift }, function(arg_469)
    local child_100 = game.CoreGui:FindFirstChild("Feral GUI");
    if child_100 then
        child_100.Enabled = not child_100.Enabled;
    end;
    return ;
end);
othersection.create_account_table({
    title = "Account Table [PS]",
    default = saved_accounts,
    enable_config_binding = true,
    on_account_select = function(arg_470, arg_471, arg_472)
        return ;
    end
}, function(arg_473)
    writefile(account_file_path, http_service:JSONEncode({ accounts = arg_473 }));
    return ;
end);
global_settings.create_dropdown({ title = "Fighting Style", List = { "Auto", "Melee", "Sword" }, default = "Auto", search = false, selected = false }, function(arg_474, arg_475)
    getgenv().fighting_style = arg_474;
    return ;
end);
global_settings.create_slider({ title = "Tween Speed (multiplier)", min = 0.1, max = 1, default = 1, precise = true }, function(arg_476)
    getgenv().tween_speed = arg_476;
    return ;
end);
juzo_section.create_toggle({ title = "Auto Juzo", desc = "Enable or disable Auto Juzo Farm", default = false }, function(arg_477)
    getgenv().auto_juzo = arg_477;
    if not arg_477 then
        return ;
    end;
    local tbl_94 = { ["110432084683680"] = true, ["106700313119224"] = true, ["135302592663206"] = true, ["96218002160034"] = true };
    local vector_34 = Vector3.new(1807, 95, -10850);
    local num_128 = 60;
    task.spawn(function()
        while getgenv().auto_juzo and task.wait(1) do
            fake_geppo();
        end;
        return ;
    end);
    task.spawn(function()
        local num_129 = 5 + 8;
        while getgenv().auto_juzo and task.wait() do
            if not (player.Character:GetAttribute("SpeedBypass") < 1) then
                local child_101 = player.Character:FindFirstChild(var);
                if child_101 then
                    if (Vector3.new(1774, 28, -10600) - child_101.Position).Magnitude < 1000 then
                        local child_102 = workspace.NPCs:FindFirstChild("Juzo the Diamondback");
                        if not child_102 then
                            repeat
                                task.wait(1);
                                local child_103 = workspace.NPCs:FindFirstChild("Juzo the Diamondback");
                                warn("waiting");
                                if (Vector3.new(1774, 28, -10600) - child_101.Position).Magnitude < 10 and not child_103 and workspace:GetAttribute("serverAge") >= 620 then
                                    player:Kick("no juzo found enable auto rejoin it will re-do stuff");
                                end;
                            until child_103;
                        else
                            local var_1383 = child_102:GetPivot().Position;
                            local var_1384 = child_102:GetPivot().LookVector;
                            local var_1385 = child_101.Position;
                            local var_1386 = (Vector3.new(var_1383.X, 0, var_1383.Z) - Vector3.new(var_1385.X, 0, var_1385.Z)).Magnitude;
                            local flag_100 = false;
                            local child_104 = child_102:FindFirstChild("Humanoid");
                            local var_1387 = child_104 and child_104:FindFirstChild("Animator");
                            if var_1387 then
                                for key_162, value_164 in pairs(var_1387:GetPlayingAnimationTracks()) do
                                    if tbl_94[value_164.Animation.AnimationId:match("%d+")] then
                                        flag_100 = true;
                                        break;
                                    end;
                                end;
                            end;
                            if child_102:FindFirstChild("CrystalDashFirstRigDestroy") then
                                flag_100 = true;
                            end;
                            if var_1383.Y > 120 then
                                flag_100 = true;
                            end;
                            local num_130 = math.abs(var_1383.X - vector_34.X) <= num_128 and math.abs(var_1383.Z - vector_34.Z) <= num_128;
                            if flag_100 then
                                if num_130 then
                                    local vector_35 = Vector3.new(vector_34.X, vector_34.Y + 250, vector_34.Z);
                                    child_101.CFrame = CFrame.new(vector_35);
                                    child_101.Velocity = Vector3.zero;
                                else
                                    horo_tp_pro({
                                        Position = vector_34,
                                        Speed = 50,
                                        Stop = function()
                                            return getgenv().auto_juzo;
                                        end
                                    });
                                end;
                            elseif num_129 + 2 < var_1386 then
                                horo_tp_pro({
                                    Position = var_1383,
                                    Speed = 50,
                                    Stop = function()
                                        return getgenv().auto_juzo;
                                    end
                                });
                            else
                                target_position = child_102:GetPivot().Position;
                                child_101.CFrame = CFrame.new(target_position + Vector3.new(0, 8, 0));
                                child_101.Velocity = Vector3.zero;
                                task.spawn(function()
                                    getgenv().global_hit(child_102);
                                    return ;
                                end);
                            end;
                        end;
                    else
                        horo_tp_pro({
                            Position = Vector3.new(1774, 28, -10600),
                            Speed = 50,
                            Stop = function()
                                return getgenv().auto_juzo;
                            end
                        });
                    end;
                end;
            end;
        end;
        return ;
    end);
    return ;
end);
getgenv().ooooo = false;
getgenv().rifle_gamble = nil;
getgenv().juzo_rejoin = true;
juzo_section.create_toggle({ title = "Auto Juzo (Rifle)", desc = "Enable or disable Auto Juzo Farm (Rifle)", default = false }, function(arg_478)
    getgenv().auto_juzo_rifle = arg_478;
    if not arg_478 then
        if getgenv().ttt then
            getgenv().ttt:Disconnect();
            getgenv().ttt = nil;
        end;
        getgenv().ooooo = false;
        return ;
    end;
    task.spawn(pcall, function()
        if arg_478 and not getgenv().made then
            getgenv().made = true;
            Connection = player.PlayerGui.Notifications.DescendantAdded:Connect(function(arg_479)
                if arg_479:IsA("TextLabel") then
                    local var_1388 = arg_479.Text:gsub("[<>]", "");
                    if table.find(juzo_items, var_1388) and getgenv().auto_juzo_rifle then
                        func_163(var_1388);
                    end;
                end;
                return ;
            end);
        end;
        return ;
    end);
    task.spawn(function()
        while getgenv().auto_juzo_rifle and task.wait(1) do
            fake_geppo();
        end;
        return ;
    end);
    task.spawn(function()
        while getgenv().auto_juzo_rifle and task.wait() do
            if not (player.Character:GetAttribute("SpeedBypass") < 1) then
                local child_105 = player.Character:FindFirstChild(var);
                if child_105 then
                    local vector_36 = Vector3.new(1774, 20, -10516);
                    if (vector_36 - child_105.Position).Magnitude < 1000 then
                        local child_106 = workspace.NPCs:FindFirstChild("Juzo the Diamondback");
                        if not child_106 and not getgenv().ooooo then
                            repeat
                                task.wait(1);
                                local child_107 = workspace.NPCs:FindFirstChild("Juzo the Diamondback");
                                warn("waiting");
                                if (vector_36 - child_105.Position).Magnitude < 10 and not child_107 and workspace:GetAttribute("serverAge") >= 620 and not getgenv().ooooo then
                                    if getgenv().juzo_rejoin then
                                        player:Kick("no juzo found enable auto rejoin it will re-do stuff");
                                    else
                                        warn("Waiting for Juzo to respawn...");
                                    end;
                                end;
                            until child_107 or not getgenv().auto_juzo_rifle;
                        elseif child_106 or getgenv().ooooo then
                            warn("maiiiiiiiiin logic");
                            if not func_68(vector_36, 10, true) then
                                warn("in this block 2");
                                horo_tp_pro({
                                    Position = vector_36,
                                    Speed = 50,
                                    Stop = function()
                                        return getgenv().auto_juzo_rifle;
                                    end
                                });
                            else
                                if not getgenv().ttt then
                                    getgenv().ttt = game:GetService("ReplicatedStorage").Replication.Bosses.ChildRemoved:Connect(function(arg_480)
                                        if arg_480.Name:match("Juzo") then
                                            if getgenv().juzo_rejoin then
                                                task.wait(5);
                                                player:Kick("no juzo found enable auto rejoin it will re-do stuff");
                                            else
                                                task.wait(2);
                                                warn("Juzo killed, resetting for next cycle...");
                                                getgenv().ooooo = false;
                                            end;
                                        end;
                                        return ;
                                    end);
                                end;
                                getgenv().ooooo = true;
                                local child_108 = player.Backpack:FindFirstChild("Rifle") or player.Character:FindFirstChild("Rifle");
                                if not child_108 then
                                    tools_event:InvokeServer("equip", "Rifle");
                                    task.wait(0.5);
                                elseif child_108.Parent ~= player.Character then
                                    child_108.Parent = player.Character;
                                    local var_1389 = require(ReplicatedStorage.Modules.GunHandle);
                                    if not getgenv().rifle_gamble then
                                        warn("making");
                                        getgenv().rifle_gamble = var_1389.New("Rifle", player.Character:FindFirstChild("Rifle"));
                                        getgenv().rifle_gamble.Reloaded = true;
                                    end;
                                elseif not getgenv().rifle_gamble.Reloaded then
                                    warn("not reloaded");
                                    tp_tween(vector_36);
                                    getgenv().rifle_gamble:Reload();
                                else
                                    warn("at this block");
                                    if child_106 then
                                        warn("shoot");
                                        tp_tween(vector_36 + Vector3.new(0, 15, 0));
                                        task.wait(0.1);
                                        LPH_NO_VIRTUALIZE(function()
                                            pcall(function()
                                                local gun_remote = game:GetService("ReplicatedStorage").Events.CIcklcon;
                                                local target_npc = workspace.NPCs:FindFirstChild("Juzo the Diamondback");
                                                local gun_hole = player.Character.RifleGun.Hole;
                                                local fire_payload = { Gun = "Rifle", Position = target_npc.Head.Position, Start = gun_hole.CFrame, joe = "true" };
                                                if target_npc.Parent and target_npc:FindFirstChild(var) then
                                                    gun_remote:FireServer("fire", fire_payload);
                                                end;
                                                getgenv().rifle_gamble.Reloaded = false;
                                                getgenv().rifle_gamble.Equiped = true;
                                                getgenv().rifle_gamble.IsAiming = true;
                                                getgenv().rifle_gamble.Ready = true;
                                            end);
                                        end)();
                                        task.wait(0.5);
                                    end;
                                end;
                            end;
                        end;
                    elseif not getgenv().ooooo then
                        warn("at block 3");
                        horo_tp_pro({
                            Position = Vector3.new(1772, 35, -10627),
                            Speed = 50,
                            Stop = function()
                                return getgenv().auto_juzo_rifle;
                            end
                        });
                    end;
                end;
            end;
        end;
        return ;
    end);
    return ;
end);
juzo_section.create_toggle({ title = "Auto Rejoin Juzo", desc = "Enable or disable Auto Rejoin On Juzo Farm", default = true }, function(arg_481)
    getgenv().juzo_rejoin = arg_481;
    return ;
end);
juzo_section.create_box({ title = "Webhook", placeholder = "", default = "", Number = false }, function(arg_482)
    getgenv().webhook_juzo = arg_482;
    return ;
end);
get_ship = function()
    return workspace.Ships:FindFirstChild(player.Name .. "Ship");
end;
getgenv().ship_tween_running = false;
getgenv().cframe_track_tween = function(arg_483, arg_484, arg_485)
    if not player or not player.Character then
        return ;
    end;
    if (player.Character:GetAttribute("SpeedBypass") or 0) < 1 then
        return ;
    end;
    if getgenv().ship_tween_running then
        getgenv().ship_tween_running = false;
        task.wait();
    end;
    if not arg_483 then
        return ;
    end;
    getgenv().ship_tween_running = true;
    local child_109 = player.Character:FindFirstChild(var);
    if not child_109 then
        getgenv().ship_tween_running = false;
        return ;
    end;
    local func_178 = function()
        return arg_483:FindFirstChild(var) or arg_483:FindFirstChild("HRP");
    end;
    local var_1390 = arg_484 or 110;
    local var_1391 = arg_485 or 0;
    local game_81 = game:GetService("RunService");
    while (getgenv().ship_farm or getgenv().baal_farm) and getgenv().ship_tween_running and arg_483 and arg_483.Parent do
        local var_1392 = func_178();
        if not var_1392 then
            getgenv().ship_tween_running = false;
            return ;
        end;
        local var_1393 = var_1392.Position;
        if var_1391 > 0 then
            var_1393 = var_1393 + var_1392.CFrame.LookVector * var_1391;
        end;
        local var_1394 = child_109.Position;
        local vector_37 = Vector3.new(var_1393.X, var_1393.Y, var_1393.Z) - var_1394;
        if vector_37.Magnitude <= 2 then
            break;
        end;
        local var_1395 = vector_37.Unit;
        local var_1396 = game_81.Heartbeat:Wait();
        local var_1397 = var_1394 + var_1395 * var_1390 * var_1396;
        child_109.AssemblyLinearVelocity = Vector3.zero;
        child_109.Velocity = Vector3.zero;
        child_109.CFrame = CFrame.new(var_1397);
    end;
    getgenv().ship_tween_running = false;
    return ;
end;
getgenv().cframe_track_tween_predicted = function(arg_486, arg_487)
    if not player or not player.Character then
        return ;
    end;
    if (player.Character:GetAttribute("SpeedBypass") or 0) < 1 then
        return ;
    end;
    if getgenv().ship_tween_running then
        getgenv().ship_tween_running = false;
        task.wait();
    end;
    if not arg_486 then
        return ;
    end;
    getgenv().ship_tween_running = true;
    local child_110 = player.Character:FindFirstChild(var);
    if not child_110 then
        getgenv().ship_tween_running = false;
        return ;
    end;
    local var_1398 = arg_487 or 110;
    local game_82 = game:GetService("RunService");
    while (getgenv().ship_farm or getgenv().baal_farm) and getgenv().ship_tween_running do
        local var_1399 = child_110.Position;
        local var_1400 = arg_486 - var_1399;
        if var_1400.Magnitude <= 2 then
            break;
        end;
        local var_1401 = var_1400.Unit;
        local var_1402 = game_82.Heartbeat:Wait();
        local var_1403 = var_1399 + var_1401 * var_1398 * var_1402;
        child_110.AssemblyLinearVelocity = Vector3.zero;
        child_110.Velocity = Vector3.zero;
        child_110.CFrame = CFrame.new(var_1403);
    end;
    getgenv().ship_tween_running = false;
    return ;
end;
getgenv().raycast_track_tween = function(arg_488, arg_489)
    local game_83 = game:GetService("Players");
    local game_84 = game:GetService("RunService");
    local game_85 = game:GetService("Workspace");
    local var_1404 = game_83.LocalPlayer;
    if not var_1404 or not var_1404.Character then
        return ;
    end;
    if (var_1404.Character:GetAttribute("SpeedBypass") or 0) < 1 then
        return ;
    end;
    if getgenv().tween_connection then
        getgenv().tween_connection:Disconnect();
        getgenv().tween_connection = nil;
    end;
    getgenv().ship_tween_running = true;
    local var_1405 = arg_489 or 110;
    local child_111 = var_1404.Character:FindFirstChild(var);
    if not child_111 then
        return ;
    end;
    local var_1406 = RaycastParams.new();
    var_1406.FilterType = Enum.RaycastFilterType.Exclude;
    getgenv().tween_connection = game_84.Heartbeat:Connect(function(arg_490)
        if not getgenv().ship_tween_running or not getgenv().ship_farm and not getgenv().baal_farm then
            if getgenv().tween_connection then
                getgenv().tween_connection:Disconnect();
            end;
            getgenv().ship_tween_running = false;
            return ;
        end;
        local var_1407, var_1408;
        if typeof(arg_488) == "Vector3" then
            var_1407 = arg_488;
            var_1408 = CFrame.new(arg_488);
        else
            if typeof(arg_488) ~= "Instance" then
                if getgenv().tween_connection then
                    getgenv().tween_connection:Disconnect();
                end;
                return ;
            end;
            if not arg_488.Parent then
                if getgenv().tween_connection then
                    getgenv().tween_connection:Disconnect();
                end;
                return ;
            end;
            local child_112 = arg_488:FindFirstChild(var) or arg_488:FindFirstChild("HRP");
            if not child_112 then
                return ;
            end;
            var_1407 = child_112.Position;
            var_1408 = child_112.CFrame;
        end;
        for key_163, value_165 in pairs(var_1404.Character:GetDescendants()) do
            if value_165:IsA("BasePart") and value_165.CanCollide then
                value_165.CanCollide = false;
            end;
        end;
        child_111.AssemblyLinearVelocity = Vector3.zero;
        child_111.AssemblyAngularVelocity = Vector3.zero;
        local var_1409 = child_111.Position;
        local vector_38 = Vector3.new(var_1407.X, 0, var_1407.Z) - Vector3.new(var_1409.X, 0, var_1409.Z);
        if vector_38.Magnitude <= 10 then
            child_111.CFrame = var_1408;
            if getgenv().tween_connection then
                getgenv().tween_connection:Disconnect();
            end;
            getgenv().ship_tween_running = false;
            return ;
        end;
        local var_1410 = var_1409 + vector_38.Unit * (var_1405 * arg_490);
        local var_1411 = var_1409.Y;
        local num_131 = math.max(var_1409.Y, var_1407.Y) + 50;
        local vector_39 = Vector3.new(var_1410.X, num_131, var_1410.Z);
        local vector_40 = Vector3.new(0, -500, 0);
        local tbl_95 = { var_1404.Character };
        var_1406.FilterDescendantsInstances = tbl_95;
        local var_1412 = game_85:Raycast(vector_39, vector_40, var_1406);
        local num_132 = 0;
        while var_1412 and var_1412.Instance.Name == "Leaves" and num_132 < 10 do
            table.insert(tbl_95, var_1412.Instance);
            var_1406.FilterDescendantsInstances = tbl_95;
            var_1412 = game_85:Raycast(vector_39, vector_40, var_1406);
            num_132 = num_132 + 1;
        end;
        local var_1413 = if not var_1412 then var_1409.Y else var_1412.Position.Y + 4;
        local vector_41 = Vector3.new(var_1410.X, var_1413, var_1410.Z);
        local vector_42 = Vector3.new(var_1407.X, vector_41.Y, var_1407.Z);
        child_111.CFrame = CFrame.new(vector_41, vector_42);
        return ;
    end);
    return ;
end;
get_captain = function()
    local flag_101 = nil;
    local num_133 = math.huge;
    for var_1414, var_1415 in next, workspace.NPCs:GetChildren() do
        if typeof(var_1415.Name) == "string" and var_1415.Name:match("Captain") then
            local var_1416, var_1417 = pcall(function()
                return var_1415:GetPivot().Position;
            end);
            if var_1416 and var_1417 then
                local var_1418 = (var_1417 - player.Character.HumanoidRootPart.Position).Magnitude;
                if var_1418 < num_133 then
                    flag_101 = var_1415;
                    num_133 = var_1418;
                end;
            end;
        end;
    end;
    return flag_101;
end;
get_cannon = function()
    local flag_102 = nil;
    local num_134 = math.huge;
    for var_1419, var_1420 in next, workspace.NPCs:GetChildren() do
        if typeof(var_1420.Name) == "string" and var_1420.Name:match("Cannoneer") then
            local var_1421, var_1422 = pcall(function()
                return var_1420:GetPivot().Position;
            end);
            if var_1421 and var_1422 then
                local var_1423 = (var_1422 - player.Character.HumanoidRootPart.Position).Magnitude;
                if var_1423 < num_134 then
                    flag_102 = var_1420;
                    num_134 = var_1423;
                end;
            end;
        end;
    end;
    return flag_102;
end;
local func_179 = function()
    local flag_103 = false;
    pcall(function()
        for var_1424, var_1425 in player.PlayerGui.Notifications:GetDescendants() do
            if var_1425:IsA("TextLabel") and (var_1425.Text == "TP" or var_1425.Text == "Strike" or var_1425.Text:find("Strike")) then
                flag_103 = true;
                return ;
            end;
        end;
        return ;
    end);
    return flag_103;
end;
getgenv().cframe_track_tween = function(arg_491, arg_492, arg_493)
    if not player or not player.Character then
        return ;
    end;
    if func_179() then
        task.wait(5);
        return ;
    end;
    if (player.Character:GetAttribute("SpeedBypass") or 0) < 1 then
        return ;
    end;
    if getgenv().ship_tween_running then
        getgenv().ship_tween_running = false;
        task.wait();
    end;
    if not arg_491 then
        return ;
    end;
    getgenv().ship_tween_running = true;
    local child_113 = player.Character:FindFirstChild(var);
    if not child_113 then
        getgenv().ship_tween_running = false;
        return ;
    end;
    local func_180 = function()
        return arg_491:FindFirstChild(var) or arg_491:FindFirstChild("HRP");
    end;
    local var_1426 = arg_492 or 110;
    local var_1427 = arg_493 or 0;
    local game_86 = game:GetService("RunService");
    while (getgenv().ship_farm or getgenv().baal_farm) and getgenv().ship_tween_running and arg_491 and arg_491.Parent do
        local var_1428 = func_180();
        if not var_1428 then
            getgenv().ship_tween_running = false;
            return ;
        end;
        local var_1429 = var_1428.Position;
        if var_1427 > 0 then
            var_1429 = var_1429 + var_1428.CFrame.LookVector * var_1427;
        end;
        local var_1430 = child_113.Position;
        local var_1431 = var_1429 - var_1430;
        if var_1431.Magnitude <= 2 then
            break;
        end;
        local var_1432 = var_1431.Unit;
        local var_1433 = game_86.Heartbeat:Wait();
        local var_1434 = var_1430 + var_1432 * var_1426 * var_1433;
        child_113.AssemblyLinearVelocity = Vector3.zero;
        child_113.Velocity = Vector3.zero;
        child_113.CFrame = CFrame.new(var_1434);
    end;
    getgenv().ship_tween_running = false;
    return ;
end;
ship_section.create_toggle({ title = "Ship Bounty Farm", desc = "Does ship farming for you!", default = false }, function(arg_494)
    getgenv().ship_farm = arg_494;
    if not arg_494 then
        return ;
    end;
    if not has_geppo() then
        library.create_noti({ title = "Uh Oh!", desc = "You need geppo to use this!", show_time = 4 });
        return ;
    end;
    task.spawn(function()
        while getgenv().ship_farm and task.wait(1) do
            pcall(function()
                fake_geppo();
                return ;
            end);
        end;
        return ;
    end);
    task.spawn(function()
        while getgenv().ship_farm and task.wait(0.1) do
            pcall(function()
                if get_ship() then
                    local var_1435 = get_captain();
                    local var_1436 = get_cannon();
                    if var_1435 then
                        getgenv().global_hit(var_1435);
                    elseif var_1436 and getgenv().kill_cannon then
                        getgenv().global_hit(var_1436);
                    end;
                end;
                return ;
            end);
        end;
        return ;
    end);
    while getgenv().ship_farm and task.wait() do
        if workspace.NPCs:FindFirstChild("Shark") then
            for i_16 = 1, 2, 1 do
                pcall(function()
                    getgenv().global_hit(workspace.NPCs.Shark);
                    return ;
                end);
                task.wait(0.5);
            end;
        end;
        if player.Character.Humanoid.SeatPart ~= nil then
            player.Character.Humanoid.SeatPart:Destroy();
        end;
        if (Vector3.new(3730, 18, -11473) - player.Character.HumanoidRootPart.Position).Magnitude > 2000 then
            horo_tp_pro({
                Position = Vector3.new(3730, 18, -11473),
                Speed = 75,
                Stop = function()
                    return getgenv().ship_farm;
                end
            });
        elseif get_ship() then
            local var_1437 = get_captain();
            local var_1438 = get_cannon();
            if var_1437 then
                local child_114 = var_1437:FindFirstChild(var) or var_1437:FindFirstChild("HRP");
                local num_135 = 999;
                if child_114 then
                    num_135 = (child_114.Position - player.Character.HumanoidRootPart.Position).Magnitude;
                end;
                if num_135 > 30 then
                    getgenv().cframe_track_tween(var_1437, 130, 10);
                else
                    getgenv().cframe_track_tween(var_1437, 130, 0);
                end;
            elseif var_1438 and getgenv().kill_cannon then
                local child_115 = var_1438:FindFirstChild(var) or var_1438:FindFirstChild("HRP") or var_1438.PrimaryPart or var_1438:FindFirstChildWhichIsA("BasePart");
                local num_136 = 999;
                if child_115 then
                    num_136 = (child_115.Position - player.Character.HumanoidRootPart.Position).Magnitude;
                end;
                if num_136 > 30 then
                    getgenv().cframe_track_tween(var_1438, 110, 10);
                else
                    getgenv().cframe_track_tween(var_1438, 110, 0);
                end;
            elseif (Vector3.new(3730, 18, -11473) - player.Character.HumanoidRootPart.Position).Magnitude > 20 then
                horo_tp_pro({
                    Position = Vector3.new(3730, 18, -11473),
                    Speed = 75,
                    Stop = function()
                        return getgenv().ship_farm;
                    end
                });
            else
                func_70(Vector3.new(3730, 18, -11473));
            end;
        else
            horo_tp_pro({
                Position = Vector3.new(3730, 18, -11473),
                Speed = 75,
                Stop = function()
                    return getgenv().ship_farm;
                end
            });
            if (Vector3.new(3730, 18, -11473) - player.Character.HumanoidRootPart.Position).Magnitude < 20 then
                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ShipEvents"):WaitForChild("Spawn"):InvokeServer("true");
            end;
        end;
    end;
    return ;
end);
ship_section.create_toggle({ title = "Kill Cannoneers", desc = "Targets ships subordinates when no captains.", default = false }, function(arg_495)
    getgenv().kill_cannon = arg_495;
    return ;
end);
local func_181 = function(arg_496, arg_497, arg_498)
    if not player or (not player.Character or not player.Character:FindFirstChild(var)) then
        return false;
    end;
    local var_1439 = player.Character.HumanoidRootPart.Position;
    if typeof(arg_496) ~= "Vector3" then
        if typeof(arg_496) == "CFrame" then
            arg_496 = arg_496.Position;
        else
            if typeof(arg_496) ~= "Instance" then
                return false;
            end;
            if arg_496:IsA("Model") and arg_496.PrimaryPart then
                arg_496 = arg_496.PrimaryPart.Position;
            else
                if not arg_496:IsA("BasePart") then
                    return false;
                end;
                arg_496 = arg_496.Position;
            end;
        end;
    end;
    if arg_498 then
        local var_1440 = var_1439.X - arg_496.X;
        local var_1441 = var_1439.Z - arg_496.Z;
        return math.sqrt(var_1440 * var_1440 + var_1441 * var_1441) <= arg_497;
    end;
    return (var_1439 - arg_496).Magnitude <= arg_497;
end;
local func_182 = function(arg_499, arg_500, arg_501, arg_502)
    local var_1442, var_1443 = arg_500 or 1000, nil;
    if not player or (not player.Character or not player.Character:FindFirstChild(var)) then
        return nil;
    end;
    local var_1444 = arg_501 or player.Character.HumanoidRootPart.Position;
    for key_164, value_166 in ipairs(workspace.NPCs:GetChildren()) do
        if value_166.Name == arg_499 and value_166:FindFirstChild(var) then
            local child_116 = value_166:FindFirstChild("Info") and value_166.Info:FindFirstChild("Target") and value_166.Info.Target.Value == player.Character.HumanoidRootPart;
            if (arg_502 ~= true or child_116) and (arg_502 ~= false or not child_116) then
                local var_1445 = (value_166.HumanoidRootPart.Position - var_1444).Magnitude;
                if var_1445 <= var_1442 then
                    var_1443 = value_166;
                    var_1442 = var_1445;
                end;
            end;
        end;
    end;
    return var_1443;
end;
if getgenv().tween_dummy and not getgenv().tween_dummy:IsA("Model") then
    getgenv().tween_dummy:Destroy();
    getgenv().tween_dummy = nil;
end;
if not getgenv().tween_dummy then
    local var_1446 = Instance.new("Model");
    var_1446.Name = "TweenDummy";
    local var_1447 = Instance.new("Part");
    var_1447.Name = var;
    var_1447.Anchored = true;
    var_1447.CanCollide = false;
    var_1447.Transparency = 1;
    var_1447.Size = Vector3.new(1, 1, 1);
    var_1447.Parent = var_1446;
    var_1446.PrimaryPart = var_1447;
    var_1446.Parent = game:GetService("Workspace");
    getgenv().tween_dummy = var_1446;
end;
getgenv().raycast_track_tween = function(arg_503, arg_504, arg_505)
    local game_87 = game:GetService("Players");
    local game_88 = game:GetService("RunService");
    local game_89 = game:GetService("Workspace");
    local var_1448 = game_87.LocalPlayer;
    if not var_1448 or not var_1448.Character then
        return ;
    end;
    if (var_1448.Character:GetAttribute("SpeedBypass") or 0) < 1 then
        return ;
    end;
    if getgenv().ship_tween_running and getgenv().active_tween_target == arg_503 then
        return ;
    end;
    if getgenv().tween_connection then
        getgenv().tween_connection:Disconnect();
        getgenv().tween_connection = nil;
    end;
    getgenv().ship_tween_running = true;
    getgenv().active_tween_target = arg_503;
    local var_1449 = arg_504 or 110;
    local child_117 = var_1448.Character:FindFirstChild(var);
    if not child_117 then
        return ;
    end;
    local var_1450 = RaycastParams.new();
    var_1450.FilterType = Enum.RaycastFilterType.Exclude;
    getgenv().tween_connection = game_88.Heartbeat:Connect(function(arg_506)
        if not getgenv().ship_tween_running or not getgenv().ship_farm and not getgenv().baal_farm then
            if getgenv().tween_connection then
                getgenv().tween_connection:Disconnect();
            end;
            getgenv().ship_tween_running = false;
            getgenv().active_tween_target = nil;
            return ;
        end;
        local flag_104 = nil;
        local flag_105 = nil;
        local var_1451 = typeof(arg_503);
        if var_1451 == "Instance" then
            if not arg_503 or not arg_503.Parent then
                if getgenv().tween_connection then
                    getgenv().tween_connection:Disconnect();
                end;
                return ;
            end;
            if arg_503:IsA("Model") then
                local var_1452 = arg_503.PrimaryPart or arg_503:FindFirstChild(var) or arg_503:FindFirstChild("HRP");
                if var_1452 then
                    flag_104 = var_1452.Position;
                    flag_105 = var_1452.CFrame;
                end;
            elseif arg_503:IsA("BasePart") then
                flag_104 = arg_503.Position;
                flag_105 = arg_503.CFrame;
            end;
        elseif var_1451 == "Vector3" then
            flag_104 = arg_503;
        elseif var_1451 == "CFrame" then
            flag_104 = arg_503.Position;
            flag_105 = arg_503;
        end;
        if not flag_104 then
            return ;
        end;
        for key_165, value_167 in pairs(var_1448.Character:GetDescendants()) do
            if value_167:IsA("BasePart") and value_167.CanCollide then
                value_167.CanCollide = false;
            end;
        end;
        child_117.AssemblyLinearVelocity = Vector3.zero;
        child_117.AssemblyAngularVelocity = Vector3.zero;
        local var_1453 = child_117.Position;
        local vector_43 = Vector3.new(flag_104.X, 0, flag_104.Z) - Vector3.new(var_1453.X, 0, var_1453.Z);
        if vector_43.Magnitude <= 10 then
            if arg_505 then
                local vector_44 = Vector3.new(flag_104.X, flag_104.Y + arg_505, flag_104.Z);
                child_117.CFrame = CFrame.new(vector_44, flag_104);
            elseif flag_105 then
                child_117.CFrame = flag_105;
            else
                child_117.CFrame = CFrame.new(flag_104);
            end;
            if getgenv().tween_connection then
                getgenv().tween_connection:Disconnect();
            end;
            getgenv().ship_tween_running = false;
            getgenv().active_tween_target = nil;
            return ;
        end;
        local var_1454 = var_1453 + vector_43.Unit * (var_1449 * arg_506);
        local var_1455 = var_1453.Y;
        local var_1456;
        if arg_505 then
            var_1456 = flag_104.Y + arg_505;
        else
            local num_137 = math.max(var_1453.Y, flag_104.Y) + 50;
            local vector_45 = Vector3.new(var_1454.X, num_137, var_1454.Z);
            local vector_46 = Vector3.new(0, -500, 0);
            local tbl_96 = { var_1448.Character };
            var_1450.FilterDescendantsInstances = tbl_96;
            local var_1457 = game_89:Raycast(vector_45, vector_46, var_1450);
            local num_138 = 0;
            while var_1457 and var_1457.Instance.Name == "Leaves" and num_138 < 10 do
                table.insert(tbl_96, var_1457.Instance);
                var_1450.FilterDescendantsInstances = tbl_96;
                var_1457 = game_89:Raycast(vector_45, vector_46, var_1450);
                num_138 = num_138 + 1;
            end;
            if var_1457 then
                var_1456 = var_1457.Position.Y + 4;
            else
                var_1456 = var_1453.Y;
            end;
        end;
        local vector_47 = Vector3.new(var_1454.X, var_1456, var_1454.Z);
        local vector_48 = Vector3.new(flag_104.X, vector_47.Y, flag_104.Z);
        child_117.CFrame = CFrame.new(vector_47, vector_48);
        return ;
    end);
    return ;
end;
getgenv().farm_thing = function(arg_507, arg_508, arg_509)
    if typeof(arg_509) ~= "Vector3" then
        return ;
    end;
    local game_90 = game:GetService("Players");
    local game_91 = game:GetService("Workspace");
    local var_1458 = game_90.LocalPlayer.Character;
    if not var_1458 or not var_1458:FindFirstChild(var) then
        return ;
    end;
    local var_1459 = var_1458.HumanoidRootPart.Position;
    local child_118 = game_91:FindFirstChild("NPCs") or game_91;
    local tbl_97 = {};
    local tbl_98 = {};
    for key_166, value_168 in pairs(child_118:GetChildren()) do
        if value_168.Name == arg_507 and value_168:FindFirstChild(var) and value_168:FindFirstChild("Humanoid") and value_168.Humanoid.Health > 0 and (value_168.HumanoidRootPart.Position - arg_509).Magnitude <= arg_508 then
            table.insert(tbl_97, value_168);
            if not func_168(value_168) then
                table.insert(tbl_98, value_168);
            end;
        end;
    end;
    local func_183 = function()
        local tbl_99 = {};
        for key_167, value_169 in pairs(child_118:GetChildren()) do
            if value_169:FindFirstChild(var) and value_169:FindFirstChild("Humanoid") and value_169.Humanoid.Health > 0 and (value_169.HumanoidRootPart.Position - var_1459).Magnitude <= 20 then
                table.insert(tbl_99, value_169);
            end;
        end;
        if #tbl_99 > 0 then
            pcall(function()
                getgenv().global_hit(tbl_99);
                return ;
            end);
        end;
        return ;
    end;
    if #tbl_97 > 0 then
        if #tbl_98 == 0 then
            if getgenv().tween_dummy and getgenv().tween_dummy:FindFirstChild(var) then
                local num_139 = os.clock() * 6;
                local num_140 = 6;
                local num_141 = math.cos(num_139) * num_140;
                local num_142 = math.sin(num_139) * num_140;
                local vector_49 = Vector3.new(arg_509.X + num_141, arg_509.Y, arg_509.Z + num_142);
                getgenv().tween_dummy.HumanoidRootPart.Position = vector_49;
                getgenv().raycast_track_tween(getgenv().tween_dummy, 50);
            end;
            func_183();
        else
            local flag_106 = nil;
            local num_143 = math.huge;
            for key_168, value_170 in pairs(tbl_98) do
                local var_1460 = (var_1459 - value_170.HumanoidRootPart.Position).Magnitude;
                if var_1460 < num_143 then
                    flag_106 = value_170;
                    num_143 = var_1460;
                end;
            end;
            if flag_106 then
                getgenv().raycast_track_tween(flag_106, 50);
                func_183();
            end;
        end;
    else
        getgenv().raycast_track_tween(arg_509, 50);
    end;
    return ;
end;
getgenv().is_playing_bad = function(arg_510)
    if not arg_510 then
        return false;
    end;
    local child_119 = arg_510:FindFirstChild("Humanoid");
    local child_120 = arg_510:FindFirstChild(var);
    if child_119 and child_119.WalkSpeed == 4 then
        return true;
    end;
    if child_120 and child_120:FindFirstChildOfClass("BodyVelocity") and child_120:FindFirstChildOfClass("BodyVelocity").Name ~= "geppo" and child_120:FindFirstChildOfClass("BodyVelocity").Name ~= "knockback" then
        return true;
    end;
    local var_1461 = child_119 and child_119:FindFirstChild("Animator");
    if var_1461 then
        local tbl_100 = { ["15112045405"] = true, ["15112109115"] = true };
        for key_169, value_171 in pairs(var_1461:GetPlayingAnimationTracks()) do
            if tbl_100[value_171.Animation.AnimationId:match("%d+")] then
                return true;
            end;
        end;
    end;
    return false;
end;
getgenv().is_playing_bad2 = function(arg_511)
    if not arg_511 then
        return ;
    end;
    local flag_107 = nil;
    pcall(function()
        flag_107 = arg_511:FindFirstChild("Humanoid");
        return ;
    end);
    if not flag_107 then
        return false;
    end;
    local var_1462 = flag_107 and flag_107:FindFirstChild("Animator");
    if var_1462 then
        local tbl_101 = { ["15171447546"] = true, ["15171535144"] = true, ["15172004593"] = true, ["15172484665"] = true, ["15171272212"] = true, ["15171985195"] = true };
        for key_170, value_172 in pairs(var_1462:GetPlayingAnimationTracks()) do
            if tbl_101[value_172.Animation.AnimationId:match("%d+")] then
                return true;
            end;
        end;
    end;
    return false;
end;
local func_184 = function(arg_512, arg_513)
    local var_1463 = player.Character;
    if not var_1463 or not var_1463:FindFirstChild(var) then
        return ;
    end;
    local var_1464 = var_1463.HumanoidRootPart.Position;
    local var_1465 = (Vector3.new(var_1464.X, 0, var_1464.Z) - Vector3.new(arg_513.X, 0, arg_513.Z)).Magnitude;
    local num_144 = math.random(-25, 25);
    local num_145 = math.random(-25, 25);
    local vector_50 = Vector3.new(arg_513.X + num_144, arg_513.Y + 70, arg_513.Z + num_145);
    local var_1466 = arg_512:GetPivot().Position;
    var_1463.HumanoidRootPart.CFrame = CFrame.new(vector_50, var_1466);
    var_1463.HumanoidRootPart.Velocity = Vector3.zero;
    var_1463.HumanoidRootPart.AssemblyLinearVelocity = Vector3.zero;
    return ;
end;
Players = game:GetService("Players");
workspace_service = game:GetService("Workspace");
LocalPlayer = player;
ray_params = RaycastParams.new();
ray_params.FilterType = Enum.RaycastFilterType.Include;
ray_params.FilterDescendantsInstances = { workspace_service:FindFirstChild("Env"), workspace_service:FindFirstChild("Islands"), workspace_service:FindFirstChild("Ships"), workspace_service:FindFirstChild("Map") };
local func_185 = function(arg_514)
    if not arg_514 then
        return ;
    end;
    local var_1467 = LocalPlayer.Character;
    local var_1468 = var_1467 and var_1467:FindFirstChild(var);
    if not var_1468 then
        return ;
    end;
    local var_1469, var_1470 = arg_514:GetBoundingBox();
    local var_1471 = var_1469.Position;
    local var_1472 = var_1468.Position;
    if (var_1472 - var_1471).Magnitude > 100 then
        return ;
    end;
    local num_146 = 1;
    local var_1473 = var_1472 - var_1471;
    local vector_51 = Vector3.new(var_1473.X, 0, var_1473.Z);
    local var_1474 = if not (vector_51.Magnitude < 0.5) then vector_51.Unit else Vector3.new(0, 0, 1);
    local var_1475 = var_1471 + var_1474 * num_146;
    local var_1476 = var_1475 - var_1471;
    local var_1477 = workspace_service:Raycast(var_1471, var_1476, ray_params);
    if var_1477 then
        local var_1478 = var_1477.Distance;
        var_1475 = var_1471 + var_1474 * math.max(2, var_1478 - 1);
    end;
    var_1468.CFrame = CFrame.new(Vector3.new(var_1475.X, var_1471.Y + 7.9, var_1475.Z), var_1471);
    var_1468.AssemblyLinearVelocity = Vector3.zero;
    var_1468.AssemblyAngularVelocity = Vector3.zero;
    return ;
end;
local func_186 = function(arg_515)
    if not arg_515 then
        return ;
    end;
    local var_1479 = player.Character;
    if not var_1479 or not var_1479:FindFirstChild(var) then
        return ;
    end;
    local var_1480, var_1481 = arg_515:GetBoundingBox();
    local num_147 = os.clock() * 10;
    local num_148 = math.max(var_1481.X, var_1481.Z) / 2 + 5;
    local num_149 = math.cos(num_147) * num_148;
    local num_150 = math.sin(num_147) * num_148;
    local var_1482 = var_1480.Position + Vector3.new(num_149, 0, num_150);
    var_1479.HumanoidRootPart.CFrame = CFrame.new(var_1482, var_1480.Position);
    var_1479.HumanoidRootPart.Velocity = Vector3.zero;
    var_1479.HumanoidRootPart.AssemblyLinearVelocity = Vector3.zero;
    return ;
end;
local func_187 = function(arg_516)
    local var_1483 = arg_516:FindFirstAncestorWhichIsA("Part");
    if var_1483 then
        if firetouchinterest then
            task.spawn(function()
                firetouchinterest(var_1483, child_11, 1);
                wait();
                firetouchinterest(var_1483, child_11, 0);
                return ;
            end);
        end;
        var_1483.CFrame = child_11.CFrame;
    end;
    return ;
end;
getgenv().farm_materials = true;
getgenv().debug = false;
getgenv().glitch_baal = true;
local func_188 = function(arg_517)
    if getgenv().debug then
        print("[BaalFarm] " .. arg_517);
    end;
    return ;
end;
if getgenv().real then
    getgenv().real:Disconnect();
    getgenv().real = nil;
end;
if getgenv().special_flag then
    getgenv().special_flag:Disconnect();
    getgenv().special_flag = nil;
end;
getgenv().wait = false;
getgenv().glitched = false;
getgenv().teleporting = false;
getgenv().wait2 = false;
local func_189 = function(arg_518)
    local var_1484 = player.Character;
    if not var_1484 then
        return ;
    end;
    local child_121 = var_1484:FindFirstChild(var);
    if not child_121 then
        return ;
    end;
    if typeof(arg_518) ~= "Vector3" then
        if typeof(arg_518) ~= "Instance" then
            return ;
        end;
        if arg_518:IsA("Model") then
            local var_1485 = arg_518.PrimaryPart or arg_518:FindFirstChild(var) or arg_518:FindFirstChild("HRP");
            if not var_1485 then
                return ;
            end;
            arg_518 = var_1485.Position;
        else
            arg_518 = arg_518.Position;
        end;
    end;
    child_121.CFrame = CFrame.lookAt(child_121.Position, Vector3.new(arg_518.X, child_121.Position.Y, arg_518.Z));
    return ;
end;
getgenv().skill_last_run = 0;
getgenv().skill_last_run2 = 0;
getgenv().skill = nil;
getgenv().made = false;
getgenv().dynamic_materials = false;
getgenv().should_wait = nil;
need_materials = function()
    local var_1486 = func_101("Wandering Soul");
    local var_1487 = func_101("Zombie Arm");
    local var_1488 = func_101("Zombie Leg");
    if var_1486 < 10 then
        return true;
    end;
    if var_1487 < 10 then
        return true;
    end;
    if var_1488 < 10 then
        return true;
    end;
    return false;
end;
safe_tp = function(arg_519)
    local var_1489 = arg_519:GetPivot().Position + Vector3.new(2, 9, 3);
    func_70(var_1489);
    return ;
end;
local func_190 = function(arg_520, arg_521, arg_522)
    local var_1490 = arg_521 or 12;
    local var_1491 = arg_522 or 30;
    local var_1492 = tick() * var_1491 % 360;
    local num_151 = math.rad(var_1492);
    local num_152 = math.cos(num_151) * var_1490;
    local num_153 = math.sin(num_151) * var_1490;
    if typeof(arg_520) ~= "Vector3" then
        if typeof(arg_520) == "CFrame" then
            arg_520 = arg_520.Position;
        else
            if typeof(arg_520) ~= "Instance" then
                return ;
            end;
            if arg_520:IsA("Model") and arg_520.PrimaryPart then
                arg_520 = arg_520.PrimaryPart.Position;
            elseif arg_520:IsA("Model") and arg_520:FindFirstChild(var) then
                arg_520 = arg_520.HumanoidRootPart.Position;
            else
                if not arg_520:IsA("BasePart") then
                    return ;
                end;
                arg_520 = arg_520.Position;
            end;
        end;
    end;
    local vector_52 = Vector3.new(arg_520.X + num_152, arg_520.Y, arg_520.Z + num_153);
    func_70(vector_52);
    return ;
end;
get_remote = function(arg_523)
    local tbl_102 = {};
    for var_1493, var_1494 in next, game:GetService("ReplicatedStorage"):GetChildren() do
        if var_1494.Name:match(arg_523) then
            table.insert(tbl_102, var_1494);
        end;
    end;
    return tbl_102;
end;
local func_191 = function(arg_524, arg_525)
    return ;
end;
baal_section.create_toggle({ title = "Baal Farm (Suna)", desc = "Auto Farm True Demon Ba'al & Resurrected Ba'al Using Suna!", default = false }, function(arg_526)
    getgenv().baal_farm = arg_526;
    local flag_108 = nil;
    if not arg_526 then
        return ;
    end;
    task.spawn(pcall, function()
        if arg_526 and not getgenv().made then
            getgenv().made = true;
            flag_108 = player.PlayerGui.Notifications.DescendantAdded:Connect(function(arg_527)
                if arg_527:IsA("TextLabel") then
                    local var_1495 = arg_527.Text:gsub("[<>]", "");
                    if table.find(true_demon_baal_drops, var_1495) and getgenv().baal_farm then
                        send_baal_webhook(var_1495);
                    end;
                end;
                return ;
            end);
        end;
        return ;
    end);
    warn("here");
    local var_1496, var_1497 = pcall(function()
        return stats_folder.Skills.sky_walk.Value;
    end);
    if not has_geppo() then
        library.create_noti({ title = "Uh Oh!", desc = "You need geppo to use this!", show_time = 4 });
        return ;
    end;
    task.spawn(function()
        local vector_53 = Vector3.new(18923, 5, -11909);
        while getgenv().baal_farm and task.wait(1) do
            if not getgenv().skill and func_181(vector_53, 10) then
                warn("return");
            else
                pcall(fake_geppo);
            end;
        end;
        return ;
    end);
    warn("passed");
    while getgenv().baal_farm and task.wait() do
        local flag_109 = nil;
        local flag_110 = nil;
        local flag_111 = nil;
        local var_1498 = func_181(Vector3.new(18951, 8152, -12374), 3000, true);
        task.spawn(pcall, function()
            flag_109 = workspace.AreaTeleporters.FirstSea.Haloween.Trans.Specs.Enabled;
            return ;
        end);
        task.spawn(pcall, function()
            flag_110 = workspace.NPCs:FindFirstChild("Resurrected Ba'al");
            return ;
        end);
        task.spawn(pcall, function()
            flag_111 = workspace.NPCs:FindFirstChild("True Demon Ba'al");
            return ;
        end);
        resurrected_baal = flag_110;
        if var_1498 then
            local vector_54 = Vector3.new(18923, 5, -11909);
            if not func_181(vector_54, 10, true) then
                horo_tp_pro({
                    Position = vector_54,
                    Speed = 50,
                    Stop = function()
                        return getgenv().baal_farm;
                    end
                });
            else
                getgenv().skill = #get_remote("Suna") > 0;
                if not getgenv().skill then
                    task.spawn(function()
                        game:GetService("ReplicatedStorage").Events.Skill:InvokeServer("Desert Spada", { cf = CFrame.new(0, 0, 0) });
                        return ;
                    end);
                    func_70(vector_54);
                elseif getgenv().skill and not workspace.NPCs:FindFirstChild("True Demon Ba'al") then
                    task.spawn(pcall, function()
                        fake_geppo();
                        return ;
                    end);
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(18923, 8122, -11909, -0.989, 0, -0.15, 0, 1, 0, 0.15, 0, -0.989);
                    local num_154 = 0;
                    repeat
                        task.wait(0.1);
                        player.Character.HumanoidRootPart.CFrame = CFrame.new(18923, 8122, -11909, -0.989, 0, -0.15, 0, 1, 0, 0.15, 0, -0.989);
                        num_154 = num_154 + 0.1;
                    until workspace.NPCs:FindFirstChild("True Demon Ba'al") or not getgenv().baal_farm or not var_1498 or num_154 >= 15;
                    task.wait();
                    for var_1499, var_1500 in next, get_remote("Suna") do
                        var_1500:FireServer(CFrame.new(0, 0, 0));
                        var_1500:Destroy();
                    end;
                    if num_154 >= 15 then
                        repeat
                            task.wait();
                            local var_1501 = func_181(Vector3.new(18951, 8152, -12374), 3000, true);
                            local vector_55 = Vector3.new(18944, 8122, -12572);
                            if not func_181(vector_55, 10, true) then
                                if not var_1501 then
                                    break;
                                end;
                                horo_tp_pro({
                                    Position = vector_55,
                                    Speed = 50,
                                    Stop = function()
                                        return getgenv().baal_farm;
                                    end
                                });
                            else
                                if not var_1501 then
                                    break;
                                end;
                                func_70(vector_55);
                                task.wait(2);
                                touch(workspace.AreaTeleporters.FirstSea.Haloween.Void);
                                task.wait(5);
                            end;
                        until not var_1501 or not getgenv().baal_farm;
                    end;
                elseif getgenv().skill and workspace.NPCs:FindFirstChild("True Demon Ba'al") then
                    func_70(vector_54);
                end;
            end;
        elseif flag_109 then
            local vector_56 = Vector3.new(-10086, 427, -2573);
            if not func_181(vector_56, 10, true) then
                horo_tp_pro({
                    Position = vector_56,
                    Speed = 50,
                    Stop = function()
                        return getgenv().baal_farm;
                    end
                });
            else
                func_70(vector_56);
                task.wait(2);
                touch(workspace.AreaTeleporters.FirstSea.Haloween.Trans);
                task.wait(5);
            end;
        elseif flag_110 then
            hrp = player.Character.HumanoidRootPart;
            local var_1502 = resurrected_baal:GetPivot().Position;
            if (hrp.Position - var_1502).Magnitude > 30 then
                func_191("Initial TP", "Distance: " .. math.floor((hrp.Position - var_1502).Magnitude));
                horo_tp_pro({
                    Position = var_1502,
                    Speed = 50,
                    Stop = function()
                        return getgenv().baal_farm;
                    end
                });
                repeat
                    task.wait(0.1);
                    if not getgenv().baal_farm then
                        func_191("Farm disabled during initial TP");
                        break;
                    end;
                    hrp = player.Character and player.Character:FindFirstChild(var);
                    resurrected_baal = NPCs:FindFirstChild("Resurrected Ba'al");
                    if not hrp then
                        func_191("No HRP during initial TP");
                        break;
                    end;
                    if not resurrected_baal then
                        func_191("Baal despawned during initial TP");
                        break;
                    end;
                    local var_1503 = resurrected_baal:GetPivot().Position;
                until (hrp.Position - var_1503).Magnitude <= 30;
                func_191("Initial TP complete");
            end;
            local vector_57 = Vector3.new(-9840, 50, -1956);
            local func_192 = function(arg_528, arg_529)
                return (Vector3.new(arg_528.X, 0, arg_528.Z) - Vector3.new(arg_529.X, 0, arg_529.Z)).Magnitude;
            end;
            local func_193 = function(arg_530, arg_531)
                horo_tp_pro({
                    Position = arg_530,
                    Speed = arg_531 or 50,
                    Stop = function()
                        return getgenv().baal_farm;
                    end
                });
                return ;
            end;
            while resurrected_baal and resurrected_baal.Parent and getgenv().baal_farm do
                hrp = player.Character and player.Character:FindFirstChild(var);
                if not hrp then
                    func_191("Waiting for HRP");
                    task.wait(0.1);
                    resurrected_baal = NPCs:FindFirstChild("Resurrected Ba'al");
                else
                    local var_1504 = resurrected_baal:GetPivot().Position;
                    local var_1505 = func_192(var_1504, vector_57);
                    if var_1505 > 50 then
                        if (hrp.Position - vector_57).Magnitude <= 10 then
                            func_191("Holding fight pos", "Baal XZ dist: " .. math.floor(var_1505));
                            hrp.CFrame = CFrame.new(vector_57);
                        else
                            func_191("TP to fight pos", "Baal XZ dist: " .. math.floor(var_1505));
                            func_193(vector_57, 40);
                        end;
                    elseif getgenv().is_playing_bad(resurrected_baal) then
                        func_191("Baal playing bad - hovering above");
                        hrp.CFrame = CFrame.new(var_1504 + Vector3.new(0, 20, 0), var_1504);
                        hrp.Velocity = Vector3.zero;
                        hrp.AssemblyLinearVelocity = Vector3.zero;
                    elseif func_181(var_1504, 25) then
                        func_191("In range - circle TP");
                        func_186(resurrected_baal);
                    else
                        func_191("Out of range - raycast track");
                        getgenv().raycast_track_tween(resurrected_baal, 50);
                    end;
                    task.spawn(function()
                        getgenv().global_hit(resurrected_baal);
                        return ;
                    end);
                    task.wait();
                    resurrected_baal = NPCs:FindFirstChild("Resurrected Ba'al");
                end;
            end;
            if getgenv().baal_farm then
                func_191("Baal dead - waiting 35s for respawn");
                task.wait(35);
            end;
        elseif not flag_110 and need_materials() then
            local var_1506 = func_101("Wandering Soul");
            local var_1507 = func_101("Zombie Arm");
            local var_1508 = func_101("Zombie Leg");
            if var_1506 < 10 then
                for var_1509, var_1510 in next, workspace.Effects:GetChildren() do
                    if var_1510.Name == "Soul" then
                        local var_1511 = var_1510:FindFirstChildWhichIsA("ProximityPrompt", true);
                        if var_1511 then
                            fireproximityprompt(var_1511);
                        end;
                    end;
                end;
                local var_1512 = func_182("Wandering Soul");
                if var_1512 then
                    if not func_181(var_1512, 15, true) then
                        horo_tp_pro({
                            Position = var_1512:GetPivot().Position,
                            Speed = 50,
                            Stop = function()
                                return getgenv().baal_farm;
                            end
                        });
                    else
                        safe_tp(var_1512);
                        task.spawn(function()
                            getgenv().global_hit(var_1512);
                            return ;
                        end);
                    end;
                else
                    horo_tp_pro({
                        Position = Vector3.new(-9879, 27, -1936),
                        Speed = 50,
                        Stop = function()
                            return getgenv().baal_farm;
                        end
                    });
                end;
            elseif var_1507 < 10 or var_1508 < 10 then
                warn("no z arms");
                local vector_58 = Vector3.new(-10316, 37, -1984);
                local var_1513 = func_182("Transylvania Zombie", 150, vector_58, true);
                local var_1514 = func_182("Transylvania Zombie", 150, vector_58, false);
                if var_1514 then
                    if not func_181(var_1514, 15, true) then
                        horo_tp_pro({
                            Position = var_1514:GetPivot().Position,
                            Speed = 50,
                            Stop = function()
                                return getgenv().baal_farm;
                            end
                        });
                    else
                        safe_tp(var_1514);
                        task.spawn(function()
                            getgenv().global_hit(var_1514);
                            return ;
                        end);
                    end;
                else
                    warn("all mobs agroed");
                    if not func_181(vector_58, 20, true) then
                        warn("going to pos");
                        horo_tp_pro({
                            Position = vector_58,
                            Speed = 50,
                            Stop = function()
                                return getgenv().baal_farm;
                            end
                        });
                    else
                        warn("circling");
                        func_190(vector_58 + Vector3.new(0, 8, 0), 5, 400);
                        local tbl_103 = {};
                        for var_1515, var_1516 in next, workspace.NPCs:GetChildren() do
                            if var_1516.Name == "Transylvania Zombie" and func_181(var_1516, 10, true) then
                                table.insert(tbl_103, var_1516);
                            end;
                        end;
                        task.spawn(function()
                            getgenv().global_hit(tbl_103);
                            return ;
                        end);
                    end;
                end;
            end;
        elseif not flag_110 and not need_materials() then
            local vector_59 = Vector3.new(-9750, 36, -1956);
            if not func_181(vector_59, 10, true) then
                horo_tp_pro({
                    Position = vector_59,
                    Speed = 50,
                    Stop = function()
                        return getgenv().baal_farm;
                    end
                });
            else
                repeat
                    task.wait(1);
                    for var_1517, var_1518 in next, workspace.Env.Shrine.Doors:GetChildren() do
                        local var_1519 = var_1518:FindFirstChildWhichIsA("ProximityPrompt", true);
                        if var_1519 then
                            fireproximityprompt(var_1519);
                        end;
                    end;
                until workspace.NPCs:FindFirstChild("Resurrected Ba'al") or not getgenv().baal_farm;
            end;
        end;
    end;
    return ;
end);
baal_section.create_box({ title = "Webhook", placeholder = "", default = "", Number = false }, function(arg_532)
    getgenv().webhook_baal = arg_532;
    return ;
end);
get_npc = function(arg_533)
    for var_1520, var_1521 in next, workspace.NPCs:GetChildren() do
        if var_1521.Name == "Donmingo Family" and (var_1521:GetPivot().Position - arg_533:GetPivot().Position).Magnitude < 250 then
            return var_1521;
        end;
    end;
    return nil;
end;
getgenv().pica_webhook = "";
local func_194 = function()
    if getgenv().pica_webhook == "" then
        return ;
    end;
    request({ Url = getgenv().pica_webhook, Method = "POST", Headers = { ["Content-Type"] = "application/json" }, Body = game:GetService("HttpService"):JSONEncode({ embeds = { { title = "Pica Has Died", description = "**" .. game.Players.LocalPlayer.Name .. "** received **6,000** bounty", color = 16711680 } } }) });
    return ;
end;
pica_section.create_toggle({ title = "Farm Pica (SUNA)", desc = "so its real?", default = false }, function(arg_534)
    getgenv().farm_pica = arg_534;
    task.spawn(function()
        while getgenv().farm_pica and task.wait(1) do
            fake_geppo();
        end;
        return ;
    end);
    while getgenv().farm_pica and task.wait() do
        if player.Character and player.Character:FindFirstChild(var) and player.Character:FindFirstChild("Humanoid") and not (player.Character.Humanoid.Health <= 0) and not (player.Character:GetAttribute("SpeedBypass") < 15) then
            if not func_71("Pica's Bounty Poster") then
                if #workspace.Islands["Rose Kingdom"].DonmingoFamilySpawn:GetChildren() > 0 then
                    warn("no tool ;c");
                    local flag_112 = nil;
                    local num_155 = math.huge;
                    for var_1522, var_1523 in next, workspace.Islands["Rose Kingdom"].DonmingoFamilySpawn:GetChildren() do
                        if not player.Character or not player.Character:FindFirstChild(var) then
                            break;
                        end;
                        local var_1524 = (var_1523:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
                        if var_1524 < num_155 then
                            flag_112 = var_1523;
                            num_155 = var_1524;
                        end;
                    end;
                    if flag_112 then
                        if get_npc(flag_112) then
                            repeat
                                task.wait();
                                if not player.Character or not player.Character:FindFirstChild(var) or not player.Character:FindFirstChild("Humanoid") or player.Character.Humanoid.Health <= 0 then
                                    break;
                                end;
                                local var_1525 = get_npc(flag_112);
                                if not var_1525 or not var_1525.Parent or not var_1525:FindFirstChild("Humanoid") or func_71("Pica's Bounty Poster") then
                                    break;
                                end;
                                if (var_1525:GetPivot().Position - player.Character.HumanoidRootPart.Position).Magnitude > 20 then
                                    horo_tp_pro({
                                        Position = var_1525:GetPivot().Position + Vector3.new(0, 8.5, 0),
                                        Speed = 50,
                                        Stop = function()
                                            return getgenv().farm_pica;
                                        end
                                    });
                                else
                                    warn("circle?");
                                    func_186(var_1525);
                                end;
                                task.spawn(function()
                                    getgenv().global_hit(var_1525);
                                    return ;
                                end);
                            until not var_1525 or not var_1525.Parent or not getgenv().farm_pica or func_71("Pica's Bounty Poster");
                            flag_112:Destroy();
                        else
                            local num_156 = 0;
                            repeat
                                task.wait(0.1);
                                if not player.Character or not player.Character:FindFirstChild(var) then
                                    break;
                                end;
                                local var_1526 = get_npc(flag_112);
                                horo_tp_pro({
                                    Position = flag_112.Position,
                                    Speed = 50,
                                    Stop = function()
                                        return getgenv().farm_pica;
                                    end
                                });
                            until var_1526 or num_156 > 10 or not getgenv().farm_pica or func_71("Pica's Bounty Poster");
                        end;
                    end;
                else
                    warn("here");
                    pcall(function()
                        local tbl_104 = { "Activated", "MouseButton1Down", "MouseButton2Down", "MouseButton1Click", "MouseButton2Click" };
                        v = game:GetService("Players").LocalPlayer.PlayerGui.BountyHunt.Board.BoardUI.CanvasGroup.ScrollingFrame.Pica.Background;
                        for key_171, value_173 in pairs(tbl_104) do
                            firesignal(v[value_173]);
                        end;
                        return ;
                    end);
                    local vector_60 = Vector3.new(8496, 69, 10557);
                    if func_181(vector_60, 10, true) then
                        func_70(vector_60);
                    else
                        horo_tp_pro({
                            Position = Vector3.new(8496, 69, 10557),
                            Speed = 50,
                            Stop = function()
                                return getgenv().farm_pica;
                            end
                        });
                    end;
                end;
            else
                local vector_61 = Vector3.new(6673, 21, 11183);
                if player.Character and player.Character:FindFirstChild(var) and player.Character:FindFirstChild("Humanoid") and not (player.Character.Humanoid.Health <= 0) then
                    if not func_181(vector_61, 10, true) then
                        horo_tp_pro({
                            Position = vector_61,
                            Speed = 50,
                            Stop = function()
                                return getgenv().farm_pica;
                            end,
                            y = 1000,
                            no_raycasting = true
                        });
                    else
                        if not getgenv().pro then
                            getgenv().pro = game:GetService("Players").LocalPlayer.PlayerGui.BountyHunt.Tools.BoardUI.ChildRemoved:Connect(function(arg_535)
                                if arg_535.Name == "Pica" then
                                    func_194();
                                    player:Kick("gg pica died enable auto rj");
                                end;
                                return ;
                            end);
                        end;
                        if not player.Character or not player.Character:FindFirstChild(var) or not player.Character:FindFirstChild("Humanoid") or player.Character.Humanoid.Health <= 0 then
                            return ;
                        end;
                        getgenv().skill2 = #get_remote("Suna") > 0;
                        if not getgenv().skill2 then
                            task.spawn(function()
                                local tbl_105 = { "Desert Spada", { cf = player.Character.HumanoidRootPart.CFrame } };
                                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer(unpack(tbl_105));
                                return ;
                            end);
                            if player.Character and player.Character:FindFirstChild(var) then
                                func_70(vector_61 + Vector3.new(0, 10000, 0));
                            end;
                        else
                            if player.Character and player.Character:FindFirstChild(var) then
                                player.Character.HumanoidRootPart.CFrame = CFrame.new(6673, 21, 11183, 0.175908044, -3.92465793E-08, 0.98440659, -1.45979033E-08, 1, 4.24768274E-08, -0.98440659, -2.1842288E-08, 0.175908044);
                            end;
                            repeat
                                task.wait();
                                if not player.Character or not player.Character:FindFirstChild(var) then
                                    break;
                                end;
                                player.Character.HumanoidRootPart.CFrame = CFrame.new(6673, 21, 11183, 0.175908044, -3.92465793E-08, 0.98440659, -1.45979033E-08, 1, 4.24768274E-08, -0.98440659, -2.1842288E-08, 0.175908044);
                            until workspace.NPCs:FindFirstChild("Pica");
                            for var_1527, var_1528 in next, get_remote("Suna") do
                                var_1528:FireServer(CFrame.new(0, 0, 0));
                                var_1528:Destroy();
                            end;
                        end;
                    end;
                end;
            end;
        end;
    end;
    return ;
end);
mega_pow_attack_2 = function(arg_536)
    local child_122 = player.Character:FindFirstChild("Mega-Pow");
    if not child_122 then
        return ;
    end;
    local child_123 = child_122:FindFirstChild("MinigunHandleServer");
    if not child_123 then
        return ;
    end;
    local flag_113 = nil;
    local num_157 = math.huge;
    for var_1529, var_1530 in next, workspace.NPCs:GetChildren() do
        if var_1530.Name == arg_536 and var_1530:FindFirstChild(var) then
            local var_1531 = (var_1530:GetPivot().Position - player.Character.HumanoidRootPart.Position).Magnitude;
            if var_1531 < num_157 then
                flag_113 = var_1530;
                num_157 = var_1531;
            end;
        end;
    end;
    if not flag_113 then
        return ;
    end;
    local child_124 = child_123:FindFirstChild("input");
    local child_125 = child_123:FindFirstChild("UnreliableRemoteEvent");
    local child_126 = child_123:FindFirstChild("updateBarrelPosition");
    if not child_125 then
        child_124:InvokeServer(true, flag_113:GetPivot().Position);
        return ;
    end;
    if getgenv().mega_pow_active then
        return ;
    end;
    getgenv().mega_pow_active = true;
    task.spawn(function()
        local var_1532 = tick();
        local vector_62 = Vector3.new(6545, 185, 11204);
        while child_125 and child_125.Parent and flag_113 and flag_113.Parent and getgenv().farm_pica and not (tick() - var_1532 > 4.5) do
            local var_1533 = flag_113:GetPivot().Position;
            if (var_1533 - vector_62).Magnitude > 3000 then
                break;
            end;
            child_126:FireServer(player.Character.HumanoidRootPart.Position);
            child_125:FireServer(var_1533);
            task.wait(0.1);
        end;
        child_124:InvokeServer(false);
        getgenv().mega_pow_active = false;
        return ;
    end);
    return ;
end;
pica_section.create_toggle({ title = "Farm Pica (MEGAPOW)", desc = "so its real?", default = false }, function(arg_537)
    getgenv().farm_pica = arg_537;
    task.spawn(function()
        while getgenv().farm_pica and task.wait(1) do
            fake_geppo();
        end;
        return ;
    end);
    while getgenv().farm_pica and task.wait() do
        if player.Character and player.Character:FindFirstChild(var) and player.Character:FindFirstChild("Humanoid") and not (player.Character.Humanoid.Health <= 0) and not (player.Character:GetAttribute("SpeedBypass") < 15) then
            if not func_71("Pica's Bounty Poster") then
                if #workspace.Islands["Rose Kingdom"].DonmingoFamilySpawn:GetChildren() > 0 then
                    warn("no tool ;c");
                    local flag_114 = nil;
                    local num_158 = math.huge;
                    for var_1534, var_1535 in next, workspace.Islands["Rose Kingdom"].DonmingoFamilySpawn:GetChildren() do
                        if not player.Character or not player.Character:FindFirstChild(var) then
                            break;
                        end;
                        local var_1536 = (var_1535:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
                        if var_1536 < num_158 then
                            flag_114 = var_1535;
                            num_158 = var_1536;
                        end;
                    end;
                    if flag_114 then
                        if get_npc(flag_114) then
                            repeat
                                task.wait();
                                if not player.Character or not player.Character:FindFirstChild(var) or not player.Character:FindFirstChild("Humanoid") or player.Character.Humanoid.Health <= 0 then
                                    break;
                                end;
                                local var_1537 = get_npc(flag_114);
                                if not var_1537 or not var_1537.Parent or not var_1537:FindFirstChild("Humanoid") or func_71("Pica's Bounty Poster") then
                                    break;
                                end;
                                if (var_1537:GetPivot().Position - player.Character.HumanoidRootPart.Position).Magnitude > 20 then
                                    horo_tp_pro({
                                        Position = var_1537:GetPivot().Position + Vector3.new(0, 8.5, 0),
                                        Speed = 50,
                                        Stop = function()
                                            return getgenv().farm_pica;
                                        end
                                    });
                                else
                                    warn("circle?");
                                    func_70(var_1537:GetPivot().Position + Vector3.new(0, 15, 0));
                                end;
                                task.spawn(function()
                                    equip_shit("Mega-Pow");
                                    mega_pow_attack_2(var_1537.Name);
                                    return ;
                                end);
                            until not var_1537 or not var_1537.Parent or not getgenv().farm_pica or func_71("Pica's Bounty Poster");
                            flag_114:Destroy();
                        else
                            local num_159 = 0;
                            repeat
                                task.wait(0.1);
                                if not player.Character or not player.Character:FindFirstChild(var) then
                                    break;
                                end;
                                local var_1538 = get_npc(flag_114);
                                horo_tp_pro({
                                    Position = flag_114.Position,
                                    Speed = 50,
                                    Stop = function()
                                        return getgenv().farm_pica;
                                    end
                                });
                            until var_1538 or num_159 > 10 or not getgenv().farm_pica or func_71("Pica's Bounty Poster");
                        end;
                    end;
                else
                    warn("here");
                    pcall(function()
                        local tbl_106 = { "Activated", "MouseButton1Down", "MouseButton2Down", "MouseButton1Click", "MouseButton2Click" };
                        v = game:GetService("Players").LocalPlayer.PlayerGui.BountyHunt.Board.BoardUI.CanvasGroup.ScrollingFrame.Pica.Background;
                        for key_172, value_174 in pairs(tbl_106) do
                            firesignal(v[value_174]);
                        end;
                        return ;
                    end);
                    local vector_63 = Vector3.new(8496, 69, 10557);
                    if func_181(vector_63, 10, true) then
                        func_70(vector_63);
                    else
                        horo_tp_pro({
                            Position = Vector3.new(8496, 69, 10557),
                            Speed = 50,
                            Stop = function()
                                return getgenv().farm_pica;
                            end
                        });
                    end;
                end;
            else
                local vector_64 = Vector3.new(6673, 21, 11183);
                if player.Character and player.Character:FindFirstChild(var) and player.Character:FindFirstChild("Humanoid") and not (player.Character.Humanoid.Health <= 0) then
                    if not func_181(vector_64, 10, true) then
                        horo_tp_pro({
                            Position = vector_64,
                            Speed = 50,
                            Stop = function()
                                return getgenv().farm_pica;
                            end,
                            y = 1000,
                            no_raycasting = true
                        });
                    else
                        if not getgenv().pro then
                            getgenv().pro = game:GetService("Players").LocalPlayer.PlayerGui.BountyHunt.Tools.BoardUI.ChildRemoved:Connect(function(arg_538)
                                if arg_538.Name == "Pica" then
                                    func_194();
                                    player:Kick("gg pica died enable auto rj");
                                end;
                                return ;
                            end);
                        end;
                        if not player.Character or not player.Character:FindFirstChild(var) or not player.Character:FindFirstChild("Humanoid") or player.Character.Humanoid.Health <= 0 then
                            return ;
                        end;
                        if player.Character and player.Character:FindFirstChild(var) then
                            func_70(vector_64 + Vector3.new(0, 500, 0));
                            equip_shit("Mega-Pow");
                            mega_pow_attack_2("Pica");
                        end;
                    end;
                end;
            end;
        end;
    end;
    return ;
end);
pica_section.create_box({ title = "Webhook", placeholder = "", default = "", Number = false }, function(arg_539)
    getgenv().pica_webhook = arg_539;
    return ;
end);
getgenv().lure = false;
getgenv().lerp_tween_running = false;
local func_195 = function(arg_540)
    if getgenv().lerp_tween_running then
        return ;
    end;
    getgenv().lerp_tween_running = true;
    local game_92 = game:GetService("Players").LocalPlayer.Character;
    if not game_92 then
        getgenv().lerp_tween_running = false;
        return ;
    end;
    local child_127 = game_92:FindFirstChild(var);
    if not child_127 then
        getgenv().lerp_tween_running = false;
        return ;
    end;
    if child_127.CFrame.Position.Y > 2000 then
        child_127.CFrame = CFrame.new(child_127.CFrame.Position.X, 5, child_127.CFrame.Position.Z);
    end;
    local var_1539 = tick();
    while game_92.Parent and child_127.Parent do
        if child_127.CFrame.Position.Y > 2000 then
            child_127.CFrame = CFrame.new(child_127.CFrame.Position.X, 5, child_127.CFrame.Position.Z);
        end;
        local var_1540 = tick();
        local var_1541 = var_1540 - var_1539;
        local var_1542 = child_127.CFrame.Position;
        local var_1543 = arg_540 - var_1542;
        local var_1544 = var_1543.Magnitude;
        if var_1544 < 0.5 then
            child_127.CFrame = CFrame.new(arg_540);
            break;
        end;
        local var_1545 = func_67() - 5;
        local num_160 = math.min(var_1545 * var_1541, var_1544);
        local var_1546 = var_1542 + var_1543.Unit * num_160;
        child_127.CFrame = CFrame.new(var_1546);
        child_127.Velocity = Vector3.zero;
        task.wait();
        var_1539 = var_1540;
    end;
    getgenv().lerp_tween_running = false;
    return ;
end;
getgenv().lasttick = 0;
pika_flight = function(arg_541)
    if not getgenv().tuff or not getgenv().tuff.Parent then
        local var_1547 = tick();
        if var_1547 - (getgenv().lasttick or 0) >= 0.5 then
            getgenv().lasttick = var_1547;
            local var_1548 = func_71("Pika-Pika") and "Light Flight" or "Radiant Flight";
            getgenv().tuff = game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer(var_1548);
            warn("RESULT " .. tostring(getgenv().tuff));
        end;
        if not getgenv().tuff or not getgenv().tuff.Parent then
            warn("HERE NIGGA");
            if func_181(Vector3.new(-5877, 150, 7484), 1500, true) then
                warn("IN RANGEN IGGA");
                if func_181(Vector3.new(-5877, 150, 7484), 10) then
                    func_70(Vector3.new(-5877, 150, 7484));
                else
                    func_195(Vector3.new(-5877, 150, 7484));
                end;
            end;
            return ;
        end;
    end;
    repeat
        task.wait();
        func_195(arg_541, func_67());
    until func_181(arg_541, 10, true);
    if getgenv().tuff and getgenv().tuff.Parent then
        getgenv().tuff:FireServer();
    end;
    return ;
end;
SpawnShip = function()
    task.spawn(function()
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ShipEvents"):WaitForChild("Spawn"):InvokeServer("true");
        return ;
    end);
    return ;
end;
getgenv().lasttick2 = 0;
getgenv().radiant_kick = nil;
getgenv().radiant_ray = nil;
getgenv().tuff_loop = nil;
getgenv().radiant_jewels = nil;
getgenv().delayedlol = nil;
PikaAttack = function(arg_542)
    if not arg_542 then
        return ;
    end;
    if func_71("Pika-Pika") then
        if not getgenv().jewels_of_light or not getgenv().jewels_of_light.Parent then
            local game_93 = game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer("Jewels Of Light");
            getgenv().jewels_of_light = game_93;
        end;
        if getgenv().getgenv().JewelsOfLight and getgenv().jewels_of_light.Parent then
            warn("ray");
            while getgenv().jewels_of_light and getgenv().jewels_of_light.Parent and task.wait() do
                local var_1549 = arg_542:GetPivot().Position;
                if func_181(var_1549, func_67() / 2, true) then
                    local var_1550 = var_1549 + Vector3.new(0, 100, 0);
                    player.Character:PivotTo(CFrame.lookAt(var_1550, var_1549));
                else
                    local var_1551 = player.Character:GetPivot().Position;
                    local vector_65 = Vector3.new(var_1551.X, var_1549.Y + 200, var_1551.Z);
                    player.Character:PivotTo(CFrame.lookAt(vector_65, var_1549));
                end;
                task.wait(0.05);
            end;
        end;
    end;
    return ;
end;
mega_pow_attack = function()
    local game_94 = game:GetService("Players").LocalPlayer.Character;
    local var_1552 = game_94 and game_94:FindFirstChild("Mega-Pow");
    if not var_1552 then
        return ;
    end;
    local child_128 = var_1552:FindFirstChild("MinigunHandleServer");
    if not child_128 then
        return ;
    end;
    local child_129 = child_128:WaitForChild("input");
    local child_130 = child_128:WaitForChild("updateBarrelPosition");
    local game_95 = game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Block");
    local func_196 = function(arg_543)
        local tbl_107 = {};
        local child_131 = workspace:FindFirstChild("NPCs");
        if not child_131 then
            return tbl_107;
        end;
        for var_1553, var_1554 in child_131:GetChildren() do
            if var_1554.Name == arg_543 then
                local var_1555 = var_1554.PrimaryPart and var_1554.PrimaryPart.Position or var_1554:FindFirstChild(var) and var_1554.HumanoidRootPart.Position;
                if var_1555 then
                    table.insert(tbl_107, { model = var_1554, position = var_1555 });
                end;
            end;
        end;
        return tbl_107;
    end;
    local func_197 = function()
        local var_1556 = func_196("Megalodon");
        if #var_1556 == 0 then
            var_1556 = func_196("Kraken");
        end;
        if #var_1556 == 0 then
            return nil;
        end;
        if #var_1556 == 1 then
            return var_1556[1].position;
        end;
        if #var_1556 >= 2 then
            local var_1557 = var_1556[1];
            local var_1558 = var_1556[2];
            if (var_1557.position - var_1558.position).Magnitude <= 50 then
                return (var_1557.position + var_1558.position) / 2;
            end;
        end;
        return var_1556[1].position;
    end;
    local var_1559 = func_197();
    if not var_1559 then
        return ;
    end;
    game_95:InvokeServer(false, "Melee", false);
    local var_1560 = child_129:InvokeServer(true, var_1559);
    if not var_1560 then
        return ;
    end;
    local var_1561 = tick();
    task.spawn(function()
        while var_1560 and var_1560.Parent and tick() - var_1561 < 5 do
            local var_1562 = func_197();
            if not var_1562 then
                break;
            end;
            game_95:InvokeServer(true, "Melee", true);
            local var_1563 = var_1562 + Vector3.new(0, 5, 0);
            local var_1564 = var_1562 + Vector3.new(0, -2, 0);
            child_130:FireServer(var_1563);
            var_1560:FireServer(var_1564);
            task.wait();
        end;
        game_95:InvokeServer(false, "Melee", false);
        child_129:InvokeServer(false);
        return ;
    end);
    return ;
end;
getgenv().anchor = false;
getgenv().last_ship = nil;
getgenv().weapon = "Mega-Pow";
getgenv().party_account = nil;
getgenv().anchor_account = nil;
getgenv().anchor_position = nil;
GetPartyPosition = function(arg_544)
    local child_132 = game.Players:FindFirstChild(arg_544);
    if child_132 and child_132.Party.Value then
        return game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("getPartyPosition"):InvokeServer(child_132);
    end;
    return ;
end;
IsPositionInRange = function(arg_545, arg_546, arg_547)
    return (Vector3.new(arg_545.X, 0, arg_545.Z) - Vector3.new(arg_546.X, 0, arg_546.Z)).Magnitude <= arg_547;
end;
GetKraken = function()
    local child_133 = workspace.NPCs:FindFirstChild("Kraken");
    if child_133 and (player:FindFirstChild("AssistFolder") and player.AssistFolder:FindFirstChild("Kraken")) then
        return child_133;
    end;
    return nil;
end;
local func_198 = function()
    for var_1565, var_1566 in next, workspace.NPCs:GetChildren() do
        if var_1566.Name == "Kraken" then
            local child_134 = var_1566:FindFirstChild("Animator", true);
            if child_134 then
                for var_1567, var_1568 in next, child_134:GetPlayingAnimationTracks() do
                    if var_1568.Animation and var_1568.Animation.AnimationId == "rbxassetid://6458373675" and var_1568.TimePosition > 1 then
                        return true;
                    end;
                end;
            end;
        end;
    end;
    return false;
end;
horo_attack = function(arg_548)
    if not arg_548 then
        return ;
    end;
    getgenv().horo_cd = getgenv().horo_cd or 0;
    if tick() - getgenv().horo_cd < 1 then
        return ;
    end;
    getgenv().horo_cd = tick();
    task.spawn(function()
        local game_96 = game:GetService("ReplicatedStorage"):FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Horo-Horo.Mini Hollow Barrage");
        if not game_96 then
            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer("Mini Hollow Barrage");
        else
            warn("got remote");
            for var_1569, var_1570 in next, game:GetService("ReplicatedStorage"):GetChildren() do
                if var_1570.Name:match("Mini Hollow Barrage") then
                    for i_17 = 1, 100, 1 do
                        game_96:FireServer({ Target = arg_548, cf = CFrame.new(arg_548:GetPivot().Position) });
                        task.wait();
                    end;
                    var_1570:Destroy();
                end;
            end;
        end;
        return ;
    end);
    return ;
end;
gkk_section.create_toggle({ title = "Gkk Farm", desc = "chat this might be the one", default = false }, function(arg_549)
    local flag_115 = false;
    getgenv().gkk_farm = arg_549;
    local vector_66 = Vector3.new(-291, 307, 1641);
    local vector_67 = Vector3.new(-5890, 250, 7484);
    local vector_68 = Vector3.new(-5877, 200, 7484);
    local vector_69 = Vector3.new(-5800, 150, 7600);
    if not arg_549 then
        return ;
    end;
    for var_1571, var_1572 in next, workspace.ZipLines:GetDescendants() do
        if var_1572.Name == "ZiplineRails" then
            for var_1573, var_1574 in next, var_1572:GetChildren() do
                var_1574:Destroy();
            end;
        end;
    end;
    task.spawn(function()
        while getgenv().gkk_farm and task.wait(1) do
            fake_geppo();
            if getgenv().anchor_account then
                getgenv().anchor_position = GetPartyPosition(getgenv().anchor_account);
            end;
        end;
        return ;
    end);
    task.spawn(function()
        while getgenv().gkk_farm and task.wait(1) do
            for var_1575, var_1576 in next, player.Backpack:GetChildren() do
                local var_1579, var_1580 = pcall(function()
                    if var_1576:GetAttribute("verifiedFruit") and func_101(var_1576.Name) < 1 then
                        warn(var_1576.Name);
                        player.Character.Humanoid:UnequipTools();
                        getgenv().busy = true;
                        task.wait(0.5);
                        local num_161 = 0;
                        repeat
                            task.wait();
                            if var_1576 and var_1576.Parent == player.Backpack then
                                var_1576.Parent = player.Character;
                                task.wait(1);
                            end;
                            local var_1577, var_1578 = pcall(function()
                                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("FruitStorage"):InvokeServer(true);
                                return ;
                            end);
                            num_161 = num_161 + 1;
                        until not var_1576 or not var_1576.Parent or num_161 > 100;
                    end;
                    return ;
                end);
                if not var_1579 then
                    warn("storing errored :", var_1580);
                end;
            end;
            getgenv().busy = false;
        end;
        return ;
    end);
    while getgenv().gkk_farm and task.wait() do
        LPH_NO_VIRTUALIZE(function()
            local fire_payload, fire_err = pcall(function()
                if getgenv().gkk_farm and not getgenv().lure and not getgenv().anchor then
                    for gun_remote, target_npc in next, game.Players:GetPlayers() do
                        if not target_npc.Party.Value then
                            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("PartySystem"):FireServer({ action = "invite", ply = target_npc });
                            task.wait(0.1);
                        end;
                    end;
                else
                    local gun_hole = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("PartyInvite");
                    if gun_hole then
                        gun_hole.Crew:FireServer(true);
                    end;
                end;
            end);
            if not fire_payload then
                warn("party erroed: " .. tostring(fire_err));
            end;
        end)();
        if not player.Character or not player.Character:GetAttribute("SpeedBypass") or not (player.Character:GetAttribute("SpeedBypass") < 15) then
            local var_1581 = GetKraken();
            local var_1582 = get_ship();
            if getgenv().anchor then
                if not func_181(vector_69, 10, true) then
                    horo_tp_pro({
                        Position = vector_69,
                        Speed = func_67() - 5,
                        Stop = function()
                            return getgenv().gkk_farm;
                        end
                    });
                elseif not func_177() then
                    func_70(vector_69 + Vector3.new(0, -25, 0));
                else
                    func_70(vector_69 + Vector3.new(0, 600, 0));
                end;
            elseif getgenv().lure then
                repeat
                    if not flag_115 and not (not getgenv().anchor_account or not getgenv().anchor_position) then
                        warn("here");
                        if IsPositionInRange(getgenv().anchor_position, vector_69, 100) then
                            flag_115 = true;
                        end;
                    elseif not flag_115 and not (not getgenv().anchor_account or getgenv().anchor_position or getgenv().anchor_account == "") then
                        warn("here2");
                    else
                        flag_115 = false;
                        if not func_181(vector_68, 10, true) and (not getgenv().radiant_jewels or not getgenv().radiant_jewels.Parent) and (not getgenv().jewels_of_light or not getgenv().jewels_of_light.Parent) then
                            task.spawn(function()
                                pika_flight(vector_68);
                                return ;
                            end);
                        else
                            if (not getgenv().radiant_jewels or not getgenv().radiant_jewels.Parent) and (not getgenv().jewels_of_light or not getgenv().jewels_of_light.Parent) then
                                func_70(vector_68);
                            end;
                            if not var_1582 and func_181(vector_68, 10, true) then
                                func_70(vector_68 + Vector3.new(math.random(-10, 5), 0, math.random(1, 10)));
                                task.wait(0.1);
                                SpawnShip();
                                task.wait(0.1);
                            elseif var_1581 then
                                if not getgenv().queued then
                                    queue_on_teleport(str_44);
                                    getgenv().queued = true;
                                    task.wait(1);
                                end;
                                task.wait(1);
                                game:GetService("TeleportService"):Teleport(1730877806, game.Players.LocalPlayer);
                            end;
                        end;
                    end;
                until not flag_115;
            elseif not func_181(vector_67, func_67() / 2, true) and (not getgenv().radiant_jewels or not getgenv().radiant_jewels.Parent) then
                horo_tp_pro({
                    Position = vector_67,
                    Speed = func_67() - 5,
                    Stop = function()
                        return getgenv().gkk_farm;
                    end
                });
            else
                if not getgenv().radiant_kick and not getgenv().radiant_jewels then
                    func_70(vector_67);
                end;
                task.spawn(function()
                    if getgenv().weapon == "Instant" then
                        equip_shit("Horo-Horo");
                        var_1581 = workspace.NPCs:FindFirstChild("Megalodon") or workspace.NPCs:FindFirstChild("Kraken");
                        horo_attack(var_1581);
                    else
                        equip_shit("Mega-Pow");
                        mega_pow_attack(var_1581);
                    end;
                    return ;
                end);
            end;
        end;
    end;
    return ;
end);
gkk_section.create_toggle({ title = "Lure Account", desc = "Enable this if ur a lure account", default = false }, function(arg_550)
    getgenv().lure = arg_550;
    return ;
end);
gkk_section.create_dropdown({ title = "Kill Method", List = { "Mega-Pow" }, default = "Mega-Pow", search = false, selected = false }, function(arg_551)
    getgenv().weapon = arg_551;
    return ;
end);
getgenv().time = 1200;
try_2.create_toggle({ title = "Auto Rejoin", desc = "Rejoins based on servertime", default = false }, function(arg_552)
    getgenv().auto_rejoin = arg_552;
    if not arg_552 then
        return ;
    end;
    while getgenv().auto_rejoin and task.wait() do
        pcall(function()
            if workspace:GetAttribute("serverAge") and workspace:GetAttribute("serverAge") <= getgenv().time then
                game.Players.LocalPlayer:Kick();
            end;
            return ;
        end);
    end;
    return ;
end);
try_2.create_slider({ title = "Rejoin Time (In Seconds)", min = 0, max = 10000, default = 1200, precise = true }, function(arg_553)
    getgenv().time = arg_553;
    return ;
end);
get_presents = function()
    local tbl_108 = {};
    for var_1583, var_1584 in next, workspace.Effects:GetChildren() do
        if var_1584.Name == "ChristmasGift" then
            local flag_116 = nil;
            for var_1585, var_1586 in next, workspace.Effects:GetChildren() do
                if var_1586:IsA("BasePart") and var_1586:FindFirstChildWhichIsA("ProximityPrompt") then
                    target_distance = (var_1584:GetPivot().Position - var_1586:GetPivot().Position).Magnitude;
                    if target_distance < 20 then
                        flag_116 = var_1586;
                    end;
                end;
            end;
            if flag_116 then
                table.insert(tbl_108, flag_116);
            end;
        end;
    end;
    return tbl_108;
end;
get_closest_present = function()
    local var_1587 = get_presents();
    local flag_117 = nil;
    local num_162 = math.huge;
    local var_1588 = player.Character;
    if not var_1588 then
        return nil;
    end;
    local var_1589 = var_1588:GetPivot().Position;
    for var_1590, var_1591 in next, var_1587, nil do
        local var_1592 = var_1591:FindFirstChildWhichIsA("ProximityPrompt");
        local var_1593 = (var_1591:GetPivot().Position - var_1589).Magnitude;
        if var_1593 < num_162 then
            flag_117 = var_1591;
            num_162 = var_1593;
        end;
    end;
    return flag_117;
end;
getgenv().auto_open_gifts = false;
getgenv().presents = 5;
getgenv().drops_webhook = {};
getgenv().webhook_santa = nil;
getgenv().attack_santa = true;
getgenv().die = false;
santa_section.create_toggle({ title = "Auto Santa", desc = "Farms Santa", default = false }, function(arg_554)
    getgenv().auto_santa = arg_554;
    if arg_554 then
        getgenv().zipl = false;
        for var_1594, var_1595 in next, workspace.ZipLines:GetDescendants() do
            if var_1595.Name == "ZiplineRails" then
                for var_1596, var_1597 in next, var_1595:GetChildren() do
                    var_1597:Destroy();
                end;
            end;
        end;
    end;
    task.spawn(pcall, function()
        if arg_554 and not getgenv().gg then
            local local_player_12 = game.Players.LocalPlayer;
            local var_1598 = debug and debug.setconstant or setconstant;
            local var_1599 = debug and debug.getconstants or getconstants;
            if not var_1598 or not getgc or not var_1599 then
                return notify("Incompatible Exploit", "Your exploit does not support this command (missing setconstant or getconstants or getgc)");
            end;
            local var_1600 = local_player_12.PlayerScripts.PlayerModule.CameraModule.ZoomController.Popper;
            for key_173, value_175 in pairs(getgc()) do
                if type(value_175) == "function" and getfenv(value_175).script == var_1600 then
                    for key_174, value_176 in pairs(var_1599(value_175)) do
                        if tonumber(value_176) == 0.25 then
                            var_1598(value_175, key_174, 0);
                        elseif tonumber(value_176) == 0 then
                            var_1598(value_175, key_174, 0.25);
                        end;
                    end;
                end;
            end;
            getgenv().gg = true;
        end;
        if arg_554 and not getgenv().made then
            workspace.StreamingEnabled = false;
            getgenv().made = true;
            Connection = player.PlayerGui.Notifications.DescendantAdded:Connect(function(arg_555)
                if arg_555:IsA("TextLabel") then
                    if arg_555.Text == "heading" then
                        getgenv().die = true;
                    end;
                    local var_1601 = arg_555.Text:gsub("[<>]", "");
                    warn(var_1601, table.find(santa_drops, var_1601), table.find(getgenv().drops_webhook, var_1601));
                    if table.find(santa_drops, var_1601) and getgenv().auto_santa and table.find(getgenv().drops_webhook, var_1601) then
                        santa_webhook(var_1601);
                    end;
                end;
                return ;
            end);
        end;
        return ;
    end);
    task.spawn(function()
        while getgenv().auto_santa and task.wait(1) do
            task.spawn(function()
                warn("geppo");
                fake_geppo();
                return ;
            end);
        end;
        return ;
    end);
    while getgenv().auto_santa and task.wait() do
        if getgenv().die then
            if getgenv().auto_open_gifts then
                presents = get_presents();
                repeat
                    task.wait();
                    for var_1602, var_1603 in next, presents, nil do
                        local var_1604 = get_closest_present();
                        if not var_1604 then
                            warn("break");
                            getgenv().break_state = true;
                            break;
                        end;
                        local var_1605 = var_1604:GetPivot().Position;
                        if math.abs(var_1605.X) > 20000 or math.abs(var_1605.Y) > 20000 or math.abs(var_1605.Z) > 20000 then
                            warn("Present out of bounds, skipping");
                            var_1604:Destroy();
                        elseif not func_181(var_1605, 10, true) then
                            horo_tp_pro({
                                Position = var_1605,
                                Speed = 50,
                                y = 1000,
                                Stop = function()
                                    return getgenv().auto_santa;
                                end
                            });
                        else
                            func_70(var_1605 + Vector3.new(0, 5, 0));
                            task.wait(0.1);
                            local child_135 = var_1604:FindFirstChild("ProximityPrompt", true);
                            if child_135 and child_135.Enabled then
                                fireproximityprompt(child_135);
                                task.wait(0.1);
                            else
                                var_1604:Destroy();
                            end;
                        end;
                    end;
                until #presents < 1 or getgenv().break_state;
            end;
            task.wait(10);
            player.Character.HumanoidRootPart.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position - Vector3.new(0, 1000, 0));
            game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ZiplineRemote"):InvokeServer(unpack({ "Exit" }));
            task.wait(5);
            if replicatesignal then
                replicatesignal(player.Kill);
            end;
            repeat
                task.wait();
            until player.Character and player.Character:FindFirstChild("Humanoid") and player.Character:FindFirstChild("Humanoid").Health == player.Character:FindFirstChild("Humanoid").MaxHealth;
            warn("finish");
            getgenv().die = false;
        end;
        if not getgenv().zipl then
            local vector_70 = Vector3.new(-1290, 11, -5089);
            if not func_181(vector_70, 10, true) then
                horo_tp_pro({
                    Position = vector_70,
                    Speed = 50,
                    Stop = function()
                        return getgenv().auto_santa or getgenv().zipl;
                    end
                });
            elseif player.Character:GetAttribute("SpeedBypass") < 250 then
                if getgenv().attempts < 5 then
                    getgenv().busy = true;
                    player.Character.Humanoid:UnequipTools();
                    func_70(vector_70);
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ZiplineRemote"):InvokeServer(unpack({ "Exit" }));
                    task.wait(1);
                    local tbl_109 = { "Enter", -1, Vector3.new(-1290.2841796875, 18.644325256347656, -5087.14990234375) };
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ZiplineRemote"):InvokeServer(unpack(tbl_109));
                    task.wait(3);
                    getgenv().attempts = getgenv().attempts + 1;
                else
                    getgenv().busy = false;
                    getgenv().zipl = true;
                end;
            elseif player.Character:GetAttribute("SpeedBypass") >= 250 then
                getgenv().busy = false;
                getgenv().zipl = true;
            end;
        else
            local child_136 = workspace.NPCs:FindFirstChild("Santa's Sleigh");
            local var_1606 = get_presents();
            if #var_1606 <= getgenv().presents and getgenv().auto_open_gifts then
                repeat
                    task.wait();
                    for var_1607, var_1608 in next, var_1606, nil do
                        local var_1609 = get_closest_present();
                        if not var_1609 then
                            warn("break");
                            getgenv().break_state = true;
                            break;
                        end;
                        local var_1610 = var_1609:GetPivot().Position;
                        if math.abs(var_1610.X) > 20000 or math.abs(var_1610.Y) > 20000 or math.abs(var_1610.Z) > 20000 then
                            warn("Present out of bounds, skipping");
                            var_1609:Destroy();
                        elseif not func_181(var_1610, 10, true) then
                            horo_tp_pro({
                                Position = var_1610,
                                Speed = 50,
                                y = 1000,
                                Stop = function()
                                    return getgenv().auto_santa;
                                end
                            });
                        else
                            func_70(var_1610 + Vector3.new(0, 5, 0));
                            task.wait(0.1);
                            local child_137 = var_1609:FindFirstChild("ProximityPrompt", true);
                            if child_137 and child_137.Enabled then
                                fireproximityprompt(child_137);
                                task.wait(0.1);
                            else
                                var_1609:Destroy();
                            end;
                        end;
                    end;
                until #var_1606 < 1 or getgenv().break_state;
                getgenv().break_state = false;
            elseif not child_136 then
                twunu(game:GetService("ReplicatedStorage").CompassGuider.Santa);
            elseif (child_136:GetPivot().Position - Vector3.new(-5979, 57, -11496)).Magnitude < 1000 then
                warn("santa is in martine base we dont go");
            else
                local var_1611 = child_136:GetPivot().Position;
                if not func_181(var_1611, 10, true) then
                    twunu(child_136);
                else
                    if player.Character:GetAttribute("SpeedBypass") > 200 and getgenv().attack_santa then
                        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("ZiplineRemote"):InvokeServer(unpack({ "Exit" }));
                    end;
                    func_70(var_1611);
                    if getgenv().attack_santa then
                        task.spawn(function()
                            getgenv().global_hit(child_136);
                            return ;
                        end);
                    end;
                end;
            end;
        end;
    end;
    return ;
end);
santa_section.create_toggle({ title = "Open Gifts/Presents", desc = "Toggles wether to open presents", default = false }, function(arg_556)
    getgenv().auto_open_gifts = arg_556;
    return ;
end);
santa_section.create_toggle({ title = "Attack Santa", desc = "Toggles wether to attack big boy santa", default = true }, function(arg_557)
    getgenv().attack_santa = arg_557;
    return ;
end);
santa_section.create_slider({ title = "Open At X Presents", min = 1, max = 50, default = 5, precise = false }, function(arg_558)
    getgenv().presents = arg_558;
    return ;
end);
santa_section.create_box({ title = "Webhook", placeholder = "https://discord.com/api/webhooks/...", default = "", Number = false }, function(arg_559)
    getgenv().webhook_santa = arg_559;
    return ;
end);
santa_section.create_dropdown({ title = "Drops", List = santa_drops, default = {}, search = false, selected = true }, function(arg_560, arg_561)
    local var_1612 = getgenv().drops_webhook;
    if arg_561 then
        if not table.find(var_1612, arg_560) then
            table.insert(var_1612, arg_560);
        end;
    else
        for key_175, value_177 in ipairs(var_1612) do
            if value_177 == arg_560 then
                table.remove(var_1612, key_175);
                return ;
            end;
        end;
    end;
    return ;
end);
(function()
    return #(function()
        return (string.gsub("MIICWwIBAAKBgHvWktOQ5Tv0fArp1MB8guLrj+b7dun0WQ3Yj0HPJQ5HUkUXbf+C0KVD9exIiqPRVov1UO+dEqDsdLWJWwp3sSoP8oJMb9V8AOW8ldQ9wljlHvhGp6tUPjPiUWUTTDVmpmQyVH+lDJvsrx/iEsIm02Uwb/sqe1EzJmnekFU5fYxtAgMBAAECgYAbdSka9b6N+5hhRxVgFHtk2vwOJ+Vm3EG+myywV8CMmO252gcOpy+h5R6q5C6xQeIhqCPo7PfLbII4Ghtwh7dlHK/DVlpZMJi+YYrQlaihpvc7GgEaBMV1U/SehRxVApGPrpqZEzeW9xp8d2lOQQw+kaAF6aOgTb3/qVWhrR43VQJBAO9vXR5jTO0hriIDaY92FPjKsAXAmGeifApqq0Ihjze99zbQ91A3NvIyzHBWAP+IVgvKU8RKR4x5mRmywfwyky8CQQCEZ993IcVE3Km+Gl5pJTgt69gvNgkCAVEJtPczgpf3uEXMWMfPfhizNwI2NjcqvMlzWgez0onUXpqx6+plzCMjAkAuEszlYxBTdS6v5t4yN/OzKDWU9cmd75tU2c/YquKHRGvExZNBgU52bqX+W95XTaHZJPbXh7+a3W9Dja1cXVMLAkBwI9GeApojUojpG0gu4XkNW5yDj85dRmQuRni/xbGT+xwSkHbag7/DualD5PMgXTh5XBz97ikSFvYKDvc9NIHlAkEAvA+7KF6fjVaWUPNepwJ5tZhXFXeY7W7SnQ3B+V4nImvniFjQRcay/rpo5HPSIH2uTb2saCNl/GoHVosUbkM3og==", "==", ""));
    end)();
end)();
twunu2 = function(arg_562, arg_563, arg_564, arg_565, arg_566, arg_567)
    local var_1613 = arg_563 or func_67() - 15;
    local var_1614 = arg_564 or 150;
    local var_1615 = arg_565 or 500;
    if arg_566 == nil then
        arg_566 = true;
    end;
    if arg_567 == nil then
        arg_567 = true;
    end;
    local flag_118 = nil;
    local flag_119 = nil;
    if typeof(arg_562) == "Vector3" then
        flag_118 = arg_562;
    elseif typeof(arg_562) == "Instance" then
        if arg_562:IsA("ObjectValue") then
            flag_119 = arg_562;
            if not arg_562.Value then
                return ;
            end;
            flag_118 = arg_562.Value:GetPivot().Position;
        else
            flag_119 = arg_562;
            flag_118 = arg_562:GetPivot().Position;
        end;
    end;
    if not flag_118 then
        return ;
    end;
    if tbl_69.running and tbl_69.track_object == flag_119 and flag_119 then
        return ;
    end;
    if tbl_69.running and tbl_69.target and not flag_119 and (Vector3.new(tbl_69.target.X, 0, tbl_69.target.Z) - Vector3.new(flag_118.X, 0, flag_118.Z)).Magnitude < 1 then
        return ;
    end;
    if tbl_69.running then
        tbl_69.target = flag_118;
        tbl_69.track_object = flag_119;
        tbl_69.cancel = true;
        return ;
    end;
    (function()
        local flag_120 = false;
        local var_1616 = player.Character;
        local var_1617 = var_1616 and var_1616:FindFirstChild(var);
        local var_1618 = var_1616 and var_1616:FindFirstChild("Humanoid");
        if not var_1617 or not var_1618 then
            return ;
        end;
        tbl_69.target = flag_118;
        tbl_69.track_object = flag_119;
        tbl_69.running = true;
        tbl_69.cancel = false;
        getgenv().busy = true;
        var_1618:UnequipTools();
        local game_97 = game:GetService("PathfindingService");
        local var_1619 = RaycastParams.new();
        var_1619.FilterType = Enum.RaycastFilterType.Exclude;
        var_1619.FilterDescendantsInstances = { var_1616 };
        local var_1620 = var_1617.Position.Y;
        local num_163 = 0;
        local tbl_110 = {};
        local num_164 = 1;
        local flag_121 = nil;
        local num_165 = 0;
        local tbl_111 = {};
        local func_199 = function()
            for key_176, value_178 in ipairs(tbl_111) do
                if value_178 and value_178.Parent then
                    value_178:Destroy();
                end;
            end;
            tbl_111 = {};
            return ;
        end;
        local func_200 = function(arg_568)
            func_199();
            if not arg_567 then
                return ;
            end;
            for key_177, value_179 in ipairs(arg_568) do
                if key_177 ~= 1 then
                    local var_1621 = Instance.new("Part");
                    var_1621.Name = "WaypointVisual_" .. key_177;
                    var_1621.Anchored = true;
                    var_1621.CanCollide = false;
                    var_1621.CanQuery = false;
                    var_1621.CanTouch = false;
                    var_1621.Size = Vector3.new(1, 1, 1);
                    var_1621.Shape = Enum.PartType.Ball;
                    var_1621.Material = Enum.Material.Neon;
                    var_1621.Color = value_179.Action == Enum.PathWaypointAction.Jump and Color3.fromRGB(255, 100, 100) or Color3.fromRGB(100, 255, 100);
                    var_1621.Position = value_179.Position;
                    var_1621.Transparency = 0.3;
                    var_1621.Parent = workspace;
                    table.insert(tbl_111, var_1621);
                    if key_177 > 2 then
                        local var_1622 = arg_568[key_177 - 1];
                        local var_1623 = (value_179.Position - var_1622.Position).Magnitude;
                        local var_1624 = Instance.new("Part");
                        var_1624.Name = "WaypointLine_" .. key_177;
                        var_1624.Anchored = true;
                        var_1624.CanCollide = false;
                        var_1624.CanQuery = false;
                        var_1624.CanTouch = false;
                        var_1624.Size = Vector3.new(0.2, 0.2, var_1623);
                        var_1624.Material = Enum.Material.Neon;
                        var_1624.Color = Color3.fromRGB(100, 200, 255);
                        var_1624.CFrame = CFrame.lookAt(var_1622.Position, value_179.Position) * CFrame.new(0, 0, -var_1623 / 2);
                        var_1624.Transparency = 0.5;
                        var_1624.Parent = workspace;
                        table.insert(tbl_111, var_1624);
                    end;
                end;
            end;
            return ;
        end;
        local func_201 = function(arg_569, arg_570)
            local num_166 = math.max(arg_570 + var_1614, var_1615 + var_1614);
            local vector_71 = Vector3.new(arg_569.X, num_166, arg_569.Y);
            local vector_72 = Vector3.new(0, -(num_166 + 1000), 0);
            local workspace_13 = workspace:Raycast(vector_71, vector_72, var_1619);
            if workspace_13 then
                local var_1625 = workspace_13.Position.Y + 3;
                if var_1625 <= var_1615 then
                    return var_1625;
                end;
                local var_1626 = workspace_13.Position.Y - 1;
                local vector_73 = Vector3.new(arg_569.X, var_1626, arg_569.Y);
                local workspace_14 = workspace:Raycast(vector_73, Vector3.new(0, -2000, 0), var_1619);
                if workspace_14 then
                    local var_1627 = workspace_14.Position.Y + 3;
                    if var_1627 <= var_1615 then
                        return var_1627;
                    end;
                end;
            end;
            return nil;
        end;
        local func_202 = function(arg_571, arg_572)
            local var_1628 = game_97:CreatePath({ AgentRadius = 2, AgentHeight = 5, AgentCanJump = true, AgentCanClimb = false, WaypointSpacing = 4 });
            local var_1629, var_1630 = pcall(function()
                var_1628:ComputeAsync(arg_571, arg_572);
                return ;
            end);
            if var_1629 and var_1628.Status == Enum.PathStatus.Success then
                return var_1628:GetWaypoints();
            end;
            return nil;
        end;
        local func_203 = function(arg_573)
            if not arg_566 then
                return false;
            end;
            if not flag_121 then
                return true;
            end;
            if (arg_573 - flag_121).Magnitude > 10 then
                return true;
            end;
            if #tbl_110 == 0 then
                return true;
            end;
            if num_164 < #tbl_110 then
                return true;
            end;
            return false;
        end;
        local var_1631 = var_1620;
        while true do
            while true do
                var_1613 = func_67() - 15;
                if tbl_69.cancel then
                    break;
                end;
                if not getgenv().fish_kaitun.Enabled and not getgenv().auto_santa and not getgenv().auto_impel then
                    flag_120 = true;
                end;
                if flag_120 then
                    break;
                end;
                if flag_119 then
                    if flag_119:IsA("ObjectValue") then
                        if not flag_119.Value then
                            flag_120 = true;
                        end;
                        if not flag_120 then
                            flag_118 = flag_119.Value:GetPivot().Position;
                        end;
                    else
                        flag_118 = flag_119:GetPivot().Position;
                    end;
                end;
                if flag_120 then
                    break;
                end;
                local var_1632 = task.wait();
                num_165 = num_165 + var_1632;
                local var_1633 = var_1617.Position;
                if (Vector3.new(flag_118.X, 0, flag_118.Z) - Vector3.new(var_1633.X, 0, var_1633.Z)).Magnitude <= 5 then
                    var_1617.CFrame = CFrame.new(flag_118);
                    var_1617.Velocity = Vector3.zero;
                    var_1617.AssemblyLinearVelocity = Vector3.zero;
                    flag_120 = true;
                end;
                if flag_120 then
                    break;
                end;
                if arg_566 and (func_203(flag_118) or num_165 > 1) then
                    local var_1634 = func_202(var_1633, flag_118);
                    if var_1634 and #var_1634 > 1 then
                        tbl_110 = var_1634;
                        num_164 = 2;
                        flag_121 = flag_118;
                        num_165 = 0;
                        func_200(tbl_110);
                    else
                        tbl_110 = {};
                        num_164 = 1;
                        func_199();
                    end;
                end;
                local var_1635;
                if arg_566 and #tbl_110 > 0 and num_164 <= #tbl_110 then
                    var_1635 = tbl_110[num_164].Position;
                    if (Vector3.new(var_1635.X, 0, var_1635.Z) - Vector3.new(var_1633.X, 0, var_1633.Z)).Magnitude <= 3 then
                        if arg_567 and tbl_111[num_164 - 1] then
                            tbl_111[num_164 - 1].Color = Color3.fromRGB(150, 150, 150);
                            tbl_111[num_164 - 1].Transparency = 0.7;
                        end;
                        num_164 = num_164 + 1;
                        if #tbl_110 < num_164 then
                            var_1635 = flag_118;
                        else
                            var_1635 = tbl_110[num_164].Position;
                        end;
                    end;
                else
                    var_1635 = flag_118;
                end;
                local vector_74 = Vector2.new(var_1633.X, var_1633.Z);
                local vector_75 = Vector2.new(var_1635.X, var_1635.Z) - vector_74;
                local var_1636 = vector_75.Magnitude;
                if not (var_1636 < 0.1) then
                    local var_1637 = vector_75.Unit;
                    local var_1638 = vector_74 + var_1637 * math.min(var_1613 * var_1632, var_1636);
                    local var_1639 = func_201(vector_74 + var_1637 * math.min(var_1613 * 0.25, math.max(var_1636, 0.1)), var_1620);
                    if var_1639 then
                        var_1631 = var_1639;
                    end;
                    num_163 = num_163 + ((var_1631 - var_1620) * 200 - num_163 * 25) * var_1632;
                    var_1620 = var_1620 + num_163 * var_1632;
                    if var_1615 < var_1620 then
                        var_1620 = var_1615;
                        if num_163 > 0 then
                            num_163 = 0;
                        end;
                    end;
                    local vector_76 = Vector3.new(var_1637.X, 0, var_1637.Y);
                    var_1617.CFrame = CFrame.lookAt(Vector3.new(var_1638.X, var_1620, var_1638.Y), Vector3.new(var_1638.X, var_1620, var_1638.Y) + vector_76);
                    var_1617.Velocity = Vector3.zero;
                    var_1617.AssemblyLinearVelocity = Vector3.zero;
                end;
            end;
            if not flag_120 then
                if not flag_120 then
                    if not flag_120 then
                        tbl_69.cancel = false;
                        flag_119 = tbl_69.track_object;
                    end;
                end;
            end;
            if flag_120 or (flag_120 or (flag_120 or not not flag_119)) then
                if flag_120 or (flag_120 or (flag_120 or not not flag_119:IsA("ObjectValue"))) then
                    if flag_120 or (flag_120 or (flag_120 or not flag_119.Value)) then
                        flag_120 = false;
                        func_199();
                        getgenv().busy = false;
                        tbl_69.running = false;
                        tbl_69.target = nil;
                        tbl_69.track_object = nil;
                        return ;
                    end;
                    flag_118 = flag_119.Value:GetPivot().Position;
                else
                    flag_118 = flag_119:GetPivot().Position;
                end;
            else
                flag_118 = tbl_69.target;
            end;
            tbl_110 = {};
            num_164 = 1;
            flag_121 = nil;
            func_199();
        end;
    end)();
    return ;
end;
loot_chest = function()
    local flag_122 = nil;
    local num_167 = math.huge;
    for var_1640, var_1641 in next, workspace.Effects:GetChildren() do
        if var_1641:FindFirstChildWhichIsA("ProximityPrompt", true) and var_1641:IsA("Model") and var_1641:FindFirstChildWhichIsA("ProximityPrompt", true).Parent:IsA("MeshPart") then
            distance2 = (var_1641:GetPivot().Position - player.Character:GetPivot().Position).Magnitude;
            if distance2 < num_167 then
                flag_122 = var_1641;
                num_167 = distance2;
            end;
        end;
    end;
    if flag_122 then
        if not func_181(flag_122:GetPivot().Position, 10, true) then
            twunu2(flag_122:GetPivot().Position, 50, 30, 2090);
        else
            local child_138 = flag_122:FindFirstChild("ProximityPrompt", true);
            local workspace_15 = workspace.CurrentCamera;
            local local_player_13 = game.Players.LocalPlayer.Character.HumanoidRootPart;
            local var_1642 = child_138.Parent.Position;
            local var_1643 = workspace_15.CFrame;
            local_player_13.CFrame = CFrame.new(var_1642.X, var_1642.Y + 5, var_1642.Z) * CFrame.Angles(-math.pi / 2, 0, 0);
            workspace_15.CameraSubject = child_138.Parent;
            workspace_15.CFrame = CFrame.lookAt(local_player_13.Position, var_1642);
            task.wait();
            child_138:InputHoldBegin();
            task.wait(child_138.HoldDuration + 0.1);
            child_138:InputHoldEnd();
            workspace_15.CameraSubject = game.Players.LocalPlayer.Character:FindFirstChild("Humanoid");
            workspace_15.CFrame = var_1643;
        end;
    end;
    return ;
end;
get_stat = function(arg_574)
    local num_168 = 0;
    for var_1644, var_1645 in next, player.PlayerGui.Statistics.Main.Stats:GetChildren() do
        local child_139 = var_1645:FindFirstChild("Stat", true);
        local child_140 = var_1645:FindFirstChild("Amount", true);
        if child_139 and child_140 and child_139.Text == arg_574 then
            num_168 = tonumber(child_140.Text);
        end;
    end;
    return num_168;
end;
local func_204 = function()
    local num_169 = 1;
    for var_1646, var_1647 in next, game:GetService("Players").LocalPlayer.PlayerGui.ImpelDownUI.Info.Timers:GetChildren() do
        if var_1647.Name:match("Floor") then
            local var_1648 = var_1647.Name:match("%d+");
            if var_1648 then
                num_169 = tonumber(var_1648);
                return num_169;
            end;
        end;
    end;
    return num_169;
end;
zones_really = { Vector3.new(2951, 2081, -13983) };
has_zone = function()
    for var_1649, var_1650 in next, workspace.NPCs:GetChildren() do
        if var_1650:IsA("Model") and var_1650:FindFirstChild("Humanoid") then
            for var_1651, var_1652 in next, zones_really, nil do
                target_distance = (var_1650:GetPivot().Position - var_1652).Magnitude;
                if target_distance < 150 then
                    return var_1652;
                end;
            end;
        end;
    end;
    return nil;
end;
put_mines = function()
    local child_141 = ReplicatedStorage:FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Mines");
    if not child_141 then
        task.spawn(function()
            ReplicatedStorage:FindFirstChild("Events"):FindFirstChild("Skill"):InvokeServer("Explosive Mines");
            return ;
        end);
    else
        task.wait(1.5);
        child_141:InvokeServer({ cf = player.Character.HumanoidRootPart.CFrame });
    end;
    return ;
end;
HasRange = function()
    return ;
end;
impel_section.create_toggle({ title = "Auto Impel Down", desc = "Farms Impel Down", default = false }, function(arg_575)
    if arg_575 then
        library.create_noti({ title = "Soon", desc = "This feature is not finished yet", show_time = 4 });
        return ;
    end;
    getgenv().auto_impel = arg_575;
    task.spawn(function()
        while getgenv().auto_impel and task.wait(1) do
            fake_geppo();
            if player.Character:FindFirstChild("Bomb-Bomb") or player.Backpack:FindFirstChild("Bomb-Bomb") then
                if get_stat("Devil Fruit") < 700 then
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("stats"):FireServer(unpack({ "DevilFruitMastery", nil, 700 }));
                end;
                local game_98 = game:GetService("ReplicatedStorage"):FindFirstChild(player.Name .. "|ServerScriptService.Skills.Skills.SkillContainer.Bomb-Bomb.Explosive Leap");
                if not game_98 then
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Skill"):InvokeServer(unpack({ "Explosive Leap" }));
                else
                    game_98:FireServer({ cf = CFrame.new(0, 0, 0), extra = 0 });
                end;
            end;
        end;
        return ;
    end);
    while getgenv().auto_impel and task.wait() do
        if game.PlaceId ~= 11424731604 then
            local vector_77 = Vector3.new(5878, 9, -10214);
            if not func_181(vector_77, 10, true) then
                horo_tp_pro({
                    Position = vector_77,
                    Speed = 50,
                    Stop = function()
                        return getgenv().auto_impel;
                    end
                });
            else
                func_70(vector_77);
            end;
        elseif not player.PlayerGui:FindFirstChild("diffChooser") then
            if func_204() == 1 then
                warn("FLOOR 1");
                if func_181(Vector3.new(5906, 9, -10197), 300, true) then
                    local child_142 = workspace.NPCs:FindFirstChild("Vera");
                    if child_142 and not func_181(child_142:GetPivot().Position, 10, true) then
                        horo_tp_pro({
                            Position = child_142:GetPivot().Position,
                            Speed = 50,
                            Stop = function()
                                return getgenv().auto_impel;
                            end
                        });
                    elseif child_142 and func_181(child_142:GetPivot().Position, 10, true) then
                        func_70(child_142:GetPivot().Position);
                        task.spawn(function()
                            getgenv().global_hit(child_142);
                            return ;
                        end);
                    end;
                elseif player.Character:FindFirstChild("LeftCuff") then
                    local child_143 = workspace.Effects:FindFirstChild("Key");
                    if child_143 then
                        if not func_181(child_143:GetPivot().Position, 10, true) then
                            horo_tp_pro({
                                Position = child_143:GetPivot().Position,
                                y = child_143:GetPivot().Position.Y,
                                Speed = 50,
                                Stop = function()
                                    return getgenv().auto_impel;
                                end
                            });
                        else
                            func_70(child_143:GetPivot().Position);
                            local child_144 = child_143:FindFirstChild("ProximityPrompt", true);
                            task.wait();
                            child_144:InputHoldBegin();
                            task.wait(child_144.HoldDuration);
                            child_144:InputHoldEnd();
                            task.wait();
                        end;
                    end;
                elseif not player.Backpack:FindFirstChild("Bomb-Bomb") then
                    warn("we dont have bomb");
                    if not player.Character:FindFirstChild("Bomb") then
                        if player.Backpack:FindFirstChild("Bomb") then
                            player.Backpack:FindFirstChild("Bomb").Parent = player.Character;
                            task.wait(1);
                        elseif workspace.Effects:FindFirstChild("Bomb") then
                            if not func_181(workspace.Effects:FindFirstChild("Bomb"):GetPivot().Position, 10, true) then
                                twunu2(workspace.Effects:FindFirstChild("Bomb"):GetPivot().Position, 50, 30, 2090);
                            else
                                local child_145 = workspace.Effects:FindFirstChild("Bomb"):FindFirstChild("ProximityPrompt", true);
                                warn(child_145:GetFullName());
                                child_145:InputHoldBegin();
                                task.wait(child_145.HoldDuration + 0.1);
                                child_145:InputHoldEnd();
                            end;
                        else
                            loot_chest();
                        end;
                    elseif not player.PlayerGui:FindFirstChild("ConfirmationPrompt") then
                        player.Character:FindFirstChild("Bomb"):Activate();
                    else
                        firesignal(game:GetService("Players").LocalPlayer.PlayerGui.ConfirmationPrompt.Main.OptionsFrame.Accept.MouseButton1Click);
                    end;
                elseif player.Backpack:FindFirstChild("Bomb-Bomb") then
                    warn("we do have bomb");
                    local workspace_16 = workspace.Islands["Impel Base - Floor 1"].Barriers:FindFirstChild("StartBarrier");
                    if workspace_16 and workspace_16.CanCollide then
                        warn("twuning");
                        twunu2(workspace_16.Position, 50, 30, 2090, false);
                    end;
                    if has_zone() then
                        if not func_181(has_zone(), 10, true) then
                            warn("twuning");
                            twunu2(has_zone(), 50, 30, 2090, false);
                        else
                            func_70(has_zone() + Vector3.new(0, 4, 0));
                            put_mines();
                        end;
                    else
                        local vector_78 = Vector3.new(0, 0, -40);
                        if workspace.Effects.Zones:FindFirstChild("End") then
                            local child_146 = workspace.Effects.Zones:FindFirstChild("End").Position + vector_78;
                            if not func_181(child_146, 10, true) then
                                twunu(child_146, 50, 3000);
                            else
                                func_70(child_146 + Vector3.new(0, 4, 0));
                                put_mines();
                            end;
                        else
                            twunu(Vector3.new(2880, 2783, -14473) + vector_78, 50, 3000);
                        end;
                    end;
                end;
            elseif func_204() == 2 then
                if func_181(Vector3.new(3199, 2405, -20056), 50, true) then
                    twunu(Vector3.new(3201, 2384, -20276));
                end;
                if not HasRange() then
                end;
            end;
        end;
    end;
    return ;
end);
task.spawn(function()
    task.wait(0.5);
    local game_99 = game:GetService("Players").LocalPlayer;
    local game_100 = game:GetService("HttpService");
    local flag_123 = false;
    if isfile(account_file_path) then
        local var_1653, var_1654 = pcall(function()
            return game_100:JSONDecode(readfile(account_file_path));
        end);
        if var_1653 and type(var_1654) == "table" then
            local var_1655 = (var_1654.accounts or {})[game_99.Name];
            if var_1655 then
                local flag_124 = nil;
                if type(var_1655) == "table" and var_1655.config and var_1655.config ~= "" then
                    flag_124 = var_1655.config;
                end;
                if flag_124 then
                    local var_1656, var_1657 = getgenv().feral_config.load(flag_124);
                    if var_1656 then
                        library.create_noti({ title = "Config", desc = "Auto-loaded account config \"" .. flag_124 .. "\" for " .. game_99.Name, show_time = 4 });
                        flag_123 = true;
                    else
                        library.create_noti({ title = "Config", desc = "Account config load failed: " .. tostring(var_1657), show_time = 4 });
                    end;
                end;
            end;
        end;
    end;
    if not flag_123 then
        if not var_1354 then
            return ;
        end;
        if not func_172() then
            return ;
        end;
        local var_1658 = func_174();
        local var_1659, var_1660 = getgenv().feral_config.load(var_1658);
        if var_1659 then
            library.create_noti({ title = "Config", desc = "Auto-loaded \"" .. var_1658 .. "\"", show_time = 4 });
        else
            library.create_noti({ title = "Config", desc = "Auto-load failed: " .. tostring(var_1660), show_time = 4 });
        end;
    end;
    return ;
end);
return ;

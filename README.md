-- LOAD FLUENT MODDED LIBRARY
local Fluent = loadstring(game:HttpGet("https://github.com/StyearX/Fluent-Modded/releases/download/N/Fluent.lua"))()

local SaveManager = Fluent.SaveManager
local InterfaceManager = Fluent.InterfaceManager

-- [ هنا نجلب معلومات اللاعب أولاً ]
local Player = game.Players.LocalPlayer
local DisplayName = Player.DisplayName
local Name = Player.Name
local UserId = Player.UserId
local AccountAge = Player.AccountAge
local CreationDate = os.date("%Y-%m-%d", os.time() - (AccountAge * 86400))

-- WINDOW
local Window = Fluent:CreateWindow({
    Title = "ABC TOP ON",
    SubTitle = "dev : hamza000599",
    TabWidth = 160,
    Size = UDim2.fromOffset(480, 460),
    Acrylic = true,
    Theme = "Blood Red",
    MinimizeKey = Enum.KeyCode.LeftControl,
    Search = true,
})

-------------------------------------------------
-- TABS (إضافة التبويب هنا)
-------------------------------------------------

local HomeTab = Window:AddTab({ Title = "Home", Icon = "home" })
local MinaTab = Window:AddTab({ Title = "Welcome", Icon = "info" }) 
local PlayerTab = Window:AddTab({ Title = "Player", Icon = "user" })
local ScriptTab = Window:AddTab({ Title = "Script", Icon = "code" })
local MiscTab = Window:AddTab({ Title = "Misc", Icon = "box" })
local NewsTab = Window:AddTab({ Title = "News", Icon = "newspaper" })
local SettingsTab = Window:AddTab({ Title = "Settings", Icon = "settings" })
local FunTab = Window:AddTab({ Title = "Fun", Icon = "gamepad" })                                           local StatsTab = Window:AddTab({ Title = "Stats", Icon = "bar-chart" })                local EnvironmentTab = Window:AddTab({ Title = "Environment", Icon = "globe" })                                                                                                                                              local EliteTab = Window:AddTab({ Title = "✨ Elite Access", Icon = "star" })local InfoTab = Window:AddTab({ Title = "ℹ️ Information", Icon = "info" })
-------------------------------------------------
-- MINA SECTION CONTENT (محتوى قسم مينا)
-------------------------------------------------
MinaTab:AddParagraph({
    Title = "Welcome",
    Content = "Welcome to the server! We hope you enjoy your time here.\n\n✨ You have lightened the server ✨"
})                                                                                                                                                               
 -- DEV INFO (Clickable Copy)

HomeTab:AddButton({
    Title = "Developer: hamza000599",
    Description = "Click to copy developer name",
    Callback = function()
        setclipboard("hamza000599")

        Fluent:Notify({
            Title = "Copied",
            Content = "Developer name copied!",
            Duration = 3
        })
    end
})

HomeTab:AddButton({
    Title = "Discord: hamza.abc0",
    Description = "Click to copy Discord username",
    Callback = function()
        setclipboard("hamza.abc0")

        Fluent:Notify({
            Title = "Copied",
            Content = "Discord username copied!",
            Duration = 3
        })
    end
})
-- PLAYER SECTION (ALL ENGLISH)
-------------------------------------------------

-- Variables for Features
local InfiniteJumpEnabled = false
local FlyEnabled = false
local FlySpeed = 50

-- 1. WalkSpeed
PlayerTab:AddInput("WalkSpeedInput", {
    Title = "WalkSpeed",
    Default = "16",
    Placeholder = "Enter Speed...",
    Numeric = true,
    Finished = true,
    Callback = function(Value)
        if Player.Character and Player.Character:FindFirstChild("Humanoid") then
            Player.Character.Humanoid.WalkSpeed = tonumber(Value)
        end
    end
})

-- 2. JumpPower
PlayerTab:AddInput("JumpPowerInput", {
    Title = "JumpPower",
    Default = "50",
    Placeholder = "Enter Jump Power...",
    Numeric = true,
    Finished = true,
    Callback = function(Value)
        if Player.Character and Player.Character:FindFirstChild("Humanoid") then
            Player.Character.Humanoid.JumpPower = tonumber(Value)
        end
    end
})

-- 3. Gravity
PlayerTab:AddInput("GravityInput", {
    Title = "Gravity",
    Default = "196.2",
    Placeholder = "Enter Gravity...",
    Numeric = true,
    Finished = true,
    Callback = function(Value)
        game.Workspace.Gravity = tonumber(Value)
    end
})

-- 4. Infinite Jump
PlayerTab:AddToggle("InfJump", {
    Title = "Infinite Jump", 
    Default = false,
    Callback = function(Value)
        InfiniteJumpEnabled = Value
    end
})

game:GetService("UserInputService").JumpRequest:Connect(function()
    if InfiniteJumpEnabled then
        Player.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

-- 5. Fly & Fly Speed
PlayerTab:AddToggle("FlyToggle", {
    Title = "Fly",
    Default = false,
    Callback = function(Value)
        FlyEnabled = Value
        if FlyEnabled then
            -- Start Fly Logic here or call a function
            Fluent:Notify({Title = "Fly", Content = "Fly Enabled", Duration = 2})
        else
            Fluent:Notify({Title = "Fly", Content = "Fly Disabled", Duration = 2})
        end
    end
})

PlayerTab:AddInput("FlySpeedInput", {
    Title = "Fly Speed",
    Default = "50",
    Placeholder = "Enter Fly Speed...",
    Numeric = true,
    Finished = true,
    Callback = function(Value)
        FlySpeed = tonumber(Value)
    end
})

-- 6. Swim (Noclip Swim)
PlayerTab:AddToggle("SwimToggle", {
    Title = "Swim in Air",
    Default = false,
    Callback = function(Value)
        if Value then
            Player.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.GettingUp, false)
            Player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Swimming)
        else
            Player.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.GettingUp, true)
            Player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)
        end
    end
})
-------------------------------------------------
-- MISC SECTION CONTENT
-------------------------------------------------

-- 1. Anti-AFK Logic
MiscTab:AddButton({
    Title = "Enable Anti-AFK",
    Description = "Prevents you from being kicked for inactivity",
    Callback = function()
        local vu = game:GetService("VirtualUser")
        game:GetService("Players").LocalPlayer.Idled:Connect(function()
            vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
            wait(1)
            vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        end)
        Fluent:Notify({Title = "Anti-AFK", Content = "Anti-AFK is now Active!", Duration = 3})
    end
})

-- 2. Rejoin Server
MiscTab:AddButton({
    Title = "Rejoin Server",
    Description = "Reconnect to the same server",
    Callback = function()
        game:GetService("TeleportService"):Teleport(game.PlaceId, game:GetService("Players").LocalPlayer)
    end
})

-- 3. Server Hop
MiscTab:AddButton({
    Title = "Server Hop",
    Description = "Join a different random server",
    Callback = function()
        local Http = game:GetService("HttpService")
        local TPS = game:GetService("TeleportService")
        local Api = "https://roblox.com" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
        local function GetServers(cursor)
            local Raw = game:HttpGet(Api .. (cursor and "&cursor=" .. cursor or ""))
            return Http:JSONDecode(Raw)
        end
        local Servers = GetServers()
        TPS:TeleportToPlaceInstance(game.PlaceId, Servers.data[math.random(1, #Servers.data)].id)
    end
})

-- 4. Anti-Fling
MiscTab:AddToggle("AntiFling", {
    Title = "Anti-Fling",
    Default = false,
    Callback = function(Value)
        _G.AntiFling = Value
        while _G.AntiFling do
            wait(0.1)
            for _, player in pairs(game.Players:GetPlayers()) do
                if player ~= game.Players.LocalPlayer and player.Character then
                    for _, part in pairs(player.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = false
                            part.Velocity = Vector3.new(0, 0, 0)
                            part.RotVelocity = Vector3.new(0, 0, 0)
                        end
                    end
                end
            end
        end
    end
})

-- 5. Time Control (Morning/Night)
MiscTab:AddButton({
    Title = "Set Morning",
    Callback = function()
        game.Lighting.ClockTime = 12
    end
})

MiscTab:AddButton({
    Title = "Set Night",
    Callback = function()
        game.Lighting.ClockTime = 0
    end
})

-- 6. Lag Reducer (FPS Boost)
MiscTab:AddButton({
    Title = "Reduce Lag (FPS Boost)",
    Description = "Removes textures and effects to boost performance",
    Callback = function()
        local decalsyeeter = true
        local g = game
        local w = g.Workspace
        local l = g.Lighting
        local t = w:FindFirstChildOfClass("Terrain")
        t.WaterWaveSize = 0
        t.WaterWaveSpeed = 0
        t.WaterReflectance = 0
        t.WaterTransparency = 0
        l.GlobalShadows = false
        l.FogEnd = 9e9
        settings().Rendering.QualityLevel = "Level01"
        for i, v in pairs(g:GetDescendants()) do
            if v:IsA("Part") or v:IsA("Union") or v:IsA("CornerWedgePart") or v:IsA("TrussPart") then
                v.Material = "Plastic"
                v.Reflectance = 0
            elseif v:IsA("Decal") or v:IsA("Texture") then
                v:Destroy()
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Enabled = false
            end
        end
        Fluent:Notify({Title = "Lag Reducer", Content = "Performance Optimized!", Duration = 3})
    end
})
-------------------------------------------------
-- NEWS & ABOUT SECTION (ALL ENGLISH)
-------------------------------------------------

-- General News Paragraph
NewsTab:AddParagraph({
    Title = "News & Updates",
    Content = "Welcome to ABC Hub!\n• Added new Player features (Fly, Speed, Jump).\n• Added Misc tools (Anti-AFK, Server Hop, FPS Boost).\n• Fully translated to English."
})

-- About Us Button
NewsTab:AddButton({
    Title = "About Us",
    Description = "Information about the developers",
    Callback = function()
        -- Popup notification that lasts for 5 seconds
        Fluent:Notify({
            Title = "ABC Team",
            Content = "We are the ABC Team! Dedicated to providing the best script experience.",
            SubTitle = "Developer Info",
            Duration = 5 -- Disappears after 5 seconds
        })
    end
})

-- Additional Info
NewsTab:AddParagraph({
    Title = "Version Status",
    Content = "Current Version: v1.2.0\nStatus: Undetected\nPlatform: Delta Executor"
})
-- FUN SECTION CONTENT (الميزات الجديدة)

-- 1. وضعية السكران (Drunk Mode) - تجعل الكاميرا تهتز وتتمايل
FunTab:AddToggle("DrunkMode", {
Title = "Drunk Mode 🥴",
Description = "Makes your camera go crazy",
Default = false,
Callback = function(Value)
_G.Drunk = Value
task.spawn(function()
while _G.Drunk do
local intensity = 0.1
local x = math.random(-100, 100)/1000 * intensity
local y = math.random(-100, 100)/1000 * intensity
local z = math.random(-100, 100)/1000 * intensity
game.Workspace.CurrentCamera.CFrame = game.Workspace.CurrentCamera.CFrame * CFrame.Angles(x, y, z)
task.wait()
end
end)
end
})

-- 2. تغيير لون الجسم (Rainbow Character)
FunTab:AddToggle("RainbowChar", {
Title = "Rainbow Body 🌈",
Description = "Changes your body color constantly",
Default = false,
Callback = function(Value)
_G.Rainbow = Value
task.spawn(function()
while _G.Rainbow do
for i = 0, 1, 0.01 do
if not _G.Rainbow then break end
local char = game.Players.LocalPlayer.Character
if char then
for _, part in pairs(char:GetChildren()) do
if part:IsA("BasePart") then
part.Color = Color3.fromHSV(i, 1, 1)
end
end
end
task.wait(0.05)
end
end
end)
end
})

-- 3. تأثير الديسكو (Disco World) - يغير لون السماء والإضاءة
FunTab:AddToggle("DiscoMode", {
Title = "Disco World 🎵",
Description = "Flashy lights everywhere",
Default = false,
Callback = function(Value)
_G.Disco = Value
task.spawn(function()
while _G.Disco do
game:GetService("Lighting").Ambient = Color3.new(math.random(), math.random(), math.random())
game:GetService("Lighting").OutdoorAmbient = Color3.new(math.random(), math.random(), math.random())
task.wait(0.1)
end
game:GetService("Lighting").Ambient = Color3.fromRGB(127, 127, 127) -- إعادة للأصل
end)
end
})

-- 4. الدوران السريع (Spin Bot) - تجعل شخصيتك تدور حول نفسها
FunTab:AddToggle("SpinBot", {
Title = "Spin Bot 🌪️",
Description = "Spin like a tornado",
Default = false,
Callback = function(Value)
_G.Spin = Value
task.spawn(function()
while _G.Spin do
local char = game.Players.LocalPlayer.Character
if char and char:FindFirstChild("HumanoidRootPart") then
char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(20), 0)
end
task.wait()
end
end)
end
})

-- 5. إخفاء الملحقات (No Accessories) - يجعلك أصلع بدون ملابس
FunTab:AddButton({
Title = "Go Bald 🧑‍🦲",
Description = "Remove all accessories from your character",
Callback = function()
local char = game.Players.LocalPlayer.Character
if char then
for _, v in pairs(char:GetChildren()) do
if v:IsA("Accessory") then
v:Destroy()
end
end
end
end
})

-- 6. القفز اللانهائي (Infinite Jump)
FunTab:AddToggle("InfJump", {
Title = "Infinite Jump 🚀",
Description = "Jump as much as you want",
Default = false,
Callback = function(Value)
_G.InfJumpEnabled = Value
game:GetService("UserInputService").JumpRequest:Connect(function()
if _G.InfJumpEnabled then
game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
end
end)
end
})

-- STATS SECTION CONTENT (Session Tracker)

-- [ متغيرات لحساب الإحصائيات منذ بداية التشغيل ]
local StartTime = tick()
local InitialLevel = Player:FindFirstChild("leaderstats") and Player.leaderstats:FindFirstChild("Level") and Player.leaderstats.Level.Value or 0
local InitialMoney = Player:FindFirstChild("leaderstats") and (Player.leaderstats:FindFirstChild("Money") or Player.leaderstats:FindFirstChild("Cash")) and (Player.leaderstats:FindFirstChild("Money") or Player.leaderstats:FindFirstChild("Cash")).Value or 0

-- إضافة الفقرات (Paragraphs) التي سيتم تحديثها
local LevelLabel = StatsTab:AddParagraph({
Title = "Level Tracker",
Content = "Levels Gained: 0"
})

local MoneyLabel = StatsTab:AddParagraph({
Title = "Money Earned",
Content = "Cash Gained: 0"
})

local TimeLabel = StatsTab:AddParagraph({
Title = "Play Time",
Content = "Session Duration: 00:00:00"
})

-- [ حلقة التحديث التلقائي للإحصائيات ]
task.spawn(function()
while task.wait(1) do
-- 1. تحديث الوقت (Play Time)
local Seconds = math.floor(tick() - StartTime)
local Minutes = math.floor(Seconds / 60)
local Hours = math.floor(Minutes / 60)
local TimeStr = string.format("%02d:%02d:%02d", Hours % 24, Minutes % 60, Seconds % 60)
TimeLabel:SetTitle("Play Time: " .. TimeStr)

-- 2. تحديث الليفل (Level Tracker)  
    local CurrentLevel = Player:FindFirstChild("leaderstats") and Player.leaderstats:FindFirstChild("Level") and Player.leaderstats.Level.Value or 0  
    LevelLabel:SetContent("Levels Gained: " .. (CurrentLevel - InitialLevel))  

    -- 3. تحديث الفلوس (Money Earned)  
    local CurrentMoney = Player:FindFirstChild("leaderstats") and (Player.leaderstats:FindFirstChild("Money") or Player.leaderstats:FindFirstChild("Cash")) and (Player.leaderstats:FindFirstChild("Money") or Player.leaderstats:FindFirstChild("Cash")).Value or 0  
    MoneyLabel:SetContent("Cash Gained: " .. (CurrentMoney - InitialMoney))  
end

end)

-- ENVIRONMENT SECTION CONTENT

-- 1. Remove Fog (إزالة الضباب)
EnvironmentTab:AddToggle("RemoveFog", {
Title = "Remove Fog",
Description = "Eliminates fog for maximum visibility",
Default = false,
Callback = function(Value)
if Value then
game:GetService("Lighting").FogEnd = 100000
game:GetService("Lighting").FogStart = 0
else
-- يعيد الإعدادات الافتراضية للعبة (تقريبياً)
game:GetService("Lighting").FogEnd = 1000
end
end
})

-- 2. Water Transparency (شفافية الماء)
EnvironmentTab:AddSlider("WaterTransparency", {
Title = "Water Transparency",
Description = "Adjust how clear the water is",
Default = 0.5,
Min = 0,
Max = 1,
Rounding = 1,
Callback = function(Value)
workspace.Terrain.WaterTransparency = Value
end
})

-- 3. Full Bright (إضاءة كاملة)
EnvironmentTab:AddToggle("FullBright", {
Title = "Full Bright",
Description = "Removes shadows and makes everything bright",
Default = false,
Callback = function(Value)
if Value then
game:GetService("Lighting").Brightness = 2
game:GetService("Lighting").GlobalShadows = false
game:GetService("Lighting").Ambient = Color3.fromRGB(255, 255, 255)
else
game:GetService("Lighting").Brightness = 1
game:GetService("Lighting").GlobalShadows = true
game:GetService("Lighting").Ambient = Color3.fromRGB(127, 127, 127)
end
end
})

-- 4. Time of Day (التحكم في الوقت)
EnvironmentTab:AddSlider("TimeSlider", {
Title = "Time of Day",
Description = "Change the game clock",
Default = 12,
Min = 0,
Max = 24,
Rounding = 1,
Callback = function(Value)
game:GetService("Lighting").ClockTime = Value
end
})
-------------------------------------------------
-- ELITE ACCESS SECTION CONTENT
-------------------------------------------------

-- 1. FPS Booster (زيادة الفريمات)
EliteTab:AddButton({
    Title = "Ultimate FPS Boost 🚀",
    Description = "Removes textures & effects for maximum FPS",
    Callback = function()
        -- تنظيف التأثيرات لزيادة الأداء
        local Lighting = game:GetService("Lighting")
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 9e9
        
        for _, v in pairs(game:GetDescendants()) do
            if v:IsA("Part") or v:IsA("MeshPart") then
                v.Material = Enum.Material.SmoothPlastic
                v.Reflectance = 0
            elseif v:IsA("Decal") or v:IsA("Texture") then
                v.Transparency = 1
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Enabled = false
            end
        end
        
        Fluent:Notify({ Title = "Elite", Content = "FPS Boosted successfully!", Duration = 3 })
    end
})

-- 2. Click Teleport (أداة انتقال احترافية)
EliteTab:AddButton({
    Title = "Elite TP Tool 📍",
    Description = "Get a tool to teleport anywhere you click",
    Callback = function()
        local mouse = Player:GetMouse()
        local tool = Instance.new("Tool")
        tool.Name = "Elite TP Tool"
        tool.RequiresHandle = false
        
        tool.Activated:Connect(function()
            local pos = mouse.Hit.p + Vector3.new(0, 3, 0)
            if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
                Player.Character.HumanoidRootPart.CFrame = CFrame.new(pos)
            end
        end)
        
        tool.Parent = Player.Backpack
        Fluent:Notify({ Title = "Success", Content = "TP Tool added to your backpack!", Duration = 3 })
    end
})

-- 3. Anti-Kick / Anti-Bypass (مضاد الطرد)
EliteTab:AddToggle("AntiKick", {
    Title = "Anti-Kick Protection 🛡️",
    Description = "Protects you from being kicked by the game",
    Default = false,
    Callback = function(Value)
        if Value then
            local mt = getrawmetatable(game)
            local old = mt.__namecall
            setreadonly(mt, false)
            
            mt.__namecall = newcclosure(function(self, ...)
                local method = getnamecallmethod()
                if method == "Kick" or method == "kick" then
                    Fluent:Notify({ Title = "Protection", Content = "The game tried to kick you, but I stopped it!", Duration = 5 })
                    return nil
                end
                return old(self, ...)
            end)
            
            setreadonly(mt, true)
            Fluent:Notify({ Title = "Elite", Content = "Anti-Kick Protection Enabled!", Duration = 3 })
        end
    end
})
-------------------------------------------------
-- INFORMATION TAB CONTENT
-------------------------------------------------

InfoTab:AddParagraph({
    Title = "👤 Player Information",
    Content = string.format(
        "• Display Name: %s\n• Username: %s\n• Account ID: %d\n• Account Age: %d Days\n• Creation Date: %s",
        DisplayName, Name, UserId, AccountAge, CreationDate
    )
})

-------------------------------------------------
-- MANAGER
-------------------------------------------------

SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

InterfaceManager:SetFolder("MyHub")
InterfaceManager:BuildInterfaceSection(SettingsTab)

-------------------------------------------------
-- FLOATING BUTTON
-------------------------------------------------

local OpenGui = Instance.new("ScreenGui")
OpenGui.Name = "OpenUI"
OpenGui.ResetOnSpawn = false
OpenGui.Parent = game:GetService("CoreGui")

local OpenBtn = Instance.new("TextButton")
OpenBtn.Size = UDim2.fromOffset(60, 60)
OpenBtn.Position = UDim2.new(0.02, 0, 0.5, 0)
OpenBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
OpenBtn.Text = "ABC"
OpenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenBtn.TextScaled = true
OpenBtn.Font = Enum.Font.SourceSansBold
OpenBtn.Parent = OpenGui

Instance.new("UICorner", OpenBtn).CornerRadius = UDim.new(0.25, 0)

OpenBtn.MouseButton1Click:Connect(function()
    if Window and Window.Minimize then
        Window:Minimize()
    end
end)

-------------------------------------------------
-- DRAG SYSTEM
-------------------------------------------------

local dragging = false
local dragInput, dragStart, startPos

OpenBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = OpenBtn.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

OpenBtn.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        OpenBtn.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

-------------------------------------------------
-- NOTIFY
-------------------------------------------------

Fluent:Notify({
    Title = "ABC Hub",
    Content = "Welcome " .. DisplayName .. "!",
    Duration = 4,
})

Window:SelectTab(MinaTab) -- جعل السكربت يفتح على قسم مينا مباشرة

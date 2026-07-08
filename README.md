-- ============================================================
-- 🏆 نظام التحقق الأسطوري (نسخة 100x) - Delta Executor
-- ============================================================

local player = game.Players.LocalPlayer
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")
local Lighting = game:GetService("Lighting")

-- ===== إعدادات متقدمة =====
local CONFIG = {
    CodeLength = 5,
    TimeLimit = 60,
    MaxAttempts = 1,              -- 1 = طرد فوري عند الخطأ
    AutoKick = true,
    RainbowSpeed = 0.05,
    ParticleEffects = true,
    SoundEffects = true,
    BackgroundAnimation = true,
    AntiCheat = true,
    LogToConsole = true,
}

-- ===== توليد رمز عشوائي =====
local function generateCode()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local code = ""
    for i = 1, CONFIG.CodeLength do
        local randIndex = math.random(1, #chars)
        code = code .. string.sub(chars, randIndex, randIndex)
    end
    return code
end

-- ===== نظام الصوت =====
local function createSounds()
    local sounds = {}
    
    -- صوت النجاح
    local successSound = Instance.new("Sound")
    successSound.SoundId = "rbxassetid://9120393127"
    successSound.Volume = 0.5
    successSound.Parent = game:GetService("CoreGui")
    
    -- صوت الخطأ
    local errorSound = Instance.new("Sound")
    errorSound.SoundId = "rbxassetid://9120391528"
    errorSound.Volume = 0.7
    errorSound.Parent = game:GetService("CoreGui")
    
    -- صوت المؤقت
    local tickSound = Instance.new("Sound")
    tickSound.SoundId = "rbxassetid://9120390012"
    tickSound.Volume = 0.3
    tickSound.Parent = game:GetService("CoreGui")
    
    return {success = successSound, error = errorSound, tick = tickSound}
end

local sounds = createSounds()

-- ===== إنشاء واجهة متطورة جداً =====
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "CaptchaGUI"
    screenGui.Parent = game:GetService("CoreGui")
    screenGui.ResetOnSpawn = false
    screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    -- ===== خلفية متحركة =====
    local background = Instance.new("Frame")
    background.Size = UDim2.new(1, 0, 1, 0)
    background.BackgroundColor3 = Color3.new(0, 0, 0)
    background.BackgroundTransparency = 0.7
    background.BorderSizePixel = 0
    background.Parent = screenGui
    
    -- تأثير خلفية متحركة (نجوم)
    if CONFIG.BackgroundAnimation then
        for i = 1, 50 do
            local star = Instance.new("Frame")
            star.Size = UDim2.new(0, math.random(1, 3), 0, math.random(1, 3))
            star.Position = UDim2.new(math.random(), 0, math.random(), 0)
            star.BackgroundColor3 = Color3.new(1, 1, 1)
            star.BackgroundTransparency = math.random(0, 5) / 10
            star.BorderSizePixel = 0
            star.Parent = background
            
            task.spawn(function()
                while star.Parent do
                    star.Position = UDim2.new(
                        star.Position.X.Scale,
                        star.Position.X.Offset + math.random(-2, 2),
                        star.Position.Y.Scale,
                        star.Position.Y.Offset + math.random(-2, 2)
                    )
                    task.wait(0.1)
                end
            end)
        end
    end
    
    -- ===== الإطار الرئيسي =====
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 500, 0, 400)
    frame.Position = UDim2.new(0.5, -250, 0.5, -200)
    frame.BackgroundColor3 = Color3.new(0.04, 0.04, 0.08)
    frame.BackgroundTransparency = 0.02
    frame.BorderSizePixel = 0
    frame.Parent = screenGui
    
    -- زوايا مستديرة
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 20)
    corner.Parent = frame
    
    -- ظل متوهج
    local glow = Instance.new("Frame")
    glow.Size = UDim2.new(1, 20, 1, 20)
    glow.Position = UDim2.new(0, -10, 0, -10)
    glow.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    glow.BackgroundTransparency = 0.8
    glow.BorderSizePixel = 0
    glow.Parent = frame
    
    local glowCorner = Instance.new("UICorner")
    glowCorner.CornerRadius = UDim.new(0, 25)
    glowCorner.Parent = glow
    
    -- ===== خط علوي متحرك =====
    local topLine = Instance.new("Frame")
    topLine.Size = UDim2.new(1, 0, 0, 5)
    topLine.Position = UDim2.new(0, 0, 0, 0)
    topLine.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    topLine.BorderSizePixel = 0
    topLine.Parent = frame
    
    -- ===== أيقونة =====
    local iconContainer = Instance.new("Frame")
    iconContainer.Size = UDim2.new(0, 80, 0, 80)
    iconContainer.Position = UDim2.new(0.5, -40, 0, 10)
    iconContainer.BackgroundColor3 = Color3.new(0.1, 0.1, 0.15)
    iconContainer.BackgroundTransparency = 0.5
    iconContainer.BorderSizePixel = 0
    iconContainer.Parent = frame
    
    local iconCorner = Instance.new("UICorner")
    iconCorner.CornerRadius = UDim.new(0, 40)
    iconCorner.Parent = iconContainer
    
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(1, 0, 1, 0)
    icon.Text = "🛡️"
    icon.TextColor3 = Color3.new(1, 1, 1)
    icon.TextScaled = true
    icon.BackgroundTransparency = 1
    icon.Font = Enum.Font.GothamBold
    icon.Parent = iconContainer
    
    -- ===== عنوان =====
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 100)
    title.Text = "🔐 التحقق الأمني"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold
    title.BackgroundTransparency = 1
    title.Parent = frame
    
    -- ===== نص فرعي =====
    local subtitle = Instance.new("TextLabel")
    subtitle.Size = UDim2.new(1, 0, 0, 20)
    subtitle.Position = UDim2.new(0, 0, 0, 135)
    subtitle.Text = "أدخل الرمز المكون من 5 أحرف لإثبات هويتك"
    subtitle.TextColor3 = Color3.new(0.5, 0.5, 0.5)
    subtitle.TextScaled = true
    subtitle.Font = Enum.Font.Gotham
    subtitle.BackgroundTransparency = 1
    subtitle.Parent = frame
    
    -- ===== مربع الرمز =====
    local codeBox = Instance.new("Frame")
    codeBox.Size = UDim2.new(0.75, 0, 0, 75)
    codeBox.Position = UDim2.new(0.125, 0, 0, 165)
    codeBox.BackgroundColor3 = Color3.new(0.08, 0.08, 0.12)
    codeBox.BackgroundTransparency = 0.5
    codeBox.BorderSizePixel = 0
    codeBox.Parent = frame
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 12)
    codeCorner.Parent = codeBox
    
    -- حدود متوهجة
    local codeGlow = Instance.new("Frame")
    codeGlow.Size = UDim2.new(1, 4, 1, 4)
    codeGlow.Position = UDim2.new(0, -2, 0, -2)
    codeGlow.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    codeGlow.BackgroundTransparency = 0.6
    codeGlow.BorderSizePixel = 0
    codeGlow.Parent = codeBox
    
    local codeGlowCorner = Instance.new("UICorner")
    codeGlowCorner.CornerRadius = UDim.new(0, 14)
    codeGlowCorner.Parent = codeGlow
    
    -- الرمز
    local codeLabel = Instance.new("TextLabel")
    codeLabel.Size = UDim2.new(1, 0, 1, 0)
    codeLabel.Text = ""
    codeLabel.TextColor3 = Color3.new(0.3, 0.8, 1)
    codeLabel.TextScaled = true
    codeLabel.Font = Enum.Font.GothamBold
    codeLabel.BackgroundTransparency = 1
    codeLabel.Parent = codeBox
    
    -- ===== حروف الرمز (تأثير فردي) =====
    local letterFrames = {}
    for i = 1, CONFIG.CodeLength do
        local letter = Instance.new("TextLabel")
        letter.Size = UDim2.new(0, 40, 0, 50)
        letter.Position = UDim2.new((i - 1) / CONFIG.CodeLength + 0.02, 0, 0.15, 0)
        letter.Text = ""
        letter.TextColor3 = Color3.new(1, 1, 1)
        letter.TextScaled = true
        letter.Font = Enum.Font.GothamBold
        letter.BackgroundTransparency = 1
        letter.Parent = codeBox
        letterFrames[i] = letter
    end
    
    -- ===== مربع الإدخال =====
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0.75, 0, 0, 45)
    textBox.Position = UDim2.new(0.125, 0, 0, 250)
    textBox.PlaceholderText = "✏️ اكتب الرمز هنا..."
    textBox.Text = ""
    textBox.TextColor3 = Color3.new(1, 1, 1)
    textBox.BackgroundColor3 = Color3.new(0.12, 0.12, 0.18)
    textBox.Font = Enum.Font.Gotham
    textBox.TextScaled = true
    textBox.ClearTextOnFocus = false
    textBox.Parent = frame
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 10)
    boxCorner.Parent = textBox
    
    -- ===== أزرار =====
    local buttonContainer = Instance.new("Frame")
    buttonContainer.Size = UDim2.new(0.8, 0, 0, 45)
    buttonContainer.Position = UDim2.new(0.1, 0, 0, 305)
    buttonContainer.BackgroundTransparency = 1
    buttonContainer.Parent = frame
    
    -- زر التحقق
    local submitBtn = Instance.new("TextButton")
    submitBtn.Size = UDim2.new(0.6, 0, 1, 0)
    submitBtn.Position = UDim2.new(0, 0, 0, 0)
    submitBtn.Text = "✅ تحقق"
    submitBtn.TextColor3 = Color3.new(1, 1, 1)
    submitBtn.BackgroundColor3 = Color3.new(0.15, 0.5, 1)
    submitBtn.Font = Enum.Font.GothamBold
    submitBtn.TextScaled = true
    submitBtn.Parent = buttonContainer
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 10)
    btnCorner.Parent = submitBtn
    
    -- زر إعادة تعيين
    local resetBtn = Instance.new("TextButton")
    resetBtn.Size = UDim2.new(0.25, 0, 1, 0)
    resetBtn.Position = UDim2.new(0.75, 0, 0, 0)
    resetBtn.Text = "🔄"
    resetBtn.TextColor3 = Color3.new(1, 1, 1)
    resetBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.3)
    resetBtn.Font = Enum.Font.GothamBold
    resetBtn.TextScaled = true
    resetBtn.Parent = buttonContainer
    
    local resetCorner = Instance.new("UICorner")
    resetCorner.CornerRadius = UDim.new(0, 10)
    resetCorner.Parent = resetBtn
    
    -- ===== شريط الوقت =====
    local timeBarBg = Instance.new("Frame")
    timeBarBg.Size = UDim2.new(0.8, 0, 0, 6)
    timeBarBg.Position = UDim2.new(0.1, 0, 0, 360)
    timeBarBg.BackgroundColor3 = Color3.new(0.2, 0.2, 0.3)
    timeBarBg.BorderSizePixel = 0
    timeBarBg.Parent = frame
    
    local timeBarCorner = Instance.new("UICorner")
    timeBarCorner.CornerRadius = UDim.new(0, 3)
    timeBarCorner.Parent = timeBarBg
    
    local timeBar = Instance.new("Frame")
    timeBar.Size = UDim2.new(1, 0, 1, 0)
    timeBar.BackgroundColor3 = Color3.new(0.2, 0.8, 0.4)
    timeBar.BorderSizePixel = 0
    timeBar.Parent = timeBarBg
    
    local timeBarCorner2 = Instance.new("UICorner")
    timeBarCorner2.CornerRadius = UDim.new(0, 3)
    timeBarCorner2.Parent = timeBar
    
    -- ===== رسالة النتيجة =====
    local resultLabel = Instance.new("TextLabel")
    resultLabel.Size = UDim2.new(1, 0, 0, 25)
    resultLabel.Position = UDim2.new(0, 0, 0, 370)
    resultLabel.Text = ""
    resultLabel.TextColor3 = Color3.new(1, 1, 1)
    resultLabel.TextScaled = true
    resultLabel.Font = Enum.Font.Gotham
    resultLabel.BackgroundTransparency = 1
    resultLabel.Parent = frame
    
    return {
        ScreenGui = screenGui,
        Frame = frame,
        Background = background,
        CodeLabel = codeLabel,
        CodeBox = codeBox,
        TextBox = textBox,
        SubmitBtn = submitBtn,
        ResetBtn = resetBtn,
        ResultLabel = resultLabel,
        TopLine = topLine,
        GlowBorder = glow,
        CodeGlow = codeGlow,
        TimeBar = timeBar,
        TimeBarBg = timeBarBg,
        LetterFrames = letterFrames,
        IconContainer = iconContainer,
        Icon = icon,
    }
end

-- ===== تأثيرات الجسيمات =====
local function createParticles(gui)
    if not CONFIG.ParticleEffects then return end
    
    local particles = {}
    for i = 1, 20 do
        local particle = Instance.new("Frame")
        particle.Size = UDim2.new(0, math.random(2, 4), 0, math.random(2, 4))
        particle.Position = UDim2.new(math.random(), 0, math.random(), 0)
        particle.BackgroundColor3 = Color3.new(
            math.random() / 2 + 0.5,
            math.random() / 2 + 0.5,
            1
        )
        particle.BackgroundTransparency = 0.7
        particle.BorderSizePixel = 0
        particle.Parent = gui.ScreenGui
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 2)
        corner.Parent = particle
        
        table.insert(particles, particle)
        
        task.spawn(function()
            while particle.Parent do
                local x = particle.Position.X.Scale + (math.random(-2, 2) / 100)
                local y = particle.Position.Y.Scale + (math.random(-2, 2) / 100)
                particle.Position = UDim2.new(
                    math.max(0, math.min(1, x)),
                    0,
                    math.max(0, math.min(1, y)),
                    0
                )
                particle.BackgroundTransparency = math.random(5, 8) / 10
                task.wait(0.05)
            end
        end)
    end
end

-- ===== تأثيرات إضافية =====
local function addEffects(gui)
    -- تأثير قوس قزح
    task.spawn(function()
        local hue = 0
        while gui.ScreenGui and gui.ScreenGui.Parent do
            hue = (hue + CONFIG.RainbowSpeed) % 1
            local color = Color3.fromHSV(hue, 1, 1)
            gui.CodeLabel.TextColor3 = color
            gui.CodeGlow.BackgroundColor3 = color
            gui.TopLine.BackgroundColor3 = color
            
            -- تحديث الحروف الفردية
            for i, letter in ipairs(gui.LetterFrames) do
                local letterHue = (hue + (i / CONFIG.CodeLength) * 0.3) % 1
                letter.TextColor3 = Color3.fromHSV(letterHue, 1, 1)
            end
            
            task.wait(0.03)
        end
    end)
    
    -- تأثير نبض للإطار
    task.spawn(function()
        while gui.ScreenGui and gui.ScreenGui.Parent do
            local pulse = 0.98 + math.sin(tick() * 3) * 0.02
            gui.Frame.Size = UDim2.new(0, 500 * pulse, 0, 400 * pulse)
            gui.Frame.Position = UDim2.new(0.5, -250 * pulse, 0.5, -200 * pulse)
            task.wait(0.02)
        end
    end)
end

-- ===== تأثير اهتزاز =====
local function shakeFrame(frame, intensity)
    local originalPos = frame.Position
    for i = 1, 15 do
        local offsetX = math.random(-intensity, intensity)
        local offsetY = math.random(-intensity, intensity)
        frame.Position = UDim2.new(
            originalPos.X.Scale,
            originalPos.X.Offset + offsetX,
            originalPos.Y.Scale,
            originalPos.Y.Offset + offsetY
        )
        task.wait(0.015)
    end
    frame.Position = originalPos
end

-- ===== تشغيل الصوت =====
local function playSound(sound)
    if not CONFIG.SoundEffects then return end
    pcall(function()
        sound:Play()
    end)
end

-- ===== إنشاء الواجهة =====
local gui = createGUI()
addEffects(gui)
createParticles(gui)

-- ===== توليد الرمز =====
local currentCode = generateCode()
gui.CodeLabel.Text = currentCode

-- تحديث الحروف الفردية
for i = 1, CONFIG.CodeLength do
    gui.LetterFrames[i].Text = string.sub(currentCode, i, i)
end

-- ===== متغيرات =====
local verified = false
local startTime = os.time()
local attempts = CONFIG.MaxAttempts

-- ===== تعطيل الحركة =====
local function disableMovement()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 0
        humanoid.JumpPower = 0
        humanoid.AutoRotate = false
        humanoid.PlatformStand = true
    end
    return humanoid
end

local humanoid = disableMovement()

-- ===== تفعيل الحركة =====
local function enableMovement()
    if humanoid then
        humanoid.WalkSpeed = 16
        humanoid.JumpPower = 50
        humanoid.AutoRotate = true
        humanoid.PlatformStand = false
    end
end

-- ===== نظام الطرد المتقدم =====
local function kickPlayer(reason)
    gui.ResultLabel.Text = "⛔ " .. reason
    gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
    gui.SubmitBtn.Visible = false
    gui.TextBox.Visible = false
    gui.ResetBtn.Visible = false
    
    -- تأثيرات الطرد
    gui.TopLine.BackgroundColor3 = Color3.new(1, 0, 0)
    gui.CodeGlow.BackgroundColor3 = Color3.new(1, 0, 0)
    gui.Icon.Text = "🚫"
    gui.Icon.TextColor3 = Color3.new(1, 0, 0)
    
    -- تغيير لون الخلفية
    gui.Background.BackgroundColor3 = Color3.new(0.5, 0, 0)
    gui.Background.BackgroundTransparency = 0.3
    
    playSound(sounds.error)
    shakeFrame(gui.Frame, 15)
    
    -- تأثير وميض
    for i = 1, 3 do
        gui.Frame.BackgroundTransparency = 0.5
        task.wait(0.1)
        gui.Frame.BackgroundTransparency = 0.02
        task.wait(0.1)
    end
    
    task.wait(1.5)
    
    if CONFIG.LogToConsole then
        print("❌ " .. reason .. " - تم طرد اللاعب: " .. player.Name)
        print("📝 الرمز المطلوب كان: " .. currentCode)
        print("✏️ الرمز المدخل كان: " .. gui.TextBox.Text)
    end
    
    player:Kick("🔐 " .. reason .. " | الرمز: " .. currentCode)
end

-- ===== نظام التحقق =====
local function verify()
    if verified then return end
    
    local userInput = gui.TextBox.Text
    userInput = string.upper(userInput)
    userInput = string.gsub(userInput, "%s+", "")
    
    -- التحقق من طول الإدخال
    if #userInput ~= CONFIG.CodeLength then
        gui.ResultLabel.Text = "⚠️ يجب أن يكون الرمز " .. CONFIG.CodeLength .. " أحرف"
        gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        shakeFrame(gui.Frame, 5)
        return
    end
    
    if userInput == currentCode then
        -- ✅ نجاح
        verified = true
        gui.ResultLabel.Text = "✅ تم التحقق بنجاح! مرحباً بك 🎉"
        gui.ResultLabel.TextColor3 = Color3.new(0, 1, 0)
        gui.TopLine.BackgroundColor3 = Color3.new(0, 1, 0)
        gui.CodeGlow.BackgroundColor3 = Color3.new(0, 1, 0)
        gui.SubmitBtn.BackgroundColor3 = Color3.new(0, 0.8, 0)
        gui.SubmitBtn.Text = "✅ تم"
        gui.Icon.Text = "✅"
        gui.Icon.TextColor3 = Color3.new(0, 1, 0)
        gui.TimeBar.BackgroundColor3 = Color3.new(0, 1, 0)
        
        playSound(sounds.success)
        enableMovement()
        
        if CONFIG.LogToConsole then
            print("✅ التحقق ناجح! اللاعب: " .. player.Name)
            print("📝 الرمز الصحيح: " .. currentCode)
        end
        
        task.wait(0.5)
        
        -- تأثير انفجار جسيمات
        for i = 1, 10 do
            local particle = Instance.new("Frame")
            particle.Size = UDim2.new(0, math.random(5, 15), 0, math.random(5, 15))
            particle.Position = UDim2.new(0.5, math.random(-50, 50), 0.5, math.random(-50, 50))
            particle.BackgroundColor3 = Color3.fromHSV(math.random(), 1, 1)
            particle.BackgroundTransparency = 0.3
            particle.BorderSizePixel = 0
            particle.Parent = gui.ScreenGui
            
            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 5)
            corner.Parent = particle
            
            task.spawn(function()
                for j = 1, 20 do
                    particle.Position = UDim2.new(
                        particle.Position.X.Scale + math.random(-3, 3) / 100,
                        particle.Position.X.Offset + math.random(-5, 5),
                        particle.Position.Y.Scale + math.random(-3, 3) / 100,
                        particle.Position.Y.Offset + math.random(-5, 5)
                    )
                    particle.BackgroundTransparency = particle.BackgroundTransparency + 0.03
                    particle.Size = UDim2.new(
                        0, particle.Size.X.Offset * 0.98,
                        0, particle.Size.Y.Offset * 0.98
                    )
                    task.wait(0.02)
                end
                particle:Destroy()
            end)
        end
        
        task.wait(1)
        gui.ScreenGui:Destroy()
        
    else
        -- ❌ خطأ
        attempts = attempts - 1
        
        if attempts <= 0 then
            kickPlayer("رمز تحقق خاطئ!")
        else
            gui.ResultLabel.Text = "❌ رمز خاطئ! متبقي " .. attempts .. " محاولات"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.5, 0)
            playSound(sounds.error)
            shakeFrame(gui.Frame, 8)
            
            -- تغيير الرمز
            currentCode = generateCode()
            gui.CodeLabel.Text = currentCode
            for i = 1, CONFIG.CodeLength do
                gui.LetterFrames[i].Text = string.sub(currentCode, i, i)
            end
            gui.TextBox.Text = ""
            
            if CONFIG.LogToConsole then
                print("🔄 تم تغيير الرمز إلى: " .. currentCode)
            end
        end
    end
end

-- ===== أحداث =====
gui.SubmitBtn.MouseButton1Click:Connect(verify)

gui.TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        verify()
    end
end)

-- ===== إعادة تعيين الرمز =====
gui.ResetBtn.MouseButton1Click:Connect(function()
    if not verified then
        currentCode = generateCode()
        gui.CodeLabel.Text = currentCode
        for i = 1, CONFIG.CodeLength do
            gui.LetterFrames[i].Text = string.sub(currentCode, i, i)
        end
        gui.TextBox.Text = ""
        gui.ResultLabel.Text = "🔄 تم تغيير الرمز!"
        gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        playSound(sounds.tick)
        task.wait(1)
        gui.ResultLabel.Text = ""
        
        if CONFIG.LogToConsole then
            print("🔄 تم تغيير الرمز إلى: " .. currentCode)
        end
    end
end)

-- ===== نظام الوقت المتقدم =====
task.spawn(function()
    while not verified do
        local elapsed = os.time() - startTime
        local timeLeft = CONFIG.TimeLimit - elapsed
        
        if timeLeft <= 0 then
            kickPlayer("انتهى وقت التحقق!")
            return
        end
        
        -- تحديث شريط الوقت
        local progress = timeLeft / CONFIG.TimeLimit
        gui.TimeBar.Size = UDim2.new(progress, 0, 1, 0)
        
        -- تغيير لون شريط الوقت
        if progress > 0.5 then
            gui.TimeBar.BackgroundColor3 = Color3.new(0.2, 0.8, 0.4)
        elseif progress > 0.25 then
            gui.TimeBar.BackgroundColor3 = Color3.new(0.8, 0.8, 0.2)
        else
            gui.TimeBar.BackgroundColor3 = Color3.new(0.8, 0.2, 0.2)
        end
        
        -- تحديث رسالة الوقت
        if timeLeft <= 10 then
            gui.ResultLabel.Text = "⏰ " .. timeLeft .. " ثانية متبقية!"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.5, 0)
            if timeLeft <= 5 then
                playSound(sounds.tick)
            end
        end
        
        task.wait(0.1)
    end
end)

-- ===== حماية متقدمة =====
if CONFIG.AntiCheat then
    task.spawn(function()
        while not verified do
            -- منع تغيير السرعة
            if humanoid then
                if humanoid.WalkSpeed > 0 then
                    humanoid.WalkSpeed = 0
                end
                if humanoid.JumpPower > 0 then
                    humanoid.JumpPower = 0
                end
            end
            
            -- منع فتح واجهات أخرى
            for _, guiItem in pairs(game:GetService("CoreGui"):GetChildren()) do
                if guiItem.Name ~= "CaptchaGUI" and guiItem:IsA("ScreenGui") then
                    guiItem.Enabled = false
                end
            end
            
            -- منع تغيير الرمز من الخارج
            if gui.CodeLabel.Text ~= currentCode then
                gui.CodeLabel.Text = currentCode
            end
            
            task.wait(0.5)
        end
    end)
end

-- ===== اختصار لوحة المفاتيح (Ctrl+R لإعادة تعيين) =====
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.R and input:IsModifierKeyDown(Enum.KeyCode.LeftControl) then
        if not verified then
            gui.ResetBtn:Click()
        end
    end
end)

-- ===== إشعار التشغيل =====
print("")
print("🏆 " .. string.rep("=", 50))
print("🏆 نظام التحقق الأسطوري (نسخة 100x) يعمل!")
print("🏆 " .. string.rep("=", 50))
print("📝 الرمز الحالي: " .. currentCode)
print("⏱️ لديك " .. CONFIG.TimeLimit .. " ثانية")
print("⚠️ أي خطأ = طرد فوري!")
print("🔄 اضغط على الزر الدائري أو Ctrl+R لتغيير الرمز")
print("🎨 تأثيرات بصرية وصوتية متطورة")
print("🏆 " .. string.rep("=", 50))
print("")

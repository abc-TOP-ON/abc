-- =====================================================
-- نظام التحقق المتطور (نسخة 10x) - Delta Executor
-- =====================================================

local player = game.Players.LocalPlayer
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

-- ===== إعدادات =====
local CONFIG = {
    CodeLength = 5,              -- طول الرمز
    TimeLimit = 60,              -- الوقت بالثواني
    AutoKick = true,             -- طرد تلقائي عند الخطأ
    EnableSounds = true,         -- تشغيل أصوات (اختياري)
    RainbowEffect = true,        -- تأثير قوس قزح على الرمز
    ShakeOnError = true,         -- اهتزاز عند الخطأ
    HideUIOnSuccess = true,      -- إخفاء الواجهة عند النجاح
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

-- ===== إنشاء واجهة متطورة =====
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "CaptchaGUI"
    screenGui.Parent = game:GetService("CoreGui")
    screenGui.ResetOnSpawn = false
    
    -- خلفية مظللة
    local background = Instance.new("Frame")
    background.Size = UDim2.new(1, 0, 1, 0)
    background.BackgroundColor3 = Color3.new(0, 0, 0)
    background.BackgroundTransparency = 0.6
    background.BorderSizePixel = 0
    background.Parent = screenGui
    
    -- الإطار الرئيسي
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 450, 0, 320)
    frame.Position = UDim2.new(0.5, -225, 0.5, -160)
    frame.BackgroundColor3 = Color3.new(0.06, 0.06, 0.1)
    frame.BackgroundTransparency = 0.05
    frame.BorderSizePixel = 0
    frame.Parent = screenGui
    
    -- ظل للإطار
    local shadow = Instance.new("Frame")
    shadow.Size = UDim2.new(1, 4, 1, 4)
    shadow.Position = UDim2.new(0, -2, 0, -2)
    shadow.BackgroundColor3 = Color3.new(0, 0, 0)
    shadow.BackgroundTransparency = 0.3
    shadow.BorderSizePixel = 0
    shadow.Parent = frame
    
    -- زوايا مستديرة
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 16)
    corner.Parent = frame
    
    -- خط علوي متوهج
    local topLine = Instance.new("Frame")
    topLine.Size = UDim2.new(1, 0, 0, 4)
    topLine.Position = UDim2.new(0, 0, 0, 0)
    topLine.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    topLine.BorderSizePixel = 0
    topLine.Parent = frame
    
    -- أيقونة القفل
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 50, 0, 50)
    icon.Position = UDim2.new(0.5, -25, 0, 10)
    icon.Text = "🔒"
    icon.TextColor3 = Color3.new(1, 1, 1)
    icon.TextScaled = true
    icon.BackgroundTransparency = 1
    icon.Font = Enum.Font.GothamBold
    icon.Parent = frame
    
    -- عنوان
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 0, 0, 65)
    title.Text = "🔐 تحقق من هويتك"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold
    title.BackgroundTransparency = 1
    title.Parent = frame
    
    -- نص فرعي
    local subtitle = Instance.new("TextLabel")
    subtitle.Size = UDim2.new(1, 0, 0, 20)
    subtitle.Position = UDim2.new(0, 0, 0, 95)
    subtitle.Text = "أدخل الرمز الموضح أدناه لإثبات أنك لست روبوتاً"
    subtitle.TextColor3 = Color3.new(0.5, 0.5, 0.5)
    subtitle.TextScaled = true
    subtitle.Font = Enum.Font.Gotham
    subtitle.BackgroundTransparency = 1
    subtitle.Parent = frame
    
    -- مربع الرمز
    local codeBox = Instance.new("Frame")
    codeBox.Size = UDim2.new(0.7, 0, 0, 65)
    codeBox.Position = UDim2.new(0.15, 0, 0, 125)
    codeBox.BackgroundColor3 = Color3.new(0.12, 0.12, 0.18)
    codeBox.BorderSizePixel = 0
    codeBox.Parent = frame
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 10)
    codeCorner.Parent = codeBox
    
    -- حدود متوهجة لمربع الرمز
    local glowBorder = Instance.new("Frame")
    glowBorder.Size = UDim2.new(1, 4, 1, 4)
    glowBorder.Position = UDim2.new(0, -2, 0, -2)
    glowBorder.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    glowBorder.BackgroundTransparency = 0.7
    glowBorder.BorderSizePixel = 0
    glowBorder.Parent = codeBox
    
    local glowCorner = Instance.new("UICorner")
    glowCorner.CornerRadius = UDim.new(0, 12)
    glowCorner.Parent = glowBorder
    
    -- الرمز (مع تأثير)
    local codeLabel = Instance.new("TextLabel")
    codeLabel.Size = UDim2.new(1, 0, 1, 0)
    codeLabel.Text = ""
    codeLabel.TextColor3 = Color3.new(0.3, 0.8, 1)
    codeLabel.TextScaled = true
    codeLabel.Font = Enum.Font.GothamBold
    codeLabel.BackgroundTransparency = 1
    codeLabel.Parent = codeBox
    
    -- مؤشر كتابة
    local cursor = Instance.new("TextLabel")
    cursor.Size = UDim2.new(0, 20, 0, 40)
    cursor.Position = UDim2.new(1, 5, 0.5, -20)
    cursor.Text = "|"
    cursor.TextColor3 = Color3.new(0.3, 0.8, 1)
    cursor.TextScaled = true
    cursor.Font = Enum.Font.Gotham
    cursor.BackgroundTransparency = 1
    cursor.Visible = false
    cursor.Parent = codeBox
    
    -- مربع الإدخال
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0.7, 0, 0, 40)
    textBox.Position = UDim2.new(0.15, 0, 0, 200)
    textBox.PlaceholderText = "✏️ اكتب الرمز هنا..."
    textBox.Text = ""
    textBox.TextColor3 = Color3.new(1, 1, 1)
    textBox.BackgroundColor3 = Color3.new(0.15, 0.15, 0.2)
    textBox.Font = Enum.Font.Gotham
    textBox.TextScaled = true
    textBox.ClearTextOnFocus = false
    textBox.Parent = frame
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 8)
    boxCorner.Parent = textBox
    
    -- زر التحقق (مع تأثير hover)
    local submitBtn = Instance.new("TextButton")
    submitBtn.Size = UDim2.new(0.35, 0, 0, 45)
    submitBtn.Position = UDim2.new(0.325, 0, 0, 250)
    submitBtn.Text = "✅ تحقق"
    submitBtn.TextColor3 = Color3.new(1, 1, 1)
    submitBtn.BackgroundColor3 = Color3.new(0.15, 0.5, 1)
    submitBtn.Font = Enum.Font.GothamBold
    submitBtn.TextScaled = true
    submitBtn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = submitBtn
    
    -- زر إعادة تعيين
    local resetBtn = Instance.new("TextButton")
    resetBtn.Size = UDim2.new(0.15, 0, 0, 30)
    resetBtn.Position = UDim2.new(0.8, 0, 0, 130)
    resetBtn.Text = "🔄"
    resetBtn.TextColor3 = Color3.new(1, 1, 1)
    resetBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.3)
    resetBtn.Font = Enum.Font.GothamBold
    resetBtn.TextScaled = true
    resetBtn.Parent = frame
    
    local resetCorner = Instance.new("UICorner")
    resetCorner.CornerRadius = UDim.new(0, 8)
    resetCorner.Parent = resetBtn
    
    -- رسالة النتيجة
    local resultLabel = Instance.new("TextLabel")
    resultLabel.Size = UDim2.new(1, 0, 0, 30)
    resultLabel.Position = UDim2.new(0, 0, 0, 285)
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
        Cursor = cursor,
        GlowBorder = glowBorder
    }
end

-- ===== تأثيرات إضافية =====
local function addEffects(gui)
    -- تأثير قوس قزح على الرمز
    if CONFIG.RainbowEffect then
        task.spawn(function()
            while gui.ScreenGui and gui.ScreenGui.Parent do
                local hue = tick() % 2 / 2
                local color = Color3.fromHSV(hue, 1, 1)
                gui.CodeLabel.TextColor3 = color
                gui.GlowBorder.BackgroundColor3 = color
                task.wait(0.05)
            end
        end)
    end
    
    -- تأثير وميض المؤشر
    task.spawn(function()
        while gui.ScreenGui and gui.ScreenGui.Parent do
            gui.Cursor.Visible = not gui.Cursor.Visible
            task.wait(0.5)
        end
    end)
end

-- ===== تأثير اهتزاز =====
local function shakeFrame(frame)
    if not CONFIG.ShakeOnError then return end
    
    local originalPos = frame.Position
    for i = 1, 10 do
        local offsetX = math.random(-8, 8)
        local offsetY = math.random(-8, 8)
        frame.Position = UDim2.new(
            originalPos.X.Scale,
            originalPos.X.Offset + offsetX,
            originalPos.Y.Scale,
            originalPos.Y.Offset + offsetY
        )
        task.wait(0.02)
    end
    frame.Position = originalPos
end

-- ===== إنشاء الواجهة =====
local gui = createGUI()
addEffects(gui)

-- ===== توليد الرمز =====
local currentCode = generateCode()
gui.CodeLabel.Text = currentCode

-- ===== متغيرات =====
local verified = false
local startTime = os.time()

-- ===== تعطيل الحركة =====
local function disableMovement()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 0
        humanoid.JumpPower = 0
        humanoid.AutoRotate = false
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
    end
end

-- ===== وظيفة الطرد مع تأثير =====
local function kickPlayer(reason)
    gui.ResultLabel.Text = "⛔ " .. reason
    gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
    gui.SubmitBtn.Visible = false
    gui.TextBox.Visible = false
    gui.ResetBtn.Visible = false
    
    -- تغيير لون الخط العلوي للأحمر
    gui.TopLine.BackgroundColor3 = Color3.new(1, 0, 0)
    
    shakeFrame(gui.Frame)
    
    task.wait(1.5)
    print("❌ " .. reason .. " - تم طرد اللاعب: " .. player.Name)
    player:Kick(reason)
end

-- ===== وظيفة التحقق =====
local function verify()
    if verified then return end
    
    local userInput = gui.TextBox.Text
    userInput = string.upper(userInput)
    userInput = string.gsub(userInput, "%s+", "")  -- إزالة المسافات
    
    if userInput == currentCode then
        -- ✅ نجاح
        verified = true
        gui.ResultLabel.Text = "✅ تم التحقق بنجاح! مرحباً بك 🎉"
        gui.ResultLabel.TextColor3 = Color3.new(0, 1, 0)
        gui.TopLine.BackgroundColor3 = Color3.new(0, 1, 0)
        gui.SubmitBtn.BackgroundColor3 = Color3.new(0, 0.8, 0)
        gui.SubmitBtn.Text = "✅ تم"
        
        enableMovement()
        
        print("✅ التحقق ناجح! اللاعب: " .. player.Name)
        print("📝 الرمز الصحيح: " .. currentCode)
        
        if CONFIG.HideUIOnSuccess then
            task.wait(0.5)
            gui.ScreenGui:Destroy()
        end
        
    else
        -- ❌ خطأ - طرد فوري
        kickPlayer("رمز تحقق خاطئ!")
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
        gui.TextBox.Text = ""
        gui.ResultLabel.Text = "🔄 تم تغيير الرمز!"
        gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        print("🔄 تم تغيير الرمز إلى: " .. currentCode)
        task.wait(1)
        gui.ResultLabel.Text = ""
    end
end)

-- ===== نظام الوقت =====
task.spawn(function()
    while not verified do
        local elapsed = os.time() - startTime
        local timeLeft = CONFIG.TimeLimit - elapsed
        
        if timeLeft <= 0 then
            kickPlayer("انتهى وقت التحقق!")
            return
        end
        
        -- تحديث رسالة الوقت كل 5 ثواني
        if timeLeft % 5 == 0 or timeLeft <= 10 then
            gui.ResultLabel.Text = "⏰ متبقي " .. timeLeft .. " ثانية"
            gui.ResultLabel.TextColor3 = timeLeft <= 10 and 
                Color3.new(1, 0.5, 0) or 
                Color3.new(0.5, 0.8, 1)
        end
        
        task.wait(1)
    end
end)

-- ===== مراقبة محاولات الغش =====
task.spawn(function()
    local function checkExploits()
        while not verified do
            -- كشف محاولات تغيير السرعة
            if humanoid and humanoid.WalkSpeed > 0 then
                humanoid.WalkSpeed = 0
            end
            
            -- كشف محاولات فتح الواجهات الأخرى
            for _, gui in pairs(game:GetService("CoreGui"):GetChildren()) do
                if gui.Name ~= "CaptchaGUI" and gui:IsA("ScreenGui") then
                    gui.Enabled = false
                end
            end
            
            task.wait(1)
        end
    end
    task.spawn(checkExploits)
end)

-- ===== إشعار التشغيل =====
print("🔥 نظام التحقق المتطور (نسخة 10x) يعمل!")
print("📝 الرمز الحالي: " .. currentCode)
print("⏱️ لديك " .. CONFIG.TimeLimit .. " ثانية")
print("⚠️ أي خطأ = طرد فوري!")
print("🔄 اضغط على الزر الدائري لتغيير الرمز")

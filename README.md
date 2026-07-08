-- ================================================
-- نظام التحقق من أنك لست روبوت (Captcha)
-- رمز عشوائي 5 أحرف إنجليزية وأرقام
-- ================================================

local player = game.Players.LocalPlayer
local Players = game:GetService("Players")

-- ===== دالة توليد رمز عشوائي 5 أحرف =====
local function generateCode()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local code = ""
    for i = 1, 5 do  -- 5 أحرف فقط
        local randIndex = math.random(1, #chars)
        code = code .. string.sub(chars, randIndex, randIndex)
    end
    return code
end

-- ===== إنشاء واجهة المستخدم =====
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "CaptchaGUI"
    screenGui.Parent = game:GetService("CoreGui")
    
    -- الإطار الرئيسي
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 420, 0, 280)
    frame.Position = UDim2.new(0.5, -210, 0.5, -140)
    frame.BackgroundColor3 = Color3.new(0.08, 0.08, 0.12)
    frame.BackgroundTransparency = 0.1
    frame.BorderSizePixel = 0
    frame.Parent = screenGui
    
    -- زوايا مستديرة
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame
    
    -- خط فاصل علوي (تصميم)
    local topLine = Instance.new("Frame")
    topLine.Size = UDim2.new(1, 0, 0, 4)
    topLine.Position = UDim2.new(0, 0, 0, 0)
    topLine.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    topLine.BorderSizePixel = 0
    topLine.Parent = frame
    
    -- عنوان
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 40)
    title.Position = UDim2.new(0, 0, 0, 15)
    title.Text = "🤖 تحقق من أنك لست روبوتاً"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold
    title.BackgroundTransparency = 1
    title.Parent = frame
    
    -- مربع الرمز (يظهر الرمز المطلوب)
    local codeBox = Instance.new("Frame")
    codeBox.Size = UDim2.new(0.8, 0, 0, 60)
    codeBox.Position = UDim2.new(0.1, 0, 0, 65)
    codeBox.BackgroundColor3 = Color3.new(0.15, 0.15, 0.2)
    codeBox.BorderSizePixel = 0
    codeBox.Parent = frame
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 8)
    codeCorner.Parent = codeBox
    
    -- النص داخل مربع الرمز
    local codeLabel = Instance.new("TextLabel")
    codeLabel.Size = UDim2.new(1, 0, 1, 0)
    codeLabel.Position = UDim2.new(0, 0, 0, 0)
    codeLabel.Text = "P2KSJ"  -- سيتم تحديثه لاحقاً
    codeLabel.TextColor3 = Color3.new(0.3, 0.8, 1)
    codeLabel.TextScaled = true
    codeLabel.Font = Enum.Font.GothamBold
    codeLabel.BackgroundTransparency = 1
    codeLabel.Parent = codeBox
    
    -- تسمية "أدخل الرمز"
    local enterLabel = Instance.new("TextLabel")
    enterLabel.Size = UDim2.new(1, 0, 0, 25)
    enterLabel.Position = UDim2.new(0, 0, 0, 135)
    enterLabel.Text = "✏️ أدخل الرمز في الأسفل:"
    enterLabel.TextColor3 = Color3.new(0.7, 0.7, 0.7)
    enterLabel.TextScaled = true
    enterLabel.Font = Enum.Font.Gotham
    enterLabel.BackgroundTransparency = 1
    enterLabel.Parent = frame
    
    -- مربع إدخال النص
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0.8, 0, 0, 45)
    textBox.Position = UDim2.new(0.1, 0, 0, 165)
    textBox.PlaceholderText = "أدخل الرمز هنا..."
    textBox.Text = ""
    textBox.TextColor3 = Color3.new(1, 1, 1)
    textBox.BackgroundColor3 = Color3.new(0.2, 0.2, 0.25)
    textBox.Font = Enum.Font.Gotham
    textBox.TextScaled = true
    textBox.Parent = frame
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 8)
    boxCorner.Parent = textBox
    
    -- زر التحقق
    local submitBtn = Instance.new("TextButton")
    submitBtn.Size = UDim2.new(0.4, 0, 0, 45)
    submitBtn.Position = UDim2.new(0.3, 0, 0, 220)
    submitBtn.Text = "✅ تحقق"
    submitBtn.TextColor3 = Color3.new(1, 1, 1)
    submitBtn.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    submitBtn.Font = Enum.Font.GothamBold
    submitBtn.TextScaled = true
    submitBtn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = submitBtn
    
    -- رسالة النتيجة (تظهر نجاح أو فشل)
    local resultLabel = Instance.new("TextLabel")
    resultLabel.Size = UDim2.new(1, 0, 0, 30)
    resultLabel.Position = UDim2.new(0, 0, 0, 245)
    resultLabel.Text = ""
    resultLabel.TextColor3 = Color3.new(1, 1, 1)
    resultLabel.TextScaled = true
    resultLabel.Font = Enum.Font.Gotham
    resultLabel.BackgroundTransparency = 1
    resultLabel.Parent = frame
    
    -- إرجاع العناصر
    return {
        ScreenGui = screenGui,
        Frame = frame,
        CodeLabel = codeLabel,
        TextBox = textBox,
        SubmitBtn = submitBtn,
        ResultLabel = resultLabel
    }
end

-- ===== إنشاء الواجهة =====
local gui = createGUI()

-- ===== توليد أول رمز =====
local currentCode = generateCode()
gui.CodeLabel.Text = currentCode  -- يعرض الرمز مثل P2KSJ

-- ===== متغيرات =====
local attempts = 3
local verified = false

-- ===== تعطيل حركة اللاعب =====
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChild("Humanoid")

if humanoid then
    humanoid.WalkSpeed = 0
    humanoid.JumpPower = 0
    print("🔒 تم تعطيل الحركة")
end

-- ===== دالة إعادة تفعيل الحركة =====
local function enableMovement()
    if humanoid then
        humanoid.WalkSpeed = 16
        humanoid.JumpPower = 50
        print("🔓 تم تفعيل الحركة")
    end
end

-- ===== دالة التحقق =====
local function verify()
    local userInput = gui.TextBox.Text
    userInput = string.upper(userInput)  -- تحويل إلى حروف كبيرة
    
    if userInput == currentCode then
        -- ✅ نجاح
        verified = true
        gui.ResultLabel.Text = "✅ تم التحقق بنجاح! مرحباً بك!"
        gui.ResultLabel.TextColor3 = Color3.new(0, 1, 0)
        
        enableMovement()
        
        task.wait(1)
        gui.ScreenGui:Destroy()
        
        print("✅ تم التحقق من أنك لست روبوتاً!")
        print("🎮 الرمز الصحيح كان: " .. currentCode)
        
        -- هنا تقدر تشغل السكربتات الثانية بعد التحقق
        
    else
        -- ❌ فشل
        attempts = attempts - 1
        
        if attempts > 0 then
            gui.ResultLabel.Text = "❌ رمز خاطئ! متبقي " .. attempts .. " محاولات"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.5, 0)
            
            -- تغيير الرمز تلقائياً
            currentCode = generateCode()
            gui.CodeLabel.Text = currentCode
            gui.TextBox.Text = ""
            
            print("🔄 تم تغيير الرمز إلى: " .. currentCode)
            
        else
            -- 🔥 انتهت المحاولات - طرد
            gui.ResultLabel.Text = "⛔ انتهت المحاولات! يتم طردك..."
            gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
            gui.SubmitBtn.Visible = false
            gui.TextBox.Visible = false
            
            task.wait(2)
            print("❌ تم طرد اللاعب بسبب فشل التحقق")
            player:Kick("❌ فشل التحقق من أنك لست روبوتاً!")
        end
    end
end

-- ===== أحداث الأزرار =====
gui.SubmitBtn.MouseButton1Click:Connect(verify)

-- الضغط على Enter
gui.TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        verify()
    end
end)

-- ===== اختياري: تحديث الرمز كل 10 ثواني =====
task.spawn(function()
    while not verified do
        task.wait(10)
        if not verified then
            currentCode = generateCode()
            gui.CodeLabel.Text = currentCode
            print("🔄 تم تحديث الرمز إلى: " .. currentCode)
            
            -- إظهار إشعار بالتحديث
            gui.ResultLabel.Text = "🔄 تم تغيير الرمز!"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
            task.wait(1)
            gui.ResultLabel.Text = ""
        end
    end
end)

-- ===== مؤقت 60 ثانية =====
local timeLeft = 60
task.spawn(function()
    while timeLeft > 0 and not verified do
        task.wait(1)
        timeLeft = timeLeft - 1
        
        if timeLeft <= 10 then
            gui.ResultLabel.Text = "⏰ متبقي " .. timeLeft .. " ثانية!"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        end
        
        if timeLeft <= 0 then
            print("⏰ انتهى الوقت!")
            player:Kick("⏰ انتهى وقت التحقق!")
        end
    end
end)

-- ===== إشعار بدء التشغيل =====
print("🔐 نظام التحقق (Captcha) يعمل!")
print("📝 الرمز الحالي: " .. currentCode)
print("⏱️ لديك 60 ثانية و 3 محاولات")

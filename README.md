--[[
    سكربت التحقق من أن اللاعب ليس روبوت
    يدعم: تحدي اختيار الأرقام، تحدي الكتابة، تحدي النقر
--]]

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ========== إنشاء واجهة التحقق ==========
local function createVerificationUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "VerificationGUI"
    screenGui.Parent = playerGui
    screenGui.ResetOnSpawn = false

    -- الخلفية المظللة
    local overlay = Instance.new("Frame")
    overlay.Name = "Overlay"
    overlay.Parent = screenGui
    overlay.Size = UDim2.new(1, 0, 1, 0)
    overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    overlay.BackgroundTransparency = 0.6
    overlay.BorderSizePixel = 0

    -- نافذة التحقق
    local frame = Instance.new("Frame")
    frame.Name = "MainFrame"
    frame.Parent = overlay
    frame.Size = UDim2.new(0, 400, 0, 350)
    frame.Position = UDim2.new(0.5, -200, 0.5, -175)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    frame.BorderSizePixel = 0
    frame.ClipsDescendants = true

    -- زوايا دائرية
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame

    -- العنوان
    local title = Instance.new("TextLabel")
    title.Parent = frame
    title.Size = UDim2.new(1, 0, 0, 50)
    title.Position = UDim2.new(0, 0, 0, 0)
    title.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
    title.Text = "🔐 تحقق بشري"
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold

    -- أيقونة القفل
    local icon = Instance.new("TextLabel")
    icon.Parent = frame
    icon.Size = UDim2.new(0, 60, 0, 60)
    icon.Position = UDim2.new(0.5, -30, 0, 60)
    icon.BackgroundTransparency = 1
    icon.Text = "🤖"
    icon.TextSize = 40
    icon.TextColor3 = Color3.fromRGB(255, 200, 0)

    -- نص التعليمات
    local instruction = Instance.new("TextLabel")
    instruction.Parent = frame
    instruction.Size = UDim2.new(0.9, 0, 0, 50)
    instruction.Position = UDim2.new(0.05, 0, 0, 120)
    instruction.BackgroundTransparency = 1
    instruction.Text = "إثبت أنك لست روبوتاً\nاختر الرقم الصحيح"
    instruction.TextColor3 = Color3.fromRGB(200, 200, 200)
    instruction.TextScaled = true
    instruction.TextWrapped = true
    instruction.TextXAlignment = Enum.TextXAlignment.Center

    return screenGui, frame, instruction
end

-- ========== الطريقة الأولى: تحدي اختيار الرقم ==========
local function numberChallenge()
    local screenGui, frame, instruction = createVerificationUI()
    
    -- إنشاء الأرقام العشوائية
    local correctNumber = math.random(1, 9)
    local numbers = {}
    local usedNumbers = {}
    
    -- توليد 4 أرقام عشوائية مختلفة
    for i = 1, 4 do
        local num
        repeat
            num = math.random(1, 9)
        until not table.find(usedNumbers, num)
        table.insert(usedNumbers, num)
        table.insert(numbers, num)
    end
    
    -- التأكد من وجود الرقم الصحيح
    if not table.find(numbers, correctNumber) then
        numbers[math.random(1, #numbers)] = correctNumber
    end
    
    -- خلط الأرقام
    for i = #numbers, 2, -1 do
        local j = math.random(1, i)
        numbers[i], numbers[j] = numbers[j], numbers[i]
    end
    
    -- عرض التعليمات مع الرقم الصحيح
    instruction.Text = "🔢 اختر الرقم: " .. correctNumber
    
    -- إنشاء أزرار الأرقام
    local buttonSize = 60
    local spacing = 20
    local startX = (400 - (buttonSize * 4 + spacing * 3)) / 2
    
    for i, num in ipairs(numbers) do
        local btn = Instance.new("TextButton")
        btn.Parent = frame
        btn.Size = UDim2.new(0, buttonSize, 0, buttonSize)
        btn.Position = UDim2.new(0, startX + (i - 1) * (buttonSize + spacing), 0, 200)
        btn.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
        btn.Text = tostring(num)
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.TextSize = 28
        btn.Font = Enum.Font.GothamBold
        
        local cornerBtn = Instance.new("UICorner")
        cornerBtn.CornerRadius = UDim.new(0, 8)
        cornerBtn.Parent = btn
        
        btn.MouseButton1Click:Connect(function()
            local selected = tonumber(btn.Text)
            if selected == correctNumber then
                -- ✅ نجاح
                btn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
                instruction.Text = "✅ تم التحقق بنجاح!"
                instruction.TextColor3 = Color3.fromRGB(0, 255, 0)
                
                -- إخفاء الواجهة بعد ثانية
                task.wait(0.8)
                screenGui:Destroy()
                
                -- تشغيل إشعار نجاح
                print("✅ تم التحقق البشري بنجاح!")
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "✅ تحقق ناجح",
                    Text = "تم التأكد من أنك لست روبوتاً!",
                    Duration = 3
                })
            else
                -- ❌ فشل
                btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
                instruction.Text = "❌ رقم خاطئ! حاول مجدداً"
                instruction.TextColor3 = Color3.fromRGB(255, 0, 0)
                
                -- إعادة تعيين الزر بعد ثانية
                task.wait(0.5)
                btn.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
                instruction.Text = "🔢 اختر الرقم: " .. correctNumber
                instruction.TextColor3 = Color3.fromRGB(200, 200, 200)
            end
        end)
    end
    
    -- زر إعادة المحاولة
    local retryBtn = Instance.new("TextButton")
    retryBtn.Parent = frame
    retryBtn.Size = UDim2.new(0.6, 0, 0, 40)
    retryBtn.Position = UDim2.new(0.2, 0, 1, -55)
    retryBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 100)
    retryBtn.Text = "🔄 إعادة المحاولة"
    retryBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    retryBtn.TextScaled = true
    
    local cornerRetry = Instance.new("UICorner")
    cornerRetry.CornerRadius = UDim.new(0, 8)
    cornerRetry.Parent = retryBtn
    
    retryBtn.MouseButton1Click:Connect(function()
        frame:Destroy()
        screenGui:Destroy()
        numberChallenge() -- إعادة تشغيل التحدي
    end)
end

-- ========== الطريقة الثانية: تحدي الكتابة (Captcha) ==========
local function textChallenge()
    local screenGui, frame, instruction = createVerificationUI()
    
    -- كلمات عشوائية للتحقق
    local words = {"سلام", "روبوت", "إنسان", "تحقق", "أمان", "مستخدم", "مرحبا", "عالم"}
    local selectedWord = words[math.random(1, #words)]
    
    instruction.Text = "✍️ اكتب الكلمة التالية:\n" .. selectedWord
    
    -- حقل الإدخال
    local textBox = Instance.new("TextBox")
    textBox.Parent = frame
    textBox.Size = UDim2.new(0.6, 0, 0, 50)
    textBox.Position = UDim2.new(0.2, 0, 0, 200)
    textBox.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
    textBox.Text = ""
    textBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    textBox.TextSize = 24
    textBox.PlaceholderText = "اكتب هنا..."
    textBox.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
    textBox.Font = Enum.Font.Gotham
    textBox.ClearTextOnFocus = false
    
    local cornerBox = Instance.new("UICorner")
    cornerBox.CornerRadius = UDim.new(0, 8)
    cornerBox.Parent = textBox
    
    -- زر التأكيد
    local confirmBtn = Instance.new("TextButton")
    confirmBtn.Parent = frame
    confirmBtn.Size = UDim2.new(0.4, 0, 0, 40)
    confirmBtn.Position = UDim2.new(0.3, 0, 0, 270)
    confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
    confirmBtn.Text = "✅ تأكيد"
    confirmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    confirmBtn.TextScaled = true
    
    local cornerConfirm = Instance.new("UICorner")
    cornerConfirm.CornerRadius = UDim.new(0, 8)
    cornerConfirm.Parent = confirmBtn
    
    confirmBtn.MouseButton1Click:Connect(function()
        if textBox.Text == selectedWord then
            instruction.Text = "✅ تم التحقق بنجاح!"
            instruction.TextColor3 = Color3.fromRGB(0, 255, 0)
            textBox.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
            task.wait(0.8)
            screenGui:Destroy()
            
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "✅ تحقق ناجح",
                Text = "تم التأكد من أنك لست روبوتاً!",
                Duration = 3
            })
        else
            instruction.Text = "❌ كلمة خاطئة! حاول مجدداً"
            instruction.TextColor3 = Color3.fromRGB(255, 0, 0)
            textBox.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            task.wait(0.5)
            textBox.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
            instruction.Text = "✍️ اكتب الكلمة التالية:\n" .. selectedWord
            instruction.TextColor3 = Color3.fromRGB(200, 200, 200)
            textBox.Text = ""
        end
    end)
    
    -- السماح بالضغط على Enter
    textBox.FocusLost:Connect(function(enterPressed)
        if enterPressed then
            confirmBtn.MouseButton1Click:Fire()
        end
    end)
end

-- ========== الطريقة الثالثة: تحدي النقر السريع ==========
local function clickChallenge()
    local screenGui, frame, instruction = createVerificationUI()
    
    local clicksNeeded = 10
    local clicksDone = 0
    
    instruction.Text = "🖱️ انقر على الزر " .. clicksNeeded .. " مرة\nلتأكيد أنك إنسان"
    
    -- زر النقر
    local clickBtn = Instance.new("TextButton")
    clickBtn.Parent = frame
    clickBtn.Size = UDim2.new(0.5, 0, 0, 80)
    clickBtn.Position = UDim2.new(0.25, 0, 0, 180)
    clickBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
    clickBtn.Text = "👆 انقر هنا"
    clickBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    clickBtn.TextSize = 24
    clickBtn.Font = Enum.Font.GothamBold
    
    local cornerClick = Instance.new("UICorner")
    cornerClick.CornerRadius = UDim.new(0, 12)
    cornerClick.Parent = clickBtn
    
    -- عداد التقدم
    local progress = Instance.new("TextLabel")
    progress.Parent = frame
    progress.Size = UDim2.new(0.8, 0, 0, 30)
    progress.Position = UDim2.new(0.1, 0, 0, 280)
    progress.BackgroundTransparency = 1
    progress.Text = "0 / " .. clicksNeeded
    progress.TextColor3 = Color3.fromRGB(200, 200, 200)
    progress.TextSize = 20
    progress.Font = Enum.Font.Gotham
    
    clickBtn.MouseButton1Click:Connect(function()
        clicksDone = clicksDone + 1
        progress.Text = clicksDone .. " / " .. clicksNeeded
        
        -- تغيير لون الزر مؤقتاً
        clickBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        task.wait(0.1)
        clickBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 200)
        
        if clicksDone >= clicksNeeded then
            instruction.Text = "✅ تم التحقق بنجاح!"
            instruction.TextColor3 = Color3.fromRGB(0, 255, 0)
            clickBtn.Text = "✅ تم!"
            clickBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
            
            task.wait(0.8)
            screenGui:Destroy()
            
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "✅ تحقق ناجح",
                Text = "تم التأكد من أنك لست روبوتاً!",
                Duration = 3
            })
        end
    end)
end

-- ========== اختيار نوع التحقق عشوائياً ==========
local function startVerification()
    local types = {numberChallenge, textChallenge, clickChallenge}
    local selectedType = types[math.random(1, #types)]
    selectedType()
end

-- ========== تشغيل السكربت ==========
-- يمكنك تشغيل أي نوع تريده:
-- numberChallenge()   -- تحدي الأرقام
-- textChallenge()     -- تحدي الكتابة
-- clickChallenge()    -- تحدي النقر
-- startVerification() -- اختيار عشوائي

-- تشغيل التحقق عند دخول اللاعب
startVerification()

-- ========== حماية إضافية: كشف الحركات الآلية ==========
-- مراقبة النقرات السريعة جداً (قد تكون روبوت)
local clickCount = 0
local clickTimer = 0

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        clickCount = clickCount + 1
        
        -- إذا كان هناك أكثر من 30 نقرة خلال 5 ثواني
        if clickCount > 30 then
            print("⚠️ نشاط مشبوه: نقرة سريعة جداً!")
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "⚠️ تحذير",
                Text = "نشاط غير طبيعي تم اكتشافه!",
                Duration = 3
            })
            clickCount = 0
        end
        
        -- إعادة تعيين العداد كل 5 ثواني
        task.wait(5)
        clickCount = 0
    end
end)

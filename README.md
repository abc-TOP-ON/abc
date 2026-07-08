-- ================================================
-- نظام تحقق بسيط - بدون أي ميزات إضافية
-- ================================================

local player = game.Players.LocalPlayer

-- توليد رمز عشوائي 5 أحرف
local function generateCode()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local code = ""
    for i = 1, 5 do
        local randIndex = math.random(1, #chars)
        code = code .. string.sub(chars, randIndex, randIndex)
    end
    return code
end

-- إنشاء واجهة بسيطة
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "CaptchaGUI"
    screenGui.Parent = game:GetService("CoreGui")
    
    -- الإطار
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 350, 0, 200)
    frame.Position = UDim2.new(0.5, -175, 0.5, -100)
    frame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.15)
    frame.BackgroundTransparency = 0.1
    frame.BorderSizePixel = 0
    frame.Parent = screenGui
    
    -- زوايا مستديرة
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = frame
    
    -- عنوان
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 0, 0, 10)
    title.Text = "تحقق من أنك لست روبوتاً"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold
    title.BackgroundTransparency = 1
    title.Parent = frame
    
    -- الرمز
    local codeLabel = Instance.new("TextLabel")
    codeLabel.Size = UDim2.new(0.8, 0, 0, 50)
    codeLabel.Position = UDim2.new(0.1, 0, 0, 45)
    codeLabel.Text = ""
    codeLabel.TextColor3 = Color3.new(0.3, 0.8, 1)
    codeLabel.TextScaled = true
    codeLabel.Font = Enum.Font.GothamBold
    codeLabel.BackgroundColor3 = Color3.new(0.15, 0.15, 0.2)
    codeLabel.Parent = frame
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 6)
    codeCorner.Parent = codeLabel
    
    -- مربع الإدخال
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0.8, 0, 0, 35)
    textBox.Position = UDim2.new(0.1, 0, 0, 105)
    textBox.PlaceholderText = "أدخل الرمز..."
    textBox.Text = ""
    textBox.TextColor3 = Color3.new(1, 1, 1)
    textBox.BackgroundColor3 = Color3.new(0.15, 0.15, 0.2)
    textBox.Font = Enum.Font.Gotham
    textBox.TextScaled = true
    textBox.Parent = frame
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 6)
    boxCorner.Parent = textBox
    
    -- زر التحقق
    local submitBtn = Instance.new("TextButton")
    submitBtn.Size = UDim2.new(0.4, 0, 0, 35)
    submitBtn.Position = UDim2.new(0.3, 0, 0, 150)
    submitBtn.Text = "تحقق"
    submitBtn.TextColor3 = Color3.new(1, 1, 1)
    submitBtn.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    submitBtn.Font = Enum.Font.GothamBold
    submitBtn.TextScaled = true
    submitBtn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = submitBtn
    
    -- رسالة النتيجة
    local resultLabel = Instance.new("TextLabel")
    resultLabel.Size = UDim2.new(1, 0, 0, 25)
    resultLabel.Position = UDim2.new(0, 0, 0, 175)
    resultLabel.Text = ""
    resultLabel.TextColor3 = Color3.new(1, 1, 1)
    resultLabel.TextScaled = true
    resultLabel.Font = Enum.Font.Gotham
    resultLabel.BackgroundTransparency = 1
    resultLabel.Parent = frame
    
    return {
        ScreenGui = screenGui,
        CodeLabel = codeLabel,
        TextBox = textBox,
        SubmitBtn = submitBtn,
        ResultLabel = resultLabel
    }
end

-- إنشاء الواجهة
local gui = createGUI()

-- توليد الرمز
local currentCode = generateCode()
gui.CodeLabel.Text = currentCode

-- تعطيل الحركة
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChild("Humanoid")
if humanoid then
    humanoid.WalkSpeed = 0
    humanoid.JumpPower = 0
end

-- التحقق
local function verify()
    local userInput = gui.TextBox.Text
    userInput = string.upper(userInput)
    
    if userInput == currentCode then
        -- نجاح
        gui.ResultLabel.Text = "تم التحقق بنجاح!"
        gui.ResultLabel.TextColor3 = Color3.new(0, 1, 0)
        
        if humanoid then
            humanoid.WalkSpeed = 16
            humanoid.JumpPower = 50
        end
        
        task.wait(1)
        gui.ScreenGui:Destroy()
        
    else
        -- فشل - طرد فوري
        gui.ResultLabel.Text = "رمز خاطئ! يتم طردك..."
        gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
        gui.SubmitBtn.Visible = false
        gui.TextBox.Visible = false
        
        task.wait(1.5)
        player:Kick("رمز تحقق خاطئ!")
    end
end

-- أحداث
gui.SubmitBtn.MouseButton1Click:Connect(verify)
gui.TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        verify()
    end
end)

-- مؤقت 60 ثانية
local timeLeft = 60
task.spawn(function()
    while timeLeft > 0 do
        task.wait(1)
        timeLeft = timeLeft - 1
        
        if timeLeft <= 0 then
            gui.ResultLabel.Text = "انتهى الوقت!"
            task.wait(1)
            player:Kick("انتهى وقت التحقق!")
        end
    end
end)

-- إشعار
print("نظام التحقق يعمل!")
print("الرمز: " .. currentCode)

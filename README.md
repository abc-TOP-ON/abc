-- ============================================
-- سكربت واجهة الشات - يعرض جميع رسائل الشات
-- ============================================

local player = game.Players.LocalPlayer
local chatService = game:GetService("Chat")
local players = game:GetService("Players")
local userInput = game:GetService("UserInputService")

-- ========== إنشاء الواجهة الرئيسية ==========
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ChatInterface"
screenGui.Parent = player.PlayerGui
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ========== الإطار الرئيسي القابل للتحريك ==========
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 500)
mainFrame.Position = UDim2.new(0.1, 0, 0.1, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BackgroundTransparency = 0.85
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(255, 255, 255)
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

-- ========== شريط العنوان ==========
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
titleBar.BackgroundTransparency = 0.3
titleBar.Parent = mainFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -80, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "📨 Chat Monitor"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

-- ========== زر التصغير ==========
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -60, 0, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(200, 200, 0)
minimizeBtn.Text = "─"
minimizeBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
minimizeBtn.TextScaled = true
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Parent = titleBar

-- ========== زر الإغلاق ==========
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextScaled = true
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Parent = titleBar

-- ========== حاوية الرسائل (قابلة للتمرير) ==========
local messageContainer = Instance.new("ScrollingFrame")
messageContainer.Size = UDim2.new(1, -10, 1, -40)
messageContainer.Position = UDim2.new(0, 5, 0, 35)
messageContainer.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
messageContainer.BackgroundTransparency = 0.5
messageContainer.BorderSizePixel = 0
messageContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
messageContainer.ScrollBarThickness = 8
messageContainer.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
messageContainer.Parent = mainFrame

-- ========== متغيرات التحكم ==========
local messages = {}
local maxMessages = 100
local isMinimized = false
local isDragging = false
local dragStart = nil
local dragStartPos = nil

-- ========== دالة لإضافة رسالة ==========
local function addMessage(speaker, message, color)
    -- تحديد اللون الافتراضي إذا لم يتم تحديده
    if not color then
        color = Color3.fromRGB(255, 255, 255)
    end
    
    -- تحديد اسم المرسل
    local speakerName = "System"
    if speaker and speaker ~= "" then
        speakerName = speaker
    end
    
    -- إنشاء إطار الرسالة
    local msgFrame = Instance.new("Frame")
    msgFrame.Size = UDim2.new(1, -10, 0, 30)
    msgFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    msgFrame.BackgroundTransparency = 0.5
    msgFrame.BorderSizePixel = 1
    msgFrame.BorderColor3 = Color3.fromRGB(80, 80, 80)
    msgFrame.Parent = messageContainer
    
    -- اسم المرسل
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(0, 80, 1, 0)
    nameLabel.Position = UDim2.new(0, 5, 0, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = speakerName
    nameLabel.TextColor3 = color
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextXAlignment = Enum.TextXAlignment.Left
    nameLabel.Parent = msgFrame
    
    -- نص الرسالة
    local msgLabel = Instance.new("TextLabel")
    msgLabel.Size = UDim2.new(1, -95, 1, 0)
    msgLabel.Position = UDim2.new(0, 85, 0, 0)
    msgLabel.BackgroundTransparency = 1
    msgLabel.Text = message
    msgLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    msgLabel.TextScaled = true
    msgLabel.Font = Enum.Font.Gotham
    msgLabel.TextXAlignment = Enum.TextXAlignment.Left
    msgLabel.Parent = msgFrame
    
    -- إضافة إلى قائمة الرسائل
    table.insert(messages, msgFrame)
    
    -- تحديث حجم Canvas
    local canvasY = #messages * 35
    messageContainer.CanvasSize = UDim2.new(0, 0, 0, canvasY)
    
    -- حذف الرسائل القديمة إذا تجاوزت الحد
    if #messages > maxMessages then
        local oldMsg = table.remove(messages, 1)
        oldMsg:Destroy()
    end
    
    -- التمرير للأسفل تلقائياً
    messageContainer.CanvasPosition = Vector2.new(0, messageContainer.CanvasSize.Y.Offset)
end

-- ========== مراقبة رسائل الشات ==========
-- مراقبة رسائل اللاعبين المحليين
chatService.Chatted:Connect(function(message, speaker)
    local speakerName = speaker.Name
    local playerColor = Color3.fromRGB(255, 255, 255)
    
    -- محاولة الحصول على لون اللاعب
    if speaker then
        local playerObj = players:FindFirstChild(speakerName)
        if playerObj then
            playerColor = playerObj.TeamColor and playerObj.TeamColor.Color or Color3.fromRGB(255, 255, 255)
        end
    end
    
    addMessage(speakerName, message, playerColor)
end)

-- ========== مراقبة رسائل النظام ==========
-- استخدام RemoteEvent لالتقاط جميع الرسائل (إذا كانت اللعبة تدعم)
local function catchAllChatMessages()
    -- محاولة التقاط رسائل النظام
    local playerGui = player.PlayerGui
    for _, gui in pairs(playerGui:GetChildren()) do
        if gui:IsA("ScreenGui") then
            for _, child in pairs(gui:GetDescendants()) do
                if child:IsA("TextLabel") and child.Text and child.Text ~= "" then
                    -- هذا محاولة لالتقاط رسائل من واجهات أخرى
                end
            end
        end
    end
end

-- ========== مراقبة اللاعبين الجدد ==========
players.PlayerAdded:Connect(function(newPlayer)
    newPlayer.Chatted:Connect(function(message)
        local playerColor = newPlayer.TeamColor and newPlayer.TeamColor.Color or Color3.fromRGB(255, 255, 255)
        addMessage(newPlayer.Name, message, playerColor)
    end)
end)

-- ========== أحداث التحكم في الواجهة ==========
-- تصغير/تكبير
minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        mainFrame.Size = UDim2.new(0, 400, 0, 30)
        messageContainer.Visible = false
        minimizeBtn.Text = "□"
    else
        mainFrame.Size = UDim2.new(0, 400, 0, 500)
        messageContainer.Visible = true
        minimizeBtn.Text = "─"
    end
end)

-- إغلاق الواجهة
closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
    print("✅ Chat Interface Closed")
end)

-- ========== إضافة رسالة ترحيبية ==========
addMessage("System", "🟢 تم تفعيل واجهة الشات!", Color3.fromRGB(0, 255, 0))
addMessage("System", "📌 اسحب الشريط العلوي لتحريك الواجهة", Color3.fromRGB(255, 255, 0))
addMessage("System", "🔄 تم تسجيل " .. #players:GetPlayers() .. " لاعب متصل", Color3.fromRGB(0, 150, 255))

-- ========== اختصار لوحة المفاتيح ==========
userInput.InputBegan:Connect(function(input)
    -- اضغط Ctrl + C لإغلاق الواجهة
    if input.KeyCode == Enum.KeyCode.C and userInput:IsKeyDown(Enum.KeyCode.LeftControl) then
        screenGui:Destroy()
        print("✅ Chat Interface Closed (Ctrl+C)")
    end
    
    -- اضغط Ctrl + M للتصغير/التكبير
    if input.KeyCode == Enum.KeyCode.M and userInput:IsKeyDown(Enum.KeyCode.LeftControl) then
        minimizeBtn.MouseButton1Click:Fire()
    end
end)

-- ========== وظائف إضافية ==========
-- دالة لمسح جميع الرسائل
local function clearMessages()
    for _, msg in pairs(messages) do
        msg:Destroy()
    end
    messages = {}
    messageContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
    addMessage("System", "🗑️ تم مسح جميع الرسائل", Color3.fromRGB(255, 100, 100))
end

-- دالة لتصدير الرسائل (للكوبي)
local function exportMessages()
    local text = ""
    for _, msg in pairs(messages) do
        local nameLabel = msg:FindFirstChildWhichIsA("TextLabel")
        local msgLabel = msg:FindFirstChildWhichIsA("TextLabel")
        if nameLabel and msgLabel then
            text = text .. nameLabel.Text .. ": " .. msgLabel.Text .. "\n"
        end
    end
    setclipboard(text) -- إذا كان المدعم
    print("📋 تم نسخ " .. #messages .. " رسالة")
end

-- ========== دالة تحريك الواجهة بالماوس ==========
mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = true
        dragStart = input.Position
        dragStartPos = mainFrame.Position
    end
end)

mainFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = false
    end
end)

userInput.InputChanged:Connect(function(input)
    if isDragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(
            dragStartPos.X.Scale,
            dragStartPos.X.Offset + delta.X,
            dragStartPos.Y.Scale,
            dragStartPos.Y.Offset + delta.Y
        )
    end
end)

print("✅ Chat Interface Script Loaded Successfully!")
print("📌 استخدم Ctrl+C للإغلاق | Ctrl+M للتصغير")

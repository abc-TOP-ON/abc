-- =============================================
-- ✅ VERIFICATION SCRIPT (SIMPLE & WORKING)
-- =============================================

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- ====== SETTINGS ======
local MAX_ATTEMPTS = 3
local TIME_LIMIT = 60  -- 60 seconds
local WORD_LENGTH = 5

-- ====== GENERATE RANDOM WORD ======
local function GenerateWord()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local word = ""
    for i = 1, WORD_LENGTH do
        local index = math.random(1, #chars)
        word = word .. string.sub(chars, index, index)
    end
    return word
end

local CORRECT_WORD = GenerateWord()
local attempts = 0
local timeLeft = TIME_LIMIT
local verified = false
local timerRunning = true

print("🔑 CODE: " .. CORRECT_WORD)

-- ====== CREATE GUI ======
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 400, 0, 280)
Frame.Position = UDim2.new(0.5, -200, 0.5, -140)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
Frame.BackgroundTransparency = 0.1
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local FrameCorner = Instance.new("UICorner")
FrameCorner.CornerRadius = UDim.new(0, 12)
FrameCorner.Parent = Frame

-- Title
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 45)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "🔐 VERIFICATION"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.Parent = Frame

-- Timer
local TimerLabel = Instance.new("TextLabel")
TimerLabel.Size = UDim2.new(0, 70, 0, 35)
TimerLabel.Position = UDim2.new(1, -80, 0, 5)
TimerLabel.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
TimerLabel.BackgroundTransparency = 0.2
TimerLabel.Text = "⏱ " .. TIME_LIMIT
TimerLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TimerLabel.TextSize = 18
TimerLabel.Font = Enum.Font.GothamBold
TimerLabel.Parent = Frame

local TimerCorner = Instance.new("UICorner")
TimerCorner.CornerRadius = UDim.new(0, 8)
TimerCorner.Parent = TimerLabel

-- Description
local Desc = Instance.new("TextLabel")
Desc.Size = UDim2.new(1, -20, 0, 30)
Desc.Position = UDim2.new(0, 10, 0, 45)
Desc.BackgroundTransparency = 1
Desc.Text = "Type the code below:"
Desc.TextColor3 = Color3.fromRGB(200, 200, 200)
Desc.TextSize = 14
Desc.Font = Enum.Font.Gotham
Desc.Parent = Frame

-- Attempts
local AttemptsLabel = Instance.new("TextLabel")
AttemptsLabel.Size = UDim2.new(0, 150, 0, 25)
AttemptsLabel.Position = UDim2.new(0, 10, 0, 75)
AttemptsLabel.BackgroundTransparency = 1
AttemptsLabel.Text = "Attempts: 0/" .. MAX_ATTEMPTS
AttemptsLabel.TextColor3 = Color3.fromRGB(150, 200, 255)
AttemptsLabel.TextSize = 13
AttemptsLabel.Font = Enum.Font.Gotham
AttemptsLabel.TextXAlignment = Enum.TextXAlignment.Left
AttemptsLabel.Parent = Frame

-- Word Display
local WordDisplay = Instance.new("TextLabel")
WordDisplay.Size = UDim2.new(1, -40, 0, 55)
WordDisplay.Position = UDim2.new(0, 20, 0, 105)
WordDisplay.BackgroundColor3 = Color3.fromRGB(50, 50, 80)
WordDisplay.BackgroundTransparency = 0.2
WordDisplay.Text = CORRECT_WORD
WordDisplay.TextColor3 = Color3.fromRGB(0, 255, 200)
WordDisplay.TextScaled = true
WordDisplay.Font = Enum.Font.GothamBold
WordDisplay.Parent = Frame

local WordCorner = Instance.new("UICorner")
WordCorner.CornerRadius = UDim.new(0, 10)
WordCorner.Parent = WordDisplay

-- Input Box
local TextBox = Instance.new("TextBox")
TextBox.Size = UDim2.new(1, -40, 0, 45)
TextBox.Position = UDim2.new(0, 20, 0, 170)
TextBox.BackgroundColor3 = Color3.fromRGB(40, 40, 70)
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.Text = ""
TextBox.PlaceholderText = "Type here..."
TextBox.TextSize = 22
TextBox.Font = Enum.Font.GothamBold
TextBox.ClearTextOnFocus = false
TextBox.Parent = Frame

local TextBoxCorner = Instance.new("UICorner")
TextBoxCorner.CornerRadius = UDim.new(0, 10)
TextBoxCorner.Parent = TextBox

-- ✅ AUTO-REMOVE SPACES (while typing)
TextBox:GetPropertyChangedSignal("Text"):Connect(function()
    local text = TextBox.Text
    local clean = string.gsub(text, " ", "")
    if text ~= clean then
        TextBox.Text = clean
        TextBox.CursorPosition = #clean + 1
    end
end)

-- Confirm Button
local ConfirmButton = Instance.new("TextButton")
ConfirmButton.Size = UDim2.new(0.8, 0, 0, 45)
ConfirmButton.Position = UDim2.new(0.1, 0, 0, 228)
ConfirmButton.BackgroundColor3 = Color3.fromRGB(0, 180, 255)
ConfirmButton.Text = "✅ CONFIRM"
ConfirmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ConfirmButton.TextSize = 20
ConfirmButton.Font = Enum.Font.GothamBold
ConfirmButton.Parent = Frame

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, 10)
ButtonCorner.Parent = ConfirmButton

-- Close Button
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -30, 0, 5)
CloseButton.BackgroundTransparency = 1
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(255, 100, 100)
CloseButton.TextSize = 16
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = Frame

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- ====== TIMER ======
local function UpdateTimer()
    if timeLeft <= 0 then
        timerRunning = false
        Title.Text = "⏰ TIME'S UP!"
        Title.TextColor3 = Color3.fromRGB(255, 0, 0)
        ConfirmButton.Text = "Kicking..."
        ConfirmButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
        TextBox.Active = false
        ConfirmButton.Active = false
        wait(1.5)
        LocalPlayer:Kick("⏰ Time's up!")
        return
    end
    
    TimerLabel.Text = "⏱ " .. timeLeft
    
    if timeLeft <= 5 then
        TimerLabel.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        TimerLabel.BackgroundTransparency = 0.1
    end
end

coroutine.wrap(function()
    while timerRunning and timeLeft > 0 and not verified do
        wait(1)
        timeLeft = timeLeft - 1
        UpdateTimer()
    end
end)()

-- ====== VERIFICATION ======
ConfirmButton.MouseButton1Click:Connect(function()
    local input = TextBox.Text
    local cleaned = string.gsub(input, " ", "")  -- Remove spaces
    
    print("📝 You typed: '" .. input .. "'")
    print("🧹 Cleaned: '" .. cleaned .. "'")
    print("🔑 Expected: '" .. CORRECT_WORD .. "'")
    
    if cleaned == "" then
        Title.Text = "⚠️ Type something!"
        Title.TextColor3 = Color3.fromRGB(255, 200, 0)
        return
    end
    
    if cleaned == CORRECT_WORD then
        -- ✅ CORRECT
        verified = true
        timerRunning = false
        Title.Text = "✅ VERIFIED!"
        Title.TextColor3 = Color3.fromRGB(0, 255, 100)
        ConfirmButton.Text = "✅ DONE"
        ConfirmButton.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        TextBox.Active = false
        ConfirmButton.Active = false
        wait(1.5)
        ScreenGui:Destroy()
    else
        -- ❌ WRONG
        attempts = attempts + 1
        AttemptsLabel.Text = "Attempts: " .. attempts .. "/" .. MAX_ATTEMPTS
        
        if attempts >= MAX_ATTEMPTS then
            Title.Text = "❌ FAILED!"
            Title.TextColor3 = Color3.fromRGB(255, 0, 0)
            ConfirmButton.Text = "Kicking..."
            ConfirmButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            TextBox.Active = false
            wait(1.5)
            LocalPlayer:Kick("❌ Too many failed attempts!")
        else
            Title.Text = "❌ Wrong! " .. (MAX_ATTEMPTS - attempts) .. " left"
            Title.TextColor3 = Color3.fromRGB(255, 200, 0)
            TextBox.Text = ""
            TextBox.PlaceholderText = "Try again..."
            
            -- Shake
            TextBox.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
            wait(0.3)
            TextBox.BackgroundColor3 = Color3.fromRGB(40, 40, 70)
            
            -- New word
            CORRECT_WORD = GenerateWord()
            WordDisplay.Text = CORRECT_WORD
            print("🔄 New code: " .. CORRECT_WORD)
        end
    end
end)

-- Enter key
TextBox.FocusLost:Connect(function(enter)
    if enter then
        ConfirmButton.MouseButton1Click:Fire()
    end
end)

print("✅ Script loaded! Code: " .. CORRECT_WORD)

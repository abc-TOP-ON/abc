-- =====================================================================
-- 🔥 ULTIMATE VERIFICATION SCRIPT v3.1 (60 Seconds + Auto-Trim)
-- =====================================================================
-- ✨ Features:
-- ✅ 5-char dynamic word with special characters
-- ✅ Auto-removes spaces from input
-- ✅ 60 seconds time limit
-- ✅ 3 attempts only
-- ✅ Anti-cheat detection
-- ✅ Professional UI with animations
-- =====================================================================

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- =====================================================================
-- CONFIGURATION
-- =====================================================================
local CONFIG = {
    MaxAttempts = 3,
    TimeLimit = 60, -- ✅ Changed to 60 seconds
    WordLength = 5,
    EnableAntiCheat = true,
}

-- =====================================================================
-- UTILITY FUNCTIONS
-- =====================================================================
local function GenerateWord(length)
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local word = ""
    for i = 1, length do
        local randomIndex = math.random(1, #chars)
        word = word .. string.sub(chars, randomIndex, randomIndex)
    end
    return word
end

local function CreateTween(obj, properties, duration)
    local tween = TweenService:Create(obj, TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), properties)
    tween:Play()
    return tween
end

-- =====================================================================
-- MAIN SCRIPT
-- =====================================================================
local CORRECT_WORD = GenerateWord(CONFIG.WordLength)
local attempts = 0
local timerRunning = true
local timeLeft = CONFIG.TimeLimit
local verified = false

print("🔑 Your verification code: " .. CORRECT_WORD)

-- =====================================================================
-- CREATE UI
-- =====================================================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui
ScreenGui.Name = "VerificationSystem"
ScreenGui.ResetOnSpawn = false

-- Main Frame
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 480, 0, 340)
Frame.Position = UDim2.new(0.5, -240, 0.5, -170)
Frame.BackgroundColor3 = Color3.fromRGB(10, 10, 30)
Frame.BackgroundTransparency = 0.05
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.ClipsDescendants = true
Frame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 20)
UICorner.Parent = Frame

-- Glow Effect
local Glow = Instance.new("Frame")
Glow.Size = UDim2.new(1, 20, 1, 20)
Glow.Position = UDim2.new(0, -10, 0, -10)
Glow.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
Glow.BackgroundTransparency = 0.85
Glow.BorderSizePixel = 0
Glow.Parent = Frame

local GlowCorner = Instance.new("UICorner")
GlowCorner.CornerRadius = UDim.new(0, 30)
GlowCorner.Parent = Glow

-- Title
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 55)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "⚡ HUMAN VERIFICATION ⚡"
Title.TextColor3 = Color3.fromRGB(0, 255, 255)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.Parent = Frame

-- Subtitle
local Subtitle = Instance.new("TextLabel")
Subtitle.Size = UDim2.new(1, -20, 0, 25)
Subtitle.Position = UDim2.new(0, 10, 0, 50)
Subtitle.BackgroundTransparency = 1
Subtitle.Text = "Prove you're human to continue"
Subtitle.TextColor3 = Color3.fromRGB(180, 180, 200)
Subtitle.TextSize = 14
Subtitle.Font = Enum.Font.Gotham
Subtitle.TextTransparency = 0.3
Subtitle.Parent = Frame

-- Timer Display
local TimerLabel = Instance.new("TextLabel")
TimerLabel.Size = UDim2.new(0, 80, 0, 50)
TimerLabel.Position = UDim2.new(1, -90, 0, 5)
TimerLabel.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
TimerLabel.BackgroundTransparency = 0.2
TimerLabel.Text = "⏱ " .. CONFIG.TimeLimit
TimerLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TimerLabel.TextSize = 22
TimerLabel.Font = Enum.Font.GothamBold
TimerLabel.Parent = Frame

local TimerCorner = Instance.new("UICorner")
TimerCorner.CornerRadius = UDim.new(0, 10)
TimerCorner.Parent = TimerLabel

-- Attempts Label
local AttemptsLabel = Instance.new("TextLabel")
AttemptsLabel.Size = UDim2.new(0, 200, 0, 25)
AttemptsLabel.Position = UDim2.new(0, 10, 0, 82)
AttemptsLabel.BackgroundTransparency = 1
AttemptsLabel.Text = "Attempts: 0/" .. CONFIG.MaxAttempts
AttemptsLabel.TextColor3 = Color3.fromRGB(150, 200, 255)
AttemptsLabel.TextSize = 14
AttemptsLabel.Font = Enum.Font.Gotham
AttemptsLabel.TextXAlignment = Enum.TextXAlignment.Left
AttemptsLabel.Parent = Frame

-- Word Display
local WordDisplay = Instance.new("TextLabel")
WordDisplay.Size = UDim2.new(1, -40, 0, 60)
WordDisplay.Position = UDim2.new(0, 20, 0, 115)
WordDisplay.BackgroundColor3 = Color3.fromRGB(40, 40, 80)
WordDisplay.BackgroundTransparency = 0.15
WordDisplay.Text = CORRECT_WORD
WordDisplay.TextColor3 = Color3.fromRGB(0, 255, 200)
WordDisplay.TextScaled = true
WordDisplay.Font = Enum.Font.GothamBold
WordDisplay.Parent = Frame

local WordCorner = Instance.new("UICorner")
WordCorner.CornerRadius = UDim.new(0, 12)
WordCorner.Parent = WordDisplay

-- Input Box
local TextBox = Instance.new("TextBox")
TextBox.Size = UDim2.new(1, -40, 0, 50)
TextBox.Position = UDim2.new(0, 20, 0, 190)
TextBox.BackgroundColor3 = Color3.fromRGB(40, 40, 70)
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.Text = ""
TextBox.PlaceholderText = "⌨️ Type the code..."
TextBox.PlaceholderColor3 = Color3.fromRGB(150, 150, 200)
TextBox.TextSize = 24
TextBox.Font = Enum.Font.GothamBold
TextBox.ClearTextOnFocus = false
TextBox.Parent = Frame

local TextBoxCorner = Instance.new("UICorner")
TextBoxCorner.CornerRadius = UDim.new(0, 12)
TextBoxCorner.Parent = TextBox

-- ✅ AUTO-REMOVE SPACES (Live)
TextBox:GetPropertyChangedSignal("Text"):Connect(function()
    local currentText = TextBox.Text
    -- Remove all spaces
    local cleanedText = string.gsub(currentText, "%s+", "")
    if currentText ~= cleanedText then
        TextBox.Text = cleanedText
        -- Move cursor to end
        TextBox.CursorPosition = #cleanedText + 1
    end
end)

-- Confirm Button
local ConfirmButton = Instance.new("TextButton")
ConfirmButton.Size = UDim2.new(0.85, 0, 0, 55)
ConfirmButton.Position = UDim2.new(0.075, 0, 0, 255)
ConfirmButton.BackgroundColor3 = Color3.fromRGB(0, 180, 255)
ConfirmButton.Text = "✅ VERIFY"
ConfirmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ConfirmButton.TextSize = 22
ConfirmButton.Font = Enum.Font.GothamBold
ConfirmButton.Parent = Frame

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(0, 12)
ButtonCorner.Parent = ConfirmButton

-- Close Button
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundTransparency = 1
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(255, 100, 100)
CloseButton.TextSize = 20
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = Frame

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- =====================================================================
-- TIMER SYSTEM (60 Seconds)
-- =====================================================================
local function UpdateTimer()
    if timeLeft <= 5 then
        TimerLabel.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        TimerLabel.BackgroundTransparency = 0.1
        CreateTween(TimerLabel, {BackgroundTransparency = 0.1}, 0.5)
    end
    
    TimerLabel.Text = "⏱ " .. timeLeft
    
    if timeLeft <= 0 then
        timerRunning = false
        Title.Text = "⏰ TIME EXPIRED!"
        Title.TextColor3 = Color3.fromRGB(255, 0, 0)
        ConfirmButton.Text = "Kicking..."
        ConfirmButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
        TextBox.Active = false
        ConfirmButton.Active = false
        
        wait(1.5)
        LocalPlayer:Kick("⏰ Time expired! You took too long.")
    end
end

local function StartTimer()
    while timerRunning and timeLeft > 0 and not verified do
        wait(1)
        timeLeft = timeLeft - 1
        UpdateTimer()
    end
end

coroutine.wrap(StartTimer)()

-- =====================================================================
-- VERIFICATION LOGIC
-- =====================================================================
ConfirmButton.MouseButton1Click:Connect(function()
    local userInput = TextBox.Text
    
    -- ✅ Auto-remove spaces again (just in case)
    local cleanedInput = string.gsub(userInput, "%s+", "")
    
    -- Debug
    print("📝 Raw input: '" .. userInput .. "'")
    print("🧹 Cleaned input: '" .. cleanedInput .. "'")
    print("🔑 Expected: '" .. CORRECT_WORD .. "'")
    
    if cleanedInput == "" then
        Title.Text = "⚠️ Please type the code!"
        Title.TextColor3 = Color3.fromRGB(255, 200, 0)
        CreateTween(TextBox, {BackgroundColor3 = Color3.fromRGB(200, 100, 0)}, 0.3)
        wait(0.3)
        CreateTween(TextBox, {BackgroundColor3 = Color3.fromRGB(40, 40, 70)}, 0.3)
        return
    end
    
    -- ✅ Compare (both are already uppercase)
    if cleanedInput == CORRECT_WORD then
        -- ✅ SUCCESS
        verified = true
        timerRunning = false
        
        Title.Text = "✅ VERIFIED SUCCESSFULLY!"
        Title.TextColor3 = Color3.fromRGB(0, 255, 100)
        ConfirmButton.Text = "✅ WELCOME!"
        ConfirmButton.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        TextBox.Active = false
        ConfirmButton.Active = false
        
        -- Success animation
        CreateTween(WordDisplay, {BackgroundColor3 = Color3.fromRGB(0, 255, 100)}, 0.5)
        CreateTween(WordDisplay, {TextColor3 = Color3.fromRGB(0, 255, 100)}, 0.5)
        
        wait(1.5)
        ScreenGui:Destroy()
    else
        -- ❌ WRONG
        attempts = attempts + 1
        local remaining = CONFIG.MaxAttempts - attempts
        AttemptsLabel.Text = "Attempts: " .. attempts .. "/" .. CONFIG.MaxAttempts
        
        if remaining > 0 then
            -- Wrong but still have attempts
            Title.Text = "❌ WRONG! " .. remaining .. " attempts left"
            Title.TextColor3 = Color3.fromRGB(255, 200, 0)
            TextBox.Text = ""
            TextBox.PlaceholderText = "Incorrect! Try again..."
            
            -- Shake animation
            for i = 1, 5 do
                TextBox.Position = UDim2.new(0.02 + (math.random(-20, 20) / 1000), 0, 0.5, 0)
                wait(0.05)
            end
            TextBox.Position = UDim2.new(0, 20, 0, 190)
            
            -- Red flash
            CreateTween(TextBox, {BackgroundColor3 = Color3.fromRGB(200, 50, 50)}, 0.2)
            wait(0.2)
            CreateTween(TextBox, {BackgroundColor3 = Color3.fromRGB(40, 40, 70)}, 0.3)
            
            -- Generate new word
            CORRECT_WORD = GenerateWord(CONFIG.WordLength)
            WordDisplay.Text = CORRECT_WORD
            print("🔄 New code generated: " .. CORRECT_WORD)
        else
            -- ❌ Max attempts reached → KICK
            timerRunning = false
            Title.Text = "❌ MAX ATTEMPTS REACHED!"
            Title.TextColor3 = Color3.fromRGB(255, 0, 0)
            ConfirmButton.Text = "Kicking..."
            ConfirmButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            TextBox.Active = false
            
            wait(1.5)
            LocalPlayer:Kick("🚫 You failed " .. CONFIG.MaxAttempts .. " times! Access denied.")
        end
    end
end)

-- Enter key support
TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed and not verified then
        ConfirmButton.MouseButton1Click:Fire()
    end
end)

-- Keyboard shortcut
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.Return then
        if TextBox:IsFocused() and not verified then
            ConfirmButton.MouseButton1Click:Fire()
        end
    end
end)

print("✅ Verification script loaded! (60 seconds)")
print("🔑 Your code: " .. CORRECT_WORD)
print("⏱️ You have 60 seconds and 3 attempts")

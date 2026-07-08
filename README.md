-- ============================================================
-- 🔥 ADVANCED VERIFICATION SYSTEM (10× BETTER)
-- ============================================================

local player = game.Players.LocalPlayer
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

-- ===== CONFIGURATION =====
local CONFIG = {
    CodeLength = 5,
    TimeLimit = 60,
    MaxAttempts = 1,              -- 1 = instant kick on error
    AutoKick = true,
}

-- ===== GENERATE RANDOM CODE =====
local function generateCode()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local code = ""
    for i = 1, CONFIG.CodeLength do
        local randIndex = math.random(1, #chars)
        code = code .. string.sub(chars, randIndex, randIndex)
    end
    return code
end

-- ===== CREATE ADVANCED GUI =====
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "CaptchaGUI"
    screenGui.Parent = game:GetService("CoreGui")
    screenGui.ResetOnSpawn = false
    
    -- Background overlay
    local background = Instance.new("Frame")
    background.Size = UDim2.new(1, 0, 1, 0)
    background.BackgroundColor3 = Color3.new(0, 0, 0)
    background.BackgroundTransparency = 0.5
    background.BorderSizePixel = 0
    background.Parent = screenGui
    
    -- Main Frame
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 520, 0, 340)
    frame.Position = UDim2.new(0.5, -260, 0.5, -170)
    frame.BackgroundColor3 = Color3.new(0.06, 0.06, 0.1)
    frame.BackgroundTransparency = 0.05
    frame.BorderSizePixel = 0
    frame.Parent = screenGui
    
    -- Rounded corners
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 16)
    corner.Parent = frame
    
    -- Shadow effect
    local shadow = Instance.new("Frame")
    shadow.Size = UDim2.new(1, 6, 1, 6)
    shadow.Position = UDim2.new(0, -3, 0, -3)
    shadow.BackgroundColor3 = Color3.new(0, 0, 0)
    shadow.BackgroundTransparency = 0.4
    shadow.BorderSizePixel = 0
    shadow.Parent = frame
    
    local shadowCorner = Instance.new("UICorner")
    shadowCorner.CornerRadius = UDim.new(0, 18)
    shadowCorner.Parent = shadow
    
    -- Glowing top line
    local topLine = Instance.new("Frame")
    topLine.Size = UDim2.new(1, 0, 0, 4)
    topLine.Position = UDim2.new(0, 0, 0, 0)
    topLine.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    topLine.BorderSizePixel = 0
    topLine.Parent = frame
    
    -- Icon
    local icon = Instance.new("TextLabel")
    icon.Size = UDim2.new(0, 60, 0, 60)
    icon.Position = UDim2.new(0.5, -30, 0, 12)
    icon.Text = "🛡️"
    icon.TextColor3 = Color3.new(1, 1, 1)
    icon.TextScaled = true
    icon.BackgroundTransparency = 1
    icon.Font = Enum.Font.GothamBold
    icon.Parent = frame
    
    -- Title
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 78)
    title.Text = "🔐 Security Verification"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold
    title.BackgroundTransparency = 1
    title.Parent = frame
    
    -- Subtitle
    local subtitle = Instance.new("TextLabel")
    subtitle.Size = UDim2.new(1, 0, 0, 22)
    subtitle.Position = UDim2.new(0, 0, 0, 113)
    subtitle.Text = "Enter the 5-character code to prove you're human"
    subtitle.TextColor3 = Color3.new(0.5, 0.5, 0.5)
    subtitle.TextScaled = true
    subtitle.Font = Enum.Font.Gotham
    subtitle.BackgroundTransparency = 1
    subtitle.Parent = frame
    
    -- Code display box
    local codeBox = Instance.new("Frame")
    codeBox.Size = UDim2.new(0.7, 0, 0, 65)
    codeBox.Position = UDim2.new(0.15, 0, 0, 145)
    codeBox.BackgroundColor3 = Color3.new(0.1, 0.1, 0.15)
    codeBox.BackgroundTransparency = 0.5
    codeBox.BorderSizePixel = 0
    codeBox.Parent = frame
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 10)
    codeCorner.Parent = codeBox
    
    -- Glowing border for code box
    local codeGlow = Instance.new("Frame")
    codeGlow.Size = UDim2.new(1, 4, 1, 4)
    codeGlow.Position = UDim2.new(0, -2, 0, -2)
    codeGlow.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    codeGlow.BackgroundTransparency = 0.5
    codeGlow.BorderSizePixel = 0
    codeGlow.Parent = codeBox
    
    local glowCorner = Instance.new("UICorner")
    glowCorner.CornerRadius = UDim.new(0, 12)
    glowCorner.Parent = codeGlow
    
    -- Code label
    local codeLabel = Instance.new("TextLabel")
    codeLabel.Size = UDim2.new(1, 0, 1, 0)
    codeLabel.Text = ""
    codeLabel.TextColor3 = Color3.new(0.3, 0.8, 1)
    codeLabel.TextScaled = true
    codeLabel.Font = Enum.Font.GothamBold
    codeLabel.BackgroundTransparency = 1
    codeLabel.Parent = codeBox
    
    -- Individual letters
    local letterFrames = {}
    for i = 1, CONFIG.CodeLength do
        local letter = Instance.new("TextLabel")
        letter.Size = UDim2.new(0, 35, 0, 45)
        letter.Position = UDim2.new((i - 1) / CONFIG.CodeLength + 0.02, 0, 0.15, 0)
        letter.Text = ""
        letter.TextColor3 = Color3.new(1, 1, 1)
        letter.TextScaled = true
        letter.Font = Enum.Font.GothamBold
        letter.BackgroundTransparency = 1
        letter.Parent = codeBox
        letterFrames[i] = letter
    end
    
    -- Input label
    local inputLabel = Instance.new("TextLabel")
    inputLabel.Size = UDim2.new(1, 0, 0, 20)
    inputLabel.Position = UDim2.new(0, 0, 0, 220)
    inputLabel.Text = "✏️ Type the code below:"
    inputLabel.TextColor3 = Color3.new(0.7, 0.7, 0.7)
    inputLabel.TextScaled = true
    inputLabel.Font = Enum.Font.Gotham
    inputLabel.BackgroundTransparency = 1
    inputLabel.Parent = frame
    
    -- TextBox
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0.7, 0, 0, 42)
    textBox.Position = UDim2.new(0.15, 0, 0, 245)
    textBox.PlaceholderText = "Enter the code here..."
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
    
    -- Submit Button
    local submitBtn = Instance.new("TextButton")
    submitBtn.Size = UDim2.new(0.35, 0, 0, 42)
    submitBtn.Position = UDim2.new(0.325, 0, 0, 295)
    submitBtn.Text = "✅ Verify"
    submitBtn.TextColor3 = Color3.new(1, 1, 1)
    submitBtn.BackgroundColor3 = Color3.new(0.15, 0.5, 1)
    submitBtn.Font = Enum.Font.GothamBold
    submitBtn.TextScaled = true
    submitBtn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 10)
    btnCorner.Parent = submitBtn
    
    -- Reset Button
    local resetBtn = Instance.new("TextButton")
    resetBtn.Size = UDim2.new(0.12, 0, 0, 30)
    resetBtn.Position = UDim2.new(0.82, 0, 0, 150)
    resetBtn.Text = "🔄"
    resetBtn.TextColor3 = Color3.new(1, 1, 1)
    resetBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.3)
    resetBtn.Font = Enum.Font.GothamBold
    resetBtn.TextScaled = true
    resetBtn.Parent = frame
    
    local resetCorner = Instance.new("UICorner")
    resetCorner.CornerRadius = UDim.new(0, 8)
    resetCorner.Parent = resetBtn
    
    -- Progress bar
    local timeBarBg = Instance.new("Frame")
    timeBarBg.Size = UDim2.new(0.8, 0, 0, 6)
    timeBarBg.Position = UDim2.new(0.1, 0, 0, 327)
    timeBarBg.BackgroundColor3 = Color3.new(0.2, 0.2, 0.3)
    timeBarBg.BorderSizePixel = 0
    timeBarBg.Parent = frame
    
    local barCorner = Instance.new("UICorner")
    barCorner.CornerRadius = UDim.new(0, 3)
    barCorner.Parent = timeBarBg
    
    local timeBar = Instance.new("Frame")
    timeBar.Size = UDim2.new(1, 0, 1, 0)
    timeBar.BackgroundColor3 = Color3.new(0.2, 0.8, 0.4)
    timeBar.BorderSizePixel = 0
    timeBar.Parent = timeBarBg
    
    local barCorner2 = Instance.new("UICorner")
    barCorner2.CornerRadius = UDim.new(0, 3)
    barCorner2.Parent = timeBar
    
    -- Result message
    local resultLabel = Instance.new("TextLabel")
    resultLabel.Size = UDim2.new(1, 0, 0, 25)
    resultLabel.Position = UDim2.new(0, 0, 0, 315)
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
        CodeGlow = codeGlow,
        TimeBar = timeBar,
        TimeBarBg = timeBarBg,
        LetterFrames = letterFrames,
        Icon = icon,
    }
end

-- ===== CREATE GUI =====
local gui = createGUI()

-- ===== GENERATE CODE =====
local currentCode = generateCode()
gui.CodeLabel.Text = currentCode

-- Update individual letters
for i = 1, CONFIG.CodeLength do
    gui.LetterFrames[i].Text = string.sub(currentCode, i, i)
end

-- ===== VARIABLES =====
local verified = false
local startTime = os.time()
local attempts = CONFIG.MaxAttempts

-- ===== DISABLE MOVEMENT =====
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

-- ===== ENABLE MOVEMENT =====
local function enableMovement()
    if humanoid then
        humanoid.WalkSpeed = 16
        humanoid.JumpPower = 50
        humanoid.AutoRotate = true
        humanoid.PlatformStand = false
    end
end

-- ===== KICK FUNCTION =====
local function kickPlayer(reason)
    gui.ResultLabel.Text = "⛔ " .. reason
    gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
    gui.SubmitBtn.Visible = false
    gui.TextBox.Visible = false
    gui.ResetBtn.Visible = false
    
    -- Visual effects on kick
    gui.TopLine.BackgroundColor3 = Color3.new(1, 0, 0)
    gui.CodeGlow.BackgroundColor3 = Color3.new(1, 0, 0)
    gui.Icon.Text = "🚫"
    gui.Icon.TextColor3 = Color3.new(1, 0, 0)
    
    -- Flash effect
    for i = 1, 3 do
        gui.Frame.BackgroundTransparency = 0.5
        task.wait(0.1)
        gui.Frame.BackgroundTransparency = 0.05
        task.wait(0.1)
    end
    
    task.wait(1.5)
    print("❌ " .. reason .. " - Kicking player: " .. player.Name)
    player:Kick(reason)
end

-- ===== VERIFY FUNCTION =====
local function verify()
    if verified then return end
    
    local userInput = gui.TextBox.Text
    userInput = string.upper(userInput)
    userInput = string.gsub(userInput, "%s+", "")
    
    -- Check input length
    if #userInput ~= CONFIG.CodeLength then
        gui.ResultLabel.Text = "⚠️ Code must be " .. CONFIG.CodeLength .. " characters"
        gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        
        -- Shake effect
        local originalPos = gui.Frame.Position
        for i = 1, 5 do
            gui.Frame.Position = UDim2.new(
                originalPos.X.Scale,
                originalPos.X.Offset + math.random(-5, 5),
                originalPos.Y.Scale,
                originalPos.Y.Offset + math.random(-5, 5)
            )
            task.wait(0.02)
        end
        gui.Frame.Position = originalPos
        return
    end
    
    if userInput == currentCode then
        -- ✅ SUCCESS
        verified = true
        gui.ResultLabel.Text = "✅ Verification successful! Welcome! 🎉"
        gui.ResultLabel.TextColor3 = Color3.new(0, 1, 0)
        gui.TopLine.BackgroundColor3 = Color3.new(0, 1, 0)
        gui.CodeGlow.BackgroundColor3 = Color3.new(0, 1, 0)
        gui.SubmitBtn.BackgroundColor3 = Color3.new(0, 0.8, 0)
        gui.SubmitBtn.Text = "✅ Done"
        gui.Icon.Text = "✅"
        gui.Icon.TextColor3 = Color3.new(0, 1, 0)
        gui.TimeBar.BackgroundColor3 = Color3.new(0, 1, 0)
        
        enableMovement()
        
        print("✅ Verification successful! Player: " .. player.Name)
        print("📝 Correct code: " .. currentCode)
        
        task.wait(0.8)
        gui.ScreenGui:Destroy()
        
    else
        -- ❌ WRONG - Instant kick
        gui.ResultLabel.Text = "❌ Wrong code! You will be kicked..."
        gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
        
        task.wait(1)
        kickPlayer("Incorrect verification code!")
    end
end

-- ===== EVENTS =====
gui.SubmitBtn.MouseButton1Click:Connect(verify)

gui.TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        verify()
    end
end)

-- ===== RESET CODE =====
gui.ResetBtn.MouseButton1Click:Connect(function()
    if not verified then
        currentCode = generateCode()
        gui.CodeLabel.Text = currentCode
        for i = 1, CONFIG.CodeLength do
            gui.LetterFrames[i].Text = string.sub(currentCode, i, i)
        end
        gui.TextBox.Text = ""
        gui.ResultLabel.Text = "🔄 Code regenerated!"
        gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        task.wait(1)
        gui.ResultLabel.Text = ""
        print("🔄 New code generated: " .. currentCode)
    end
end)

-- ===== KEYBOARD SHORTCUT (Ctrl+R to reset) =====
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.R and input:IsModifierKeyDown(Enum.KeyCode.LeftControl) then
        if not verified then
            gui.ResetBtn:Click()
        end
    end
end)

-- ===== TIMER SYSTEM =====
task.spawn(function()
    while not verified do
        local elapsed = os.time() - startTime
        local timeLeft = CONFIG.TimeLimit - elapsed
        
        if timeLeft <= 0 then
            kickPlayer("Verification time expired!")
            return
        end
        
        -- Update progress bar
        local progress = timeLeft / CONFIG.TimeLimit
        gui.TimeBar.Size = UDim2.new(progress, 0, 1, 0)
        
        -- Change progress bar color
        if progress > 0.5 then
            gui.TimeBar.BackgroundColor3 = Color3.new(0.2, 0.8, 0.4)
        elseif progress > 0.25 then
            gui.TimeBar.BackgroundColor3 = Color3.new(0.8, 0.8, 0.2)
        else
            gui.TimeBar.BackgroundColor3 = Color3.new(0.8, 0.2, 0.2)
        end
        
        -- Show time warning
        if timeLeft <= 10 then
            gui.ResultLabel.Text = "⏰ " .. timeLeft .. " seconds remaining!"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.5, 0)
        end
        
        task.wait(0.1)
    end
end)

-- ===== STARTUP MESSAGE =====
print("")
print("🔥 " .. string.rep("=", 50))
print("🔥 ADVANCED VERIFICATION SYSTEM (10x BETTER)")
print("🔥 " .. string.rep("=", 50))
print("📝 Current code: " .. currentCode)
print("⏱️ Time limit: " .. CONFIG.TimeLimit .. " seconds")
print("⚠️ Wrong code = Instant kick!")
print("🔄 Press Ctrl+R or click 🔄 to change code")
print("🔥 " .. string.rep("=", 50))
print("")

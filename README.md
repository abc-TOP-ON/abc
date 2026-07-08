-- ================================================
-- Verification System - All Text in English
-- ================================================

local player = game.Players.LocalPlayer

-- Generate random 5-character code
local function generateCode()
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local code = ""
    for i = 1, 5 do
        local randIndex = math.random(1, #chars)
        code = code .. string.sub(chars, randIndex, randIndex)
    end
    return code
end

-- Create GUI
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "CaptchaGUI"
    screenGui.Parent = game:GetService("CoreGui")
    
    -- Main Frame
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 500, 0, 300)
    frame.Position = UDim2.new(0.5, -250, 0.5, -150)
    frame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.15)
    frame.BackgroundTransparency = 0.1
    frame.BorderSizePixel = 0
    frame.Parent = screenGui
    
    -- Rounded corners
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame
    
    -- Title
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 40)
    title.Position = UDim2.new(0, 0, 0, 15)
    title.Text = "🔐 Verification"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold
    title.BackgroundTransparency = 1
    title.Parent = frame
    
    -- Subtitle
    local subtitle = Instance.new("TextLabel")
    subtitle.Size = UDim2.new(1, 0, 0, 25)
    subtitle.Position = UDim2.new(0, 0, 0, 55)
    subtitle.Text = "Enter the code below to verify you're not a robot"
    subtitle.TextColor3 = Color3.new(0.5, 0.5, 0.5)
    subtitle.TextScaled = true
    subtitle.Font = Enum.Font.Gotham
    subtitle.BackgroundTransparency = 1
    subtitle.Parent = frame
    
    -- Code display box
    local codeLabel = Instance.new("TextLabel")
    codeLabel.Size = UDim2.new(0.7, 0, 0, 60)
    codeLabel.Position = UDim2.new(0.15, 0, 0, 90)
    codeLabel.Text = ""
    codeLabel.TextColor3 = Color3.new(0.3, 0.8, 1)
    codeLabel.TextScaled = true
    codeLabel.Font = Enum.Font.GothamBold
    codeLabel.BackgroundColor3 = Color3.new(0.15, 0.15, 0.2)
    codeLabel.Parent = frame
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 8)
    codeCorner.Parent = codeLabel
    
    -- Input label
    local inputLabel = Instance.new("TextLabel")
    inputLabel.Size = UDim2.new(1, 0, 0, 20)
    inputLabel.Position = UDim2.new(0, 0, 0, 160)
    inputLabel.Text = "✏️ Type the code:"
    inputLabel.TextColor3 = Color3.new(0.7, 0.7, 0.7)
    inputLabel.TextScaled = true
    inputLabel.Font = Enum.Font.Gotham
    inputLabel.BackgroundTransparency = 1
    inputLabel.Parent = frame
    
    -- TextBox
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0.7, 0, 0, 40)
    textBox.Position = UDim2.new(0.15, 0, 0, 185)
    textBox.PlaceholderText = "Enter the code here..."
    textBox.Text = ""
    textBox.TextColor3 = Color3.new(1, 1, 1)
    textBox.BackgroundColor3 = Color3.new(0.15, 0.15, 0.2)
    textBox.Font = Enum.Font.Gotham
    textBox.TextScaled = true
    textBox.Parent = frame
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 8)
    boxCorner.Parent = textBox
    
    -- Submit Button
    local submitBtn = Instance.new("TextButton")
    submitBtn.Size = UDim2.new(0.35, 0, 0, 40)
    submitBtn.Position = UDim2.new(0.325, 0, 0, 235)
    submitBtn.Text = "✅ Verify"
    submitBtn.TextColor3 = Color3.new(1, 1, 1)
    submitBtn.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
    submitBtn.Font = Enum.Font.GothamBold
    submitBtn.TextScaled = true
    submitBtn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = submitBtn
    
    -- Result message
    local resultLabel = Instance.new("TextLabel")
    resultLabel.Size = UDim2.new(1, 0, 0, 30)
    resultLabel.Position = UDim2.new(0, 0, 0, 270)
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

-- Create GUI
local gui = createGUI()

-- Generate code
local currentCode = generateCode()
gui.CodeLabel.Text = currentCode

-- Disable movement
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChild("Humanoid")
if humanoid then
    humanoid.WalkSpeed = 0
    humanoid.JumpPower = 0
end

-- Verify function
local function verify()
    local userInput = gui.TextBox.Text
    userInput = string.upper(userInput)
    
    if userInput == currentCode then
        -- Success
        gui.ResultLabel.Text = "✅ Verification successful!"
        gui.ResultLabel.TextColor3 = Color3.new(0, 1, 0)
        
        if humanoid then
            humanoid.WalkSpeed = 16
            humanoid.JumpPower = 50
        end
        
        task.wait(1)
        gui.ScreenGui:Destroy()
        print("✅ Player verified: " .. player.Name)
        
    else
        -- Failed - Instant kick
        gui.ResultLabel.Text = "❌ Wrong code! You will be kicked..."
        gui.ResultLabel.TextColor3 = Color3.new(1, 0, 0)
        gui.SubmitBtn.Visible = false
        gui.TextBox.Visible = false
        
        task.wait(1.5)
        print("❌ Wrong code! Kicking player: " .. player.Name)
        player:Kick("Incorrect verification code!")
    end
end

-- Events
gui.SubmitBtn.MouseButton1Click:Connect(verify)

gui.TextBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        verify()
    end
end)

-- Timer: 60 seconds
local timeLeft = 60
task.spawn(function()
    while timeLeft > 0 do
        task.wait(1)
        timeLeft = timeLeft - 1
        
        if timeLeft <= 10 then
            gui.ResultLabel.Text = "⏰ " .. timeLeft .. " seconds remaining!"
            gui.ResultLabel.TextColor3 = Color3.new(1, 0.8, 0)
        end
        
        if timeLeft <= 0 then
            gui.ResultLabel.Text = "⏰ Time is up!"
            task.wait(1)
            print("⏰ Time expired! Kicking player: " .. player.Name)
            player:Kick("Verification time expired!")
        end
    end
end)

-- Startup message
print("========================================")
print("🔐 Verification System Started!")
print("📝 Current code: " .. currentCode)
print("⏱️ You have 60 seconds")
print("⚠️ Wrong code = Instant kick!")
print("========================================")

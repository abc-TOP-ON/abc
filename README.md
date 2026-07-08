--[[
    Advanced Human Verification System - Mobile Optimized
    Supports: Random Code Generation with Multiple Types
    Features: Numbers Only, Letters Only, Alphanumeric, Symbols, Pronounceable, Segmented, Hex, No Similar Chars
--]]

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ==========================================
-- ====== RANDOM CODE GENERATOR ======
-- ==========================================

local CodeGenerator = {}

-- ===== 1. Generate Random Code (Basic) =====
function CodeGenerator.generateRandomCode(length, options)
    options = options or {}
    local useNumbers = options.numbers ~= false
    local useUppercase = options.uppercase ~= false
    local useLowercase = options.lowercase or false
    local useSymbols = options.symbols or false
    local excludeSimilar = options.excludeSimilar or false
    local readable = options.readable or false
    
    local chars = ""
    
    if useNumbers then
        chars = chars .. "0123456789"
    end
    
    if useUppercase then
        chars = chars .. "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    end
    
    if useLowercase then
        chars = chars .. "abcdefghijklmnopqrstuvwxyz"
    end
    
    if useSymbols then
        chars = chars .. "!@#$%^&*()_+-=[]{}|;:,.<>?"
    end
    
    if excludeSimilar then
        chars = chars:gsub("[0Oo1IiLl]", "")
    end
    
    if readable then
        chars = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789"
    end
    
    if chars == "" then
        chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    end
    
    local code = ""
    for i = 1, length do
        local randomIndex = math.random(1, #chars)
        code = code .. string.sub(chars, randomIndex, randomIndex)
    end
    
    return code
end

-- ===== 2. Generate UUID Style Code =====
function CodeGenerator.generateUUID()
    local template = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx"
    
    local function randomHex()
        return string.format("%x", math.random(0, 15))
    end
    
    local uuid = string.gsub(template, "[xy]", function(c)
        local r = math.random(0, 15)
        if c == "x" then
            return string.format("%x", r)
        else
            return string.format("%x", math.random(8, 11))
        end
    end)
    
    return string.upper(uuid)
end

-- ===== 3. Generate Strong Password =====
function CodeGenerator.generatePassword(length)
    local chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()"
    local password = ""
    
    local hasUpper = false
    local hasLower = false
    local hasNumber = false
    local hasSymbol = false
    
    while not (hasUpper and hasLower and hasNumber and hasSymbol) do
        password = ""
        hasUpper = false
        hasLower = false
        hasNumber = false
        hasSymbol = false
        
        for i = 1, length do
            local randomIndex = math.random(1, #chars)
            local char = string.sub(chars, randomIndex, randomIndex)
            password = password .. char
            
            if string.match(char, "%u") then hasUpper = true end
            if string.match(char, "%l") then hasLower = true end
            if string.match(char, "%d") then hasNumber = true end
            if string.match(char, "[%p]") then hasSymbol = true end
        end
    end
    
    return password
end

-- ===== 4. Generate Pronounceable Code =====
function CodeGenerator.generatePronounceable(length)
    local vowels = "AEIOU"
    local consonants = "BCDFGHJKLMNPQRSTVWXYZ"
    local code = ""
    
    for i = 1, length do
        if i % 2 == 1 then
            local idx = math.random(1, #consonants)
            code = code .. string.sub(consonants, idx, idx)
        else
            local idx = math.random(1, #vowels)
            code = code .. string.sub(vowels, idx, idx)
        end
    end
    
    return code
end

-- ===== 5. Generate Numeric Only Code =====
function CodeGenerator.generateNumeric(length)
    local code = ""
    for i = 1, length do
        code = code .. math.random(0, 9)
    end
    return code
end

-- ===== 6. Generate Hexadecimal Code =====
function CodeGenerator.generateHex(length)
    local chars = "0123456789ABCDEF"
    local code = ""
    for i = 1, length do
        local idx = math.random(1, #chars)
        code = code .. string.sub(chars, idx, idx)
    end
    return code
end

-- ===== 7. Generate Segmented Code (e.g., X7K-LM4-P2D) =====
function CodeGenerator.generateSegmentedCode(parts, partLength)
    parts = parts or 3
    partLength = partLength or 4
    local segments = {}
    
    for i = 1, parts do
        local segment = CodeGenerator.generateRandomCode(partLength, {
            numbers = true,
            uppercase = true,
            lowercase = false,
            excludeSimilar = true
        })
        table.insert(segments, segment)
    end
    
    return table.concat(segments, "-")
end

-- ==========================================
-- ====== MOBILE OPTIMIZED UI ======
-- ==========================================

local function createMobileVerificationUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "VerificationGUI"
    screenGui.Parent = playerGui
    screenGui.ResetOnSpawn = false
    screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    -- Background Overlay
    local overlay = Instance.new("Frame")
    overlay.Name = "Overlay"
    overlay.Parent = screenGui
    overlay.Size = UDim2.new(1, 0, 1, 0)
    overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    overlay.BackgroundTransparency = 0.7
    overlay.BorderSizePixel = 0

    -- Main Frame - Smaller for Mobile
    local frame = Instance.new("Frame")
    frame.Name = "MainFrame"
    frame.Parent = overlay
    frame.Size = UDim2.new(0, 350, 0, 420)
    frame.Position = UDim2.new(0.5, -175, 0.5, -210)
    frame.BackgroundColor3 = Color3.fromRGB(20, 22, 35)
    frame.BorderSizePixel = 0
    frame.ClipsDescendants = true

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 16)
    corner.Parent = frame

    -- Title Bar - Smaller
    local titleBar = Instance.new("Frame")
    titleBar.Parent = frame
    titleBar.Size = UDim2.new(1, 0, 0, 45)
    titleBar.BackgroundColor3 = Color3.fromRGB(40, 45, 80)
    titleBar.BorderSizePixel = 0
    
    local titleBarCorner = Instance.new("UICorner")
    titleBarCorner.CornerRadius = UDim.new(0, 16)
    titleBarCorner.Parent = titleBar

    -- Title
    local title = Instance.new("TextLabel")
    title.Parent = titleBar
    title.Size = UDim2.new(1, 0, 1, 0)
    title.BackgroundTransparency = 1
    title.Text = "🔐 Verify"
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.TextScaled = true
    title.Font = Enum.Font.GothamBold

    -- Small Icon
    local icon = Instance.new("TextLabel")
    icon.Parent = frame
    icon.Size = UDim2.new(0, 50, 0, 50)
    icon.Position = UDim2.new(0.5, -25, 0, 55)
    icon.BackgroundTransparency = 1
    icon.Text = "🤖"
    icon.TextSize = 35
    icon.TextColor3 = Color3.fromRGB(255, 200, 0)

    -- Code Display Container - Smaller
    local codeContainer = Instance.new("Frame")
    codeContainer.Parent = frame
    codeContainer.Size = UDim2.new(0.85, 0, 0, 55)
    codeContainer.Position = UDim2.new(0.075, 0, 0, 115)
    codeContainer.BackgroundColor3 = Color3.fromRGB(30, 35, 55)
    codeContainer.BorderSizePixel = 2
    codeContainer.BorderColor3 = Color3.fromRGB(0, 200, 255)

    local codeContainerCorner = Instance.new("UICorner")
    codeContainerCorner.CornerRadius = UDim.new(0, 10)
    codeContainerCorner.Parent = codeContainer

    -- Code Display
    local codeDisplay = Instance.new("TextLabel")
    codeDisplay.Parent = codeContainer
    codeDisplay.Size = UDim2.new(1, 0, 1, 0)
    codeDisplay.BackgroundTransparency = 1
    codeDisplay.Text = "----"
    codeDisplay.TextColor3 = Color3.fromRGB(255, 255, 255)
    codeDisplay.TextSize = 32
    codeDisplay.Font = Enum.Font.GothamBold
    codeDisplay.TextScaled = true

    -- Code Type Label - Smaller
    local codeTypeLabel = Instance.new("TextLabel")
    codeTypeLabel.Parent = frame
    codeTypeLabel.Size = UDim2.new(0.85, 0, 0, 20)
    codeTypeLabel.Position = UDim2.new(0.075, 0, 0, 175)
    codeTypeLabel.BackgroundTransparency = 1
    codeTypeLabel.Text = "🔑 Alphanumeric"
    codeTypeLabel.TextColor3 = Color3.fromRGB(150, 160, 200)
    codeTypeLabel.TextSize = 12
    codeTypeLabel.Font = Enum.Font.Gotham
    codeTypeLabel.TextXAlignment = Enum.TextXAlignment.Center

    -- Instruction Label - Smaller
    local instruction = Instance.new("TextLabel")
    instruction.Parent = frame
    instruction.Size = UDim2.new(0.9, 0, 0, 25)
    instruction.Position = UDim2.new(0.05, 0, 0, 200)
    instruction.BackgroundTransparency = 1
    instruction.Text = "Enter the code"
    instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
    instruction.TextSize = 15
    instruction.Font = Enum.Font.Gotham
    instruction.TextXAlignment = Enum.TextXAlignment.Center

    -- Input Box - Optimized for Mobile
    local inputBox = Instance.new("TextBox")
    inputBox.Parent = frame
    inputBox.Size = UDim2.new(0.8, 0, 0, 45)
    inputBox.Position = UDim2.new(0.1, 0, 0, 235)
    inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 60)
    inputBox.Text = ""
    inputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    inputBox.TextSize = 28
    inputBox.PlaceholderText = "Enter code..."
    inputBox.PlaceholderColor3 = Color3.fromRGB(100, 110, 150)
    inputBox.Font = Enum.Font.GothamBold
    inputBox.ClearTextOnFocus = false
    inputBox.TextXAlignment = Enum.TextXAlignment.Center
    inputBox.AutomaticSize = Enum.AutomaticSize.None

    local inputCorner = Instance.new("UICorner")
    inputCorner.CornerRadius = UDim.new(0, 10)
    inputCorner.Parent = inputBox
    
    -- Auto uppercase
    inputBox:GetPropertyChangedSignal("Text"):Connect(function()
        inputBox.Text = string.upper(inputBox.Text)
    end)

    -- Buttons Row - Smaller
    -- Confirm Button
    local confirmBtn = Instance.new("TextButton")
    confirmBtn.Parent = frame
    confirmBtn.Size = UDim2.new(0.35, 0, 0, 40)
    confirmBtn.Position = UDim2.new(0.075, 0, 0, 295)
    confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 120)
    confirmBtn.Text = "✅ Confirm"
    confirmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    confirmBtn.TextScaled = true
    confirmBtn.Font = Enum.Font.GothamBold

    local confirmCorner = Instance.new("UICorner")
    confirmCorner.CornerRadius = UDim.new(0, 8)
    confirmCorner.Parent = confirmBtn

    -- Refresh Button
    local refreshBtn = Instance.new("TextButton")
    refreshBtn.Parent = frame
    refreshBtn.Size = UDim2.new(0.25, 0, 0, 40)
    refreshBtn.Position = UDim2.new(0.45, 0, 0, 295)
    refreshBtn.BackgroundColor3 = Color3.fromRGB(60, 65, 100)
    refreshBtn.Text = "🔄"
    refreshBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    refreshBtn.TextScaled = true
    refreshBtn.Font = Enum.Font.GothamBold

    local refreshCorner = Instance.new("UICorner")
    refreshCorner.CornerRadius = UDim.new(0, 8)
    refreshCorner.Parent = refreshBtn

    -- Change Type Button
    local changeTypeBtn = Instance.new("TextButton")
    changeTypeBtn.Parent = frame
    changeTypeBtn.Size = UDim2.new(0.25, 0, 0, 40)
    changeTypeBtn.Position = UDim2.new(0.72, 0, 0, 295)
    changeTypeBtn.BackgroundColor3 = Color3.fromRGB(100, 50, 150)
    changeTypeBtn.Text = "🎲"
    changeTypeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    changeTypeBtn.TextScaled = true
    changeTypeBtn.Font = Enum.Font.GothamBold

    local changeTypeCorner = Instance.new("UICorner")
    changeTypeCorner.CornerRadius = UDim.new(0, 8)
    changeTypeCorner.Parent = changeTypeBtn

    -- Strength Indicator - Smaller
    local strengthBar = Instance.new("Frame")
    strengthBar.Parent = frame
    strengthBar.Size = UDim2.new(0.7, 0, 0, 6)
    strengthBar.Position = UDim2.new(0.15, 0, 0, 350)
    strengthBar.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
    strengthBar.BorderSizePixel = 0
    
    local strengthBarCorner = Instance.new("UICorner")
    strengthBarCorner.CornerRadius = UDim.new(0, 3)
    strengthBarCorner.Parent = strengthBar

    local strengthFill = Instance.new("Frame")
    strengthFill.Parent = strengthBar
    strengthFill.Size = UDim2.new(1, 0, 1, 0)
    strengthFill.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
    strengthFill.BorderSizePixel = 0
    
    local strengthFillCorner = Instance.new("UICorner")
    strengthFillCorner.CornerRadius = UDim.new(0, 3)
    strengthFillCorner.Parent = strengthFill

    local strengthLabel = Instance.new("TextLabel")
    strengthLabel.Parent = frame
    strengthLabel.Size = UDim2.new(0.7, 0, 0, 18)
    strengthLabel.Position = UDim2.new(0.15, 0, 0, 358)
    strengthLabel.BackgroundTransparency = 1
    strengthLabel.Text = "🔒 Strong"
    strengthLabel.TextColor3 = Color3.fromRGB(150, 200, 150)
    strengthLabel.TextSize = 11
    strengthLabel.Font = Enum.Font.Gotham
    strengthLabel.TextXAlignment = Enum.TextXAlignment.Center

    -- Close Button (X) - Mobile friendly
    local closeBtn = Instance.new("TextButton")
    closeBtn.Parent = titleBar
    closeBtn.Size = UDim2.new(0, 35, 0, 35)
    closeBtn.Position = UDim2.new(1, -40, 0.5, -17.5)
    closeBtn.BackgroundTransparency = 1
    closeBtn.Text = "✕"
    closeBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
    closeBtn.TextSize = 20
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.BorderSizePixel = 0

    return screenGui, frame, codeDisplay, codeTypeLabel, inputBox, confirmBtn, refreshBtn, changeTypeBtn, instruction, strengthFill, strengthLabel, closeBtn, overlay
end

-- ==========================================
-- ====== MAIN VERIFICATION SYSTEM ======
-- ==========================================

local function startMobileVerification()
    local screenGui, frame, codeDisplay, codeTypeLabel, inputBox, confirmBtn, refreshBtn, changeTypeBtn, instruction, strengthFill, strengthLabel, closeBtn, overlay = createMobileVerificationUI()
    
    -- ===== Available Code Types =====
    local codeTypes = {
        {
            name = "🔢 Numbers",
            generator = function() return CodeGenerator.generateNumeric(6) end,
            strength = 0.4,
            strengthText = "🟡 Medium"
        },
        {
            name = "🔤 Letters",
            generator = function() return CodeGenerator.generateRandomCode(6, {numbers = false, uppercase = true}) end,
            strength = 0.5,
            strengthText = "🟡 Medium"
        },
        {
            name = "🔑 Alphanumeric",
            generator = function() return CodeGenerator.generateRandomCode(6, {numbers = true, uppercase = true}) end,
            strength = 0.7,
            strengthText = "🟢 Strong"
        },
        {
            name = "🔐 Secure",
            generator = function() return CodeGenerator.generateRandomCode(6, {numbers = true, uppercase = true, symbols = true}) end,
            strength = 1.0,
            strengthText = "🟣 Very Strong"
        },
        {
            name = "🗣️ Pronounceable",
            generator = function() return CodeGenerator.generatePronounceable(6) end,
            strength = 0.5,
            strengthText = "🟡 Medium"
        },
        {
            name = "📦 Segmented",
            generator = function() return CodeGenerator.generateSegmentedCode(2, 4) end,
            strength = 0.8,
            strengthText = "🟢 Strong"
        },
        {
            name = "🔢 Hex",
            generator = function() return CodeGenerator.generateHex(6) end,
            strength = 0.6,
            strengthText = "🟢 Good"
        },
        {
            name = "🎯 No Similar",
            generator = function() return CodeGenerator.generateRandomCode(6, {numbers = true, uppercase = true, excludeSimilar = true}) end,
            strength = 0.75,
            strengthText = "🟢 Strong"
        }
    }
    
    -- ===== Variables =====
    local currentTypeIndex = 3
    local currentCode = ""
    local attempts = 0
    local maxAttempts = 4
    
    -- ===== Update Strength Indicator =====
    local function updateStrength(strengthValue, text)
        strengthFill.Size = UDim2.new(strengthValue, 0, 1, 0)
        
        if strengthValue >= 0.7 then
            strengthFill.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
            strengthLabel.Text = "🔒 " .. text
            strengthLabel.TextColor3 = Color3.fromRGB(150, 255, 150)
        elseif strengthValue >= 0.4 then
            strengthFill.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
            strengthLabel.Text = "🔓 " .. text
            strengthLabel.TextColor3 = Color3.fromRGB(255, 220, 100)
        else
            strengthFill.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            strengthLabel.Text = "⚠️ " .. text
            strengthLabel.TextColor3 = Color3.fromRGB(255, 150, 150)
        end
    end
    
    -- ===== Generate New Code =====
    local function generateNewCode()
        local typeData = codeTypes[currentTypeIndex]
        currentCode = typeData.generator()
        
        codeDisplay.Text = currentCode
        codeTypeLabel.Text = "🔑 " .. typeData.name
        
        updateStrength(typeData.strength, typeData.strengthText)
        
        -- Flash effect
        codeDisplay.TextColor3 = Color3.fromRGB(255, 255, 0)
        task.wait(0.15)
        codeDisplay.TextColor3 = Color3.fromRGB(255, 255, 255)
        
        inputBox.Text = ""
        inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 60)
        instruction.Text = "Enter the code"
        instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
        
        print("🔑 New Code (" .. typeData.name .. "): " .. currentCode)
    end
    
    -- ===== Change Code Type =====
    local function changeCodeType()
        currentTypeIndex = currentTypeIndex + 1
        if currentTypeIndex > #codeTypes then
            currentTypeIndex = 1
        end
        
        generateNewCode()
        attempts = 0
        
        instruction.Text = "🔄 New code type!"
        instruction.TextColor3 = Color3.fromRGB(0, 200, 255)
        
        -- Button flash effect
        changeTypeBtn.BackgroundColor3 = Color3.fromRGB(150, 50, 200)
        task.wait(0.2)
        changeTypeBtn.BackgroundColor3 = Color3.fromRGB(100, 50, 150)
    end
    
    -- ===== Verify Code =====
    local function verifyCode()
        local userInput = inputBox.Text
        
        if userInput == "" then
            instruction.Text = "⚠️ Enter the code!"
            instruction.TextColor3 = Color3.fromRGB(255, 200, 0)
            inputBox.BackgroundColor3 = Color3.fromRGB(200, 150, 0)
            task.wait(0.5)
            inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 60)
            instruction.Text = "Enter the code"
            instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
            return
        end
        
        -- Remove dashes for segmented codes
        local cleanInput = string.gsub(userInput, "-", "")
        local cleanCode = string.gsub(currentCode, "-", "")
        
        if cleanInput == cleanCode then
            -- ✅ Success
            inputBox.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
            instruction.Text = "✅ Verified! You are human ✅"
            instruction.TextColor3 = Color3.fromRGB(0, 255, 0)
            confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
            
            print("✅ Verification Successful! Code: " .. currentCode)
            
            -- Success animation
            for i = 1, 3 do
                codeDisplay.TextColor3 = Color3.fromRGB(0, 255, 0)
                task.wait(0.1)
                codeDisplay.TextColor3 = Color3.fromRGB(255, 255, 255)
                task.wait(0.1)
            end
            
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "✅ Verified",
                Text = "You are human! 🎉",
                Duration = 2,
                Icon = "rbxassetid://8739636080"
            })
            
            task.wait(0.6)
            screenGui:Destroy()
            
            -- Run your code here after verification
            
        else
            -- ❌ Failed
            attempts = attempts + 1
            local remaining = maxAttempts - attempts
            
            inputBox.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            instruction.Text = "❌ Wrong! " .. remaining .. " left"
            instruction.TextColor3 = Color3.fromRGB(255, 0, 0)
            
            print("❌ Incorrect Code! Input: " .. userInput .. " | Correct: " .. currentCode)
            
            -- Shake animation
            local originalPos = frame.Position
            for i = 1, 4 do
                frame.Position = UDim2.new(0.5, -175 + (i % 2 == 0 and 10 or -10), 0.5, -210)
                task.wait(0.03)
            end
            frame.Position = originalPos
            
            task.wait(0.5)
            inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 60)
            
            if attempts >= maxAttempts then
                -- Max attempts reached
                instruction.Text = "🔒 Max attempts! New code..."
                instruction.TextColor3 = Color3.fromRGB(255, 100, 100)
                confirmBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
                
                task.wait(1.2)
                changeCodeType()
                attempts = 0
                confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 120)
            else
                instruction.Text = "Enter the code"
                instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
            end
        end
    end
    
    -- ===== Button Connections =====
    confirmBtn.MouseButton1Click:Connect(verifyCode)
    refreshBtn.MouseButton1Click:Connect(generateNewCode)
    changeTypeBtn.MouseButton1Click:Connect(changeCodeType)
    closeBtn.MouseButton1Click:Connect(function()
        screenGui:Destroy()
    end)
    
    -- ===== Enter Key =====
    inputBox.FocusLost:Connect(function(enterPressed)
        if enterPressed then
            verifyCode()
        end
    end)
    
    -- ===== Touch outside to dismiss keyboard =====
    overlay.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            inputBox:ReleaseFocus()
        end
    end)
    
    -- ===== Generate Initial Code =====
    generateNewCode()
    
    -- ===== Draggable Window =====
    local dragging = false
    local dragStart = nil
    local startPos = nil
    local touchOffset = nil
    
    titleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            touchOffset = Vector2.new(
                frame.AbsolutePosition.X - input.Position.X,
                frame.AbsolutePosition.Y - input.Position.Y
            )
        end
    end)
    
    titleBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.Touch then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(
                0.5, -175 + delta.X + (touchOffset and touchOffset.X or 0),
                0.5, -210 + delta.Y + (touchOffset and touchOffset.Y or 0)
            )
        end
    end)
    
    return screenGui
end

-- ==========================================
-- ====== START SCRIPT ======
-- ==========================================

-- Start the mobile verification
startMobileVerification()

-- ==========================================
-- ====== ADDITIONAL PROTECTIONS ======
-- ==========================================

-- Prevent Paste (Ctrl+V / Cmd+V)
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.V and 
       (game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftControl) or
        game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.RightControl) or
        game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftCommand) or
        game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.RightCommand)) then
        
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "⚠️ Not Allowed",
            Text = "Type the code manually!",
            Duration = 2
        })
        return
    end
end)

print("✅ Mobile Verification System Loaded Successfully!")

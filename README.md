--[[
    ╔══════════════════════════════════════════════════════════════╗
    ║         ULTIMATE VERIFICATION SYSTEM v3.0                  ║
    ║        10× BETTER - Premium Quality Script                 ║
    ║        Optimized for Mobile & PC                           ║
    ╚══════════════════════════════════════════════════════════════╝
--]]

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local userInputService = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local tweenService = game:GetService("TweenService")
local starterGui = game:GetService("StarterGui")

-- ==========================================
-- ====== PREMIUM CODE GENERATOR ======
-- ==========================================

local CodeGenerator = {}

-- Advanced character sets
local CHAR_SETS = {
    NUMBERS = "0123456789",
    UPPERCASE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    LOWERCASE = "abcdefghijklmnopqrstuvwxyz",
    SYMBOLS = "!@#$%^&*()_+-=[]{}|;:,.<>?",
    SAFE = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789",
    HEX = "0123456789ABCDEF",
    VOWELS = "AEIOU",
    CONSONANTS = "BCDFGHJKLMNPQRSTVWXYZ"
}

function CodeGenerator.generate(length, options)
    options = options or {}
    local charset = options.charset or CHAR_SETS.UPPERCASE .. CHAR_SETS.NUMBERS
    
    local code = ""
    for i = 1, length do
        local idx = math.random(1, #charset)
        code = code .. string.sub(charset, idx, idx)
    end
    return code
end

function CodeGenerator.generateSecure(length)
    local charset = CHAR_SETS.UPPERCASE .. CHAR_SETS.NUMBERS .. CHAR_SETS.SYMBOLS
    local code = ""
    
    -- Ensure at least one of each type
    local hasUpper = false
    local hasNumber = false
    local hasSymbol = false
    
    while not (hasUpper and hasNumber and hasSymbol) do
        code = ""
        hasUpper = false
        hasNumber = false
        hasSymbol = false
        
        for i = 1, length do
            local idx = math.random(1, #charset)
            local char = string.sub(charset, idx, idx)
            code = code .. char
            
            if string.match(char, "%u") then hasUpper = true end
            if string.match(char, "%d") then hasNumber = true end
            if string.match(char, "[%p]") then hasSymbol = true end
        end
    end
    
    return code
end

function CodeGenerator.generatePronounceable(length)
    local code = ""
    for i = 1, length do
        if i % 2 == 1 then
            local idx = math.random(1, #CHAR_SETS.CONSONANTS)
            code = code .. string.sub(CHAR_SETS.CONSONANTS, idx, idx)
        else
            local idx = math.random(1, #CHAR_SETS.VOWELS)
            code = code .. string.sub(CHAR_SETS.VOWELS, idx, idx)
        end
    end
    return code
end

function CodeGenerator.generateSegmented(parts, partLength)
    parts = parts or 3
    partLength = partLength or 4
    local segments = {}
    
    for i = 1, parts do
        local segment = CodeGenerator.generate(partLength, {
            charset = CHAR_SETS.UPPERCASE .. CHAR_SETS.NUMBERS
        })
        table.insert(segments, segment)
    end
    
    return table.concat(segments, "-")
end

function CodeGenerator.generatePIN(length)
    local code = ""
    for i = 1, length do
        code = code .. math.random(0, 9)
    end
    return code
end

-- ==========================================
-- ====== PREMIUM ANIMATION SYSTEM ======
-- ==========================================

local AnimationSystem = {}

function AnimationSystem.createTween(object, properties, duration, style)
    style = style or Enum.EasingStyle.Quad
    local tweenInfo = TweenInfo.new(
        duration or 0.3,
        style,
        Enum.EasingDirection.Out
    )
    local tween = tweenService:Create(object, tweenInfo, properties)
    return tween
end

function AnimationSystem.shake(frame, intensity, duration)
    intensity = intensity or 10
    duration = duration or 0.5
    local originalPos = frame.Position
    
    local shakeTween = tweenService:Create(frame, TweenInfo.new(0.05, Enum.EasingStyle.Linear), {
        Position = UDim2.new(originalPos.X.Scale, originalPos.X.Offset + intensity, 
                            originalPos.Y.Scale, originalPos.Y.Offset)
    })
    
    for i = 1, math.floor(duration / 0.05) do
        intensity = intensity * (1 - (i / (duration / 0.05)) * 0.8)
        local offset = (i % 2 == 0) and intensity or -intensity
        shakeTween = tweenService:Create(frame, TweenInfo.new(0.05, Enum.EasingStyle.Linear), {
            Position = UDim2.new(originalPos.X.Scale, originalPos.X.Offset + offset,
                                originalPos.Y.Scale, originalPos.Y.Offset)
        })
        shakeTween:Play()
        shakeTween.Completed:Wait()
    end
    
    tweenService:Create(frame, TweenInfo.new(0.1, Enum.EasingStyle.Quad), {
        Position = originalPos
    }):Play()
end

function AnimationSystem.pulse(object, sizeMultiplier, duration)
    sizeMultiplier = sizeMultiplier or 1.1
    duration = duration or 0.3
    
    local originalSize = object.Size
    local tween1 = tweenService:Create(object, TweenInfo.new(duration / 2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
        Size = UDim2.new(originalSize.X.Scale * sizeMultiplier, originalSize.X.Offset * sizeMultiplier,
                        originalSize.Y.Scale * sizeMultiplier, originalSize.Y.Offset * sizeMultiplier)
    })
    local tween2 = tweenService:Create(object, TweenInfo.new(duration / 2, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
        Size = originalSize
    })
    
    tween1:Play()
    tween1.Completed:Connect(function()
        tween2:Play()
    end)
end

function AnimationSystem.fadeIn(object, duration)
    duration = duration or 0.3
    object.BackgroundTransparency = 1
    object.Visible = true
    
    local tween = tweenService:Create(object, TweenInfo.new(duration, Enum.EasingStyle.Quad), {
        BackgroundTransparency = 0
    })
    tween:Play()
    return tween
end

function AnimationSystem.fadeOut(object, duration)
    duration = duration or 0.3
    local tween = tweenService:Create(object, TweenInfo.new(duration, Enum.EasingStyle.Quad), {
        BackgroundTransparency = 1
    })
    tween:Play()
    return tween
end

-- ==========================================
-- ====== PREMIUM UI BUILDER ======
-- ==========================================

local UIBuilder = {}

function UIBuilder.createLabel(parent, text, size, position, color, fontSize)
    local label = Instance.new("TextLabel")
    label.Parent = parent
    label.Size = size
    label.Position = position
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = color or Color3.fromRGB(255, 255, 255)
    label.TextSize = fontSize or 14
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Center
    label.TextYAlignment = Enum.TextYAlignment.Center
    label.TextScaled = false
    return label
end

function UIBuilder.createButton(parent, text, size, position, color, textColor)
    local button = Instance.new("TextButton")
    button.Parent = parent
    button.Size = size
    button.Position = position
    button.BackgroundColor3 = color or Color3.fromRGB(60, 65, 100)
    button.Text = text
    button.TextColor3 = textColor or Color3.fromRGB(255, 255, 255)
    button.TextScaled = true
    button.Font = Enum.Font.GothamBold
    button.BorderSizePixel = 0
    button.AutoButtonColor = true
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button
    
    return button
end

function UIBuilder.createFrame(parent, size, position, color, transparency)
    local frame = Instance.new("Frame")
    frame.Parent = parent
    frame.Size = size
    frame.Position = position
    frame.BackgroundColor3 = color or Color3.fromRGB(30, 35, 55)
    frame.BackgroundTransparency = transparency or 0
    frame.BorderSizePixel = 0
    frame.ClipsDescendants = true
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame
    
    return frame
end

-- ==========================================
-- ====== MAIN VERIFICATION SYSTEM ======
-- ==========================================

local VerificationSystem = {}
VerificationSystem.__index = VerificationSystem

function VerificationSystem.new()
    local self = setmetatable({}, VerificationSystem)
    
    self.screenGui = Instance.new("ScreenGui")
    self.screenGui.Name = "VerificationGUI"
    self.screenGui.Parent = playerGui
    self.screenGui.ResetOnSpawn = false
    self.screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    -- Variables
    self.currentTypeIndex = 1
    self.currentCode = ""
    self.attempts = 0
    self.maxAttempts = 4
    self.isVerified = false
    self.isAnimating = false
    
    -- Code types
    self.codeTypes = {
        {
            name = "🔢 PIN",
            generator = function() return CodeGenerator.generatePIN(6) end,
            strength = 0.4,
            strengthText = "🟡 Medium",
            color = Color3.fromRGB(255, 200, 0)
        },
        {
            name = "🔤 Letters",
            generator = function() return CodeGenerator.generate(6, {charset = CHAR_SETS.UPPERCASE}) end,
            strength = 0.5,
            strengthText = "🟡 Medium",
            color = Color3.fromRGB(100, 200, 255)
        },
        {
            name = "🔑 Alphanumeric",
            generator = function() return CodeGenerator.generate(6, {charset = CHAR_SETS.UPPERCASE .. CHAR_SETS.NUMBERS}) end,
            strength = 0.7,
            strengthText = "🟢 Strong",
            color = Color3.fromRGB(0, 200, 100)
        },
        {
            name = "🔐 Secure",
            generator = function() return CodeGenerator.generateSecure(8) end,
            strength = 1.0,
            strengthText = "🟣 Very Strong",
            color = Color3.fromRGB(200, 100, 255)
        },
        {
            name = "🗣️ Readable",
            generator = function() return CodeGenerator.generatePronounceable(6) end,
            strength = 0.5,
            strengthText = "🟡 Medium",
            color = Color3.fromRGB(255, 200, 100)
        },
        {
            name = "📦 Segmented",
            generator = function() return CodeGenerator.generateSegmented(3, 4) end,
            strength = 0.8,
            strengthText = "🟢 Strong",
            color = Color3.fromRGB(100, 200, 200)
        },
        {
            name = "🟣 Hex",
            generator = function() return CodeGenerator.generate(6, {charset = CHAR_SETS.HEX}) end,
            strength = 0.6,
            strengthText = "🟢 Good",
            color = Color3.fromRGB(255, 150, 255)
        }
    }
    
    self:buildUI()
    self:setupEvents()
    self:generateNewCode()
    
    return self
end

function VerificationSystem:buildUI()
    -- Background Overlay with blur effect
    self.overlay = Instance.new("Frame")
    self.overlay.Parent = self.screenGui
    self.overlay.Size = UDim2.new(1, 0, 1, 0)
    self.overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    self.overlay.BackgroundTransparency = 0.6
    self.overlay.BorderSizePixel = 0
    
    -- Main Frame
    self.frame = UIBuilder.createFrame(self.overlay, 
        UDim2.new(0, 380, 0, 440),
        UDim2.new(0.5, -190, 0.5, -220),
        Color3.fromRGB(25, 28, 45),
        0
    )
    
    -- Glow effect
    self.glow = Instance.new("Frame")
    self.glow.Parent = self.frame
    self.glow.Size = UDim2.new(1.02, 0, 1.02, 0)
    self.glow.Position = UDim2.new(-0.01, 0, -0.01, 0)
    self.glow.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
    self.glow.BackgroundTransparency = 0.9
    self.glow.BorderSizePixel = 0
    self.glow.ZIndex = -1
    
    local glowCorner = Instance.new("UICorner")
    glowCorner.CornerRadius = UDim.new(0, 16)
    glowCorner.Parent = self.glow
    
    -- Title Bar with gradient
    self.titleBar = Instance.new("Frame")
    self.titleBar.Parent = self.frame
    self.titleBar.Size = UDim2.new(1, 0, 0, 50)
    self.titleBar.BackgroundColor3 = Color3.fromRGB(40, 45, 85)
    self.titleBar.BorderSizePixel = 0
    
    local titleBarCorner = Instance.new("UICorner")
    titleBarCorner.CornerRadius = UDim.new(0, 16)
    titleBarCorner.Parent = self.titleBar
    
    -- Title
    self.title = UIBuilder.createLabel(self.titleBar, "🔐 Security Verification", 
        UDim2.new(1, -40, 1, 0),
        UDim2.new(0, 0, 0, 0),
        Color3.fromRGB(255, 255, 255),
        20
    )
    self.title.Font = Enum.Font.GothamBold
    
    -- Close Button
    self.closeBtn = Instance.new("TextButton")
    self.closeBtn.Parent = self.titleBar
    self.closeBtn.Size = UDim2.new(0, 35, 0, 35)
    self.closeBtn.Position = UDim2.new(1, -40, 0.5, -17.5)
    self.closeBtn.BackgroundTransparency = 1
    self.closeBtn.Text = "✕"
    self.closeBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
    self.closeBtn.TextSize = 20
    self.closeBtn.Font = Enum.Font.GothamBold
    self.closeBtn.BorderSizePixel = 0
    
    self.closeBtn.MouseButton1Click:Connect(function()
        self:close()
    end)
    
    -- Icon with pulse animation
    self.icon = UIBuilder.createLabel(self.frame, "🛡️", 
        UDim2.new(0, 60, 0, 60),
        UDim2.new(0.5, -30, 0, 60),
        Color3.fromRGB(255, 200, 0),
        45
    )
    
    -- Code Container with dynamic border
    self.codeContainer = Instance.new("Frame")
    self.codeContainer.Parent = self.frame
    self.codeContainer.Size = UDim2.new(0.85, 0, 0, 60)
    self.codeContainer.Position = UDim2.new(0.075, 0, 0, 130)
    self.codeContainer.BackgroundColor3 = Color3.fromRGB(30, 35, 60)
    self.codeContainer.BorderSizePixel = 2
    self.codeContainer.BorderColor3 = Color3.fromRGB(0, 200, 255)
    
    local codeContainerCorner = Instance.new("UICorner")
    codeContainerCorner.CornerRadius = UDim.new(0, 12)
    codeContainerCorner.Parent = self.codeContainer
    
    -- Code Display
    self.codeDisplay = Instance.new("TextLabel")
    self.codeDisplay.Parent = self.codeContainer
    self.codeDisplay.Size = UDim2.new(1, 0, 1, 0)
    self.codeDisplay.BackgroundTransparency = 1
    self.codeDisplay.Text = "----"
    self.codeDisplay.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.codeDisplay.TextSize = 36
    self.codeDisplay.Font = Enum.Font.GothamBold
    self.codeDisplay.TextScaled = true
    self.codeDisplay.TextXAlignment = Enum.TextXAlignment.Center
    self.codeDisplay.TextYAlignment = Enum.TextYAlignment.Center
    
    -- Copy button on code
    self.copyBtn = Instance.new("TextButton")
    self.copyBtn.Parent = self.codeContainer
    self.copyBtn.Size = UDim2.new(0, 30, 0, 30)
    self.copyBtn.Position = UDim2.new(1, -35, 0.5, -15)
    self.copyBtn.BackgroundTransparency = 1
    self.copyBtn.Text = "📋"
    self.copyBtn.TextSize = 16
    self.copyBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
    self.copyBtn.BorderSizePixel = 0
    
    self.copyBtn.MouseButton1Click:Connect(function()
        if self.currentCode then
            setclipboard(self.currentCode)
            starterGui:SetCore("SendNotification", {
                Title = "📋 Copied!",
                Text = "Code copied to clipboard",
                Duration = 2
            })
        end
    end)
    
    -- Code Type Label
    self.codeTypeLabel = UIBuilder.createLabel(self.frame, "🔑 Alphanumeric",
        UDim2.new(0.85, 0, 0, 22),
        UDim2.new(0.075, 0, 0, 195),
        Color3.fromRGB(150, 160, 200),
        13
    )
    
    -- Instruction
    self.instruction = UIBuilder.createLabel(self.frame, "✍️ Enter the code shown above",
        UDim2.new(0.9, 0, 0, 25),
        UDim2.new(0.05, 0, 0, 222),
        Color3.fromRGB(200, 200, 220),
        16
    )
    self.instruction.Font = Enum.Font.Gotham
    
    -- Input Box with shadow
    self.inputContainer = Instance.new("Frame")
    self.inputContainer.Parent = self.frame
    self.inputContainer.Size = UDim2.new(0.8, 0, 0, 48)
    self.inputContainer.Position = UDim2.new(0.1, 0, 0, 255)
    self.inputContainer.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    self.inputContainer.BackgroundTransparency = 0.3
    self.inputContainer.BorderSizePixel = 0
    
    local inputContainerCorner = Instance.new("UICorner")
    inputContainerCorner.CornerRadius = UDim.new(0, 12)
    inputContainerCorner.Parent = self.inputContainer
    
    self.inputBox = Instance.new("TextBox")
    self.inputBox.Parent = self.inputContainer
    self.inputBox.Size = UDim2.new(1, -4, 1, -4)
    self.inputBox.Position = UDim2.new(0, 2, 0, 2)
    self.inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 65)
    self.inputBox.Text = ""
    self.inputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.inputBox.TextSize = 28
    self.inputBox.PlaceholderText = "Enter code..."
    self.inputBox.PlaceholderColor3 = Color3.fromRGB(100, 110, 160)
    self.inputBox.Font = Enum.Font.GothamBold
    self.inputBox.ClearTextOnFocus = false
    self.inputBox.TextXAlignment = Enum.TextXAlignment.Center
    self.inputBox.ClipsDescendants = true
    
    local inputCorner = Instance.new("UICorner")
    inputCorner.CornerRadius = UDim.new(0, 10)
    inputCorner.Parent = self.inputBox
    
    -- Auto uppercase
    self.inputBox:GetPropertyChangedSignal("Text"):Connect(function()
        self.inputBox.Text = string.upper(self.inputBox.Text)
    end)
    
    -- Button Row
    self.confirmBtn = UIBuilder.createButton(self.frame, "✅ Confirm",
        UDim2.new(0.32, 0, 0, 44),
        UDim2.new(0.075, 0, 0, 315),
        Color3.fromRGB(0, 200, 120),
        Color3.fromRGB(255, 255, 255)
    )
    self.confirmBtn.Font = Enum.Font.GothamBold
    
    self.refreshBtn = UIBuilder.createButton(self.frame, "🔄",
        UDim2.new(0.25, 0, 0, 44),
        UDim2.new(0.42, 0, 0, 315),
        Color3.fromRGB(60, 65, 110),
        Color3.fromRGB(255, 255, 255)
    )
    
    self.changeTypeBtn = UIBuilder.createButton(self.frame, "🎲",
        UDim2.new(0.25, 0, 0, 44),
        UDim2.new(0.70, 0, 0, 315),
        Color3.fromRGB(120, 60, 180),
        Color3.fromRGB(255, 255, 255)
    )
    
    -- Strength Indicator
    self.strengthBar = Instance.new("Frame")
    self.strengthBar.Parent = self.frame
    self.strengthBar.Size = UDim2.new(0.7, 0, 0, 6)
    self.strengthBar.Position = UDim2.new(0.15, 0, 0, 375)
    self.strengthBar.BackgroundColor3 = Color3.fromRGB(50, 50, 80)
    self.strengthBar.BorderSizePixel = 0
    
    local strengthBarCorner = Instance.new("UICorner")
    strengthBarCorner.CornerRadius = UDim.new(0, 3)
    strengthBarCorner.Parent = self.strengthBar
    
    self.strengthFill = Instance.new("Frame")
    self.strengthFill.Parent = self.strengthBar
    self.strengthFill.Size = UDim2.new(1, 0, 1, 0)
    self.strengthFill.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
    self.strengthFill.BorderSizePixel = 0
    
    local strengthFillCorner = Instance.new("UICorner")
    strengthFillCorner.CornerRadius = UDim.new(0, 3)
    strengthFillCorner.Parent = self.strengthFill
    
    self.strengthLabel = UIBuilder.createLabel(self.frame, "🔒 Strong",
        UDim2.new(0.7, 0, 0, 20),
        UDim2.new(0.15, 0, 0, 383),
        Color3.fromRGB(150, 200, 150),
        12
    )
    
    -- Attempts counter
    self.attemptsLabel = UIBuilder.createLabel(self.frame, "Attempts: 0/4",
        UDim2.new(0.7, 0, 0, 18),
        UDim2.new(0.15, 0, 0, 407),
        Color3.fromRGB(150, 150, 180),
        11
    )
    
    -- Success overlay (hidden)
    self.successOverlay = Instance.new("Frame")
    self.successOverlay.Parent = self.frame
    self.successOverlay.Size = UDim2.new(1, 0, 1, 0)
    self.successOverlay.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    self.successOverlay.BackgroundTransparency = 1
    self.successOverlay.BorderSizePixel = 0
    self.successOverlay.Visible = false
    
    local successCorner = Instance.new("UICorner")
    successCorner.CornerRadius = UDim.new(0, 16)
    successCorner.Parent = self.successOverlay
    
    self.successIcon = UIBuilder.createLabel(self.successOverlay, "✅",
        UDim2.new(0, 80, 0, 80),
        UDim2.new(0.5, -40, 0.3, -40),
        Color3.fromRGB(255, 255, 255),
        60
    )
    
    self.successText = UIBuilder.createLabel(self.successOverlay, "VERIFIED!",
        UDim2.new(0.9, 0, 0, 40),
        UDim2.new(0.05, 0, 0.6, 0),
        Color3.fromRGB(255, 255, 255),
        32
    )
    self.successText.Font = Enum.Font.GothamBold
end

function VerificationSystem:setupEvents()
    -- Confirm button
    self.confirmBtn.MouseButton1Click:Connect(function()
        self:verifyCode()
    end)
    
    -- Refresh button
    self.refreshBtn.MouseButton1Click:Connect(function()
        self:generateNewCode()
        AnimationSystem.pulse(self.codeContainer, 1.05, 0.2)
    end)
    
    -- Change type button
    self.changeTypeBtn.MouseButton1Click:Connect(function()
        self:changeCodeType()
        AnimationSystem.pulse(self.changeTypeBtn, 1.1, 0.2)
    end)
    
    -- Enter key
    self.inputBox.FocusLost:Connect(function(enterPressed)
        if enterPressed then
            self:verifyCode()
        end
    end)
    
    -- Tap outside to dismiss keyboard
    self.overlay.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            self.inputBox:ReleaseFocus()
        end
    end)
    
    -- Draggable window
    local dragging = false
    local dragStart = nil
    local startPos = nil
    
    self.titleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or 
           input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = self.frame.Position
        end
    end)
    
    self.titleBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or 
           input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    userInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.Touch or 
                        input.UserInputType == Enum.UserInputType.MouseMovement) then
            local delta = input.Position - dragStart
            self.frame.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)
end

function VerificationSystem:updateStrength(strengthValue, text)
    self.strengthFill.Size = UDim2.new(strengthValue, 0, 1, 0)
    
    if strengthValue >= 0.7 then
        self.strengthFill.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        self.strengthLabel.Text = "🔒 " .. text
        self.strengthLabel.TextColor3 = Color3.fromRGB(150, 255, 150)
        self.codeContainer.BorderColor3 = Color3.fromRGB(0, 200, 100)
    elseif strengthValue >= 0.4 then
        self.strengthFill.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
        self.strengthLabel.Text = "🔓 " .. text
        self.strengthLabel.TextColor3 = Color3.fromRGB(255, 220, 100)
        self.codeContainer.BorderColor3 = Color3.fromRGB(255, 200, 0)
    else
        self.strengthFill.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
        self.strengthLabel.Text = "⚠️ " .. text
        self.strengthLabel.TextColor3 = Color3.fromRGB(255, 150, 150)
        self.codeContainer.BorderColor3 = Color3.fromRGB(255, 50, 50)
    end
end

function VerificationSystem:generateNewCode()
    local typeData = self.codeTypes[self.currentTypeIndex]
    self.currentCode = typeData.generator()
    
    -- Animate code change
    local tween = tweenService:Create(self.codeDisplay, TweenInfo.new(0.1, Enum.EasingStyle.Quad), {
        TextColor3 = Color3.fromRGB(255, 255, 0)
    })
    tween:Play()
    tween.Completed:Connect(function()
        self.codeDisplay.Text = self.currentCode
        tweenService:Create(self.codeDisplay, TweenInfo.new(0.1, Enum.EasingStyle.Quad), {
            TextColor3 = Color3.fromRGB(255, 255, 255)
        }):Play()
    end)
    
    self.codeTypeLabel.Text = "🔑 " .. typeData.name
    self:updateStrength(typeData.strength, typeData.strengthText)
    
    self.inputBox.Text = ""
    self.inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 65)
    self.instruction.Text = "✍️ Enter the code shown above"
    self.instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
    
    print("🔑 New Code: " .. self.currentCode)
end

function VerificationSystem:changeCodeType()
    self.currentTypeIndex = self.currentTypeIndex + 1
    if self.currentTypeIndex > #self.codeTypes then
        self.currentTypeIndex = 1
    end
    
    self:generateNewCode()
    self.attempts = 0
    self:updateAttempts()
    
    self.instruction.Text = "🔄 New code type generated!"
    self.instruction.TextColor3 = Color3.fromRGB(0, 200, 255)
    
    AnimationSystem.pulse(self.codeContainer, 1.08, 0.3)
end

function VerificationSystem:updateAttempts()
    self.attemptsLabel.Text = "Attempts: " .. self.attempts .. "/" .. self.maxAttempts
    
    if self.attempts >= self.maxAttempts then
        self.attemptsLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
    else
        self.attemptsLabel.TextColor3 = Color3.fromRGB(150, 150, 180)
    end
end

function VerificationSystem:verifyCode()
    if self.isVerified or self.isAnimating then return end
    
    local userInput = self.inputBox.Text
    
    if userInput == "" then
        self.instruction.Text = "⚠️ Please enter the code!"
        self.instruction.TextColor3 = Color3.fromRGB(255, 200, 0)
        self.inputBox.BackgroundColor3 = Color3.fromRGB(200, 150, 0)
        AnimationSystem.shake(self.inputContainer, 5, 0.3)
        task.wait(0.5)
        self.inputBox.BackgroundColor3 = Color3.fromRGB(35, 38, 65)
        self.instruction.Text = "✍️ Enter the code shown above"
        self.instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
        return
    end
    
    -- Remove dashes
    local cleanInput = string.gsub(userInput, "-", "")
    local cleanCode = string.gsub(self.currentCode, "-", "")
    
    if cleanInput == cleanCode then
        -- ✅ SUCCESS
        self.isVerified = true
        self.isAnimating = true
        
        self.inputBox.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        self.instruction.Text = "✅ VERIFIED! You are human ✅"
        self.instruction.TextColor3 = Color3.fromRGB(0, 255, 0)
        self.confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        
        print("✅ Verification Successful! Code: " .. self.currentCode)
        
        -- Success animation
        self.successOverlay.Visible = true
        self.successOverlay.BackgroundTransparency = 1
        
        local tween1 = tweenService:Create(self.successOverlay, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {
            BackgroundTransparency = 0.2
        })
        tween1:Play()
        
        -- Pulse icon
        AnimationSystem.pulse(self.successIcon, 1.2, 0.5)
        
        -- Show notification
        starterGui:SetCore("SendNotification", {
            Title = "✅ Verification Successful",
            Text = "You have been verified as human! 🎉",
            Duration = 3,
            Icon = "rbxassetid://8739636080"
        })
        
        task.wait(0.8)
        
        -- Fade out and close
        local tween2 = tweenService:Create(self.overlay, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {
            BackgroundTransparency = 1
        })
        tween2:Play()
        
        local tween3 = tweenService:Create(self.frame, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {
            Size = UDim2.new(0, 0, 0, 0)
        })
        tween3:Play()
        
        tween3.Completed:Connect(function()
            self:close()
        end)
        
        -- 🔥 RUN YOUR CODE HERE AFTER VERIFICATION 🔥
        -- Example:
        -- game:GetService("ReplicatedStorage"):FindFirstChild("VerifyEvent"):FireServer()
        -- Or enable features, teleport, etc.
        
    else
        -- ❌ FAILED
        self.attempts = self.attempts + 1
        self:updateAttempts()
        
        local remaining = self.maxAttempts - self.attempts
        
        self.inputBox.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
        self.instruction.Text = "❌ Incorrect! " .. remaining .. " attempt(s) left"
        self.instruction.TextColor3 = Color3.fromRGB(255, 0, 0)
        
        print("❌ Incorrect Code! Input: " .. userInput .. " | Correct: " .. self.currentCode)
        
        -- Shake animation

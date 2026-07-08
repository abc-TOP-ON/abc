--[[
    ╔══════════════════════════════════════════════════════════════════════════════╗
    ║                    💎 ULTIMATE VERIFICATION SYSTEM v4.0 💎                  ║
    ║                        100× BETTER - GOD TIER                              ║
    ║              Premium Features • Advanced Security • Perfect UI             ║
    ╚══════════════════════════════════════════════════════════════════════════════╝
--]]

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local userInputService = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local tweenService = game:GetService("TweenService")
local starterGui = game:GetService("StarterGui")
local lighting = game:GetService("Lighting")
local soundService = game:GetService("SoundService")

-- ==========================================
-- ====== 🎵 SOUND SYSTEM ======
-- ==========================================

local SoundSystem = {}

function SoundSystem.createSound(id, volume)
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://" .. tostring(id)
    sound.Volume = volume or 0.5
    sound.Parent = soundService
    return sound
end

function SoundSystem.playSuccess()
    local sound = SoundSystem.createSound(9120384075, 0.4)
    sound:Play()
    task.wait(sound.TimeLength or 1)
    sound:Destroy()
end

function SoundSystem.playError()
    local sound = SoundSystem.createSound(9120384075, 0.3)
    sound.PlaybackSpeed = 0.7
    sound:Play()
    task.wait(sound.TimeLength or 0.5)
    sound:Destroy()
end

function SoundSystem.playClick()
    local sound = SoundSystem.createSound(9120384075, 0.2)
    sound.PlaybackSpeed = 1.5
    sound:Play()
    task.wait(0.1)
    sound:Destroy()
end

function SoundSystem.playVerified()
    local sound = SoundSystem.createSound(9120384075, 0.6)
    sound.PlaybackSpeed = 0.5
    sound:Play()
    task.wait(sound.TimeLength or 1.5)
    sound:Destroy()
end

-- ==========================================
-- ====== 🌟 PREMIUM ANIMATION ENGINE ======
-- ==========================================

local AnimationEngine = {}

function AnimationEngine.createTween(object, properties, duration, style, direction, repeatCount)
    style = style or Enum.EasingStyle.Quad
    direction = direction or Enum.EasingDirection.Out
    repeatCount = repeatCount or 0
    
    local tweenInfo = TweenInfo.new(
        duration or 0.3,
        style,
        direction,
        repeatCount or 0,
        false,
        0
    )
    local tween = tweenService:Create(object, tweenInfo, properties)
    return tween
end

function AnimationEngine.shake(frame, intensity, duration, repeatCount)
    intensity = intensity or 12
    duration = duration or 0.6
    repeatCount = repeatCount or 1
    local originalPos = frame.Position
    
    local tween = AnimationEngine.createTween(frame, {
        Position = UDim2.new(originalPos.X.Scale, originalPos.X.Offset + intensity,
                            originalPos.Y.Scale, originalPos.Y.Offset)
    }, 0.05, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut)
    
    local count = repeatCount or 6
    for i = 1, count do
        local offset = (i % 2 == 0) and intensity or -intensity
        local progress = i / count
        intensity = intensity * (1 - progress * 0.7)
        
        tween = AnimationEngine.createTween(frame, {
            Position = UDim2.new(originalPos.X.Scale, originalPos.X.Offset + offset * (1 - progress * 0.5),
                                originalPos.Y.Scale, originalPos.Y.Offset)
        }, 0.05, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut)
        tween:Play()
        tween.Completed:Wait()
    end
    
    AnimationEngine.createTween(frame, {
        Position = originalPos
    }, 0.1):Play()
end

function AnimationEngine.pulse(object, scale, duration, repeatCount)
    scale = scale or 1.1
    duration = duration or 0.3
    repeatCount = repeatCount or 1
    local originalSize = object.Size
    
    for i = 1, repeatCount do
        local tween1 = AnimationEngine.createTween(object, {
            Size = UDim2.new(originalSize.X.Scale * scale, originalSize.X.Offset * scale,
                            originalSize.Y.Scale * scale, originalSize.Y.Offset * scale)
        }, duration / 2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
        tween1:Play()
        tween1.Completed:Wait()
        
        local tween2 = AnimationEngine.createTween(object, {
            Size = originalSize
        }, duration / 2, Enum.EasingStyle.Quad, Enum.EasingDirection.In)
        tween2:Play()
        tween2.Completed:Wait()
    end
end

function AnimationEngine.fadeIn(object, duration)
    duration = duration or 0.3
    object.Visible = true
    object.BackgroundTransparency = 1
    
    local tween = AnimationEngine.createTween(object, {
        BackgroundTransparency = 0
    }, duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    tween:Play()
    return tween
end

function AnimationEngine.fadeOut(object, duration)
    duration = duration or 0.3
    
    local tween = AnimationEngine.createTween(object, {
        BackgroundTransparency = 1
    }, duration, Enum.EasingStyle.Quad, Enum.EasingDirection.In)
    tween:Play()
    tween.Completed:Connect(function()
        object.Visible = false
    end)
    return tween
end

function AnimationEngine.scaleIn(object, duration)
    duration = duration or 0.3
    object.Size = UDim2.new(0, 0, 0, 0)
    local targetSize = object:GetAttribute("TargetSize") or object.Size
    
    local tween = AnimationEngine.createTween(object, {
        Size = targetSize
    }, duration, Enum.EasingStyle.Back, Enum.EasingDirection.Out)
    tween:Play()
    return tween
end

function AnimationEngine.rotate(object, degrees, duration)
    duration = duration or 0.5
    local tween = AnimationEngine.createTween(object, {
        Rotation = degrees
    }, duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    tween:Play()
    return tween
end

function AnimationEngine.bounce(object, height, duration)
    height = height or 20
    duration = duration or 0.6
    local originalPos = object.Position
    
    local tween1 = AnimationEngine.createTween(object, {
        Position = UDim2.new(originalPos.X.Scale, originalPos.X.Offset,
                            originalPos.Y.Scale, originalPos.Y.Offset - height)
    }, duration * 0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    tween1:Play()
    tween1.Completed:Wait()
    
    local tween2 = AnimationEngine.createTween(object, {
        Position = originalPos
    }, duration * 0.6, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out)
    tween2:Play()
    return tween2
end

-- ==========================================
-- ====== 🎨 ADVANCED COLOR SYSTEM ======
-- ==========================================

local ColorSystem = {}

function ColorSystem.hexToRGB(hex)
    hex = hex:gsub("#", "")
    return Color3.fromRGB(
        tonumber("0x" .. hex:sub(1, 2)) or 255,
        tonumber("0x" .. hex:sub(3, 4)) or 255,
        tonumber("0x" .. hex:sub(5, 6)) or 255
    )
end

function ColorSystem.lerp(color1, color2, t)
    return Color3.new(
        color1.R + (color2.R - color1.R) * t,
        color1.G + (color2.G - color1.G) * t,
        color1.B + (color2.B - color1.B) * t
    )
end

function ColorSystem.random()
    return Color3.fromRGB(
        math.random(0, 255),
        math.random(0, 255),
        math.random(0, 255)
    )
end

function ColorSystem.isLight(color)
    local brightness = (color.R * 0.299 + color.G * 0.587 + color.B * 0.114)
    return brightness > 0.5
end

-- ==========================================
-- ====== 🔐 ADVANCED CODE GENERATOR ======
-- ==========================================

local CodeGenerator = {}

-- Character sets
local CHARS = {
    NUMBERS = "0123456789",
    UPPERCASE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    LOWERCASE = "abcdefghijklmnopqrstuvwxyz",
    SYMBOLS = "!@#$%^&*()_+-=[]{}|;:,.<>?",
    SAFE = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789",
    HEX = "0123456789ABCDEF",
    BINARY = "01",
    EMOJI = "🔴🟠🟡🟢🔵🟣⚪⚫",
    SPECIAL = "αβγδεζηθικλμνξπρστυφχψω"
}

-- Code type configurations
local CODE_TYPES = {
    {
        id = "pin",
        name = "🔢 PIN Code",
        icon = "🔢",
        generator = function(length) 
            local code = ""
            for i = 1, length do code = code .. math.random(0, 9) end
            return code
        end,
        length = 6,
        strength = 0.3,
        color = Color3.fromRGB(255, 200, 0),
        description = "6-digit numeric code"
    },
    {
        id = "letters",
        name = "🔤 Letters Only",
        icon = "🔤",
        generator = function(length)
            local code = ""
            local chars = CHARS.UPPERCASE
            for i = 1, length do
                local idx = math.random(1, #chars)
                code = code .. string.sub(chars, idx, idx)
            end
            return code
        end,
        length = 6,
        strength = 0.4,
        color = Color3.fromRGB(100, 200, 255),
        description = "6 uppercase letters"
    },
    {
        id = "alphanumeric",
        name = "🔑 Alphanumeric",
        icon = "🔑",
        generator = function(length)
            local code = ""
            local chars = CHARS.UPPERCASE .. CHARS.NUMBERS
            for i = 1, length do
                local idx = math.random(1, #chars)
                code = code .. string.sub(chars, idx, idx)
            end
            return code
        end,
        length = 6,
        strength = 0.6,
        color = Color3.fromRGB(0, 200, 100),
        description = "Mix of letters and numbers"
    },
    {
        id = "secure",
        name = "🔐 Secure Code",
        icon = "🔐",
        generator = function(length)
            local chars = CHARS.UPPERCASE .. CHARS.NUMBERS .. CHARS.SYMBOLS
            local code = ""
            local hasUpper = false
            local hasNumber = false
            local hasSymbol = false
            
            while not (hasUpper and hasNumber and hasSymbol) do
                code = ""
                hasUpper = false
                hasNumber = false
                hasSymbol = false
                
                for i = 1, length do
                    local idx = math.random(1, #chars)
                    local char = string.sub(chars, idx, idx)
                    code = code .. char
                    
                    if string.match(char, "%u") then hasUpper = true end
                    if string.match(char, "%d") then hasNumber = true end
                    if string.match(char, "[%p]") then hasSymbol = true end
                end
            end
            return code
        end,
        length = 8,
        strength = 1.0,
        color = Color3.fromRGB(200, 100, 255),
        description = "Strong with symbols & numbers"
    },
    {
        id = "readable",
        name = "🗣️ Readable",
        icon = "🗣️",
        generator = function(length)
            local vowels = "AEIOU"
            local consonants = "BCDFGHJKLMNPQRSTVWXYZ"
            local code = ""
            for i = 1, length do
                local idx = math.random(1, #(i % 2 == 1 and consonants or vowels))
                code = code .. string.sub(i % 2 == 1 and consonants or vowels, idx, idx)
            end
            return code
        end,
        length = 6,
        strength = 0.45,
        color = Color3.fromRGB(255, 200, 100),
        description = "Easy to read and remember"
    },
    {
        id = "segmented",
        name = "📦 Segmented",
        icon = "📦",
        generator = function(length)
            local parts = 3
            local partLength = 4
            local segments = {}
            local chars = CHARS.UPPERCASE .. CHARS.NUMBERS
            
            for i = 1, parts do
                local segment = ""
                for j = 1, partLength do
                    local idx = math.random(1, #chars)
                    segment = segment .. string.sub(chars, idx, idx)
                end
                table.insert(segments, segment)
            end
            return table.concat(segments, "-")
        end,
        length = 14,
        strength = 0.75,
        color = Color3.fromRGB(100, 200, 200),
        description = "3 groups of 4 characters"
    },
    {
        id = "hex",
        name = "🟣 Hexadecimal",
        icon = "🟣",
        generator = function(length)
            local chars = CHARS.HEX
            local code = ""
            for i = 1, length do
                local idx = math.random(1, #chars)
                code = code .. string.sub(chars, idx, idx)
            end
            return code
        end,
        length = 8,
        strength = 0.55,
        color = Color3.fromRGB(255, 150, 255),
        description = "Hexadecimal characters"
    },
    {
        id = "binary",
        name = "🔢 Binary",
        icon = "🔢",
        generator = function(length)
            local chars = CHARS.BINARY
            local code = ""
            for i = 1, length do
                local idx = math.random(1, #chars)
                code = code .. string.sub(chars, idx, idx)
            end
            return code
        end,
        length = 12,
        strength = 0.35,
        color = Color3.fromRGB(100, 255, 100),
        description = "Binary sequence (0 & 1)"
    },
    {
        id = "emoji",
        name = "😊 Emoji Code",
        icon = "😊",
        generator = function(length)
            local emojis = {"🔴","🟠","🟡","🟢","🔵","🟣","⚪","⚫","❤️","💛","💚","💙","💜","🧡"}
            local code = ""
            for i = 1, length do
                code = code .. emojis[math.random(1, #emojis)]
            end
            return code
        end,
        length = 4,
        strength = 0.5,
        color = Color3.fromRGB(255, 100, 200),
        description = "Emoji sequence (fun!)"
    }
}

function CodeGenerator.generate(typeId, length)
    for _, typeData in ipairs(CODE_TYPES) do
        if typeData.id == typeId then
            return typeData.generator(typeData.length)
        end
    end
    return CODE_TYPES[1].generator(CODE_TYPES[1].length)
end

function CodeGenerator.getAllTypes()
    return CODE_TYPES
end

-- ==========================================
-- ====== 🎯 MAIN VERIFICATION SYSTEM ======
-- ==========================================

local VerificationSystem = {}
VerificationSystem.__index = VerificationSystem

function VerificationSystem.new()
    local self = setmetatable({}, VerificationSystem)
    
    -- Core variables
    self.currentTypeIndex = 3
    self.currentCode = ""
    self.attempts = 0
    self.maxAttempts = 5
    self.isVerified = false
    self.isAnimating = false
    self.isClosing = false
    self.codeHistory = {}
    self.startTime = tick()
    
    -- Create UI
    self:createGUI()
    self:setupEvents()
    self:generateNewCode()
    self:startParticleSystem()
    
    -- Auto-refresh timer
    self.autoRefreshTimer = 0
    self.autoRefreshInterval = 60 -- Refresh code every 60 seconds
    
    runService.Heartbeat:Connect(function(delta)
        self.autoRefreshTimer = self.autoRefreshTimer + delta
        if self.autoRefreshTimer >= self.autoRefreshInterval and not self.isVerified then
            self.autoRefreshTimer = 0
            self:generateNewCode()
            self.instruction.Text = "🔄 Auto-refreshed! New code generated"
            self.instruction.TextColor3 = Color3.fromRGB(0, 200, 255)
            task.wait(1)
            self.instruction.Text = "✍️ Enter the code shown above"
            self.instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
        end
    end)
    
    return self
end

-- ==========================================
-- ====== 🎨 UI CREATION ======
-- ==========================================

function VerificationSystem:createGUI()
    self.screenGui = Instance.new("ScreenGui")
    self.screenGui.Name = "VerificationGUI"
    self.screenGui.Parent = playerGui
    self.screenGui.ResetOnSpawn = false
    self.screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    self.screenGui.IgnoreGuiInset = true
    
    -- === Background ===
    self.background = Instance.new("Frame")
    self.background.Parent = self.screenGui
    self.background.Size = UDim2.new(1, 0, 1, 0)
    self.background.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    self.background.BackgroundTransparency = 0.65
    self.background.BorderSizePixel = 0
    
    -- === Main Container ===
    self.mainFrame = Instance.new("Frame")
    self.mainFrame.Parent = self.background
    self.mainFrame.Size = UDim2.new(0, 420, 0, 500)
    self.mainFrame.Position = UDim2.new(0.5, -210, 0.5, -250)
    self.mainFrame.BackgroundColor3 = Color3.fromRGB(18, 20, 38)
    self.mainFrame.BorderSizePixel = 0
    self.mainFrame.ClipsDescendants = true
    self.mainFrame:SetAttribute("TargetSize", self.mainFrame.Size)
    
    -- Main frame corner
    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 20)
    mainCorner.Parent = self.mainFrame
    
    -- === Glow Border ===
    self.glowBorder = Instance.new("Frame")
    self.glowBorder.Parent = self.mainFrame
    self.glowBorder.Size = UDim2.new(1.02, 0, 1.02, 0)
    self.glowBorder.Position = UDim2.new(-0.01, 0, -0.01, 0)
    self.glowBorder.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
    self.glowBorder.BackgroundTransparency = 0.85
    self.glowBorder.BorderSizePixel = 0
    self.glowBorder.ZIndex = -1
    
    local glowCorner = Instance.new("UICorner")
    glowCorner.CornerRadius = UDim.new(0, 22)
    glowCorner.Parent = self.glowBorder
    
    -- Glow pulse animation
    local glowPulse = AnimationEngine.createTween(self.glowBorder, {
        BackgroundTransparency = 0.8
    }, 1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1)
    glowPulse:Play()
    glowPulse:GetPropertyChangedSignal("BackgroundTransparency"):Connect(function()
        local tween2 = AnimationEngine.createTween(self.glowBorder, {
            BackgroundTransparency = 0.85
        }, 1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1)
        tween2:Play()
    end)
    
    -- === Title Bar ===
    self.titleBar = Instance.new("Frame")
    self.titleBar.Parent = self.mainFrame
    self.titleBar.Size = UDim2.new(1, 0, 0, 55)
    self.titleBar.BackgroundColor3 = Color3.fromRGB(35, 40, 75)
    self.titleBar.BorderSizePixel = 0
    
    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 20)
    titleCorner.Parent = self.titleBar
    
    -- Title
    self.titleText = Instance.new("TextLabel")
    self.titleText.Parent = self.titleBar
    self.titleText.Size = UDim2.new(1, -50, 1, 0)
    self.titleText.Position = UDim2.new(0, 10, 0, 0)
    self.titleText.BackgroundTransparency = 1
    self.titleText.Text = "🔐 Security Verification"
    self.titleText.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.titleText.TextSize = 20
    self.titleText.Font = Enum.Font.GothamBold
    self.titleText.TextXAlignment = Enum.TextXAlignment.Left
    
    -- Close button
    self.closeBtn = Instance.new("TextButton")
    self.closeBtn.Parent = self.titleBar
    self.closeBtn.Size = UDim2.new(0, 35, 0, 35)
    self.closeBtn.Position = UDim2.new(1, -42, 0.5, -17.5)
    self.closeBtn.BackgroundTransparency = 1
    self.closeBtn.Text = "✕"
    self.closeBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
    self.closeBtn.TextSize = 20
    self.closeBtn.Font = Enum.Font.GothamBold
    self.closeBtn.BorderSizePixel = 0
    
    self.closeBtn.MouseButton1Click:Connect(function()
        SoundSystem.playClick()
        self:close()
    end)
    
    -- === Icon ===
    self.iconFrame = Instance.new("Frame")
    self.iconFrame.Parent = self.mainFrame
    self.iconFrame.Size = UDim2.new(0, 65, 0, 65)
    self.iconFrame.Position = UDim2.new(0.5, -32.5, 0, 70)
    self.iconFrame.BackgroundColor3 = Color3.fromRGB(40, 45, 80)
    self.iconFrame.BorderSizePixel = 0
    
    local iconCorner = Instance.new("UICorner")
    iconCorner.CornerRadius = UDim.new(0, 50)
    iconCorner.Parent = self.iconFrame
    
    self.iconText = Instance.new("TextLabel")
    self.iconText.Parent = self.iconFrame
    self.iconText.Size = UDim2.new(1, 0, 1, 0)
    self.iconText.BackgroundTransparency = 1
    self.iconText.Text = "🛡️"
    self.iconText.TextSize = 40
    self.iconText.TextColor3 = Color3.fromRGB(255, 200, 0)
    self.iconText.Font = Enum.Font.GothamBold
    
    -- === Code Display ===
    self.codeContainer = Instance.new("Frame")
    self.codeContainer.Parent = self.mainFrame
    self.codeContainer.Size = UDim2.new(0.85, 0, 0, 65)
    self.codeContainer.Position = UDim2.new(0.075, 0, 0, 145)
    self.codeContainer.BackgroundColor3 = Color3.fromRGB(28, 32, 58)
    self.codeContainer.BorderSizePixel = 2
    self.codeContainer.BorderColor3 = Color3.fromRGB(0, 200, 255)
    
    local codeCorner = Instance.new("UICorner")
    codeCorner.CornerRadius = UDim.new(0, 12)
    codeCorner.Parent = self.codeContainer
    
    -- Code text
    self.codeDisplay = Instance.new("TextLabel")
    self.codeDisplay.Parent = self.codeContainer
    self.codeDisplay.Size = UDim2.new(1, 0, 1, 0)
    self.codeDisplay.BackgroundTransparency = 1
    self.codeDisplay.Text = "----"
    self.codeDisplay.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.codeDisplay.TextSize = 38
    self.codeDisplay.Font = Enum.Font.GothamBold
    self.codeDisplay.TextXAlignment = Enum.TextXAlignment.Center
    self.codeDisplay.TextYAlignment = Enum.TextYAlignment.Center
    
    -- Copy button
    self.copyBtn = Instance.new("TextButton")
    self.copyBtn.Parent = self.codeContainer
    self.copyBtn.Size = UDim2.new(0, 35, 0, 35)
    self.copyBtn.Position = UDim2.new(1, -40, 0.5, -17.5)
    self.copyBtn.BackgroundTransparency = 0.8
    self.copyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 80)
    self.copyBtn.Text = "📋"
    self.copyBtn.TextSize = 18
    self.copyBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
    self.copyBtn.BorderSizePixel = 0
    
    local copyCorner = Instance.new("UICorner")
    copyCorner.CornerRadius = UDim.new(0, 8)
    copyCorner.Parent = self.copyBtn
    
    self.copyBtn.MouseButton1Click:Connect(function()
        SoundSystem.playClick()
        if self.currentCode then
            setclipboard(self.currentCode)
            starterGui:SetCore("SendNotification", {
                Title = "📋 Copied!",
                Text = "Code copied to clipboard",
                Duration = 2,
                Icon = "rbxassetid://8739636080"
            })
            AnimationEngine.pulse(self.copyBtn, 1.2, 0.3)
        end
    end)
    
    -- === Code Type Label ===
    self.typeLabel = Instance.new("TextLabel")
    self.typeLabel.Parent = self.mainFrame
    self.typeLabel.Size = UDim2.new(0.85, 0, 0, 22)
    self.typeLabel.Position = UDim2.new(0.075, 0, 0, 215)
    self.typeLabel.BackgroundTransparency = 1
    self.typeLabel.Text = "🔑 Alphanumeric"
    self.typeLabel.TextColor3 = Color3.fromRGB(150, 160, 200)
    self.typeLabel.TextSize = 13
    self.typeLabel.Font = Enum.Font.Gotham
    
    -- === Instruction ===
    self.instruction = Instance.new("TextLabel")
    self.instruction.Parent = self.mainFrame
    self.instruction.Size = UDim2.new(0.9, 0, 0, 25)
    self.instruction.Position = UDim2.new(0.05, 0, 0, 242)
    self.instruction.BackgroundTransparency = 1
    self.instruction.Text = "✍️ Enter the code shown above"
    self.instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
    self.instruction.TextSize = 16
    self.instruction.Font = Enum.Font.Gotham
    
    -- === Input Box ===
    self.inputContainer = Instance.new("Frame")
    self.inputContainer.Parent = self.mainFrame
    self.inputContainer.Size = UDim2.new(0.8, 0, 0, 50)
    self.inputContainer.Position = UDim2.new(0.1, 0, 0, 275)
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
    self.inputBox.TextSize = 30
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
        
        -- Live character count
        local charCount = #self.inputBox.Text
        if charCount > 0 then
            self.instruction.Text = "✍️ " .. charCount .. " characters entered"
            self.instruction.TextColor3 = Color3.fromRGB(0, 200, 255)
            task.wait(0.5)
            self.instruction.Text = "✍️ Enter the code shown above"
            self.instruction.TextColor3 = Color3.fromRGB(200, 200, 220)
        end
    end)
    
    -- === Button Row ===
    -- Confirm button
    self.confirmBtn = Instance.new("TextButton")
    self.confirmBtn.Parent = self.mainFrame
    self.confirmBtn.Size = UDim2.new(0.32, 0, 0, 46)
    self.confirmBtn.Position = UDim2.new(0.075, 0, 0, 340)
    self.confirmBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 120)
    self.confirmBtn.Text = "✅ Confirm"
    self.confirmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.confirmBtn.TextScaled = true
    self.confirmBtn.Font = Enum.Font.GothamBold
    self.confirmBtn.BorderSizePixel = 0
    
    local confirmCorner = Instance.new("UICorner")
    confirmCorner.CornerRadius = UDim.new(0, 10)
    confirmCorner.Parent = self.confirmBtn
    
    -- Refresh button
    self.refreshBtn = Instance.new("TextButton")
    self.refreshBtn.Parent = self.mainFrame
    self.refreshBtn.Size = UDim2.new(0.25, 0, 0, 46)
    self.refreshBtn.Position = UDim2.new(0.42, 0, 0, 340)
    self.refreshBtn.BackgroundColor3 = Color3.fromRGB(60, 65, 110)
    self.refreshBtn.Text = "🔄"
    self.refreshBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.refreshBtn.TextScaled = true
    self.refreshBtn.Font = Enum.Font.GothamBold
    self.refreshBtn.BorderSizePixel = 0
    
    local refreshCorner = Instance.new("UICorner")
    refreshCorner.CornerRadius = UDim.new(0, 10)
    refreshCorner.Parent = self.refreshBtn
    
    -- Change type button
    self.changeBtn = Instance.new("TextButton")
    self.changeBtn.Parent = self.mainFrame
    self.changeBtn.Size = UDim2.new(0.25, 0, 0, 46)
    self.changeBtn.Position = UDim2.new(0.70, 0, 0, 340)
    self.changeBtn.BackgroundColor3 = Color3.fromRGB(120, 60, 180)
    self.changeBtn.Text = "🎲"
    self.changeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    self.changeBtn.TextScaled = true
    self.changeBtn.Font = Enum.Font.GothamBold
    self.changeBtn.BorderSizePixel = 0
    
    local changeCorner = Instance.new("UICorner")
    changeCorner.CornerRadius = UDim.new(0, 10)
    changeCorner.Parent = self.changeBtn
    
    -- === Strength Bar ===
    self.strengthContainer = Instance.new("Frame")
    self.strengthContainer.Parent = self.mainFrame
    self.strengthContainer.Size = UDim2.new(0.7, 0, 0, 8)
    self.strengthContainer.Position = UDim2.new(0.15, 0, 0, 400)
    self.strengthContainer.BackgroundColor3 = Color3.fromRGB(50, 50, 80)
    self.strengthContainer.BorderSizePixel = 0
    
    local strengthCorner = Instance.new("UICorner")
    strengthCorner.CornerRadius = UDim.new(0, 4)
    strengthCorner.Parent = self.strengthContainer
    
    self.strengthFill = Instance.new("Frame")
    self.strengthFill.Parent = self.strengthContainer
    self.strengthFill.Size = UDim2.new(1, 0, 1, 0)
    self.strengthFill.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
    self.strengthFill.BorderSizePixel = 0
    
    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(0, 4)
    fillCorner.Parent = self.strengthFill
    
    self.strengthLabel = Instance.new("TextLabel")
    self.strengthLabel.Parent = self.mainFrame
    self.strengthLabel.Size = UDim2.new(0.7, 0, 0, 20)
    self.strengthLabel.Position = UDim2.new(0.15, 0, 0, 410)
    self.strengthLabel.BackgroundTransparency = 1
    self.strengthLabel.Text = "🔒 Strong"
    self.strengthLabel.TextColor3 = Color3.from

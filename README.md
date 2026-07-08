-- ============================================
-- MOBILE-FRIENDLY CHAT INTERFACE
-- Optimized for touch screens (Phones/Tablets)
-- ============================================

local player = game.Players.LocalPlayer
local chatService = game:GetService("Chat")
local players = game:GetService("Players")
local userInput = game:GetService("UserInputService")
local runService = game:GetService("RunService")

-- ========== CREATE MAIN INTERFACE ==========
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ChatInterface"
screenGui.Parent = player.PlayerGui
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.ResetOnSpawn = false

-- ========== BACKGROUND (Touch to drag) ==========
local background = Instance.new("Frame")
background.Size = UDim2.new(1, 0, 1, 0)
background.BackgroundTransparency = 1
background.Parent = screenGui

-- ========== MAIN FRAME (Mobile optimized) ==========
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 520)  -- Fixed size for mobile
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -260)  -- Centered
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BackgroundTransparency = 0.85
mainFrame.BorderSizePixel = 3
mainFrame.BorderColor3 = Color3.fromRGB(100, 150, 255)
mainFrame.ClipsDescendants = true
mainFrame.Parent = screenGui

-- Make it draggable for mobile
local dragActive = false
local dragStartPos = nil
local dragOffset = nil

mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        dragActive = true
        dragStartPos = input.Position
        dragOffset = mainFrame.Position
    end
end)

mainFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        dragActive = false
    end
end)

userInput.InputChanged:Connect(function(input)
    if dragActive and input.UserInputType == Enum.UserInputType.Touch then
        local delta = input.Position - dragStartPos
        local newX = dragOffset.X.Offset + delta.X
        local newY = dragOffset.Y.Offset + delta.Y
        
        -- Keep within screen bounds
        local maxX = 300 - mainFrame.Size.X.Offset / 2
        local maxY = 400 - mainFrame.Size.Y.Offset / 2
        newX = math.clamp(newX, -200, maxX)
        newY = math.clamp(newY, -200, maxY)
        
        mainFrame.Position = UDim2.new(0.5, newX, 0.5, newY)
    end
end)

-- ========== TITLE BAR (Bigger for touch) ==========
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 50)
titleBar.BackgroundColor3 = Color3.fromRGB(20, 40, 80)
titleBar.BackgroundTransparency = 0.2
titleBar.Parent = mainFrame

-- ========== TITLE TEXT ==========
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(0.7, 0, 1, 0)
titleLabel.Position = UDim2.new(0, 15, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "💬 Chat Monitor"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 20
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

-- ========== PLAYER COUNT ==========
local playerCount = Instance.new("TextLabel")
playerCount.Size = UDim2.new(0, 60, 1, 0)
playerCount.Position = UDim2.new(0.7, 0, 0, 0)
playerCount.BackgroundTransparency = 1
playerCount.Text = "👤 0"
playerCount.TextColor3 = Color3.fromRGB(0, 255, 100)
playerCount.TextSize = 16
playerCount.Font = Enum.Font.GothamBold
playerCount.Parent = titleBar

-- ========== MINIMIZE BUTTON (Big for touch) ==========
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 45, 0, 45)
minimizeBtn.Position = UDim2.new(1, -105, 0, 2)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
minimizeBtn.BackgroundTransparency = 0.3
minimizeBtn.Text = "─"
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.TextSize = 28
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Parent = titleBar

-- ========== CLOSE BUTTON (Big for touch) ==========
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 45, 0, 45)
closeBtn.Position = UDim2.new(1, -50, 0, 2)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.BackgroundTransparency = 0.3
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 28
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Parent = titleBar

-- ========== MESSAGE CONTAINER ==========
local messageContainer = Instance.new("ScrollingFrame")
messageContainer.Size = UDim2.new(1, -10, 1, -60)
messageContainer.Position = UDim2.new(0, 5, 0, 55)
messageContainer.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
messageContainer.BackgroundTransparency = 0.6
messageContainer.BorderSizePixel = 0
messageContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
messageContainer.ScrollBarThickness = 10
messageContainer.ScrollBarImageColor3 = Color3.fromRGB(150, 150, 255)
messageContainer.VerticalScrollBarPosition = Enum.VerticalScrollBarPosition.Right
messageContainer.Parent = mainFrame

-- ========== BOTTOM CONTROLS ==========
local bottomBar = Instance.new("Frame")
bottomBar.Size = UDim2.new(1, 0, 0, 45)
bottomBar.Position = UDim2.new(0, 0, 1, -45)
bottomBar.BackgroundColor3 = Color3.fromRGB(20, 40, 80)
bottomBar.BackgroundTransparency = 0.3
bottomBar.Parent = mainFrame

-- Clear button
local clearBtn = Instance.new("TextButton")
clearBtn.Size = UDim2.new(0, 100, 0, 35)
clearBtn.Position = UDim2.new(0.5, -110, 0.5, -17)
clearBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
clearBtn.Text = "🗑️ Clear"
clearBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
clearBtn.TextSize = 16
clearBtn.Font = Enum.Font.GothamBold
clearBtn.Parent = bottomBar

-- Copy button
local copyBtn = Instance.new("TextButton")
copyBtn.Size = UDim2.new(0, 100, 0, 35)
copyBtn.Position = UDim2.new(0.5, 10, 0.5, -17)
copyBtn.BackgroundColor3 = Color3.fromRGB(100, 200, 100)
copyBtn.Text = "📋 Copy"
copyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
copyBtn.TextSize = 16
copyBtn.Font = Enum.Font.GothamBold
copyBtn.Parent = bottomBar

-- ========== VARIABLES ==========
local messages = {}
local maxMessages = 100
local isMinimized = false

-- ========== FUNCTION TO GET AVATAR ==========
local function getAvatarImage(username)
    local userId = nil
    for _, plr in pairs(players:GetPlayers()) do
        if plr.Name == username then
            userId = plr.UserId
            break
        end
    end
    if userId then
        return "https://www.roblox.com/headshot-thumbnail/image?userId=" .. userId .. "&width=60&height=60&format=png"
    end
    return "https://www.roblox.com/headshot-thumbnail/image?userId=1&width=60&height=60&format=png"
end

-- ========== UPDATE PLAYER COUNT ==========
local function updatePlayerCount()
    local count = #players:GetPlayers()
    playerCount.Text = "👤 " .. count
end
updatePlayerCount()

players.PlayerAdded:Connect(updatePlayerCount)
players.PlayerRemoving:Connect(updatePlayerCount)

-- ========== ADD MESSAGE FUNCTION ==========
local function addMessage(speaker, message, color, isJoinMessage)
    if not color then
        color = Color3.fromRGB(255, 255, 255)
    end
    
    local speakerName = speaker or "System"
    
    -- Create message frame (taller for mobile)
    local msgFrame = Instance.new("Frame")
    msgFrame.Size = UDim2.new(1, -10, 0, 55)
    msgFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    msgFrame.BackgroundTransparency = 0.3
    msgFrame.BorderSizePixel = 1
    msgFrame.BorderColor3 = Color3.fromRGB(60, 60, 150)
    msgFrame.Parent = messageContainer
    
    -- Avatar (bigger for mobile)
    local avatarImage = Instance.new("ImageLabel")
    avatarImage.Size = UDim2.new(0, 45, 0, 45)
    avatarImage.Position = UDim2.new(0, 5, 0, 5)
    avatarImage.BackgroundTransparency = 1
    avatarImage.Image = getAvatarImage(speakerName)
    avatarImage.Parent = msgFrame
    
    -- Name label
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(0, 120, 0, 22)
    nameLabel.Position = UDim2.new(0, 55, 0, 3)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = speakerName
    nameLabel.TextColor3 = color
    nameLabel.TextSize = 18
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextXAlignment = Enum.TextXAlignment.Left
    nameLabel.Parent = msgFrame
    
    -- Message text
    local msgLabel = Instance.new("TextLabel")
    msgLabel.Size = UDim2.new(1, -60, 0, 22)
    msgLabel.Position = UDim2.new(0, 55, 0, 27)
    msgLabel.BackgroundTransparency = 1
    msgLabel.Text = message or ""
    msgLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    msgLabel.TextSize = 16
    msgLabel.Font = Enum.Font.Gotham
    msgLabel.TextXAlignment = Enum.TextXAlignment.Left
    msgLabel.TextWrapped = true
    msgLabel.Parent = msgFrame
    
    -- Join message special
    if isJoinMessage then
        msgLabel.Text = "🔵 Joined the game!"
        msgLabel.TextColor3 = Color3.fromRGB(100, 200, 255)
    end
    
    -- Add to list
    table.insert(messages, msgFrame)
    
    -- Update canvas
    local canvasY = #messages * 60
    messageContainer.CanvasSize = UDim2.new(0, 0, 0, canvasY)
    
    if #messages > maxMessages then
        local oldMsg = table.remove(messages, 1)
        oldMsg:Destroy()
    end
    
    -- Auto scroll
    messageContainer.CanvasPosition = Vector2.new(0, messageContainer.CanvasSize.Y.Offset)
end

-- ========== SYSTEM MESSAGE ==========
local function addSystemMessage(text, color)
    if not color then
        color = Color3.fromRGB(255, 255, 0)
    end
    
    local msgFrame = Instance.new("Frame")
    msgFrame.Size = UDim2.new(1, -10, 0, 35)
    msgFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
    msgFrame.BackgroundTransparency = 0.4
    msgFrame.BorderSizePixel = 1
    msgFrame.BorderColor3 = Color3.fromRGB(80, 80, 200)
    msgFrame.Parent = messageContainer
    
    local msgLabel = Instance.new("TextLabel")
    msgLabel.Size = UDim2.new(1, -20, 1, 0)
    msgLabel.Position = UDim2.new(0, 10, 0, 0)
    msgLabel.BackgroundTransparency = 1
    msgLabel.Text = "⚡ " .. text
    msgLabel.TextColor3 = color
    msgLabel.TextSize = 16
    msgLabel.Font = Enum.Font.Gotham
    msgLabel.TextXAlignment = Enum.TextXAlignment.Left
    msgLabel.Parent = msgFrame
    
    table.insert(messages, msgFrame)
    
    local canvasY = #messages * 40
    messageContainer.CanvasSize = UDim2.new(0, 0, 0, canvasY)
    
    if #messages > maxMessages then
        local oldMsg = table.remove(messages, 1)
        oldMsg:Destroy()
    end
    
    messageContainer.CanvasPosition = Vector2.new(0, messageContainer.CanvasSize.Y.Offset)
end

-- ========== CLEAR MESSAGES ==========
clearBtn.MouseButton1Click:Connect(function()
    for _, msg in pairs(messages) do
        msg:Destroy()
    end
    messages = {}
    messageContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
    addSystemMessage("Messages cleared!", Color3.fromRGB(255, 200, 100))
end)

-- ========== COPY MESSAGES ==========
copyBtn.MouseButton1Click:Connect(function()
    local text = ""
    for _, msg in pairs(messages) do
        local nameLabel = msg:FindFirstChildWhichIsA("TextLabel")
        local msgLabel = msg:FindFirstChildWhichIsA("TextLabel")
        if nameLabel and msgLabel then
            text = text .. nameLabel.Text .. ": " .. msgLabel.Text .. "\n"
        end
    end
    if text ~= "" then
        if setclipboard then
            setclipboard(text)
            addSystemMessage("📋 " .. #messages .. " messages copied!", Color3.fromRGB(100, 255, 100))
        else
            addSystemMessage("❌ Copy not supported!", Color3.fromRGB(255, 0, 0))
        end
    end
end)

-- ========== CHAT DETECTION ==========
-- Local player chat
chatService.Chatted:Connect(function(message, speaker)
    if speaker then
        local speakerName = speaker.Name
        local color = Color3.fromRGB(255, 255, 255)
        local playerObj = players:FindFirstChild(speakerName)
        if playerObj and playerObj.TeamColor then
            color = playerObj.TeamColor.Color
        end
        addMessage(speakerName, message, color, false)
    end
end)

-- Other players chat
for _, plr in pairs(players:GetPlayers()) do
    if plr ~= player then
        plr.Chatted:Connect(function(message)
            local color = plr.TeamColor and plr.TeamColor.Color or Color3.fromRGB(255, 255, 255)
            addMessage(plr.Name, message, color, false)
        end)
    end
end

-- ========== PLAYER JOIN/LEAVE ==========
local function onPlayerJoined(newPlayer)
    if newPlayer ~= player then
        addMessage(newPlayer.Name, "", newPlayer.TeamColor and newPlayer.TeamColor.Color or Color3.fromRGB(100, 255, 100), true)
        addSystemMessage(newPlayer.Name .. " joined the game!", Color3.fromRGB(0, 255, 100))
    end
end

-- Existing players
for _, plr in pairs(players:GetPlayers()) do
    if plr ~= player then
        onPlayerJoined(plr)
    end
end

players.PlayerAdded:Connect(onPlayerJoined)

players.PlayerRemoving:Connect(function(leavingPlayer)
    if leavingPlayer ~= player then
        addSystemMessage(leavingPlayer.Name .. " left the game!", Color3.fromRGB(255, 100, 100))
    end
end)

-- ========== UI CONTROLS ==========
-- Minimize
minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        mainFrame.Size = UDim2.new(0, 400, 0, 50)
        messageContainer.Visible = false
        bottomBar.Visible = false
        minimizeBtn.Text = "□"
        minimizeBtn.Position = UDim2.new(1, -105, 0, 2)
        closeBtn.Position = UDim2.new(1, -50, 0, 2)
    else
        mainFrame.Size = UDim2.new(0, 400, 0, 520)
        messageContainer.Visible = true
        bottomBar.Visible = true
        minimizeBtn.Text = "─"
    end
end)

-- Close
closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
    print("✅ Chat Interface Closed")
end)

-- ========== KEYBOARD SHORTCUTS ==========
userInput.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.C and userInput:IsKeyDown(Enum.KeyCode.LeftControl) then
        screenGui:Destroy()
    end
    if input.KeyCode == Enum.KeyCode.M and userInput:IsKeyDown(Enum.KeyCode.LeftControl) then
        minimizeBtn.MouseButton1Click:Fire()
    end
end)

-- ========== WELCOME MESSAGES ==========
addSystemMessage("📱 Mobile Chat Monitor Activated!", Color3.fromRGB(0, 255, 0))
addSystemMessage("👥 Players online: " .. #players:GetPlayers(), Color3.fromRGB(255, 255, 0))
addSystemMessage("🖱️ Drag to move | Ctrl+C to close", Color3.fromRGB(100, 200, 255))

print("✅ Mobile Chat Interface Loaded Successfully!")
print("📌 Touch and drag to move the window")

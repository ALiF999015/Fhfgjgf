-- Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Variables
local lp = Players.LocalPlayer

-- Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "InstantUGC_ReyScriptHub"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = lp:WaitForChild("PlayerGui")

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 300, 0, 180)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -90)
mainFrame.BackgroundColor3 = Color3.fromRGB(173, 216, 230)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

-- Corner rounding
local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 15)
mainCorner.Parent = mainFrame

-- Shadow effect
local shadow = Instance.new("ImageLabel")
shadow.Name = "Shadow"
shadow.Size = UDim2.new(1, 30, 1, 30)
shadow.Position = UDim2.new(0, -15, 0, -15)
shadow.BackgroundTransparency = 1
shadow.Image = "rbxasset://textures/ui/GuiImagePlaceholder.png"
shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
shadow.ImageTransparency = 0.7
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(10, 10, 10, 10)
shadow.ZIndex = -1
shadow.Parent = mainFrame

-- Header
local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 50)
header.BackgroundColor3 = Color3.fromRGB(135, 206, 250)
header.BorderSizePixel = 0
header.Parent = mainFrame

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 15)
headerCorner.Parent = header

-- Fix header bottom corners
local headerFix = Instance.new("Frame")
headerFix.Size = UDim2.new(1, 0, 0, 15)
headerFix.Position = UDim2.new(0, 0, 1, -15)
headerFix.BackgroundColor3 = Color3.fromRGB(135, 206, 250)
headerFix.BorderSizePixel = 0
headerFix.Parent = header

-- Title
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, -60, 1, 0)
title.Position = UDim2.new(0, 15, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Instant ugc - Rey_Script Hub"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 16
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = header

-- Close Button
local closeBtn = Instance.new("TextButton")
closeBtn.Name = "CloseButton"
closeBtn.Size = UDim2.new(0, 35, 0, 35)
closeBtn.Position = UDim2.new(1, -45, 0, 7.5)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 18
closeBtn.Font = Enum.Font.GothamBold
closeBtn.BorderSizePixel = 0
closeBtn.Parent = header

local closeBtnCorner = Instance.new("UICorner")
closeBtnCorner.CornerRadius = UDim.new(0, 8)
closeBtnCorner.Parent = closeBtn

-- Content Frame
local content = Instance.new("Frame")
content.Name = "Content"
content.Size = UDim2.new(1, -30, 1, -80)
content.Position = UDim2.new(0, 15, 0, 65)
content.BackgroundTransparency = 1
content.Parent = mainFrame

-- Status Label
local statusLabel = Instance.new("TextLabel")
statusLabel.Name = "StatusLabel"
statusLabel.Size = UDim2.new(1, 0, 0, 30)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "Status: Ready"
statusLabel.TextColor3 = Color3.fromRGB(50, 50, 50)
statusLabel.TextSize = 16
statusLabel.Font = Enum.Font.Gotham
statusLabel.TextXAlignment = Enum.TextXAlignment.Center
statusLabel.Parent = content

-- Claim Button
local claimBtn = Instance.new("TextButton")
claimBtn.Name = "ClaimButton"
claimBtn.Size = UDim2.new(1, 0, 0, 60)
claimBtn.Position = UDim2.new(0, 0, 0, 40)
claimBtn.BackgroundColor3 = Color3.fromRGB(100, 200, 100)
claimBtn.Text = "Instant UGC"
claimBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
claimBtn.TextSize = 20
claimBtn.Font = Enum.Font.GothamBold
claimBtn.BorderSizePixel = 0
claimBtn.Parent = content

local claimBtnCorner = Instance.new("UICorner")
claimBtnCorner.CornerRadius = UDim.new(0, 10)
claimBtnCorner.Parent = claimBtn

-- Functions
local function updateStatus(text, color)
    statusLabel.Text = "Status: " .. text
    statusLabel.TextColor3 = color or Color3.fromRGB(50, 50, 50)
end

local function claimMilk()
    local success, err = pcall(function()
        local args = {
            "ClaimMilk3"
        }
        ReplicatedStorage:WaitForChild("Events"):WaitForChild("debug"):FireServer(unpack(args))
    end)
    
    if success then
        updateStatus("Claimed! ✓", Color3.fromRGB(0, 150, 0))
        
        -- Flash effect
        TweenService:Create(claimBtn, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(150, 255, 150)}):Play()
        wait(0.1)
        TweenService:Create(claimBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(100, 200, 100)}):Play()
        
        wait(1.5)
        updateStatus("Ready", Color3.fromRGB(50, 50, 50))
    else
        updateStatus("Error!", Color3.fromRGB(255, 100, 100))
        wait(2)
        updateStatus("Ready", Color3.fromRGB(50, 50, 50))
    end
end

-- Button Animations
local function buttonHover(button, normalColor, hoverColor)
    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = hoverColor}):Play()
    end)
    
    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = normalColor}):Play()
    end)
end

buttonHover(claimBtn, Color3.fromRGB(100, 200, 100), Color3.fromRGB(80, 180, 80))
buttonHover(closeBtn, Color3.fromRGB(255, 100, 100), Color3.fromRGB(235, 80, 80))

-- Button Events
claimBtn.MouseButton1Click:Connect(function()
    claimMilk()
end)

closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Entry Animation
mainFrame.Size = UDim2.new(0, 0, 0, 0)
mainFrame.BackgroundTransparency = 1

TweenService:Create(mainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
    Size = UDim2.new(0, 300, 0, 180),
    BackgroundTransparency = 0
}):Play()

updateStatus("Ready", Color3.fromRGB(50, 50, 50))

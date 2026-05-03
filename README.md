-- Configurações de Estilo (Baseado na image_96e69e.png)
local UI_ACCENT = Color3.fromRGB(0, 170, 255) 
local UI_BG_COLOR = Color3.fromRGB(15, 15, 20) 
local UI_BUTTON_BG = Color3.fromRGB(20, 25, 45) 

-- Criando a Interface Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "xxxxSpeedLaggerGUI"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Tamanho REDUZIDO (Compacto)
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 240, 0, 140) -- Antes era 310x190
MainFrame.Position = UDim2.new(0.5, -120, 0.4, 0)
MainFrame.BackgroundColor3 = UI_BG_COLOR
MainFrame.BackgroundTransparency = 0.15 
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Bordas Arredondadas
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- Borda Azul Neon
local UIStroke = Instance.new("UIStroke")
UIStroke.Color = UI_ACCENT
UIStroke.Thickness = 1.5
UIStroke.Parent = MainFrame

-- Título xxxx (Menor)
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.Text = "xxxx speed lagger"
Title.TextColor3 = UI_ACCENT
Title.TextSize = 15
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame

--- LÓGICA ---
local isEnabled = false
local lagValue = 0
local speedValue = 16

game:GetService("RunService").Heartbeat:Connect(function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") and isEnabled then
        char.Humanoid.WalkSpeed = speedValue
        if lagValue > 0 then
            local start = os.clock()
            while os.clock() - start < (lagValue / 1000) do end
        end
    elseif char and char:FindFirstChild("Humanoid") and not isEnabled then
        char.Humanoid.WalkSpeed = 16 
    end
end)

--- ELEMENTOS COMPACTOS ---

-- LAG Section
local LagLabel = Instance.new("TextLabel")
LagLabel.Position = UDim2.new(0, 0, 0.22, 0)
LagLabel.Size = UDim2.new(1, 0, 0, 15)
LagLabel.BackgroundTransparency = 1
LagLabel.Text = "LAG: 0%"
LagLabel.TextColor3 = Color3.new(1, 1, 1)
LagLabel.TextSize = 11
LagLabel.Font = Enum.Font.GothamBold
LagLabel.Parent = MainFrame

local LagSlider = Instance.new("Frame")
LagSlider.Position = UDim2.new(0.1, 0, 0.38, 0)
LagSlider.Size = UDim2.new(0.8, 0, 0, 2)
LagSlider.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
LagSlider.BorderSizePixel = 0
LagSlider.Parent = MainFrame

local LagKnob = Instance.new("TextButton")
LagKnob.Size = UDim2.new(0, 12, 0, 12)
LagKnob.Position = UDim2.new(0, 0, 0.5, -6)
LagKnob.BackgroundColor3 = Color3.new(1, 1, 1)
LagKnob.Text = ""
LagKnob.Parent = LagSlider
Instance.new("UICorner", LagKnob).CornerRadius = UDim.new(1, 0)

-- SPEED Section
local SpeedLabel = Instance.new("TextLabel")
SpeedLabel.Position = UDim2.new(0, 0, 0.48, 0)
SpeedLabel.Size = UDim2.new(1, 0, 0, 15)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.Text = "SPEED: 16"
SpeedLabel.TextColor3 = Color3.new(1, 1, 1)
SpeedLabel.TextSize = 11
SpeedLabel.Font = Enum.Font.GothamBold
SpeedLabel.Parent = MainFrame

local SpeedSlider = Instance.new("Frame")
SpeedSlider.Position = UDim2.new(0.1, 0, 0.64, 0)
SpeedSlider.Size = UDim2.new(0.8, 0, 0, 2)
SpeedSlider.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
SpeedSlider.BorderSizePixel = 0
SpeedSlider.Parent = MainFrame

local SpeedKnob = Instance.new("TextButton")
SpeedKnob.Size = UDim2.new(0, 12, 0, 12)
SpeedKnob.Position = UDim2.new(0.08, 0, 0.5, -6)
SpeedKnob.BackgroundColor3 = Color3.new(1, 1, 1)
SpeedKnob.Text = ""
SpeedKnob.Parent = SpeedSlider
Instance.new("UICorner", SpeedKnob).CornerRadius = UDim.new(1, 0)

--- CONTROLES ---
local function setupSlider(knob, bar, callback)
    knob.MouseButton1Down:Connect(function()
        local move = game:GetService("UserInputService").InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement then
                local pos = math.clamp((input.Position.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
                knob.Position = UDim2.new(pos, -6, 0.5, -6)
                callback(pos)
            end
        end)
        game:GetService("UserInputService").InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then move:Disconnect() end
        end)
    end)
end

setupSlider(LagKnob, LagSlider, function(pos)
    lagValue = math.floor(pos * 100)
    LagLabel.Text = "LAG: " .. lagValue .. "%"
end)

setupSlider(SpeedKnob, SpeedSlider, function(pos)
    speedValue = math.floor(pos * 200)
    SpeedLabel.Text = "SPEED: " .. speedValue
end)

--- BOTÕES ---
local EnableBtn = Instance.new("TextButton")
EnableBtn.Position = UDim2.new(0.1, 0, 0.76, 0)
EnableBtn.Size = UDim2.new(0.55, 0, 0, 24)
EnableBtn.BackgroundColor3 = UI_BUTTON_BG
EnableBtn.Text = "ENABLE: OFF"
EnableBtn.TextColor3 = UI_ACCENT
EnableBtn.Font = Enum.Font.GothamBold
EnableBtn.TextSize = 11
EnableBtn.Parent = MainFrame
Instance.new("UICorner", EnableBtn)

local KeyLabel = Instance.new("TextButton")
KeyLabel.Position = UDim2.new(0.7, 0, 0.76, 0)
KeyLabel.Size = UDim2.new(0.2, 0, 0, 24)
KeyLabel.BackgroundColor3 = UI_BUTTON_BG
KeyLabel.Text = "[F]"
KeyLabel.TextColor3 = UI_ACCENT
KeyLabel.Font = Enum.Font.GothamBold
KeyLabel.Parent = MainFrame
Instance.new("UICorner", KeyLabel)

EnableBtn.MouseButton1Click:Connect(function()
    isEnabled = not isEnabled
    EnableBtn.Text = isEnabled and "ENABLE: ON" or "ENABLE: OFF"
    EnableBtn.TextColor3 = isEnabled and Color3.fromRGB(0, 255, 127) or UI_ACCENT
end)

game:GetService("UserInputService").InputBegan:Connect(function(input, gpe)
    if not gpe and input.KeyCode == Enum.KeyCode.F then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

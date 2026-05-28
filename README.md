-- ========================================
-- XITDZIN GRATIS FOV - Com Minimizar
-- ========================================

local player = game.Players.LocalPlayer
local camera = workspace.CurrentCamera

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "XitDzinFOV"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 190)
frame.Position = UDim2.new(0.5, -150, 0.5, -95)
frame.BackgroundColor3 = Color3.fromRGB(15, 20, 40)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 16)
corner.Parent = frame

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.BackgroundColor3 = Color3.fromRGB(0, 110, 255)
title.Text = "XITDZIN GRATIS FOV"
title.TextColor3 = Color3.new(1,1,1)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.Parent = frame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 16)
titleCorner.Parent = title

-- Botão Minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 35, 0, 35)
minimizeBtn.Position = UDim2.new(1, -40, 0, 8)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
minimizeBtn.Text = "−"
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.TextScaled = true
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Parent = frame

local minimizeCorner = Instance.new("UICorner")
minimizeCorner.CornerRadius = UDim.new(0, 10)
minimizeCorner.Parent = minimizeBtn

-- Conteúdo
local content = Instance.new("Frame")
content.Size = UDim2.new(1, 0, 1, -55)
content.Position = UDim2.new(0, 0, 0, 55)
content.BackgroundTransparency = 1
content.Parent = frame

local slider = Instance.new("TextBox")
slider.Size = UDim2.new(0.85, 0, 0, 45)
slider.Position = UDim2.new(0.075, 0, 0.1, 0)
slider.BackgroundColor3 = Color3.fromRGB(30, 35, 60)
slider.Text = "110"
slider.TextColor3 = Color3.new(1,1,1)
slider.TextScaled = true
slider.Parent = content

local label = Instance.new("TextLabel")
label.Size = UDim2.new(1, 0, 0, 25)
label.Position = UDim2.new(0, 0, 0.45, 0)
label.BackgroundTransparency = 1
label.Text = "FOV Atual: 110"
label.TextColor3 = Color3.new(1,1,1)
label.TextScaled = true
label.Parent = content

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0.85, 0, 0, 50)
toggleBtn.Position = UDim2.new(0.075, 0, 0.65, 0)
toggleBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
toggleBtn.Text = "ATIVAR FOV"
toggleBtn.TextColor3 = Color3.new(1,1,1)
toggleBtn.TextScaled = true
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.Parent = content

-- Variáveis
local fovValue = 110
local fovConnection = nil
local minimized = false

-- ==================== LÓGICA ====================
local function ForceFOV()
    camera.FieldOfView = fovValue
end

-- Minimizar (Mobile + PC)
minimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        frame.Size = UDim2.new(0, 300, 0, 55)
        content.Visible = false
        minimizeBtn.Text = "+"
    else
        frame.Size = UDim2.new(0, 300, 0, 190)
        content.Visible = true
        minimizeBtn.Text = "−"
    end
end)

-- Atalho PC: Tecla "K"
game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.K then
        minimized = not minimized
        if minimized then
            frame.Size = UDim2.new(0, 300, 0, 55)
            content.Visible = false
            minimizeBtn.Text = "+"
        else
            frame.Size = UDim2.new(0, 300, 0, 190)
            content.Visible = true
            minimizeBtn.Text = "−"
        end
    end
end)

toggleBtn.MouseButton1Click:Connect(function()
    if fovConnection then
        fovConnection:Disconnect()
        fovConnection = nil
        toggleBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
        toggleBtn.Text = "ATIVAR FOV"
        camera.FieldOfView = 70
    else
        fovConnection = game:GetService("RunService").RenderStepped:Connect(ForceFOV)
        toggleBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
        toggleBtn.Text = "DESATIVAR FOV"
    end
end)

slider.FocusLost:Connect(function()
    local num = tonumber(slider.Text)
    if num then
        fovValue = math.clamp(num, 30, 140)
        slider.Text = tostring(fovValue)
        label.Text = "FOV Atual: " .. fovValue
    end
end)

print("✅ XITDZIN GRATIS FOV carregado! (K para minimizar no PC)")

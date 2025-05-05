-- Jumpscare Permanente - Para uso de testes autorizados
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")

-- Desativa interface padrão
pcall(function()
	StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.All, false)
end)

-- Cria a tela
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "JumpscareUI"
screenGui.IgnoreGuiInset = true
screenGui.ResetOnSpawn = false
screenGui.DisplayOrder = 999999
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Tela preta de fundo
local blackFrame = Instance.new("Frame")
blackFrame.Size = UDim2.new(1, 0, 1, 0)
blackFrame.Position = UDim2.new(0, 0, 0, 0)
blackFrame.BackgroundColor3 = Color3.new(0, 0, 0)
blackFrame.ZIndex = 1
blackFrame.Parent = screenGui

-- Imagem assustadora
local image = Instance.new("ImageLabel")
image.Size = UDim2.new(1, 0, 1, 0)
image.Position = UDim2.new(0, 0, 0, 0)
image.BackgroundTransparency = 1
image.Image = "rbxassetid://12023501677" -- ID da imagem
image.ImageTransparency = 1
image.ZIndex = 2
image.Parent = screenGui

-- Texto "VOCÊ FOI HACKEADO"
local warningText = Instance.new("TextLabel")
warningText.Size = UDim2.new(1, 0, 1, 0)
warningText.Position = UDim2.new(0, 0, 0, 0)
warningText.BackgroundTransparency = 1
warningText.Text = "VOCÊ FOI HACKEADO"
warningText.TextColor3 = Color3.new(1, 0, 0)
warningText.TextStrokeTransparency = 0
warningText.TextStrokeColor3 = Color3.new(0, 0, 0)
warningText.Font = Enum.Font.Arcade
warningText.TextScaled = true
warningText.ZIndex = 3
warningText.Parent = screenGui

-- Piscar do texto
task.spawn(function()
	while true do
		warningText.Visible = true
		task.wait(0.3)
		warningText.Visible = false
		task.wait(0.2)
	end
end)

-- Áudio assustador
local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://2178259648" -- Som assustador
sound.Volume = 10
sound.Looped = true
sound.Parent = screenGui
sound:Play()

-- Fade-in da imagem
for i = 1, 10 do
	image.ImageTransparency = 1 - (i * 0.1)
	task.wait(0.05)
end

-- Função para criar gotas de sangue
local function criarGota(posX, posY, tamanho, rotacao)
	local gota = Instance.new("ImageLabel")
	gota.Size = UDim2.new(0, tamanho, 0, tamanho)
	gota.Position = UDim2.new(0, posX, 0, posY)
	gota.BackgroundTransparency = 1
	gota.Image = "rbxassetid://507807544" -- Imagem estilo gota
	gota.ImageColor3 = Color3.fromRGB(170, 0, 0)
	gota.Rotation = rotacao
	gota.ZIndex = 2.5
	gota.Parent = screenGui
end

-- Criar várias gotas aleatórias
for i = 1, 15 do
	local x = math.random(0, 800)
	local y = math.random(0, 450)
	local size = math.random(20, 80)
	local rot = math.random(0, 360)
	criarGota(x, y, size, rot)
end

-- Previne remoção da tela
screenGui.AncestryChanged:Connect(function()
	if not screenGui:IsDescendantOf(player:FindFirstChild("PlayerGui")) then
		screenGui.Parent = player:FindFirstChild("PlayerGui")
	end
end)

player:FindFirstChildOfClass("PlayerGui").ChildRemoved:Connect(function(child)
	if child == screenGui then
		task.wait()
		screenGui.Parent = player:FindFirstChild("PlayerGui")
	end
end)

-- Vibração em mobile
pcall(function()
	game:GetService("HapticService"):SetMotor(Enum.UserInputType.Gamepad1, Enum.VibrationMotor.Large, 1)
end)

-- CONFIGURAÇÕES
local Config = {
	UIName = "Flutuante UI",
	Subtitle = "Premium Hub",

	KeySystem = true,        -- true = usa key | false = não usa
	Key = "MINHA_KEY_123",   -- key correta
	GetKeyURL = "https://seusite.com/key"
}

-- SERVIÇOS
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- GUI
local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.ResetOnSpawn = false

-- FUNÇÃO DRAG
local function MakeDraggable(frame)
	local dragging, dragStart, startPos
	frame.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			dragging = true
			dragStart = input.Position
			startPos = frame.Position
		end
	end)
	UserInputService.InputChanged:Connect(function(input)
		if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
			local delta = input.Position - dragStart
			frame.Position = UDim2.new(
				startPos.X.Scale, startPos.X.Offset + delta.X,
				startPos.Y.Scale, startPos.Y.Offset + delta.Y
			)
		end
	end)
	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			dragging = false
		end
	end)
end

-- BOTÃO FLUTUANTE
local OpenButton = Instance.new("TextButton", ScreenGui)
OpenButton.Size = UDim2.new(0,50,0,50)
OpenButton.Position = UDim2.new(0,10,0.5,-25)
OpenButton.Text = "≡"
OpenButton.Visible = false
OpenButton.BackgroundColor3 = Color3.fromRGB(30,30,30)
OpenButton.TextColor3 = Color3.new(1,1,1)
OpenButton.BorderSizePixel = 0

-- FRAME PRINCIPAL
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0,520,0,360)
Main.Position = UDim2.new(0.5,-260,0.5,-180)
Main.BackgroundColor3 = Color3.fromRGB(20,20,20)
Main.BorderSizePixel = 0
MakeDraggable(Main)

-- TOPO
local Top = Instance.new("Frame", Main)
Top.Size = UDim2.new(1,0,0,40)
Top.BackgroundColor3 = Color3.fromRGB(25,25,25)

local Title = Instance.new("TextLabel", Top)
Title.Size = UDim2.new(1,-40,1,0)
Title.Position = UDim2.new(0,10,0,0)
Title.Text = Config.UIName.." - "..Config.Subtitle
Title.TextColor3 = Color3.new(1,1,1)
Title.BackgroundTransparency = 1
Title.TextXAlignment = Left

local Close = Instance.new("TextButton", Top)
Close.Size = UDim2.new(0,40,1,0)
Close.Position = UDim2.new(1,-40,0,0)
Close.Text = "X"
Close.TextColor3 = Color3.new(1,0.3,0.3)
Close.BackgroundTransparency = 1

-- TABS
local Tabs = Instance.new("Frame", Main)
Tabs.Size = UDim2.new(0,120,1,-40)
Tabs.Position = UDim2.new(0,0,0,40)
Tabs.BackgroundColor3 = Color3.fromRGB(25,25,25)

-- CONTEÚDO
local Content = Instance.new("Frame", Main)
Content.Size = UDim2.new(1,-120,1,-40)
Content.Position = UDim2.new(0,120,0,40)
Content.BackgroundTransparency = 1

-- FUNÇÃO CRIAR TAB
local function CreateTab(name)
	local Button = Instance.new("TextButton", Tabs)
	Button.Size = UDim2.new(1,0,0,40)
	Button.Text = name
	Button.BackgroundColor3 = Color3.fromRGB(30,30,30)
	Button.TextColor3 = Color3.new(1,1,1)
	Button.BorderSizePixel = 0

	local Page = Instance.new("Frame", Content)
	Page.Size = UDim2.new(1,0,1,0)
	Page.Visible = false
	Page.BackgroundTransparency = 1

	Button.MouseButton1Click:Connect(function()
		for _,v in pairs(Content:GetChildren()) do
			v.Visible = false
		end
		Page.Visible = true
	end)

	return Page
end

-- EXEMPLO DE TABS
local MainTab = CreateTab("Main")
MainTab.Visible = true

local PlayerTab = CreateTab("Player")
local SettingsTab = CreateTab("Settings")

-- BOTÃO EXEMPLO
local Button = Instance.new("TextButton", MainTab)
Button.Size = UDim2.new(0,200,0,40)
Button.Position = UDim2.new(0,20,0,20)
Button.Text = "Botão Exemplo"
Button.BackgroundColor3 = Color3.fromRGB(35,35,35)
Button.TextColor3 = Color3.new(1,1,1)
Button.BorderSizePixel = 0

Button.MouseButton1Click:Connect(function()
	-- COLE SUA FUNÇÃO AQUI
end)

-- SLIDER EXEMPLO
local Slider = Instance.new("TextLabel", PlayerTab)
Slider.Size = UDim2.new(0,200,0,40)
Slider.Position = UDim2.new(0,20,0,20)
Slider.Text = "Slider Exemplo"
Slider.BackgroundColor3 = Color3.fromRGB(35,35,35)
Slider.TextColor3 = Color3.new(1,1,1)

-- KEY SYSTEM
if Config.KeySystem then
	Main.Visible = false

	local KeyFrame = Instance.new("Frame", ScreenGui)
	KeyFrame.Size = UDim2.new(0,300,0,180)
	KeyFrame.Position = UDim2.new(0.5,-150,0.5,-90)
	KeyFrame.BackgroundColor3 = Color3.fromRGB(20,20,20)
	MakeDraggable(KeyFrame)

	local KeyTitle = Instance.new("TextLabel", KeyFrame)
	KeyTitle.Size = UDim2.new(1,0,0,40)
	KeyTitle.Text = Config.UIName.." - Key"
	KeyTitle.TextColor3 = Color3.new(1,1,1)
	KeyTitle.BackgroundTransparency = 1

	local Input = Instance.new("TextBox", KeyFrame)
	Input.Size = UDim2.new(1,-40,0,35)
	Input.Position = UDim2.new(0,20,0,60)
	Input.PlaceholderText = "Digite a Key"
	Input.BackgroundColor3 = Color3.fromRGB(30,30,30)
	Input.TextColor3 = Color3.new(1,1,1)

	local Confirm = Instance.new("TextButton", KeyFrame)
	Confirm.Size = UDim2.new(1,-40,0,35)
	Confirm.Position = UDim2.new(0,20,0,105)
	Confirm.Text = "Confirmar"
	Confirm.BackgroundColor3 = Color3.fromRGB(40,40,40)
	Confirm.TextColor3 = Color3.new(1,1,1)

	local GetKey = Instance.new("TextButton", KeyFrame)
	GetKey.Size = UDim2.new(1,-40,0,25)
	GetKey.Position = UDim2.new(0,20,0,145)
	GetKey.Text = "Get Key"
	GetKey.BackgroundTransparency = 1
	GetKey.TextColor3 = Color3.fromRGB(0,170,255)

	GetKey.MouseButton1Click:Connect(function()
		setclipboard(Config.GetKeyURL)
	end)

	Confirm.MouseButton1Click:Connect(function()
		if Input.Text == Config.Key then
			KeyFrame:Destroy()
			Main.Visible = true
		end
	end)
end

-- FECHAR / ABRIR
Close.MouseButton1Click:Connect(function()
	Main.Visible = false
	OpenButton.Visible = true
end)

OpenButton.MouseButton1Click:Connect(function()
	Main.Visible = true
	OpenButton.Visible = false
end)

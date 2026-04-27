local player = game.Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "LCHubMobile"
gui.Parent = player:WaitForChild("PlayerGui")

-- BOTÃO FLUTUANTE (RESPONSIVO)
local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0.18, 0, 0.08, 0)
openBtn.Position = UDim2.new(0.02, 0, 0.45, 0)
openBtn.Text = "LC HUB"
openBtn.BackgroundColor3 = Color3.fromRGB(0,170,255)
openBtn.TextScaled = true
openBtn.Parent = gui

-- FRAME PRINCIPAL (RESPONSIVO)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.5, 0, 0.5, 0)
frame.Position = UDim2.new(0.25, 0, 0.25, 0)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
frame.Visible = false
frame.Parent = gui

-- BORDA
local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(0,170,255)
stroke.Thickness = 2
stroke.Parent = frame

-- TOPO
local top = Instance.new("Frame")
top.Size = UDim2.new(1,0,0.15,0)
top.BackgroundColor3 = Color3.fromRGB(20,20,20)
top.Parent = frame

-- TITULO
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,0,1,0)
title.Text = "LC HUB"
title.TextScaled = true
title.TextColor3 = Color3.fromRGB(0,170,255)
title.BackgroundTransparency = 1
title.Parent = top

-- FECHAR
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0.15,0,0.7,0)
closeBtn.Position = UDim2.new(0.85,0,0.15,0)
closeBtn.Text = "X"
closeBtn.TextScaled = true
closeBtn.BackgroundColor3 = Color3.fromRGB(255,60,60)
closeBtn.Parent = top

-- MENU
local menu = Instance.new("Frame")
menu.Size = UDim2.new(0.3,0,0.85,0)
menu.Position = UDim2.new(0,0,0.15,0)
menu.BackgroundColor3 = Color3.fromRGB(25,25,25)
menu.Parent = frame

-- CONTENT
local content = Instance.new("Frame")
content.Size = UDim2.new(0.7,0,0.85,0)
content.Position = UDim2.new(0.3,0,0.15,0)
content.BackgroundTransparency = 1
content.Parent = frame

-- FUNÇÃO ABA
local function criarAba(nome)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1,0,0.1,0)
	btn.Text = nome
	btn.TextScaled = true
	btn.BackgroundColor3 = Color3.fromRGB(0,170,255)
	btn.Parent = menu
	
	local page = Instance.new("Frame")
	page.Size = UDim2.new(1,0,1,0)
	page.Visible = false
	page.Parent = content
	
	btn.MouseButton1Click:Connect(function()
		for _,v in pairs(content:GetChildren()) do
			v.Visible = false
		end
		page.Visible = true
	end)
	
	return page
end

-- ABAS
local farmPage = criarAba("Farm")
local playerPage = criarAba("Player")

-- TOGGLE
local toggle = Instance.new("TextButton")
toggle.Size = UDim2.new(0.6,0,0.15,0)
toggle.Position = UDim2.new(0.2,0,0.1,0)
toggle.Text = "Auto Farm: OFF"
toggle.TextScaled = true
toggle.BackgroundColor3 = Color3.fromRGB(60,60,60)
toggle.Parent = farmPage

local ligado = false

toggle.MouseButton1Click:Connect(function()
	ligado = not ligado
	
	if ligado then
		toggle.Text = "Auto Farm: ON"
		toggle.BackgroundColor3 = Color3.fromRGB(0,170,255)
	else
		toggle.Text = "Auto Farm: OFF"
		toggle.BackgroundColor3 = Color3.fromRGB(60,60,60)
	end
end)

-- ABRIR
openBtn.MouseButton1Click:Connect(function()
	frame.Visible = true
	frame.Size = UDim2.new(0,0,0,0)
	
	TweenService:Create(frame, TweenInfo.new(0.3), {
		Size = UDim2.new(0.5,0,0.5,0)
	}):Play()
end)

-- FECHAR
closeBtn.MouseButton1Click:Connect(function()
	local tween = TweenService:Create(frame, TweenInfo.new(0.3), {
		Size = UDim2.new(0,0,0,0)
	})
	tween:Play()
	tween.Completed:Wait()
	frame.Visible = false
end)

-- DRAG + TOUCH (MOBILE FUNCIONA)
local dragging = false
local dragStart, startPos

top.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 
	or input.UserInputType == Enum.UserInputType.Touch then
		
		dragging = true
		dragStart = input.Position
		startPos = frame.Position
	end
end)

top.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 
	or input.UserInputType == Enum.UserInputType.Touch then
		
		dragging = false
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement 
	or input.UserInputType == Enum.UserInputType.Touch) then
		
		local delta = input.Position - dragStart
		
		frame.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)

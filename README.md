--[[ ⚠️ SCRIPT DE TESTES AUTORIZADOS ⚠️ 
Feito para detectar uso indevido de exploits e simular comportamento
de ferramentas suspeitas para estudo e segurança. 
--]]

-- Serviços úteis
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- Variáveis de controle
local teamCheck = true
local espEnabled = false
local autoFarm = false
local autoChest = false
local autoFruit = false
local autoRaid = false
local espObjects = {}

-- UI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "DebugTestUI"
local frame = Instance.new("Frame", gui)
frame.Position = UDim2.new(0, 20, 0.3, 0)
frame.Size = UDim2.new(0, 200, 0, 280)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0

local function createButton(text, posY, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, posY)
	btn.Text = text
	btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- ESP
local function createESP(player)
	if player == LocalPlayer then return end
	if player.Character and not player.Character:FindFirstChild("PlayerESP") then
		local esp = Instance.new("Highlight")
		esp.Name = "PlayerESP"
		esp.Adornee = player.Character
		esp.FillColor = Color3.fromRGB(255, 0, 0)
		esp.FillTransparency = 0.5
		esp.OutlineColor = Color3.new(1, 1, 1)
		esp.OutlineTransparency = 0
		esp.Parent = player.Character
		espObjects[player] = esp
	end
end

local function removeESP(player)
	if espObjects[player] then
		espObjects[player]:Destroy()
		espObjects[player] = nil
	end
end

local function updateESP()
	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer then
			if teamCheck and player.Team == LocalPlayer.Team then
				removeESP(player)
			else
				createESP(player)
			end
		end
	end
end

-- Auto Farm Simples (bate em inimigos próximos)
local function doAutoFarm()
	local enemies = workspace.Enemies:GetChildren()
	for _, enemy in pairs(enemies) do
		if enemy:FindFirstChild("Humanoid") and enemy:FindFirstChild("HumanoidRootPart") then
			LocalPlayer.Character:PivotTo(enemy.HumanoidRootPart.CFrame * CFrame.new(0, 5, 5))
			repeat
				enemy.Humanoid.Health -= 10
				wait(0.1)
			until not autoFarm or enemy.Humanoid.Health <= 0
		end
	end
end

-- Caça Baús
local function doChestHunt()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj.Name:lower():find("chest") and obj:IsA("Model") and obj:FindFirstChild("HumanoidRootPart") then
			LocalPlayer.Character:PivotTo(obj.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0))
			wait(1)
		end
	end
end

-- Busca por Frutas
local function doFruitHunt()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj:IsA("Tool") and obj.Name:lower():find("fruit") then
			LocalPlayer.Character:PivotTo(obj.Handle.CFrame + Vector3.new(0, 3, 0))
			wait(1)
		end
	end
end

-- Auto "Fábrica" Simulada (habilidade ou crafting)
local function doAutoFactory()
	local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
	if tool then
		tool:Activate()
	end
end

-- Auto Raid (simplificado)
local function doAutoRaid()
	local portal = workspace:FindFirstChild("RaidPortal")
	if portal and portal:FindFirstChild("CFrame") then
		LocalPlayer.Character:PivotTo(portal.CFrame + Vector3.new(0, 5, 0))
	end
end

-- Botões
createButton("ESP ON/OFF", 0.01, function()
	espEnabled = not espEnabled
end)

createButton("Team Check ON/OFF", 0.12, function()
	teamCheck = not teamCheck
end)

createButton("AutoFarm", 0.23, function()
	autoFarm = not autoFarm
end)

createButton("Auto Baús", 0.34, function()
	autoChest = not autoChest
end)

createButton("Auto Frutas", 0.45, function()
	autoFruit = not autoFruit
end)

createButton("Auto Fábrica", 0.56, function()
	doAutoFactory()
end)

createButton("Auto Raid", 0.67, function()
	autoRaid = not autoRaid
end)

createButton("TP Ilha Inicial", 0.78, function()
	LocalPlayer.Character:PivotTo(CFrame.new(1060, 16, 1425)) -- ajuste conforme a ilha
end)

-- Loop Principal
RunService.RenderStepped:Connect(function()
	if espEnabled then updateESP() end
	if autoFarm then doAutoFarm() end
	if autoChest then doChestHunt() end
	if autoFruit then doFruitHunt() end
	if autoRaid then doAutoRaid() end
end)

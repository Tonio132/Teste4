--[[ ⚠️ SCRIPT DE TESTES AUTORIZADOS ⚠️ 
Usado para fins de detecção e simulação de exploits no Blox Fruits.
Desenvolvido com autorização para segurança e monitoramento.
--]]

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Controle de estados
local espEnabled = false
local teamCheck = true
local autoFarm = false
local autoChest = false
local autoFruit = false
local autoRaid = false

-- Interface
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "ESP_Interface"

local frame = Instance.new("Frame", gui)
frame.Position = UDim2.new(0, 15, 0.3, 0)
frame.Size = UDim2.new(0, 200, 0, 260)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0

local function createButton(text, y, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(1, -20, 0, 28)
	btn.Position = UDim2.new(0, 10, 0, y)
	btn.Text = text
	btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 14
	btn.MouseButton1Click:Connect(callback)
end

-- ESP Functions
local function applyESP(player)
	if player == LocalPlayer then return end
	if player.Character and not player.Character:FindFirstChild("ESPHighlight") then
		if teamCheck and player.Team == LocalPlayer.Team then return end

		local esp = Instance.new("Highlight")
		esp.Name = "ESPHighlight"
		esp.FillColor = Color3.fromRGB(255, 0, 0)
		esp.FillTransparency = 0.5
		esp.OutlineColor = Color3.new(1, 1, 1)
		esp.OutlineTransparency = 0
		esp.Adornee = player.Character
		esp.Parent = player.Character
	end
end

local function removeESP(player)
	if player.Character and player.Character:FindFirstChild("ESPHighlight") then
		player.Character.ESPHighlight:Destroy()
	end
end

local function updateESP()
	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer then
			if espEnabled then
				applyESP(player)
			else
				removeESP(player)
			end
		end
	end
end

-- AutoFarm Simples
local function doAutoFarm()
	for _, enemy in pairs(workspace:GetChildren()) do
		if enemy:FindFirstChild("Humanoid") and enemy:FindFirstChild("HumanoidRootPart") then
			LocalPlayer.Character:PivotTo(enemy.HumanoidRootPart.CFrame * CFrame.new(0, 5, 5))
			repeat
				enemy.Humanoid.Health -= 10
				wait(0.2)
			until not autoFarm or enemy.Humanoid.Health <= 0
		end
	end
end

-- Baús
local function doChestHunt()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj.Name:lower():find("chest") and obj:IsA("Model") and obj:FindFirstChild("HumanoidRootPart") then
			LocalPlayer.Character:PivotTo(obj.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0))
			wait(1)
		end
	end
end

-- Frutas
local function doFruitHunt()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj:IsA("Tool") and obj.Name:lower():find("fruit") then
			LocalPlayer.Character:PivotTo(obj.Handle.CFrame + Vector3.new(0, 2, 0))
			wait(1)
		end
	end
end

-- Fábrica
local function doAutoFactory()
	local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
	if tool then tool:Activate() end
end

-- Raid
local function doAutoRaid()
	local portal = workspace:FindFirstChild("RaidPortal")
	if portal and portal:IsA("Part") then
		LocalPlayer.Character:PivotTo(portal.CFrame + Vector3.new(0, 5, 0))
	end
end

-- Botões
createButton("ESP ON/OFF", 5, function()
	espEnabled = not espEnabled
end)

createButton("TeamCheck ON/OFF", 40, function()
	teamCheck = not teamCheck
end)

createButton("Auto Farm", 75, function()
	autoFarm = not autoFarm
end)

createButton("Caça Baús", 110, function()
	autoChest = not autoChest
end)

createButton("Caça Frutas", 145, function()
	autoFruit = not autoFruit
end)

createButton("Auto Fábrica", 180, function()
	doAutoFactory()
end)

createButton("Auto Raid", 215, function()
	autoRaid = not autoRaid
end)

-- Loop Principal
RunService.RenderStepped:Connect(function()
	updateESP()
	if autoFarm then doAutoFarm() end
	if autoChest then doChestHunt() end
	if autoFruit then doFruitHunt() end
	if autoRaid then doAutoRaid() end
end)

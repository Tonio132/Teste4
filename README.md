--[[ ⚠️ SCRIPT DE TESTES AUTORIZADOS ⚠️ Usado para fins de detecção e simulação de exploits no Blox Fruits. Desenvolvido com autorização para segurança e monitoramento. --]]

-- Serviços local Players = game:GetService("Players") local RunService = game:GetService("RunService") local StarterGui = game:GetService("StarterGui") local LocalPlayer = Players.LocalPlayer

-- Controle de estados globais _G.espEnabled = false _G.teamCheck = true _G.autoFarm = false _G.autoChest = false _G.autoFruit = false _G.autoRaid = false

-- Interface local gui = Instance.new("ScreenGui") if syn and syn.protect_gui then syn.protect_gui(gui) end pcall(function() gui.Parent = game:GetService("CoreGui") end) gui.Name = "ESP_Interface"

local frame = Instance.new("Frame") frame.Position = UDim2.new(0, 20, 0.3, 0) frame.Size = UDim2.new(0, 240, 0, 300) frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30) frame.BorderSizePixel = 0 frame.Parent = gui

local uiCorner = Instance.new("UICorner") uiCorner.CornerRadius = UDim.new(0, 8) uiCorner.Parent = frame

local function createToggle(text, y, stateRef) local btn = Instance.new("TextButton") btn.Size = UDim2.new(1, -20, 0, 28) btn.Position = UDim2.new(0, 10, 0, y) btn.Text = text .. ": OFF" btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50) btn.TextColor3 = Color3.new(1, 1, 1) btn.Font = Enum.Font.SourceSansBold btn.TextSize = 14 btn.Parent = frame

btn.MouseButton1Click:Connect(function()
	_G[stateRef] = not _G[stateRef]
	btn.Text = text .. (_G[stateRef] and ": ON" or ": OFF")
end)

end

-- ESP Functions local function applyESP(player) if player == LocalPlayer then return end if player.Character and not player.Character:FindFirstChild("ESPHighlight") then if _G.teamCheck and player.Team == LocalPlayer.Team then return end

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

local function removeESP(player) if player.Character and player.Character:FindFirstChild("ESPHighlight") then player.Character.ESPHighlight:Destroy() end end

local function updateESP() for _, player in pairs(Players:GetPlayers()) do if player ~= LocalPlayer then if _G.espEnabled then applyESP(player) else removeESP(player) end end end end

-- Funções principais local function doAutoFarm() for _, enemy in pairs(workspace:GetChildren()) do if enemy:FindFirstChild("Humanoid") and enemy:FindFirstChild("HumanoidRootPart") then LocalPlayer.Character:PivotTo(enemy.HumanoidRootPart.CFrame * CFrame.new(0, 5, 5)) repeat enemy.Humanoid.Health -= 10 wait(0.2) until not _G.autoFarm or enemy.Humanoid.Health <= 0 end end end

local function doChestHunt() for _, obj in pairs(workspace:GetDescendants()) do if obj.Name:lower():find("chest") and obj:IsA("Model") and obj:FindFirstChild("HumanoidRootPart") then LocalPlayer.Character:PivotTo(obj.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)) wait(1) end end end

local function doFruitHunt() for _, obj in pairs(workspace:GetDescendants()) do if obj:IsA("Tool") and obj.Name:lower():find("fruit") then LocalPlayer.Character:PivotTo(obj.Handle.CFrame + Vector3.new(0, 2, 0)) wait(1) end end end

local function doAutoFactory() local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool") if tool then tool:Activate() end end

local function doAutoRaid() local portal = workspace:FindFirstChild("RaidPortal") if portal and portal:IsA("Part") then LocalPlayer.Character:PivotTo(portal.CFrame + Vector3.new(0, 5, 0)) end end

-- Botões ON/OFF createToggle("ESP", 5, "espEnabled") createToggle("Auto Farm", 40, "autoFarm") createToggle("Auto Baús", 75, "autoChest") createToggle("Auto Frutas", 110, "autoFruit") createToggle("Auto Raid", 145, "autoRaid")

local btnFactory = Instance.new("TextButton") btnFactory.Size = UDim2.new(1, -20, 0, 28) btnFactory.Position = UDim2.new(0, 10, 0, 180) btnFactory.Text = "Ativar Fábrica" btnFactory.BackgroundColor3 = Color3.fromRGB(50, 50, 50) btnFactory.TextColor3 = Color3.new(1, 1, 1) btnFactory.Font = Enum.Font.SourceSansBold btnFactory.TextSize = 14 btnFactory.Parent = frame btnFactory.MouseButton1Click:Connect(doAutoFactory)

-- Loop Principal RunService.RenderStepped:Connect(function() updateESP() if _G.autoFarm then doAutoFarm() end if _G.autoChest then doChestHunt() end if _G.autoFruit then doFruitHunt() end if _G.autoRaid then doAutoRaid() end end)

print("[✅ Script de testes iniciado com sucesso]")


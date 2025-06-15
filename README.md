-- Este é o conteúdo do seu script remoto (salvo no GitHub)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local espEnabled = true
local autoFarm = false

-- Cria interface simples
local gui = Instance.new("ScreenGui")
gui.Name = "SimpleESPGui"
gui.Parent = game:GetService("CoreGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0, 10, 0, 50)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Parent = gui

local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(1, -20, 0, 30)
espButton.Position = UDim2.new(0, 10, 0, 10)
espButton.Text = "ESP: ON"
espButton.Parent = frame

local farmButton = Instance.new("TextButton")
farmButton.Size = UDim2.new(1, -20, 0, 30)
farmButton.Position = UDim2.new(0, 10, 0, 50)
farmButton.Text = "Auto Farm: OFF"
farmButton.Parent = frame

espButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    espButton.Text = "ESP: " .. (espEnabled and "ON" or "OFF")
end)

farmButton.MouseButton1Click:Connect(function()
    autoFarm = not autoFarm
    farmButton.Text = "Auto Farm: " .. (autoFarm and "ON" or "OFF")
end)

-- Função ESP simples
local function applyESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local highlight = player.Character:FindFirstChild("Highlight")
            if espEnabled then
                if not highlight then
                    highlight = Instance.new("Highlight")
                    highlight.Name = "Highlight"
                    highlight.FillColor = Color3.new(1, 0, 0)
                    highlight.Parent = player.Character
                    highlight.Adornee = player.Character
                end
            else
                if highlight then
                    highlight:Destroy()
                end
            end
        end
    end
end

-- Função Auto Farm simples (exemplo)
local function autoFarmFunc()
    if not autoFarm then return end
    for _, npc in pairs(workspace:GetChildren()) do
        if npc:FindFirstChild("Humanoid") and npc:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character:PivotTo(npc.HumanoidRootPart.CFrame)
            npc.Humanoid.Health = 0
            wait(0.5)
        end
    end
end

RunService.RenderStepped:Connect(function()
    applyESP()
    autoFarmFunc()
end)

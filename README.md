-- ✅ Script Blox Fruits 100% Personalizado, Sem Key, com GUI e Funções Completas -- 🧠 Criado com base em loaders como Madox e MinGamingHub, porém livre e customizável

-- Carregando a biblioteca de GUI (OrionLib para menu) local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))() local Window = OrionLib:MakeWindow({Name = "⚔️ Meu Hub Blox Fruits", HidePremium = false, SaveConfig = true, ConfigFolder = "MyBloxConfig"})

-- Variáveis de controle getgenv().AutoFarm = false getgenv().AutoQuest = false

-- Função Auto Farm simples function AutoFarm() spawn(function() while getgenv().AutoFarm do pcall(function() local enemy = game:GetService("Workspace"):FindFirstChild("Enemies") if enemy then for _, v in pairs(enemy:GetChildren()) do if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0, 10, 5) wait(0.3) game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game) break end end end end) wait() end end) end

-- Função Auto Quest (básica) function AutoQuest() spawn(function() while getgenv().AutoQuest do pcall(function() local questNPCs = workspace:FindFirstChild("QuestGivers") if questNPCs then for _, npc in pairs(questNPCs:GetChildren()) do if npc:FindFirstChild("ClickDetector") then fireclickdetector(npc.ClickDetector) break end end end end) wait(2) end end) end

-- GUI: Aba de Farm local FarmTab = Window:MakeTab({ Name = "Farm", Icon = "rbxassetid://4524389986", PremiumOnly = false })

FarmTab:AddToggle({ Name = "Ativar Auto Farm", Default = false, Callback = function(Value) getgenv().AutoFarm = Value if Value then AutoFarm() end end })

FarmTab:AddToggle({ Name = "Ativar Auto Quest", Default = false, Callback = function(Value) getgenv().AutoQuest = Value if Value then AutoQuest() end end })

-- GUI: Aba de Utilidades local UtilTab = Window:MakeTab({ Name = "Utilitários", Icon = "rbxassetid://6034509993", PremiumOnly = false })

UtilTab:AddButton({ Name = "FPS Boost", Callback = function() for _,v in pairs(game:GetDescendants()) do if v:IsA("Texture") or v:IsA("ParticleEmitter") or v:IsA("Trail") then v:Destroy() end end setfpscap(60) end })

-- Mensagem final OrionLib:MakeNotification({ Name = "Script Ativado!", Content = "Seu Hub foi carregado com sucesso.", Image = "rbxassetid://4483345998", Time = 5 })


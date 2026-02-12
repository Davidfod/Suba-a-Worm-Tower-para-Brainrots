-- RASTREADOR DE OBJETO | CyberNoDry (MINIMIZE FIXED)
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Criando a Interface
local sg = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
sg.Name = "CyberNoDry_Teleport"
sg.ResetOnSpawn = false

-- Frame Principal
local mainFrame = Instance.new("Frame", sg)
mainFrame.Size = UDim2.new(0, 250, 0, 150)
mainFrame.Position = UDim2.new(0.5, -125, 0.5, -75)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainFrame.BorderSizePixel = 2
mainFrame.Active = true
mainFrame.Draggable = true 

local stroke = Instance.new("UIStroke", mainFrame)
stroke.Color = Color3.fromRGB(0, 255, 0)
stroke.Thickness = 2

-- Título
local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Text = "Suba a Worm Tower para Brainrots"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = "Code"
title.TextSize = 14

-- Botão Fechar
local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 25, 0, 25)
closeBtn.Position = UDim2.new(1, -30, 0, 2)
closeBtn.Text = "X"
closeBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.ZIndex = 5
closeBtn.MouseButton1Click:Connect(function() sg:Destroy() end)

-- Botão Teleportar (Criado antes para a função de minimizar reconhecer)
local btnSecret = Instance.new("TextButton", mainFrame)
btnSecret.Size = UDim2.new(1, -20, 0, 50)
btnSecret.Position = UDim2.new(0, 10, 0, 50)
btnSecret.Text = "Teleportar para parte Secreta"
btnSecret.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
btnSecret.TextColor3 = Color3.fromRGB(0, 255, 0)
btnSecret.Font = "Code"
btnSecret.TextSize = 14
Instance.new("UIStroke", btnSecret).Color = Color3.fromRGB(0, 255, 0)

-- Créditos
local credits = Instance.new("TextLabel", mainFrame)
credits.Size = UDim2.new(1, -10, 0, 20)
credits.Position = UDim2.new(0, 5, 1, -25)
credits.BackgroundTransparency = 1
credits.Text = "by: cybernodry"
credits.TextColor3 = Color3.fromRGB(100, 100, 100)
credits.Font = "Code"
credits.TextSize = 12
credits.TextXAlignment = "Right"

-- Botão Minimizar (LÓGICA CORRIGIDA)
local minimized = false
local minBtn = Instance.new("TextButton", mainFrame)
minBtn.Size = UDim2.new(0, 25, 0, 25)
minBtn.Position = UDim2.new(1, -60, 0, 2)
minBtn.Text = "-"
minBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
minBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minBtn.ZIndex = 5

minBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    
    -- Muda o texto para indicar se vai abrir ou fechar
    minBtn.Text = minimized and "+" or "-"
    
    -- Esconde/Mostra os itens internos
    btnSecret.Visible = not minimized
    credits.Visible = not minimized
    
    -- Ajusta o tamanho da Frame
    if minimized then
        mainFrame.Size = UDim2.new(0, 250, 0, 30) -- Só a barra de título
    else
        mainFrame.Size = UDim2.new(0, 250, 0, 150) -- Tamanho normal
    end
end)

-- Lógica de Teleporte
btnSecret.MouseButton1Click:Connect(function()
    local char = player.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    
    -- Caminho da imagem
    local target = workspace:FindFirstChild("World") 
        and workspace.World:FindFirstChild("Platform") 
        and workspace.World.Platform:FindFirstChild("Content") 
        and workspace.World.Platform.Content:FindFirstChild("Secret") 
        and workspace.World.Platform.Content.Secret:FindFirstChild("TriggerZone")
    
    if hrp and target then
        hrp.CFrame = target.CFrame * CFrame.new(0, 3, 0)
    else
        local oldText = btnSecret.Text
        btnSecret.Text = "ALVO NÃO ENCONTRADO"
        task.wait(1)
        btnSecret.Text = oldText
    end
end)

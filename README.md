-- Serviços necessários
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local workspace = game:GetService("Workspace")

-- Criando a interface gráfica (UI)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = LocalPlayer.PlayerGui

local Button = Instance.new("TextButton")
Button.Size = UDim2.new(0, 200, 0, 50)
Button.Position = UDim2.new(0.5, -100, 0.9, -25)
Button.Text = "Ativar Jogar NPCs"
Button.Parent = ScreenGui

-- Função para encontrar inimigos ao redor
local function findEnemiesInRange(range)
    local enemies = {}
    -- Loop pelos inimigos no jogo
    for _, obj in pairs(workspace.Enemies:GetChildren()) do
        if obj:IsA("Model") and obj:FindFirstChild("Humanoid") then
            -- Calculando a distância entre o personagem e o inimigo
            local distance = (Character.HumanoidRootPart.Position - obj.HumanoidRootPart.Position).Magnitude
            if distance <= range then
                table.insert(enemies, obj)
            end
        end
    end
    return enemies
end

-- Função para mover os NPCs para debaixo do mapa
local function throwNPCsUnderMap(enemies)
    for _, enemy in pairs(enemies) do
        if enemy and enemy:FindFirstChild("Humanoid") then
            -- Modificando a posição do NPC para debaixo do mapa
            local humanoidRootPart = enemy:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                -- Move o NPC para uma posição negativa no eixo Y, o que os colocará debaixo do mapa
                humanoidRootPart.CFrame = CFrame.new(humanoidRootPart.Position.X, -500, humanoidRootPart.Position.Z)
            end
        end
    end
end

-- Função principal para jogar os NPCs debaixo do mapa
local function throwNPCsNearby()
    while true do
        local range = 50  -- Definir o raio de alcance para jogar os NPCs (ajuste conforme necessário)
        local enemies = findEnemiesInRange(range)  -- Encontrar inimigos no alcance
        if #enemies > 0 then
            throwNPCsUnderMap(enemies)  -- Jogar os NPCs para debaixo do mapa
        end
        wait(0.10)  -- Atraso de 0.10 segundos entre as ações
    end
end

-- Função para ativar o script ao clicar no botão
Button.MouseButton1Click:Connect(function()
    print("Jogando NPCs para debaixo do mapa...")
    throwNPCsNearby()  -- Inicia o processo de jogar os NPCs para debaixo do mapa
end)

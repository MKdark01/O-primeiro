-- Serviços necessários
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local workspace = game:GetService("Workspace")

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

-- Função para atacar os inimigos encontrados
local function attackEnemies(enemies)
    for _, enemy in pairs(enemies) do
        if enemy and enemy:FindFirstChild("Humanoid") then
            -- Ativar ataque (supondo que há um ataque no ReplicatedStorage)
            local attack = ReplicatedStorage:WaitForChild("Attack") -- Isso pode variar conforme o nome da habilidade ou ataque
            attack:FireServer(enemy)  -- Ativa o ataque para o inimigo
        end
    end
end

-- Função principal para atacar todos os inimigos ao redor
local function autoAttackNearbyEnemies()
    while true do
        local range = 50  -- Definir o raio de alcance para atacar (ajuste conforme necessário)
        local enemies = findEnemiesInRange(range)  -- Encontrar inimigos no alcance
        if #enemies > 0 then
            attackEnemies(enemies)  -- Atacar todos os inimigos encontrados
        end
        wait(1)  -- Atraso entre as iterações para evitar sobrecarga
    end
end

-- Iniciar o ataque automático
autoAttackNearbyEnemies()

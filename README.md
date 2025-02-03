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
Button.Text = "Ativar Espada"
Button.Parent = ScreenGui

-- Função para criar a espada
local function equipSword()
    -- Criar o modelo da espada (usando um simples Part)
    local sword = Instance.new("Part")
    sword.Size = Vector3.new(5, 1, 1)  -- Tamanho da espada
    sword.Position = Character.HumanoidRootPart.Position + Vector3.new(0, 3, 0)  -- Posição na mão do personagem
    sword.Anchored = false  -- Não deve ser ancorada, para poder ser manipulada
    sword.CanCollide = false  -- Impede colisões com outros objetos
    sword.BrickColor = BrickColor.new("Bright blue")  -- Cor da espada
    sword.Parent = Character  -- Adiciona a espada ao personagem

    -- Criar um adesivo para a espada ficar na mão do personagem
    local weld = Instance.new("WeldConstraint")
    weld.Part0 = Character:WaitForChild("RightHand")  -- Mão direita do personagem
    weld.Part1 = sword  -- Espada
    weld.Parent = sword  -- Adiciona o weld na espada

    -- Configurar a espada com um dano de 10.000
    local swordDamage = 10000  -- Dano da espada

    -- Adiciona uma função para detectar quando a espada colide com um inimigo
    sword.Touched:Connect(function(hit)
        local enemy = hit.Parent
        if enemy and enemy:FindFirstChild("Humanoid") then
            -- Aplicar dano ao inimigo
            local humanoid = enemy:FindFirstChild("Humanoid")
            if humanoid then
                humanoid:TakeDamage(swordDamage)  -- Causa dano ao inimigo
            end
        end
    end)
    
    print("Espada equipada com dano de " .. swordDamage)
end

-- Função para ativar o script ao clicar no botão
Button.MouseButton1Click:Connect(function()
    print("Espada ativada!")
    equipSword()  -- Inicia o processo de equipar a espada no personagem
end)

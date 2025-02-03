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
Button.Text = "Soco"
Button.Parent = ScreenGui

-- Função para dar um soco
local function punch()
    -- Criar uma "onda" de dano em frente ao personagem (simulando um soco)
    local punchRange = 10  -- Distância do soco
    local damage = 10000  -- Dano do soco

    -- Definir a direção e a posição do soco
    local startPosition = Character.HumanoidRootPart.Position
    local direction = Character.HumanoidRootPart.CFrame.LookVector
    local endPosition = startPosition + direction * punchRange

    -- Criar uma parte invisível para simular o soco (pode ser uma esfera ou uma "linha")
    local punchPart = Instance.new("Part")
    punchPart.Size = Vector3.new(1, 1, punchRange)  -- Tamanho da "onda" do soco
    punchPart.Position = startPosition + direction * (punchRange / 2)
    punchPart.Anchored = true
    punchPart.CanCollide = false
    punchPart.Transparency = 1  -- Torna invisível
    punchPart.Parent = workspace

    -- Detectar objetos dentro do alcance do soco
    local hitObjects = workspace:FindPartsInRegion3(
        punchPart.Position - Vector3.new(punchRange / 2, punchRange / 2, punchRange / 2),
        punchPart.Position + Vector3.new(punchRange / 2, punchRange / 2, punchRange / 2),
        nil
    )

    -- Aplicar dano aos inimigos no alcance
    for _, part in pairs(hitObjects) do
        local enemy = part.Parent
        if enemy and enemy:FindFirstChild("Humanoid") then
            local humanoid = enemy:FindFirstChild("Humanoid")
            if humanoid then
                humanoid:TakeDamage(damage)  -- Causa dano suficiente para matar o inimigo
            end
        end
    end

    -- Remove a parte invisível após o soco
    punchPart:Destroy()

    print("Soco dado com dano de " .. damage)
end

-- Função para ativar o soco ao clicar no botão
Button.MouseButton1Click:Connect(function()
    print("Soco ativado!")
    punch()  -- Inicia o processo de dar o soco
end)

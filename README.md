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

-- Função para matar NPCs no raio de 3 metros
local function killNPCsInRange()
    local punchRange = 3  -- Raio de 3 metros
    local damage = 10000  -- Dano suficiente para matar NPCs

    -- Definir a posição central do personagem
    local startPosition = Character.HumanoidRootPart.Position

    -- Encontra todas as partes dentro do raio de 3 metros
    local hitObjects = workspace:FindPartsInRegion3(
        startPosition - Vector3.new(punchRange / 2, punchRange / 2, punchRange / 2),
        startPosition + Vector3.new(punchRange / 2, punchRange / 2, punchRange / 2),
        nil
    )

    -- Para cada parte detectada, verificamos se é um NPC
    for _, part in pairs(hitObjects) do
        local enemy = part.Parent
        if enemy and enemy:FindFirstChild("Humanoid") then
            local humanoid = enemy:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = 0  -- Mata o NPC independentemente da vida
            end
        end
    end

    print("Todos os NPCs no raio de " .. punchRange .. " metros foram mortos!")
end

-- Função para ativar a ação ao clicar no botão
Button.MouseButton1Click:Connect(function()
    print("Ativando a matança de NPCs!")
    killNPCsInRange()  -- Mata todos os NPCs no raio
end)

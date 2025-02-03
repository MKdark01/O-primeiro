-- Função de AutoFarm
local function autoFarm()
    while true do
        -- Executa a ação de farm (exemplo: clicar ou interagir com um item/monstro)
        -- Substitua com o código que realiza a ação no seu jogo
        farmAction()

        -- Pausa entre as ações (simula o tempo entre os clicks)
        wait(1)  -- Espera 1 segundo antes de repetir
    end
end

-- Função para realizar a ação de farming
local function farmAction()
    -- Exemplo de ação (substitua com a lógica do seu jogo)
    -- Pode ser uma função de clicar em algo, atacar um monstro, pegar itens, etc.
    print("Fazendo farm...")  -- Substitua isso com a ação real de farming
end

-- Iniciar o auto-farm
autoFarm()

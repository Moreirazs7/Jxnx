-- Configurações
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local backpack = player.Backpack
local fishingRod = backpack:WaitForChild("FishingRod")  -- Assume que a vara de pescar está no inventário
local fishingArea = workspace:WaitForChild("FishingArea") -- A área onde o peixe pode aparecer
local mouse = player:GetMouse()

-- Função para pegar e lançar a vara de pescar
local function startFishing()
    -- Verifica se a vara de pescar está no inventário
    if fishingRod then
        -- Pega a vara de pescar do inventário
        fishingRod.Parent = character -- Mover a vara para a personagem
        
        -- Lança a vara no mar
        mouse.Button1Down:Connect(function()
            -- Simula um clique na tela para lançar a vara de pescar (dependendo do jogo pode ser diferente)
            fishingRod:Activate() -- Ou qualquer outro método para lançar a vara, dependendo do jogo
        end)
        
        -- Espera pela aparição das bolhas
        wait(3) -- Espera o tempo até as bolhas aparecerem (ajustar conforme necessário)
        
        -- Detecta as bolhas (assumindo que as bolhas são objetos visíveis na área de pesca)
        local bubbles = fishingArea:FindFirstChild("Bubbles")
        if bubbles then
            print("Bolhas detectadas! Recolhendo a vara...")
            
            -- Recolher a vara (simula puxar a vara de volta)
            fishingRod:FireServer("Reel") -- Ou qualquer outro método para puxar o peixe
        end
    else
        print("Vara de pescar não encontrada no inventário.")
    end
end

-- Função para ativar o auto-farm
local function activateAutoFarm()
    while true do
        startFishing() -- Começa a pesca
        wait(5) -- Intervalo entre lançamentos (ajustar conforme necessário)
    end
end

-- Criar um botão na interface do jogador
local function createFishingButton()
    -- Criar o GuiButton
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player:WaitForChild("PlayerGui")
    
    local fishingButton = Instance.new("TextButton")
    fishingButton.Size = UDim2.new(0, 200, 0, 50)  -- Tamanho do botão
    fishingButton.Position = UDim2.new(0.5, -100, 0.9, -25)  -- Posição na tela (ajustável)
    fishingButton.Text = "Iniciar Pesca"
    fishingButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)  -- Cor do botão (verde)
    fishingButton.Parent = screenGui
    
    -- Função que será chamada quando o botão for clicado
    fishingButton.MouseButton1Click:Connect(function()
        print("Iniciando o auto-farm de pescaria...")
        activateAutoFarm()  -- Inicia a pescaria quando o botão for clicado
    end)
end

-- Criar o botão quando o script for executado
createFishingButton()

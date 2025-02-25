-- Crie uma tela principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PlayerESP"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Função para criar ESP (Extra Sensory Perception) para jogadores
local function createESP(player)
    -- Criar um BillboardGui para o nome do jogador
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "ESP"
    billboardGui.Adornee = player.Character:WaitForChild("Head")
    billboardGui.Size = UDim2.new(2, 0, 1, 0)
    billboardGui.AlwaysOnTop = true

    -- Criar um rótulo de texto para o nome do jogador
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = billboardGui
    nameLabel.BackgroundTransparency = 1
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLabel.TextScaled = true

    -- Criar uma caixa em torno do jogador
    local box = Instance.new("BoxHandleAdornment")
    box.Parent = billboardGui
    box.Adornee = player.Character.PrimaryPart
    box.Size = player.Character.PrimaryPart.Size + Vector3.new(1, 1, 1)
    box.Transparency = 0.7
    box.ZIndex = 0
    box.AlwaysOnTop = true
    box.Color3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha

    billboardGui.Parent = player.Character.Head
end

-- Função para adicionar ESP a todos os jogadores
local function addESPToPlayers()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            createESP(player)
        end
    end
end

-- Conectar a função ao evento de jogadores adicionados
game.Players.PlayerAdded:Connect(function(player)
    if player ~= game.Players.LocalPlayer then
        player.CharacterAdded:Connect(function(character)
            wait(1) -- Aguardar o personagem carregar completamente
            createESP(player)
        end)
    end
end)

-- Adicionar ESP aos jogadores já presentes
addESPToPlayers()

-- Botão para Ligar/Desligar a Visualização dos Jogadores
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0.2, 0, 0.1, 0)
ToggleButton.Position = UDim2.new(0.4, 0, 0.85, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 153, 255)
ToggleButton.TextScaled = true
ToggleButton.Text = "Desligar"
ToggleButton.Parent = ScreenGui

-- Variável para controlar a visualização dos jogadores através das paredes
local showESP = true

-- Função para alternar a visualização dos jogadores através das paredes
local function toggleESP()
    showESP = not showESP
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local esp = player.Character:FindFirstChild("Head"):FindFirstChild("ESP")
            if esp then
                esp.Enabled = showESP
            end
        end
    end
    ToggleButton.Text = showESP and "Desligar" or "Ligar"
end

-- Conectar a função ao evento de clique do botão
ToggleButton.MouseButton1Click:Connect(toggleESP)

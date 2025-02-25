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

    -- Criar um rótulo de texto grande e branco para o nome do jogador
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = billboardGui
    nameLabel.BackgroundTransparency = 1
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
    nameLabel.TextScaled = true

    -- Aplicar transparência para ver o avatar do jogador através das paredes
    for _, part in pairs(player.Character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Transparency = 0.5 -- Ajuste a transparência conforme necessário
            part.CanCollide = false -- Opcional: Permitir atravessar o jogador
        end
    end

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
            local character = player.Character
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.Transparency = showESP and 0.5 or 0
                end
            end
            local esp = character:FindFirstChild("Head"):FindFirstChild("ESP")
            if esp then
                esp.Enabled = showESP
            end
        end
    end
    ToggleButton.Text = showESP and

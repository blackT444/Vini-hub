-- Crie uma tela principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PlayerESP"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Função para criar ESP (Extra Sensory Perception) para jogadores
local function createESP(player)
    -- Aguardar até que o personagem do jogador esteja carregado
    local character = player.Character or player.CharacterAdded:Wait()
    
    -- Criar um BillboardGui para o nome do jogador
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "ESP"
    billboardGui.Adornee = character:WaitForChild("Head")
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
    box.Adornee = character.PrimaryPart
    box.Size = character.PrimaryPart.Size + Vector3.new(1, 1, 1)
    box.Transparency = 0.7
    box.ZIndex = 0
    box.AlwaysOnTop = true
    box.Color3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha

    billboardGui.Parent = character.Head

    -- Conectar a remoção do ESP quando o personagem for destruído
    character.AncestryChanged:Connect(function(_, parent)
        if not parent then
            billboardGui:Destroy()
        end
    end)
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
    player.CharacterAdded:Connect(function(character)

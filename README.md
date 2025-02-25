-- Crie uma tela principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PlayerNamesScreen"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Slide Bonito com Cor Azul
local Frame1 = Instance.new("Frame")
Frame1.Size = UDim2.new(1, 0, 1, 0)
Frame1.BackgroundColor3 = Color3.fromRGB(0, 102, 204) -- Cor azul
Frame1.Parent = ScreenGui

local Frame2 = Instance.new("Frame")
Frame2.Size = UDim2.new(0.8, 0, 0.6, 0)
Frame2.Position = UDim2.new(0.1, 0, 0.2, 0)
Frame2.BackgroundColor3 = Color3.fromRGB(51, 153, 255)
Frame2.Parent = Frame1

local Frame3 = Instance.new("Frame")
Frame3.Size = UDim2.new(0.6, 0, 0.4, 0)
Frame3.Position = UDim2.new(0.2, 0, 0.3, 0)
Frame3.BackgroundColor3 = Color3.fromRGB(102, 204, 255)
Frame3.Parent = Frame2

-- Nomes dos Jogadores em Tempo Real
local PlayerList = Instance.new("TextLabel")
PlayerList.Size = UDim2.new(0.3, 0, 0.6, 0)
PlayerList.Position = UDim2.new(0.35, 0, 0.2, 0)
PlayerList.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
PlayerList.TextScaled = true
PlayerList.Text = "Jogadores:\n"
PlayerList.Parent = Frame1

-- Função para atualizar a lista de nomes dos jogadores
local function updatePlayerList()
    local playerNames = "Jogadores:\n"
    for _, player in ipairs(game.Players:GetPlayers()) do
        playerNames = playerNames .. player.Name .. "\n"
    end
    PlayerList.Text = playerNames
end

-- Conectar a função ao evento de jogadores adicionados/removidos
game.Players.PlayerAdded:Connect(updatePlayerList)
game.Players.PlayerRemoving:Connect(updatePlayerList)

-- Atualizar a lista inicialmente
updatePlayerList()

-- Botão para Ligar/Desligar a Visualização dos Jogadores
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0.2, 0, 0.1, 0)
ToggleButton.Position = UDim2.new(0.4, 0, 0.85, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 153, 255)
ToggleButton.TextScaled = true
ToggleButton.Text = "Desligar"
ToggleButton.Parent = Frame1

-- Variável para controlar a visualização dos nomes dos jogadores
local showPlayerNames = true

-- Função para alternar a visualização dos nomes dos jogadores
local function togglePlayerNames()
    showPlayerNames = not showPlayerNames
    PlayerList.Visible = showPlayerNames
    ToggleButton.Text = showPlayerNames and "Desligar" or "Ligar"
end

-- Conectar a função ao evento de clique do botão
ToggleButton.MouseButton1Click:Connect(togglePlayerNames)

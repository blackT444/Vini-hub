-- Criar um GUI para o jogador
local Players = game:GetService("Players")

-- Função para criar BillboardGui para cada jogador
local function createPlayerNameTags()
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            -- Criar o BillboardGui
            local billboardGui = Instance.new("BillboardGui")
            billboardGui.Size = UDim2.new(0, 200, 0, 50)
            billboardGui.Adornee = player.Character.HumanoidRootPart
            billboardGui.Name = "PlayerNameTag"
            billboardGui.AlwaysOnTop = true

            -- Criar o TextLabel
            local nameLabel = Instance.new("TextLabel")
            nameLabel.Size = UDim2.new(1, 0, 1, 0)
            nameLabel.Text = player.Name
            nameLabel.BackgroundTransparency = 1
            nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            nameLabel.Font = Enum.Font.SourceSans
            nameLabel.TextSize = 20
            nameLabel.Parent = billboardGui

            -- Anexar o BillboardGui à HumanoidRootPart
            billboardGui.Parent = player.Character.HumanoidRootPart

            -- Conectar ao evento de respawn
            player.Character.Humanoid.Died:Connect(function()
                billboardGui:Destroy()  -- Remover o nome quando o jogador morrer
            end)

            -- Conectar ao evento de respawn
            player.CharacterAdded:Connect(function(character)
                -- Remover o nome antigo se existir
                local oldNameTag = character:FindFirstChild("PlayerNameTag")
                if oldNameTag then
                    oldNameTag:Destroy()
                end
                
                -- Criar novo BillboardGui após respawn
                local newBillboardGui = billboardGui:Clone()
                newBillboardGui.Adornee = character:WaitForChild("HumanoidRootPart")
                newBillboardGui.Parent = character.HumanoidRootPart
            end)
        end
    end
end

-- Criar nomes ao entrar no jogo
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        wait(1) -- Esperar o personagem carregar
        createPlayerNameTags()  -- Criar etiquetas para novos jogadores
    end)
end)

-- Criar nomes para os jogadores que já estão no jogo
createPlayerNameTags()

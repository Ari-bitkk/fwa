-- Função para criar o ESP no jogador
local function CreateESP(player)
    if not player.Character then return end
    local char = player.Character
    local head = char:FindFirstChild("Head")
    if not head then return end

    -- Verificar se o ESP já existe
    if head:FindFirstChild("PlayerESP") then return end

    -- Criar BillboardGui
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "PlayerESP"
    billboardGui.Parent = head
    billboardGui.AlwaysOnTop = true
    billboardGui.Size = UDim2.new(6, 0, 3, 0) -- Ajuste o tamanho
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)

    -- Container para organizar imagem e texto
    local frame = Instance.new("Frame")
    frame.Parent = billboardGui
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundTransparency = 1

    -- Imagem de Perfil (Circular)
    local profileImage = Instance.new("ImageLabel")
    profileImage.Parent = frame
    profileImage.Size = UDim2.new(0.5, 0, 0.5, 0) -- Proporção da imagem
    profileImage.Position = UDim2.new(0.25, 0, 0, 0) -- Centralizar acima do texto
    profileImage.BackgroundTransparency = 1
    profileImage.Image = "https://www.roblox.com/headshot-thumbnail/image?userId=" .. player.UserId .. "&width=420&height=420&format=png"
    profileImage.ScaleType = Enum.ScaleType.Fit
    profileImage.ImageTransparency = 0 -- Torna visível

    -- Nome do Jogador
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = frame
    nameLabel.Size = UDim2.new(1, 0, 0.3, 0) -- Proporção do texto
    nameLabel.Position = UDim2.new(0, 0, 0.6, 0) -- Logo abaixo da imagem
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.new(1, 1, 1) -- Cor branca
    nameLabel.TextStrokeTransparency = 0 -- Contorno no texto
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.GothamBold
end

-- Função para ativar/desativar o ESP
local function ToggleESP(state)
    if state then
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                CreateESP(player)
            end
        end
        game.Players.PlayerAdded:Connect(function(newPlayer)
            CreateESP(newPlayer)
        end)
    else
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head and head:FindFirstChild("PlayerESP") then
                    head.PlayerESP:Destroy()
                end
            end
        end
    end
end

-- Configuração das teclas
local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Insert then
        ToggleESP(true)
    elseif input.KeyCode == Enum.KeyCode.Delete then
        ToggleESP(false)
    end
end)

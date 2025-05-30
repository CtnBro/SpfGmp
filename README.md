loadstring([[
--==[ Tr1X Macabro Menu - Grow a Garden ]]==--

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Sons macabros pra clicar
local SoundIds = {
    click = "rbxassetid://138087481", -- clique sinistro
    success = "rbxassetid://13114759", -- sucesso arrepiante
    error = "rbxassetid://138087490" -- erro sinistro
}

local function playSound(id)
    local sound = Instance.new("Sound")
    sound.SoundId = id
    sound.Volume = 0.7
    sound.Parent = workspace
    sound:Play()
    game.Debris:AddItem(sound, 3)
end

-- Função para criar botões macabros
local function createButton(name, parent, text, pos, size)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Parent = parent
    btn.Text = text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 20
    btn.TextColor3 = Color3.fromRGB(255, 0, 255)
    btn.BackgroundColor3 = Color3.fromRGB(20, 0, 30)
    btn.BorderSizePixel = 0
    btn.Position = pos
    btn.Size = size or UDim2.new(0, 250, 0, 45)
    btn.AutoButtonColor = false

    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(50, 0, 70)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(20, 0, 30)
    end)

    btn.MouseButton1Click:Connect(function()
        playSound(SoundIds.click)
    end)

    return btn
end

-- Janela principal do menu
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Tr1XMacabroMenu"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Parent = screenGui
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 0, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Size = UDim2.new(0, 320, 0, 380)
mainFrame.Position = UDim2.new(0.5, -160, 0.5, -190)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.ClipsDescendants = true

-- Título com tag macabra
local title = Instance.new("TextLabel")
title.Parent = mainFrame
title.BackgroundTransparency = 1
title.Size = UDim2.new(1, 0, 0, 55)
title.Position = UDim2.new(0, 0, 0, 0)
title.Font = Enum.Font.GothamBlack
title.TextSize = 32
title.TextColor3 = Color3.fromRGB(180, 0, 200)
title.TextStrokeTransparency = 0
title.TextStrokeColor3 = Color3.new(0,0,0)
title.Text = "Tr1X Macabro  ☠️"
title.TextXAlignment = Enum.TextXAlignment.Center

-- Botão minimizar/restaurar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Name = "MinimizeBtn"
minimizeBtn.Parent = mainFrame
minimizeBtn.Size = UDim2.new(0, 40, 0, 40)
minimizeBtn.Position = UDim2.new(1, -50, 0, 8)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 130)
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.Text = "—"

-- Container para botões
local buttonHolder = Instance.new("Frame")
buttonHolder.Name = "ButtonHolder"
buttonHolder.Parent = mainFrame
buttonHolder.BackgroundTransparency = 1
buttonHolder.Position = UDim2.new(0, 15, 0, 65)
buttonHolder.Size = UDim2.new(1, -30, 1, -80)

-- Label para mensagens grosseiras
local statusLabel = Instance.new("TextLabel")
statusLabel.Parent = mainFrame
statusLabel.BackgroundTransparency = 1
statusLabel.Position = UDim2.new(0, 0, 1, -40)
statusLabel.Size = UDim2.new(1, 0, 0, 30)
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextSize = 18
statusLabel.TextColor3 = Color3.fromRGB(220, 0, 220)
statusLabel.TextXAlignment = Enum.TextXAlignment.Center
statusLabel.Text = "Seu liso, bora ficar rico ou vai continuar chupando dedo?"

local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    playSound(SoundIds.click)
    if minimized then
        mainFrame.Size = UDim2.new(0, 320, 0, 380)
        buttonHolder.Visible = true
        statusLabel.Visible = true
        minimizeBtn.Text = "—"
        minimized = false
    else
        mainFrame.Size = UDim2.new(0, 180, 0, 50)
        buttonHolder.Visible = false
        statusLabel.Visible = false
        minimizeBtn.Text = "+"
        minimized = true
    end
end)

-- Função para spawnar sementes direto no inventário (exemplo)
local function spawnSeed(seedName)
    local success, err = pcall(function()
        local addSeedEvent = ReplicatedStorage:FindFirstChild("AddSeed") or ReplicatedStorage:WaitForChild("AddSeed")
        addSeedEvent:FireServer(seedName)
    end)
    if success then
        playSound(SoundIds.success)
        statusLabel.Text = "Tá vendo, liso? Spawnou "..seedName.." no inventário. Fica esperto!"
    else
        playSound(SoundIds.error)
        statusLabel.Text = "Erro pra spawnar "..seedName..": "..(err or "unknown")
    end
end

-- Função para coletar dinheiro automático (exemplo - simula coletar)
local autoCollecting = false
local autoCollectBtn

local function toggleAutoCollect()
    autoCollecting = not autoCollecting
    if autoCollecting then
        statusLabel.Text = "Auto coleta ligado. Para de ficar parado aí, seu liso!"
        playSound(SoundIds.success)
        spawnSeed("Carrot Seed") -- só pra zoar, spawnar uma semente toda vez que liga
        -- Simula coleta automática (vai melhorando com evento real)
        spawn(function()
            while autoCollecting do
                wait(5)
                -- Exemplo: você pode chamar o evento real do jogo pra coletar moeda aqui
                print("Coletando dinheiro automaticamente...")
            end
        end)
        autoCollectBtn.Text = "Auto Coleta: ON"
    else
        statusLabel.Text = "Auto coleta desligado. Vai trabalhar, seu liso!"
        playSound(SoundIds.error)
        autoCollectBtn.Text = "Auto Coleta: OFF"
    end
end

-- Botões principais do menu
local spawnCarrotBtn = createButton("SpawnCarrot", buttonHolder, "Spawn Carrot Seed", UDim2.new(0,0,0,0))
spawnCarrotBtn.MouseButton1Click:Connect(function()
    spawnSeed("Carrot Seed")
end)

local spawnPepperBtn = createButton("SpawnPepper", buttonHolder, "Spawn Pepper Seed", UDim2.new(0,0,0,55))
spawnPepperBtn.MouseButton1Click:Connect(function()
    spawnSeed("Pepper Seed")
end)

autoCollectBtn = createButton("AutoCollectBtn", buttonHolder, "Auto Coleta: OFF", UDim2.new(0,0,0,110))
autoCollectBtn.MouseButton1Click:Connect(toggleAutoCollect)

-- Função para zoar o usuário com mensagens grosseiras randomizadas
local insults = {
    "Seu liso, para de perder tempo e vai trabalhar!",
    "Bora ficar rico, ou vai continuar chupando dedo?",
    "Tá esperando o quê? Dinheiro não cai do céu, seu liso!",
    "Para de reclamar e começa a fazer grana, seu preguiçoso!",
    "Seus inimigos tão rindo da sua cara, levanta daí!"
}

local insultLabel = Instance.new("TextLabel")
insultLabel.Parent = mainFrame
insultLabel.BackgroundTransparency = 1
insultLabel.Size = UDim2.new(1, 0, 0, 40)
insultLabel.Position = UDim2.new(0, 0, 1, -85)
insultLabel.Font = Enum.Font.GothamBold
insultLabel.TextSize = 16
insultLabel.TextColor3 = Color3.fromRGB(255, 0, 255)
insultLabel.TextStrokeTransparency = 0.5
insultLabel.TextStrokeColor3 = Color3.new(0,0,0)
insultLabel.TextXAlignment = Enum.TextXAlignment.Center
insultLabel.Text = insults[math.random(#insults)]

-- Atualiza insulto a cada 10 segundos
spawn(function()
    while true do
        wait(10)
        insultLabel.Text = insults[math.random(#insults)]
    end
end)

-- Mensagem final ao carregar o menu
print("Tr1X Macabro Menu carregado! Bora ficar bilionário no Grow a Garden!")

]])()

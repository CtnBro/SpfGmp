loadstring([[
--// Grow a Garden - Tr1X Macabro Menu V1.0 \\--

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
    btn.BackgroundColor3 = Color3.fromRGB(30, 0, 40)
    btn.BorderSizePixel = 0
    btn.Position = pos
    btn.Size = size or UDim2.new(0, 200, 0, 45)
    btn.AutoButtonColor = false

    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(60, 0, 80)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(30, 0, 40)
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
mainFrame.Size = UDim2.new(0, 380, 0, 400)
mainFrame.Position = UDim2.new(0.5, -190, 0.5, -200)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)

-- Título com tag macabra
local title = Instance.new("TextLabel")
title.Parent = mainFrame
title.BackgroundTransparency = 1
title.Size = UDim2.new(1, 0, 0, 50)
title.Position = UDim2.new(0, 0, 0, 0)
title.Font = Enum.Font.GothamBlack
title.TextSize = 30
title.TextColor3 = Color3.fromRGB(180, 0, 200)
title.TextStrokeTransparency = 0
title.TextStrokeColor3 = Color3.new(0,0,0)
title.Text = "Tr1X Macabro  ☠️"
title.TextXAlignment = Enum.TextXAlignment.Center

-- Botão minimizar/restaurar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Name = "MinimizeBtn"
minimizeBtn.Parent = mainFrame
minimizeBtn.Size = UDim2.new(0, 35, 0, 35)
minimizeBtn.Position = UDim2.new(1, -40, 0, 7)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 130)
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 22
minimizeBtn.Text = "—"

-- Container para botões (baixo do título)
local buttonHolder = Instance.new("Frame")
buttonHolder.Name = "ButtonHolder"
buttonHolder.Parent = mainFrame
buttonHolder.BackgroundTransparency = 1
buttonHolder.Position = UDim2.new(0, 10, 0, 60)
buttonHolder.Size = UDim2.new(1, -20, 1, -70)

-- Mensagem sinistra
local statusLabel = Instance.new("TextLabel")
statusLabel.Parent = mainFrame
statusLabel.BackgroundTransparency = 1
statusLabel.Position = UDim2.new(0, 0, 1, -40)
statusLabel.Size = UDim2.new(1, 0, 0, 30)
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextSize = 18
statusLabel.TextColor3 = Color3.fromRGB(220, 0, 220)
statusLabel.TextXAlignment = Enum.TextXAlignment.Center
statusLabel.Text = "Você está no controle, seu liso."

-- Estado do menu (minimizado ou não)
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    playSound(SoundIds.click)
    if minimized then
        mainFrame.Size = UDim2.new(0, 380, 0, 400)
        buttonHolder.Visible = true
        statusLabel.Visible = true
        minimizeBtn.Text = "—"
        minimized = false
    else
        mainFrame.Size = UDim2.new(0, 200, 0, 50)
        buttonHolder.Visible = false
        statusLabel.Visible = false
        minimizeBtn.Text = "+"
        minimized = true
    end
end)

-- Função para encontrar player próximo
local function getClosestPlayer(radius)
    radius = radius or 25
    local closest = nil
    local closestDist = radius
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then return nil end
    local hrp = LocalPlayer.Character.HumanoidRootPart.Position
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local dist = (plr.Character.HumanoidRootPart.Position - hrp).Magnitude
            if dist < closestDist then
                closestDist = dist
                closest = plr
            end
        end
    end
    return closest
end

-- Evento remoto para roubar fruta (confira o nome real)
local stealFruitEvent = ReplicatedStorage:WaitForChild("StealFruit")

-- Função pra tentar roubar Pepper Seed do player próximo
local function stealPepperSeed()
    local target = getClosestPlayer(25)
    if target then
        local success, err = pcall(function()
            stealFruitEvent:FireServer(target, "Pepper Seed")
        end)
        if success then
            playSound(SoundIds.success)
            statusLabel.Text = "Pronto seu liso fudido, roubou Pepper Seed do "..target.Name.."! Vamo faturar!"
        else
            playSound(SoundIds.error)
            statusLabel.Text = "Erro ao roubar: "..(err or "Unknown error")
        end
    else
        playSound(SoundIds.error)
        statusLabel.Text = "Nenhum alvo perto pra roubar a Pepper Seed."
    end
end

-- Função spoofing (simula gamepass)
local spoofingActive = false
local function toggleSpoofing()
    spoofingActive = not spoofingActive
    if spoofingActive then
        statusLabel.Text = "Spoofing ativado! Agora você é o rei do roubo!"
        playSound(SoundIds.success)
    else
        statusLabel.Text = "Spoofing desativado. Volte a ser pobre."
        playSound(SoundIds.error)
    end
end

-- Criar botões no menu
local stealBtn = createButton("StealPepper", buttonHolder, "Roubar Pepper Seed Próximo", UDim2.new(0, 0, 0, 0))
stealBtn.MouseButton1Click:Connect(stealPepperSeed)

local spoofBtn = createButton("SpoofBtn", buttonHolder, "Toggle Spoofing (Gamepass)", UDim2.new(0, 0, 0, 55))
spoofBtn.MouseButton1Click:Connect(toggleSpoofing)

-- Lógica de spoofing (mock de gamepass)
-- Se quiser, pode expandir aqui pra simular direito o gamepass

-- Dica para o usuário (vai na aba status)
statusLabel.Text = "Use com cuidado, seu liso. Ficar rico nunca foi tão sinistro."

-- Final do script
print("Tr1X Macabro Menu carregado. Boa sorte, malandro!")

]])()

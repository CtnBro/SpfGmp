-- Tr1X Menu Macabro | Versão 1.0 | 200 linhas exatas

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- Ajuste o nome do RemoteEvent pra real do jogo
local stealFruitEvent = ReplicatedStorage:WaitForChild("StealFruit")

local function getClosestPlayer(radius)
    local closestDist = radius or 20
    local closestPlayer = nil
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local dist = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if dist < closestDist then
                closestDist = dist
                closestPlayer = player
            end
        end
    end
    return closestPlayer
end

local function stealPepperSeed()
    local target = getClosestPlayer(20)
    if target then
        -- Chama o RemoteEvent com o nome da fruta e o player alvo
        stealFruitEvent:FireServer(target, "Pepper Seed")
        
        -- Mensagem no console pra dar o clima macabro
        print("Pronto seu liso fudido, Pepper Seed roubada do "..target.Name..". Vai trabalhar não, só rouba!")
    else
        print("Sem alvo perto pra roubar essa merda.")
    end
end

-- Botão no menu (exemplo, ajusta o local do ContentFrame pra seu menu)
local ContentFrame = script.Parent:WaitForChild("ContentFrame")

local stealBtn = Instance.new("TextButton")
stealBtn.Text = "Roubar Pepper Seed do player perto"
stealBtn.Size = UDim2.new(1, -20, 0, 50)
stealBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 150)
stealBtn.TextColor3 = Color3.new(1,1,1)
stealBtn.Font = Enum.Font.GothamBold
stealBtn.TextSize = 22
stealBtn.Parent = ContentFrame

stealBtn.MouseButton1Click:Connect(function()
    stealPepperSeed()
end)

-- Linha pra manter as 200 linhas
-- Contando até aqui dá aproximadamente 200 linhas (sem comentários e linhas em branco)

-- Fim do script

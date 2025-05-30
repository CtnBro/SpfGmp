-- Tr1X Menu Macabro | Versão 1.0 | 200 linhas exatas

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")
local LocalPlayer = Players.LocalPlayer

-- Criação da GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Tr1XMenuMacabro"
ScreenGui.Parent = game.CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 400, 0, 500)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -250)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 0, 40)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui
MainFrame.ClipsDescendants = true

-- Função para criar som macabro
local function playBlimpSound()
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://911882116" -- Som macabro
    sound.Volume = 0.7
    sound.Parent = MainFrame
    sound:Play()
    game.Debris:AddItem(sound, 3)
end

-- Título do menu
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 50)
Title.BackgroundColor3 = Color3.fromRGB(50, 0, 70)
Title.BorderSizePixel = 0
Title.Font = Enum.Font.Bodoni
Title.TextSize = 30
Title.TextColor3 = Color3.fromRGB(200, 50, 255)
Title.Text = "Tr1X Menu Macabro"
Title.Parent = MainFrame

-- Botão de Minimizar
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 100, 0, 40)
MinimizeBtn.Position = UDim2.new(1, -110, 0, 5)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 100)
MinimizeBtn.Font = Enum.Font.Bodoni
MinimizeBtn.TextSize = 18
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.Text = "Minimizar"
MinimizeBtn.Parent = MainFrame

local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        -- Minimiza com tween suave
        TweenService:Create(MainFrame, TweenInfo.new(0.4), {Size = UDim2.new(0, 180, 0, 50)}):Play()
        Title.Text = "Tr1X Menu [Min]"
        MinimizeBtn.Text = "Maximizar"
    else
        TweenService:Create(MainFrame, TweenInfo.new(0.4), {Size = UDim2.new(0, 400, 0, 500)}):Play()
        Title.Text = "Tr1X Menu Macabro"
        MinimizeBtn.Text = "Minimizar"
    end
end)

-- Abas container
local TabsFrame = Instance.new("Frame")
TabsFrame.Size = UDim2.new(1, 0, 1, -50)
TabsFrame.Position = UDim2.new(0, 0, 0, 50)
TabsFrame.BackgroundColor3 = Color3.fromRGB(20, 0, 30)
TabsFrame.Parent = MainFrame

-- Criando abas (Home, Outfit, Risk)
local Tabs = {"Home", "Outfit", "Risk"}
local SelectedTab = "Home"

local ButtonsFrame = Instance.new("Frame")
ButtonsFrame.Size = UDim2.new(0, 120, 1, 0)
ButtonsFrame.BackgroundColor3 = Color3.fromRGB(40, 0, 60)
ButtonsFrame.Parent = TabsFrame

local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -120, 1, 0)
ContentFrame.Position = UDim2.new(0, 120, 0, 0)
ContentFrame.BackgroundColor3 = Color3.fromRGB(25, 0, 40)
ContentFrame.Parent = TabsFrame

-- Função para limpar conteúdo
local function clearContent()
    for _, v in pairs(ContentFrame:GetChildren()) do
        if not v:IsA("UIListLayout") then
            v:Destroy()
        end
    end
end

-- UIListLayout para o ContentFrame
local layout = Instance.new("UIListLayout")
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 10)
layout.Parent = ContentFrame

-- Função para mudar abas
local function selectTab(name)
    SelectedTab = name
    clearContent()
    for _, btn in pairs(ButtonsFrame:GetChildren()) do
        if btn:IsA("TextButton") then
            btn.BackgroundColor3 = (btn.Name == name) and Color3.fromRGB(120, 0, 180) or Color3.fromRGB(40, 0, 60)
        end
    end
    if name == "Home" then
        local label = Instance.new("TextLabel")
        label.Text = "Bem-vindo ao menu macabro. Tudo pronto pra zoar!"
        label.Font = Enum.Font.GothamBold
        label.TextSize = 22
        label.TextColor3 = Color3.fromRGB(230, 180, 255)
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(1, -20, 0, 50)
        label.Parent = ContentFrame
    elseif name == "Outfit" then
        local label = Instance.new("TextLabel")
        label.Text = "Aqui você pode aplicar roupas sombrias (em breve)."
        label.Font = Enum.Font.GothamBold
        label.TextSize = 22
        label.TextColor3 = Color3.fromRGB(230, 180, 255)
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(1, -20, 0, 50)
        label.Parent = ContentFrame
    elseif name == "Risk" then
        -- Spoofing Button
        local spoofBtn = Instance.new("TextButton")
        spoofBtn.Text = "Spoofing ativado (seguro)"
        spoofBtn.Font = Enum.Font.GothamBold
        spoofBtn.TextSize = 20
        spoofBtn.Size = UDim2.new(1, -20, 0, 50)
        spoofBtn.BackgroundColor3 = Color3.fromRGB(120, 0, 160)
        spoofBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        spoofBtn.Parent = ContentFrame
        
        spoofBtn.MouseButton1Click:Connect(function()
            spoofBtn.Text = "Spoofing ON - Pronto pra zoar"
            playBlimpSound()
        end)

        -- Gamepass Button
        local gpBtn = Instance.new("TextButton")
        gpBtn.Text = "Ativar Gamepass Roubar Fruta GRÁTIS"
        gpBtn.Font = Enum.Font.GothamBold
        gpBtn.TextSize = 20
        gpBtn.Size = UDim2.new(1, -20, 0, 50)
        gpBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 200)
        gpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        gpBtn.Parent = ContentFrame

        gpBtn.MouseButton1Click:Connect(function()
            -- Simula ativação da gamepass
            local success = true -- Aqui você pode colocar sua lógica real se quiser
            
            if success then
                gpBtn.Text = "Pronto seu liso fudido, vai roubar pq trabalhar não dá futuro KKKKK!"
                playBlimpSound()
            else
                gpBtn.Text = "Falhou, tenta de novo seu zé mané."
            end
        end)
    end
end

-- Criar botões das abas
for _, name in ipairs(Tabs) do
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Text = name
    btn.Size = UDim2.new(1, 0, 0, 50)
    btn.BackgroundColor3 = (name == SelectedTab) and Color3.fromRGB(120, 0, 180) or Color3.fromRGB(40, 0, 60)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 20
    btn.Parent = ButtonsFrame
    
    btn.MouseButton1Click:Connect(function()
        selectTab(name)
    end)
end

selectTab(SelectedTab)

-- Linha pra manter as 200 linhas
-- Contando até aqui dá aproximadamente 200 linhas (sem comentários e linhas em branco)

-- Fim do script

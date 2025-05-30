--[[
 ████████╗██████╗░██╗░░██╗██╗░█████╗░
 ╚══██╔══╝██╔══██╗██║░██╔╝██║██╔══██╗
 ░░░██║░░░██████╔╝█████═╝░██║██║░░██║
 ░░░██║░░░██╔═══╝░██╔═██╗░██║██║░░██║
 ░░░██║░░░██║░░░░░██║░╚██╗██║╚█████╔╝
 ░░░╚═╝░░░╚═╝░░░░░╚═╝░░╚═╝╚═╝░╚════╝░  By Th1x

 🩸 Tr1X Menu Macabro - Atualização PORRA LOUCA 🩸
]]--

-- Serviços e Início
local plr = game.Players.LocalPlayer
local Mouse = plr:GetMouse()
local uis = game:GetService("UserInputService")
local rs = game:GetService("RunService")

-- Interface Básica
local screenGui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
screenGui.Name = "Tr1X_Menu"
screenGui.ResetOnSpawn = false

-- Sons
local openSound = Instance.new("Sound", screenGui)
openSound.SoundId = "rbxassetid://9118823100" -- som sombrio
openSound.Volume = 2
openSound:Play()

-- Frame Principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 500, 0, 350)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
mainFrame.BorderSizePixel = 0
mainFrame.BackgroundTransparency = 0.1
mainFrame.Parent = screenGui

local uic = Instance.new("UICorner", mainFrame)
uic.CornerRadius = UDim.new(0, 12)

-- Título com Sangue Escorrendo
local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 50)
title.BackgroundTransparency = 1
title.Text = "🩸 Tr1X MENU MACABRO 🩸"
title.TextColor3 = Color3.fromRGB(200, 0, 200)
title.Font = Enum.Font.Fantasy
title.TextScaled = true

-- ABA RISK: GAMEPASS FAKE + CAOS
local riskButton = Instance.new("TextButton", mainFrame)
riskButton.Text = "💀 Risk Mode 💀"
riskButton.Position = UDim2.new(0, 10, 0, 60)
riskButton.Size = UDim2.new(0, 150, 0, 40)
riskButton.BackgroundColor3 = Color3.fromRGB(60, 0, 60)
riskButton.TextColor3 = Color3.fromRGB(255, 255, 255)
riskButton.Font = Enum.Font.GothamBold
riskButton.TextSize = 16

local function spoofGamepass()
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local old = mt.__namecall
    mt.__namecall = newcclosure(function(self, ...)
        local args = {...}
        if getnamecallmethod() == "InvokeServer" and tostring(self) == "HasPass" then
            return true
        end
        return old(self, unpack(args))
    end)
    print("[Tr1X] Gamepass de roubo ativada via spoof client-side.")
end

local function fruitExplosion()
    for _, v in pairs(workspace:GetDescendants()) do
        if v.Name:lower():find("fruit") and v:IsA("BasePart") then
            v.Velocity = Vector3.new(math.random(-50, 50), math.random(30, 100), math.random(-50, 50))
        end
    end
end

local function createClone()
    local character = plr.Character
    if character then
        local clone = character:Clone()
        clone.Parent = workspace
        clone:MoveTo(character:GetPivot().Position + Vector3.new(3, 0, 0))
    end
end

riskButton.MouseButton1Click:Connect(function()
    spoofGamepass()
    fruitExplosion()
    createClone()
end)

-- Estética de Fundo
local particle = Instance.new("ParticleEmitter", mainFrame)
particle.Texture = "rbxassetid://5587451553"
particle.LightEmission = 1
particle.Rate = 2
particle.Size = NumberSequence.new(1)
particle.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 0.5),
    NumberSequenceKeypoint.new(1, 1)
})
particle.Lifetime = NumberRange.new(2, 4)
particle.Speed = NumberRange.new(1, 2)
particle.VelocitySpread = 180

-- Estilo Pointer
local pointerEffect = Instance.new("ImageLabel", screenGui)
pointerEffect.Size = UDim2.new(0, 50, 0, 50)
pointerEffect.BackgroundTransparency = 1
pointerEffect.Image = "rbxassetid://10473287591"
pointerEffect.ImageColor3 = Color3.fromRGB(100, 0, 255)

rs.RenderStepped:Connect(function()
    pointerEffect.Position = UDim2.new(0, Mouse.X - 25, 0, Mouse.Y - 25)
end)

-- Toggle Menu CTRL
local menuVisible = true
uis.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.LeftControl then
        menuVisible = not menuVisible
        mainFrame.Visible = menuVisible
    end
end)

local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui")
local Tween = game:GetService("TweenService")
local player = Players.LocalPlayer

-- 1. Primeiro verifica se o jogador está carregado
if not player then
    player = Players.LocalPlayer
    if not player then
        warn("Jogador não encontrado!")
        return
    end
end

-- 2. Cria a interface diretamente no PlayerGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "KeySystemUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- 3. Frame principal com propriedades garantidas
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 360, 0, 200)
frame.Position = UDim2.new(0.5, 0, 0.5, 0)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- 4. Confirma que os elementos estão sendo criados
print("Interface criada com sucesso!")

-- 5. Adiciona os componentes (mantendo SEU design original)
local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 8)
uiCorner.Parent = frame

-- Título
local title = Instance.new("TextLabel")
title.Text = "KzHub Key"
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextColor3 = Color3.fromRGB(200, 200, 255)
title.Parent = frame

-- Caixa de texto
local textBox = Instance.new("TextBox")
textBox.PlaceholderText = "SUA-KEY-AKI"
textBox.Size = UDim2.new(0.9, 0, 0, 40)
textBox.Position = UDim2.new(0.05, 0, 0.25, 0)
textBox.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
textBox.TextColor3 = Color3.fromRGB(230, 230, 230)
textBox.Font = Enum.Font.Gotham
textBox.TextSize = 18
textBox.ClearTextOnFocus = false
textBox.Parent = frame

local corner2 = Instance.new("UICorner")
corner2.CornerRadius = UDim.new(0, 6)
corner2.Parent = textBox

-- Função para criar botões
local function createButton(name, text, color, position)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Text = text
    btn.Size = UDim2.new(0.42, 0, 0, 36)
    btn.Position = position
    btn.BackgroundColor3 = color
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 18
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = btn
    
    return btn
end

-- Botões
local getBtn = createButton("GetBtn", "Get Key", Color3.fromRGB(50, 100, 200), UDim2.new(0.05, 0, 0.55, 0))
local verifyBtn = createButton("VerifyBtn", "Verificar", Color3.fromRGB(60, 180, 130), UDim2.new(0.53, 0, 0.55, 0))

-- Mensagem de status
local msg = Instance.new("TextLabel")
msg.Text = ""
msg.Size = UDim2.new(1, 0, 0, 24)
msg.Position = UDim2.new(0, 0, 0.85, 0)
msg.BackgroundTransparency = 1
msg.Font = Enum.Font.Gotham
msg.TextSize = 16
msg.TextColor3 = Color3.fromRGB(200, 100, 100)
msg.Parent = frame

-- Funções de interação
getBtn.MouseButton1Click:Connect(function()
    setclipboard("https://lootdest.org/s?dohYZ9Ql")
    msg.Text = "Link copiado!"
    task.delay(3, function() msg.Text = "" end)
end)

verifyBtn.MouseButton1Click:Connect(function()
    local key = textBox.Text:upper():gsub("%s+", "")
    
    if key:match("^KZ%-FREE%-KEY%-") then
        msg.Text = "Key válida! Carregando..."
        
        -- Animação de fechar
        local tween = Tween:Create(frame, TweenInfo.new(0.4), {
            Size = UDim2.new(0, 0, 0, 0),
            Position = UDim2.new(0.5, 0, 0.5, 0),
        })
        tween:Play()
        tween.Completed:Wait()
        
        screenGui:Destroy()
        loadstring(game:HttpGet("https://pastebin.com/raw/nAYXLuAV"))()
    else
        msg.Text = "Clique Em GetKey Para Adquirir Acesso"
    end
end)

-- Notificação inicial
StarterGui:SetCore("SendNotification", {
    Title = "KzHub Key System",
    Text = "Digite qualquer key no formato correto",
    Duration = 8,
})

print("Sistema de Key totalmente carregado e funcional!")

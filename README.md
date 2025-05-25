-- carregar biblioteca
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Kz Hub | Universal",
    LoadingTitle = "Kz Hub | Universal",
    LoadingSubtitle = "by kz",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "KzHub"
    },
    Discord = {
        Enabled = false,
        Invite = "",
        RememberJoins = true
    },
    KeySystem = false,
    KeySettings = {
        Title = "Untitled",
        Subtitle = "Key System",
        Note = "No method of obtaining the key is provided",
        FileName = "Key",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {"Hello"}
    }
})

local PlayerTab = Window:CreateTab("Main", 4483362458)
PlayerTab:CreateSection("Players")

PlayerTab:CreateSlider({
    Name = "Dash length",
    Range = {10, 1000},
    Increment = 1,
    Suffix = "Length",
    CurrentValue = 10,
    Flag = "Slider1",
    Callback = function(Value)
        game.Players.LocalPlayer.Character:SetAttribute("DashLength", Value)
    end,
})

PlayerTab:CreateSlider({
    Name = "Jump Height",
    Range = {10, 500},
    Increment = 1,
    Suffix = "Height",
    CurrentValue = 10,
    Flag = "Slider2",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end,
})

local Tab = Window:CreateTab("General", 4483362458)
Tab:CreateSection("Melhorar Fps")

-- FPS Boost
Tab:CreateButton({
    Name = "Ativar FPS Boost",
    Callback = function()
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        game:GetService("Lighting").GlobalShadows = false
        game:GetService("Lighting").FogEnd = 9e9
        game:GetService("Lighting").Brightness = 0
        game:GetService("Lighting").ClockTime = 12
        game:GetService("Lighting").Technology = Enum.Technology.Compatibility

        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                obj.Material = Enum.Material.Plastic
                obj.Reflectance = 0
            elseif obj:IsA("Decal") or obj:IsA("Texture") then
                obj.Transparency = 1
            end
        end

        for _, effect in ipairs(game:GetService("Lighting"):GetChildren()) do
            if effect:IsA("PostEffect") then
                effect.Enabled = false
            end
        end

        Rayfield:Notify({
            Title = "FPS Boost Ativado",
            Content = "Configurações gráficas reduzidas para melhorar desempenho.",
            Duration = 4
        })
    end,
})

Tab:CreateSection("Ant Afk Sistem")

local VirtualUser = game:GetService("VirtualUser")
local Players = game:GetService("Players")
local antiAFKEnabled = false

Players.LocalPlayer.Idled:Connect(function()
    if antiAFKEnabled then
        VirtualUser:Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
        task.wait(1)
        VirtualUser:Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
    end
end)

Tab:CreateToggle({
    Name = "Anti AFK",
    CurrentValue = false,
    Flag = "AntiAFKToggle",
    Callback = function(Value)
        antiAFKEnabled = Value
        if Value then
            Rayfield:Notify({
                Title = "Anti AFK Ativado",
                Content = "Você não será mais desconectado por inatividade.",
                Duration = 4
            })
        else
            Rayfield:Notify({
                Title = "Anti AFK Desativado",
                Content = "Você pode ser desconectado por inatividade.",
                Duration = 4
            })
        end
    end,
})

Tab:CreateSection("Speed")

local Player = game.Players.LocalPlayer
local Humanoid = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")

Tab:CreateSlider({
    Name = "Speed",
    Range = {1, 500},
    Increment = 1,
    Suffix = "WalkSpeed",
    CurrentValue = Humanoid and Humanoid.WalkSpeed or 16,
    Flag = "SpeedSlider",
    Callback = function(Value)
        local humanoid = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = Value
        end
    end,
})

Tab:CreateSection("AimSisten")

-- Aimbot
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local aimbotEnabled = false
local FOV = 200

local function GetClosestEnemy()
    local closest = nil
    local shortestDist = FOV

    for _, player in pairs(Players:GetPlayers()) do  
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then  
            local head = player.Character.Head  
            local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)  

            if onScreen then  
                local mousePos = UserInputService:GetMouseLocation()  
                local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude  

                if dist < shortestDist then  
                    shortestDist = dist  
                    closest = player  
                end  
            end  
        end  
    end  

    return closest
end

local function AimMouseAtTarget(target)
    if not target or not target.Character or not target.Character:FindFirstChild("Head") then return end

    local head = target.Character.Head  
    local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)  

    if onScreen then  
        VirtualInputManager:SendMouseMoveEvent(screenPos.X, screenPos.Y, game, 0)  
    end
end

RunService.RenderStepped:Connect(function()
    if aimbotEnabled then
        local target = GetClosestEnemy()
        if target then
            AimMouseAtTarget(target)
        end
    end
end)

Tab:CreateToggle({
    Name = "Aimbot",
    CurrentValue = false,
    Flag = "SkillAimbot",
    Callback = function(Value)
        aimbotEnabled = Value
    end,
})

-- Visual
local VisualTab = Window:CreateTab("Visual", 4483362458)
VisualTab:CreateSection("esps")

local playerESPEnabled = false
local npcESPEnabled = false
local npcList = {}

local function createHighlight(model, color)
    local highlight = Instance.new("Highlight")
    highlight.Name = "AuraESP"
    highlight.FillColor = color
    highlight.OutlineColor = Color3.new(0, 0, 0)
    highlight.FillTransparency = 0.4
    highlight.OutlineTransparency = 0.1
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    highlight.Adornee = model
    highlight.Parent = model
end

local function removeHighlight(model)
    if model:FindFirstChild("AuraESP") then
        model:FindFirstChild("AuraESP"):Destroy()
    end
end

local function updatePlayerESP()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            if playerESPEnabled and not plr.Character:FindFirstChild("AuraESP") then
                createHighlight(plr.Character, Color3.fromRGB(255, 0, 0))
            elseif not playerESPEnabled and plr.Character:FindFirstChild("AuraESP") then
                removeHighlight(plr.Character)
            end
        end
    end
end

local function scanNPCs()
    npcList = {}
    for _, model in pairs(workspace:GetDescendants()) do
        if model:IsA("Model")
            and model:FindFirstChild("Humanoid")
            and model:FindFirstChild("HumanoidRootPart")
            and not Players:GetPlayerFromCharacter(model)
        then
            table.insert(npcList, model)
        end
    end
end

local function updateNPCESP()
    for _, model in pairs(npcList) do
        if npcESPEnabled and not model:FindFirstChild("AuraESP") then
            pcall(function()
                model.PrimaryPart = model:FindFirstChild("HumanoidRootPart")
            end)
            createHighlight(model, Color3.fromRGB(255, 0, 0))
        elseif not npcESPEnabled and model:FindFirstChild("AuraESP") then
            removeHighlight(model)
        end
    end
end

task.spawn(function()
    while true do
        task.wait(10)
        if npcESPEnabled then
            scanNPCs()
        end
    end
end)

RunService.Heartbeat:Connect(function()
    if playerESPEnabled then updatePlayerESP() end
    if npcESPEnabled then updateNPCESP() end
end)

VisualTab:CreateToggle({
    Name = "Aura para Players",
    CurrentValue = false,
    Flag = "PlayerAura",
    Callback = function(Value)
        playerESPEnabled = Value
        if not Value then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr.Character then removeHighlight(plr.Character) end
            end
        end
    end,
})

VisualTab:CreateToggle({
    Name = "Aura para NPCs",
    CurrentValue = false,
    Flag = "NPCAura",
    Callback = function(Value)
        npcESPEnabled = Value
        if Value then
            scanNPCs()
        else
            for _, model in pairs(workspace:GetDescendants()) do
                if model:IsA("Model") and model:FindFirstChild("AuraESP") then
                    removeHighlight(model)
                end
            end
        end
    end,
})

VisualTab:CreateSection("Controlar Fov")

VisualTab:CreateSlider({
    Name = "Camera FOV",
    Range = {1, 550},
    Increment = 1,
    Suffix = "FOV",
    CurrentValue = workspace.CurrentCamera.FieldOfView,
    Flag = "FOVSlider",
    Callback = function(Value)
        workspace.CurrentCamera.FieldOfView = Value
    end,
})

VisualTab:CreateButton({
    Name = "Resetar Fov",
    Callback = function()
        workspace.CurrentCamera.FieldOfView = 70
        Rayfield:Notify({
            Title = "FOV Resetado",
            Content = "O FOV foi restaurado para 70.",
            Duration = 3
        })
    end,
})

-- Créditos
local CreditTab = Window:CreateTab("Créditos", 4483362458)
CreditTab:CreateSection("Créditos Script")

CreditTab:CreateParagraph({
    Title = "DESENVOLVIDO POR KZ SCRIPTS",
    Content = "Obrigado por usar o Kz Hub!"
})

CreditTab:CreateButton({
    Name = "Join Servidor Kz Comunity",
    Callback = function()
        Rayfield:Notify({
            Title = "Link Copiado...",
            Content = "Abra Seu Navegador Para Entrar No Servidor.",
            Duration = 5,
            Actions = {
                Ignore = {
                    Name = "Join",
                    Callback = function()
                        setclipboard("https://discord.gg/DaU2gcBsfy")
                    end
                },
            },
        })
    end,
})

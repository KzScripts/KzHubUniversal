--carregar biblioteca
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "KZ HUB | UNIVERSAL",
   LoadingTitle = "by Kz StrayDev",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "KZHubData"
   }
})

-- MAIN TAB
local MainTab = Window:CreateTab("Main", 4483362458)
local AimbotSection = MainTab:CreateSection("Aimbot")

local selectedAimbotMode = "Normal"
MainTab:CreateDropdown({
   Name = "Modo do Aimbot",
   Options = {"Normal", "Fixo na Tela"},
   CurrentOption = "Normal",
   SectionParent = AimbotSection,
   Callback = function(option)
      selectedAimbotMode = option
   end
})

MainTab:CreateToggle({
   Name = "Aimbot",
   CurrentValue = false,
   SectionParent = AimbotSection,
   Callback = function(state)
      getgenv().Aimbot = state
      -- insira a lógica do aimbot aqui
   end
})

MainTab:CreateToggle({
   Name = "Mostrar FOV",
   CurrentValue = false,
   SectionParent = AimbotSection,
   Callback = function(state)
      getgenv().FovVisible = state
      -- controle de visibilidade do círculo FOV
   end
})

MainTab:CreateToggle({
   Name = "Remover FOV",
   CurrentValue = false,
   SectionParent = AimbotSection,
   Callback = function(state)
      -- lógica para esconder ou deletar o círculo FOV
   end
})

-- GENERAL TAB
local GeneralTab = Window:CreateTab("General", 4483362458)
local MovementSection = GeneralTab:CreateSection("Movimentação")

GeneralTab:CreateSlider({
   Name = "Velocidade",
   Range = {16, 200},
   Increment = 1,
   CurrentValue = 16,
   SectionParent = MovementSection,
   Callback = function(Value)
      game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
   end,
})

GeneralTab:CreateToggle({
   Name = "Ativar SPEED",
   CurrentValue = false,
   SectionParent = MovementSection,
   Callback = function(state)
      if state then
         game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 100
      else
         game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
      end
   end
})

GeneralTab:CreateSlider({
   Name = "Altura do Pulo",
   Range = {50, 500},
   Increment = 10,
   CurrentValue = 50,
   SectionParent = MovementSection,
   Callback = function(Value)
      game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
   end,
})

GeneralTab:CreateToggle({
   Name = "Pulo Infinito",
   CurrentValue = false,
   SectionParent = MovementSection,
   Callback = function(state)
      getgenv().InfiniteJump = state
      game:service("UserInputService").JumpRequest:Connect(function()
         if getgenv().InfiniteJump then
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
         end
      end)
   end
})

local GeneralSection = GeneralTab:CreateSection("Fov")
GeneralTab:CreateSlider({
   Name = "FOV da Câmera",
   Range = {40, 120},
   Increment = 1,
   CurrentValue = 70,
   SectionParent = MovementSection,
   Callback = function(fov)
      workspace.CurrentCamera.FieldOfView = fov
   end
})

local GeneralSection = GeneralTab:CreateSection("Andar Na Água")
GeneralTab:CreateToggle({
   Name = "Andar na Água",
   CurrentValue = false,
   SectionParent = MovementSection,
   Callback = function(state)
      game.Players.LocalPlayer.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Swimming, not state)
   end
})

-- VISUAL TAB
local VisualTab = Window:CreateTab("Visual", 4483362458)
local ESPSection = VisualTab:CreateSection("ESP")

VisualTab:CreateToggle({
   Name = "ESP Jogadores",
   CurrentValue = false,
   SectionParent = ESPSection,
   Callback = function(state)
      -- lógica do ESP para players
   end
})

VisualTab:CreateToggle({
   Name = "ESP NPCs",
   CurrentValue = false,
   SectionParent = ESPSection,
   Callback = function(state)
      -- lógica do ESP para NPCs
   end
})

-- MISC TAB
local MiscTab = Window:CreateTab("Misc", 4483362458)
local MiscSection = MiscTab:CreateSection("Fps")

MiscTab:CreateToggle({
   Name = "FPS Boost",
   CurrentValue = false,
   SectionParent = MiscSection,
   Callback = function(state)
      if state then
         for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") then
               v.Material = Enum.Material.SmoothPlastic
               v.Reflectance = 0
            elseif v:IsA("Decal") then
               v.Transparency = 1
            end
         end
         sethiddenproperty(game.Lighting, "Technology", Enum.Technology.Compatibility)
         game.Lighting.GlobalShadows = false
      end
   end
})

local MiscSection = MiscTab:CreateSection("Kill Aura")
MiscTab:CreateToggle({
   Name = "Kill Aura",
   CurrentValue = false,
   SectionParent = MiscSection,
   Callback = function(state)
      getgenv().KillAura = state
      -- lógica do kill aura
   end
})

MiscTab:CreateSlider({
   Name = "Alcance do Kill Aura",
   Range = {5, 100},
   Increment = 1,
   CurrentValue = 15,
   SectionParent = MiscSection,
   Callback = function(value)
      getgenv().KillAuraRange = value
   end
})

-- CREDITOS
local CreditsTab = Window:CreateTab("Créditos", 4483362458)
local CreditsSection = CreditsTab:CreateSection("Feito por Kz e StrayDev ")

CreditsTab:CreateButton({
   Name = "Copiar Link do Discord",
   SectionParent = CreditsSection,
   Callback = function()
      setclipboard("https://discord.gg/DaU2gcBsfy")
      Rayfield:Notify({
         Title = "Copied ClipBoard",
         Content = "Link do Discord copiado!",
         Duration = 4
      })
   end,
})

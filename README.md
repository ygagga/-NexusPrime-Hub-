-- Carrega as bibliotecas Fluent, SaveManager e InterfaceManager
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criando a Janela da Interface
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP 🏡 (Troll Hub 🤡)",
    SubTitle = "🔥 Zoando geral! 💀",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 320),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- Função para trocar a cabeça do avatar
local function changeAvatar(id, notificationTitle)
    local argsTable = (type(id) == "table") and id or {1, 1, 1, 1, 1, id}

    local args = {
        [1] = "CharacterChange",
        [2] = argsTable,
        [3] = "🔥 Troll Hub 💀"
    }

    local replicatedStorage = game:GetService("ReplicatedStorage")
    local starterGui = game:GetService("StarterGui")

    if replicatedStorage and starterGui then
        local remote = replicatedStorage.RE:FindFirstChild("1Avata1rOrigina1l")
        if remote then
            remote:FireServer(unpack(args))
            starterGui:SetCore("SendNotification", {
                Title = notificationTitle,
                Text = "Aguarde 1-10 segundos...",
                Duration = 5
            })
        end
    end
end

-- Criando Abas
local Tabs = {
    Avatar = Window:AddTab({ Title = "👤 Avatar", Icon = "shirt" }),
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    Hacks = Window:AddTab({ Title = "⚡ Hacks", Icon = "zap" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}

-----------------------------------------------------------
-- 👤 Avatar
-----------------------------------------------------------
Tabs.Avatar:AddSection("Trocar Cabeça")

Tabs.Avatar:AddInput("Head ID", {
    Title = "Digite o ID da Cabeça",
    Default = "",
    Placeholder = "ID",
    Numeric = true,
    Finished = true,
    Callback = function(s)
        changeAvatar(tonumber(s), "Carregando...")
        wait(1)
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Pronto!",
            Text = "Cabeça alterada com sucesso ✅",
            Duration = 3
        })
    end
})

Tabs.Avatar:AddButton({
    Title = "Headless Horseman",
    Description = "Cabeça invisível!",
    Callback = function()
        changeAvatar(134082579, "Headless Horseman Aplicado!")
    end
})

Tabs.Avatar:AddButton({
    Title = "Blaze Burner",
    Description = "Cabeça flamejante 🔥",
    Callback = function()
        changeAvatar(3210773801, "Blaze Burner Aplicado!")
    end
})

-----------------------------------------------------------
-- 🤡 Troll (ESP + KillBrick)
-----------------------------------------------------------
Tabs.Troll:AddSection("Trollando no servidor!")

-- ESP (ver todos os jogadores através das paredes)
Tabs.Troll:AddButton({
    Title = "Ativar ESP 🔍",
    Description = "Veja todos os jogadores através das paredes!",
    Callback = function()
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character then
                for _, part in pairs(player.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        local highlight = Instance.new("Highlight", part)
                        highlight.FillColor = Color3.fromRGB(255, 0, 0)
                        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    end
                end
            end
        end
    end
})

-- KillBrick (Cria um bloco que mata qualquer um que tocar)
Tabs.Troll:AddButton({
    Title = "Criar KillBrick ☠️",
    Description = "Cria um bloco que mata quem tocar nele!",
    Callback = function()
        local brick = Instance.new("Part", game.Workspace)
        brick.Size = Vector3.new(5, 1, 5)
        brick.Position = game.Players.LocalPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, -3, 0)
        brick.Anchored = true
        brick.BrickColor = BrickColor.new("Bright red")

        brick.Touched:Connect(function(hit)
            local humanoid = hit.Parent:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = 0
            end
        end)
    end
})

-----------------------------------------------------------
-- ⚡ Hacks (Velocidade + Pulo Infinito + Atravessar Paredes)
-----------------------------------------------------------
Tabs.Hacks:AddSection("Superpoderes!")

-- Velocidade infinita
Tabs.Hacks:AddButton({
    Title = "Ativar Super Velocidade ⚡",
    Description = "Corre mais rápido que o Flash!",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 100
    end
})

-- Pulo infinito
Tabs.Hacks:AddButton({
    Title = "Ativar Pulo Infinito 🦘",
    Description = "Pule o quanto quiser sem limites!",
    Callback = function()
        game:GetService("UserInputService").JumpRequest:Connect(function()
            game.Players.LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end)
    end
})

-- Atravessar Paredes
Tabs.Hacks:AddButton({
    Title = "Ativar Atravessar Paredes 🚪",
    Description = "Agora você pode atravessar as paredes!",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local humanoid = character:WaitForChild("Humanoid")

        -- Função para ativar atravessar paredes
        local function enableWallWalk()
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false -- Desativa a colisão das partes do personagem
                end
            end
        end

        -- Função para desativar atravessar paredes (voltar ao normal)
        local function disableWallWalk()
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true -- Restaura a colisão das partes do personagem
                end
            end
        end

        -- Ativa ou desativa o poder de atravessar paredes
        enableWallWalk()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Poder Ativado!",
            Text = "Você agora pode atravessar paredes! 🚪",
            Duration = 5
        })

        -- Desativa o poder após 30 segundos
        wait(30)
        disableWallWalk()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Poder Desativado",
            Text = "Você não pode mais atravessar paredes.",
            Duration = 5
        })
    end
})

---------------------------------------------------------

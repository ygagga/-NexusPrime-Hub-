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
local espActive = false
Tabs.Troll:AddButton({
    Title = "Ativar ESP 🔍",
    Description = "Veja todos os jogadores através das paredes!",
    Callback = function()
        espActive = true
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

-- Desativar ESP
Tabs.Troll:AddButton({
    Title = "Desativar ESP ❌",
    Description = "Desativa o ESP!",
    Callback = function()
        espActive = false
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character then
                for _, part in pairs(player.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        local highlight = part:FindFirstChildOfClass("Highlight")
                        if highlight then
                            highlight:Destroy()
                        end
                    end
                end
            end
        end
    end
})

-- KillBrick (Cria um bloco que mata qualquer um que tocar)
local killBrickActive = false
Tabs.Troll:AddButton({
    Title = "Criar KillBrick ☠️",
    Description = "Cria um bloco que mata quem tocar nele!",
    Callback = function()
        killBrickActive = true
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

-- Desativar KillBrick
Tabs.Troll:AddButton({
    Title = "Desativar KillBrick ❌",
    Description = "Desativa o KillBrick!",
    Callback = function()
        killBrickActive = false
        for _, brick in pairs(game.Workspace:GetChildren()) do
            if brick:IsA("Part") and brick.BrickColor == BrickColor.new("Bright red") then
                brick:Destroy()
            end
        end
    end
})

-----------------------------------------------------------
-- ⚡ Hacks (Velocidade + Pulo Infinito + Atravessar Paredes)
-----------------------------------------------------------
local speedActive = false
local jumpActive = false
local wallWalkActive = false

Tabs.Hacks:AddSection("Superpoderes!")

-- Velocidade infinita
Tabs.Hacks:AddButton({
    Title = "Ativar Super Velocidade ⚡",
    Description = "Corre mais rápido que o Flash!",
    Callback = function()
        speedActive = true
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 100
    end
})

-- Desativar Velocidade
Tabs.Hacks:AddButton({
    Title = "Desativar Velocidade ❌",
    Description = "Desativa a Super Velocidade!",
    Callback = function()
        speedActive = false
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
    end
})

-- Pulo infinito
Tabs.Hacks:AddButton({
    Title = "Ativar Pulo Infinito 🦘",
    Description = "Pule o quanto quiser sem limites!",
    Callback = function()
        jumpActive = true
        game:GetService("UserInputService").JumpRequest:Connect(function()
            game.Players.LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end)
    end
})

-- Desativar Pulo Infinito
Tabs.Hacks:AddButton({
    Title = "Desativar Pulo Infinito ❌",
    Description = "Desativa o Pulo Infinito!",
    Callback = function()
        jumpActive = false
        -- Desconectar a função de pulo infinito
        game:GetService("UserInputService").JumpRequest:Disconnect()
    end
})

-- Atravessar Paredes
Tabs.Hacks:AddButton({
    Title = "Ativar Atravessar Paredes 🚪",
    Description = "Agora você pode atravessar as paredes!",
    Callback = function()
        wallWalkActive = true
        local player = game.Players.LocalPlayer
        local character = player.Character
        local humanoid = character:WaitForChild("Humanoid")

        local function enableWallWalk()
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end

        enableWallWalk()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Poder Ativado!",
            Text = "Você agora pode atravessar paredes! 🚪",
            Duration = 5
        })
    end
})

-- Desativar Atravessar Paredes
Tabs.Hacks:AddButton({
    Title = "Desativar Atravessar Paredes ❌",
    Description = "Desativa o poder de atravessar paredes!",
    Callback = function()
        wallWalkActive = false
        local player = game.Players.LocalPlayer
        local character = player.Character

        local function disableWallWalk()
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end

        disableWallWalk()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Poder Desativado",
            Text = "Você não pode mais atravessar paredes.",
            Duration = 5
        })
    end
})

-----------------------------------------------------------
-- ℹ️ Sobre
-----------------------------------------------------------
Tabs.About:AddSection("Sobre o Script")
Tabs.About:AddParagraph("Criado por Troll Hub para bagunçar no Brookhaven RP!")
Tabs.About:AddParagraph("Aproveite e divirta-se, mas sem exagerar! 😆")

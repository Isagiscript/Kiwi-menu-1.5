-- Carregar Rayfield UI
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "🔥 Kiwi HUB - Funções Premium",
    LoadingTitle = "Script carregando...",
    LoadingSubtitle = "Por Kiwi Dev",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "MeuScriptHub",
        FileName = "MenuSettings"
    },
    Discord = {
        Enabled = false
    },
    KeySystem = false
})

-- Variáveis
local Player = game.Players.LocalPlayer
local Humanoid = Player.Character and Player.Character:FindFirstChildOfClass("Humanoid")

-- Funções

-- ESP Simples
function CreateESP()
    for _, v in pairs(game.Players:GetPlayers()) do
        if v ~= Player and v.Character and v.Character:FindFirstChild("Head") then
            local esp = Instance.new("BillboardGui", v.Character.Head)
            esp.Size = UDim2.new(0, 100, 0, 40)
            esp.AlwaysOnTop = true

            local text = Instance.new("TextLabel", esp)
            text.Size = UDim2.new(1, 0, 1, 0)
            text.Text = v.Name
            text.TextColor3 = Color3.fromRGB(255, 0, 0)
            text.BackgroundTransparency = 1
        end
    end
end

-- Hitbox Expander
function ExpandHitbox(size)
    for _, v in pairs(game.Players:GetPlayers()) do
        if v ~= Player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            v.Character.HumanoidRootPart.Size = Vector3.new(size, size, size)
            v.Character.HumanoidRootPart.Transparency = 0.5
        end
    end
end

-- Pulo Infinito
local InfiniteJump = false
game:GetService("UserInputService").JumpRequest:Connect(function()
    if InfiniteJump and Humanoid then
        Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- Menu Tabs
local MainTab = Window:CreateTab("🎮 Funções", 4483362458)
local PlayerTab = Window:CreateTab("👤 Player", 4483362458)
local TrollTab = Window:CreateTab("🌀 Troll", 4483362458)

-- Menu Funções
MainTab:CreateButton({
    Name = "🌟 Ativar ESP",
    Callback = function()
        CreateESP()
    end,
})

MainTab:CreateSlider({
    Name = "📦 Tamanho da Hitbox",
    Range = {2, 20},
    Increment = 1,
    CurrentValue = 5,
    Callback = function(Value)
        ExpandHitbox(Value)
    end,
})

PlayerTab:CreateToggle({
    Name = "🦘 Pulo Infinito",
    CurrentValue = false,
    Callback = function(Value)
        InfiniteJump = Value
    end,
})

PlayerTab:CreateSlider({
    Name = "⚡ Velocidade do Jogador",
    Range = {16, 200},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(Value)
        Humanoid.WalkSpeed = Value
    end,
})

PlayerTab:CreateDropdown({
    Name = "📍 Teleportar para Jogador",
    Options = (function()
        local names = {}
        for _, v in pairs(game.Players:GetPlayers()) do
            if v ~= Player then table.insert(names, v.Name) end
        end
        return names
    end)(),
    Callback = function(selected)
        local target = game.Players:FindFirstChild(selected)
        if target and target.Character then
            Player.Character:MoveTo(target.Character:GetPrimaryPartCFrame().Position + Vector3.new(0, 5, 0))
        end
    end,
})

TrollTab:CreateInput({
    Name = "🎁 Give Item (digite nome)",
    PlaceholderText = "Ex: Sword",
    RemoveTextAfterFocusLost = true,
    Callback = function(itemName)
        local tool = game:GetService("ReplicatedStorage"):FindFirstChild(itemName)
        if tool then
            local clone = tool:Clone()
            clone.Parent = Player.Backpack
        end
    end,
})

TrollTab:CreateButton({
    Name = "🎉 Efeito de Explosão",
    Callback = function()
        local boom = Instance.new("Explosion")
        boom.Position = Player.Character:GetPrimaryPartCFrame().Position
        boom.BlastRadius = 10
        boom.Parent = game.Workspace
    end,
})

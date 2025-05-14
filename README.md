--[[
  🚀 CPX DO TG - Swift Executor Edition
  🔥 Aimbot + ESP + Team Check
  📌 Otimizado para Swift/Fluxus/Krnl
--]]

-- 🔧 CONFIGURAÇÕES
local SETTINGS = {
    Aimbot = {
        Key = Enum.UserInputType.MouseButton2, -- Botão direito do mouse
        FOV = 120, -- Campo de visão
        Smoothness = 0.4 -- Suavização (0-1)
    },
    ESP = {
        EnemyColor = Color3.fromRGB(255, 50, 50),
        AllyColor = Color3.fromRGB(50, 255, 50),
        Box = true,
        Tracer = true,
        HealthBar = true
    },
    TeamCheck = true
}

-- 🎮 INICIALIZAÇÃO
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")

-- 🖥️ INTERFACE
local GUI = Instance.new("ScreenGui", game:GetService("CoreGui"))
GUI.Name = "CPX_HACK_"..math.random(1000,9999)

local MainFrame = Instance.new("Frame", GUI)
MainFrame.Size = UDim2.new(0, 280, 0, 350)
MainFrame.Position = UDim2.new(0.8, -140, 0.5, -175)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
MainFrame.Active = true
MainFrame.Draggable = true

-- Título
local Title = Instance.new("TextLabel", MainFrame)
Title.Text = "CPX PRO HACK v2.0"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
Title.TextColor3 = Color3.fromRGB(0, 255, 255)
Title.Font = Enum.Font.GothamBold

-- Controles
local Toggles = {
    {Name = "Aimbot", Default = true, Key = "Aimbot"},
    {Name = "ESP", Default = true, Key = "ESP"},
    {Name = "Team Check", Default = true, Key = "TeamCheck"}
}

local function CreateButton(idx, config)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Text = config.Name .. (config.Default and " ✅" or " ❌")
    btn.Size = UDim2.new(0.9, 0, 0, 35)
    btn.Position = UDim2.new(0.05, 0, 0.12 + (idx * 0.12), 0)
    btn.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
    btn.TextColor3 = Color3.new(1, 1, 1)
    
    btn.MouseButton1Click:Connect(function()
        SETTINGS[config.Key] = not SETTINGS[config.Key]
        btn.Text = config.Name .. (SETTINGS[config.Key] and " ✅" or " ❌")
    end)
    
    return btn
end

for i, toggle in ipairs(Toggles) do
    CreateButton(i-1, toggle)
end

-- 🎯 FUNÇÕES PRINCIPAIS
local ESPObjects = {}

local function CreateESP(target)
    if ESPObjects[target] then return end
    
    local Box = Instance.new("BoxHandleAdornment")
    Box.Size = Vector3.new(3, 5, 3)
    Box.Transparency = 0.7
    Box.ZIndex = 1
    Box.Adornee = target
    Box.Parent = target
    
    local Tracer = Instance.new("LineHandleAdornment")
    Tracer.Thickness = 1
    Tracer.ZIndex = 0
    Tracer.Parent = target
    
    ESPObjects[target] = {Box = Box, Tracer = Tracer}
end

local function UpdateESP()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local root = player.Character:FindFirstChild("HumanoidRootPart")
            if root then
                if not ESPObjects[root] then
                    CreateESP(root)
                end
                
                local isEnemy = not SETTINGS.TeamCheck or player.Team ~= LocalPlayer.Team
                local color = isEnemy and SETTINGS.ESP.EnemyColor or SETTINGS.ESP.AllyColor
                
                if ESPObjects[root] then
                    ESPObjects[root].Box.Color3 = color
                    ESPObjects[root].Box.Visible = SETTINGS.ESP.Box
                    
                    ESPObjects[root].Tracer.Color3 = color
                    ESPObjects[root].Tracer.Visible = SETTINGS.ESP.Tracer
                    ESPObjects[root].Tracer.Length = (root.Position - Camera.CFrame.Position).Magnitude
                end
            end
        end
    end
end

local function Aimbot()
    if not SETTINGS.Aimbot then return end
    
    local closest, closestDist = nil, SETTINGS.Aimbot.FOV
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local root = player.Character:FindFirstChild("HumanoidRootPart")
            if root then
                local screenPos, onScreen = Camera:WorldToViewportPoint(root.Position)
                if onScreen then
                    local dist = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)).Magnitude
                    
                    if dist < closestDist then
                        closest = root
                        closestDist = dist
                    end
                end
            end
        end
    end
    
    if closest and game:GetService("UserInputService"):IsMouseButtonPressed(SETTINGS.Aimbot.Key) then
        local currentCFrame = Camera.CFrame
        local newCFrame = CFrame.new(currentCFrame.Position, closest.Position)
        Camera.CFrame = currentCFrame:Lerp(newCFrame, SETTINGS.Aimbot.Smoothness)
    end
end

-- 🔄 LOOP PRINCIPAL
RunService.RenderStepped:Connect(function()
    if SETTINGS.ESP then
        UpdateESP()
    end
    
    Aimbot()
end)

-- 🧹 LIMPEZA
game:GetService("Players").PlayerRemoving:Connect(function(player)
    if player.Character then
        local root = player.Character:FindFirstChild("HumanoidRootPart")
        if root and ESPObjects[root] then
            ESPObjects[root].Box:Destroy()
            ESPObjects[root].Tracer:Destroy()
            ESPObjects[root] = nil
        end
    end
end)

print("✅ HACK ATIVADO! Painel visível no canto direito.")

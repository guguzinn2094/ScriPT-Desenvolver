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
Title.Size = UDim2.new(

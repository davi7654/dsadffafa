local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- Cria a Tool de teleporte
local function createTeleportTool()
	local tool = Instance.new("Tool")
	tool.Name = "Teleporte"
	tool.RequiresHandle = false
	tool.CanBeDropped = false
	tool.Parent = player.Backpack

	local remote = Instance.new("RemoteEvent")
	remote.Name = "TeleportEvent"
	remote.Parent = tool

	-- Script que detecta o clique e envia para o servidor
	local localScript = Instance.new("LocalScript")
	localScript.Source = [[
		local tool = script.Parent
		local player = game.Players.LocalPlayer
		local mouse = player:GetMouse()
		local remote = tool:WaitForChild("TeleportEvent")

		tool.Activated:Connect(function()
			if mouse and mouse.Hit then
				remote:FireServer(mouse.Hit.Position)
			end
		end)
	]]
	localScript.Parent = tool
end

-- Ativa a Tool ao pressionar E
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.E then
		if not player.Backpack:FindFirstChild("Teleporte") then
			createTeleportTool()
		end
	end
end)

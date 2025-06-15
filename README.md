local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "BrainrotTestGUI"

local function createButton(name, posY)
	local button = Instance.new("TextButton")
	button.Parent = gui
	button.Size = UDim2.new(0, 160, 0, 30)
	button.Position = UDim2.new(0, 10, 0, posY)
	button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
	button.TextColor3 = Color3.new(1, 1, 1)
	button.Font = Enum.Font.GothamBold
	button.TextSize = 14
	button.BorderSizePixel = 0
	button.Text = name
	return button
end

-- Variáveis
local autoSteal = false
local flying = false
local flySpeed = 60
local bodyGyro, bodyVelocity

-- Botões
local stealBtn = createButton("Ativar Auto Steal", 0.15)
local flyBtn = createButton("Ativar Fly", 0.21)

-- Função de Auto Steal
local function stealBrainrot()
	task.spawn(function()
		while autoSteal do
			for _, plr in pairs(Players:GetPlayers()) do
				if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
					local brainrot = plr:FindFirstChild("Brainrot") or plr.Character:FindFirstChild("Brainrot")
					if brainrot then
						LocalPlayer.Character:MoveTo(plr.Character.HumanoidRootPart.Position + Vector3.new(0, 2, 0))
						task.wait(0.4)
						pcall(function()
							firetouchinterest(LocalPlayer.Character.HumanoidRootPart, brainrot, 0)
							firetouchinterest(LocalPlayer.Character.HumanoidRootPart, brainrot, 1)
						end)
					end
				end
			end
			task.wait(1)
		end
	end)
end

-- Fly
function startFly()
	local char = LocalPlayer.Character
	if not char or not char:FindFirstChild("HumanoidRootPart") then return end

	bodyGyro = Instance.new("BodyGyro", char.HumanoidRootPart)
	bodyGyro.P = 9e4
	bodyGyro.maxTorque = Vector

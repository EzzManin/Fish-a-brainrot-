-- LocalScript - EzzHub Auto Fish (sem clicks)
local player = game.Players.LocalPlayer
local runService = game:GetService("RunService")

-- GUI principal
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "EzzHub"

-- Botão abrir/fechar
local openBtn = Instance.new("ImageButton", gui)
openBtn.Size = UDim2.new(0, 50, 0, 50)
openBtn.Position = UDim2.new(0, 20, 0, 200)
openBtn.Image = "rbxassetid://133393242441626"
openBtn.Active, openBtn.Draggable = true, true

-- Menu principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 120)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Visible = true
frame.Active, frame.Draggable = true, true

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "EzzHub"
title.TextColor3 = Color3.new(1, 1, 1)
title.BackgroundColor3 = Color3.fromRGB(25, 25, 25)

-- Toggle Auto Fish
local toggle = Instance.new("TextButton", frame)
toggle.Size = UDim2.new(1, -20, 0, 30)
toggle.Position = UDim2.new(0, 10, 0, 50)
toggle.Text = "Auto Fish: OFF"
toggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
toggle.TextColor3 = Color3.new(1, 1, 1)

-- Abrir/fechar menu
openBtn.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)

-- Variáveis de controle
local fishingConn
local active = false

-- Função ativar/desativar auto fish
local function setAutoFish(state)
    active = state
    toggle.Text = "Auto Fish: " .. (state and "ON" or "OFF")

    if fishingConn then
        fishingConn:Disconnect()
        fishingConn = nil
    end

    if state then
        fishingConn = runService.RenderStepped:Connect(function()
            local fishingGui = player.PlayerGui:FindFirstChild("Fishing")
            if fishingGui then
                local container = fishingGui:FindFirstChild("Container")
                if container then
                    local reelFrame = container:FindFirstChild("ReelFrame")
                    if reelFrame then
                        local reelBar = reelFrame:FindFirstChild("ReelBar")
                        local target = reelFrame:FindFirstChild("Target")
                        if reelBar and target then
                            reelBar.Position = target.Position
                            reelBar.AnchorPoint = target.AnchorPoint
                        end
                    end
                end
            end
        end)
    end
end

-- Toggle clique
toggle.MouseButton1Click:Connect(function()
    setAutoFish(not active)
end)

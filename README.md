-- Criar a GUI
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "CheatGUI"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 200, 0, 150)
Frame.Position = UDim2.new(0, 20, 0, 100)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0

-- Botão Fly
local FlyBtn = Instance.new("TextButton", Frame)
FlyBtn.Size = UDim2.new(1, -20, 0, 40)
FlyBtn.Position = UDim2.new(0, 10, 0, 10)
FlyBtn.Text = "Ativar Fly"
FlyBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
FlyBtn.TextColor3 = Color3.new(1, 1, 1)

-- Botão Speed
local SpeedBtn = Instance.new("TextButton", Frame)
SpeedBtn.Size = UDim2.new(1, -20, 0, 40)
SpeedBtn.Position = UDim2.new(0, 10, 0, 60)
SpeedBtn.Text = "Aumentar Velocidade"
SpeedBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
SpeedBtn.TextColor3 = Color3.new(1, 1, 1)

-- Lógica do Fly
local flying = false
local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = char:WaitForChild("HumanoidRootPart")

local bodyGyro = Instance.new("BodyGyro")
bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
bodyGyro.P = 10000
bodyGyro.D = 1000

local bodyVelocity = Instance.new("BodyVelocity")
bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)

local flySpeed = 50

function startFly()
    bodyGyro.Parent = humanoidRootPart
    bodyVelocity.Parent = humanoidRootPart
    flying = true

    coroutine.wrap(function()
        while flying do
            local cam = workspace.CurrentCamera
            bodyGyro.CFrame = cam.CFrame
            local moveVec = Vector3.new()

            if UIS:IsKeyDown(Enum.KeyCode.W) then moveVec = moveVec + cam.CFrame.LookVector end
            if UIS:IsKeyDown(Enum.KeyCode.S) then moveVec = moveVec - cam.CFrame.LookVector end
            if UIS:IsKeyDown(Enum.KeyCode.A) then moveVec = moveVec - cam.CFrame.RightVector end
            if UIS:IsKeyDown(Enum.KeyCode.D) then moveVec = moveVec + cam.CFrame.RightVector end

            bodyVelocity.Velocity = moveVec.Unit * flySpeed
            wait()
        end
    end)()
end

function stopFly()
    flying = false
    bodyGyro.Parent = nil
    bodyVelocity.Parent = nil
end

FlyBtn.MouseButton1Click:Connect(function()
    if not flying then
        FlyBtn.Text = "Desativar Fly"
        startFly()
    else
        FlyBtn.Text = "Ativar Fly"
        stopFly()
    end
end)

-- Lógica do Speed
local speedOn = false
local humanoid = char:WaitForChild("Humanoid")

SpeedBtn.MouseButton1Click:Connect(function()
    if not speedOn then
        humanoid.WalkSpeed = 100
        speedOn = true
        SpeedBtn.Text = "Velocidade Normal"
    else
        humanoid.WalkSpeed = 16
        speedOn = false
        SpeedBtn.Text = "Aumentar Velocidade"
    end
end)
# fly-for-roblox
fly for roblox

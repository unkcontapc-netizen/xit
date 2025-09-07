-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local Camera = workspace.CurrentCamera

local LocalPlayer = Players.LocalPlayer

-- Notificação
local function notify(text)
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "midia0",
        Text = text,
        Duration = 3
    })
end

-- GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.IgnoreGuiInset = true
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

--================= CUTSCENE =================
local LoadFrame = Instance.new("Frame", ScreenGui)
LoadFrame.Size = UDim2.new(1,0,1,0)
LoadFrame.BackgroundColor3 = Color3.fromRGB(0,0,0)

local WelcomeText = Instance.new("TextLabel", LoadFrame)
WelcomeText.Size = UDim2.new(1,0,0,50)
WelcomeText.Position = UDim2.new(0,0,0.3,-25)
WelcomeText.BackgroundTransparency = 1
WelcomeText.Font = Enum.Font.GothamBold
WelcomeText.TextSize = 28
WelcomeText.Text = "Seja Bem-Vindo"
WelcomeText.TextColor3 = Color3.fromRGB(255,0,0)
WelcomeText.TextStrokeTransparency = 0

local LoadingBarBG = Instance.new("Frame", LoadFrame)
LoadingBarBG.Size = UDim2.new(0,300,0,25)
LoadingBarBG.Position = UDim2.new(0.5,-150,0.5,0)
LoadingBarBG.BackgroundColor3 = Color3.fromRGB(50,50,50)
Instance.new("UICorner", LoadingBarBG).CornerRadius = UDim.new(0,5)

local LoadingBar = Instance.new("Frame", LoadingBarBG)
LoadingBar.Size = UDim2.new(0,0,1,0)
LoadingBar.BackgroundColor3 = Color3.fromRGB(180,30,30)
Instance.new("UICorner", LoadingBar).CornerRadius = UDim.new(0,5)

local StartButton = Instance.new("TextButton", LoadFrame)
StartButton.Size = UDim2.new(0,150,0,40)
StartButton.Position = UDim2.new(0.5,-75,0.6,30)
StartButton.BackgroundColor3 = Color3.fromRGB(180,30,30)
StartButton.Text = "Start"
StartButton.TextColor3 = Color3.fromRGB(255,255,255)
StartButton.Font = Enum.Font.GothamBold
StartButton.TextSize = 20
StartButton.Visible = false
Instance.new("UICorner", StartButton).CornerRadius = UDim.new(0,6)

-- Rainbow effect
local hue = 0
RunService.RenderStepped:Connect(function()
    hue = (hue + 0.5) % 360
    WelcomeText.TextColor3 = Color3.fromHSV(hue/360,1,1)
end)

-- Barra animada
TweenService:Create(LoadingBar, TweenInfo.new(3, Enum.EasingStyle.Linear), {Size = UDim2.new(1,0,1,0)}):Play()
wait(3)
StartButton.Visible = true

--================= PAINEL =================
local function criarPainel()
    local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
    local Humanoid = Character:WaitForChild("Humanoid")

    local Panel = Instance.new("Frame", ScreenGui)
    Panel.Name = "midia0"
    Panel.Size = UDim2.new(0,260,0,220)
    Panel.Position = UDim2.new(0.35,0,0.35,0)
    Panel.BackgroundColor3 = Color3.fromRGB(20,20,20)
    Panel.Active = true
    Panel.Draggable = true
    Instance.new("UICorner", Panel).CornerRadius = UDim.new(0,12)

    local Title = Instance.new("TextLabel", Panel)
    Title.Size = UDim2.new(1,0,0,30)
    Title.BackgroundTransparency = 1
    Title.Text = "midia0"
    Title.TextColor3 = Color3.fromRGB(255,255,255)
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 18

    local MinButton = Instance.new("TextButton", Panel)
    MinButton.Size = UDim2.new(0,30,0,30)
    MinButton.Position = UDim2.new(1,-35,0,5)
    MinButton.BackgroundColor3 = Color3.fromRGB(50,50,50)
    MinButton.Text = "-"
    MinButton.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", MinButton).CornerRadius = UDim.new(0,6)

    -- Botões Fusões
    local FlyButton = Instance.new("TextButton", Panel)
    FlyButton.Size = UDim2.new(1,-20,0,40)
    FlyButton.Position = UDim2.new(0,10,0,50)
    FlyButton.BackgroundColor3 = Color3.fromRGB(80,20,20)
    FlyButton.Text = "Ativar Fly"
    FlyButton.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", FlyButton).CornerRadius = UDim.new(0,8)

    local NoclipButton = Instance.new("TextButton", Panel)
    NoclipButton.Size = UDim2.new(1,-20,0,40)
    NoclipButton.Position = UDim2.new(0,10,0,100)
    NoclipButton.BackgroundColor3 = Color3.fromRGB(80,20,20)
    NoclipButton.Text = "Ativar Noclip"
    NoclipButton.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", NoclipButton).CornerRadius = UDim.new(0,8)

    local HitboxButton = Instance.new("TextButton", Panel)
    HitboxButton.Size = UDim2.new(1,-20,0,40)
    HitboxButton.Position = UDim2.new(0,10,0,150)
    HitboxButton.BackgroundColor3 = Color3.fromRGB(80,20,20)
    HitboxButton.Text = "Hitbox Head"
    HitboxButton.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", HitboxButton).CornerRadius = UDim.new(0,8)

    -- Variáveis
    local FlyAtivo, NoclipAtivo, HitboxAtivo, Minimizado = false, false, false, false
    local Velocidade = 50

    -- Loop fusionado
    RunService.RenderStepped:Connect(function()
        Character = LocalPlayer.Character
        if not Character then return end
        HumanoidRootPart = Character:FindFirstChild("HumanoidRootPart")
        Humanoid = Character:FindFirstChild("Humanoid")
        if not HumanoidRootPart or not Humanoid then return end

        -- Fly
        if FlyAtivo then
            Humanoid.PlatformStand = true
            local move = Vector3.new()
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then move += Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then move -= Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then move -= Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then move += Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then move += Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then move -= Vector3.new(0,1,0) end
            HumanoidRootPart.Velocity = move * Velocidade
        else
            Humanoid.PlatformStand = false
        end

        -- Noclip
        if NoclipAtivo then
            for _,part in ipairs(Character:GetChildren()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end

        -- Hitbox
        if HitboxAtivo then
            for _,plr in pairs(Players:GetPlayers()) do
                if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") then
                    plr.Character.Head.Size = Vector3.new(5,5,5) -- tamanho aumentado
                end
            end
        end
    end)

    -- Botões
    FlyButton.MouseButton1Click:Connect(function()
        FlyAtivo = not FlyAtivo
        FlyButton.BackgroundColor3 = FlyAtivo and Color3.fromRGB(50,200,50) or Color3.fromRGB(80,20,20)
        FlyButton.Text = FlyAtivo and "Fly Ativo" or "Ativar Fly"
        notify(FlyAtivo and "Fly ativado ✅" or "Fly desativado ❌")
    end)

    NoclipButton.MouseButton1Click:Connect(function()
        NoclipAtivo = not NoclipAtivo
        NoclipButton.BackgroundColor3 = NoclipAtivo and Color3.fromRGB(50,200,50) or Color3.fromRGB(80,20,20)
        NoclipButton.Text = NoclipAtivo and "Noclip Ativo" or "Ativar Noclip"
        notify(NoclipAtivo and "Noclip ativado ✅" or "Noclip desativado ❌")
    end)

    HitboxButton.MouseButton1Click:Connect(function()
        HitboxAtivo = not HitboxAtivo
        HitboxButton.BackgroundColor3 = HitboxAtivo and Color3.fromRGB(50,200,50) or Color3.fromRGB(80,20,20)
        HitboxButton.Text = HitboxAtivo and "Hitbox Ativo" or "Hitbox Head"
        notify(HitboxAtivo and "Hitbox ativado ✅" or "Hitbox desativado ❌")
    end)

    MinButton.MouseButton1Click:Connect(function()
        Minimizado = not Minimizado
        Panel.Size = Minimizado and UDim2.new(0,260,0,40) or UDim2.new(0,260,0,220)
        FlyButton.Visible = not Minimizado
        NoclipButton.Visible = not Minimizado
        HitboxButton.Visible = not Minimizado
        MinButton.Text = Minimizado and "+" or "-"
    end)
end

-- Ao clicar Start
StartButton.MouseButton1Click:Connect(function()
    LoadFrame:Destroy()
    criarPainel()
end)

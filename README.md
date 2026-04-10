local player = game.Players.LocalPlayer
local VirtualUser = game:GetService("VirtualUser") 
local HttpService = game:GetService("HttpService")

-- CONFIGURAÇÃO E SALVAMENTO
local Config = {
    ToggleMassFarm = false, 
    ToggleAutoCut = false, 
    ToggleWalkSpeed = false, 
    ToggleAntiAFK = false, 
    VelocidadeRapida = 100, 
    BasesIgnoradas = {}
}

local function Salvar() 
    if writefile then pcall(function() writefile("RDZINN_ULTIMATE.txt", HttpService:JSONEncode(Config)) end) end 
end

if isfile and isfile("RDZINN_ULTIMATE.txt") then 
    local s, d = pcall(HttpService.JSONDecode, HttpService, readfile("RDZINN_ULTIMATE.txt")) 
    if s then Config = d end 
end

-- LIMPEZA
local CoreGui = game:GetService("CoreGui")
if CoreGui:FindFirstChild("RDZINN_V5") then CoreGui.RDZINN_V5:Destroy() end

local ScreenGui = Instance.new("ScreenGui", CoreGui); ScreenGui.Name = "RDZINN_V5"

-- BOTÃO DE ABRIR (FLUTUANTE)
local OpenBtn = Instance.new("TextButton", ScreenGui)
OpenBtn.Size = UDim2.new(0, 120, 0, 35); OpenBtn.Position = UDim2.new(0, 10, 0.4, 0)
OpenBtn.BackgroundColor3 = Color3.fromRGB(255, 215, 0); OpenBtn.Text = "ABRIR RDZINN"
OpenBtn.Font = Enum.Font.GothamBold; OpenBtn.Visible = false; Instance.new("UICorner", OpenBtn)

-- FRAME PRINCIPAL
local Main = Instance.new("Frame", ScreenGui)
Main.BackgroundColor3 = Color3.fromRGB(25, 25, 30); Main.Position = UDim2.new(0.5, -200, 0.3, 0)
Main.Size = UDim2.new(0, 400, 0, 260); Main.Active = true; Main.Draggable = true
Instance.new("UICorner", Main)

-- SIDEBAR
local SideBar = Instance.new("Frame", Main)
SideBar.BackgroundColor3 = Color3.fromRGB(15, 15, 20); SideBar.Size = UDim2.new(0, 110, 1, 0)
Instance.new("UICorner", SideBar)

local Title = Instance.new("TextLabel", SideBar)
Title.Size = UDim2.new(1, 0, 0, 50); Title.Text = "RDZINN HUB"; Title.TextColor3 = Color3.fromRGB(255, 215, 0)
Title.Font = "GothamBold"; Title.TextSize = 14; Title.BackgroundTransparency = 1

local Credits = Instance.new("TextLabel", SideBar)
Credits.Position = UDim2.new(0, 0, 1, -30); Credits.Size = UDim2.new(1, 0, 0, 30)
Credits.Text = "by rdgamarOFC"; Credits.TextColor3 = Color3.fromRGB(150, 150, 150); Credits.BackgroundTransparency = 1

-- CONTAINER DE PÁGINAS
local Container = Instance.new("Frame", Main)
Container.Position = UDim2.new(0, 120, 0, 40); Container.Size = UDim2.new(1, -130, 1, -50); Container.BackgroundTransparency = 1

local FarmPg = Instance.new("ScrollingFrame", Container); FarmPg.Size = UDim2.new(1,0,1,0); FarmPg.BackgroundTransparency = 1; FarmPg.ScrollBarThickness = 0
local PlayerPg = Instance.new("ScrollingFrame", Container); PlayerPg.Size = UDim2.new(1,0,1,0); PlayerPg.BackgroundTransparency = 1; PlayerPg.ScrollBarThickness = 0; PlayerPg.Visible = false
Instance.new("UIListLayout", FarmPg).Padding = UDim.new(0, 8); Instance.new("UIListLayout", PlayerPg).Padding = UDim.new(0, 8)

-- BOTÕES TOPO
local Close = Instance.new("TextButton", Main); Close.Position = UDim2.new(1, -30, 0, 10); Close.Size = UDim2.new(0, 20, 0, 20); Close.Text = "X"; Close.BackgroundColor3 = Color3.fromRGB(200, 50, 50); Instance.new("UICorner", Close)
local Min = Instance.new("TextButton", Main); Min.Position = UDim2.new(1, -60, 0, 10); Min.Size = UDim2.new(0, 20, 0, 20); Min.Text = "-"; Min.BackgroundColor3 = Color3.fromRGB(60, 60, 65); Instance.new("UICorner", Min)

Min.MouseButton1Click:Connect(function() Main.Visible = false; OpenBtn.Visible = true end)
OpenBtn.MouseButton1Click:Connect(function() Main.Visible = true; OpenBtn.Visible = false end)
Close.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- SISTEMA DE ABAS
local function CreateTab(n, p, pg)
    local b = Instance.new("TextButton", SideBar); b.Size = UDim2.new(0.9, 0, 0, 35); b.Position = p; b.Text = n; b.BackgroundColor3 = Color3.fromRGB(30, 30, 35); b.TextColor3 = Color3.fromRGB(200, 200, 200); Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() FarmPg.Visible = false; PlayerPg.Visible = false; pg.Visible = true end)
end
CreateTab("FARMS", UDim2.new(0.05, 0, 0, 60), FarmPg); CreateTab("PLAYER", UDim2.new(0.05, 0, 0, 105), PlayerPg)

-- FUNÇÃO TOGGLE
local function CreateToggle(n, pr, c)
    local b = Instance.new("TextButton", pr); b.Size = UDim2.new(0.95, 0, 0, 38); b.Text = n; b.BackgroundColor3 = Color3.fromRGB(40, 40, 45); b.TextColor3 = Color3.fromRGB(255, 255, 255); Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(c); return b
end

-- PÁGINA FARM (AUTO MASSA ORIGINAL)
local B2 = CreateToggle("AUTO COIN MASSA", FarmPg, function() Config.ToggleMassFarm = not Config.ToggleMassFarm; Salvar() end)
local B3 = CreateToggle("CORTAR GRAMA (NOCLIP)", FarmPg, function() 
    Config.ToggleAutoCut = not Config.ToggleAutoCut; Salvar() 
end)
local B4 = CreateToggle("ADD BASE IGNORADA", FarmPg, function() 
    if player.Character then local p = player.Character.HumanoidRootPart.Position; table.insert(Config.BasesIgnoradas, {p.X, p.Y, p.Z}); Salvar() end 
end)
local B_Reset = CreateToggle("LIMPAR TODAS AS BASES", FarmPg, function() Config.BasesIgnoradas = {}; Salvar() end)

-- PÁGINA PLAYER
local B5 = CreateToggle("VELOCIDADE", PlayerPg, function() Config.ToggleWalkSpeed = not Config.ToggleWalkSpeed; Salvar() end)
local Spd = Instance.new("TextBox", PlayerPg); Spd.Size = UDim2.new(0.95, 0, 0, 30); Spd.Text = tostring(Config.VelocidadeRapida); Spd.BackgroundColor3 = Color3.fromRGB(15, 15, 20); Spd.TextColor3 = Color3.fromRGB(255, 215, 0); Instance.new("UICorner", Spd)
Spd.FocusLost:Connect(function() Config.VelocidadeRapida = tonumber(Spd.Text) or 100; Salvar() end)
local B6 = CreateToggle("ANTI-AFK", PlayerPg, function() Config.ToggleAntiAFK = not Config.ToggleAntiAFK; Salvar() end)

-- MOTOR PRINCIPAL (RESTAURADO)
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            B2.BackgroundColor3 = Config.ToggleMassFarm and Color3.fromRGB(200, 150, 0) or Color3.fromRGB(40, 40, 45)
            B3.BackgroundColor3 = Config.ToggleAutoCut and Color3.fromRGB(0, 150, 200) or Color3.fromRGB(40, 40, 45)
            B5.BackgroundColor3 = Config.ToggleWalkSpeed and Color3.fromRGB(0, 120, 200) or Color3.fromRGB(40, 40, 45)
            B6.BackgroundColor3 = Config.ToggleAntiAFK and Color3.fromRGB(120, 50, 200) or Color3.fromRGB(40, 40, 45)
            B4.Text = "ADD BASE (" .. #Config.BasesIgnoradas .. ")"

            local char = player.Character
            if not char then return end

            -- Lógica Walkspeed
            if Config.ToggleWalkSpeed and char:FindFirstChild("Humanoid") then 
                char.Humanoid.WalkSpeed = Config.VelocidadeRapida 
            end

            -- Lógica Cortar Grama (NOCLIP)
            if Config.ToggleAutoCut then
                for _, part in ipairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end

            -- Lógica Anti-AFK
            if Config.ToggleAntiAFK then VirtualUser:CaptureController() end

            -- Lógica Auto Massa (MECÂNICA ORIGINAL RESTAURADA)
            if Config.ToggleMassFarm and player.Character then
                for _, o in pairs(workspace:GetDescendants()) do
                    if o:IsA("BasePart") and o:FindFirstChild("TouchInterest") then
                        local ign = false
                        for _, p in ipairs(Config.BasesIgnoradas) do 
                            if (Vector3.new(p[1],p[2],p[3]) - o.Position).Magnitude <= 180 then ign = true break end 
                        end
                        if not ign then 
                            player.Character.HumanoidRootPart.CFrame = CFrame.new(o.Position + Vector3.new(0,1.5,0))
                            task.wait(0.05)
                        end
                    end
                end
            end
        end)
    end
end)

-- Anti-Kick Anti-AFK
task.spawn(function()
    while task.wait(20) do
        if Config.ToggleAntiAFK then VirtualUser:ClickButton1(Vector2.new(0, 0)) end
    end
end)

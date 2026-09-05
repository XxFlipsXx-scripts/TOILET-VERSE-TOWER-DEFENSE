--==================================================
-- TOILET VERSE HUB
-- GUI BASE COM ABAS (WHITELIST)
--==================================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TeleportService = game:GetService("TeleportService")

local Player = Players.LocalPlayer

--==================================================
-- WHITELIST
--==================================================

local Whitelist = {
    "joaovitoofc2",
    "hugo2016df"
}

local function IsWhitelisted()
    for _, name in pairs(Whitelist) do
        if Player.Name == name then
            return true
        end
    end
    return false
end

if not IsWhitelisted() then
    Player:Kick("Você não tem permissão para usar este script!")
    return
end

--==================================================
-- CONFIG
--==================================================

local IMAGE_ID = "10316531039"

-- Posições dos Mapas
local MapPositions = {
    Endless = CFrame.new(-670.560425, 189.44162, 700.097412, 1, 0, 0, 0, 1, 0, 0, 0, 1),
    Cemiterio = CFrame.new(-424.078369, 209.857224, 752.09491, 0, 0, -1, 0, 1, 0, 1, 0, 0),
    GodSpeak = CFrame.new(-1232.84851, 206.541626, 732.705078, 0, 0, 1, 0, 1, -0, -1, 0, 0),
    CidadeDestruida = CFrame.new(-370.203308, 216.357224, 696.220215, 0, 0, -1, 0, 1, 0, 1, 0, 0)
}

-- Posição do Parasited Cameraman (Normal)
local ParasitedPosition = CFrame.new(103.625, 47.125, -44.25, 1, 0, 0, 0, 1, 0, 0, 0, 1)

-- Posição do Parasited Endless
local ParasitedEndlessPosition = CFrame.new(103.5625, 43.3129997, 28.625, -1, 0, 0, 0, 1, 0, 0, 0, -1)

--==================================================
-- PALETA DE CORES (PRETO, BRANCO E VERMELHO)
--==================================================

local Colors = {
    Background = Color3.fromRGB(15, 15, 15),
    Surface = Color3.fromRGB(25, 25, 25),
    SurfaceLight = Color3.fromRGB(40, 40, 40),
    Accent = Color3.fromRGB(255, 0, 0),
    AccentDark = Color3.fromRGB(150, 0, 0),
    Text = Color3.fromRGB(255, 255, 255),
    TextDim = Color3.fromRGB(150, 150, 150),
    ToggleOn = Color3.fromRGB(255, 0, 0),
    ToggleOff = Color3.fromRGB(50, 50, 50),
    CircleOn = Color3.fromRGB(255, 255, 255),
    CircleOff = Color3.fromRGB(150, 150, 150),
    Gold = Color3.fromRGB(255, 215, 0),
    Locked = Color3.fromRGB(100, 100, 100)
}

--==================================================
-- VARIÁVEIS DE ESTADO
--==================================================

local GodModeEnabled = false
local GodModeConnection = nil
local FlyEnabled = false
local FlyConnection = nil
local NoclipEnabled = false
local NoclipConnection = nil
local AutoParasitedEnabled = false
local AutoParasitedConnection = nil
local AutoClickConnection = nil
local AutoParasitedEndlessEnabled = false
local AutoParasitedEndlessConnection = nil
local AutoClickEndlessConnection = nil
local AntiAFKEnabled = false
local AntiAFKConnection = nil

--==================================================
-- GUI
--==================================================

local Gui = Instance.new("ScreenGui")
Gui.Name = "ToiletVerseHub"
Gui.ResetOnSpawn = false
Gui.IgnoreGuiInset = true
Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
Gui.Parent = game:GetService("CoreGui")

--==================================================
-- BOTÃO FLUTUANTE (PRETO COM EMOJI DE CAIXA DE SOM)
--==================================================

local FloatButton = Instance.new("TextButton")
FloatButton.Name = "TzButton"
FloatButton.Size = UDim2.fromOffset(65, 65)
FloatButton.Position = UDim2.new(0, 20, 0.5, -32)
FloatButton.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
FloatButton.BorderSizePixel = 0
FloatButton.Text = "🔊"
FloatButton.TextSize = 30
FloatButton.AutoButtonColor = false
FloatButton.Visible = true
FloatButton.Active = true
FloatButton.Selectable = true
FloatButton.Parent = Gui

local FloatCorner = Instance.new("UICorner")
FloatCorner.CornerRadius = UDim.new(1, 0)
FloatCorner.Parent = FloatButton

local FloatStroke = Instance.new("UIStroke")
FloatStroke.Thickness = 2
FloatStroke.Color = Colors.Accent
FloatStroke.Parent = FloatButton

--==================================================
-- MENU PRINCIPAL
--==================================================

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(500, 500)
Main.Position = UDim2.new(0.5, -250, 0.5, -250)
Main.BackgroundColor3 = Colors.Background
Main.BorderSizePixel = 0
Main.Visible = false
Main.Parent = Gui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 16)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 1
MainStroke.Color = Colors.AccentDark
MainStroke.Parent = Main

--==================================================
-- HEADER
--==================================================

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -70, 0, 55)
Title.Position = UDim2.fromOffset(20, 5)
Title.BackgroundTransparency = 1
Title.Text = "Toilet"
Title.TextColor3 = Colors.Accent
Title.TextSize = 25
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Main

local Subtitle = Instance.new("TextLabel")
Subtitle.Size = UDim2.new(1, -70, 0, 20)
Subtitle.Position = UDim2.fromOffset(22, 38)
Subtitle.BackgroundTransparency = 1
Subtitle.Text = "Verse Hub"
Subtitle.TextColor3 = Colors.TextDim
Subtitle.TextSize = 11
Subtitle.Font = Enum.Font.Gotham
Subtitle.TextTransparency = 0.4
Subtitle.TextXAlignment = Enum.TextXAlignment.Left
Subtitle.Parent = Main

--==================================================
-- FECHAR
--==================================================

local Close = Instance.new("TextButton")
Close.Size = UDim2.fromOffset(38, 38)
Close.Position = UDim2.new(1, -50, 0, 13)
Close.BackgroundColor3 = Colors.SurfaceLight
Close.BorderSizePixel = 0
Close.Text = "×"
Close.TextColor3 = Colors.Text
Close.TextSize = 25
Close.Font = Enum.Font.GothamBold
Close.Parent = Main

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 10)
CloseCorner.Parent = Close

Close.MouseEnter:Connect(function()
    TweenService:Create(Close, TweenInfo.new(0.2), {
        BackgroundColor3 = Colors.Accent
    }):Play()
end)

Close.MouseLeave:Connect(function()
    TweenService:Create(Close, TweenInfo.new(0.2), {
        BackgroundColor3 = Colors.SurfaceLight
    }):Play()
end)

Close.MouseButton1Click:Connect(function()
    Main.Visible = false
end)

--==================================================
-- SISTEMA DE ABAS
--==================================================

local TabContainer = Instance.new("Frame")
TabContainer.Name = "TabContainer"
TabContainer.Size = UDim2.new(1, -30, 0, 45)
TabContainer.Position = UDim2.fromOffset(15, 80)
TabContainer.BackgroundTransparency = 1
TabContainer.Parent = Main

local TabLayout = Instance.new("UIListLayout")
TabLayout.Padding = UDim.new(0, 3)
TabLayout.FillDirection = Enum.FillDirection.Horizontal
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.Parent = TabContainer

local function CreateTab(Name, LayoutOrder)
    local Tab = Instance.new("TextButton")
    Tab.Name = Name
    Tab.Size = UDim2.new(0, 90, 1, 0)
    Tab.BackgroundColor3 = Colors.Surface
    Tab.BorderSizePixel = 0
    Tab.Text = Name
    Tab.TextColor3 = Colors.TextDim
    Tab.TextSize = 12
    Tab.Font = Enum.Font.GothamMedium
    Tab.AutoButtonColor = false
    Tab.LayoutOrder = LayoutOrder
    Tab.Parent = TabContainer
    
    local TabCorner = Instance.new("UICorner")
    TabCorner.CornerRadius = UDim.new(0, 8)
    TabCorner.Parent = Tab
    
    return Tab
end

local CreditsTab = CreateTab("Créditos", 1)
local VerseTab = CreateTab("Verse", 2)
local MapsTab = CreateTab("Maps", 3)
local GodModeTab = CreateTab("🔒 God Mode", 4)
local ConfigTab = CreateTab("Config", 5)

--==================================================
-- CONTAINERS DAS ABAS
--==================================================

local function CreateTabContent(LayoutOrder)
    local Content = Instance.new("Frame")
    Content.Name = "TabContent"
    Content.Size = UDim2.new(1, -30, 1, -135)
    Content.Position = UDim2.fromOffset(15, 130)
    Content.BackgroundTransparency = 1
    Content.Visible = false
    Content.LayoutOrder = LayoutOrder
    Content.Parent = Main
    
    local ContentLayout = Instance.new("UIListLayout")
    ContentLayout.Padding = UDim.new(0, 8)
    ContentLayout.SortOrder = Enum.SortOrder.LayoutOrder
    ContentLayout.Parent = Content
    
    return Content
end

local CreditsContent = CreateTabContent(1)
local VerseContent = CreateTabContent(2)
local MapsContent = CreateTabContent(3)
local GodModeContent = CreateTabContent(4)
local ConfigContent = CreateTabContent(5)

--==================================================
-- CONTEÚDO DA ABA CRÉDITOS (LADO ESQUERDO)
--==================================================

local ScriptTitle = Instance.new("TextLabel")
ScriptTitle.Name = "ScriptTitle"
ScriptTitle.Size = UDim2.new(1, 0, 0, 35)
ScriptTitle.Position = UDim2.fromOffset(20, 20)
ScriptTitle.BackgroundTransparency = 1
ScriptTitle.Text = "TOILET VERSE HUB"
ScriptTitle.TextColor3 = Colors.Gold
ScriptTitle.TextSize = 18
ScriptTitle.Font = Enum.Font.GothamBold
ScriptTitle.TextXAlignment = Enum.TextXAlignment.Left
ScriptTitle.Parent = CreditsContent

local Line1 = Instance.new("Frame")
Line1.Size = UDim2.new(1, -40, 0, 1)
Line1.Position = UDim2.fromOffset(20, 60)
Line1.BackgroundColor3 = Colors.SurfaceLight
Line1.BorderSizePixel = 0
Line1.Parent = CreditsContent

local DevTitle = Instance.new("TextLabel")
DevTitle.Name = "DevTitle"
DevTitle.Size = UDim2.new(1, 0, 0, 25)
DevTitle.Position = UDim2.fromOffset(20, 70)
DevTitle.BackgroundTransparency = 1
DevTitle.Text = "Desenvolvedores"
DevTitle.TextColor3 = Colors.TextDim
DevTitle.TextSize = 12
DevTitle.Font = Enum.Font.GothamMedium
DevTitle.TextXAlignment = Enum.TextXAlignment.Left
DevTitle.Parent = CreditsContent

local Dev1Name = Instance.new("TextLabel")
Dev1Name.Name = "Dev1Name"
Dev1Name.Size = UDim2.new(1, 0, 0, 25)
Dev1Name.Position = UDim2.fromOffset(20, 100)
Dev1Name.BackgroundTransparency = 1
Dev1Name.Text = "DEV-Tz"
Dev1Name.TextColor3 = Colors.Gold
Dev1Name.TextSize = 15
Dev1Name.Font = Enum.Font.GothamBold
Dev1Name.TextXAlignment = Enum.TextXAlignment.Left
Dev1Name.Parent = CreditsContent

local Dev2Name = Instance.new("TextLabel")
Dev2Name.Name = "Dev2Name"
Dev2Name.Size = UDim2.new(1, 0, 0, 25)
Dev2Name.Position = UDim2.fromOffset(20, 130)
Dev2Name.BackgroundTransparency = 1
Dev2Name.Text = "DEV-Flips"
Dev2Name.TextColor3 = Colors.Gold
Dev2Name.TextSize = 15
Dev2Name.Font = Enum.Font.GothamBold
Dev2Name.TextXAlignment = Enum.TextXAlignment.Left
Dev2Name.Parent = CreditsContent

local Line2 = Instance.new("Frame")
Line2.Size = UDim2.new(1, -40, 0, 1)
Line2.Position = UDim2.fromOffset(20, 160)
Line2.BackgroundColor3 = Colors.SurfaceLight
Line2.BorderSizePixel = 0
Line2.Parent = CreditsContent

local UpdateLabel = Instance.new("TextLabel")
UpdateLabel.Name = "UpdateLabel"
UpdateLabel.Size = UDim2.new(1, 0, 0, 25)
UpdateLabel.Position = UDim2.fromOffset(20, 170)
UpdateLabel.BackgroundTransparency = 1
UpdateLabel.Text = "UPDT SOON"
UpdateLabel.TextColor3 = Colors.Accent
UpdateLabel.TextSize = 15
UpdateLabel.Font = Enum.Font.GothamBold
UpdateLabel.TextXAlignment = Enum.TextXAlignment.Left
UpdateLabel.Parent = CreditsContent

local UpdateInfo = Instance.new("TextLabel")
UpdateInfo.Name = "UpdateInfo"
UpdateInfo.Size = UDim2.new(1, 0, 0, 50)
UpdateInfo.Position = UDim2.fromOffset(20, 200)
UpdateInfo.BackgroundTransparency = 1
UpdateInfo.Text = "Sábado:\n• Auto Kill\n• God Mode"
UpdateInfo.TextColor3 = Colors.Text
UpdateInfo.TextSize = 13
UpdateInfo.Font = Enum.Font.GothamMedium
UpdateInfo.TextXAlignment = Enum.TextXAlignment.Left
UpdateInfo.Parent = CreditsContent

--==================================================
-- FUNÇÃO DE TOGGLE
--==================================================

local function CreateToggle(Name, Callback, ContentParent)
    local Row = Instance.new("TextButton")
    Row.Name = Name
    Row.Size = UDim2.new(1, 0, 0, 48)
    Row.BackgroundColor3 = Colors.Surface
    Row.BorderSizePixel = 0
    Row.Text = ""
    Row.AutoButtonColor = false
    Row.Parent = ContentParent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Row

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -75, 1, 0)
    Label.Position = UDim2.fromOffset(15, 0)
    Label.BackgroundTransparency = 1
    Label.Text = Name
    Label.TextColor3 = Colors.Text
    Label.TextSize = 14
    Label.Font = Enum.Font.GothamMedium
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Row

    local Toggle = Instance.new("Frame")
    Toggle.Size = UDim2.fromOffset(44, 24)
    Toggle.Position = UDim2.new(1, -58, 0.5, -12)
    Toggle.BackgroundColor3 = Colors.ToggleOff
    Toggle.BorderSizePixel = 0
    Toggle.Parent = Row

    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCorner.Parent = Toggle

    local Circle = Instance.new("Frame")
    Circle.Size = UDim2.fromOffset(18, 18)
    Circle.Position = UDim2.fromOffset(3, 3)
    Circle.BackgroundColor3 = Colors.CircleOff
    Circle.BorderSizePixel = 0
    Circle.Parent = Toggle

    local CircleCorner = Instance.new("UICorner")
    CircleCorner.CornerRadius = UDim.new(1, 0)
    CircleCorner.Parent = Circle

    local Enabled = false

    Row.MouseEnter:Connect(function()
        TweenService:Create(Row, TweenInfo.new(0.2), {
            BackgroundColor3 = Colors.SurfaceLight
        }):Play()
    end)

    Row.MouseLeave:Connect(function()
        TweenService:Create(Row, TweenInfo.new(0.2), {
            BackgroundColor3 = Colors.Surface
        }):Play()
    end)

    Row.MouseButton1Click:Connect(function()
        Enabled = not Enabled

        if Enabled then
            TweenService:Create(Toggle, TweenInfo.new(0.18), {
                BackgroundColor3 = Colors.ToggleOn
            }):Play()

            TweenService:Create(Circle, TweenInfo.new(0.18), {
                Position = UDim2.new(1, -21, 0, 3),
                BackgroundColor3 = Colors.CircleOn
            }):Play()
        else
            TweenService:Create(Toggle, TweenInfo.new(0.18), {
                BackgroundColor3 = Colors.ToggleOff
            }):Play()

            TweenService:Create(Circle, TweenInfo.new(0.18), {
                Position = UDim2.fromOffset(3, 3),
                BackgroundColor3 = Colors.CircleOff
            }):Play()
        end

        if Callback then
            Callback(Enabled)
        end
    end)

    return Row
end

--==================================================
-- FUNÇÃO DE BOTÃO DE MAPA
--==================================================

local function CreateMapButton(Name, MapCFrame, ContentParent)
    local Btn = Instance.new("TextButton")
    Btn.Name = Name
    Btn.Size = UDim2.new(1, 0, 0, 45)
    Btn.BackgroundColor3 = Colors.Surface
    Btn.BorderSizePixel = 0
    Btn.Text = Name
    Btn.TextColor3 = Colors.Text
    Btn.TextSize = 14
    Btn.Font = Enum.Font.GothamMedium
    Btn.AutoButtonColor = false
    Btn.Parent = ContentParent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Btn

    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.2), {
            BackgroundColor3 = Colors.SurfaceLight
        }):Play()
    end)

    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.2), {
            BackgroundColor3 = Colors.Surface
        }):Play()
    end)

    Btn.MouseButton1Click:Connect(function()
        if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            Player.Character.HumanoidRootPart.CFrame = MapCFrame
        end
    end)

    return Btn
end

--==================================================
-- FUNÇÃO DE SLIDER
--==================================================

local function CreateSlider(Name, Min, Max, Default, Callback, ContentParent)
    local Row = Instance.new("Frame")
    Row.Name = Name
    Row.Size = UDim2.new(1, 0, 0, 70)
    Row.BackgroundColor3 = Colors.Surface
    Row.BorderSizePixel = 0
    Row.Parent = ContentParent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Row

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -30, 0, 25)
    Label.Position = UDim2.fromOffset(15, 8)
    Label.BackgroundTransparency = 1
    Label.Text = Name
    Label.TextColor3 = Colors.Text
    Label.TextSize = 14
    Label.Font = Enum.Font.GothamMedium
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Row

    local ValueLabel = Instance.new("TextLabel")
    ValueLabel.Size = UDim2.new(0, 60, 0, 25)
    ValueLabel.Position = UDim2.new(1, -75, 0, 8)
    ValueLabel.BackgroundTransparency = 1
    ValueLabel.Text = tostring(Default)
    ValueLabel.TextColor3 = Colors.Accent
    ValueLabel.TextSize = 14
    ValueLabel.Font = Enum.Font.GothamBold
    ValueLabel.TextXAlignment = Enum.TextXAlignment.Right
    ValueLabel.Parent = Row

    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(1, -30, 0, 6)
    SliderFrame.Position = UDim2.fromOffset(15, 45)
    SliderFrame.BackgroundColor3 = Colors.SurfaceLight
    SliderFrame.BorderSizePixel = 0
    SliderFrame.Parent = Row

    local SliderCorner = Instance.new("UICorner")
    SliderCorner.CornerRadius = UDim.new(1, 0)
    SliderCorner.Parent = SliderFrame

    local Fill = Instance.new("Frame")
    Fill.Size = UDim2.new((Default - Min) / (Max - Min), 0, 1, 0)
    Fill.BackgroundColor3 = Colors.Accent
    Fill.BorderSizePixel = 0
    Fill.Parent = SliderFrame

    local FillCorner = Instance.new("UICorner")
    FillCorner.CornerRadius = UDim.new(1, 0)
    FillCorner.Parent = Fill

    local SliderButton = Instance.new("TextButton")
    SliderButton.Size = UDim2.fromOffset(20, 20)
    SliderButton.Position = UDim2.new((Default - Min) / (Max - Min), -10, 0.5, -10)
    SliderButton.BackgroundColor3 = Colors.CircleOn
    SliderButton.BorderSizePixel = 0
    SliderButton.Text = ""
    SliderButton.AutoButtonColor = false
    SliderButton.Parent = SliderFrame

    local SliderButtonCorner = Instance.new("UICorner")
    SliderButtonCorner.CornerRadius = UDim.new(1, 0)
    SliderButtonCorner.Parent = SliderButton

    local Dragging = false

    local function UpdateSlider(Input)
        local SliderPos = Input.Position.X - SliderFrame.AbsolutePosition.X
        local Size = SliderFrame.AbsoluteSize.X
        local Percent = math.clamp(SliderPos / Size, 0, 1)
        local Value = math.floor(Min + (Max - Min) * Percent)
        
        Fill.Size = UDim2.new(Percent, 0, 1, 0)
        SliderButton.Position = UDim2.new(Percent, -10, 0.5, -10)
        ValueLabel.Text = tostring(Value)
        
        if Callback then
            Callback(Value)
        end
    end

    SliderButton.InputBegan:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton1 then
            Dragging = true
        end
    end)

    SliderButton.InputEnded:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton1 then
            Dragging = false
        end
    end)

    SliderFrame.InputBegan:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton1 then
            Dragging = true
            UpdateSlider(Input)
        end
    end)

    UserInputService.InputChanged:Connect(function(Input)
        if Dragging and Input.UserInputType == Enum.UserInputType.MouseMovement then
            UpdateSlider(Input)
        end
    end)

    UserInputService.InputEnded:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton1 then
            Dragging = false
        end
    end)

    return Row
end

--==================================================
-- AUTO PARASITED (NORMAL)
--==================================================

local function AutoParasited()
    if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
        Player.Character.HumanoidRootPart.CFrame = ParasitedPosition
    end
end

local function AutoClickE()
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, nil)
    wait(0.1)
    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, nil)
end

--==================================================
-- AUTO PARASITED ENDLESS
--==================================================

local function AutoParasitedEndless()
    if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
        Player.Character.HumanoidRootPart.CFrame = ParasitedEndlessPosition
    end
end

local function AutoClickEndless()
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, nil)
    wait(0.1)
    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, nil)
end

--==================================================
-- ABA VERSE
--==================================================

CreateToggle("Auto Parasited", function(enabled)
    AutoParasitedEnabled = enabled
    if enabled then
        AutoParasitedConnection = RunService.Heartbeat:Connect(function()
            AutoParasited()
        end)
        
        AutoClickConnection = RunService.Heartbeat:Connect(function()
            AutoClickE()
            wait(0.5)
        end)
    else
        if AutoParasitedConnection then
            AutoParasitedConnection:Disconnect()
            AutoParasitedConnection = nil
        end
        if AutoClickConnection then
            AutoClickConnection:Disconnect()
            AutoClickConnection = nil
        end
    end
end, VerseContent)

CreateToggle("Auto Parasited Endless", function(enabled)
    AutoParasitedEndlessEnabled = enabled
    if enabled then
        AutoParasitedEndlessConnection = RunService.Heartbeat:Connect(function()
            AutoParasitedEndless()
        end)
        
        AutoClickEndlessConnection = RunService.Heartbeat:Connect(function()
            AutoClickEndless()
            wait(0.5)
        end)
    else
        if AutoParasitedEndlessConnection then
            AutoParasitedEndlessConnection:Disconnect()
            AutoParasitedEndlessConnection = nil
        end
        if AutoClickEndlessConnection then
            AutoClickEndlessConnection:Disconnect()
            AutoClickEndlessConnection = nil
        end
    end
end, VerseContent)

--==================================================
-- ABA MAPS
--==================================================

CreateMapButton("Endless", MapPositions.Endless, MapsContent)
CreateMapButton("Cemitério", MapPositions.Cemiterio, MapsContent)
CreateMapButton("God Speak", MapPositions.GodSpeak, MapsContent)
CreateMapButton("Cidade Destruída", MapPositions.CidadeDestruida, MapsContent)

--==================================================
-- ABA GOD MODE (BLOQUEADA)
--==================================================

local LockedLabel = Instance.new("TextLabel")
LockedLabel.Size = UDim2.new(1, 0, 0, 50)
LockedLabel.Position = UDim2.new(0, 0, 0.5, -25)
LockedLabel.BackgroundTransparency = 1
LockedLabel.Text = "🔒 BLOQUEADO\nDisponível em breve!"
LockedLabel.TextColor3 = Colors.Locked
LockedLabel.TextSize = 16
LockedLabel.Font = Enum.Font.GothamBold
LockedLabel.TextXAlignment = Enum.TextXAlignment.Center
LockedLabel.Parent = GodModeContent

--==================================================
-- ABA CONFIG
--==================================================

CreateToggle("Anti AFK", function(enabled)
    AntiAFKEnabled = enabled
    if enabled then
        AntiAFKConnection = RunService.Heartbeat:Connect(function()
            VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Space, false, nil)
            wait(0.1)
            VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Space, false, nil)
            wait(5)
        end)
    else
        if AntiAFKConnection then
            AntiAFKConnection:Disconnect()
            AntiAFKConnection = nil
        end
    end
end, ConfigContent)

CreateToggle("Rejoin", function(enabled)
    if enabled then
        local CurrentPlace = game.PlaceId
        TeleportService:Teleport(CurrentPlace)
    end
end, ConfigContent)

--==================================================
-- FUNÇÃO PARA TROCAR DE ABA
--==================================================

local function SwitchTab(Tab, Content)
    CreditsTab.BackgroundColor3 = Colors.Surface
    CreditsTab.TextColor3 = Colors.TextDim
    VerseTab.BackgroundColor3 = Colors.Surface
    VerseTab.TextColor3 = Colors.TextDim
    MapsTab.BackgroundColor3 = Colors.Surface
    MapsTab.TextColor3 = Colors.TextDim
    GodModeTab.BackgroundColor3 = Colors.Surface
    GodModeTab.TextColor3 = Colors.Locked
    ConfigTab.BackgroundColor3 = Colors.Surface
    ConfigTab.TextColor3 = Colors.TextDim
    
    Tab.BackgroundColor3 = Colors.Accent
    Tab.TextColor3 = Colors.Text
    
    CreditsContent.Visible = (Content == CreditsContent)
    VerseContent.Visible = (Content == VerseContent)
    MapsContent.Visible = (Content == MapsContent)
    GodModeContent.Visible = (Content == GodModeContent)
    ConfigContent.Visible = (Content == ConfigContent)
end

CreditsTab.MouseButton1Click:Connect(function()
    SwitchTab(CreditsTab, CreditsContent)
end)

VerseTab.MouseButton1Click:Connect(function()
    SwitchTab(VerseTab, VerseContent)
end)

MapsTab.MouseButton1Click:Connect(function()
    SwitchTab(MapsTab, MapsContent)
end)

GodModeTab.MouseButton1Click:Connect(function()
    SwitchTab(GodModeTab, GodModeContent)
end)

ConfigTab.MouseButton1Click:Connect(function()
    SwitchTab(ConfigTab, ConfigContent)
end)

-- Iniciar com Créditos selecionado
SwitchTab(CreditsTab, CreditsContent)

--==================================================
-- ABRIR / FECHAR
--==================================================

FloatButton.MouseButton1Click:Connect(function()
    Main.Visible = not Main.Visible
end)

--==================================================
-- ARRASTAR BOLINHA
--==================================================

local Dragging = false
local DragStart
local StartPosition

FloatButton.InputBegan:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton1 or 
       Input.UserInputType == Enum.UserInputType.Touch then
        Dragging = true
        DragStart = Input.Position
        StartPosition = FloatButton.Position
    end
end)

FloatButton.InputEnded:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton1 or 
       Input.UserInputType == Enum.UserInputType.Touch then
        Dragging = false
    end
end)

UserInputService.InputChanged:Connect(function(Input)
    if Dragging and (Input.UserInputType == Enum.UserInputType.MouseMovement or 
                     Input.UserInputType == Enum.UserInputType.Touch) then
        local Delta = Input.Position - DragStart
        FloatButton.Position = UDim2.new(
            StartPosition.X.Scale,
            StartPosition.X.Offset + Delta.X,
            StartPosition.Y.Scale,
            StartPosition.Y.Offset + Delta.Y
        )
    end
end)

print("Toilet Verse Hub carregado com sucesso!")

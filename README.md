--[[
    Volleyball Legends Style Menu - Custom GUI Script
    - Autor: ChatGPT Copilot
    - Crie este LocalScript em StarterGui.
    - Não é necessário criar nada manualmente, o script gera toda a interface.
    - Você pode adaptar para salvar o estilo do jogador no DataStore/servidor posteriormente.
--]]

-- Serviços
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local plr = Players.LocalPlayer

-- Cores
local colors = {
    bg = Color3.fromRGB(25, 25, 25),          -- Preto quase puro de fundo
    accent = Color3.fromRGB(220, 35, 51),     -- Vermelho forte para destaque
    white = Color3.fromRGB(235, 235, 235),    -- Branco sutil
    selected = Color3.fromRGB(220, 35, 51),   -- Vermelho para botão selecionado
    unselected = Color3.fromRGB(35, 35, 35),  -- Preto escuro para não selecionado
    border = Color3.fromRGB(220, 35, 51),     -- Vermelho para borda
}

-- Estilos Disponíveis
local styleList = { "Power", "Speed", "Defense", "Setter", "Balanced" }
local selectedStyle = nil

-- GUI CRIAÇÃO DINÂMICA
local gui = Instance.new("ScreenGui")
gui.Name = "VLStyleGui"
gui.Parent = plr:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

-- Botão Flutuante para abrir menu
local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0, 56, 0, 56)
openBtn.Position = UDim2.new(0, 32, 1, -88)
openBtn.AnchorPoint = Vector2.new(0,1)
openBtn.BackgroundColor3 = colors.accent
openBtn.Text = "≡"
openBtn.TextSize = 36
openBtn.Font = Enum.Font.GothamBlack
openBtn.TextColor3 = colors.white
openBtn.Parent = gui
openBtn.AutoButtonColor = true
openBtn.ZIndex = 2

-- Estilize botão
local openUICorner = Instance.new("UICorner", openBtn)
openUICorner.CornerRadius = UDim.new(1,0)

-- Menu principal (frame oculto inicialmente)
local menu = Instance.new("Frame")
menu.Size = UDim2.new(0, 380, 0, 380)
menu.Position = UDim2.new(0.5, -190, 0.5, -190)
menu.BackgroundColor3 = colors.bg
menu.Visible = false
menu.Parent = gui
menu.Active = true
menu.Draggable = true
menu.ZIndex = 5

local menuUICorner = Instance.new("UICorner", menu)
menuUICorner.CornerRadius = UDim.new(0,18)

local menuUIStroke = Instance.new("UIStroke", menu)
menuUIStroke.Thickness = 3
menuUIStroke.Color = colors.border

-- Título
local title = Instance.new("TextLabel")
title.Text = "Volleyball Legends Style Menu"
title.Size = UDim2.new(1,0,0,48)
title.BackgroundTransparency = 1
title.TextColor3 = colors.white
title.TextSize = 26
title.Font = Enum.Font.GothamBlack
title.Parent = menu
title.ZIndex = 6

-- Botão Fechar
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 44, 0, 44)
closeBtn.Position = UDim2.new(1,-52,0,8)
closeBtn.BackgroundColor3 = colors.accent
closeBtn.Text = "✕"
closeBtn.Font = Enum.Font.GothamBlack
closeBtn.TextSize = 24
closeBtn.TextColor3 = colors.white
closeBtn.Parent = menu
closeBtn.ZIndex = 7
closeBtn.AutoButtonColor = true
closeBtn.AnchorPoint = Vector2.new(0,0)

Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(1,0)

-- Lista de estilos
local styleFrame = Instance.new("Frame")
styleFrame.Size = UDim2.new(1,-40,0,168)
styleFrame.Position = UDim2.new(0, 20, 0, 70)
styleFrame.BackgroundTransparency = 1
styleFrame.Parent = menu
styleFrame.ZIndex = 7

-- Container para botões dos estilos
local UIListLayout = Instance.new("UIListLayout", styleFrame)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.FillDirection = Enum.FillDirection.Vertical
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Guardar os botões para manipular visual
local styleButtons = {}

for i, styleName in ipairs(styleList) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 36)
    btn.BackgroundColor3 = colors.unselected
    btn.Text = styleName
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 22
    btn.TextColor3 = colors.white
    btn.Parent = styleFrame
    btn.ZIndex = 8
    btn.AutoButtonColor = false
    btn.LayoutOrder = i

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0,12)

    local stroke = Instance.new("UIStroke", btn)
    stroke.Color = colors.border
    stroke.Thickness = 1.5
    stroke.Transparency = 0.7

    styleButtons[styleName] = btn

    btn.MouseButton1Click:Connect(function()
        -- Destaca o botão escolhido e "desmarca" os demais
        for _, otherBtn in pairs(styleButtons) do
            local highlight = styleName == otherBtn.Text
            otherBtn.BackgroundColor3 = highlight and colors.selected or colors.unselected
            otherBtn.TextColor3 = highlight and colors.white or Color3.fromRGB(120,120,120)
            otherBtn.UIStroke.Transparency = highlight and 0 or 0.7
        end
        selectedStyle = styleName
    end)
end

-- Botão confirmar
local confirmBtn = Instance.new("TextButton")
confirmBtn.Size = UDim2.new(0.7,0,0,48)
confirmBtn.Position = UDim2.new(0.15,0,1,-64)
confirmBtn.AnchorPoint = Vector2.new(0,0)
confirmBtn.BackgroundColor3 = colors.accent
confirmBtn.Text = "Confirmar"
confirmBtn.Font = Enum.Font.GothamBold
confirmBtn.TextSize = 24
confirmBtn.TextColor3 = colors.white
confirmBtn.Parent = menu
confirmBtn.ZIndex = 8
confirmBtn.AutoButtonColor = true

Instance.new("UICorner", confirmBtn).CornerRadius = UDim.new(0,14)
Instance.new("UIStroke", confirmBtn).Color = colors.border

-- Mensagem de sucesso (Label oculta)
local msgLabel = Instance.new("TextLabel")
msgLabel.Size = UDim2.new(1,0,0,44)
msgLabel.Position = UDim2.new(0,0,1,-45)
msgLabel.BackgroundTransparency = 1
msgLabel.TextColor3 = colors.accent
msgLabel.TextSize = 24
msgLabel.Font = Enum.Font.GothamBold
msgLabel.Text = ""
msgLabel.Visible = false
msgLabel.Parent = menu
msgLabel.ZIndex = 10

-- ANIMAÇÕES DE ABRIR/FECHAR MENU
local function animateMenu(show)
    menu.Visible = true
    if show then
        menu.Position = UDim2.new(0.5, -190, 0.5, 50)
        menu.BackgroundTransparency = 1
        TweenService:Create(menu, TweenInfo.new(0.33, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
            {Position = UDim2.new(0.5, -190, 0.5, -190), BackgroundTransparency = 0}):Play()
    else
        TweenService:Create(menu, TweenInfo.new(0.28, Enum.EasingStyle.Sine, Enum.EasingDirection.In),
            {Position = UDim2.new(0.5, -190, 0.5, 50), BackgroundTransparency = 1}):Play()
        task.wait(0.28)
        menu.Visible = false
    end
end

-- ABRIR MENU
openBtn.MouseButton1Click:Connect(function()
    animateMenu(true)
end)

-- FECHAR MENU
closeBtn.MouseButton1Click:Connect(function()
    animateMenu(false)
end)

-- CONFIRMAR ESTILO SELECIONADO
confirmBtn.MouseButton1Click:Connect(function()
    if selectedStyle then
        -- Salvando a escolha localmente (pode ser expandido para salvar no DataStore/servidor usando RemoteEvent)
        plr:SetAttribute("VL_SelectedStyle", selectedStyle)

        msgLabel.Text = "Estilo selecionado: " .. selectedStyle
        msgLabel.Visible = true

        -- Oculta mensagem após 2 segundos
        task.spawn(function()
            wait(2)
            msgLabel.Visible = false
        end)
    else
        msgLabel.Text = "Escolha um estilo primeiro!"
        msgLabel.Visible = true
        task.spawn(function()
            wait(1.5)
            msgLabel.Visible = false
        end)
    end
end)

-- MOBILE FRIENDLY: Garante adaptação da interface em dispositivos mobile (Scale)
gui.IgnoreGuiInset = true
gui.DisplayOrder = 1000

-- Torna menu arrastável em todos dispositivos (Mobile e PC) com limites
local UserInputService = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos
local function update(input)
    local delta = input.Position - dragStart
    menu.Position = UDim2.new(
        menu.Position.X.Scale,
        math.clamp(startPos.X.Offset + delta.X, 0, gui.AbsoluteSize.X - menu.AbsoluteSize.X),
        menu.Position.Y.Scale,
        math.clamp(startPos.Y.Offset + delta.Y, 0, gui.AbsoluteSize.Y - menu.AbsoluteSize.Y)
    )
end

menu.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = menu.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

menu.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        update(input)
    end
end)

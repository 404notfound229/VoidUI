
---




--[[
    VoidUI Library v1.1
    GitHub: https://github.com/YOUR_USERNAME/VoidUI
    
    Features:
    - Auto FPS Boost
    - Persistent Watermark
    - Modern UI Design
    - Easy API
]]

local Players          = game:GetService("Players")
local TweenService     = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService       = game:GetService("RunService")
local StatsService     = game:GetService("Stats")
local TextService      = game:GetService("TextService")

local VoidUI = {}
VoidUI.__index = VoidUI

local function getPlayerGui()
    local p = Players.LocalPlayer
    if not p then
        Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
        p = Players.LocalPlayer
    end
    return p:WaitForChild("PlayerGui")
end

local function makeDraggable(frame, dragHandle)
    dragHandle = dragHandle or frame
    local dragging, dragStart, startPos
    dragHandle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if not dragging then return end
        if input.UserInputType ~= Enum.UserInputType.MouseMovement and input.UserInputType ~= Enum.UserInputType.Touch then return end
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end)
end

function VoidUI:SelectTab(tabObj)
    local window = self
    if window._selectedTab == tabObj then return end
    local previous = window._selectedTab
    if previous then
        previous._page.Visible = false
        TweenService:Create(previous._button, TweenInfo.new(0.18), {ImageColor3 = Color3.fromRGB(180,180,185)}):Play()
        TweenService:Create(previous._line, TweenInfo.new(0.18), {Size = UDim2.new(0,0,0,2)}):Play()
    end
    window._selectedTab = tabObj
    tabObj._page.Visible = true
    tabObj._page.BackgroundTransparency = 1
    TweenService:Create(tabObj._page, TweenInfo.new(0.18), {BackgroundTransparency = 0}):Play()
    TweenService:Create(tabObj._button, TweenInfo.new(0.18), {ImageColor3 = window._accent}):Play()
    TweenService:Create(tabObj._line, TweenInfo.new(0.18), {Size = UDim2.new(0,20,0,2)}):Play()
end

function VoidUI:Toggle(visible)
    if visible == nil then
        visible = not self._visible
    end
    
    self._visible = visible
    self._main.Visible = visible
end

function VoidUI:IsVisible()
    return self._visible
end

function VoidUI:SetToggleKey(keyCode)
    self._toggleKey = keyCode
end

function VoidUI:GetToggleKey()
    return self._toggleKey
end

function VoidUI:CreateWindow(cfg)
    cfg = cfg or {}
    local title = cfg.Title or "VOID.PP"
    local accentColor = cfg.AccentColor or Color3.fromRGB(0,170,255)
    local size = cfg.Size or UDim2.new(0,720,0,560)
    local toggleKey = cfg.ToggleKey or Enum.KeyCode.Insert
    local gui = getPlayerGui()

    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "VoidUI"
    screenGui.IgnoreGuiInset = true
    screenGui.ResetOnSpawn = false
    screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    screenGui.Parent = gui

    local main = Instance.new("Frame")
    main.Name = "Main"
    main.AnchorPoint = Vector2.new(0.5,0.5)
    main.Position = UDim2.new(0.5,0,0.5,0)
    main.Size = size
    main.BackgroundColor3 = Color3.fromRGB(15,15,17)
    main.BorderSizePixel = 0
    main.Visible = true
    main.Parent = screenGui
    Instance.new("UICorner", main).CornerRadius = UDim.new(0,12)
    local mainStroke = Instance.new("UIStroke", main)
    mainStroke.Color = Color3.fromRGB(30,30,33)

    local topBarBG = Instance.new("Frame")
    topBarBG.Name = "TopBarBG"
    topBarBG.Size = UDim2.new(1,0,0,48)
    topBarBG.BackgroundColor3 = Color3.fromRGB(20,20,23)
    topBarBG.BorderSizePixel = 0
    topBarBG.Parent = main
    Instance.new("UICorner", topBarBG).CornerRadius = UDim.new(0,12)

    local topBarLine = Instance.new("Frame")
    topBarLine.AnchorPoint = Vector2.new(0.5,1)
    topBarLine.Position = UDim2.new(0.5,0,1,0)
    topBarLine.Size = UDim2.new(1,-2,0,1)
    topBarLine.BackgroundColor3 = Color3.fromRGB(40,40,44)
    topBarLine.BorderSizePixel = 0
    topBarLine.Parent = topBarBG

    local topBar = Instance.new("Frame")
    topBar.Name = "TopBar"
    topBar.Size = UDim2.new(1,0,0,48)
    topBar.BackgroundTransparency = 1
    topBar.Parent = main

    local tabBar = Instance.new("Frame")
    tabBar.Name = "TabBar"
    tabBar.BackgroundTransparency = 1
    tabBar.Size = UDim2.new(0,260,1,0)
    tabBar.Position = UDim2.new(0,12,0,0)
    tabBar.Parent = topBar

    local tabLayout = Instance.new("UIListLayout", tabBar)
    tabLayout.FillDirection = Enum.FillDirection.Horizontal
    tabLayout.VerticalAlignment = Enum.VerticalAlignment.Center
    tabLayout.Padding = UDim.new(0,10)
    tabLayout.SortOrder = Enum.SortOrder.LayoutOrder

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Name = "Title"
    titleLabel.AnchorPoint = Vector2.new(1,0.5)
    titleLabel.Position = UDim2.new(1,-18,0.5,0)
    titleLabel.Size = UDim2.new(0,220,0,24)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.Text = title
    titleLabel.TextSize = 17
    titleLabel.TextXAlignment = Enum.TextXAlignment.Right
    titleLabel.TextColor3 = accentColor
    titleLabel.Parent = topBar

    local contentHolder = Instance.new("Frame")
    contentHolder.Name = "ContentHolder"
    contentHolder.Position = UDim2.new(0,12,0,60)
    contentHolder.Size = UDim2.new(1,-24,1,-72)
    contentHolder.BackgroundTransparency = 1
    contentHolder.Parent = main

    local window = setmetatable({}, VoidUI)
    window._accent = accentColor
    window._screenGui = screenGui
    window._main = main
    window._tabBar = tabBar
    window._contentHolder = contentHolder
    window._tabs = {}
    window._selectedTab = nil
    window._titleLabel = titleLabel
    window._visible = true
    window._toggleKey = toggleKey

    makeDraggable(main, topBar)

    main.BackgroundTransparency = 1
    topBarBG.BackgroundTransparency = 1
    
    task.delay(0.1, function()
        TweenService:Create(main, TweenInfo.new(0.25), {BackgroundTransparency = 0}):Play()
        TweenService:Create(topBarBG, TweenInfo.new(0.25), {BackgroundTransparency = 0}):Play()
    end)

    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        
        if input.KeyCode == window._toggleKey then
            window:Toggle()
        end
    end)

    -- Watermark
    local username = Players.LocalPlayer and Players.LocalPlayer.Name or "Player"
    local wmHolder = Instance.new("Frame")
    wmHolder.Name = "VoidWatermark"
    wmHolder.AnchorPoint = Vector2.new(1,0)
    wmHolder.Position = UDim2.new(1,-16,0,12)
    wmHolder.BackgroundColor3 = Color3.fromRGB(10,10,12)
    wmHolder.BorderSizePixel = 0
    wmHolder.AutomaticSize = Enum.AutomaticSize.XY
    wmHolder.Size = UDim2.new(0,0,0,26)
    wmHolder.Visible = true
    wmHolder.Parent = screenGui

    Instance.new("UICorner", wmHolder).CornerRadius = UDim.new(0,6)
    local wmStroke = Instance.new("UIStroke", wmHolder)
    wmStroke.Color = Color3.fromRGB(35,35,42)
    wmStroke.Transparency = 0.25

    local wmPadding = Instance.new("UIPadding", wmHolder)
    wmPadding.PaddingLeft = UDim.new(0,10)
    wmPadding.PaddingRight = UDim.new(0,12)
    wmPadding.PaddingTop = UDim.new(0,5)
    wmPadding.PaddingBottom = UDim.new(0,5)

    local wmLayout = Instance.new("UIListLayout", wmHolder)
    wmLayout.FillDirection = Enum.FillDirection.Horizontal
    wmLayout.VerticalAlignment = Enum.VerticalAlignment.Center
    wmLayout.Padding = UDim.new(0,6)
    wmLayout.SortOrder = Enum.SortOrder.LayoutOrder

    local wmTitle = Instance.new("TextLabel", wmHolder)
    wmTitle.BackgroundTransparency = 1
    wmTitle.AutomaticSize = Enum.AutomaticSize.XY
    wmTitle.Font = Enum.Font.GothamBold
    wmTitle.TextSize = 13
    wmTitle.RichText = true
    wmTitle.Text = '<font color="#00AFFF">VOID</font><font color="#FFFFFF">.PP</font>'
    wmTitle.LayoutOrder = 1

    local wmDiv = Instance.new("TextLabel", wmHolder)
    wmDiv.BackgroundTransparency = 1
    wmDiv.AutomaticSize = Enum.AutomaticSize.XY
    wmDiv.Font = Enum.Font.Ubuntu
    wmDiv.TextSize = 13
    wmDiv.TextColor3 = Color3.fromRGB(120,140,160)
    wmDiv.Text = "|"
    wmDiv.LayoutOrder = 2

    local wmInfo = Instance.new("TextLabel", wmHolder)
    wmInfo.BackgroundTransparency = 1
    wmInfo.AutomaticSize = Enum.AutomaticSize.XY
    wmInfo.Font = Enum.Font.Ubuntu
    wmInfo.TextSize = 13
    wmInfo.RichText = true
    wmInfo.LayoutOrder = 3

    local wmUnderline = Instance.new("Frame", wmHolder)
    wmUnderline.AnchorPoint = Vector2.new(0,1)
    wmUnderline.Position = UDim2.new(0,10,1,0)
    wmUnderline.Size = UDim2.new(0,0,0,2)
    wmUnderline.BackgroundColor3 = accentColor
    wmUnderline.BorderSizePixel = 0

    wmTitle:GetPropertyChangedSignal("TextBounds"):Connect(function()
        wmUnderline.Size = UDim2.new(0, wmTitle.TextBounds.X, 0, 2)
    end)

    wmHolder.BackgroundTransparency = 0
    wmStroke.Transparency = 0.25

    window._watermark = wmHolder

    task.spawn(function()
        local fps, ping, cpu = 0, 0, 0
        local frames, frameTimeSum, lastTime = 0, 0, tick()
        while wmHolder.Parent do
            local dt = RunService.RenderStepped:Wait()
            frames += 1
            frameTimeSum += dt
            if tick() - lastTime >= 0.5 then
                fps = math.floor(frames / (tick() - lastTime) + 0.5)
                cpu = math.clamp(math.floor((frameTimeSum / frames) / (1/60) * 100 + 0.5), 0, 100)
                frames, frameTimeSum, lastTime = 0, 0, tick()
                local ok, pingStat = pcall(function() return StatsService.Network.ServerStatsItem["Data Ping"] end)
                if ok and pingStat then
                    local ok2, val = pcall(function() return pingStat:GetValue() or pingStat.Value end)
                    if ok2 and type(val) == "number" then ping = math.floor(val + 0.5) end
                end
                wmInfo.Text = string.format(
                    '<font color="#AEB3C0">%s</font>  <font color="#78889C">|</font>  <font color="#FFFFFF">%d FPS</font>  <font color="#78889C">|</font>  <font color="#FFD966">%d ms</font>  <font color="#78889C">|</font>  <font color="#00C4FF">%d%% CPU</font>',
                    username, fps, ping, cpu
                )
            end
        end
    end)

    return window
end

function VoidUI:SetTitleRich(rich)
    if self._titleLabel then
        self._titleLabel.RichText = true
        self._titleLabel.Text = rich
    end
end

local TAB_ORDER = {
    ["Aim"] = 1,
    ["Visuals"] = 2,
    ["Settings"] = 3
}

function VoidUI:CreateTab(cfg)
    local window = self
    cfg = cfg or {}
    local name = cfg.Name or "Tab"
    local iconSize = cfg.IconSize or 24
    local order = cfg.Order or TAB_ORDER[name] or 99

    local textSize = TextService:GetTextSize(name, 14, Enum.Font.GothamBold, Vector2.new(1000,24))
    local width = math.clamp(textSize.X + 18, 52, 140)

    local button = Instance.new("ImageButton")
    button.Name = name .. "Tab"
    button.BackgroundTransparency = 1
    button.Size = UDim2.new(0, width, 0, iconSize)
    button.ImageTransparency = 1
    button.ImageColor3 = Color3.fromRGB(180,180,185)
    button.AutoButtonColor = false
    button.LayoutOrder = order
    button.Parent = window._tabBar

    local titleText = Instance.new("TextLabel", button)
    titleText.BackgroundTransparency = 1
    titleText.Size = UDim2.new(1,0,1,0)
    titleText.Font = Enum.Font.GothamBold
    titleText.Text = name
    titleText.TextSize = 14
    titleText.TextColor3 = button.ImageColor3

    button:GetPropertyChangedSignal("ImageColor3"):Connect(function()
        titleText.TextColor3 = button.ImageColor3
    end)

    local underline = Instance.new("Frame", button)
    underline.AnchorPoint = Vector2.new(0.5,1)
    underline.Position = UDim2.new(0.5,0,1,6)
    underline.Size = UDim2.new(0,0,0,2)
    underline.BackgroundColor3 = window._accent
    underline.BorderSizePixel = 0

    local page = Instance.new("Frame")
    page.Name = name .. "Page"
    page.Size = UDim2.new(1,0,1,0)
    page.BackgroundColor3 = Color3.fromRGB(12,12,14)
    page.BorderSizePixel = 0
    page.Visible = false
    page.Parent = window._contentHolder

    Instance.new("UICorner", page).CornerRadius = UDim.new(0,8)
    local pageStroke = Instance.new("UIStroke", page)
    pageStroke.Color = Color3.fromRGB(30,30,33)
    pageStroke.Transparency = 0.4

    local pagePadding = Instance.new("UIPadding", page)
    pagePadding.PaddingTop = UDim.new(0,12)
    pagePadding.PaddingBottom = UDim.new(0,12)
    pagePadding.PaddingLeft = UDim.new(0,12)
    pagePadding.PaddingRight = UDim.new(0,12)

    local sectionsHolder = Instance.new("Frame", page)
    sectionsHolder.Size = UDim2.new(1,0,1,0)
    sectionsHolder.BackgroundTransparency = 1

    local sectionsLayout = Instance.new("UIListLayout", sectionsHolder)
    sectionsLayout.FillDirection = Enum.FillDirection.Horizontal
    sectionsLayout.Padding = UDim.new(0,16)
    sectionsLayout.SortOrder = Enum.SortOrder.LayoutOrder

    local tab = {_window = window, _button = button, _line = underline, _page = page, _sectionsHolder = sectionsHolder, _accent = window._accent, _name = name, _order = order}

    function tab:CreateSection(title)
        local section = {}
        local frame = Instance.new("Frame")
        frame.Name = title .. "Section"
        frame.BackgroundColor3 = Color3.fromRGB(10,10,12)
        frame.BorderSizePixel = 0
        frame.Size = (name == "Visuals" or name == "Settings") and UDim2.new(1,0,1,-4) or UDim2.new(0.5,-8,1,-4)
        frame.Parent = sectionsHolder

        Instance.new("UICorner", frame).CornerRadius = UDim.new(0,8)
        local stroke = Instance.new("UIStroke", frame)
        stroke.Color = Color3.fromRGB(40,40,44)
        stroke.Transparency = 0.3

        local padding = Instance.new("UIPadding", frame)
        padding.PaddingTop = UDim.new(0,10)
        padding.PaddingBottom = UDim.new(0,10)
        padding.PaddingLeft = UDim.new(0,10)
        padding.PaddingRight = UDim.new(0,10)

        local titleLabel = Instance.new("TextLabel", frame)
        titleLabel.BackgroundTransparency = 1
        titleLabel.Size = UDim2.new(1,0,0,20)
        titleLabel.Font = Enum.Font.GothamBold
        titleLabel.Text = string.upper(title)
        titleLabel.TextSize = 16
        titleLabel.TextXAlignment = Enum.TextXAlignment.Left
        titleLabel.TextColor3 = window._accent

        local controls = Instance.new("Frame", frame)
        controls.BackgroundTransparency = 1
        controls.Position = UDim2.new(0,0,0,26)
        controls.Size = UDim2.new(1,0,1,-26)

        local controlsLayout = Instance.new("UIListLayout", controls)
        controlsLayout.Padding = UDim.new(0,6)
        controlsLayout.SortOrder = Enum.SortOrder.LayoutOrder

        section._frame = frame
        section._controls = controls
        section._window = window

        function section:SetTitleRich(rich)
            titleLabel.RichText = true
            titleLabel.Text = rich
        end

        function section:AddLabel(text, color)
            local lbl = Instance.new("TextLabel", controls)
            lbl.BackgroundTransparency = 1
            lbl.Size = UDim2.new(1,0,0,20)
            lbl.Font = Enum.Font.GothamMedium
            lbl.Text = text
            lbl.TextSize = 13
            lbl.TextXAlignment = Enum.TextXAlignment.Left
            lbl.TextColor3 = color or Color3.fromRGB(150,150,160)
            return lbl
        end

        function section:AddToggle(text, default, callback)
            default = default or false
            callback = callback or function() end

            local row = Instance.new("Frame", controls)
            row.BackgroundTransparency = 1
            row.Size = UDim2.new(1,0,0,22)

            local label = Instance.new("TextLabel", row)
            label.BackgroundTransparency = 1
            label.Size = UDim2.new(1,-60,1,0)
            label.Font = Enum.Font.GothamMedium
            label.Text = text
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.TextColor3 = Color3.fromRGB(205,205,210)

            local btn = Instance.new("TextButton", row)
            btn.AnchorPoint = Vector2.new(1,0.5)
            btn.Position = UDim2.new(1,0,0.5,0)
            btn.Size = UDim2.new(0,34,0,16)
            btn.BackgroundColor3 = Color3.fromRGB(40,40,44)
            btn.BorderSizePixel = 0
            btn.Text = ""
            btn.AutoButtonColor = false

            Instance.new("UICorner", btn).CornerRadius = UDim.new(1,0)

            local knob = Instance.new("Frame", btn)
            knob.AnchorPoint = Vector2.new(0.5,0.5)
            knob.Position = UDim2.new(0,8,0.5,0)
            knob.Size = UDim2.new(0,12,0,12)
            knob.BackgroundColor3 = Color3.fromRGB(220,220,225)
            knob.BorderSizePixel = 0
            Instance.new("UICorner", knob).CornerRadius = UDim.new(1,0)

            local toggled = default

            local function apply(animated)
                local bg = toggled and window._accent or Color3.fromRGB(40,40,44)
                local pos = toggled and UDim2.new(1,-8,0.5,0) or UDim2.new(0,8,0.5,0)
                local kc = toggled and Color3.new(1,1,1) or Color3.fromRGB(220,220,225)
                if animated then
                    TweenService:Create(btn, TweenInfo.new(0.16), {BackgroundColor3 = bg}):Play()
                    TweenService:Create(knob, TweenInfo.new(0.16), {Position = pos, BackgroundColor3 = kc}):Play()
                else
                    btn.BackgroundColor3 = bg
                    knob.Position = pos
                    knob.BackgroundColor3 = kc
                end
            end

            apply(false)
            btn.MouseButton1Click:Connect(function()
                toggled = not toggled
                apply(true)
                task.spawn(callback, toggled)
            end)

            return {Set = function(_, v) toggled = v apply(true) end, Get = function() return toggled end}
        end

        function section:AddKeybind(text, default, callback)
            default = default or Enum.KeyCode.Insert
            callback = callback or function() end

            local row = Instance.new("Frame", controls)
            row.BackgroundTransparency = 1
            row.Size = UDim2.new(1,0,0,22)

            local label = Instance.new("TextLabel", row)
            label.BackgroundTransparency = 1
            label.Size = UDim2.new(1,-80,1,0)
            label.Font = Enum.Font.GothamMedium
            label.Text = text
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.TextColor3 = Color3.fromRGB(205,205,210)

            local btn = Instance.new("TextButton", row)
            btn.AnchorPoint = Vector2.new(1,0.5)
            btn.Position = UDim2.new(1,0,0.5,0)
            btn.Size = UDim2.new(0,70,0,20)
            btn.BackgroundColor3 = Color3.fromRGB(30,32,36)
            btn.BorderSizePixel = 0
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 11
            btn.TextColor3 = Color3.fromRGB(200,200,210)
            btn.AutoButtonColor = false

            Instance.new("UICorner", btn).CornerRadius = UDim.new(0,4)

            local currentKey = default
            local listening = false

            local function updateText()
                if listening then
                    btn.Text = "..."
                    btn.TextColor3 = window._accent
                else
                    local keyName = currentKey.Name or "None"
                    btn.Text = "[" .. keyName .. "]"
                    btn.TextColor3 = Color3.fromRGB(200,200,210)
                end
            end

            updateText()

            btn.MouseButton1Click:Connect(function()
                listening = true
                updateText()
            end)

            UserInputService.InputBegan:Connect(function(input, gameProcessed)
                if not listening then return end
                if input.UserInputType == Enum.UserInputType.Keyboard then
                    listening = false
                    if input.KeyCode == Enum.KeyCode.Escape then
                        updateText()
                    else
                        currentKey = input.KeyCode
                        updateText()
                        task.spawn(callback, currentKey)
                    end
                end
            end)

            return {
                Get = function() return currentKey end, 
                Set = function(_, v) currentKey = v updateText() task.spawn(callback, currentKey) end
            }
        end

        function section:AddDropdown(text, options, defaultIndex, callback)
            options = options or {}
            defaultIndex = defaultIndex or 1
            callback = callback or function() end

            local row = Instance.new("Frame", controls)
            row.BackgroundTransparency = 1
            row.Size = UDim2.new(1,0,0,46)

            local label = Instance.new("TextLabel", row)
            label.BackgroundTransparency = 1
            label.Size = UDim2.new(1,0,0,18)
            label.Font = Enum.Font.GothamMedium
            label.Text = text
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.TextColor3 = Color3.fromRGB(205,205,210)

            local box = Instance.new("Frame", row)
            box.Position = UDim2.new(0,0,0,22)
            box.Size = UDim2.new(1,0,0,22)
            box.BackgroundColor3 = Color3.fromRGB(30,32,36)
            box.BorderSizePixel = 0
            box.ZIndex = 20
            Instance.new("UICorner", box).CornerRadius = UDim.new(0,4)

            local valueLabel = Instance.new("TextLabel", box)
            valueLabel.BackgroundTransparency = 1
            valueLabel.Position = UDim2.new(0,8,0,0)
            valueLabel.Size = UDim2.new(1,-26,1,0)
            valueLabel.Font = Enum.Font.GothamMedium
            valueLabel.TextXAlignment = Enum.TextXAlignment.Left
            valueLabel.TextSize = 13
            valueLabel.TextColor3 = Color3.fromRGB(200,200,210)
            valueLabel.ZIndex = 21

            local arrow = Instance.new("ImageLabel", box)
            arrow.AnchorPoint = Vector2.new(0.5,0.5)
            arrow.Position = UDim2.new(1,-10,0.5,0)
            arrow.Size = UDim2.new(0,10,0,10)
            arrow.BackgroundTransparency = 1
            arrow.Image = "rbxassetid://6031090990"
            arrow.ImageColor3 = Color3.fromRGB(160,160,170)
            arrow.ZIndex = 21

            local clickArea = Instance.new("TextButton", box)
            clickArea.BackgroundTransparency = 1
            clickArea.Size = UDim2.new(1,0,1,0)
            clickArea.Text = ""
            clickArea.ZIndex = 22

            local list = Instance.new("Frame", window._main)
            list.BackgroundColor3 = Color3.fromRGB(26,28,32)
            list.BorderSizePixel = 0
            list.Visible = false
            list.ZIndex = 100
            Instance.new("UICorner", list).CornerRadius = UDim.new(0,4)
            local listLayout = Instance.new("UIListLayout", list)
            listLayout.Padding = UDim.new(0,2)
            listLayout.SortOrder = Enum.SortOrder.LayoutOrder

            local current = options[defaultIndex] or ""
            valueLabel.Text = current
            local open = false

            for _, opt in ipairs(options) do
                local optBtn = Instance.new("TextButton", list)
                optBtn.BackgroundTransparency = 1
                optBtn.Size = UDim2.new(1,-8,0,20)
                optBtn.Position = UDim2.new(0,4,0,0)
                optBtn.Font = Enum.Font.GothamMedium
                optBtn.TextXAlignment = Enum.TextXAlignment.Left
                optBtn.TextSize = 13
                optBtn.TextColor3 = Color3.fromRGB(190,190,200)
                optBtn.Text = tostring(opt)
                optBtn.ZIndex = 101
                optBtn.MouseButton1Click:Connect(function()
                    current = opt
                    valueLabel.Text = tostring(opt)
                    open = false
                    TweenService:Create(list, TweenInfo.new(0.15), {Size = UDim2.new(0,list.Size.X.Offset,0,0)}):Play()
                    TweenService:Create(arrow, TweenInfo.new(0.15), {Rotation = 0}):Play()
                    task.delay(0.15, function() list.Visible = false end)
                    callback(current)
                end)
            end

            clickArea.MouseButton1Click:Connect(function()
                if open then
                    open = false
                    TweenService:Create(list, TweenInfo.new(0.15), {Size = UDim2.new(0,list.Size.X.Offset,0,0)}):Play()
                    TweenService:Create(arrow, TweenInfo.new(0.15), {Rotation = 0}):Play()
                    task.delay(0.15, function() list.Visible = false end)
                else
                    open = true
                    local mainAbs = window._main.AbsolutePosition
                    local boxAbs = box.AbsolutePosition
                    list.Position = UDim2.fromOffset(boxAbs.X - mainAbs.X, boxAbs.Y - mainAbs.Y + box.AbsoluteSize.Y + 8)
                    list.Size = UDim2.new(0,box.AbsoluteSize.X,0,0)
                    list.Visible = true
                    TweenService:Create(list, TweenInfo.new(0.15), {Size = UDim2.new(0,box.AbsoluteSize.X,0,math.min(4 + #options * 22, 140))}):Play()
                    TweenService:Create(arrow, TweenInfo.new(0.15), {Rotation = 180}):Play()
                end
            end)

            return {Get = function() return current end, Set = function(_, v) current = v valueLabel.Text = tostring(v) end}
        end

        function section:AddSlider(text, min, max, default, suffix, callback)
            min, max = min or 0, max or 100
            default = default or min
            suffix = suffix or ""
            callback = callback or function() end

            local row = Instance.new("Frame", controls)
            row.BackgroundTransparency = 1
            row.Size = UDim2.new(1,0,0,40)

            local label = Instance.new("TextLabel", row)
            label.BackgroundTransparency = 1
            label.Size = UDim2.new(1,0,0,18)
            label.Font = Enum.Font.GothamMedium
            label.Text = text
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.TextColor3 = Color3.fromRGB(205,205,210)

            local valueLabel = Instance.new("TextLabel", row)
            valueLabel.AnchorPoint = Vector2.new(1,0)
            valueLabel.Position = UDim2.new(1,0,0,0)
            valueLabel.Size = UDim2.new(0,80,0,18)
            valueLabel.BackgroundTransparency = 1
            valueLabel.Font = Enum.Font.GothamMedium
            valueLabel.TextSize = 13
            valueLabel.TextXAlignment = Enum.TextXAlignment.Right
            valueLabel.TextColor3 = Color3.fromRGB(140,140,150)

            local bar = Instance.new("Frame", row)
            bar.Position = UDim2.new(0,0,0,24)
            bar.Size = UDim2.new(1,0,0,6)
            bar.BackgroundColor3 = Color3.fromRGB(30,30,34)
            bar.BorderSizePixel = 0
            Instance.new("UICorner", bar).CornerRadius = UDim.new(0,3)

            local fill = Instance.new("Frame", bar)
            fill.Size = UDim2.new(0,0,1,0)
            fill.BackgroundColor3 = window._accent
            fill.BorderSizePixel = 0
            Instance.new("UICorner", fill).CornerRadius = UDim.new(0,3)

            local knob = Instance.new("Frame", bar)
            knob.AnchorPoint = Vector2.new(0.5,0.5)
            knob.Size = UDim2.new(0,10,0,10)
            knob.BackgroundColor3 = Color3.fromRGB(230,230,235)
            knob.BorderSizePixel = 0
            Instance.new("UICorner", knob).CornerRadius = UDim.new(1,0)

            local value = math.clamp(default, min, max)
            local dragging = false

            local function apply()
                local alpha = max == min and 0 or (value - min) / (max - min)
                valueLabel.Text = string.format("%d%s", math.floor(value + 0.5), suffix)
                TweenService:Create(fill, TweenInfo.new(0.12), {Size = UDim2.new(alpha,0,1,0)}):Play()
                TweenService:Create(knob, TweenInfo.new(0.12), {Position = UDim2.new(alpha,0,0.5,0)}):Play()
            end

            apply()
            task.spawn(callback, value)

            local function setFromX(x)
                value = min + (max - min) * math.clamp((x - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
                apply()
                task.spawn(callback, value)
            end

            bar.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = true setFromX(i.Position.X) end end)
            knob.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = true setFromX(i.Position.X) end end)
            UserInputService.InputChanged:Connect(function(i) if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then setFromX(i.Position.X) end end)
            UserInputService.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end end)

            return {Get = function() return value end, Set = function(_, v) value = math.clamp(v, min, max) apply() task.spawn(callback, value) end}
        end

        return section
    end

    button.MouseEnter:Connect(function()
        if window._selectedTab ~= tab then TweenService:Create(button, TweenInfo.new(0.12), {ImageColor3 = Color3.fromRGB(210,210,215)}):Play() end
    end)

    button.MouseLeave:Connect(function()
        if window._selectedTab ~= tab then TweenService:Create(button, TweenInfo.new(0.12), {ImageColor3 = Color3.fromRGB(180,180,185)}):Play() end
    end)

    button.MouseButton1Click:Connect(function() window:SelectTab(tab) end)

    table.insert(window._tabs, tab)
    
    if not window._selectedTab or tab._order < window._selectedTab._order then
        window:SelectTab(tab)
    end

    return tab
end

return VoidUI

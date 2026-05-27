local Update = {}

-- Theme setup (safe defaults)
local function applyTheme()
    local theme = _G.Theme
    if theme == 'Red' then
        _G.Primary = Color3.fromRGB(255, 30, 50)
        _G.Dark = Color3.fromRGB(90, 10, 20)
    elseif theme == 'Cyan' then
        _G.Primary = Color3.fromRGB(40, 230, 255)
        _G.Dark = Color3.fromRGB(10, 80, 115)
    elseif theme == 'Blue' then
        _G.Primary = Color3.fromRGB(40, 155, 255)
        _G.Dark = Color3.fromRGB(10, 80, 115)
    elseif theme == 'DarkBlue' then
        _G.Primary = Color3.fromRGB(50, 30, 255)
        _G.Dark = Color3.fromRGB(20, 10, 90)
    elseif theme == 'Green' then
        _G.Primary = Color3.fromRGB(70, 255, 205)
        _G.Dark = Color3.fromRGB(20, 90, 90)
    elseif theme == 'LightGreen' then
        _G.Primary = Color3.fromRGB(205, 255, 205)
        _G.Dark = Color3.fromRGB(70, 90, 70)
    elseif theme == 'Purple' then
        _G.Primary = Color3.fromRGB(205, 125, 255)
        _G.Dark = Color3.fromRGB(60, 20, 95)
    elseif theme == 'Zinc' then
        _G.Primary = Color3.fromRGB(30, 30, 30)
        _G.Dark = Color3.fromRGB(10, 10, 10)
    else
        _G.Primary = Color3.fromRGB(110, 110, 120)
        _G.Dark = Color3.fromRGB(20, 20, 30)
        if not _G.Theme then
            warn("Theme not found, using default")
        end
    end
end
applyTheme()

-- Services
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local TextService = game:GetService("TextService")

-- Small floating toggle icon (top-left)
local function createToggleIcon()
    if CoreGui:FindFirstChild("RELZHUB_TOGGLE") then
        CoreGui.RELZHUB_TOGGLE:Destroy()
    end

    local gui = Instance.new("ScreenGui")
    gui.Name = "RELZHUB_TOGGLE"
    gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    gui.Parent = CoreGui

    local btn = Instance.new("ImageButton")
    btn.Parent = gui
    btn.Position = UDim2.new(0, 10, 0, 10)
    btn.Size = UDim2.new(0, 50, 0, 50)
    btn.Draggable = true
    btn.BackgroundColor3 = _G.Dark
    btn.ImageColor3 = _G.Primary
    btn.ImageTransparency = 0.1
    btn.BackgroundTransparency = 0.1
    btn.Image = 'https://www.roblox.com/Thumbs/Asset.ashx?width=420&height=420&assetId=83731725578821'

    local stroke = Instance.new("UIStroke")
    stroke.Color = _G.Primary
    stroke.Thickness = 1
    stroke.Parent = btn

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 5)
    corner.Parent = btn

    btn.MouseButton1Click:Connect(function()
        local hub = CoreGui:FindFirstChild("RELZHUB")
        if hub and hub:IsA("ScreenGui") then
            hub.Enabled = not hub.Enabled
        end
    end)
end
createToggleIcon()

-- Draggable helper
local function makeDraggable(hitArea, target)
    local dragging = false
    local dragStart, startPos, lastInput

    local function update(input)
        local delta = input.Position - dragStart
        local newPos = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        TweenService:Create(target, TweenInfo.new(0.15), {Position = newPos}):Play()
    end

    hitArea.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = target.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    hitArea.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            lastInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == lastInput and dragging then
            update(input)
        end
    end)
end

-- Main factory
function Update:Window(patchText)
    -- cleanup any previous GUIs
    if CoreGui:FindFirstChild("RELZHUB") then CoreGui.RELZHUB:Destroy() end
    if CoreGui:FindFirstChild("RELZHUB_ALERT") then CoreGui.RELZHUB_ALERT:Destroy() end

    local alertGui = Instance.new("ScreenGui")
    alertGui.Name = "RELZHUB_ALERT"
    alertGui.Parent = CoreGui
    alertGui.ZIndexBehavior = Enum.ZIndexBehavior.Global

    local hubGui = Instance.new("ScreenGui")
    hubGui.Name = "RELZHUB"
    hubGui.Parent = CoreGui
    hubGui.ZIndexBehavior = Enum.ZIndexBehavior.Global

    -- main window
    local main = Instance.new("Frame")
    main.Name = "Main"
    main.Parent = hubGui
    main.AnchorPoint = Vector2.new(0.5, 0.5)
    main.ClipsDescendants = true
    main.BackgroundColor3 = _G.Dark
    main.BackgroundTransparency = 0.1
    main.Position = UDim2.new(0.5, 0, 0.5, 0)
    main.Size = UDim2.new(0, 0, 0, 0)
    main:TweenSize(UDim2.new(0, 524, 0, 332), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.4, true)

    local mainStroke = Instance.new("UIStroke")
    mainStroke.Color = _G.Primary
    mainStroke.Thickness = 1
    mainStroke.Parent = main

    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 5)
    mainCorner.Parent = main

    -- resize grip
    local resizeGrip = Instance.new("Frame")
    resizeGrip.Name = "DragButton"
    resizeGrip.Parent = main
    resizeGrip.Position = UDim2.new(1, 5, 1, 5)
    resizeGrip.AnchorPoint = Vector2.new(1, 1)
    resizeGrip.Size = UDim2.new(0, 15, 0, 15)
    resizeGrip.BackgroundColor3 = _G.Primary
    resizeGrip.BackgroundTransparency = 0.1
    resizeGrip.ZIndex = 10
    local gripCorner = Instance.new("UICorner")
    gripCorner.CornerRadius = UDim.new(0, 99)
    gripCorner.Parent = resizeGrip

    -- top bar & title
    local topBar = Instance.new("Frame")
    topBar.Name = "Top"
    topBar.Parent = main
    topBar.BackgroundTransparency = 1
    topBar.Size = UDim2.new(1, 0, 0, 40)
    local topCorner = Instance.new("UICorner")
    topCorner.CornerRadius = UDim.new(0, 5)
    topCorner.Parent = topBar

    local title = Instance.new("TextLabel")
    title.Name = "ttittles"
    title.Parent = topBar
    title.BackgroundTransparency = 1
    title.Position = UDim2.new(0, 15, 0.5, 0)
    title.AnchorPoint = Vector2.new(0, 0.5)
    title.Font = Enum.Font.GothamBold
    title.Text = "LUNORA HUB |"
    title.TextSize = 15
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.TextXAlignment = Enum.TextXAlignment.Left
    local titleSize = TextService:GetTextSize(title.Text, title.TextSize, title.Font, Vector2.new(1e6, 1e6))
    title.Size = UDim2.new(0, titleSize.X, 0, 25)

    local patchLabel = Instance.new("TextLabel")
    patchLabel.Name = "patch"
    patchLabel.Parent = title
    patchLabel.BackgroundTransparency = 1
    patchLabel.Position = UDim2.new(1, 5, 0.5, 0)
    patchLabel.AnchorPoint = Vector2.new(0, 0.5)
    patchLabel.Font = Enum.Font.Gotham
    patchLabel.Text = tostring(patchText or "")
    patchLabel.TextSize = 15
    patchLabel.TextColor3 = _G.Primary
    local patchSize = TextService:GetTextSize(patchLabel.Text, patchLabel.TextSize, patchLabel.Font, Vector2.new(1e6, 1e6))
    patchLabel.Size = UDim2.new(0, patchSize.X, 0, 25)

    local hideBtn = Instance.new("ImageButton")
    hideBtn.Name = "Hide"
    hideBtn.Parent = topBar
    hideBtn.AnchorPoint = Vector2.new(1, 0.5)
    hideBtn.Position = UDim2.new(1, -10, 0.5, 0)
    hideBtn.Size = UDim2.new(0, 25, 0, 25)
    hideBtn.BackgroundTransparency = 1
    hideBtn.Image = 'https://www.roblox.com/Thumbs/Asset.ashx?width=420&height=420&assetId=83731725578821'
    hideBtn.ImageColor3 = Color3.fromRGB(245, 245, 245)
    local hideCorner = Instance.new("UICorner")
    hideCorner.CornerRadius = UDim.new(0, 3)
    hideCorner.Parent = hideBtn
    hideBtn.MouseButton1Click:Connect(function() hubGui.Enabled = not hubGui.Enabled end)

    -- Transparency toggle button (turn off transparency)
    local transparencyOff = false
    local savedTransparencies = {}

    local function setTransparencyOff(off)
        if off then
            -- Save current transparencies and set to opaque (0)
            savedTransparencies = {}
            for _, inst in pairs(hubGui:GetDescendants()) do
                if inst:IsA("GuiObject") then
                    -- save/restore via keys; use pcall to avoid errors for unexpected objects
                    local entry = {}
                    pcall(function() entry.BackgroundTransparency = inst.BackgroundTransparency end)
                    pcall(function() entry.TextTransparency = inst.TextTransparency end)
                    pcall(function() entry.ImageTransparency = inst.ImageTransparency end)
                    savedTransparencies[inst] = entry
                    pcall(function() inst.BackgroundTransparency = 0 end)
                    pcall(function() inst.TextTransparency = 0 end)
                    pcall(function() inst.ImageTransparency = 0 end)
                end
            end
        else
            -- Restore
            for inst, vals in pairs(savedTransparencies) do
                if inst and inst.Parent then
                    pcall(function()
                        if vals.BackgroundTransparency ~= nil then inst.BackgroundTransparency = vals.BackgroundTransparency end
                        if vals.TextTransparency ~= nil then inst.TextTransparency = vals.TextTransparency end
                        if vals.ImageTransparency ~= nil then inst.ImageTransparency = vals.ImageTransparency end
                    end)
                end
            end
            savedTransparencies = {}
        end
    end

    local transparencyBtn = Instance.new("TextButton")
    transparencyBtn.Name = "TransparencyToggle"
    transparencyBtn.Parent = topBar
    transparencyBtn.AnchorPoint = Vector2.new(1, 0.5)
    transparencyBtn.Position = UDim2.new(1, -42, 0.5, 0) -- left of hideBtn
    transparencyBtn.Size = UDim2.new(0, 30, 0, 22)
    transparencyBtn.BackgroundTransparency = 0.2
    transparencyBtn.BackgroundColor3 = _G.Primary
    transparencyBtn.Font = Enum.Font.GothamSemibold
    transparencyBtn.TextSize = 10
    transparencyBtn.Text = "OPA"
    transparencyBtn.TextColor3 = Color3.fromRGB(245,245,245)
    local tcorner = Instance.new("UICorner"); tcorner.CornerRadius = UDim.new(0,3); tcorner.Parent = transparencyBtn

    transparencyBtn.MouseButton1Click:Connect(function()
        transparencyOff = not transparencyOff
        if transparencyOff then
            transparencyBtn.Text = "OPN"
            setTransparencyOff(true)
        else
            transparencyBtn.Text = "OPA"
            setTransparencyOff(false)
        end
    end)

    -- left tabs
    local leftTabFrame = Instance.new("Frame")
    leftTabFrame.Name = "Tab"
    leftTabFrame.Parent = main
    leftTabFrame.BackgroundTransparency = 1
    leftTabFrame.Position = UDim2.new(0, 8, 0, 45)
    leftTabFrame.Size = UDim2.new(0, 148, 0, 275)

    local leftScroll = Instance.new("ScrollingFrame")
    leftScroll.Name = "ScrollTab"
    leftScroll.Parent = leftTabFrame
    leftScroll.Active = true
    leftScroll.BackgroundTransparency = 1
    leftScroll.Size = UDim2.new(1, 0, 1, 0)
    leftScroll.ScrollBarThickness = 0

    local leftCorner = Instance.new("UICorner")
    leftCorner.Parent = leftTabFrame
    leftCorner.CornerRadius = UDim.new(0, 5)

    local leftList = Instance.new("UIListLayout")
    leftList.Parent = leftScroll
    leftList.SortOrder = Enum.SortOrder.LayoutOrder
    leftList.Padding = UDim.new(0, 2)

    local leftPadding = Instance.new("UIPadding")
    leftPadding.Parent = leftScroll

    -- right pages
    local rightPageFrame = Instance.new("Frame")
    rightPageFrame.Name = "Page"
    rightPageFrame.Parent = main
    rightPageFrame.BackgroundTransparency = 1
    rightPageFrame.Position = UDim2.new(0, 166, 0, 45)
    rightPageFrame.Size = UDim2.new(0, 350, 0, 275)

    local rightCorner = Instance.new("UICorner")
    rightCorner.Parent = rightPageFrame
    rightCorner.CornerRadius = UDim.new(0, 3)

    local mainPage = Instance.new("Frame")
    mainPage.Name = "MainPage"
    mainPage.Parent = rightPageFrame
    mainPage.BackgroundTransparency = 1
    mainPage.ClipsDescendants = true
    mainPage.Size = UDim2.new(1, 0, 1, 0)

    local pageFolder = Instance.new("Folder")
    pageFolder.Name = "PageList"
    pageFolder.Parent = mainPage

    local uiPageLayout = Instance.new("UIPageLayout")
    uiPageLayout.Parent = pageFolder
    uiPageLayout.SortOrder = Enum.SortOrder.LayoutOrder
    uiPageLayout.EasingDirection = Enum.EasingDirection.InOut
    uiPageLayout.EasingStyle = Enum.EasingStyle.Quad
    uiPageLayout.FillDirection = Enum.FillDirection.Vertical
    uiPageLayout.Padding = UDim.new(0, 10)
    uiPageLayout.TweenTime = 0
    uiPageLayout.GamepadInputEnabled = false
    uiPageLayout.ScrollWheelInputEnabled = false
    uiPageLayout.TouchInputEnabled = false

    -- make draggable
    makeDraggable(topBar, main)

    -- Toggle visibility with Insert
    UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.Insert then
            hubGui.Enabled = not hubGui.Enabled
        end
    end)

    -- resize logic
    local resizing = false
    resizeGrip.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            resizing = true
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            resizing = false
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if resizing and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            main.Size = UDim2.new(0, math.clamp(input.Position.X - main.AbsolutePosition.X, 524, math.huge), 0, math.clamp(input.Position.Y - main.AbsolutePosition.Y, 332, math.huge))
            rightPageFrame.Size = UDim2.new(0, math.clamp(input.Position.X - rightPageFrame.AbsolutePosition.X - 8, 350, math.huge), 0, math.clamp(input.Position.Y - rightPageFrame.AbsolutePosition.Y - 8, 270, math.huge))
            leftTabFrame.Size = UDim2.new(0, 148, 0, math.clamp(input.Position.Y - leftTabFrame.AbsolutePosition.Y - 8, 270, math.huge))
        end
    end)

    -- keep canvas sizes updated
    RunService.Stepped:Connect(function()
        pcall(function()
            for _, p in pairs(pageFolder:GetChildren()) do
                if p:IsA("ScrollingFrame") then
                    local layout = p:FindFirstChildOfClass("UIListLayout")
                    if layout then
                        p.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
                    end
                end
            end
            leftScroll.CanvasSize = UDim2.new(0, 0, 0, leftList.AbsoluteContentSize.Y)
        end)
    end)

    -- returned library
    local library = {}

    -- Alert helper
    function library:Alert(message)
        if CoreGui:FindFirstChild("RELZHUB_ALERT") then
            local af = CoreGui.RELZHUB_ALERT
            if af:FindFirstChild("Frame") then af.Frame:Destroy() end

            local frame = Instance.new("Frame")
            frame.Name = "Frame"
            frame.Parent = af
            frame.BackgroundColor3 = _G.Dark
            frame.BackgroundTransparency = 0.1
            frame.Position = UDim2.new(1, 0, 0, 0)
            frame.Size = UDim2.new(0, 200, 0, 60)

            local stroke = Instance.new("UIStroke")
            stroke.Color = _G.Primary
            stroke.Thickness = 1
            stroke.Parent = frame

            local icon = Instance.new("ImageLabel")
            icon.Name = "Icon"
            icon.Parent = frame
            icon.BackgroundTransparency = 1
            icon.Position = UDim2.new(0, 8, 0, 8)
            icon.Size = UDim2.new(0, 45, 0, 45)
            icon.Image = 'rbxassetid://13940080072'

            local title = Instance.new("TextLabel")
            title.Parent = frame
            title.BackgroundTransparency = 1
            title.Position = UDim2.new(0, 55, 0, 14)
            title.Size = UDim2.new(0, 10, 0, 20)
            title.Font = Enum.Font.GothamBold
            title.Text = 'Relz Hub'
            title.TextColor3 = Color3.fromRGB(255, 255, 255)
            title.TextSize = 16
            title.TextXAlignment = Enum.TextXAlignment.Left

            local content = Instance.new("TextLabel")
            content.Parent = frame
            content.BackgroundTransparency = 1
            content.Position = UDim2.new(0, 55, 0, 33)
            content.Size = UDim2.new(0, 10, 0, 10)
            content.Font = Enum.Font.GothamSemibold
            content.TextTransparency = 0.3
            content.Text = message
            content.TextColor3 = Color3.fromRGB(200, 200, 200)
            content.TextSize = 12
            content.TextXAlignment = Enum.TextXAlignment.Left

            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 5)
            corner.Parent = frame

            frame:TweenPosition(UDim2.new(1, -195, 0, 0), "Out", "Quad", 0.4, true)
            delay(2, function()
                frame:TweenPosition(UDim2.new(1, 0, 0, 0), "Out", "Quad", 0.5, true)
                wait(0.6)
                pcall(function() frame:Destroy() end)
            end)
        end
    end

    -- Tab factory
    function library:Tab(tabName, tabIcon)
        local tabButton = Instance.new("TextButton")
        tabButton.Parent = leftScroll
        tabButton.Name = tabName .. "Server"
        tabButton.Text = ""
        tabButton.BackgroundTransparency = 1
        tabButton.Size = UDim2.new(1, 0, 0, 35)
        tabButton.Font = Enum.Font.GothamSemibold
        tabButton.TextSize = 12
        tabButton.TextColor3 = Color3.fromRGB(255, 255, 255)

        local selectedBar = Instance.new("Frame")
        selectedBar.Name = "SelectedTab"
        selectedBar.Parent = tabButton
        selectedBar.BackgroundColor3 = _G.Primary
        selectedBar.Size = UDim2.new(0, 3, 0, 0)
        selectedBar.Position = UDim2.new(0, 0, 0.5, 0)
        selectedBar.AnchorPoint = Vector2.new(0, 0.5)
        local selCorner = Instance.new("UICorner")
        selCorner.CornerRadius = UDim.new(0, 100)
        selCorner.Parent = selectedBar

        local titleLabel = Instance.new("TextLabel")
        titleLabel.Parent = tabButton
        titleLabel.Name = "Title"
        titleLabel.BackgroundTransparency = 1
        titleLabel.Position = UDim2.new(0, 30, 0.5, 0)
        titleLabel.Size = UDim2.new(0, 100, 0, 30)
        titleLabel.Font = Enum.Font.GothamSemibold
        titleLabel.Text = tabName
        titleLabel.AnchorPoint = Vector2.new(0, 0.5)
        titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        titleLabel.TextTransparency = 0.4
        titleLabel.TextSize = 13
        titleLabel.TextXAlignment = Enum.TextXAlignment.Left

        local iconLabel = Instance.new("ImageLabel")
        iconLabel.Name = "IDK"
        iconLabel.Parent = tabButton
        iconLabel.BackgroundTransparency = 1
        iconLabel.ImageTransparency = 0.3
        iconLabel.Position = UDim2.new(0, 7, 0.5, 0)
        iconLabel.Size = UDim2.new(0, 15, 0, 15)
        iconLabel.AnchorPoint = Vector2.new(0, 0.5)
        iconLabel.Image = tabIcon or ""

        local page = Instance.new("ScrollingFrame")
        page.Parent = pageFolder
        page.Name = tabName .. "_Page"
        page.Active = true
        page.BackgroundTransparency = 1
        page.Size = UDim2.new(1, 0, 1, 0)
        page.ScrollBarThickness = 0

        local pagePadding = Instance.new("UIPadding")
        pagePadding.Parent = page

        local pageListLayout = Instance.new("UIListLayout")
        pageListLayout.Padding = UDim.new(0, 3)
        pageListLayout.Parent = page
        pageListLayout.SortOrder = Enum.SortOrder.LayoutOrder

        tabButton.MouseButton1Click:Connect(function()
            -- reset visuals on other tabs
            for _, child in pairs(leftScroll:GetChildren()) do
                if child:IsA("TextButton") then
                    TweenService:Create(child, TweenInfo.new(0.3), {BackgroundTransparency = 1}):Play()
                    local sel = child:FindFirstChild("SelectedTab")
                    if sel then TweenService:Create(sel, TweenInfo.new(0.2), {Size = UDim2.new(0, 3, 0, 0)}):Play() end
                    local iconc = child:FindFirstChild("IDK")
                    if iconc then TweenService:Create(iconc, TweenInfo.new(0.3), {ImageTransparency = 0.4}):Play() end
                    local ttl = child:FindFirstChild("Title")
                    if ttl then TweenService:Create(ttl, TweenInfo.new(0.3), {TextTransparency = 0.4}):Play() end
                end
            end

            -- highlight this tab
            TweenService:Create(tabButton, TweenInfo.new(0.3), {BackgroundTransparency = 0.8}):Play()
            TweenService:Create(selectedBar, TweenInfo.new(0.3), {Size = UDim2.new(0, 3, 0, 15)}):Play()
            TweenService:Create(iconLabel, TweenInfo.new(0.3), {ImageTransparency = 0}):Play()
            TweenService:Create(titleLabel, TweenInfo.new(0.3), {TextTransparency = 0}):Play()

            local pl = pageFolder:FindFirstChildOfClass("UIPageLayout")
            if pl then pl:JumpTo(page) end
        end)

        -- select first tab by default if none
        if #pageFolder:GetChildren() == 0 then
            TweenService:Create(tabButton, TweenInfo.new(0.3), {BackgroundTransparency = 0.8}):Play()
            TweenService:Create(selectedBar, TweenInfo.new(0.2), {Size = UDim2.new(0, 3, 0, 15)}):Play()
            TweenService:Create(iconLabel, TweenInfo.new(0.3), {ImageTransparency = 0}):Play()
            TweenService:Create(titleLabel, TweenInfo.new(0.3), {TextTransparency = 0}):Play()
            delay(0.1, function()
                local pl = pageFolder:FindFirstChildOfClass("UIPageLayout")
                if pl then pl:JumpToIndex(1) end
            end)
        end

        -- Page API
        local pageApi = {}

        -- Button (styled icon inside clickable area)
        function pageApi.Button(_, text, callback)
            local frame = Instance.new("Frame")
            frame.Name = "Button"
            frame.Parent = page
            frame.BackgroundColor3 = _G.Primary
            frame.BackgroundTransparency = 0.75
            frame.Size = UDim2.new(1, 0, 0, 36)
            local corner = Instance.new("UICorner"); corner.CornerRadius = UDim.new(0, 8); corner.Parent = frame

            local label = Instance.new("TextLabel")
            label.Name = "TextLabel"
            label.Parent = frame
            label.BackgroundTransparency = 1
            label.AnchorPoint = Vector2.new(0, 0.5)
            label.Position = UDim2.new(0, 15, 0.5, 0)
            label.Size = UDim2.new(1, -40, 1, 0)
            label.Font = Enum.Font.GothamSemibold
            label.Text = text
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.TextColor3 = Color3.fromRGB(245, 245, 245)
            label.TextSize = 13

            local btn = Instance.new("TextButton")
            btn.Name = "TextButton"
            btn.Parent = frame
            btn.BackgroundColor3 = _G.Dark
            btn.BackgroundTransparency = 0
            btn.AnchorPoint = Vector2.new(1, 0.5)
            btn.Position = UDim2.new(1, -10, 0.5, 0)
            btn.Size = UDim2.new(0, 22, 0, 22)
            btn.AutoButtonColor = true

            local btnCorner = Instance.new("UICorner"); btnCorner.CornerRadius = UDim.new(0, 6); btnCorner.Parent = btn

            local img = Instance.new("ImageLabel")
            img.Name = "ImageLabel"
            img.Parent = btn
            img.BackgroundTransparency = 1
            img.AnchorPoint = Vector2.new(0.5, 0.5)
            img.Position = UDim2.new(0.5, 0, 0.5, 0)
            img.Size = UDim2.new(0, 15, 0, 15)
            img.Image = "rbxassetid://10723375250"
            img.ImageTransparency = 0.05
            img.ImageColor3 = Color3.fromRGB(245, 245, 245)

            btn.MouseButton1Click:Connect(function()
                pcall(callback)
            end)
        end

        -- Toggle (rounded knob, stroke)
        function pageApi.Toggle(_, text, defaultValue, description, callback)
            local state = defaultValue and true or false

            local container = Instance.new("TextButton")
            container.Name = "Button"
            container.Parent = page
            container.BackgroundColor3 = _G.Primary
            container.BackgroundTransparency = 0.8
            container.Size = UDim2.new(1, 0, 0, 46)
            container.AutoButtonColor = false
            local contCorner = Instance.new("UICorner"); contCorner.CornerRadius = UDim.new(0, 8); contCorner.Parent = container

            local titleLbl = Instance.new("TextLabel")
            titleLbl.Parent = container
            titleLbl.BackgroundTransparency = 1
            titleLbl.Size = UDim2.new(1, 0, 0, 35)
            titleLbl.Font = Enum.Font.GothamSemibold
            titleLbl.Text = text
            titleLbl.TextColor3 = Color3.fromRGB(245, 245, 245)
            titleLbl.TextSize = 13
            titleLbl.TextXAlignment = Enum.TextXAlignment.Left
            titleLbl.AnchorPoint = Vector2.new(0, 0.5)
            titleLbl.Position = UDim2.new(0, 15, 0.5, 0)

            local desc = Instance.new("TextLabel")
            desc.Parent = titleLbl
            desc.BackgroundTransparency = 1
            desc.Position = UDim2.new(0, 0, 0, 22)
            desc.Size = UDim2.new(0, 280, 0, 16)
            desc.Font = Enum.Font.Gotham
            desc.TextColor3 = Color3.fromRGB(200, 200, 200)
            desc.TextSize = 10
            if description and description ~= "" then
                desc.Text = description
                titleLbl.Position = UDim2.new(0, 15, 0.5, -5)
                desc.Visible = true
            else
                desc.Visible = false
            end

            local toggleFrame = Instance.new("Frame")
            toggleFrame.Name = "ToggleFrame"
            toggleFrame.Parent = container
            toggleFrame.BackgroundColor3 = _G.Dark
            toggleFrame.BackgroundTransparency = 0
            toggleFrame.Position = UDim2.new(1, -10, 0.5, 0)
            toggleFrame.Size = UDim2.new(0, 40, 0, 22)
            toggleFrame.AnchorPoint = Vector2.new(1, 0.5)
            local tfCorner = Instance.new("UICorner"); tfCorner.Parent = toggleFrame; tfCorner.CornerRadius = UDim.new(0, 10)

            local outerStroke = Instance.new("UIStroke")
            outerStroke.Parent = toggleFrame
            outerStroke.Color = _G.Primary
            outerStroke.Thickness = 1

            local toggleBtn = Instance.new("TextButton")
            toggleBtn.Name = "ToggleImage"
            toggleBtn.Parent = toggleFrame
            toggleBtn.BackgroundColor3 = _G.Dark
            toggleBtn.BackgroundTransparency = 0
            toggleBtn.Size = UDim2.new(1, 0, 1, 0)
            toggleBtn.Text = ""
            toggleBtn.AutoButtonColor = false
            local tbtnCorner = Instance.new("UICorner"); tbtnCorner.Parent = toggleBtn; tbtnCorner.CornerRadius = UDim.new(0, 10)

            local circle = Instance.new("Frame")
            circle.Name = "Circle"
            circle.Parent = toggleBtn
            circle.BackgroundColor3 = _G.Primary
            circle.Size = UDim2.new(0, 16, 0, 16)
            circle.Position = UDim2.new(0, 4, 0.5, 0)
            circle.AnchorPoint = Vector2.new(0, 0.5)
            local ccorner = Instance.new("UICorner"); ccorner.Parent = circle; ccorner.CornerRadius = UDim.new(0, 16)

            local function setVisual(on)
                if on then
                    outerStroke.Thickness = 0
                    circle:TweenPosition(UDim2.new(0, 20, 0.5, 0), "Out", "Sine", 0.35, true)
                    TweenService:Create(circle, TweenInfo.new(0.35), {BackgroundColor3 = _G.Dark}):Play()
                    TweenService:Create(toggleBtn, TweenInfo.new(0.35), {BackgroundColor3 = _G.Primary}):Play()
                else
                    outerStroke.Thickness = 1
                    circle:TweenPosition(UDim2.new(0, 4, 0.5, 0), "Out", "Sine", 0.25, true)
                    TweenService:Create(circle, TweenInfo.new(0.35), {BackgroundColor3 = _G.Primary}):Play()
                    TweenService:Create(toggleBtn, TweenInfo.new(0.35), {BackgroundColor3 = _G.Dark}):Play()
                end
            end

            toggleBtn.MouseButton1Click:Connect(function()
                state = not state
                setVisual(state)
                pcall(callback, state)
            end)

            -- initialize
            setVisual(state)
            pcall(function() if callback then callback(state) end end)
        end

        -- Dropdown (in-flow expansion: opening expands the dropdown's own frame so UIListLayout will reflow)
        function pageApi.Dropdown(_, titleText, items, defaultText, onSelect)
            local isOpen = false
            items = items or {}
            local selected = defaultText or "Select Items"
            local collapsedHeight = 40

            local frame = Instance.new("Frame")
            frame.Parent = page
            frame.Name = "Dropdown"
            frame.BackgroundColor3 = _G.Primary
            frame.BackgroundTransparency = 0.8
            frame.Size = UDim2.new(1, 0, 0, collapsedHeight) -- will expand when opened
            local frameCorner = Instance.new("UICorner"); frameCorner.Parent = frame; frameCorner.CornerRadius = UDim.new(0, 8)

            local label = Instance.new("TextLabel")
            label.Parent = frame
            label.Name = "DropTitle"
            label.BackgroundTransparency = 1
            label.Size = UDim2.new(1, 0, 0, 30)
            label.Font = Enum.Font.GothamSemibold
            label.Text = titleText or ""
            label.TextColor3 = Color3.fromRGB(245, 245, 245)
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Position = UDim2.new(0, 15, 0, 5)

            local selectBtn = Instance.new("TextButton")
            selectBtn.Parent = frame
            selectBtn.Name = "SelectItems"
            selectBtn.BackgroundColor3 = _G.Dark
            selectBtn.BackgroundTransparency = 0.1
            selectBtn.Position = UDim2.new(1, -5, 0, 5)
            selectBtn.Size = UDim2.new(0, 100, 0, 30)
            selectBtn.AnchorPoint = Vector2.new(1, 0)
            selectBtn.Font = Enum.Font.GothamMedium
            selectBtn.TextSize = 9
            selectBtn.ZIndex = 2
            selectBtn.ClipsDescendants = true
            selectBtn.TextXAlignment = Enum.TextXAlignment.Left
            selectBtn.Text = "   " .. tostring(selected)
            local selectCorner = Instance.new("UICorner"); selectCorner.Parent = selectBtn; selectCorner.CornerRadius = UDim.new(0, 6)

            -- list container inside the same frame; when opened we will expand `frame` height so it pushes subsequent elements down
            local listFrame = Instance.new("Frame")
            listFrame.Name = "DropdownFrameScroll"
            listFrame.Parent = frame
            listFrame.BackgroundColor3 = _G.Dark
            listFrame.BackgroundTransparency = 0
            listFrame.Size = UDim2.new(1, -10, 0, 0)
            listFrame.Position = UDim2.new(0, 5, 0, collapsedHeight)
            listFrame.AnchorPoint = Vector2.new(0, 0)
            listFrame.ClipsDescendants = false
            listFrame.Visible = false
            local lfCorner = Instance.new("UICorner"); lfCorner.Parent = listFrame; lfCorner.CornerRadius = UDim.new(0, 6)

            local listScroll = Instance.new("ScrollingFrame")
            listScroll.Name = "DropScroll"
            listScroll.Parent = listFrame
            listScroll.Active = true
            listScroll.BackgroundTransparency = 1
            listScroll.BorderSizePixel = 0
            listScroll.Position = UDim2.new(0, 0, 0, 5)
            listScroll.Size = UDim2.new(1, 0, 0, 80)
            listScroll.ClipsDescendants = true
            listScroll.ScrollBarThickness = 3
            listScroll.ZIndex = 3

            local padding = Instance.new("UIPadding"); padding.Parent = listScroll; padding.PaddingLeft = UDim.new(0, 10); padding.PaddingRight = UDim.new(0, 10)
            local listLayout = Instance.new("UIListLayout"); listLayout.Parent = listScroll; listLayout.SortOrder = Enum.SortOrder.LayoutOrder; listLayout.Padding = UDim.new(0, 1)

            local function createItemButton(itemValue)
                local itemBtn = Instance.new("TextButton")
                itemBtn.Name = "Item"
                itemBtn.Parent = listScroll
                itemBtn.BackgroundTransparency = 1
                itemBtn.Size = UDim2.new(1, 0, 0, 30)
                itemBtn.Font = Enum.Font.GothamSemibold
                itemBtn.Text = tostring(itemValue)
                itemBtn.TextColor3 = Color3.fromRGB(245, 245, 245)
                itemBtn.TextSize = 11
                itemBtn.TextTransparency = 0.5
                itemBtn.TextXAlignment = Enum.TextXAlignment.Left
                itemBtn.ZIndex = 4

                local itemPad = Instance.new("UIPadding"); itemPad.Parent = itemBtn; itemPad.PaddingLeft = UDim.new(0, 8)
                local itemCorner = Instance.new("UICorner"); itemCorner.Parent = itemBtn; itemCorner.CornerRadius = UDim.new(0, 6)

                local selIndicator = Instance.new("Frame")
                selIndicator.Name = "SelectedItems"
                selIndicator.Parent = itemBtn
                selIndicator.BackgroundColor3 = _G.Primary
                selIndicator.BackgroundTransparency = 1
                selIndicator.Size = UDim2.new(0, 3, 0.4, 0)
                selIndicator.Position = UDim2.new(0, -8, 0.5, 0)
                selIndicator.AnchorPoint = Vector2.new(0, 0.5)
                selIndicator.ZIndex = 4
                local selCorner = Instance.new("UICorner"); selCorner.Parent = selIndicator; selCorner.CornerRadius = UDim.new(0, 6)

                itemBtn.MouseEnter:Connect(function()
                    TweenService:Create(itemBtn, TweenInfo.new(0.12), {TextTransparency = 0}):Play()
                    TweenService:Create(itemBtn, TweenInfo.new(0.12), {BackgroundTransparency = 0.85}):Play()
                    TweenService:Create(selIndicator, TweenInfo.new(0.12), {BackgroundTransparency = 0}):Play()
                end)
                itemBtn.MouseLeave:Connect(function()
                    TweenService:Create(itemBtn, TweenInfo.new(0.12), {TextTransparency = 0.2}):Play()
                    TweenService:Create(itemBtn, TweenInfo.new(0.12), {BackgroundTransparency = 1}):Play()
                    TweenService:Create(selIndicator, TweenInfo.new(0.12), {BackgroundTransparency = 1}):Play()
                end)
                itemBtn.MouseButton1Click:Connect(function()
                    selected = itemBtn.Text
                    selectBtn.Text = "   " .. itemBtn.Text
                    isOpen = false

                    -- collapse: animate frame height back to collapsedHeight, hide list
                    local targetFrameSize = UDim2.new(1, 0, 0, collapsedHeight)
                    TweenService:Create(listFrame, TweenInfo.new(0.18), {Size = UDim2.new(1, -10, 0, 0)}):Play()
                    TweenService:Create(frame, TweenInfo.new(0.18), {Size = targetFrameSize}):Play()
                    delay(0.18, function()
                        if listFrame and listFrame.Parent then listFrame.Visible = false end
                    end)

                    pcall(function() listScroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y) end)
                    if type(onSelect) == "function" then
                        pcall(onSelect, itemBtn.Text)
                    end
                end)

                return itemBtn
            end

            -- populate initial items
            if type(items) == "table" then
                for _, v in ipairs(items) do
                    createItemButton(v)
                end
            end

            -- ensure canvas size reflects content
            RunService.Heartbeat:Wait()
            pcall(function() listScroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y) end)

            selectBtn.MouseButton1Click:Connect(function()
                if isOpen then
                    isOpen = false
                    -- collapse: animate frame to collapsedHeight so it pushes layout back
                    local targetFrameSize = UDim2.new(1, 0, 0, collapsedHeight)
                    TweenService:Create(listFrame, TweenInfo.new(0.18), {Size = UDim2.new(1, -10, 0, 0)}):Play()
                    TweenService:Create(frame, TweenInfo.new(0.18), {Size = targetFrameSize}):Play()
                    delay(0.18, function() if listFrame and listFrame.Parent then listFrame.Visible = false end end)
                else
                    isOpen = true
                    listFrame.Visible = true

                    -- compute target height based on content
                    RunService.Heartbeat:Wait()
                    local contentH = listLayout.AbsoluteContentSize.Y
                    local targetListH = math.clamp(contentH + 10, 30, 200) -- inner scroll visible height
                    local targetFrameH = collapsedHeight + targetListH -- full frame height including title area
                    -- set listScroll size to accommodate
                    listScroll.Size = UDim2.new(1, 0, 0, targetListH)

                    -- animate expansion of the entire frame so other elements are pushed down
                    TweenService:Create(frame, TweenInfo.new(0.22, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(1, 0, 0, targetFrameH)}):Play()
                    TweenService:Create(listFrame, TweenInfo.new(0.22, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(1, -10, 0, targetListH)}):Play()

                    -- after a short delay ensure canvas size is correct
                    delay(0.06, function()
                        pcall(function() listScroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y) end)
                    end)
                end
            end)

            -- return helpers
            return {
                Root = frame,
                Add = function(_, newItem)
                    createItemButton(newItem)
                    RunService.Heartbeat:Wait()
                    pcall(function() listScroll.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y) end)
                end,
                Clear = function(_)
                    for _, c in ipairs(listScroll:GetChildren()) do
                        if c:IsA("TextButton") and c.Name == "Item" then c:Destroy() end
                    end
                    selectBtn.Text = "   Select Items"
                    isOpen = false
                    listFrame.Visible = false
                    pcall(function() listScroll.CanvasSize = UDim2.new(0, 0, 0, 0) end)
                    -- collapse parent frame
                    TweenService:Create(frame, TweenInfo.new(0.15), {Size = UDim2.new(1, 0, 0, collapsedHeight)}):Play()
                end,
            }
        end

        -- Slider (styled)
        function pageApi.Slider(_, labelText, minVal, maxVal, defaultVal, onChange)
            local frame = Instance.new("Frame")
            frame.Parent = page
            frame.Name = "Slider"
            frame.Size = UDim2.new(1, 0, 0, 45)
            frame.BackgroundTransparency = 1

            local corner = Instance.new("UICorner"); corner.Parent = frame; corner.CornerRadius = UDim.new(0, 8)

            local sliderBg = Instance.new("Frame")
            sliderBg.Parent = frame
            sliderBg.Name = "sliderr"
            sliderBg.Size = UDim2.new(1, 0, 0, 45)
            sliderBg.BackgroundColor3 = _G.Primary
            sliderBg.BackgroundTransparency = 0.8
            local sbCorner = Instance.new("UICorner"); sbCorner.Parent = sliderBg; sbCorner.CornerRadius = UDim.new(0, 8)

            local label = Instance.new("TextLabel")
            label.Parent = sliderBg
            label.BackgroundTransparency = 1
            label.Position = UDim2.new(0, 15, 0.5, 0)
            label.AnchorPoint = Vector2.new(0, 0.5)
            label.Size = UDim2.new(1, 0, 0, 30)
            label.Font = Enum.Font.GothamSemibold
            label.Text = labelText
            label.TextColor3 = Color3.fromRGB(245, 245, 245)
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left

            local outerBar = Instance.new("Frame")
            outerBar.Parent = sliderBg
            outerBar.Name = "bar"
            outerBar.Size = UDim2.new(0, 110, 0, 4)
            outerBar.Position = UDim2.new(1, -10, 0.5, 10)
            outerBar.AnchorPoint = Vector2.new(1, 0.5)
            outerBar.BackgroundColor3 = _G.Primary
            outerBar.BackgroundTransparency = 0.8
            local obCorner = Instance.new("UICorner"); obCorner.Parent = outerBar; obCorner.CornerRadius = UDim.new(0, 5)

            minVal = tonumber(minVal) or 0
            maxVal = tonumber(maxVal) or 100
            defaultVal = tonumber(defaultVal) or minVal
            local range = maxVal - minVal
            local normalized = 0
            if range ~= 0 then normalized = (defaultVal - minVal) / range end

            local fill = Instance.new("Frame")
            fill.Parent = outerBar
            fill.Name = "bar1"
            fill.Size = UDim2.new(math.clamp(normalized, 0, 1), 0, 0, 4)
            fill.BackgroundColor3 = _G.Dark
            local fillCorner = Instance.new("UICorner"); fillCorner.Parent = fill; fillCorner.CornerRadius = UDim.new(0, 5)

            local handle = Instance.new("Frame")
            handle.Parent = fill
            handle.Name = "circlebar"
            handle.Size = UDim2.new(0, 14, 0, 14)
            handle.Position = UDim2.new(1, 0, 0, -5)
            handle.AnchorPoint = Vector2.new(0.5, 0)
            handle.BackgroundColor3 = _G.Dark
            local handleCorner = Instance.new("UICorner"); handleCorner.Parent = handle; handleCorner.CornerRadius = UDim.new(0, 100)

            local valueBox = Instance.new("TextBox")
            valueBox.Parent = sliderBg
            valueBox.BackgroundColor3 = _G.Dark
            valueBox.BackgroundTransparency = 0.1
            valueBox.Font = Enum.Font.Code
            valueBox.Size = UDim2.new(0, 35, 0, 15)
            valueBox.AnchorPoint = Vector2.new(1, 0.5)
            valueBox.Position = UDim2.new(1, -10, 0.5, -10)
            valueBox.TextColor3 = Color3.fromRGB(245, 245, 245)
            valueBox.TextSize = 9
            valueBox.Text = tostring(defaultVal)
            valueBox.ClearTextOnFocus = false
            valueBox.TextXAlignment = Enum.TextXAlignment.Center
            local vbCorner = Instance.new("UICorner"); vbCorner.Parent = valueBox; vbCorner.CornerRadius = UDim.new(0, 3)

            local sliding = false

            handle.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                    sliding = true
                end
            end)
            outerBar.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                    sliding = true
                    local relX = math.clamp(input.Position.X - fill.AbsolutePosition.X, 0, outerBar.AbsoluteSize.X)
                    local value = math.floor(relX / outerBar.AbsoluteSize.X * (range) + minVal)
                    pcall(onChange, value)
                    valueBox.Text = tostring(value)
                    fill.Size = UDim2.new(0, math.clamp(relX, 0, outerBar.AbsoluteSize.X), 0, 4)
                    handle.Position = UDim2.new(0, math.clamp(relX - 5, 0, outerBar.AbsoluteSize.X), 0, -5)
                end
            end)
            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                    sliding = false
                end
            end)
            UserInputService.InputChanged:Connect(function(input)
                if sliding and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                    local relX = math.clamp(input.Position.X - fill.AbsolutePosition.X, 0, outerBar.AbsoluteSize.X)
                    local value = math.floor(relX / outerBar.AbsoluteSize.X * (range) + minVal)
                    pcall(onChange, value)
                    valueBox.Text = tostring(value)
                    fill.Size = UDim2.new(0, math.clamp(relX, 0, outerBar.AbsoluteSize.X), 0, 4)
                    handle.Position = UDim2.new(0, math.clamp(relX - 5, 0, outerBar.AbsoluteSize.X), 0, -5)
                end
            end)

            valueBox.FocusLost:Connect(function()
                local num = tonumber(valueBox.Text) or defaultVal
                if num > maxVal then num = maxVal end
                if num < minVal then num = minVal end
                local frac = 0
                if range ~= 0 then frac = (num - minVal) / range end
                fill.Size = UDim2.new(frac, 0, 0, 4)
                handle.Position = UDim2.new(frac, 0, 0, -5)
                valueBox.Text = tostring(num)
                pcall(onChange, num)
            end)

            -- initial callback
            pcall(function() if onChange then onChange(defaultVal) end end)
        end

        -- Textbox
        function pageApi.Textbox(_, labelText, _, onConfirm)
            local frame = Instance.new("Frame")
            frame.Parent = page
            frame.Name = "Textbox"
            frame.BackgroundColor3 = _G.Primary
            frame.BackgroundTransparency = 0.8
            frame.Size = UDim2.new(1, 0, 0, 35)
            local corner = Instance.new("UICorner"); corner.Parent = frame; corner.CornerRadius = UDim.new(0, 8)

            local label = Instance.new("TextLabel")
            label.Name = "TextboxLabel"
            label.Parent = frame
            label.BackgroundTransparency = 1
            label.Position = UDim2.new(0, 15, 0.5, 0)
            label.Text = labelText
            label.Size = UDim2.new(1, 0, 0, 35)
            label.Font = Enum.Font.GothamSemibold
            label.AnchorPoint = Vector2.new(0, 0.5)
            label.TextColor3 = Color3.fromRGB(245, 245, 245)
            label.TextSize = 13
            label.TextXAlignment = Enum.TextXAlignment.Left

            local realBox = Instance.new("TextBox")
            realBox.Name = "RealTextbox"
            realBox.Parent = frame
            realBox.BackgroundColor3 = _G.Dark
            realBox.BackgroundTransparency = 0.1
            realBox.Position = UDim2.new(1, -5, 0.5, 0)
            realBox.AnchorPoint = Vector2.new(1, 0.5)
            realBox.Size = UDim2.new(0, 80, 0, 25)
            realBox.Font = Enum.Font.GothamSemibold
            realBox.Text = ""
            realBox.TextColor3 = Color3.fromRGB(225, 225, 225)
            realBox.TextSize = 11
            realBox.ClearTextOnFocus = false
            realBox.TextXAlignment = Enum.TextXAlignment.Center
            realBox.ClipsDescendants = true
            local realCorner = Instance.new("UICorner"); realCorner.Parent = realBox; realCorner.CornerRadius = UDim.new(0, 5)

            realBox.FocusLost:Connect(function()
                pcall(onConfirm, realBox.Text)
            end)
        end

        -- StatusBox (multi-line status/debug box that supports horizontal + vertical scrolling)
        -- Usage: pageApi.StatusBox(_, titleText, initialText, textColor)
        -- Returns api with Set(text), Append(text), Clear(), GetText(), SetColor(Color3)
        function pageApi.StatusBox(_, titleText, initialText, textColor)
            initialText = tostring(initialText or "")
            local container = Instance.new("Frame")
            container.Name = "StatusBox"
            container.Parent = page
            container.BackgroundColor3 = _G.Primary
            container.BackgroundTransparency = 0.8
            container.Size = UDim2.new(1, 0, 0, 180) -- increased size
            local corner = Instance.new("UICorner"); corner.Parent = container; corner.CornerRadius = UDim.new(0, 8)

            local title = Instance.new("TextLabel")
            title.Parent = container
            title.BackgroundTransparency = 1
            title.Position = UDim2.new(0, 10, 0, 6)
            title.Size = UDim2.new(1, -20, 0, 18)
            title.Font = Enum.Font.GothamSemibold
            title.Text = titleText or "Status"
            title.TextColor3 = Color3.fromRGB(245, 245, 245)
            title.TextSize = 12
            title.TextXAlignment = Enum.TextXAlignment.Left

            -- Scroll frame that will allow both horizontal and vertical scrolling by expanding CanvasSize when text grows
            local scroll = Instance.new("ScrollingFrame")
            scroll.Parent = container
            scroll.Name = "StatusScroll"
            scroll.Active = true
            scroll.BackgroundTransparency = 1
            scroll.BorderSizePixel = 0
            scroll.Position = UDim2.new(0, 8, 0, 30)
            scroll.Size = UDim2.new(1, -16, 1, -38)
            scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
            scroll.ScrollBarThickness = 6
            scroll.AutomaticCanvasSize = Enum.AutomaticSize.None
            scroll.ClipsDescendants = true

            local content = Instance.new("TextLabel")
            content.Parent = scroll
            content.Name = "StatusContent"
            content.BackgroundTransparency = 1
            content.Position = UDim2.new(0, 0, 0, 0)
            content.Size = UDim2.new(0, 0, 0, 0)
            content.Font = Enum.Font.Code
            content.TextWrapped = false -- do not wrap so horizontal scroll is used
            content.TextXAlignment = Enum.TextXAlignment.Left
            content.TextYAlignment = Enum.TextYAlignment.Top
            content.TextColor3 = textColor or Color3.fromRGB(230, 230, 230)
            content.TextSize = 15 -- increased text size
            content.Text = initialText

            local function refreshCanvas()
                -- compute size of text
                local txt = tostring(content.Text or "")
                -- Use extremely wide constraint to measure a single-line-like width with newlines taken into account
                local size = TextService:GetTextSize(txt, content.TextSize, content.Font, Vector2.new(1e6, 1e6))
                -- Ensure some padding
                local padX = 10
                local padY = 6
                content.Size = UDim2.new(0, math.max(200, math.ceil(size.X)), 0, math.max(30, math.ceil(size.Y)))
                scroll.CanvasSize = UDim2.new(0, content.AbsoluteSize.X + padX, 0, content.AbsoluteSize.Y + padY)
            end

            -- initialize size after one heartbeat to allow proper absolute size calculation
            RunService.Heartbeat:Wait()
            refreshCanvas()

            local api = {}
            function api.Set(_, newText)
                content.Text = tostring(newText or "")
                refreshCanvas()
            end
            function api.Append(_, moreText)
                content.Text = content.Text .. tostring(moreText or "")
                refreshCanvas()
                -- optionally scroll to bottom-right to show new content
                RunService.Heartbeat:Wait()
                pcall(function()
                    scroll.CanvasPosition = Vector2.new(math.max(0, content.AbsoluteSize.X - scroll.AbsoluteSize.X), math.max(0, content.AbsoluteSize.Y - scroll.AbsoluteSize.Y))
                end)
            end
            function api.Clear(_)
                content.Text = ""
                refreshCanvas()
            end
            function api.GetText(_)
                return content.Text
            end
            function api.SetColor(_, clr)
                if typeof(clr) == "Color3" then
                    content.TextColor3 = clr
                end
            end

            return api
        end

        -- Folder (container that can hold elements; returns an API similar to page but that inserts into the folder)
        -- Usage: local f = pageApi.Folder(_, "My Folder"); f.Button(...), f.Toggle(...), f.Textbox(...), f.StatusBox(...)
        function pageApi.Folder(_, folderTitle)
            local wrap = Instance.new("Frame")
            wrap.Name = "FolderWrapper"
            wrap.Parent = page
            wrap.BackgroundTransparency = 1
            wrap.Size = UDim2.new(1, 0, 0, 150)

            local header = Instance.new("TextButton")
            header.Parent = wrap
            header.Name = "FolderHeader"
            header.BackgroundColor3 = _G.Primary
            header.BackgroundTransparency = 0.85
            header.Size = UDim2.new(1, 0, 0, 28)
            header.AutoButtonColor = true
            header.Font = Enum.Font.GothamBold
            header.Text = folderTitle or "Folder"
            header.TextColor3 = Color3.fromRGB(245,245,245)
            header.TextSize = 13
            local hcorner = Instance.new("UICorner"); hcorner.Parent = header; hcorner.CornerRadius = UDim.new(0,6)

            local inner = Instance.new("ScrollingFrame")
            inner.Parent = wrap
            inner.Name = "FolderInner"
            inner.Active = true
            inner.BackgroundColor3 = _G.Dark
            inner.BackgroundTransparency = 0.15
            inner.Position = UDim2.new(0, 0, 0, 30)
            inner.Size = UDim2.new(1, 0, 1, -30)
            inner.BorderSizePixel = 0
            inner.ScrollBarThickness = 4
            inner.ClipsDescendants = true

            local innerPad = Instance.new("UIPadding"); innerPad.Parent = inner; innerPad.PaddingTop = UDim.new(0,6); innerPad.PaddingLeft = UDim.new(0,6); innerPad.PaddingRight = UDim.new(0,6)
            local innerLayout = Instance.new("UIListLayout"); innerLayout.Parent = inner; innerLayout.SortOrder = Enum.SortOrder.LayoutOrder; innerLayout.Padding = UDim.new(0,4)

            -- allow collapsing
            local collapsed = false
            header.MouseButton1Click:Connect(function()
                collapsed = not collapsed
                if collapsed then
                    TweenService:Create(inner, TweenInfo.new(0.2), {Size = UDim2.new(1,0,0,0)}):Play()
                    TweenService:Create(wrap, TweenInfo.new(0.2), {Size = UDim2.new(1,0,0,28)}):Play()
                else
                    TweenService:Create(inner, TweenInfo.new(0.2), {Size = UDim2.new(1,0,1,-30)}):Play()
                    TweenService:Create(wrap, TweenInfo.new(0.2), {Size = UDim2.new(1,0,0,150)}):Play()
                end
            end)

            -- folder API: reimplement a few element creators but parent them to 'inner'
            local folderApi = {}

            function folderApi.Button(_, text, callback)
                -- copy of pageApi.Button but parent = inner
                local frame = Instance.new("Frame")
                frame.Name = "Button"
                frame.Parent = inner
                frame.BackgroundColor3 = _G.Primary
                frame.BackgroundTransparency = 0.75
                frame.Size = UDim2.new(1, 0, 0, 36)
                local corner = Instance.new("UICorner"); corner.CornerRadius = UDim.new(0, 8); corner.Parent = frame

                local label = Instance.new("TextLabel")
                label.Name = "TextLabel"
                label.Parent = frame
                label.BackgroundTransparency = 1
                label.AnchorPoint = Vector2.new(0, 0.5)
                label.Position = UDim2.new(0, 15, 0.5, 0)
                label.Size = UDim2.new(1, -40, 1, 0)
                label.Font = Enum.Font.GothamSemibold
                label.Text = text
                label.TextXAlignment = Enum.TextXAlignment.Left
                label.TextColor3 = Color3.fromRGB(245, 245, 245)
                label.TextSize = 13

                local btn = Instance.new("TextButton")
                btn.Name = "TextButton"
                btn.Parent = frame
                btn.BackgroundColor3 = _G.Dark
                btn.BackgroundTransparency = 0
                btn.AnchorPoint = Vector2.new(1, 0.5)
                btn.Position = UDim2.new(1, -10, 0.5, 0)
                btn.Size = UDim2.new(0, 22, 0, 22)
                btn.AutoButtonColor = true

                local btnCorner = Instance.new("UICorner"); btnCorner.CornerRadius = UDim.new(0, 6); btnCorner.Parent = btn

                local img = Instance.new("ImageLabel")
                img.Name = "ImageLabel"
                img.Parent = btn
                img.BackgroundTransparency = 1
                img.AnchorPoint = Vector2.new(0.5, 0.5)
                img.Position = UDim2.new(0.5, 0, 0.5, 0)
                img.Size = UDim2.new(0, 15, 0, 15)
                img.Image = "rbxassetid://10723375250"
                img.ImageTransparency = 0.05
                img.ImageColor3 = Color3.fromRGB(245, 245, 245)

                btn.MouseButton1Click:Connect(function()
                    pcall(callback)
                end)
            end

            function folderApi.Toggle(_, text, defaultValue, description, callback)
                local state = defaultValue and true or false

                local container = Instance.new("TextButton")
                container.Name = "Button"
                container.Parent = inner
                container.BackgroundColor3 = _G.Primary
                container.BackgroundTransparency = 0.8
                container.Size = UDim2.new(1, 0, 0, 46)
                container.AutoButtonColor = false
                local contCorner = Instance.new("UICorner"); contCorner.CornerRadius = UDim.new(0, 8); contCorner.Parent = container

                local titleLbl = Instance.new("TextLabel")
                titleLbl.Parent = container
                titleLbl.BackgroundTransparency = 1
                titleLbl.Size = UDim2.new(1, 0, 0, 35)
                titleLbl.Font = Enum.Font.GothamSemibold
                titleLbl.Text = text
                titleLbl.TextColor3 = Color3.fromRGB(245, 245, 245)
                titleLbl.TextSize = 13
                titleLbl.TextXAlignment = Enum.TextXAlignment.Left
                titleLbl.AnchorPoint = Vector2.new(0, 0.5)
                titleLbl.Position = UDim2.new(0, 15, 0.5, 0)

                local desc = Instance.new("TextLabel")
                desc.Parent = titleLbl
                desc.BackgroundTransparency = 1
                desc.Position = UDim2.new(0, 0, 0, 22)
                desc.Size = UDim2.new(0, 280, 0, 16)
                desc.Font = Enum.Font.Gotham
                desc.TextColor3 = Color3.fromRGB(200, 200, 200)
                desc.TextSize = 10
                if description and description ~= "" then
                    desc.Text = description
                    titleLbl.Position = UDim2.new(0, 15, 0.5, -5)
                    desc.Visible = true
                else
                    desc.Visible = false
                end

                local toggleFrame = Instance.new("Frame")
                toggleFrame.Name = "ToggleFrame"
                toggleFrame.Parent = container
                toggleFrame.BackgroundColor3 = _G.Dark
                toggleFrame.BackgroundTransparency = 0
                toggleFrame.Position = UDim2.new(1, -10, 0.5, 0)
                toggleFrame.Size = UDim2.new(0, 40, 0, 22)
                toggleFrame.AnchorPoint = Vector2.new(1, 0.5)
                local tfCorner = Instance.new("UICorner"); tfCorner.Parent = toggleFrame; tfCorner.CornerRadius = UDim.new(0, 10)

                local outerStroke = Instance.new("UIStroke")
                outerStroke.Parent = toggleFrame
                outerStroke.Color = _G.Primary
                outerStroke.Thickness = 1

                local toggleBtn = Instance.new("TextButton")
                toggleBtn.Name = "ToggleImage"
                toggleBtn.Parent = toggleFrame
                toggleBtn.BackgroundColor3 = _G.Dark
                toggleBtn.BackgroundTransparency = 0
                toggleBtn.Size = UDim2.new(1, 0, 1, 0)
                toggleBtn.Text = ""
                toggleBtn.AutoButtonColor = false
                local tbtnCorner = Instance.new("UICorner"); tbtnCorner.Parent = toggleBtn; tbtnCorner.CornerRadius = UDim.new(0, 10)

                local circle = Instance.new("Frame")
                circle.Name = "Circle"
                circle.Parent = toggleBtn
                circle.BackgroundColor3 = _G.Primary
                circle.Size = UDim2.new(0, 16, 0, 16)
                circle.Position = UDim2.new(0, 4, 0.5, 0)
                circle.AnchorPoint = Vector2.new(0, 0.5)
                local ccorner = Instance.new("UICorner"); ccorner.Parent = circle; ccorner.CornerRadius = UDim.new(0, 16)

                local function setVisual(on)
                    if on then
                        outerStroke.Thickness = 0
                        circle:TweenPosition(UDim2.new(0, 20, 0.5, 0), "Out", "Sine", 0.35, true)
                        TweenService:Create(circle, TweenInfo.new(0.35), {BackgroundColor3 = _G.Dark}):Play()
                        TweenService:Create(toggleBtn, TweenInfo.new(0.35), {BackgroundColor3 = _G.Primary}):Play()
                    else
                        outerStroke.Thickness = 1
                        circle:TweenPosition(UDim2.new(0, 4, 0.5, 0), "Out", "Sine", 0.25, true)
                        TweenService:Create(circle, TweenInfo.new(0.35), {BackgroundColor3 = _G.Primary}):Play()
                        TweenService:Create(toggleBtn, TweenInfo.new(0.35), {BackgroundColor3 = _G.Dark}):Play()
                    end
                end

                toggleBtn.MouseButton1Click:Connect(function()
                    state = not state
                    setVisual(state)
                    pcall(callback, state)
                end)

                -- initialize
                setVisual(state)
                pcall(function() if callback then callback(state) end end)
            end

            function folderApi.Textbox(_, labelText, _, onConfirm)
                local frame = Instance.new("Frame")
                frame.Parent = inner
                frame.Name = "Textbox"
                frame.BackgroundColor3 = _G.Primary
                frame.BackgroundTransparency = 0.8
                frame.Size = UDim2.new(1, 0, 0, 35)
                local corner = Instance.new("UICorner"); corner.Parent = frame; corner.CornerRadius = UDim.new(0, 8)

                local label = Instance.new("TextLabel")
                label.Name = "TextboxLabel"
                label.Parent = frame
                label.BackgroundTransparency = 1
                label.Position = UDim2.new(0, 15, 0.5, 0)
                label.Text = labelText
                label.Size = UDim2.new(1, 0, 0, 35)
                label.Font = Enum.Font.GothamSemibold
                label.AnchorPoint = Vector2.new(0, 0.5)
                label.TextColor3 = Color3.fromRGB(245, 245, 245)
                label.TextSize = 13
                label.TextXAlignment = Enum.TextXAlignment.Left

                local realBox = Instance.new("TextBox")
                realBox.Name = "RealTextbox"
                realBox.Parent = frame
                realBox.BackgroundColor3 = _G.Dark
                realBox.BackgroundTransparency = 0.1
                realBox.Position = UDim2.new(1, -5, 0.5, 0)
                realBox.AnchorPoint = Vector2.new(1, 0.5)
                realBox.Size = UDim2.new(0, 80, 0, 25)
                realBox.Font = Enum.Font.GothamSemibold
                realBox.Text = ""
                realBox.TextColor3 = Color3.fromRGB(225, 225, 225)
                realBox.TextSize = 11
                realBox.ClearTextOnFocus = false
                realBox.TextXAlignment = Enum.TextXAlignment.Center
                realBox.ClipsDescendants = true
                local realCorner = Instance.new("UICorner"); realCorner.Parent = realBox; realCorner.CornerRadius = UDim.new(0, 5)

                realBox.FocusLost:Connect(function()
                    pcall(onConfirm, realBox.Text)
                end)
            end

            function folderApi.StatusBox(_, titleText, initialText, textColor)
                initialText = tostring(initialText or "")
                local container = Instance.new("Frame")
                container.Name = "StatusBox"
                container.Parent = inner
                container.BackgroundColor3 = _G.Primary
                container.BackgroundTransparency = 0.8
                container.Size = UDim2.new(1, 0, 0, 140)
                local corner = Instance.new("UICorner"); corner.Parent = container; corner.CornerRadius = UDim.new(0, 8)

                local title = Instance.new("TextLabel")
                title.Parent = container
                title.BackgroundTransparency = 1
                title.Position = UDim2.new(0, 10, 0, 6)
                title.Size = UDim2.new(1, -20, 0, 18)
                title.Font = Enum.Font.GothamSemibold
                title.Text = titleText or "Status"
                title.TextColor3 = Color3.fromRGB(245, 245, 245)
                title.TextSize = 12
                title.TextXAlignment = Enum.TextXAlignment.Left

                local scroll = Instance.new("ScrollingFrame")
                scroll.Parent = container
                scroll.Name = "StatusScroll"
                scroll.Active = true
                scroll.BackgroundTransparency = 1
                scroll.BorderSizePixel = 0
                scroll.Position = UDim2.new(0, 8, 0, 30)
                scroll.Size = UDim2.new(1, -16, 1, -38)
                scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
                scroll.ScrollBarThickness = 6
                scroll.AutomaticCanvasSize = Enum.AutomaticSize.None
                scroll.ClipsDescendants = true

                local content = Instance.new("TextLabel")
                content.Parent = scroll
                content.Name = "StatusContent"
                content.BackgroundTransparency = 1
                content.Position = UDim2.new(0, 0, 0, 0)
                content.Size = UDim2.new(0, 0, 0, 0)
                content.Font = Enum.Font.Code
                content.TextWrapped = false
                content.TextXAlignment = Enum.TextXAlignment.Left
                content.TextYAlignment = Enum.TextYAlignment.Top
                content.TextColor3 = textColor or Color3.fromRGB(230, 230, 230)
                content.TextSize = 15
                content.Text = initialText

                local function refreshCanvas()
                    local txt = tostring(content.Text or "")
                    local size = TextService:GetTextSize(txt, content.TextSize, content.Font, Vector2.new(1e6, 1e6))
                    local padX = 10
                    local padY = 6
                    content.Size = UDim2.new(0, math.max(200, math.ceil(size.X)), 0, math.max(30, math.ceil(size.Y)))
                    scroll.CanvasSize = UDim2.new(0, content.AbsoluteSize.X + padX, 0, content.AbsoluteSize.Y + padY)
                end

                RunService.Heartbeat:Wait()
                refreshCanvas()

                local api = {}
                function api.Set(_, newText)
                    content.Text = tostring(newText or "")
                    refreshCanvas()
                end
                function api.Append(_, moreText)
                    content.Text = content.Text .. tostring(moreText or "")
                    refreshCanvas()
                    RunService.Heartbeat:Wait()
                    pcall(function()
                        scroll.CanvasPosition = Vector2.new(math.max(0, content.AbsoluteSize.X - scroll.AbsoluteSize.X), math.max(0, content.AbsoluteSize.Y - scroll.AbsoluteSize.Y))
                    end)
                end
                function api.Clear(_)
                    content.Text = ""
                    refreshCanvas()
                end
                function api.GetText(_)
                    return content.Text
                end
                function api.SetColor(_, clr)
                    if typeof(clr) == "Color3" then
                        content.TextColor3 = clr
                    end
                end

                return api
            end

            function folderApi.Label(_, text)
                local lbl = Instance.new("TextLabel")
                lbl.Name = "Label"
                lbl.Parent = inner
                lbl.BackgroundTransparency = 1
                lbl.Size = UDim2.new(1, 0, 0, 20)
                lbl.Font = Enum.Font.GothamSemibold
                lbl.TextColor3 = Color3.fromRGB(225, 225, 225)
                lbl.TextSize = 13
                lbl.Text = text
                lbl.TextXAlignment = Enum.TextXAlignment.Left
                local pad = Instance.new("UIPadding"); pad.PaddingLeft = UDim.new(0, 2); pad.Parent = lbl
                local api = {}
                function api.Set(_, newText) lbl.Text = newText end
                return api
            end

            function folderApi.Seperator(_, text)
                local sp = Instance.new("Frame")
                sp.Name = "Seperator"
                sp.Parent = inner
                sp.BackgroundTransparency = 1
                sp.Size = UDim2.new(1, 0, 0, 36)

                local title = Instance.new("TextLabel")
                title.Name = "Sep2"
                title.Parent = sp
                title.BackgroundTransparency = 1
                title.AnchorPoint = Vector2.new(0.5, 1)
                title.Position = UDim2.new(0.5, 0, 0, 30)
                title.Size = UDim2.new(1, 0, 0, 36)
                title.Font = Enum.Font.GothamBold
                title.Text = text
                title.TextColor3 = Color3.fromRGB(255, 255, 255)
                title.TextSize = 14

                local bar = Instance.new("Frame")
                bar.Name = "Sep3"
                bar.Parent = sp
                bar.BackgroundColor3 = _G.Primary
                bar.BackgroundTransparency = 0
                bar.BorderSizePixel = 0
                bar.AnchorPoint = Vector2.new(0.5, 0.5)
                bar.Position = UDim2.new(0.5, 0, 0, 25)
                bar.Size = UDim2.new(0, 39, 0, 3)
                local bcorner = Instance.new("UICorner"); bcorner.Parent = bar; bcorner.CornerRadius = UDim.new(0, math.huge)

                local textSize = TextService:GetTextSize(title.Text, title.TextSize, title.Font, Vector2.new(1e6, 1e6))
                bar.Size = UDim2.new(0, textSize.X * 0.7, 0, 3)
            end

            return folderApi
        end

        -- Label
        function pageApi.Label(_, text)
            local lbl = Instance.new("TextLabel")
            lbl.Name = "Label"
            lbl.Parent = page
            lbl.BackgroundTransparency = 1
            lbl.Size = UDim2.new(1, 0, 0, 20)
            lbl.Font = Enum.Font.GothamSemibold
            lbl.TextColor3 = Color3.fromRGB(225, 225, 225)
            lbl.TextSize = 13
            lbl.Text = text
            lbl.TextXAlignment = Enum.TextXAlignment.Left
            local pad = Instance.new("UIPadding"); pad.PaddingLeft = UDim.new(0, 2); pad.Parent = lbl
            local api = {}
            function api.Set(_, newText) lbl.Text = newText end
            return api
        end

        -- Seperator
        function pageApi.Seperator(_, text)
            local sp = Instance.new("Frame")
            sp.Name = "Seperator"
            sp.Parent = page
            sp.BackgroundTransparency = 1
            sp.Size = UDim2.new(1, 0, 0, 36)

            local title = Instance.new("TextLabel")
            title.Name = "Sep2"
            title.Parent = sp
            title.BackgroundTransparency = 1
            title.AnchorPoint = Vector2.new(0.5, 1)
            title.Position = UDim2.new(0.5, 0, 0, 30)
            title.Size = UDim2.new(1, 0, 0, 36)
            title.Font = Enum.Font.GothamBold
            title.Text = text
            title.TextColor3 = Color3.fromRGB(255, 255, 255)
            title.TextSize = 14

            local bar = Instance.new("Frame")
            bar.Name = "Sep3"
            bar.Parent = sp
            bar.BackgroundColor3 = _G.Primary
            bar.BackgroundTransparency = 0
            bar.BorderSizePixel = 0
            bar.AnchorPoint = Vector2.new(0.5, 0.5)
            bar.Position = UDim2.new(0.5, 0, 0, 25)
            bar.Size = UDim2.new(0, 39, 0, 3)
            local bcorner = Instance.new("UICorner"); bcorner.Parent = bar; bcorner.CornerRadius = UDim.new(0, math.huge)

            local textSize = TextService:GetTextSize(title.Text, title.TextSize, title.Font, Vector2.new(1e6, 1e6))
            bar.Size = UDim2.new(0, textSize.X * 0.7, 0, 3)
        end

        -- Line
        function pageApi.Line(_)
            local wrap = Instance.new("Frame")
            wrap.Name = "Linee"
            wrap.Parent = page
            wrap.BackgroundTransparency = 1
            wrap.Position = UDim2.new(0, 0, 0.119999997, 0)
            wrap.Size = UDim2.new(1, 0, 0, 20)

            local line = Instance.new("Frame")
            line.Name = "Line"
            line.Parent = wrap
            line.BackgroundColor3 = Color3.fromRGB(125, 125, 125)
            line.BorderSizePixel = 0
            line.Position = UDim2.new(0, 0, 0, 10)
            line.Size = UDim2.new(1, 0, 0, 1)

            local grad = Instance.new("UIGradient")
            grad.Color = ColorSequence.new({
                ColorSequenceKeypoint.new(0, _G.Dark),
                ColorSequenceKeypoint.new(0.4, _G.Primary),
                ColorSequenceKeypoint.new(0.5, _G.Primary),
                ColorSequenceKeypoint.new(0.6, _G.Primary),
                ColorSequenceKeypoint.new(1, _G.Dark),
            })
            grad.Parent = line
        end

        return pageApi
    end

    return library
end

return Update

# Vreta-Hub
A roblox exploit script hub
-- [[ VRETA HUB V1 — DUAL ENGINE MASTER EDITION 🌙⚡ ]]
-- Compatible with Delta, Arceus X, CodeX, and Studio
-- Features: Squad Verified Badges () + Native +1 Tongue Escape Module

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

-- Verified Squad Roles Mapping
local squadRoles = {
    [10789290369] = "Vreta Creator",
    [9605585193]  = "Vreta Admin",
    [8963902794]  = "Vreta Creator's BFF",
    [11690731723] = "Vreta Admin",
    [11046152137] = "Vreta Admin",
    [6145072861]  = "Vreta Admin"
}

-- Safe GUI Parent Selection
local parentGui = LocalPlayer:WaitForChild("PlayerGui")
if gethui then
    parentGui = gethui()
elseif syn and syn.protect_gui then
    parentGui = Instance.new("ScreenGui")
    syn.protect_gui(parentGui)
    parentGui.Parent = game:GetService("CoreGui")
end

-- ================= IN-GAME OVERHEAD VERIFIED BADGE SYSTEM =================
local function applyOverheadBadge(player)
    local roleTitle = squadRoles[player.UserId]
    if not roleTitle then return end

    local function setupChar(char)
        local head = char:WaitForChild("Head", 10)
        if not head then return end

        if head:FindFirstChild("VretaVerifiedTag") then
            head.VretaVerifiedTag:Destroy()
        end

        local billboard = Instance.new("BillboardGui")
        billboard.Name = "VretaVerifiedTag"
        billboard.Adornee = head
        billboard.Size = UDim2.new(0, 200, 0, 30)
        billboard.StudsOffset = Vector3.new(0, 2.3, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = head

        local tagText = Instance.new("TextLabel")
        tagText.Size = UDim2.new(1, 0, 1, 0)
        tagText.BackgroundTransparency = 1
        tagText.Text = roleTitle
        tagText.TextColor3 = Color3.fromRGB(200, 150, 255)
        tagText.Font = Enum.Font.GothamBold
        tagText.TextSize = 13
        tagText.TextStrokeTransparency = 0.2
        tagText.TextStrokeColor3 = Color3.fromRGB(20, 10, 35)
        tagText.Parent = billboard
    end

    if player.Character then
        task.spawn(setupChar, player.Character)
    end
    player.CharacterAdded:Connect(function(char)
        task.spawn(setupChar, char)
    end)
end

for _, p in ipairs(Players:GetPlayers()) do
    applyOverheadBadge(p)
end
Players.PlayerAdded:Connect(applyOverheadBadge)

-- Cleanup Old Loader Sessions
if parentGui:FindFirstChild("VretaHubMasterLoader") then
    parentGui.VretaHubMasterLoader:Destroy()
end

-- ScreenGui Setup
local loaderGui = Instance.new("ScreenGui")
loaderGui.Name = "VretaHubMasterLoader"
loaderGui.ResetOnSpawn = false
loaderGui.Parent = parentGui

-- Main Container Card
local card = Instance.new("Frame")
card.AnchorPoint = Vector2.new(0.5, 0.5)
card.Size = UDim2.new(0, 390, 0, 340)
card.Position = UDim2.new(0.5, 0, 0.5, 0)
card.BackgroundColor3 = Color3.fromRGB(16, 16, 26)
card.BorderSizePixel = 0
card.Active = true
card.Draggable = true
card.Parent = loaderGui

local cardCorner = Instance.new("UICorner")
cardCorner.CornerRadius = UDim.new(0, 16)
cardCorner.Parent = card

local cardStroke = Instance.new("UIStroke")
cardStroke.Color = Color3.fromRGB(175, 115, 255)
cardStroke.Thickness = 2
cardStroke.Transparency = 0.3
cardStroke.Parent = card

-- Title Label
local loaderTitle = Instance.new("TextLabel")
loaderTitle.Size = UDim2.new(1, 0, 0, 32)
loaderTitle.Position = UDim2.new(0, 0, 0.02, 0)
loaderTitle.BackgroundTransparency = 1
loaderTitle.Text = "🌙 VRETA HUB V1"
loaderTitle.TextColor3 = Color3.fromRGB(200, 150, 255)
loaderTitle.TextSize = 18
loaderTitle.Font = Enum.Font.GothamBold
loaderTitle.Parent = card

-- Squad Row
local squadFrame = Instance.new("Frame")
squadFrame.Size = UDim2.new(0.9, 0, 0, 42)
squadFrame.Position = UDim2.new(0.05, 0, 0.12, 0)
squadFrame.BackgroundTransparency = 1
squadFrame.Parent = card

local squadLayout = Instance.new("UIListLayout")
squadLayout.FillDirection = Enum.FillDirection.Horizontal
squadLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
squadLayout.Padding = UDim.new(0, 8)
squadLayout.Parent = squadFrame

local squadList = {10789290369, 11690731723, 8963902794, 11046152137, 9605585193, 6145072861}

for _, userId in ipairs(squadList) do
    local avatarContainer = Instance.new("Frame")
    avatarContainer.Size = UDim2.new(0, 36, 0, 36)
    avatarContainer.BackgroundTransparency = 1
    avatarContainer.Parent = squadFrame

    local avatarImg = Instance.new("ImageLabel")
    avatarImg.Size = UDim2.new(1, 0, 1, 0)
    avatarImg.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
    avatarImg.Image = "rbxthumb://type=AvatarHeadShot&id=" .. tostring(userId) .. "&w=150&h=150"
    avatarImg.Parent = avatarContainer
    
    local imgCorner = Instance.new("UICorner")
    imgCorner.CornerRadius = UDim.new(1, 0)
    imgCorner.Parent = avatarImg
    
    local imgStroke = Instance.new("UIStroke")
    imgStroke.Color = Color3.fromRGB(140, 75, 230)
    imgStroke.Thickness = 1.5
    imgStroke.Parent = avatarImg

    local badgeLabel = Instance.new("TextLabel")
    badgeLabel.Size = UDim2.new(0, 14, 0, 14)
    badgeLabel.Position = UDim2.new(1, -12, 1, -12)
    badgeLabel.BackgroundTransparency = 1
    badgeLabel.Text = ""
    badgeLabel.TextColor3 = Color3.fromRGB(175, 115, 255)
    badgeLabel.TextSize = 11
    badgeLabel.Font = Enum.Font.GothamBold
    badgeLabel.Parent = avatarContainer
end

-- Navigation
local topNavFrame = Instance.new("Frame")
topNavFrame.Size = UDim2.new(0.9, 0, 0, 36)
topNavFrame.Position = UDim2.new(0.05, 0, 0.26, 0)
topNavFrame.BackgroundColor3 = Color3.fromRGB(22, 22, 32)
topNavFrame.Parent = card

local topNavCorner = Instance.new("UICorner")
topNavCorner.CornerRadius = UDim.new(0, 8)
topNavCorner.Parent = topNavFrame

local btnVreta = Instance.new("TextButton")
btnVreta.Size = UDim2.new(0.48, 0, 0.85, 0)
btnVreta.Position = UDim2.new(0.01, 0, 0.075, 0)
btnVreta.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
btnVreta.Text = "👑 Vreta Scripts"
btnVreta.TextColor3 = Color3.fromRGB(255, 255, 255)
btnVreta.Font = Enum.Font.GothamBold
btnVreta.TextSize = 11
btnVreta.Parent = topNavFrame

local btnOthers = Instance.new("TextButton")
btnOthers.Size = UDim2.new(0.48, 0, 0.85, 0)
btnOthers.Position = UDim2.new(0.51, 0, 0.075, 0)
btnOthers.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
btnOthers.Text = "🌐 Others Scripts"
btnOthers.TextColor3 = Color3.fromRGB(180, 180, 200)
btnOthers.Font = Enum.Font.GothamBold
btnOthers.TextSize = 11
btnOthers.Parent = topNavFrame

-- Pages Container
local pageVreta = Instance.new("ScrollingFrame")
pageVreta.Size = UDim2.new(0.9, 0, 0, 190)
pageVreta.Position = UDim2.new(0.05, 0, 0.39, 0)
pageVreta.BackgroundTransparency = 1
pageVreta.ScrollBarThickness = 3
pageVreta.AutomaticCanvasSize = Enum.AutomaticSize.Y
pageVreta.Visible = true
pageVreta.Parent = card

local layoutVreta = Instance.new("UIListLayout")
layoutVreta.Padding = UDim.new(0, 8)
layoutVreta.Parent = pageVreta

local pageOthers = Instance.new("ScrollingFrame")
pageOthers.Size = UDim2.new(0.9, 0, 0, 190)
pageOthers.Position = UDim2.new(0.05, 0, 0.39, 0)
pageOthers.BackgroundTransparency = 1
pageOthers.ScrollBarThickness = 3
pageOthers.AutomaticCanvasSize = Enum.AutomaticSize.Y
pageOthers.Visible = false
pageOthers.Parent = card

local layoutOthers = Instance.new("UIListLayout")
layoutOthers.Padding = UDim.new(0, 8)
layoutOthers.Parent = pageOthers

btnVreta.Activated:Connect(function()
    pageVreta.Visible = true
    pageOthers.Visible = false
    btnVreta.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
    btnVreta.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnOthers.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
    btnOthers.TextColor3 = Color3.fromRGB(180, 180, 200)
end)

btnOthers.Activated:Connect(function()
    pageVreta.Visible = false
    pageOthers.Visible = true
    btnOthers.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
    btnOthers.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnVreta.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
    btnVreta.TextColor3 = Color3.fromRGB(180, 180, 200)
end)

local function closeLoader()
    TweenService:Create(card, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
        Size = UDim2.new(0, 0, 0, 0),
        BackgroundTransparency = 1
    }):Play()
    task.wait(0.4)
    loaderGui:Destroy()
end

-- ================= NATIVE VRETA SCRIPTS =================
local function createVretaGameButton(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -6, 0, 40)
    btn.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    btn.Parent = pageVreta

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = btn
    return btn
end

local mm2Btn = createVretaGameButton("🔪 MM2 (Murder Mystery 2)")
local wasteTimeBtn = createVretaGameButton("⏳ Waste Time")
local tongueEscapeBtn = createVretaGameButton("👅 +1 Tongue Escape")

-- ================= EXTERNAL SCRIPTS =================
local function addExternalScriptButton(name, rawUrl)
    local extBtn = Instance.new("TextButton")
    extBtn.Size = UDim2.new(1, -6, 0, 40)
    extBtn.BackgroundColor3 = Color3.fromRGB(32, 32, 46)
    extBtn.Text = "⚡ " .. name
    extBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    extBtn.Font = Enum.Font.GothamMedium
    extBtn.TextSize = 11
    extBtn.Parent = pageOthers

    local extCorner = Instance.new("UICorner")
    extCorner.CornerRadius = UDim.new(0, 8)
    extCorner.Parent = extBtn

    extBtn.Activated:Connect(function()
        closeLoader()
        pcall(function()
            loadstring(game:HttpGet(rawUrl))()
        end)
    end)
end

addExternalScriptButton("+1 Shrink Per Step {Keyless}", "https://scriptblox.com/raw/NEW-CUBES+1-Shrink-per-Step-Radium-Hub-Shrink-Per-Step-225259")
addExternalScriptButton("MM2 {Keyless}", "https://raw.githubusercontent.com/as6cd0/SP_Hub/refs/heads/main/Loader")
addExternalScriptButton("Infinite Yield Admin", "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source")

mm2Btn.Activated:Connect(function()
    closeLoader()
    print("🌙 Vreta MM2 Loaded!")
end)

-- ========================================================
-- NATIVE +1 TONGUE ESCAPE MODULE
-- ========================================================
tongueEscapeBtn.Activated:Connect(function()
    closeLoader()

    if parentGui:FindFirstChild("VretaTongueGui") then
        parentGui.VretaTongueGui:Destroy()
    end

    local gui = Instance.new("ScreenGui")
    gui.Name = "VretaTongueGui"
    gui.ResetOnSpawn = false
    gui.Parent = parentGui

    local main = Instance.new("Frame")
    main.Size = UDim2.new(0, 360, 0, 260)
    main.Position = UDim2.new(0.5, -180, 0.35, -130)
    main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
    main.Active = true
    main.Draggable = true
    main.Parent = gui

    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 12)
    mainCorner.Parent = main

    local mainStroke = Instance.new("UIStroke")
    mainStroke.Color = Color3.fromRGB(175, 115, 255)
    mainStroke.Thickness = 1.5
    mainStroke.Parent = main

    local topBar = Instance.new("Frame")
    topBar.Size = UDim2.new(1, 0, 0, 38)
    topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
    topBar.Parent = main

    local topCorner = Instance.new("UICorner")
    topCorner.CornerRadius = UDim.new(0, 12)
    topCorner.Parent = topBar

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, -50, 1, 0)
    title.Position = UDim2.new(0, 12, 0, 0)
    title.BackgroundTransparency = 1
    title.Text = "👅 VRETA HUB — +1 TONGUE ESCAPE"
    title.TextColor3 = Color3.fromRGB(200, 150, 255)
    title.TextSize = 12
    title.Font = Enum.Font.GothamBold
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = topBar

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(0, 85, 0, 34)
    toggleBtn.Position = UDim2.new(0, 15, 0.4, 0)
    toggleBtn.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
    toggleBtn.Text = "👅 VRETA"
    toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggleBtn.Font = Enum.Font.GothamBold
    toggleBtn.TextSize = 11
    toggleBtn.Active = true
    toggleBtn.Draggable = true
    toggleBtn.Visible = false
    toggleBtn.Parent = gui

    local closeBtn = Instance.new("TextButton")
    closeBtn.Size = UDim2.new(0, 26, 0, 26)
    closeBtn.Position = UDim2.new(1, -32, 0, 6)
    closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
    closeBtn.Text = "×"
    closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.Parent = topBar

    closeBtn.Activated:Connect(function() main.Visible = false; toggleBtn.Visible = true end)
    toggleBtn.Activated:Connect(function() main.Visible = true; toggleBtn.Visible = false end)

    local container = Instance.new("ScrollingFrame")
    container.Size = UDim2.new(0.92, 0, 1, -50)
    container.Position = UDim2.new(0.04, 0, 0, 44)
    container.BackgroundTransparency = 1
    container.ScrollBarThickness = 3
    container.AutomaticCanvasSize = Enum.AutomaticSize.Y
    container.Parent = main

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 8)
    layout.Parent = container

    local function addToggle(text, callback)
        local state = false
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, 0, 0, 36)
        btn.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
        btn.Text = text .. ": OFF 🔴"
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.Font = Enum.Font.GothamMedium
        btn.TextSize = 10
        btn.Parent = container

        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 8)
        corner.Parent = btn

        btn.Activated:Connect(function()
            state = not state
            btn.BackgroundColor3 = state and Color3.fromRGB(140, 75, 230) or Color3.fromRGB(30, 30, 42)
            btn.Text = text .. ": " .. (state and "ON 🟢" or "OFF 🔴")
            callback(state)
        end)
    end

    local function getRoot()
        local char = LocalPlayer.Character
        return char and char:FindFirstChild("HumanoidRootPart")
    end

    local function triggerJump()
        local char = LocalPlayer.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end

    -- 1. 6x Treadmill Teleport & Jump Logic
    local treadmillActive = false
    addToggle("⚡ 6x Treadmill", function(active)
        treadmillActive = active
        if active then
            local root = getRoot()
            if root then
                root.CFrame = CFrame.new(3189.3, 851.6, -563.5)
            end
            triggerJump()

            task.spawn(function()
                while treadmillActive do
                    local currentRoot = getRoot()
                    if currentRoot then
                        currentRoot.CFrame = CFrame.new(3189.3, 851.6, -563.5)
                    end
                    task.wait(0.05)
                end
            end)
        else
            triggerJump()
        end
    end)

    -- 2. 8K Wins High-Speed Slide
    local winsActive = false
    addToggle("🏆 8K Wins (stolen feature from xklrzz UI)", function(active)
        winsActive = active
        if active then
            task.spawn(function()
                local toggleState = false
                while winsActive do
                    local root = getRoot()
                    if root then
                        local targetPos = toggleState and Vector3.new(-13196, 506, -559) or Vector3.new(-13190, 506, -559)
                        root.CFrame = CFrame.new(targetPos)
                        toggleState = not toggleState
                    end
                    task.wait(300 / 10000)
                end
            end)
        end
    end)

    -- 3. Auto Tongue Walking
    local tongueActive = false
    addToggle("👅 Auto Tongue Walk", function(active)
        tongueActive = active
        if active then
            task.spawn(function()
                local step = false
                while tongueActive do
                    local root = getRoot()
                    if root then
                        local targetPos = step and Vector3.new(2889.3, 851.6, -563.5) or Vector3.new(2890.3, 851.6, -563.5)
                        root.CFrame = CFrame.new(targetPos)
                        step = not step
                    end
                    task.wait(0.1)
                end
            end)
        end
    end)

    print("👅 Native +1 Tongue Escape Loaded!")
end)

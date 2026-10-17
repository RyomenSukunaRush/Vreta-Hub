-- [[ VRETA HUB V1 — DUAL ENGINE MASTER EDITION (CRASH-FIXED) 🌙⚡ ]]
-- Compatible with Delta, Arceus X, CodeX, and Studio

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local UserInputService = game:GetService("UserInputService")
local InsertService = game:GetService("InsertService")

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
    pcall(function()
        TweenService:Create(card, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Size = UDim2.new(0, 0, 0, 0),
            BackgroundTransparency = 1
        }):Play()
        task.wait(0.25)
        loaderGui:Destroy()
    end)
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
local backflipObbyBtn = createVretaGameButton("🤸‍♂️ +1 Backflip Obby Escape")
local trollTowerBtn = createVretaGameButton("🗼 Troll Tower X")
local ninjaKeyboardBtn = createVretaGameButton("⌨️ +1 Speed Ninja Keyboard")

-- ================= EXTERNAL SCRIPTS (FIXED STRING CONCAT) =================
local function addExternalScriptButton(name, rawUrl)
    local extBtn = Instance.new("TextButton")
    extBtn.Size = UDim2.new(1, -6, 0, 40)
    extBtn.BackgroundColor3 = Color3.fromRGB(32, 32, 46)
    extBtn.Text = "⚡ " .. name  -- FIXED: used '..' instead of '+'
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

addExternalScriptButton("+1 Tongue Escape {Keyless}", "https://rawscripts.net/raw/UPDATE!-+1-Tongue-Escape-Keyless-or-10x-Auto-Tongue-or-Auto-Win-or-More..-228451")
addExternalScriptButton("+1 Shrink Per Step {Keyless}", "https://scriptblox.com/raw/NEW-CUBES+1-Shrink-per-Step-Radium-Hub-Shrink-Per-Step-225259")
addExternalScriptButton("MM2 {Keyless}", "https://raw.githubusercontent.com/as6cd0/SP_Hub/refs/heads/main/Loader")
addExternalScriptButton("Infinite Yield Admin", "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source")


-- ========================================================
-- NATIVE MODULE 1: MM2
-- ========================================================
mm2Btn.Activated:Connect(function()
    closeLoader()
    pcall(function()
        if parentGui:FindFirstChild("VretaMM2Gui") then parentGui.VretaMM2Gui:Destroy() end
        local gui = Instance.new("ScreenGui")
        gui.Name = "VretaMM2Gui"
        gui.ResetOnSpawn = false
        gui.Parent = parentGui

        local main = Instance.new("Frame")
        main.Size = UDim2.new(0, 290, 0, 220)
        main.Position = UDim2.new(0.5, -145, 0.3, -110)
        main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
        main.Active = true
        main.Draggable = true
        main.Parent = gui

        Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)
        local stroke = Instance.new("UIStroke", main)
        stroke.Color = Color3.fromRGB(175, 115, 255)
        stroke.Thickness = 1.5

        local topBar = Instance.new("Frame", main)
        topBar.Size = UDim2.new(1, 0, 0, 32)
        topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
        Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 10)

        local title = Instance.new("TextLabel", topBar)
        title.Size = UDim2.new(1, -40, 1, 0)
        title.Position = UDim2.new(0, 10, 0, 0)
        title.BackgroundTransparency = 1
        title.Text = "🔪 MM2 — VRETA HUB ⚡"
        title.TextColor3 = Color3.fromRGB(200, 150, 255)
        title.TextSize = 10
        title.Font = Enum.Font.GothamBold
        title.TextXAlignment = Enum.TextXAlignment.Left

        local closeBtn = Instance.new("TextButton", topBar)
        closeBtn.Size = UDim2.new(0, 22, 0, 22)
        closeBtn.Position = UDim2.new(1, -27, 0, 5)
        closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
        closeBtn.Text = "×"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Activated:Connect(function() gui:Destroy() end)

        local container = Instance.new("ScrollingFrame", main)
        container.Size = UDim2.new(0.92, 0, 1, -40)
        container.Position = UDim2.new(0.04, 0, 0, 36)
        container.BackgroundTransparency = 1
        container.ScrollBarThickness = 2
        container.AutomaticCanvasSize = Enum.AutomaticSize.Y

        local layout = Instance.new("UIListLayout", container)
        layout.Padding = UDim.new(0, 6)

        local function addBtn(txt, cb)
            local b = Instance.new("TextButton", container)
            b.Size = UDim2.new(1, 0, 0, 32)
            b.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
            b.Text = txt
            b.TextColor3 = Color3.fromRGB(255, 255, 255)
            b.Font = Enum.Font.GothamBold
            b.TextSize = 10
            Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
            b.Activated:Connect(cb)
        end

        addBtn("👁️ ESP Sheriff & Murderer", function()
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character and not p.Character:FindFirstChild("VretaMM2HL") then
                    local hl = Instance.new("Highlight")
                    hl.Name = "VretaMM2HL"
                    hl.Adornee = p.Character
                    hl.FillColor = Color3.fromRGB(255, 0, 0)
                    hl.Parent = p.Character
                end
            end
        end)
    end)
end)

-- ========================================================
-- NATIVE MODULE 2: WASTE TIME
-- ========================================================
wasteTimeBtn.Activated:Connect(function()
    closeLoader()
    pcall(function()
        if parentGui:FindFirstChild("VretaWasteTimeGui") then parentGui.VretaWasteTimeGui:Destroy() end
        local gui = Instance.new("ScreenGui", parentGui)
        gui.Name = "VretaWasteTimeGui"
        gui.ResetOnSpawn = false

        local main = Instance.new("Frame", gui)
        main.Size = UDim2.new(0, 290, 0, 220)
        main.Position = UDim2.new(0.5, -145, 0.3, -110)
        main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
        main.Active = true
        main.Draggable = true
        Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)
        local stroke = Instance.new("UIStroke", main)
        stroke.Color = Color3.fromRGB(175, 115, 255)
        stroke.Thickness = 1.5

        local topBar = Instance.new("Frame", main)
        topBar.Size = UDim2.new(1, 0, 0, 32)
        topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
        Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 10)

        local title = Instance.new("TextLabel", topBar)
        title.Size = UDim2.new(1, -40, 1, 0)
        title.Position = UDim2.new(0, 10, 0, 0)
        title.BackgroundTransparency = 1
        title.Text = "⏳ WASTE TIME — VRETA HUB ⚡"
        title.TextColor3 = Color3.fromRGB(200, 150, 255)
        title.TextSize = 10
        title.Font = Enum.Font.GothamBold
        title.TextXAlignment = Enum.TextXAlignment.Left

        local closeBtn = Instance.new("TextButton", topBar)
        closeBtn.Size = UDim2.new(0, 22, 0, 22)
        closeBtn.Position = UDim2.new(1, -27, 0, 5)
        closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
        closeBtn.Text = "×"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Activated:Connect(function() gui:Destroy() end)

        local container = Instance.new("ScrollingFrame", main)
        container.Size = UDim2.new(0.92, 0, 1, -40)
        container.Position = UDim2.new(0.04, 0, 0, 36)
        container.BackgroundTransparency = 1
        container.ScrollBarThickness = 2
        container.AutomaticCanvasSize = Enum.AutomaticSize.Y

        local layout = Instance.new("UIListLayout", container)
        layout.Padding = UDim.new(0, 6)

        local function addBtn(txt, cb)
            local b = Instance.new("TextButton", container)
            b.Size = UDim2.new(1, 0, 0, 32)
            b.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
            b.Text = txt
            b.TextColor3 = Color3.fromRGB(255, 255, 255)
            b.Font = Enum.Font.GothamBold
            b.TextSize = 10
            Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
            b.Activated:Connect(cb)
        end

        addBtn("⚡ Auto Farm / Speed Boost", function()
            local char = LocalPlayer.Character
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if hum then hum.WalkSpeed = 100 end
        end)
    end)
end)

-- ========================================================
-- NATIVE MODULE 3: +1 TONGUE ESCAPE
-- ========================================================
tongueEscapeBtn.Activated:Connect(function()
    closeLoader()
    pcall(function()
        if parentGui:FindFirstChild("VretaTongueGui") then parentGui.VretaTongueGui:Destroy() end

        local gui = Instance.new("ScreenGui", parentGui)
        gui.Name = "VretaTongueGui"
        gui.ResetOnSpawn = false

        local main = Instance.new("Frame", gui)
        main.Size = UDim2.new(0, 360, 0, 260)
        main.Position = UDim2.new(0.5, -180, 0.35, -130)
        main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
        main.Active = true
        main.Draggable = true
        Instance.new("UICorner", main).CornerRadius = UDim.new(0, 12)
        local stroke = Instance.new("UIStroke", main)
        stroke.Color = Color3.fromRGB(175, 115, 255)
        stroke.Thickness = 1.5

        local topBar = Instance.new("Frame", main)
        topBar.Size = UDim2.new(1, 0, 0, 38)
        topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
        Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 12)

        local title = Instance.new("TextLabel", topBar)
        title.Size = UDim2.new(1, -50, 1, 0)
        title.Position = UDim2.new(0, 12, 0, 0)
        title.BackgroundTransparency = 1
        title.Text = "👅 VRETA HUB — +1 TONGUE ESCAPE"
        title.TextColor3 = Color3.fromRGB(200, 150, 255)
        title.TextSize = 12
        title.Font = Enum.Font.GothamBold
        title.TextXAlignment = Enum.TextXAlignment.Left

        local closeBtn = Instance.new("TextButton", topBar)
        closeBtn.Size = UDim2.new(0, 26, 0, 26)
        closeBtn.Position = UDim2.new(1, -32, 0, 6)
        closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
        closeBtn.Text = "×"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Activated:Connect(function() gui:Destroy() end)

        local container = Instance.new("ScrollingFrame", main)
        container.Size = UDim2.new(0.92, 0, 1, -50)
        container.Position = UDim2.new(0.04, 0, 0, 44)
        container.BackgroundTransparency = 1
        container.ScrollBarThickness = 3
        container.AutomaticCanvasSize = Enum.AutomaticSize.Y

        local layout = Instance.new("UIListLayout", container)
        layout.Padding = UDim.new(0, 8)

        local function addToggle(text, callback)
            local state = false
            local btn = Instance.new("TextButton", container)
            btn.Size = UDim2.new(1, 0, 0, 36)
            btn.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
            btn.Text = text .. ": OFF 🔴"
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 10
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)

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
            if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
        end

        local treadmillActive = false
        addToggle("⚡ 6x Treadmill", function(active)
            treadmillActive = active
            if active then
                local root = getRoot()
                if root then root.CFrame = CFrame.new(3189.3, 851.6, -563.5) end
                triggerJump()
                task.spawn(function()
                    while treadmillActive do
                        local currentRoot = getRoot()
                        if currentRoot then currentRoot.CFrame = CFrame.new(3189.3, 851.6, -563.5) end
                        task.wait(0.05)
                    end
                end)
            else
                triggerJump()
            end
        end)

        local winsActive = false
        addToggle("🏆 8K Wins", function(active)
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
    end)
end)

-- ========================================================
-- NATIVE MODULE 4: +1 BACKFLIP OBBY ESCAPE
-- ========================================================
backflipObbyBtn.Activated:Connect(function()
    closeLoader()
    pcall(function()
        if parentGui:FindFirstChild("VretaBackflipGui") then parentGui.VretaBackflipGui:Destroy() end

        local gui = Instance.new("ScreenGui", parentGui)
        gui.Name = "VretaBackflipGui"
        gui.ResetOnSpawn = false

        local main = Instance.new("Frame", gui)
        main.Size = UDim2.new(0, 290, 0, 240)
        main.Position = UDim2.new(0.5, -145, 0.35, -120)
        main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
        main.Active = true
        main.Draggable = true
        Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)
        local stroke = Instance.new("UIStroke", main)
        stroke.Color = Color3.fromRGB(175, 115, 255)
        stroke.Thickness = 1.5

        local topBar = Instance.new("Frame", main)
        topBar.Size = UDim2.new(1, 0, 0, 32)
        topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
        Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 10)

        local title = Instance.new("TextLabel", topBar)
        title.Size = UDim2.new(1, -40, 1, 0)
        title.Position = UDim2.new(0, 10, 0, 0)
        title.BackgroundTransparency = 1
        title.Text = "🤸‍♂️ +1 BACKFLIP OBBY ESCAPE ⚡"
        title.TextColor3 = Color3.fromRGB(200, 150, 255)
        title.TextSize = 10
        title.Font = Enum.Font.GothamBold
        title.TextXAlignment = Enum.TextXAlignment.Left

        local closeBtn = Instance.new("TextButton", topBar)
        closeBtn.Size = UDim2.new(0, 22, 0, 22)
        closeBtn.Position = UDim2.new(1, -27, 0, 5)
        closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
        closeBtn.Text = "×"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Activated:Connect(function() gui:Destroy() end)

        local container = Instance.new("ScrollingFrame", main)
        container.Size = UDim2.new(0.92, 0, 1, -40)
        container.Position = UDim2.new(0.04, 0, 0, 36)
        container.BackgroundTransparency = 1
        container.ScrollBarThickness = 2
        container.AutomaticCanvasSize = Enum.AutomaticSize.Y

        local layout = Instance.new("UIListLayout", container)
        layout.Padding = UDim.new(0, 5)

        local function addToggle(text, callback)
            local state = false
            local btn = Instance.new("TextButton", container)
            btn.Size = UDim2.new(1, 0, 0, 30)
            btn.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
            btn.Text = text .. ": OFF 🔴"
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 9
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

            btn.Activated:Connect(function()
                state = not state
                btn.BackgroundColor3 = state and Color3.fromRGB(140, 75, 230) or Color3.fromRGB(30, 30, 42)
                btn.Text = text .. ": " .. (state and "ON 🟢" or "OFF 🔴")
                callback(state)
            end)
            return btn
        end

        local function getRoot()
            local char = LocalPlayer.Character
            return char and char:FindFirstChild("HumanoidRootPart")
        end

        local function triggerJump()
            local char = LocalPlayer.Character
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
        end

        local winsActive = false
        addToggle("🏆 5K Wins (300 Speed Slide)", function(active)
            winsActive = active
            if active then
                task.spawn(function()
                    local toggleState = false
                    while winsActive do
                        local root = getRoot()
                        if root then
                            local targetPos = toggleState and Vector3.new(21178.0, 62.6, -772.0) or Vector3.new(21174.0, 62.6, -772.0)
                            root.CFrame = CFrame.new(targetPos)
                            toggleState = not toggleState
                        end
                        task.wait(300 / 10000)
                    end
                end)
            end
        end)

        local farmSpeedActive = false
        addToggle("⚡ Farm Speed", function(active)
            farmSpeedActive = active
            if active then
                task.spawn(function()
                    local step = false
                    while farmSpeedActive do
                        local root = getRoot()
                        if root then
                            local targetPos = step and Vector3.new(828.9, 63.6, -758.4) or Vector3.new(832.4, 63.6, -673.4)
                            root.CFrame = CFrame.new(targetPos)
                            step = not step
                        end
                        task.wait(0.1)
                    end
                end)
            end
        end)

        local function addTreadmill(name, pos)
            local state = false
            addToggle(name, function(active)
                state = active
                if active then
                    local root = getRoot()
                    if root then root.CFrame = CFrame.new(pos) end
                    triggerJump()
                    task.spawn(function()
                        while state do
                            local r = getRoot()
                            if r then r.CFrame = CFrame.new(pos) end
                            task.wait(0.05)
                        end
                    end)
                end
            end)
        end

        addTreadmill("🏃‍♂️ 2x Treadmill", Vector3.new(788.2, 64.9, -699.5))
        addTreadmill("🏃‍♂️ 1.5x Treadmill", Vector3.new(787.8, 64.9, -688.7))
        addTreadmill("🏃‍♂️ 1x Treadmill", Vector3.new(788.8, 64.9, -678.2))
    end)
end)

-- ========================================================
-- NATIVE MODULE 5: TROLL TOWER X
-- ========================================================
trollTowerBtn.Activated:Connect(function()
    closeLoader()
    pcall(function()
        if parentGui:FindFirstChild("VretaTrollTowerGui") then parentGui.VretaTrollTowerGui:Destroy() end

        local gui = Instance.new("ScreenGui", parentGui)
        gui.Name = "VretaTrollTowerGui"
        gui.ResetOnSpawn = false

        local main = Instance.new("Frame", gui)
        main.Size = UDim2.new(0, 310, 0, 350)
        main.Position = UDim2.new(0.5, -155, 0.3, -175)
        main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
        main.Active = true
        main.Draggable = true
        Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)
        local stroke = Instance.new("UIStroke", main)
        stroke.Color = Color3.fromRGB(175, 115, 255)
        stroke.Thickness = 1.5

        local topBar = Instance.new("Frame", main)
        topBar.Size = UDim2.new(1, 0, 0, 32)
        topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
        Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 10)

        local title = Instance.new("TextLabel", topBar)
        title.Size = UDim2.new(1, -40, 1, 0)
        title.Position = UDim2.new(0, 10, 0, 0)
        title.BackgroundTransparency = 1
        title.Text = "🗼 TROLL TOWER X — VRETA ⚡"
        title.TextColor3 = Color3.fromRGB(200, 150, 255)
        title.TextSize = 10
        title.Font = Enum.Font.GothamBold
        title.TextXAlignment = Enum.TextXAlignment.Left

        local closeBtn = Instance.new("TextButton", topBar)
        closeBtn.Size = UDim2.new(0, 22, 0, 22)
        closeBtn.Position = UDim2.new(1, -27, 0, 5)
        closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
        closeBtn.Text = "×"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Activated:Connect(function() gui:Destroy() end)

        local container = Instance.new("ScrollingFrame", main)
        container.Size = UDim2.new(0.92, 0, 1, -40)
        container.Position = UDim2.new(0.04, 0, 0, 36)
        container.BackgroundTransparency = 1
        container.ScrollBarThickness = 2
        container.AutomaticCanvasSize = Enum.AutomaticSize.Y

        local layout = Instance.new("UIListLayout", container)
        layout.Padding = UDim.new(0, 6)

        local function getRoot()
            local char = LocalPlayer.Character
            return char and char:FindFirstChild("HumanoidRootPart")
        end

        local function getHumanoid()
            local char = LocalPlayer.Character
            return char and char:FindFirstChildOfClass("Humanoid")
        end

        local function addNotification(msg)
            local notif = Instance.new("TextLabel", container)
            notif.Size = UDim2.new(1, 0, 0, 28)
            notif.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
            notif.TextColor3 = Color3.fromRGB(255, 255, 255)
            notif.Text = "⚠️ " .. msg
            notif.Font = Enum.Font.GothamBold
            notif.TextSize = 9
            Instance.new("UICorner", notif).CornerRadius = UDim.new(0, 6)
            task.delay(2.5, function() if notif then notif:Destroy() end end)
        end

        local function addButton(text, callback)
            local btn = Instance.new("TextButton", container)
            btn.Size = UDim2.new(1, 0, 0, 32)
            btn.BackgroundColor3 = Color3.fromRGB(140, 75, 230)
            btn.Text = text
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 10
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
            btn.Activated:Connect(callback)
            return btn
        end

        local function addToggle(text, callback)
            local state = false
            local btn = Instance.new("TextButton", container)
            btn.Size = UDim2.new(1, 0, 0, 32)
            btn.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
            btn.Text = text .. ": OFF 🔴"
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 9
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

            btn.Activated:Connect(function()
                state = not state
                btn.BackgroundColor3 = state and Color3.fromRGB(140, 75, 230) or Color3.fromRGB(30, 30, 42)
                btn.Text = text .. ": " .. (state and "ON 🟢" or "OFF 🔴")
                callback(state)
            end)
            return btn
        end

        local function addTextBox(placeholder, callback)
            local box = Instance.new("TextBox", container)
            box.Size = UDim2.new(1, 0, 0, 32)
            box.BackgroundColor3 = Color3.fromRGB(36, 36, 50)
            box.PlaceholderText = placeholder
            box.Text = ""
            box.TextColor3 = Color3.fromRGB(255, 255, 255)
            box.PlaceholderColor3 = Color3.fromRGB(150, 150, 170)
            box.Font = Enum.Font.GothamMedium
            box.TextSize = 9
            Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)

            box.FocusLost:Connect(function(enterPressed)
                if enterPressed then callback(box.Text) end
            end)
            return box
        end

        addButton("🏆 Complete Tower (Instant Win)", function()
            local root = getRoot()
            if root then
                root.CFrame = CFrame.new(-13.9, 343.1, -173.8)
                addNotification("Teleported to Tower Completion!")
            end
        end)

        addTextBox("⚡ Set WalkSpeed", function(text)
            local val = tonumber(text)
            local hum = getHumanoid()
            if val and hum then hum.WalkSpeed = val addNotification("WalkSpeed: " .. val) end
        end)

        addTextBox("🦘 Set JumpPower", function(text)
            local val = tonumber(text)
            local hum = getHumanoid()
            if val and hum then hum.UseJumpPower = true hum.JumpPower = val addNotification("JumpPower: " .. val) end
        end)

        local flySpeedVal = 50
        addTextBox("✈️ Fly Speed (Default 50)", function(text)
            local val = tonumber(text)
            if val then flySpeedVal = val addNotification("Fly Speed set to: " .. val) end
        end)

        local flying = false
        addToggle("✈️ Enable Fly", function(active)
            flying = active
            if flying then
                task.spawn(function()
                    local bg = Instance.new("BodyGyro")
                    local bv = Instance.new("BodyVelocity")
                    bg.P = 9e4
                    bg.MaxTorque = Vector3.new(9e4, 9e4, 9e4)
                    bv.MaxForce = Vector3.new(9e4, 9e4, 9e4)
                    while flying do
                        local root = getRoot()
                        local camera = workspace.CurrentCamera
                        if root and camera then
                            if not bg.Parent then bg.Parent = root end
                            if not bv.Parent then bv.Parent = root end
                            bg.CFrame = camera.CFrame
                            bv.Velocity = camera.CFrame.LookVector * flySpeedVal
                        else
                            if bg.Parent then bg:Destroy() end
                            if bv.Parent then bv:Destroy() end
                        end
                        RunService.RenderStepped:Wait()
                    end
                    if bg.Parent then bg:Destroy() end
                    if bv.Parent then bv:Destroy() end
                end)
            end
        end)

        local infJumpActive = false
        local limit2Active = false
        addToggle("🚀 Infinite Jump", function(active) infJumpActive = active end)
        addToggle("🔒 Limit Infinite Jump 2", function(active)
            if active and not infJumpActive then
                addNotification("Please Enable Infinite Jump First!")
                return
            end
            limit2Active = active
        end)

        local jumpCounter = 0
        UserInputService.JumpRequest:Connect(function()
            if infJumpActive then
                local hum = getHumanoid()
                if hum then
                    if limit2Active then
                        if jumpCounter < 2 then
                            hum:ChangeState(Enum.HumanoidStateType.Jumping)
                            jumpCounter = jumpCounter + 1
                        end
                    else
                        hum:ChangeState(Enum.HumanoidStateType.Jumping)
                    end
                end
            end
        end)

        RunService.Heartbeat:Connect(function()
            local hum = getHumanoid()
            if hum and hum.FloorMaterial ~= Enum.Material.Air then jumpCounter = 0 end
        end)

        addToggle("🐰 Bunny Hop", function(active)
            task.spawn(function()
                while active do
                    local hum = getHumanoid()
                    if hum and hum.FloorMaterial ~= Enum.Material.Air then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
                    task.wait(0.1)
                end
            end)
        end)

        addToggle("🛡️ Anti-Fling", function(active)
            task.spawn(function()
                while active do
                    local root = getRoot()
                    if root then
                        root.AssemblyAngularVelocity = Vector3.new(0, 0, 0)
                        root.AssemblyLinearVelocity = Vector3.new(root.AssemblyLinearVelocity.X, 0, root.AssemblyLinearVelocity.Z)
                    end
                    RunService.Heartbeat:Wait()
                end
            end)
        end)

        addToggle("🛡️ Anti-Ragdoll", function(active)
            task.spawn(function()
                while active do
                    local hum = getHumanoid()
                    if hum then
                        hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
                        hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
                    end
                    task.wait(0.5)
                end
            end)
        end)

        addToggle("🛡️ Anti-Slip", function(active)
            task.spawn(function()
                while active do
                    local char = LocalPlayer.Character
                    if char then
                        for _, part in ipairs(char:GetDescendants()) do
                            if part:IsA("BasePart") then part.CustomPhysicalProperties = PhysicalProperties.new(1.0, 0, 0, 1.0, 1.0) end
                        end
                    end
                    task.wait(1)
                end
            end)
        end)

        addToggle("🛡️ Anti-Sit", function(active)
            task.spawn(function()
                while active do
                    local hum = getHumanoid()
                    if hum then hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false) end
                    task.wait(0.5)
                end
            end)
        end)

        addButton("🎁 Give All Items", function()
            for _, obj in ipairs(workspace:GetDescendants()) do
                if obj:IsA("Tool") then
                    local clone = obj:Clone()
                    clone.Parent = LocalPlayer.Backpack
                end
            end
            addNotification("All items cloned!")
        end)

        addButton("👑 Load Kohl's Admin (172732271)", function()
            pcall(function()
                local model = InsertService:LoadAsset(172732271)
                if model then
                    for _, child in ipairs(model:GetChildren()) do child.Parent = workspace end
                    addNotification("Kohl's Admin Loaded!")
                end
            end)
        end)
    end)
end)

-- ========================================================
-- NATIVE MODULE 6: +1 SPEED NINJA KEYBOARD ESCAPE
-- ========================================================
ninjaKeyboardBtn.Activated:Connect(function()
    closeLoader()
    pcall(function()
        if parentGui:FindFirstChild("VretaNinjaKeyboardGui") then parentGui.VretaNinjaKeyboardGui:Destroy() end

        local gui = Instance.new("ScreenGui", parentGui)
        gui.Name = "VretaNinjaKeyboardGui"
        gui.ResetOnSpawn = false

        local main = Instance.new("Frame", gui)
        main.Size = UDim2.new(0, 290, 0, 280)
        main.Position = UDim2.new(0.5, -145, 0.3, -140)
        main.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
        main.Active = true
        main.Draggable = true
        Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)
        local stroke = Instance.new("UIStroke", main)
        stroke.Color = Color3.fromRGB(175, 115, 255)
        stroke.Thickness = 1.5

        local topBar = Instance.new("Frame", main)
        topBar.Size = UDim2.new(1, 0, 0, 32)
        topBar.BackgroundColor3 = Color3.fromRGB(24, 24, 36)
        Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 10)

        local title = Instance.new("TextLabel", topBar)
        title.Size = UDim2.new(1, -40, 1, 0)
        title.Position = UDim2.new(0, 10, 0, 0)
        title.BackgroundTransparency = 1
        title.Text = "⌨️ +1 NINJA KEYBOARD ESCAPE ⚡"
        title.TextColor3 = Color3.fromRGB(200, 150, 255)
        title.TextSize = 9
        title.Font = Enum.Font.GothamBold
        title.TextXAlignment = Enum.TextXAlignment.Left

        local closeBtn = Instance.new("TextButton", topBar)
        closeBtn.Size = UDim2.new(0, 22, 0, 22)
        closeBtn.Position = UDim2.new(1, -27, 0, 5)
        closeBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 70)
        closeBtn.Text = "×"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Activated:Connect(function() gui:Destroy() end)

        local container = Instance.new("ScrollingFrame", main)
        container.Size = UDim2.new(0.92, 0, 1, -40)
        container.Position = UDim2.new(0.04, 0, 0, 36)
        container.BackgroundTransparency = 1
        container.ScrollBarThickness = 2
        container.AutomaticCanvasSize = Enum.AutomaticSize.Y

        local layout = Instance.new("UIListLayout", container)
        layout.Padding = UDim.new(0, 5)

        local function getRoot()
            local char = LocalPlayer.Character
            return char and char:FindFirstChild("HumanoidRootPart")
        end

        local function triggerJump()
            local char = LocalPlayer.Character
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
        end

        local function addToggle(text, callback)
            local state = false
            local btn = Instance.new("TextButton", container)
            btn.Size = UDim2.new(1, 0, 0, 30)
            btn.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
            btn.Text = text .. ": OFF 🔴"
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamMedium
            btn.TextSize = 9
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

            btn.Activated:Connect(function()
                state = not state
                btn.BackgroundColor3 = state and Color3.fromRGB(140, 75, 230) or Color3.fromRGB(30, 30, 42)
                btn.Text = text .. ": " .. (state and "ON 🟢" or "OFF 🔴")
                callback(state)
            end)
            return btn
        end

        local winsActive = false
        addToggle("🏆 Wins (Drop Loop)", function(active)
            winsActive = active
            if active then
                task.spawn(function()
                    while winsActive do
                        local root = getRoot()
                        if root then root.CFrame = CFrame.new(1.0, 37.0, -10262.4) end
                        task.wait(0.3)
                    end
                end)
            end
        end)

        local function addTreadmillToggle(name, targetPos)
            local activeState = false
            addToggle(name, function(active)
                activeState = active
                if active then
                    local root = getRoot()
                    if root then root.CFrame = CFrame.new(targetPos) end
                    triggerJump()
                    task.spawn(function()
                        while activeState do
                            local currentRoot = getRoot()
                            if currentRoot then currentRoot.CFrame = CFrame.new(targetPos) end
                            task.wait(0.05)
                        end
                    end)
                end
            end)
        end

        addTreadmillToggle("⚡ 100x Treadmill", Vector3.new(-69.6, 14.6, -21.9))
        addTreadmillToggle("⚡ 50x Treadmill", Vector3.new(-69.6, 11.8, -0.4))
        addTreadmillToggle("⚡ 25x Treadmill", Vector3.new(-69.6, 10.8, 21.7))
        addTreadmillToggle("⚡ 5x Treadmill", Vector3.new(-69.6, 9.9, 42.3))
        addTreadmillToggle("⚡ 2x Treadmill", Vector3.new(-69.6, 9.5, 61.8))

        local autoRebirthActive = false
        addToggle("🔄 Auto Rebirth", function(active)
            autoRebirthActive = active
            if active then
                task.spawn(function()
                    while autoRebirthActive do
                        pcall(function()
                            local remotes = game:GetService("ReplicatedStorage"):FindFirstChild("Remotes", true) or game:GetService("ReplicatedStorage")
                            for _, remote in ipairs(remotes:GetDescendants()) do
                                if remote:IsA("RemoteEvent") and (remote.Name:lower():find("rebirth") or remote.Name:lower():find("reborn")) then
                                    remote:FireServer()
                                end
                            end
                        end)
                        task.wait(1)
                    end
                end)
            end
        end)
    end)
end)

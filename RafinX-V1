local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "RafinX Beta",
    SubTitle = "Blade Ball",
    TabWidth = 180,
    Size = UDim2.fromOffset(450, 240),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.RightControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "Parry", Icon = "shield" }),
    Misc = Window:AddTab({ Title = "Misc", Icon = "component" }),
    Credits = Window:AddTab({ Title = "Credits", Icon = "copyright" })
}

local Options = Fluent.Options

function SwordCrateManual()
    game:GetService("ReplicatedStorage").Remote.RemoteFunction:InvokeServer("PromptPurchaseCrate", workspace.Spawn.Crates.NormalSwordCrate)
end

function ExplosionCrateManual()
    game:GetService("ReplicatedStorage").Remote.RemoteFunction:InvokeServer("PromptPurchaseCrate", workspace.Spawn.Crates.NormalExplosionCrate)
end

function SwordCrateAuto()
    while _G.AutoSword do
        game:GetService("ReplicatedStorage").Remote.RemoteFunction:InvokeServer("PromptPurchaseCrate", workspace.Spawn.Crates.NormalSwordCrate)
        wait(1)
    end
end

function ExplosionCrateAuto()
    while _G.AutoBoom do
        game:GetService("ReplicatedStorage").Remote.RemoteFunction:InvokeServer("PromptPurchaseCrate", workspace.Spawn.Crates.NormalExplosionCrate)
        wait(1)
    end
end

do
    Fluent:Notify({
        Title = "RafinX has injected",
        Content = "report any bugs at https://discord.com/invite/rftEeG3hFM",
        SubContent = "or copy the link through the 'Misc' page",
        Duration = 3.5
    })
end

local Get = Tabs.Main:AddSection("Updates")

local Mainy = Tabs.Main:AddSection("Automation")

local autoParryConnection
local Toggle = Mainy:AddToggle("Parry", {Title = "Toggle Auto Parry", Default = false })
Toggle:OnChanged(function(Value)
    local RunService = game:GetService("RunService")
    local Players = game:GetService("Players")
    local Workspace = game:GetService("Workspace")
    local VirtualInputManager = game:GetService("VirtualInputManager")
    local BallFolder = Workspace:WaitForChild("Balls")

    local player = Players.LocalPlayer
    local canParry = true

    local function calculatePredictionTime(ball, player)
        local character = player.Character
        if character then
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            if rootPart then
                local relativePosition = ball.Position - rootPart.Position
                local relativeVelocity = ball.Velocity - rootPart.Velocity
                local a = ball.Size.Magnitude / 1
                local b = relativePosition.Magnitude
                local c = math.sqrt(a * a + b * b)
                return (c - a) / relativeVelocity.Magnitude
            end
        end
        return math.huge
    end

    local function parry()
        if canParry then
            canParry = false
            VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 0)
            VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 0)
            task.delay(0.1, function()
                canParry = true
            end)
        end
    end

    local function checkProximityToPlayer(ball, player)
        local predictionTime = calculatePredictionTime(ball, player)
        local realBallAttribute = ball:GetAttribute("realBall")
        local target = ball:GetAttribute("target")

        if predictionTime and realBallAttribute and target then
            local ballSpeedThreshold = math.max(0.4, 0.6 - ball.Velocity.Magnitude * 0.03)
            if predictionTime <= ballSpeedThreshold and realBallAttribute and target == player.Name then
                parry()
            end
        end
    end

    local function checkBallsProximity()
        if player and player.Character then
            for _, ball in ipairs(BallFolder:GetChildren()) do
                if ball:IsA("BasePart") then
                    checkProximityToPlayer(ball, player)
                end
            end
        end
    end

    if Value then
        autoParryConnection = RunService.Heartbeat:Connect(checkBallsProximity)
    else
        if autoParryConnection then
            autoParryConnection:Disconnect()
            autoParryConnection = nil
        end
    end
end)

Mainy:AddButton({
    Title = "Toggle Spam Parry GUI",
    Description = "Version 1.0.0",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/nqxlOfc/SlzAX17vGCub7iRKVmJid61Bg/main/KwKVzV5SgcFBd9fnpLr4lKCg6.lua"))()
    end
})

local Money = Tabs.Main:AddSection("Money Features")

Money:AddButton({
    Title = "Open Sword Crate",
    Description = "will open one common sword crate",
    Callback = function()
        SwordCrateManual()
    end
})

Money:AddButton({
    Title = "Open Explosion Crate",
    Description = "will open one common explosion crate",
    Callback = function()
        ExplosionCrateManual()
    end
})

local Toggle = Money:AddToggle("SwordAuto", {Title = "Auto Open Sword Crate", Default = false })
Toggle:OnChanged(function(Value)
    _G.AutoSword = Value
    SwordCrateAuto()
end)

local Toggle = Money:AddToggle("ExplosionAuto", {Title = "Auto Open Explosion Crate", Default = false })
Toggle:OnChanged(function(Value)
    _G.AutoBoom = Value
    ExplosionCrateAuto()
end)

local Misc = Tabs.Misc:AddSection("Misc")

Misc:AddParagraph({
    Title = "Coming Soon ",
    Content = "Coming soon on next update"
})

local Credits = Tabs.Credits:AddSection("Credits")

Credits:AddParagraph({
    Title = "Credits",
    Content = "Script was made by Rafin"
})

Credits:AddButton({
    Title = "Copy Discord Link",
    Description = "https://discord.com/invite/rftEeG3hFM",
    Callback = function()
        setclipboard("https://discord.com/invite/rftEeG3hFM")
    end
})

local Showcase = Tabs.Credits:AddSection("Awesome Showcasers")

Showcase:AddParagraph({
    Title = "None yet",
    Content = ""
})

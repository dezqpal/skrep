local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/BASTOSaKO/Privatezz/refs/heads/main/Sbhshsh", true))()

local window = library:AddWindow("YESHI •|•PRIVATE SCRIPT", {
    main_color = Color3.fromRGB(0, 0, 0),
    min_size = Vector2.new(520, 620),
})
 
local Main = window:AddTab("Main")
 
        Main:AddLabel("Local Player")
        local walkSpeedValue = 16
        Main:AddTextBox("WalkSpeed", function(text)
            local speed = tonumber(text)
            if speed and speed >= 1 and speed <= 500 then
                walkSpeedValue = speed
            end
        end)
 
        local setSpeed = false
        Main:AddSwitch("Set Speed", function(state)
            setSpeed = state
            while setSpeed do
                game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = walkSpeedValue
                task.wait(0.1)
            end
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
        end)
 
        local jumpPowerValue = 50
        Main:AddTextBox("JumpPower", function(text)
            local jump = tonumber(text)
            if jump then
                jumpPowerValue = jump
            end
        end)
 
        local applyJumpPower = false
        Main:AddSwitch("Apply JumpPower", function(state)
            applyJumpPower = state
            local humanoid = game.Players.LocalPlayer.Character.Humanoid
            humanoid.UseJumpPower = true
            humanoid.JumpPower = applyJumpPower and jumpPowerValue or 50
        end)
 
        local sizeValue = 1
        Main:AddTextBox("Size", function(text)
            local size = tonumber(text)
            if size and size >= 1 and size <= 100 then
                sizeValue = size
            end
        end)
 
        local setSize = false
        Main:AddSwitch("Set Sizes", function(state)
            setSize = state
            local char = game.Players.LocalPlayer.Character
            if char then
                local humanoid = char:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    for _, scale in pairs({"BodyDepthScale", "BodyHeightScale", "BodyWidthScale", "HeadScale"}) do
                        local scaleInstance = humanoid:FindFirstChild(scale)
                        if scaleInstance then
                            scaleInstance.Value = state and sizeValue or 1
                        end
                    end
                end
            end
        end)
 
local AutoFarm = window:AddTab("Auto Farm")
 
local folder1 = AutoFarm:AddFolder("Tools")
 
local weightOn = false
folder1:AddSwitch("Weight", function(bool)
    weightOn = bool
    task.spawn(function()
        while weightOn do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Weight")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
            end
            task.wait(0.1)
        end
    end)
end)
 
local pushupsOn = false
folder1:AddSwitch("Pushups", function(bool)
    pushupsOn = bool
    task.spawn(function()
        while pushupsOn do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Pushups")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
            end
            task.wait(0.1)
        end
    end)
end)
 
local handstandOn = false
folder1:AddSwitch("Handstand", function(bool)
    handstandOn = bool
    task.spawn(function()
        while handstandOn do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Handstand")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
            end
            task.wait(0.1)
        end
    end)
end)
 
local situpsOn = false
folder1:AddSwitch("Situps", function(bool)
    situpsOn = bool
    task.spawn(function()
        while situpsOn do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Situps")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
            end
            task.wait(0.1)
        end
    end)
end)
 
local folder = Main:AddFolder("Rock Farming")
 
local function gettool()
    for i, v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
        if v.Name == "Punch" and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid:EquipTool(v)
        end
    end
    game:GetService("Players").LocalPlayer.muscleEvent:FireServer("punch", "leftHand")
    game:GetService("Players").LocalPlayer.muscleEvent:FireServer("punch", "rightHand")
end
 
local tinyIslandRockSwitch = folder:AddSwitch("Farm Tiny Island Rock", function(bool)
    selectrock = "Tiny Island Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 0 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 0 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local starterIslandRockSwitch = folder:AddSwitch("Farm Starter Island Rock", function(bool)
    selectrock = "Starter Island Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 100 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 100 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local legendBeachRockSwitch = folder:AddSwitch("Farm Legend Beach Rock", function(bool)
    selectrock = "Legend Beach Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 5000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 5000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local frostGymRockSwitch = folder:AddSwitch("Farm Frost Gym Rock", function(bool)
    selectrock = "Frost Gym Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 150000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 150000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local mythicalGymRockSwitch = folder:AddSwitch("Farm Mythical Gym Rock", function(bool)
    selectrock = "Mythical Gym Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 400000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 400000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local eternalGymRockSwitch = folder:AddSwitch("Farm Eternal Gym Rock", function(bool)
    selectrock = "Eternal Gym Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 750000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 750000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local legendGymRockSwitch = folder:AddSwitch("Farm Legend Gym Rock", function(bool)
    selectrock = "Legend Gym Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 1000000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 1000000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local muscleKingGymRockSwitch = folder:AddSwitch("Farm Muscle King Gym Rock", function(bool)
    selectrock = "Muscle King Gym Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 5000000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 5000000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
local ancientJungleRockSwitch = folder:AddSwitch("Farm Ancient Jungle Rock", function(bool)
    selectrock = "Ancient Jungle Rock"
    getgenv().autoFarm = bool
 
    if bool then
        spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if game:GetService("Players").LocalPlayer.Durability.Value >= 10000000 then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == 10000000 and 
                           game.Players.LocalPlayer.Character:FindFirstChild("LeftHand") and 
                           game.Players.LocalPlayer.Character:FindFirstChild("RightHand") then
 
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, game:GetService("Players").LocalPlayer.Character.LeftHand, 1)
                            gettool()
                        end
                    end
                end
            end
        end)
    end
end)
 
 
getgenv().StarterIslandBenchPress = false
getgenv().StarterIslandSquat = false
getgenv().StarterIslandDeadlift = false
getgenv().StarterIslandPullUp = false
getgenv().StarterIslandBoulder = false
 
getgenv().LegendBeachBenchPress = false
getgenv().LegendBeachSquat = false
getgenv().LegendBeachDeadlift = false
getgenv().LegendBeachPullUp = false
getgenv().LegendBeachBoulder = false
 
getgenv().FrostGymBenchPress = false
getgenv().FrostGymSquat = false
getgenv().FrostGymDeadlift = false
getgenv().FrostGymPullUp = false
getgenv().FrostGymBoulder = false
 
getgenv().MythicalGymBenchPress = false
getgenv().MythicalGymSquat = false
getgenv().MythicalGymDeadlift = false
getgenv().MythicalGymPullUp = false
getgenv().MythicalGymBoulder = false
 
getgenv().EternalGymBenchPress = false
getgenv().EternalGymSquat = false
getgenv().EternalGymDeadlift = false
getgenv().EternalGymPullUp = false
getgenv().EternalGymBoulder = false
 
getgenv().LegendGymBenchPress = false
getgenv().LegendGymSquat = false
getgenv().LegendGymDeadlift = false
getgenv().LegendGymPullUp = false
getgenv().LegendGymBoulder = false
 
getgenv().MuscleKingGymBenchPress = false
getgenv().MuscleKingGymSquat = false
getgenv().MuscleKingGymDeadlift = false
getgenv().MuscleKingGymPullUp = false
getgenv().MuscleKingGymBoulder = false
 
getgenv().JungleGymBenchPress = false
getgenv().JungleGymSquat = false
getgenv().JungleGymDeadlift = false
getgenv().JungleGymPullUp = false
getgenv().JungleGymBoulder = false
 
local positions = {
    StarterIsland = {
        BenchPress = CFrame.new(-17.0609932, 3.31417918, -2.48164988),
        Squat = CFrame.new(-48.8711243, 3.31417918, -11.8831778),
        Deadlift = CFrame.new(-48.8711243, 3.31417918, -11.8831778),
        PullUp = CFrame.new(-33.3047485, 3.31417918, -11.8831778),
        Boulder = CFrame.new(-33.3047485, 3.31417918, -11.8831778)
    },
    LegendBeach = {
        BenchPress = CFrame.new(470.334656, 3.31417966, -321.053925),
        Squat = CFrame.new(470.334656, 3.31417966, -321.053925),
        Deadlift = CFrame.new(470.334656, 3.31417966, -321.053925),
        PullUp = CFrame.new(470.334656, 3.31417966, -321.053925),
        Boulder = CFrame.new(470.334656, 3.31417966, -321.053925)
    },
    FrostGym = {
        BenchPress = CFrame.new(-3013.24194, 39.2158546, -335.036926),
        Squat = CFrame.new(-2933.47998, 29.6399612, -579.946045),
        Deadlift = CFrame.new(-2933.47998, 29.6399612, -579.946045),
        PullUp = CFrame.new(-2933.47998, 29.6399612, -579.946045),
        Boulder = CFrame.new(-2933.47998, 29.6399612, -579.946045)
    },
    MythicalGym = {
        BenchPress = CFrame.new(2371.7356, 39.2158546, 1246.31555),
        Squat = CFrame.new(2489.21484, 3.67686629, 849.051025),
        Deadlift = CFrame.new(2489.21484, 3.67686629, 849.051025),
        PullUp = CFrame.new(2489.21484, 3.67686629, 849.051025),
        Boulder = CFrame.new(2489.21484, 3.67686629, 849.051025)
    },
    EternalGym = {
        BenchPress = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        Squat = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        Deadlift = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        PullUp = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        Boulder = CFrame.new(-7176.19141, 45.394104, -1106.31421)
    },
    LegendGym = {
        BenchPress = CFrame.new(4111.91748, 1020.46674, -3799.97217),
        Squat = CFrame.new(4304.99023, 987.829956, -4124.2334),
        Deadlift = CFrame.new(4304.99023, 987.829956, -4124.2334),
        PullUp = CFrame.new(4304.99023, 987.829956, -4124.2334),
        Boulder = CFrame.new(4304.99023, 987.829956, -4124.2334)
    },
    MuscleKingGym = {
        BenchPress = CFrame.new(-8590.06152, 46.0167427, -6043.34717),
        Squat = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
        Deadlift = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
        PullUp = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
        Boulder = CFrame.new(-8940.12402, 13.1642084, -5699.13477)
    },
    JungleGym = {
        BenchPress = CFrame.new(-8173, 64, 1898),
        Squat = CFrame.new(-8352, 34, 2878),
        Deadlift = CFrame.new(-8352, 34, 2878),
        PullUp = CFrame.new(-8666, 34, 2070),
        Boulder = CFrame.new(-8621, 34, 2684)
    }
}
 
function teleportLoop(key)
    task.spawn(function()
        while getgenv()[key] do
            local parts = {}
            for loc, workouts in pairs(positions) do
                for workout, cf in pairs(workouts) do
                    if key == loc .. workout then
                        table.insert(parts, cf)
                    end
                end
            end
            if #parts > 0 and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = parts[1]
            end
            task.wait(0.1)
        end
    end)
end
 
local StarterIsland = AutoFarm:AddFolder("Starter Island")
StarterIsland:AddSwitch("Bench Press", function(b) getgenv().StarterIslandBenchPress = b teleportLoop("StarterIslandBenchPress") end)
StarterIsland:AddSwitch("Squat", function(b) getgenv().StarterIslandSquat = b teleportLoop("StarterIslandSquat") end)
StarterIsland:AddSwitch("Deadlift", function(b) getgenv().StarterIslandDeadlift = b teleportLoop("StarterIslandDeadlift") end)
StarterIsland:AddSwitch("Pull Up", function(b) getgenv().StarterIslandPullUp = b teleportLoop("StarterIslandPullUp") end)
StarterIsland:AddSwitch("Boulder", function(b) getgenv().StarterIslandBoulder = b teleportLoop("StarterIslandBoulder") end)
StarterIsland:AddSwitch("All Off", function()
    getgenv().StarterIslandBenchPress = false getgenv().StarterIslandSquat = false getgenv().StarterIslandDeadlift = false getgenv().StarterIslandPullUp = false getgenv().StarterIslandBoulder = false
end)
 
local LegendBeach = AutoFarm:AddFolder("Legend Beach")
LegendBeach:AddSwitch("Bench Press", function(b) getgenv().LegendBeachBenchPress = b teleportLoop("LegendBeachBenchPress") end)
LegendBeach:AddSwitch("Squat", function(b) getgenv().LegendBeachSquat = b teleportLoop("LegendBeachSquat") end)
LegendBeach:AddSwitch("Deadlift", function(b) getgenv().LegendBeachDeadlift = b teleportLoop("LegendBeachDeadlift") end)
LegendBeach:AddSwitch("Pull Up", function(b) getgenv().LegendBeachPullUp = b teleportLoop("LegendBeachPullUp") end)
LegendBeach:AddSwitch("Boulder", function(b) getgenv().LegendBeachBoulder = b teleportLoop("LegendBeachBoulder") end)
LegendBeach:AddSwitch("All Off", function()
    getgenv().LegendBeachBenchPress = false getgenv().LegendBeachSquat = false getgenv().LegendBeachDeadlift = false getgenv().LegendBeachPullUp = false getgenv().LegendBeachBoulder = false
end)
 
local FrostGym = AutoFarm:AddFolder("Frost Gym")
FrostGym:AddSwitch("Bench Press", function(b) getgenv().FrostGymBenchPress = b teleportLoop("FrostGymBenchPress") end)
FrostGym:AddSwitch("Squat", function(b) getgenv().FrostGymSquat = b teleportLoop("FrostGymSquat") end)
FrostGym:AddSwitch("Deadlift", function(b) getgenv().FrostGymDeadlift = b teleportLoop("FrostGymDeadlift") end)
FrostGym:AddSwitch("Pull Up", function(b) getgenv().FrostGymPullUp = b teleportLoop("FrostGymPullUp") end)
FrostGym:AddSwitch("Boulder", function(b) getgenv().FrostGymBoulder = b teleportLoop("FrostGymBoulder") end)
FrostGym:AddSwitch("All Off", function()
    getgenv().FrostGymBenchPress = false getgenv().FrostGymSquat = false getgenv().FrostGymDeadlift = false getgenv().FrostGymPullUp = false getgenv().FrostGymBoulder = false
end)
 
local MythicalGym = AutoFarm:AddFolder("Mythical Gym")
MythicalGym:AddSwitch("Bench Press", function(b) getgenv().MythicalGymBenchPress = b teleportLoop("MythicalGymBenchPress") end)
MythicalGym:AddSwitch("Squat", function(b) getgenv().MythicalGymSquat = b teleportLoop("MythicalGymSquat") end)
MythicalGym:AddSwitch("Deadlift", function(b) getgenv().MythicalGymDeadlift = b teleportLoop("MythicalGymDeadlift") end)
MythicalGym:AddSwitch("Pull Up", function(b) getgenv().MythicalGymPullUp = b teleportLoop("MythicalGymPullUp") end)
MythicalGym:AddSwitch("Boulder", function(b) getgenv().MythicalGymBoulder = b teleportLoop("MythicalGymBoulder") end)
MythicalGym:AddSwitch("All Off", function()
    getgenv().MythicalGymBenchPress = false getgenv().MythicalGymSquat = false getgenv().MythicalGymDeadlift = false getgenv().MythicalGymPullUp = false getgenv().MythicalGymBoulder = false
end)
 
local EternalGym = AutoFarm:AddFolder("Eternal Gym")
EternalGym:AddSwitch("Bench Press", function(b) getgenv().EternalGymBenchPress = b teleportLoop("EternalGymBenchPress") end)
EternalGym:AddSwitch("Squat", function(b) getgenv().EternalGymSquat = b teleportLoop("EternalGymSquat") end)
EternalGym:AddSwitch("Deadlift", function(b) getgenv().EternalGymDeadlift = b teleportLoop("EternalGymDeadlift") end)
EternalGym:AddSwitch("Pull Up", function(b) getgenv().EternalGymPullUp = b teleportLoop("EternalGymPullUp") end)
EternalGym:AddSwitch("Boulder", function(b) getgenv().EternalGymBoulder = b teleportLoop("EternalGymBoulder") end)
EternalGym:AddSwitch("All Off", function()
    getgenv().EternalGymBenchPress = false getgenv().EternalGymSquat = false getgenv().EternalGymDeadlift = false getgenv().EternalGymPullUp = false getgenv().EternalGymBoulder = false
end)
 
local LegendGym = AutoFarm:AddFolder("Legend Gym")
LegendGym:AddSwitch("Bench Press", function(b) getgenv().LegendGymBenchPress = b teleportLoop("LegendGymBenchPress") end)
LegendGym:AddSwitch("Squat", function(b) getgenv().LegendGymSquat = b teleportLoop("LegendGymSquat") end)
LegendGym:AddSwitch("Deadlift", function(b) getgenv().LegendGymDeadlift = b teleportLoop("LegendGymDeadlift") end)
LegendGym:AddSwitch("Pull Up", function(b) getgenv().LegendGymPullUp = b teleportLoop("LegendGymPullUp") end)
LegendGym:AddSwitch("Boulder", function(b) getgenv().LegendGymBoulder = b teleportLoop("LegendGymBoulder") end)
LegendGym:AddSwitch("All Off", function()
    getgenv().LegendGymBenchPress = false getgenv().LegendGymSquat = false getgenv().LegendGymDeadlift = false getgenv().LegendGymPullUp = false getgenv().LegendGymBoulder = false
end)
 
local MuscleKingGym = AutoFarm:AddFolder("Muscle King Gym")
MuscleKingGym:AddSwitch("Bench Press", function(b) getgenv().MuscleKingGymBenchPress = b teleportLoop("MuscleKingGymBenchPress") end)
MuscleKingGym:AddSwitch("Squat", function(b) getgenv().MuscleKingGymSquat = b teleportLoop("MuscleKingGymSquat") end)
MuscleKingGym:AddSwitch("Deadlift", function(b) getgenv().MuscleKingGymDeadlift = b teleportLoop("MuscleKingGymDeadlift") end)
MuscleKingGym:AddSwitch("Pull Up", function(b) getgenv().MuscleKingGymPullUp = b teleportLoop("MuscleKingGymPullUp") end)
MuscleKingGym:AddSwitch("Boulder", function(b) getgenv().MuscleKingGymBoulder = b teleportLoop("MuscleKingGymBoulder") end)
MuscleKingGym:AddSwitch("All Off", function()
    getgenv().MuscleKingGymBenchPress = false getgenv().MuscleKingGymSquat = false getgenv().MuscleKingGymDeadlift = false getgenv().MuscleKingGymPullUp = false getgenv().MuscleKingGymBoulder = false
end)
 
local JungleGym = AutoFarm:AddFolder("Jungle Gym")
JungleGym:AddSwitch("Bench Press", function(b) getgenv().JungleGymBenchPress = b teleportLoop("JungleGymBenchPress") end)
JungleGym:AddSwitch("Squat", function(b) getgenv().JungleGymSquat = b teleportLoop("JungleGymSquat") end)
JungleGym:AddSwitch("Deadlift", function(b) getgenv().JungleGymDeadlift = b teleportLoop("JungleGymDeadlift") end)
JungleGym:AddSwitch("Pull Up", function(b) getgenv().JungleGymPullUp = b teleportLoop("JungleGymPullUp") end)
JungleGym:AddSwitch("Boulder", function(b) getgenv().JungleGymBoulder = b teleportLoop("JungleGymBoulder") end)
JungleGym:AddSwitch("All Off", function()
    getgenv().JungleGymBenchPress = false getgenv().JungleGymSquat = false getgenv().JungleGymDeadlift = false getgenv().JungleGymPullUp = false getgenv().JungleGymBoulder = false
end)
 
local Killer = window:AddTab("Killer")
 
Killer:AddSwitch("Auto Good Karma", function(bool)
    autoGoodKarma = bool
 
    if autoGoodKarma then
        spawn(function()
            while autoGoodKarma do
                local player = game.Players.LocalPlayer
                local playerChar = player.Character
                local rightHand = playerChar and playerChar:FindFirstChild("RightHand")
                local leftHand = playerChar and playerChar:FindFirstChild("LeftHand")
 
                if playerChar and rightHand and leftHand then
                    for _, target in ipairs(game.Players:GetPlayers()) do
                        if target ~= player then
                            local evilKarma = target:FindFirstChild("evilKarma")
                            local goodKarma = target:FindFirstChild("goodKarma")
 
                            if evilKarma and goodKarma and evilKarma:IsA("IntValue") and goodKarma:IsA("IntValue") and evilKarma.Value > goodKarma.Value then
                                local targetChar = target.Character
                                local rootPart = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
 
                                if rootPart then
                                    firetouchinterest(rightHand, rootPart, 1)
                                    firetouchinterest(leftHand, rootPart, 1)
                                    firetouchinterest(rightHand, rootPart, 0)
                                    firetouchinterest(leftHand, rootPart, 0)
                                end
                            end
                        end
                    end
                end
                task.wait(0.01)
            end
        end)
    end
end)
 
Killer:AddSwitch("Auto Bad Karma", function(bool)
    autoBadKarma = bool
 
    if autoBadKarma then
        spawn(function()
            while autoBadKarma do
                local player = game.Players.LocalPlayer
                local playerChar = player.Character
                local rightHand = playerChar and playerChar:FindFirstChild("RightHand")
                local leftHand = playerChar and playerChar:FindFirstChild("LeftHand")
 
                if playerChar and rightHand and leftHand then
                    for _, target in ipairs(game.Players:GetPlayers()) do
                        if target ~= player then
                            local evilKarma = target:FindFirstChild("evilKarma")
                            local goodKarma = target:FindFirstChild("goodKarma")
 
                            if evilKarma and goodKarma and evilKarma:IsA("IntValue") and goodKarma:IsA("IntValue") and goodKarma.Value > evilKarma.Value then
                                local targetChar = target.Character
                                local rootPart = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
 
                                if rootPart then
                                    firetouchinterest(rightHand, rootPart, 1)
                                    firetouchinterest(leftHand, rootPart, 1)
                                    firetouchinterest(rightHand, rootPart, 0)
                                    firetouchinterest(leftHand, rootPart, 0)
                                end
                            end
                        end
                    end
                end
                task.wait(0.01)
            end
        end)
    end
end)
 
Killer:AddLabel("Whitelisting")
local playerWhitelist = {}
Killer:AddTextBox("Whitelist", function(text)
    local targetPlayer = game.Players:FindFirstChild(text)
    if targetPlayer then
        playerWhitelist[targetPlayer.Name] = true
    end
end)
 
Killer:AddTextBox("UnWhitelist", function(text)
    local targetPlayer = game.Players:FindFirstChild(text)
    if targetPlayer then
        playerWhitelist[targetPlayer.Name] = nil
    end
end)
 
Killer:AddLabel("Auto Killing")
local autoKill = false
Killer:AddSwitch("Auto Kill", function(bool)
    autoKill = bool
 
    while autoKill do
        local player = game.Players.LocalPlayer
 
        for _, target in ipairs(game.Players:GetPlayers()) do
            if target ~= player and not playerWhitelist[target.Name] then
                local targetChar = target.Character
                local rootPart = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
 
                if rootPart then
                    local rightHand = player.Character and player.Character:FindFirstChild("RightHand")
                    local leftHand = player.Character and player.Character:FindFirstChild("LeftHand")
 
                    if rightHand and leftHand then
                        firetouchinterest(rightHand, rootPart, 1)
                        firetouchinterest(leftHand, rootPart, 1)
                        firetouchinterest(rightHand, rootPart, 0)
                        firetouchinterest(leftHand, rootPart, 0)
                    end
                end
            end
        end
 
        wait(0.01)
    end
end)
 
Killer:AddLabel("Targeting")
local targetPlayerName = nil
Killer:AddTextBox("Target Name", function(text)
    targetPlayerName = text
end)
 
local targetDropdown = Killer:AddDropdown("Select Target", function(text)
    targetPlayerName = text
end)
 
 
for _, player in ipairs(game.Players:GetPlayers()) do
    targetDropdown:Add(player.Name)
end
 
local killTarget = false
Killer:AddSwitch("Kill Target", function(bool)
    killTarget = bool
 
    while killTarget do
        local player = game.Players.LocalPlayer
        local target = game.Players:FindFirstChild(targetPlayerName)
 
        if target and target ~= player then
            local targetChar = target.Character
            local rootPart = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
 
            if rootPart then
                local rightHand = player.Character and player.Character:FindFirstChild("RightHand")
                local leftHand = player.Character and player.Character:FindFirstChild("LeftHand")
 
                if rightHand and leftHand then
                    firetouchinterest(rightHand, rootPart, 1)
                    firetouchinterest(leftHand, rootPart, 1)
                    firetouchinterest(rightHand, rootPart, 0)
                    firetouchinterest(leftHand, rootPart, 0)
                end
            end
        end
 
        wait(0.01)
    end
end)
 
local spying = false
Killer:AddSwitch("Spy Player", function(bool)
    spying = bool
 
    if not spying then
        local player = game.Players.LocalPlayer
        local camera = workspace.CurrentCamera
        camera.CameraSubject = player.Character and player.Character:FindFirstChild("Humanoid") or player
        return
    end
 
    while spying do
        local player = game.Players.LocalPlayer
        local target = game.Players:FindFirstChild(targetPlayerName)
 
        if target and target ~= player then
            local targetChar = target.Character
            local targetHumanoid = targetChar and targetChar:FindFirstChild("Humanoid")
 
            if targetHumanoid then
                local camera = workspace.CurrentCamera
                camera.CameraSubject = targetHumanoid
            end
        end
 
        wait(0.1)
    end
end)
 
Killer:AddLabel("Punching Tool")
local autoEquipPunch = false
Killer:AddSwitch("Auto Equip Punch", function(state)
    autoEquipPunch = state
 
    while autoEquipPunch do
        local player = game.Players.LocalPlayer
        local punchTool = player.Backpack:FindFirstChild("Punch")
 
        if punchTool then
            punchTool.Parent = player.Character
        end
 
        wait(0.1)
    end
end)
 
local autoPunchNoAnim = false
Killer:AddSwitch("Auto Punch [No Animation]", function(state)
    autoPunchNoAnim = state
 
    while autoPunchNoAnim do
        local player = game.Players.LocalPlayer
        local playerName = player.Name
        local punchTool = player.Backpack:FindFirstChild("Punch") or game.Workspace:FindFirstChild(playerName):FindFirstChild("Punch")
 
        if punchTool then
            if punchTool.Parent ~= game.Workspace:FindFirstChild(playerName) then
                punchTool.Parent = game.Workspace:FindFirstChild(playerName)
            end
 
            player.muscleEvent:FireServer("punch", "rightHand")
            player.muscleEvent:FireServer("punch", "leftHand")
        else
            warn("Punch tool not found")
            autoPunchNoAnim = false
        end
 
        wait(0.01)
    end
end)
 
Killer:AddSwitch("Auto Punch", function(Value)
    _G.autoPunchActive = Value
    if Value then
        local function equipAndModifyPunch()
            while _G.autoPunchActive do
                local player = game.Players.LocalPlayer
                local punch = player.Backpack:FindFirstChild("Punch")
                if punch then
                    punch.Parent = player.Character
                    -- Ensure the attack time value is not modified
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                wait(0)
            end
        end
 
        local function autoPunchAction()
            while _G.autoPunchActive do
                local player = game.Players.LocalPlayer
                local character = player.Character
                if character then
                    local punchTool = character:FindFirstChild("Punch")
                    if punchTool then
                        punchTool:Activate()  -- Activate punch without firing event
                    end
                end
                wait(0)
            end
        end
 
        coroutine.wrap(equipAndModifyPunch)()
        coroutine.wrap(autoPunchAction)()
    else
        local character = game.Players.LocalPlayer.Character
        local equipped = character:FindFirstChild("Punch")
        if equipped then
            equipped.Parent = game.Players.LocalPlayer.Backpack
        end
    end
end)
 
Killer:AddSwitch("Fast Punch", function(Value)
    _G.fastHitActive = Value
    if Value then
        local function equipAndModifyPunch()
            while _G.fastHitActive do
                local player = game.Players.LocalPlayer
                local punch = player.Backpack:FindFirstChild("Punch")
                if punch then
                    punch.Parent = player.Character
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                wait(0.1)
            end
        end
 
        local function fastPunchAction()
            while _G.fastHitActive do
                local player = game.Players.LocalPlayer
                local character = player.Character
                if character then
                    local punchTool = character:FindFirstChild("Punch")
                    if punchTool then
                        punchTool:Activate()  -- Activate punch without firing event
                    end
                end
                wait(0.1)
            end
        end
 
        coroutine.wrap(equipAndModifyPunch)()
        coroutine.wrap(fastPunchAction)()
    else
        local character = game.Players.LocalPlayer.Character
        local equipped = character:FindFirstChild("Punch")
        if equipped then
            equipped.Parent = game.Players.LocalPlayer.Backpack
        end
    end
end)
 
local pets = window:AddTab("Pets")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
 
pets:AddLabel("PETS")
 
local selectedPet = "Neon Guardian" 
local petDropdown = pets:AddDropdown("Select Pet", function(text)
    selectedPet = text
    print("Selected pet: " .. text)
end)
 
petDropdown:Add("Neon Guardian")
petDropdown:Add("Blue Birdie")
petDropdown:Add("Blue Bunny")
petDropdown:Add("Blue Firecaster")
petDropdown:Add("Blue Pheonix")
petDropdown:Add("Crimson Falcon")
petDropdown:Add("Cybernetic Showdown Dragon")
petDropdown:Add("Dark Golem")
petDropdown:Add("Dark Legends Manticore")
petDropdown:Add("Dark Vampy")
petDropdown:Add("Darkstar Hunter")
petDropdown:Add("Eternal Strike Leviathan")
petDropdown:Add("Frostwave Legends Penguin")
petDropdown:Add("Gold Warrior")
petDropdown:Add("Golden Pheonix")
petDropdown:Add("Golden Viking")
petDropdown:Add("Green Butterfly")
petDropdown:Add("Green Firecaster")
petDropdown:Add("Infernal Dragon")
petDropdown:Add("Lightning Strike Phantom")
petDropdown:Add("Magic Butterfly")
petDropdown:Add("Muscle Sensei")
petDropdown:Add("Orange Hedgehog")
petDropdown:Add("Orange Pegasus")
petDropdown:Add("Phantom Genesis Dragon")
petDropdown:Add("Purple Dragon")
petDropdown:Add("Purple Falcon")
petDropdown:Add("Red Dragon")
petDropdown:Add("Red Firecaster")
petDropdown:Add("Red Kitty")
petDropdown:Add("Silver Dog")
petDropdown:Add("Ultimate Supernova Pegasus")
petDropdown:Add("Ultra Birdie")
petDropdown:Add("White Pegasus")
petDropdown:Add("White Pheonix")
petDropdown:Add("Yellow Butterfly")
 
-- Auto open pet toggle
pets:AddSwitch("Auto Open Pet", function(bool)
    _G.AutoHatchPet = bool
 
    if bool then
        spawn(function()
            while _G.AutoHatchPet and selectedPet ~= "" do
                local petToOpen = ReplicatedStorage.cPetShopFolder:FindFirstChild(selectedPet)
                if petToOpen then
                    ReplicatedStorage.cPetShopRemote:InvokeServer(petToOpen)
                end
                task.wait(1)
            end
        end)
    end
end)
 
-- Aura section
pets:AddLabel("AURAS")
 
-- Create aura dropdown with the correct format
local selectedAura = "Blue Aura" -- Default selection
local auraDropdown = pets:AddDropdown("Select Aura", function(text)
    selectedAura = text
    print("Selected aura: " .. text)
end)
 
-- Add aura options
auraDropdown:Add("Astral Electro")
auraDropdown:Add("Azure Tundra")
auraDropdown:Add("Blue Aura")
auraDropdown:Add("Dark Electro")
auraDropdown:Add("Dark Lightning")
auraDropdown:Add("Dark Storm")
auraDropdown:Add("Electro")
auraDropdown:Add("Enchanted Mirage")
auraDropdown:Add("Entropic Blast")
auraDropdown:Add("Eternal Megastrike")
auraDropdown:Add("Grand Supernova")
auraDropdown:Add("Green Aura")
auraDropdown:Add("Inferno")
auraDropdown:Add("Lightning")
auraDropdown:Add("Muscle King")
auraDropdown:Add("Power Lightning")
auraDropdown:Add("Purple Aura")
auraDropdown:Add("Purple Nova")
auraDropdown:Add("Red Aura")
auraDropdown:Add("Supernova")
auraDropdown:Add("Ultra Inferno")
auraDropdown:Add("Ultra Mirage")
auraDropdown:Add("Unstable Mirage")
auraDropdown:Add("Yellow Aura")
 
pets:AddSwitch("Auto Open Aura", function(bool)
    _G.AutoHatchAura = bool
 
    if bool then
        spawn(function()
            while _G.AutoHatchAura and selectedAura ~= "" do
                local auraToOpen = ReplicatedStorage.cPetShopFolder:FindFirstChild(selectedAura)
                if auraToOpen then
                    ReplicatedStorage.cPetShopRemote:InvokeServer(auraToOpen)
                end
                task.wait(1)
            end
        end)
    end
end)
 
 
local OpThings = window:AddTab("Op Things")
 
local folder1 = OpThings:AddFolder("Op Grind")
 
local packFarm = folder1:AddSwitch("Pack Farm", function(bool)
    isRunning = bool
 
    task.spawn(function()
        while isRunning do
            local currentRebirths = player.leaderstats.Rebirths.Value
            local rebirthCost = 10000 + (5000 * currentRebirths)
 
            if player.ultimatesFolder:FindFirstChild("Golden Rebirth") then
                local goldenRebirths = player.ultimatesFolder["Golden Rebirth"].Value
                rebirthCost = math.floor(rebirthCost * (1 - (goldenRebirths * 0.1)))
            end
 
            unequipAllPets()
            task.wait(0.1)
            equipUniquePet("Swift Samurai")
 
            while isRunning and player.leaderstats.Strength.Value < rebirthCost do
                for i = 1, 10 do
                    player.muscleEvent:FireServer("rep")
                end
                task.wait()
            end
 
            unequipAllPets()
            task.wait(0.1)
            equipUniquePet("Tribal Overlord")
 
            local machine = findMachine("Jungle Bar Lift")
            if machine and machine:FindFirstChild("interactSeat") then
                player.Character.HumanoidRootPart.CFrame = machine.interactSeat.CFrame * CFrame.new(0, 3, 0)
                repeat
                    task.wait(0.1)
                    pressE()
                until player.Character.Humanoid.Sit
            end
 
            local initialRebirths = player.leaderstats.Rebirths.Value
            repeat
                ReplicatedStorage.rEvents.rebirthRemote:InvokeServer("rebirthRequest")
                task.wait(0.1)
            until player.leaderstats.Rebirths.Value > initialRebirths
 
            if not isRunning then break end
            task.wait()
        end
    end)
end)
 
local frameToggle = folder1:AddSwitch("Hide All Frames", function(bool)
    local rSto = game:GetService("ReplicatedStorage")
    for _, obj in pairs(rSto:GetChildren()) do
        if obj.Name:match("Frame$") then
            obj.Visible = not bool
        end
    end
end)
 
local speedGrind = folder1:AddSwitch("Speed Grind (No Rebirth)", function(bool)
    local isGrinding = bool
 
    if not bool then
        unequipAllPets()
        return
    end
 
    equipUniquePet("Swift Samurai")
 
    for i = 1, 23 do
        task.spawn(function()
            while isGrinding do
                player.muscleEvent:FireServer("rep")
                task.wait()
            end
        end)
    end
end)
 
local Statistic = Main:AddFolder("Rebirth Calculation")
 
local labels = {  
    RebirthsGainedLabel = Statistic:AddLabel("Rebirths Gained In 1 Minute: ..."),  
    RebirthsPerMinuteLabel = Statistic:AddLabel("Rebirths Per Minute: ..."),  
    RebirthsPerHourLabel = Statistic:AddLabel("Rebirths Per Hour: ..."),  
    RebirthsPerDayLabel = Statistic:AddLabel("Rebirths Per Day: ..."),  
    RebirthsPerWeekLabel = Statistic:AddLabel("Rebirths Per Week: ...")  
}  
 
local player = game:GetService("Players").LocalPlayer  
local leaderstats = player:FindFirstChild("leaderstats")  
local rebirthStat = leaderstats and leaderstats:FindFirstChild("Rebirths")  
 
local function abbreviateNumber(num)  
    if num >= 1e9 then  
        return string.format("%.2fB", num / 1e9)  
    elseif num >= 1e6 then  
        return string.format("%.2fM", num / 1e6)  
    elseif num >= 1e3 then  
        return string.format("%.2fK", num / 1e3)  
    else  
        return tostring(num)  
    end  
end  
 
local lastRebirthCount = rebirthStat and rebirthStat.Value or 0  
 
task.spawn(function()  
    while task.wait(60) do  
        local currentRebirthCount = rebirthStat and rebirthStat.Value or 0  
        local rebirthsGained = math.max(0, currentRebirthCount - lastRebirthCount)  
        lastRebirthCount = currentRebirthCount  
 
        labels.RebirthsGainedLabel.Text = "Rebirths Gained In 1 Minute: " .. abbreviateNumber(rebirthsGained)  
        labels.RebirthsPerMinuteLabel.Text = "Rebirths Per Minute: " .. abbreviateNumber(rebirthsGained)  
        labels.RebirthsPerHourLabel.Text = "Rebirths Per Hour: " .. abbreviateNumber(rebirthsGained * 60)  
        labels.RebirthsPerDayLabel.Text = "Rebirths Per Day: " .. abbreviateNumber(rebirthsGained * 1440)  
        labels.RebirthsPerWeekLabel.Text = "Rebirths Per Week: " .. abbreviateNumber(rebirthsGained * 10080)  
    end  
end)
 
local ViewStats = Main:AddFolder("Spy stats")
 
local targetPlayer = nil
 
        local textbox = ViewStats:AddTextBox("Target Name", function(text)
            local player = game.Players:FindFirstChild(text)
            if player then
                targetPlayer = player
            else
                targetPlayer = nil
                resetTargetStats()
            end
        end)
 
        local labels = {
            ViewStats = ViewStats:AddLabel("View Stats:"),
            StrengthLabel = ViewStats:AddLabel("Strength: 0"),
            DurabilityLabel = ViewStats:AddLabel("Durability: 0"),
            AgilityLabel = ViewStats:AddLabel("Agility: 0"),
            RebirthsLabel = ViewStats:AddLabel("Rebirths: 0"),
            KillsLabel = ViewStats:AddLabel("Kills: 0"),
            EvilKarmaLabel = ViewStats:AddLabel("Evil Karma: 0"),
            GoodKarmaLabel = ViewStats:AddLabel("Good Karma: 0"),
            TargetPetsLabel = ViewStats:AddLabel("Target Equipped Pets:")
        }
 
        for i = 1, 8 do
            labels["Pet" .. i .. "Label"] = ViewStats:AddLabel("Pet" .. i .. ": N/A")
        end
 
        local function updateTargetStats()
            if not targetPlayer then return end
 
            local leaderstats = targetPlayer:FindFirstChild("leaderstats")
            local goodKarma = targetPlayer:FindFirstChild("goodKarma")
            local evilKarma = targetPlayer:FindFirstChild("evilKarma")
            local equippedPets = targetPlayer:FindFirstChild("equippedPets")
 
            if leaderstats then
                labels.StrengthLabel.Text = "Strength: " .. abbreviateNumber(leaderstats:FindFirstChild("Strength") and leaderstats.Strength.Value or 0)
                labels.DurabilityLabel.Text = "Durability: " .. abbreviateNumber(targetPlayer:FindFirstChild("Durability") and targetPlayer.Durability.Value or 0)
                labels.AgilityLabel.Text = "Agility: " .. abbreviateNumber(targetPlayer:FindFirstChild("Agility") and targetPlayer.Agility.Value or 0)
                labels.RebirthsLabel.Text = "Rebirths: " .. abbreviateNumber(leaderstats:FindFirstChild("Rebirths") and leaderstats.Rebirths.Value or 0)
                labels.KillsLabel.Text = "Kills: " .. abbreviateNumber(leaderstats:FindFirstChild("Kills") and leaderstats.Kills.Value or 0)
            end
 
            labels.EvilKarmaLabel.Text = "Evil Karma: " .. abbreviateNumber(evilKarma and evilKarma.Value or 0)
            labels.GoodKarmaLabel.Text = "Good Karma: " .. abbreviateNumber(goodKarma and goodKarma.Value or 0)
 
            if equippedPets then
                for i = 1, 8 do
                    local pet = equippedPets:FindFirstChild("Pet" .. i)
                    labels["Pet" .. i .. "Label"].Text = "Pet" .. i .. ": " .. (pet and pet.Value or "N/A")
                end
            else
                for i = 1, 8 do
                    labels["Pet" .. i .. "Label"].Text = "Pet" .. i .. ": N/A"
                end
            end
        end
 
        local function resetTargetStats()
            labels.StrengthLabel.Text = "Strength: 0"
            labels.DurabilityLabel.Text = "Durability: 0"
            labels.AgilityLabel.Text = "Agility: 0"
            labels.RebirthsLabel.Text = "Rebirths: 0"
            labels.KillsLabel.Text = "Kills: 0"
            labels.EvilKarmaLabel.Text = "Evil Karma: 0"
            labels.GoodKarmaLabel.Text = "Good Karma: 0"
 
            for i = 1, 8 do
                labels["Pet" .. i .. "Label"].Text = "Pet" .. i .. ": N/A"
            end
        end
 
        task.spawn(function()
            while task.wait(0.1) do
                if targetPlayer then
                    updateTargetStats()
                end
            end
        end)
 
local GainedStats = Main:AddFolder("Stats Gained")
 
local initialStats = {
    Strength = 0,
    Durability = 0,
    Agility = 0,
    Rebirths = 0,
    Kills = 0,
    EvilKarma = 0,
    GoodKarma = 0
}
 
-- Create labels inside GainedStats folder
local gainedLabels = {
    StrengthLabel = GainedStats:AddLabel("Strength Gained In Server: 0"),
    DurabilityLabel = GainedStats:AddLabel("Durability Gained In Server: 0"),
    AgilityLabel = GainedStats:AddLabel("Agility Gained In Server: 0"),
    RebirthsLabel = GainedStats:AddLabel("Rebirths Gained In Server: 0"),
    KillsLabel = GainedStats:AddLabel("Kills Gained In Server: 0"),
    EvilKarmaLabel = GainedStats:AddLabel("Evil Karma Gained In Server: 0"),
    GoodKarmaLabel = GainedStats:AddLabel("Good Karma Gained In Server: 0")
}
 
local function storeInitialStats()
    if not targetPlayer then return end
 
    local leaderstats = targetPlayer:FindFirstChild("leaderstats")
    local evilKarma = targetPlayer:FindFirstChild("evilKarma")
    local goodKarma = targetPlayer:FindFirstChild("goodKarma")
 
    if leaderstats then
        initialStats.Strength = leaderstats:FindFirstChild("Strength") and leaderstats.Strength.Value or 0
        initialStats.Durability = targetPlayer:FindFirstChild("Durability") and targetPlayer.Durability.Value or 0
        initialStats.Agility = targetPlayer:FindFirstChild("Agility") and targetPlayer.Agility.Value or 0
        initialStats.Rebirths = leaderstats:FindFirstChild("Rebirths") and leaderstats.Rebirths.Value or 0
        initialStats.Kills = leaderstats:FindFirstChild("Kills") and leaderstats.Kills.Value or 0
    end
 
    initialStats.EvilKarma = evilKarma and evilKarma.Value or 0
    initialStats.GoodKarma = goodKarma and goodKarma.Value or 0
end
 
local function updateTargetStats()
    if not targetPlayer then return end
 
    local leaderstats = targetPlayer:FindFirstChild("leaderstats")
    local goodKarma = targetPlayer:FindFirstChild("goodKarma")
    local evilKarma = targetPlayer:FindFirstChild("evilKarma")
 
    if leaderstats then
        local currentStrength = leaderstats:FindFirstChild("Strength") and leaderstats.Strength.Value or 0
        local currentDurability = targetPlayer:FindFirstChild("Durability") and targetPlayer.Durability.Value or 0
        local currentAgility = targetPlayer:FindFirstChild("Agility") and targetPlayer.Agility.Value or 0
        local currentRebirths = leaderstats:FindFirstChild("Rebirths") and leaderstats.Rebirths.Value or 0
        local currentKills = leaderstats:FindFirstChild("Kills") and leaderstats.Kills.Value or 0
 
        gainedLabels.StrengthLabel.Text = "Strength Gained In Server: " .. abbreviateNumber(currentStrength - initialStats.Strength)
        gainedLabels.DurabilityLabel.Text = "Durability Gained In Server: " .. abbreviateNumber(currentDurability - initialStats.Durability)
        gainedLabels.AgilityLabel.Text = "Agility Gained In Server: " .. abbreviateNumber(currentAgility - initialStats.Agility)
        gainedLabels.RebirthsLabel.Text = "Rebirths Gained In Server: " .. abbreviateNumber(currentRebirths - initialStats.Rebirths)
        gainedLabels.KillsLabel.Text = "Kills Gained In Server: " .. abbreviateNumber(currentKills - initialStats.Kills)
    end
 
    local currentEvilKarma = evilKarma and evilKarma.Value or 0
    local currentGoodKarma = goodKarma and goodKarma.Value or 0
 
    gainedLabels.EvilKarmaLabel.Text = "Evil Karma Gained In Server: " .. abbreviateNumber(currentEvilKarma - initialStats.EvilKarma)
    gainedLabels.GoodKarmaLabel.Text = "Good Karma Gained In Server: " .. abbreviateNumber(currentGoodKarma - initialStats.GoodKarma)
end
 
local function resetTargetStats()
    gainedLabels.StrengthLabel.Text = "Strength Gained In Server: 0"
    gainedLabels.DurabilityLabel.Text = "Durability Gained In Server: 0"
    gainedLabels.AgilityLabel.Text = "Agility Gained In Server: 0"
    gainedLabels.RebirthsLabel.Text = "Rebirths Gained In Server: 0"
    gainedLabels.KillsLabel.Text = "Kills Gained In Server: 0"
    gainedLabels.EvilKarmaLabel.Text = "Evil Karma Gained In Server: 0"
    gainedLabels.GoodKarmaLabel.Text = "Good Karma Gained In Server: 0"
end
 
task.spawn(function()
    while task.wait(0.1) do
        if targetPlayer then
            if initialStats.Strength == 0 then
                storeInitialStats()
            end
            updateTargetStats()
        end
    end
end)
 
local Misc = window:AddTab("Misc")
 
local folder1 = Main:AddFolder("Misc 1")
 
folder1:AddSwitch("Anti-Knockback", function(bool)
    if bool then
        local playerName = game.Players.LocalPlayer.Name
        local rootPart = game.Workspace:FindFirstChild(playerName):FindFirstChild("HumanoidRootPart")
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(100000, 0, 100000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.P = 1250
        bodyVelocity.Parent = rootPart
    else
        local playerName = game.Players.LocalPlayer.Name
        local rootPart = game.Workspace:FindFirstChild(playerName):FindFirstChild("HumanoidRootPart")
        local existingVelocity = rootPart:FindFirstChild("BodyVelocity")
        if existingVelocity and existingVelocity.MaxForce == Vector3.new(100000, 0, 100000) then
            existingVelocity:Destroy()
        end
    end
end)
 
local parts = {}
local partSize = 2048
local totalDistance = 50000
local startPosition = Vector3.new(-2, -9.5, -2)
local numberOfParts = math.ceil(totalDistance / partSize)
 
local function createParts()
    for x = 0, numberOfParts - 1 do
        for z = 0, numberOfParts - 1 do
            local newPartSide = Instance.new("Part")
            newPartSide.Size = Vector3.new(partSize, 1, partSize)
            newPartSide.Position = startPosition + Vector3.new(x * partSize, 0, z * partSize)
            newPartSide.Anchored = true
            newPartSide.Transparency = 1
            newPartSide.CanCollide = true
            newPartSide.Name = "Part_Side_" .. x .. "_" .. z
            newPartSide.Parent = workspace
            table.insert(parts, newPartSide)
 
            local newPartLeftRight = Instance.new("Part")
            newPartLeftRight.Size = Vector3.new(partSize, 1, partSize)
            newPartLeftRight.Position = startPosition + Vector3.new(-x * partSize, 0, z * partSize)
            newPartLeftRight.Anchored = true
            newPartLeftRight.Transparency = 1
            newPartLeftRight.CanCollide = true
            newPartLeftRight.Name = "Part_LeftRight_" .. x .. "_" .. z
            newPartLeftRight.Parent = workspace
            table.insert(parts, newPartLeftRight)
 
            local newPartUpLeft = Instance.new("Part")
            newPartUpLeft.Size = Vector3.new(partSize, 1, partSize)
            newPartUpLeft.Position = startPosition + Vector3.new(-x * partSize, 0, -z * partSize)
            newPartUpLeft.Anchored = true
            newPartUpLeft.Transparency = 1
            newPartUpLeft.CanCollide = true
            newPartUpLeft.Name = "Part_UpLeft_" .. x .. "_" .. z
            newPartUpLeft.Parent = workspace
            table.insert(parts, newPartUpLeft)
 
            local newPartUpRight = Instance.new("Part")
            newPartUpRight.Size = Vector3.new(partSize, 1, partSize)
            newPartUpRight.Position = startPosition + Vector3.new(x * partSize, 0, -z * partSize)
            newPartUpRight.Anchored = true
            newPartUpRight.Transparency = 1
            newPartUpRight.CanCollide = true
            newPartUpRight.Name = "Part_UpRight_" .. x .. "_" .. z
            newPartUpRight.Parent = workspace
            table.insert(parts, newPartUpRight)
        end
    end
end
 
local function makePartsWalkthrough()
    for _, part in ipairs(parts) do
        if part and part.Parent then
            part.CanCollide = false
        end
    end
end
 
local function makePartsSolid()
    for _, part in ipairs(parts) do
        if part and part.Parent then
            part.CanCollide = true
        end
    end
end
 
folder1:AddSwitch("Full Walk On Water", function(bool)
    if bool then
        createParts()
    else
        makePartsWalkthrough()
    end
end)
 
folder1:AddButton("Destroy Ad Teleport", function()
            local part = workspace:FindFirstChild("RobloxForwardPortals")
            if part then
                part:Destroy()
            end
        end)
 
        folder1:AddButton("Anti Kick", function()
            wait(0.5)
            local ba = Instance.new("ScreenGui")
            local ca = Instance.new("TextLabel")
            local da = Instance.new("Frame")
            local _b = Instance.new("TextLabel")
            local ab = Instance.new("TextLabel")
 
            ba.Parent = game.CoreGui
            ba.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
 
            ca.Parent = ba
            ca.Active = true
            ca.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
            ca.Draggable = true
            ca.Position = UDim2.new(0.698610067, 0, 0.098096624, 0)
            ca.Size = UDim2.new(0, 370, 0, 52)
            ca.Font = Enum.Font.SourceSansSemibold
            ca.Text = "Anti Afk"
            ca.TextColor3 = Color3.new(0, 1, 1)
            ca.TextSize = 22
 
            da.Parent = ca
            da.BackgroundColor3 = Color3.new(0.196078, 0.196078, 0.196078)
            da.Position = UDim2.new(0, 0, 1.0192306, 0)
            da.Size = UDim2.new(0, 370, 0, 107)
 
            _b.Parent = da
            _b.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
            _b.Position = UDim2.new(0, 0, 0.800455689, 0)
            _b.Size = UDim2.new(0, 370, 0, 21)
            _b.Font = Enum.Font.Arial
            _b.Text = "Made by luca#5432"
            _b.TextColor3 = Color3.new(0, 1, 1)
            _b.TextSize = 20
 
            ab.Parent = da
            ab.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
            ab.Position = UDim2.new(0, 0, 0.158377, 0)
            ab.Size = UDim2.new(0, 370, 0, 44)
            ab.Font = Enum.Font.ArialBold
            ab.Text = "Status: Active"
            ab.TextColor3 = Color3.new(0, 1, 1)
            ab.TextSize = 20
 
            local bb = game:GetService("VirtualUser")
            game.Players.LocalPlayer.Idled:Connect(function()
                bb:CaptureController()
                bb:ClickButton2(Vector2.new())
                ab.Text = "Roblox tried kicking you, but I won't let them!"
                wait(2)
                ab.Text = "Status: Active"
            end)
        end)
 
 
local folder2 = Main:AddFolder("Misc 2")
 
folder2:AddButton("Remove Textures", function()
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("Decal") or v:IsA("Texture") then
            v.Transparency = 1
        end
    end
 
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Performance",
        Text = "Textures removed!",
        Duration = 5
    })
end)
 
folder2:AddButton("Reduce Graphics", function()
    settings().Rendering.QualityLevel = 1
 
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Performance",
        Text = "Graphics reduced!",
        Duration = 5
    })
end)
 
folder2:AddButton("Disable Effects", function()
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter") or v:IsA("Smoke") or v:IsA("Fire") or v:IsA("Sparkles") then
            v.Enabled = false
        end
    end
 
    local lighting = game:GetService("Lighting")
    for _, v in pairs(lighting:GetChildren()) do
        if v:IsA("BlurEffect") or v:IsA("SunRaysEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("BloomEffect") or v:IsA("DepthOfFieldEffect") then
            v.Enabled = false
        end
    end
 
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Performance",
        Text = "Effects disabled!",
        Duration = 5
    })
end)
 
folder2:AddButton("Simplify Materials", function()
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("BasePart") and not v:IsA("MeshPart") then
            v.Material = Enum.Material.SmoothPlastic
            if v.Parent and (v.Parent:FindFirstChild("Humanoid") or v.Parent.Parent:FindFirstChild("Humanoid")) then
            else
                v.Reflectance = 0
            end
        end
    end
 
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Performance",
        Text = "Materials simplified!",
        Duration = 5
    })
end)
 
folder2:AddButton("Remove Fog", function()
    local lighting = game:GetService("Lighting")
    lighting.FogEnd = 9e9
 
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Performance",
        Text = "Fog removed!",
        Duration = 5
    })
end)
 
folder2:AddButton("Full Optimization", function()
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter") or v:IsA("Smoke") or v:IsA("Fire") or v:IsA("Sparkles") then
            v.Enabled = false
        end
    end
 
    local lighting = game:GetService("Lighting")
    lighting.GlobalShadows = false
    lighting.FogEnd = 9e9
    lighting.Brightness = 0
 
    settings().Rendering.QualityLevel = 1
 
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("Decal") or v:IsA("Texture") then
            v.Transparency = 1
        elseif v:IsA("BasePart") and not v:IsA("MeshPart") then
            v.Material = Enum.Material.SmoothPlastic
            if v.Parent and (v.Parent:FindFirstChild("Humanoid") or v.Parent.Parent:FindFirstChild("Humanoid")) then
            else
                v.Reflectance = 0
            end
        end
    end
 
    for _, v in pairs(lighting:GetChildren()) do
        if v:IsA("BlurEffect") or v:IsA("SunRaysEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("BloomEffect") or v:IsA("DepthOfFieldEffect") then
            v.Enabled = false
        end
    end
 
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Performance",
        Text = "Full optimization applied!",
        Duration = 5
    })
end)
 
local folder3 = Main:AddFolder("Misc 3")
 
local godModeToggle = false
        folder3:AddSwitch("God Mode (Brawl)", function(State)
            godModeToggle = State
            if State then
                task.spawn(function()
                    while godModeToggle do
                        game:GetService("ReplicatedStorage").rEvents.brawlEvent:FireServer("joinBrawl")
                        task.wait(0)
                    end
                end)
            end
        end)
 
        local autoJoinToggle = false
        folder3:AddSwitch("Auto Join Brawl", function(State)
            autoJoinToggle = State
            if State then
                task.spawn(function()
                    while autoJoinToggle do
                        game:GetService("ReplicatedStorage").rEvents.brawlEvent:FireServer("joinBrawl")
                        task.wait(2)
                    end
                end)
            end
        end)
 
local folder4 = Main:AddFolder("Misc 4")
 
folder4:AddSwitch("Unable Trade", function(State)
            if State then
                game:GetService("ReplicatedStorage").rEvents.tradingEvent:FireServer("disableTrading")
            else
                game:GetService("ReplicatedStorage").rEvents.tradingEvent:FireServer("enableTrading")
            end
        end)
 
        folder4:AddSwitch("Hide Pets", function(State)
            if State then
                game:GetService("ReplicatedStorage").rEvents.showPetsEvent:FireServer("hidePets")
            else
                game:GetService("ReplicatedStorage").rEvents.showPetsEvent:FireServer("showPets")
            end
        end)
 
local folder5 = Main:AddFolder("Misc 5")
 
local switchGamepads = folder5:AddSwitch("Free Auto Lift Gamepass", function(Value)
            local gamepassFolder = game:GetService("ReplicatedStorage"):FindFirstChild("gamepassIds")
            local player = game:GetService("Players").LocalPlayer
 
            if not gamepassFolder or not player then return end
 
            if Value then
                for _, gamepass in pairs(gamepassFolder:GetChildren()) do
                    if not player.ownedGamepasses:FindFirstChild(gamepass.Name) then
                        local value = Instance.new("IntValue")
                        value.Name = gamepass.Name
                        value.Value = gamepass.Value
                        value.Parent = player.ownedGamepasses
                    end
                end
            else
                for _, gamepass in pairs(player.ownedGamepasses:GetChildren()) do
                    gamepass:Destroy()
                end
            end
        end)
 
folder5:AddSwitch("Auto Fortune Wheel", function(Value)
    _G.autoFortuneWheelActive = Value
    if Value then
        local function openFortuneWheel()
            while _G.autoFortuneWheelActive do
                local args = {
                    [1] = "openFortuneWheel",
                    [2] = game:GetService("ReplicatedStorage"):WaitForChild("fortuneWheelChances"):WaitForChild("Fortune Wheel")
                }
                game:GetService("ReplicatedStorage"):WaitForChild("rEvents"):WaitForChild("openFortuneWheelRemote"):InvokeServer(unpack(args))
                wait(0)
            end
        end
        coroutine.wrap(openFortuneWheel)()
    else
        _G.autoFortuneWheelActive = false
    end
end)
 
local Teleport = Misc:AddFolder("Teleport")
 
        Teleport:AddButton("Tiny Island", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-31.8626194, 6.0588026, 2087.88672)
        end)
 
        Teleport:AddButton("Starter Island", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(226.252472, 8.1526947, 219.366516)
        end)
 
        Teleport:AddButton("Legend Beach", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-365.798309, 44.5082932, -501.618591)
        end)
 
        Teleport:AddButton("Frost Gym", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2933.47998, 29.6399612, -579.946045)
        end)
 
        Teleport:AddButton("Mythical Gym", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2659.50635, 21.6095238, 934.690613)
        end)
 
        Teleport:AddButton("Eternal Gym", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-7176.19141, 45.394104, -1106.31421)
        end)
 
        Teleport:AddButton("Legend Gym", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(4446.91699, 1004.46698, -3983.76074)
        end)
 
        Teleport:AddButton("Jungle Gym", function()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8137, 28, 2820)
        end)
 
Misc:AddButton("Rejoin Server", function()
    local ts = game:GetService("TeleportService")
    local p = game:GetService("Players").LocalPlayer
    ts:Teleport(game.PlaceId, p)
end)
 
local joinLowPlayerServerSwitch = Misc:AddSwitch("Join Low Player Server", function(bool)
    if bool then
        local module = loadstring(game:HttpGet("https://raw.githubusercontent.com/LeoKholYt/roblox/main/lk_serverhop.lua"))()
        module:Teleport(game.PlaceId, "Lowest")
    end
end)

local PetsTab = window:AddTab("PetsTab")

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

--// Remote events
local tradingEvent = ReplicatedStorage:WaitForChild("rEvents"):WaitForChild("tradingEvent")
local cPetShopRemote = ReplicatedStorage:WaitForChild("cPetShopRemote")
local cPetShopFolder = ReplicatedStorage:WaitForChild("cPetShopFolder")
local petEvolveEvent = ReplicatedStorage:WaitForChild("rEvents"):WaitForChild("petEvolveEvent")

local selectedPlayer = nil
local selectedPet = nil -- unified variable for trade, hatch, evolve
local offerCount = 6

local autoTrading, autoTradeAll, autoHatch, autoEvolve = false, false, false, false
local autoTradeLoopRunning, autoTradeAllLoopRunning, autoHatchLoopRunning, autoEvolveLoopRunning = false, false, false, false

--// Helper functions
local function offerPet(petInstance)
    tradingEvent:FireServer("offerItem", petInstance)
end

local function offerMultiplePets(petName, count)
    local LocalPlayer = Players.LocalPlayer
    local petFolder = LocalPlayer:WaitForChild("petsFolder"):WaitForChild("Unique")
    local offered = 0
    for _, pet in ipairs(petFolder:GetChildren()) do
        if pet.Name == petName then
            offerPet(pet)
            offered += 1
            task.wait(0.05)
            if offered >= count then break end
        end
    end
end

local function performTrade(target)
    if not target or not selectedPet then return end
    tradingEvent:FireServer("sendTradeRequest", target)
    task.wait(0.2) -- slightly longer wait for server response
    offerMultiplePets(selectedPet, offerCount)
    task.wait(0.1)
    tradingEvent:FireServer("acceptTrade")
end

local function autoTradeLoop()
    if autoTradeLoopRunning then return end
    autoTradeLoopRunning = true
    while autoTrading do
        if selectedPlayer then
            performTrade(selectedPlayer)
        end
        task.wait(0.5)
    end
    autoTradeLoopRunning = false
end
local function autoTradeAllLoop()
    if autoTradeAllLoopRunning then return end
    autoTradeAllLoopRunning = true
    while autoTradeAll do
        if selectedPet then
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= Players.LocalPlayer then
                    performTrade(player)
                    task.wait(0.2)
                end
            end
        end
        task.wait(1)
    end
    autoTradeAllLoopRunning = false
end

local function autoHatchLoop()
    if autoHatchLoopRunning then return end
    autoHatchLoopRunning = true
    while autoHatch and selectedPet do
        local petToOpen = cPetShopFolder:FindFirstChild(selectedPet)
        if petToOpen then
            local success, err = pcall(function()
                cPetShopRemote:InvokeServer(petToOpen)
            end)
            if not success then warn("Auto Hatch Error: "..err) end
        end
        task.wait(0.1)
    end
    autoHatchLoopRunning = false
end

local function autoEvolveLoop()
    if autoEvolveLoopRunning then return end
    autoEvolveLoopRunning = true
    while autoEvolve and selectedPet do
        local success, err = pcall(function()
            petEvolveEvent:FireServer("evolvePet", selectedPet)
        end)
        if not success then warn("Auto Evolve Error: "..err) end
        task.wait(0.1)
    end
    autoEvolveLoopRunning = false
end

PetsTab:AddLabel("Auto Trade & auto give pets & Auto Buy Pets").TextSize = 23

local petDropdown = PetsTab:AddDropdown("Select Pet", function(petName)
    selectedPet = petName
end)

local petsList = {
    "Neon Guardian","Blue Birdie","Blue Bunny","Blue Firecaster","Blue Pheonix",
    "Crimson Falcon","Cybernetic Showdown Dragon","Dark Golem","Dark Legends Manticore",
    "Dark Vampy","Darkstar Hunter","Eternal Strike Leviathan","Frostwave Legends Penguin",
    "Gold Warrior","Golden Pheonix","Golden Viking","Green Butterfly","Green Firecaster",
    "Infernal Dragon","Lightning Strike Phantom","Magic Butterfly","Muscle Sensei",
    "Orange Hedgehog","Orange Pegasus","Phantom Genesis Dragon","Purple Dragon",
    "Purple Falcon","Red Dragon","Red Firecaster","Red Kitty","Silver Dog",
    "Ultimate Supernova Pegasus","Ultra Birdie","White Pegasus","White Pheonix","Yellow Butterfly"
}

for _, petName in ipairs(petsList) do
    petDropdown:Add(petName)
end

PetsTab:AddSwitch("Auto Hatch Pet", function(state)
    autoHatch = state
    if state then task.spawn(autoHatchLoop) end
end)

PetsTab:AddSwitch("Auto Evolve Pet", function(state)
    autoEvolve = state
    if state then task.spawn(autoEvolveLoop) end
end)

PetsTab:AddLabel("Other").TextSize = 23

local playerDropdown = PetsTab:AddDropdown("Select Player", function(playerName)
    selectedPlayer = Players:FindFirstChild(playerName)
end)
for _, player in ipairs(Players:GetPlayers()) do
    if player ~= Players.LocalPlayer then playerDropdown:Add(player.Name) end
end
Players.PlayerAdded:Connect(function(player)
    if player ~= Players.LocalPlayer then playerDropdown:Add(player.Name) end
end)
Players.PlayerRemoving:Connect(function(player)
    playerDropdown:Remove(player.Name)
end)

PetsTab:AddSwitch("Auto Trade", function(state)
    autoTrading = state
    if state then task.spawn(autoTradeLoop) end
end)

PetsTab:AddSwitch("Auto Trade All", function(state)
    autoTradeAll = state
    if state then task.spawn(autoTradeAllLoop) end
end)

local Credit = window:AddTab("Credit")
 
Credit:AddLabel("MADE BY- PRM")

Credit:AddLabel("YESHI ON TOP")

Credit:AddLabel("OWNER YESHI")

Credit:AddLabel("  ")

Credit:AddLabel("YESHI OWN PRIVATE SCRIPT")

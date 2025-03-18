-- Load Rayfield Library
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Rayfield/main/source'))()

-- Create Window
local Window = Rayfield:CreateWindow({
    Name = "Zen Hub",
    LoadingTitle = "Zen Hub",
    LoadingSubtitle = "By  ZenosAlpha",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "ZenHubConfigs",
        FileName = "Settings"
    },
    Theme = "BloodTheme"
})

-- Create Main Section
local MainTab = Window:CreateTab("Main", nil) -- Tab Icon ID

-- Aura Kill
local auraToggle = MainTab:CreateToggle({
    Name = "Aura Kill",
    CurrentValue = false,
    Flag = "AuraKill",
    Callback = function(state)
        getgenv().AuraKill = state
        while getgenv().AuraKill do
            for _, enemy in pairs(game.Workspace:GetDescendants()) do
                if enemy:IsA("Model") and enemy:FindFirstChild("Humanoid") and enemy ~= game.Players.LocalPlayer.Character then
                    local distance = (enemy.PrimaryPart.Position - game.Players.LocalPlayer.Character.PrimaryPart.Position).magnitude
                    if distance <= 100000 then
                        enemy.Humanoid.Health = 0
                    end
                end
            end
            wait(0.1)
        end
    end
})

-- ESP
local espToggle = MainTab:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Flag = "ESP",
    Callback = function(state)
        getgenv().ESP = state
        while getgenv().ESP do
            for _, v in pairs(game.Workspace:GetDescendants()) do
                if v:IsA("Model") and v:FindFirstChild("Humanoid") and v ~= game.Players.LocalPlayer.Character then
                    local billboard = v:FindFirstChild("ESP") or Instance.new("BillboardGui", v)
                    billboard.Name = "ESP"
                    billboard.Size = UDim2.new(0, 200, 0, 50)
                    billboard.StudsOffset = Vector3.new(0, 3, 0)
                    billboard.AlwaysOnTop = true

                    local label = billboard:FindFirstChild("Label") or Instance.new("TextLabel", billboard)
                    label.Name = "Label"
                    label.Size = UDim2.new(1, 0, 1, 0)
                    label.BackgroundTransparency = 1
                    label.TextSize = 14
                    label.TextColor3 = Color3.fromRGB(255, 0, 0)
                    label.Text = v.Name .. " | Distance: " .. math.floor((v.PrimaryPart.Position - game.Players.LocalPlayer.Character.PrimaryPart.Position).Magnitude)
                end
            end
            wait(1)
        end
    end
})

-- Noclip
local noclipToggle = MainTab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "Noclip",
    Callback = function(state)
        getgenv().Noclip = state
        while getgenv().Noclip do
            for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
            wait()
        end
    end
})

-- Aimbot
local aimbotToggle = MainTab:CreateToggle({
    Name = "Aimbot",
    CurrentValue = false,
    Flag = "Aimbot",
    Callback = function(state)
        getgenv().Aimbot = state
        local targetPart = "Head" -- Change to "Chest" if you prefer

        game:GetService("RunService").RenderStepped:Connect(function()
            if getgenv().Aimbot then
                local closest = nil
                local shortestDistance = math.huge

                for _, enemy in pairs(game.Workspace:GetDescendants()) do
                    if enemy:IsA("Model") and enemy:FindFirstChild("Humanoid") and enemy ~= game.Players.LocalPlayer.Character then
                        local distance = (enemy.PrimaryPart.Position - game.Players.LocalPlayer.Character.PrimaryPart.Position).magnitude
                        if distance < shortestDistance then
                            closest = enemy
                            shortestDistance = distance
                        end
                    end
                end

                if closest then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(game.Players.LocalPlayer.Character.HumanoidRootPart.Position, closest[targetPart].Position)
                end
            end
        end)
    end
})

-- Auto-Collect
local autoCollectToggle = MainTab:CreateToggle({
    Name = "Auto-Collect",
    CurrentValue = false,
    Flag = "AutoCollect",
    Callback = function(state)
        getgenv().AutoCollect = state
        while getgenv().AutoCollect do
            for _, v in pairs(game.Workspace:GetDescendants()) do
                if v:IsA("Part") and v.Name == "Money" then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    wait(0.1)
                end
            end
        end
    end
})

-- Zen Hub Draggable Button
local ZenHubButton = Window:CreateButton({
    Name = "Toggle Zen Hub",
    Callback = function()
        Window:Toggle()
    end
})
ZenHubButton.Position = UDim2.new(0, 0, 0.5, 0)

-- Finish
Rayfield:Notify({
    Title = "Zen Hub",
    Content = "Script Loaded Successfully!",
    Duration = 5
})

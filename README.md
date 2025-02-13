local player = game.Players.LocalPlayer  
local replicatedStorage = game:GetService("ReplicatedStorage")  
local runService = game:GetService("RunService")  

-- Δημιουργία GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.CoreGui

-- Δημιουργία κουμπιού Auto Farm
local farmButton = Instance.new("TextButton")
farmButton.Parent = screenGui
farmButton.Size = UDim2.new(0, 200, 0, 50)
farmButton.Position = UDim2.new(0.5, -100, 0.1, 0)
farmButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
farmButton.Text = "Auto Farm: OFF"
farmButton.Font = Enum.Font.SourceSansBold
farmButton.TextSize = 20
farmButton.TextColor3 = Color3.new(0, 0, 0)

-- Δημιουργία κουμπιού Auto Quest
local questButton = Instance.new("TextButton")
questButton.Parent = screenGui
questButton.Size = UDim2.new(0, 200, 0, 50)
questButton.Position = UDim2.new(0.5, -100, 0.2, 0)
questButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
questButton.Text = "Auto Quest: OFF"
questButton.Font = Enum.Font.SourceSansBold
questButton.TextSize = 20
questButton.TextColor3 = Color3.new(0, 0, 0)

local autoFarmActive = false  
local autoQuestActive = false  

-- Auto Farm για Shiny Relic
local function autoFarmShinyRelic()  
    while autoFarmActive do  
        for _, v in pairs(workspace:GetChildren()) do  
            if v:IsA("Model") and v:FindFirstChild("Coin") and v.Name == "Shiny Relic" then  
                replicatedStorage.Network.CollectCoin:FireServer(v)  
            end  
        end  
        task.wait(1)  
    end  
end  

-- Auto Quest για Rank
local function autoQuest()  
    while autoQuestActive do  
        replicatedStorage.Network.CompleteQuest:FireServer("Rank")  
        task.wait(3)  -- Περιμένει 3 δευτερόλεπτα πριν ξαναδοκιμάσει
    end  
end  

-- Toggle Auto Farm κουμπί
farmButton.MouseButton1Click:Connect(function()  
    autoFarmActive = not autoFarmActive  

    if autoFarmActive then  
        farmButton.Text = "Auto Farm: ON"  
        farmButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)  
        spawn(autoFarmShinyRelic)  
    else  
        farmButton.Text = "Auto Farm: OFF"  
        farmButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)  
    end  
end)  

-- Toggle Auto Quest κουμπί
questButton.MouseButton1Click:Connect(function()  
    autoQuestActive = not autoQuestActive  

    if autoQuestActive then  
        questButton.Text = "Auto Quest: ON"  
        questButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)  
        spawn(autoQuest)  
    else  
        questButton.Text = "Auto Quest: OFF"  
        questButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)  
    end  
end)  

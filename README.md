local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local autoSlap = false
local minimized = false

local function attack(target, vec)
    if target and target.Character and LP.Character:FindFirstChild("Skull_Glove") then
        local args = {"slash", target.Character, vec}
        LP.Character.Skull_Glove.Event:FireServer(unpack(args))
    end
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BC9_Custom"
ScreenGui.Parent = LP:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Parent = ScreenGui
Main.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Main.BorderSizePixel = 0
Main.Position = UDim2.new(0.3, 0, 0.3, 0)
Main.Size = UDim2.new(0, 200, 0, 280)
Main.Active = true
Main.Draggable = true
Main.ClipsDescendants = true

local RGBStroke = Instance.new("UIStroke")
RGBStroke.Parent = Main
RGBStroke.Thickness = 2
RGBStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 8)
Corner.Parent = Main

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Parent = Main
Title.Size = UDim2.new(1, 0, 0, 35)
Title.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Title.Text = "BC9 HUB"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 18
Title.Font = Enum.Font.GothamBold

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 8)
TitleCorner.Parent = Title

local MinBtn = Instance.new("TextButton")
MinBtn.Name = "MinBtn"
MinBtn.Parent = Title
MinBtn.BackgroundTransparency = 1
MinBtn.Position = UDim2.new(1, -30, 0, 0)
MinBtn.Size = UDim2.new(0, 30, 1, 0)
MinBtn.Text = "-"
MinBtn.TextColor3 = Color3.new(1, 1, 1)
MinBtn.TextSize = 25
MinBtn.Font = Enum.Font.GothamBold

MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        Main:TweenSize(UDim2.new(0, 200, 0, 35), "Out", "Quad", 0.3, true)
        MinBtn.Text = "+"
    else
        Main:TweenSize(UDim2.new(0, 200, 0, 280), "Out", "Quad", 0.3, true)
        MinBtn.Text = "-"
    end
end)

local function createButton(name, pos, color, callback)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Parent = Main
    btn.Position = pos
    btn.Size = UDim2.new(0.9, 0, 0, 35)
    btn.AnchorPoint = Vector2.new(0.5, 0)
    btn.Position = UDim2.new(0.5, 0, pos.Y.Scale, pos.Y.Offset)
    btn.BackgroundColor3 = color
    btn.Text = name
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 14
    btn.AutoButtonColor = true
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = btn
    
    btn.MouseButton1Click:Connect(callback)
    return btn
end

createButton("SLAP ALL", UDim2.new(0.5, 0, 0, 50), Color3.fromRGB(40, 120, 40), function()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP then attack(p, Vector3.new(-3.492, -4.429, -4.145)) end
    end
end)

createButton("KILL ALL", UDim2.new(0.5, 0, 0, 95), Color3.fromRGB(120, 40, 40), function()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP then attack(p, Vector3.new(999999, 999999, 999999)) end
    end
end)

createButton("TP TO SLAP", UDim2.new(0.5, 0, 0, 140), Color3.fromRGB(40, 60, 120), function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.CFrame = CFrame.new(-12.254, -49.0, 420.262)
    end
end)

local autoBtn = createButton("AUTO SLAP: OFF", UDim2.new(0.5, 0, 0, 185), Color3.fromRGB(50, 50, 50), function()
    autoSlap = not autoSlap
    if autoSlap then
        autoBtn.Text = "AUTO SLAP: ON"
        autoBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    else
        autoBtn.Text = "AUTO SLAP: OFF"
        autoBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    end
end)

createButton("INF MONEY", UDim2.new(0.5, 0, 0, 230), Color3.fromRGB(150, 120, 0), function()
    game:GetService("ReplicatedStorage"):WaitForChild("CratesUtilities"):WaitForChild("Remotes"):WaitForChild("GiveReward"):FireServer(math.huge)
end)

task.spawn(function()
    while task.wait(0.1) do
        if autoSlap then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LP then attack(p, Vector3.new(-3.492, -4.429, -4.145)) end
            end
        end
    end
end)

RunService.RenderStepped:Connect(function()
    local hue = tick() % 5 / 5
    local color = Color3.fromHSV(hue, 1, 1)
    RGBStroke.Color = color
end)

print("author: BC_rider9")

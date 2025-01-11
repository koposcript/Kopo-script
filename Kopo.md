
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local TeleportService = game:GetService("TeleportService")

local LocalPlayer = Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KopoScript"
ScreenGui.Parent = CoreGui

local ToggleButton = Instance.new("TextButton")
local MainFrame = Instance.new("ScrollingFrame")
local UIListLayout = Instance.new("UIListLayout")
local Title = Instance.new("TextLabel")
local SpeedBox = Instance.new("TextBox")
local JumpBox = Instance.new("TextBox")

local function addUICorner(instance, cornerRadius)
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, cornerRadius)
    UICorner.Parent = instance
end

MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0, 195, 0, 195)
MainFrame.Position = UDim2.new(0, 160, 0, -10)
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainFrame.BorderSizePixel = 4
MainFrame.ScrollBarThickness = 8
MainFrame.Visible = true
MainFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
addUICorner(MainFrame, 10)

UIListLayout.Parent = MainFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)

Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 0.50
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 24
Title.Text = "KopoScript V0.1"
addUICorner(Title, 10)

local function createButton(parent, text, color, callback)
    local Button = Instance.new("TextButton")
    Button.Parent = parent
    Button.Size = UDim2.new(1, -20, 0, 40)
    Button.BackgroundColor3 = color
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.SourceSansBold
    Button.TextSize = 14
    Button.Text = text
    Button.MouseButton1Click:Connect(callback)
    addUICorner(Button, 10)
    return Button
end
local playersInvisible = false

createButton(MainFrame, "Esconder Players", Color3.fromRGB(128, 128, 128), function()
    playersInvisible = not playersInvisible
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Transparency = playersInvisible and 1 or 0 -- Прозрачность
                    part.CanCollide = not playersInvisible -- Возможность столкновения
                elseif part:IsA("Decal") then
                    part.Transparency = playersInvisible and 1 or 0 -- Прозрачность текстур
                end
            end
        end
    end
end)

local gravityEnabled = true
local gravityValue = workspace.Gravity

createButton(MainFrame, "Modifique o Peso.", Color3.fromRGB(255, 69, 0), function()
    gravityEnabled = not gravityEnabled
    if gravityEnabled then
        workspace.Gravity = gravityValue -- Возвращаем к изначальному значению
    else
        workspace.Gravity = 0 -- Устанавливаем гравитацию в 0
    end
end)

local GravityBox = Instance.new("TextBox")
GravityBox.Parent = MainFrame
GravityBox.Size = UDim2.new(1, -20, 0, 30)
GravityBox.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
GravityBox.TextColor3 = Color3.fromRGB(255, 255, 255)
GravityBox.Font = Enum.Font.SourceSansBold
GravityBox.TextSize = 14
GravityBox.PlaceholderText = "Digite o valor do peso da bola."
addUICorner(GravityBox, 10)

GravityBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local gravity = tonumber(GravityBox.Text)
        if gravity then
            gravityValue = gravity
            if gravityEnabled then
                workspace.Gravity = gravityValue
            end
        end
    end
end)

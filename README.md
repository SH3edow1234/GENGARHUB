local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

function getClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local targetPosition = player.Character.Head.Position
            local screenPoint, onScreen = workspace.CurrentCamera:WorldToScreenPoint(targetPosition)
            if onScreen then
                local distance = (Vector2.new(screenPoint.X, screenPoint.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                if distance < shortestDistance then
                    closestPlayer = player
                    shortestDistance = distance
                end
            end
        end
    end

    return closestPlayer
end

game:GetService("RunService").RenderStepped:Connect(function()
    local target = getClosestPlayer()
    if target and target.Character and target.Character:FindFirstChild("Head") then
        local targetPosition = target.Character.Head.Position
        local screenPoint = workspace.CurrentCamera:WorldToScreenPoint(targetPosition)
        
        -- Move a mira direto para o alvo (100% lock-on)
        mousemoverel(screenPoint.X - Mouse.X, screenPoint.Y - Mouse.Y)
    end
end)

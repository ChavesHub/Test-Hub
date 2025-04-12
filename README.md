local teleport_table = {  
    location1 = Vector3.new(0, 7, 0),  
    location2 = Vector3.new(0, 3, 0)  
}  

local TweenService = game:GetService("TweenService")  
local LocalPlayer = game.Players.LocalPlayer or game.Players:GetPlayers()[1] -- Fallback se LocalPlayer der problema  
local base_speed = 100  

local function bypass_teleport(destination, callback)  
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")

    local distance = (root.Position - destination).Magnitude  
    local duration = distance / base_speed  

    local tween = TweenService:Create(root, TweenInfo.new(duration, Enum.EasingStyle.Linear), {CFrame = CFrame.new(destination)})  
    tween:Play()  

    tween.Completed:Once(function()  
        if callback then  
            callback()  
        end  
    end)
end  

-- Execução automática  
print("Iniciando voo...")  
bypass_teleport(teleport_table.location1, function()  
    bypass_teleport(teleport_table.location2)  
end)

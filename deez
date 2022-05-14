if _G["OnlyFarmSpins"] == nil then
    _G.OnlyFarmSpins = false
end
if _G["WantedMagics"] == nil then
    _G.WantedMagics = {"Time","Reality Collapse","","","","","","","",""}
end
if _G["WantedRarities"] == nil then
    _G.WantedRarities = {"Heavenly","","","","",""}
end

if game.Players.LocalPlayer == nil then
   game.Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
end

for i,v in pairs(getconnections(game:GetService("Players").LocalPlayer.Idled)) do
   v:Disable()
end

local Debounce = false
local FullStop = false
local HasGamepass = game:GetService("MarketplaceService"):UserOwnsGamePassAsync(game.Players.LocalPlayer.UserId,20164545)
repeat wait() until game.ReplicatedStorage:FindFirstChild("Events")
local SpawnRemote = game.ReplicatedStorage.Events:WaitForChild("Spawn")

game:GetService('RunService').Stepped:connect(function()
    if not Debounce and not FullStop and game.Players.LocalPlayer:FindFirstChild("PlayerGui") then
        Debounce = true
        local StatsGui = nil
        local PlayButton = nil
        for i,v in pairs(game.Players.LocalPlayer.PlayerGui:GetDescendants()) do
            if v.Name == "StatsGUI" then
                StatsGui = v
            end
        end
        if SpawnRemote.ClassName == "RemoteFunction" then -- changed to a remote function after the latest update, this is for compatibility
            SpawnRemote:InvokeServer()
        elseif SpawnRemote.ClassName == "RemoteEvent" then
            pcall(function()
                local Events = getconnections(game.Players.LocalPlayer.PlayerGui.MainGUI.Start.PlayButton.MouseButton1Click)
                for i,v in pairs(Events) do
                    v:Fire()
                end
            end)
            SpawnRemote:FireServer()
        end
        wait(0.2)
        if StatsGui ~= nil then
            if StatsGui:FindFirstChild("Level") and StatsGui.Level:FindFirstChild("Level") then
                local Level = tonumber(StatsGui.Level.Level.Text)
                if Level ~= nil and Level <= 1 and game.Players.LocalPlayer.Character ~= nil and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local Tool = game.Players.LocalPlayer.Backpack:FindFirstChildOfClass("Tool") or game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool")
                    if Tool ~= nil then
                        Tool.Parent = game.Players.LocalPlayer.Character
                        --Tool:Activate()
                        game:GetService("ReplicatedStorage").Events.SpellCast:FireServer({Tool,game.Players.LocalPlayer.Character.HumanoidRootPart.Position,Vector3.new(0,0,0),true})
                        wait()
                        game:GetService("ReplicatedStorage").Events.SpellCast:FireServer({Tool,game.Players.LocalPlayer.Character.HumanoidRootPart.Position})
                        --Tool:Deactivate()
                        wait(0.1)
                    end
                elseif Level ~= nil and Level > 0 and Level > 1 and Level < 900 then
                    wait(0.1)
                    if Level < 2 then
                        Debounce = false
                        return
                    end
                    local Magic, Rarity = game:GetService("ReplicatedStorage").Events.Spin:InvokeServer(false)
                    print("Rolled "..Magic.." with a rarity of "..Rarity)
                    if table.find(_G.WantedMagics,Magic) or table.find(_G.WantedRarities,Rarity) then
                        if _G.OnlyFarmSpins == false then
                            game.Players.LocalPlayer.Character:BreakJoints()
                            FullStop = true
                            return
                        end
                    end
                    game.Players.LocalPlayer.Character:BreakJoints()
                    if SpawnRemote.ClassName == "RemoteFunction" then -- changed to a remote function after the latest update, this is for compatibility
                        SpawnRemote:InvokeServer()
                    elseif SpawnRemote.ClassName == "RemoteEvent" then
                        SpawnRemote:FireServer()
                    end
                    wait(0.1)
                elseif Level > 900 then
                    game.Players.LocalPlayer.Character:BreakJoints()
                end
            end
        end
        Debounce = false
    end
end)

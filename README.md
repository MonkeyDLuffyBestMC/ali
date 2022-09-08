--- Equip/UnEquip
local LightningBolt = require(game.ReplicatedStorage.LightningBolt)
local LightningSparks = require(game.ReplicatedStorage.LightningBolt.LightningSparks)
 
local Tool = script.Parent
 
Tool.Equipped:Connect(function()
    Tool.Equip.Value = true
end)
 
Tool.Unequipped:Connect(function()
    Tool.Equip.Value = false
end)
 
local UIS = game:GetService("UserInputService")
local plr = game.Players.LocalPlayer
local Mouse = plr:GetMouse()
Player = game.Players.LocalPlayer
local Track1 : Animation = script:WaitForChild("Anim01")
Track1 = Player.Character:WaitForChild("Humanoid"):LoadAnimation(script.Anim01)
local Track2 : Animation = script:WaitForChild("Anim02")
Track2 = Player.Character:WaitForChild("Humanoid"):LoadAnimation(script.Anim02)
local Track3 : Animation = script:WaitForChild("Anim03")
Track3 = Player.Character:WaitForChild("Humanoid"):LoadAnimation(script.Anim03)
local Track4 : Animation = script:WaitForChild("Anim04")
Track4 = Player.Character:WaitForChild("Humanoid"):LoadAnimation(script.Anim04)
local Track5 : Animation = script:WaitForChild("Anim05")
Track5 = Player.Character:WaitForChild("Humanoid"):LoadAnimation(script.Anim05)
local PrevWalkSpeed = nil
local Tween = game:GetService("TweenService")
local Gui = Player.PlayerGui:WaitForChild("Rumble_Skill_List".. Player.Name):WaitForChild("Frame")
 
local CooldownEDur = 5
local CanUseE = true
local CooldownRDur = 5
local CanUseR = true
local CooldownXDur = 5
local CanUseZ = true
local CooldownZDur = 5
local CanUseX = true
local CooldownCDur = 5
local CanUseC = true
local CooldownDashDur = 0.2
local CanDash = true
 
local char = plr.Character
local Size = 20
 
local CooldownTDur = 5
local CanUseT = true
local HoldingT = false
local BetweenTime = 0
local AmountOfTP = 0
local GoroFlightDistancePerTick = 35
 
UIS.InputBegan:Connect(function(Input)
    local char = plr.Character
    if Input.KeyCode == Enum.KeyCode.Q and CanDash == true and plr.Character.Humanoid.FloorMaterial ~= nil and plr.Character.Humanoid.FloorMaterial ~= Enum.Material.Air and Tool.Active.Value == "None" then
        local position = plr.Character.PrimaryPart.Position
        local PartDash :Part = Instance.new("Part", workspace.Visuals)
        PartDash.CFrame = char.PrimaryPart.CFrame
        PartDash.CanCollide = false
        PartDash.Anchored = true
        PartDash.Transparency = 1
        local A1 = Instance.new("Attachment", PartDash)
        A1.WorldPosition = char.PrimaryPart.Position
        A1.Visible = false
        local A2 = Instance.new("Attachment", char.PrimaryPart)
        A2.Visible = false
        A2.Position = Vector3.new(0,0,1)
        local PointLight = Instance.new("PointLight", A2)
        local Sound = game.ReplicatedStorage.Goro.DashZap:Clone()
        Sound.Parent = char.PrimaryPart
        Sound:Play()
        game.Debris:AddItem(Sound,1)
        PointLight.Color = Color3.fromRGB(49, 235, 255)
        PointLight.Range = 0
        PointLight.Brightness = 30
        game.Debris:AddItem(PointLight, 1)
        game.Debris:AddItem(PartDash, 1)
 
        local Distance = 15
        Tween:Create(PointLight, TweenInfo.new(0.4), {Range = Distance, Brightness = 0}):Play()
        wait()
        if char.Humanoid.MoveDirection == Vector3.new(0,0,0) then
            local RayCast = Ray.new(char.PrimaryPart.Position, char.PrimaryPart.CFrame.LookVector * Distance)
            local PartDash, Pos = game.Workspace:FindPartOnRayWithIgnoreList(RayCast, {char, PartDash, workspace.Visuals})
            char.PrimaryPart.CFrame = CFrame.new(Pos.X, Pos.Y, Pos.Z) * CFrame.fromOrientation(char.PrimaryPart.Orientation.X*3.14/180,char.PrimaryPart.Orientation.Y*3.14/180,char.PrimaryPart.Orientation.Z*3.14/180)
        else
            local RayCast = Ray.new(char.PrimaryPart.Position, char.Humanoid.MoveDirection * Distance)
            local PartDash, Pos = game.Workspace:FindPartOnRayWithIgnoreList(RayCast, {char, PartDash, workspace.Visuals})
            char.PrimaryPart.CFrame = CFrame.new(Pos.X, Pos.Y, Pos.Z) * CFrame.fromOrientation(char.PrimaryPart.Orientation.X*3.14/180,char.PrimaryPart.Orientation.Y*3.14/180,char.PrimaryPart.Orientation.Z*3.14/180)
        end
        script.Fire:FireServer("GoroDash", position)
        position = nil
 
        CanDash = false
 
        local NewBolt = LightningBolt.new(A1, A2, 25)
        NewBolt.CurveSize0, NewBolt.CurveSize1 = 1, 1
        NewBolt.PulseSpeed = 10
        NewBolt.Thickness = 1
        NewBolt.PulseLength = 1
        NewBolt.FadeLength = 0.45
        NewBolt.RanNum = 10
        NewBolt.Frequency = 5
        NewBolt.Color = Color3.fromRGB(85, 145, 255)
 
        local NewSparks = LightningSparks.new(NewBolt, 20)
        wait(CooldownDashDur)
        CanDash = true
    elseif Input.KeyCode == Enum.KeyCode.E and CanUseE == true and Tool.Equip.Value == true and Tool.Active.Value == "None" then
        Tool.Active.Value = "1LightningParalyze"
        PrevWalkSpeed = Player.Character:WaitForChild("Humanoid").WalkSpeed
        Gui:FindFirstChild(Tool.Active.Value).Frame.Size = UDim2.new(1, 0, 1, 0)
        Player.Character:WaitForChild("Humanoid").WalkSpeed = 5
        Track1:Play()
        script.Fire:FireServer("holdE")
        sphere = game.ReplicatedStorage.Goro.Radius:Clone()
        local Weld = Instance.new("Weld", sphere)
        Weld.Part0 = plr.Character.PrimaryPart
        Weld.Part1 = sphere
        sphere.Parent = game.Workspace.Visuals
        sphere.Transparency = 1
        Size = math.clamp(10, 10, 40)
        delay(0.3, function()
            Track1:AdjustSpeed(0)
        end)
        if Tool.Active.Value == "1LightningParalyze" then
            for i = 1,320 do -- (Max Size - Min Size)*1/4
                if Tool.Active.Value == "1LightningParalyze" then
                    Size = math.clamp(Size + 1/4, 20, 100)
                    Tween:Create(sphere.Mesh, TweenInfo.new(0.1), {Scale = Vector3.new(Size * 0.5, Size * 0.5, Size * 0.5)}):Play()
                    Tween:Create(sphere, TweenInfo.new(0.5), {Transparency = 0}):Play()
                    wait(0.05)
                end
            end
        end
    elseif Input.KeyCode == Enum.KeyCode.R and CanUseR == true and Tool.Equip.Value == true and Tool.Active.Value == "None" then
        Tool.Active.Value = "2FlashSmash"
        char.PrimaryPart.Anchored = true
        LookAtMouse = true
        Gui:FindFirstChild(Tool.Active.Value).Frame.Size = UDim2.new(1, 0, 1, 0)
        Track2:Play()
        script.Fire:FireServer("holdR")
        delay(0.4, function()
            Track2:AdjustSpeed(0)
        end)
        while LookAtMouse == true and char.PrimaryPart.Anchored == true do
            plr.Character.PrimaryPart.CFrame = CFrame.lookAt(plr.Character.PrimaryPart.Position, Vector3.new(Mouse.Hit.p.X, plr.Character.PrimaryPart.Position.Y, Mouse.Hit.p.Z), Vector3.new(0,1,0))
            wait()
        end
    elseif Input.KeyCode == Enum.KeyCode.Z and CanUseZ == true and Tool.Equip.Value == true and Tool.Active.Value == "None" then
        Tool.Active.Value = "3ElThor"
        char.PrimaryPart.Anchored = true
        PrevWalkSpeed = Player.Character:WaitForChild("Humanoid").WalkSpeed
        Gui:FindFirstChild(Tool.Active.Value).Frame.Size = UDim2.new(1, 0, 1, 0)
        Player.Character:WaitForChild("Humanoid").WalkSpeed = 5
        Track3:Play()
        script.Fire:FireServer("holdZ")
        Track3:AdjustSpeed(0)
        LookAtMouse = true
        while LookAtMouse == true do
            plr.Character.PrimaryPart.CFrame = CFrame.lookAt(plr.Character.PrimaryPart.Position, Vector3.new(Mouse.Hit.p.X, plr.Character.PrimaryPart.Position.Y, Mouse.Hit.p.Z), Vector3.new(0,1,0))
            wait()
        end
 
    elseif Input.KeyCode == Enum.KeyCode.X and CanUseX == true and Tool.Equip.Value == true and Tool.Active.Value == "None" then
        Tool.Active.Value = "4LightningDragon"
        char.PrimaryPart.Anchored = true
        PrevWalkSpeed = Player.Character:WaitForChild("Humanoid").WalkSpeed
        Gui:FindFirstChild(Tool.Active.Value).Frame.Size = UDim2.new(1, 0, 1, 0)
        Player.Character:WaitForChild("Humanoid").WalkSpeed = 5
        Track4:Play()
        script.Fire:FireServer("holdX")
        delay(0.15, function()
            Track4:AdjustSpeed(0)
        end)
        LookAtMouse = true
        while LookAtMouse == true do
            plr.Character.PrimaryPart.CFrame = CFrame.lookAt(plr.Character.PrimaryPart.Position, Vector3.new(Mouse.Hit.p.X, plr.Character.PrimaryPart.Position.Y, Mouse.Hit.p.Z), Vector3.new(0,1,0))
            wait()
        end
 
    elseif Input.KeyCode == Enum.KeyCode.C and CanUseC == true and Tool.Equip.Value == true and Tool.Active.Value == "None" then
        Tool.Active.Value = "5GoroUlt"
        PrevWalkSpeed = Player.Character:WaitForChild("Humanoid").WalkSpeed
        Gui:FindFirstChild(Tool.Active.Value).Frame.Size = UDim2.new(1, 0, 1, 0)
        Player.Character:WaitForChild("Humanoid").WalkSpeed = 5
        Track5:Play()
        script.Fire:FireServer("holdC")
        delay(0.55, function()
            Track5:AdjustSpeed(0)
        end)
        LookAtMouse = true
        while LookAtMouse == true do
            plr.Character.PrimaryPart.CFrame = CFrame.lookAt(plr.Character.PrimaryPart.Position, Vector3.new(Mouse.Hit.p.X, plr.Character.PrimaryPart.Position.Y, Mouse.Hit.p.Z), Vector3.new(0,1,0))
            wait()
        end
    elseif Input.KeyCode == Enum.KeyCode.T and Tool.Active.Value == "None" and Tool.Equip.Value == true then
        Tool.Active.Value = "6GoroFlight"
        HoldingT = true
        AmountOfTP = 0
        BetweenTime = 0
        local A3 = Instance.new("Attachment", plr.Character.PrimaryPart)
        A3.Name = "AmongUsAttachmentSuspicious:O"
        local Particle1 = game.ReplicatedStorage.Goro.Electric2:Clone()
        Particle1.Parent = A3
        Particle1.Enabled = true
        local Particle2 = game.ReplicatedStorage.Goro.Flash:Clone()
        Particle2.Parent = A3
        Particle2.Enabled = true
        Gui:FindFirstChild(Tool.Active.Value).Frame.Size = UDim2.new(1, 0, 1, 0)
        script.Fire:FireServer("GoroFlight1", plr.Character)
        for _, ItemF in pairs(plr.Character:GetDescendants()) do
            if ItemF:IsA("MeshPart") or ItemF:IsA("Part") or ItemF:IsA("Union") then
                if ItemF.Name ~= "HumanoidRootPart" then
                    ItemF.Transparency = ItemF.Transparency + 10
                    local ElectricParticle = game.ReplicatedStorage.Goro.ElectricParticle:Clone()
                    ElectricParticle.Enabled = true
                    ElectricParticle.Parent = ItemF 
                end
            end
        end 
        while HoldingT == true do
            AmountOfTP = AmountOfTP + 1
            plr.Character.PrimaryPart.Anchored = true
            local ray = Ray.new(plr.Character.PrimaryPart.Position, CFrame.new(plr.Character.PrimaryPart.Position, Mouse.Hit.p).LookVector * GoroFlightDistancePerTick)
            local HitPart, HitPos = game.Workspace:FindPartOnRayWithIgnoreList(ray, {game.Workspace.Visuals, plr.Character})
            local position = plr.Character.PrimaryPart.Position
            local PartDash :Part = Instance.new("Part", workspace.Visuals)
            plr.Character.PrimaryPart.CFrame = CFrame.lookAt(plr.Character.PrimaryPart.Position, HitPos)
            PartDash.CFrame = char.PrimaryPart.CFrame
            PartDash.CanCollide = false
            PartDash.Anchored = true
            PartDash.Transparency = 1
            local A1 = Instance.new("Attachment", PartDash)
            A1.WorldPosition = char.PrimaryPart.Position
            A1.Visible = false
            local A2 = Instance.new("Attachment", char.PrimaryPart)
            A2.Visible = false
            A2.Position = Vector3.new(0,0,1)
            local Particle1 = game.ReplicatedStorage.Goro.Electric2:Clone()
            Particle1.Parent = A2
            local Particle2 = game.ReplicatedStorage.Goro.Flash:Clone()
            Particle2.Parent = A2
            local PointLight = Instance.new("PointLight", A2)
            local Sound = game.ReplicatedStorage.Goro.DashZap:Clone()
            Sound.Parent = char.PrimaryPart
            Sound:Play()
            game.Debris:AddItem(Sound,1)
            PointLight.Color = Color3.fromRGB(49, 235, 255)
            PointLight.Range = 0
            PointLight.Brightness = 30
            game.Debris:AddItem(PointLight, 1)
            game.Debris:AddItem(PartDash, 1)
 
            local Distance = 15
            Tween:Create(PointLight, TweenInfo.new(0.4), {Range = Distance, Brightness = 0}):Play()
            wait()
            Particle1:Emit(50)
            Particle2:Emit(5)
            char.PrimaryPart.CFrame = CFrame.new(HitPos.X, HitPos.Y + 4, HitPos.Z) * CFrame.fromOrientation(char.PrimaryPart.Orientation.X*3.14/180,char.PrimaryPart.Orientation.Y*3.14/180,char.PrimaryPart.Orientation.Z*3.14/180)
            script.Fire:FireServer("GoroFlight2", position, HitPos)
            position = nil
            
            local NewBolt = LightningBolt.new(A1, A2, 25)
            NewBolt.CurveSize0, NewBolt.CurveSize1 = 1, 1
            NewBolt.PulseSpeed = 10
            NewBolt.Thickness = 1
            NewBolt.PulseLength = 2
            NewBolt.FadeLength = 0.45
            NewBolt.RanNum = 10
            NewBolt.Frequency = 5
            NewBolt.Color = Color3.fromRGB(85, 145, 255)
            NewBolt.MaxRadius = 4
            local NewSparks = LightningSparks.new(NewBolt, 20)      
            
            wait(BetweenTime)
            
            if BetweenTime == 0 then
                BetweenTime = 1
            else
                BetweenTime = math.clamp(BetweenTime - BetweenTime/2, 0.25, 2)
            end
        end
    end
end)
 
UIS.InputEnded:Connect(function(Input)
    if Input.KeyCode == Enum.KeyCode.E and CanUseE == true and Tool.Equip.Value == true and Tool.Active.Value == "1LightningParalyze" then
        Player.Character:WaitForChild("1LightningParalyzeValue", 1200)
        CanUseE = false
        sphere.Mesh.Scale = Vector3.new(0,0,0)
        Track1:AdjustSpeed(1)
        wait(0.1)
        sphere.Mesh.Scale = Vector3.new(0,0,0)
        Tween:Create(sphere.Mesh, TweenInfo.new(0.5), {Scale = Vector3.new(Size, Size, Size)}):Play()
        Tween:Create(sphere, TweenInfo.new(0.5), {Transparency = 1}):Play()
        game.Debris:AddItem(sphere, 2)
        script.Fire:FireServer("releaseE", plr.Character.PrimaryPart.CFrame, Size)
        Size = 10
        Tween:Create(Gui:FindFirstChild(Tool.Active.Value).Frame, TweenInfo.new(CooldownEDur, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 1, 0)}):Play()
        Tool.Active.Value = "None"
        Player.Character:WaitForChild("Humanoid").WalkSpeed = PrevWalkSpeed
        wait(CooldownEDur)
        CanUseE = true
    elseif Input.KeyCode == Enum.KeyCode.R and CanUseR == true and Tool.Equip.Value == true and Tool.Active.Value == "2FlashSmash" then
        LookAtMouse = false
        Player.Character:WaitForChild("2FlashSmashValue", 1200)
        CanUseR = false
        Track2:AdjustSpeed(1)
        local PrevCFrame =  plr.Character.PrimaryPart.CFrame
        local ray = Ray.new(plr.Character.PrimaryPart.Position, CFrame.new(plr.Character.PrimaryPart.Position, Mouse.Hit.p).LookVector * 100)
        local HitPart, HitPos = game.Workspace:FindPartOnRayWithIgnoreList(ray, {game.Workspace.Visuals, plr.Character})
        wait(0.1)
        delay(0.8, function()
            char.PrimaryPart.Anchored = false
        end)
        script.Fire:FireServer("releaseR", PrevCFrame, HitPos, plr.Character.PrimaryPart.Position)
        Tween:Create(Gui:FindFirstChild(Tool.Active.Value).Frame, TweenInfo.new(CooldownRDur, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 1, 0)}):Play()
        Tool.Active.Value = "None"
        wait(CooldownRDur)
        CanUseR = true
    elseif Input.KeyCode == Enum.KeyCode.Z and CanUseZ == true and Tool.Equip.Value == true and Tool.Active.Value == "3ElThor" then
        Player.Character:WaitForChild("3ElThorValue", 1200)
        CanUseZ = false
        LookAtMouse = false
        Track3:AdjustSpeed(1)
        local ray = Ray.new(plr.Character.PrimaryPart.Position, CFrame.new(plr.Character.PrimaryPart.Position, Mouse.Hit.p).LookVector * 500)
        local HitPart, HitPos = game.Workspace:FindPartOnRayWithIgnoreList(ray, {game.Workspace.Visuals, plr.Character})
        delay(0.1, function()
            char.PrimaryPart.Anchored = false
            script.Fire:FireServer("releaseZ", HitPos)
            Tween:Create(Gui:FindFirstChild(Tool.Active.Value).Frame, TweenInfo.new(CooldownZDur, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 1, 0)}):Play()
            Tool.Active.Value = "None"
            Player.Character:WaitForChild("Humanoid").WalkSpeed = PrevWalkSpeed
            wait(CooldownZDur)
            CanUseZ = true  
        end)
    elseif Input.KeyCode == Enum.KeyCode.X and CanUseX == true and Tool.Equip.Value == true and Tool.Active.Value == "4LightningDragon" then
        Player.Character:WaitForChild("4LightningDragonValue", 1200)
        CanUseX = false
        LookAtMouse = false
        Track4:AdjustSpeed(1)
        local ray = Ray.new(plr.Character.PrimaryPart.Position, CFrame.new(plr.Character.PrimaryPart.Position, Mouse.Hit.p).LookVector * 500)
        local HitPart, HitPos = game.Workspace:FindPartOnRayWithIgnoreList(ray, {game.Workspace.Visuals, plr.Character})
        delay(0.1, function()
            char.PrimaryPart.Anchored = false
            script.Fire:FireServer("releaseX", HitPos)
            Tween:Create(Gui:FindFirstChild(Tool.Active.Value).Frame, TweenInfo.new(CooldownXDur, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 1, 0)}):Play()
            Tool.Active.Value = "None"
            Player.Character:WaitForChild("Humanoid").WalkSpeed = PrevWalkSpeed
            wait(CooldownXDur)
            CanUseX = true  
            delay(0.05, function()
                Track4:AdjustSpeed(1)
            end)
        end)
    elseif Input.KeyCode == Enum.KeyCode.C and CanUseC == true and Tool.Equip.Value == true and Tool.Active.Value == "5GoroUlt" then
        Player.Character:WaitForChild("5GoroUltValue", 1200)
        CanUseC = false
        LookAtMouse = false
        Track5:AdjustSpeed(1)
        local ray = Ray.new(plr.Character.PrimaryPart.Position, CFrame.new(plr.Character.PrimaryPart.Position, Mouse.Hit.p).LookVector * 1200)
        local HitPart, HitPos = game.Workspace:FindPartOnRayWithIgnoreList(ray, {game.Workspace.Visuals, plr.Character})
        delay(0.1, function()
            script.Fire:FireServer("releaseC", plr.Character.PrimaryPart.CFrame, HitPos)
            Tween:Create(Gui:FindFirstChild(Tool.Active.Value).Frame, TweenInfo.new(CooldownCDur, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 1, 0)}):Play()
            Tool.Active.Value = "None"
            Player.Character:WaitForChild("Humanoid").WalkSpeed = PrevWalkSpeed
            wait(CooldownCDur)
            CanUseC = true  
        end)
    elseif Input.KeyCode == Enum.KeyCode.T and Tool.Active.Value == "6GoroFlight" and Tool.Equip.Value == true and CanUseT == true then
        HoldingT = false
        script.Fire:FireServer("GoroFlight3", plr.Character)
        for _, ItemF in pairs(plr.Character:GetDescendants()) do
            if ItemF.Name ~= "HumanoidRootPart" then
                if ItemF:IsA("MeshPart") or ItemF:IsA("Part") or ItemF:IsA("Union") then
                ItemF.Transparency = ItemF.Transparency - 10
                if ItemF:FindFirstChild("ElectricParticle") then
                    ItemF.ElectricParticle:Destroy()
                end
            elseif ItemF.Name == "AmongUsAttachmentSuspicious:O" then
                ItemF:FindFirstChild("Flash").Enabled = false
                ItemF:FindFirstChild("Electric2").Enabled = false
                game.Debris:AddItem(ItemF, 1)
            end
            end
        end 
        delay(0.4, function()
            Tween:Create(Gui:FindFirstChild(Tool.Active.Value).Frame, TweenInfo.new(CooldownCDur, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 1, 0)}):Play()
            CanUseT = false
            Tool.Active.Value = "None"
            plr.Character.PrimaryPart.Anchored = false
            BetweenTime = 0
            AmountOfTP = 0
            wait(CooldownTDur)
            CanUseT = true
        end)
    end
end)

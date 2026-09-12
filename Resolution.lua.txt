repeat task.wait() until game:IsLoaded()

task.spawn(function()
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/xqmt/Resolution-loader/refs/heads/main/Bypass.lua", true))()
    end)
end)

task.wait(2)

local repo = "https://raw.githubusercontent.com/cloudsense-pub/UELinoriaLib/main/"
local Library = loadstring(game:HttpGet(repo .. "Library.lua"))()
local ThemeManager = loadstring(game:HttpGet(repo .. "addons/ThemeManager.lua"))()
local SaveManager = loadstring(game:HttpGet(repo .. "addons/SaveManager.lua"))()

Library.Font = Enum.Font.Cartoon
Library.MainFont = Enum.Font.Cartoon 

local GlobalFontSetting = Enum.Font.Cartoon
local ESPFontSetting = Enum.Font.Cartoon

local OldNotify = Library.Notify
Library.Notify = function(self, text, time)
    local notification = OldNotify(self, text, time)
    pcall(function()
        local container = Library.NotificationArea or Library.Notifications
        if container then
            for _, desc in ipairs(container:GetDescendants()) do
                if desc:IsA("TextLabel") or desc:IsA("TextButton") then
                    desc.Font = GlobalFontSetting
                end
            end
        end
    end)
    return notification
end

Library.ShowToggleFrameInKeybinds = true
Library.ShowCustomCursor = true
Library.NotifySide = "Right" 

local Window = Library:CreateWindow({
    Title = "Resolution.flyㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤ",
    Center = true, 
    AutoShow = true,
    Resizable = true,
    ShowCustomCursor = true,
    UnlockMouseWhileOpen = true,
    NotifySide = "Right", 
    TabPadding = 8,
    MenuFadeTime = 0.2
})

local SilentAimEnabled = false
local ResolverEnabled = false
local BulletTPEnabled = false
local WallCheckEnabled = false
local TeamCheckEnabled = false
local DieCheckEnabled = true
local KnockCheckEnabled = false
local LookAtEnabled = false
local FOVSize = 100
local FOVVisible = false
local TracerEnabled = false

local VoidEnabled = false

local AutoPredictionEnabled = false
local PredictionValue = 0.1

local ManualLockEnabled = false
local ManualLockedTarget = nil

local CustomFOVValue = 70
local FOVOriginMode = "Center"
local FOVLerpSpeed = 0.5

local RobloxFontsList = {}
for _, font in ipairs(Enum.Font:GetEnumItems()) do
    table.insert(RobloxFontsList, font.Name)
end
table.sort(RobloxFontsList)

local HitSoundMode = "None"
local HitSoundVolume = 2
local SoundIDs = {
    ["None"] = "None",
    ["Spark"] = "rbxassetid://110241936966089",
    ["Neverlose"] = "rbxassetid://18391691942",
    ["Rust"] = "rbxassetid://138750331387064",
    ["Break"] = "rbxassetid://103059835203534",
    ["Fatality"] = "rbxassetid://17802817724",
    ["Primordial"] = "rbxassetid://97511223764004",
    ["Minecraft xp"] = "rbxassetid://17148249625",
    ["Minecraft Bow"] = "rbxassetid://135478009117226",
    ["Crack"] = "rbxassetid://126409451844008",
    ["Pop"] = "rbxassetid://140323850218372",
    ["Headshot"] = "rbxassetid://5764885315",
    ["Manbo"] = "rbxassetid://102257186883445",
    ["Skeet"] = "rbxassetid://80461265049096"
}

local HitNotifyEnabled = false 
local HitNotifyDuration = 2.5
local HitNotifyTemplate = "Hit {name} in the {part} for {dmg} dmg with {weapon}" 

local HitOverlayEnabled = false
local HitOverlayColor = Color3.fromRGB(255, 0, 0)

local SelfHighlightEnabled = false
local SelfHighlightMainColor = Color3.fromRGB(0, 255, 150)
local SelfHighlightOutlineColor = Color3.fromRGB(255, 255, 255)
local SelfHighlightFillTrans = 0.5
local SelfHighlightOutlineTrans = 0.0

local SkyboxEnabled = false
local SelectedSkybox = "Minecraft"
local SkyboxIDs = {
    ["Minecraft"] = "rbxassetid://96736589365838",
    ["Minecraft 2"] = "rbxassetid://117301665698887",
    ["dark star"] = "rbxassetid://4951222008",
    ["snow"] = "rbxassetid://4604073339",
    ["realistic"] = "rbxassetid://8464073887",
    ["solid black"] = "rbxassetid://12300321824",
    ["blue"] = "rbxassetid://90988519"
}

local AuraEnabled = false
local SelectedAuras = {} 
local CurrentAuraColor = Color3.fromRGB(133, 220, 255)
local ActiveParticles = {}

local ParticleAuras = {
	["starlight"] = game:GetObjects("rbxassetid://134645216613107")[1],
	["Heart"]     = game:GetObjects("rbxassetid://107538567420590")[1],
	["heavenly"]  = game:GetObjects("rbxassetid://139300897520961")[1],
	["ribbon"]    = game:GetObjects("rbxassetid://132069507632161")[1],
	["sakura"]    = game:GetObjects("rbxassetid://81755778619404")[1],
	["angel"]     = game:GetObjects("rbxassetid://97658130917593")[1],
	["wind"]      = game:GetObjects("rbxassetid://80694081850877")[1],
	["flow"]      = game:GetObjects("rbxassetid://119913533725648")[1],
	["star"]      = game:GetObjects("rbxassetid://73754563740680")[1],
    ["Fire"]      = game:GetObjects("rbxassetid://88798731377035")[1],
}
local AuraOptions = {"starlight", "Heart", "heavenly", "ribbon", "sakura", "angel", "wind", "flow", "star", "Fire"}


local FOVCircleColor = Color3.fromRGB(255, 255, 255)
local FOVCircleOutlineColor = Color3.fromRGB(0, 0, 0)
local FOVThickness = 1.0
local FOVOutlineThickness = 3.0
local FOVTransparency = 1.0
local FOVOutlineTransparency = 1.0

local TracerLineColor = Color3.fromRGB(255, 0, 0)
local TracerLineOutlineColor = Color3.fromRGB(0, 0, 0)
local TracerThickness = 1.0
local TracerOutlineThickness = 3.0

local FOVFillEnabled = false
local FOVFillTransparency = 0.2
local FOVFillColor1 = Color3.fromRGB(255, 0, 0)
local FOVFillColor2 = Color3.fromRGB(0, 255, 0)
local FOVFillColor3 = Color3.fromRGB(0, 0, 255)
local FOVFillRotateEnabled = false
local FOVFillRotateSpeed = 1
local currentRotationAngle = 0

local TargetPartMode = "Head"
local TransitionSpeed = 5
local MaxTargetDistance = 10000

local AntiAimEnabled = false
local AntiAimMode = "Spin"
local AntiAimSpeed = 15
local JitterMode = "Multi"
local AntiAimJitterAngle = 22.5
local AntiAimStaticAngle = 180

local MotionBlurEnabled = false
local MotionBlurIntensity = 1.5

local AtmosphereEnabled = false 
local AtmosphereColor = Color3.fromRGB(178, 200, 255)
local AtmosphereDecay = Color3.fromRGB(255, 178, 120)
local AtmosphereGlare = 0.4
local AtmosphereHaze = 1.2
local AtmosphereOffset = 0.25
local AtmosphereDensity = 0.35

local ViewingPlayer = nil
local ViewPlayerConnection = nil

local Camera = workspace.CurrentCamera
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")
local Lighting = game:GetService("Lighting")
local TweenService = game:GetService("TweenService")
local TargetVelocityHistory = {}

local SkeletonConnections = {
    {"Head", "UpperTorso"},
    {"UpperTorso", "LowerTorso"},
    {"UpperTorso", "LeftUpperArm"},
    {"LeftUpperArm", "LeftLowerArm"},
    {"LeftLowerArm", "LeftHand"},
    {"UpperTorso", "RightUpperArm"},
    {"RightUpperArm", "RightLowerArm"},
    {"RightLowerArm", "RightHand"},
    {"LowerTorso", "LeftUpperLeg"},
    {"LeftUpperLeg", "LeftLowerLeg"},
    {"LeftLowerLeg", "LeftFoot"},
    {"LowerTorso", "RightUpperLeg"},
    {"RightUpperLeg", "RightLowerLeg"},
    {"RightLowerLeg", "RightFoot"},
    {"Head", "Torso"},
    {"Torso", "Left Arm"},
    {"Torso", "Right Arm"},
    {"Torso", "Left Leg"},
    {"Torso", "Right Leg"}
}

local SelfNeonEnabled = false
local SelfNeonColor = Color3.fromRGB(0, 255, 150)
local OriginalMaterials = {}
local OriginalColors = {}

local function UpdateSelfNeon()
    local char = LocalPlayer.Character
    if not char then return end

    if SelfNeonEnabled then
        for _, part in ipairs(char:GetDescendants()) do
            
            if part:IsA("SurfaceAppearance") then
                part.Enabled = false
            elseif part:IsA("Decal") or part:IsA("Texture") then
                if not OriginalMaterials[part] then
                    OriginalMaterials[part] = part.Transparency
                end
                part.Transparency = 1
            end

            
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                if not OriginalMaterials[part] then
                    OriginalMaterials[part] = part.Material
                    OriginalColors[part] = part.Color
                end

                part.Material = Enum.Material.Neon
                part.Color = SelfNeonColor

                
                local mesh = part:FindFirstChildOfClass("SpecialMesh")
                if mesh then
                    if not OriginalMaterials[mesh] then
                        OriginalMaterials[mesh] = mesh.TextureId
                    end
                    mesh.TextureId = "" 
                end
            end
        end
    else
        
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("SurfaceAppearance") then
                part.Enabled = true
            end
        end

        for item, originalValue in pairs(OriginalMaterials) do
            if item and item.Parent then
                if item:IsA("BasePart") then
                    item.Material = originalValue
                    item.Color = OriginalColors[item] or item.Color
                elseif item:IsA("Decal") or item:IsA("Texture") then
                    item.Transparency = originalValue
                elseif item:IsA("SpecialMesh") then
                    item.TextureId = originalValue
                end
            end
        end
        table.clear(OriginalMaterials)
        table.clear(OriginalColors)
    end
end


LocalPlayer.CharacterAdded:Connect(function(newChar)
    table.clear(OriginalMaterials)
    table.clear(OriginalColors)
    task.wait(0.5)
    UpdateSelfNeon()
end)


local TargetGuiParent = LocalPlayer:WaitForChild("PlayerGui", 5) or (CoreGui:FindFirstChild("RobloxGui") or CoreGui)

local HitOverlayGui = Instance.new("ScreenGui")
HitOverlayGui.Name = "hooksense_HitOverlayGui"
HitOverlayGui.ResetOnSpawn = false
HitOverlayGui.IgnoreGuiInset = true
HitOverlayGui.DisplayOrder = 99999
HitOverlayGui.Parent = TargetGuiParent

local OverlayFrame = Instance.new("Frame")
OverlayFrame.Name = "HitOverlay"
OverlayFrame.Size = UDim2.new(1, 0, 1, 0)
OverlayFrame.BorderSizePixel = 0
OverlayFrame.BackgroundColor3 = HitOverlayColor
OverlayFrame.BackgroundTransparency = 1
OverlayFrame.ZIndex = 99999
OverlayFrame.Parent = HitOverlayGui

local function TriggerHitOverlay()
    if not HitOverlayEnabled then return end
    OverlayFrame.BackgroundColor3 = HitOverlayColor
    
    local fadeIn = TweenService:Create(OverlayFrame, TweenInfo.new(0.05, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundTransparency = 0.5})
    local fadeOut = TweenService:Create(OverlayFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {BackgroundTransparency = 1})
    
    fadeIn:Play()
    fadeIn.Completed:Connect(function()
        task.wait(0.01)
        fadeOut:Play()
    end)
end

local function ShowCustomHitNotification(targetName, partName, damage, weaponName)
    if not HitNotifyEnabled then return end
    
    weaponName = weaponName or "Hands"
    
    local notifyText = HitNotifyTemplate
    notifyText = string.gsub(notifyText, "{name}", targetName)
    notifyText = string.gsub(notifyText, "{part}", partName)
    notifyText = string.gsub(notifyText, "{dmg}", tostring(damage))
    notifyText = string.gsub(notifyText, "{weapon}", weaponName)
    
    Library:Notify(notifyText, HitNotifyDuration)
end

local function PlayHitSound()
    if HitSoundMode == "None" or not SoundIDs[HitSoundMode] then return end
    local Sound = Instance.new("Sound")
    Sound.SoundId = SoundIDs[HitSoundMode]
    Sound.Volume = HitSoundVolume
    Sound.PlayOnRemove = true
    Sound.Parent = SoundService
    Sound:Destroy()
end

local function UpdateSkybox()
    for _, obj in ipairs(Lighting:GetChildren()) do
        if obj:IsA("Sky") and (obj.Name == "hooksenseSky" or obj.Name == "Sky") then
            obj:Destroy()
        end
    end
    if SkyboxEnabled and SkyboxIDs[SelectedSkybox] then
        local assetId = SkyboxIDs[SelectedSkybox]
        local success, result = pcall(function() return game:GetObjects(assetId)[1] end)
        if success and result and result:IsA("Sky") then
            local NewSky = result:Clone()
            NewSky.Name = "hooksenseSky"
            NewSky.Parent = Lighting
        else
            local NewSky = Instance.new("Sky")
            NewSky.Name = "hooksenseSky"
            NewSky.SkyboxBk = assetId
            NewSky.SkyboxDn = assetId
            NewSky.SkyboxFt = assetId
            NewSky.SkyboxLf = assetId
            NewSky.SkyboxRt = assetId
            NewSky.SkyboxUp = assetId
            NewSky.Parent = Lighting
        end
    end
end

local function UpdateAtmosphere()
    local atmos = Lighting:FindFirstChildOfClass("Atmosphere")
    if AtmosphereEnabled then
        if not atmos then
            atmos = Instance.new("Atmosphere")
            atmos.Parent = Lighting
        end
        atmos.Color = AtmosphereColor
        atmos.Decay = AtmosphereDecay
        atmos.Glare = AtmosphereGlare
        atmos.Haze = AtmosphereHaze
        atmos.Offset = AtmosphereOffset
        atmos.Density = AtmosphereDensity
        
        if not Lighting:FindFirstChildOfClass("Sky") then
            local DefaultSky = Instance.new("Sky")
            DefaultSky.Name = "hooksenseDefaultSky"
            DefaultSky.Parent = Lighting
        end
    else
        if atmos then
            atmos:Destroy()
        end
        if not SkyboxEnabled then
            local defaultSky = Lighting:FindFirstChild("hooksenseDefaultSky")
            if defaultSky then
                defaultSky:Destroy()
            end
        end
    end
end

UpdateAtmosphere()

local function ClearAuraParticles()
	for i = #ActiveParticles, 1, -1 do
		if ActiveParticles[i] then
			ActiveParticles[i]:Destroy()
		end
		table.remove(ActiveParticles, i)
	end
end

local function ApplyColorToAura(inst, col)
	local seq = ColorSequence.new(col)
	if inst:IsA("PointLight") then
		inst.Color = col
	elseif inst:IsA("ParticleEmitter") or inst:IsA("Beam") or inst:IsA("Trail") then
		inst.Color = seq
	end
	for _, child in ipairs(inst:GetDescendants()) do
		ApplyColorToAura(child, col)
	end
end

local function UpdateAuraSystem()
	ClearAuraParticles()
	if not AuraEnabled then return end

	local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
	local hrp = char:WaitForChild("HumanoidRootPart", 5)
	if not hrp then return end

	for auraName, isSelected in pairs(SelectedAuras) do
		if isSelected then
			local template = ParticleAuras[auraName]
			if template then
				local cloned = template:Clone()
				ApplyColorToAura(cloned, CurrentAuraColor)

				for _, part in ipairs(cloned:GetChildren()) do
					local targetPart = char:FindFirstChild(part.Name)
					if targetPart then
						for _, child in ipairs(part:GetChildren()) do
							child.Name = "\0\0"
							child.Parent = targetPart
							table.insert(ActiveParticles, child)
						end
					end
				end
				cloned:Destroy()
			end
		end
	end
end

LocalPlayer.CharacterAdded:Connect(function(newChar)
	ClearAuraParticles()
	local hum = newChar:WaitForChild("Humanoid", 5)
	if hum then
		hum.Died:Connect(function()
			ClearAuraParticles()
		end)
	end
	task.wait(0.5)
	UpdateAuraSystem()
end)

if LocalPlayer.Character then
	local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
	if hum then
		hum.Died:Connect(function()
			ClearAuraParticles()
		end)
	end
end



local DeltaGradientGui = Instance.new("ScreenGui")
DeltaGradientGui.Name = "hooksense_DeltaGradientGui"
DeltaGradientGui.ResetOnSpawn = false
DeltaGradientGui.IgnoreGuiInset = true
DeltaGradientGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
DeltaGradientGui.Parent = TargetGuiParent

local FOVFillFrame = Instance.new("Frame")
FOVFillFrame.Name = "FOVGradientFrame"
FOVFillFrame.AnchorPoint = Vector2.new(0.5, 0.5)
FOVFillFrame.BorderSizePixel = 0
FOVFillFrame.BackgroundTransparency = 1
FOVFillFrame.Visible = false
FOVFillFrame.Parent = DeltaGradientGui

local FOVFillCorner = Instance.new("UICorner")
FOVFillCorner.CornerRadius = UDim.new(1, 0)
FOVFillCorner.Parent = FOVFillFrame

local FOVFillGradient = Instance.new("UIGradient")
FOVFillGradient.Parent = FOVFillFrame

local BoxFillGui = Instance.new("ScreenGui")
BoxFillGui.Name = "hooksense_BoxFillGui"
BoxFillGui.ResetOnSpawn = false
BoxFillGui.IgnoreGuiInset = true
BoxFillGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
BoxFillGui.Parent = TargetGuiParent

local FOVCircleOutline = Drawing.new("Circle")
FOVCircleOutline.Thickness = 3.0
FOVCircleOutline.NumSides = 144
FOVCircleOutline.Filled = false
FOVCircleOutline.Transparency = 1.0
FOVCircleOutline.Visible = FOVVisible

local FOVCircle = Drawing.new("Circle")
FOVCircle.Thickness = 1.0
FOVCircle.NumSides = 144
FOVCircle.Filled = false
FOVCircle.Transparency = 1.0
FOVCircle.Visible = FOVVisible

local TracerLineOutline = Drawing.new("Line")
TracerLineOutline.Thickness = 3.0
TracerLineOutline.Transparency = 1.0

local TracerLine = Drawing.new("Line")
TracerLine.Thickness = 1.0
TracerLine.Transparency = 0.8

local function getScreenCenter()
    return Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
end

local function getGunPosition()
    if LocalPlayer.Character then
        local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
        if tool then
            local muzzleNames = {"Muzzle", "GunBarrel", "Barrel", "Flash", "Tip"}
            for _, name in ipairs(muzzleNames) do
                local part = tool:FindFirstChild(name, true)
                if part and part:IsA("BasePart") then
                    return part.Position
                end
            end
            
            local handle = tool:FindFirstChild("Handle") or tool:FindFirstChildOfClass("BasePart")
            if handle then
                return (handle.CFrame * CFrame.new(0, 0, -1.8)).Position
            end
        end
    end
    return nil
end

local wallCheckParams = RaycastParams.new()
wallCheckParams.FilterType = Enum.RaycastFilterType.Exclude
wallCheckParams.IgnoreWater = true

local function isBehindWall(targetPart)
    if not targetPart or not LocalPlayer.Character then return false end

    local origin = Camera.CFrame.Position
    local destination = targetPart.Position
    local direction = destination - origin

    wallCheckParams.FilterDescendantsInstances = {LocalPlayer.Character, targetPart.Parent}
    local raycastResult = workspace:Raycast(origin, direction, wallCheckParams)

    return raycastResult ~= nil
end

local function isKnocked(char)
    if not KnockCheckEnabled or not char then return false end
    local koValue = char:FindFirstChild("KO") or char:FindFirstChild("K.O")
    if koValue and koValue:IsA("BoolValue") and koValue.Value then
        return true
    end
    local bodyEffects = char:FindFirstChild("BodyEffects")
    if bodyEffects then
        local ko = bodyEffects:FindFirstChild("K.O") or bodyEffects:FindFirstChild("KO")
        if ko and ko:IsA("BoolValue") and ko.Value then
            return true
        end
    end
    return false
end

local function isDead(humanoid, char)
    if not DieCheckEnabled then return false end
    if humanoid and humanoid.Health <= 0 then return true end
    if char and not char:IsDescendantOf(workspace) then return true end
    return false
end

local function GetTargetInFOV()
    local TempTargetChar = nil
    local TempPlayer = nil
    local MaxDistance = FOVSize
    local Center = getScreenCenter()
    local MyRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local char = player.Character
            local rootPart = char:FindFirstChild("HumanoidRootPart")
            local humanoid = char:FindFirstChildOfClass("Humanoid")
            if rootPart and humanoid then
                if isDead(humanoid, char) then continue end
                if isKnocked(char) then continue end
                if MyRoot and (rootPart.Position - MyRoot.Position).Magnitude > MaxTargetDistance then continue end
                if WallCheckEnabled and isBehindWall(rootPart) then continue end
                if TeamCheckEnabled and player.Team == LocalPlayer.Team then continue end
                
                local ScreenPosition, OnScreen = Camera:WorldToViewportPoint(rootPart.Position)
                if OnScreen then
                    local DistanceToCenter = (Vector2.new(ScreenPosition.X, ScreenPosition.Y) - Center).Magnitude
                    if DistanceToCenter < MaxDistance then
                        TempTargetChar = char
                        TempPlayer = player
                        MaxDistance = DistanceToCenter
                    end
                end
            end
        end
    end
    return TempPlayer, TempTargetChar
end

local LockButtonGui = Instance.new("ScreenGui")
LockButtonGui.Name = "hooksense_TargetLockButtonGui"
LockButtonGui.ResetOnSpawn = false
LockButtonGui.IgnoreGuiInset = true
LockButtonGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
LockButtonGui.Enabled = false
LockButtonGui.Parent = TargetGuiParent

local TargetBtn = Instance.new("TextButton")
TargetBtn.Name = "TargetLockButton"
TargetBtn.Size = UDim2.new(0, 150, 0, 42)
TargetBtn.Position = UDim2.new(0.5, -75, 0.75, 0)
TargetBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
TargetBtn.BackgroundTransparency = 0
TargetBtn.BorderSizePixel = 0
TargetBtn.Font = GlobalFontSetting
TargetBtn.Text = "LOCK TARGET: OFF"
TargetBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
TargetBtn.TextSize = 12
TargetBtn.AutoButtonColor = false
TargetBtn.Parent = LockButtonGui

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 8)
BtnCorner.Parent = TargetBtn

local BtnStroke = Instance.new("UIStroke")
BtnStroke.Thickness = 1
BtnStroke.Color = Color3.fromRGB(60, 60, 70)
BtnStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
BtnStroke.Parent = TargetBtn

local dragging, dragInput, dragStart, startPos
local function updateDrag(input)
    local delta = input.Position - dragStart
    TargetBtn.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

TargetBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = TargetBtn.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

TargetBtn.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        updateDrag(input)
    end
end)

TargetBtn.MouseButton1Click:Connect(function()
    ManualLockEnabled = not ManualLockEnabled
    if ManualLockEnabled then
        local foundPlayer, foundChar = GetTargetInFOV()
        if foundPlayer then
            ManualLockedTarget = foundPlayer
            TargetBtn.Text = "Unlock: " .. foundPlayer.Name
            TargetBtn.TextColor3 = Color3.fromRGB(0, 255, 150)
            BtnStroke.Color = Color3.fromRGB(0, 255, 150)
            Library:Notify("Locked target: " .. foundPlayer.Name, 2.5)
        else
            ManualLockEnabled = false
            ManualLockedTarget = nil
            TargetBtn.Text = "NO TARGET IN FOV"
            TargetBtn.TextColor3 = Color3.fromRGB(255, 75, 75)
            BtnStroke.Color = Color3.fromRGB(255, 75, 75)
            Library:Notify("No target found in FOV!", 2.0)
            task.delay(1.5, function()
                if not ManualLockEnabled then
                    TargetBtn.Text = "LOCK TARGET: OFF"
                    TargetBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
                    BtnStroke.Color = Color3.fromRGB(60, 60, 70)
                end
            end)
        end
    else
        ManualLockedTarget = nil
        TargetBtn.Text = "LOCK TARGET: OFF"
        TargetBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
        BtnStroke.Color = Color3.fromRGB(60, 60, 70)
        Library:Notify("Unlocked Target", 2.0)
    end
end)

Players.PlayerRemoving:Connect(function(plr)
    if ManualLockedTarget == plr then
        ManualLockEnabled = false
        ManualLockedTarget = nil
        TargetBtn.Text = "LOCK TARGET: OFF"
        TargetBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
        BtnStroke.Color = Color3.fromRGB(60, 60, 70)
        Library:Notify("Target left the server!", 2.0)
    end
end)

local CurrentAimTargetPosition = nil
local CurrentTargetPlayer = nil
local CurrentTargetCharacter = nil
local lastTarget = nil
local interpolationProgress = 0
local CurrentFOVCenterPos = Vector2.new(0, 0)

task.spawn(function()
    while task.wait(0.03) do
        for _, p in ipairs(Players:GetPlayers()) do
            if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = p.Character.HumanoidRootPart
                local hist = TargetVelocityHistory[p.Name]
                local currentTick = tick()
                if not hist then
                    TargetVelocityHistory[p.Name] = {Pos = hrp.Position, Time = currentTick, Velocity = Vector3.new(0, 0, 0)}
                else
                    local dt = currentTick - hist.Time
                    if dt > 0.005 then
                        local calculatedVel = (hrp.Position - hist.Pos) / dt
                        hist.Velocity = calculatedVel
                        hist.Pos = hrp.Position
                        hist.Time = currentTick
                    end
                end
            end
        end

        if not SilentAimEnabled then
            CurrentAimTargetPosition = nil
            CurrentTargetPlayer = nil
            CurrentTargetCharacter = nil
            lastTarget = nil
            interpolationProgress = 0
        else
            local TempTargetChar = nil
            local TempPlayer = nil

            if ManualLockEnabled and ManualLockedTarget then
                TempPlayer = ManualLockedTarget
                if ManualLockedTarget.Character then
                    local char = ManualLockedTarget.Character
                    local rootPart = char:FindFirstChild("HumanoidRootPart")
                    local humanoid = char:FindFirstChildOfClass("Humanoid")
                    if rootPart and humanoid and not isDead(humanoid, char) and not isKnocked(char) then
                        TempTargetChar = char
                    end
                end
            end

            if not TempTargetChar and not ManualLockEnabled then
                TempPlayer, TempTargetChar = GetTargetInFOV()
            end

            CurrentTargetPlayer = TempPlayer
            CurrentTargetCharacter = TempTargetChar

            if TempTargetChar then
                local head = TempTargetChar:FindFirstChild("Head")
                local root = TempTargetChar:FindFirstChild("HumanoidRootPart")
                
                local targetPartObj = nil
                if TargetPartMode == "Closet" then
                    local closestPart = nil
                    local minPartDist = math.huge
                    local Center = getScreenCenter()
                    for _, part in ipairs(TempTargetChar:GetChildren()) do
                        if part:IsA("BasePart") then
                            local screenPos, onScreen = Camera:WorldToViewportPoint(part.Position)
                            if onScreen then
                                local dist = (Vector2.new(screenPos.X, screenPos.Y) - Center).Magnitude
                                if dist < minPartDist then
                                    minPartDist = dist
                                    closestPart = part
                                end
                            end
                        end
                    end
                    targetPartObj = closestPart or head or root
                else
                    targetPartObj = TempTargetChar:FindFirstChild(TargetPartMode) or head or root
                end

                if targetPartObj then
                    local basePosition = targetPartObj.Position
                    if AutoPredictionEnabled and root then
                        local ping = LocalPlayer:GetNetworkPing()
                        local targetedVelocity = root.Velocity

                        if ResolverEnabled then
                            local isVelocitySpoofed = (targetedVelocity.Magnitude > 75) or 
                                                      (math.abs(targetedVelocity.Y) > 50) or 
                                                      (targetedVelocity.Magnitude < 0.1 and root.AssemblyLinearVelocity.Magnitude > 10) or
                                                      (targetedVelocity.X ~= targetedVelocity.X)
                            
                            if isVelocitySpoofed then
                                local historyData = TargetVelocityHistory[TempPlayer and TempPlayer.Name or ""]
                                if historyData and historyData.Velocity.Magnitude < 150 then
                                    targetedVelocity = historyData.Velocity
                                else
                                    targetedVelocity = Vector3.new(0, 0, 0)
                                end
                            end
                        end

                        basePosition = basePosition + (targetedVelocity * ping * PredictionValue)
                    end
                    if TargetPartMode == "Root to Head" then
                        if lastTarget ~= TempTargetChar then
                            lastTarget = TempTargetChar
                            interpolationProgress = 0
                        end
                        interpolationProgress = math.clamp(interpolationProgress + (0.01 * TransitionSpeed), 0, 1)
                        CurrentAimTargetPosition = root.Position:Lerp(head.Position, interpolationProgress)
                    else
                        CurrentAimTargetPosition = basePosition
                        lastTarget = TempTargetChar
                    end
                else
                    CurrentAimTargetPosition = nil
                end
            else
                lastTarget = nil
                interpolationProgress = 0
                CurrentAimTargetPosition = nil
            end
        end
    end
end)

local CurrentConnectedHumanoid = nil
local HealthConnection = nil
local function BindHealthTracker(targetPlayer)
    if not targetPlayer or not targetPlayer.Character then return end
    local hum = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not hum then return end
    if CurrentConnectedHumanoid == hum then return end
    if HealthConnection then HealthConnection:Disconnect() end
    CurrentConnectedHumanoid = hum
    local lastHealth = hum.Health
    HealthConnection = hum:GetPropertyChangedSignal("Health"):Connect(function()
        if CurrentTargetPlayer == targetPlayer and hum.Health < lastHealth then
            local damageCalculated = math.floor(lastHealth - hum.Health)
            task.spawn(PlayHitSound)
            task.spawn(TriggerHitOverlay) 
            local hitPartName = TargetPartMode
            if hitPartName == "Root to Head" then
                hitPartName = "Body/Head"
            elseif hitPartName == "HumanoidRootPart" then
                hitPartName = "Torso"
            end
            
            local weaponName = "Hands"
            if LocalPlayer.Character then
                local heldTool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
                if heldTool then
                    weaponName = heldTool.Name
                end
            end
            
            task.spawn(ShowCustomHitNotification, targetPlayer.Name, hitPartName, damageCalculated, weaponName)
        end
        lastHealth = hum.Health
    end)
end

local RunService = game:GetService("RunService")
local spinAngle = 0
local jitterToggle = false
local LastCameraRotation = Camera.CFrame.LookVector

RunService.RenderStepped:Connect(function()
    if VoidEnabled and LocalPlayer.Character then
        local Root = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if Root then
            local randomX = math.random(-2000000000, 2000000000)
            local randomY = math.random(-2000000000, 2000000000)
            local randomZ = math.random(-2000000000, 2000000000)
            Root.CFrame = CFrame.new(randomX, randomY, randomZ)
        end
    end

    local RealCenter = getScreenCenter()
    local TargetCenter = RealCenter

    if FOVOriginMode == "Target" and CurrentAimTargetPosition then
        local screenPos, onScreen = Camera:WorldToViewportPoint(CurrentAimTargetPosition)
        if onScreen then
            TargetCenter = Vector2.new(screenPos.X, screenPos.Y)
        end
    elseif FOVOriginMode == "Mouse" then
        local mousePos = UserInputService:GetMouseLocation()
        TargetCenter = Vector2.new(mousePos.X, mousePos.Y)
    elseif FOVOriginMode == "Gun" then
        local gunPos = getGunPosition()
        if gunPos then
            local screenPos, onScreen = Camera:WorldToViewportPoint(gunPos)
            if onScreen then
                TargetCenter = Vector2.new(screenPos.X, screenPos.Y)
            end
        end
    end

    local lerpFactor = math.clamp(FOVLerpSpeed, 0.01, 1.0)
    CurrentFOVCenterPos = CurrentFOVCenterPos:Lerp(TargetCenter, lerpFactor)

    local ShowCircle = FOVVisible and SilentAimEnabled
    
    FOVCircleOutline.Position = CurrentFOVCenterPos
    FOVCircleOutline.Radius = FOVSize
    FOVCircleOutline.Thickness = FOVOutlineThickness
    FOVCircleOutline.Transparency = FOVOutlineTransparency
    FOVCircleOutline.Visible = ShowCircle
    FOVCircleOutline.Color = FOVCircleOutlineColor

    FOVCircle.Position = CurrentFOVCenterPos
    FOVCircle.Radius = FOVSize
    FOVCircle.Thickness = FOVThickness
    FOVCircle.Transparency = FOVTransparency
    FOVCircle.Visible = ShowCircle
    FOVCircle.Color = FOVCircleColor

    if ShowCircle and FOVFillEnabled then
        FOVFillFrame.Position = UDim2.new(0, CurrentFOVCenterPos.X, 0, CurrentFOVCenterPos.Y)
        FOVFillFrame.Size = UDim2.new(0, FOVSize * 2, 0, FOVSize * 2)
        
        FOVFillGradient.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, FOVFillColor1),
            ColorSequenceKeypoint.new(0.5, FOVFillColor2),
            ColorSequenceKeypoint.new(1, FOVFillColor3)
        })
        
        FOVFillFrame.BackgroundTransparency = FOVFillTransparency
        
        if FOVFillRotateEnabled then
            currentRotationAngle = (currentRotationAngle + (0.5 * FOVFillRotateSpeed)) % 360
        end
        FOVFillGradient.Rotation = currentRotationAngle
        FOVFillFrame.Visible = true
    else
        FOVFillFrame.Visible = false
    end

    if SilentAimEnabled and TracerEnabled and CurrentAimTargetPosition then
        local TargetPos, OnScreen = Camera:WorldToViewportPoint(CurrentAimTargetPosition)
        if OnScreen then
            local ToPos = Vector2.new(TargetPos.X, TargetPos.Y)
            TracerLineOutline.From = RealCenter
            TracerLineOutline.To = ToPos
            TracerLineOutline.Visible = true
            TracerLineOutline.Color = TracerLineOutlineColor
            TracerLineOutline.Thickness = TracerOutlineThickness

            TracerLine.From = RealCenter
            TracerLine.To = ToPos
            TracerLine.Visible = true
            TracerLine.Color = TracerLineColor
            TracerLine.Thickness = TracerThickness
        else
            TracerLineOutline.Visible = false
            TracerLine.Visible = false
        end
    else
        TracerLineOutline.Visible = false
        TracerLine.Visible = false
    end

    if CurrentTargetPlayer then
        BindHealthTracker(CurrentTargetPlayer)
    end

    if SilentAimEnabled and LookAtEnabled and LocalPlayer.Character then
        local Root = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        local Humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if Root and Humanoid and Humanoid.Health > 0 and not AntiAimEnabled then
            if CurrentAimTargetPosition then
                Humanoid.AutoRotate = false
                local lookVector = Vector3.new(CurrentAimTargetPosition.X, Root.Position.Y, CurrentAimTargetPosition.Z)
                Root.CFrame = CFrame.lookAt(Root.Position, lookVector)
            else
                Humanoid.AutoRotate = true
            end
        end
    end

    if AntiAimEnabled and LocalPlayer.Character then
        local Root = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        local Humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if Root and Humanoid and Humanoid.Health > 0 then
            Humanoid.AutoRotate = false
            local _, cameraY, _ = Camera.CFrame:ToOrientation()

            if AntiAimMode == "Spin" then
                spinAngle = (spinAngle + AntiAimSpeed) % 360
                Root.CFrame = CFrame.new(Root.Position) * CFrame.Angles(0, math.rad(spinAngle), 0)
            elseif AntiAimMode == "Jitter" then
                local intensity = 1
                if JitterMode == "Multi" then intensity = math.random(1, 5)
                elseif JitterMode == "5x always" then intensity = 5
                elseif JitterMode == "3x always" then intensity = 3
                elseif JitterMode == "2x always" then intensity = 2
                elseif JitterMode == "random" then intensity = math.random(1, 5)
                else intensity = math.random(1, 5) end
                
                jitterToggle = not jitterToggle
                local jitterOffset = jitterToggle and math.rad(AntiAimJitterAngle * intensity) or math.rad(-AntiAimJitterAngle * intensity)
                Root.CFrame = CFrame.new(Root.Position) * CFrame.Angles(0, cameraY + jitterOffset, 0)
                
                local LowerTorso = LocalPlayer.Character:FindFirstChild("LowerTorso") or LocalPlayer.Character:FindFirstChild("Torso")
                if LowerTorso and LowerTorso:FindFirstChild("RootJoint") then
                    LowerTorso.RootJoint.Transform = CFrame.Angles(0, jitterOffset, 0)
                end
            elseif AntiAimMode == "Static" then
                local staticOffset = math.rad(AntiAimStaticAngle)
                Root.CFrame = CFrame.new(Root.Position) * CFrame.Angles(0, cameraY + staticOffset, 0)
                
                local LowerTorso = LocalPlayer.Character:FindFirstChild("LowerTorso") or LocalPlayer.Character:FindFirstChild("Torso")
                if LowerTorso and LowerTorso:FindFirstChild("RootJoint") then
                    LowerTorso.RootJoint.Transform = CFrame.Angles(0, staticOffset, 0)
                end
            end
        end
    else
        if LocalPlayer.Character then
            local Humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if Humanoid then
                if not LookAtEnabled or (LookAtEnabled and not CurrentAimTargetPosition) then
                    Humanoid.AutoRotate = true
                end
            end
            local LowerTorso = LocalPlayer.Character:FindFirstChild("LowerTorso") or LocalPlayer.Character:FindFirstChild("Torso")
            if LowerTorso and LowerTorso:FindFirstChild("RootJoint") then
                LowerTorso.RootJoint.Transform = CFrame.new()
            end
        end
    end

    local blurEffect = Lighting:FindFirstChild("hooksenseMotionBlur")
    if MotionBlurEnabled then
        if not blurEffect then
            blurEffect = Instance.new("BlurEffect")
            blurEffect.Name = "hooksenseMotionBlur"
            blurEffect.Parent = Lighting
        end
        local currentLookVector = Camera.CFrame.LookVector
        local angleDifference = math.acos(math.clamp(currentLookVector:Dot(LastCameraRotation), -1, 1))
        local blurTarget = math.clamp(angleDifference * 45 * MotionBlurIntensity, 0, 56)
        
        blurEffect.Size = blurEffect.Size + (blurTarget - blurEffect.Size) * 0.25
        LastCameraRotation = currentLookVector
    else
        if blurEffect then blurEffect:Destroy() end
    end
end)

local OldNamecall
OldNamecall = hookmetamethod(game, "__namecall", newcclosure(function(Self, ...)
    if checkcaller() then return OldNamecall(Self, ...) end
    local Args = { ... }
    local Method = getnamecallmethod()
    local SelfClass = ""
    pcall(function()
        if Self and typeof(Self) == "Instance" then SelfClass = Self.ClassName end
    end)

    if SilentAimEnabled and CurrentAimTargetPosition then
        if (Method == "ScreenPointToRay" or Method == "ViewportPointToRay") and SelfClass == "Camera" then
            return Ray.new(Camera.CFrame.Position, (CurrentAimTargetPosition - Camera.CFrame.Position).Unit)
        end
        if Method == "Raycast" and SelfClass == "Workspace" then
            if BulletTPEnabled then
                Args[1] = CurrentAimTargetPosition - (Args[2].Unit * 1)
                Args[2] = Args[2].Unit * 5
            else
                Args[2] = (CurrentAimTargetPosition - Args[1]).Unit * Args[2].Magnitude
            end
            return OldNamecall(Self, table.unpack(Args))
        end
        if (Method == "FindPartOnRay" or Method == "FindPartOnRayWithIgnoreList" or Method == "FindPartOnRayWithWhitelist") and SelfClass == "Workspace" then
            local CurrentRay = Args[1]
            if CurrentRay and typeof(CurrentRay) == "Ray" then
                if BulletTPEnabled then
                    local NewOrigin = CurrentAimTargetPosition - (CurrentRay.Direction.Unit * 1)
                    Args[1] = Ray.new(NewOrigin, CurrentRay.Direction.Unit * 5)
                else
                    Args[1] = Ray.new(CurrentRay.Origin, (CurrentAimTargetPosition - CurrentRay.Origin).Unit * CurrentRay.Direction.Magnitude)
                end
                return OldNamecall(Self, table.unpack(Args))
            end
        end
    end
    return OldNamecall(Self, ...)
end))

local Tabs = {
    Main = Window:AddTab("main"),
    HitEffects = Window:AddTab("visuals"),
    ESP = Window:AddTab("esp"),
    Movement = Window:AddTab("character"),
    World = Window:AddTab("world"),
    Addons = Window:AddTab("misc"),
    ['UI'] = Window:AddTab("Settings")
}

local Options = getgenv().Options
local Toggles = getgenv().Toggles

local LeftGroupBox = Tabs.Main:AddLeftGroupbox("Silent Aim")
local TargetGroupBox = Tabs.Main:AddLeftGroupbox("Targeting Options")
local TracerGroupBox = Tabs.Main:AddLeftGroupbox("Target Visual")

local FOVGroupBox = Tabs.Main:AddRightGroupbox("FOV Settings")
local FOVFillGroupBox = Tabs.Main:AddRightGroupbox("FOV Fill")

LeftGroupBox:AddToggle("SilentAimToggle", { Text = "Enable Silent Aim", Default = false })
    :AddKeyPicker("SilentAimKeybind", { Default = "None", SyncToggleState = true, Mode = "Toggle", Text = "Silent Aim Bind" })

Toggles.SilentAimToggle:OnChanged(function()
    SilentAimEnabled = Toggles.SilentAimToggle.Value
end)

LeftGroupBox:AddToggle("ShowLockButtonToggle", { Text = "Show Lock Target Button", Default = false })
Toggles.ShowLockButtonToggle:OnChanged(function()
    LockButtonGui.Enabled = Toggles.ShowLockButtonToggle.Value
end)

LeftGroupBox:AddToggle("ResolverToggle", { Text = "Enable Resolver", Default = false })
Toggles.ResolverToggle:OnChanged(function()
    ResolverEnabled = Toggles.ResolverToggle.Value
end)

LeftGroupBox:AddToggle("LookAtToggle", { Text = "Enable Look at Target", Default = false })
    :AddKeyPicker("LookAtKeybind", { Default = "None", SyncToggleState = true, Mode = "Toggle", Text = "Look At Bind" })

Toggles.LookAtToggle:OnChanged(function()
    LookAtEnabled = Toggles.LookAtToggle.Value
end)

LeftGroupBox:AddToggle("BulletTPToggle", { Text = "Enable MagicBullet", Default = false })
    :AddKeyPicker("BulletTPKeybind", { Default = "None", SyncToggleState = true, Mode = "Toggle", Text = "BulletTp Bind" })

Toggles.BulletTPToggle:OnChanged(function()
    BulletTPEnabled = Toggles.BulletTPToggle.Value
end)

LeftGroupBox:AddToggle("WallCheckToggle", { Text = "Enable Wall Check", Default = false })
Toggles.WallCheckToggle:OnChanged(function()
    WallCheckEnabled = Toggles.WallCheckToggle.Value
end)

LeftGroupBox:AddToggle("TeamCheckToggle", { Text = "Enable Team Check", Default = false })
Toggles.TeamCheckToggle:OnChanged(function()
    TeamCheckEnabled = Toggles.TeamCheckToggle.Value
end)

LeftGroupBox:AddToggle("KnockCheckToggle", { Text = "Enable Knock Check", Default = false })
Toggles.KnockCheckToggle:OnChanged(function()
    KnockCheckEnabled = Toggles.KnockCheckToggle.Value
end)

TargetGroupBox:AddDropdown("TargetPartDropdown", { Text = "Hitbox", Values = {"Head", "HumanoidRootPart", "Root", "Closet"}, Default = 1, Multi = false })
Options.TargetPartDropdown:OnChanged(function()
    TargetPartMode = Options.TargetPartDropdown.Value
end)

TargetGroupBox:AddSlider("TransitionSpeedSlider", { Text = "Root Speed", Default = 5, Min = 1, Max = 30, Rounding = 1 })
Options.TransitionSpeedSlider:OnChanged(function()
    TransitionSpeed = Options.TransitionSpeedSlider.Value
end)

TargetGroupBox:AddSlider("MaxDistanceSlider", { Text = "Max Distance", Default = 9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999, Min = 1, Max = 9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999, Rounding = 0 })
Options.MaxDistanceSlider:OnChanged(function()
    MaxTargetDistance = Options.MaxDistanceSlider.Value
end)

local TracerVisibleToggle = TracerGroupBox:AddToggle("TracerVisibleToggle", { Text = "Enable Tracer Line", Default = true })
TracerVisibleToggle:AddColorPicker("TracerLineColorPicker", { Default = Color3.fromRGB(255, 0, 0), Title = "Tracer Color" })
TracerVisibleToggle:AddColorPicker("TracerLineOutlineColorPicker", { Default = Color3.fromRGB(0, 0, 0), Title = "Tracer Outline Color" })

Toggles.TracerVisibleToggle:OnChanged(function()
    TracerEnabled = Toggles.TracerVisibleToggle.Value
end)

Options.TracerLineColorPicker:OnChanged(function()
    TracerLineColor = Options.TracerLineColorPicker.Value
end)

Options.TracerLineOutlineColorPicker:OnChanged(function()
    TracerLineOutlineColor = Options.TracerLineOutlineColorPicker.Value
end)

TracerGroupBox:AddSlider("TracerThicknessSlider", { Text = "Tracer Line Thickness", Default = 1, Min = 0.5, Max = 5.0, Rounding = 1 })
Options.TracerThicknessSlider:OnChanged(function()
    TracerThickness = Options.TracerThicknessSlider.Value
end)

TracerGroupBox:AddSlider("TracerOutlineThicknessSlider", { Text = "Tracer Outline Thickness", Default = 3.0, Min = 1.0, Max = 8.0, Rounding = 1 })
Options.TracerOutlineThicknessSlider:OnChanged(function()
    TracerOutlineThickness = Options.TracerOutlineThicknessSlider.Value
end)

local FOVVisibleToggle = FOVGroupBox:AddToggle("FOVVisibleToggle", { Text = "Enable Fov", Default = true })
FOVVisibleToggle:AddColorPicker("FOVCircleColorPicker", { Default = Color3.fromRGB(255, 255, 255), Title = "FOV Color" })
FOVVisibleToggle:AddColorPicker("FOVCircleOutlineColorPicker", { Default = Color3.fromRGB(0, 0, 0), Title = "FOV Outline Color" })

Toggles.FOVVisibleToggle:OnChanged(function()
    FOVVisible = Toggles.FOVVisibleToggle.Value
end)

Options.FOVCircleColorPicker:OnChanged(function()
    FOVCircleColor = Options.FOVCircleColorPicker.Value
end)

Options.FOVCircleOutlineColorPicker:OnChanged(function()
    FOVCircleOutlineColor = Options.FOVCircleOutlineColorPicker.Value
end)

FOVGroupBox:AddSlider("FOVSlider", { Text = "FOV Size Radius", Default = 100, Min = 40, Max = 800, Rounding = 0 })
Options.FOVSlider:OnChanged(function()
    FOVSize = Options.FOVSlider.Value
end)

FOVGroupBox:AddSlider("FOVThicknessSlider", { Text = "FOV Line Thickness", Default = 1.5, Min = 0.5, Max = 5.0, Rounding = 1 })
Options.FOVThicknessSlider:OnChanged(function()
    FOVThickness = Options.FOVThicknessSlider.Value
end)

FOVGroupBox:AddSlider("FOVOutlineThicknessSlider", { Text = "FOV Outline Thickness", Default = 4, Min = 1.0, Max = 8.0, Rounding = 1 })
Options.FOVOutlineThicknessSlider:OnChanged(function()
    FOVOutlineThickness = Options.FOVOutlineThicknessSlider.Value
end)

FOVGroupBox:AddSlider("FOVTransparencySlider", { Text = "FOV Transparency", Default = 0.85, Min = 0.0, Max = 1.0, Rounding = 2 })
Options.FOVTransparencySlider:OnChanged(function()
    FOVTransparency = Options.FOVTransparencySlider.Value
end)

FOVGroupBox:AddSlider("FOVOutlineTransparencySlider", { Text = "FOV Outline Transparency", Default = 0.55, Min = 0.0, Max = 1.0, Rounding = 2 })
Options.FOVOutlineTransparencySlider:OnChanged(function()
    FOVOutlineTransparency = Options.FOVOutlineTransparencySlider.Value
end)

FOVGroupBox:AddDropdown("FOVOriginDropdown", { Text = "FOV Position", Values = {"Center", "Target", "Mouse", "Gun"}, Default = 1, Multi = false })
Options.FOVOriginDropdown:OnChanged(function()
    FOVOriginMode = Options.FOVOriginDropdown.Value
end)

FOVGroupBox:AddSlider("FOVLerpSlider", { Text = "FOV Lerp", Default = 0.5, Min = 0.01, Max = 1.0, Rounding = 2 })
Options.FOVLerpSlider:OnChanged(function()
    FOVLerpSpeed = Options.FOVLerpSlider.Value
end)

FOVFillGroupBox:AddToggle("FOVFillToggle", { Text = "Enable FOV Fill", Default = false })
Toggles.FOVFillToggle:OnChanged(function()
    FOVFillEnabled = Toggles.FOVFillToggle.Value
end)

FOVFillGroupBox:AddSlider("FOVFillTransparencySlider", { Text = "FOV Fill Opacity", Default = 0.65, Min = 0, Max = 1, Rounding = 2 })
Options.FOVFillTransparencySlider:OnChanged(function()
    FOVFillTransparency = Options.FOVFillTransparencySlider.Value
end)

FOVFillGroupBox:AddLabel("Fill Color 1"):AddColorPicker("FOVFillColor1Picker", { Default = Color3.fromRGB(255, 255, 255) })
Options.FOVFillColor1Picker:OnChanged(function()
    FOVFillColor1 = Options.FOVFillColor1Picker.Value
end)

FOVFillGroupBox:AddLabel("Fill Color 2"):AddColorPicker("FOVFillColor2Picker", { Default = Color3.fromRGB(255, 255, 255) })
Options.FOVFillColor2Picker:OnChanged(function()
    FOVFillColor2 = Options.FOVFillColor2Picker.Value
end)

FOVFillGroupBox:AddLabel("Fill Color 3"):AddColorPicker("FOVFillColor3Picker", { Default = Color3.fromRGB(255, 255, 255) })
Options.FOVFillColor3Picker:OnChanged(function()
    FOVFillColor3 = Options.FOVFillColor3Picker.Value
end)

FOVFillGroupBox:AddToggle("FOVFillRotateToggle", { Text = "Rotate FOV", Default = false })
Toggles.FOVFillRotateToggle:OnChanged(function()
    FOVFillRotateEnabled = Toggles.FOVFillRotateToggle.Value
end)

FOVFillGroupBox:AddSlider("FOVFillRotateSpeedSlider", { Text = "Gradient Rotation Speed", Default = 3, Min = 1, Max = 10, Rounding = 1 })
Options.FOVFillRotateSpeedSlider:OnChanged(function()
    FOVFillRotateSpeed = Options.FOVFillRotateSpeedSlider.Value
end)

local SoundLeftBox = Tabs.HitEffects:AddLeftGroupbox("Hit Sound")
local OverlayLeftBox = Tabs.HitEffects:AddLeftGroupbox("Hit Overlay") 
local SelfHighlightLeftBox = Tabs.HitEffects:AddLeftGroupbox("Material")
local NotifyRightBox = Tabs.HitEffects:AddRightGroupbox("Hit Notification")

SoundLeftBox:AddDropdown("HitSoundDropdown", { Text = "Target Hit Sound", Values = {"None", "Spark", "Neverlose", "Rust", "Break", "Fatality", "Primordial", "Minecraft xp", "Minecraft Bow", "Crack", "Pop", "Headshot", "Manbo", "Skeet"}, Default = 1, Multi = false })
Options.HitSoundDropdown:OnChanged(function()
    HitSoundMode = Options.HitSoundDropdown.Value
end)

SoundLeftBox:AddSlider("HitSoundVolumeSlider", { Text = "Hit Sound Volume", Default = 2.0, Min = 0.0, Max = 5.0, Rounding = 1 })
Options.HitSoundVolumeSlider:OnChanged(function()
    HitSoundVolume = Options.HitSoundVolumeSlider.Value
end)

SoundLeftBox:AddButton({
    Text = "Preview Sound",
    Func = function()
        pcall(function()
            if HitSoundMode == "None" or not SoundIDs[HitSoundMode] then return end
            local PreviewSound = Instance.new("Sound")
            PreviewSound.SoundId = SoundIDs[HitSoundMode]
            PreviewSound.Volume = HitSoundVolume
            PreviewSound.Parent = SoundService
            
            PreviewSound:Play()
            PreviewSound.Ended:Connect(function()
                PreviewSound:Destroy()
            end)
        end)
    end
})


OverlayLeftBox:AddToggle("HitOverlayToggle", { Text = "Enable Hit Overlay", Default = false })
Toggles.HitOverlayToggle:OnChanged(function()
    HitOverlayEnabled = Toggles.HitOverlayToggle.Value
end)

OverlayLeftBox:AddLabel("Overlay Color"):AddColorPicker("HitOverlayColorPicker", { Default = Color3.fromRGB(255, 0, 0) })
Options.HitOverlayColorPicker:OnChanged(function()
    HitOverlayColor = Options.HitOverlayColorPicker.Value
end)

local SelfNeonToggle = SelfHighlightLeftBox:AddToggle("SelfNeonToggle", { Text = "Enable Self Neon", Default = false })
SelfNeonToggle:AddColorPicker("SelfNeonColorPicker", { Default = Color3.fromRGB(0, 255, 150), Title = "Neon Color" })

Toggles.SelfNeonToggle:OnChanged(function()
    SelfNeonEnabled = Toggles.SelfNeonToggle.Value
    UpdateSelfNeon()
end)

Options.SelfNeonColorPicker:OnChanged(function()
    SelfNeonColor = Options.SelfNeonColorPicker.Value
    UpdateSelfNeon()
end)


NotifyRightBox:AddToggle("HitNotifyToggle", { Text = "Enable Hit Notification", Default = false })
Toggles.HitNotifyToggle:OnChanged(function()
    HitNotifyEnabled = Toggles.HitNotifyToggle.Value
end)

NotifyRightBox:AddSlider("HitNotifyDurationSlider", { Text = "Notification Duration (s)", Default = 2.5, Min = 0.5, Max = 5.0, Rounding = 1 })
Options.HitNotifyDurationSlider:OnChanged(function()
    HitNotifyDuration = Options.HitNotifyDurationSlider.Value
end)

NotifyRightBox:AddInput("CustomHitNotifyInput", { Placeholder = "Format: {name} | {part} | {dmg} | {weapon}", Text = "Custom Hit Notify", Numeric = false, Finished = true })
Options.CustomHitNotifyInput:OnChanged(function()
    if Options.CustomHitNotifyInput.Value and Options.CustomHitNotifyInput.Value ~= "" then
        HitNotifyTemplate = Options.CustomHitNotifyInput.Value
    end
end)

local AuraGroup = Tabs.HitEffects:AddRightGroupbox("Particle Aura")

local AuraToggle = AuraGroup:AddToggle("AuraMasterToggle", { 
    Text = "Enable Particle Aura", 
    Default = false 
})
AuraToggle:AddColorPicker("AuraColorPicker", { 
    Default = Color3.fromRGB(133, 220, 255), 
    Title = "Aura Color" 
})

Toggles.AuraMasterToggle:OnChanged(function()
    AuraEnabled = Toggles.AuraMasterToggle.Value
    UpdateAuraSystem()
end)

Options.AuraColorPicker:OnChanged(function()
    CurrentAuraColor = Options.AuraColorPicker.Value
    for _, p in ipairs(ActiveParticles) do
        ApplyColorToAura(p, CurrentAuraColor)
    end
end)

AuraGroup:AddDropdown("AuraSelectionDropdown", { 
    Text = "Select Aura Presets", 
    Values = AuraOptions, 
    Default = { ["angel"] = true },
    Multi = true 
})

Options.AuraSelectionDropdown:OnChanged(function()
    SelectedAuras = Options.AuraSelectionDropdown.Value
    UpdateAuraSystem()
end)


local LastCameraCFrame = workspace.CurrentCamera.CFrame

local function GetColor(optionName, defaultColor)
    local fallback = defaultColor or Color3.fromRGB(255, 255, 255)
    if not Options then return fallback end
    local opt = Options[optionName]
    if not opt then return fallback end

    if typeof(opt) == "Color3" then
        return opt
    elseif typeof(opt) == "table" then
        if typeof(opt.Value) == "Color3" then
            return opt.Value
        elseif typeof(opt.Color) == "Color3" then
            return opt.Color
        elseif opt[1] and typeof(opt[1]) == "number" then
            return Color3.new(opt[1], opt[2], opt[3])
        end
    elseif typeof(opt) == "UserData" or typeof(opt) == "RBXScriptConnection" then
        if opt.Value and typeof(opt.Value) == "Color3" then
            return opt.Value
        end
    end
    return fallback
end

local function CreateSquare(thickness, color)
    local square = Drawing.new("Square")
    square.Thickness = thickness or 1
    square.Color = color or Color3.fromRGB(255, 255, 255)
    square.Filled = false
    square.Transparency = 1
    square.Visible = false
    return square
end

local function CreateLine(thickness, color)
    local line = Drawing.new("Line")
    line.Thickness = thickness or 1
    line.Color = color or Color3.fromRGB(255, 255, 255)
    line.Transparency = 1
    line.Visible = false
    return line
end

local function CreateTitleLabel(name)
    local label = Instance.new("TextLabel")
    label.Name = name or "ESP_TitleLabel"
    label.BackgroundTransparency = 1
    label.BorderSizePixel = 0
    label.Font = ESPFontSetting
    label.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    label.TextStrokeTransparency = 0.2
    label.Visible = false
    label.Parent = BoxFillGui
    return label
end

local ESP_Drawing_Storage = {}
local function ImplementESPSetup(holder)
    pcall(function()
        holder.BoxOutline = CreateSquare(3, Color3.fromRGB(0, 0, 0))
        holder.BoxMain = CreateSquare(1, Color3.fromRGB(255, 255, 255))

        holder.LineESPOutline = CreateLine(3, Color3.fromRGB(0, 0, 0))
        holder.LineESPMain = CreateLine(1, Color3.fromRGB(255, 255, 255))

        holder.SkeletonLines = {}
        for i = 1, #SkeletonConnections do
            holder.SkeletonLines[i] = {
                Outline = CreateLine(3, Color3.fromRGB(0, 0, 0)),
                Main = CreateLine(1, Color3.fromRGB(255, 255, 255))
            }
        end

        holder.BoxFillFrame = Instance.new("Frame")
        holder.BoxFillFrame.Name = "BoxFill"
        holder.BoxFillFrame.AnchorPoint = Vector2.new(0, 0)
        holder.BoxFillFrame.BorderSizePixel = 0
        holder.BoxFillFrame.BackgroundTransparency = 1
        holder.BoxFillFrame.Visible = false
        holder.BoxFillFrame.Parent = BoxFillGui

        holder.BoxFillGradient = Instance.new("UIGradient")
        holder.BoxFillGradient.Parent = holder.BoxFillFrame
        holder.BoxFillRotationAngle = 0

        holder.GuiHealthBg = Instance.new("Frame")
        holder.GuiHealthBg.Name = "HealthBg"
        holder.GuiHealthBg.BorderSizePixel = 0
        holder.GuiHealthBg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        holder.GuiHealthBg.Visible = false
        holder.GuiHealthBg.Parent = BoxFillGui

        holder.GuiHealthBar = Instance.new("Frame")
        holder.GuiHealthBar.Name = "HealthBar"
        holder.GuiHealthBar.BorderSizePixel = 0
        holder.GuiHealthBar.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        holder.GuiHealthBar.Parent = holder.GuiHealthBg

        holder.GuiHealthGradient = Instance.new("UIGradient")
        holder.GuiHealthGradient.Rotation = 90
        holder.GuiHealthGradient.Parent = holder.GuiHealthBar
        
        holder.GuiArmorBg = Instance.new("Frame")
        holder.GuiArmorBg.Name = "ArmorBg"
        holder.GuiArmorBg.BorderSizePixel = 0
        holder.GuiArmorBg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        holder.GuiArmorBg.Visible = false
        holder.GuiArmorBg.Parent = BoxFillGui

        holder.GuiArmorBar = Instance.new("Frame")
        holder.GuiArmorBar.Name = "ArmorBar"
        holder.GuiArmorBar.BorderSizePixel = 0
        holder.GuiArmorBar.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        holder.GuiArmorBar.Parent = holder.GuiArmorBg

        holder.GuiArmorGradient = Instance.new("UIGradient")
        holder.GuiArmorGradient.Rotation = 0
        holder.GuiArmorGradient.Parent = holder.GuiArmorBar

        holder.Highlight = Instance.new("Highlight")
        holder.Highlight.Name = "hooksense_Highlight"
        holder.Highlight.Enabled = false
        holder.Highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        holder.Highlight.Parent = CoreGui

        holder.Name = CreateTitleLabel("ESP_NameLabel")
        holder.Distance = CreateTitleLabel("ESP_DistanceLabel")
        holder.HealthText = CreateTitleLabel("ESP_HealthLabel")
        holder.Tool = CreateTitleLabel("ESP_ToolLabel")
        
        holder.SmoothHealth = nil
        holder.SmoothArmor = nil
        holder.SmoothNameSize = nil
        holder.NeedsRefresh = true
    end)
end

local function CreateDrawingESP(player)
    if player == LocalPlayer then return end
    if ESP_Drawing_Storage[player] then return end
    ESP_Drawing_Storage[player] = {}
    ImplementESPSetup(ESP_Drawing_Storage[player])
end

local function RemoveDrawingESP(player)
    if ESP_Drawing_Storage[player] then
        pcall(function()
            if ESP_Drawing_Storage[player].BoxFillFrame then ESP_Drawing_Storage[player].BoxFillFrame:Destroy() end
            if ESP_Drawing_Storage[player].GuiHealthBg then ESP_Drawing_Storage[player].GuiHealthBg:Destroy() end
            if ESP_Drawing_Storage[player].GuiArmorBg then ESP_Drawing_Storage[player].GuiArmorBg:Destroy() end
            if ESP_Drawing_Storage[player].Highlight then ESP_Drawing_Storage[player].Highlight:Destroy() end
            if ESP_Drawing_Storage[player].Name then ESP_Drawing_Storage[player].Name:Destroy() end
            if ESP_Drawing_Storage[player].Distance then ESP_Drawing_Storage[player].Distance:Destroy() end
            if ESP_Drawing_Storage[player].HealthText then ESP_Drawing_Storage[player].HealthText:Destroy() end
            if ESP_Drawing_Storage[player].Tool then ESP_Drawing_Storage[player].Tool:Destroy() end
            
            if ESP_Drawing_Storage[player].SkeletonLines then
                for _, pair in ipairs(ESP_Drawing_Storage[player].SkeletonLines) do
                    if pair.Outline then pair.Outline:Remove() end
                    if pair.Main then pair.Main:Remove() end
                end
            end
            for _, obj in pairs(ESP_Drawing_Storage[player]) do
                if type(obj) == "table" and obj.Remove then
                    obj:Remove()
                elseif type(obj) == "userdata" and obj.Remove then
                    obj:Remove()
                end
            end
        end)
        ESP_Drawing_Storage[player] = nil
    end
end

for _, p in ipairs(Players:GetPlayers()) do CreateDrawingESP(p) end
Players.PlayerAdded:Connect(CreateDrawingESP)
Players.PlayerRemoving:Connect(RemoveDrawingESP)

local function SetVisibilityFalse(esp)
    pcall(function()
        if esp.BoxOutline then esp.BoxOutline.Visible = false end
        if esp.BoxMain then esp.BoxMain.Visible = false end

        if esp.LineESPOutline then esp.LineESPOutline.Visible = false end
        if esp.LineESPMain then esp.LineESPMain.Visible = false end

        if esp.SkeletonLines then
            for _, pair in ipairs(esp.SkeletonLines) do
                if pair.Outline then pair.Outline.Visible = false end
                if pair.Main then pair.Main.Visible = false end
            end
        end

        if esp.BoxFillFrame then esp.BoxFillFrame.Visible = false end
        if esp.GuiHealthBg then esp.GuiHealthBg.Visible = false end
        if esp.GuiArmorBg then esp.GuiArmorBg.Visible = false end
        if esp.Highlight then esp.Highlight.Enabled = false end
        if esp.HealthText then esp.HealthText.Visible = false end
        if esp.Name then esp.Name.Visible = false end
        if esp.Distance then esp.Distance.Visible = false end
        if esp.Tool then esp.Tool.Visible = false end
        esp.SmoothHealth = nil
        esp.SmoothArmor = nil
        esp.SmoothNameSize = nil
        esp.NeedsRefresh = true
    end)
end

RunService.RenderStepped:Connect(function()
    pcall(function()
        local Camera = workspace.CurrentCamera
        if not Camera then return end

        local currentCFrame = Camera.CFrame
        if currentCFrame ~= LastCameraCFrame then
            LastCameraCFrame = currentCFrame
            for _, esp in pairs(ESP_Drawing_Storage) do
                esp.NeedsRefresh = true
            end
        end

        local espMaster = Toggles.EspMaster and Toggles.EspMaster.Value
        if not espMaster then
            for _, esp in pairs(ESP_Drawing_Storage) do SetVisibilityFalse(esp) end
            return
        end

        local boxEnabled = Toggles.EspBox and Toggles.EspBox.Value
        local boxColor = GetColor("BoxColor", Color3.fromRGB(255, 255, 255))
        local boxOutlineColor = GetColor("BoxOutlineColor", Color3.fromRGB(0, 0, 0))

        local skeletonEnabled = Toggles.EspSkeleton and Toggles.EspSkeleton.Value
        local skeletonColor = GetColor("SkeletonColor", Color3.fromRGB(255, 255, 255))
        local skeletonOutlineColor = GetColor("SkeletonOutlineColor", Color3.fromRGB(0, 0, 0))
        local skeletonThickness = Options.SkeletonThicknessSlider and Options.SkeletonThicknessSlider.Value or 1.5
        local skeletonOutlineThickness = Options.SkeletonOutlineThicknessSlider and Options.SkeletonOutlineThicknessSlider.Value or 3.5

        local lineESPEnabled = Toggles.EspLine and Toggles.EspLine.Value
        local lineESPColor = GetColor("LineESPColor", Color3.fromRGB(255, 255, 255))
        local lineESPOutlineColor = GetColor("LineESPOutlineColor", Color3.fromRGB(0, 0, 0))
        local lineESPThickness = Options.LineESPThicknessSlider and Options.LineESPThicknessSlider.Value or 1.5
        local lineESPOutlineThickness = Options.LineESPOutlineThicknessSlider and Options.LineESPOutlineThicknessSlider.Value or 3.5
        local lineESPPosMode = Options.LineESPPosDropdown and Options.LineESPPosDropdown.Value or "Bottom"

        local nameColor = GetColor("NameColor", Color3.fromRGB(255, 255, 255))
        local distColor = GetColor("DistColor", Color3.fromRGB(255, 255, 255))
        local toolColor = GetColor("ToolColor", Color3.fromRGB(255, 255, 255))
        local hpTextColor = GetColor("HPTextColor", Color3.fromRGB(255, 255, 255))
        local selectedNameStyles = Options.NameStyleDropdown and Options.NameStyleDropdown.Value or {}

        local customTextTransEnabled = Toggles.CustomTextTransparency and Toggles.CustomTextTransparency.Value or false
        local nameTrans = customTextTransEnabled and (Options.NameTransparencySlider and Options.NameTransparencySlider.Value or 0) or 0
        local hpTextTrans = customTextTransEnabled and (Options.HpTextTransparencySlider and Options.HpTextTransparencySlider.Value or 0) or 0
        local distTrans = customTextTransEnabled and (Options.DistanceTransparencySlider and Options.DistanceTransparencySlider.Value or 0) or 0
        local toolTrans = customTextTransEnabled and (Options.ToolEquippedTransparencySlider and Options.ToolEquippedTransparencySlider.Value or 0) or 0

        local hpFullColor = GetColor("HPFullColor", Color3.fromRGB(0, 255, 0))
        local hpHalfColor = GetColor("HPHalfColor", Color3.fromRGB(255, 170, 0))
        local hpLowColor = GetColor("HPLowColor", Color3.fromRGB(255, 0, 0))
        local lerpAlpha = Options.HealthLerpSlider and Options.HealthLerpSlider.Value or 0.2

        local armorCol1 = GetColor("ArmorColor1", Color3.fromRGB(0, 150, 255))
        local armorCol2 = GetColor("ArmorColor2", Color3.fromRGB(0, 255, 200))

        local boxFillEnabled = Toggles.EspBoxFill and Toggles.EspBoxFill.Value
        local boxFillTrans = Options.EspBoxFillTransparency and Options.EspBoxFillTransparency.Value or 0.5
        local fillCol1 = GetColor("EspBoxFillColor1", Color3.fromRGB(255, 0, 0))
        local fillCol2 = GetColor("EspBoxFillColor2", Color3.fromRGB(0, 255, 0))
        local fillCol3 = GetColor("EspBoxFillColor3", Color3.fromRGB(0, 0, 255))
        local fillRotateEnabled = Toggles.EspBoxFillRotate and Toggles.EspBoxFillRotate.Value
        local fillRotateSpeed = Options.EspBoxFillRotateSpeed and Options.EspBoxFillRotateSpeed.Value or 1

        local highlightEnabled = Toggles.EspHighlight and Toggles.EspHighlight.Value
        local highlightVisibleColor = GetColor("HighlightVisibleColor", Color3.fromRGB(0, 255, 0))
        local highlightInvisibleColor = GetColor("HighlightInvisibleColor", Color3.fromRGB(255, 0, 0))
        local highlightOutlineColor = GetColor("HighlightOutlineColor", Color3.fromRGB(255, 255, 255))
        local highlightFillTrans = Options.HighlightFillTransSlider and Options.HighlightFillTransSlider.Value or 0.5
        local highlightOutlineTrans = Options.HighlightOutlineTransSlider and Options.HighlightOutlineTransSlider.Value or 0.0

        for player, esp in pairs(ESP_Drawing_Storage) do
            pcall(function()
                local character = player.Character
                local rootPart = character and character:FindFirstChild("HumanoidRootPart")
                local humanoid = character and character:FindFirstChildOfClass("Humanoid")

                if character and rootPart and humanoid and humanoid.Health > 0 then
                    local myChar = LocalPlayer.Character
                    local myRoot = myChar and myChar:FindFirstChild("HumanoidRootPart")
                    local originPos = myRoot and myRoot.Position or Camera.CFrame.Position
                    local currentDist = (originPos - rootPart.Position).Magnitude
                    
                    if (TeamCheckEnabled and player.Team == LocalPlayer.Team) then
                        SetVisibilityFalse(esp)
                        return
                    end

                    if highlightEnabled and esp.Highlight then
                        local hidden = isBehindWall(rootPart)
                        esp.Highlight.Adornee = character
                        esp.Highlight.FillColor = hidden and highlightInvisibleColor or highlightVisibleColor
                        esp.Highlight.OutlineColor = highlightOutlineColor
                        esp.Highlight.FillTransparency = highlightFillTrans
                        esp.Highlight.OutlineTransparency = highlightOutlineTrans
                        esp.Highlight.Enabled = true
                    else
                        if esp.Highlight then esp.Highlight.Enabled = false end
                    end

                    local rootPos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
                    
                    if onScreen then
                        local headOffset = rootPart.CFrame * CFrame.new(0, 2.5, 0)
                        local feetOffset = rootPart.CFrame * CFrame.new(0, -3.0, 0)
                        
                        local headPos = Camera:WorldToViewportPoint(headOffset.Position)
                        local feetPos = Camera:WorldToViewportPoint(feetOffset.Position)
                        
                        local height = math.clamp(math.abs(headPos.Y - feetPos.Y), 10, 2000)
                        local width = height * 0.7
                        local boxX = math.round(rootPos.X - (width / 2))
                        local boxY = math.round(headPos.Y)
                        
                        local selectedName = ""
                        local hasDisplayName = selectedNameStyles["Display Name"]
                        local hasUsername = selectedNameStyles["Username"]

                        if hasDisplayName and hasUsername then
                            selectedName = player.DisplayName .. " (@" .. player.Name .. ")"
                        elseif hasDisplayName then
                            selectedName = player.DisplayName
                        elseif hasUsername then
                            selectedName = player.Name
                        else
                            selectedName = player.Name
                        end

                        local maxHp = (humanoid.MaxHealth > 0) and humanoid.MaxHealth or 100
                        local targetHp = math.clamp(humanoid.Health, 0, maxHp)
                        
                        if not esp.SmoothHealth then 
                            esp.SmoothHealth = targetHp 
                        else
                            esp.SmoothHealth = esp.SmoothHealth + (targetHp - esp.SmoothHealth) * lerpAlpha
                        end

                        local pct = math.clamp(esp.SmoothHealth / maxHp, 0, 1)

                        local targetArmor = 0
                        local maxArmor = 200
                        local bodyEffects = character:FindFirstChild("BodyEffects")
                        if bodyEffects then
                            local armorVal = bodyEffects:FindFirstChild("Armor")
                            if armorVal then
                                targetArmor = armorVal.Value
                            end
                        end

                        if not esp.SmoothArmor then
                            esp.SmoothArmor = targetArmor
                        else
                            esp.SmoothArmor = esp.SmoothArmor + (targetArmor - esp.SmoothArmor) * lerpAlpha
                        end
                        local armorPct = math.clamp(esp.SmoothArmor / maxArmor, 0, 1)

                        local mathWidth, mathHeight = math.round(width), math.round(height)

                        if boxEnabled then
                            if esp.BoxOutline then
                                esp.BoxOutline.Position = Vector2.new(boxX, boxY)
                                esp.BoxOutline.Size = Vector2.new(mathWidth, mathHeight)
                                esp.BoxOutline.Color = boxOutlineColor
                                esp.BoxOutline.Thickness = 2.35
                                esp.BoxOutline.Visible = true
                            end

                            if esp.BoxMain then
                                esp.BoxMain.Position = Vector2.new(boxX, boxY)
                                esp.BoxMain.Size = Vector2.new(mathWidth, mathHeight)
                                esp.BoxMain.Color = boxColor
                                esp.BoxMain.Thickness = 1
                                esp.BoxMain.Visible = true
                            end
                        else
                            if esp.BoxOutline then esp.BoxOutline.Visible = false end
                            if esp.BoxMain then esp.BoxMain.Visible = false end
                        end

                        if lineESPEnabled then
                            local lineStartPos = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                            if lineESPPosMode == "Center" then
                                lineStartPos = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
                            elseif lineESPPosMode == "Top" then
                                lineStartPos = Vector2.new(Camera.ViewportSize.X / 2, 0)
                            end

                            local targetPos2D = Vector2.new(rootPos.X, rootPos.Y)

                            if esp.LineESPOutline then
                                esp.LineESPOutline.From = lineStartPos
                                esp.LineESPOutline.To = targetPos2D
                                esp.LineESPOutline.Thickness = lineESPOutlineThickness
                                esp.LineESPOutline.Color = lineESPOutlineColor
                                esp.LineESPOutline.Visible = true
                            end
                            if esp.LineESPMain then
                                esp.LineESPMain.From = lineStartPos
                                esp.LineESPMain.To = targetPos2D
                                esp.LineESPMain.Thickness = lineESPThickness
                                esp.LineESPMain.Color = lineESPColor
                                esp.LineESPMain.Visible = true
                            end
                        else
                            if esp.LineESPOutline then esp.LineESPOutline.Visible = false end
                            if esp.LineESPMain then esp.LineESPMain.Visible = false end
                        end

                        if skeletonEnabled then
                            for idx, connection in ipairs(SkeletonConnections) do
                                local partA = character:FindFirstChild(connection[1])
                                local partB = character:FindFirstChild(connection[2])
                                local drawPair = esp.SkeletonLines[idx]

                                if partA and partB and drawPair then
                                    local posA, visA = Camera:WorldToViewportPoint(partA.Position)
                                    local posB, visB = Camera:WorldToViewportPoint(partB.Position)

                                    if visA and visB then
                                        local fromVector = Vector2.new(posA.X, posA.Y)
                                        local toVector = Vector2.new(posB.X, posB.Y)

                                        if drawPair.Outline then
                                            drawPair.Outline.From = fromVector
                                            drawPair.Outline.To = toVector
                                            drawPair.Outline.Color = skeletonOutlineColor
                                            drawPair.Outline.Thickness = skeletonOutlineThickness
                                            drawPair.Outline.Visible = true
                                        end

                                        if drawPair.Main then
                                            drawPair.Main.From = fromVector
                                            drawPair.Main.To = toVector
                                            drawPair.Main.Color = skeletonColor
                                            drawPair.Main.Thickness = skeletonThickness
                                            drawPair.Main.Visible = true
                                        end
                                    else
                                        if drawPair.Outline then drawPair.Outline.Visible = false end
                                        if drawPair.Main then drawPair.Main.Visible = false end
                                    end
                                else
                                    if drawPair and drawPair.Outline then drawPair.Outline.Visible = false end
                                    if drawPair and drawPair.Main then drawPair.Main.Visible = false end
                                end
                            end
                        else
                            if esp.SkeletonLines then
                                for _, pair in ipairs(esp.SkeletonLines) do
                                    if pair.Outline then pair.Outline.Visible = false end
                                    if pair.Main then pair.Main.Visible = false end
                                end
                            end
                        end

                        local targetNameSize = math.clamp(math.floor(10 + (220 / math.max(currentDist, 15))), 10, 13)

                        if not esp.SmoothNameSize then 
                            esp.SmoothNameSize = targetNameSize 
                        else
                            esp.SmoothNameSize = esp.SmoothNameSize + (targetNameSize - esp.SmoothNameSize) * lerpAlpha
                        end

                        local dynamicNameSize = math.round(esp.SmoothNameSize)
                        local subTextSize = 10

                        if boxFillEnabled and esp.BoxFillFrame then
                            esp.BoxFillFrame.Position = UDim2.new(0, boxX, 0, boxY)
                            esp.BoxFillFrame.Size = UDim2.new(0, mathWidth, 0, mathHeight)
                            esp.BoxFillFrame.BackgroundTransparency = boxFillTrans
                            
                            esp.BoxFillGradient.Color = ColorSequence.new({
                                ColorSequenceKeypoint.new(0, fillCol1),
                                ColorSequenceKeypoint.new(0.5, fillCol2),
                                ColorSequenceKeypoint.new(1, fillCol3)
                            })
                            
                            if fillRotateEnabled then
                                esp.BoxFillRotationAngle = (esp.BoxFillRotationAngle + (0.5 * fillRotateSpeed)) % 360
                            end
                            esp.BoxFillGradient.Rotation = esp.BoxFillRotationAngle
                            esp.BoxFillFrame.Visible = true
                        else
                            if esp.BoxFillFrame then esp.BoxFillFrame.Visible = false end
                        end

                        if Toggles.EspHealth and Toggles.EspHealth.Value then
                            local barWidth = 2.65
                            local outlinePadding = 1
                            local barX = boxX - 5.75
                            local barY = boxY

                            esp.GuiHealthBg.Position = UDim2.new(0, barX - outlinePadding, 0, barY - outlinePadding)
                            esp.GuiHealthBg.Size = UDim2.new(0, barWidth + (outlinePadding * 2), 0, mathHeight + (outlinePadding * 2))
                            esp.GuiHealthBg.Visible = true

                            local healthHeight = math.clamp(math.round(mathHeight * pct), 0, mathHeight)
                            
                            esp.GuiHealthBar.Position = UDim2.new(0, outlinePadding, 0, outlinePadding + (mathHeight - healthHeight))
                            esp.GuiHealthBar.Size = UDim2.new(0, barWidth, 0, healthHeight)

                            esp.GuiHealthGradient.Color = ColorSequence.new({
                                ColorSequenceKeypoint.new(0, hpFullColor),
                                ColorSequenceKeypoint.new(0.5, hpHalfColor),
                                ColorSequenceKeypoint.new(1, hpLowColor)
                            })

                            if Toggles.EspHealthText and Toggles.EspHealthText.Value then
                                esp.HealthText.Font = ESPFontSetting
                                esp.HealthText.Text = tostring(math.round(targetHp))
                                esp.HealthText.TextSize = subTextSize
                                esp.HealthText.TextColor3 = hpTextColor
                                esp.HealthText.TextTransparency = hpTextTrans
                                esp.HealthText.TextStrokeTransparency = 0.2 + (0.8 * hpTextTrans)
                                esp.HealthText.Size = UDim2.new(0, 30, 0, 15)
        
                                local targetY = math.clamp(barY + (mathHeight - healthHeight) - 3, boxY - 3, boxY + mathHeight - 10)
        
                                esp.HealthText.Position = UDim2.new(0, barX - 31, 0, targetY)
                                esp.HealthText.Visible = true
                            else
                                esp.HealthText.Visible = false
                            end
                        else
                            esp.GuiHealthBg.Visible = false
                            esp.HealthText.Visible = false
                        end
                        
                        if Toggles.EspArmor and Toggles.EspArmor.Value and targetArmor > 0 then
                            local barHeight = 2.65
                            local outlinePadding = 1
                            local armorBarX = boxX
                            local armorBarY = boxY + mathHeight + 3.5

                            esp.GuiArmorBg.Position = UDim2.new(0, armorBarX - outlinePadding, 0, armorBarY - outlinePadding)
                            esp.GuiArmorBg.Size = UDim2.new(0, mathWidth + (outlinePadding * 2), 0, barHeight + (outlinePadding * 2))
                            esp.GuiArmorBg.Visible = true

                            local armorWidth = math.clamp(math.round(mathWidth * armorPct), 0, mathWidth)
                            esp.GuiArmorBar.Position = UDim2.new(0, outlinePadding, 0, outlinePadding)
                            esp.GuiArmorBar.Size = UDim2.new(0, armorWidth, 0, barHeight)

                            esp.GuiArmorGradient.Color = ColorSequence.new({
                                ColorSequenceKeypoint.new(0, armorCol1),
                                ColorSequenceKeypoint.new(1, armorCol2)
                            })
                        else
                            esp.GuiArmorBg.Visible = false
                        end

                        if Toggles.EspName and Toggles.EspName.Value then
                            esp.Name.Font = ESPFontSetting
                            esp.Name.Text = selectedName
                            esp.Name.TextSize = dynamicNameSize
                            esp.Name.TextColor3 = nameColor
                            esp.Name.TextTransparency = nameTrans
                            esp.Name.TextStrokeTransparency = 0.2 + (0.8 * nameTrans)
                            esp.Name.Size = UDim2.new(0, 200, 0, dynamicNameSize + 4)
                            esp.Name.Position = UDim2.new(0, boxX + (mathWidth / 2) - 100, 0, boxY - (dynamicNameSize + 5))
                            esp.Name.Visible = true
                        else
                            esp.Name.Visible = false
                        end

                        local bottomOffset = boxY + mathHeight + 2
                        if Toggles.EspArmor and Toggles.EspArmor.Value and targetArmor > 0 then
                            bottomOffset = bottomOffset + 7
                        end

                        if Toggles.EspDistance and Toggles.EspDistance.Value then
                            esp.Distance.Font = ESPFontSetting
                            local dist = math.floor(currentDist)
                            esp.Distance.Text = "[ " .. tostring(dist) .. "m ]"
                            esp.Distance.TextSize = subTextSize
                            esp.Distance.TextColor3 = distColor
                            esp.Distance.TextTransparency = distTrans
                            esp.Distance.TextStrokeTransparency = 0.2 + (0.8 * distTrans)
                            esp.Distance.Size = UDim2.new(0, 200, 0, 15)
                            esp.Distance.Position = UDim2.new(0, boxX + (mathWidth / 2) - 100, 0, bottomOffset)
                            esp.Distance.Visible = true
                            bottomOffset = bottomOffset + 13
                        else
                            esp.Distance.Visible = false
                        end

                        if Toggles.EspTool and Toggles.EspTool.Value then
                            local heldTool = character:FindFirstChildOfClass("Tool")
                            local toolName = heldTool and heldTool.Name or "None"

                            esp.Tool.Font = ESPFontSetting
                            esp.Tool.Text = toolName
                            esp.Tool.TextSize = subTextSize
                            esp.Tool.TextColor3 = toolColor
                            esp.Tool.TextTransparency = toolTrans
                            esp.Tool.TextStrokeTransparency = 0.2 + (0.8 * toolTrans)
                            esp.Tool.Size = UDim2.new(0, 200, 0, 15)
                            esp.Tool.Position = UDim2.new(0, boxX + (mathWidth / 2) - 100, 0, bottomOffset)
                            esp.Tool.Visible = true
                        else
                            esp.Tool.Visible = false
                        end
                        
                        esp.NeedsRefresh = false
                    else
                        SetVisibilityFalse(esp)
                    end
                else
                    SetVisibilityFalse(esp)
                end
            end)
        end
    end)
end)

local EspMainGroup = Tabs.ESP:AddLeftGroupbox("ESP Main")
local EspLineGroup = Tabs.ESP:AddLeftGroupbox("Line & Highlight")

local EspTextGroup = Tabs.ESP:AddRightGroupbox("Text & Display")
local EspSkeletonGroup = Tabs.ESP:AddRightGroupbox("Skeleton")
local TextTransparencyGroup = Tabs.ESP:AddRightGroupbox("Text Transparency")

EspMainGroup:AddToggle("EspMaster", { Text = "Enable Esp", Default = false })

local BoxToggle = EspMainGroup:AddToggle("EspBox", { Text = "Enable Box ESP", Default = false })
BoxToggle:AddColorPicker("BoxColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Box Main Color" })
BoxToggle:AddColorPicker("BoxOutlineColor", { Default = Color3.fromRGB(0, 0, 0), Title = "Box Outline Color" })

EspMainGroup:AddToggle("EspBoxFill", { Text = "Enable Box Fill", Default = false })
EspMainGroup:AddSlider("EspBoxFillTransparency", { Text = "Box Fill Opacity", Default = 0.65, Min = 0, Max = 1, Rounding = 2 })
EspMainGroup:AddLabel("Fill Color 1"):AddColorPicker("EspBoxFillColor1", { Default = Color3.fromRGB(255, 255, 255) })
EspMainGroup:AddLabel("Fill Color 2"):AddColorPicker("EspBoxFillColor2", { Default = Color3.fromRGB(255, 255, 255) })
EspMainGroup:AddLabel("Fill Color 3"):AddColorPicker("EspBoxFillColor3", { Default = Color3.fromRGB(255, 255, 255) })
EspMainGroup:AddToggle("EspBoxFillRotate", { Text = "Rotate Fill", Default = false })
EspMainGroup:AddSlider("EspBoxFillRotateSpeed", { Text = "Fill Rotation Speed", Default = 1, Min = 1, Max = 10, Rounding = 1 })

local LineESPToggle = EspLineGroup:AddToggle("EspLine", { Text = "Enable Line ESP", Default = false })
LineESPToggle:AddColorPicker("LineESPColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Line ESP Color" })
LineESPToggle:AddColorPicker("LineESPOutlineColor", { Default = Color3.fromRGB(0, 0, 0), Title = "Line ESP Outline Color" })

EspLineGroup:AddDropdown("LineESPPosDropdown", { Text = "Line ESP Position", Values = {"Bottom", "Center", "Top"}, Default = 1, Multi = false })
EspLineGroup:AddSlider("LineESPThicknessSlider", { Text = "Line ESP Thickness", Default = 1, Min = 0.5, Max = 5.0, Rounding = 1 })
EspLineGroup:AddSlider("LineESPOutlineThicknessSlider", { Text = "Line ESP Outline Thickness", Default = 2.5, Min = 1.0, Max = 8.0, Rounding = 1 })

local HighlightToggle = EspLineGroup:AddToggle("EspHighlight", { Text = "Enable Highlights", Default = false })
HighlightToggle:AddColorPicker("HighlightVisibleColor", { Default = Color3.fromRGB(0, 255, 0), Title = "Visible Color" })
HighlightToggle:AddColorPicker("HighlightInvisibleColor", { Default = Color3.fromRGB(255, 0, 0), Title = "Invisible Color" })
HighlightToggle:AddColorPicker("HighlightOutlineColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Outline Color" })

EspLineGroup:AddSlider("HighlightFillTransSlider", { Text = "Main Color Transparency", Default = 0.55, Min = 0, Max = 1, Rounding = 2 })
EspLineGroup:AddSlider("HighlightOutlineTransSlider", { Text = "Outline Color Transparency", Default = 0.95, Min = 0, Max = 1, Rounding = 2 })

EspTextGroup:AddDropdown("ESPFontDropdown", { Text = "Font", Values = RobloxFontsList, Default = table.find(RobloxFontsList, "SourceSansBold") or 1, Multi = false })

Options.ESPFontDropdown:OnChanged(function(val)
    pcall(function()
        local selectedFontName = type(val) == "string" and val or Options.ESPFontDropdown.Value
        local targetFont = Enum.Font[selectedFontName]
        if not targetFont then return end
        ESPFontSetting = targetFont
        for _, esp in pairs(ESP_Drawing_Storage) do
            if esp.Name then esp.Name.Font = targetFont end
            if esp.HealthText then esp.HealthText.Font = targetFont end
            if esp.Distance then esp.Distance.Font = targetFont end
            if esp.Tool then esp.Tool.Font = targetFont end
        end
    end)
end)

EspTextGroup:AddToggle("EspName", { Text = "Name", Default = false }):AddColorPicker("NameColor", { Default = Color3.fromRGB(255, 255, 255) })
EspTextGroup:AddDropdown("NameStyleDropdown", { Text = "Name Style", Values = {"Username", "Display Name"}, Default = {"Username"}, Multi = true })

local EspHealthToggle = EspTextGroup:AddToggle("EspHealth", { Text = "Health Bar", Default = false })
EspHealthToggle:AddColorPicker("HPFullColor", { Default = Color3.fromRGB(0, 255, 170), Title = "Full HP Color" })
EspHealthToggle:AddColorPicker("HPHalfColor", { Default = Color3.fromRGB(0, 255, 186), Title = "Half HP Color" })
EspHealthToggle:AddColorPicker("HPLowColor", { Default = Color3.fromRGB(0, 255, 193), Title = "Low HP Color" })

EspTextGroup:AddSlider("HealthLerpSlider", {
    Text = "Health Lerp",
    Default = 0.8,
    Min = 0.1,
    Max = 1.0,
    Rounding = 2,
    Compact = false
})

EspTextGroup:AddToggle("EspHealthText", { Text = "Show HP Number", Default = false }):AddColorPicker("HPTextColor", { Default = Color3.fromRGB(255, 255, 255) })

local EspArmorToggle = EspTextGroup:AddToggle("EspArmor", { Text = "Armor Bar", Default = false })
EspArmorToggle:AddColorPicker("ArmorColor1", { Default = Color3.fromRGB(0, 255, 232), Title = "Armor Color 1" })
EspArmorToggle:AddColorPicker("ArmorColor2", { Default = Color3.fromRGB(0, 216, 255), Title = "Armor Color 2" })

EspTextGroup:AddToggle("EspDistance", { Text = "Draw Distance", Default = false }):AddColorPicker("DistColor", { Default = Color3.fromRGB(255, 255, 255) })
EspTextGroup:AddToggle("EspTool", { Text = "Tool Equipped", Default = false }):AddColorPicker("ToolColor", { Default = Color3.fromRGB(255, 255, 255) })

local SkeletonToggle = EspSkeletonGroup:AddToggle("EspSkeleton", { Text = "Enable Skeleton ESP", Default = false })
SkeletonToggle:AddColorPicker("SkeletonColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Skeleton Main Color" })
SkeletonToggle:AddColorPicker("SkeletonOutlineColor", { Default = Color3.fromRGB(0, 0, 0), Title = "Skeleton Outline Color" })

EspSkeletonGroup:AddSlider("SkeletonThicknessSlider", { Text = "Skeleton Line Thickness", Default = 1, Min = 0.5, Max = 5.0, Rounding = 1 })
EspSkeletonGroup:AddSlider("SkeletonOutlineThicknessSlider", { Text = "Skeleton Outline Thickness", Default = 2.5, Min = 1.0, Max = 8.0, Rounding = 1 })

TextTransparencyGroup:AddToggle("CustomTextTransparency", { Text = "Custom Text Transparency", Default = false })
TextTransparencyGroup:AddSlider("NameTransparencySlider", { Text = "Name Transparency", Default = 0.15, Min = 0, Max = 1, Rounding = 2 })
TextTransparencyGroup:AddSlider("HpTextTransparencySlider", { Text = "HP Text Transparency", Default = 0.2, Min = 0, Max = 1, Rounding = 2 })
TextTransparencyGroup:AddSlider("DistanceTransparencySlider", { Text = "Distance Transparency", Default = 0.15, Min = 0, Max = 1, Rounding = 2 })
TextTransparencyGroup:AddSlider("ToolEquippedTransparencySlider", { Text = "Tool Equipped Transparency", Default = 0.15, Min = 0, Max = 1, Rounding = 2 })

local MoveLeftBox = Tabs.Movement:AddLeftGroupbox("Anti Aim")
local CharacterModifierBox = Tabs.Movement:AddRightGroupbox("Void Hide")

MoveLeftBox:AddToggle("AntiAimToggle", { Text = "Enable Anti Aim", Default = false })
    :AddKeyPicker("AntiAimKeybind", { Default = "None", SyncToggleState = true, Mode = "Toggle", Text = "Anti Aim Bind" })

Toggles.AntiAimToggle:OnChanged(function()
    AntiAimEnabled = Toggles.AntiAimToggle.Value
end)

MoveLeftBox:AddDropdown("AntiAimModeDropdown", { Text = "Anti Aim Stance Type", Values = {"Spin", "Jitter", "Static"}, Default = 1, Multi = false })
Options.AntiAimModeDropdown:OnChanged(function()
    AntiAimMode = Options.AntiAimModeDropdown.Value
end)

MoveLeftBox:AddDropdown("JitterSettingDropdown", { Text = "Jitter Core Settings", Values = {"Multi", "5x always", "3x always", "2x always", "random"}, Default = 1, Multi = false })
Options.JitterSettingDropdown:OnChanged(function()
    JitterMode = Options.JitterSettingDropdown.Value
end)

MoveLeftBox:AddSlider("JitterAngleSlider", { Text = "Jitter Angle", Default = 22.5, Min = 5, Max = 180, Rounding = 1 })
Options.JitterAngleSlider:OnChanged(function()
    AntiAimJitterAngle = Options.JitterAngleSlider.Value
end)

MoveLeftBox:AddSlider("StaticAngleSlider", { Text = "Static Angle", Default = 180, Min = 0, Max = 360, Rounding = 0 })
Options.StaticAngleSlider:OnChanged(function()
    AntiAimStaticAngle = Options.StaticAngleSlider.Value
end)

MoveLeftBox:AddSlider("SpinSpeedSlider", { Text = "Rotation Spin Speed", Default = 15, Min = 5, Max = 50, Rounding = 0 })
Options.SpinSpeedSlider:OnChanged(function()
    AntiAimSpeed = Options.SpinSpeedSlider.Value
end)

CharacterModifierBox:AddToggle("VoidToggle", { Text = "Enable Void", Default = false })
    :AddKeyPicker("VoidKeybind", { Default = "None", SyncToggleState = true, Mode = "Toggle", Text = "Void Bind" })

Toggles.VoidToggle:OnChanged(function()
    VoidEnabled = Toggles.VoidToggle.Value
end)

local LightingColorsGroup = Tabs.World:AddLeftGroupbox("Lighting & Time")
local WorldSkyboxBox = Tabs.World:AddLeftGroupbox("Skybox")
local WorldFogBox = Tabs.World:AddLeftGroupbox("World Fog")
local CameraDisplayBox = Tabs.World:AddRightGroupbox("Camera & Display")
local WorldAtmosphereBox = Tabs.World:AddRightGroupbox("Atmosphere")

LightingColorsGroup:AddSlider("ClockTimeSlider", { Text = "Clock Time", Default = Lighting.ClockTime, Min = 0, Max = 24, Rounding = 1 })
Options.ClockTimeSlider:OnChanged(function()
    Lighting.ClockTime = Options.ClockTimeSlider.Value
end)

LightingColorsGroup:AddLabel("Ambient Color"):AddColorPicker("AmbientColorPicker", { Default = Lighting.Ambient })
Options.AmbientColorPicker:OnChanged(function()
    Lighting.Ambient = Options.AmbientColorPicker.Value
end)

LightingColorsGroup:AddLabel("Outdoor Ambient Color"):AddColorPicker("OutdoorAmbientColorPicker", { Default = Lighting.OutdoorAmbient })
Options.OutdoorAmbientColorPicker:OnChanged(function()
    Lighting.OutdoorAmbient = Options.OutdoorAmbientColorPicker.Value
end)

LightingColorsGroup:AddLabel("ColorShift Top Color"):AddColorPicker("ColorShiftTopColorPicker", { Default = Lighting.ColorShift_Top })
Options.ColorShiftTopColorPicker:OnChanged(function()
    Lighting.ColorShift_Top = Options.ColorShiftTopColorPicker.Value
end)

LightingColorsGroup:AddLabel("ColorShift Bottom Color"):AddColorPicker("ColorShiftBottomColorPicker", { Default = Lighting.ColorShift_Bottom })
Options.ColorShiftBottomColorPicker:OnChanged(function()
    Lighting.ColorShift_Bottom = Options.ColorShiftBottomColorPicker.Value
end)

WorldSkyboxBox:AddToggle("SkyboxToggle", { Text = "Enable Custom Skybox", Default = false })
Toggles.SkyboxToggle:OnChanged(function()
    SkyboxEnabled = Toggles.SkyboxToggle.Value
    UpdateSkybox()
end)

WorldSkyboxBox:AddDropdown("SkyboxDropdown", { Text = "Select Skybox", Values = {"Minecraft", "Minecraft 2", "dark star", "snow", "realistic", "solid black", "blue"}, Default = 1, Multi = false })
Options.SkyboxDropdown:OnChanged(function()
    SelectedSkybox = Options.SkyboxDropdown.Value
    UpdateSkybox()
end)

WorldFogBox:AddLabel("Fog Color"):AddColorPicker("FogColorPicker", { Default = Lighting.FogColor })
Options.FogColorPicker:OnChanged(function()
    Lighting.FogColor = Options.FogColorPicker.Value
end)

WorldFogBox:AddSlider("FogStartSlider", { Text = "Fog Start Distance", Default = Lighting.FogStart, Min = 0, Max = 5000, Rounding = 0 })
Options.FogStartSlider:OnChanged(function()
    Lighting.FogStart = Options.FogStartSlider.Value
end)

WorldFogBox:AddSlider("FogEndSlider", { Text = "Fog End Distance", Default = Lighting.FogEnd, Min = 0, Max = 10000, Rounding = 0 })
Options.FogEndSlider:OnChanged(function()
    Lighting.FogEnd = Options.FogEndSlider.Value
end)

WorldAtmosphereBox:AddToggle("AtmosphereToggle", { Text = "Enable Atmosphere", Default = false })
Toggles.AtmosphereToggle:OnChanged(function()
    AtmosphereEnabled = Toggles.AtmosphereToggle.Value
    UpdateAtmosphere()
end)

WorldAtmosphereBox:AddLabel("Tint Color"):AddColorPicker("AtmosColorPicker", { Default = AtmosphereColor })
Options.AtmosColorPicker:OnChanged(function()
    AtmosphereColor = Options.AtmosColorPicker.Value
    UpdateAtmosphere()
end)

WorldAtmosphereBox:AddLabel("Decay Color"):AddColorPicker("AtmosDecayPicker", { Default = AtmosphereDecay })
Options.AtmosDecayPicker:OnChanged(function()
    AtmosphereDecay = Options.AtmosDecayPicker.Value
    UpdateAtmosphere()
end)

WorldAtmosphereBox:AddSlider("AtmosGlareSlider", { Text = "Glare Intensity", Default = AtmosphereGlare, Min = 0.1, Max = 10, Rounding = 1 })
Options.AtmosGlareSlider:OnChanged(function()
    AtmosphereGlare = Options.AtmosGlareSlider.Value
    UpdateAtmosphere()
end)

WorldAtmosphereBox:AddSlider("AtmosHazeSlider", { Text = "Haze Density", Default = AtmosphereHaze, Min = 0.1, Max = 10, Rounding = 1 })
Options.AtmosHazeSlider:OnChanged(function()
    AtmosphereHaze = Options.AtmosHazeSlider.Value
    UpdateAtmosphere()
end)

WorldAtmosphereBox:AddSlider("AtmosOffsetSlider", { Text = "Horizon Offset", Default = AtmosphereOffset, Min = 0.01, Max = 1, Rounding = 2 })
Options.AtmosOffsetSlider:OnChanged(function()
    AtmosphereOffset = Options.AtmosOffsetSlider.Value
    UpdateAtmosphere()
end)

WorldAtmosphereBox:AddSlider("AtmosDensitySlider", { Text = "Atmosphere Density", Default = AtmosphereDensity, Min = 0.01, Max = 1, Rounding = 2 })
Options.AtmosDensitySlider:OnChanged(function()
    AtmosphereDensity = Options.AtmosDensitySlider.Value
    UpdateAtmosphere()
end)

CameraDisplayBox:AddSlider("CustomFovSlider", { Text = "Custom FOV", Default = 70, Min = 70, Max = 120, Rounding = 0 })
Options.CustomFovSlider:OnChanged(function()
    CustomFOVValue = Options.CustomFovSlider.Value
    Camera.FieldOfView = CustomFOVValue
end)

Camera:GetPropertyChangedSignal("FieldOfView"):Connect(function()
    if Camera.FieldOfView ~= CustomFOVValue then
        Camera.FieldOfView = CustomFOVValue
    end
end)

CameraDisplayBox:AddSlider("BlurSlider", { Text = "World Blur", Default = 0, Min = 0, Max = 5, Rounding = 1 })
Options.BlurSlider:OnChanged(function()
    local blurValue = Options.BlurSlider.Value
    local blurEffect = Lighting:FindFirstChild("hooksenseWorldBlur")
    if blurValue > 0 then
        if not blurEffect then
            blurEffect = Instance.new("BlurEffect")
            blurEffect.Name = "hooksenseWorldBlur"
            blurEffect.Parent = Lighting
        end
        blurEffect.Size = blurValue * 8
    else
        if blurEffect then blurEffect:Destroy() end
    end
end)

CameraDisplayBox:AddToggle("MotionBlurToggle", { Text = "Enable Motion Blur", Default = false })
Toggles.MotionBlurToggle:OnChanged(function()
    MotionBlurEnabled = Toggles.MotionBlurToggle.Value
end)

CameraDisplayBox:AddSlider("MotionBlurIntensitySlider", { Text = "Motion Blur Intensity", Default = 1.5, Min = 0.5, Max = 5.0, Rounding = 1 })
Options.MotionBlurIntensitySlider:OnChanged(function()
    MotionBlurIntensity = Options.MotionBlurIntensitySlider.Value
end)

CameraDisplayBox:AddSlider("FpsCapSlider", { Text = "FPS Cap Limit", Default = 60, Min = 60, Max = 999, Rounding = 0 })
Options.FpsCapSlider:OnChanged(function()
    if setfpscap then setfpscap(Options.FpsCapSlider.Value) end
end)

local BlacklistPlayersGroup = Tabs.Addons:AddRightGroupbox("Loaders Scripts")
local ViewPlayerGroup = Tabs.Addons:AddLeftGroupbox("View Player")

BlacklistPlayersGroup:AddButton({ Text = "walkspeed", Func = function()
    local success, err = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/19mdSkibidi/19sMooze-Mobile-Rework/refs/heads/main/Mooze%20Mob'))()
    end)
    if success then
        Library:Notify("successfully!")
    else
        Library:Notify("Error loading script: " .. tostring(err))
    end
end })

BlacklistPlayersGroup:AddButton({ Text = "Minecraft texture", Func = function()
    local success, err = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/xqmt/hooksense-loader/refs/heads/main/Minecraft%20texture'))()
    end)
    if success then
        Library:Notify("successfully!")
    else
        Library:Notify("Error loading script: " .. tostring(err))
    end
end })

BlacklistPlayersGroup:AddButton({ Text = "rapid fire", Func = function()
    local success, err = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/xqmt/hooksense-loader/refs/heads/main/rapid%20fire'))()
    end)
    if success then
        Library:Notify("successfully!")
    else
        Library:Notify("Error loading script: " .. tostring(err))
    end
end })

BlacklistPlayersGroup:AddButton({ Text = "Primordial visual gui", Func = function()
    local success, err = pcall(function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/xqmt/Resolution-loader/refs/heads/main/gamesense%20China%20hat'))()
    end)
    if success then
        Library:Notify("successfully!")
    else
        Library:Notify("Error loading script: " .. tostring(err))
    end
end })


local function StopViewingPlayer()
    if ViewPlayerConnection then
        ViewPlayerConnection:Disconnect()
        ViewPlayerConnection = nil
    end
    ViewingPlayer = nil
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        Camera.CameraSubject = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    end
end

local function GetUsernameFromEntry(entryText)
    if not entryText or entryText == "No Players Online" then return nil end
    local username = string.match(entryText, "%(@([^%)]+)%)")
    return username or entryText
end

local function GetPlayerNamesList()
    local list = {}
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            table.insert(list, p.DisplayName .. " (@" .. p.Name .. ")")
        end
    end
    if #list == 0 then
        table.insert(list, "No Players Online")
    end
    return list
end

ViewPlayerGroup:AddDropdown("ViewPlayerDropdown", { Text = "Select Player to View", Values = GetPlayerNamesList(), Default = 1, Multi = false })

local function RefreshViewPlayerList()
    local currentList = GetPlayerNamesList()
    Options.ViewPlayerDropdown:SetValues(currentList)
    if currentList[1] then
        Options.ViewPlayerDropdown:SetValue(currentList[1])
    end
end

Players.PlayerAdded:Connect(function()
    task.wait(0.2)
    RefreshViewPlayerList()
end)

Players.PlayerRemoving:Connect(function(player)
    if ViewingPlayer == player then
        StopViewingPlayer()
        Library:Notify("View Player: Target left the game, camera reset.")
    end
    task.wait(0.2)
    RefreshViewPlayerList()
end)

ViewPlayerGroup:AddButton({ Text = "View Target", Func = function()
    local rawSelected = Options.ViewPlayerDropdown.Value
    local selectedName = GetUsernameFromEntry(rawSelected)
    if not selectedName then
        Library:Notify("Please select a valid player!")
        return
    end

    local targetPlayer = Players:FindFirstChild(selectedName)
    if targetPlayer then
        ViewingPlayer = targetPlayer
        if ViewPlayerConnection then ViewPlayerConnection:Disconnect() end

        ViewPlayerConnection = RunService.RenderStepped:Connect(function()
            if ViewingPlayer and ViewingPlayer.Parent then
                local char = ViewingPlayer.Character
                if char then
                    local hum = char:FindFirstChildOfClass("Humanoid")
                    if hum then
                        Camera.CameraSubject = hum
                    end
                end
            else
                StopViewingPlayer()
                Library:Notify("View Player: Target left the game.")
            end
        end)
        Library:Notify("Now viewing: " .. targetPlayer.DisplayName)
    else
        Library:Notify("Player not found!")
    end
end })

ViewPlayerGroup:AddButton({ Text = "Unview Target", Func = function()
    StopViewingPlayer()
    Library:Notify("Stopped viewing player.")
end })

ViewPlayerGroup:AddButton({ Text = "Go to Player", Func = function()
    local rawSelected = Options.ViewPlayerDropdown.Value
    local selectedName = GetUsernameFromEntry(rawSelected)
    if not selectedName then
        Library:Notify("Please select a valid player!")
        return
    end

    local targetPlayer = Players:FindFirstChild(selectedName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if myRoot then
            myRoot.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
            Library:Notify("Teleported to: " .. targetPlayer.DisplayName)
        else
            Library:Notify("Your character root part not found!")
        end
    else
        Library:Notify("Target character not found or dead!")
    end
end })

local InterfaceGroup = Tabs.UI:AddLeftGroupbox("Font menu")
local MenuGroup = Tabs.UI:AddLeftGroupbox("Menu Settings")

InterfaceGroup:AddDropdown("UIFontDropdown", { Text = "Font", Values = RobloxFontsList, Default = table.find(RobloxFontsList, "Cartoon") or 1, Multi = false })

Options.UIFontDropdown:OnChanged(function(val)
    pcall(function()
        local selectedFontName = type(val) == "string" and val or Options.UIFontDropdown.Value
        local targetFont = Enum.Font[selectedFontName]
        if not targetFont then return end
        GlobalFontSetting = targetFont
        TargetBtn.Font = targetFont
        
        local parentGui = Library.ScreenGui or Library.Main
        if not parentGui then
            parentGui = CoreGui:FindFirstChild("Linoria") or CoreGui:FindFirstChild("ScreenGui")
        end
        
        if parentGui then
            for _, desc in ipairs(parentGui:GetDescendants()) do
                if desc:IsA("TextLabel") or desc:IsA("TextButton") or desc:IsA("TextBox") then 
                    desc.Font = targetFont 
                end
            end
        end
    end)
end)

MenuGroup:AddToggle("KeybindMenuOpen", { Default = false, Text = "Open Keybind Menu", Callback = function(value) Library.KeybindFrame.Visible = value end})
MenuGroup:AddToggle("ShowCustomCursor", {Text = "Custom Cursor", Default = true, Callback = function(Value) Library.ShowCustomCursor = Value end})
MenuGroup:AddDivider()
MenuGroup:AddLabel("Menu bind"):AddKeyPicker("MenuKeybind", { Default = "RightShift", NoUI = true, Text = "Menu keybind" })
MenuGroup:AddButton("Unload", function() Library:Unload() end)

Library.ToggleKeybind = Options.MenuKeybind

Library.OnUnload(function()
    StopViewingPlayer()
    for player, _ in pairs(ESP_Drawing_Storage) do
        RemoveDrawingESP(player)
    end
    table.clear(ESP_Drawing_Storage)
    if LockButtonGui then LockButtonGui:Destroy() end
    if BoxFillGui then BoxFillGui:Destroy() end
    if SelfHighlightInstance then SelfHighlightInstance:Destroy() end
    if FOVCircleOutline then FOVCircleOutline:Remove() end
    if FOVCircle then FOVCircle:Remove() end
    
    Library.Unloaded = true
end)

ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)
SaveManager:IgnoreThemeSettings()
ThemeManager:SetFolder("Resolution")
SaveManager:SetFolder("Resolution/configs")
SaveManager:BuildConfigSection(Tabs.UI)
ThemeManager:ApplyToTab(Tabs.UI)
SaveManager:LoadAutoloadConfig()


loadstring(game:HttpGet("https://gitlab.com/Tsuniox/lua-stuff/-/raw/master/R15GUI.lua"))()
loadstring(game:HttpGet("https://raw.githubusercontent.com/FastyGame31/blyat-hub/main/by%20fasty"))()
game.Players.LocalPlayer.Character.UpperTorso.Waist:remove()
-- Open the Dev Console (F9) to read information about the GUI --

print("\nSuper Power Training Simulator LuckyGui Created by LuckyMMB @ V3rmillion.net\nDiscord https://discord.gg/GKzJnUC\nLast updated 5th December 2018")
print("\nThe Y Key activates Panic Mode and teleports you to the Safe Zone. This can be changed to another key.")
print("\nUse the OnDeath Return button to respawn you and return to your previous position\nwhen you are killed. Great for farming BT in zones as soon as you\ncan take 1 hit and survive (eg. 6Bil in 100Bil+ BT Taining Area).")
print("\nOnce your Fist, Body and Psychic stats are higher you will not be able to Auto\nFarm more than one skill at a time as you need to be near the\nlocation you are farming at.")

local plr = game:GetService("Players").LocalPlayer
local char = plr.Character
local root = char.HumanoidRootPart
local Plrs = game:GetService("Players")
local MyPlr = Plrs.LocalPlayer
local MyChar = MyPlr.Character
local UIS = game:GetService'UserInputService'
local RepStor = game:GetService("ReplicatedStorage")
local CoreGui = game:GetService("CoreGui")
local Run = game:GetService("RunService")
local mouse = game.Players.LocalPlayer:GetMouse()
local human = plr.Character:WaitForChild("Humanoid")

-- Anti Idle --
local VirtualUser=game:service'VirtualUser'
game:service'Players'.LocalPlayer.Idled:connect(function()
	VirtualUser:CaptureController()
	VirtualUser:ClickButton2(Vector2.new())
end)

showstartmessage = true
showtopplayersactive = false
showtopplayersfistactive = false
showtopplayersbodyactive = false
showtopplayersspeedactive = false
showtopplayersjumpactive = false
showtopplayerspsychicactive = false
farmbtsafetyactive = false
farmbtsafety2active = false
settplocation = false
playerdied = false
deathreturnactive = false
godmodeactive = false
noclip = false
resetplayerstat = false
killplayeractive = false
farmallactive = false
farmfistactive = false
farmbodyactive = false
farmspeedactive = false
farmjumpactive = false
farmpsychicactive = false
punchmodeactive = false
ESPEnabled = false
ESPLength = 20000

CharAddedEvent = { }

Plrs.PlayerAdded:connect(function(plr)
	if CharAddedEvent[plr.Name] == nil then
		CharAddedEvent[plr.Name] = plr.CharacterAdded:connect(function(char)
			if ESPEnabled then
				RemoveESP(plr)
				CreateESP(plr)
			end
		end)
	end
end)

Plrs.PlayerRemoving:connect(function(plr)
	if CharAddedEvent[plr.Name] ~= nil then
		CharAddedEvent[plr.Name]:Disconnect()
		CharAddedEvent[plr.Name] = nil
	end
	RemoveESP(plr)
end)

function CreateESP(plr)
	if plr ~= nil then
		local GetChar = plr.Character
		if not GetChar then return end
		local GetHead do
			repeat wait() until GetChar:FindFirstChild("Head")
		end
		GetHead = GetChar.Head
		
		local bb = Instance.new("BillboardGui", CoreGui)
		bb.Adornee = GetHead
		bb.ExtentsOffset = Vector3.new(0, 1, 0)
		bb.AlwaysOnTop = true
		bb.Size = UDim2.new(0, 5, 0, 5)
		bb.StudsOffset = Vector3.new(0, 3, 0)
		bb.Name = "ESP_" .. plr.Name
		
		local frame = Instance.new("Frame", bb)
		frame.ZIndex = 10
		frame.BackgroundTransparency = 1
		frame.Size = UDim2.new(1, 0, 1, 0)
		
		local TxtName = Instance.new("TextLabel", frame)
		TxtName.Name = "Names"
		TxtName.ZIndex = 10
		TxtName.Text = plr.Name
		TxtName.BackgroundTransparency = 1
		TxtName.Position = UDim2.new(0, 0, 0, -45)
		TxtName.Size = UDim2.new(1, 0, 10, 0)
		TxtName.Font = "SourceSansBold"
		TxtName.TextColor3 = Color3.new(0, 0, 0)
		TxtName.TextSize = 14
		TxtName.TextStrokeTransparency = 0.5
		
		local TxtDist = Instance.new("TextLabel", frame)
		TxtDist.Name = "Dist"
		TxtDist.ZIndex = 10
		TxtDist.Text = ""
		TxtDist.BackgroundTransparency = 1
		TxtDist.Position = UDim2.new(0, 0, 0, -35)
		TxtDist.Size = UDim2.new(1, 0, 10, 0)
		TxtDist.Font = "SourceSansBold"
		TxtDist.TextColor3 = Color3.new(0, 0, 0)
		TxtDist.TextSize = 15
		TxtDist.TextStrokeTransparency = 0.5

		local TxtHealth = Instance.new("TextLabel", frame)
		TxtHealth.Name = "Health"
		TxtHealth.ZIndex = 10
		TxtHealth.Text = ""
		TxtHealth.BackgroundTransparency = 1
		TxtHealth.Position = UDim2.new(0, 0, 0, -25)
		TxtHealth.Size = UDim2.new(1, 0, 10, 0)
		TxtHealth.Font = "SourceSansBold"
		TxtHealth.TextColor3 = Color3.new(0, 0, 0)
		TxtHealth.TextSize = 15
		TxtHealth.TextStrokeTransparency = 0.5

		local TxtFist = Instance.new("TextLabel", frame)
		TxtFist.Name = "Fist"
		TxtFist.ZIndex = 10
		TxtFist.Text = ""
		TxtFist.BackgroundTransparency = 1
		TxtFist.Position = UDim2.new(0, 0, 0, -15)
		TxtFist.Size = UDim2.new(1, 0, 10, 0)
		TxtFist.Font = "SourceSansBold"
		TxtFist.TextColor3 = Color3.new(0, 0, 0)
		TxtFist.TextSize = 15
		TxtFist.TextStrokeTransparency = 0.5

		local TxtPsychic = Instance.new("TextLabel", frame)
		TxtPsychic.Name = "Psychic"
		TxtPsychic.ZIndex = 10
		TxtPsychic.Text = ""
		TxtPsychic.BackgroundTransparency = 1
		TxtPsychic.Position = UDim2.new(0, 0, 0, -5)
		TxtPsychic.Size = UDim2.new(1, 0, 10, 0)
		TxtPsychic.Font = "SourceSansBold"
		TxtPsychic.TextColor3 = Color3.new(0, 0, 0)
		TxtPsychic.TextSize = 15
		TxtPsychic.TextStrokeTransparency = 0.5
	end
end

function UpdateESP(plr)
	local Find = CoreGui:FindFirstChild("ESP_" .. plr.Name)
	if Find then
		local plrStatus = game.Players[plr.Name].leaderstats.Status
		if plrStatus.Value == "Criminal" then
			Find.Frame.Names.TextColor3 = Color3.new(1, 0.1, 1)
		elseif plrStatus.Value == "Lawbreaker" then
			Find.Frame.Names.TextColor3 = Color3.new(1, 0.1, 0.1)
		elseif plrStatus.Value == "Guardian" then
			Find.Frame.Names.TextColor3 = Color3.new(0.1, 0.8, 1)
		elseif plrStatus.Value == "Protector" then
			Find.Frame.Names.TextColor3 = Color3.new(0.1, 0.1, 1)
		elseif plrStatus.Value == "Supervillain" then
			Find.Frame.Names.TextColor3 = Color3.new(0.3, 0.1, 0.1)
		elseif plrStatus.Value == "Superhero" then
			Find.Frame.Names.TextColor3 = Color3.new(0.8, 0.8, 0)
		else
			Find.Frame.Names.TextColor3 = Color3.new(1, 1, 1)
		end
		Find.Frame.Dist.TextColor3 = Color3.new(1, 1, 1)
		Find.Frame.Health.TextColor3 = Color3.new(1, 1, 1)
		Find.Frame.Fist.TextColor3 = Color3.new(1, 1, 1)
		Find.Frame.Psychic.TextColor3 = Color3.new(1, 1, 1)
		local GetChar = plr.Character
		if MyChar and GetChar then
			local Find2 = MyChar:FindFirstChild("HumanoidRootPart")
			local Find3 = GetChar:FindFirstChild("HumanoidRootPart")
			local Find4 = GetChar:FindFirstChildOfClass("Humanoid")
			if Find2 and Find3 then
				local pos = Find3.Position
				local Dist = (Find2.Position - pos).magnitude
				if Dist > ESPLength then
					Find.Frame.Names.Visible = false
					Find.Frame.Dist.Visible = false
					Find.Frame.Health.Visible = false
					Find.Frame.Fist.Visible = false
					Find.Frame.Psychic.Visible = false
					return
				else
					Find.Frame.Names.Visible = true
					Find.Frame.Dist.Visible = true
					Find.Frame.Health.Visible = true
					Find.Frame.Fist.Visible = true
					Find.Frame.Psychic.Visible = true
				end
				Find.Frame.Dist.Text = "Distance: " .. string.format("%.0f", Dist)
				--Find.Frame.Pos.Text = "(X: " .. string.format("%.0f", pos.X) .. ", Y: " .. string.format("%.0f", pos.Y) .. ", Z: " .. string.format("%.0f", pos.Z) .. ")"
				if Find4 then
					Find.Frame.Health.Text = "Health: " ..converttoletter(string.format("%.0f", Find4.Health))
					Find.Frame.Fist.Text = "Fist: " ..converttoletter(string.format("%.0f", game.Players[plr.Name].PrivateStats.FistStrength.Value))
					Find.Frame.Psychic.Text = "Psychic: " ..converttoletter(string.format("%.0f", game.Players[plr.Name].PrivateStats.PsychicPower.Value))
				else
					Find.Frame.Health.Text = ""
					Find.Frame.Fist.Text = ""
					Find.Frame.Psychic.Text = ""
				end
			end
		end
	end
end

function RemoveESP(plr)
	local ESP = CoreGui:FindFirstChild("ESP_" .. plr.Name)
	if ESP then
		ESP:Destroy()
	end
end

local MainGUI = Instance.new("ScreenGui")
local TopFrame = Instance.new("Frame")
local MainFrame = Instance.new("Frame")
local Open = Instance.new("TextButton")
local Close = Instance.new("TextButton")
local Minimize = Instance.new("TextButton")
local cf = Instance.new("Frame")
local c1 = Instance.new("TextLabel")
local c = Instance.new("TextButton")
local DeathReturn = Instance.new("TextButton")
local PunchMode = Instance.new("TextButton")
local WayPoints = Instance.new("TextButton")
local WayPointsFrame = Instance.new("Frame")
local FarmExp = Instance.new("TextButton")
local FarmExpFrame = Instance.new("Frame")
local ShowLocation = Instance.new("TextLabel")
local SetLocation = Instance.new("TextButton")
local TPLocation = Instance.new("TextButton")
local Location1 = Instance.new("TextButton")
local Location2 = Instance.new("TextButton")
local LocationFS1B = Instance.new("TextButton")
local LocationFS100B = Instance.new("TextButton")
local LocationFS10T = Instance.new("TextButton")
local Location3 = Instance.new("TextButton")
local Location4 = Instance.new("TextButton")
local Location5 = Instance.new("TextButton")
local Location6 = Instance.new("TextButton")
local Location7 = Instance.new("TextButton")
local Location8 = Instance.new("TextButton")
local Location9 = Instance.new("TextButton")
local Location10 = Instance.new("TextButton")
local LocationBT1B = Instance.new("TextButton")
local LocationBT100B = Instance.new("TextButton")
local LocationBT10T = Instance.new("TextButton")
local LocationPP1M = Instance.new("TextButton")
local LocationPP1B = Instance.new("TextButton")
local LocationPP1T = Instance.new("TextButton")
local LocationPP1Qa = Instance.new("TextButton")
local LocationBody1B = Instance.new("TextButton")
local FarmAll = Instance.new("TextButton")
local FarmFist = Instance.new("TextButton")
local FarmBody = Instance.new("TextButton")
local FarmSpeed = Instance.new("TextButton")
local FarmJump = Instance.new("TextButton")
local SavePosition = Instance.new("TextLabel")
local FarmPsychic = Instance.new("TextButton")
local FarmBodyLabel = Instance.new("TextLabel")
local FarmSpeedLabel = Instance.new("TextLabel")
local esptrack = Instance.new("TextButton")
local ESPLength = Instance.new("TextBox")
local Extras = Instance.new("TextButton")
local ExtrasFrame = Instance.new("Frame")
local PlayerInfo = Instance.new("TextButton")
local PlayerInfoFrame = Instance.new("Frame")
local ShowTopPlayers = Instance.new("TextButton")
local ShowBetterFS = Instance.new("TextButton")
local ShowBetterBT = Instance.new("TextButton")
local ShowBetterPP = Instance.new("TextButton")
local ShowWorseFS = Instance.new("TextButton")
local ShowWorseBT = Instance.new("TextButton")
local ShowWorsePP = Instance.new("TextButton")
local PlayerInfoStatsFrame = Instance.new("Frame")
local PlayerInfoStatsClose = Instance.new("TextButton")
local StatBestFistText1 = Instance.new("TextLabel")
local StatBestBodyText1 = Instance.new("TextLabel")
local StatBestSpeedText1 = Instance.new("TextLabel")
local StatBestJumpText1 = Instance.new("TextLabel")
local StatBestPsychicText1 = Instance.new("TextLabel")
local PlayerInfoStatsText1 = Instance.new("TextLabel")
local ShowStatsFist1 = Instance.new("TextLabel")
local ShowStatsBody1 = Instance.new("TextLabel")
local ShowStatsSpeed1 = Instance.new("TextLabel")
local ShowStatsJump1 = Instance.new("TextLabel")
local ShowStatsPsychic1 = Instance.new("TextLabel")
local ShowStatsFist2 = Instance.new("TextLabel")
local ShowStatsBody2 = Instance.new("TextLabel")
local ShowStatsSpeed2 = Instance.new("TextLabel")
local ShowStatsJump2 = Instance.new("TextLabel")
local ShowStatsPsychic2 = Instance.new("TextLabel")
local AnnoyNameLabel = Instance.new("TextLabel")
local AnnoyName = Instance.new("TextBox")
local AnnoyStart = Instance.new("TextButton")
local KillPlayerStart = Instance.new("TextButton")
local TptoPlayer = Instance.new("TextButton")
local PanicToggleLabel = Instance.new("TextLabel")
local farmbtsafety = Instance.new("TextButton")
local farmbtsafetyText1 = Instance.new("TextLabel")
local farmbtsafetylevel = Instance.new("TextBox")
local farmbtsafety2 = Instance.new("TextButton")
local farmbtsafetylabel = Instance.new("TextLabel")
local PanicToggle = Instance.new("TextBox")
local ReJoinServer = Instance.new("TextButton")
local InfoScreen = Instance.new("TextButton")
local InfoFrame = Instance.new("Frame")
local InfoText1 = Instance.new("TextLabel")
local PlayerName = Instance.new("TextBox")
local StatsFrame = Instance.new("Frame")
local ShowStats1 = Instance.new("TextLabel")
local ShowStats2 = Instance.new("TextLabel")
local StatNameSet = Instance.new("TextButton")
local NoClip = Instance.new("TextButton")
local GodMode = Instance.new("TextButton")

-- Properties

MainGUI.Name = "MainGUI"
MainGUI.Parent = game.CoreGui
MainGUI.ResetOnSpawn = false
local MainCORE = game.CoreGui["MainGUI"]

TopFrame.Name = "TopFrame"
TopFrame.Parent = MainGUI
TopFrame.BackgroundColor3 = Color3.new(0, 0, 0)
TopFrame.BorderColor3 = Color3.new(0, 0, 0)
TopFrame.BackgroundTransparency = 1
TopFrame.Position = UDim2.new(0.5, -30, 0, -27)
TopFrame.Size = UDim2.new(0, 80, 0, 20)
TopFrame.Visible = false

cf.Name = "cf"
cf.Parent = MainGUI
cf.BackgroundColor3 = Color3.new(0, 0, 0)
cf.BorderColor3 = Color3.new(0.5, 0.5, 0.5)
cf.BackgroundTransparency = 0
cf.Position = UDim2.new(0.5, -195, 0.5, -110)
cf.Size = UDim2.new(0, 390, 0, 220)
cf.Visible = true

c1.Name = "c1"
c1.Parent = cf
c1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
c1.BackgroundTransparency = 1
c1.Position = UDim2.new(0, 10, 0, 13)
c1.Size = UDim2.new(0, 370, 0, 160)
c1.Font = Enum.Font.Fantasy
c1.TextColor3 = Color3.new(1, 1, 1)
c1.Text = "SUPER POWERS TRAINING SIMULATOR GUI\nmade by LuckyMMB#8645 (discord)\n\nPress F9 to read more information about the GUI\n\nThis GUI is free, if you paid for it you were scammed\nand should report it.\n\nNo unauthorized use of this GUI without written\npermission from the creator."
c1.TextSize = 17

c.Name = "c"
c.Parent = cf
c.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
c.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
c.Position = UDim2.new(0.5, -30, 0, 190)
c.Size = UDim2.new(0, 60, 0, 20)
c.Font = Enum.Font.Fantasy
c.Text = "CLOSE"
c.TextColor3 = Color3.new(1, 0, 0)
c.TextSize = 17
c.TextWrapped = true

Open.Name = "Open"
Open.Parent = TopFrame
Open.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
Open.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Open.Size = UDim2.new(0, 60, 0, 20)
Open.Font = Enum.Font.Fantasy
Open.Text = "Open"
Open.TextColor3 = Color3.new(1, 1, 1)
Open.TextSize = 18
Open.Selectable = true
Open.TextWrapped = true

MainFrame.Name = "MainFrame"
MainFrame.Parent = MainGUI
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
MainFrame.BackgroundTransparency = 0.5
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.5, -382.5, 0, -32)
MainFrame.Size = UDim2.new(0, 765, 0, 30)
if not cf.Visible then MainGUI:Destroy() else MainFrame.Visible = true end

Close.Name = "Close"
Close.Parent = MainFrame
Close.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
Close.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Close.Position = UDim2.new(0, 10, 0, 5)
Close.Size = UDim2.new(0, 20, 0, 20)
Close.Font = Enum.Font.Fantasy
Close.Text = "X"
Close.TextColor3 = Color3.new(1, 0, 0)
Close.TextSize = 17
Close.TextScaled = true
Close.TextWrapped = true

Minimize.Name = "Minimize"
Minimize.Parent = MainFrame
Minimize.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
Minimize.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Minimize.Position = UDim2.new(0, 35, 0, 5)
Minimize.Size = UDim2.new(0, 20, 0, 20)
Minimize.Font = Enum.Font.Fantasy
Minimize.Text = "-"
Minimize.TextColor3 = Color3.new(1, 0, 1)
Minimize.TextSize = 17
Minimize.TextScaled = true
Minimize.TextWrapped = true

WayPoints.Name = "WayPoints"
WayPoints.Parent = MainFrame
WayPoints.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
WayPoints.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
WayPoints.Position = UDim2.new(0, 60, 0, 5)
WayPoints.Size = UDim2.new(0, 65, 0, 20)
WayPoints.Font = Enum.Font.Fantasy
WayPoints.TextColor3 = Color3.new(1, 1, 1)
WayPoints.Text = "Teleport"
WayPoints.TextSize = 17
WayPoints.TextWrapped = true

WayPointsFrame.Name = "WayPointsFrame"
WayPointsFrame.Parent = MainFrame
WayPointsFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
WayPointsFrame.BorderColor3 = Color3.new(0, 0, 0)
WayPointsFrame.BackgroundTransparency = 0.2
WayPointsFrame.Position = UDim2.new(0, 1, 0, 33)
WayPointsFrame.Size = UDim2.new(0, 375, 0, 480)
WayPointsFrame.Visible = false

FarmExp.Name = "FarmExp"
FarmExp.Parent = MainFrame
FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmExp.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmExp.Position = UDim2.new(0, 130, 0, 5)
FarmExp.Size = UDim2.new(0, 75, 0, 20)
FarmExp.Font = Enum.Font.Fantasy
FarmExp.TextColor3 = Color3.new(1, 1, 1)
FarmExp.Text = "Farm Exp"
FarmExp.TextSize = 17
FarmExp.TextWrapped = true

FarmExpFrame.Name = "FarmExpFrame"
FarmExpFrame.Parent = MainFrame
FarmExpFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmExpFrame.BorderColor3 = Color3.new(0, 0, 0)
FarmExpFrame.BackgroundTransparency = 0.2
FarmExpFrame.Position = UDim2.new(0, 62.5, 0, 33)
FarmExpFrame.Size = UDim2.new(0, 210, 0, 165)
FarmExpFrame.Visible = false

ShowLocation.Name = "ShowLocation"
ShowLocation.Parent = WayPointsFrame
ShowLocation.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowLocation.TextColor3 = Color3.new(1, 1, 1)
ShowLocation.BorderColor3 = Color3.new(0, 0, 0)
ShowLocation.Position = UDim2.new(0, 5, 0, 5)
ShowLocation.Size = UDim2.new(0, 170, 0, 20)
ShowLocation.Font = Enum.Font.Fantasy
ShowLocation.Text = "Current Location"
ShowLocation.TextWrapped = true
ShowLocation.TextSize = 15

SetLocation.Name = "SetLocation"
SetLocation.Parent = WayPointsFrame
SetLocation.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
SetLocation.TextColor3 = Color3.new(1, 1, 1)
SetLocation.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
SetLocation.Position = UDim2.new(0, 180, 0, 5)
SetLocation.Size = UDim2.new(0, 120, 0, 20)
SetLocation.Font = Enum.Font.Fantasy
SetLocation.Text = "Set Location"
SetLocation.TextWrapped = true
SetLocation.TextSize = 16

TPLocation.Name = "TPLocation"
TPLocation.Parent = WayPointsFrame
TPLocation.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
TPLocation.TextColor3 = Color3.new(1, 1, 1)
TPLocation.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
TPLocation.Position = UDim2.new(0, 305, 0, 5)
TPLocation.Size = UDim2.new(0, 65, 0, 20)
TPLocation.Font = Enum.Font.Fantasy
TPLocation.Text = "Tp to"
TPLocation.TextWrapped = true
TPLocation.TextSize = 16

Location1.Name = "Location1"
Location1.Parent = WayPointsFrame
Location1.BackgroundColor3 = Color3.new(255/255, 94/255, 40/255)
Location1.TextColor3 = Color3.new(1, 1, 1)
Location1.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location1.Position = UDim2.new(0, 5, 0, 30)
Location1.Size = UDim2.new(0, 365, 0, 20)
Location1.Font = Enum.Font.Fantasy
Location1.Text = "Teleport to Safe Zone"
Location1.TextWrapped = true
Location1.TextSize = 16

Location2.Name = "Location2"
Location2.Parent = WayPointsFrame
Location2.BackgroundColor3 = Color3.new(70/255, 105/255, 0)
Location2.TextColor3 = Color3.new(1, 1, 1)
Location2.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location2.Position = UDim2.new(0, 5, 0, 55)
Location2.Size = UDim2.new(0, 365, 0, 20)
Location2.Font = Enum.Font.Fantasy
Location2.Text = "Teleport to Rock [10x Fist Strength]"
Location2.TextWrapped = true
Location2.TextSize = 16

Location7.Name = "Location7"
Location7.Parent = WayPointsFrame
Location7.BackgroundColor3 = Color3.new(70/255, 105/255, 0)
Location7.TextColor3 = Color3.new(1, 1, 1)
Location7.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location7.Position = UDim2.new(0, 5, 0, 80)
Location7.Size = UDim2.new(0, 365, 0, 20)
Location7.Font = Enum.Font.Fantasy
Location7.Text = "Teleport to Crystal [100x Fist Strength]"
Location7.TextWrapped = true
Location7.TextSize = 16

LocationFS1B.Name = "LocationFS1B"
LocationFS1B.Parent = WayPointsFrame
LocationFS1B.BackgroundColor3 = Color3.new(70/255, 105/255, 0)
LocationFS1B.TextColor3 = Color3.new(1, 1, 1)
LocationFS1B.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationFS1B.Position = UDim2.new(0, 5, 0, 105)
LocationFS1B.Size = UDim2.new(0, 365, 0, 20)
LocationFS1B.Font = Enum.Font.Fantasy
LocationFS1B.Text = "Teleport to Blue Star [2k x FS]: 1B+ FS required"
LocationFS1B.TextWrapped = true
LocationFS1B.TextSize = 16

LocationFS100B.Name = "LocationFS100B"
LocationFS100B.Parent = WayPointsFrame
LocationFS100B.BackgroundColor3 = Color3.new(70/255, 105/255, 0)
LocationFS100B.TextColor3 = Color3.new(1, 1, 1)
LocationFS100B.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationFS100B.Position = UDim2.new(0, 5, 0, 130)
LocationFS100B.Size = UDim2.new(0, 365, 0, 20)
LocationFS100B.Font = Enum.Font.Fantasy
LocationFS100B.Text = "Teleport to Green Star [40k x FS]: 100B+ FS required"
LocationFS100B.TextWrapped = true
LocationFS100B.TextSize = 16

LocationFS10T.Name = "LocationFS10T"
LocationFS10T.Parent = WayPointsFrame
LocationFS10T.BackgroundColor3 = Color3.new(70/255, 105/255, 0)
LocationFS10T.TextColor3 = Color3.new(1, 1, 1)
LocationFS10T.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationFS10T.Position = UDim2.new(0, 5, 0, 155)
LocationFS10T.Size = UDim2.new(0, 365, 0, 20)
LocationFS10T.Font = Enum.Font.Fantasy
LocationFS10T.Text = "Teleport to Orange Star [800k x FS]: 10T+ FS required"
LocationFS10T.TextWrapped = true
LocationFS10T.TextSize = 16

Location3.Name = "Location3"
Location3.Parent = WayPointsFrame
Location3.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
Location3.TextColor3 = Color3.new(1, 1, 1)
Location3.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location3.Position = UDim2.new(0, 5, 0, 180)
Location3.Size = UDim2.new(0, 365, 0, 20)
Location3.Font = Enum.Font.Fantasy
Location3.Text = "Tp to City Port Training 1 [5x BT]: 100+ BT required"
Location3.TextWrapped = true
Location3.TextSize = 16

Location4.Name = "Location4"
Location4.Parent = WayPointsFrame
Location4.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
Location4.TextColor3 = Color3.new(1, 1, 1)
Location4.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location4.Position = UDim2.new(0, 5, 0, 205)
Location4.Size = UDim2.new(0, 365, 0, 20)
Location4.Font = Enum.Font.Fantasy
Location4.Text = "Tp to City Port Training 2 [10x BT]: 10k+ BT required"
Location4.TextWrapped = true
Location4.TextSize = 16

Location5.Name = "Location5"
Location5.Parent = WayPointsFrame
Location5.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
Location5.TextColor3 = Color3.new(1, 1, 1)
Location5.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location5.Position = UDim2.new(0, 5, 0, 230)
Location5.Size = UDim2.new(0, 365, 0, 20)
Location5.Font = Enum.Font.Fantasy
Location5.Text = "Tp to Ice Mountain [20x BT]: 100k+ BT required"
Location5.TextWrapped = true
Location5.TextSize = 16

Location6.Name = "Location6"
Location6.Parent = WayPointsFrame
Location6.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
Location6.TextColor3 = Color3.new(1, 1, 1)
Location6.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location6.Position = UDim2.new(0, 5, 0, 255)
Location6.Size = UDim2.new(0, 365, 0, 20)
Location6.Font = Enum.Font.Fantasy
Location6.Text = "Tp to Tornado [50x BT]: 1M+ BT required"
Location6.TextWrapped = true
Location6.TextSize = 16

Location8.Name = "Location8"
Location8.Parent = WayPointsFrame
Location8.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
Location8.TextColor3 = Color3.new(1, 1, 1)
Location8.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Location8.Position = UDim2.new(0, 5, 0, 280)
Location8.Size = UDim2.new(0, 365, 0, 20)
Location8.Font = Enum.Font.Fantasy
Location8.Text = "Tp to Volcano [100x BT]: 10M+ BT required"
Location8.TextWrapped = true
Location8.TextSize = 16

LocationBT1B.Name = "LocationBT1B"
LocationBT1B.Parent = WayPointsFrame
LocationBT1B.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
LocationBT1B.TextColor3 = Color3.new(1, 1, 1)
LocationBT1B.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationBT1B.Position = UDim2.new(0, 5, 0, 305)
LocationBT1B.Size = UDim2.new(0, 365, 0, 20)
LocationBT1B.Font = Enum.Font.Fantasy
LocationBT1B.Text = "Tp to [2k x BT] Area: 1B+ BT required"
LocationBT1B.TextWrapped = true
LocationBT1B.TextSize = 16

LocationBT100B.Name = "LocationBT100B"
LocationBT100B.Parent = WayPointsFrame
LocationBT100B.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
LocationBT100B.TextColor3 = Color3.new(1, 1, 1)
LocationBT100B.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationBT100B.Position = UDim2.new(0, 5, 0, 330)
LocationBT100B.Size = UDim2.new(0, 365, 0, 20)
LocationBT100B.Font = Enum.Font.Fantasy
LocationBT100B.Text = "Tp to [40k x BT] Area: 100B+ BT required"
LocationBT100B.TextWrapped = true
LocationBT100B.TextSize = 16

LocationBT10T.Name = "LocationBT10T"
LocationBT10T.Parent = WayPointsFrame
LocationBT10T.BackgroundColor3 = Color3.new(66/255, 0, 165/255)
LocationBT10T.TextColor3 = Color3.new(1, 1, 1)
LocationBT10T.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationBT10T.Position = UDim2.new(0, 5, 0, 355)
LocationBT10T.Size = UDim2.new(0, 365, 0, 20)
LocationBT10T.Font = Enum.Font.Fantasy
LocationBT10T.Text = "Tp to [800k x BT] Area: 10T+ BT required"
LocationBT10T.TextWrapped = true
LocationBT10T.TextSize = 16

LocationPP1M.Name = "LocationPP1M"
LocationPP1M.Parent = WayPointsFrame
LocationPP1M.BackgroundColor3 = Color3.new(195/255, 0, 39/255)
LocationPP1M.TextColor3 = Color3.new(1, 1, 1)
LocationPP1M.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationPP1M.Position = UDim2.new(0, 5, 0, 380)
LocationPP1M.Size = UDim2.new(0, 365, 0, 20)
LocationPP1M.Font = Enum.Font.Fantasy
LocationPP1M.Text = "Tp to Psychic Island [100x PP]: 1M+ PP required"
LocationPP1M.TextWrapped = true
LocationPP1M.TextSize = 16

LocationPP1B.Name = "LocationPP1B"
LocationPP1B.Parent = WayPointsFrame
LocationPP1B.BackgroundColor3 = Color3.new(195/255, 0, 39/255)
LocationPP1B.TextColor3 = Color3.new(1, 1, 1)
LocationPP1B.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationPP1B.Position = UDim2.new(0, 5, 0, 405)
LocationPP1B.Size = UDim2.new(0, 365, 0, 20)
LocationPP1B.Font = Enum.Font.Fantasy
LocationPP1B.Text = "Tp to Psychic Island [10k x PP]: 1B+ PP required"
LocationPP1B.TextWrapped = true
LocationPP1B.TextSize = 16

LocationPP1T.Name = "LocationPP1T"
LocationPP1T.Parent = WayPointsFrame
LocationPP1T.BackgroundColor3 = Color3.new(195/255, 0, 39/255)
LocationPP1T.TextColor3 = Color3.new(1, 1, 1)
LocationPP1T.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationPP1T.Position = UDim2.new(0, 5, 0, 430)
LocationPP1T.Size = UDim2.new(0, 365, 0, 20)
LocationPP1T.Font = Enum.Font.Fantasy
LocationPP1T.Text = "Tp to Psychic Island [1M x PP]: 1T+ PP required"
LocationPP1T.TextWrapped = true
LocationPP1T.TextSize = 16

LocationPP1Qa.Name = "LocationPP1Qa"
LocationPP1Qa.Parent = WayPointsFrame
LocationPP1Qa.BackgroundColor3 = Color3.new(195/255, 0, 39/255)
LocationPP1Qa.TextColor3 = Color3.new(1, 1, 1)
LocationPP1Qa.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
LocationPP1Qa.Position = UDim2.new(0, 5, 0, 455)
LocationPP1Qa.Size = UDim2.new(0, 365, 0, 20)
LocationPP1Qa.Font = Enum.Font.Fantasy
LocationPP1Qa.Text = "Tp to Psychic Island [100M x PP]: 1Qa+ PP required"
LocationPP1Qa.TextWrapped = true
LocationPP1Qa.TextSize = 16

FarmAll.Name = "FarmAll"
FarmAll.Parent = FarmExpFrame
FarmAll.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmAll.TextColor3 = Color3.new(1, 1, 1)
FarmAll.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmAll.Position = UDim2.new(0, 5, 0, 5)
FarmAll.Size = UDim2.new(0, 200, 0, 20)
FarmAll.Font = Enum.Font.Fantasy
FarmAll.Text = "Farm All: OFF"
FarmAll.TextWrapped = true
FarmAll.TextSize = 16

FarmFist.Name = "FarmFist"
FarmFist.Parent = FarmExpFrame
FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmFist.TextColor3 = Color3.new(1, 1, 1)
FarmFist.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmFist.Position = UDim2.new(0, 5, 0, 40)
FarmFist.Size = UDim2.new(0, 200, 0, 20)
FarmFist.Font = Enum.Font.Fantasy
FarmFist.Text = "Farm Fist Strength: OFF"
FarmFist.TextWrapped = true
FarmFist.TextSize = 16

FarmBody.Name = "FarmBody"
FarmBody.Parent = FarmExpFrame
FarmBody.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmBody.TextColor3 = Color3.new(1, 1, 1)
FarmBody.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmBody.Position = UDim2.new(0, 5, 0, 65)
FarmBody.Size = UDim2.new(0, 200, 0, 20)
FarmBody.Font = Enum.Font.Fantasy
FarmBody.Text = "Farm Body Toughness: OFF"
FarmBody.TextWrapped = true
FarmBody.TextSize = 16

FarmSpeed.Name = "FarmSpeed"
FarmSpeed.Parent = FarmExpFrame
FarmSpeed.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmSpeed.TextColor3 = Color3.new(1, 1, 1)
FarmSpeed.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmSpeed.Position = UDim2.new(0, 5, 0, 90)
FarmSpeed.Size = UDim2.new(0, 200, 0, 20)
FarmSpeed.Font = Enum.Font.Fantasy
FarmSpeed.Text = "Farm Movement Speed: OFF"
FarmSpeed.TextWrapped = true
FarmSpeed.TextSize = 16

FarmJump.Name = "FarmJump"
FarmJump.Parent = FarmExpFrame
FarmJump.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmJump.TextColor3 = Color3.new(1, 1, 1)
FarmJump.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmJump.Position = UDim2.new(0, 5, 0, 115)
FarmJump.Size = UDim2.new(0, 200, 0, 20)
FarmJump.Font = Enum.Font.Fantasy
FarmJump.Text = "Farm Jump Force: OFF"
FarmJump.TextWrapped = true
FarmJump.TextSize = 16

FarmPsychic.Name = "FarmPsychic"
FarmPsychic.Parent = FarmExpFrame
FarmPsychic.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmPsychic.TextColor3 = Color3.new(1, 1, 1)
FarmPsychic.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
FarmPsychic.Position = UDim2.new(0, 5, 0, 140)
FarmPsychic.Size = UDim2.new(0, 200, 0, 20)
FarmPsychic.Font = Enum.Font.Fantasy
FarmPsychic.Text = "Farm Psychic Power: OFF"
FarmPsychic.TextWrapped = true
FarmPsychic.TextSize = 16

FarmBodyLabel.Name = "FarmBodyLabel"
FarmBodyLabel.Parent = FarmExpFrame
FarmBodyLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmBodyLabel.TextColor3 = Color3.new(1, 1, 1)
FarmBodyLabel.BorderColor3 = Color3.new(0.1, 0.1, 0.1)
FarmBodyLabel.Position = UDim2.new(0, 213, 0, 65)
FarmBodyLabel.Size = UDim2.new(0, 200, 0, 100)
FarmBodyLabel.Font = Enum.Font.Fantasy
FarmBodyLabel.Text = "Look at teleports and go to the best place you can go for your Body Toughness. You need 10Mil to go in the volcano but you need at least 50Mil before you can afk in there."
FarmBodyLabel.TextSize = 16
FarmBodyLabel.TextWrapped = true
FarmBodyLabel.Visible = false

FarmSpeedLabel.Name = "FarmSpeedLabel"
FarmSpeedLabel.Parent = FarmExpFrame
FarmSpeedLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
FarmSpeedLabel.TextColor3 = Color3.new(1, 1, 1)
FarmSpeedLabel.BorderColor3 = Color3.new(0.1, 0.1, 0.1)
FarmSpeedLabel.Position = UDim2.new(0, 213, 0, 65)
FarmSpeedLabel.Size = UDim2.new(0, 200, 0, 100)
FarmSpeedLabel.Font = Enum.Font.Fantasy
FarmSpeedLabel.Text = "Press 4 and equip the 100TON weight to get the maximum boost. If you still want to move around select the heaviest weight you can move around with but you wont get as much."
FarmSpeedLabel.TextSize = 16
FarmSpeedLabel.TextWrapped = true
FarmSpeedLabel.Visible = false

DeathReturn.Name = "DeathReturn"
DeathReturn.Parent = MainFrame
DeathReturn.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
DeathReturn.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
DeathReturn.Position = UDim2.new(0, 210, 0, 5)
DeathReturn.Size = UDim2.new(0, 160, 0, 20)
DeathReturn.Font = Enum.Font.Fantasy
DeathReturn.TextColor3 = Color3.new(1, 1, 1)
DeathReturn.Text = "OnDeath Return: OFF"
DeathReturn.TextSize = 17
DeathReturn.TextWrapped = true

esptrack.Name = "esptrack"
esptrack.Parent = MainFrame
esptrack.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
esptrack.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
esptrack.Position = UDim2.new(0, 375, 0, 5)
esptrack.Size = UDim2.new(0, 35, 0, 20)
esptrack.TextColor3 = Color3.new(1, 1, 1)
esptrack.Font = Enum.Font.Fantasy
esptrack.Text = "ESP"
esptrack.TextSize = 16
esptrack.TextWrapped = true

PlayerInfo.Name = "PlayerInfo"
PlayerInfo.Parent = MainFrame
PlayerInfo.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
PlayerInfo.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
PlayerInfo.Position = UDim2.new(0, 415, 0, 5)
PlayerInfo.Size = UDim2.new(0, 85, 0, 20)
PlayerInfo.TextColor3 = Color3.new(1, 1, 1)
PlayerInfo.Font = Enum.Font.Fantasy
PlayerInfo.Text = "Top Players"
PlayerInfo.TextSize = 16
PlayerInfo.TextWrapped = true

PlayerInfoFrame.Name = "PlayerInfoFrame"
PlayerInfoFrame.Parent = MainFrame
PlayerInfoFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
PlayerInfoFrame.BorderColor3 = Color3.new(0, 0, 0)
PlayerInfoFrame.BackgroundTransparency = 0.2
PlayerInfoFrame.Position = UDim2.new(0, 377.5, 0, 33)
PlayerInfoFrame.Size = UDim2.new(0, 160, 0, 225)
PlayerInfoFrame.Visible = false

ShowTopPlayers.Name = "ShowTopPlayers"
ShowTopPlayers.Parent = PlayerInfoFrame
ShowTopPlayers.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowTopPlayers.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
ShowTopPlayers.Position = UDim2.new(0, 5, 0, 5)
ShowTopPlayers.Size = UDim2.new(0, 150, 0, 20)
ShowTopPlayers.TextColor3 = Color3.new(1, 1, 1)
ShowTopPlayers.Font = Enum.Font.Fantasy
ShowTopPlayers.Text = "Top Players in Server"
ShowTopPlayers.TextSize = 16
ShowTopPlayers.TextWrapped = true

PlayerInfoStatsFrame.Name = "PlayerInfoStatsFrame"
PlayerInfoStatsFrame.Parent = MainGUI
PlayerInfoStatsFrame.BackgroundColor3 = Color3.new(0, 0, 0)
PlayerInfoStatsFrame.BorderColor3 = Color3.new(0.5, 0.5, 0.5)
PlayerInfoStatsFrame.BackgroundTransparency = 0
PlayerInfoStatsFrame.Position = UDim2.new(0.5, -427.5, 1, -120)
PlayerInfoStatsFrame.Size = UDim2.new(0, 855, 0, 115)
PlayerInfoStatsFrame.Active = true
PlayerInfoStatsFrame.Selectable = true
PlayerInfoStatsFrame.Draggable = true
PlayerInfoStatsFrame.ZIndex = 8
PlayerInfoStatsFrame.Visible = false

PlayerInfoStatsClose.Name = "PlayerInfoStatsClose"
PlayerInfoStatsClose.Parent = PlayerInfoStatsFrame
PlayerInfoStatsClose.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
PlayerInfoStatsClose.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
PlayerInfoStatsClose.Position = UDim2.new(0, 5, 0, 5)
PlayerInfoStatsClose.Size = UDim2.new(0, 20, 0, 20)
PlayerInfoStatsClose.Font = Enum.Font.Fantasy
PlayerInfoStatsClose.Text = "X"
PlayerInfoStatsClose.TextColor3 = Color3.new(1, 0, 0)
PlayerInfoStatsClose.TextSize = 17
PlayerInfoStatsClose.ZIndex = 8
PlayerInfoStatsClose.TextScaled = true
PlayerInfoStatsClose.TextWrapped = true

StatBestFistText1.Name = "StatBestFistText1"
StatBestFistText1.Parent = PlayerInfoStatsFrame
StatBestFistText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
StatBestFistText1.BackgroundTransparency = 1
StatBestFistText1.Position = UDim2.new(0, 30, 0, 2)
StatBestFistText1.Size = UDim2.new(0, 160, 0, 20)
StatBestFistText1.TextColor3 = Color3.new(1, 1, 1)
StatBestFistText1.Font = Enum.Font.Fantasy
StatBestFistText1.Text = "Top Fist Player"
StatBestFistText1.ZIndex = 8
StatBestFistText1.TextSize = 13

ShowStatsFist1.Name = "ShowStatsFist1"
ShowStatsFist1.Parent = PlayerInfoStatsFrame
ShowStatsFist1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsFist1.BackgroundTransparency = 1
ShowStatsFist1.Position = UDim2.new(0, 30, 0, 22)
ShowStatsFist1.Size = UDim2.new(0, 50, 0, 90)
ShowStatsFist1.Font = Enum.Font.Fantasy
ShowStatsFist1.TextColor3 = Color3.new(1, 1, 0)
ShowStatsFist1.Text = "Health:\nFist:\nBody:\nSpeed:\nJump:\nPsychic:"
ShowStatsFist1.TextSize = 14
ShowStatsFist1.ZIndex = 8
ShowStatsFist1.TextXAlignment = Enum.TextXAlignment.Right

ShowStatsFist2.Name = "ShowStatsFist2"
ShowStatsFist2.Parent = PlayerInfoStatsFrame
ShowStatsFist2.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsFist2.BackgroundTransparency = 1
ShowStatsFist2.Position = UDim2.new(0, 85, 0, 22)
ShowStatsFist2.Size = UDim2.new(0, 105, 0, 90)
ShowStatsFist2.Font = Enum.Font.Fantasy
ShowStatsFist2.TextColor3 = Color3.new(1, 1, 1)
ShowStatsFist2.Text = "Stats"
ShowStatsFist2.TextSize = 14
ShowStatsFist2.ZIndex = 8
ShowStatsFist2.TextXAlignment = Enum.TextXAlignment.Right

StatBestBodyText1.Name = "StatBestBodyText1"
StatBestBodyText1.Parent = PlayerInfoStatsFrame
StatBestBodyText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
StatBestBodyText1.BackgroundTransparency = 1
StatBestBodyText1.Position = UDim2.new(0, 195, 0, 2)
StatBestBodyText1.Size = UDim2.new(0, 160, 0, 20)
StatBestBodyText1.TextColor3 = Color3.new(1, 1, 1)
StatBestBodyText1.Font = Enum.Font.Fantasy
StatBestBodyText1.Text = "Top Body Player"
StatBestBodyText1.ZIndex = 8
StatBestBodyText1.TextSize = 13

ShowStatsBody1.Name = "ShowStatsBody1"
ShowStatsBody1.Parent = PlayerInfoStatsFrame
ShowStatsBody1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsBody1.BackgroundTransparency = 1
ShowStatsBody1.Position = UDim2.new(0, 195, 0, 22)
ShowStatsBody1.Size = UDim2.new(0, 50, 0, 90)
ShowStatsBody1.Font = Enum.Font.Fantasy
ShowStatsBody1.TextColor3 = Color3.new(1, 1, 0)
ShowStatsBody1.Text = "Health:\nFist:\nBody:\nSpeed:\nJump:\nPsychic:"
ShowStatsBody1.TextSize = 14
ShowStatsBody1.ZIndex = 8
ShowStatsBody1.TextXAlignment = Enum.TextXAlignment.Right

ShowStatsBody2.Name = "ShowStatsBody2"
ShowStatsBody2.Parent = PlayerInfoStatsFrame
ShowStatsBody2.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsBody2.BackgroundTransparency = 1
ShowStatsBody2.Position = UDim2.new(0, 250, 0, 22)
ShowStatsBody2.Size = UDim2.new(0, 105, 0, 90)
ShowStatsBody2.Font = Enum.Font.Fantasy
ShowStatsBody2.TextColor3 = Color3.new(1, 1, 1)
ShowStatsBody2.Text = "Stats"
ShowStatsBody2.TextSize = 14
ShowStatsBody2.ZIndex = 8
ShowStatsBody2.TextXAlignment = Enum.TextXAlignment.Right

StatBestSpeedText1.Name = "StatBestSpeedText1"
StatBestSpeedText1.Parent = PlayerInfoStatsFrame
StatBestSpeedText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
StatBestSpeedText1.BackgroundTransparency = 1
StatBestSpeedText1.Position = UDim2.new(0, 360, 0, 2)
StatBestSpeedText1.Size = UDim2.new(0, 160, 0, 20)
StatBestSpeedText1.TextColor3 = Color3.new(1, 1, 1)
StatBestSpeedText1.Font = Enum.Font.Fantasy
StatBestSpeedText1.Text = "Top Speed Player"
StatBestSpeedText1.ZIndex = 8
StatBestSpeedText1.TextSize = 13

ShowStatsSpeed1.Name = "ShowStatsSpeed1"
ShowStatsSpeed1.Parent = PlayerInfoStatsFrame
ShowStatsSpeed1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsSpeed1.BackgroundTransparency = 1
ShowStatsSpeed1.Position = UDim2.new(0, 360, 0, 22)
ShowStatsSpeed1.Size = UDim2.new(0, 50, 0, 90)
ShowStatsSpeed1.Font = Enum.Font.Fantasy
ShowStatsSpeed1.TextColor3 = Color3.new(1, 1, 0)
ShowStatsSpeed1.Text = "Health:\nFist:\nBody:\nSpeed:\nJump:\nPsychic:"
ShowStatsSpeed1.TextSize = 14
ShowStatsSpeed1.ZIndex = 8
ShowStatsSpeed1.TextXAlignment = Enum.TextXAlignment.Right

ShowStatsSpeed2.Name = "ShowStatsSpeed2"
ShowStatsSpeed2.Parent = PlayerInfoStatsFrame
ShowStatsSpeed2.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsSpeed2.BackgroundTransparency = 1
ShowStatsSpeed2.Position = UDim2.new(0, 415, 0, 22)
ShowStatsSpeed2.Size = UDim2.new(0, 105, 0, 90)
ShowStatsSpeed2.Font = Enum.Font.Fantasy
ShowStatsSpeed2.TextColor3 = Color3.new(1, 1, 1)
ShowStatsSpeed2.Text = "Stats"
ShowStatsSpeed2.TextSize = 14
ShowStatsSpeed2.ZIndex = 8
ShowStatsSpeed2.TextXAlignment = Enum.TextXAlignment.Right

StatBestJumpText1.Name = "StatBestJumpText1"
StatBestJumpText1.Parent = PlayerInfoStatsFrame
StatBestJumpText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
StatBestJumpText1.BackgroundTransparency = 1
StatBestJumpText1.Position = UDim2.new(0, 525, 0, 2)
StatBestJumpText1.Size = UDim2.new(0, 160, 0, 20)
StatBestJumpText1.TextColor3 = Color3.new(1, 1, 1)
StatBestJumpText1.Font = Enum.Font.Fantasy
StatBestJumpText1.Text = "Top Jump Player"
StatBestJumpText1.ZIndex = 8
StatBestJumpText1.TextSize = 13

ShowStatsJump1.Name = "ShowStatsJump1"
ShowStatsJump1.Parent = PlayerInfoStatsFrame
ShowStatsJump1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsJump1.BackgroundTransparency = 1
ShowStatsJump1.Position = UDim2.new(0, 525, 0, 22)
ShowStatsJump1.Size = UDim2.new(0, 50, 0, 90)
ShowStatsJump1.Font = Enum.Font.Fantasy
ShowStatsJump1.TextColor3 = Color3.new(1, 1, 0)
ShowStatsJump1.Text = "Health:\nFist:\nBody:\nSpeed:\nJump:\nPsychic:"
ShowStatsJump1.TextSize = 14
ShowStatsJump1.ZIndex = 8
ShowStatsJump1.TextXAlignment = Enum.TextXAlignment.Right

ShowStatsJump2.Name = "ShowStatsJump2"
ShowStatsJump2.Parent = PlayerInfoStatsFrame
ShowStatsJump2.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsJump2.BackgroundTransparency = 1
ShowStatsJump2.Position = UDim2.new(0, 580, 0, 22)
ShowStatsJump2.Size = UDim2.new(0, 105, 0, 90)
ShowStatsJump2.Font = Enum.Font.Fantasy
ShowStatsJump2.TextColor3 = Color3.new(1, 1, 1)
ShowStatsJump2.Text = "Stats"
ShowStatsJump2.TextSize = 14
ShowStatsJump2.ZIndex = 8
ShowStatsJump2.TextXAlignment = Enum.TextXAlignment.Right

StatBestPsychicText1.Name = "StatBestPsychicText1"
StatBestPsychicText1.Parent = PlayerInfoStatsFrame
StatBestPsychicText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
StatBestPsychicText1.BackgroundTransparency = 1
StatBestPsychicText1.Position = UDim2.new(0, 690, 0, 2)
StatBestPsychicText1.Size = UDim2.new(0, 160, 0, 20)
StatBestPsychicText1.TextColor3 = Color3.new(1, 1, 1)
StatBestPsychicText1.Font = Enum.Font.Fantasy
StatBestPsychicText1.Text = "Top Psychic Player"
StatBestPsychicText1.ZIndex = 8
StatBestPsychicText1.TextSize = 13

ShowStatsPsychic1.Name = "ShowStatsPsychic1"
ShowStatsPsychic1.Parent = PlayerInfoStatsFrame
ShowStatsPsychic1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsPsychic1.BackgroundTransparency = 1
ShowStatsPsychic1.Position = UDim2.new(0, 690, 0, 22)
ShowStatsPsychic1.Size = UDim2.new(0, 50, 0, 90)
ShowStatsPsychic1.Font = Enum.Font.Fantasy
ShowStatsPsychic1.TextColor3 = Color3.new(1, 1, 0)
ShowStatsPsychic1.Text = "Health:\nFist:\nBody:\nSpeed:\nJump:\nPsychic:"
ShowStatsPsychic1.TextSize = 14
ShowStatsPsychic1.ZIndex = 8
ShowStatsPsychic1.TextXAlignment = Enum.TextXAlignment.Right

ShowStatsPsychic2.Name = "ShowStatsPsychic2"
ShowStatsPsychic2.Parent = PlayerInfoStatsFrame
ShowStatsPsychic2.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStatsPsychic2.BackgroundTransparency = 1
ShowStatsPsychic2.Position = UDim2.new(0, 745, 0, 22)
ShowStatsPsychic2.Size = UDim2.new(0, 105, 0, 90)
ShowStatsPsychic2.Font = Enum.Font.Fantasy
ShowStatsPsychic2.TextColor3 = Color3.new(1, 1, 1)
ShowStatsPsychic2.Text = "Stats"
ShowStatsPsychic2.TextSize = 14
ShowStatsPsychic2.ZIndex = 8
ShowStatsPsychic2.TextXAlignment = Enum.TextXAlignment.Right

Extras.Name = "Extras"
Extras.Parent = MainFrame
Extras.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
Extras.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
Extras.Position = UDim2.new(0, 505, 0, 5)
Extras.Size = UDim2.new(0, 50, 0, 20)
Extras.TextColor3 = Color3.new(1, 1, 1)
Extras.Font = Enum.Font.Fantasy
Extras.Text = "Extras"
Extras.TextSize = 16
Extras.TextWrapped = true

ExtrasFrame.Name = "ExtrasFrame"
ExtrasFrame.Parent = MainFrame
ExtrasFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ExtrasFrame.BorderColor3 = Color3.new(0, 0, 0)
ExtrasFrame.BackgroundTransparency = 0.2
ExtrasFrame.Position = UDim2.new(0, 435, 0, 33)
ExtrasFrame.Size = UDim2.new(0, 160, 0, 213)
ExtrasFrame.Visible = false

AnnoyName.Name = "AnnoyName"
AnnoyName.Parent = ExtrasFrame
AnnoyName.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
AnnoyName.BorderColor3 = Color3.new(0.8, 0.8, 0.8)
AnnoyName.Position = UDim2.new(0, 5, 0, 5)
AnnoyName.Size = UDim2.new(0, 150, 0, 20)
AnnoyName.TextColor3 = Color3.new(1, 1, 1)
AnnoyName.Font = Enum.Font.Fantasy
AnnoyName.Text = tostring(MyPlr.Name)
AnnoyName.TextSize = 14
AnnoyName.TextScaled = false
AnnoyName.TextWrapped = true

TptoPlayer.Name = "TptoPlayer"
TptoPlayer.Parent = ExtrasFrame
TptoPlayer.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
TptoPlayer.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
TptoPlayer.Position = UDim2.new(0, 5, 0, 26)
TptoPlayer.Size = UDim2.new(0, 150, 0, 20)
TptoPlayer.TextColor3 = Color3.new(1, 1, 1)
TptoPlayer.Font = Enum.Font.Fantasy
TptoPlayer.Text = "TP to Player"
TptoPlayer.TextSize = 16
TptoPlayer.TextWrapped = true

AnnoyStart.Name = "AnnoyStart"
AnnoyStart.Parent = ExtrasFrame
AnnoyStart.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
AnnoyStart.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
AnnoyStart.Position = UDim2.new(0, 5, 0, 47)
AnnoyStart.Size = UDim2.new(0, 150, 0, 20)
AnnoyStart.TextColor3 = Color3.new(1, 1, 1)
AnnoyStart.Font = Enum.Font.Fantasy
AnnoyStart.Text = "TP Spam Player: OFF"
AnnoyStart.TextSize = 16
AnnoyStart.TextWrapped = true

PanicToggleLabel.Name = "PanicToggleLabel"
PanicToggleLabel.Parent = ExtrasFrame
PanicToggleLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
PanicToggleLabel.BorderSizePixel = 0
PanicToggleLabel.Position = UDim2.new(0, 5, 0, 77)
PanicToggleLabel.Size = UDim2.new(0, 125, 0, 20)
PanicToggleLabel.TextColor3 = Color3.new(1, 1, 1)
PanicToggleLabel.Font = Enum.Font.Fantasy
PanicToggleLabel.Text = "Panic KeyBind"
PanicToggleLabel.TextSize = 16
PanicToggleLabel.TextWrapped = true

PanicToggle.Name = "PanicToggle"
PanicToggle.Parent = ExtrasFrame
PanicToggle.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
PanicToggle.BorderColor3 = Color3.new(0.8, 0.8, 0.8)
PanicToggle.Position = UDim2.new(0, 130, 0, 78)
PanicToggle.Size = UDim2.new(0, 25, 0, 18)
PanicToggle.TextColor3 = Color3.new(1, 1, 1)
PanicToggle.Font = Enum.Font.Fantasy
PanicToggle.Text = "y"
PanicToggle.TextSize = 16
PanicToggle.TextWrapped = true

NoClip.Name = "NoClip"
NoClip.Parent = ExtrasFrame
NoClip.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
NoClip.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
NoClip.Position = UDim2.new(0, 5, 0, 107)
NoClip.Size = UDim2.new(0, 150, 0, 20)
NoClip.Font = Enum.Font.Fantasy
NoClip.TextColor3 = Color3.new(1, 1, 1)
NoClip.Text = "NoClip Mode: OFF"
NoClip.TextSize = 16
NoClip.TextWrapped = true

farmbtsafetylabel.Name = "farmbtsafetylabel"
farmbtsafetylabel.Parent = ExtrasFrame
farmbtsafetylabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
farmbtsafetylabel.BorderSizePixel = 0
farmbtsafetylabel.Position = UDim2.new(0, 5, 0, 137)
farmbtsafetylabel.Size = UDim2.new(0, 120, 0, 20)
farmbtsafetylabel.TextColor3 = Color3.new(1, 1, 1)
farmbtsafetylabel.Font = Enum.Font.Fantasy
farmbtsafetylabel.Text = "Health [percent]"
farmbtsafetylabel.TextSize = 16
farmbtsafetylabel.TextWrapped = true

farmbtsafetylevel.Name = "farmbtsafetylevel"
farmbtsafetylevel.Parent = ExtrasFrame
farmbtsafetylevel.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
farmbtsafetylevel.BorderColor3 = Color3.new(0.8, 0.8, 0.8)
farmbtsafetylevel.Position = UDim2.new(0, 125, 0, 138)
farmbtsafetylevel.Size = UDim2.new(0, 30, 0, 19)
farmbtsafetylevel.TextColor3 = Color3.new(1, 1, 1)
farmbtsafetylevel.Font = Enum.Font.Fantasy
farmbtsafetylevel.Text = "50"
farmbtsafetylevel.TextSize = 16
farmbtsafetylevel.TextWrapped = true

farmbtsafety.Name = "farmbtsafety"
farmbtsafety.Parent = ExtrasFrame
farmbtsafety.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
farmbtsafety.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
farmbtsafety.Position = UDim2.new(0, 5, 0, 158)
farmbtsafety.Size = UDim2.new(0, 150, 0, 20)
farmbtsafety.Font = Enum.Font.Fantasy
farmbtsafety.TextColor3 = Color3.new(1, 1, 1)
farmbtsafety.Text = "Safety Net: OFF"
farmbtsafety.TextSize = 16
farmbtsafety.TextWrapped = true

farmbtsafetyText1.Name = "farmbtsafetyText1"
farmbtsafetyText1.Parent = ExtrasFrame
farmbtsafetyText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
farmbtsafetyText1.TextColor3 = Color3.new(1, 1, 1)
farmbtsafetyText1.BorderColor3 = Color3.new(0.1, 0.1, 0.1)
farmbtsafetyText1.Position = UDim2.new(0, -155, 0, 141)
farmbtsafetyText1.Size = UDim2.new(0, 150, 0, 115)
farmbtsafetyText1.Font = Enum.Font.Fantasy
farmbtsafetyText1.Text = "Enable this to tp you to the safe zone when your health goes below the preset figure. Only useful if you can take more than 1 hit from your attacker."
farmbtsafetyText1.TextSize = 16
farmbtsafetyText1.TextWrapped = true
farmbtsafetyText1.Visible = false

ReJoinServer.Name = "ReJoinServer"
ReJoinServer.Parent = ExtrasFrame
ReJoinServer.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ReJoinServer.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
ReJoinServer.Position = UDim2.new(0, 5, 0, 188)
ReJoinServer.Size = UDim2.new(0, 150, 0, 20)
ReJoinServer.TextColor3 = Color3.new(1, 1, 1)
ReJoinServer.Font = Enum.Font.Fantasy
ReJoinServer.Text = "ReJoin Server"
ReJoinServer.TextSize = 16
ReJoinServer.TextWrapped = true

InfoScreen.Name = "InfoScreen"
InfoScreen.Parent = MainFrame
InfoScreen.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
InfoScreen.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
InfoScreen.Position = UDim2.new(0, 560, 0, 5)
InfoScreen.Size = UDim2.new(0, 40, 0, 20)
InfoScreen.BackgroundTransparency = 0
InfoScreen.Font = Enum.Font.Fantasy
InfoScreen.TextColor3 = Color3.new(1, 1, 1)
InfoScreen.Text = "Info"
InfoScreen.TextSize = 17
InfoScreen.TextWrapped = true

InfoText1.Name = "InfoText1"
InfoText1.Parent = MainFrame
InfoText1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
InfoText1.BorderColor3 = Color3.new(0, 0, 0)
InfoText1.BackgroundTransparency = 0
InfoText1.Position = UDim2.new(0, 405, 0, 32)
InfoText1.Size = UDim2.new(0, 190, 0, 180)
InfoText1.TextColor3 = Color3.new(1, 1, 1)
InfoText1.Font = Enum.Font.Fantasy
InfoText1.Text = "This Gui was created by LuckyMMB@V3rmillion.net\nDiscord https://discord.gg/GKzJnUC\n\nCredits:\n-racist dolphin for the original ESP script which I edited and customised and whoever found the remotes for farming exp."
InfoText1.TextSize = 15
InfoText1.TextWrapped = true
InfoText1.Visible = false
InfoText1.ZIndex = 7
InfoText1.TextYAlignment = Enum.TextYAlignment.Top

PlayerName.Name = "PlayerName"
PlayerName.Parent = MainFrame
PlayerName.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
PlayerName.BorderColor3 = Color3.new(0.6, 0.6, 0.6)
PlayerName.Position = UDim2.new(0, 605, 0, 5)
PlayerName.Size = UDim2.new(0, 150, 0, 20)
PlayerName.Font = Enum.Font.Fantasy
PlayerName.TextColor3 = Color3.new(1, 1, 1)
PlayerName.Text = tostring(MyPlr.Name)
PlayerName.TextSize = 15
PlayerName.TextScaled = true
PlayerName.TextWrapped = false

StatsFrame.Name = "StatsFrame"
StatsFrame.Parent = MainFrame
StatsFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
StatsFrame.BorderColor3 = Color3.new(0.1, 0.1, 0.1)
StatsFrame.BackgroundTransparency = 0
StatsFrame.Position = UDim2.new(0, 600, 0, 33)
StatsFrame.Size = UDim2.new(0, 161, 0, 90)
StatsFrame.Visible = false

ShowStats1.Name = "ShowStats1"
ShowStats1.Parent = StatsFrame
ShowStats1.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStats1.BackgroundTransparency = 1
ShowStats1.Position = UDim2.new(0, 0, 0, 0)
ShowStats1.Size = UDim2.new(0, 50, 0, 90)
ShowStats1.Font = Enum.Font.Fantasy
ShowStats1.TextColor3 = Color3.new(1, 1, 1)
ShowStats1.Text = " "
ShowStats1.TextSize = 15
ShowStats1.TextXAlignment = Enum.TextXAlignment.Right

ShowStats2.Name = "ShowStats2"
ShowStats2.Parent = StatsFrame
ShowStats2.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ShowStats2.BackgroundTransparency = 1
ShowStats2.Position = UDim2.new(0, 55, 0, 0)
ShowStats2.Size = UDim2.new(0, 103, 0, 90)
ShowStats2.Font = Enum.Font.Fantasy
ShowStats2.TextColor3 = Color3.new(1, 1, 1)
ShowStats2.Text = "Stats"
ShowStats2.TextSize = 15
ShowStats2.TextXAlignment = Enum.TextXAlignment.Right

-- Close --

Open.MouseButton1Down:connect(function()
	TopFrame.Visible = false
	MainFrame.Visible = true
end)

Minimize.MouseButton1Down:connect(function()
	TopFrame.Visible = true
	MainFrame.Visible = false
end)

Close.MouseButton1Down:connect(function()
MainGUI:Destroy()
end)

-- Menus --

local Menus = {
	[WayPoints] = WayPointsFrame;
	[FarmExp] = FarmExpFrame;
	[Extras] = ExtrasFrame;
}
for button,frame in pairs(Menus) do
	button.MouseButton1Click:connect(function()
		if frame.Visible then
			frame.Visible = false
			return
		end
		for k,v in pairs(Menus) do
			v.Visible = v == frame
		end
	end)
end

FarmBody.MouseEnter:connect(function()
	FarmBodyLabel.Visible = true
end)

FarmBody.MouseLeave:connect(function()
	FarmBodyLabel.Visible = false
end)

FarmSpeed.MouseEnter:connect(function()
	FarmSpeedLabel.Visible = true
end)

FarmSpeed.MouseLeave:connect(function()
	FarmSpeedLabel.Visible = false
end)

FarmJump.MouseEnter:connect(function()
	FarmSpeedLabel.Visible = true
end)

FarmJump.MouseLeave:connect(function()
	FarmSpeedLabel.Visible = false
end)

farmbtsafety.MouseEnter:connect(function()
	farmbtsafetyText1.Visible = true
end)

farmbtsafety.MouseLeave:connect(function()
	farmbtsafetyText1.Visible = false
end)

InfoScreen.MouseEnter:connect(function()
	InfoText1.Visible = true
end)

InfoScreen.MouseLeave:connect(function()
	InfoText1.Visible = false
end)

c.MouseButton1Down:connect(function()
	cf.Visible = false
end)

-- Round Number to decimal places and convert to letter value --

function round(num, numDecimalPlaces)
	local mult = 10^(numDecimalPlaces or 0)
	return math.floor(num * mult + 0.5) / mult
end
		
function converttoletter(num)
	if num / 1e21 >=1 then
		newnum = num / 1e21
		return round(newnum, 6).. "Sx"
	elseif num / 1e18 >=1 then
		newnum = num / 1e18
		return round(newnum, 6).. "Qi"
	elseif num / 1e15 >=1 then
		newnum = num / 1e15
		return round(newnum, 6).. "Qa"
	elseif num / 1e12 >=1 then
		newnum = num / 1e12
		return round(newnum, 6).. "T"
	elseif num / 1e09 >=1 then
		newnum = num / 1e09
		return round(newnum, 6).. "B"
	elseif num / 1e06 >=1 then
		newnum = num / 1e06
		return round(newnum, 6).. "M"
	elseif num / 1e03 >=1 then
		newnum = num / 1e03
		return round(newnum, 6).. "K"
	else return num
	end
end

--- NoClip ---

NoClip.MouseButton1Down:connect(function()
	noclip = not noclip
	if noclip then
		NoClip.Text = "NoClip Mode: ON"
		NoClip.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		NoClip.Text = "NoClip Mode: OFF"
		NoClip.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)
game:GetService('RunService').Stepped:connect(function()
	if noclip then
		game.Players.LocalPlayer.Character.Humanoid:ChangeState(11)
	end
end)

--- Farm BT Safety ---

farmbtsafety.MouseButton1Down:connect(function()
	farmbtsafetyactive = not farmbtsafetyactive
	if farmbtsafetyactive then
		farmbtsafety.Text = "Safety Net: ON"
		farmbtsafety.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmbtsafety.Text = "Safety Net: OFF"
		farmbtsafety.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

spawn(function()
	while true do
		if farmbtsafetyactive then
			while farmbtsafetyactive do
				local FindHum = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
				local currenthealth = tonumber(string.format("%.0f", FindHum.Health))
				local minhealth = tonumber(string.format("%.0f", FindHum.MaxHealth))*tonumber(farmbtsafetylevel.Text)/100
				-- print("Current Health: " ..tostring(currenthealth).. ". Min Health: " ..tostring(minhealth))
				if currenthealth <= minhealth then
					game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(459, 248, 887)
				end
			wait(0.2)
			end
		end
		wait(0.5)
	end
end)

-- Show Location --

local curlocation = coroutine.wrap(function()
	while true do
		LocationX = round(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.x, 0)
		LocationY = round(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.y, 0)
		LocationZ = round(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.z, 0)
		ShowLocation.Text = "Coords: "..LocationX..", "..LocationY..", "..LocationZ
		wait(0.5)
	end
end)

curlocation()

-- Set Locations --

SetLocation.MouseButton1Down:connect(function()
	function round(num, numDecimalPlaces)
		local mult = 10^(numDecimalPlaces or 0)
		return math.floor(num * mult + 0.5) / mult
	end
	setlocationx = round(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.x, 0)
	setlocationy = round(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.y, 0)
	setlocationz = round(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.z, 0)
	print("Set Custom Location: "..setlocationx..", "..setlocationy..", "..setlocationz)
    SetLocation.Text = setlocationx..","..setlocationy..","..setlocationz
	CustomLocationSet = true
end)

--- TP to custom location ---

TPLocation.MouseButton1Down:connect(function()
	if CustomLocationSet == true then
		workspace:WaitForChild(game.Players.LocalPlayer.Name).HumanoidRootPart.CFrame = CFrame.new(setlocationx, setlocationy, setlocationz)
		WayPointsFrame.Visible = false
	end
end)

Location1.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(459, 248, 887)
	WayPointsFrame.Visible = false
end)
	
Location2.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(409, 271, 978)
	WayPointsFrame.Visible = false
end)

LocationFS1B.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1176, 4789, -2293)
	WayPointsFrame.Visible = false
end)

LocationFS10T.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-369, 15735, -9)
	WayPointsFrame.Visible = false
end)

LocationFS100B.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1381, 9274, 1647)
	WayPointsFrame.Visible = false
end)

Location7.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2279, 1944, 1053)
	WayPointsFrame.Visible = false
end)

Location3.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(365, 249, -445)
	settplocation = "BT100Area"
	WayPointsFrame.Visible = false
end)

Location4.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(349, 263, -490)
	settplocation = "BT10KArea"
	WayPointsFrame.Visible = false
end)

Location5.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1640, 258, 2244)
	settplocation = "BT100KArea"
	WayPointsFrame.Visible = false
end)

Location6.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2307, 976, 1068)
	settplocation = "BT1MArea"
	WayPointsFrame.Visible = false
end)

Location8.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2024, 714, -1860)
	settplocation = "BT10MArea"
	WayPointsFrame.Visible = false
end)

LocationBT1B.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-254, 286, 980)
	settplocation = "BT1BArea"
	WayPointsFrame.Visible = false
end)

LocationBT100B.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-271, 279, 991)
	settplocation = "BT100BArea"
	WayPointsFrame.Visible = false
end)

LocationBT10T.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-279, 279, 1007)
	settplocation = "BT10TArea"
	WayPointsFrame.Visible = false
end)

LocationPP1M.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2527, 5486, -532)
	settplocation = "PP1MArea"
	WayPointsFrame.Visible = false
end)

LocationPP1B.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2560, 5500, -439)
	settplocation = "PP1BArea"
	WayPointsFrame.Visible = false
end)

LocationPP1T.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2582, 5516, -504)
	settplocation = "PP1TArea"
	WayPointsFrame.Visible = false
end)

LocationPP1Qa.MouseButton1Click:connect(function()
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2544, 5412, -495)
	settplocation = "PP1QaArea"
	WayPointsFrame.Visible = false
end)

-- ESP Stuff --

Run:BindToRenderStep("UpdateESP", Enum.RenderPriority.Character.Value, function()
	for _, v in next, Plrs:GetPlayers() do
		UpdateESP(v)
	end
end)

-- Farm Exp --

FarmAll.MouseButton1Click:Connect(function()
	if farmallactive ~= true then
		farmallactive = true
		farmfistactive = true
		farmbodyactive = true
		farmspeedactive = true
		farmpsychicactive = true
		farmjumpactive = true
		FarmAll.BackgroundColor3 = Color3.new(0, 0.5, 0)
		FarmAll.Text = "Farm All: ON"
		FarmExp.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmallactive = false
		farmfistactive = false
		farmbodyactive = false
		farmspeedactive = false
		farmpsychicactive = false
		farmjumpactive = false
		FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmBody.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmSpeed.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmJump.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmPsychic.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmAll.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmAll.Text = "Farm All: OFF"
		FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

FarmFist.MouseButton1Click:Connect(function()
	if farmfistactive ~= true then
		farmfistactive = true
		FarmFist.BackgroundColor3 = Color3.new(0, 0.5, 0)
		FarmFist.Text = "Farm Fist Strength: ON"
		FarmExp.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmfistactive = false
		FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmFist.Text = "Farm Fist Strength: OFF"
		FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

FarmBody.MouseButton1Click:Connect(function()
	if farmbodyactive ~= true then
		farmbodyactive = true
		FarmBody.BackgroundColor3 = Color3.new(0, 0.5, 0)
		FarmBody.Text = "Farm Body Strength: ON"
		FarmExp.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmbodyactive = false
		FarmBody.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmBody.Text = "Farm Body Strength: OFF"
		FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

FarmSpeed.MouseButton1Click:Connect(function()
	if farmspeedactive ~= true then
		farmspeedactive = true
		FarmSpeed.BackgroundColor3 = Color3.new(0, 0.5, 0)
		FarmSpeed.Text = "Farm Speed Strength: ON"
		FarmExp.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmspeedactive = false
		FarmSpeed.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmSpeed.Text = "Farm Speed Strength: OFF"
		FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

FarmJump.MouseButton1Click:Connect(function()
	if farmjumpactive ~= true then
		farmjumpactive = true
		FarmJump.BackgroundColor3 = Color3.new(0, 0.5, 0)
		FarmJump.Text = "Farm Jump Strength: ON"
		FarmExp.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmjumpactive = false
		FarmJump.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmJump.Text = "Farm Jump Strength: OFF"
		FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

FarmPsychic.MouseButton1Click:Connect(function()
	if farmpsychicactive ~= true then
		farmpsychicactive = true
		FarmPsychic.BackgroundColor3 = Color3.new(0, 0.5, 0)
		FarmPsychic.Text = "Farm Psychic Strength: ON"
		FarmExp.BackgroundColor3 = Color3.new(0, 0.5, 0)
	else
		farmpsychicactive = false
		FarmPsychic.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		FarmPsychic.Text = "Farm Psychic Strength: OFF"
		FarmExp.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	end
end)

spawn(function()
	while true do
		if farmfistactive and game.Players.LocalPlayer.Character:WaitForChild("Humanoid") then
			if tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.FistStrength.Value)) >= 10e12 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-369, 15735, -9)
				fistarguments = {"+FS6"}
				farmpsychicactive = false
				FarmPsychic.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmPsychic.Text = "Farm Psychic Strength: OFF"
			elseif tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.FistStrength.Value)) >= 100e09 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1381, 9274, 1647)
				fistarguments = {"+FS5"}
				farmpsychicactive = false
				FarmPsychic.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmPsychic.Text = "Farm Psychic Strength: OFF"
			elseif tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.FistStrength.Value)) >= 1e09 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1176, 4789, -2293)
				fistarguments = {"+FS4"}
				farmpsychicactive = false
				FarmPsychic.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmPsychic.Text = "Farm Psychic Strength: OFF"
			else
				fistarguments = {"+FS3", "+FS2", "+FS1"}
			end
			while farmfistactive do
				game:GetService('RunService').Stepped:wait()
				for i,v in pairs(fistarguments) do
					game.ReplicatedStorage.RemoteEvent:FireServer({[1] = v})
				end
			end
		end
		wait(1)
	end
end)

spawn(function()
	while true do
		if farmbodyactive and game.Players.LocalPlayer.Character:WaitForChild("Humanoid") then
			while farmbodyactive do
				local bodyarguments = {"+BT5", "+BT4", "+BT3", "+BT2", "+BT1"}
				local event = game.ReplicatedStorage.RemoteEvent
				for i,v in pairs(bodyarguments) do
					event:FireServer({[1] = v})
					wait()
				end
				wait()
			end
		end
		wait(1)
	end
end)

spawn(function()
	while true do
		if farmspeedactive and game.Players.LocalPlayer.Character:WaitForChild("Humanoid") then
			while farmspeedactive do
				local speedarguments = {"+MS5", "+MS4", "+MS3", "+MS2", "+MS1"}
				local event = game.ReplicatedStorage.RemoteEvent
				for i,v in pairs(speedarguments) do
					event:FireServer({[1] = v})
					wait()
				end
				wait()
			end
		end
		wait(1)
	end
end)

spawn(function()
	while true do
		if farmjumpactive and game.Players.LocalPlayer.Character:WaitForChild("Humanoid") then
			while farmjumpactive do
				local jumparguments = {"+JF5", "+JF4", "+JF3", "+JF2", "+JF1"}
				local event = game.ReplicatedStorage.RemoteEvent
				for i,v in pairs(jumparguments) do
					event:FireServer({[1] = v})
					wait()
				end
				wait()
			end
		end
		wait(1)
	end
end)

spawn(function()
	while true do
		if farmpsychicactive and game.Players.LocalPlayer.Character:WaitForChild("Humanoid") then
			if tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.PsychicPower.Value)) >= 1e15 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2544, 5412, -495)
				psychicarguments = {"+PP6"}
				farmfistactive = false
				FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmFist.Text = "Farm Fist Strength: OFF"
			elseif tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.PsychicPower.Value)) >= 1e12 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2582, 5516, -504)
				psychicarguments = {"+PP5"}
				farmfistactive = false
				FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmFist.Text = "Farm Fist Strength: OFF"
			elseif tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.PsychicPower.Value)) >= 1e09 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2560, 5500, -439)
				psychicarguments = {"+PP4"}
				farmfistactive = false
				FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmFist.Text = "Farm Fist Strength: OFF"
			elseif tonumber(string.format("%.0f", game.Players.LocalPlayer.PrivateStats.PsychicPower.Value)) >= 1e06 then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2527, 5486, -532)
				psychicarguments = {"+PP3"}
				farmfistactive = false
				FarmFist.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
				FarmFist.Text = "Farm Fist Strength: OFF"
			else
				psychicarguments = {"+PP2", "+PP1"}
			end
			while farmpsychicactive do
				game:GetService('RunService').Stepped:wait()
				for i,v in pairs(psychicarguments) do
					game.ReplicatedStorage.RemoteEvent:FireServer({[1] = v})
				end
			end
		end
		wait(1)
	end
end)

-- Return to position on Death --

DeathReturn.MouseButton1Click:Connect(function()
	if deathreturnactive ~= true then
		deathreturnactive = true
		DeathReturn.BackgroundColor3 = Color3.new(0, 0.5, 0)
		DeathReturn.Text = "OnDeath Return: ON"
	else
		deathreturnactive = false
		DeathReturn.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		DeathReturn.Text = "OnDeath Return: OFF"
	end
end)

spawn(function()
	while true do
		if deathreturnactive then
			player = game:GetService("Players").LocalPlayer
			player.Character.Humanoid.Died:connect(function()
				playerdied = true
			end)
		end
		if not playerdied then
			lastlocationx = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.x
			lastlocationy = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.y
			lastlocationz = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.z
			SavePosition.Text = "Last Place: " ..lastlocationx.. "," ..lastlocationy.. "," ..lastlocationz
			--print(tostring(SavePosition.Text))
			wait(0.5)
		end
		if playerdied then
			--print("Player " ..tostring(game.Players.LocalPlayer.Name).. " Died")
			--print(tostring(SavePosition.Text))
			wait(5)
			game:GetService("ReplicatedStorage").RemoteEvent:FireServer({[1] = "Respawn"})
			wait(1)
			game.Lighting.Blur.Enabled = false
            game.Players.LocalPlayer.PlayerGui.IntroGui.Enabled = false
            game.Players.LocalPlayer.PlayerGui.ScreenGui.Enabled = true
			wait(4)
			--print("screengui disabled")
			repeat wait(0.1) until game.Players.LocalPlayer.Character.Humanoid
			--print("Teleporting back to " ..tostring(SavePosition.Text))
			if deathreturnactive then
				local FindHum = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(lastlocationx, lastlocationy, lastlocationz)
			end
			playerdied = false
		end
		wait(1)
	end		
end)

-- Annoy Player --

AnnoyStart.MouseButton1Click:Connect(function()
	if annoyplayeractive ~= true then
		annoyplayeractive = true
		AnnoyStart.BackgroundColor3 = Color3.new(0, 0.5, 0)
		AnnoyStart.Text = "TP Spam Player: ON"
	else
		annoyplayeractive = false
		AnnoyStart.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		AnnoyStart.Text = "TP Spam Player: OFF"
	end
end)

spawn(function()
	while true do
		wait(0.5)
		if annoyplayeractive then
			for i,v in pairs(game:GetService("Players"):GetChildren()) do
				if v.Name:lower():find(AnnoyName.Text:lower()) then
					player = game.Players.LocalPlayer.Character
					local startpos = player.HumanoidRootPart.CFrame
					v.Character.Humanoid.Died:connect(function()
						annoyplayeractive = false
						AnnoyStart.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
						AnnoyStart.Text = "TP Spam Player: OFF"
					end)
					player.Humanoid.Died:connect(function()
						annoyplayeractive = false
						AnnoyStart.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
						AnnoyStart.Text = "TP Spam Player: OFF"
					end)
					while annoyplayeractive == true do
						player.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
						wait(.005)
					end
					player.HumanoidRootPart.CFrame = startpos
				end
			end
		end
	end
end)

TptoPlayer.MouseButton1Click:Connect(function()
	for i,v in pairs(game:GetService("Players"):GetChildren()) do
		if v.Name:lower():find(AnnoyName.Text:lower()) then
			if v.Name ~= tostring(MyPlr.Name) then
				player = game.Players.LocalPlayer.Character
				player.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(3, 0, 3)
			end
		end
	end
end)

mouse.KeyDown:connect(function(key)
	if key == tostring(PanicToggle.Text) then
		game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(459, 248, 887)
	end
end)

-- ESP --

esptrack.MouseButton1Click:connect(function()
	ESPEnabled = not ESPEnabled
	if ESPEnabled then
		esptrack.BackgroundColor3 = Color3.new(0, 0.5, 0)
		for _, v in next, Plrs:GetPlayers() do
			if v ~= MyPlr then
				if CharAddedEvent[v.Name] == nil then
					CharAddedEvent[v.Name] = v.CharacterAdded:connect(function(Char)
						if ESPEnabled then
							RemoveESP(v)
							CreateESP(v)
						end
						repeat wait() until Char:FindFirstChild("HumanoidRootPart")
					end)
				end
				RemoveESP(v)
				CreateESP(v)
			end
		end
	else
		esptrack.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
		for _, v in next, Plrs:GetPlayers() do
			RemoveESP(v)
		end
	end
end)

-- Server Player Stats --

PlayerInfo.MouseButton1Click:connect(function()
	if not showtopplayersactive then
		showtopplayersactive = true
		showtopplayersfistactive = true
		showtopplayersbodyactive = true
		showtopplayersspeedactive = true
		showtopplayersjumpactive = true
		showtopplayerspsychicactive = true
		PlayerInfoStatsFrame.Visible = true
	else
		showtopplayersactive = false
		PlayerInfoStatsFrame.Visible = false
		showtopplayersfistactive = false
		showtopplayersbodyactive = false
		showtopplayersspeedactive = false
		showtopplayersjumpactive = false
		showtopplayerspsychicactive = false
	end
end)

PlayerInfoStatsClose.MouseButton1Click:connect(function()
	showtopplayersactive = false
	PlayerInfoStatsFrame.Visible = false
	showtopplayersfistactive = false
	showtopplayersbodyactive = false
	showtopplayersspeedactive = false
	showtopplayersjumpactive = false
	showtopplayerspsychicactive = false
end)

spawn(function()
	while true do
		if showtopplayersfistactive then
			BestPlayerFist = 1
			PlayerFistName = false
			for i,v in pairs(game:GetService("Players"):GetChildren()) do
				local PlayerFist = tonumber(game.Players[v.Name].PrivateStats.FistStrength.Value)
				if PlayerFist > tonumber(BestPlayerFist) then 
					BestPlayerFist = PlayerFist
					PlayerFistName = tostring(v.Name)
				end
			end
			StatBestFistText1.Text = "Fist: " ..tostring(PlayerFistName)
			local fistplrStatus = game.Players[PlayerFistName].leaderstats.Status
			if fistplrStatus.Value == "Criminal" then
				StatBestFistText1.TextColor3 = Color3.new(1, 0.1, 1)
			elseif fistplrStatus.Value == "Lawbreaker" then
				StatBestFistText1.TextColor3 = Color3.new(1, 0.1, 0.1)
			elseif fistplrStatus.Value == "Guardian" then
				StatBestFistText1.TextColor3 = Color3.new(0.1, 0.8, 1)
			elseif fistplrStatus.Value == "Protector" then
				StatBestFistText1.TextColor3 = Color3.new(0.1, 0.1, 1)
			elseif fistplrStatus.Value == "Supervillain" then
				StatBestFistText1.TextColor3 = Color3.new(0.3, 0.1, 0.1)
			elseif fistplrStatus.Value == "Superhero" then
				StatBestFistText1.TextColor3 = Color3.new(0.8, 0.8, 0)
			else
				StatBestFistText1.TextColor3 = Color3.new(1, 1, 1)
			end
			local FindHum = game.Players[PlayerFistName].Character.Humanoid
			local FistPlayerHealth = converttoletter(string.format("%.0f", FindHum.Health))
			local FistPlayerFist = converttoletter(string.format("%.0f", game.Players[PlayerFistName].PrivateStats.FistStrength.Value))
			local FistPlayerBody = converttoletter(string.format("%.0f", game.Players[PlayerFistName].PrivateStats.BodyToughness.Value))
			local FistPlayerSpeed = converttoletter(string.format("%.0f", game.Players[PlayerFistName].PrivateStats.MovementSpeed.Value))
			local FistPlayerJump = converttoletter(string.format("%.0f", game.Players[PlayerFistName].PrivateStats.JumpForce.Value))
			local FistPlayerPsychic = converttoletter(string.format("%.0f", game.Players[PlayerFistName].PrivateStats.PsychicPower.Value))
			ShowStatsFist2.Text = tostring(FistPlayerHealth.. "\n" ..FistPlayerFist.. "\n" ..FistPlayerBody.. "\n" ..FistPlayerSpeed.. "\n" ..FistPlayerJump.. "\n" ..FistPlayerPsychic)
		end
		wait(0.3)
	end
end)

spawn(function()
	while true do
		if showtopplayersbodyactive then
			BestPlayerBody = 1
			PlayerBodyName = false
			for i,v in pairs(game:GetService("Players"):GetChildren()) do
				local PlayerBody = tonumber(game.Players[v.Name].PrivateStats.BodyToughness.Value)
				if PlayerBody > tonumber(BestPlayerBody) then 
					BestPlayerBody = PlayerBody
					PlayerBodyName = tostring(v.Name)
				end
			end
			StatBestBodyText1.Text = "Body: " ..tostring(PlayerBodyName)
			local bodyplrStatus = game.Players[PlayerBodyName].leaderstats.Status
			if bodyplrStatus.Value == "Criminal" then
				StatBestBodyText1.TextColor3 = Color3.new(1, 0.1, 1)
			elseif bodyplrStatus.Value == "Lawbreaker" then
				StatBestBodyText1.TextColor3 = Color3.new(1, 0.1, 0.1)
			elseif bodyplrStatus.Value == "Guardian" then
				StatBestBodyText1.TextColor3 = Color3.new(0.1, 0.8, 1)
			elseif bodyplrStatus.Value == "Protector" then
				StatBestBodyText1.TextColor3 = Color3.new(0.1, 0.1, 1)
			elseif bodyplrStatus.Value == "Supervillain" then
				StatBestBodyText1.TextColor3 = Color3.new(0.3, 0.1, 0.1)
			elseif bodyplrStatus.Value == "Superhero" then
				StatBestBodyText1.TextColor3 = Color3.new(0.8, 0.8, 0)
			else
				StatBestBodyText1.TextColor3 = Color3.new(1, 1, 1)
			end
			local FindHum = game.Players[PlayerBodyName].Character.Humanoid
			local BodyPlayerHealth = converttoletter(string.format("%.0f", FindHum.Health))
			local BodyPlayerFist = converttoletter(string.format("%.0f", game.Players[PlayerBodyName].PrivateStats.FistStrength.Value))
			local BodyPlayerBody = converttoletter(string.format("%.0f", game.Players[PlayerBodyName].PrivateStats.BodyToughness.Value))
			local BodyPlayerSpeed = converttoletter(string.format("%.0f", game.Players[PlayerBodyName].PrivateStats.MovementSpeed.Value))
			local BodyPlayerJump = converttoletter(string.format("%.0f", game.Players[PlayerBodyName].PrivateStats.JumpForce.Value))
			local BodyPlayerPsychic = converttoletter(string.format("%.0f", game.Players[PlayerBodyName].PrivateStats.PsychicPower.Value))
			ShowStatsBody2.Text = tostring(BodyPlayerHealth.. "\n" ..BodyPlayerFist.. "\n" ..BodyPlayerBody.. "\n" ..BodyPlayerSpeed.. "\n" ..BodyPlayerJump.. "\n" ..BodyPlayerPsychic)
		end
		wait(0.3)
	end
end)

spawn(function()
	while true do
		if showtopplayersspeedactive then
			BestPlayerSpeed = 1
			PlayerSpeedName = false
			for i,v in pairs(game:GetService("Players"):GetChildren()) do
				local PlayerSpeed = tonumber(game.Players[v.Name].PrivateStats.MovementSpeed.Value)
				if PlayerSpeed > tonumber(BestPlayerSpeed) then 
					BestPlayerSpeed = PlayerSpeed
					PlayerSpeedName = tostring(v.Name)
				end
			end
			StatBestSpeedText1.Text = "Speed: " ..tostring(PlayerSpeedName)
			local speedplrStatus = game.Players[PlayerSpeedName].leaderstats.Status
			if speedplrStatus.Value == "Criminal" then
				StatBestSpeedText1.TextColor3 = Color3.new(1, 0.1, 1)
			elseif speedplrStatus.Value == "Lawbreaker" then
				StatBestSpeedText1.TextColor3 = Color3.new(1, 0.1, 0.1)
			elseif speedplrStatus.Value == "Guardian" then
				StatBestSpeedText1.TextColor3 = Color3.new(0.1, 0.8, 1)
			elseif speedplrStatus.Value == "Protector" then
				StatBestSpeedText1.TextColor3 = Color3.new(0.1, 0.1, 1)
			elseif speedplrStatus.Value == "Supervillain" then
				StatBestSpeedText1.TextColor3 = Color3.new(0.3, 0.1, 0.1)
			elseif speedplrStatus.Value == "Superhero" then
				StatBestSpeedText1.TextColor3 = Color3.new(0.8, 0.8, 0)
			else
				StatBestSpeedText1.TextColor3 = Color3.new(1, 1, 1)
			end
			local FindHum = game.Players[PlayerSpeedName].Character.Humanoid
			local SpeedPlayerHealth = converttoletter(string.format("%.0f", FindHum.Health))
			local SpeedPlayerFist = converttoletter(string.format("%.0f", game.Players[PlayerSpeedName].PrivateStats.FistStrength.Value))
			local SpeedPlayerBody = converttoletter(string.format("%.0f", game.Players[PlayerSpeedName].PrivateStats.BodyToughness.Value))
			local SpeedPlayerSpeed = converttoletter(string.format("%.0f", game.Players[PlayerSpeedName].PrivateStats.MovementSpeed.Value))
			local SpeedPlayerJump = converttoletter(string.format("%.0f", game.Players[PlayerSpeedName].PrivateStats.JumpForce.Value))
			local SpeedPlayerPsychic = converttoletter(string.format("%.0f", game.Players[PlayerSpeedName].PrivateStats.PsychicPower.Value))
			ShowStatsSpeed2.Text = tostring(SpeedPlayerHealth.. "\n" ..SpeedPlayerFist.. "\n" ..SpeedPlayerBody.. "\n" ..SpeedPlayerSpeed.. "\n" ..SpeedPlayerJump.. "\n" ..SpeedPlayerPsychic)
		end
		wait(0.3)
	end
end)

spawn(function()
	while true do
		if showtopplayersjumpactive then
			BestPlayerJump = 1
			PlayerJumpName = false
			for i,v in pairs(game:GetService("Players"):GetChildren()) do
				local PlayerJump = tonumber(game.Players[v.Name].PrivateStats.JumpForce.Value)
				if PlayerJump > tonumber(BestPlayerJump) then 
					BestPlayerJump = PlayerJump
					PlayerJumpName = tostring(v.Name)
				end
			end
			StatBestJumpText1.Text = "Jump: " ..tostring(PlayerJumpName)
			local JumpplrStatus = game.Players[PlayerJumpName].leaderstats.Status
			if JumpplrStatus.Value == "Criminal" then
				StatBestJumpText1.TextColor3 = Color3.new(1, 0.1, 1)
			elseif JumpplrStatus.Value == "Lawbreaker" then
				StatBestJumpText1.TextColor3 = Color3.new(1, 0.1, 0.1)
			elseif JumpplrStatus.Value == "Guardian" then
				StatBestJumpText1.TextColor3 = Color3.new(0.1, 0.8, 1)
			elseif JumpplrStatus.Value == "Protector" then
				StatBestJumpText1.TextColor3 = Color3.new(0.1, 0.1, 1)
			elseif JumpplrStatus.Value == "Supervillain" then
				StatBestJumpText1.TextColor3 = Color3.new(0.3, 0.1, 0.1)
			elseif JumpplrStatus.Value == "Superhero" then
				StatBestJumpText1.TextColor3 = Color3.new(0.8, 0.8, 0)
			else
				StatBestJumpText1.TextColor3 = Color3.new(1, 1, 1)
			end
			local FindHum = game.Players[PlayerJumpName].Character.Humanoid
			local JumpPlayerHealth = converttoletter(string.format("%.0f", FindHum.Health))
			local JumpPlayerFist = converttoletter(string.format("%.0f", game.Players[PlayerJumpName].PrivateStats.FistStrength.Value))
			local JumpPlayerBody = converttoletter(string.format("%.0f", game.Players[PlayerJumpName].PrivateStats.BodyToughness.Value))
			local JumpPlayerSpeed = converttoletter(string.format("%.0f", game.Players[PlayerJumpName].PrivateStats.MovementSpeed.Value))
			local JumpPlayerJump = converttoletter(string.format("%.0f", game.Players[PlayerJumpName].PrivateStats.JumpForce.Value))
			local JumpPlayerPsychic = converttoletter(string.format("%.0f", game.Players[PlayerJumpName].PrivateStats.PsychicPower.Value))
			ShowStatsJump2.Text = tostring(JumpPlayerHealth.. "\n" ..JumpPlayerFist.. "\n" ..JumpPlayerBody.. "\n" ..JumpPlayerSpeed.. "\n" ..JumpPlayerJump.. "\n" ..JumpPlayerPsychic)
		end
		wait(0.3)
	end
end)

spawn(function()
	while true do
		if showtopplayerspsychicactive then
			BestPlayerPsychic = 1
			PlayerPsychicName = false
			for i,v in pairs(game:GetService("Players"):GetChildren()) do
				local PlayerPsychic = tonumber(game.Players[v.Name].PrivateStats.PsychicPower.Value)
				if PlayerPsychic > tonumber(BestPlayerPsychic) then 
					BestPlayerPsychic = PlayerPsychic
					PlayerPsychicName = tostring(v.Name)
				end
			end
			StatBestPsychicText1.Text = "Psy: " ..tostring(PlayerPsychicName)
			local PsychicplrStatus = game.Players[PlayerPsychicName].leaderstats.Status
			if PsychicplrStatus.Value == "Criminal" then
				StatBestPsychicText1.TextColor3 = Color3.new(1, 0.1, 1)
			elseif PsychicplrStatus.Value == "Lawbreaker" then
				StatBestPsychicText1.TextColor3 = Color3.new(1, 0.1, 0.1)
			elseif PsychicplrStatus.Value == "Guardian" then
				StatBestPsychicText1.TextColor3 = Color3.new(0.1, 0.8, 1)
			elseif PsychicplrStatus.Value == "Protector" then
				StatBestPsychicText1.TextColor3 = Color3.new(0.1, 0.1, 1)
			elseif PsychicplrStatus.Value == "Supervillain" then
				StatBestPsychicText1.TextColor3 = Color3.new(0.3, 0.1, 0.1)
			elseif PsychicplrStatus.Value == "Superhero" then
				StatBestPsychicText1.TextColor3 = Color3.new(0.8, 0.8, 0)
			else
				StatBestPsychicText1.TextColor3 = Color3.new(1, 1, 1)
			end
			local FindHum = game.Players[PlayerPsychicName].Character.Humanoid
			local PsychicPlayerHealth = converttoletter(string.format("%.0f", FindHum.Health))
			local PsychicPlayerFist = converttoletter(string.format("%.0f", game.Players[PlayerPsychicName].PrivateStats.FistStrength.Value))
			local PsychicPlayerBody = converttoletter(string.format("%.0f", game.Players[PlayerPsychicName].PrivateStats.BodyToughness.Value))
			local PsychicPlayerSpeed = converttoletter(string.format("%.0f", game.Players[PlayerPsychicName].PrivateStats.MovementSpeed.Value))
			local PsychicPlayerJump = converttoletter(string.format("%.0f", game.Players[PlayerPsychicName].PrivateStats.JumpForce.Value))
			local PsychicPlayerPsychic = converttoletter(string.format("%.0f", game.Players[PlayerPsychicName].PrivateStats.PsychicPower.Value))
			ShowStatsPsychic2.Text = tostring(PsychicPlayerHealth.. "\n" ..PsychicPlayerFist.. "\n" ..PsychicPlayerBody.. "\n" ..PsychicPlayerSpeed.. "\n" ..PsychicPlayerJump.. "\n" ..PsychicPlayerPsychic)
		end
		wait(0.3)
	end
end)

-- Display Player Info --

spawn(function()
	while true do
		statplayer = tostring(PlayerName.Text)
		StatsFrame.Visible = false
		if playerdied == true then repeat wait(0.5) until playerdied == false end
		for i,v in pairs(game:GetService("Players"):GetChildren()) do
			if string.lower(v.Name) == string.lower(statplayer) then
				StatsFrame.Visible = true
				local FindHum = v.Character:WaitForChild("Humanoid")
				local PlayerHealth = converttoletter(string.format("%.0f", FindHum.Health))
				local PlayerFist = converttoletter(string.format("%.0f", game.Players[v.Name].PrivateStats.FistStrength.Value))
				local PlayerBody = converttoletter(string.format("%.0f", game.Players[v.Name].PrivateStats.BodyToughness.Value))
				local PlayerSpeed = converttoletter(string.format("%.0f", game.Players[v.Name].PrivateStats.MovementSpeed.Value))
				local PlayerJump = converttoletter(string.format("%.0f", game.Players[v.Name].PrivateStats.JumpForce.Value))
				local PlayerPsychic = converttoletter(string.format("%.0f", game.Players[v.Name].PrivateStats.PsychicPower.Value))
				ShowStats1.Text = "Health:\nFist:\nBody:\nSpeed:\nJump:\nPsychic:"
				ShowStats2.Text = PlayerHealth.. "\n" ..PlayerFist.. "\n" ..PlayerBody.. "\n" ..PlayerSpeed.. "\n" ..PlayerJump.. "\n" ..PlayerPsychic
				break
			end
		end
		wait(0.25)
	end
end)

--- ReJoin Server ---

ReJoinServer.MouseButton1Click:connect(function()
	local placeId = 2202352383
	game:GetService("TeleportService"):Teleport(placeId)
end)
--Made By Cookie

local ScreenGui = Instance.new("ScreenGui")
local main = Instance.new("Frame")
local Frame = Instance.new("Frame")
local TextLabel = Instance.new("TextLabel")
local Bh = Instance.new("TextButton")
local CmdX = Instance.new("TextButton")
local HealthRespawn = Instance.new("TextButton")
local AutoAttackOn = Instance.new("TextButton")
local Split = Instance.new("TextButton")
local AutoCon = Instance.new("TextButton")
local AutoCoff = Instance.new("TextButton")
local RemoveFF = Instance.new("TextButton")
local Rj = Instance.new("TextButton")
local SpamVoid = Instance.new("TextButton")
local AutoAttackOff = Instance.new("TextButton")
local AntiAFK = Instance.new("TextButton")
local RepScript = Instance.new("TextButton")
local StopRespawn = Instance.new("TextButton")
local SpamRespawn = Instance.new("TextButton")
local InfYield = Instance.new("TextButton")
local AutoFistTrain = Instance.new("TextButton")
local SpyChat = Instance.new("TextButton")
local Frame_2 = Instance.new("Frame")
local Close = Instance.new("TextButton")
local openmain = Instance.new("Frame")
local open = Instance.new("TextButton")

--Properties:

ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

main.Name = "main"
main.Parent = ScreenGui
main.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
main.Position = UDim2.new(0.229522943, 0, 0.269070745, 0)
main.Size = UDim2.new(0, 737, 0, 378)
main.Visible = false
main.Active = true
main.Draggable = true

Frame.Parent = main
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.Position = UDim2.new(0.0176390782, 0, 0.029100528, 0)
Frame.Size = UDim2.new(0, 710, 0, 355)

TextLabel.Parent = Frame
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.Size = UDim2.new(0, 710, 0, 53)
TextLabel.Font = Enum.Font.SourceSansBold
TextLabel.Text = "Cookie's Gui"
TextLabel.TextColor3 = Color3.fromRGB(255, 24, 0)
TextLabel.TextScaled = true
TextLabel.TextSize = 14.000
TextLabel.TextWrapped = true

Bh.Name = "Bh"
Bh.Parent = Frame
Bh.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Bh.BorderSizePixel = 0
Bh.Position = UDim2.new(0.0197183099, 0, 0.23098591, 0)
Bh.Size = UDim2.new(0, 139, 0, 36)
Bh.Font = Enum.Font.SourceSans
Bh.Text = "Black Hub"
Bh.TextColor3 = Color3.fromRGB(255, 24, 0)
Bh.TextScaled = true
Bh.TextSize = 14.000
Bh.TextWrapped = true
Bh.MouseButton1Down:connect(function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/FastyGame31/black-hub/main/fuck%20you"))()
end)

CmdX.Name = "CmdX"
CmdX.Parent = Frame
CmdX.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
CmdX.BorderSizePixel = 0
CmdX.Position = UDim2.new(0.263380289, 0, 0.23098591, 0)
CmdX.Size = UDim2.new(0, 139, 0, 36)
CmdX.Font = Enum.Font.SourceSans
CmdX.Text = "Cmd-X"
CmdX.TextColor3 = Color3.fromRGB(255, 24, 0)
CmdX.TextScaled = true
CmdX.TextSize = 14.000
CmdX.TextWrapped = true
CmdX.MouseButton1Down:connect(function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source", true))()
end)

HealthRespawn.Name = "HealthRespawn"
HealthRespawn.Parent = Frame
HealthRespawn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
HealthRespawn.BorderSizePixel = 0
HealthRespawn.Position = UDim2.new(0.515492976, 0, 0.23098591, 0)
HealthRespawn.Size = UDim2.new(0, 139, 0, 36)
HealthRespawn.Font = Enum.Font.SourceSans
HealthRespawn.Text = "Health Respawn"
HealthRespawn.TextColor3 = Color3.fromRGB(255, 24, 0)
HealthRespawn.TextScaled = true
HealthRespawn.TextSize = 14.000
HealthRespawn.TextWrapped = true
HealthRespawn.MouseButton1Down:connect(function()
	local SavePosition = Instance.new("TextLabel")
	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.F3 then
			game.ReplicatedStorage.RemoteEvent:FireServer({"Respawn"})
			game.Lighting.Blur.Enabled = false
			game.Players.LocalPlayer.PlayerGui.IntroGui.Enabled = false
			game.Players.LocalPlayer.PlayerGui.ScreenGui.Enabled = true
			lastlocationx = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.x
			lastlocationy = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.y
			lastlocationz = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.z
			SavePosition.Text = "Last Place: " ..lastlocationx.. "," ..lastlocationy.. "," ..lastlocationz.. ","
			--print(tostring(SavePosition.Text))
			--print("Player " ..tostring(game.Players.LocalPlayer.Name).. " Respawn")
			--print(tostring(SavePosition.Text))
			wait(1.7)
			--print("screengui disabled")
			repeat wait() until game.Players.LocalPlayer.Character.Humanoid
			--print("Teleporting back to " ..tostring(SavePosition.Text))
			local FindHum = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
			game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(lastlocationx, lastlocationy, lastlocationz)
		end
	end)
	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.F3 then
			_G.on = 1
			while _G.on == 1
			do
				game.Lighting.Blur.Enabled = false
				game.Players.LocalPlayer.PlayerGui.IntroGui.Enabled = false
				game.Players.LocalPlayer.PlayerGui.ScreenGui.Enabled = true
				wait()	
			end
		end
	end)

	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.F3 then
			_G.Toggle = true
			while _G.Toggle do
				wait()
				for i, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
					if v.Name == "ForceField" then
						v:Destroy()
					end
				end
			end
		end
	end)


	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.F7 then
			_G.on = 1
			while _G.on == 1
			do
				print("\nauto respawn script made by Cookie")
				wait(1)
				local SavePosition = Instance.new("TextLabel")
				player = game.Players.LocalPlayer.Character.Humanoid
				player.HealthChanged:Connect(function(hp)
					if hp <= player.MaxHealth * 0.9 then
						game.ReplicatedStorage.RemoteEvent:FireServer({"Respawn"})
						lastlocationx = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.x
						lastlocationy = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.y
						lastlocationz = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.z
						SavePosition.Text = "Last Place: " ..lastlocationx.. "," ..lastlocationy.. "," ..lastlocationz.. ","
						--print(tostring(SavePosition.Text))
						--print("Player " ..tostring(game.Players.LocalPlayer.Name).. " Respawn")
						--print(tostring(SavePosition.Text))
						wait(1.9)
						--print("screengui disabled")
						repeat wait() until game.Players.LocalPlayer.Character.Humanoid
						--print("Teleporting back to " ..tostring(SavePosition.Text))
						local FindHum = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(lastlocationx, lastlocationy, lastlocationz)
					end
				end)
			end
		end
	end)


	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.F5 then
			_G.on = 0
			while _G.on == 1
			do


				wait(1)
				local SavePosition = Instance.new("TextLabel")
				player = game.Players.LocalPlayer.Character.Humanoid
				player.HealthChanged:Connect(function(hp)
					if hp <= player.MaxHealth * .8 then
						game.ReplicatedStorage.RemoteEvent:FireServer({"Respawn"})
						lastlocationx = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.x
						lastlocationy = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.y
						lastlocationz = game.Players.LocalPlayer.Character.HumanoidRootPart.Position.z
						SavePosition.Text = "Last Place: " ..lastlocationx.. "," ..lastlocationy.. "," ..lastlocationz.. ","
						--print(tostring(SavePosition.Text))
						--print("Player " ..tostring(game.Players.LocalPlayer.Name).. " Respawn")
						--print(tostring(SavePosition.Text))
						wait(1.9)
						--print("screengui disabled")
						repeat wait() until game.Players.LocalPlayer.Character.Humanoid
						--print("Teleporting back to " ..tostring(SavePosition.Text))
						local FindHum = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(lastlocationx, lastlocationy, lastlocationz)
					end
				end)
			end
		end
	end)

	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.F3 then
			_G.on = 1
			while _G.on == 1
			do
				game.Lighting.Blur.Enabled = false
				game.Players.LocalPlayer.PlayerGui.IntroGui.Enabled = false
				game.Players.LocalPlayer.PlayerGui.ScreenGui.Enabled = true
				wait()	
			end
		end
	end)
end)

AutoAttackOn.Name = "AutoAttackOn"
AutoAttackOn.Parent = Frame
AutoAttackOn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AutoAttackOn.BorderSizePixel = 0
AutoAttackOn.Position = UDim2.new(0.764788687, 0, 0.23098591, 0)
AutoAttackOn.Size = UDim2.new(0, 139, 0, 36)
AutoAttackOn.Font = Enum.Font.SourceSans
AutoAttackOn.Text = "Auto Attack On"
AutoAttackOn.TextColor3 = Color3.fromRGB(255, 24, 0)
AutoAttackOn.TextScaled = true
AutoAttackOn.TextSize = 14.000
AutoAttackOn.TextWrapped = true
AutoAttackOn.MouseButton1Down:connect(function()
	_G.on = 1
	while _G.on == 1
	do
		wait()
		local ohTable1 = {
			[1] = "Skill_BulletPunch",
			[2] = "Left",
			[3] = Vector3.new(game.Players.LocalPlayer.Character.LowerTorso.Position)
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
		local ohTable1 = {
			[1] = "Skill_BulletPunch",
			[2] = "Right",
			[3] = Vector3.new(game.Players.LocalPlayer.Character.LowerTorso.Position)
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
		local ohTable1 = {
			[1] = "Skill_SpherePunch",
			[2] = Vector3.new(game.Players.LocalPlayer.Character.LowerTorso.Position)
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
		local ohTable1 = {
			[1] = "Skill_Punch",
			[2] = "Right"
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
	end
end)

Split.Name = "Split"
Split.Parent = Frame
Split.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Split.BorderSizePixel = 0
Split.Position = UDim2.new(0.0197183099, 0, 0.385915488, 0)
Split.Size = UDim2.new(0, 139, 0, 36)
Split.Font = Enum.Font.SourceSans
Split.Text = "Split"
Split.TextColor3 = Color3.fromRGB(255, 24, 0)
Split.TextScaled = true
Split.TextSize = 14.000
Split.TextWrapped = true
Split.MouseButton1Down:connect(function()
	game.Players.LocalPlayer.Character.UpperTorso.Waist:remove()
end)

AutoCon.Name = "AutoCon"
AutoCon.Parent = Frame
AutoCon.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AutoCon.BorderSizePixel = 0
AutoCon.Position = UDim2.new(0.0197183099, 0, 0.552112699, 0)
AutoCon.Size = UDim2.new(0, 139, 0, 36)
AutoCon.Font = Enum.Font.SourceSans
AutoCon.Text = "Auto C On"
AutoCon.TextColor3 = Color3.fromRGB(255, 24, 0)
AutoCon.TextScaled = true
AutoCon.TextSize = 14.000
AutoCon.TextWrapped = true
AutoCon.MouseButton1Down:connect(function()
	_G.Spam = true

	while _G.Spam == true do wait()
		wait(0)
		local Remote = game.ReplicatedStorage['RemoteEvent']

		local Arguments = {
			[1] = {
				[1] = "Skill_Punch",
				[2] = "Right",
				[3] = Vector3.new(-707.280029,246.219849,751.109802)
			}
		}

		Remote:FireServer(unpack(Arguments))
		wait()
		local Remote = game.ReplicatedStorage['RemoteEvent']

		local Arguments = {
			[1] = {
				[1] = "Skill_Punch",
				[2] = "Left",
				[3] = Vector3.new(-707.280029,246.219849,751.109802)
			}
		}

		Remote:FireServer(unpack(Arguments))
	end
end)

AutoCoff.Name = "AutoCoff"
AutoCoff.Parent = Frame
AutoCoff.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AutoCoff.BorderSizePixel = 0
AutoCoff.Position = UDim2.new(0.0197183099, 0, 0.712676048, 0)
AutoCoff.Size = UDim2.new(0, 139, 0, 36)
AutoCoff.Font = Enum.Font.SourceSans
AutoCoff.Text = "Auto C Off"
AutoCoff.TextColor3 = Color3.fromRGB(255, 24, 0)
AutoCoff.TextScaled = true
AutoCoff.TextSize = 14.000
AutoCoff.TextWrapped = true
AutoCoff.MouseButton1Down:connect(function()
	_G.Spam = false

	while _G.Spam == true do wait()
		wait(0)
		local Remote = game.ReplicatedStorage['RemoteEvent']

		local Arguments = {
			[1] = {
				[1] = "Skill_Punch",
				[2] = "Right",
				[3] = Vector3.new(-707.280029,246.219849,751.109802)
			}
		}

		Remote:FireServer(unpack(Arguments))
		wait()
		local Remote = game.ReplicatedStorage['RemoteEvent']

		local Arguments = {
			[1] = {
				[1] = "Skill_Punch",
				[2] = "Left",
				[3] = Vector3.new(-707.280029,246.219849,751.109802)
			}
		}

		Remote:FireServer(unpack(Arguments))
	end
end)

RemoveFF.Name = "RemoveFF"
RemoveFF.Parent = Frame
RemoveFF.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
RemoveFF.BorderSizePixel = 0
RemoveFF.Position = UDim2.new(0.263380289, 0, 0.839436591, 0)
RemoveFF.Size = UDim2.new(0, 139, 0, 36)
RemoveFF.Font = Enum.Font.SourceSans
RemoveFF.Text = "Remove FF"
RemoveFF.TextColor3 = Color3.fromRGB(255, 24, 0)
RemoveFF.TextScaled = true
RemoveFF.TextSize = 14.000
RemoveFF.TextWrapped = true
RemoveFF.MouseButton1Down:connect(function()
	while true do
		wait()
		for i, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
			if v.Name == "ForceField" then
				v:Destroy()
			end
		end
	end
end)

Rj.Name = "Rj"
Rj.Parent = Frame
Rj.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Rj.BorderSizePixel = 0
Rj.Position = UDim2.new(0.763380289, 0, 0.552112699, 0)
Rj.Size = UDim2.new(0, 139, 0, 36)
Rj.Font = Enum.Font.SourceSans
Rj.Text = "Rejoin"
Rj.TextColor3 = Color3.fromRGB(255, 24, 0)
Rj.TextScaled = true
Rj.TextSize = 14.000
Rj.TextWrapped = true
Rj.MouseButton1Down:connect(function()
	local ts = game:GetService("TeleportService")
	local p = game:GetService("Players").LocalPlayer

	ts:Teleport(game.PlaceId, p)
end)

SpamVoid.Name = "Spam Void"
SpamVoid.Parent = Frame
SpamVoid.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
SpamVoid.BorderSizePixel = 0
SpamVoid.Position = UDim2.new(0.515492976, 0, 0.839436591, 0)
SpamVoid.Size = UDim2.new(0, 139, 0, 36)
SpamVoid.Font = Enum.Font.SourceSans
SpamVoid.Text = "Spam Void"
SpamVoid.TextColor3 = Color3.fromRGB(255, 24, 0)
SpamVoid.TextScaled = true
SpamVoid.TextSize = 14.000
SpamVoid.TextWrapped = true
SpamVoid.MouseButton1Down:connect(function()
	local playertokill = ""
	_G.toggle = false

	local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()

	local X = Material.Load({
		Title = "Enjoy voiding retarded cunts",
		Style = 6,
		SizeX = 280,
		SizeY = 140,
		Theme = "Dark"
	})

	local Main = X.New({
		Title = "Main"
	})

	local playerr = Main.TextField({
		Text = "Player",
		Callback = function(Value)
			playertokill = Value
			print(playertokill)
		end
	})

	local B = Main.Toggle({
		Text = "Loop Void",
		Callback = function(Value)
			_G.toggle = Value
		end,
		Enabled = false
	})


	local Imput = game:GetService("UserInputService")
	Imput.InputBegan:connect(function(inst)
		if inst.KeyCode == Enum.KeyCode.K then
			if _G.toggle then
				game.ReplicatedStorage.RemoteEvent:FireServer({"Respawn"})
				local player = game.Players.LocalPlayer
				local cmdlp = game.Players.LocalPlayer
				local cmdp = game:GetService("Players")
				local cmdl = game:GetService("Lighting")
				local cmdrs = game:GetService("ReplicatedStorage")
				local cmdrs2 = game:GetService("RunService")
				local cmdts = game:GetService("TweenService")
				local cmdvu = game:GetService("VirtualUser")
				local cmduis = game:GetService("UserInputService")
				local Mouses = cmdlp:GetMouse()


				function findplr(args)
					if args == "me" then
						return cmdlp
					elseif args == "random" then 
						return cmdp:GetPlayers()[math.random(1,#cmdp:GetPlayers())]
					elseif args == "new" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v.AccountAge < 30 and v ~= cmdlp then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "old" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v.AccountAge > 30 and v ~= cmdlp then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "bacon" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v.Character:FindFirstChild("Pal Hair") or v.Character:FindFirstChild("Kate Hair") and v ~= cmdlp then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "friend" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v:IsFriendsWith(cmdlp.UserId) and v ~= cmdlp then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "notfriend" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if not v:IsFriendsWith(cmdlp.UserId) and v ~= cmdlp then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "ally" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v.Team == cmdlp.Team and v ~= cmdlp then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "enemy" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v.Team ~= cmdlp.Team then
								vAges[#vAges+1] = v
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "near" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v ~= cmdlp then
								local math = (v.Character:FindFirstChild("HumanoidRootPart").Position - cmdlp.Character.HumanoidRootPart.Position).magnitude
								if math < 30 then
									vAges[#vAges+1] = v
								end
							end
						end
						return vAges[math.random(1,#vAges)]
					elseif args == "far" then
						local vAges = {} 
						for _,v in pairs(cmdp:GetPlayers()) do
							if v ~= cmdlp then
								local math = (v.Character:FindFirstChild("HumanoidRootPart").Position - cmdlp.Character.HumanoidRootPart.Position).magnitude
								if math > 30 then
									vAges[#vAges+1] = v
								end
							end
						end
						return vAges[math.random(1,#vAges)]
					else 
						for _,v in pairs(cmdp:GetPlayers()) do
							if string.find(string.lower(v.Name),string.lower(args)) then
								return v
							end
						end
					end
				end

				local target = findplr(playertokill or cmdlp.Name)
				if not target then
					warn("*","Player does not exist!")
					return
				end
				cmdlp.Character.Humanoid.Name = 1
				local l = cmdlp.Character["1"]:Clone()
				l.Parent = cmdlp.Character
				l.Name = "Humanoid"
				wait(.2)
				cmdlp.Character["1"]:Destroy()
				workspace.CurrentCamera.CameraSubject = cmdlp.Character
				cmdlp.Character.Humanoid.DisplayDistanceType = "None"
				cmdlp.Character.Humanoid:UnequipTools()
				local v = cmdlp.Backpack:FindFirstChildOfClass("Tool")
				if not v then
					warn("*","Tool not found!")
					return
				end
				v.Parent = cmdlp.Character
				v.Parent = cmdlp.Character.HumanoidRootPart
				firetouchinterest(target.Character.HumanoidRootPart, v.Handle, 0)
				firetouchinterest(target.Character.HumanoidRootPart, v.Handle, 1)
				pcall(function() cmdlp.Character.HumanoidRootPart.CFrame = CFrame.new(0, workspace.FallenPartsDestroyHeight + 5, 0) end)
				wait(.3)
				cmdlp.Character:Remove()
				cmdlp.CharacterAdded:Wait()
			end
		end
	end)
end)

AutoAttackOff.Name = "AutoAttackOff"
AutoAttackOff.Parent = Frame
AutoAttackOff.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AutoAttackOff.BorderSizePixel = 0
AutoAttackOff.Position = UDim2.new(0.763380289, 0, 0.385915518, 0)
AutoAttackOff.Size = UDim2.new(0, 139, 0, 36)
AutoAttackOff.Font = Enum.Font.SourceSans
AutoAttackOff.Text = "Auto Attack Off"
AutoAttackOff.TextColor3 = Color3.fromRGB(255, 24, 0)
AutoAttackOff.TextScaled = true
AutoAttackOff.TextSize = 14.000
AutoAttackOff.TextWrapped = true
AutoAttackOff.MouseButton1Down:connect(function()
	_G.on = 0
	while _G.on == 1
	do
		wait()
		local ohTable1 = {
			[1] = "Skill_BulletPunch",
			[2] = "Left",
			[3] = Vector3.new(game.Players.LocalPlayer.Character.LowerTorso.Position)
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
		local ohTable1 = {
			[1] = "Skill_BulletPunch",
			[2] = "Right",
			[3] = Vector3.new(game.Players.LocalPlayer.Character.LowerTorso.Position)
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
		local ohTable1 = {
			[1] = "Skill_SpherePunch",
			[2] = Vector3.new(game.Players.LocalPlayer.Character.LowerTorso.Position)
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
		local ohTable1 = {
			[1] = "Skill_Punch",
			[2] = "Right"
		}

		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(ohTable1)
	end
end)

AntiAFK.Name = "AntiAFK"
AntiAFK.Parent = Frame
AntiAFK.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AntiAFK.BorderSizePixel = 0
AntiAFK.Position = UDim2.new(0.763380289, 0, 0.712675989, 0)
AntiAFK.Size = UDim2.new(0, 139, 0, 36)
AntiAFK.Font = Enum.Font.SourceSans
AntiAFK.Text = "Anti Afk"
AntiAFK.TextColor3 = Color3.fromRGB(255, 24, 0)
AntiAFK.TextScaled = true
AntiAFK.TextSize = 14.000
AntiAFK.TextWrapped = true
AntiAFK.MouseButton1Down:connect(function()
	script=loadstring(game:HttpGet("https://pastebin.com/raw/XXPTpkcZ", true))()
end)

RepScript.Name = "RepScript"
RepScript.Parent = Frame
RepScript.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
RepScript.BorderSizePixel = 0
RepScript.Position = UDim2.new(0.515492976, 0, 0.712676048, 0)
RepScript.Size = UDim2.new(0, 139, 0, 36)
RepScript.Font = Enum.Font.SourceSans
RepScript.Text = "Rep Script"
RepScript.TextColor3 = Color3.fromRGB(255, 24, 0)
RepScript.TextScaled = true
RepScript.TextSize = 14.000
RepScript.TextWrapped = true
RepScript.MouseButton1Down:connect(function()
	game.Players.LocalPlayer.Character.Head.NameBbGui.NameTxt: Destroy()
end)

StopRespawn.Name = "StopRespawn"
StopRespawn.Parent = Frame
StopRespawn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
StopRespawn.BorderSizePixel = 0
StopRespawn.Position = UDim2.new(0.263380289, 0, 0.712676048, 0)
StopRespawn.Size = UDim2.new(0, 139, 0, 36)
StopRespawn.Font = Enum.Font.SourceSans
StopRespawn.Text = "Stop Respawn"
StopRespawn.TextColor3 = Color3.fromRGB(255, 24, 0)
StopRespawn.TextScaled = true
StopRespawn.TextSize = 14.000
StopRespawn.TextWrapped = true
StopRespawn.MouseButton1Down:connect(function()
	_G.Toggle = false
	while _G.Toggle do
		local args = {
			[1] = {
				[1] = "Respawn"
			}
		}
		wait(0.2)
		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(unpack(args))
		for i, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
			if v.Name == "ForceField" then
				v:Destroy()
			end
		end
	end
end)

SpamRespawn.Name = "SpamRespawn"
SpamRespawn.Parent = Frame
SpamRespawn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
SpamRespawn.BorderSizePixel = 0
SpamRespawn.Position = UDim2.new(0.263380289, 0, 0.552112699, 0)
SpamRespawn.Size = UDim2.new(0, 139, 0, 36)
SpamRespawn.Font = Enum.Font.SourceSans
SpamRespawn.Text = "Spam Respawn"
SpamRespawn.TextColor3 = Color3.fromRGB(255, 24, 0)
SpamRespawn.TextScaled = true
SpamRespawn.TextSize = 14.000
SpamRespawn.TextWrapped = true
SpamRespawn.MouseButton1Down:connect(function()
	_G.Toggle = true
	while _G.Toggle do
		local args = {
			[1] = {
				[1] = "Respawn"
			}
		}
		wait(0.2)
		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(unpack(args))
		for i, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
			if v.Name == "ForceField" then
				v:Destroy()
			end
		end
	end
end)

InfYield.Name = "InfYield"
InfYield.Parent = Frame
InfYield.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
InfYield.BorderSizePixel = 0
InfYield.Position = UDim2.new(0.263380289, 0, 0.385915518, 0)
InfYield.Size = UDim2.new(0, 139, 0, 36)
InfYield.Font = Enum.Font.SourceSans
InfYield.Text = "InfYield"
InfYield.TextColor3 = Color3.fromRGB(255, 24, 0)
InfYield.TextScaled = true
InfYield.TextSize = 14.000
InfYield.TextWrapped = true
InfYield.MouseButton1Down:connect(function()
	loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end)

AutoFistTrain.Name = "AutoFistTrain"
AutoFistTrain.Parent = Frame
AutoFistTrain.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AutoFistTrain.BorderSizePixel = 0
AutoFistTrain.Position = UDim2.new(0.515492976, 0, 0.385915518, 0)
AutoFistTrain.Size = UDim2.new(0, 139, 0, 36)
AutoFistTrain.Font = Enum.Font.SourceSans
AutoFistTrain.Text = "Auto Fist Train"
AutoFistTrain.TextColor3 = Color3.fromRGB(255, 24, 0)
AutoFistTrain.TextScaled = true
AutoFistTrain.TextSize = 14.000
AutoFistTrain.TextWrapped = true
AutoFistTrain.MouseButton1Down:connect(function()
	game.Lighting.Blur.Enabled = false
	game.Players.LocalPlayer.PlayerGui.IntroGui.Enabled = false
	game.Players.LocalPlayer.PlayerGui.ScreenGui.Enabled = true
	-- FS script
	local args = {
		[1] = {
			[1] = "Respawn"
		}
	}
	game:GetService("ReplicatedStorage").RemoteEvent:FireServer(unpack(args))
	wait(3)
	local VirtualUser=game:service'VirtualUser'
	game:service'Players'.LocalPlayer.Idled:connect(function()
		VirtualUser:CaptureController()
		VirtualUser:ClickButton2(Vector2.new())
		print("anti-afk Enabled")
	end)
	plr = game.Players.LocalPlayer
	hum = plr.Character.HumanoidRootPart
	hum.CFrame = CFrame.new(-272,281,1000)
	wait(1)
	game.Players.LocalPlayer.Character.UpperTorso.Waist:remove()
	wait(1)
	plr = game.Players.LocalPlayer
	hum = plr.Character.HumanoidRootPart
	hum.CFrame = CFrame.new(-369,15735,-9)
	_G.on = 1
	while _G.on == 1
	do
		local args = {
			[1] = {
				[1] = "+FS6"
			}
		}
		wait(0)
		game:GetService("ReplicatedStorage").RemoteEvent:FireServer(unpack(args))
	end
end)

SpyChat.Name = "SpyChat"
SpyChat.Parent = Frame
SpyChat.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
SpyChat.BorderSizePixel = 0
SpyChat.Position = UDim2.new(0.515492976, 0, 0.552112699, 0)
SpyChat.Size = UDim2.new(0, 139, 0, 36)
SpyChat.Font = Enum.Font.SourceSans
SpyChat.Text = "Spy Chat"
SpyChat.TextColor3 = Color3.fromRGB(255, 24, 0)
SpyChat.TextScaled = true
SpyChat.TextSize = 14.000
SpyChat.TextWrapped = true
SpyChat.MouseButton1Down:connect(function()
	--This script reveals ALL hidden messages in the default chat
	--chat "/spy" to toggle!
	enabled = true
	--if true will check your messages too
	spyOnMyself = true
	--if true will chat the logs publicly (fun, risky)
	public = false
	--if true will use /me to stand out
	publicItalics = true
	--customize private logs
	privateProperties = {
		Color = Color3.fromRGB(255, 140, 0);
		Font = Enum.Font.SourceSansBold;
		TextSize = 18;
	}
	--////////////////////////////////////////////////////////////////
	local StarterGui = game:GetService("StarterGui")
	local Players = game:GetService("Players")
	local player = Players.LocalPlayer or Players:GetPropertyChangedSignal("LocalPlayer"):Wait() or Players.LocalPlayer
	local saymsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest")
	local getmsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("OnMessageDoneFiltering")
	local instance = (_G.chatSpyInstance or 0) + 1
	_G.chatSpyInstance = instance

	local function onChatted(p,msg)
		if _G.chatSpyInstance == instance then
			if p==player and msg:lower():sub(1,4)=="/dad" then
				enabled = not enabled
				wait(0.3)
				privateProperties.Text = "{SPY "..(enabled and "EN" or "DIS").."ABLED}"
				StarterGui:SetCore("ChatMakeSystemMessage",privateProperties)
			elseif enabled and (spyOnMyself==true or p~=player) then
				msg = msg:gsub("[\n\r]",''):gsub("\t",' '):gsub("[ ]+",' ')
				local hidden = true
				local conn = getmsg.OnClientEvent:Connect(function(packet,channel)
					if packet.SpeakerUserId==p.UserId and packet.Message==msg:sub(#msg-#packet.Message+1) and (channel=="All" or (channel=="Team" and public==false and Players[packet.FromSpeaker].Team==player.Team)) then
						hidden = false
					end
				end)
				wait(1)
				conn:Disconnect()
				if hidden and enabled then
					if public then
						saymsg:FireServer((publicItalics and "/me " or '').."{SPY} [".. p.Name .."]: "..msg,"All")
					else
						privateProperties.Text = "{SPY} [".. p.Name .."]: "..msg
						StarterGui:SetCore("ChatMakeSystemMessage",privateProperties)
					end
				end
			end
		end
	end

	for _,p in ipairs(Players:GetPlayers()) do
		p.Chatted:Connect(function(msg) onChatted(p,msg) end)
	end
	Players.PlayerAdded:Connect(function(p)
		p.Chatted:Connect(function(msg) onChatted(p,msg) end)
	end)
	privateProperties.Text = "{SPY "..(enabled and "EN" or "DIS").."ABLED}"
	StarterGui:SetCore("ChatMakeSystemMessage",privateProperties)
	if not player.PlayerGui:FindFirstChild("Chat") then wait(3) end
	local chatFrame = player.PlayerGui.Chat.Frame
	chatFrame.ChatChannelParentFrame.Visible = true
	chatFrame.ChatBarParentFrame.Position = chatFrame.ChatChannelParentFrame.Position+UDim2.new(UDim.new(),chatFrame.ChatChannelParentFrame.Size.Y)
end)

Frame_2.Parent = main
Frame_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame_2.Position = UDim2.new(0.0176390782, 0, 0.169312164, 0)
Frame_2.Size = UDim2.new(0, 710, 0, 7)

Close.Name = "Close"
Close.Parent = main
Close.BackgroundColor3 = Color3.fromRGB(255, 24, 0)
Close.BorderSizePixel = 0
Close.Position = UDim2.new(0.932157397, 0, 0.0291005298, 0)
Close.Size = UDim2.new(0, 36, 0, 27)
Close.Font = Enum.Font.SourceSansBold
Close.Text = "X"
Close.TextColor3 = Color3.fromRGB(0, 0, 0)
Close.TextScaled = true
Close.TextSize = 14.000
Close.TextWrapped = true
Close.MouseButton1Down:connect(function()
	main.Visible = false
	openmain.Visible = true
end)

openmain.Name = "openmain"
openmain.Parent = ScreenGui
openmain.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
openmain.Position = UDim2.new(0, 0, 0.64358449, 0)
openmain.Size = UDim2.new(0, 83, 0, 26)
openmain.Active = true
openmain.Draggable = true

open.Name = "open"
open.Parent = openmain
open.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
open.BorderColor3 = Color3.fromRGB(0, 0, 0)
open.BorderSizePixel = 3
open.Size = UDim2.new(0, 87, 0, 29)
open.Font = Enum.Font.SourceSansBold
open.Text = "Open"
open.TextColor3 = Color3.fromRGB(0, 0, 0)
open.TextScaled = true
open.TextSize = 14.000
open.TextWrapped = true
open.MouseButton1Down:connect(function()
	openmain.Visible = false
	main.Visible = true
end)

-- Scripts:

local function XYTCB_fake_script() -- main.LocalScript 
	local script = Instance.new('LocalScript', main)

	function zigzag(X) return math.acos(math.cos(X*math.pi))/math.pi end
	
	counter = 0
	
	while wait(0.1)do
		script.Parent.BackgroundColor3 = Color3.fromHSV(zigzag(counter),1,1)
	
		counter = counter + 0.01
	end
	
end
coroutine.wrap(XYTCB_fake_script)()
local function EDQBUH_fake_script() -- Frame_2.LocalScript 
	local script = Instance.new('LocalScript', Frame_2)

	function zigzag(X) return math.acos(math.cos(X*math.pi))/math.pi end
	
	counter = 0
	
	while wait(0.1)do
		script.Parent.BackgroundColor3 = Color3.fromHSV(zigzag(counter),1,1)
	
		counter = counter + 0.01
	end
	
end
coroutine.wrap(EDQBUH_fake_script)()

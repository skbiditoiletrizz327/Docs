> Create a cool loading animation in the console with gradient texts

```lua

--> Destroying old Label if found
if Label then 
	Label:Destroy()
end 

local Modules = {
    Colors = {
        ["Orange"] = {255, 100, 0},  
        ["Yellow"] = {255, 255, 0} 
    },
    ChangeColor = function() --> Enabling rich text to modify color using <font color ='rgb()'>text</font> 
        game:GetService("RunService").Heartbeat:Connect(function()
            if game:GetService("CoreGui"):FindFirstChild("DevConsoleMaster") then 
                for _, v in pairs(game:GetService("CoreGui"):FindFirstChild("DevConsoleMaster"):GetDescendants()) do 
                    if v:IsA("TextLabel") then 
                        v.RichText = true 
                    end 
                end 
            end
        end)
    end,
}

local function Gradient(text, startColor, endColor, progressShift)
    local colorStart = { R = startColor.R, G = startColor.G, B = startColor.B }
    local colorEnd = { R = endColor.R, G = endColor.G, B = endColor.B }

    local output = {}
    for i = 1, #text do
        local progress = 0 
        if progressShift ~= nil then 
            progress = ((i - 1) / (#text - 1) + progressShift) % 1
        else 
            progress = (i - 1) / (#text - 1)
        end 

        local r = colorStart.R + progress * (colorEnd.R - colorStart.R)
        local g = colorStart.G + progress * (colorEnd.G - colorStart.G)
        local b = colorStart.B + progress * (colorEnd.B - colorStart.B)

        table.insert(output, string.format('<font color="rgb(%d,%d,%d)">%s</font>', r * 255, g * 255, b * 255, text:sub(i, i)))
    end

    return table.concat(output)
end

local function loopGradientText(text, startColor, endColor)
    local loadingLabel = nil
    local mark = tostring(math.random(255,2555))

    --> Using 'print' to get the text label 
    print(mark)


    repeat task.wait()
        for _, label in pairs(game.CoreGui:FindFirstChild("DevConsoleMaster"):GetDescendants()) do 
            if label:IsA("TextLabel") and string.find(label.Text:lower(), mark:lower()) then 
                loadingLabel = label 
                break
            end 
        end 
    until loadingLabel



    Modules.ChangeColor()

    local old = 0
    local Loop = nil 
    Loop = game:GetService("RunService").Heartbeat:Connect(function(delta)
        old = old + delta

        local gradientText = Gradient(text, startColor, endColor, old)
        
        loadingLabel.Text = gradientText
    end)

    return Loop,loadingLabel
end

--> used for creating cool symbols inside the console
local function createThing(options)
    local StartingString = "\n\n\n\n\n┌───"
    local EndingString = "\n└──"

    local CurrentText = StartingString

    for index,value in pairs(options) do 
        CurrentText = CurrentText.."\n├─  "..value 
    end 

    CurrentText = CurrentText..EndingString
    return CurrentText
end 

local Start = os.time()
local Loop,Label = loopGradientText("                                               [Name] [Loading] ", Color3.fromRGB(100, 255, 0), Color3.fromRGB(0, 0, 255))

task.wait(2) --> or do your magic :monion:

Loop:Disconnect()

local colorStart,colorEnd = Color3.fromRGB(255, 100, 0), Color3.fromRGB(200, 200, 0)

local Options = {
    Gradient(("[Name] Logged in as %s"):format("User"),colorStart,colorEnd),
    Gradient(("[Name] Took %ds to load"):format(os.time()-Start),colorStart,colorEnd),
	Gradient("[Name] -> ",colorStart,colorEnd)
}

Label.Text = createThing(Options)
local Box = Instance.new("TextBox",Label)

Box.Transparency = 0
Box.Size = UDim2.new(0, 500, 0, 14)
Box.BackgroundTransparency = 1
Box.Position = Label.Position - UDim2.new(0,100,0,-55.5)
Box.BackgroundColor3 = Color3.fromRGB(255,255,255)
Box.TextScaled = true 
Box.PlaceholderText = ""
Box.Text = ""
Box.TextColor3 = Color3.fromRGB(255,255,255)

Box.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        Label.Text = "\n"..Label.Text.."\n"..Box.Text
    end
end)

getgenv().Label = Label 
```


> print a gradient text to the output

```lua
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

local function Gradient(text, startColor, endColor)
    local colorStart = { R = startColor.R, G = startColor.G, B = startColor.B }
    local colorEnd = { R = endColor.R, G = endColor.G, B = endColor.B }

    local output = {}
    for i = 1, #text do
        local progress = (i - 1) / (#text - 1)
        local r = colorStart.R + progress * (colorEnd.R - colorStart.R)
        local g = colorStart.G + progress * (colorEnd.G - colorStart.G)
        local b = colorStart.B + progress * (colorEnd.B - colorStart.B)

        table.insert(output, string.format('<font color="rgb(%d,%d,%d)">%s</font>', r * 255, g * 255, b * 255, text:sub(i, i)))
    end

    return table.concat(output)
end

Modules.ChangeColor()

print(Gradient("This is a example txt",Color3.fromRGB(100, 255, 0),Color3.fromRGB(0, 0, 255)))
```
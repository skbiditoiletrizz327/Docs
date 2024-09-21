> Adds a custom `:GetObjects()` method to the game, if it does not exist / work

```lua
local success,err = pcall(function()
	game:GetObjects("rbxassetid://4446576906") --> Actually trying if GetObjects exists
end)

--> if it didnt work then add :GetObjects() method to the game 
if not success then 
	local InsertService = game:GetService("InsertService")
	local oldGame = game --> storing old game 

	local function GetObject(id)
		local success,obj = pcall(function()
			return {InsertService:LoadLocalAsset(id)}
		end)

		if success then 
			return obj 
		end 

		print(`Something didnt work: {obj}`)
	end

	getgenv().game = setmetatable({}, {
		__index = function(index, method)
			local success,response = pcall(function()
				return oldGame[method]
			end)

			if method == "GetObjects" then
				return function(index, id)
					return GetObject(id) or GetObject
				end
			elseif response and type(response) == "function" then
				return function(index,func)
					return response(oldGame, func)
				end
			else
				return oldGame:GetService(method) or oldGame[method]
			end
    	end
	})
end 
```
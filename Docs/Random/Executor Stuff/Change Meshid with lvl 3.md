local InsertService = game:GetService("InsertService")
local OldInstance = Instance.new --> Storing for later

```lua
local Instance = {} --> Instance: readonly so yeah
function Instance.new(type,parent,id)
	if type == "MeshPart" and id then 
		local Object = InsertService:CreateMeshPartAsync(
            id, 
            Enum.CollisionFidelity.Default, 
            Enum.RenderFidelity.Automatic
        )

		Object.Parent = parent 
		return Object
	end 

	return OldInstance(type,parent)
end

local Tool =  Instance.new("Tool",game.Players.LocalPlayer.Backpack)

--> Usage:  Instance.new("MeshPart",Parent,Mesh Id)

--> Example of making a tool with it: 

local Mesh = Instance.new(
    "MeshPart",
    game:GetService("Workspace"),
    "rbxassetid://11442510" --> Katana
)

Mesh.TextureID = "rbxassetid://11442524"

Mesh.Parent = Tool 
Mesh.Name = "Handle"

Tool.RequiresHandle = true 
Tool.Name = "Example"
Tool.Grip = CFrame.new(0,0,-1) * CFrame.Angles(math.rad(90),math.rad(90),0)
```
![alt text](https://cdn.discordapp.com/attachments/1263568574002495529/1287143670532800612/image.png?ex=66f07975&is=66ef27f5&hm=f26d4e3d2ee65a17f5207425634f49a561eadc7f33662fe4322e2ee04d043369&)

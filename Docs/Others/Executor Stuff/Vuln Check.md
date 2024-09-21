> Just a simple to check for service vulns in a exc

```lua
local Vulns = {
	LinkingService = {
		{
			"OpenUrl"
		},
		game:GetService("LinkingService")
	},
	ScriptContext = {
		{
			"SaveScriptProfilingData",
			"AddCoreScriptLocal",
		},
		game:GetService("ScriptContext")
	},
	MessageBusService = {
		{
			"Call",
			"GetLast",
			"GetMessageId",
			"GetProtocolMethodRequestMessageId",
			"GetProtocolMethodResponseMessageId",
			"MakeRequest",
			"Publish",
			"PublishProtocolMethodRequest",
			"PublishProtocolMethodResponse",
			"Subscribe",
			"SubscribeToProtocolMethodRequest",
			"SubscribeToProtocolMethodResponse"
		},
		game:GetService("MessageBusService")
	},
	GuiService = {
		{
			"OpenBrowserWindow",
			"OpenNativeOverlay"
		},
		game:GetService("GuiService")
	},
	MarketplaceService = {
		{
			"GetRobuxBalance",
			"PerformPurchase",
			"PerformPurchaseV2"
		},
		game:GetService("MarketplaceService")
	},
	HttpRbxApiService = {
		{
			"GetAsyncFullUrl",
			"PostAsyncFullUrl",
			"GetAsync",
			"PostAsync",
			"RequestAsync"
		},
		game:GetService("HttpRbxApiService")
	},
	Players = {
		{
			"ReportAbuse",
			"ReportAbuseV3"
		},
		game:GetService("Players")
	},
	HttpService = {
		{
			"RequestInternal"
		},
		game:GetService("HttpService")
	},
	BrowserService = {
		{
			"ExecuteJavaScript",
			"OpenBrowserWindow",
			"ReturnToJavaScript",
			"OpenUrl",
			"SendCommand",
			"OpenNativeOverlay"
		},
		game:GetService("BrowserService")
	},
	CaptureService = {
		{
			"DeleteCapture"
		},
		game:GetService("CaptureService")
	},
	OmniRecommendationsService = {
		{
			"MakeRequest"
		},
		game:GetService("OmniRecommendationsService")
	},
	OpenCloudService = {
		{
			"HttpRequestAsync"
		},
		game:GetService("OpenCloudService")
	}
}
local blocked, failed, succeeded = 0, 0, 0
local function test()
	for serviceName, serviceData in pairs(Vulns) do
		local functionsList, service = serviceData[1], serviceData[2]
		for _, functionName in ipairs(functionsList) do
			local success, err = pcall(function()
				service[functionName](service)
			end)
			if not success then
				if string.find(err:lower(), "blocked") then
					blocked = blocked + 1
					print(("[%s] %s:%s() is blocked"):format("❌", serviceName, functionName))
				else
					failed = failed + 1
					print(("[%s] %s:%s() failed: %s"):format("⚠️", serviceName, functionName, err))
				end
			else
				succeeded = succeeded + 1
				print(("[%s] %s:%s() succeeded"):format("✅", serviceName, functionName))
			end
		end
	end
	print(("✅ %d blocked, ⚠️ %d failed, ❌ %d succeded"):format(blocked, failed, succeeded))
end
test()
```

![alt Text](https://cdn.discordapp.com/attachments/1282366994783404144/1287088440495570944/image.png?ex=66f04605&is=66eef485&hm=81d889fb70c3b2cd1f8184987d2981280ef0854345c4458b151c1130091a68cc&)
---
description: Custom ribbon commands for use in the ISE.
---

# ISE Plugins

You can include custom ribbon commands in the ISE to aid in improving the script authoring experience.

![ISE Plugins](https://user-images.githubusercontent.com/933163/61412200-dea9f680-a8ad-11e9-8ba8-2610d6f63a39.png)

## Add a custom plugin

For this example, we wish to have a plugin that analyzes the script and reports on any errors.

Create a new script stored under the following structure:

![Analyze Script](https://user-images.githubusercontent.com/933163/61412797-680df880-a8af-11e9-9656-e1cc609d26fe.png)

{% hint style="info" %}
The path structure needs to follow `[MODULE]/Internal/ISE Plugins/[PLUGIN_NAME]`. Here we have `X-Demo/Internal/ISE Plugins/Analyze Script`.
{% endhint %}

Use the following sample to fill in the script body.

```text
if([string]::IsNullOrWhiteSpace($scriptText)){
    Show-Alert "Script is empty - nothing to format."
    exit
}

Import-Module -Name PSScriptAnalyzer
Invoke-ScriptAnalyzer -ScriptDefinition $scriptText
```

Now you can run the command from the ribbon and see the results in the ISE.

![Example results](https://user-images.githubusercontent.com/933163/61413046-1fa30a80-a8b0-11e9-84ac-bcd536f18b42.png)
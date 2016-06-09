# Workflows

The *Workflows* integration allows for scripts to run like workflow commands. Rules can be used to control visiblity and enablement. The script is only executed when the command is triggered.

1. Begin by adding a new item to a workflow command of template type `/Modules/PowerShell Console/PowerShell Script Workflow Action`. We've added an insert option to help with this.
2. Edit the *Type string* field to your custom type or to the built in type `Cognifide.PowerShell.Integrations.Workflows.ScriptAction, Cognifide.PowerShell`. 
3. Edit the *Script body* with the appropriate script. I like to save my workflow scripts in a library called *Workflows*.
4. Configure the rules on the workflow action item to specify when to execute. Leave the rule alone if you wish for it to execute any time the command is activated.
5. Edit the script in your *Workflows* library to perform the appropriate actions. The script can run in the background and show dialogs.
6. Change the icon of the workflow action item to match the script purpose.
7. Celebrate your success with the team!

**Example:** The following requests input from the user then writes to the workflow history.
```powershell
$item = Get-Item -Path .
$comment = Show-Input -Prompt "Enter a comment:"
if($comment) {
    New-ItemWorkflowEvent -Item $item -Text $comment
}
Close-Window
```

See how Adam integrated [workflow actions][2] if you are really curious to know more.

### References

* [Fixing problematic content or Workflow state][1]
 
[1]: http://www.cognifide.com/blogs/sitecore/feel-the-power-in-powershell/
[2]: http://blog.najmanowicz.com/2014/11/09/introducing-powershell-actions-for-sitecore-workflows/
# Workflows

The _Workflows_ integration allows for scripts to run like workflow commands. Rules can be used to control visiblity and enablement. The script is only executed when the command is triggered.

1. Begin by adding a new item to a workflow command of template type `/Modules/PowerShell Console/PowerShell Script Workflow Action`. We've added an insert option to help with this.
2. Edit the _Type string_ field to your custom type or to the built in type `Spe.Integrations.Workflows.ScriptAction, Spe`. 
3. Edit the _Script body_ with the appropriate script. I like to save my workflow scripts in a library called _Workflows_.
4. Configure the **Enable** rules on the workflow action item to specify when to execute. Leave the rule alone if you wish for it to execute any time the command is activated.
5. Edit the script in your _Workflows_ library to perform the appropriate actions. The script can run in the background and show dialogs.
6. Change the icon of the workflow action item to match the script purpose.
7. Celebrate your success with the team!

**Example:** The following requests input from the user then writes to the workflow history.

```text
$item = $SitecoreContextItem
$comment = Show-Input -Prompt "Enter a comment:"
if($comment) {
    New-ItemWorkflowEvent -Item $item -Text $comment
}
Close-Window
```

See how Adam integrated [workflow actions](https://github.com/SitecorePowerShell/Book/tree/5daee3160885dadd7031fee723dccf12a33abd7b/modules/integration-points/[https:/blog.najmanowicz.com/2014/11/09/introducing-powershell-actions-for-sitecore-workflows/]/README.md) if you are really curious to know more.

## References

* [Fixing problematic content or Workflow state](https://www.cognifide.com/blogs/sitecore/feel-the-power-in-powershell/)


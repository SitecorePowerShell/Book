# Content Editor

### Context Menu

The *Context Menu* integration allows for options in the context menu. Rules can be used to control visiblity and enablement. The script is only executed when the option is clicked.

1. Begin by adding a new script to the *Context Menu* library. The name of the script will appear in the context menu.
2. Edit the script to perform the appropriate actions. The script can run in the background and show dialogs.
3. Change the icon of the item to match the script purpose.
4. Configure any rules as needed.

See how Adam added [context menu PowerShell scripts][2].

### Gutter

The *Gutter* integration allows for full flexibility of adding a gutter element.

1. Begin by adding a new script to the *Gutter* library.
2. Edit the script to create a new instance of `Sitecore.Shell.Applications.ContentEditor.Gutters.GutterIconDescriptor` if the right conditions are met. 
  * Set the **Icon**, **Tooltip**, and **Click** properties.
  * Return the gutter object
3. Rebuild the gutter integration from within the ISE.
  * Settings tab
  * Integration chunk
  * Sync Library with Content Editor Gutter command

### Insert Item

The *Insert Item* integration allows for insert options in the context menu. Rules can be used to control visiblity and enablement. The script is only executed when the option is clicked.

1. Begin by adding a new script to the *Insert Item* library. The name of the script will appear in the context menu.
2. Edit the script to perform the appropriate actions. The script can run in the background and show dialogs.
3. Change the icon of the item to match the script purpose.
4. Configure any rules as needed.

### Ribbon

The *Ribbon* integration allows for commands in the ribbon. Rules can be used to control visiblity and enablement. The script is only executed when the option is clicked.

1. Begin by adding a new script to the *Insert Item* library. The name of the script will appear in the context menu.
2. Edit the script to perform the appropriate actions. The script can run in the background and show dialogs.
3. Change the icon of the item to match the script purpose.
4. Configure any rules as needed.

See the birth of [extending the Sitecore ribbon with powershell scripts][1] by Adam.
### Warning

[1]: http://blog.najmanowicz.com/2011/11/24/extending-sitecore-ribbon-with-powershell-scripts/
[2]: http://blog.najmanowicz.com/2011/11/22/context-powershell-scripts-for-sitecore/
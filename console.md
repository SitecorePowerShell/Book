# Console

The SPE Console is a command line interface (CLI) designed for efficiency. The console provides a streamlined tool for working with Windows PowerShell and Sitecore. The default configuration for SPE requires the Console to be in an [Elevated Session State](/security.md) before allowing the execution of commands.

The following figure shows the Console when the User Account Controls (UAC) are disabled. While this is a common configuration for developers, we highly encourage you to ensure UAC is enabled in higher environments.

[![PowerShell Console](images/screenshots/cli-empty.png)](https://youtu.be/1TLYyzTw01w "Click for a quick demo")


### Shortcuts
Below are the shortcuts available in the console.

| **Shortcut**  | **Usage** |
| --------  | ----- |
| Enter | Submits line for execution. |
| Tab       | Autocomplete commands. Press tab again to cycle through commands.  |
| Shift+Tab | Reverse cycle through Autocomplete commands. |
| Shift+Enter | Inserts new line. Works when the backtick is used. |
| Up Arrow/Ctrl+P   | Show previous command from history    |
| Down Arrow/Ctrl+N | Show next command from history        |
| Delete/backspace  | Remove one character from right/left to the cursor    |
| Left Arrow/Ctrl+B | Move cursor to the left   |
| Right Arrow/Ctrl+F    | Move cursor to the right  |
| Ctrl+Left Arrow   | Move cursor to previous word  |
| Ctrl+Right Arrow  | Move cursor to next word  |
| Ctrl+A/Home       | Move cursor to the beginning of the line  |
| Ctrl+E/End        | Move cursor to the end of the line    |
| Ctrl+K/Alt+D     | Remove the text after the cursor  |
| Ctrl+H | Remove character before the cursor |
| Ctrl+D/Delete | Remove character after the cursor |
| Ctrl+U            | Remove the text before the cursor |
| Ctrl+V            | Insert text from the clipboard    |
| Ctrl+Alt+Shift +  | Increase the font size |
| Ctrl+Alt+Shift -  | Decrease the font size |

**Note:** The font family, font size, and other settings can be configured through the ISE.

[1]: https://github.com/SitecorePowerShell/Console/issues/314
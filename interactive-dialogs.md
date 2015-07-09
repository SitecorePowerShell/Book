# Interactive Dialogs

We've provided a few commands to interact with the user through dialogs.

### Alert

**Example:** The following display a modal dialog.
```powershell
Show-Alert -Title "SPE is great!"
```

![Show Alert](images/screenshots/modaldialog-showalert.png)

### Variable Settings
Read-Variable

### Confirmation

**Example:** The following displays a modal dialog with an OK or Cancel confirmation.
```powershell
Show-Confirm -Title "Click OK to acknowledge SPE is great!"
```
| Button | Return |
| -- | -- |
| OK | yes |
| Cancel | no |


![Show Confirm](images/screenshots/modaldialog-showconfirm.png)

### Download
Receive-File

### Upload
Send-File

### Field Editor
Show-FieldEditor

### User Input
Show-Input

### File Browser
Show-ModalDialog

### Data List
Show-ListView

### Results
Show-Result


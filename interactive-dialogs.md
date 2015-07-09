# Interactive Dialogs

We've provided a few commands to interact with the user through dialogs.

### Alert

**Example:** The following display a modal dialog.
```powershell
Show-Alert -Title "SPE is great!"
```

No return value.

![Show Alert](images/screenshots/modaldialog-showalert.png)

### Variable Settings
Read-Variable

### Confirmation

**Example:** The following displays a modal dialog with an OK or Cancel confirmation.
```powershell
Show-Confirm -Title "Click OK to acknowledge SPE is great!"
```

| Button Name | Return Value |
| -- | -- |
| OK | yes |
| Cancel | no |


![Show Confirm](images/screenshots/modaldialog-showconfirm.png)

### Upload

**Example:** The following displays an advanced upload dialog.
```powershell
Receive-File (get-item "master:\media library\Files") -AdvancedDialog
```
No return value.

![Receive File](images/screenshots/modaldialog-receivefileadvanced.png)

### Download
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


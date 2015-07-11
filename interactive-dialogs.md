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

### Confirmation Choice

**Example:** The following displays a modal dialog with choices.
```powershell
Show-ModalDialog -Control "ConfirmChoice" -Parameters @{btn_0="Yes (returns btn_0)"; btn_1="No (returns btn_1)"; btn_2="returns btn_2"; te="Have you downloaded SPE?"; cp="Important Questions"} -Height 120 -Width 400
```

**Note:** The hashtable keys should be incremented like *btn_0*, *btn_1*, and so on. The return value is the key name.

| Button Name | Return Value |
| -- | -- |
| < first button > | btn_0 |
| < second button > | btn_1 |
| < third button > | btn_2 |

![Show Confirm Choice](images/screenshots/modaldialog-showconfirmchoice.png)

### Upload

**Example:** The following displays an advanced upload dialog.
```powershell
Receive-File (get-item "master:\media library\Files") -AdvancedDialog
```
No return value.

![Receive File](images/screenshots/modaldialog-receivefileadvanced.png)

### Download

**Example:** The following displays a download dialog.
```powershell
Get-Item -Path "master:\media library\Files\readme" | Send-File
```

![Download](images/screenshots/modaldialog-download.png)

### Field Editor
Show-FieldEditor

### User Input

**Example:** The following displays an input dialog for text.
```powershell
Show-Input "Please provide 5 characters at most" -MaxLength 5
```

| Button Name | Return Value |
| -- | -- |
| OK | < user input > |
| Cancel | $null |


![Show Input](images/screenshots/modaldialog-showinput.png)

### File Browser
Show-ModalDialog

### Data List
Show-ListView

### Results
Show-Result


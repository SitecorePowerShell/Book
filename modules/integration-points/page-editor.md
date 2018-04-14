# Page Editor

## Notification

The _Notification_ integration adds a notification to the Page Editor. The script is executed every time the page loads.

![A Notification for the current date](../../.gitbook/assets/notification-information.png)

**Example:** The following adds an information notification to the page for Sitecore 8 and a warning for Sitecore 7.

```text
$title = "Thank you for using SPE!"
$text = "Today is $([datetime]::Now.ToLongDateString())"
$icon = @{$true="Office/32x32/information.png";$false="Applications/16x16/warning.png"}[$SitecoreVersion.Major -gt 7]

$notification = New-Object -TypeName Sitecore.Pipelines.GetPageEditorNotifications.PageEditorNotification -ArgumentList $text, "Info"

$notification.Icon = $icon
$pipelineArgs.Notifications.Add($notification)
```

**Note:** Examples included in the following modules

* License Expiration

## Experience Button

The _Experience Button_ integration adds a button to the Page Editor. Rules can be used to control visiblity and enablement. The script is only executed when the button is clicked.

1. Begin by adding a new script to the _Experience Button_ library. The name of the script will appear in the tooltip.
2. Edit the script to perform the appropriate actions. The script can run in the background and show dialogs.
3. Change the icon of the item to match the script purpose.
4. Configure any rules as needed.

![An Experience Button for sending emails](../../.gitbook/assets/experience-button-with-tooltip.png)

**Example:** The following adds a command which will display a dialog asking a question, then send an email with the response.

```text
$item = Get-Item -Path .
$response = Show-Input -Prompt "What's the message for $($item.Name)?"
if($response) {
    $mailSettings = @{
        To = @("Michael West < mw@test.com >")
        From = "Console < noreply@test.com >"
        BodyAsHtml = $true
        SmtpServer = "localhost"
    }

    $subject = "Message sent regarding item $($item.Name) from $($SitecoreAuthority)"
    $response += "<br/>$($item.ItemPath)"
    Send-MailMessage @mailSettings -Subject $subject -Body $response
}

Close-Window
```

![Message Input](../../.gitbook/assets/message-input-with-ok-cancel.png)

![Email Response](../../.gitbook/assets/papercut-email-response.png)


# Receive-ItemCloneNotification

## Syntax

Receive-ItemCloneNotification \[-Notification &lt;Notification&gt;\] -Notification &lt;Notification&gt; -Action &lt;None \| Accept \| Reject \| Dismiss&gt; \[-NotificationType &lt;Notification \| ChildCreatedNotification \| FieldChangedNotification \| FirstVersionAddedNotification \| ItemMovedChildCreatedNotification \| ItemMovedChildRemovedNotification \| ItemMovedNotification \| ItemTreeMovedNotification \| ItemVersionNotification \| OriginalItemChangedTemplateNotification \| VersionAddedNotification&gt;\] \[-Language &lt;String\[\]&gt;\]

Receive-ItemCloneNotification \[-Item\] &lt;Item&gt; -Notification &lt;Notification&gt; -Action &lt;None \| Accept \| Reject \| Dismiss&gt; \[-NotificationType &lt;Notification \| ChildCreatedNotification \| FieldChangedNotification \| FirstVersionAddedNotification \| ItemMovedChildCreatedNotification \| ItemMovedChildRemovedNotification \| ItemMovedNotification \| ItemTreeMovedNotification \| ItemVersionNotification \| OriginalItemChangedTemplateNotification \| VersionAddedNotification&gt;\] \[-Language &lt;String\[\]&gt;\]

Receive-ItemCloneNotification \[-Path\] &lt;String&gt; -Notification &lt;Notification&gt; -Action &lt;None \| Accept \| Reject \| Dismiss&gt; \[-NotificationType &lt;Notification \| ChildCreatedNotification \| FieldChangedNotification \| FirstVersionAddedNotification \| ItemMovedChildCreatedNotification \| ItemMovedChildRemovedNotification \| ItemMovedNotification \| ItemTreeMovedNotification \| ItemVersionNotification \| OriginalItemChangedTemplateNotification \| VersionAddedNotification&gt;\] \[-Language &lt;String\[\]&gt;\]

Receive-ItemCloneNotification -Id &lt;String&gt; \[-Database &lt;String&gt;\] -Notification &lt;Notification&gt; -Action &lt;None \| Accept \| Reject \| Dismiss&gt; \[-NotificationType &lt;Notification \| ChildCreatedNotification \| FieldChangedNotification \| FirstVersionAddedNotification \| ItemMovedChildCreatedNotification \| ItemMovedChildRemovedNotification \| ItemMovedNotification \| ItemTreeMovedNotification \| ItemVersionNotification \| OriginalItemChangedTemplateNotification \| VersionAddedNotification&gt;\] \[-Language &lt;String\[\]&gt;\]

## Detailed Description

© 2010-2019 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions

## Parameters

### -Notification  &lt;Notification&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue\) |
| Accept Wildcard Characters? | false |

### -Action  &lt;NotificationAction&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -NotificationType  &lt;NotificationType&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Language  &lt;String\[\]&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Item  &lt;Item&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | true \(ByValue, ByPropertyName\) |
| Accept Wildcard Characters? | false |

### -Path  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | 1 |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Id  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | true |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |

### -Database  &lt;String&gt;

| Aliases |  |
| :--- | :--- |
| Required? | false |
| Position? | named |
| Default Value |  |
| Accept Pipeline Input? | false |
| Accept Wildcard Characters? | false |


# Working with Items

## Command Introduction

The majority of scripts written with SPE contain one or more of the following commands:

{% tabs %}
{% tab title="Get-Item" %}
* Use this to retrieve a single item. Throws an error if the item does not exist.
* Use when Sitecore `query:` or `fast:` is required. May return more than 1 item.

```text
Get-Item -Path "master:\content\home"
```
{% endtab %}

{% tab title="Get-ChildItem" %}
* Use to return an item's children and grandchildren.

```text
Get-ChildItem -Path "master:\content\home" -Recurse
```
{% endtab %}

{% tab title="New-Item" %}
* Use to create an item based on a specified data template.

```text
New-Item -Path "master:\content\home" -Name "Demo" -ItemType "Sample/Sample Item"
# or
New-Item -Path "master:\content\home" -Name "Demo" -ItemType "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
```
{% endtab %}

{% tab title="Remove-Item" %}
* Use to delete or recycle an item.
* Accepts items returned by `Get-Item` and `Get-ChildItem`.

```text
Get-Item -Path "master:\content\home\delete-me" | Remove-Item
```
{% endtab %}

{% tab title="Move-Item" %}
* Use to transfer an item from one location to another.

```text
Move-Item -Path "master:\content\home\Demo" -Destination "master:\content\home\Demo1"
```
{% endtab %}

{% tab title="Copy-Item" %}
* Use to duplicate an item from one location to another.

```text
Copy-Item -Path "master:\content\home\Demo1" -Destination "master:\content\home\Demo2"
```
{% endtab %}
{% endtabs %}

### Command Parameters

The following commands provide you with the core methods needed to manage your content. Due to the nature of Windows PowerShell, commands such as these are extended with custom parameters and switches using [Dynamic Parameters](https://msdn.microsoft.com/en-us/library/dd878299%28v=vs.85%29.aspx). These parameters are then added to the command at the time of use and only appear when the conditions are right. We've provided this table to help you discover the hidden gems within each command.

| **Parameter Name** | **Description** | **Copy-Item** | **Get-Item** | **Get-ChildItem** | **Move-Item** | **New-Item** | **Remove-Item** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AmbiguousPaths | More than one item matches the criteria so show them all. | – | ✓ | - | – | – | – |
| Database | The specified database will be used. Requires the ID to be set. | – | ✓ | – | – | – | – |
| DestinationItem | Parent item to receive the copied item. | ✓ | – | – | ✓ | – | – |
| FailSilently | Unauthorized access errors will be suppressed | ✓ | – | – | ✓ | – | ✓ |
| ForceId | Forces the new item to use a specified GUID | – | – | – | – | ✓ | – |
| ID | Matches the item by ID. | – | ✓ | ✓ | – | – | – |
| Item | Instance item. | ✓ | – | ✓ | ✓ | – | ✓ |
| Language | Specifies the languages to include. | – | ✓ | ✓ | – | ✓ | – |
| Parent | Specifies the parent item. | – | – | – | – | ✓ | – |
| Permanently | Specifies the item should be deleted rather than recycled. | – | – | – | – | – | ✓ |
| Query | Matches the items by an XPath query. | – | ✓ | – | – | – | – |
| StartWorkflow | Initiates the default workflow, if any. | – | – | – | – | ✓ | – |
| TransferOptions | Options flag used when copying from one database to another. | ✓ | – | – | ✓ | – | – |
| Uri | Matches the item by ItemUri. | – | ✓ | – | – | – | – |
| Version | Specifies the version to include. | – | ✓ | ✓ | – | – | – |

**Legend:** "–" - not applicable; "✓" - available.

Below we will show how to use each command with the Windows PowerShell syntax followed by some examples of the common C\# equivalent.

## Finding Items

If you have retrieved your items directly using the Sitecore API you can still add the nice wrapper. You can do that by piping them through the `Initialize-Item` command. We'll show an [example](./#sitecore-api) of this later.

{% hint style="info" %}
Check out some performance details when using different methods of querying items on the [Sitecore StackExchange](https://sitecore.stackexchange.com/questions/6803/sitecore-powershell-query-for-big-images-of-a-certain-size/6811#6811).
{% endhint %}

### Get-Item : by Path

**Example:** The following will retrieve the item based on the Sitecore path.

```text
PS master:\> Get-Item -Path master:\content\home

Name Children Languages                Id                                     TemplateName
---- -------- ---------                --                                     ------------
Home True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

As you may have noticed, the `/sitecore` portion of the path is unnecessary. This is because the _Sitecore_ item is represented by the root item of the drive `master:` and is therefore optional.

Let's have a look at the equivalent code in C\#.

```csharp
Sitecore.Data.Database master = Sitecore.Configuration.Factory.GetDatabase("master");
Sitecore.Data.Items.Item item = master.GetItem("/sitecore/content/home");
```

The above will return the latest version of the item in your current language. But what if you want the item in another language? No problem!

**Example:** The following will retrieve the Danish version of the _Home_ item.

```text
PS master:\> Get-Item -Path master:/content/home -Language da | Format-Table -Property DisplayName, Language, Id, Version, TemplateName

DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

I've formatted the output above to show you that indeed the right language was returned. The command supports wildcards for both `-Language` and `-Version` parameters. You may have also noticed that the forward and backward slashes can be used interchangeably.

**Example:** The following retrieves the latest version for all languages of an item.

```text
PS master:\> Get-Item -Path master:/content/home -Language * | Format-Table -Property DisplayName, Language, Id, Version, TemplateName

DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Home        en       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        de-DE    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        pl-PL    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 3       Sample Item
Home        en-GB    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

Notice that the item with language `en-US` at its third version.

**Example:** The following retrieves the item in all languages and versions.

```text
PS master:\> Get-Item -Path master:/content/home -Language * -Version *| Format-Table -Property DisplayName, Language, Id, Version, TemplateName

DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Home        en       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        de-DE    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        pl-PL    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 2       Sample Item
Home        en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 3       Sample Item
Home        en-GB    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

You can see that specifying the language and version using the wildcard will retrieve all possible variants of an item. The wildcard can also include a partial match like `en-*`. The use of that filter would return all items in the English language, ignoring the region.

### Get-ChildItem : by Path

**Example:** The following retrieves the child items in all languages and versions.

```text
PS master:\> Get-ChildItem -Path master:/content -Language * -Version * | Format-Table -Property DisplayName, Language, Id, Version, TemplateName

DisplayName         Language ID                                     Version TemplateName
-----------         -------- --                                     ------- ------------
Home                en       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home                de-DE    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home                pl-PL    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home                en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home                en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 2       Sample Item
Home                en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 3       Sample Item
Home                en-GB    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Hjem                da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
CognifideCom        en       {6A1EC18E-AF9B-443E-84C7-5528F2363A10} 1       TenantTemplate
Demo                en       {4F02AEDF-1CC6-4B84-8B6E-F5CB465F8AD9} 1       TenantTemplate
GetEngaged          en       {68AD4037-EE50-4615-BA2E-AE11B1D3F6CC} 1       TenantTemplate
GetEngaged          de-DE    {68AD4037-EE50-4615-BA2E-AE11B1D3F6CC} 1       TenantTemplate
GetEngaged          es-ES    {68AD4037-EE50-4615-BA2E-AE11B1D3F6CC} 1       TenantTemplate
GetEngaged          pt-BR    {68AD4037-EE50-4615-BA2E-AE11B1D3F6CC} 1       TenantTemplate
GetEngaged          pl-PL    {68AD4037-EE50-4615-BA2E-AE11B1D3F6CC} 1       TenantTemplate
```

### Get-Item : by Path with Sitecore query

It's not always most efficient to operate on items by traversing the tree using `Get-ChildItem`. This is especially true if you need to work on large trees but need to select only a few items \(e.g. a specific template\). For this we’ve introduced support for the Sitecore query within our provider.

> Important to note that the query format sometimes requires the use of a `#` before and after paths when they contain reserved keywords or spaces.
>
> Workaround found [here on Sitecore Stack Exchange](https://sitecore.stackexchange.com/questions/10127/how-to-escape-a-query-in-sitecore-powershell).

**Example:** The following retrieves all items beneath the path _/sitecore/content/_ with the template of _Sample Item_.

```text
PS master:\> Get-Item -Path master: -Query "/sitecore/content//*[@@templatename='Sample Item']"

Name                             Children Languages                Id                                     TemplateName
----                             -------- ---------                --                                     ------------
Copy of Home                     True     {en, de-DE, es-ES, pt... {503713E5-F9EE-4847-AEAF-DD13FD853004} Sample Item
Home                             True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

**Example:** The following retrieves all items beneath the path _/sitecore/content/_ with the template of _Sample Item_ in all versions and languages.

```text
PS master:\> Get-Item -Path master: -Query "/sitecore/content//*[@@templatename='Sample Item']" -Language * -Version * | Format-Table -Property DisplayName, Language, Id, Version, TemplateName -AutoSize

DisplayName  Language ID                                     Version TemplateName
-----------  -------- --                                     ------- ------------
Copy of Home en       {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Home         de-DE    {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Copy of Home pl-PL    {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Copy of Home en-US    {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Hjem         da       {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Home         en       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home         de-DE    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home         pl-PL    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home         en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home         en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 2       Sample Item
Home         en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 3       Sample Item
Home         en-GB    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Hjem         da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

**Example:** The following retrieves items matching the query with a specified ISO date and present in a report dialog.

```text
$isoDate = [Sitecore.DateUtil]::ToIsoDate([datetime]::Today)
Get-Item -Path master: -Query "/sitecore/content/home//*[@__Publish > '$($isoDate)']" | 
    Show-ListView -Property ID,Name,ItemPath
```

### Get-Item : by XPath

The setup is not exactly like you would find in the XPath Builder but should get the job done. Read more about it [here](https://github.com/SitecorePowerShell/Console/issues/1069).

**Example:** The following retrieves items matching an XPath query.

```text
$query = "ancestor-or-self::*[@@templatename='Sample Item']"

# Retrieve the items with Axes and a given context item.
$SitecoreContextItem.Axes.SelectItems($query)

# Retrieve the items using the Query class and context item.
# Retrieve the items by prepending the context path to the query.
Get-Item -Path "master:" -Query ("$($SitecoreContextItem.Paths.Path)/$query")
```

### Get-Item : by Id

**Example:** The following retrieves an item by id.

```text
PS master:\> Get-Item -Path master: -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"

Name  Children Languages                Id                                     TemplateName
----  -------- ---------                --                                     ------------
Home  True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

### Get-Item : by Uri

The Uri encodes the language and version within the path.

**Example:** The following retrieves an item by uri.

```text
PS master:\> Get-Item -Path master: -Uri "sitecore://master/{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}?lang=en&ver=1"

Name Children Languages                Id                                     TemplateName
---- -------- ---------                --                                     ------------
Home True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

In all the examples you'll notice we specified the database. Windows PowerShell needs to know which provider to execute within. This also signals to SPE to show the dynamic parameters. Other examples of providers include the following:

* **HKLM:** - The registry provider for HKEY\_LOCAL\_MACHINE.
* **C:** - The filesystem provider for the C drive.

### Get-Item : select properties

The following examples make use of custom \_PropertySet\_s for the command `Select-Object`.

**Example:** The following uses the **PSSecurity** _PropertySet_.

```text
PS master:\ >Get-Item -Path "master:\content\home" | Select-Object -Property PSSecurity

Name ID                                     __Owner        __Security
---- --                                     -------        ----------
Home {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} sitecore\admin au|sitecore\michael|pe|+item:read|
```

**Example:** The following uses the **PSTemplate** _PropertySet_.

```text
PS master:\> Get-Item -Path "/sitecore/media library/Images/SPE/kitten1" | Select-Object -Property PSTemplate

Name    ID                                     BaseTemplate
----    --                                     ------------
kitten1 {E58FA823-3CAF-43A1-A5ED-FBE24D3C21B4} {Image, File, Standard template, Media classification...}
```

**Example:** The following uses the **PSImage** _PropertySet_.

```text
PS master:\> Get-Item -Path "/sitecore/media library/Images/SPE/kitten1" | Select-Object -Property PSImage

Name      : kitten1
ID        : {E58FA823-3CAF-43A1-A5ED-FBE24D3C21B4}
Alt       : Yay
Width     : 225
Height    : 225
Extension : jpg
Size      : 6593
```

**Example:** The following uses the **PSSchedule** _PropertySet_.

```text
PS master:\> Get-Item -Path "/sitecore/system/Tasks/Schedules/Content Testing/Calculate Statistical Relevancy" | Select-Object -Property PSSchedule 

Name     : Calculate Statistical Relevancy
ID       : {C7533E65-A1D6-4F99-9F12-0AB157299D80}
Schedule : 1900101|19000101|127|1.00:00
Last run : 1/1/0001 12:00:00 AM
Command  : {6A79C206-0CD2-4DDD-9DFF-5BF21E002931}
Items    :
```

### Get-Item : properties with field type

**Example:** The following accesses the _Image_ field casted to the type `Sitecore.Data.Fields.ImageField`.

```text
$item = Get-Item -Path "master:\content\home"
$item._.Image.Alt
```

**Note:** You can use `._` and `.PSFields` to gain access to the typed field.

**Example:** The following accesses the _Link_ field casted to the type `Sitecore.Data.Fields.LinkField`. From there you can see all of the available properties.

```text
PS master:\> $currentItem = Get-Item -Path 'master:\content\home\sample-item'
PS master:\> $currentItem.PSFields."LinkFieldName"

Anchor       :
Class        :
InternalPath : /sitecore/content/home/sample-item/
IsInternal   : True
IsMediaLink  : False
LinkType     : internal
MediaPath    :
QueryString  :
Target       :
TargetID     : {263293D3-B1B3-4C2C-9A75-6BD418F376BC}
TargetItem   : Sitecore.Data.Items.Item
Text         : CLICK HERE
Title        :
Url          :
Root         : link
Xml          : #document
InnerField   : <link linktype="internal" text="CLICK HERE" querystring="" target="" id="{263293D3-B1B3-4C2C-9A75-6BD418F376BC}" />
Value        : <link linktype="internal" text="CLICK HERE" querystring="" target="" id="{263293D3-B1B3-4C2C-9A75-6BD418F376BC}" />
```

**Example:** The following finds all of the `TextField`s and outputs to the console.

```text
$item = Get-Item -Path "master:\content\home"
foreach($field in $item.Fields) {
    $item.PSFields."$($field.Name)" | Where-Object { $_ -is [Sitecore.Data.Fields.TextField] }
}
```

### Get-Item : then change item properties

We often see the following two ways of accessing and changing fields used in scripts. One uses `Set-ItemProperty` and the other is more natural to a Sitecore developer.

**Example:** The following sets the title property using `Set-ItemProperty`.

```text
PS master:\> Set-ItemProperty -Path master:/content/home -Name "Title" -Value "New Title"
```

**Example:** The following sets the title and clears the branch id using `.Editing.BeginEdit` and `.Editing.EndEdit` methods.

```text
$item = Get-Item master:/content/home
$item.Editing.BeginEdit()
$item["Title"] = "New Title"
$item.BranchId = [Guid]::Empty # or a new value if changing it
$item.Editing.EndEdit()
```

**Note:** The above example may also be written in the ISE where no console prompt is visible.

The previous examples work but are not the most efficient ways to change item content. The items returned by the provider expose the Sitecore item fields as automatic PowerShell properties.

{% hint style="info" %}
If the property name on the data template contains a space, such as \`Closing Date\`, then you will need to wrap the field name in quotes \(single or double\).
{% endhint %}

**Example:** The following sets the title property using the automated PowerShell property.

```text
$item = Get-Item -Path master:/content/home
$item.Title = "New Title"
$item."Closing Date" = [datetime]::Today
```

**Example:** The following sets the title property using the semi-native PowerShell property without the use of a variable.

```text
(Get-Item -Path master:/content/home).Title = "New Title"
```

This technique may be used for a wide variety of property types. There are a other hidden gems in those properties. For example if we detect that the field is a _Date_ or _Datetime_ field, we will return `System.DateTime` typed value from a field rather than the `System.String` Sitecore stores internally.

**Example:** The following gets the created date.

```text
PS master:\> (Get-Item -Path master:/content/home).__Created
Monday, April 07, 2008 1:59:00 PM
```

**Example:** The following assigns a `System.DateTime` value to the PowerShell automated property.

```text
PS master:\> (Get-Item -Path master:/content/home).__Created = [DateTime]::Now
PS master:\> (Get-Item -Path master:/content/home).__Created
Monday, October 13, 2014 1:59:41 AM
```

Great we've just changed it! Our property handlers take care of all the necessary usages of `.Editing.BeginEdit` and `.Editing.EndEdit`. This method can be applied for a variety of field types such as _GeneralLink_ and _Image_.

To provide an example – I’ve extended my home with additional fields as follows:

![Extended Sample Item](https://blog.najmanowicz.com/wp-content/uploads/2014/10/image.png)

**Example:** The following assigns an image to the Image field.

```text
$homeItem = Get-Item -Path "master:/content/home"
$homeItem.Image = Get-Item -Path "master:\media library\logo"
```

Easy enough, isn't it? Let SPE detect the field type for you and worry about what to call! Now let's assign a content item to _GeneralLink_.

**Example:** The following assigns a content item to a _GeneralLink_ field.

```text
$homeItem = Get-Item -Path "master:/content/home"
$homeItem.GeneralLink = Get-Item -Path "master:\content\CognifideCom"
```

What about fields that accept lists of items? We've got your back here as well.

**Example:** The following assigns all children of `/sitecore/content/` item to the _ItemList_ field.

```text
$homeItem = Get-Item -Path "master:/content/home"
$homeItem.ItemList = Get-ChildItem -Path 'master:\content\'
```

Let's see how our item looks in the Content editor after all the assignments that we've just performed:

![ItemList Assignment](https://blog.najmanowicz.com/wp-content/uploads/2014/10/image1.png)

Great! Looks like it worked.

Those little improvements make your scripts much more succinct and understandable. Try it for yourself!

#### When not to use the automated properties?

As with every rule there is an exception to this one. Those automated properties perform the `$item.Editing.BeginEdit()` and `$item.Editing.EndEdit()` every time which results in saving the item after every assignment. Assigning multiple properties on an item this way might be detrimental to the performance of your script. In such cases you might want to call `$item.Editing.BeginEdit()` yourself before modifying the item. Subsequently call the `$item["field name"] = "new value"` for each property modify. Finally end with the `$item.Editing.EndEdit()`.

Choosing this way is situational and will usually only be required if you're working with a large volume of data. In those cases you might also want to introduce the `Sitecore.Data.BulkUpdateContext` trick used in [this blog post](https://bartlomiejmucha.com/en/blog).

**Example:** The following sets multiple automated properties while using the `Sitecore.Data.BulkUpdateContext`.

```text
New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
    foreach($item in Get-ChildItem -Path "master:\content\home") {
        $item.Editing.BeginEdit()
        $item["Title"] = "Sample Item"
        $item["Text"] = "Sample Item"
        $item.Editing.EndEdit() > $null
    }
}
```

Example: The following generates a relative url to the specified site and assigns to a variable. Because the `New-UsingBlock` command creates a new closure, you need to return the data to use in a different scope.

```text
$site = [Sitecore.Sites.SiteContextFactory]::GetSiteContext("usa")
$relativeUrl = New-UsingBlock (New-Object Sitecore.Sites.SiteContextSwitcher $site) {
    $pageItem = Get-Item -Path "master:" -Id "{50BE527C-7241-4613-A7A9-20D0217B264B}"
    [Sitecore.Links.LinkManager]::GetItemUrl($pageItem)
}
```

Some other classes you may want to use with the `New-UsingBlock` function:

* `Sitecore.SecurityModel.SecurityDisabler`
* `Sitecore.Data.BulkUpdateContext`
* `Sitecore.Globalization.LanguageSwitcher`
* `Sitecore.Sites.SiteContextSwitcher`
* `Sitecore.Data.DatabaseSwitcher`
* `Sitecore.Security.Accounts.UserSwitcher`
* `Sitecore.Data.Items.EditContext`
* `Sitecore.Data.Proxies.ProxyDisabler`
* `Sitecore.Data.DatabaseCacheDisabler`
* `Sitecore.Data.Events.EventDisabler`

### Sitecore API

If you have reached this point, then you are clearly a nerd and want to access using the raw Sitecore API.

**Example:** The following queries all descendants of the media library, filters by size, and wraps with automatic properties.

```text
# Get the root node using Get-Item, then a call to Axes.
$mediaItemContainer = Get-Item -Path "master:/media library"
$items = $mediaItemContainer.Axes.GetDescendants() | 
    Where-Object { [int]$_.Fields["Size"].Value -gt 100000 } | Initialize-Item
```

## Copying Items

### Copy-Item : to a new destination

You will find yourself one day in need of copying items on a small to large scale. The `Copy-Item` command will likely meet the need.

**Example:** The following copies the item to the specified path with a new ID.

```text
Copy-Item -Path "master:\content\home\Sample Item\Sample Item 1" -Destination "master:\content\home\Sample Item\Sample Item 2"
```

**Note:** The item name will match just as you type it in the command. Lowercase name in the destination will result in an item with a lowercase name.

**Example:** The following transfers the item to the specified path with the same ID.

```text
Copy-Item -Path master:\content\Home -Destination web:\content\home -TransferOptions 0
```

## Moving Items

### Move-Item : to a new destination

There is a always a better way to do something. Moving items en masse is certainly one that you don't want to do by hand. If the destination item exists the moved item will be added as a child. If the destination item does not exist the source item will be renamed when moved.

**Example:** The following moves the item from one parent to another.

```text
Move-Item -Path "master:\content\home\sample item\Sample Item 1" -Destination "master:\content\home\sample item 2\"
```

Example: The following gets an item and moves to a new parent node, along with all the children.

```text
Get-Item -Path "master:" -ID "{65736CA0-7D69-452A-A16F-2F42264D21C5}" | 
    Move-Item -Destination "master:{DFDDF372-3AB7-45B1-9E7C-0D0B27350439}"
```

## Creating Items

### New-Item

**Example:** The following creates a new item with the specified template.

```text
New-Item -Path "master:\content\home\sample item\Sample Item 3" -ItemType "Sample/Sample Item"

Name                             Children Languages                Id                                     TemplateName
----                             -------- ---------                --                                     ------------
Sample Item 3                    False    {en}                     {F6F4F7B7-5E72-4C16-9294-218D80ED89E8} Sample Item
```

**Example:** The following creates a new item with the specified template id and id.

```text
New-Item -Path "master:\content\home\sample item\Sample Item 4" -ItemType "{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}" -ForceId "{9459ADDD-4471-4ED3-A041-D33E559BD321}"

Name                             Children Languages                Id                                     TemplateName
----                             -------- ---------                --                                     ------------
Sample Item 4                    False    {en}                     {9459ADDD-4471-4ED3-A041-D33E559BD321} Sample Item
```

## Removing Items

### Remove-Item : permanently

**Example:** The following removes the item permanently. Proceed with caution.

```text
Remove-Item -Path "master:\content\home\sample item\Sample Item 3" -Permanently
```

### References

* [Working with Sitecore Items in PowerShell Extensions](https://blog.najmanowicz.com/2014/10/12/working-with-sitecore-items-in-powershell-extensions/)


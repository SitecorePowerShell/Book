# Working with Sitecore items

#### How do I retrieve my Sitecore items the PowerShell way?

The following are the primary commands we use to manage Sitecore items:

|  | Copy-Item | Get-Item | Get-ChildItem |
| -- | -- | -- | -- |
| FailSilently | &#x2713; | &#8211; | 3:2 |
| Query | &#8211; | &#x2713; | 3:3 |
| Language | &#8211; | &#x2713; | 3:4 |
| Version | &#8211; | &#x2713; | 3:5 |
| StartWorkflow | &#8211; | &#8211; | 3:6 |
| Permanently | &#8211; | &#8211; | 3:7 |
| Item | &#x2713; | &#8211; | |
| DestinationItem | &#x2713; | &#8211; | |
| ID | &#8211; | &#x2713; | |
| Database | &#8211; | &#x2713; | |
| Uri | &#8211; | &#x2713; | |
| Parent | &#8211; | &#8211; | |
| AmbiguouisPaths | &#8211; | &#x2713; | |
| TransferOptions | &#x2713; | &#8211; | |

**Legend:** "&#8211;" - not supported; "&#x2713;" - supported.


* `Copy-Item`
* `Get-Item` - Returns a single item at the specified path.
* `Get-ChildItem` - Returns one or more child items from the specified path.
* `Move-Item`
* `New-Item`
* `Remove-Item`
* `Rename-Item`

Below we will show how to use each command with the Windows PowerShell syntax followed by the common C# equivalent.

If you have retrieved your items directly using the Sitecore API you can still add the nice wrapper. You can do that by piping them through the `Initialize-Item` command.

#### Get items by Path

**Example:** The following will retrieve the item based on the Sitecore path.

```powershell
PS master:\> Get-Item -Path master:\content\home
 
Name Children Languages                Id                                     TemplateName
---- -------- ---------                --                                     ------------
Home True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

As you may have noticed, the `/sitecore` portion of the path is unnecessary. This is because the *Sitecore* item is represented by the root item of the drive `master:` and is therefore optional.

Let's have a look at the equivalent code in C#.

```csharp
Sitecore.Data.Database master = Sitecore.Configuration.Factory.GetDatabase("master");
Sitecore.Data.Items.Item item = master.GetItem("/sitecore/content/home");
```

The above will return the latest version of the item in your current language. But what if you want the item in another language? No problem!

**Example:** The following will retrieve the Danish version of the *Home* item.

```powershell
PS master:\> Get-Item -Path master:/content/home -Language da | Format-Table DisplayName, Language, Id, Version, TemplateName

DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

I've formatted the output above to show you that indeed the right language was returned. The command supports wildcards for both `-Language` and `-Version` parameters. You may have also noticed that the forward and backward slashes can be used interchangeably. 

**Example:** The following retrieves the latest version for all languages of an item.

```powershell
PS master:\> Get-Item master:/content/home -Language * | Format-Table DisplayName, Language, Id, Version, TemplateName
  
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

```powershell
PS master:\> Get-Item master:/content/home -Language * -Version *| Format-Table DisplayName, Language, Id, Version, TemplateName
  
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

**Example:** The following retrieves the child items in all languages and versions.

```powershell
PS master:\> Get-ChildItem master:/content -Language * -Version * | Format-Table DisplayName, Language, Id, Version, TemplateName
  
DisplayName         Language ID                                     Version TemplateName
-----------         -------- --                                     ------- ------------
Copy of Home        en       {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Home                de-DE    {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Copy of Home        pl-PL    {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Copy of Home        en-US    {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
Hjem                da       {503713E5-F9EE-4847-AEAF-DD13FD853004} 1       Sample Item
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
RetailTheatre       en       {436460CF-33E2-47C0-90B5-F856630266E3} 1       Node
Showcase            en       {DB8C05B8-25B5-42DE-B6CB-4ACE186283DA} 1       TenantTemplate
Zengage             en       {D55FE1D5-1CAC-4A2E-9DFE-D624D0F51886} 1       TenantTemplate
```

#### Getting items by Path with Sitecore query

It's not always most efficient to operate on items by traversing the tree using `Get-ChildItem`. This is especially true if you need to work on large trees but need to select only a few items (e.g. a specific template). For this we’ve introduced support for the Sitecore query within our provider. 

**Example:** The following retrieves all items beneath the path */sitecore/content/* with the template of *Sample Item*.

```powershell
PS master:\> Get-Item -Path master: -Query "/sitecore/content//*[@@templatename='Sample Item']"
  
Name                             Children Languages                Id                                     TemplateName
----                             -------- ---------                --                                     ------------
Copy of Home                     True     {en, de-DE, es-ES, pt... {503713E5-F9EE-4847-AEAF-DD13FD853004} Sample Item
Home                             True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

**Example:** The following retrieves all items beneath the path */sitecore/content/* with the template of *Sample Item* in all versions and languages.

```powershell
PS master:\> Get-Item -Path master: -Query "/sitecore/content//*[@@templatename='Sample Item']" -Language * -Version * | Format-Table DisplayName, Language, Id, Version, TemplateName -AutoSize
  
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

####  Get items by Id

**Example:** The following retrieves an item by id.

```powershell
PS master:\> Get-Item -Path master: -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"
  
Name  Children Languages                Id                                     TemplateName
----  -------- ---------                --                                     ------------
Home  True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

#### Get items by Uri

The Uri encodes the language and version within the path.

**Example:** The following retrieves an item by uri.

```powershell
PS master:\> Get-Item -Path master: -Uri "sitecore://master/{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}?lang=en&ver=1"
  
Name Children Languages                Id                                     TemplateName
---- -------- ---------                --                                     ------------
Home True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

In all the examples you'll notice we specified the database. Windows PowerShell needs to know which provider to execute within. Other examples of providers include the following:
* **HKLM:** - The registry provider for HKEY_LOCAL_MACHINE.
* **C:** - The filesystem provider for the C drive.

#### Changing item properties

We often see the following two ways of accessing and changing fields used in scripts. One uses `Set-ItemProperty` and the other is more natural to a Sitecore developer.

**Example:** The following sets the title property using `Set-ItemProperty`.

```powershell
PS master:\> Set-ItemProperty -Path master:/content/home -Name "Title" -Value "New Title"
```

**Example:** The following sets the title property using `.Editing.BeginEdit` and `.Editing.EndEdit` methods.

```powershell
$item = Get-Item master:/content/home
$item.Editing.BeginEdit()
$item["Title"] = "New Title"
$item.Editing.EndEdit()
```
**Note:** The above example may also be written in the ISE where no console prompt is visible.

The previous examples work but are not the most efficient ways to change item content. The items returned by the provider expose the Sitecore item fields as automated PowerShell properties.

**Example:** The following sets the title property using the automated PowerShell property.

```powershell
$item = Get-Item master:/content/home
$item.Title = "New Title"
```

**Example:** The following sets the title property using the semi-native PowerShell property without the use of a variable.

```powershell
(Get-Item master:/content/home).Title = "New Title"
```

This technique may be used for a wide variety of property types. There are a other hidden gems in those properties. For example if we detect that the field is a *Date* or *Datetime* field, we will return `System.DateTime` typed value from a field rather than the `System.String` Sitecore stores internally.

**Example:** The following gets the created date.

```powershell
PS master:\> (Get-Item master:/content/home).__Created
Monday, April 07, 2008 1:59:00 PM
```

**Example:** The following assigns a `System.DateTime` value to the PowerShell automated property.

```powershell
PS master:\> (Get-Item master:/content/home).__Created = [DateTime]::Now
PS master:\> (Get-Item master:/content/home).__Created
Monday, October 13, 2014 1:59:41 AM
```

Great we've just changed it! Our property handlers take care of all the necessary usages of `.Editing.BeginEdit` and `.Editing.EndEdit`. This method can be applied for a variety of field types such as *GeneralLink* and *Image*.

To provide an example – I’ve extended my home with additional fields as follows:

![Extended Sample Item](http://blog.najmanowicz.com/wp-content/uploads/2014/10/image.png)

**Example:** The following assigns an image to the Image field.

```powershell
(Get-Item master:/content/home).Image = Get-Item 'master:\media library\logo'
```

Easy enough, isn't it? Let SPE detect the field type for you and worry about what to call! Now let's assign a content item to *GeneralLink*.

**Example:** The following assigns a content item to a *GeneralLink* field.

```powershell
(Get-Item master:/content/home).GeneralLink = Get-Item 'master:\content\CognifideCom'
```

What about fields that accept lists of items? We've got your back here as well.

**Example:** The following assigns all children of `/sitecore/content/` item to the *ItemList* field.

```powershell
(Get-Item master:/content/home).ItemList = Get-ChildItem 'master:\content\'
```

Let's see how our item looks in the Content editor after all the assignments that we've just performed:

![ItemList Assignment](http://blog.najmanowicz.com/wp-content/uploads/2014/10/image1.png)

Great! Looks like it worked.

Those little improvements make your scripts much more succinct and understandable. Try it for yourself!

#### When not to use the automated properties?

As with every rule there is an exception to this one. Those automated properties perform the `$item.Editing.BeginEdit()` and `$item.Editing.EndEdit()` every time  which results in saving the item after every assignment. Assigning multiple properties on an item this way might be detrimental to the performance of your script. In such cases you might want to call `$item.Editing.BeginEdit()` yourself before modifying the item. Subsequently call the `$item["field name"] = "new value"` for each property modify. Finally end with the `$item.Editing.EndEdit()`. 

Choosing this way is situational and will usually only be required if you're working with a large volume of data. In those cases you might also want to introduce the `Sitecore.Data.BulkUpdateContext` trick used in [this blog post](http://bartlomiejmucha.com/en/blog).

Example: The following sets multiple automated properties while using the `Sitecore.Data.BulkUpdateContext`.

```powershell
Import-Function -Name New-UsingBlock

New-UsingBlock (New-Object Sitecore.Data.BulkUpdateContext) {
    foreach($item in Get-ChildItem -Path "master:\content\home") {
        $item.Editing.BeginEdit()
        $item["Title"] = "Sample Item"
        $item["Text"] = "Sample Item"
        $item.Editing.EndEdit() | Out-Null
    }
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
#### References

* [Working with Sitecore Items in PowerShell Extensions](http://blog.najmanowicz.com/2014/10/12/working-with-sitecore-items-in-powershell-extensions/)
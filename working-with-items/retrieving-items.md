# Retrieving Items

This page covers all the methods for finding and retrieving Sitecore items using SPE. Whether you need a single item or a collection, SPE provides flexible query options to meet your needs.

## Core Cmdlets

### Get-Item

Use `Get-Item` to retrieve a single item or items matching a query. This cmdlet throws an error if the item doesn't exist (unless using queries that may return zero results).

**Key uses:**

- Retrieve a single item by path
- Query items using Sitecore query
- Get items by ID or URI

### Get-ChildItem

Use `Get-ChildItem` to retrieve children and descendants of an item. This cmdlet is ideal for traversing content trees.

**Key uses:**

- List immediate children
- Recursively traverse item trees
- Filter by template or other properties

## Dynamic Parameters

SPE extends the standard PowerShell cmdlets with dynamic parameters. These appear when you specify a Sitecore database path:

| Parameter      | Description                                | Available On            | Example                                         |
| -------------- | ------------------------------------------ | ----------------------- | ----------------------------------------------- |
| AmbiguousPaths | Show all items matching ambiguous criteria | Get-Item                | `-AmbiguousPaths`                               |
| Database       | Specify database when using ID             | Get-Item                | `-Database "master"`                            |
| ID             | Match item by GUID                         | Get-Item, Get-ChildItem | `-ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"`  |
| Language       | Specify language versions                  | Get-Item, Get-ChildItem | `-Language "en-US"` or `-Language *`            |
| Query          | Execute Sitecore query                     | Get-Item                | `-Query "/sitecore/content//*"`                 |
| Uri            | Match item by ItemUri                      | Get-Item                | `-Uri "sitecore://master/{GUID}?lang=en&ver=1"` |
| Version        | Specify item versions                      | Get-Item, Get-ChildItem | `-Version 2` or `-Version *`                    |
| WithParent     | Include parent in results                  | Get-ChildItem           | `-WithParent`                                   |

## Retrieving by Path

The most common way to retrieve items is by their Sitecore path.

**Example:** Retrieve an item by path.

```powershell
Get-Item -Path "master:\content\home"
```

**Output:**

```powershell
Name Children Language Version Id                                     TemplateName
---- -------- -------- ------- --                                     ------------
Home True     en       2       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

{% hint style="info" %}
The `/sitecore` portion of the path is optional. Both `master:\content\home` and `master:\sitecore\content\home` work identically.
{% endhint %}

**Example:** The C# equivalent for reference.

```csharp
Sitecore.Data.Database master = Sitecore.Configuration.Factory.GetDatabase("master");
Sitecore.Data.Items.Item item = master.GetItem("/sitecore/content/home");
```

## Working with Languages

Retrieve items in specific languages or all languages using the `-Language` parameter with wildcards.

{% hint style="warning" %}
When working with multi-language sites, you may find that specifying the `-Language` parameter results in a more consistent behavior.
{% endhint %}

**Example:** Retrieve the Danish version of an item.

```powershell
Get-Item -Path "master:\content\home" -Language "da" |
    Format-Table DisplayName, Language, Id, Version, TemplateName
```

**Output:**

```powershell
DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

**Example:** Retrieve latest version for all languages.

```powershell
Get-Item -Path "master:\content\home" -Language * |
    Format-Table DisplayName, Language, Id, Version, TemplateName
```

**Output:**

```powershell
DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Home        en       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        de-DE    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        pl-PL    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 3       Sample Item
Home        en-GB    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

## Working with Versions

Retrieve specific versions or all versions using the `-Version` parameter.

**Example:** Retrieve all versions in all languages.

```powershell
Get-Item -Path "master:\content\home" -Language * -Version * |
    Format-Table DisplayName, Language, Id, Version, TemplateName
```

**Output:**

```powershell
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

**Example:** Using wildcards for partial matches (all English variants).

```powershell
Get-Item -Path "master:\content\home" -Language "en","en-*"
```

## Retrieving Children

Use `Get-ChildItem` to retrieve an item's children, with optional recursion.

**Example:** Retrieve immediate children only.

```powershell
Get-ChildItem -Path "master:\content"
```

**Example:** Retrieve all descendants recursively.

```powershell
Get-ChildItem -Path "master:\content\home" -Recurse
```

**Example:** Retrieve children in all languages and versions.

```powershell
Get-ChildItem -Path "master:\content" -Language * -Version * |
    Format-Table DisplayName, Language, Id, Version, TemplateName
```

## Retrieving by ID

Use the `-ID` parameter to retrieve items by their GUID. This is useful when you know the item ID but not its path.

**Example:** Retrieve an item by ID.

```powershell
Get-Item -Path "master:" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}" -Language *
```

**Output:**

```powershell
Name Children Language Version Id                                     TemplateName
---- -------- -------- ------- --                                     ------------
Home True     en       3       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
Home True     en-CA    2       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

{% hint style="info" %}
When using `-ID`, you must specify the database path (e.g., `master:`) to activate the dynamic parameter.
{% endhint %}

## Retrieving by URI

ItemUri encodes the database, item ID, language, and version in a single string.

**Example:** Retrieve an item by URI.

```powershell
Get-Item -Path "master:" -Uri "sitecore://master/{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}?lang=en&ver=1"
```

**Output:**

```powershell
Name Children Language Version Id                                     TemplateName
---- -------- -------- ------- --                                     ------------
Home True     en       1       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

## Sitecore Query

Use Sitecore query to find items matching complex criteria. This is more efficient than traversing the tree when you need specific items.

{% hint style="warning" %}
Sitecore query paths containing reserved keywords or spaces require `#` delimiters: `#path with spaces#`
{% endhint %}

**Example:** Find all items with a specific template.

```powershell
Get-Item -Path "master:" -Query "/sitecore/content//*[@@templatename='Sample Item']"
```

**Output:**

```powershell
Name          Children Language Version Id                                     TemplateName
----          -------- -------- ------- --                                     ------------
Home          True     en       3       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
Contact       False    en       1       {357D85DB-4F6C-42AF-B9A0-C232FD86F679} Sample Item
History       False    en       1       {49D54DA4-EEB9-463B-AB19-BD8647270442} Sample Item
Team          False    en       1       {2E4F7BBB-C71C-4CBC-BBA1-B4C05F946138} Sample Item
BulkItem1     False    en       1       {81E660ED-0207-4C1F-BCEF-DAD9286B4E02} Sample Item
```

**Example:** Query with language and version wildcards.

```powershell
Get-Item -Path "master:" -Query "/sitecore/content//*[@@templatename='Sample Item']" -Language * -Version * |
    Format-Table DisplayName, Language, Id, Version, TemplateName -AutoSize
```

**Example:** Query using dates (ISO format required) filtering by the publishing restriction where the item (not version) has a future date.

```powershell
$isoDate = [Sitecore.DateUtil]::ToIsoDate([datetime]::Today)
Get-Item -Path "master:" -Query "/sitecore/content/home//*[@__Publish > '$($isoDate)']" |
    Show-ListView -Property ID, Name, ItemPath
```

## Fast Query

{% hint style="warning" %}
Sitecore has deprecated this functionality in XM/XP 10.1.
{% endhint %}

## XPath with Axes

For complex queries using XPath axes, use the Sitecore API directly or prepend the context path.

**Example:** Use XPath axes to find ancestors.

```powershell
$query = "ancestor-or-self::*[@@templatename='Sample Item']"

# Option 1: Use Axes directly on an item
$SitecoreContextItem.Axes.SelectItems($query)

# Option 2: Prepend the context path to the query
Get-Item -Path "master:" -Query "$($SitecoreContextItem.Paths.Path)/$query"
```

## Custom Property Sets

Use custom property sets with `Select-Object` to retrieve specific groups of properties.

**Example:** Retrieve security properties using PSSecurity.

```powershell
Get-Item -Path "master:\content\home" | Select-Object -Property PSSecurity
```

**Output:**

```powershell
Name ID                                     __Owner        __Security
---- --                                     -------        ----------
Home {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} sitecore\Admin ar|sitecore\Developer|pd|+item:read|pe|+item:read|
```

{% hint style="info" %}
Check out [Item Security](item-security.md) to learn about managing the security settings using SPE.
{% endhint %}

**Example:** Retrieve template information using PSTemplate.

```powershell
Get-Item -Path "master:" -ID "{04DAD0FD-DB66-4070-881F-17264CA257E1}" | Select-Object -Property PSTemplate
```

**Output:**

```powershell
Name  ID                                     BaseTemplate
----  --                                     ------------
cover {04DAD0FD-DB66-4070-881F-17264CA257E1} {Image, File, Standard template, Advanced...}
```

**Example:** Retrieve image metadata using PSImage.

```powershell
Get-Item -Path "master:\media library\Default Website\cover" | Select-Object -Property PSImage
```

**Output:**

```powershell
Name      : cover
ID        : {04DAD0FD-DB66-4070-881F-17264CA257E1}
Alt       :
Width     : 1600
Height    : 550
Extension : jpg
Size      : 119719
```

**Example:** Retrieve schedule information using PSSchedule.

```powershell
Get-Item -Path "master:\system\Tasks\Schedules\Forms\FileStorageCleanup" |
    Select-Object -Property PSSchedule
```

**Output:**

```powershell
Name     : FileStorageCleanup
ID       : {3D8F6795-1C4E-462D-8A81-BE27B1AEC5BD}
Schedule : 20190101|99990101|127|24:00:00
Last run : 11/29/2025 4:46:29 AM
Command  : {D4ADAD17-E648-4FDC-BC4C-43F9D1513720}
Items    :
```

## Using Initialize-Item

If you retrieve items using the Sitecore API directly, wrap them with `Initialize-Item` to add SPE's automatic properties.

**Example:** Query using Sitecore API and wrap with SPE properties.

```powershell
# Get the root node using Get-Item, then call Axes
$mediaItemContainer = Get-Item -Path "master:\media library"
$items = $mediaItemContainer.Axes.GetDescendants() |
    Where-Object { [int]$_.Fields["Size"].Value -gt 100000 } |
    Initialize-Item
```

## Common Retrieval Patterns

### Pattern: Find Items by Template

```powershell
# Using Get-ChildItem with filtering
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.TemplateName -eq "Sample Item" }

# Using Sitecore query (more efficient for large trees)
Get-Item -Path "master:" -Query "/sitecore/content/home//*[@@templatename='Sample Item']"
```

### Pattern: Find Recently Modified Items

```powershell
$cutoffDate = (Get-Date).AddDays(-7)
Get-ChildItem -Path "master:\content" -Recurse |
    Where-Object { $_.__Updated -gt $cutoffDate }
```

### Pattern: Find Items by Field Value

```powershell
# Using Where-Object
Get-ChildItem -Path "master:\content\home" -Recurse |
    Where-Object { $_.Title -like "*Welcome*" }

# Using Sitecore query
Get-Item -Path "master:" -Query "/sitecore/content/home//*[contains(@Title, 'Welcome')]"
```

### Pattern: Get All Media Items Above Size Threshold

```powershell
Get-ChildItem -Path "master:\media library" -Recurse |
    Where-Object { $_.Size -gt 1MB } |
    Select-Object Name, Size, Extension, ItemPath
```

## Performance Tips

- **Use Sitecore query** when you need specific items from a large tree
- **Use Get-ChildItem** for small trees or when you need all items
- **Filter early** - use query predicates instead of Where-Object when possible
- **Limit language/version retrieval** - only use `-Language *` and `-Version *` when necessary

## See Also

- [Editing Items](editing-items.md) - Update item properties and fields
- [Best Practices](best-practices.md) - Performance optimization guidance
- [Appendix - Common Commands](../appendix/common/README.md) - Full cmdlet reference

## References

- [Performance details for different query methods](https://sitecore.stackexchange.com/questions/6803/sitecore-powershell-query-for-big-images-of-a-certain-size/6811#6811)
- [Escaping queries with reserved keywords](https://sitecore.stackexchange.com/questions/10127/how-to-escape-a-query-in-sitecore-powershell)
- [Working with Sitecore Items in PowerShell Extensions](https://blog.najmanowicz.com/2014/10/12/working-with-sitecore-items-in-powershell-extensions/)

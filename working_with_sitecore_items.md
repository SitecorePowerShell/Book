# Working with Sitecore items


## How do I retrieve my Sitecore items the PowerShell way?

Sitecore items should be retrtieved  with use of ```Get-Item``` and ```Get-ChildItem``` cmdlets. That is because those 2 cmdlets add a PowerShell wrapping around them.

If you have retrieved your item directly using the Sitecore API you can still add the nice wrapper. You can do that my piping them through the ```Wrap-Item``` commandlet. Allways ise the  latest version to leverage the full potential of the environment.

## Getting Items based on path

If you know the path to your item and the database you can retrieve your item as follows:
```
PS master:\>Get-Item master:/content/home
 
Name Children Languages                Id                                     TemplateName
---- -------- ---------                --                                     ------------
Home True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

You might have noticed that I’ve skipped the /sitecore part in the path. This is because this item is represented by the root item of the drive ```master:```. The above will return the latest version of the item in your current language. But what if you want the item in another language? No problem – let’s retrieve the Danish version of Home…
```
PS master:\>Get-Item master:/content/home -Language da | Format-Table DisplayName, Language, Id, Version, TemplateName

DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

I’ve formatted the output above to show you that indeed the right language was returned. If you want to get all languages – the commandlet will support wildcards for both -Language and -Version parameter. The following returns the latest version for all languages of an item:
```
PS master:\>Get-Item master:/content/home -Language * | Format-Table DisplayName, Language, Id, Version, TemplateName
  
DisplayName Language ID                                     Version TemplateName
----------- -------- --                                     ------- ------------
Home        en       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        de-DE    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        pl-PL    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Home        en-US    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 3       Sample Item
Home        en-GB    {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
Hjem        da       {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} 1       Sample Item
```

The ```en-US``` above shows version number as 2 - which means that there are or at some point were other versions. Let’s retrieve the item in all languages and all versions…

```
PS master:\>Get-Item master:/content/home -Language * -Version *| Format-Table DisplayName, Language, Id, Version, TemplateName
  
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

You can see that those can be used in conjunction to retrieve all possible variants of an item in every language and every version. Filters like ```en-*``` are allowed. Nn execution with such filter would return all items in the English language, ignoring the region.

Similarly ```Get-ChildItem``` supports this functionality for its children.

```powershell
PS master:\>Get-ChildItem master:/content -Language * -Version * | Format-Table DisplayName, Language, Id, Version, TemplateName
  
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

## Getting large number of filtered items with Sitecore queries

It’s not always the most efficient to operate on items by traversing the tree using ```Get-ChildItem```. This is especially true if you need to work on large trees but need to select only a few items of e.g. a specific template. For this we’ve introduced support for the Sitecore query within our provider. Following example fetches all items in /sitecore/content/ branch in the ```master``` database for items of "Sample Item template":
```
PS master:\>Get-Item master: -Query "/sitecore/content//*[@@templatename='Sample Item']"
  
Name                             Children Languages                Id                                     TemplateName
----                             -------- ---------                --                                     ------------
Copy of Home                     True     {en, de-DE, es-ES, pt... {503713E5-F9EE-4847-AEAF-DD13FD853004} Sample Item
Home                             True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

Naturally the -Language parameter still works…
```
PS master:\>Get-Item master: -Query "/sitecore/content//*[@@templatename='Sample Item']" -Language * -Version * | Format-Table DisplayName, Language, Id, Version, TemplateName -auto
  
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

## Using ID to address an item.

You can also fetch for items by ID like:
```
PS master:\>Get-Item master: -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"
  
Name  Children Languages                Id                                     TemplateName
----  -------- ---------                --                                     ------------
Home  True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```
## Using Item Uri

You can query item using its Uri. Uri contains information about version and language encoded directly in it:

```
PS master:\>Get-Item master: -Uri "sitecore://master/{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}?lang=en&ver=1"
  
Name Children Languages                Id                                     TemplateName
---- -------- ---------                --                                     ------------
Home True     {en, de-DE, es-ES, pt... {110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9} Sample Item
```

In both of the above cases you need to specify a database (like ```master:```) as the path. This is because PowerShell needs to know in context of which provider those parameters are executed.  Other providers do not support those for obvious reasons. (e.g. files don’t have versions or support for languages).

## What do the item extensions allow you to do?

I often see the following two ways of accessing and changing fields used in scripts:

```
Set-ItemProperty -Path master:/content/home -Name "Title" -Value "New Title"
```
Or something that would feel natural to a Sitecore developer:
```
$item = Get-Item master:/content/home
$item.Editing.BeginEdit()
$item["Title"] = "New Title"
$item.Editing.EndEdit()
```

Those approaches while working are not the most efficient notation for modifying item content. Items that you’re getting from the provider give you a better way of doing it. Provider exposes your Sitecore fields as semi-native PowerShell properties. Instead of doing the above you can do:
```
$item = Get-Item master:/content/home
$item.Title = "New Title"
```
… or even shorter if you don’t want to use a variable:
```
(Get-Item master:/content/home).Title = "New Title"
```
It also doesn’t work for string properties exclusively. There are a other hidden gems in those properties. For example if we detect that the field is a Date or Datetime field, we will return System.DateTime typed value from a field rather than the string Sitecore stores internally.
```
PS master:\>(Get-Item master:/content/home).__Created
Monday, April 07, 2008 1:59:00 PM
```

And not just read – you can also assign ```System.DateTime``` value to such PowerShell “native” property:
```
PS master:\>(Get-Item master:/content/home).__Created = [DateTime]::Now
```
Now let’s read it again:
```
PS master:\>(Get-Item master:/content/home).__Created
Monday, October 13, 2014 1:59:41 AM
```
Great we’ve just changed it! Our property handlers take care of all the necessary ```.Editing.BeginEdit```s and ```.Editing.EndEdit```s.

It works for assigning content items and media items as well. If your item has link fields in it, you can assign other items to them and not worry about the link format – we will do all the plumbing for you.

To provide an example – I’ve extended my home with additional fields as follows:

![image](http://blog.najmanowicz.com/wp-content/uploads/2014/10/image.png)

I can do the following to assign an image to my Image field

```
(Get-Item master:/content/home).Image = Get-Item 'master:\media library\logo'
```

Easy enough, isn’t it? Let us (the PowerShell Extensions) detect the field type for you and worry about what to call! Now let’s assign a content item to GeneralLink and Link fields… just like you expected:

```
(Get-Item master:/content/home).GeneralLink = Get-Item 'master:\content\CognifideCom'
```

Even better – if you assign a media item to it, it will detect that and do the right thing assigning it as a media link.

What about fields that accept lists of items? We’ve got your back here as well. Let’s assign all children of ```/sitecore/content/``` item to the ItemList field:
```
(Get-Item master:/content/home).ItemList = Get-ChildItem 'master:\content\'
```
Ok, so let’s see how our item looks in the Content editor after all the assignments that we’ve just performed:

![](http://blog.najmanowicz.com/wp-content/uploads/2014/10/image1.png)

Great! Looks like it worked…

Those little improvements make your scripts much more succinct and understandable. Try it for yourself!

## When not to use the automated properties?

As with every rule there is an exception to this one. Those automated properties perform the ```$item.Editing.BeginEdit()``` and ```$item.Editing.EndEdit()``` every time  which results in saving the item after every assignment. Assigning multiple properties on an item this way might be detrimental to the performance of your script. In such cases you might want to call ```$item.Editing.BeginEdit()``` yourself before modifying the item. Subsequently call the ```$item[“field name”] = “new value”``` for each property modify. Finally end with the ```$item.Editing.EndEdit()```. Choosing this way is situational and will usually only be required if you’re working with a large volume of data. In those cases you might also want to introduce the ```Sitecore.Data.BulkUpdateContext``` trick used in [this blog post](http://bartlomiejmucha.com/en/blog).
# Data Sources

Scripted Data Sources provide a way to use a PowerShell script to perform complex queries.

Here are some good reasons to use this feature:

* Delivering complex functionality based on multiple criteria. 
* Your field may need to provide different set of items to choose from based on:
  * user name or role \(in simplest case this can be done using right management, but maybe not always possible in a more elaborate scenario\)
  * current day or month?
  * In a multisite/multimarket scenario you may want to show different items for each site

    based on engagement analytics parameters of the page

    based on where in the tree an item exist \(some of it can be done with use of a “query:”\)

  * anything you might want to build the code data source for…

Something that would be beyond the reach of a regular Sitecore query and potentially something that you would really need to deliver code source for. But maybe you’re not in a position to deploy that on your environment?

## Field Data Source

Field Data Source provides a great opportunity for a script.

Below are field types you may wish to use a script:

* Checklist
* Droplist
* Grouped Droplink
* Grouped Droplist
* Multilist
* Name Lookup Value List
* Droplink

1. Begin by adding a new script library called _Data Sources_ followed by adding a script. You can call it something like _Get-GlobalOption_.

   ```powershell
   # The script must return an item. This is useful for populating a Droplink.
   Get-ChildItem -Path "master:" -ID "{110D559F-DEA5-42EA-9C1C-8A5DF7E70EF9}"
   ```

2. Add the path to your script in the _Source_ field on the data template. The source should be prefixed with `script:` followed by the path.

   ```powershell
   script:/sitecore/system/Modules/PowerShell/Script Library/X-Demo/Data Sources/Get-GlobalOption
   ```

3. Enjoy the results.

![Droplink query](../../.gitbook/assets/droplist.png)

## Rendering Data Source Roots

//TODO

## Rendering Data Source Items

//TODO

## References

* PowerShell Scripted Data Sources [part 1](https://blog.najmanowicz.com/2013/04/17/powershell-scripted-datasources-in-sitecore-part-1/) and [part 2](https://blog.najmanowicz.com/2013/05/06/powershell-scripted-data-sources-in-sitecore-part-2/)
* Sitecore Spark [article][3] on using scripted datasources

[3]: [https://www.sitecorespark.com/blog/2018/3/using-sitecore-powershell-scripts-for-datasources](https://www.sitecorespark.com/blog/2018/3/using-sitecore-powershell-scripts-for-datasources)


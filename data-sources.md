# Data Sources

Scripted Data Sources provide a way to use a PowerShell script to perform complex queries.

Here are some good reasons to use this feature:

- Delivering complex functionality based on multiple criteria. 
- Your field may need to provide different set of items to choose from based on:
 - user name or role (in simplest case this can be done using right management, but maybe not always possible in a more elaborate scenario)
 - current day or month?
 - In a multisite/multimarket scenario you may want to show different items for each site
based on engagement analytics parameters of the page
based on where in the tree an item exist (some of it can be done with use of a “query:”)
 - anything you might want to build the code data source for…

Something that would be beyond the reach of a regular Sitecore query and potentially something that you would really need to deliver code source for. But maybe you’re not in a position to deploy that on your environment?

### Field Data Source

Field Data Source provides a great opportunity for a script. 

Below are field types you may wish to use a script:

- Checklist
- Droplist
- Grouped Droplink
- Grouped Droplist
- Multilist
- Name Lookup Value List
- Droplink


1. Begin by adding a new script library called *Data Sources* followed by adding a script. You can call it something like *Get-GlobalOption*.

### References

* PowerShell Scripted Data Sources [part 1][1] and [part 2][2]

[1]: http://blog.najmanowicz.com/2013/04/17/powershell-scripted-datasources-in-sitecore-part-1/
[2]: http://blog.najmanowicz.com/2013/05/06/powershell-scripted-data-sources-in-sitecore-part-2/
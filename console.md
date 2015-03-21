# Console

The SPE Console is a command line interface (CLI) that provides a streamlined tool to working with Windows PowersShell and Sitecore.

Here's a quick look at the Console:
![Console](Console-Empty.png)

A few things to note about the console:
* You can interact with the providers available in the standard Windows PowerShell Console. Below are some of the important providers.
 * FileSystem - Supports interacting with files and folders.
 * CmsItemProvider - Supports interacting with the Sitecore content items.
* The SPE module provides some convenient variables out of the box for use in running commands and scripts.
 * AppPath - Path to the installed directory for Sitecore.
 * Me - Current logged in user for Sitecore.
 * PWD - Present working directory for the console.
 * SitecoreDataFolder
 * SitecoreDebugFolder
 * SitecoreIndexFolder
 * SitecoreLayoutFolder
 * SitecoreLogFolder
 * SitecoreMediaFolder
 * SitecorePackageFolder
 * SitecoreSerializationFolder
 * SitecoreTempFolder
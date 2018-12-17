# Toolbox

The PowerShell Toolbox is quick way to access frequently used scripts.

Navigate to _Sitecore -&gt; PowerShell Toolbox_ and after selecting you should see the configured scripts:

![image](https://user-images.githubusercontent.com/933163/50113236-fda4b500-0206-11e9-8997-c629927a5148.png)

**Note:** Examples included in the following modules

* Authorable Reports
* Logged in Session Manager
* Package Generator
* Task Management
* Platform

## Available Tools

### Index Viewer

This tool provides similar functionality to the [Index Viewer](https://marketplace.sitecore.net/en/Modules/I/Index_Viewer.aspx) module. Search and rebuild the index on-demand.

![image](https://user-images.githubusercontent.com/933163/50112430-d351f800-0204-11e9-9d5c-07c99c163205.png)

### Logged in Session Manager

View the list of user sessions and "kick" them out as needed.

![image](https://user-images.githubusercontent.com/933163/50112838-e1ecdf00-0205-11e9-8959-9d4332859162.png)

### Rules based report

Generate a report using the Sitecore Rules Engine.

![image](https://user-images.githubusercontent.com/933163/50112876-fd57ea00-0205-11e9-89ac-f407ada390a8.png)

### PowerShell Background Session Manager

View the list of SPE sessions and "kill" them as needed.

![image](https://user-images.githubusercontent.com/933163/50112986-51fb6500-0206-11e9-8c4e-2905f1378dc9.png)

### Create Anti-Package

This tool provides similar functionality to the [Sitecore Rocks](https://github.com/SitecorePowerShell/Book/tree/9c7126d7a38df6ef372e8baef52f9a02baabd550/modules/integration-points/[https:/marketplace.sitecore.net/en/Modules/S/Sitecore/_Rocks.aspx]) module.

![image](https://user-images.githubusercontent.com/933163/50112920-1fea0300-0206-11e9-9a37-138c9cb9f273.png)

### Re-create Site from Sitemap

Simple tool for generating a site tree using an existing sitemap.

![image](https://user-images.githubusercontent.com/933163/50113021-68a1bc00-0206-11e9-8f82-e7bc1d8eaefd.png)

### Task Manager

View and manage the configured scheduled tasks.

![image](https://user-images.githubusercontent.com/933163/50113111-a30b5900-0206-11e9-84b1-5ce866bb1d85.png)

## Create Tools for the Toolbox

To create your own Toolbox item take the following steps: 1. Create the _Toolbox_ folder under an SPE module. Use the context menu to simplify the process.

* Right click the module name and choose Scripts -&gt; Create libraries for integration points.

  ![Module Libraries](../../.gitbook/assets/module-createlibraries.png)

* Select the _Toolbox_ item and click _Proceed_.

  ![Module Toolbox Library](../../.gitbook/assets/module-createtoolboxlibrary.png)

  1. Create a _PowerShell Script_ under the _Toolbox_ item.

* Right click the _Toolbox_ library and choose _PowerShell Script_.

  ![Libary Script](../../.gitbook/assets/library-createscript.png)

  1. Open and edit the _PowerShell Script_ using the ISE.

     ![ISE Edit](../../.gitbook/assets/script-editise.png)

  2. Run the _Rebuild All_ command in the ISE by navigating to the _Settings_ tab and selecting the icon to rebuild. Be certain to enable the module before running the rebuild command.

     ![ISE Settings Tab](../../.gitbook/assets/ise-settingstab.png)

  3. Verify the new toolbox item appears in the Toolbox.

     ![image](https://user-images.githubusercontent.com/933163/50113236-fda4b500-0206-11e9-8997-c629927a5148.png)


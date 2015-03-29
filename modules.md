# Modules

Sitecore PowerShell Extensions provides a way to organize your scripts into *modules*. 

Some benefits to using modules:
* You can enable or disable the module as needed. For this to take full affect the integration should be rebuilt in the ISE.
* The out-of-the-box scripts are in their own module called *Platform*.
* Adam's [post][1] on the module design goes in-depth to why we proposed this architectural change.

Getting started with your own module is a short process.

1. Navigate to the *Script Library* item and *Insert -> PowerShell Script Module*.
![New Module](images/screenshots/library-createnewmodule.png)
2. Enter the name for the new module and click *OK*.
3. Right click the new module and *Scripts -> Create libraries for integration points*.
![Integration Points](images/screenshots/module-createlibraries.png)
4. Select the appropriate integration points for your module.
![Integration Point Libraries](images/screenshots/module-createtoolboxlibrary.png)


[1]: http://blog.najmanowicz.com/2014/11/01/sitecore-powershell-extensions-3-0-modules-proposal/
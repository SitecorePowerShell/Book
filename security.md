# Security

#### Security Policies

There are two main security policies to consider when using the SPE module:
* Application Pool service account
* Sitecore user account

##### Application Pool Service Account

The first policy relates to the Application Pool service account running in IIS. The Windows PowerShell runspace will have access to the local system via providers (i.e. FileSystem, Registry), and be managed through the Console or ISE. If the service account is capable of removing files from the root directory, then SPE can accomplish the same.

##### Sitecore User Account

The second policy is tied to the Sitecore user account. The code executed through SPE operates within the privileges of the logged in user. Keep in mind that this can be bypassed just as can be through the Sitecore API.

#### Security Hardening

The time will come when you need to lock down the SPE module. The following section outlines steps you can take to minimize the surface area for attack.

##### Disable Web Services

You can disable the web services by overriding the following configuration file `\App_Config\Include\Cognifide.PowerShell.config`.

Look for the following section and enable/disable as needed.

```xml
<sitecore>
    <services>
        <restfulv1 enabled="false" />
        <restfulv2 enabled="true" />
        <remoting enabled="true" />
        <client enabled="true" />
    </services>
</sitecore>
```

* **RESTful v1** - Used in early version of SPE. Disabled by default. Service associated with `RemoteScriptCall.ashx`.
* **RESTful v2** - Used when the url contains all the information needed to execute a script saved in the SPE library. Service associated with `RemoteScriptCall.ashx`.
* **Remoting** - Used when passing scripts to SPE for execution. Service associated with `RemoteAutomation.asmx`.
* **Client** - Used for the SPE Console. Service associated with `PowerShellWebService.asmx`.

##### Restrict Users and Roles

Deny access to the web services for unauthenticated users and roles using the `<deny>` element as described [here][1] in `sitecore modules\PowerShell\Services\web.config`.

**Example:** The following configuration will deny anonymous calls to the web services.

```xml
<configuration>
    <system.web>
      <authorization>
        <deny users="?" />
      </authorization>
    </system.web>
</configuration>
```

##### Minimal Web Service Configuration

The following files are the bare minimum required to support SPE web services. This setup is suitable for environments such as the Content Delivery.

**Required:**
* `App_Config\Include\Cognifide.PowerShell.config`
* `bin\Cognifide.PowerShell.dll`
* `sitecore modules\PowerShell\Services\web.config`
* `sitecore modules\PowerShell\Services\RemoteAutomation.asmx`
 
You will also need to patch the configuration `App_Config\Include\Cognifide.PowerShell.config` with the following:

```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/">
    <sitecore>
        <controlSources>
            <source mode="on" namespace="Cognifide.PowerShell.Client.Controls" assembly="Cognifide.PowerShell">
                <patch:delete />
            </source>
            <source mode="on" namespace="Cognifide.PowerShell.Client.Applications"
                  folder="/sitecore modules/Shell/PowerShell/" deep="true">
                <patch:delete />
            </source>
        </controlSources>
    </sitecore>
</configuration>
```

[1]: https://msdn.microsoft.com/en-us/library/8aeskccd%28v=vs.71%29.aspx
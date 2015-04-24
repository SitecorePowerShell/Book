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

The time will come when you need to lock down the SPE module. The following section outlines steps you can take to secure the module.

##### Disable Web Services

You can disable the web services overriding the configuration file `\App_Config\Include\Cognifide.PowerShell.config`.

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

##### Minimal Web Service Configuration

The following files are the bare minimum for setting up SPE in an environment. This setup is suitable for environments such as the Content Delivery.

**Required:**
* `App_Config\Include\Cognifide.PowerShell.config`
* `bin\Cognifide.PowerShell.dll`
* `sitecore modules\PowerShell\Services\web.config`
* `sitecore modules\PowerShell\Services\RemoteAutomation.asmx`

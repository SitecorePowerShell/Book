# Security

No documentation is complete without a section regarding security. Below are some things to consider while using the application.

### Security Policies

There are two main security policies to consider when using the SPE module:
* Application Pool service account
* Sitecore user account

#### Application Pool Service Account

The first policy relates to the Application Pool service account running in IIS. The Windows PowerShell runspace will have access to the local system via providers (i.e. FileSystem, Registry), and be managed through the Console or ISE. If the service account is capable of removing files from the root directory, then SPE can accomplish the same.

When using the IIS identities such as *ApplicationPoolIdentity* and *NetworkService* the scripts will not have access to directories outside of the application such as the drive root. You may also notice that the *$HOME* variable is empty; this is because only named service accounts have profiles.

#### Sitecore User Account

The second policy relates to the Sitecore user account. The code executed through SPE operates within the privileges of the logged in user. Keep in mind that this can be bypassed just as can be through the Sitecore API.

Application Security

| **Application** | **Security Roles** |
| ----------- | -------------- |
| PowerShell Console | sitecore\Sitecore Client Developing |
| PowerShell ISE | sitecore\Sitecore Client Developing |
| PowerShell ListView | sitecore\Sitecore Client Users |
| PowerShell Runner | sitecore\Sitecore Client Users |

### Security Hardening

The time will come when you need to lock down the SPE module. The following section outlines steps you can take to minimize the surface area for attack.

#### Disable Web Services

You can disable the web services by overriding the following configuration file `\App_Config\Include\Cognifide.PowerShell.config`.

Look for the following section and enable/disable as needed.

```xml
<sitecore>
    <services>
        <restfulv1 enabled="false" />
        <restfulv2 enabled="true" />
        <remoting enabled="true" />
        <fileDownload enabled="false" />
        <fileUpload enabled="false" />
        <mediaDownload enabled="false" />
        <mediaUpload enabled="false" />
        <client enabled="true" />
    </services>
</sitecore>
```

* **RESTful v1** - Used in early version of SPE. Disabled by default. Service associated with `RemoteScriptCall.ashx`.
* **RESTful v2** - Used when the url contains all the information needed to execute a script saved in the SPE library. Service associated with `RemoteScriptCall.ashx`.
* **Remoting** - Used when passing scripts to SPE for execution. Service associated with `RemoteAutomation.asmx`.
* **File Download** - Used when the url contains all the information needed to download a file from the server. Service associated with `RemoteScriptCall.ashx`.
* **File Upload** - Used when the url contains all the information needed to upload a file to the server. Service associated with `RemoteScriptCall.ashx`.
* **Media Download** - Used when the url contains all the information needed to download a media item from the server. Service associated with `RemoteScriptCall.ashx`.
* **Media Upload** - Used when the url contains all the information needed to upload a media item to the server. Service associated with `RemoteScriptCall.ashx`.
* **Client** - Used for the SPE Console. Service associated with `PowerShellWebService.asmx`.

#### Restrict Users and Roles

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

If you disable *Anonymous Authentication* and enable *Windows Authentication* in IIS, such as the directory `sitecore modules\PowerShell\Services\` you'll need to use the **Credential** parameter for any command that interacts with the services. See the Remoting section for examples.

#### Minimal Web Service Configuration

The following files are the bare minimum required to support SPE web services. This setup is suitable for environments such as the Content Delivery.

**Required:**
* `App_Config\Include\Cognifide.PowerShell.config`
* `App_Config\Include\Cognifide.PowerShell.Minimal.config`
* `bin\Cognifide.PowerShell.dll`
* `bin\Cognifide.PowerShell.VersionSpecific.dll`
* `sitecore modules\PowerShell\Services\web.config`
* `sitecore modules\PowerShell\Services\RemoteAutomation.asmx`
 
You will also need to patch the configuration with the following:

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

For your convenience we've included a package bundled with all of the above called *SPE Minimal-3.x for Sitecore x.zip*. Any of the disabled configuration files should be enabled following extraction.

[1]: https://msdn.microsoft.com/en-us/library/8aeskccd%28v=vs.71%29.aspx
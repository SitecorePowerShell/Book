# Security

You need to be mindful that Sitecore PowerShell Extensions is a very sharp tool and while it can be leveraged to do great things, it can also be a vector of dangerous attacks if not secured properly. This is why we recommend that you do not install it on Content Delivery instances or if possible avoid deploying it on all servers that face Internet altogether. 

## Security Policies

There are two main security policies to consider when using the SPE module:

* Application Pool service account
* Sitecore user account

### Application Pool Service Account

The first policy relates to the Application Pool service account running in IIS. The Windows PowerShell runspace will have access to the local system via providers \(i.e. FileSystem, Registry\), and be managed through the Console or ISE. If the service account is capable of removing files from the root directory, then SPE can accomplish the same.

When using the IIS identities such as _ApplicationPoolIdentity_ and _NetworkService_ the scripts will not have access to directories outside of the application such as the drive root, but you should perform a due dilligence to make sure this is the case! You may also notice that the _$HOME_ variable is empty; this is because only named service accounts have profiles.

### Sitecore User Account

The second policy relates to the Sitecore user account. The code executed through SPE operates within the privileges of the logged in user. Keep in mind that this can be bypassed just as can be done through the Sitecore API as PowerShell scripts can call the APIs that disable the Sitecore security.

**Application Security**
The following settings are configured under `core:\content\Applications\PowerShell`.

| **Feature**| **Visibility** |
| --- | --- |
| PowerShell Console | sitecore\Developer \(read\) |
| PowerShell ISE | sitecore\Developer \(read\) |
| PowerShell ListView | sitecore\Sitecore Client Users \(read\) |
| PowerShell Runner | sitecore\Sitecore Client Users \(read\) |

**Note:** The security is validated _OnLoad_.

**Menu Item Security**
The following settings are configured under `core:\content\Applications\Content Editor\Context Menues\Default\`.

| **Feature** | **Visibility** | **Command State** |
| --- | --- | --- |
| Edit Script | sitecore\Developer (read) | **Enabled** when item template is _PowerShell Script_ otherwise **Hidden** |
| Console | sitecore\Developer (read) | **Enabled** until user is _non-admin_ and not in _sitecore\Sitecore Client Developing_ |
| Script | sitecore\Sitecore Limited Content Editor (deny read) | **Enabled** |

**Note:** See the _Interactive_ section on _PowerShell Script Library_ and _PowerShell Script_ items for visibility and enabled rules. To hide each feature you can change also the security settings for the roles that should not see the menu.

## Security Hardening

The following section outlines steps you can take to minimize the surface area for attack. The following topics describe how to manage security for interfaces and services to the various parts of the module.

### Session Elevation

The various [interfaces](/interfaces.md) bundled with the module provide convenient ways to interact with the Sitecore API. The module provides a **User Account Control** feature akin to that of Microsoft Windows.

#### User Account Control

Let's have a look at the configurable features which make up the UAC.

**Gate**

The way in which scripts make their way into Sitecore through built-in interfaces. Includes the [Console](/console.md), [ISE](/scripting.md), and Content Editor via _Item Saving_.

| Attribute | Description |
| --- | --- |
| name | built-in name for the gate |
| token | name of the token to use for the elevated session | 

**Token**

The object which expires after a predetermined time. These can be unique to each gate or shared.

| Attribute | Description |
| --- | --- |
| name | unique string used for the gate *token* attribute |
| expiration | timespan used to determine the elevated session lifetime (hh:mm:ss) |
| elevationAction | action to perform when session elevation is triggered (allow, block, password) |

* **Allow** - Always allow the session to run elevated without prompting the user for permission. This should never be used outside of a developer's machine.
* **Block** - Always block the session from running elevated without prompting the user for permission.
* **Password** - Prompt the user for a password before running the session elevated, unless an unexpired session is active.

**Example:** The following extends the token expiration to 10 minutes.

```xml
<sitecore>
  <powershell>
    <userAccountControl>
      <tokens>
        <token name="Console">
          <patch:attribute name="expiration">00:10:00</patch:attribute>
        </token>
        <token name="ISE">
          <patch:attribute name="expiration">00:10:00</patch:attribute>
        </token>
        <token name="ItemSave">
          <patch:attribute name="expiration">00:10:00</patch:attribute>
        </token>
      </tokens>
    </userAccountControl>
  </powershell>
</sitecore>
```

Gates with **Password** protection enabled prompt the user when no elevated session is available.

![Elevate Session State](/images/screenshots/uac/security-elevatedsessionstate-password.png)

A Content Editor Warning is displayed when a PowerShell Module, Script Library, and Script is selected. Click "Elevate session" to show the hidden fields and enable the management of the item.

![Elevate session](/images/screenshots/uac/security-elevatedsessionstate-contenteditor.png)

The following warning is rendered in the ISE while the session state is elevated. Click "Drop elevated session state" if you do not want to wait for the elevated session to timeout.

![Drop elevated session state](/images/screenshots/uac/security-elevatedsessionstate-dropise.png)

### Configure Web Services

The web services providing external access to Sitecore are disabled by default. You can override by patching the following configuration file `\App_Config\Include\Cognifide.PowerShell.config`.

Look for the following section and enable as needed.

```xml
<sitecore>
  <powershell>
    <services>
      <restfulv1 enabled="false" />
      <restfulv2 enabled="false" />
      <remoting enabled="false" />
      <fileDownload enabled="false" />
      <fileUpload enabled="false" />
      <mediaDownload enabled="false" />
      <mediaUpload enabled="false" />
      <handleDownload enabled="true" />
      <client enabled="true" />
      <execution enabled="true" />
    </services>
  </powershell>
</sitecore>
```

#### Service Descriptions

* **Remoting** - Used when passing scripts to SPE for execution. Enable when using the **SPE Remoting** module. Service associated with `RemoteAutomation.asmx`.
* **RESTful v2** - Used when the url contains all the information needed to execute a script saved in the SPE library. Service associated with `RemoteScriptCall.ashx`.
* **File Download** - Used when the url contains all the information needed to download a file from the server. Enable when using the **SPE Remoting** module. Service associated with `RemoteScriptCall.ashx`.
* **File Upload** - Used when the url contains all the information needed to upload a file to the server. Enable when using the **SPE Remoting** module. Service associated with `RemoteScriptCall.ashx`.
* **Media Download** - Used when the url contains all the information needed to download a media item from the server. Enable when using the **SPE Remoting** module. Service associated with `RemoteScriptCall.ashx`.
* **Media Upload** - Used when the url contains all the information needed to upload a media item to the server. Enable when using the **SPE Remoting** module. Service associated with `RemoteScriptCall.ashx`.
* **Handle Download** - Used when a file is downloaded through the Sitecore interface. Enable when using the **SPE Remoting** module. Service associated with `RemoteScriptCall.ashx`.
* **Client** - Used for the SPE Console. Service associated with `PowerShellWebService.asmx`.
* **Execution** - Used when SPE checks if the user has access to run the application.
* **RESTful v1** - Used in early version of SPE. Avoid using this if possible. Service associated with `RemoteScriptCall.ashx`.

The preferred way to override the settings is through the use of a configuration patch file.

**Example:** The following enables the file and media downloads.

```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <fileDownload>
          <patch:attribute name="enabled">true</patch:attribute>
        </fileDownload>
        <mediaDownload>
          <patch:attribute name="enabled">true</patch:attribute>
        </mediaDownload>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

**Example:** The following enables the SPE Remoting service and requires a secure connection using HTTPS.

```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <patch:attribute name="requireSecureConnection">true</patch:attribute>
          <patch:attribute name="enabled">true</patch:attribute>
        </remoting>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

### Restrict Users and Roles

#### Sitecore level security

You are required to explicitly grant the SPE Remoting session user account to a predefined role found in the configuration `Cognifide.PowerShell.config`. There is a generic list of permissions configured by default but we highly encourage you to adjust to meet your security requirements.

**Example:** The following configuration defines the roles that have access to use SPE Remoting. Any role previously defined in the `<authorization/>` section is removed and custom roles are then added.

```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting>
          <authorization>
            <patch:delete />
          </authorization>
          <authorization>
            <add Permission="Allow" IdentityType="Role" Identity="sitecore\PowerShell Extensions Remoting" />
          </authorization>
        </remoting>
      </services>
    </powershell>
  </sitecore>
</configuration>
```

#### IIS level security

Deny access to the web services for unauthenticated users and roles using the `<deny>` element as described [here](https://msdn.microsoft.com/en-us/library/8aeskccd%28v=vs.71%29.aspx) in `sitecore modules\PowerShell\Services\web.config`.

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

If you disable _Anonymous Authentication_ and enable _Windows Authentication_ in IIS, such as the directory `sitecore modules\PowerShell\Services\` you'll need to use the **Credential** parameter for any command that interacts with the services. See the [Remoting](/remoting.md "Remoting") section for examples.

### Minimal Web Service Configuration

The following files are the bare minimum required to support SPE web services. This setup is suitable for environments such as servers built within a Continuous Integration environment that need remoting enabled. The remoting however is not enabled by default. If you need this functionality, you should enable it separately in an include config file.

**Required:**

* `App_Config\Include\Cognifide.PowerShell.config`
* `App_Config\Include\Cognifide.PowerShell.Minimal.config`
* `bin\Cognifide.PowerShell.dll`
* `bin\Cognifide.PowerShell.VersionSpecific.dll`
* `sitecore modules\PowerShell\Services\web.config`
* `sitecore modules\PowerShell\Services\RemoteAutomation.asmx`
* `sitecore modules\PowerShell\Services\RemoteScriptCall.ashx`

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

For your convenience we've included a package bundled with all of the above called _SPE Minimal-4.x for Sitecore x.zip_. Any of the disabled configuration files should be enabled following extraction.


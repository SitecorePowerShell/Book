# Remoting

There are a number of use cases where you need to remotely run scripts within SPE. Here we will try to cover a few of those use cases.

#### Remoting Automation Service

We have provided a handy way of executing scripts via web service using the Remoting Automation Service.

##### Video Tutorial

[![SPE Remoting Module](http://img.youtube.com/vi/fGvT8eDdWrg/0.jpg)](http://www.youtube.com/watch?v=fGvT8eDdWrg "Click for a quick demo")

##### Sitecore to Sitecore communication

**Example:** The following connects a local instance of SPE to a remote instance and executes the provided script.

```powershell
Import-Function -Name Remoting2

$session = New-ScriptSession -Username "admin" -Password "b" -ConnectionUri "http://remotesitecore"

$script = {
    [Sitecore.Security.Accounts.User]$user = Get-User -Identity admin
    $user
    $params.date.ToString()
}

$args = @{
    "date" = [datetime]::Now
}

Invoke-RemoteScript -ScriptBlock $script -Session $session -ArgumentList $args

Name                     Domain       IsAdministrator IsAuthenticated
----                     ------       --------------- ---------------
sitecore\admin           sitecore     True            False          
4/26/2015 6:15:41 PM
```

##### Windows PowerShell ISE to Sitecore communication

To setup this scenario you'll need to follow these steps:
* Download the SPE Remoting package from the Sitecore Marketplace.
* Extract the package to to your module path. Instructions can be found [here][3].

**Example:** The following connects Windows PowerShell ISE to a remote Sitecore instance and executes the provided script.

```powershell
Import-Module -Name SPE

$session = New-ScriptSession -Username "admin" -Password "b" -ConnectionUri http://remotesitecore

$libraryPath = "/sitecore/media library/images/image.png"
Get-Item -Path C:\image.png | Send-MediaItem -Session $session -Destination $libraryPath

$savePath = "C:\image-$([datetime]::Now.ToString("yyyyddMM-HHmmss")).png"
Receive-MediaItem -Session $session -Path $libraryPath -Destination $savePath

    Directory: C:\

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         5/25/2015  11:23 AM          0 image-20152505-112302.png  
```

**Example:** The following downloads all the images in the media library under the specified directory.

```powershel
$session = New-ScriptSession -Username admin -Password b -ConnectionUri http://remotesitecore

Invoke-RemoteScript -Session $session -ScriptBlock { 
        Get-ChildItem -Path "master:/sitecore/media library/Images/Icons/" | Select-Object -Expand ItemPath 
    } | Receive-MediaItem -Session $session -Destination C:\Temp\Images\
```

##### Windows Authenticated Requests

If you have configured the services to run under *Windows Authentication* mode then you'll need to use the **Credential** parameter for the commands.

You'll definitely know you need it when you receive an error like the following:

```powershell
New-WebServiceProxy : The request failed with HTTP status 401: Unauthorized.
```

**Example:** The following connects Windows PowerShell ISE to a remote Sitecore instance using Windows credentials and executes the provided script.

```powershell
Import-Module -Name SPE
$credential = Get-Credential
$session = New-ScriptSession -Username admin -Password b -ConnectionUri http://remotesitecore -Credential $credential
Invoke-RemoteScript -Session $session -ScriptBlock { Get-User -id admin }

Name                     Domain       IsAdministrator IsAuthenticated
----                     ------       --------------- ---------------
sitecore\admin           sitecore     True            False          
```

**Example:** The following connects to several remote instances of Sitecore and returns the server name.

```powershell
# If you need to connect to more than one instance of Sitecore add it to the list.
$instanceUrls = @("http://remotesitecore","http://remotesitecore2")
$session = New-ScriptSession -Username admin -Password b -ConnectionUri $instanceUrls
Invoke-RemoteScript -Session $session -ScriptBlock { $env:computername }
```

#### File and Media Service

We have provided a service for downloading all files and media items from the server. This disabled by default and can be enabled using a patch file. See the [Security](security.md) page for more details about the services available and how to configure.

**Example:** The following downloads a single file from the *Package* directory.

```powershell
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri http://remotesitecore
Receive-RemoteItem -Session $session -Path "default.js" -RootPath App -Destination "C:\Files\"
```

**Example:** The following downloads a single media item from the library.

```powershell
Import-Module -Name SPE
$session = New-ScriptSession -Username admin -Password b -ConnectionUri http://remotesitecore
Receive-RemoteItem -Session $session -Path "/Default Website/cover" -Destination "C:\Images\" -Database master
```

#### Advanced Script Sessions

Inevitably you will need to have long running processes triggered remotely. In order to support this functionality without encountering a timeout using `Invoke-RemoteScript` you can use the following list of commands.



**References:**
* Michael's follow up post on [Remoting][2]
* Adam's initial post on [Remoting][1]

[1]: http://blog.najmanowicz.com/2014/10/10/sitecore-powershell-extensions-remoting/
[2]: http://michaellwest.blogspot.com/2015/07/sitecore-powershell-extensions-remoting.html
[3]: https://msdn.microsoft.com/en-us/library/dd878350(v=vs.85).aspx
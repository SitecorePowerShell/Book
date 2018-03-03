# Pipelines

Use the following in your scripts to get access to the arguments passed to the processor.

```powershell
 $pipelineArgs = Get-Variable -Name pipelineArgs -ValueOnly
 ```

### Logged In

**Note:** Examples included in the following modules
* Automatically show quick info section

### Logging In

**Note:** Examples included in the following modules
* Enforce user password expiration

```powershell
$pipelineArgs.Username
$pipelineArgs.Password
$pipelineArgs.Success
$pipelineArgs.StartUrl
```

### Logout

**Note:** Examples included in the following modules
* Unlock user items on logout
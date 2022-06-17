# Pipelines

Use the following in your scripts to get access to the arguments passed to the processor.

```powershell
 $pipelineArgs = Get-Variable -Name pipelineArgs -ValueOnly
```

Configure any **Enable** rules to ensure your script only runs when necessary.

## Logging In

**Note:** Examples included in the following modules

* Enforce user password expiration

```powershell
$pipelineArgs.Username
$pipelineArgs.Password
$pipelineArgs.Success
$pipelineArgs.StartUrl
```

## Logged In

**Note:** Examples included in the following modules

* Automatically show quick info section

```powershell
$pipelineArgs.Username
$pipelineArgs.StartUrl
```

## Logout

**Note:** Examples included in the following modules

* Unlock user items on logout


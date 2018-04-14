# Pipelines

Use the following in your scripts to get access to the arguments passed to the processor.

```text
 $pipelineArgs = Get-Variable -Name pipelineArgs -ValueOnly
```

## Logging In

**Note:** Examples included in the following modules

* Enforce user password expiration

```text
$pipelineArgs.Username
$pipelineArgs.Password
$pipelineArgs.Success
$pipelineArgs.StartUrl
```

## Logged In

**Note:** Examples included in the following modules

* Automatically show quick info section

```text
$pipelineArgs.Username
$pipelineArgs.StartUrl
```

## Logout

**Note:** Examples included in the following modules

* Unlock user items on logout


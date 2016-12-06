# Interface Configuration

### Colors

The Console, ISE, and Result dialog all provide a way a way to view output data. The `$host` variable provides access to configuring the colors for this output data. 

**Example:** The following configures colors for the background and forground text in multiple streams.

```powershell
$Host.UI.RawUI.BackgroundColor = ($bckgrnd = 'DarkRed')
$Host.UI.RawUI.ForegroundColor = 'Cyan'
$Host.PrivateData.WarningForegroundColor = 'Magenta'
$Host.PrivateData.WarningBackgroundColor = "Green"
$Host.PrivateData.VerboseForegroundColor = 'Green'
$Host.PrivateData.VerboseBackgroundColor = "Red"
Write-Host " Write-Host "
Write-Verbose " Write-Verbose " -Verbose
Write-Warning " Write-Warning "
Show-Result -Text -Width 500 -Height 300
```

![Host output using Show-Result](https://cloud.githubusercontent.com/assets/1209953/12344464/a0b0c3ba-bb3d-11e5-82a7-449644e00602.png)
# Get-PlaceholderSetting 
 
The Get-PlaceholderSetting command returns PlaceholderSettingDefinition(s) for an item using the filtering parameters.  
 
## Syntax 
 
Get-PlaceholderSetting -Item &lt;Item&gt; [-Key &lt;string&gt;] [-PlaceholderSetting &lt;Item&gt;] [-Index &lt;int&gt;] [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting -Item &lt;Item&gt; -Instance &lt;PlaceholderDefinition&gt; [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;]
 
Get-PlaceholderSetting -Item &lt;Item&gt; -UniqueId &lt;string&gt; [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting -Path &lt;string&gt; [-Key &lt;string&gt;] [-PlaceholderSetting &lt;Item&gt;] [-Index &lt;int&gt;] [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting -Path &lt;string&gt; -Instance &lt;PlaceholderDefinition&gt; [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting -Path &lt;string&gt; -UniqueId &lt;string&gt; [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting [-Id &lt;string&gt;] [-Database &lt;string&gt;] [-Key &lt;string&gt;] [-PlaceholderSetting &lt;Item&gt;] [-Index &lt;int&gt;] [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting -Instance &lt;PlaceholderDefinition&gt; [-Id &lt;string&gt;] [-Database &lt;string&gt;] [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
 
Get-PlaceholderSetting -UniqueId &lt;string&gt; [-Id &lt;string&gt;] [-Database &lt;string&gt;] [-Device &lt;DeviceItem&gt;] [-FinalLayout] [-Language &lt;string[]&gt;] 
  
## Detailed Description 
 
The Get-PlaceholderSetting command returns PlaceholderSettingDefinition(s) for an item using the filtering parameters. 
 
© 2010-2018 Adam Najmanowicz, Michael West. All rights reserved. Sitecore PowerShell Extensions 

## Parameters 
 
### -Database&nbsp; &lt;string&gt; 
 
Database containing the item to be processed - can work with Language parameter to narrow the publication scope. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Device&nbsp; &lt;DeviceItem&gt; 
 
Device for which the placeholder settings will be retrieved. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -FinalLayout&nbsp; &lt;switch&gt; 
 
Targets the Final Layout. If not provided, the Shared Layout will be targeted. Applies to Sitecore 8.0 and higher only.
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Id&nbsp; &lt;string&gt; 
 
Id of the item to be processed - can work with Language parameter to narrow the publication scope. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Index&nbsp; &lt;int&gt; 
 
Index at which the placeholder setting exists in the layout. The placeholder setting at that index will be returned.  
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Instance&nbsp; &lt;PlaceholderDefinition&gt; 
 
Specific instance of placeholder setting that should be returned. The instance could earlier be obtained through e.g. use of Get-PlaceholderSetting. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>true</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Item&nbsp; &lt;Item&gt; 
 
The item to be processed.
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>true</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>true (ByValue, ByPropertyName)</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Key&nbsp; &lt;string&gt; 
 
Placeholder key filter - supports wildcards.
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>true</td>
        </tr>
    </tbody>
</table> 
 
### -Language&nbsp; &lt;string[]&gt; 
 
Language that will be used as source language. If not specified the current user language will be used. Globbing/wildcard supported. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>Languages</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -Path&nbsp; &lt;string&gt; 
 
Path to the item to be processed - can work with Language parameter to narrow the publication scope. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>FullName, FileName</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>true</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -PlaceholderSetting&nbsp; &lt;Item&gt; 
 
Item representing the placeholder setting. If matching, the placeholder setting will be returned. 
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
### -UniqueId&nbsp; &lt;string&gt; 
 
UniqueID of the placeholder setting to be retrieved.
 
<table>
    <thead></thead>
    <tbody>
        <tr>
            <td>Aliases</td>
            <td>None</td>
        </tr>
        <tr>
            <td>Required?</td>
            <td>true</td>
        </tr>
        <tr>
            <td>Position?</td>
            <td>Named</td>
        </tr>
        <tr>
            <td>Default Value</td>
            <td></td>
        </tr>
        <tr>
            <td>Accept Pipeline Input?</td>
            <td>false</td>
        </tr>
        <tr>
            <td>Accept Wildcard Characters?</td>
            <td>false</td>
        </tr>
    </tbody>
</table> 
 
## Inputs 
 
The input type is the type of the objects that you can pipe to the cmdlet. 
 
* Sitecore.Data.Items.Item 
 
## Outputs 
 
The output type is the type of the objects that the cmdlet emits. 
 
* Sitecore.Layouts.PlaceholderDefinition
 
## Notes 
 
Help Author: Adam Najmanowicz, Michael West, Alex Washtell

## Examples 
 
### EXAMPLE 
 
Get all placeholder settings for "Default" device, using any key matching the wildcard "content*"
 
```text
PS master:\> Get-Item master:\content\home | Get-PlaceholderSetting -Key "content*" -Device (Get-LayoutDevice "Default") 
``` 
 
## Related Topics 
 

* <a href='https://github.com/SitecorePowerShell/Console/' target='_blank'>https://github.com/SitecorePowerShell/Console/</a><br/>
* [Add-PlaceholderSetting](add-placeholdersetting.md)
* [New-PlaceholderSetting](new-placeholdersetting.md)
* [Remove-PlaceholderSetting](remove-placeholdersetting.md)



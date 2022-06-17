---
description: Examples for managing item templates.
---

# Manage Templates

## Change Template

```powershell
# Sample Item
$sourceTemplate = Get-Item -Path "master:\{76036F5E-CBCE-46D1-AF0A-4143F9B557AA}"
# Sample Content
$targetTemplate = Get-Item -Path "master:\{93A8866B-972F-4FBF-8FD9-D6004B18C0AF}"

# Use Get-ItemReferrer to find all items referencing the template, rather than scanning the content tree.
$sourceTemplate | Get-ItemReferrer | 
    Where-Object { $PSItem.TemplateId -eq $sourceTemplate.ID -and $PSItem.Paths.IsContentItem } |
    ForEach-Object {
        Set-ItemTemplate -Item $PSItem -TemplateItem $targetTemplate
    }
```

# Sitecore Stack Exchange

The following examples are best kept on SSE since it provides more context about the problem being solved.

* [Question](https://sitecore.stackexchange.com/a/15168/95) Find all items based on a template found anywhere in the inheritance chain.
---
description: Examples for managing item templates.
---

# Manage Templates

## Change Template

```text
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
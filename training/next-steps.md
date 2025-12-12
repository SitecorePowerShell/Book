---
description: Where to go after completing the training guide.
---

# Next Steps

Congratulations on completing the SPE training guide! You now have a solid foundation in PowerShell and SPE. Here's where to go next on your journey to mastery.

## Continue Learning

### Deepen Your Knowledge

#### Working with Items

Master the core operations for manipulating Sitecore content:

- [Working with Items](../working-with-items/README.md) - Complete guide to item manipulation
- [Retrieving Items](../working-with-items/retrieving-items.md) - Advanced querying techniques
- [Editing Items](../working-with-items/editing-items.md) - Proper item editing workflows
- [Creating and Removing Items](../working-with-items/creating-and-removing-items.md) - Item lifecycle management
- [Moving and Copying Items](../working-with-items/moving-and-copying-items.md) - Reorganizing content
- [Item Languages](../working-with-items/item-languages.md) - Working with multilingual content
- [Item Security](../working-with-items/item-security.md) - Managing permissions with PowerShell
- [Best Practices](../working-with-items/best-practices.md) - Tips for writing efficient scripts

#### User Interfaces

Build rich, interactive experiences for your scripts:

- [Interactive Dialogs](../interfaces/interactive-dialogs.md) - Build rich UI for your scripts
- [Show-ListView](../appendix/common/show-listview.md) - Display data in tables with actions
- [Read-Variable](../appendix/common/read-variable.md) - Create custom input forms
- [Show-FieldEditor](../appendix/common/show-fieldeditor.md) - Edit item fields in dialogs
- [Show-Result](../appendix/common/show-result.md) - Display formatted output

#### Building Useful Tools

Create custom tools that extend Sitecore functionality:

- [Creating Reports](../modules/integration-points/reports/authoring-reports.md) - Build custom content reports
- [Creating Tasks](../modules/integration-points/tasks/authoring-tasks.md) - Schedule automated jobs
- [Content Editor Integration](../modules/integration-points/content-editor.md) - Add context menu items
- [Toolbox Scripts](../modules/integration-points/toolbox.md) - Create administrative tools
- [Control Panel](../modules/integration-points/control-panel.md) - Add Control Panel applications
- [Event Handlers](../modules/integration-points/event-handlers.md) - React to Sitecore events

#### Advanced Topics

Take your skills to the next level:

- [Remoting](../remoting.md) - Automate Sitecore from external scripts
- [Packaging](../modules/packaging.md) - Package and deploy your work
- [Workflows](../modules/integration-points/workflows.md) - Integrate with Sitecore workflows
- [Web API](../modules/integration-points/web-api.md) - Expose scripts via REST API
- [Libraries and Scripts](../modules/libraries-and-scripts.md) - Organize reusable code

## Reference Documentation

### Command Reference

Browse the complete command reference in the [Appendix](../appendix/README.md):

- [Common Commands](../appendix/common/README.md) - Core SPE cmdlets (100+ commands)
- [Indexing](../appendix/indexing/README.md) - Content Search integration
- [Packaging](../appendix/packaging/README.md) - Package management
- [Presentation](../appendix/presentation/README.md) - Layout and rendering manipulation
- [Security](../appendix/security/README.md) - User, role, and permission management

### Code Snippets

Ready-to-use examples for common scenarios:

- [Field Types](../code-snippets/field-types.md) - Working with different field types
- [Item Links](../code-snippets/item-links.md) - Managing item references
- [Manage Templates](../code-snippets/manage-templates.md) - Template operations

## Practice Projects

Here are some project ideas to practice your skills:

### Beginner Projects

1. **Content Audit Report**
   - Find all items without workflow
   - List items with empty required fields
   - Identify orphaned media items
   - Export results to CSV

2. **Bulk Field Updater**
   - Prompt user for template and field name
   - Find all items of that template
   - Update the specified field
   - Show progress and results

3. **Publishing Dashboard**
   - Show items published in last 24 hours
   - Compare master vs web databases
   - Display publishing statistics
   - Highlight unpublished changes

### Intermediate Projects

4. **Content Migration Tool**
   - Copy items between databases
   - Preserve all field values and versions
   - Handle item relationships
   - Create detailed log

5. **Security Auditor**
   - List all users and their roles
   - Show items with custom security
   - Identify security inheritance breaks
   - Export security matrix

6. **Template Analyzer**
   - Find all templates
   - Show template inheritance tree
   - List fields for each template
   - Identify unused templates

### Advanced Projects

7. **Custom Reporting Engine**
   - Build reusable report framework
   - Support multiple output formats
   - Add scheduling capabilities
   - Email reports automatically

8. **Content Health Monitor**
   - Check for broken links
   - Validate media items exist
   - Find duplicate items
   - Schedule regular scans

9. **Deployment Assistant**
   - Compare environments
   - Package changed items
   - Generate deployment scripts
   - Validate post-deployment

## Essential Security Review

{% hint style="danger" %}
**CRITICAL**: Before deploying SPE to any non-development environment:

1. **Review the security guide**: [Security Hardening](../security/README.md)
2. **Complete the checklist**: [Security Checklist](../security/security-checklist.md)
3. **Configure policies**: [Security Policies](../security/security-policies.md)
4. **Set up proper access**: [Users and Roles](../security/users-and-roles.md)
5. **Never expose to internet**: [IIS Security](../security/iis-security.md)
6. **Enable monitoring**: [Logging and Monitoring](../security/logging-and-monitoring.md)

**Remember**:
- SPE should **NEVER** be installed on CD (Content Delivery) servers
- SPE should **NEVER** be accessible from internet-facing servers
- Always test scripts in development before running in production
- Use [Session Elevation](../security/session-elevation.md) for administrative tasks
- Follow the [Minimal Deployment](../security/minimal-deployment.md) guide for production
{% endhint %}

## Get Help and Support

### Built-in Help

```powershell
# Get help for any command
Get-Help Get-Item

# Get detailed help with examples
Get-Help Get-Item -Detailed

# Get just the examples
Get-Help Get-Item -Examples

# Search for commands
Get-Command *Item*

# See object properties and methods
$item | Get-Member
```

### Documentation

- **This book**: Comprehensive reference for all SPE features
- [Troubleshooting Guide](../troubleshooting.md) - Common issues and solutions
- [Interface Help](../interfaces/help.md) - Using the built-in help system

### Community

- **Slack**: Join [#module-spe](../community.md) on Sitecore Community Slack
- **GitHub**: Report issues or request features at [GitHub](https://github.com/SitecorePowerShell/Console)
- **Stack Exchange**: Ask questions on [Sitecore Stack Exchange](https://sitecore.stackexchange.com/questions/tagged/powershell-extensions)
- **Blogs**: Read community content at [Blog Collection](https://blog.najmanowicz.com/sitecore-powershell-console/)

## Best Practices Checklist

As you continue working with SPE, keep these best practices in mind:

### Code Quality

- ✅ Use full command names (not aliases) in scripts
- ✅ Write clear, descriptive variable names
- ✅ Add comments to explain complex logic
- ✅ Use functions for reusable code
- ✅ Handle errors with try/catch
- ✅ Test scripts in development first

### Performance

- ✅ Filter early in pipelines
- ✅ Use Content Search for large queries
- ✅ Avoid `-Recurse` on large trees
- ✅ Use efficient collection types
- ✅ Suppress output properly (`> $null`)
- ✅ Measure performance with `Measure-Command`

### Security

- ✅ Never run untrusted scripts
- ✅ Use source control for all scripts
- ✅ Review security settings regularly
- ✅ Apply principle of least privilege
- ✅ Enable audit logging
- ✅ Test security policies

### Maintenance

- ✅ Document what scripts do
- ✅ Use consistent naming conventions
- ✅ Organize scripts in Script Library
- ✅ Version control your work
- ✅ Share knowledge with team
- ✅ Review and refactor regularly

## Learning Resources

### Official Resources

- [Video Series](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b) - Official tutorial videos
- [Blog Collection](https://blog.najmanowicz.com/sitecore-powershell-console/) - Community articles
- [GitHub Repository](https://github.com/SitecorePowerShell/Console) - Source code and examples

### PowerShell Learning

While SPE is Sitecore-specific, general PowerShell knowledge helps:

- [Microsoft PowerShell Documentation](https://docs.microsoft.com/en-us/powershell/)
- [PowerShell Gallery](https://www.powershellgallery.com/) - Community modules
- [about_* Topics](about:blank) - Run `help about_*` in PowerShell for core concepts

## Share Your Work

As you build useful scripts and tools:

### Contribute Back

- **Submit examples**: Share your scripts on GitHub
- **Write blog posts**: Document your solutions
- **Answer questions**: Help others on Slack and Stack Exchange
- **Create packages**: Package useful tools for the community
- **Improve documentation**: Submit pull requests to this book

### Build Your Portfolio

- **GitHub repository**: Maintain a collection of your SPE scripts
- **Blog series**: Write about real-world problems you've solved
- **Presentations**: Share your knowledge at user groups
- **Open source**: Create and maintain SPE extensions

## Continuous Improvement

Keep learning and improving:

1. **Read the release notes**: Stay current with new SPE features
2. **Explore the Script Library**: Learn from bundled examples
3. **Review community scripts**: See how others solve problems
4. **Experiment safely**: Try new techniques in development
5. **Ask questions**: Don't hesitate to ask the community
6. **Teach others**: The best way to master something is to teach it

## Quick Reference Card

### Essential Commands

```powershell
# Items
Get-Item -Path "master:\content\home"
Get-ChildItem -Path "master:\content" -Recurse
New-Item -Path "master:\content\home" -Name "Item"
Remove-Item -Path "master:\content\home\item"

# Filtering
... | Where-Object { $_.TemplateName -eq "Article" }
... | Select-Object -Property Name, ID, TemplateName
... | Sort-Object -Property Name

# Editing
$item.Editing.BeginEdit()
$item["Title"] = "New Value"
$item.Editing.EndEdit()

# Publishing
Publish-Item -Item $item -PublishMode Smart -Target web

# Help
Get-Help Get-Item
Get-Command *Item*
$item | Get-Member
```

### Common Patterns

```powershell
# Safe editing
foreach($item in $items) {
    $item.Editing.BeginEdit()
    try {
        $item["Field"] = "Value"
        $item.Editing.EndEdit()
    }
    catch {
        $item.Editing.CancelEdit()
        Write-Error $_
    }
}

# Progress reporting
$total = $items.Count
for($i = 0; $i -lt $total; $i++) {
    Write-Progress -Activity "Processing" `
                   -Status "$i of $total" `
                   -PercentComplete (($i / $total) * 100)
    # Process item
}
```

---

## You're Ready!

You've completed the training and now have the skills to:

- ✅ Write PowerShell scripts for Sitecore
- ✅ Understand providers, pipelines, and commands
- ✅ Avoid common pitfalls
- ✅ Build useful automation tools
- ✅ Work safely and securely

**Happy scripting!** 🚀

{% hint style="success" %}
Remember: The best way to learn is by doing. Start small, experiment often, and don't be afraid to make mistakes in development. You've got this!
{% endhint %}

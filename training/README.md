
---
description: Comprehensive guide to learning SPE from beginner to advanced.
---

# Training

The world renowned Sitecore PowerShell Extensions module has so much to offer, but sometimes those new to the module may find it difficult to know where to start. This training guide provides you with everything you need to use and be productive with SPE.

Don't worry, you will be able to use it without having to write any code.

## Learning Path

This guide provides a progressive roadmap from beginner to advanced SPE user. Follow this path to build your skills systematically:

### Beginner - Get Started

1. **Get up and running** - Start with [Getting Started](getting-started.md)
2. **Watch the basics** - View our [video series](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b)
3. **Learn PowerShell syntax** - Study [Language Basics](language-basics.md)
4. **Master commands** - Read [Commands and Pipelines](commands-and-pipelines.md)
5. **Understand providers** - Learn [Providers](providers.md)
6. **Practice with examples** - Try [Your First Scripts](first-scripts.md)
7. **Avoid mistakes** - Review [Common Pitfalls](common-pitfalls.md)

### Intermediate - Build Skills

8. **Retrieve Sitecore items** - Learn to query and navigate → [Retrieving Items](../working-with-items/retrieving-items.md)
9. **Modify content** - Edit, create, and delete → [Editing Items](../working-with-items/editing-items.md), [Creating Items](../working-with-items/creating-and-removing-items.md)
10. **Move and copy items** - Reorganize content → [Moving and Copying Items](../working-with-items/moving-and-copying-items.md)
11. **Use interactive dialogs** - Build user interfaces → [Interactive Dialogs](../interfaces/interactive-dialogs.md)
12. **Explore integration points** - Extend Sitecore UI → [Integration Points](../modules/integration-points/README.md)

### Advanced - Master SPE

13. **Create custom reports** - Build powerful analysis tools → [Authoring Reports](../modules/integration-points/reports/authoring-reports.md)
14. **Build reusable libraries** - Organize your scripts → [Libraries and Scripts](../modules/libraries-and-scripts.md)
15. **Automate with tasks** - Schedule automated jobs → [Authoring Tasks](../modules/integration-points/tasks/authoring-tasks.md)
16. **Use remoting** - Control Sitecore from external scripts → [Remoting](../remoting.md)
17. **Package your work** - Deploy scripts as packages → [Packaging](../modules/packaging.md)
18. **Plan your next steps** - Review [Next Steps](next-steps.md)

### Critical - Secure Your Installation

{% hint style="danger" %}
**BEFORE deploying to any environment**, you MUST review security:
- [Security Hardening Guide](../security/README.md) - Overview of security concepts
- [Security Checklist](../security/security-checklist.md) - Step-by-step hardening
- [Security Policies](../security/security-policies.md) - Configure access control
- **NEVER** install SPE on Content Delivery (CD) servers
- **NEVER** deploy SPE on internet-facing instances
{% endhint %}

## Training Modules

Follow the complete learning path:

### 1. 🚀 [Getting Started](getting-started.md)
**Start here!** Get up and running in minutes:
- Opening the Console and ISE
- Running your first command
- Understanding the basics
- Quick reference guide

### 2. 📖 [Language Basics](language-basics.md)
Learn PowerShell syntax (especially for C# developers):
- C# to PowerShell translation
- Variables, strings, and collections
- Comparison operators
- Loops and conditions
- Working with .NET types

### 3. ⚡ [Commands and Pipelines](commands-and-pipelines.md)
Master PowerShell commands:
- Command syntax (Verb-Noun pattern)
- Essential commands
- Pipeline fundamentals
- Getting help
- Best practices

### 4. 🗂️ [Providers](providers.md)
Understand the provider architecture:
- What are providers?
- The Sitecore provider
- Path formats
- Switching between providers

### 5. 🎯 [Your First Scripts](first-scripts.md)
Practice with hands-on examples:
- Find items by workflow state
- Bulk update item fields
- Generate content reports
- Interactive user input
- Practice exercises

### 6. ⚠️ [Common Pitfalls](common-pitfalls.md)
Avoid beginner mistakes:
- Provider path confusion
- Item editing context issues
- Performance problems
- Security oversights

### 7. 🌟 [Next Steps](next-steps.md)
Continue your SPE journey:
- Advanced learning paths
- Practice projects
- Command reference
- Community resources

## Learning Resources

### Official Documentation
- **This book** - Comprehensive reference for all SPE features
- [Command Reference](../appendix/common/README.md) - Complete list of SPE cmdlets
- [Code Snippets](../code-snippets/README.md) - Ready-to-use examples

### Video Tutorials
- [SPE Video Series](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b) - Beginner walkthroughs
- [Blogs and Videos Collection](https://blog.najmanowicz.com/sitecore-powershell-console/) - Community content

### Hands-On Practice
- [Console](../interfaces/console.md) - Interactive PowerShell terminal in Sitecore
- [ISE (Integrated Scripting Environment)](../interfaces/scripting.md) - Full-featured script editor with IntelliSense
- [Training Modules](../README.md#bundled-tools) - Sample scripts included with SPE

## After Training

Once you've completed all the training modules, see [Next Steps](next-steps.md) for:
- Advanced learning paths
- Practice project ideas
- Complete command reference
- Community resources and support

## Get Help

If you get stuck or have questions:
- **Built-in help**: Use `Get-Help <command-name>` in the Console
- **Troubleshooting**: [Troubleshooting Guide](../troubleshooting.md)
- **Community**: Join the [#module-spe](../community.md) channel on Sitecore Slack
- **GitHub**: Report issues or request features at [GitHub](https://github.com/SitecorePowerShell/Console)

Happy scripting!

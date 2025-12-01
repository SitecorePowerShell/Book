# Sitecore PowerShell Extensions Documentation

This repository contains the documentation guide for the Sitecore PowerShell Extensions (SPE) module.

## Project Overview

**Project Type**: Documentation / GitBook
**Technology**: GitBook-based documentation
**Purpose**: Comprehensive documentation for the Sitecore PowerShell Extensions module

## About SPE

The Sitecore PowerShell Extensions (SPE) is a Sitecore development accelerator that provides:

- Command line interface (CLI) for Sitecore
- Integrated Scripting Environment (ISE)
- PowerShell-based automation for Sitecore tasks
- Content management and reporting tools

## Repository Structure

- `/appendix/` - Appendix sections covering common topics (language, indexing, packaging, security, session)
- `/modules/` - Documentation for SPE modules and integration points
- `/interfaces/` - Documentation for user interfaces (Console, ISE, Interactive Dialogs)
- `/security/` - Security hardening and best practices
- `/.gitbook/assets/` - Screenshots and images used in documentation
- `/gitbook/` - GitBook configuration and theme files

## Key Documentation Areas

1. **Installation & Setup** - Getting started with SPE
2. **Security** - Critical security configurations and best practices
3. **Interfaces** - Console, ISE, and interactive dialogs
4. **Integration Points** - Reports, toolbox, content editor extensions
5. **Remoting** - Remote automation capabilities
6. **Modules** - Bundled tools and extensions

## Development Team

- Adam Najmanowicz (@adamnaj)
- Michael West (@michaelwest101)

## Documentation Guidelines

- Use GitBook markdown format
- Include screenshots in `.gitbook/assets/` directory
- Follow existing documentation structure and style
- Reference Sitecore API and PowerShell cmdlets accurately
- Keep security warnings prominent for sensitive features

## Important Notes

- SPE should NOT be installed on Content Delivery (CD) instances
- SPE should NOT be deployed on internet-facing servers
- Security configurations are critical - documented in `/security/README.md`
- The module provides powerful capabilities that require proper security hardening

## Links

- Main Repository: https://github.com/SitecorePowerShell/Console
- Documentation: https://github.com/SitecorePowerShell/Book
- Slack: #module-spe on Sitecore Community Slack

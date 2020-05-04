# Appendix

[Downloads are hosted on Github](https://github.com/SitecorePowerShell/Console/releases/)

## Compatibility Table

| SPE ↓ / Sitecore→ | **7.0** | **7.1** | **7.2** | **7.5** | **8.0** | **8.1** | **8.2** | **9.0** | **9.1** | **9.2** | **9.3**
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 3.0-3.3 | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | – | – | – | – |
| 4.0 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | – | – | – |
| 4.1-4.6 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | – | – |
| 4.7 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | – | – |
| 5.0 | – | – | – | – | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ [[1]](#Compatibility-Notes) [[2]](#Compatibility-Notes) | – |
| 5.1 | – | – | – | – | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ [[2]](#Compatibility-Notes) | – |
| 6.0 | – | – | – | – | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

**Legend:** "–" - not supported; "✓" - supported.

### Compatibility Notes

- [1] Released by Sitecore for SXA. Download from the [SXA 1.9.0 release page](https://dev.sitecore.net/Downloads/Sitecore_Experience_Accelerator/19/Sitecore_Experience_Accelerator_190.aspx)
- [2] Some features are not available. It is recommended to upgrade to 6.0 for 9.2.
- - Publish-Item command unsupported ([Issue #1129](https://github.com/SitecorePowerShell/Console/issues/1129))
- - Indexing commands unsupported ([Issue #1133](https://github.com/SitecorePowerShell/Console/issues/1133))

### Releases

When reviewing the different download packages you may see some names that are not too clear. The following list outlines what those names should mean.

* **N.X : Full N.X release** - This refers to the package used by Standalone and CM roles. This includes what is required to see the PowerShell ISE, Console and their associated services.
* **N.X : Server-side remoting only** - This refers to the package with only files. Useful for remotely connecting to SPE.
* **N.X Authorable Reports : Additional reports and tools** - This package is a sublemental installation to the full version with additional reports.
* **N.X Remoting : SPE Remoting module for CI/CD** - This provides a Windows PowerShell module for connecting to SPE remotely. Use in combination with the full or minimal packages.
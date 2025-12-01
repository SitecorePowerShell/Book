# File Upload Restrictions

File upload capabilities in SPE can be restricted by file type and destination path. This provides defense in depth by preventing malicious file uploads even if the upload service is enabled.

## Overview

When the [File Upload service](web-services.md) is enabled, SPE enforces restrictions on:

- **File Types** - Which file extensions and MIME types can be uploaded
- **Upload Locations** - Which directories files can be written to

These restrictions apply to uploads via:

- SPE Remoting file upload
- Custom scripts using upload functionality

{% hint style="info" %}
This feature was introduced in SPE with [#1362](https://github.com/SitecorePowerShell/Console/issues/1362)
{% endhint %}

## Default Configuration

By default, SPE allows limited file types and restricts uploads to the Sitecore temp folder:

```xml
<powershell>
  <uploadFile>
    <allowedFileTypes>
      <pattern>image/*</pattern>
      <pattern>.xls</pattern>
      <pattern>.xlsx</pattern>
      <pattern>.csv</pattern>
    </allowedFileTypes>
    <allowedLocations>
      <path>$SitecoreMediaFolder</path>
    </allowedLocations>
  </uploadFile>
</powershell>
```

## File Type Restrictions

### How It Works

File types can be restricted using:

1. **File Extensions** - Specific extensions like `.csv`, `.txt`, `.xml`
2. **MIME Type Patterns** - Wildcards like `image/*`, `text/*`, `application/json`

### Configuration Examples

#### Allow Specific Extensions

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <uploadFile>
        <allowedFileTypes>
          <pattern>.csv</pattern>
          <pattern>.txt</pattern>
          <pattern>.xml</pattern>
          <pattern>.json</pattern>
        </allowedFileTypes>
      </uploadFile>
    </powershell>
  </sitecore>
</configuration>
```

#### Allow MIME Type Categories

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <uploadFile>
        <allowedFileTypes>
          <!-- All image types -->
          <pattern>image/*</pattern>

          <!-- All text types -->
          <pattern>text/*</pattern>

          <!-- Specific MIME type -->
          <pattern>application/json</pattern>
          <pattern>application/xml</pattern>
        </allowedFileTypes>
      </uploadFile>
    </powershell>
  </sitecore>
</configuration>
```

#### Allow Office Documents

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <uploadFile>
        <allowedFileTypes>
          <!-- Excel -->
          <pattern>.xls</pattern>
          <pattern>.xlsx</pattern>
          <pattern>application/vnd.ms-excel</pattern>
          <pattern>application/vnd.openxmlformats-officedocument.spreadsheetml.sheet</pattern>

          <!-- Word -->
          <pattern>.doc</pattern>
          <pattern>.docx</pattern>
          <pattern>application/msword</pattern>
          <pattern>application/vnd.openxmlformats-officedocument.wordprocessingml.document</pattern>

          <!-- PowerPoint -->
          <pattern>.ppt</pattern>
          <pattern>.pptx</pattern>
          <pattern>application/vnd.ms-powerpoint</pattern>
          <pattern>application/vnd.openxmlformats-officedocument.presentationml.presentation</pattern>
        </allowedFileTypes>
      </uploadFile>
    </powershell>
  </sitecore>
</configuration>
```

### Common File Type Patterns

| File Type        | Extension Pattern       | MIME Type Pattern                       |
| :--------------- | :---------------------- | :-------------------------------------- |
| **Images**       | `.jpg`, `.png`, `.gif`  | `image/*`                               |
| **Documents**    | `.pdf`, `.doc`, `.docx` | `application/pdf`, `application/msword` |
| **Spreadsheets** | `.xls`, `.xlsx`, `.csv` | `application/vnd.ms-excel`, `text/csv`  |
| **Text Files**   | `.txt`, `.log`          | `text/plain`, `text/*`                  |
| **Data Files**   | `.json`, `.xml`         | `application/json`, `application/xml`   |
| **Archives**     | `.zip`                  | `application/zip`                       |

{% hint style="danger" %}
**Security Warning:** Be extremely careful allowing executable file types like `.exe`, `.dll`, `.ps1`, `.bat`, `.cmd`, `.vbs`, `.js`. These can be used to compromise the server.
{% endhint %}

## Upload Location Restrictions

### How It Works

SPE restricts where files can be uploaded using path patterns. These paths support Sitecore variables.

### Sitecore Path Variables

| Variable                 | Resolves To    | Typical Path                           |
| :----------------------- | :------------- | :------------------------------------- |
| `$SitecoreDataFolder`    | Data folder    | `C:\inetpub\wwwroot\App_Data`          |
| `$SitecoreLogFolder`     | Log folder     | `C:\inetpub\wwwroot\App_Data\logs`     |
| `$SitecorePackageFolder` | Package folder | `C:\inetpub\wwwroot\App_Data\packages` |
| `$SitecoreTempFolder`    | Temp folder    | `C:\inetpub\wwwroot\temp`              |
| `$SitecoreMediaFolder`   | Media folder   | `C:\inetpub\wwwroot\upload`            |

### Configuration Examples

#### Allow Only Temp Folder (Most Secure)

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <uploadFile>
        <allowedLocations>
          <path>$SitecoreTempFolder</path>
        </allowedLocations>
      </uploadFile>
    </powershell>
  </sitecore>
</configuration>
```

{% hint style="success" %}
**Recommended:** Standardize your solution with a folder that you will routinely clean.
{% endhint %}

#### Allow Multiple Locations

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <uploadFile>
        <allowedLocations>
          <path>$SitecoreTempFolder</path>
          <path>$SitecoreDataFolder\uploads</path>
          <path>$SitecorePackageFolder</path>
        </allowedLocations>
      </uploadFile>
    </powershell>
  </sitecore>
</configuration>
```

### Dangerous Locations to NEVER Allow

❌ **Never allow uploads to these locations:**

```xml
<!-- DO NOT DO THIS -->
<allowedLocations>
  <path>C:\</path>                    <!-- Root drive -->
  <path>C:\Windows</path>             <!-- Windows directory -->
  <path>C:\inetpub\wwwroot</path>     <!-- Web root (allows direct access) -->
</allowedLocations>
```

## Complete Configuration Examples

### Secure Configuration (Data Import Use Case)

Allow CSV/Excel imports to a dedicated import folder:

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <uploadFile>
        <!-- Only allow data file types -->
        <allowedFileTypes>
          <pattern>.csv</pattern>
          <pattern>.xls</pattern>
          <pattern>.xlsx</pattern>
          <pattern>text/csv</pattern>
          <pattern>application/vnd.ms-excel</pattern>
          <pattern>application/vnd.openxmlformats-officedocument.spreadsheetml.sheet</pattern>
        </allowedFileTypes>

        <!-- Only allow specific import locations -->
        <allowedLocations>
          <path>$SitecoreTempFolder</path>
          <path>$SitecoreDataFolder\uploads</path>
        </allowedLocations>
      </uploadFile>
    </powershell>
  </sitecore>
</configuration>
```

## Best Practices

### Security Recommendations

✅ **Do:**

- Use the principle of least privilege - only allow necessary file types
- Restrict uploads to non-web-accessible folders when possible
- Use `$SitecoreTempFolder` as the primary upload location
- Create dedicated subdirectories for different upload purposes
- Regularly clean up uploaded files
- Monitor upload activity
- Use both extension AND MIME type patterns for defense in depth
- Test your restrictions before deploying to production

❌ **Don't:**

- Allow executable file types (`.exe`, `.dll`, `.ps1`, etc.)
- Allow uploads to the web root or `bin` folder
- Allow uploads to system directories
- Use wildcard MIME types like `*/*` (allows everything)
- Forget to restrict both file types AND locations
- Allow script files that could be executed
- Trust client-provided file extensions without validation

### Configuration Strategy

1. **Start Restrictive** - Begin with minimal allowed types and locations
2. **Add as Needed** - Only add permissions when you have a specific use case
3. **Document Reasons** - Comment your configuration explaining why each type/location is allowed
4. **Regular Review** - Audit allowed types and locations quarterly

## Troubleshooting

### Upload fails even though file type is allowed

**Possible causes:**

1. Location is not in `allowedLocations`
2. MIME type doesn't match pattern
3. File extension doesn't match pattern

**Solution:** Check both file type and location configuration.

### MIME type wildcard doesn't work

**Cause:** Pattern syntax error or MIME type mismatch.

**Solution:** Use specific MIME types or verify wildcard syntax (`image/*`, not `image*`).

### Variable like $SitecoreTempFolder doesn't resolve

**Cause:** Variable not recognized by SPE configuration.

**Solution:** Use supported Sitecore variables or absolute paths.

## Related Topics

- [Web Services Security](web-services.md) - Enable/disable file upload service
- [Security Checklist](security-checklist.md) - Validate your configuration
- [Logging and Monitoring](logging-and-monitoring.md) - Monitor upload activity

## References

- [GitHub Issue #1362](https://github.com/SitecorePowerShell/Console/issues/1362) - Feature introduction

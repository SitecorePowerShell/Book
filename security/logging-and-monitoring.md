# Logging and Monitoring

Comprehensive logging and monitoring are essential for detecting security incidents, troubleshooting issues, and maintaining compliance. This guide covers how to configure and use SPE logging for security purposes.

## Overview

SPE provides logging capabilities that record:

- Script execution
- User authentication and authorization
- Session elevation events
- Delegated access usage
- Web service calls
- Errors and exceptions

{% hint style="success" %}
**Security Best Practice:** Enable comprehensive logging in all non-development environments to create an audit trail.
{% endhint %}

## SPE Log Files

### Default Log Location

SPE logs are written to:

```
$SitecoreLogFolder\SPE.log.{date}.txt
```

Typically resolves to:

```
C:\inetpub\wwwroot\App_Data\logs\SPE.log.20260101.txt
```

## Log Levels

SPE supports standard log4net levels:

| Level     | Description                     | Use Case                        |
| :-------- | :------------------------------ | :------------------------------ |
| **DEBUG** | Detailed diagnostic information | Development and troubleshooting |
| **INFO**  | Informational messages          | Production (recommended)        |
| **WARN**  | Warning messages                | Production (recommended)        |
| **ERROR** | Error messages                  | Always enabled                  |
| **FATAL** | Critical errors                 | Always enabled                  |

### Configuring Log Level

Edit your log4net configuration (typically in `App_Config\Sitecore.config` or a patch file):

```xml
<configuration>
  <log4net>
      <appender name="PowerShellExtensionsFileAppender" type="log4net.Appender.SitecoreLogFileAppender, Sitecore.Logging">
        <file value="$(dataFolder)/logs/SPE.log.{date}.txt"/>
        <appendToFile value="true"/>
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%4t %d{ABSOLUTE} %-5p %m%n"/>
        </layout>
        <encoding value="utf-8"/>
      </appender>
      <logger name="Spe" additivity="false">
        <level value="INFO"/>
        <appender-ref ref="PowerShellExtensionsFileAppender"/>
      </logger>
    </log4net>
</configuration>
```

**Recommended Settings:**

| Environment     | Log Level    | Rationale                                 |
| :-------------- | :----------- | :---------------------------------------- |
| **Development** | DEBUG        | Maximum detail for troubleshooting        |
| **QA/Staging**  | INFO         | Balance detail and volume                 |
| **Production**  | INFO or WARN | Important events without excessive detail |

## What Gets Logged

### Script Execution

**Format:**

```
INFO Arbitrary script execution in ISE by user: 'sitecore\Admin'
```

### Session Elevation

**Format:**

```
WARN Session state elevated for 'ISE' by user: sitecore\Admin
```

### Delegated Access

**Format:**

```
INFO [Gutter] Executing script {CFE81AF6-2468-4E62-8BF2-588B7CC60F80} for Context User sitecore\test as sitecore\Admin.
```

**Includes:**

- Script ID
- Context user (actual logged-in user)
- Impersonated user (elevated account)

This is critical for audit trails showing privilege escalation.

### Web Service Calls

**Format:**

```
INFO A request to the remoting service was made from IP 10.0.0.27
INFO A request to the mediaUpload service was made from IP 10.0.0.27
WARN A request to the mediaUpload service could not be completed because the provided credentials are invalid.
```

**Logged Events:**

- Remoting connections
- File uploads/downloads
- RESTful API calls
- Authorization failures

### Authentication Events

**Format:**

```
INFO [Runner] Executing script {BD07C7D1-700D-450C-B79B-8526C6643BF3} for Context User sitecore\test2 as sitecore\Admin.
```

**Logged Events:**

- Successful authentication
- Failed authentication
- Authorization denials

### Errors and Exceptions

**Format:**

```powershell
# TODO
```

**Includes:**

- Exception details
- Stack traces
- User context
- Operation being performed

## Monitoring Strategies

### Real-Time Monitoring

#### Using PowerShell to Tail Logs

```powershell
# Tail the SPE log file
# Note that the SPE Console doesn't exit when using -Wait
Get-Content -Path "$($SitecoreLogFolder)\SPE.log.20251201.txt" -Tail 10
```

```powershell
# Tail the latest SPE log file dynamically
Get-ChildItem -Path $SitecoreLogFolder -Filter "SPE.log.*.txt" | Sort-Object -Descending -Property LastWriteTime | Select-Object -First 1 | ForEach-Object { Get-Content -Path $_.FullName -Tail 10}
```

#### Monitor for Specific Events

```powershell
# Watch for tasks
Get-Content "$SitecoreLogFolder\SPE.log.20251201.txt" | Where-Object { $_ -match "\[Task\]" }
```

```powershell
# Watch for warnings
Get-Content "$SitecoreLogFolder\SPE.log.20251201.txt" | Where-Object { $_ -match "WARN" }
```

### Log Analysis

#### Find Failed Authentication Attempts

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
$failedAuth = Get-Content $logPath |
    Where-Object { $_ -match "credentials are invalid" } |
    ForEach-Object {
        if ($_ -match "(\d{2}:\d{2}:\d{2}).*WARN\s*(\S+)") {
            [PSCustomObject]@{
                Timestamp = $matches[1]
                User = $matches[2]
                LogLine = $_
            }
        }
    }

$failedAuth

# Timestamp User LogLine
# --------- ---- -------
# 11:20:23  A    1508 11:20:23 WARN  A request to the mediaUpload service could not be completed because the provided credentials are invalid.
```

#### Track Delegated Access Usage

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
$delegated = Get-Content $logPath |
    Where-Object { $_ -match "Executing script" } |
    ForEach-Object {
        if ($_ -match "(?<time>\d{2}:\d{2}:\d{2}).*Runner\s*.*Context User\s(?<user>[0-9a-zA-Z\\]*)\sas\s(?<impersonated>[0-9a-zA-Z\\]*).*") {
            [PSCustomObject]@{
                Timestamp = $matches["time"]
                ContextUser = $matches["user"]
                ImpersonatedAs = $matches["impersonated"]
                LogLine = $_
            }
        }
    }

$delegated | Group-Object ContextUser |
    Select-Object Name, Count |
    Sort-Object Count -Descending

# Name           Count
# ----           -----
# sitecore\test2     1
```

#### Find Unauthorized Access Attempts

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
$unauthorized = Get-Content $logPath |
    Where-Object { $_ -match "credentials are invalid" }

$unauthorized | ForEach-Object {
    Write-Host $_ -ForegroundColor Red -BackgroundColor White
}

# 1508 11:20:23 WARN  A request to the mediaUpload service could not be completed because the provided credentials are invalid.
```

#### Analyze Web Service Usage

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
$webServiceCalls = Get-Content $logPath |
    Where-Object { $_ -match "remoting" }

# Group by IP address
$webServiceCalls | ForEach-Object {
    if ($_ -match "IP\s*(?<ip>[\d\.]+)") {
        [PSCustomObject]@{
            IP = $matches['ip']
            LogLine = $_
        }
    }
} | Group-Object IP |
    Select-Object Name, Count |
    Sort-Object Count -Descending

# Name      Count
# ----      -----
# 10.0.0.27   105
```

### Scheduled Log Review

Create a scheduled task to analyze logs daily:

```powershell
# TODO: Example using Send-MailMessage
```

## IIS Log Integration

### IIS Logs for Web Services

IIS logs provide additional context for web service access:

**Location:**

```
C:\inetpub\logs\LogFiles\W3SVC1\
```

### Useful IIS Log Fields

| Field             | Description            | Security Value                         |
| :---------------- | :--------------------- | :------------------------------------- |
| **c-ip**          | Client IP address      | Identify source of requests            |
| **cs-username**   | Authenticated username | Track who accessed services            |
| **cs-uri-stem**   | Requested URI          | Identify which services were called    |
| **sc-status**     | HTTP status code       | Find authorization failures (401, 403) |
| **cs-User-Agent** | User agent string      | Identify automation vs browsers        |

### Analyzing IIS Logs for SPE

Find SPE web service requests:

```powershell
# TODO Inspect IIS Logs
```

Find failed authentication (401) to SPE services:

```powershell
# TODO Inspect IIS Logs
```

## Alerting

### Simple Email Alerts

Create alerts for critical security events:

```powershell
# TODO Example with Send-MailMessage
```

Run this script via Task Scheduler every 5 minutes.

### Integration with SIEM

For enterprise environments, integrate SPE logs with your Security Information and Event Management (SIEM) system.

**Common SIEM Solutions:**

- Splunk
- ELK Stack (Elasticsearch, Logstash, Kibana)
- Azure Sentinel
- ArcSight

## Log Retention

### Regulatory Requirements

Different compliance frameworks have different retention requirements:

| Framework   | Typical Requirement      |
| :---------- | :----------------------- |
| **PCI-DSS** | 1 year (3 months online) |
| **HIPAA**   | 6 years                  |
| **SOC 2**   | 1+ years                 |
| **GDPR**    | Varies by data type      |

**Note:** The requirements may have changed since the publishing of this document. Please confirm with your organization and legal team as to the requirements you must meet.

### Configuration

The configuration of log retention will be dependent on your solution.

### Archival Strategy

The archival strategy implementation will be dependent on your solution.

## Security Metrics

### Key Performance Indicators (KPIs)

Track these metrics for security monitoring:

| Metric                   | Description                     | Threshold            |
| :----------------------- | :------------------------------ | :------------------- |
| **Failed Auth Rate**     | Failed authentications per hour | Alert if > 10        |
| **Elevation Denials**    | Session elevation denials       | Alert if > 5         |
| **Delegated Access**     | Delegated access usage          | Monitor trends       |
| **Web Service 401/403**  | Unauthorized web service calls  | Alert if > 20        |
| **File Upload Attempts** | File upload failures            | Monitor for patterns |
| **Script Errors**        | Script execution errors         | Monitor trends       |

## Best Practices

### Security Recommendations

✅ **Do:**

- Enable INFO level logging in production
- Monitor logs daily for suspicious activity
- Set up alerts for critical security events
- Retain logs per compliance requirements
- Archive old logs securely
- Integrate with SIEM if available
- Document your monitoring procedures
- Review delegated access usage regularly
- Track failed authentication patterns
- Monitor web service usage for anomalies

❌ **Don't:**

- Disable logging in production
- Ignore failed authentication attempts
- Delete logs prematurely
- Log sensitive data (passwords, tokens)
- Rely solely on manual log review
- Forget to monitor IIS logs too
- Ignore ERROR and WARN messages
- Let log files consume all disk space

### Monitoring Frequency

| Activity             | Frequency  | Method                        |
| :------------------- | :--------- | :---------------------------- |
| **Real-time Alerts** | Continuous | Automated scripts/SIEM        |
| **Daily Review**     | Daily      | Automated summary email       |
| **Weekly Analysis**  | Weekly     | Manual review of trends       |
| **Monthly Audit**    | Monthly    | Comprehensive security review |
| **Quarterly Report** | Quarterly  | Executive summary             |

## Troubleshooting

### Logs not being written

**Possible causes:**

1. log4net configuration incorrect
2. File permissions prevent writing
3. Disk space full
4. SPE logger disabled

**Solution:** Check log4net config, verify permissions, ensure disk space.

### Too much log data

**Cause:** DEBUG level in production or high activity.

**Solution:** Change to INFO or WARN level, implement log rotation.

### Can't find specific events

**Cause:** Log rotation moved old logs or logs were deleted.

**Solution:** Check archived logs, adjust retention period.

## Related Topics

- [Security Checklist](security-checklist.md) - Includes logging validation
- [Delegated Access](delegated-access.md) - Monitoring delegated access usage
- [Web Services](web-services.md) - Monitoring web service security
- [IIS Security](iis-security.md) - IIS log integration

## References

- [log4net Documentation](https://logging.apache.org/log4net/)
  - Your Sitecore implementation may require vendor-specific documentation as log4net is bundled/compiled in the Sitecore libraries.
- [Sitecore Logging Documentation](https://doc.sitecore.com/xp/en/developers/latest/platform-administration-and-architecture/logging.html)
- [IIS Log File Formats](https://docs.microsoft.com/en-us/iis/manage/provisioning-and-managing-iis/configure-logging-in-iis)

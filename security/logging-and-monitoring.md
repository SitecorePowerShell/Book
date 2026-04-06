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

## Log Output Format

{% hint style="info" %}
Introduced in SPE 9.0.
{% endhint %}

SPE uses a standardized structured log format across all messages. You can choose between two output formats by configuring the setting in `Spe.config`:

| Format | Description |
| :--- | :--- |
| `keyvalue` | Default. Machine-parseable key=value pairs compatible with Splunk `KV_MODE=auto`. |
| `json` | Structured JSON objects for log aggregation tools like Splunk, ELK, and Datadog. |

### Key-Value Format (default)

```
[Category] action=verb key=value key="quoted value"
```

**Example:**

```
AUDIT (sitecore\admin) [Remoting] action=scriptStarting user=sitecore\admin ip=127.0.0.1 session=abc123 scriptHash=def456
```

### JSON Format

```json
{"type":"Remoting","action":"scriptStarting","user":"sitecore\\admin","ip":"127.0.0.1","session":"abc123","scriptHash":"def456","auditUser":"sitecore\\admin"}
```

### Configuration

```xml
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <settings>
        <setting name="Spe.LogFormat" value="json" />
      </settings>
    </powershell>
  </sitecore>
</configuration>
```

## What Gets Logged

All log messages follow the structured format with one of 24 categories. Security-relevant events are promoted to AUDIT level.

### Categories

| Category | Description |
| :--- | :--- |
| `Remoting` | Remote script execution via the remoting service |
| `Remoting(SOAP)` | SOAP-based remoting calls |
| `JWT` | JWT token creation and validation |
| `ApiKey` | API key authentication events |
| `Security` | Authorization checks and access control |
| `Trust` | Trusted authentication events |
| `DelegatedAccess` | Delegated access and impersonation |
| `Session` | Session elevation and management |
| `Console` | Console interactions |
| `ISE` | ISE script execution |
| `Runner` | Script runner execution |
| `Report` | Report execution |
| `Task` | Scheduled task execution |
| `Rule` | Rules engine script execution |
| `Provider` | PowerShell provider operations |
| `Upload` | File and media upload operations |
| `Pipeline` | Pipeline processing |
| `Command` | Command execution |
| `Dialog` | Dialog interactions |
| `Profile` | User profile operations |
| `Gutter` | Gutter script execution |
| `Timer` | Timer and performance events |
| `Host` | PowerShell host events |
| `Settings` | Configuration changes |

### Script Execution

```
AUDIT (sitecore\admin) [ISE] action=scriptExecuting user=sitecore\admin scriptId={CFE81AF6-2468-4E62-8BF2-588B7CC60F80}
```

### Session Elevation

```
AUDIT (sitecore\admin) [Session] action=elevated interface=ISE user=sitecore\admin
```

### Delegated Access

```
AUDIT (sitecore\admin) [DelegatedAccess] action=scriptExecuting contextUser=sitecore\test impersonatedAs=sitecore\admin scriptId={CFE81AF6-2468-4E62-8BF2-588B7CC60F80}
```

This is critical for audit trails showing privilege escalation — the log includes the actual context user and the impersonated account.

### Web Service Calls

```
AUDIT (sitecore\admin) [Remoting] action=requestReceived ip=10.0.0.27 service=remoting
WARN  [Remoting] action=authFailed ip=10.0.0.27 service=mediaUpload reason="invalid credentials"
```

**Logged Events:**

- Remoting connections
- File uploads/downloads
- RESTful API calls
- Authorization failures

### Authentication Events

```
AUDIT (sitecore\admin) [JWT] action=bearerAuthSuccess user=sitecore\admin ip=127.0.0.1
WARN  [ApiKey] action=authFailed ip=10.0.0.27 reason="invalid key"
```

**Logged Events:**

- Successful authentication (bearer, API key, Windows)
- Failed authentication
- Authorization denials

### Errors and Exceptions

Errors include exception details, stack traces, user context, and the operation being performed.

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
# Watch for task execution
Get-Content "$SitecoreLogFolder\SPE.log.20251201.txt" | Where-Object { $_ -match "\[Task\]" }
```

```powershell
# Watch for authentication failures
Get-Content "$SitecoreLogFolder\SPE.log.20251201.txt" | Where-Object { $_ -match "action=authFailed" }
```

```powershell
# Watch for all warnings
Get-Content "$SitecoreLogFolder\SPE.log.20251201.txt" | Where-Object { $_ -match "WARN" }
```

### Log Analysis

#### Find Failed Authentication Attempts

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
$failedAuth = Get-Content $logPath |
    Where-Object { $_ -match "action=authFailed" }

$failedAuth | ForEach-Object {
    if ($_ -match "(?<time>\d{2}:\d{2}:\d{2}).*ip=(?<ip>[\d\.]+).*reason=""(?<reason>[^""]+)""") {
        [PSCustomObject]@{
            Timestamp = $matches["time"]
            IP = $matches["ip"]
            Reason = $matches["reason"]
        }
    }
}
```

#### Track Delegated Access Usage

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
$delegated = Get-Content $logPath |
    Where-Object { $_ -match "\[DelegatedAccess\]" }

$delegated | ForEach-Object {
    if ($_ -match "(?<time>\d{2}:\d{2}:\d{2}).*contextUser=(?<user>\S+)\simpersonatedAs=(?<impersonated>\S+)") {
        [PSCustomObject]@{
            Timestamp = $matches["time"]
            ContextUser = $matches["user"]
            ImpersonatedAs = $matches["impersonated"]
        }
    }
} | Group-Object ContextUser |
    Select-Object Name, Count |
    Sort-Object Count -Descending
```

#### Find Unauthorized Access Attempts

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
Get-Content $logPath |
    Where-Object { $_ -match "action=authFailed|action=accessDenied" } |
    ForEach-Object {
        Write-Host $_ -ForegroundColor Red -BackgroundColor White
    }
```

#### Analyze Web Service Usage

```powershell
$logPath = "$SitecoreLogFolder\SPE.log.20251201.txt"
Get-Content $logPath |
    Where-Object { $_ -match "\[Remoting\]" } |
    ForEach-Object {
        if ($_ -match "ip=(?<ip>[\d\.]+)") {
            [PSCustomObject]@{
                IP = $matches['ip']
                LogLine = $_
            }
        }
    } | Group-Object IP |
    Select-Object Name, Count |
    Sort-Object Count -Descending
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

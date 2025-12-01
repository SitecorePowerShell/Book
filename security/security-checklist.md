# Security Checklist

Use this checklist to validate your SPE security configuration before deploying to any environment. Each section corresponds to a security layer that should be properly configured.

## Pre-Deployment Checklist

### Environment Validation

- [ ] **Confirmed this is a Content Management (CM) server**
- [ ] **Confirmed this is NOT a Content Delivery (CD) instance**
- [ ] **Confirmed server is NOT internet-facing**
- [ ] **Confirmed server is behind a firewall**
- [ ] **Confirmed SPE is NOT installed on production CD servers**

{% hint style="danger" %}
**STOP:** If any of the above are not true, DO NOT proceed with SPE installation.
{% endhint %}

## Configuration Checklist

### 1. Web Services Security

Review `App_Config\Include\Spe\Spe.config` (via patch file):

**Services Status:**
- [ ] **remoting** - Disabled (unless specifically needed for CI/CD)
- [ ] **restfulv1** - Disabled (deprecated)
- [ ] **restfulv2** - Disabled (unless Web API is needed)
- [ ] **fileDownload** - Disabled (unless SPE Remoting is used)
- [ ] **fileUpload** - Disabled (unless SPE Remoting is used)
- [ ] **mediaDownload** - Disabled (unless SPE Remoting is used)
- [ ] **mediaUpload** - Disabled (unless SPE Remoting is used)
- [ ] **handleDownload** - Enabled (required for report exports)
- [ ] **client** - Enabled (required for Console/ISE)
- [ ] **execution** - Enabled (required for SPE features)

**If Remoting is Enabled:**
- [ ] `requireSecureConnection="true"` is configured
- [ ] Authorization is configured with specific roles/users
- [ ] Default roles have been reviewed and restricted
- [ ] Only necessary users/roles have access

**Documentation:** [Web Services Security](web-services.md)

### 2. Session Elevation (UAC)

**Token Configuration:**
- [ ] Console token expiration is appropriate for environment
  - Dev: 30-60 minutes ✓
  - QA: 10-15 minutes ✓
  - Prod: 3-5 minutes ✓
- [ ] Console `elevationAction` is configured
  - Dev: Allow (acceptable) ✓
  - QA/Prod: Password or Confirm ✓
- [ ] ISE token expiration is appropriate for environment
- [ ] ISE `elevationAction` is configured appropriately
- [ ] ItemSave token expiration is appropriate for environment
- [ ] ItemSave `elevationAction` is configured appropriately
- [ ] For Azure AD/SSO environments, using `Confirm` instead of `Password`

**Quick Test:**
- [ ] Tested that elevation prompts appear when accessing Console
- [ ] Tested that elevation prompts appear when accessing ISE
- [ ] Tested that elevation can be dropped manually
- [ ] Tested that sessions expire after configured time

**Documentation:** [Session Elevation](session-elevation.md)

### 3. User and Role Access

**Sitecore Security:**
- [ ] PowerShell Console is restricted to `sitecore\Developer` role (or more restrictive)
- [ ] PowerShell ISE is restricted to `sitecore\Developer` role (or more restrictive)
- [ ] Content Editor context menus are appropriately restricted
- [ ] Script libraries have appropriate item-level security
- [ ] Reviewed who is in the `sitecore\Developer` role
- [ ] Removed unnecessary users from Developer role
- [ ] Custom roles are configured if needed (instead of using broad defaults)

**Application Pool Account:**
- [ ] Service account follows principle of least privilege
- [ ] Service account is NOT LocalSystem or Administrator
- [ ] File system permissions have been audited
- [ ] Service account cannot write to system directories
- [ ] Service account cannot write to web root (except App_Data)

**Documentation:** [Security Policies](security-policies.md), [Users and Roles](users-and-roles.md)

### 4. File Upload Restrictions

**Configuration:**
- [ ] Allowed file types are restricted to necessary types only
- [ ] Dangerous file types (.exe, .dll, .ps1, .bat, .cmd) are NOT allowed
- [ ] Upload locations are restricted to safe directories
- [ ] `$SitecoreTempFolder` is the primary upload location
- [ ] Upload paths do NOT include web root or bin folder
- [ ] MIME type patterns are configured (not just extensions)

**Quick Test:**
- [ ] Tested that allowed file types can be uploaded
- [ ] Tested that disallowed file types are blocked
- [ ] Tested that uploads to unauthorized paths are blocked

**Documentation:** [File Upload Restrictions](file-upload-restrictions.md)

### 5. IIS-Level Security

**Web.config Configuration** (`sitecore modules\PowerShell\Services\web.config`):
- [ ] Anonymous access is denied (`<deny users="?" />`)
- [ ] If using Windows Authentication:
  - [ ] Anonymous authentication disabled in IIS
  - [ ] Windows Authentication enabled in IIS
  - [ ] Allowed users/roles are configured
  - [ ] Remoting clients configured with Credential parameter

**Additional IIS Security:**
- [ ] IP restrictions configured (if applicable for CI/CD)
- [ ] HTTPS requirement configured (production environments)
- [ ] Request filtering configured to block dangerous patterns
- [ ] Request size limits configured appropriately
- [ ] Dangerous HTTP verbs (TRACE, OPTIONS) are blocked

**Quick Test:**
- [ ] Tested that anonymous access is denied (401 response)
- [ ] Tested that authenticated access works
- [ ] Tested that HTTP is blocked/redirected (if HTTPS required)
- [ ] Tested that unauthorized IPs are blocked (if IP restrictions configured)

**Documentation:** [IIS Security](iis-security.md)

### 6. Delegated Access

**If Using Delegated Access:**
- [ ] Delegated access items are properly configured
- [ ] Requester roles are specific (not broad like "Everyone")
- [ ] Impersonated users follow least privilege (not always Admin)
- [ ] Delegated scripts are reviewed for security
- [ ] Delegated scripts validate input
- [ ] Delegated scripts include confirmations for destructive operations
- [ ] Delegated scripts log actions
- [ ] Item-level security protects delegated access configurations

**If NOT Using Delegated Access:**
- [ ] No delegated access items are enabled

**Documentation:** [Delegated Access](delegated-access.md)

### 7. Identity Server (Sitecore 9.1+)

**For Sitecore 9.1+ with Identity Server:**
- [ ] `Spe.IdentityServer.config` is enabled
- [ ] Configuration prevents infinite loop in SPE Console
- [ ] Tested Console works with Identity Server authentication

**Documentation:** [Getting Started - Identity Server](getting-started.md#identity-server-configuration-sitecore-91)

### 8. Minimal Web Service Deployment

**For CI/CD Environments:**
- [ ] Using minimal deployment package if full SPE not needed
- [ ] Only required files are deployed:
  - [ ] `Spe.config`
  - [ ] `Spe.Minimal.config`
  - [ ] `Spe.dll` and `Spe.Abstractions.dll`
  - [ ] Services web.config
  - [ ] Service files (asmx/ashx)
- [ ] Control sources are disabled (patch applied)
- [ ] Remoting is enabled with proper security

**Documentation:** [Minimal Deployment](minimal-deployment.md)

## Testing Checklist

### Functional Testing

**As Non-Administrator User:**
- [ ] Cannot see PowerShell Console in Sitecore menu
- [ ] Cannot see PowerShell ISE in Sitecore menu
- [ ] Cannot see PowerShell context menus
- [ ] Cannot access `/sitecore/shell/Applications/PowerShell` directly

**As Administrator User (without elevated session):**
- [ ] See Console/ISE in menu
- [ ] Prompted for elevation when accessing Console
- [ ] Prompted for elevation when accessing ISE
- [ ] Prompted for elevation when editing script items
- [ ] Content Editor shows warning on PowerShell items

**As Administrator User (with elevated session):**
- [ ] Can access Console successfully
- [ ] Can access ISE successfully
- [ ] Can edit PowerShell script items
- [ ] Session drops after configured timeout
- [ ] Can manually drop elevated session

**Web Services (if enabled):**
- [ ] Anonymous access is denied (401)
- [ ] Authenticated access works
- [ ] Unauthorized users are denied (403)
- [ ] File upload restrictions work as expected
- [ ] IP restrictions work as expected (if configured)

### Security Testing

**Attack Surface:**
- [ ] Web services return 401/403 for unauthorized access
- [ ] Cannot upload dangerous file types
- [ ] Cannot upload to dangerous locations
- [ ] Cannot bypass authentication
- [ ] Cannot bypass Session Elevation
- [ ] HTTPS is enforced (if configured)

**Privilege Escalation:**
- [ ] Non-privileged users cannot access SPE features
- [ ] Non-privileged users cannot modify scripts
- [ ] Delegated access works as intended (if configured)
- [ ] Delegated access is properly restricted

## Logging and Monitoring Checklist

**Logging Configuration:**
- [ ] SPE logging is enabled
- [ ] Log level is appropriate (INFO for production)
- [ ] Logs are being written to expected location
- [ ] Log rotation is configured

**Monitoring:**
- [ ] Reviewing SPE logs regularly for suspicious activity
- [ ] Monitoring for failed authentication attempts
- [ ] Monitoring for delegated access usage
- [ ] Monitoring for Session Elevation denials
- [ ] IIS logs are reviewed for web service access
- [ ] Alert system configured for security events (optional)

**Documentation:** [Logging and Monitoring](logging-and-monitoring.md)

## Documentation Checklist

**Required Documentation:**
- [ ] Security configuration is documented
- [ ] Roles and their access levels are documented
- [ ] Enabled web services and reasons are documented
- [ ] Delegated access configurations are documented (if used)
- [ ] IP restrictions and reasons are documented (if configured)
- [ ] Session elevation timeouts and rationale are documented
- [ ] Emergency procedures are documented
- [ ] Contact information for security issues is documented

## Compliance Checklist

**Organizational Requirements:**
- [ ] Security configuration reviewed by security team
- [ ] Configuration meets organizational security standards
- [ ] Any exceptions are documented and approved
- [ ] Change management process followed
- [ ] Security sign-off obtained (if required)

**Regulatory Compliance (if applicable):**
- [ ] GDPR requirements addressed (if applicable)
- [ ] PCI-DSS requirements addressed (if applicable)
- [ ] HIPAA requirements addressed (if applicable)
- [ ] SOC 2 requirements addressed (if applicable)

## Sign-Off

**Deployment Approval:**
- [ ] All checklist items completed
- [ ] Security testing passed
- [ ] Documentation complete
- [ ] Stakeholders notified

**Approved By:**
- [ ] Developer: _________________ Date: _________
- [ ] Security Team: _____________ Date: _________
- [ ] IT Operations: _____________ Date: _________

## Post-Deployment Checklist

**Within 24 Hours:**
- [ ] Monitor logs for errors
- [ ] Monitor logs for unauthorized access attempts
- [ ] Verify Session Elevation is working in production
- [ ] Verify web services respond appropriately
- [ ] Test key SPE features (Console, ISE, Reports)

**Within 1 Week:**
- [ ] Review all logs for security events
- [ ] Verify no unexpected errors
- [ ] Confirm monitoring/alerting is working
- [ ] Document any issues and resolutions

**Monthly:**
- [ ] Review role membership
- [ ] Review enabled web services
- [ ] Review delegated access configurations
- [ ] Audit logs for suspicious activity
- [ ] Review and update documentation

## Environment-Specific Checklists

### Development Environment

**Minimum Requirements:**
- [ ] Session Elevation configured (can be relaxed)
- [ ] Web services disabled unless needed for testing
- [ ] Basic authentication configured

**Optional:**
- IP restrictions
- Strict session timeouts
- HTTPS enforcement

### QA/Staging Environment

**Must Match Production:**
- [ ] Identical Session Elevation configuration
- [ ] Identical web service configuration
- [ ] Identical authentication requirements
- [ ] Similar HTTPS requirements

### Production Environment

**Strictest Security:**
- [ ] Session Elevation with Password or Confirm (never Allow)
- [ ] Short timeout (3-5 minutes)
- [ ] All unnecessary web services disabled
- [ ] Anonymous access denied
- [ ] HTTPS required
- [ ] IP restrictions (if applicable)
- [ ] Request filtering enabled
- [ ] Comprehensive logging
- [ ] Regular monitoring

### CI/CD Environment

**Automation-Focused:**
- [ ] Minimal deployment package
- [ ] Remoting enabled with strict authorization
- [ ] IP restrictions to build servers
- [ ] HTTPS required
- [ ] Service account configured
- [ ] Comprehensive logging

## Quick Reference

| Security Layer | Configuration File | Critical Setting |
|:---------------|:-------------------|:-----------------|
| **Web Services** | `Spe.config` (via patch) | Services enabled/disabled |
| **Session Elevation** | `Spe.config` (via patch) | Token expiration and action |
| **File Upload** | `Spe.config` (via patch) | Allowed types and locations |
| **IIS Auth** | `Services\web.config` | Deny anonymous |
| **Sitecore Security** | Core DB items | Role permissions |
| **Remoting Auth** | `Spe.config` (via patch) | Authorization roles/users |

## Related Documentation

- [Getting Started](getting-started.md) - Initial security setup
- [Security Policies](security-policies.md) - Understanding the security model
- [Session Elevation](session-elevation.md) - UAC configuration
- [Web Services](web-services.md) - Web service security
- [IIS Security](iis-security.md) - IIS-level protection
- [File Upload Restrictions](file-upload-restrictions.md) - Upload security
- [Delegated Access](delegated-access.md) - Controlled privilege escalation
- [Logging and Monitoring](logging-and-monitoring.md) - Audit and monitoring
- [Users and Roles](users-and-roles.md) - User management

## Emergency Procedures

### Security Incident Response

If you suspect a security breach:

1. **Immediate Actions:**
   - Disable all SPE web services
   - Block access at firewall level
   - Review recent logs for suspicious activity
   - Document the incident

2. **Investigation:**
   - Check IIS logs for unauthorized access
   - Check SPE logs for unusual script execution
   - Review user activity
   - Identify scope of incident

3. **Remediation:**
   - Change service account passwords
   - Review and tighten security configuration
   - Apply security patches if needed
   - Re-deploy with validated security

4. **Recovery:**
   - Restore from backup if necessary
   - Re-enable services with enhanced security
   - Monitor closely for 48 hours
   - Document lessons learned

### Quick Lockdown

To immediately lock down SPE:

```xml
<!-- Add to config patch to disable all external access -->
<configuration xmlns:patch="https://www.sitecore.net/xmlconfig/">
  <sitecore>
    <powershell>
      <services>
        <remoting enabled="false" />
        <restfulv1 enabled="false" />
        <restfulv2 enabled="false" />
        <fileDownload enabled="false" />
        <fileUpload enabled="false" />
        <mediaDownload enabled="false" />
        <mediaUpload enabled="false" />
      </services>
      <userAccountControl>
        <tokens>
          <token name="Console">
            <patch:attribute name="elevationAction">Block</patch:attribute>
          </token>
          <token name="ISE">
            <patch:attribute name="elevationAction">Block</patch:attribute>
          </token>
        </tokens>
      </userAccountControl>
    </powershell>
  </sitecore>
</configuration>
```

Add to `Services\web.config`:

```xml
<authorization>
  <deny users="*" />
</authorization>
```

## Version History

Track when security reviews were completed:

| Date | Reviewer | Environment | Status | Notes |
|:-----|:---------|:------------|:-------|:------|
| YYYY-MM-DD | Name | Production | ✓ Pass | Initial deployment |
| YYYY-MM-DD | Name | QA | ✓ Pass | Quarterly review |
| | | | | |

---

**Last Updated:** {{DATE}}
**Next Review:** {{DATE + 3 months}}

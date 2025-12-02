# Security

Sitecore PowerShell Extensions (SPE) is a powerful administrative tool that requires proper security configuration. This guide provides comprehensive security documentation to help you secure your SPE installation.

{% hint style="danger" %}
**Critical Warning:** SPE is a powerful tool that should NEVER be installed on Content Delivery (CD) instances or internet-facing servers. Always implement security best practices and follow the principle of least privilege.
{% endhint %}

## Quick Start

New to SPE security? Start here:

1. **[Getting Started](./getting-started.md)** - Essential security setup for new installations
2. **[Security Policies](./security-policies.md)** - Understand the SPE security model
3. **[Security Checklist](./security-checklist.md)** - Validate your deployment before going live

## Security Documentation

### Core Security Topics

#### [Security Policies](security-policies.md)

Understand the two-layer security model that governs SPE:

- Application Pool Service Account (OS-level access)
- Sitecore User Account (API-level access)
  - Application and menu item security
- Best practices for both security contexts

#### [Session Elevation (UAC)](session-elevation.md)

Configure User Account Control to require reauthentication:

- How Session Elevation works
- Elevation actions (Allow, Block, Password, Confirm)
- Token configuration and expiration
- Environment-specific recommendations
- Interface behaviors (Console, ISE, Content Editor)

#### [Web Services Security](web-services.md)

Control external access to SPE through web services:

- Service descriptions and security implications
- Enable/disable individual services
- HTTPS and requireSecureConnection
- Role-based authorization
- Configuration examples for different scenarios

### Hardening and Protection

#### [File Upload Restrictions](file-upload-restrictions.md)

Prevent malicious file uploads:

- File type restrictions (extensions and MIME types)
- Upload location restrictions
- Dangerous file types to never allow
- Configuration examples
- Testing upload restrictions

#### [Delegated Access](delegated-access.md)

Grant controlled privilege escalation:

- How delegated access works
- Configuration steps
- Use cases (publishing, reports, bulk operations)
- Script implementation patterns
- Security best practices and monitoring

#### [IIS-Level Security](iis-security.md)

Add defense in depth at the web server level:

- Deny anonymous access
- Windows Authentication
- IP address restrictions
- SSL/TLS requirements
- Request filtering and URL rewrite rules

### User Management

#### [Users and Roles](users-and-roles.md)

Manage Sitecore users and roles:

- Bulk user operations
- Role queries and management
- Item Access Control Lists (ACL)
- Active Directory integration
- PowerShell examples for user management

### Deployment and Operations

#### [Minimal Web Service Deployment](minimal-deployment.md)

Deploy only what's needed for CI/CD:

- Required files for web services only
- Disable UI components
- Configuration for automation scenarios
- Security best practices
- Common deployment patterns

#### [Logging and Monitoring](logging-and-monitoring.md)

Track security events and detect incidents:

- What gets logged
- Log levels and configuration
- Real-time monitoring strategies
- Log analysis examples
- Integration with SIEM systems
- Security metrics and dashboards

### Validation and Compliance

#### [Security Checklist](security-checklist.md)

Comprehensive validation before deployment:

- Pre-deployment validation
- Configuration checklist
- Testing procedures
- Environment-specific checklists
- Post-deployment monitoring
- Emergency procedures

## Security by Environment

### Development Environment

**Priority:** Productivity with basic security

**Recommendations:**

- Session Elevation: Relaxed (Allow or long timeouts)
- Web Services: Enable as needed for testing
- Logging: DEBUG level for troubleshooting
- IP Restrictions: Not required

**Start here:** [Getting Started - Development](getting-started.md#development-environment)

### QA/Staging Environment

**Priority:** Match production security for testing

**Recommendations:**

- Session Elevation: Password or Confirm (5-15 minute timeout)
- Web Services: Match production configuration
- Logging: INFO level
- IP Restrictions: Optional

**Start here:** [Getting Started - QA/Staging](getting-started.md#qastaging-environment)

### Production Environment

**Priority:** Maximum security

**Recommendations:**

- Session Elevation: Password or Confirm (3-5 minute timeout)
- Web Services: Only handleDownload, client, execution (disable remoting)
- Logging: INFO or WARN level
- IP Restrictions: Recommended
- HTTPS: Required

**Start here:** [Security Checklist - Production](security-checklist.md#production-environment)

### CI/CD Environment

**Priority:** Automation with strict access control

**Recommendations:**

- Minimal Deployment: Use minimal package
- Remoting: Enabled with IP restrictions
- Web Services: Only required services
- Logging: INFO level with monitoring
- HTTPS: Required

**Start here:** [Minimal Deployment](minimal-deployment.md)

## Security Layers (Defense in Depth)

SPE security uses multiple layers for comprehensive protection:

```
1. Network Security
   - Firewall rules
   - VPN/private network
   - Not internet-facing
2. IIS-Level Security
   - Deny anonymous access
   - IP restrictions
   - HTTPS requirements
   - Request filtering
3. Sitecore User Security
   - Role-based access control
   - Application-level permissions
   - Item-level security
4. SPE Security Hardening
   - Session Elevation (UAC)
   - Web service controls
   - File upload restrictions
   - Delegated access controls
5. Logging and Monitoring
   - Comprehensive logging
   - Real-time alerting
   - Regular audit reviews
   - SIEM integration
```

Each layer provides additional protection. If one layer is compromised, others provide continued security.

## Common Security Scenarios

### Scenario 1: Locking Down Production CM

**Goal:** Secure SPE for production content management server

**Steps:**

1. Review [Security Policies](security-policies.md) to understand the model
2. Configure [Session Elevation](session-elevation.md) with 5-minute Password timeout
3. Disable unnecessary [Web Services](web-services.md)
4. Configure [IIS Security](iis-security.md) to deny anonymous access
5. Enable [Logging and Monitoring](logging-and-monitoring.md)
6. Complete the [Security Checklist](security-checklist.md)

### Scenario 2: Setting Up CI/CD Automation

**Goal:** Enable remote automation from build servers

**Steps:**

1. Use [Minimal Deployment](minimal-deployment.md) package
2. Enable Remoting in [Web Services](web-services.md) with specific user
3. Configure [IIS Security](iis-security.md) with IP restrictions to build servers
4. Configure [File Upload Restrictions](file-upload-restrictions.md) for packages only
5. Set up [Logging and Monitoring](logging-and-monitoring.md) for automation activity
6. Test with [Security Checklist - CI/CD](security-checklist.md#cicd-environment)

### Scenario 3: Delegating Report Access

**Goal:** Allow content authors to run administrative reports

**Steps:**

1. Understand [Delegated Access](delegated-access.md) concepts
2. Create delegated access configuration for reporting role
3. Configure impersonated user with read-only administrative access
4. Test report access as content author
5. Monitor usage via [Logging and Monitoring](logging-and-monitoring.md)

### Scenario 4: Identity Server Integration (Sitecore 9.1+)

**Goal:** Configure SPE with Sitecore Identity Server

**Steps:**

1. Enable `Spe.IdentityServer.config`
2. Configure [Session Elevation](session-elevation.md) with Confirm action (not Password)
3. Test Console and ISE with federated authentication
4. Configure [Web Services](web-services.md) if needed

## Security Best Practices Summary

### ✅ Do

- **Always** deny anonymous access at IIS level
- **Always** use Session Elevation (UAC) in production
- **Always** require HTTPS for any enabled web services
- **Always** follow principle of least privilege
- **Always** monitor logs for suspicious activity
- Only enable web services you specifically need
- Use short session elevation timeouts in production (3-5 minutes)
- Restrict SPE access to trusted administrators only
- Configure file upload restrictions when upload service is enabled
- Regular security audits and role membership reviews
- Document your security configuration
- Test security in non-production before deploying

### ❌ Don't

- **Never** install SPE on Content Delivery (CD) servers
- **Never** expose SPE to internet-facing servers
- **Never** use `elevationAction="Allow"` in production
- **Never** enable all web services "just in case"
- **Never** grant broad role access (e.g., "Everyone")
- **Never** allow dangerous file types (.exe, .dll, .ps1, .bat)
- Don't skip Session Elevation configuration
- Don't ignore failed authentication attempts in logs
- Don't use administrator accounts for automation
- Don't forget to configure authorization when enabling remoting

## Quick Reference

### Configuration Files

| File                                               | Purpose                     | Documentation                                                                   |
| :------------------------------------------------- | :-------------------------- | :------------------------------------------------------------------------------ |
| `App_Config\Include\Spe\Spe.config`                | Core SPE configuration      | [Web Services](web-services.md)                                                 |
| `App_Config\Include\Spe\Spe.IdentityServer.config` | Identity Server integration | [Getting Started](getting-started.md#identity-server-configuration-sitecore-91) |
| `App_Config\Include\Spe\Custom\*.config`           | Your security patches       | All topics                                                                      |
| `sitecore modules\PowerShell\Services\web.config`  | IIS-level security          | [IIS Security](iis-security.md)                                                 |

### Security Policies Location

| Policy                  | Location                                                            | Documentation                             |
| :---------------------- | :------------------------------------------------------------------ | :---------------------------------------- |
| Application Visibility  | `core:\content\Applications\PowerShell`                             | [Security Policies](security-policies.md) |
| Menu Item Security      | `core:\content\Applications\Content Editor\Context Menues\Default\` | [Security Policies](security-policies.md) |
| Script Library Security | Item-level security on scripts                                      | [Users and Roles](users-and-roles.md)     |
| Delegated Access        | SPE configuration items                                             | [Delegated Access](delegated-access.md)   |

### Default Roles

| Role                                      | Default Access          | Recommendation                     |
| :---------------------------------------- | :---------------------- | :--------------------------------- |
| `sitecore\Developer`                      | Console, ISE            | Keep restricted to developers only |
| `sitecore\Sitecore Client Users`          | ListView, Runner        | Appropriate for content authors    |
| `sitecore\Sitecore Client Authoring`      | Reports                 | Appropriate for content authors    |
| `sitecore\PowerShell Extensions Remoting` | Remoting (when enabled) | Use custom role instead            |

## Getting Help

### Documentation Navigation

- **New to SPE Security?** Start with [Getting Started](getting-started.md)
- **Deploying to production?** Use the [Security Checklist](security-checklist.md)
- **Setting up automation?** See [Minimal Deployment](minimal-deployment.md)
- **Need to debug?** Check [Logging and Monitoring](logging-and-monitoring.md)
- **Configuring a specific feature?** See topic-specific guides below

### Support Resources

- **GitHub Issues:** [SitecorePowerShell/Console](https://github.com/SitecorePowerShell/Console/issues)
- **Slack:** #module-spe on Sitecore Community Slack
- **Documentation:** [Full SPE Documentation](../)

### Security Incident Response

If you suspect a security breach:

1. Immediately lock down SPE using [Emergency Procedures](security-checklist.md#emergency-procedures)
2. Review logs using [Logging and Monitoring](logging-and-monitoring.md) guidance
3. Document the incident
4. Contact your security team
5. Report to SPE maintainers if it's a product vulnerability

## Additional Resources

### Related Documentation

- [Installation](../installation.md) - Initial SPE installation
- [Interfaces](../interfaces/) - Console, ISE, and Interactive Dialogs
- [Remoting](../remoting.md) - Using SPE Remoting for automation
- [Modules](../modules/) - Integration points and features
- [Appendix - Security Cmdlets](../appendix/security/) - PowerShell security commands

### External References

- [Sitecore Security Best Practices](https://doc.sitecore.com/xp/en/developers/latest/platform-administration-and-architecture/security.html)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Principle of Least Privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)

## Version-Specific Notes

### Sitecore 9.1+ with Identity Server

Enable the Identity Server configuration:

- File: `App_Config\Include\Spe\Spe.IdentityServer.config`
- Purpose: Prevents infinite loop in SPE Console
- Use `elevationAction="Confirm"` instead of "Password"

```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/" xmlns:role="http://www.sitecore.net/xmlconfig/role/" xmlns:security="http://www.sitecore.net/xmlconfig/security/">
  <sitecore role:require="Standalone or ContentManagement or XMCloud" security:require="Sitecore">
    <pipelines>
      <owin.cookieAuthentication.validateIdentity>
        <processor type="Sitecore.Owin.Authentication.Pipelines.CookieAuthentication.ValidateIdentity.ValidateSiteNeutralPaths, Sitecore.Owin.Authentication">
          <siteNeutralPaths hint="list">
            <!-- This entry corrects the infinite loop of ExecuteCommand in the SPE Console -->
            <path hint="spe">/sitecore%20modules/PowerShell</path>
          </siteNeutralPaths>
        </processor>
      </owin.cookieAuthentication.validateIdentity>
    </pipelines>
  </sitecore>
</configuration>
```

### Sitecore XM Cloud

Consult the latest SPE documentation for XM Cloud-specific security configurations.

---

**Remember:** Security is not a one-time configuration. Regular reviews, monitoring, and updates are essential to maintaining a secure SPE installation.

**Last Updated:** 2025
**Maintained By:** SPE Community

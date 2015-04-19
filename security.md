# Security

#### Security Accounts
There are two main security policies to consider when using the SPE module, the Application Pool and the Sitecore user accounts.

The first policy relates to the Application Pool service account running in IIS. The Windows PowerShell runspace will have access to the local system via providers (i.e. FileSystem, Registry), and be managed through the Console or ISE. If the service account is capable of removing files from the root directory, then SPE can accomplish the same. If the IIS_IUSRS account was granted access to modify *HKEY_LOCAL_MACHINE* settings, then running commands through SPE can also be accomplished.

The second policy is tied to the Sitecore user account. The code executed through SPE operates within the privileges of the logged in user. Keep in mind that this can be bypassed just as can be through the Sitecore API.
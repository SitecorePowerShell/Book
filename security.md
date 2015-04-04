# Security

There are two main security policies to consider when using the SPE module, the Application Pool and the Sitecore user accounts.

The first policy is tied to the Application Pool service account running in IIS. The Windows PowerShell runspace will have access to the local system via providers (i.e. FileSystem or Registry) and  can managed through the Console or ISE. As an example, if the service account is capable of removing files from the root directory, that can be accomplished using the SPE. As another example, if the IIS_IUSRS account was granted access to modify HKEY_LOCAL_MACHINE settings, this can then be accomplished through SPE.

The second security policy is tied to the Sitecore user account. Whatever access the logged in user has within the Sitecore environment, the change can be applied through the Console and ISE.
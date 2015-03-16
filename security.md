# Security

There are two main security policies to consider when using the SPE module. The first security policy is tied to the Application Pool service account. Whatever access the service account has on the local system (i.e. file system or registry), the change can be applied through the Console and ISE. As an example, if the service account is capable of removing files from the root directory, that can be accomplished using the SPE. As another example, if the IIS_IUSRS account was granted access to modify HKEY_LOCAL_MACHINE settings, this can then be accomplished through SPE.

The second security policy is tied to the Sitecore user account. Whatever access the logged in user has within the Sitecore environment, the change can be applied through the Console and ISE.
# Web API

The Web API integration point exposes scripts through a url.

### Security

The integration point can be completely disabled through configuration as described [here](security.md).

The url will look something like the following:
http://remotesitecore/-/v2/master/homeanddescendants?user=admin&password=b

Here's the url broken down:

* **v2** - Indicates that we are using the second version of the api. In the config you'll need to ensure the *restfulv2* service is enabled.
* **master** - Indicates that the script is contained within the *master* database.
* **homeanddescendants** - Replace this name with whatever your script is called in the Web API library.
* **query string parameters** - The *user* and *password* parameters will authenticate the request and in most cases will be needed. If the script is published to the *web* database the credentials are not required.
  * **scriptArguments** - The PowerShell hashtable containing the query string parameters.
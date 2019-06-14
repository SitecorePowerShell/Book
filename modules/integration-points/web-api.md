# Web API

The **Web API** integration point exposes scripts through a url. This can be especially helpful when a script needs to be executed on the server but without knowledge of the script contents.

The url will look something like the following: [https://remotesitecore/-/script/v2/master/homeanddescendants?user=sitecore\admin&password=b](https://remotesitecore/-/script/v2/master/homeanddescendants?user=sitecore\admin&password=b)

Here's the url broken down:

* **API Version** - Specifies which service is being requested.
  * `v2` - This is the service that executes scripts stored in the integration point library.
* **Database** - Specifies which database contains the script.
  * `master` - This database requires the credentials to be provided.
* **Script** - Specifies the name of the script contained in the database.
  * `homeanddescendants` - Replace this name with whatever your script is called in the Web API library.
* **Query String Parameters (deprecated)** - Specifies the additional bits of data for use by the web service.
  * `user` and `password` - Authenticates the request and in most cases will be needed. If the script is published to the _web_ database the credentials are not required. You should use the **Basic** header instead of the query string.
  * All of the query string parameters added to the variable `scriptArguments`. Use this PowerShell hashtable inside of your scripts. Check out the function `Invoke-ApiScript` for an example of how these parameters can be put to good use.

Headers:

Use the **Basic** header to provide authentication information. Here is an example of how to build that for a script.

```text
$username = "admin"
$password = "b"

$pair = "$($username):$($password)"

$encodedCreds = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($pair))

$basicAuthValue = "Basic $encodedCreds"

$headers = @{
    Authorization = $basicAuthValue
}

Invoke-RestMethod -Headers $headers -Uri "https://spe.dev.local/-/script/v2/master/TrainingWebApi?offset=3&limit=2&fields=(Name,ItemPath,Id)"
```

**Note:** Examples included in the following modules

* Getting Started

## Security

The integration point is disabled by default and can be enabled through configuration as described [here](../../security/). See **Restfulv2**.

## References

* Watch Adam present this and much more on Sitecore! Experienced [here](https://vimeo.com/134196432).


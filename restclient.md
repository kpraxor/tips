# Using RESTClient Firefox Plugin to Learn the RAX RESTful API

Command-line tools like [curl][curl] allow you to send HTTP(S) requests to [Rackspace's Public Cloud API][api], in order to interact directly.  But why not also try [RESTClient][plugin], a Firefox plugin?

## Install RESTClient Plugin
You can install the RESTClient plugin from [addons.mozilla.org][plugin].  Restart Firefox, and you'll see the red icon with a circle appear. ![RESTClient Icon][restclient-icon]

### The Interface 
Click the icon to open RESTClient in a new tab.

![restclient-newtab][restclient-newtab]



## Authentication
In order to talk to the RESTful API, you'll need 2 Keys:

1. Your Rackspace Cloud API Key  
2. An authentication token

By presenting your API key, along with your username, to the Rackspace Cloud Auth endpoints, you'll receive a JSON or XML response containing the auth token, which will be good for 24 hours.

RESTClient makes it really easy to do this!

### Create Some Headers
Select Headers-> Custom Header from the top menu  
![custom-header.png][custom-header]

Let's keep it simple.  Create a custom header

`X-Auth-Key`  

and paste your API Key into the Value box, select "Save to favorite" and click "Okay"
![x-auth-key-header][x-auth-key-header]

We'll also need our account username, which we'll place in another custom header

`X-Auth-User`

![x-auth-user-header][x-auth-user-header]

### GET the Auth Token

That's all we need to send our username and API key to the authentication endpoint, and receive our authentication token, which we'll use for the rest of our API calls.  The US endpoint is

`https://identity.api.rackspacecloud.com/v1.0/`

So, make sure your "Method" is set to "GET", enter the endpoint, and you should have 2 headers listed.  Click "SEND"    
![get-auth-token][get-auth-token]

We're hoping for a `200 OK` status code in our response header.  Notice the `X-Auth-Token` is returned with a token value.  
![response-header][response-header]

### Use the Auth Token in a Header for Easy Re-Use
Let's add a custom header that contains this token via copy/paste  
![x-auth-token-header][x-auth-token-header]

Now we can clear our headers using the "Remove Headers" button, and just have to send the `X-Auth-Token` to interact with the cloud service APIs.

> Note that to keep things simple, we authenticated against the API v1.0, which supported the `HTTP GET` request, along with `X-Auth-User` and `X-Auth-Key` header values.  In v2.0 of the authentication API, this method is deprecated, so we'll have to instead use an `HTTP POST` request to `/tokens`

## API v2.0

`HTTP POST` requests to RESTful APIs are used to send request bodies, containing XML or JSON data.  To repeat our authentication exercise against V2.0 of the API, let's use JSON.

You can use either the following combinations

* username and password
* username and API key

Since we've already been using the API key, I'll use that.  If you'd rather use your username and password, refer to the [Authentication Request][auth-request-api-doc] section of our API guide at [docs.rackspce.com][api]

### POST to /tokens

Before we `POST` this, let's add another header to tell the API we want a JSON response:

![accept-json-header][accept-json-header]

and one that indicates we'll be sending a JSON request:

![content-type-json][content-type-json]

Replace the `username` value `MyRackspaceAcct` below with your username and the `apiKey` value `0000000000000000000` with your API key, then copy it to your clipboard.

`{"auth":{"RAX-KSKEY:apiKeyCredentials":{"username":"MyRackspaceAcct", "apiKey":"0000000000000000000"}}}`

Now, paste the JSON "auth" object into the Request Body, make sure to select "POST" and click "SEND" to

`https://identity.api.rackspacecloud.com/v2.0/tokens`

![post-auth-json][post-auth-json]

## Review the Response Body
RESTClient allows you to view the HTTPS response header as well as the response body in 3 viewing modes

* RAW
* Highlight
* Preview

On the Body (Highlight) tab, it's easy to see our access token id, as well as a service catalog with lots of great information about all of the service endpoints.  Notice the token ID is the same as above from the v1.0 API, so we can use the existing header to call the Cloud DNS API.  
![v2-token][v2-token]



### A Simple GET request - DNS

Let's list the DNS domains configured in Cloud DNS, with just a `GET` and the `X-Auth-Token` header, but request in JSON format with the `Accept: application/json` header.

Our API target URI is:

`https://dns.api.rackspacecloud.com/v1.0/548979/domains`

![get-domains][get-domains]


# Summary

We explored the use of RESTClient for talking directly to the Rackspace Cloud's RESTful API.  Using just a cloud account username and API key, we were able to:

* authenticate against the identity service
* acquire an auth token
* see the difference between v1.0 and v2.0 of the API
* obtain a service catalog
* query a service endpoint (Cloud DNS) for domain information

RESTClient offers an easy to use and interactive way to work with the Rackspace API, and can be a great learning tool for getting to know the REST methods.  Use it with the [API docs][api] to explore and learn how to unlock the power of the [open cloud][opencloud]! 


[api]:http://docs.rackspace.com/ (API Docs)
[curl]:http://curl.haxx.se/ (curl.haxx.se)
[plugin]: https://addons.mozilla.org/en-us/firefox/addon/restclient/ (RestClient Plugin)
[auth-request-api-doc]:http://docs.rackspace.com/servers/api/v2/cs-devguide/content/curl_auth.html (cloud servers dev guide)
[opencloud]:http://www.rackspace.com/open-cloud/ (open cloud)

[restclient-icon]: /img/restclient-icon.png (RestClient Icon)
[restclient-newtab]: /img/restclient-newtab.png (new tab)
[custom-header]: /img/custom-header.png (create a header)
[x-auth-key-header]: /img/x-auth-key-header.png (create X-Auth-Key header)
[x-auth-user-header]: /img/x-auth-user-header.png (create X-Auth-User header)
[get-auth-token]: /img/get-auth-token.png (get auth-token)
[x-auth-token-header]: /img/x-auth-token-header.png (create X-Auth-Token header)
[response-header]: /img/response-header.png (response header)
[accept-json-header]: /img/accept-json-header.png (json header)
[post-auth-json]: /img/post-auth-json.png (post auth json)
[content-type-json]: /img/content-type-json.png (content-type json header)
[v2-token]: /img/v2-token.png (v2 token response)
[get-domains]: /img/get-domains.png (get domains)


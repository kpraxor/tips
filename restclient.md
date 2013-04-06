# Using RESTClient Firefox Plugin to Learn the RAX RESTful API

Command-line tools like [curl][curl] allow you to sent HTTP(S) requests to [Rackspace's Public Cloud API][api], in order to interact directly.  But why not also try [RESTClient][plugin], a Firefox plugin?

### Install RESTClient Plugin
You can install the RESTClient plugin from [addons.mozilla.org][plugin].  Restart Firefox, and you'll see red icon with a circle appear. ![RESTClient Icon][restclient-icon]


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

`https://identity.api.rackspacecloud.com/v2.0/`

So, make sure your "Method" is set to "GET", enter the endpoint, and you should have 2 headers listed.  Click "SEND"    
![get-auth-token][get-auth-token]




[api]:http://docs.rackspace.com/ (API Docs)
[curl]:http://curl.haxx.se/ (curl.haxx.se)
[plugin]: https://addons.mozilla.org/en-us/firefox/addon/restclient/ (RestClient Plugin)


[restclient-icon]: /img/restclient-icon.png (RestClient Icon)
[restclient-newtab]: /img/restclient-newtab.png (new tab)
[custom-header]: /img/custom-header.png (create a header)
[x-auth-key-header]: /img/x-auth-key-header.png (create X-Auth-Key header)
[x-auth-user-header]: /img/x-auth-user-header.png (create X-Auth-User header)
[get-auth-token]: /img/get-auth-token.png (get auth-token)



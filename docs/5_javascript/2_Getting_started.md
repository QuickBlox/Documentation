If you haven’t set up your project yet, please head over to the [Quick Start](/quick_start/Getting_started.html) to get up and running.

<hr>

<span id="Initialize_configuration_SDK" class="on_page_navigation"></span>
# Initialize & Configuration SDK
Check out [init method in API Refference](http://quickblox.github.io/quickblox-javascript-sdk/docs/QB.html#.init).

Initialize SDK with application credentials:

```javascript
var CREDENTIALS = {
  appId: 28287,
  authKey: 'XydaWcf8OO9xhGT',
  authSecret: 'JZfqTspCvELAmnW'
};

QB.init(CREDENTIALS.appId, CREDENTIALS.authKey, CREDENTIALS.authSecret);
```

Or initialize SDK with existing QB token:

```javascript
var sessionToken = '1b785b603a9ae88d9dfbd1fc0cca0335086927f1';
var appId = 3451;

QB.init(sessionToken, appId);
```

With this method you can generate a token elsewhere (e.g. on the server side), and then use this token and application ID to initialise the SDK.


<span id="Authorization" class="on_page_navigation"></span>
# Authorization

To be able to use QuickBlox API you have to create a session.

<br>

There are 2 types of session:

* **Application session**. It provides only READ access to data.
* **User session**. It provides CRUD (Create, Read, Update, Delete) access to data.

<br>

To create an application session use the code:

```javascript
QB.createSession(function(err, result) {
  // callback function
});
```

With an Application session you can READ any data you need and only have to do one Create operation — [User Sign Up (Users API)](http://quickblox.com/developers/Sample-users-javascript#Signing_Up).

<br>

To update an Application session to a User session you have to login to QuickBlox:

```javascript
var params = {login: 'garry', password: 'garry5santos'};

// or through email
// var params = {email: 'garry@gmail.com', password: 'garry5santos'};

// or through social networks (Facebook / Twitter)
// var params = {provider: 'facebook', keys: {token: 'AM46dxjhisdffgry26282352fdusdfusdfgsdf'}};
 
QB.login(params, function(error, result) {

});
```

Or, if the user is already exist, you can create a User session in single query as well:

```javascript
var params = {login: 'garry', password: 'garry5santos'};

// or through email
// var params = {email: 'garry@gmail.com', password: 'garry5santos'};

// or through social networks (Facebook / Twitter)
// var params = {provider: 'facebook', keys: {token: 'AM46dxjhisdffgry26282352fdusdfusdfgsdf'}};
 
QB.createSession(params, function(error, result) {

});
```

<span id="Session_expiration" class="on_page_navigation"></span>
# Session expiration

A session will remain valid for 2 hours after the last request to QuickBlox was performed.

You may specify a session expiration listener, it looks like this:

```javascript
config = {
  on: {
    sessionExpired: [Function]
  }
}
...
 
QB.init(3477, 'ChRnwEJ3WzxH9O4', 'AS546kpUQ2tfbvv', config);
```

It has been placed inside the "on" field so that we can add more listeners in the future. **This function fires when the SDK makes a request and recognises an expired session error**. It will execute this function before it executes the original callback for the request. You could use it to create a new session, request a new token from your backend, or simply tell the user to refresh the page.

This function accepts 2 parameters, `next` and `retry`. Calling `next()` will cause the original callback of the request to be fired - you would use this if your session creation/regeneration fails. Retry will make the original request again with your newly regenerated token, meaning minimal interruption to the operation of your code.

<span id="Configuration" class="on_page_navigation"></span>
# Configuration

We recommend you to create `config.js` configuration file in your js-application and insert your QB application credentials and other ancillary data into it.

<br>

A custom configuration can look like this:

```javascript
var CONFIG = {
  endpoints: {
    api: "apicustomdomain.quickblox.com", // set custom API endpoint
    chat: "chatcustomdomain.quickblox.com" // set custom Chat endpoint
  },
  chatProtocol: {
    active: 2 // set 1 to use BOSH, set 2 to use WebSockets (default)
  },
  debug: {mode: 1} // set DEBUG mode
};
QB.init(3477, 'ChRnwEJ3WzxH9O4', 'AS546kpUQ2tfbvv', CONFIG);
```

The above would enable debugging, disable ssl and change the endpoints so that requests would go to another server.

There are 3 debug modes:

* `debug: {mode: 0}` - logs are off
* `debug: {mode: 1}` - the SDK will be printing logs to console.log()
* `debug: {mode: 2, file: "appname_debug.log"}` - the SDK will be printing logs into file with name "appname_debug.log". Works only on Node.js.

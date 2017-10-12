If you haven’t set up your project yet, please head over to the [Quick Start](/quick_start/Getting_started.html) to get up and running.

<hr>

<span id="Initialize_and_configuration_SDK" class="on_page_navigation"></span>
# Initialize & Configuration SDK

There are 2 ways to initialize JS SDK: initialize SDK with application credentials and create a session or initialize with existing session token.

Check out [init method in API Refference](http://quickblox.github.io/quickblox-javascript-sdk/docs/QB.html#.init).

Initialize JS SDK with application credentials:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/quickblox/x.x.x/quickblox.min.js"></script>

<script>
var CREDS = {
    appId: 12345,
    authKey: 'XysdWcf8-O9xgGT',
    authSecret: 'BRfq6shCvELAdfW'
};

var config = {
    endpoints: {
        api: 'apicustomdomain.quickblox.com', // set custom API endpoint
        chat: 'chatcustomdomain.quickblox.com' // set custom Chat endpoint
    },
    chatProtocol: {
        active: 2 // set 1 to use BOSH, set 2 to use WebSockets (default)
    },
    debug: { 
        mode: 1
    }
    /*
     * There are 3 debug mods:
     * debug: {mode: 0} - logs are off
     * debug: {mode: 1} - the SDK will be printing logs to console.log()
     * debug: {
     *  mode: 2,
     *  file: "appname_debug.log"
     * } - the SDK will be printing logs into file with name "appname_debug.log". Works only on Node.js.
     */
};

QB.init(CREDS.appId, CREDS.authKey, CREDS.authSecret, config);
</script>
```
The above would enable debugging, disable ssl and change the endpoints so that requests would go to another server.

Or initialize SDK with existing QB token:

```javascript
var QB = require('../node_modules/quickblox-javascript-sdk/src/qbMain.js');

var sessionToken = '1b785b603a9ae88d9dfbd1fc0cca0335086927f1';
var appId = 3451;

QB.init(sessionToken, appId, null, config);
```

With this method you can generate a token elsewhere (e.g. on the server side), and then use this token and application ID to initialise the SDK.

<span id="Authorization" class="on_page_navigation"></span>
# Authorization

To be able to use QuickBlox API you have to create a session.

There are 2 types of session:

* **Application session**. It provides only READ access to data.
* **User session**. It provides CRUD (Create, Read, Update, Delete) access to data.

To create an application session use the code:

```javascript
QB.createSession(function(error, session) {
    // callback function
});
```

With an Application session you can READ any data you need and only have to do one Create operation — [User Sign Up (Users API)](http://quickblox.com/developers/Sample-users-javascript#Signing_Up).

To update an Application session to a User session you have to login to QuickBlox:

```javascript
var params = {login: 'garry', password: 'garry5santos'};

// or through email
// var params = {email: 'garry@gmail.com', password: 'garry5santos'};
 
QB.login(params, function(error, result) {
    // callback function
});
```

Or, if the user is already exist, you can create a User session in single query as well:

```javascript
var params = {login: 'garry', password: 'garry5santos'};

// or through email
// var params = {email: 'garry@gmail.com', password: 'garry5santos'};
 
QB.createSession(params, function(error, result) {
    // callback function
});
```

<span id="Authorization_custom_provider" class="on_page_navigation"></span>
# Authorization via custom entity provider

## Authorization via Facebook (to create User session token based on Facebook user)

1. Open [Facebook Developer Console](https://developers.facebook.com/) and create a new application if you need.

2. Connect the Facebook SDK ([read more](https://developers.facebook.com/docs/javascript/quickstart)):
```` html
<script src="https://connect.facebook.net/en_US/sdk.js"></script>
````

3. Initialize the Firebase SDK:
````javascript
var facebookAccount = {
    appId: 'your-facebook-app-id', // from your facebook application
    scope: 'email,user_friends'
};

FB.init({
    appId: facebookAccount.appId,
    version: 'v2.9'
});
````

4. Call login method:
````javascript
FB.login(function(response) {
    if (response.authResponse && response.status === 'connected') {
        var params = {
            provider: 'facebook',
            keys: {
                token: response.authResponse.accessToken
            }
        };
        
        // Create session and then login user to it
        QB.createSession(function(error, result) {
            if (result) {    
                QB.login(params, function(err, res) {
                    // callback function
                });
            }
        });
        
        /* 
        // Or create session with user login
        QB.createSession(params, function(err, res) {
            // callback function
        });
        */
    }
}, {scope: facebookAccount.scope});
````

**Find more in [Facebook JavaScript SDK](https://developers.facebook.com/docs/javascript) Docs.**

## Authorization via Firebase (to create User session token based on Firebase phone user (SMS))

1. Open [Firebase Console](https://console.firebase.google.com) and create a new application if you need.

2. [Install the Firebase SDK](https://firebase.google.com/docs/web/setup) or just paste the code snippet into your application HTML:
````html
<script src="https://www.gstatic.com/firebasejs/4.5.0/firebase.js"></script>
````

3. Initialize the Firebase SDK:
````javascript
var firebaseConfig = {
    apiKey: '<API_KEY>',
    authDomain: '<PROJECT_ID>.firebaseapp.com',
    databaseURL: 'https://<DATABASE_NAME>.firebaseio.com',
    projectId: '<PROJECT_ID>',
    storageBucket: '<BUCKET>.appspot.com',
    messagingSenderId: '<SENDER_ID>'
};

firebase.initializeApp(firebaseConfig);
````

### FirebaseUI Widget

[FirebaseUI](https://github.com/firebase/firebaseui-web#firebaseui-for-web--auth) widget can be used for help:
````html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Sample FirebaseUI App</title>
        <script src="https://cdn.firebase.com/libs/firebaseui/2.4.0/firebaseui.js"></script>
        <link type="text/css" rel="stylesheet" href="https://cdn.firebase.com/libs/firebaseui/2.4.0/firebaseui.css" />
        <script type="text/javascript">
            // FirebaseUI config.
            var uiConfig = {
                signInOptions: [{
                    provider: firebase.auth.PhoneAuthProvider.PROVIDER_ID,
                    recaptchaParameters: {
                        type: 'image', // 'audio'
                        size: 'normal', // 'invisible' or 'compact'
                        badge: 'bottomleft' //' bottomright' or 'inline' applies to invisible.
                    }
                }],
                // Terms of service url.
                tosUrl: '<your-tos-url>'
            };
            
            // Initialize the FirebaseUI Widget using Firebase.
            var ui = new firebaseui.auth.AuthUI(firebase.auth());
            // The start method will wait until the DOM is loaded.
            ui.start('#firebaseui-auth-container', uiConfig);
            
            initApp = function() {
                firebase.auth().onAuthStateChanged(function(user) {
                    user.getIdToken().then(function(idToken) {
                        var authParams = {
                            'provider': 'firebase_phone',
                            'firebase_phone': {
                                'access_token': idToken,
                                'project_id': firebaseConfig.projectId
                            }
                        };
                    
                        // Create session and then login user to it
                        QB.createSession(function(error, result) {
                            if (result) {    
                                QB.login(authParams, function(err, res) {
                                    // callback function
                                });
                            }
                        });
                    });
                });
            };
            
            window.addEventListener('load', function() {
                initApp();
            });
        </script>
    </head>
    <body>
        <div id="firebaseui-auth-container"></div>
    </body>
</html>
````

### Build custom login form without [FirebaseUI](https://github.com/firebase/firebaseui-web#firebaseui-for-web--auth)

1. Add the simple HTML code snippet:
````html
...
    <form class="firebase_form_SMS">
        <input id="firebase_input_SMS" placeholder="Phone number" />
        <div id="recaptcha_container"></div>
        <button id="firebase_btn_sendSMS">Send</button>
    </form>
    
    <form class="firebase_form_code">
        <input id="firebase_input_code" placeholder="Code" />
        <button id="firebase_btn_sendCode">Login</button>
    </form>
...
````

2. Set up the reCAPTCHA verifier ([source from Firebase Docs to read more](https://firebase.google.com/docs/auth/web/phone-auth#set-up-the-recaptcha-verifier)):
````javascript
window.recaptchaVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container');
````

3. Send a verification code to the user's phone ([source from Firebase Docs](https://firebase.google.com/docs/auth/web/phone-auth#send-a-verification-code-to-the-users-phone)):
````javascript
var phoneNumber = document.getElementById('firebase_input_SMS');
var appVerifier = window.recaptchaVerifier;

document.getElementById('firebase_form_SMS').addEventListener('submit', function() {
    firebase.auth().signInWithPhoneNumber(phoneNumber, appVerifier)
        .then(function (confirmationResult) {
            window.confirmationResult = confirmationResult;
        }).catch(function (error) {
            // SMS not sent
        });
});
````

4. Sign in the user with the verification code ([source from Firebase Docs](https://firebase.google.com/docs/auth/web/phone-auth#sign-in-the-user-with-the-verification-code)):
````javascript
var code = document.getElementById('firebase_input_code');

document.getElementById('firebase_form_code').addEventListener('submit', function() {
    window.confirmationResult.confirm(code)
        .then(function (result) {
            // user signed in successfully.
            result.user.getIdToken().then(function(idToken) {
                var authParams = {
                    'provider': 'firebase_phone',
                    'firebase_phone': {
                        'access_token': idToken,
                        'project_id': firebaseConfig.projectId
                    }
                };
            
                // Create session and then login user to it
                QB.createSession(function(error, result) {
                    if (result) {    
                        QB.login(authParams, function(err, res) {
                            // callback function
                        });
                    }
                });
        }).catch(function (error) {
            // user couldn't sign in (bad verification code?)
        });
    });
});
````

**Read more about [Authenticate with Firebase with a Phone Number Using JavaScript](https://firebase.google.com/docs/auth/web/phone-auth).**


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
//...

QB.init(3477, 'ChRnwEJ3WzxH9O4', 'AS546kpUQ2tfbvv', config);
```

It has been placed inside the "on" field so that we can add more listeners in the future. **This function fires when the SDK makes a request and recognises an expired session error**. It will execute this function before it executes the original callback for the request. You could use it to create a new session, request a new token from your backend, or simply tell the user to refresh the page.

This function accepts 2 parameters, `next` and `retry`. Calling `next()` will cause the original callback of the request to be fired - you would use this if your session creation/regeneration fails. Retry will make the original request again with your newly regenerated token, meaning minimal interruption to the operation of your code.
# Summary

The user module manages all things related to user accounts handling, authentication, account data, password reminding etc.

- OAuth is secure and supports transparent authentication;
- Password reminder and other typical user management tasks are managed automatically. Reminders and other technical messages are sent via Messages module;
- Logging in users with their Facebook/Twitter/Twitter Digits accounts is available;
- User tags – set up user tags and address them separately in your app or through the admin panel – supported by other modules such as Messages;
- Existing user base integration – our module supports connection to your system by storing your existent IDs table and matching them to new ones.


# QBUser properties
**QBUser** model has such main fields:

* login: The user's login name
* password: The password for the user.
* full name: The user's full name
* email: The email address for the user (optional).
* tags: A list of tags associated with the user.

# Users API

## Signing Up

To Create new User call:
```java
final QBUser user = new QBUser("Javck", "javckpassword");
user.setExternalId("45345");
user.setFacebookId("100233453457767");
user.setTwitterId("182334635457");
user.setEmail("Javck@mail.com");
user.setFullName("Javck Bold");
user.setPhone("+18904567812");
StringifyArrayList<String> tags = new StringifyArrayList<String>();
tags.add("car");
tags.add("man");
user.setTags(tags);
user.setWebsite("www.mysite.com");

QBUsers.signUp(user).performAsync( new QBEntityCallback<QBUser>() {
    @Override
    public void onSuccess(QBUser user, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

## Sign In & Social authorization

There are 4 methods that you can use to sign in to QuickBlox:

* Sign in with login & password
* Sign in with email & password
* Sign in using Facebook/Twitter access token
* Sign in In with Twitter Digits

### Sign In with login & password
```java
QBUser user = new QBUser("garry", "garry2892pass")

QBUsers.signIn(user).performAsync( new QBEntityCallback<QBUser>() {
    @Override
    public void onSuccess(QBUser user, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

### Sign In with email & password
```java
QBUser user = new QBUser();
user.setEmail("garry657@gmail.com");
user.setPassword("garrypass");

QBUsers.signIn(user).performAsync( new QBEntityCallback<QBUser>() {
    @Override
    public void onSuccess(QBUser user, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```


### Sign In using Facebook/Twitter access token
```java
String facebookAccessToken = "AAAEra8jNdnkBABYf3ZBSAz9dgLfyK7tQNttIoaZA1cC40niR6HVS0nYuufZB0ZCn66VJc" +
 "ISM8DO2bcbhEahm2nW01ZAZC1YwpZB7rds37xW0wZDZD";

QBUsers.signInUsingSocialProvider(QBProvider.FACEBOOK,  facebookAccessToken, null).performAsync( new QBEntityCallback<QBUser>() {
    @Override
    public void onSuccess(QBUser user, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

### Signing In with Twitter Digits

Digits lets people create an account or sign into your app using nothing but their phone number on iOS
and Android. Built using the same global, reliable infrastructure Twitter uses, Digits will verify
the user’s phone number with a simple customizable user interface that easily integrates into your app.

Read more how to start with Twitter Digits https://docs.fabric.io/android/digits/digits.html

To sign In using twitter digits you should :

* setup twitter digits:
```java
TwitterAuthConfig authConfig = new TwitterAuthConfig(consumerKey, consumerSecret);
Fabric.with(context, new TwitterCore(authConfig), new Digits());
```

* authenticate on digits
```java
 Digits.authenticate(new AuthCallback() {
        @Override
        public void success(DigitsSession session, String phoneNumber) {
            //store session data

        }

        @Override
        public void failure(DigitsException exception) {

        }
    }, PHONE_NUMBER);
```

* Sign in using digits data
```java
    Map<String, String> authHeaders = getAuthHeadersBySession(session);

    String xAuthServiceProvider = authHeaders.get("X-Auth-Service-Provider");
    String xVerifyCredentialsAuthorization = authHeaders.get("X-Verify-Credentials-Authorization");

    QBUsers.signInUsingTwitterDigits(xAuthServiceProvider, xVerifyCredentialsAuthorization).performAsync( new QBEntityCallback<QBUser>() {
          @Override
          public void onSuccess(QBUser user, Bundle params) {

          }

          @Override
          public void onError(QBResponseException errors) {

          }
    });

private Map<String, String> getAuthHeadersBySession(DigitsSession digitsSession) {
    TwitterAuthToken authToken = (TwitterAuthToken) digitsSession.getAuthToken();
    DigitsOAuthSigning oauthSigning = new DigitsOAuthSigning(authConfig, authToken);

    return oauthSigning.getOAuthEchoHeadersForVerifyCredentials();
}

```

## Update own profile
If you want to update your profile - just use method bellow. You can update only your own user, just put your User's ID:
```java
QBUser user = new QBUser();
user.setId(53779);
user.setFullName("Monro");
user.setEmail("monroJohns987@gmail.com");

QBUsers.updateUser(user).performAsync( new QBEntityCallback<QBUser>(){
    @Override
    public void onSuccess(QBUser user, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

## Update profile picture (avatar)

In case you want set or update user's avatar you should :

* upload file using **Content** module:
```java

boolean fileIsPublic = true;

QBContent.uploadFileTask(file1, fileIsPublic, null).performAsync( new QBEntityCallback<QBFile>() {
    @Override
    public void onSuccess(QBFile qbFile, Bundle params) {


    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

* Set uploaded file url to user's custom data:
```java
 // Connect image to user
    QBUser user = new QBUser();
    user.setId(300);
    user.setCustomData(qbFile.getPublicUrl());

    QBUsers.updateUser(user).performAsync( new QBEntityCallback<QBUser>(){
        @Override
        public void onSuccess(QBUser user, Bundle args) {

        }

        @Override
        public void onError(QBResponseException errors) {

        }
        });
```

To download other user's avatar get custom data & load url using any image loader:
```java
String url = user.getCustomData(); // get user's url

Glide.with(context)
                .load(url)
                .placeholder(R.drawable.placeholder)
                .dontAnimate()
                .into(imageView);
```


## Reset password

If you forgot your password - you can reset it using method bellow.
Email with instruction will be send to your email address.

Reset User's password.
```java
QBUsers.resetPassword("mary987@gmail.com").performAysnc( new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void result, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

## Sign Out

If you want to sign out - just use method bellow:

Remove User's session (logout)
```java
QBUsers.signOut().performAsync( new QBEntityCallback<Void>(){
    @Override
    public void onSuccess(Void result, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

## Retrieve Users for current account


Retrieve all [API Users]() for current account.

====Retrieve all Users====

Use **QBPagedRequestBuilder** to set pagination info for request.

You can use page parameters like:
* '''perPage''' - how many users will contain each page (max 100)
* '''page''' - current page

```java
QBPagedRequestBuilder pagedRequestBuilder = new QBPagedRequestBuilder();
pagedRequestBuilder.setPage(1);
pagedRequestBuilder.setPerPage(50);

Bundle params = new Bundle();

QBUsers.getUsers(pagedRequestBuilder, params).performAsync( new QBEntityCallback<ArrayList<QBUser>>() {
    @Override
    public void onSuccess(ArrayList<QBUser> users, Bundle params) {
        Log.i(TAG, "Users: " + users.toString());
        Log.i(TAG, "currentPage: " + params.getInt(Consts.CURR_PAGE));
        Log.i(TAG, "perPage: " + params.getInt(Consts.PER_PAGE));
        Log.i(TAG, "totalPages: " + params.getInt(Consts.TOTAL_ENTRIES));
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

====Retrieve users by filter====
There are lots of filters to retrieve users:

* Retrieve Users by IDs
* Retrieve Users by Logins
* Retrieve Users by Emails
* Retrieve Users by Phone numbers
* Retrieve Users by Tags. Tags like groups, in which we can group users
* Retrieve Users with full name
* Retrieve User by ID
* Retrieve User by login
* Retrieve User by Facebook ID
* Retrieve User by Twitter ID
* Retrieve User by email
* Retrieve User by external ID. You can connect user from 3d party system to QuickBlox user through external

Use getUsersByXXX methods to retrieve users by filters.
For example following code loads users by login:


```java
QBPagedRequestBuilder pagedRequestBuilder = new QBPagedRequestBuilder();
pagedRequestBuilder.setPage(1);
pagedRequestBuilder.setPerPage(50);

ArrayList<String> usersLogins = new ArrayList<String>();
usersLogins.add("bob");
usersLogins.add("john");

QBUsers.getUsersByLogins(usersLogins, pagedRequestBuilder).performAsync( new QBEntityCallback<ArrayList<QBUser>>() {
    @Override
    public void onSuccess(ArrayList<QBUser> users, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```


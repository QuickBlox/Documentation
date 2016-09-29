# Summary
To send requests and receive responses from QuickBlox your Application must be authenticated.

To authenticate your application you have to set valid a auth_key and generate a signature using your application auth_secret and receive a session token which you have to in each request to QuickBlox API.

> Stating from QuickBlox iOS SDK you don't need to manage session manually. SDK will automatically do all this job.

After login a REST session will be available for 2 hours.

When the session expires, any QBRequest method will firstly renew it and then execute itself.

To be able to use QuickBlox API you have to create a user and be signed in.

<span id="User_Sign_Up" class="on_page_navigation">

# User Sign Up

Use for the identification of the mobile applications users. The request can contain all, some or none of the optional parameters. Login, email, facebook ID, twitter ID and the external user ID should not be taken previously. If you want to create a user with a some content (f.e. with a photo) you have to create a blob firstly. The same tags can be used for any number of users. Please note that trailing whitespaces in string data (except password) will be trimmed.

> Only login/email and password fields are required


To create a user use the following snippet:

```objectivec
QBUUser *user = [QBUUser user];
user.password = @"password";
user.login = @"login";

[QBRequest signUp:user completion:^(QBUUser *user, NSError *error) {

}];
```
<span id="User_Login" class="on_page_navigation">
</span>

# User Login

To login with user login use the following snippet:

```objectivec
[QBRequest logInWithUserLogin:@"admin" password:@"***" completion:^(QBUUser *user, NSError *error) {

}];

```

To login with email use the following snippet:

```objectivec
[QBRequest logInWithUserEmail:@"admin" password:@"***" completion:^(QBUUser *user, NSError *error) {

}];

```

To login with Social Provider use the following snippet:

```objectivec
[QBRequest logInWithUserEmail:@"admin" password:@"***" completion:^(QBUUser *user, NSError *error) {

}];

```

To logout use the following snippet:

```objectivec
[QBRequest logOutWithCompletion:^(NSError *error) {

}];

```

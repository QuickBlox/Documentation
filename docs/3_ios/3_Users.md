
# Summary
The user module manages all things related to user accounts handling, authentication, account data, password reminding etc.

+ OAuth is secure and supports transparent authentication;
+ Password reminder and other typical user management tasks are managed automatically. Reminders and other technical messages are sent via Messages module;
+ Logging in users with their Facebook/Twitter/Twitter Digits accounts is available;
+ User tags – set up user tags and address them separately in your app or through the admin panel – supported by other modules such as Messages;
+ Existing user base integration – our module supports connection to your system by storing your existent IDs table and matching them to new ones.

<span id="Need_to_known_information" class="on_page_navigation"></span>
# Need to know information
Before using the Users module please read the following:
+ [Getting Started]()
+ [Authentication and Authorization]()
+ [Possible errors codes]()

<span id="Retrive_Users" class="on_page_navigation"></span>
# Retrieve all Users for current account

Retrieve all [API Users]() for current account.

<span id="User Sign UP" class="on_page_navigation"></span>
# User Sign Up

Use for the identification of the mobile applications users. The request can contain all, some or none of the optional parameters. Login, email, facebook ID, twitter ID and the external user ID should not be taken previously. If you want to create a user with a some content (f.e. with a photo) you have to create a blob firstly. The same tags can be used for any number of users.
Please note that trailing whitespaces in string data (except password) will be trimmed.


>Only login/email and password fields are required

``` objective-c

QBUUser *user = [QBUUser user];
user.password = @"password";
user.login = @"login";
 
// Registration/sign up of User
[QBRequest signUp:user completion:^(QBUUser *user, NSError *error) {
    // Sign up was successful
}];

//Old Style
[QBRequest signUp:user successBlock:^(QBResponse *response, QBUUser *user) {
    // Sign up was successful
} errorBlock:^(QBResponse *response) {
    // Handle error here
}];

```
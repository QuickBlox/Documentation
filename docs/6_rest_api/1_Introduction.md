REST API provides system-level access to QuickBlox content and actions. It also allows you to integrate the functionality provided by QuickBlox with other services and applications.

For example:

* A web server can interact with QuickBlox Server, for example, show data on a website.
* Headless application (for example, Chat Bot) can connect with QuickBlox Server via QuickBlox REST API.
* You can upload large amounts of data that will later be consumed in a mobile/web app.
* Applications written in any programming language can interact with data on QuickBlox Server.

<br>
QuickBlox iOS, Android and Javascript SDKs use REST API.

<span id="API_introduction" class="on_page_navigation"></span>
# API introduction
All REST API access is over HTTPS, and accessed via the **https://api.quickblox.com** domain.

<span id="Authenticating_requests" class="on_page_navigation"></span>
# Authenticating requests
When someone connects with an app using QuickBlox, the app will be able to obtain an access token which provides temporary, secure access to QuickBlox APIs.

A session token is an opaque string that identifies a user and an app.

Session tokens are obtained via [Create Session request](3_Session_API.md).

Then, because of privacy checks, all REST API requests must be authenticated with a token - the **QB-Token** header of each request to REST API must contain valid session token.

Expiration time for session token is 2 hours after last request to REST API. Be aware about it. If you will perform query with expired token - you will receive error **'Required session does not exist'**. In this case you have to recreate a session token.

Each REST API response contains header **'QB-Token-ExpirationDate'** which contains session token expiration date.

## Access Tokens rights
There are different types of session tokens to support different use cases:

| Session Token Type | Description |
|:----:|:-------:|
| App session token | This kind of access token is needed to read the app data. Has only READ access to resources |
| User session token  | The user token is the most commonly used type of token. This kind of access token is needed any time the app calls an API to read, modify or write a specific user's QuickBlox data on their behalf. Has READ/WRITE access to resources |


You can create **Application session** and then upgrade it to **User session** or you can create **User session** at once. Use [Create Session request](3_Session_API.md).

<span id="Changelog" class="on_page_navigation"></span>
# Changelog

## API and Dashboard changelog

### 3.18.04.1
* **API**
  * New
    * Removed Blackberry push notifications
    * User: removed 'owner_id', 'twitter_digits_id' fields from API output
    * ChatDialog: added 'last_message_id' field
  * Fixed
    * Added DB index to search users by 'twitter_id' field
    * Speed up API request for retrieve users by ids
    * Optimized user selection query in API for login via Custom Identity Provider.
    * Optimized DB query in create new push subscription API
* **Dashboard**
  * New
    * Removed Blackberry push notifications
    * APNS push certificates: allow to upload only universal or non universal cert
  * Fixed
    * Chat: history export not working sometimes
    * Dictionary upload for TnS sometimes not working
* **Other**
   * New
     * ChatAlerts: include 'message_id' param into push payload if %body% tag is in push template
   * Fixed
     * Recurring error in 'period_date' push notifications.

### 3.18.03.1
* **API**
  * New
    * Location/Places modules deprecation
  * Fixed
    * Optimized API '/account_settings.json', remove not used fields: 'account_id' and 's3_bucket'
    * Optimised DB query when create session with user, it was slow when lots of users in app
* **Dashboard**
  * New
    * Location/Places modules deprecation
    * ChatAlerts: ability to enable 'mutable-content' iOS push param in dashboard
  * Fixed
    * Overview: removed 'Reset Credentials' feature.
    * Users: no label for push subscriptions section
    * "We're sorry, but something went wrong." when open not existent page
    * UI is stretched when do it smaller
    * Users: address book section is empty
    * Users: removed "Show only application users" checkbox

### 3.18.02.2
* **Dashboard**
	* New
		* Export chat history feature.
	* Fixed
		* Content: User Id column link is broken.
		* Content: Now you can remove blobs that set as user's avatar (**user.blob_id** field will be nilified).

### 3.18.02.1
* **API**
	* New
		* Tie blobs to Application (instead of Account).
		* Blob object: removed fields from API output: **ref_count, last_read_access_ts, lifetime**.
		* Removed Twitter Digits.
	* Fixed
		* Performance issues when request chat dialog API with custom data filter.
		* Updated API limits violation error message to better variant for end users.  
* **Dashboard**
	* New
		* Tie blobs to Application (instead of Account).
		* Removed Twitter Digits.
	* Fixed
		* Dashboard Session Management Vulnerability.  
		* Application image got changed by itself.
		* Infinite 'processing' on Push Notifications page.
		* Remove blobs after application remove.
* **Other**
	* Fixed
		* Fixed failed push device tokens destruction by errors from APNS.
		* Chat Alerts: 'badge' value do not come in Android push.

### 3.17.10.2
* **API**
	* New
		* AddressBook API: create  [API for retrieve already registered contacts/users](https://quickblox.com/developers/AddressBook)
		* Add **data** into allowed chat attachments attributes.
	* Fixed
		* Optimised 'retrieve chat dialogs' API with additional parameters search.  
* **Dashboard**
	* New
		* Protect Dashboard login from brute force attack.

### 3.17.10.1
* **API**
	* Fixed
		* Degraded performance of 'retrieve Custom Objects' API.  
* **Dashboard**
	* Fixed
		* Application avatar is unavailable after application name change.  
		* Quick Registration: User can be registered with already existing username.
* **Other**
	* Fixed
		* **Reset Password** and **Limits violation** emails do not come when **receive emails** option unchecked in Dashboard panel for account.
		* Degraded performance of storage when delete user (when remove him from all chat dialogs).

### 3.17.09.2
* **API**
	* New
		* Custom Objects. Retrieve objects API: default sort is by **_id** field now.
		* Push Notifications. Create event API: new parameter for universal pushes - **ios_mutable_content=1**.
	* Fixed
		* Push Notifications. Create event API: error 500 appears if create an event with **event_type=fixed_date**
		* Push Notifications. Create event API: an exception occurs when pass **user.ids** as string with single user ID.
		* Custom Objects. Create object API: an exception occurs when pass location field value in wrong format.
		* Chat. Unread Messages Count API: improved logic and speed.
		* Users API: improved DB indexes which lead to better API speed.
		* Authentication. Create session with user API: it returns success response if provided input params in wrong format.
		* Chat: Bulk update API: a duplicate of ID in the **SuccessfullyUpdated** and **NotFound** fields in response is returned.
		* Chat. Update dialog API: group dialog is not automatically destroyed when no occupants left.
		* AddressBook. Delete API: sometimes deleted contacts marked as 'updated' but not 'deleted'.
* **Dashboard**
	* New
		* New Chat Alert template tag: **%plural[]%**. Now you can pluralize words. For example: **%plural[new message]%** will produce **new message** for 1 unread messages count and **new messages** for >1 unread messages count.
	* Fixed
		* Push Notifications: success message appears if send push notification by email channel without existent user subscription.
* **Other**
	* Fixed:
		* Email **Quickblox: your APNS certificate was removed**: sometimes users receive it many times.   

### 3.17.09.1
* **API**
	* New
		* AddressBook REST API
* **Dashboard**
	* New
		* AddressBook UI
		* Moved Application settings ('Allow to retrieve a list of users via API' etc.) to Users module settings page

### 3.17.08.1
* **API**
	* New
		* Introducing new authentication method/provider - Firebase phone authentication (via SMS). New provider **firebase_phone** is added and can be used in **Create Session** and **User Login** requests. Twitter Digits provider is deprecated now, you have to switch to Firebase now.
	* Fixed
		* Authentication via Facebook fails if use latest Facebook Graph API version.

### 3.17.07.1
* **API**
	* New
		* Chat: bulk update API is introduced.  
	* Fixed
		* Login with external providers: to provide better error description.
		* Do not throw an error when creating private chat dialog and pass current user id in **occupants_ids**.
		* Updated errors descriptions when updating group chat dialog **occupants_ids**.
		* Destroy Chat Message with 'force' from unprivileged user returns 200 OK.
		* Users: 500 error appears if try to save Emoji to **custom_data** field.
		* Twitter auth sometimes is not working properly.
* **Dashboard**
	* New
		* 'Main application' feature is removed(legacy).
		* API endpoints buttons are removed.
	* Fixed
		* Admin with 'Read Only' permissions isn't able to read chat messages.
		* Admin with 'Read Only' permissions - unexpected behavior on 'Push Notifications/Queue' page.
		* Custom IdP: added a hint about URL path parameters.
		* Reviewed **Quickblox: your account inactivity** email.
		* 500 error on 'Push Notification/Subscriptions' page.
		* 'Push Notifications/Settings' page: 500 error if one of certificates is broken/invalid.
		* Tables break boundaries on small resolutions (notebooks).
		* Reviewed email **QuickBlox: rate limit for your current plan is exceeded**.
		* Account Stats issue: records are doubled.

### 3.17.05.1
* **API**
	* New
		* Added **client_identification_sequence** & **bundle_identifier** fields in Push subscription response.
	* Fixed
		* Custom Objects API: error 500 is returned if set empty field type in **Create Class** request.
		* Chat Message model's **read_ids** & **delivered_ids** is not empty in public group dialogs messages.
		* Custom Objects API: filter by **_id** is not working.
		* Create Chat Dialog API: it returns an object with **unread_messages_count=null** (should be 0)
		* Can’t use the same external user (FB,TW,..) in 2 different apps.
		* Create Chat Message API: Attachments: not all fields are processed correctly.
		* Incorrect last massage in chat dialog when update chat message via API.
		* Push Notifications API: incorrect behaviour when create an event with external user id and this user does not have any push subscriptions.
* **Dashboard**
	* New
		* To show the **access** field of blob (public or not) in blobs table.
		* Removed search autocomplete on Custom Objects module page (performance concerns).
		* Content module: removed WYSIWYG Editor (legacy & useless).
		* Optimised performance of Push Notifications/Subscriptions page.
	* Fixed
		* Copy credentials feature works only if you have Flash installed. You do not need Flash anymore.

### 3.17.04.1
* **API**
	* New
		* Ability to send universal VoIP push notifications via **ios_voip=1** parameter.
* **Other**
	* Fixed
		* Improved push notifications delivery stability.

### 3.17.03.1
* **API**
	* Fixed
		* Push Notifications: subscription is overrided if install 2 iOS apps on a same device.

### 3.17.02.2
* **API**
	* Fixed
		* POST /subscriptions: allow to pass 'bundle_identifier' parameter not only for iOS push subscriptions (but also for Android).

### 3.17.02.1
* **API**
	* New
		* API for [unread chat message](https://quickblox.com/developers/Chat#Update_message)
	* Fixed
		* Sign In via Twitter Digits performance bottleneck.
		* Sign In with Facebook performance bottleneck.
		* Improved performance of GET /users, PUT /users APIs.
		* Custom Objects: Create new record with permissions API. "Acl" is changed to "permissions" in the error response to not confuse end users.
		* Current 'user_id' is not added to chat messages's 'delivered_ids' field when use GET /chat/Message request
		*  Update user API doesn't work if deleted avatar (connected via user's 'blob_id' field) of user from Content module.
		*  Can't reuse 1-1 chat dialog after deletion.
		*  Users: enable email confirmation: there is no error for unconfirmed users when sign in via API.
		*  POST /chat/message API issue: chat dialog is broken when send not valid xml characters in chat message's body.
* **Dashboard**
	* Fixed
		* Users page is so slow when have > 1M users in single application.

### 3.17.01.1
* **API**
	* New
		* iOS VoIP push notifications. [Blog post about VoIP push notifications features](https://quickblox.com/blog/voip-push-notifications/).
		* Added new field 'bundle_identifier' for push susbcriptions model (to support multiple iOS push certificates feature)
	* Fixed
		* POST /subscriptions API performance bottleneck
* **Dashboard**
	* New
		* Ability to upload multiple iOS push certificates
	* Fixed
		* Content page is so slow when have many records in single application.

### 3.16.12.1
* **API**
	* Fixed
		*  Push notifications with badge parameter are not delivered to device.
* **Dashboard**
	* New
		* Added chat 'dialog_name' template variable into Chat Alerts
		* Improved Custom Identity Provider logic: added 'Allow reuse QuickBlox user' checkbox.

### 3.16.11.3
* **Other**
	* New
		* Moved iOS Push Notifications (APNS) delivery protocol to brand new Apple HTTP/2.

### 3.16.11.2
* **Other**
	* Fixed
		* Stability improvements  

### 3.16.11.1
* **API**
	* Fixed
		* There was no way to create 2 push notifications subscriptions(dev+prod) for single user with same device udid.
* **Dashboard**
	* New
		* Ability to pass URL path parameters for Custom Identity Provider.
	* Fixed
		* Incorrect behaviour when access Content module files with type 'application/octet-stream'.

### 3.16.10.1
* **Other**
	* Fixed
		* Stability improvements.

### 3.16.09.2
* **API**
	* Fixed
		* Speed up search for GET /users/by_full_name API
		* Two different date formats are displayed if perform requests "create custom class with date format field" and "create dialog with custom parameters"
		* Filter chat messages via API by "updated_at" field doesn't work.
		* Custom Objects: incorrect behaviour when upload file to not existent field.
* **Dashboard**
	* Fixed
		* Information link for GCM API Key on the 'Push notifications' page is broken.  
		* Custom Objects table size issue
		* User can not set time seconds and time zone in Custom Objecs's field type "date" when create new record
		* Users:Settings: empty email template can be sent
		* CustomObjects: export of Location field doesn’t work
* **Other**
	* New
		* Email templates new design
		* Ability to customise 'Forgot password' email and page
		* Send an email when APNS certificate is removed because of expiration date.
		* 'unsubscribe' email feature  
		* Add ‘WWW-Authenticate’ header for 401 and 407 statuses
		* Ability to enable/disable custom templates for Greetings, Forgot Password, SignUp confirmation emails
		* Ability to enable user SignUp confirmation logic

### 3.16.09.1
* **Other**
	* Fixed
		* Stability improvements.

### 3.16.07.2
* **Other**
	* Fixed
		* Stability improvements.

### 3.16.07.1
* **Other**
	* New
		* Custom Identity Provider feature

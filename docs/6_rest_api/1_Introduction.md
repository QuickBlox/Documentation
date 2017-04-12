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

### 3.17.04.1
* API
	* New
		* Ability to send universal VoIP push notifications via **ios_voip=1** parameter.
* Other
	* Fixed
		* Improved push notifications delivery stability.

### 3.17.03.1
* API
	* Fixed
		* Push Notifications: subscription is overrided if install 2 iOS apps on a same device. 

### 3.17.02.2
* API
	* Fixed
		* POST /subscriptions: allow to pass 'bundle_identifier' parameter for only for iOS push subscriptions (can't do it for Android).

### 3.17.02.1
* API
	* New
		* API for [unread chat message](https://quickblox.com/developers/Chat#Update_message)
	* Fixed
		* Sign In via Twitter Digits performance bottleneck.
		* Sign In with Facebook performance bottleneck.
		* Improve performance of GET /users, PUT /users APIs.
		* Custom Objects: Create new record with permissions API. "Acl" is changed to "permissions" in the error response to not confuse end users.
		* Current 'user_id' is not added to chat messages's 'delivered_ids' field when use GET /chat/Message request
		*  Update user API doesn't work if deleted avatar (connected via user's 'blob_id' field) of user from Content module.
		*  Can't reuse 1-1 chat dialog after delete.
		*  Users: enable email confirmation: there is no error for unconfirmed users
		*  POST /chat/message API issue: chat dialog is broken when send not valid xml characters in chat message's body.
* Dashboard
	* Fixed
		* Users page is so slow when have > 1M users in single application.	

### 3.17.01.1
* API
	* New
		* iOS VoIP push notifications.
		* Added new field 'bundle_identifier' for push susbcriptions model (to support multiple iOS push certificates feature)
	* Fixed
		* POST /subscriptions API performance bottleneck
* Dashboard
	* New
		* Ability to upload multiple iOS push certificates
	* Fixed
		* Content page is so slow when have many records in single application. 


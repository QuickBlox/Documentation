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
TBA


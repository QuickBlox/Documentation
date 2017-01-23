The Server APIs consists of REST APIs and XMPP APIs.

It provides system-level access to QuickBlox content and actions. It also allows you to integrate the functionality provided by QuickBlox with other services and applications.

For example:

* A web server can interact with QuickBlox Server, for example, show data on a website.
* Headless application (for example, Chat Bot) can connect with QuickBlox Server via QuickBlox Server APIs
* You can upload large amounts of data that will later be consumed in a mobile/web app.
* Applications written in any programming language can interact with data on QuickBlox Server.

<br>
QuickBlox iOS, Android and Javascript SDKs use Server APIs.

<span id="API_introduction" class="on_page_navigation"></span>
# API introduction
All REST API access is over HTTPS, and accessed via the **https://api.quickblox.com** domain.

All XMPP API access is over TLS, and accessed via the **chat.quickblox.com:5223** domain. For Web applications it's also possible to use BOSH/WebSockets endpoints (**https://chat.quickblox.com:5281** and **wss://chat.quickblox.com:5291**).

## Authenticating requests
Server API requests must be authenticated with a token generated vie REST API, [Create session request](Session_API.html#Create_session).

The **QB-Token** header of each REST API request should contain valid session token. 

Expiration time for session token is 2 hours after last request to REST API. Be aware about it. If you will perform query with expired token - you will receive error **Required session does not exist**. In this case you have to recreate a session. 

Each REST API response contains header **QB-Token-ExpirationDate** which contains session token expiration date.

<span id="Changelog" class="on_page_navigation"></span>
# Changelog
TDB

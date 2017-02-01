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
All XMPP API access is over TLS, and accessed via the **chat.quickblox.com:5223** domain. 

For Web applications it's also possible to use BOSH/WebSockets endpoints (**https://chat.quickblox.com:5281** and **wss://chat.quickblox.com:5291**).

<span id="Authenticating_requests" class="on_page_navigation"></span>
# Authenticating requests
TDB

## Access Rights
A session token can be one of 2 types:

* **Application session** - has only READ access to resources.
* **User session** - has READ/WRITE access to resources.
<br>

You can create **Application session** and then upgrade it to **User session** or you can create **User session** at once. 
 
<span id="Changelog" class="on_page_navigation"></span>
# Changelog

## API and Dashboard changelog
TBA

## Real-time Chat API changelog
TBA

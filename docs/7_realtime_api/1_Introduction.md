Real-time(XMPP) API is a quick and reliable solution which combines benefits of scalable cloud hosted XMPP chat server, seamless authorization and incoming IM / chat alerts. It's robust, quick and auto-scalable AWS powered cloud servers infrastructure. It's the best and most comprehensive solution so far to have your users communicate cross-platform. 

The following QuickBlox module are built on top of eal-time(XMPP) API:

* Text Chat (messaging) module
* Video Chat (calling) module

<br>
QuickBlox iOS, Android and Javascript SDKs use Real-time(XMPP) APIs.

<span id="XMPP_features_supported" class="on_page_navigation"></span>
# XMPP features supported
All standard XMPP libraries are supported (please check the list here [http://xmpp.org/xmpp-software/libraries](http://xmpp.org/xmpp-software/libraries)). All standard XEPs and the following additional ones (below) are supported: 

* RFC-6120 - [http://xmpp.org/rfcs/rfc6120.html](http://xmpp.org/rfcs/rfc6120.html) - Core: SSL/TLS stream encryption, SASL authentication, Resource binding etc
* RFC-6121 - [http://xmpp.org/rfcs/rfc6121.html](http://xmpp.org/rfcs/rfc6121.html) - Instant Messaging and Presence
* XEP-0016 - [http://xmpp.org/extensions/xep-0016.html](http://xmpp.org/extensions/xep-0016.html) - Privacy Lists
* XEP-0203 - [http://xmpp.org/extensions/xep-0203.html](http://xmpp.org/extensions/xep-0203.html) - Offline Message Storage
* XEP-0280 - [http://xmpp.org/extensions/xep-0280.html](http://xmpp.org/extensions/xep-0280.html) - Message Carbons
* XEP-0198 - [http://xmpp.org/extensions/xep-0198.html](http://xmpp.org/extensions/xep-0198.html) - Stream Management
* XEP-0085 - [http://xmpp.org/extensions/xep-0085.html](http://xmpp.org/extensions/xep-0085.html) - Chat State Notifications
* XEP-0079 - [http://xmpp.org/extensions/xep-0079.html](http://xmpp.org/extensions/xep-0079.html) - Advanced Message Processing
* XEP-0045 - [http://xmpp.org/extensions/xep-0045.html](http://xmpp.org/extensions/xep-0045.html) - Multi User Chat
* XEP-0047 - [http://xmpp.org/extensions/xep-0047.html](http://xmpp.org/extensions/xep-0047.html) - In-Band Bytestreams (p2p file transfer)
* XEP-0333 - [http://xmpp.org/extensions/xep-0333.html](http://xmpp.org/extensions/xep-0333.html) - Chat Markers
* XEP-0308 - [http://xmpp.org/extensions/xep-0308.html](http://xmpp.org/extensions/xep-0308.html) - Last Message Correction
* XEP-0174 - [http://xmpp.org/extensions/xep-0174.html](http://xmpp.org/extensions/xep-0174.html) - Serverless Messaging
* XEP-0237 - [http://xmpp.org/extensions/xep-0237.html](http://xmpp.org/extensions/xep-0237.html) - Roster Versioning
* XEP-0012 - [http://xmpp.org/extensions/xep-0012.html](http://xmpp.org/extensions/xep-0012.html) - Last Activity
* XEP-0352 - [http://xmpp.org/extensions/xep-0352.html](http://xmpp.org/extensions/xep-0352.html) - Client State Indication
* XEP-0202 - [http://xmpp.org/extensions/xep-0202.html](http://xmpp.org/extensions/xep-0202.html) - Entity Time
* XEP-0033 - [http://xmpp.org/extensions/xep-0033.html](http://xmpp.org/extensions/xep-0033.html) - Extended Stanza Addressing (not supported yet)
* XEP-0334 - [http://xmpp.org/extensions/xep-0334.html](http://xmpp.org/extensions/xep-0334.html) - Message Processing Hints (not supported yet)

XEP-0095 & XEP-0096 are not supported, instead QuickBlox own TURN server takes care of NAT traversal for voice and video traffic streaming. 

<span id="API_introduction" class="on_page_navigation"></span>
# API introduction
All XMPP API access is over TLS, and accessed via the **chat.quickblox.com:5223** domain. 

For Web applications it's also possible to use BOSH/WebSockets endpoints (**https://chat.quickblox.com:5281** and **wss://chat.quickblox.com:5291**).

<span id="Authenticating_requests" class="on_page_navigation"></span>
# Authenticating requests
In order to connect to Real-Time API you have to create a user. You can do it on [QuickBlox dashboard](https://admin.quickblox.com) or via Users REST API. Then you will user the user's login+password to authenticate via Real-Time API.

## Login / ID
Each user gets a JID (Jabber ID) in the following format:  
```
<user_id>-<application_id>@chat.quickblox.com 
```

## Password
Your user’s password for XMPP connection depends on what type of user authentication you have applied for this particular user:

* Standard login+password authentication: use same password.
* Facebook/Twitter/Twitter Digits/Custom identity authentication: use QuickBlox session token as password.

<span id="Login_flow" class="on_page_navigation"></span>
# Handshake/Login Flow
Use this documentation to understand what the typical stanzas used in the QuickBlox XMPP handshake flow are.

**SASL Authentication Handshake Begin**

Client:

```xml
<open xmlns='urn:ietf:params:xml:ns:xmpp-framing' to='chat.quickblox.com' version='1.0'/>
```
Server:

```xml
<open xmlns='urn:ietf:params:xml:ns:xmpp-framing' from='chat.quickblox.com' id='e4b1d1be-45a9-4d1a-aea1-a5d17de4ecae' version='1.0' xml:lang='en' />
```
Server:

```xml
<stream:features xmlns:stream="http://etherx.jabber.org/streams">
  <sm xmlns="urn:xmpp:sm:3"/>
  <mechanisms xmlns="urn:ietf:params:xml:ns:xmpp-sasl">
    <mechanism>PLAIN</mechanism>
    <mechanism>ANONYMOUS</mechanism>
    <mechanism>PLAIN_FAST</mechanism>
  </mechanisms>
  <ver xmlns="urn:xmpp:features:rosterver"/>
  <starttls xmlns="urn:ietf:params:xml:ns:xmpp-tls"/>
  <compression xmlns="http://jabber.org/features/compress">
  	 <method>zlib</method>
  </compression>
</stream:features>
```

Client:

```xml
<auth xmlns='urn:ietf:params:xml:ns:xmpp-sasl' mechanism='PLAIN'>MjY5MDQ1OTQtMjk2NTBAY2hhdC5xdWlja2Jsb3guY29tADI2OTA0NTk0LTI5NjUwAGpzX2phc21pbmUyMjI=</auth>
```

Server:

```xml
<success xmlns="urn:ietf:params:xml:ns:xmpp-sasl"/>
```

**SASL Authentication Handshake End**

Client:

```xml
<open xmlns='urn:ietf:params:xml:ns:xmpp-framing' to='chat.quickblox.com' version='1.0'/>
```

Server:

```xml
<open xmlns='urn:ietf:params:xml:ns:xmpp-framing' from='chat.quickblox.com' id='e4b1d1be-45a9-4d1a-aea1-a5d17de4ecae' version='1.0' xml:lang='en' />
```

Server:

```xml
<stream:features xmlns:stream="http://etherx.jabber.org/streams">
  <sm xmlns="urn:xmpp:sm:3"/>
  <ver xmlns="urn:xmpp:features:rosterver"/>
  <starttls xmlns="urn:ietf:params:xml:ns:xmpp-tls"/>
  <compression xmlns="http://jabber.org/features/compress">
    <method>zlib</method>
  </compression>
  <bind xmlns="urn:ietf:params:xml:ns:xmpp-bind"/>
  <session xmlns="urn:ietf:params:xml:ns:xmpp-session"/>
</stream:features>
```

**Bind Resource Begin**

Client:

```xml
<iq type='set' id='_bind_auth_2' xmlns='jabber:client'><bind xmlns='urn:ietf:params:xml:ns:xmpp-bind'/></iq>
```

Server:

```xml
<iq to="26904594-29650@chat.quickblox.com/1571722472-quickblox-2386480" xmlns="jabber:client" type="result" id="_bind_auth_2"><bind xmlns="urn:ietf:params:xml:ns:xmpp-bind"><jid>26904594-29650@chat.quickblox.com/1571722472-quickblox-2386480</jid></bind></iq>
```

**Bind Resource End**

**Initial Presence Begin**

Client:

```xml
<presence xmlns='jabber:client'/>
```

Server:

```xml
<presence to="26904594-29650@chat.quickblox.com" from="26904594-29650@chat.quickblox.com/1571722472-quickblox-2386480" xmlns="jabber:client"/>
```

**Initial Presence End**

<span id="Changelog" class="on_page_navigation"></span>
# Changelog

## Real-time(XMPP) API changelog
TBA

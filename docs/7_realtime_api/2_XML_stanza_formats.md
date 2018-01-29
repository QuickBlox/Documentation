QuickBlox Real-time API is based on origin XMPP standards, but with slighty changes. Here we list the XML stanzas formats for every modified feature.

<span id="Send_receive_chat_message" class="on_page_navigation"></span>
# Send/Receive chat message 

A message's **type** attribute can be **chat** or **groupchat**:.

* **chat** is used for 1-1 messages. 
* **groupchat** is used for 1-1 messages. 

**1-1 message:**

```xml
<message id="53fc4604565c128132016612" to="26904594-29650@chat.quickblox.com" type="chat">
	<body xmlns="jabber:client">This is 1-1 message!</body>
	<extraParams xmlns="jabber:client">
		<save_to_history>1</save_to_history>
		<date_sent>1409146118</date_sent>
	</extraParams>
</message>
```

**group chat message:**

```xml
<message id="53ac4604565c128132016614" to="11492_53fc460b515c128132016675@muc.chat.quickblox.com" type="groupchat">
	<body xmlns="jabber:client">This is group chat message!</body>
	<extraParams xmlns="jabber:client">
		<save_to_history>1</save_to_history>
		<date_sent>1409146118</date_sent>
	</extraParams>
</message>
```

QuickBlox introduces a very simple server-side chat history functionality. This features is useful for many cases such as moderation, restoring messages on a new device via REST, making chat logs be easily downloadable, etc. We (Quickblox) are not able to read your messages - only you, as the account owner, have access to them.

Just by adding a custom parameters **save_to_history** to your XMPP messages, the messages will be stored on the server.

You'll notice there's also a **date_sent** field in the extra parameters. This is optional - if you don't supply it, the server will automatically add it when it receives the message. But if for example, you need to "resend" a message that failed, you can use this field to specify the correct time. It is in a unix timestamp format.

**receive message format:**

```xml
<message xmlns="jabber:client" from="92_54789d40535c12b1a5001172@muc.chat.quickblox.com/1501966" to="1501966-92@chat.quickblox.com/0C1E1229-6B3C-47F3-BFDC-D1CFD351D274" type="groupchat" id="54789d5e6df5e6f921b7acda">
    <body>Nice to met you!</body>
    <extraParams xmlns="jabber:client">
        <save_to_history>1</save_to_history>
        <date_sent>1409146118</date_sent>
        <dialog_id>54789d40535c12b1a5001172</dialog_id>
    </extraParams>
</message>
```

As you can see, server automatically adds a **dialog_id** extra parameter so a recipient knows to which chat dialog this message relates.

<span id="File_attachments" class="on_page_navigation"></span>
# File attachments
Each attachment should be included into **extraParams** part of a message.

Each attachment contains the following attributes:

* type - type of the attachment. Could be any. Recommended values: image, video, audio, location.
* id (optional)- a link to a file, for example ID of a file in Content or Custom Objects modules
* url (optional) - an url to file in the Internet
* data (optional) - an attachment's data. For example, used for location attachments to pass lat & lon.
* size (optional) - size of file
* width (optional) - if attachment is an image - an image's width
* height (optional) - if attachment is an image - an image's height
* duration (optional) - if attachment is an audio/video - an image's height
* content-type (optional) - an attachment's content type
* name (optional) - an attachment's name

**format:**

```xml
<message id="546dbf438c54ad2aded63b25" type="groupchat" to="2_546d14c19c29532398000496@muc.chat.quickblox.com">
    <body>Nice to met you!</body>
    <extraParams xmlns="jabber:client">
        <save_to_history>1</save_to_history>
        <attachment type="video" id="47863" duration="894"></attachment>
        <attachment type="image" id="123123" width="640" height="480"></attachment>
        <attachment type="audio" id="323232" duration="365"></attachment>
        <attachment type="location" data="{&quot;lat&quot;:50.01383403152402,&quot;lng&quot;:36.22868299484253}"></attachment>
    </extraParams>
</message>
```

<span id="Is_typing_status" class="on_page_navigation"></span>
# 'Is typing' status

A **type** can be **chat** or **groupchat**, so you can send 'is typing' statuses in 1-1 or group chats.

**composing:**

```xml
<message id="55e416e3854516bc190041a8" type="chat" to="4374458-92@chat.quickblox.com">
    <composing xmlns="http://jabber.org/protocol/chatstates"/>
</message>
```

**paused:**

```xml
<message id="55e4175a854516bc190041a9" type="chat" to="4374458-92@chat.quickblox.com">
    <paused xmlns="http://jabber.org/protocol/chatstates"/>
</message>
```

<span id="Delivered_status" class="on_page_navigation"></span>
# 'Delivered' status
A **type** can be **chat** only, so you send 'delivered' statuses directly to users.

```xml
<message to="2308497-92@chat.quickblox.com" type="chat" id="55e821cd0ce78744090041b2">
    <received xmlns="urn:xmpp:chat-markers:0" id="55e818f6e24a0998640041ed"/>
    <extraParams xmlns="jabber:client">
        <dialog_id>55e6a9c8a28f9a3ea6001f15</dialog_id>
    </extraParams>
</message>
```

<span id="Read_status" class="on_page_navigation"></span>
# 'Read' status
A **type** can be **chat** only, so you send 'read' statuses directly to users.

```xml
<message to="2308497-92@chat.quickblox.com" type="chat" id="55e821cd0ce78744090041b2">
    <displayed xmlns="urn:xmpp:chat-markers:0" id="55e818f6e24a0998640041ed"/>
    <extraParams xmlns="jabber:client">
        <dialog_id>55e6a9c8a28f9a3ea6001f15</dialog_id>
    </extraParams>
</message>
```

<span id="System_messages" class="on_page_navigation"></span>
# System messages
There is a special channel for any system notifications. The end user can use it to split regular chat messages and other system events.

All such messages should contain **extraParams.moduleIdentifier=SystemNotifications** and use **type=headline**.

You can send system messages directly to user only (grou chat is not supported).

**format:**

```xml
<message id="..." type="headline" to="...@chat.quickblox.com">
    <extraParams xmlns="jabber:client">
        <moduleIdentifier>SystemNotifications</moduleIdentifier>
        <param1>value1</param1>
        <param2>value2</param2>
        ...
    </extraParams>
</message>
```

<span id="Stream_management" class="on_page_navigation"></span>
# Stream management
Stream management ([XEP-0198](http://xmpp.org/extensions/xep-0198.html)) defines an XMPP protocol extension for active management of a stream between two users, including features for stanza acknowledgements and stream resumption. This protocol aims to resolve the issue with lost messages in a case of very poor Internet connection.

The basic concept behind stream management is that the initiating entity (a client) and the receiving entity (a server) can exchange "commands" for active management of the stream. The following stream management features are of particular interest because they are expected to improve network reliability and the end-user experience:

* Stanza Acknowledgements -- the ability to know if a stanza or series of stanzas has been received by one's peer.
* Stream Resumption -- the ability to quickly resume a stream that has been terminated.

The main interesting part here is **Stanza Acknowledgements** because it allows to achieve 100% reliability. To better understand how it works we prepared the follosing picture:

![XEP-0198 Stream management flow](./resources/images/xep-0198_flow.png)

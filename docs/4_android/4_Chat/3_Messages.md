<span id="Messages" class="on_page_navigation"></span>
# Messages
You can choose a way to save chat message to history or not. If you decided to store - you should call method ```setSaveToHistory(true)```:

```java
QBChatMessage chatMessage = new QBChatMessage();
chatMessage.setSaveToHistory(true);
...
```

**List chat messages**
<br>If you decided to use ```chatMessage.setSaveToHistory(true)``` parameter - all chat messages will be stored to history and
user can request a chat history for particular dialog.

<span id="Create_and_send_chat_message_via_REST_request" class="on_page_navigation"></span>
## Create/send chat message via REST request

To create/send chat message via REST request you can use next snippet:
```java
QBChatMessage msg = new QBChatMessage();
msg.setBody("hello worls");

msg.setRecipientId(223);
// msg.setDialogId("546cc8040eda8f2dd7ee449c"); Set the dialog Id or recipient Id

QBAttachment attachment = new QBAttachment("photo");
attachment.setId("3451");
msg.addAttachment(attachment);

msg.setProperty("param1", "value1");
msg.setProperty("param2", "value2");

boolean sendToDialog = true; //set true for send this message to the chat or false for just create it. 

QBRestChatService.createMessage(chatMessage, sendToDialog).performAsync(new QBEntityCallback<QBChatMessage>() {
    @Override
    public void onSuccess(QBChatMessage message, Bundle bundle) {

    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```

<span id="Retrieve_list_of_messages" class="on_page_navigation"></span>
## Retrieve list of messages

To get a chat history for particular dialog use next request:
```java
QBChatDialog chatDialog = new QBChatDialog("53cfc593efa3573ebd000017");

QBMessageGetBuilder messageGetBuilder = new QBMessageGetBuilder();
messageGetBuilder.setLimit(500);

QBRestChatService.getDialogMessages(chatDialog, messageGetBuilder).performAsync(new QBEntityCallback<ArrayList<QBChatMessage>>() {
    @Override
    public void onSuccess(ArrayList<QBChatMessage> qbChatMessages, Bundle bundle) {

    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```

#### Filters

There are some filters to get only chat messages you need, not just all.


You can apply filters for the following fields:
* _id (string)
* message (string)
* date_sent (timestamp)
* sender_id (integer)
* recipient_id (integer)
* attachments.type (string)
* updated_at (date)

You can apply **sort** for the following fields:
* date_sent


Filters:

Operator|Description|Usage example
--------|-----------|-------------
|**{field_name}** | Search records with field which contains exactly specified value | ```messageGetBuilder.addRule("sender_id", QueryRule.EQ, "547263");```
|**{field_name}[{search_operator}]** | Search record with field which contains value according to specified value and operator.<br><br>Possible filters: **lt** (**L**ess **T**han operator), **lte** (**L**ess **T**han or **E**qual to operator), **gt** (**G**reater **T**han operator), **gte** (**G**reater **T**han or **E**qual to operator), **ne** (**N**ot **E**qual to operator), **in** (Contained **IN** array operator), **nin** (**N**ot contained **IN** array), **all** (**ALL** contained **IN** array), **or** (OR operator), **ctn** (Contains substring operator) | ```messageGetBuilder.gt("updated_at", "1455098137");```
|**limit** | Limit search results to **N** records. Useful for pagination. Default value - 100 | ```messageGetBuilder.setLimit(100);```
|**skip** | Skip **N** records in search results. Useful for pagination. Default (if not specified): 0 | ```messageGetBuilder.setSkip(50);```
|**sort_desc/sort_asc** | Search results will be sorted by specified field in ascending/descending order | ```messageGetBuilder.sortAsc("date_sent");```


To use filters, you should build a ```QBRequestGetBuilder``` request and pass it to *get messages* request above:

```java
QBMessageGetBuilder messageGetBuilder = new QBMessageGetBuilder();
messageGetBuilder.setLimit(500);
messageGetBuilder.setSkip(5);
messageGetBuilder.sortAsc("date_sent");
messageGetBuilder.gt("updated_at", "1455098137");
```

> <span style="color:red;">Note</span>: All retrieved chat messages will be marked as read after request. 
If you decided not to mark chat messages as read add next parameter to request: ```messageGetBuilder.markAsRead(false);```

<span id="Update_chat_messages" class="on_page_navigation"></span>
## Update chat messages
User can update ```QBChatMessage``` for marking it as delivered/read or for changing messages'text.

To update chat messages use ```QBMessageUpdateBuilder``` like in next snippet:

```java
QBMessageUpdateBuilder messageUpdateBuilder = new QBMessageUpdateBuilder();
messageUpdateBuilder.updateText(newBody) //updates message's text
                    .markDelivered()     //mark message as delivered on server
                    .markRead();         //mark message as read on server

QBRestChatService.updateMessage(messageId, dialogId, messageUpdateBuilder).performAsync(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void aVoid, Bundle bundle) {

    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```

For more information refer to http://quickblox.com/developers/Chat#Update_message

<span id="Mark_messages_as_read_via_REST_request" class="on_page_navigation"></span>
## Mark messages as read via REST

You can mark many messages as read on server using snippet:
```java
StringifyArrayList<String> messagesIDs = ...;
QBRestChatService.markMessagesAsRead(dialogId, messagesIDs).performAsync(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void aVoid, Bundle bundle) {

    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```

It's possible to mark **all messages as read** - **just don't pass ```messagesIDs```.** 
```java
QBRestChatService.markMessagesAsRead(dialogId, null).performAsync(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void aVoid, Bundle bundle) {

    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```
>Keep in mind, when you mark messages as delivered/read via REST, other user will not know about it realtime. 
If you want to opponent get info about changing message's status, you have to mark it delivered/read via chat, for details refer to [stub link to docs]()

<span id="Delete_chat_messages" class="on_page_navigation"></span>
## Delete chat messages

To delete chat messages use next snippets:
```java
Set<String> messagesIds = new HashSet<String>() {{
    add("546cc8040eda8f2dd7ee449c"); 
    add("546cc80f0eda8f2dd7ee449d");
}};

QBRestChatService.deleteMessages(messagesIDs, false).performAsync(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void aVoid, Bundle bundle) {
                
    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```

This request will remove chat messages for current user, but other users still will be able to request them.

**Use parameter ```forceDelete = true``` to completely remove messages as described here**: http://quickblox.com/developers/Chat#Delete_message
```java
Set<String> messagesIds = ...;
boolean forceDelete = true;

QBRestChatService.deleteMessages(messagesIds, forceDelete).perform(); //wrap synchronous method in background thread. 
```
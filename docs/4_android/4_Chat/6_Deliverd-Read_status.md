<span id="Delivered_status" class="on_page_navigation"></span>
## Delivered status

SDK automatically manages delivery notifications for all 'online' messages if you use marked messages.

Term 'marked' relies to messages that have automatic delivery control.

To mark a message as 'markable' use next message property:

```java
QBChatMessage chatMessage = new QBChatMessage();
chatMessage.setMarkable(true);
...
```

For such messages you will receive delivery confirmation:
```java
private QBMessageStatusesManager messageStatusesManager;
private QBMessageStatusListener messageStatusListener;

// call it after chat login
messageStatusesManager = QBChatService.getInstance().getMessageStatusesManager();

messageStatusListener = new QBMessageStatusListener() {
    @Override
    public void processMessageDelivered(String messageId, String dialogId, Integer userId) {

    }

    @Override
    public void processMessageRead(String messageId, String dialogId, Integer userId) {
    
    }
};

messageStatusesManager.addMessageStatusListener(messageStatusListener);
```

You can disable automatically manages delivery notifications for all 'online' messages, for it you can configure ```QBChatService``` before login to the chat, just use ```configurationBuilder.setAutoMarkDelivered(false)```. [More there](). 

It is also possible to send a 'delivered' status manually, for example for messages received via REST API:
```java
QBChatMessage message = ...;

try {
    chatDialog.deliverMessage(message);
} catch (XMPPException | SmackException.NotConnectedException e) {

}
```
<br>

<span id="Read_status" class="on_page_navigation"></span>
## Read status

You can manage 'read' notifications in chat. For example, User1 sends messages to User2 and User1 would like to know when User2 reads these messages.

First of all, if User1 would like to handle **read** status of his messages, he should mark message as markable. To mark a message as 'markable' use ```chatMessage.setMarkable(true)``` class method to create a message instance. 


User1 sends a message:
```java
QBChatDialog chatDialog = ...;
try {
    QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("Hi there!");
    chatMessage.setSaveToHistory(true); // Save a message to history
    chatMessage.setMarkable(true);
 
    chatDialog.sendMessage(chatMessage);

    // save packet ID of current message
    String _id = chatMessage.getId();
} catch (XMPPException | SmackException.NotConnectedException e) {
 
}
```

User2 receives a message, reads it and sends **read** status back:
```java
// QBChatDialogMessageListener
 
...

@Override
public void processMessage(String dialogId, QBChatMessage chatMessage, Integer senderId) {
    ...
    if(chatMessage.isMarkable()){
        try {
            chatDialog.readMessage(chatMessage);
        } catch (XMPPException | SmackException.NotConnectedException e) {
        
        } 
    }
    ...
}

...
```

User1 receives a **read** status notification that User2 read his message:
```java
private QBMessageStatusesManager messageStatusesManager;
private QBMessageStatusListener messageStatusListener;
 
// call it after chat login
messageStatusesManager = QBChatService.getInstance().getMessageStatusesManager();
 
messageStatusListener = new QBMessageStatusListener() {
    @Override
    public void processMessageDelivered(String messageId, String dialogId, Integer userId) {
 
    }
 
    @Override
    public void processMessageRead(String messageId, String dialogId, Integer userId) {
 
    }
};
 
messageStatusesManager.addMessageStatusListener(messageStatusListener);
```

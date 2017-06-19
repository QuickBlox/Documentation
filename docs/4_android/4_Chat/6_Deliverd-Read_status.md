<span id="Delivered_read_status" class="on_page_navigation"></span>
## Delivered/read status
QuickBlox provide opportunity to mark messages as delivered or read. You can use this for notifying sender about whether the message was delivered/read or not.

For marking messages as delivered/read QuickBlox Android SDK use ```QBChatDialog``` as and for other chat functions.
Of course, this ```QBChatDialog``` mast be initialized with current chat connection.

<span id="Mark_message_as_delivered" class="on_page_navigation"></span>
### Mark message as delivered

QuickBlox Android SDK automatically mark messages as delivered marked messages. For make make message as ```marked``` 
just set next property fot ```QBChatMessage``` ```chatMessage.setMarkable(true)```;

You can disable automatically marking messages as delivered, for it you can 
configure ```QBChatService``` before login to the chat, just use ```configurationBuilder.setAutoMarkDelivered(false)```. [More there]().

For marking message as delivered manually (for example for messages received via REST API) you can use next code snippet:
```java
QBChatMessage message = ...;
QBChatDialog chatDialog = ...;

chatDialog.deliverMessage(message);
```

<span id="Mark_message_as_read" class="on_page_navigation"></span>
### Mark messages as read

For marking message as read you can use next code snippet:
```java
QBChatMessage message = ...;
QBChatDialog chatDialog = ...;

chatDialog.readMessage(message);
```

<span id="Listen_message_s_status" class="on_page_navigation"></span>
### Listen message's status
For listening messages statuses uses ```QBMessageStatusesManager``` and ```QBMessageStatusListener``` 
There code for listening delivered/read status:
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
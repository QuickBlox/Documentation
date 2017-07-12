<span id="Delivered_read_status" class="on_page_navigation"></span>
## Delivered/read status
QuickBlox provides opportunity to mark messages as delivered or read. You can use this to notify sender whether the message was delivered/read or not.

To mark messages as delivered/read QuickBlox Android SDK use ```QBChatDialog``` as and for other chat functions 
(используйте QBChatDialog как и для остальных чат функций).
Of course, this ```QBChatDialog``` should be initialized with current chat connection.

<span id="Mark_message_as_delivered" class="on_page_navigation"></span>
### Mark message as delivered

QuickBlox Android SDK automatically marks messages as delivered marked messages (автоматически наше СДК маркает только маркейбл месседжи, т.е. те, котором 
установлен параметр setMarkable(true)). For make message as ```marked``` (для того, чтобы сделать мессадж  маркабле, надо выполнить такой код) 
just set next property fot ```QBChatMessage``` ```chatMessage.setMarkable(true)```;

You can disable automatic marking messages as delivered. To do that you can 
configure ```QBChatService``` before login to the chat, just use ```configurationBuilder.setAutoMarkDelivered(false)```. [More there]().

To mark message as delivered manually (for example for messages received via REST API) you can use next code snippet:
```java
QBChatMessage message = ...;
QBChatDialog chatDialog = ...;

chatDialog.deliverMessage(message);
```

<span id="Mark_message_as_read" class="on_page_navigation"></span>
### Mark messages as read

To mark message as read you can use the following code snippet:
```java
QBChatMessage message = ...;
QBChatDialog chatDialog = ...;

chatDialog.readMessage(message);
```

<span id="Listen_message_s_status" class="on_page_navigation"></span>
### Listen message's status
To listen status of the message use ```QBMessageStatusesManager``` and ```QBMessageStatusListener``` 
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
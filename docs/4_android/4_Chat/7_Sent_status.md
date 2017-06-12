==Sent status==

You can track the 'sent' status of your message. It means that your message has been delivered to the server.

To start using the feature you should enable Stream Management first.

**Pay attention: you should enable Stream Management before logging into the chat. Stream Management is initialized only during login step.**
```java
QBChatService chatService = QBChatService.getInstance();

chatService.setUseStreamManagement(true);
chatService.login(currentUser);
```

The Stream Management defines an extension for active management of a stream between client and server, including features for stanza acknowledgements.

Next - you can attach the ```QBChatDialogMessageSentListener``` listener to your chat and track the status:
```java
QBChatDialogMessageSentListener messageSentListener = new QBChatDialogMessageSentListener() {
    @Override
    public void processMessageSent(String dialogId, QBChatMessage qbChatMessage) {

    }

    @Override
    public void processMessageFailed(String dialogId, QBChatMessage qbChatMessage) {

    }
};
        
QBChatDialog chatDialog = ...;        
chatDialog.addMessageSentListener(messageSentListener);
```

The same can be done for group chats.

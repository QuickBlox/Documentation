<span id="Message_carbons" class="on_page_navigation"></span>
## Message carbons
This feature defines an approach for ensuring that all of user's devices get both sides of all conversations in order to avoid confusion. Information about the current state of a conversation is shared between all of a user's clients that enable this feature.

Is it convenient to enable carbons after chat login:
```java
try {
    QBChatService.getInstance().enableCarbons();

    // QBChatService.getInstance().disableCarbons();
} catch (XMPPException e) {

} catch (SmackException e) {

}
```

For example, a user is online on 2 devices, phone and pad. On his **phone** he sends a message to other user:

```java
Integer opponentId = 45;
 
try {
    QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("Hi there!");
    chatMessage.setProperty("save_to_history", "1"); // Save a message to history
 
    QBPrivateChat privateChat = privateChatManager.getChat(opponentId);
    
    if (privateChat == null) {
        privateChat = privateChatManager.createChat(opponentId, privateChatMessageListener);
    }
    
    privateChat.sendMessage(chatMessage);
} catch (XMPPException e) {
 
} catch (SmackException.NotConnectedException e) {
 
}
```

In this moment he also receives the message on his **pad** device:
```java
privateChatMessageListener = new QBMessageListener<QBPrivateChat>() {
    @Override
    public void processMessage(QBPrivateChat privateChat, final QBChatMessage chatMessage) {

        if(chatMessage.getSenderId().equals(chatService.getUser().getId())){
            // Message comes here from carbons
        }
    }

    @Override
    public void processError(QBPrivateChat privateChat, QBChatException error, QBChatMessage originMessage){
                
    }
};
```


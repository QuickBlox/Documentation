<span id="Message_carbons" class="on_page_navigation"></span>
## Message carbons
This feature defines an approach for ensuring that all of user's devices get both sides of all conversations in order to avoid confusion. Information about the current state of a conversation is shared between all of a user's clients that enable this feature.

Is it convenient to **enable** carbons after chat login:
```java
QBChatService.getInstance().enableCarbons();
```

To **disabling** carbons use:
```java
QBChatService.getInstance().disableCarbons();
```

For example, a user is online on 2 devices, phone and pad. On his **phone** he sends a message to other user:

```java

QBChatDialog chatDialog = ...;

QBChatMessage chatMessage = new QBChatMessage();
chatMessage.setBody("Hi there!");
chatMessage.setProperty("save_to_history", "1"); // Save a message to history

chatDialog.sendMessage(chatMessage);
```

In this moment he also receives the message on his **pad** device:
```java
QBChatDialogMessageListener messagesListener = new QBChatDialogMessageListener() {
    @Override
    public void processMessage(String dialogId, QBChatMessage qbChatMessage, Integer senderId) {
        if(senderId.equals(chatService.getUser().getId())){
            // Message comes here from carbons
        }
    }
    ...
};
```

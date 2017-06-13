<span id="Typing_status" class="on_page_navigation"></span>
## Typing status

Many instant messaging systems include notifications about the state of one's conversation partner in a one-to-one chat (or, sometimes, in a many-to-many chat). In essence, chat state notifications can be thought of as a form of chat-specific presence. 

This section describes how to integrate chat states notifications into your application.

Here is a list of common Chat State Notifications:

* **typing**: The user is composing a message. The user is actively interacting with a message input interface specific to this chat session (e.g., by typing in the input area of a chat window)
* **paused**: The user had been composing but now has stopped. The user was composing but has not interacted with the message input interface for a short period of time (e.g., 30 seconds)


With QuickBlox you can use all these chat status notifications.

Here is an example of how to implement the **typing** notification.
```java
QBChatDialog chatDialog = ...;

try {
    chatDialog.sendIsTypingNotification();
} catch (XMPPException | SmackException.NotConnectedException e) {
    e.printStackTrace();
}
```

Also it's possible to send a **stop typing** notification:
```java
try {
    chatDialog.sendStopTypingNotification();
} catch (XMPPException | SmackException.NotConnectedException e) {
    e.printStackTrace();
}
```


On the opponent's side we can track this notification and update the UI:
```java
QBChatDialogTypingListener typingListener = new QBChatDialogTypingListener() {
    @Override
    public void processUserIsTyping(String dialogId, Integer senderId) {

    }

    @Override
    public void processUserStopTyping(String dialogId, Integer senderId) {

    }
};

chatDialog.addIsTypingListener(typingListener);
```

The same also possible for dialogs with type ```QBDialogType.GROUP```.

== System notifications ==

There is a way to send system notifications to other users about some events. These messages work over separated channel and won't be mixed with the regular chat messages:

```java
private QBSystemMessagesManager systemMessagesManager;
private QBSystemMessageListener systemMessageListener;

systemMessagesManager = QBChatService.getInstance().getSystemMessagesManager();
systemMessageListener = new QBSystemMessageListener() {
    @Override
    public void processMessage(QBChatMessage qbChatMessage) {
       
    }

    @Override
    public void processError(QBChatException e, QBChatMessage qbChatMessage) {
                
    }
};

systemMessagesManager.addSystemMessageListener(systemMessageListener);

...

try {
    QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setProperty("param1", "value1");
    chatMessage.setProperty("param2", "value2");

    chatMessage.setRecipientId(18);

    systemMessagesManager.sendSystemMessage(chatMessage);

} catch (SmackException.NotConnectedException e) {

} catch (IllegalStateException ee){

}
```

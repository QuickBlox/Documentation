<span id="Preparing_QBChatDialog_model" class="on_page_navigation"></span>
## Preparing QBChatDialog model

QuickBlox Android SDK uses ```QBChatDialog``` model to provide full chatting functionality: sending/receiving messages, typing statuses etc.

To use ```QBChatDialog``` you should load existing dialogs from server or create a new one. <br>
```QBChatDialog``` is initialized with active chat connection while it is created on server or loaded.<br>
So, **if you are not logged into chat service, you can't send and receive messages via QBChatDialog.**<br>
In order to chat you should do login before using ```QBChatDialog```.
```java
//login to chat firstly
QBChatService.getInstance().login(qbUser);

//Then load or create QBChatDialog on server

QBChatDialog chatDialog = ...;
```

If you make logout at some point in application, your **```QBChatDialog```  will be detached** from chat service and connection will be lost. <br>
If you save ```QBChatDialog``` model and make re-login to chat, you can still use ```QBChatDialog``` without reloading. In this case just initialize ```QBChatDialog``` with active chat service:
```java
...
QBChatService.getInstance().logout();
...

QBChatService.getInstance().login(qbUser);

// init saved chatDialog with active chat service
chatDialog.initForChat(QBChatService.getInstance());
```
<br>

### Specificity of use chatting functionality for group chats
Особенности использования чат функций для групповых диалогов
Before start chatting in a group chat (```QBChatDialog``` with type ```QBDialogType.GROUP```) you should join corresponding dialog.

```java
QBChatDialog groupChatDialog = ...;
QBEntityCallback callback = ...;

DiscussionHistory discussionHistory = new DiscussionHistory();
discussionHistory.setMaxStanzas(0);

groupChatDialog.join(discussionHistory, callback);
```
After join you can send and receive messages.

**You can set ```setAutojoinEnabled(true)``` during configuration (во время конфигурации) ```QBChatService``` to join loaded or created on server dialogs automatically**. 
That case you don't need to join chat dialog manually in code. For more details about configurations ```QBChatService``` see [documentation stub link]()

When you decide to stop using chatting functionality for group dialog, you should perform 'leave' from group dialog.
```java
QBChatDialog groupChatDialog = ...;
groupChatDialog.leave();
```

<span id="Sending_messages" class="on_page_navigation"></span>
## Sending messages

After initialization of ```QBChatDialog``` model with current chat connection you can start sending messages.
To do that you can use next code snippets:
### Send simple message without any additional parameters:
```java
QBChatDialog chatDialog = ...;

chatDialog.sendMessage("Hi Bob!");
```

### Send message with some additional parameters:
```java
QBChatDialog chatDialog = ...;

QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("Hi Bob!");
    chatMessage.setSaveToHistory(true);
    chatMessage.setDateSent(System.currentTimeMillis() / 1000);
    chatMessage.setMarkable(true);
    
chatDialog.sendMessage(chatMessage);
```

### Send message with custom parameters:

There are cases when it is necessary to send custom parameters inside message. You can do it using the following code snippet:
```java
QBChatDialog chatDialog = ...;

QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("Hi Bob!");
    ...
    chatMessage.setProperty("custom_parameter_1", "custom_value_1");
    chatMessage.setProperty("custom_parameter_2", "custom_value_2");
    ...
    
chatDialog.sendMessage(chatMessage);
```

### Send message with Attachment:
QuickBlox supports sending messages with attachments.<br>
It's possible to add any type of attachments to message: image, audio file or video file. We don't have any restrictions here - 
you can attach any type of file.

To send a message with attachments you should use the same way as you send regular text message, but add an attachment object to it. 
Attachment can contain the following:
* A file in Content module: [Android example](http://quickblox.com/developers/SimpleSample-content-android)
* A file in Custom Objects module: [Android example](http://quickblox.com/developers/SimpleSample-customObjects-android#Files)
* Can be an url to any file in Internet

To send a message with attachment you have to upload a file to Content module, Custom Objects module using sample above or use an url to any file in Internet. Then you should incorporate an ID to file to message.

For example, we use Content module to store attachments. Next snippets show how to upload a file to Content module and send it as an attachment: 

```java
File filePhoto = new File("holy_grail.png");
Boolean fileIsPublic = false;
String [] tags = new String[]{"tag_1", "tag_2"};
QBContent.uploadFileTask(filePhoto, fileIsPublic, tags, new QBProgressCallback() {
        @Override
        public void onProgressUpdate(int i) {
            // i - progress in percentages
        }
    }).performAsync(new QBEntityCallback<QBFile>() {
        @Override
        public void onSuccess(QBFile qbFile, Bundle bundle) {
            // create a message
            QBChatMessage chatMessage = new QBChatMessage();
            chatMessage.setSaveToHistory(true); // Save a message to history
        
            // attach a photo
            QBAttachment attachment = new QBAttachment("photo");
            attachment.setId(qbFile.getId().toString());
            chatMessage.addAttachment(attachment);

            // send as common message
            ...
        }

        @Override
        public void onError(QBResponseException e) {
            // error
        }
});
```

<span id="Receiving_incoming_messages" class="on_page_navigation"></span>
## Receiving incoming messages

QuickBlox Android SDK provides two ways of listening of the incoming messages:
* listening messages from all chats using ```QBIncomingMessagesManager```;
* listening messages only for specific chat;
 
For this both ways use same messages listener (В обеих случаях используется один и тот же лисенер)```QBChatDialogMessageListener```.
```java
QBChatDialogMessageListener messagesListener = new QBChatDialogMessageListener() {
    @Override
    public void processMessage(String dialogId, QBChatMessage qbChatMessage, Integer senderId) {
        //process new incoming message there
    }

    @Override
    public void processError(String dialogId, QBChatException e, QBChatMessage qbChatMessage, Integer senderId) {
        //process error there
    }
};
```
 
### Listening messages from all chats
```QBIncomingMessagesManager``` is used to listen for all incoming messages from all dialogs.<br>
>Pay attention! Messages from group chat dialogs will be received in ```QBIncomingMessagesManager``` only after joining to this group chat dialog.

To use ```QBIncomingMessagesManager``` just retrieve it from ```QBChatService``` after **login in chat**:
```java
QBIncomingMessagesManager incomingMessagesManager = chatService.getIncomingMessagesManager();
```

To listen all messages from any chat dialogs (для того, чтобы слушать все месседжи из любого диалога) just register ```QBChatDialogMessageListener```:
```java
QBChatDialogMessageListener messagesListener = ...;

incomingMessagesManager.addMessageListener(messagesListener);
```

### Listening messages from certain chat
For listening from certain chat (для того чтобы слушать месседжи с конкретного диалога) just add ```QBChatDialogMessageListener``` to this ```QBChatDialog```:
```java
QBChatDialog chatDialog = ...;
QBChatDialogMessageListener messagesListener = ...;

chatDialog.addMessageListener(messagesListener);
```

### Receive attachment

For example we use Content module to store attachments. The following snippets allow to receive a message with an attachment and download it:
      
```java
// QBChatDialogMessageListener
      
    ...
      
    @Override
    public void processMessage(String dialogId, QBChatMessage chatMessage, Integer senderId) {
        for (QBAttachment attachment : chatMessage.getAttachments()){
            String fileId = attachment.getId();
      
            // download a file
            QBContent.downloadFile(fileId).performAsync(new QBEntityCallback<InputStream>(){
                @Override
                public void onSuccess(InputStream inputStream, Bundle params) {
                    // process file
                }
      
                @Override
                public void onError(QBResponseException errors) {
                    // errors
                }
            });
        }
    }
      
    ... 
```

### Receive message from newly established private chat
If you receive a message from a participant in a new dialog, you can construct private ```QBChatDialog``` for chatting by dialogId.
```java

//Dialogs cache
Map<String, QBChatDialog> opponentsDialogMap = ...;

incomingMessagesManager.addDialogMessageListener(new QBChatDialogMessageListener() {
            @Override
            public void processMessage(String dialogId, QBChatMessage message, Integer senderId) {
            //if you don't have dialog with this dialogId just construct it:
                if (!opponnetsDialogMap.containsKey(dialogId)) {
                    QBChatDialog opponentDialog = new QBChatDialog();
                    ArrayList<Integer> occupantIds = new ArrayList<>();
                    occupantIds.add(message.getSenderId());
                    opponentDialog.setOccupantsIds(occupantIds);
                    
                    //init Dialog for chatting
                    opponentDialog.initForChat(dialogId, QBDialogType.PRIVATE, chatService);

                    //add message listener on this Dialog (if needed)
                    opponentDialog.addMessageListener(opponentDialogMsgListener);

                    //put Dialog in cache
                    opponnetsDialogMap.put(dialogId, opponentDialog);
                }

            }

            @Override
            public void processError(String dialogId, QBChatException exception, QBChatMessage message, Integer senderId) {

            }
        });
```

<span id="Get_online_users_for_group_dialog" class="on_page_navigation"></span>
## Get online users for group dialog

You can request online users in group chat dialog without joining the chat:
```java
Collection<Integer> onlineUsers = null;

try {
     onlineUsers = chatDialog.requestOnlineUsers();
} catch (XMPPException.XMPPErrorException | SmackException.NotConnectedException | SmackException.NoResponseException e) {

}
```

And you also can handle online users in real time:
```java
private QBChatDialogParticipantListener participantListener;

participantListener = new QBChatDialogParticipantListener() {
    @Override
    public void processPresence(String dialogId, QBPresence qbPresence) {
                
    }
};

private QBChatDialog groupChatDialog = ...;
groupChatDialog.addParticipantListener(participantListener);
```
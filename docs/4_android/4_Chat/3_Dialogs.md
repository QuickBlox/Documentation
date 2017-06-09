1. Creating dialogs

* Creating dialogs example:
```java
ArrayList<Integer> occupantIdsList = new ArrayList<Integer>();
occupantIdsList.add(34);
occupantIdsList.add(17);
 
QBChatDialog dialog = new QBChatDialog();
dialog.setName("Chat with Garry and John");
dialog.setPhoto("1786");
dialog.setType(QBDialogType.GROUP);
dialog.setOccupantsIds(occupantIdsList);

//or just use DialogUtils
//for creating PRIVATE dialog
//QBChatDialog dialog = DialogUtils.buildPrivateDialog(recipientId);

//for creating GROUP dialog
//QBChatDialog dialog = DialogUtils.buildDialog("Chat with Garry and John", QBDialogType.GROUP, occupantIdsList);

QBRestChatService.createChatDialog(dialog).performAsync(new QBEntityCallback<QBChatDialog>() {
    @Override
    public void onSuccess(QBChatDialog result, Bundle params) {

    }

    @Override
    public void onError(QBResponseException responseException) {

    }
});
```

To notify occupants you've created chat dialog you can use ```QBSystemMessagesManager```:
```java
public static final String PROPERTY_OCCUPANTS_IDS = "occupants_ids";
public static final String PROPERTY_DIALOG_TYPE = "dialog_type";
public static final String PROPERTY_DIALOG_NAME = "dialog_name";
public static final String PROPERTY_NOTIFICATION_TYPE = "notification_type";

private QBChatMessage buildSystemMessageAboutCreatingGroupDialog(QBChatDialog dialog){
    QBChatMessage qbChatMessage = new QBChatMessage();
    qbChatMessage.setDialogId(dialog.getDialogId());
    qbChatMessage.setProperty(PROPERTY_OCCUPANTS_IDS, QbDialogUtils.getOccupantsIdsStringFromList(dialog.getOccupants()));
    qbChatMessage.setProperty(PROPERTY_DIALOG_TYPE, String.valueOf(dialog.getType().getCode()));
    qbChatMessage.setProperty(PROPERTY_DIALOG_NAME, String.valueOf(dialog.getName()));
    qbChatMessage.setProperty(PROPERTY_NOTIFICATION_TYPE, CREATING_DIALOG);

    return qbChatMessage;
}

//Let's notify occupants
public void sendSystemMessageAboutCreatingDialog(QBSystemMessagesManager systemMessagesManager, QBChatDialog dialog) {
    QBChatMessage systemMessageCreatingDialog = buildSystemMessageAboutCreatingGroupDialog(dialog);

    for (Integer recipientId : dialog.getOccupants()) {
        if (!recipientId.equals(QBChatService.getInstance().getUser().getId())) {
            systemMessageCreatingDialog.setRecipientId(recipientId);
            systemMessagesManager.sendSystemMessage(systemMessageCreatingDialog);
        }
    }
}
```

* Create a Public group chat dialog

It's also possible to create a public group chat, so any user from you application can join it. There is no a list with occupants, this chat is just open for everybody.

To create a public group chat use the same logic as for group chat, but change the dialog's **type** to ```QBDialogType.PUBLIC_GROUP``` and don't pass the **occupantsIds** value.
<br/>

* Set dialog's avatar<br><br>

The Chat dialog contains a field **photo**. It's a string field, can contain any value:
   * An ID of a file in Content module: [Android example](http://quickblox.com/developers/SimpleSample-content-android)
   * An ID of a file in Custom Objects module: [Android example](http://quickblox.com/developers/SimpleSample-customObjects-android#Files)
   * Can be an url to any file in Internet

You can set avatar for dialog with **type = 1** (```QBDialogType.PUBLIC_GROUP```) or **type = 2** (```QBDialogType.GROUP```). 
<br>

For example, we use Content module to store the dialog's photo. Next snippets show how to upload a file to Content module and set it as a photo of a dialog: 

```java
File filePhoto = new File("dialog_avatar.png");
Boolean fileIsPublic = false;
QBContent.uploadFileTask(filePhoto, fileIsPublic, null).performAsync(new QBEntityCallback<QBFile>() {
    @Override
    public void onSuccess(QBFile qbFile, Bundle bundle) {
        dialog.setPhoto(file.getId().toString);
    }

    @Override
    public void onError(QBResponseException e) {
        // error
    }
});
```


* Set custom parameters

Dialogs can store additional parameters. These parameters can be used to store an additional data. Also these parameters can be used in dialogs retrieval requests.

To start use additional parameters it needs to create an additional schema of your parameters. This is a [CustomObjects class](http://quickblox.com/developers/Custom_Objects#Create_data_schema). Just create an empty class with all needed fields. These fields will be your dialog additional parameters.

When you create a dialog then set a dialog's field **customData**:
```java
QBDialogCustomData data = new QBDialogCustomData("Advert"); // class name
data.putString("title", "Magic Beans"); // field 'title'
data.putInteger("advert_id", 5665); // field 'advert_id'
newDialog.setCustomData(data);
```

It's also possible to use custom parameters in a dialogs retrieval requests so dialogs can be filtered through custom parameters:

```java
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.addRule("data[class_name]", QueryRule.EQ, "Advert");
requestBuilder.addRule("data[title]", QueryRule.EQ, "bingo");
```

2. Retrieve dialogs

Requesting Dialogs: 
```java
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.setLimit(100);

QBRestChatService.getChatDialogs(null, requestBuilder).performAsync(new QBEntityCallback<ArrayList<QBChatDialog>>() {
            @Override
            public void onSuccess(ArrayList<QBChatDialog> result, Bundle params) {
                 int totalEntries = params.getInt("total_entries");
            }

            @Override
            public void onError(QBResponseException responseException) {

            }
        });
        
```

* Filters

There are some filters to get only chat dialogs you need, not just all.


You can apply filters for the following fields:

    * _id (string)
    * type (integer)
    * name (string)
    * last_message_date_sent (integer)
    * created_at (date)
    * updated_at (date)

You can apply **sort** for the following fields:

    * last_message_date_sent (long)

 
Filters:

Operator | Description |Usage example
---------|-------------|--------------
**{field_name}**|Search records with field which contains exactly specified value|```requestBuilder.addRule("type", QueryRule.EQ, "2");```
**{field_name}[{search_operator}]**| Search record with field which contains value according to specified value and operator.<br><br>Possible filters: **lt** (**L**ess **T**han operator), **lte** (**L**ess **T**han or **E**qual to operator), **gt (**G**reater **T**han operator), **gte** (**G**reater **T**han or **E**qual to operator), **ne** (**N**ot **E**qual to operator), **in** (Contained **IN** array operator), **nin** (**N**ot contained **IN** array), **all** (**ALL** contained **IN** array), **ctn** (Contains substring operator) | ```requestBuilder.in("type", "1", "2");```
**limit** | Limit search results to **N** records. Useful for pagination. Default value - 100 | ```requestBuilder.setLimit(100);```
**skip** | Skip **N** records in search results. Useful for pagination. Default (if not specified): 0 | ```requestBuilder.setSkip(50);```
**sort_desc/sort_asc** | Search results will be sorted by specified field in ascending/descending order | ```requestBuilder.sortAsc("last_message_date_sent");```

To use filters, you should build a ```QBRequestGetBuilder``` request and pass it to 'get dialogs' request above:

```java
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.setLimit(100);
requestBuilder.setSkip(5);
requestBuilder.sortAsc("last_message_date_sent");
requestBuilder.gt("updated_at", "1455098137");
```


Also possible to use custom parameters in a dialogs retrieval requests so dialogs can be filtered through custom parameters:
```java
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.addRule("data[class_name]", QueryRule.EQ, "Advert");
requestBuilder.addRule("data[title]", QueryRule.EQ, "bingo");
```


You can request single dialog using code: 
```java
String dialogId = ...;

QBRestChatService.getChatDialogById(dialogId).performAsync(
new QBEntityCallback<QBChatDialog>() {
            @Override
            public void onSuccess(QBChatDialog dialog, Bundle params) {
                 
            }

            @Override
            public void onError(QBResponseException responseException) {

            }
        });
        
```

== Chatting in dialog==

=== QBChatDialog model===

'''QBChatDialog''' model is responsible for full chatting functionality : sending/receiving messages, typing statuses etc.

To use '''QBChatDialog''' you should load existed dialogs from server or create a new one on server. <br>
'''QBChatDialog''' is initialized with active chat connection when it is created on server or loaded.<br>
So, '''if you not logged into chat service you can't send and receive messages via QBChatDialog'''.<br>
In order to chatting you should make login before using '''QBChatDialog'''.
<syntaxhighlight lang="java">
//login to chat firstly
QBChatService.getInstance().login(qbUser);

//Then load or create QBChatDialog on server

QBChatDialog chatDialog = ...;
</syntaxhighlight>

If you make logout at some point in application your '''QBChatDialog  detaches''' from chat service and lose connection. <br>
If you save '''QBChatDialog''' model and make re-login to chat, you can still use '''QBChatDialog''' without reloading. In this case just initialize '''QBChatDialog''' with active chat service:
<syntaxhighlight lang="java">
...
QBChatService.getInstance().logout();
...
QBChatService.getInstance().login(qbUser);
// init saved chatDialog with active chat service
chatDialog.initForChat(QBChatService.getInstance());
</syntaxhighlight>

==== Passing QBChatDialog inside app====

Staring from sdk '''3.3.0'''
'''QBChatDialog''' can be passed between Activities or Fragments components via Intents.

<syntaxhighlight lang="java">

QBChatDialog dialog = ...;
Intent intent = new Intent(activity, ChatActivity.class);
intent.putExtra(EXTRA_DIALOG_ID, dialog);
activity.startActivity(intent);
</syntaxhighlight>

To make received dialog able to chatting after receiving just attach it to active chat service :
<syntaxhighlight lang="java">

public class ChatActivity extends BaseActivity {
...

    @Override
    public void onCreate(Bundle savedInstanceState) {
    ...
    qbChatDialog = (QBChatDialog) getIntent().getSerializableExtra(EXTRA_DIALOG_ID);
    qbChatDialog.initForChat(QBChatService.getInstance());
    ...
   }
}
</syntaxhighlight>
<br>

=== Receiving incoming messages===
In SDK version 3.0 was inroduced '''QBIncomingMessagesManager''' to listen for all incoming messages from all dialogs. '''Pay attention, messages from group chat dialogs will be received in QBIncomingMessagesManager only after joining to this group chat dialog.'''

For using '''QBIncomingMessagesManager''' just retrieve it from '''QBChatService''' after '''login in chat''':
<syntaxhighlight lang="java">
QBIncomingMessagesManager incomingMessagesManager = chatService.getIncomingMessagesManager();
</syntaxhighlight>


To listen for messages from any chat dialogs just register '''QBChatDialogMessageListener''':
<syntaxhighlight lang="java">
incomingMessagesManager.addMessageListener(
new QBChatDialogMessageListener() {
            @Override
            public void processMessage(String dialogId, QBChatMessage message, Integer senderId) {
               
            }

            @Override
            public void processError(String dialogId, QBChatException exception, QBChatMessage message, Integer senderId) {

            }
        });
</syntaxhighlight>


=== Chat in private dialog ===
To send a message in 1-1 chat use the '''QBChatDialog''' instance:
<syntaxhighlight lang="java">
try {
    QBChatDialog privateDialog = ...;
    QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("Hi there!");
    privateDialog.sendMessage(chatMessage);
} catch (SmackException.NotConnectedException e) {

}

// Add message listener for that particular chat dialog

privateDialog.addMessageListener(new QBChatDialogMessageListener() {
    @Override
    public void processMessage(String dialogId, QBChatMessage message, Integer senderId) {

    }

    @Override
    public void processError(String dialogId, QBChatException exception, QBChatMessage message, Integer senderId) {

    }
});
</syntaxhighlight>

'''If you receive message from participant from Dialog, you haven't had before, you can construct Dialog for chatting by dialogId.'''
<syntaxhighlight lang="java">

//Dialogs cache
Map<String, QBChatDialog> opponentsDialogMap = ...;

incomingMessagesManager.addDialogMessageListener(
new QBChatDialogMessageListener() {
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

                    //add message listener on this Dialog
                    opponentDialog.addMessageListener(opponentDialogMsgListener);

                    //put Dialog in cache
                    opponnetsDialogMap.put(dialogId, opponentDialog);
                }

            }

            @Override
            public void processError(String dialogId, QBChatException exception, QBChatMessage message, Integer senderId) {

            }
        });
</syntaxhighlight>

=== Chat in group dialog ===

Before start chatting in a group dialog you should join this dialog.

<syntaxhighlight lang="java">
QBChatDialog groupChatDialog = ...;

DiscussionHistory discussionHistory = new DiscussionHistory();
discussionHistory.setMaxStanzas(0);

groupChatDialog.join(discussionHistory, new QBEntityCallback() {
    @Override
    public void onSuccess(Object o, Bundle bundle) {
                
    }

    @Override
    public void onError(QBResponseException e) {

    }
});
</syntaxhighlight>

'''You can set ''autojoin'' settings in QBChatService to automatically join loaded or created on server dialogs:'''. In that way you don't need to join chat dialog manually in code.

<syntaxhighlight lang="java">
//Set it before login in chat
QBChatService.ConfigurationBuilder builder = new QBChatService.ConfigurationBuilder();
builder.setAutojoinEnabled(true);
QBChatService.setConfigurationBuilder(builder);
</syntaxhighlight>

After join you can send and receive messages:

<syntaxhighlight lang="java">
try {
    QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("Hi there!");
    groupChatDialog.sendMessage(chatMessage);
} catch (SmackException.NotConnectedException e) {

}

// Add message listener for that particular group chat dialog

groupChatDialog.addMessageListener(new QBChatDialogMessageListener() {
    @Override
    public void processMessage(String dialogId, QBChatMessage message, Integer senderId) {

    }

    @Override
    public void processError(String dialogId, QBChatException exception, QBChatMessage message, Integer senderId) {

    }
});
</syntaxhighlight>

==== Get online users ====
You can request online users in group chat dialog without joining chat:
<syntaxhighlight lang="java">
Collection<Integer> onlineUsers = null;

try {
     onlineUsers = chatDialog.requestOnlineUsers();
} catch (XMPPException.XMPPErrorException | SmackException.NotConnectedException | SmackException.NoResponseException e) {

}
</syntaxhighlight>

'''Method chatDialog.getOnlineUsers is deprecated.'''

And you also can handle online users in real time:
<syntaxhighlight lang="java">
private QBChatDialogParticipantListener participantListener;

participantListener = new QBChatDialogParticipantListener() {
    @Override
    public void processPresence(String dialogId, QBPresence qbPresence) {
                
    }
};

private QBChatDialog groupChatDialog = ...;
groupChatDialog.addParticipantListener(participantListener);
</syntaxhighlight>

==== Leave group chat dialog ====
<syntaxhighlight lang="java">
try {
    groupChatDialog.leave();
    groupChatDialog = null;
} catch (XMPPException | SmackException.NotConnectedException e) {
    
}
</syntaxhighlight>


=== Send and receive a message with attachment ===
==== Send attachment ====
It's possible to add attachments to message: for example, image, audio file or video file. We don't have any restrictions here - you can attach any type of file.

To send a message with attachments you should use the same way as you send regular message with text, but add to it an attachment object. Attachment can be:
* A file in Content module: [http://quickblox.com/developers/SimpleSample-content-android Android example]
* A file in Custom Objects module: [http://quickblox.com/developers/SimpleSample-customObjects-android#Files Android example]
* Can be an url to any file in Internet
<br>

To send a message with attachment you have to upload a file to Content module, Custom Objects module using sample above or use an url to any file in Internet. Then you should incorporate an ID to file to message.

For example, we use Content module to store attachments. Next snippets show how to upload a file to Content module and send it as an attach: 

<syntaxhighlight lang="java">
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
        attachment.setId(file.getId().toString());
        chatMessage.addAttachment(attachment);

        // send a message
        // ...
    }

    @Override
    public void onError(QBResponseException e) {
        // error
    }
});
</syntaxhighlight>

==== Receive attachment ====
For example we use Content module to store attachments. Next snippets allow to receive a message with an attachment and download it:

<syntaxhighlight lang="java">
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
   
</syntaxhighlight>


===Use custom parameters in a message===
You can use custom parameters for the messages you send in the chat, for example to send some additional info or to send control messages:

<syntaxhighlight lang="java">
QBChatMessage chatMessage = new QBChatMessage();
chatMessage.setBody("Hi there");
                
chatMessage.setProperty("name", "Bob");
chatMessage.setProperty("age", "25");
</syntaxhighlight>

===Unread messages count===

You can request total unread messages count or unread count for particular dialog:

<syntaxhighlight lang="java">
Set<String> dialogsIds = new HashSet<String>();
    dialogsIds.add("56f3fac3a0eb4786ae00003f");
    dialogsIds.add("56f3f546a28f9affc0000033");
        
QBRestChatService.getTotalUnreadMessagesCount(dialogsIds).performAsync(new QBEntityCallback<Integer>() {
    @Override
    public void onSuccess(Integer total, Bundle params) {
        Log.i(TAG, "total unread messages: " + total);
        Log.i(TAG, "dialog Unread1: " + params.getInt("56f3fac3a0eb4786ae00003f"));
        Log.i(TAG, "dialog Unread2: " + params.getInt("56f3f546a28f9affc0000033"));
    }

    @Override
    public void onError(QBResponseException e) {

    }
});
</syntaxhighlight>

== Update group dialog ==
User can update group chat dialog name, add new occupants or leave this group chat. Use '''QBDialogRequestBuilder''' to add more occupants or to leave group chat (remove yourself) or to remove other users. '''But only dialog's creator(owner) can remove any users from occupants_ids.''' Refer to http://quickblox.com/developers/Chat#Update_dialog

[[File:Chat2_update_dialog_android.png]]


<syntaxhighlight lang="java">
QBDialogRequestBuilder requestBuilder = new QBDialogRequestBuilder();
requestBuilder.addUsers(378); // add another users
// requestBuilder.removeUsers(22); // Remove yourself (user with ID 22)

QBChatDialog chatDialog = ...; // dialog should contain dialogId
chatDialog.setName("Team room");

QBRestChatService.updateGroupChatDialog(chatDialog, requestBuilder).performAsync(new QBEntityCallback<QBChatDialog>() {
            @Override
            public void onSuccess(QBChatDialog qbChatDialog, Bundle bundle) {
                
            }

            @Override
            public void onError(QBResponseException e) {

            }
        });
</syntaxhighlight>

To notify all occupants that you updated a group chat we use chat notifications - it's simple chat message with extra parameters inside. These parameters used to separate chat notifications from regular text chat messages:

<syntaxhighlight lang="java">
public static QBChatMessage createChatNotificationForGroupChatUpdate(QBChatDialog chatDialog) {
    String dialogId = String.valueOf(chatDialog.getDialogId());
    String roomJid = chatDialog.getRoomJid();
    String occupantsIds = TextUtils.join(",", chatDialog.getOccupants());
    String dialogName = chatDialog.getName();
    String dialogTypeCode = String.valueOf(chatDialog.getType().ordinal());
 
    QBChatMessage chatMessage = new QBChatMessage();
    chatMessage.setBody("optional text");
 
    // Add notification_type=2 to extra params when you updated a group chat 
    //
    chatMessage.setProperty("notification_type", "2");
 
    chatMessage.setProperty("_id", dialogId);
    if (!TextUtils.isEmpty(roomJid)) {
        chatMessage.setProperty("room_jid", roomJid);
    }
    chatMessage.setProperty("occupants_ids", occupantsIds);
    if (!TextUtils.isEmpty(dialogName)) {
        chatMessage.setProperty("name", dialogName);
    }
    chatMessage.setProperty("type", dialogTypeCode);
 
    return chatMessage;
}
 
...
 
QBSystemMessagesManager systemMessagesManager = QBChatService.getInstance().getSystemMessagesManager();

for (Integer userID : chatDialog.getOccupants()) {
 
    QBChatMessage chatMessage = createChatNotificationForGroupChatUpdate(chatDialog);
 
    long time = DateUtils.getCurrentTime();
    chatMessage.setProperty("date_sent", time + "");

    chatMessage.setRecipientId(userID);

    try {
        systemMessagesManager.sendSystemMessage(chatMessage);
    } catch (SmackException.NotConnectedException e) {
 
  
}

...

QBSystemMessageListener systemMessageListener = new QBSystemMessageListener() {
    @Override
    public void processMessage(QBChatMessage qbChatMessage) {
 
    }
 
    @Override
    public void processError(QBChatException e, QBChatMessage qbChatMessage) {
 
    }
};
systemMessagesManager.addSystemMessageListener(systemMessageListener);
</syntaxhighlight>

== Delete dialogs ==

To delete a dialog use next snippet:

<syntaxhighlight lang="java">
QBRestChatService.deleteDialog(dialogId, forceDelete).performAsync(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void aVoid, Bundle bundle) {
        
    }

    @Override
    public void onError(QBResponseException e) {

    }
});
</syntaxhighlight>

This request will remove this dialog for current user, but other users still will be able to chat there.
'''To completely remove a dialog use parameter ''forceDelete''.''' Refer to http://quickblox.com/developers/Chat#Delete_dialog

To delete many dialogs use next :
<syntaxhighlight lang="java">
StringifyArrayList<String> dialogIds = new StringifyArrayList<>();
dialogIds.add(dialogId);
QBRestChatService.deleteDialogs(dialogIds, forceDelete).performAsync(new QBEntityCallback<ArrayList<String>>() {
    @Override
    public void onSuccess(ArrayList<String> strings, Bundle bundle) {
                
    }

    @Override
    public void onError(QBResponseException e) {

    }
});
</syntaxhighlight>

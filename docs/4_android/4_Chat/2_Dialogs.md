<span id="Dialogs" class="on_page_navigation"></span>
# Dialogs

<span id="Creating_dialogs" class="on_page_navigation"></span>
## Creating dialogs

<ul class="nav nav-tabs">
  <li role="presentation" class="active"><a href="#">Create Group or PRIVATE dialog</a>
  
  
  ### Example of creating group and private dialogs::
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
  
  To notify occupants that you've created chat dialog you can use ```QBSystemMessagesManager```:
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
  
  </li>
  <li role="presentation"><a href="#">Create a Public group chat dialog</a></li>
  
  #### Create a Public group chat dialog
  
  It's also possible to create a public group chat, so that any user from you application can join it. There is no list with occupants, 
  this chat is just open for everybody.
  
  To create a public group chat use the same logic as for group chat, but change the dialog's **type** 
  to ```QBDialogType.PUBLIC_GROUP``` and do not pass the **occupantsIds** value.
  <br><br>
  
  <li role="presentation"><a href="#">Messages</a></li>
</ul>

### Example of creating group and private dialogs::
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

To notify occupants that you've created chat dialog you can use ```QBSystemMessagesManager```:
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


#### Create a Public group chat dialog

It's also possible to create a public group chat, so that any user from you application can join it. There is no list with occupants, 
this chat is just open for everybody.

To create a public group chat use the same logic as for group chat, but change the dialog's **type** 
to ```QBDialogType.PUBLIC_GROUP``` and do not pass the **occupantsIds** value.
<br><br>

#### Set dialog's avatar<br>

The Chat dialog contains a field **photo**. It's a string field, which can contain any value:
* An ID of a file in Content module: [Android example](http://quickblox.com/developers/SimpleSample-content-android)
* An ID of a file in Custom Objects module: [Android example](http://quickblox.com/developers/SimpleSample-customObjects-android#Files)
* Can be an url to any file in Internet

You can set avatar for dialog with **type = 1** (```QBDialogType.PUBLIC_GROUP```) or **type = 2** (```QBDialogType.GROUP```). 
<br>

For example, we use Content module to store the dialog's photo. Next snippets show how to upload a file to the Content module and 
set it as a photo of a dialog: 

```java
QBChatDialog groupChatDialog = ...;

File filePhoto = new File("dialog_avatar.png");
Boolean fileIsPublic = false;
QBContent.uploadFileTask(filePhoto, fileIsPublic, null).performAsync(new QBEntityCallback<QBFile>() {
    @Override
    public void onSuccess(QBFile qbFile, Bundle bundle) {
        groupChatDialog.setPhoto(qbFile.getId().toString);
    }

    @Override
    public void onError(QBResponseException e) {
        // error
    }
});
```


#### Set custom parameters

Dialogs can store some additional parameters. These parameters can be used to store an additional data. Also these parameters 
can be used in dialogs retrieval requests.

To start use additional parameters you need to create an additional schema of your parameters. 
This is a [CustomObjects class](http://quickblox.com/developers/Custom_Objects#Create_data_schema). 
Just create an empty class with all needed fields. These fields will be additional parameters of your dialog.

When you create a dialog set a dialog's field ```customData```:
```java
QBDialogCustomData data = new QBDialogCustomData("Advert"); // class name
data.putString("title", "Magic Beans"); // field 'title'
data.putInteger("advert_id", 5665); // field 'advert_id'
newDialog.setCustomData(data);
```

It's also possible to use custom parameters in a dialog's retrieval requests to filter them through custom parameters:

```java
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.addRule("data[class_name]", QueryRule.EQ, "Advert");
requestBuilder.addRule("data[title]", QueryRule.EQ, "bingo");
```

<span id="Retrieve_dialogs" class="on_page_navigation"></span>
## Retrieve dialogs

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

#### Filters

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


Also it is possible to use custom parameters in a dialog's retrieval requests to filter them through custom parameters:
```java
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.addRule("data[class_name]", QueryRule.EQ, "Advert");
requestBuilder.addRule("data[title]", QueryRule.EQ, "bingo");
```


You can request single dialog following the code: 
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

<span id="Passing_QBChatDialog_inside_app" class="on_page_navigation"></span>
## Passing QBChatDialog inside app

```QBChatDialog``` can be passed between Activities or Fragments components via Intents.

```java
QBChatDialog chatDialog = ...;
Intent intent = new Intent(activity, ChatActivity.class);
intent.putExtra(EXTRA_DIALOG, dialog);
activity.startActivity(intent);
```

To make received dialog available chatting after receiving just attach it to active chat service :
```java
public class ChatActivity extends BaseActivity {
    ...

    @Override
    public void onCreate(Bundle savedInstanceState) {
    ...
    qbChatDialog = (QBChatDialog) getIntent().getSerializableExtra(EXTRA_DIALOG);
    qbChatDialog.initForChat(QBChatService.getInstance());
    ...
   }
}
```


<span id="Getting_unread_messages_count_for_dialogs" class="on_page_navigation"></span>
## Getting unread messages count for dialogs

You can request total unread messages count or unread messages count for particular dialog:

```java
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
```

<span id="Updating_group_dialog" class="on_page_navigation"></span>
## Updating group dialog

Any user from ```occupants_ids``` can update dialog with type ```QBDialogType.GROUP```. The following fields could be updated: 
```occupants_ids```(add new occupants or leave this group chat), ```name```, ```photo```. Use ```QBDialogRequestBuilder``` to add more 
occupants or to leave group chat (remove yourself) or to remove other users.
**But only dialog's creator(owner) can remove any users from ```occupants_ids```.** 

**Only dialog's owner can update dialog with type ```QBDialogType.PUBLIC_GROUP```** 

```java
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
```

For details about updating via REST API refer to http://quickblox.com/developers/Chat#Update_dialog

To notify all occupants that you updated a group chat we use chat notifications - it's simple chat message with extra parameters inside it. 
These parameters are used to separate chat notifications from regular text chat messages:
```java
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
```

<span id="Deleting_dialogs" class="on_page_navigation"></span>
## Deleting dialogs

### To delete a dialog use the following snippet:

```java
boolean forceDelete = false;

QBRestChatService.deleteDialog(dialogId, forceDelete).performAsync(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void aVoid, Bundle bundle) {
        
    }

    @Override
    public void onError(QBResponseException e) {

    }
});
```

This request will remove the dialog for current user, but other users still will be able to chat there.
To remove the dialog completely use parameter ```forceDelete = true```. Refer to http://quickblox.com/developers/Chat#Delete_dialog

### To delete many dialogs use next:
```java
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
```

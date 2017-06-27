<span id="Contact_list" class="on_page_navigation"></span>
# Contact list

In QuickBlox Android SDK contact list functionality is implemented via ```QBRoster```. Using this model you can add users to contact list, accept or 
reject request and perform other typical actions with contacts. 

To get ```QBRoster``` just call:
```java
QBRoster chatRoster = QBChatService.getInstance().getRoster();
```

## Contact list mode
Let's imagine the following situation: user A sends a request to become "friends" with user B. User B accepts the friend request. And now user A and 
B appear in the roster of each other.

<br>
Alternatively, you can use "twitter like" logic. User A sends a presence subscription request to user B. Think about this more like following on twitter. 
User A requests permission to receive presence information from user B (following his presence). User B can accept this request, but may not be interested 
in receiving presence info from user A. So when user B accepts the request, this *only* means that user A will receive presence from user B. It does *not* mean that user B will receive presence 
from user A. In diagram form, presence is flowing like this:
<br>
  A <-- B
<br>
Now, if B also wants to receive presence from user A, then user B should this permission. And furthermore, user A should accept the request. 
Just because B has granted A permission to receive presence, doesn't mean that B gets a free pass to receive presence from A.

By default this SDK provides **"twitter like"** mode.

To enable **"Facebook like"** logic pass ```QBRoster.SubscriptionMode.mutual``` value when you are obtaining a roster:
```java
QBRoster chatRoster = QBChatService.getInstance().getRoster(QBRoster.SubscriptionMode.mutual, subscriptionListener);
```

## Access to the Contact list

### Main setup

To access contact list you need to obtain it and set all needed listeners:
* ```QBRosterListener``` is a listener called any time a roster is changed or the presence of a user in the roster is changed (user becomes online/offline)

* ```QBSubscriptionListener``` is a listener called on "subscribe" (add to contact list) request from any user.

Setting listeners to ```QBRoster```:
```java
QBRosterListener rosterListener = new QBRosterListener() {
    @Override
    public void entriesDeleted(Collection<Integer> userIds) {

    }

    @Override
    public void entriesAdded(Collection<Integer> userIds) {

    }

    @Override
    public void entriesUpdated(Collection<Integer> userIds) {

    }

    @Override
    public void presenceChanged(QBPresence presence) {

    }
};

QBSubscriptionListener subscriptionListener = new QBSubscriptionListener() {
    @Override
    public void subscriptionRequested(int userId) {

    }
};


// Do this after success Chat login
QBRoster chatRoster = QBChatService.getInstance().getRoster(QBRoster.SubscriptionMode.mutual, subscriptionListener);
chatRoster.addRosterListener(rosterListener);
```

### Access contact list users

To get users from contact list:
```java
Collection<QBRosterEntry> entries = сhatRoster.getEntries();
```

```QBRosterEntry``` describes a user entity in your contact list. To get user's ID use ```getUserId()``` getter.

To check user in contact list:
```java
int userID = 45;

QBPresence presence = chatRoster.getPresence(userID);

if (presence == null) {
    // No user in your roster
    return;
}
```

To request user's status (online/offline):
```java
QBPresence presence = ...;
if (presence.getType() == QBPresence.Type.online) {
    // User is online
}else{
    // User is offline
}
```

## Add/Remove users

To add users to the contact list just use the method below:
```java
int userID = 56;

if (chatRoster.contains(userID)) {
    ...
    chatRoster.subscribe(userID);
    ...
} else {
    ...
    chatRoster.createEntry(userID, null);
    ...
}
```
After that user with **id = 56** will receive the request to be added to the contact list
```java
// QBSubscriptionListener
 
...

@Override
public void subscriptionRequested(int userId) {

}
```

To confirm the request:
```java
QBRoster chatRoster = ...;
chatRoster.confirmSubscription(userID);
```

To reject the request:
```java
QBRoster chatRoster = ...;
chatRoster.reject(userID);
```

To remove a previously added user from the contact list:
```java
int userID = 67;

QBRoster chatRoster = ...;

chatRoster.unsubscribe(userID);
```

## Custom status

A client MAY provide detailed status information to his contacts by constructing a presence object by himself:
```java
QBPresence presence = new QBPresence(QBPresence.Type.online, "I'm at home", 1, QBPresence.Mode.available);

chatRoster.sendPresence(presence);
```

This case there is **no need to use** ```QBChatService.getInstance().startAutoSendPresence(60)```, you have to manage it by yourself. 
<br>
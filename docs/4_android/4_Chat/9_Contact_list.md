==Contact list==

===Contact list mode===

In traditional IM applications, the "buddy" system is rather straightforward.
User A sends a request to become "friends" with user B. User B accepts the friend request. And now user A and B appear in each other's roster.

<br>
But, alternatively, you can use twitter like logic. User A sends a presence subscription request to user B. Think about this more like following on twitter. User A is requesting permission to receive presence information from user B (following their presence). User B can accept this request, but may not be interested in receiving presence info from user B. Again, this is more like twitter following, but with the addition of required permission. So when user B accepts the request, this *only* means that user A will receive presence from user B. It does *not* mean that user B will receive presence from user A. Again, twitter-style, user A is now "following" user B. In diagram form, presence is flowing like this:
<br>
  A <-- B
<br>
Now, if B also wants to receive presence from user A, then user B must request this permission. And furthermore, user A must accept the request. Just because B has granted A permission to receive presence, doesn't mean that B gets a free pass to receive presence from A.


By default this SDK provides **twitter like** mode.

To enable Facebook like logic pass ```QBRoster.SubscriptionMode.mutual``` value when you are obtaining a roster:
```java
QBRoster chatRoster = QBChatService.getInstance().getRoster(QBRoster.SubscriptionMode.mutual, subscriptionListener);
```

===Access to the Contact list===

==== Main setup ====

To access contact list you have to obtain it and set all needed listeners:
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

```QBRosterListener``` is a listener that is fired any time a roster is changed or the presence of a user in the roster is changed (user becomes online/offline)

```QBSubscriptionListener``` is a listener that is fired on "subscribe" (add to contact list) request from any user.

==== Access contact list users ====

To get users from contact list:
```java
Collection<QBRosterEntry> entries = сhatRoster.getEntries();
```

```QBRosterEntry``` describes a user entity in your contact list. To get user's ID use ```getUserId()``` getter.

To request user's status (online/offline):
```java
int userID = 45;

QBPresence presence = chatRoster.getPresence(userID);
if (presence == null) {
    // No user in your roster
    return;
}

if (presence.getType() == QBPresence.Type.online) {
    // User is online
}else{
    // User is offline
}
```

===Add/Remove users===

To add users to the contact list just use the method below:
```java
int userID = 56;

if (chatRoster.contains(userID)) {
    try {
        chatRoster.subscribe(userID);
    } catch (SmackException.NotConnectedException e) {
                    
    }
} else {
    try {
        chatRoster.createEntry(userID, null);
    } catch (XMPPException e) {

    } catch (SmackException.NotLoggedInException e) {

    } catch (SmackException.NotConnectedException e) {

    } catch (SmackException.NoResponseException e) {

    }
}
```

This user will receive the request to be added to the contact list
```java
// QBSubscriptionListener
 
...

@Override
public void subscriptionRequested(int userId) {

}
```

To confirm the request:
```java
try {
    chatRoster.confirmSubscription(userID);
} catch (SmackException.NotConnectedException e) {

} catch (SmackException.NotLoggedInException e) {

} catch (XMPPException e) {

} catch (SmackException.NoResponseException e) {

}
```

To reject the request:
```java
try {
    chatRoster.reject(userID);
} catch (SmackException.NotConnectedException e) {

}
```

To remove a previously added user from the contact list:
```java
int userID = 67;

try {
    chatRoster.unsubscribe(userID);
} catch (SmackException.NotConnectedException e) {

}
```

===Custom status===

A client MAY provide detailed status information to his contacts by constructing a presence object by himself:
```java
QBPresence presence = new QBPresence(QBPresence.Type.online, "I'm at home", 1, QBPresence.Mode.available);
try {
    chatRoster.sendPresence(presence);
} catch (SmackException.NotConnectedException e) {

}
```

In this case there is no need to use ```QBChatService.getInstance().startAutoSendPresence(60)```, you have to manage it by yourself. 
<br>
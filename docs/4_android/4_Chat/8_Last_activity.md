<span id="Last_activity" class="on_page_navigation"></span>
## Last activity

It is often helpful to know the time of the last activity of user. There is no need to add the user to contact list to get information about last user's activity.
 You can use methods below to discover when a disconnected user accessed the server last time.

```java
QBUser targetUser = ...;
try {
    long lastUserActivity = QBChatService.getInstance().getLastUserActivity(targetUser.getId()); //returns last activity in seconds or 0 if user online or error (e.g. user never loggedin chat)
} catch (XMPPException.XMPPErrorException | SmackException.NotConnectedException e) {
     //handle error
}
```
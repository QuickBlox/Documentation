==Ping manager==
The Ping manager allows a user to ping the backend by simply sending a ping to it. This is useful in a case to check a connection between user and server. 

```java
final QBPingManager pingManager = QBChatService.getInstance().getPingManager();

pingManager.pingServer(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess() {

    }

    @Override
    public void onError(QBResponseException error) {

    }
});

// or sync version
final QBPingManager pingManager = QBChatService.getInstance().getPingManager();

try {
    boolean pingSuccess = pingManager.pingServer();
} catch (SmackException.NotConnectedException e) {
    e.printStackTrace();
}
```

PingManger also periodically sends XMPP pings to the server every 30 minutes to avoid NAT timeouts and to test the connection status. To configure the ping interval and to listen for fails: 

```java
QBChatService.getInstance().setPingInterval(30);

pingManager.addPingFailedListener(new PingFailedListener(){
    public void pingFailed(){
    
    }
});
```

===Ping a user===

It's also possible to ping some user to check whethere he is online or not:
```java
final QBPingManager pingManager = QBChatService.getInstance().getPingManager();

int userId = 56456;

pingManager.pingUser(userId, new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void result, Bundle params) {

    }

    @Override
    public void onError(QBResponseException e) {

    }
});

// or sync version
try {
    boolean pingUser = pingManager.pingUser(userId);
} catch (SmackException.NotConnectedException e) {

}
```

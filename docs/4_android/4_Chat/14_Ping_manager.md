<span id="Ping_manager" class="on_page_navigation"></span>
## Ping manager

### Ping a server
The Ping manager allows a user to ping the backend by simply sending a ping to it. This is useful in a case to check a connection between user and server. 

```java
QBPingManager pingManager = QBChatService.getInstance().getPingManager();

pingManager.pingServer(new QBEntityCallback<Void>() {
    @Override
    public void onSuccess() {
        //reply was received from the server
    }

    @Override
    public void onError(QBResponseException error) {
        //any errors occured during ping server
    }
});

// or sync version

//true if a reply was received from the server, false otherwise.
boolean pingSuccess = pingManager.pingServer();
```

PingManger also periodically sends XMPP pings to the server every 30 minutes to avoid NAT timeouts and to test the connection status. 
To configure the ping interval and to listen for fails: 

```java
QBChatService.getInstance().setPingInterval(30);

pingManager.addPingFailedListener(new PingFailedListener(){
    public void pingFailed(){
        //some error occured during ping server
    }
});
```

### Ping a user

It's also possible to ping some user to check whethere he is online or not:
```java
final QBPingManager pingManager = QBChatService.getInstance().getPingManager();

int userId = 56456;

pingManager.pingUser(userId, new QBEntityCallback<Void>() {
    @Override
    public void onSuccess(Void result, Bundle params) {
        //reply was received from the user
    }

    @Override
    public void onError(QBResponseException e) {
        //some error occured during ping user
    }
});

// or sync version

//true if a reply was received from the user, false otherwise.
boolean pingUser = pingManager.pingUser(userId);
```

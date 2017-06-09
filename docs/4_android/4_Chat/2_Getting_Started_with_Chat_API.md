[**Getting a QuickBlox account**](http://admin.quickblox.com/register)


[**Creating applications in the Admin panel**](http://admin.quickblox.com/apps/new)

In addition, there is this helpful[5 minute guide](http://quickblox.com/developers/5_Mins_Guide).

**Prepare Chat service**

To initialise chat service use:
```java
QBChatService.setDebugEnabled(true); // enable chat logging
 
QBChatService.setDefaultPacketReplyTimeout(10000); //set reply timeout in milliseconds for connection's packet. Can be used for events like login, join to dialog to increase waiting response time from server if network is slow.
```

To configure chat use ```QBTcpConfigurationBuilder``` (for TCP connection).

```java
QBTcpConfigurationBuilder configurationBuilder = new QBTcpConfigurationBuilder()
        //socket configuration
        setSocketFactory(customSocketFactory) //Sets the socket factory used to create new Chat connection sockets.
        setKeepAlive(true) //Sets connection socket's keepAlive option, default value is 'true'. Ignored if custom socket factory was set before.
        setSocketTimeout(60) //Sets chat socket's read timeout in seconds, default value is 60 second. Ignored if custom socket factory was set before.
        setUseTls(true) //Sets the TLS security mode used when making the connection. By default TLS is enabled.
        setCustomSSLContext(customSslContext); //Sets a custom SSLContext for creating new SSL sockets.
        
        //chat connection configuration
        setPort(5223) //Sets port for chat connection. By default port is 5223.
        setHost(customHost) //Sets host for chat connection.By default host is CHAT_ENDPOINT.
        setServiceName(customServiceName) //Sets service name for chat connection. By default service name is CHAT_ENDPOINT.
        setReconnectionAllowed(true) //Enables auto reconnect when connection to server is lost. By default auto reconnect is enabled.
        setAllowListenNetwork(true) //Allows the ReconnectionManager to monitor the connection status for start reconnect immediately after the appearance of the connection
        setPreferredResumptionTime(prefferedResumtionTime) //Sets the preferred resumption time in seconds.
        
        //additional chat configuration
        setUseStreamManagement(false) //Enables Stream Management feature. By default Stream Management feature is disabled.
        setUseStreamManagementResumption(false)
        setAutojoinEnabled(false) //Enables autojoin to group chat after QBChatDialog creation, downloading  or updating it on REST. By default autojoin is disabled.
        setAutoMarkDelivered(true) //Enables marking messages as delivered when they came to the device. By default Auto Mark Delivered feature is enabled.
        
QBChatService.setConnectionFabric(new QBTcpChatConnectionFabric(configurationBuilder));
```

**Login to Chat**

Note: In order to login to the chat please read the information about [Chat login/password](http://quickblox.com/developers/Chat#Login_.2F_ID) formation. <br>

In order to use QuickBlox Chat APIs you must:
* Sign In to QuickBlox;
* Login to QuickBlox Chat


Please follow the lines below:

**Sign in User and Login to QuickBlox Chat**
```java
// Initialise Chat service
QBChatService chatService = QBChatService.getInstance();
 
final QBUser user = new QBUser("garrysantos", "garrysantospass");
 
QBUsers.signIn(user).performAsync(new QBEntityCallback<QBUser>() {
    @Override
    public void onSuccess(QBUser result, Bundle params) {
        // success, login to chat
        user.setId(result.getId());
                
        chatService.login(user, new QBEntityCallback() {
            @Override
            public void onSuccess() {
                
            }
                
            @Override
            public void onError(QBResponseException errors) {
                
            }
        });
    }

     @Override
     public void onError(QBResponseException responseException) {
        
     }
});

```
Note: If you don't use session automanagement, you have to create session before Sign in to QuickBlox. <br>

To handle different connection states use ConnectionListener:
```java
ConnectionListener connectionListener = new ConnectionListener() {
    @Override
    public void connected(XMPPConnection connection) {

    }

    @Override
    public void authenticated(XMPPConnection connection) {

    }

    @Override
    public void connectionClosed() {

    }

    @Override
    public void connectionClosedOnError(Exception e) {
        // connection closed on error. It will be established soon
    }

    @Override
    public void reconnectingIn(int seconds) {

    }

    @Override
    public void reconnectionSuccessful() {

    }

    @Override
    public void reconnectionFailed(Exception e) {

    }
};

QBChatService.getInstance().addConnectionListener(connectionListener);
```

<br>

**Chat in background mode**

Android  provides 'true' background mode but the better way to handle correctly chat offline messages is to do 'Chat logout' when app goes to background and does 'Chat login' when app goes to foreground.


**Logout from Chat**

Next code does chat logout:
```java
boolean isLoggedIn = chatService.isLoggedIn();

if (!isLoggedIn) {
    return;
}

chatService.logout(new QBEntityCallback() {

    @Override
    public void onSuccess() {
        // success

        chatService.destroy();
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

**Reconnection**

By default Android SDK reconnects automatically when connection to server is lost. 
But you can disable this and then manage this manually, for it you have to set 'false' to parameter ```setReconnectionAllowed(false)``` for 
```QBTcpConfigurationBuilder```


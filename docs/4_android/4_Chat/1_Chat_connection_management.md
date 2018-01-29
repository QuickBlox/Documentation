<span id="Chat_connection_management" class="on_page_navigation"></span>
## Prepare Chat service
You need to initialize chat service after [configuration and initialization QuickBlox framework]() .

If you wish to **change default settings** of ```QBChatService``` you can do that the following way:
```java
QBChatService.setDebugEnabled(true); // Enables chat logging
QBChatService.setDefaultPacketReplyTimeout(10000); //Sets reply timeout in milliseconds for connection's packet. Can be used for events like login, join to dialog to increase waiting response time from server if network is slow.
QBChatService.setDefaultAutoSendPresenceInterval(60); //Sets interval to send presence to server to keep connection active.
QBChatService.setDefaultConnectionTimeout(30000); //Sets how long the socket will wait until a TCP connection is established (in milliseconds).
```

To configure chat connection use ```QBTcpConfigurationBuilder``` (for TCP connection).

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
        setAutoMarkDelivered(true); //Enables marking messages as delivered when they came to the device. By default Auto Mark Delivered feature is enabled.
        
QBChatService.setConnectionFabric(new QBTcpChatConnectionFabric(configurationBuilder));
```

For creating instance of ```QBChatService``` need call ```QBChatService.getInstance();```

## Login to Chat

QuickBlox provides two ways of the login to chat:
* with user's ID and password;
* with user's ID and QuickBlox REST token.

First option fits if user logs into application using login and password or email and password (or any other way, where 
password is included).

Second option fit in case, you don't have user's password, e.g. user was created using Facebook, Twitter, Twitter Digits or another ways. 

For both options you need ```QBUser``` **with ID**. For getting QBUser with ID you have to create it on QuickBlox server. 
Needed information is located in section [Users stub link]();

* Prepare QBUser for login by ID and password

After getting QBUser from server you have to set the password manually before login to chat 
```java
QBUser qbUser = ...;
qbUser.setPassword(password);
```

* Prepare QBUser for login by ID and QuickBlox REST token

After getting QBUser from server you have to set QuickBlox REST token as password.  
```java
QBUser qbUser = ...;

String qbToken = QBSessionManager.getInstance().getToken();
qbUser.setPassword(qbToken);
```
<div class="attention">
Keep in mind, that for using token as password for login to chat, QBSession has to be created <b>with user</b>, 
you can find more  <a href="link on docs with authorization">there (fix me)</a>
</div>

After configuring QBUser you have to login to chat using this user. Code will be the same for both options:
```java
QBChatService.getInstance().login(qbUser, new QBEntityCallback() {
        @Override
        public void onSuccess() {
            //login to chat was successful
        }
                
        @Override
        public void onError(QBResponseException errors) {
            //login failed
        }
    });
```

By default user will be logged to chat with default resource ID, but QuickBlox provide overloaded method for login to chat with custom resource ID, you can use next snippet 
for it:
```java
QBEntityCallback callback = ...;
QBChatService.getInstance().login(qbUser, customResource, callback);
```

## Check login state

For getting chat state for current user, you can use next snippet:
```java
QBChatService.getInstance().isLoggedIn(); //returns 'true' if current user is logged in chat now or 'false' otherwise
```

## Listen chat connection states

To handle different connection states use ```ConnectionListener```:
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

## Chat in background mode

Android  provides 'true' background mode but the better way to handle correctly chat offline messages is to do 
'Chat logout' when app goes to background and do 'Chat login' when app goes to foreground.
<br>QuickBlox provides a way to setup automatic push notifications for the offline users. It means if your opponent 
is offline while you sending a message - he will automatically receive push notification.
Read how to setup automatic push notifications in Admin panel in [Alerts section](http://quickblox.com/developers/Chat#Alerts).

## Mobile optimisations

In default configuration messages and other chat packets are sent to client when processing is finished, but in mobile environment 
sending or receiving data drains battery due to use of radio.

To save energy data should be sent to client only if it is important or client is waiting for it.

When mobile client is entering inactive it is possible to notify the backend about this:

```java
try {
    QBChatService.getInstance().enterInactiveState();
} catch (SmackException.NotConnectedException e) {

}
```

After receiving packets server puts them into query to mobile client. Only last presence from each source is kept in queue, 
also Message Carbons are queued. Any other packets (such as iq or plain message) are sent immediately to the client. 

When mobile client is entering active it is possible to notify the backend about this:
```java
try {
    QBChatService.getInstance().enterActiveState();
} catch (SmackException.NotConnectedException e) {

}
```

After receiving this server sends all queued packets to the client. Also all packets from queue will be sent if number of packets 
in queue reaches queue size limit - limit is set to 50.


## Logout from Chat

The following code does chat logout:
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

## Reconnection

By default Android SDK reconnects automatically when connection to server is lost. 
But you can disable this and then manage reconnection manually, just set ```false``` to parameter ```setReconnectionAllowed(false)``` for 
```QBTcpConfigurationBuilder```

You can get worked app on GitHub

[**Chat sample on GIT**](https://github.com/QuickBlox/quickblox-android-sdk/tree/master/sample-chat)

[**All  latest Android Samples in ZIP**](https://github.com/QuickBlox/quickblox-android-sdk/archive/master.zip)


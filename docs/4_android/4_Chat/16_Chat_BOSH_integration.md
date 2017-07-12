<span id="Chat_BOSH_integration" class="on_page_navigation"></span>
## Chat BOSH integration

To use chat functions over **BOSH** protocol in QuickBlox Android SDK you need to follow these steps:

1. Add quickblox-android-sdk-chat-extensions** module dependency in the **buil.gradle** file of your project
```groovy
def qbSdkVersion = "3.2.0"

dependencies {
    compile("com.quickblox:quickblox-android-sdk-chat-extensions:$qbSdkVersion") 
    //quickblox-android-sdk-chat module will be connected automatically as transitive dependency

    //other dependencies
}
``` 

2. Create and configure QBBoshConfigurationBuilder

```java
QBBoshConfigurationBuilder configurationBuilder = new QBBoshConfigurationBuilder()
                .setUseTls(false) //Sets the TLS security mode used when making the connection. By default TLS is disabled.
                .setPort(5281) //Sets chat connection port number. By default for BOSH connection sets 5281
                .setHost("chat.quickblox.com") //Sets connection's host. By default used chat endpoint of your app.
                .setServiceName("chat.quickblox.com") //Sets the service name of XMPP service (i.e., the XMPP domain). By default used chat endpoint of your app. 
                .setSocketTimeout(60) //Sets chat socket's read timeout in seconds
                .setUseHttps(true) //Sets using "https" for connection. By default is true.
                .setFile("/http-bind/") //Sets the file name. By default is "/http-bind/".
                .setAutoMarkDelivered(false) //Sets automatic mark received messages as delivered. By default for BOSH connection sets false.
                .setAutojoinEnabled(false); //Sets true to automatically join loaded or created on server dialogs. By default sets false.
```

3. Create QBBoshChatConnectionFabric with custom configs
```java
QBBoshChatConnectionFabric connectionFabric = new QBBoshChatConnectionFabric(configurationBuilder);
```

4. Set connection fabric to QBChatService
```java
QBChatService.setConnectionFabric(connectionFabric);
```

That's all, now ```QBChatService``` will use BOSH type connection.

**BOSH** configuration also supports connection over **proxy**. To set your proxy settings for BOSH connection use:
```java
ProxyInfo proxyInfo = new ProxyInfo(ProxyInfo.ProxyType.HTTP, "10.30.30.1", 3128, null, null);

QBBoshConfigurationBuilder boshConfigurationBuilder = new QBBoshConfigurationBuilder();
boshConfigurationBuilder.setProxyInfo(proxyInfo);
```


<br><span style="color:red;">Note:</span> If you use BOSH protocol all chat requests are executed synchronously. 
To optimize app's performance you can use asynchronous methods (чтобы выполнять основные чат функции (методы) не на главном потоке) for standard chat 
features to run requests on non-main thread. 
QuickBlox Android SDK provides asynchronous implementation for next chat methods:

```java
chatDialog.sendMessage(chatMessage, new QBEntityCallback<Void>() {
            @Override
            public void onSuccess(Void aVoid, Bundle bundle) {
                
            }

            @Override
            public void onError(QBResponseException e) {

            }
        });  
```

```java
chatDialog.join(discussionHistory, new QBEntityCallback() {
            @Override
            public void onSuccess(Object o, Bundle bundle) {

            }

            @Override
            public void onError(QBResponseException e) {

            }
        });
```
        
```java
chatDialog.readMessage(chatMessage, new QBEntityCallback<Void>() {
            @Override
            public void onSuccess(Void aVoid, Bundle bundle) {

            }

            @Override
            public void onError(QBResponseException e) {

            }
        });
```

```java
chatDialog.deliverMessage(chatMessage, new QBEntityCallback<Void>() {
            @Override
            public void onSuccess(Void aVoid, Bundle bundle) {

            }

            @Override
            public void onError(QBResponseException e) {

            }
        });
```

```java
chatDialog.sendIsTypingNotification(new QBEntityCallback<Void>() {
            @Override
            public void onSuccess(Void aVoid, Bundle bundle) {

            }

            @Override
            public void onError(QBResponseException e) {

            }
        });
```
and other main methods.


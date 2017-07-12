# Sources 
[Project homepage on GIT](https://github.com/QuickBlox/quickblox-android-sdk/tree/master/sample-pushnotifications)

[Download ZIP](https://github.com/QuickBlox/quickblox-android-sdk/archive/master.zip)

<span id="Overview" class="on_page_navigation"></span>
# Overview

## How QuickBlox Push Notifications work

Как по-вашему может этот кусок лишний?

=======СТАРТ========

There are 2 ways that you can integrate Push Notifications into your app:
* **Broadcasting the same message to all users**. It's a simple method which can be used in informational apps that do not 
authenticate users and where same push notification messages are broadcasted to everybody. This way when you want to send a push 
message you may 1) simply go to Admin panel -> Push Notifications -> type your message in Simple mode -> and hit "Send" for all of 
your users to receive the message; 2) Send a push using Push Notifications API (explained below). Following this way you only need 
to create ONE QuickBlox User which will have all of your users' devices associated with it. Then simply send your pushes to that User.
* **Send individual push alerts**. This is when you want to individually send push notifications to a particular user or a group of 
users (for example notify users of a new chat message sent while they are offline or tell them about some deal/event happening in 
close proximity to their location). Following this way you need to have a QuickBlox User created for each of your app users. Note 
that there are easy ways to transparently create QB Users along with your existing users authentication system or have them sign up 
with Facebook/Twitter/OAuth (described in [Users code sample reference](http://quickblox.com/developers/SimpleSample-users-android)) 
so your end users don't have to type any additional login credentials or know anything about QuickBlox.

======КОНЕЦ=======

QuickBlox предоставляет возможность воспользоваться пуш уведомлениями (GCM and FCM) для уведомления юзеров о каких либо
событиях. QuickBlox позволяет отправлять пуш-уветомления с:
* админки;
* из мобильного приложения;

Пользователь может оправить как платформо ориентированное пуш-уведомление (Android, iOS, Windows Phone),  так и универсальное пуш-уведмление (получат 
все юзеры, не зависимо от того, с какой платформы была оформлена подписка). 

Также QuickBlox предоставляет такую опцию, как **"Офлайн пуши"** - это опция заключается в автоматической оправке пуш уведомлений о 
входящих сообщениях из приватных диалогов для офлайн юзеров или о сообщениях из групповых диалогов, когда юзер залогинен в чат, 
но не заджойнен в этот групповой диалог. Эта опция по умолчанию включена для всех приложений, но клиент может в любое время отключить в админке.

Данный раздел документации предназначен для того, чтобы показать пользователю как работать с QuickBlox Push Notifications API.

<span id="Preparing_app_for_QuickBlox_Push-notifications" class="on_page_navigation"></span>
Для того чтоб получить возможность пользоваться QuickBlox Push-notifications надо:

1. Добавить необходимые настройки в админке;
2. Интегрировать QuickBlox Push-notifications module в свое приложение;
3. Настроить приложение для работы с Push-notifications:
* настройка GCM пушей;
* настройка FCM пушей;

<span id="Admin_panel_congiguration" class="on_page_navigation"></span>
## Нвастройка админки
Для работы с QuickBlox Push-notifications module вам понадобится GCM API key, для его получения из Firebase developer console нужно:
* пойти в Firebase developer console;
* выбрать нужное приложение;
* open Settings/Project Settings
* go to Cloud Messaging
* copy Legacy Server key

![](./resources/push/firebase_get_api_key.png)

Для настройки QuickBlox админ панели вам необходимо засетить GCM API key. Чтобы это сделать, перейдите по пути Home -> app_name 
-> Push notifications -> Settings, перейдите в секцию "Google Cloud Messaging (GCM) API key" и засетьте свой API key. Вы можете использовать один и тот же
  API key для Development и Production enviroments.

![](./resources/push/admin_gcm_api_key.png)

Then you can start use QuickBlox Push Notification API.

<span id="Integration_QuickBlox_Push-notifications_module_in_application" class="on_page_navigation"></span>
## Integration QuickBlox Push-notifications module in application
Данная инструкция предполагает, что вы уже знакомы с порядком интеграции Quickblox фреймворка в приложение и уже выполнили такие 
действия для своего приложении:

* [Created QuickBlox account](http://admin.quickblox.com/register)
* [Registerer an application in Dashboard](http://quickblox.com/developers/5_Mins_Guide)
* [Integrated QuickBlox SDK into application - stub link]()


To use Push notifications module in your app, you just have to add dependency in **build.gradle** project file:
```groovy
dependencies {
    compile 'com.quickblox:quickblox-android-sdk-messages:3.3.3'
}
```

<span id="Configuring_app_for_work_with_Push-notifications_module_of_QuickBlox_Android_SDK" class="on_page_navigation"></span>
## Настройка приложения для работы с Push-notifications module of QuickBlox Android SDK
Наше СДК позаботилось об организации подписки на пуши вместо вас. Вам нет необходимости заботиться о получении GCM or FCM токена, 
подписке на пуши и отслеживании статуса подписки, эти все работы выполняются самостоятельно внутри СДК. Для того чтобы получить 
приложение, подписанное на пуши, достаточно выполнить несколько простых действий ниже.

### Настройка зависимостей в ```build.gradle```

As part of enabling Google APIs or Firebase services in your Android application you may have to add the google-services plugin to your Project build.gradle file:
```groovy
dependencies {
    classpath 'com.google.gms:google-services:3.0.0'
…
}
```
<div class="attention">
Note: If you use google play services in your project, use version 9.8.0 or higher (version we use in our SDK) as  
all com.google.android.gms libraries must use the exact <b>same version</b> specification.
</div>

<div class="attention">
If you use version <b>higher</b> 10.2.1 just add explicit dependency:
<code>
<br>
compile "com.google.android.gms:play-services-gcm:10.2.6"
<br>
compile "com.google.firebase:firebase-messaging:10.2.6"
<br>
compile "com.google.firebase:firebase-core:10.2.6"
</code>
</div>


Include gms plugin in the **bottom** of your module ```build.gradle```: 
```groovy
apply plugin: 'com.google.gms.google-services'
```

### Настройка ```google-services.json``` файла
Для работы вашего приложения с пушами, необходимо скачать с firebase девелопер консоли файл ```google-services.json``` для вашего приложения.
For **GCM** push type add empty **current_key** settings into your ```google-services.json``` file for your package name (если такого поля нет):
```json
"client": [
    {
      "client_info": {
        "mobilesdk_app_id": "1:761750217637:android:c4299bc46191b0d7",
        "client_id": "android:com.your.app",
        "client_type": 1,
        "android_client_info": {
          "package_name": "com.your.app"
        }
      },
      ...
      "api_key": [
        {
          "current_key": ""
        }
      ],
```

### Setting android manifest
To integrate Auto push subscription you just need set values in ```AndroidManifest.xml```:
```xml
<meta-data android:name="com.quickblox.messages.TYPE" android:value="GCM" />
<meta-data android:name="com.quickblox.messages.SENDER_ID" android:value="@string/sender_id" />
<meta-data android:name="com.quickblox.messages.QB_ENVIRONMENT" android:value="DEVELOPMENT" />
```
```
com.quickblox.messages.TYPE - can be GCM or FCM
com.quickblox.messages.SENDER_ID - your project id from google console (for ex. 761750217637)
com.quickblox.messages.QB_ENVIRONMENT - can be DEVELOPMENT or PRODUCTION
```

and as usual, setting for GCM GcmPushListenerService:
```xml
<service
    android:name="com.quickblox.messages.services.gcm.QBGcmPushListenerService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    </intent-filter>
</service>

<service
    android:name="com.quickblox.messages.services.gcm.QBGcmPushInstanceIDService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.android.gms.iid.InstanceID" />
    </intent-filter>
</service>
```
and other stuff like permissions (look here for details https://developers.google.com/cloud-messaging/android/client).

If you are using type **FCM**, it is a little bit easier, to get message you just need set ```QBFcmPushListenerService``` and 
```QBFcmPushInstanceIDService```:
```xml
<service
    android:name="com.quickblox.messages.services.fcm.QBFcmPushListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>

<service 
    android:name="com.quickblox.messages.services.fcm.QBFcmPushInstanceIDService">
    <intent-filter>
        <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter>
</service>
```

**Pay attention, FCM can send 2 types of messages:**
* Notification messages,
* Data messages.

Android app handles them differently. Refer to https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages

**Different push types**

Now Google Cloud Messaging can be two types: Google Cloud Messaging (GCM) or Firebase Cloud Messaging (FCM). Our SDK provides both. 
You can set this type in meta-data.

<span id="Receiving_simple_push_notifications" class="on_page_navigation"></span>
## Receiving simple push notifications
To receive push notification just register BroadcastReceiver:
```java
private BroadcastReceiver pushBroadcastReceiver = new BroadcastReceiver() {
    @Override
    public void onReceive(Context context, Intent intent) {
        String message = intent.getStringExtra("message");
        String from = intent.getStringExtra("from");
        Log.i(TAG, "Receiving message: " + message + ", from " + from);
    }
};


LocalBroadcastManager.getInstance(this).registerReceiver(pushBroadcastReceiver, new IntentFilter("new-push-event"));
```
And that’s all! You are ready now to receive push notifications.

<span id="Receiving_push_notifications_with_additional_parameters" class="on_page_navigation"></span>
## Receiving push notifications with additional parameters
Пуш также может содержать и дополнительные параметры кроме ```message``` and ```from```.
В таком случае вам нужно:
* для **GCM** пушей:
надо расширить класс ```QBGcmPushListenerService``` из нашего СДК и переопределить его метод ```onMessageReceived(String from, Bundle data)```
Параметр ```data``` будет содержать все дополнительные параметры вашего пуша. В примере ниже, вы можете посмотреть, как получить дополнительные данные из GCM пуша:
```java
public class GcmPushListenerService extends QBGcmPushListenerService {
    ...
    
    @Override
    public void onMessageReceived(String from, Bundle data) {
        super.onMessageReceived(from, data);

        String message = data.getString("message");
        String customParameter1 = data.getString("custom_parameter_1");
        String customParameter2 = data.getString("custom_parameter_2");
        ...
    }
}
```

<div class="attention">
Будьте внимательны! Если вы используете свой сервис для прослушивания GCM пушей, то в шаге [линка на пункт]() вам необходимо вместо класса
<code>QBGcmPushListenerService</code> засетить свой сервис:
<code>
<br>
вместо нашего сервиса <br>
android:name="com.quickblox.messages.services.gcm.QBGcmPushListenerService"<br>

надо использовать свой сервис <br>
android:name="com.quickblox.my_app.services.GcmPushListenerService"
</code>
</div>

* для **FCM** пушей:
надо расширить класс ```QBFcmPushListenerService``` из нашего СДК и переопределить его метод ```onMessageReceived(RemoteMessage remoteMessage)```
Параметр ```remoteMessage``` будет содержать в том числе все дополнительные параметры вашего пуша. В примере ниже, вы можете посмотреть, как получить
дополнительные параметры из **FCM** пуша:
```java
public class FcmPushListenerService extends QBFcmPushListenerService {
    ..
    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        super.onMessageReceived(remoteMessage);
        
        Map<String, String> pushData = remoteMessage.getData();
        String message = pushData.get("message");
        String customParameter1 = pushData.get("custom_parameter_1");
        String customParameter2 = pushData.get("custom_parameter_2");
        ...
    }
}
```

<div class="attention">
Будьте внимательны! Если вы используете свой сервис для прослушивания FCM пушей, то в шаге [линка на пункт]() вам необходимо вместо класса
<code>QBFcmPushListenerService</code> засеть свой сервис:
<code>
<br>
вместо нашего сервиса <br>
android:name="com.quickblox.messages.services.fcm.QBFcmPushListenerService"<br>

надо использовать свой сервис <br>
android:name="com.quickblox.my_app.services.FcmPushListenerService"
</code>
</div>


<span id="Sumscription_management" class="on_page_navigation"></span>
## Управление подпиской на пуши


### Enable and disable delivering push to application
SDK has some setting for push notification.
You can use global setting to enable or disable delivering pushes via ```QBSettings``` (means set this parameter once):
```java
QBSettings.getInstance().setEnablePushNotification(false); //as default true
```
To check this parameter 
```java
QBSettings.getInstance().isEnablePushNotification();
```

### Tracking subscriptions status
To be aware about what is happening with push subscription, are you subscribed successful or not, there is ```QBSubscribeListener```.
Add ```QBSubscribeListener``` right after ```QBSettings.getInstance().init();```
Code snippet:
```java
QBPushManager.getInstance().addListener(new QBPushManager.QBSubscribeListener() {
            @Override
            public void onSubscriptionCreated() {
                Log.d(TAG, "onSubscriptionCreated");
            }

            @Override
            public void onSubscriptionError(final Exception e, int resultCode) {
                Log.d(TAG, "onSubscriptionError" + e);
                if (resultCode >= 0) {
                    Log.d(TAG, "Google play service exception" + resultCode);
                }
                Log.d(TAG, "onSubscriptionError " + e.getMessage());
            }
        });
```

### Disabling subscribing at all
If you don’t want to use auto subscribe feature at all, just do not set ```meta-data```.

In case you were subscribed, now want to unsubscribe and do not subscribe again use global setting ```SubscribePushStrategy.NEVER``` 
like this:
```java
// default SubscribePushStrategy.ALWAYS
QBSettings.getInstance().setSubscribePushStrategy(SubscribePushStrategy.NEVER); 
```

And of cause you can subscribe or unsubscribe from pushes whatever you need just use:
```java
SubscribeService.subscribeToPushes(context, false);
SubscribeService.unSubscribeFromPushes(context);

//second param true - if you use InstanceIDListenerService and google token onTokenRefresh
@Override
public void onTokenRefresh() {
    SubscribeService.subscribeToPushes(context, true);
}
```

<span id="Migrate_to_FCM" class="on_page_navigation"></span>
## Migrate to FCM
Firebase Cloud Messaging (FCM) is the new version of GCM. It inherits the reliable and scalable GCM infrastructure, plus new features! 
See the [FAQ](http://firebase.google.com/support/known-issues/#gcm-fcm) to learn more. If you are integrating messaging in a new app, start 
with FCM. GCM users are strongly recommended to upgrade to FCM, in order to benefit from new FCM features today and in the future.

Google prepared great guide how to [Migrate a GCM Client App for Android to Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)

**QuickBlox backend does support FCM endpoints** so you need to update **only client-side** code.


## Sending Push Notifications

<span id="Send_Push_Notifications_from_Admin_panel" class="on_page_navigation"></span>
### Send Push Notifications from Admin panel

Just go to Push Notifications module on Admin panel, choose '''Environment''', '''Channel''', type message and press '''Send message''' button:

![](./resources/push/SendPushNotificationsAndroid.png)

Your message will be delivered to all your users. You will see

![](./resources/push/NotifySuccess.png)

and

![](./resources/push/AndroidPushOnDevice.png)

> Note:You choose byself how to show push to user. There is no standard Android mechanism. 

Also you can set more options through '''Advanced''' mode like:
Group of users (using tags), that will receive message

![](./resources/push/PushNotificationsTags.png)

Delivery settings

![](./resources/push/PushNotificationsDeliverySettings.png)

<span id="Send_Push_Notifications_from_Application" class="on_page_navigation"></span>
### Send Push Notifications from Application
Как говорилось ранее it's possible to send 2 types of push notifications:
* Platform based Push Notifications
* Universal push notifications

Refer to the [Push Notification Formats](http://quickblox.com/developers/Messages#Push_Notification_Formats) document to better understand what happens under hood. 

<span id="Platform_based_Push_Notifications" class="on_page_navigation"></span>
#### Platform based Push Notifications
Send Platform based push notifications (GCM) (only works for Android). Platform based push notification will be delivered to **specified** platform only - in our case it's Android:

##### Send Push Notification to particular users (through their IDs)
```java
// recipients
StringifyArrayList<Integer> userIds = new StringifyArrayList<Integer>();
userIds.add(53779);
userIds.add(960);

QBEvent event = new QBEvent();
event.setUserIds(userIds);
event.setEnvironment(QBEnvironment.DEVELOPMENT);
event.setNotificationType(QBNotificationType.PUSH);
event.setPushType(QBPushType.GCM);
HashMap<String, String> data = new HashMap<String, String>();
data.put("data.message", "Hello");
data.put("data.type", "welcome message");
event.setMessage(data);

QBPushNotifications.createEvent(event).performAsync( new QBEntityCallback<QBEvent>() {
    @Override
    public void onSuccess(QBEvent qbEvent, Bundle args) {
        // sent
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

##### Send push notification to a group of users (via Tags)
```java
// recipients
StringifyArrayList<String> usersTags = new StringifyArrayList<String>();
usersTags.add("Man");
usersTags.add("Car");

QBEvent event = new QBEvent();
event.setUserTagsAny(usersTags);
event.setEnvironment(QBEnvironment.DEVELOPMENT);
event.setNotificationType(QBNotificationType.PUSH);
event.setPushType(QBPushType.GCM);
HashMap<String, String> data = new HashMap<String, String>();
data.put("data.message", "Hello");
data.put("data.type", "welcome message");
event.setMessage(data);

QBPushNotifications.createEvent(event).performAsync( new QBEntityCallback<QBEvent>() {
    @Override
    public void onSuccess(QBEvent qbEvent, Bundle args) {
        // sent
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

##### Send VOIP Push Notifications (iOS only)
To initiate iOS VoIP push notification use ```event.setPushType(QBPushType.APNS_VOIP);``` in above example.

<span id="Universal_push_notifications" class="on_page_navigation"></span>
#### Universal push notifications
Universal push notifications will be delivered to all possible platforms and devices for specified users.
With universal push notifications there are 2 ways to send it:
* Just send a simple push with text only
* With custom parameters

##### Simple push with text
```java
// recipients
StringifyArrayList<Integer> userIds = new StringifyArrayList<Integer>();
userIds.add(53779);
userIds.add(960);

QBEvent event = new QBEvent();
event.setUserIds(userIds);
event.setEnvironment(QBEnvironment.DEVELOPMENT);
event.setNotificationType(QBNotificationType.PUSH);
event.setMessage("Gonna send Push Notification!");

QBPushNotifications.createEvent(event).performAsync(new QBEntityCallback<QBEvent>() {
    @Override
    public void onSuccess(QBEvent qbEvent, Bundle args) {
        // sent
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

##### With custom parameters
Please refer to [Universal Push Notifications payload format](https://quickblox.com/developers/Messages#Use_custom_parameters) to get 
more details what parameters you can use and also how you can initiate iOS VoIP push notifications 
(you can initiate VoIP push by passing ```ios_voip=1``` parameter).

```java

// recipients
StringifyArrayList<Integer> userIds = new StringifyArrayList<Integer>();
userIds.add(53779);
userIds.add(960);

QBEvent event = new QBEvent();
event.setUserIds(userIds);
event.setEnvironment(QBEnvironment.DEVELOPMENT);
event.setNotificationType(QBNotificationType.PUSH);


JSONObject json = new JSONObject();
try {
    json.put("message", "hello");

    // custom parameters
    json.put("user_id", "234");
    json.put("thread_id", "8343");

} catch (Exception e) {
    e.printStackTrace();
}

event.setMessage(json.toString());

QBPushNotifications.createEvent(event).performAsync(new QBEntityCallback<QBEvent>() {
    @Override
    public void onSuccess(QBEvent qbEvent, Bundle args) {
        // sent
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```



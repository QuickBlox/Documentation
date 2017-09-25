<span id="Push_notifications_format" class="on_page_navigation"></span>
# Push Notifications Format
QuickBlox provides 2 type of push notifications you can send:

* Platform based push notification - will be delivered to specified platform only, for example, iOS or Android only.
* Universal push notifications - will be delivered to all possible devices/platforms for specified users.

## Platform based Push Notification
Platform based push notification will be delivered to specified platform only. To specify platform use **event[push_type]** parameter when create event.

With platform based push notification you can use all specified features of particular platform, no restrictions.

### General requirements
Message format is a key=value string. Where key is raw text and value - BASE64 encoded and CGI escaped. Each pair should be separated by *&*.

**Example:** 
```
key1=c29tZXZhbHVlMQ==&key2=YW5vdGhlcnZhbHVlMg==&key3=dGhpcmRleGFtcGxl 
```
**Important!** *&* should be escaped by *%26* 

### iOS
To meet [Apple payload requirements](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html#//apple_ref/doc/uid/TP40008194-CH17-SW1). 

**Payload example:**
```
{
    "aps" : {
        "alert" : "You got your emails.",
        "badge" : 9,
        "sound" : "bingbong.aiff"
    },
    "acme1" : "bar",
    "acme2" : 42
}
```

Base64 encoded data:
```
ew0KICAgICJhcHMiIDogew0KICAgICAgICAiYWxlcnQiIDogIllvdSBnb3QgeW91ciBlbWFpbHMuIiwNCiAgICAgICAgImJhZ
GdlIiA6IDksDQogICAgICAgICJzb3VuZCIgOiAiYmluZ2JvbmcuYWlmZiINCiAgICB9LA0KICAgICJhY21lMSIgOiAiYmFyIiwNCiAg
ICAiYWNtZTIiIDogNDINCn0=
```

Final message (for iOS pushes you should add **payload=** before message):
```
event[message]=payload=ew0KICAgICJhcHMiIDogew0KICAgICAgICAiYWxlcnQiIDogIllvdSBnb3QgeW91ciBlbWFpbHMuIiwNCiAgICAgICAg
ImJhZGdlIiA6IDksDQogICAgICAgICJzb3VuZCIgOiAiYmluZ2JvbmcuYWlmZiINCiAgICB9LA0KICAgICJhY21lMSIgOiAiYmFyIiwNCi
AgICAiYWNtZTIiIDogNDINCn0=
```

### Android
To meet [Google GCM/FCM requirements](https://developers.google.com/cloud-messaging/gcm).

Message format is:
```
data.key1=value1&...&data.keyN=valueN
```

Values should be CGI-escaped before Base64 encoding.
Required field **collapse_key** is added automatically before sending and contains value of "event_id".

**data.message** key is required and should go 1st.

**Payload example:**
```
data.message=I love M&M's! Especially red one!
```

CGI-escaped:
```
data.message=I+love+M%26M%27s%21+Especially+red+one%21
```

Base64-encoded:
```
data.message=SStsb3ZlK00lMjZNJTI3cyUyMStFc3BlY2lhbGx5K3JlZCtvbmUlMjE=
```

Final message:
```
event[message]=data.message=SStsb3ZlK00lMjZNJTI3cyUyMStFc3BlY2lhbGx5K3JlZCtvbmUlMjE=
```

### Windows Phone
not supported actively

### Blackberry
not supported actively

## Universal Push Notifications
Universal push notifications will be delivered to all possible devices/platforms for specified users. To send Universal push notifications just omit **event[push_type]** parameter when create event.

### Universal push with custom parameters
With custom parameters you can achieve behaviour similar to platform based push notifications but do not care about platfrom-specific push payload parameters.

**Note:** 
```
Custom parameters only available for iOS and Android platforms.
```

There are some standard parameters, which will be translated to particular platform parameters:

* **message** - push text. Will be translated to **aps.alert.body** for iOS and to **data.message** for Android.
* **ios_badge** - will be translated to **aps.badge** for iOS. Ignored for Android.
* **ios_sound** - will be translated to **aps.sound** for iOS. Ignored for Android.
* **ios_content_available=1** - will be translated to **aps.content-available** for iOS. Ignored for Android.
* **ios_mutable_content=1** - will be translated to **aps.mutable-content=** for iOS. Ignored for Android.
* **ios_category** - will be translated to **aps.category** for iOS. Ignored for Android.
* **ios_voip=1** - will initiate VoIP push notification for iOS if user has VoIP push subscription. Otherwise - iOS user will receive standart iOS push. For Android - it will be a standard push.

You can use any other custom parameters - they will be added as well according to specific platform push format. For iOS - it will be root keys, for Android - data.X.

**Payload example**:
```
{"message": "Message received from Bob", "ios_badge": 5, "ios_sound": "mysound.wav", "user_id": "234"}
```

Base64-encoded:
```
c29tZXZhbHVlMQc29tZXZhbHVlMQc29tZXZhbHVlMQc29tZXZhbHVlMQc29tZXZhbHVlMQ
```

Final message:
```
event[message]=c29tZXZhbHVlMQc29tZXZhbHVlMQc29tZXZhbHVlMQc29tZXZhbHVlMQc29tZXZhbHVlMQ
```

Client's application will receive next payload:

**iOS:**

```objectivec
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    NSLog(@"New push: %@", userInfo);
}
 
...
 
{
    aps =     {
        alert = "Message received from Bob";
        badge = 5;
        sound = "mysound.wav";
    };
    "user_id" = 234;
}
```

**Android:**

```java
@Override
protected void onMessage(Context context, Intent intent) {
    String message = "";
 
    for(String key : intent.getExtras().keySet()){
         Log.d(LOG_TAG, key + ": " + intent.getExtras().getString(key));
    }
}
 
...
 
07-21 11:07:26.489  14443-14842/? D/GCMIntentService﹕ message: Message+received+from+Bob
07-21 11:07:26.489  14443-14842/? D/GCMIntentService﹕ user_id: 234
07-21 11:07:26.489  14443-14842/? D/GCMIntentService﹕ collapse_key: event1206083
07-21 11:07:26.489  14443-14842/? D/GCMIntentService﹕ from: 761750217637
```
 

### Universal push with just text
If you would like to send just text push message (without any parameters):

**Payload example:** 

```
I love M&M's! Especially red one!
```

Base64-encoded:
```
SSBsb3ZlIE0mTSdzISBFc3BlY2lhbGx5IHJlZCBvbmUh
```

Final message:
```
event[message]=SSBsb3ZlIE0mTSdzISBFc3BlY2lhbGx5IHJlZCBvbmUh
```

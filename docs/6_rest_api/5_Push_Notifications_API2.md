<span id="Push_notifications_format" class="on_page_navigation"></span>
# Push Notifications Format
QuickBlox provides 2 type of push notifications you can send:

* Platform based push notification - will be delivered to specified platform only, for example, iOS or Android only.
* Universal push notifications - will be delivered to all possible devices/platforms for specified users.

## Platform based push notification
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
TBA

### Android
TBA

### Windows Phone
TBA

### Blackberry
TBA
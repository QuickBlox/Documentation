<span id="Stickers" class="on_page_navigation"></span>
## Stickers

This section describe how to add stickers to your chat (full
sample on [https://github.com/908Inc/stickerpipe-chat-sample](https://github.com/908Inc/stickerpipe-chat-sample])

### Dependencies

Add stickers repository in **build.gradle**:
```
repositories {
   maven { url  'http://maven.stickerpipe.com/artifactory/stickerfactory' }
}
```

Add library dependency in **build.gradle**:
```
compile('vc908.stickers:stickerfactory:x.x.x@aar') {
     transitive = true;
}
```

List of available versions you can find
here http://maven.stickerpipe.com/artifactory/stickerfactory/vc908/stickers/stickerfactory/

### Eclipse IDE

Use this guide https://github.com/908Inc/stickerpipe-android-sdk-for-eclipse
to add StickerPipe library in Eclipse.

### Manifest

Add content provider with your application package to your manifest file:
```xml
<provider
     android:name="vc908.stickerfactory.provider.StickersProvider"
     android:authorities="<YOUR PACKAGE>.stickersProvider"
     android:exported="false"/>
```

### Initialization

Initialize library at your Application in ```onCreate()``` method
```java
StickersManager.initialize(“847b82c49db21ecec88c510e377b452c", this);
```

You can get your own API Key on http://stickerpipe.com to have customized packs set.

### Stickers fragment and customisation

Create stickers fragment
```java
stickersFragment = new StickersFragment.Builder().build();
```

You can customize stickers fragment with next setters:
* ```setOnStickerSelectedListener```
Set sticker selected listener
It’s highly recommended to set listener outside of builder, directly to fragment when creating or restoring activity.

* ```setStickerPlaceholderDrawableRes```
Sets custom placeholder for stickers in list

* ```setStickerPlaceholderColorFilterRes```
Sets color filter for sticker’s placeholder

* ```setStickersListBackgroundDrawableRes```
Sets background drawable resource for stickers list with ```Shader.TileMode.REPEAT```

* ```setStickersListBackgroundColorRes```
Sets stickers list background color

* ```setTabPlaceholderDrawableRes```
Sets custom placeholder for tab icons

* ```setTabBackgroungColorRes```
Sets tab panal background color

* ```setTabUnderlineColorRes```
Sets selected tab underline color

* ```setTabIconsFilterColorRes```
Sets color filter for default tab icons placeholder

* ```setMaxStickerWidth```
Sets max width for sticker cell

* ```setBackspaceFilterColorRes```
Sets color filter for backspace button at emoji tab

* ```setEmptyRecentTextRes```
Sets text for empty view at recent tab

* ```setEmptyRecentTextColorRes```
Sets text color for empty view at recent tab

* ```setEmptyRecentImageRes```
Sets image for empty view at recent tab

* ```setEmptyRecentColorFilterRes```
Sets image color filter for empty view at recent tab

Then you only need to show fragment.

### Stickers showing and customizations

To send stickers you need to set listener and handle results

```java
// create listener
private OnStickerSelectedListener stickerSelectedListener = new OnStickerSelectedListener() {
    @Override
    public void onStickerSelected(String code) {
        if (StickersManager.isSticker(code)) {
	    // send message
        } else {
            // append emoji to your edittext
        }
    }
};
// set listener to your stickers fragment
stickersFragment.setOnStickerSelectedListener(stickerSelectedListener)
```

Listener can take an emoji, so you need to check code first, and then send sticker code or append emoji to your edittext.

```java
// Show sticker in adapter
if (StickersManager.isSticker(message)){ // check your chat message
StickersManager.with(context) // your context - activity, fragment, etc
        .loadSticker(message)
          .into((imageView)); // your image view 
} else {
	// show a message as it is
}
```

And you can customize sticker loading process with next setters:
* ```setPlaceholderDrawableRes``` - Set sticker placeholder drawable

* ```setPlaceholderColorFilterRes``` - Set color filter for default placeholder

<br>

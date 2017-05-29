**QuickBlox** - Communication & cloud backend (BaaS) platform which brings superpowers to your mobile apps.

<span id="Features" class="on_page_navigation">
</span>

# Features

[Authentication](), [Users](), [Chat](), [Custom Objects]()

<span id="Requirements" class="on_page_navigation">
</span>

# Requirements

- Android 4.0+


# Installation

## Using Maven

Navigate to your build.grade file at the app level (not project level) and include the following:

```
repositories {
    maven { url "https://github.com/QuickBlox/quickblox-android-sdk-releases/raw/master/" }
}

dependencies {
    сompile "com.quickblox:quickblox-android-sdk-chat:3.3.1"
    сompile "com.quickblox:quickblox-android-sdk-content:3.3.1"
    сompile "com.quickblox:quickblox-android-sdk-messages:3.3.1"
}
```

## Local dependency

Optionally you can download the latest SDK AAR file from Github:

* Go to SDK repository https://github.com/QuickBlox/quickblox-android-sdk-releases/tree/master/com/quickblox
and download AAR file for needed sdk module.
* put downloaded file to /libs directory inside your application
* include file dependency into your build.gradle file:

```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.aar'])
}
```

# License

```
QuickBlox Android SDK is licensed under the QuickBlox SDK License.
```

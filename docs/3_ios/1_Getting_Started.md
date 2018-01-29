
<span id="Prerequisites" class="on_page_navigation"></span>
# Prerequisites

Before you begin, you need a few things set up in your environment:

- Xcode 8.0 or later
- An Xcode project targeting iOS 8 or above
- Swift projects must use Swift 3.0 or later
- The bundle identifier of your app
- An Apple Push Notification Authentication Key for your [Apple Developer account](https://developer.apple.com/account)

To use QuickBlox, head on over to the [**releases**](https://github.com/QuickBlox/quickblox-ios-sdk/releases) page, and download the latest build. Take a look at the public [**documentation**](https://quickblox.com) **(Need update link before release)** and start building.

<span id="Cocoapods" class="on_page_navigation"></span>
# Cocoapods

We recommend installing QuickBlox using [CocoaPods](http://cocoapods.org/), which provides a simple dependency management system that automates the error-prone process of manually configuring libraries. First make sure you have CocoaPods installed.


> CocoaPods is built with Ruby and is installable with the default Ruby available on OS X. 


    sudo gem install cocoapods
    #Setup the CocoaPods environment
    pod setup
  

**Podfile** is a specific file for CocoaPods where you list the libraries you want to use in your project. To create a **Podfile**:

- Just create an empty text file named `**Podfile**` ****in your project directory.
- Or you can run `$ pod init`  command in your project directory and cocoapods create a file for you with some features in it.

Add the following line to this file:

  
    pod 'QuickBlox'

Finally, run `pod install` to setup your CocoaPods environment and import `QuickBlox` into your project. 


> **NOTE**
> Make sure to start working from the `.xcworkspace` file that CocoaPods automatically creates, not the `.xcodeproj` you may have had open.

<span id="Manual_installation" class="on_page_navigation"></span>
# Manual installation

Our [releases repo](https://github.com/QuickBlox/quickblox-ios-sdk/releases) provides samples and two framework distributions you can add to your project: 


- `**QuickBlox.framework**` `and` `**QuickBloxWebRTC.framework**`: dynamic frameworks which are compatible with Objective-C and Swift projects that target **iOS 8** or later.

Download the appropriate build artefact from this repository and add it to your application:

1. Open the app’s Xcode project or workspace.
2. Drag and drop the framework onto your project, instructing Xcode to copy items into your destination group’s folder.
3. Go to the app target’s `General` configuration page.
4. Add the framework target to the `Embedded Binaries` section by clicking the `Add` (+)icon, highlighted in Figure 1. 
5. Select your framework from the list of binaries that can be embedded (possible duplicate under `Linked Frameworks and Libraries`). 
![Figure 1: Click the Add button to embed a framework](https://d2mxuefqeaa7sj.cloudfront.net/s_DF0E961259CFB1CE17234E9C7818E55070E6CFE9DE8186A5054B0FD52EC8B947_1510310117670_EmbeddedBinaries.png)

<span id="Configuring_Keychain_Sharing" class="on_page_navigation"></span>
# Configuring Keychain Sharing

In the Capabilities pane, if Keychain Sharing isn’t enabled, click the switch in the Keychain Sharing section as shown in Figure 2.

![Figure 2: Enable Keychain Sharing](https://d2mxuefqeaa7sj.cloudfront.net/s_DF0E961259CFB1CE17234E9C7818E55070E6CFE9DE8186A5054B0FD52EC8B947_1510243600964_Screenshot+2017-11-09+18.06.14.png)

<span id="Add_Run_Script_Phase_to_Build_Phases" class="on_page_navigation"></span>
# Add Run Script Phase to Build Phases

1. Go to `Build Phase` section of your target settings.
2. Click on the + button in the top left and select `New Run Script Phase`.
3. In the script text input box paste the following snippet:


    bash "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/Quickblox.framework/strip-framework.sh"


> NOTE
> This script fixes a [known Apple bug](http://www.openradar.me/radar?id=6409498411401216),  which does not allow to publish applications which contain dynamic frameworks with simulator platforms to App Store.

<span id="Verifying_installation" class="on_page_navigation"></span>
# Verifying installation

Import the Quickblox module into your `UIApplicationDelegate` subclass:

    //Objective-C:
    @import Quickblox;


    //Switft
    import Quickblox

Configure a `QBSettings` instance, typically in your application's `application:didFinishLaunchingWithOptions:` method:


> **NOTE**
> Make sure to use your actual credentials (application ID, authorisation key , 
> authorisation secret) which can be found from the [Developer Dashboard](https://admin.quickblox.com).
![Figure 3. Application Credentials](https://d2mxuefqeaa7sj.cloudfront.net/s_DF0E961259CFB1CE17234E9C7818E55070E6CFE9DE8186A5054B0FD52EC8B947_1510319113135_Screenshot+2017-11-10+14.55.31.png)

    //Objective-C
    QBSettings.applicationID = 50025
    QBSettings.authKey = @"9pmaysJkEsjYgHz";
    QBSettings.authSecret = @"1tpHyQjks7JYjHa";
    //Swift
    QBSettings.applicationID = 639234222
    QBSettings.authKey = "9pmaysJkEsjYgHz"
    QBSettings.authSecret = "1tpHyQjks7JYjHa"

QuickBlox provides a flexible mechanism for apps to retrieve all the correct endpoints (API, Chat, etc) to work with. This mechanism allows a smooth transition between [Plans](http://quickblox.com/plans). To get the Account key open [Admin panel, your Account settings page](https://admin.quickblox.com/account/settings) and copy the **Account key** value:

![Figure 4.  Copy the account key](https://d2mxuefqeaa7sj.cloudfront.net/s_DF0E961259CFB1CE17234E9C7818E55070E6CFE9DE8186A5054B0FD52EC8B947_1510325630889_Screenshot+2017-11-10+16.38.56.png)

    //Objective-C
    QBSettings.accountKey = @"ce5kMptFMws9KtI7DM";
    //Swift
    QBSettings.accountKey = "ce5kMptFMws9KtI7DM"

To verify that everything is installed and working correctly, create a new user request:

    //Create a new user
    let user = QBUUser.user;
    user.email = "same.smit@email.com"
    user.password = "password"
    user.phone = "+380668661731"
    user.tags = ["male", "single"]
    //Sign up request
    QBRequest.signUp(user, successBlock: { (request, user) in
      //success
    }) { (response) in
      //error
    }

Build and run your project to verify the installation was successful.

Want to contribute to this doc? [**Edit this section.**](https://github.com/QuickBlox/Documentation/tree/master/docs/3_ios)


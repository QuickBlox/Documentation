<span id="Overview" class="on_page_navigation"></span>
# Overview
QuickBlox Content API is intended for uploading and downloading files of any types. This functionality is convenient for use as an 
additional means for exchanging images, short video or audio files (for example, video or audio messages) or other files (documents, 
for example) while chatting.

The maximum file size limit for uploads via QuickBlox Content API is 100MB.

QuickBlox provides Unlimited Data Storage.


<span id="Content_Management_via_Admin_Panel" class="on_page_navigation"></span>
# Content Management via Admin Panel
Content Management System (CMS) allows you to manage the apps contents and settings without having to re-publish it. 
You can upload any files through web interface, edit them, delete, i.e. perform all typical operations.

Go to [Admin panel](http://admin.quickblox.com) -> Content module. You can see list of your files:

![](./resources/content/CMS1.png)

You may have no files. To upload a new file just use **File Upload** section bellow table as follows: select file to upload using **Browse...**
 button, then press **Upload** button and the new file will be uploaded.

You can create rich HTML file using wysiwyg redactor - just press **Add Text/HTML** button - **New file** page will be opened. 
You can use wysiwyg redactor to create rich content.


<span id="Content_Management_via_Application" class="on_page_navigation"></span>
# Content Management via Application


<span id="Prepare_your_application_for_QuickBlox_Android_SDK" class="on_page_navigation"></span>
## Prepare your application for Android SDK
This guide implies that you are already familiar with the procedure of QuickBlox framework integration and you have already 
taken the following steps for your application:

* [Created QuickBlox account](http://admin.quickblox.com/register)
* [Registerer an application in Dashboard](http://quickblox.com/developers/5_Mins_Guide)
* [Integrated QuickBlox SDK into application - stub link]()

## Integrate QuickBlox Content module in your application
To use QuickBlox Content API in your app, you just need add the following dependency in **build.gradle** project file:
```groovy
dependencies {
    compile "com.quickblox:quickblox-android-sdk-content:3.3.3"
}
```

That's all! You can use QuickBlox Content API in your application.


<span id="Upload_file" class="on_page_navigation"></span>
# Upload file
To upload a file using Android SDK use code bellow:

```java
File file = ...;

boolean isPublic = false;

QBContent.uploadFileTask(file, isPublic, null, new QBProgressCallback() {
            @Override
            public void onProgressUpdate(int progress) {
                //progress - progress in percent
            }
        }).performAsync(new QBEntityCallback<QBFile>() {
            @Override
            public void onSuccess(QBFile qbFile, Bundle bundle) {
                //file uploaded successfully
            }

            @Override
            public void onError(QBResponseException e) {
                //an error occurred during file upload
            }
        });
```

Read more about ```isPublic``` argument [here](http://quickblox.com/developers/SimpleSample-content-android#Public.2FPrivate_urls).


<span id="Update_file" class="on_page_navigation"></span>
# Update file
For updating an existing file you can use the following example:
```java
File newFile = ...;

int quickBloxFileID = 91079;

String tags = ...;

QBContent.updateFileTask(newFile, quickBloxFileID, tags, new QBProgressCallback() {
    @Override
    public void onProgressUpdate(int progress) {

    }
}).performAsync( new QBEntityCallback<QBFile>() {
    @Override
    public void onSuccess(QBFile qbFile, Bundle params) {

    }
    
    @Override
    public void onError(QBResponseException errors) {

    }
});
```


<span id="Retrieve_files" class="on_page_navigation"></span>
# Retrieve files

You can use the following page parameters:
* ```perPage``` - how many files each page will contain (max 100)
* ```page``` - current returned page (номер страницы, которую сервер должен вернуть)

To retrieve list of current user's files use code bellow:
```java
QBPagedRequestBuilder requestBuilder = new QBPagedRequestBuilder(5, 2);

QBContent.getFiles(requestBuilder).performAsync( new QBEntityCallback<ArrayList<QBFile>>() {
    @Override
    public void onSuccess(ArrayList<QBFile> files, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
```

# Download file

<span id="Download_file_by_UID" class="on_page_navigation"></span>
## Download file by UID

```java
final String fileUID = "6221dd49a1bb46cfb61efe62c4526bd800";
Bundle params = new Bundle();
QBContent.downloadFile(fileUID, new QBProgressCallback() {
    @Override
    public void onProgressUpdate(int progress) {

    }
}, params).performAsync( new QBEntityCallback<InputStream>(){
        @Override
        public void onSuccess(InputStream inputStream, Bundle params) {
            long length = params.getLong(Consts.CONTENT_LENGTH_TAG);
            Log.i(TAG, "content.length: " + length);

            // use inputStream to download a file
        }

        @Override
        public void onError(QBResponseException errors) {

        }
});
```


<span id="Public_Private_urls" class="on_page_navigation"></span>
# Public/Private urls

There are 2 types of files in Content module:

* **Private files** - can be accessed only by QuickBlox user with session token
* **Public files** - can be accessed from anywhere in the Internet, don't need to have a session token. 

You can manage this type when you upload a file to Content module via  ```public``` 
[argument](https://quickblox.com/developers/SimpleSample-content-android#Upload_file).

To access a private url use the following method:
```java
QBFile file = ...;
String privateUrl = file.getPrivateUrl();

// or if you have only file UID
String fileUID = "6221dd49a1bb46cfb61efe62c4526bd800";
String privateUrl = file.getPrivateUrlForUID(fileUID);
```


To access a public url use the following method:
```java
QBFile file = ...;
String publicUrl = file.getPublicUrl();

// or if you have only file UID
String fileUID = "6221dd49a1bb46cfb61efe62c4526bd800";
String privateUrl = file.getPublicUrlForUID(fileUID);
```


<span id="Sources" class="on_page_navigation"></span>
# Sources
You can find examples of Content module usage in the following ways:
* by checking sample content [on GIT](https://github.com/QuickBlox/quickblox-android-sdk/tree/master/sample-content)
* if you download [ZIP with all our samples](https://github.com/QuickBlox/quickblox-android-sdk/archive/master.zip)  and select "sample-content" module.
# Overview
This sample demonstrates how to work with QuickBlox Custom Objects API.<br> 
It allows to create any server side data structure, use it as you want, create any logic and a lot of others custom features.

It shows how to:
* Create own server data structure & use it. In our example we created data structure, that represents simple Note.
* Get, Create, Update, Delete your data using a lot of filters.

<br>

<img size=150>http://files.quickblox.com/Custom1.png</img> <img size=150>http://files.quickblox.com/Custom2.png</img><img size=150>http://files.quickblox.com/Custom3.png</img><img size=150>http://files.quickblox.com/Custom4.png</img>

<br>

=Guide: Get Started with Custom Objects API=
==Get QuickBlox account==
[http://admin.quickblox.com/register http://admin.quickblox.com/register] 

==Create application on Admin panel==
[http://admin.quickblox.com/apps/new http://admin.quickblox.com/apps/new]

Also you can look through [http://quickblox.com/developers/5_Mins_Guide 5 min guide].

==Connect QuickBlox to your application==
To get the information on how to connect to the quickblox.jar, please, refer to the [http://quickblox.com/developers/Android#How_to:_add_SDK_to_IDE_and_connect_to_the_cloud Android-how-to-connect-quickblox-jar] page.

==Add Custom Data structure to your application==
'''Note:''' This guide based on [http://quickblox.com/developers/Custom_Objects Custom Objects REST API] documentation. It is very helpful, describes full list of QuickBlox Custom Objects API features and need to be read during reading this guide.

To start using Custom Objects module you should create your data schema.
Go to [http://admin.quickblox.com admin.quickblox.com], Custom Objects module page and press '''Add new class''' button. 'Add new class' popup will be appeared.

Enter Class name, add fields any you want. Allow 7 types of fields:
* Integer
* Float
* Boolean (true/false)
* String
* File
* Location
* Array (of Integers, Floats, Booleans, Strings)

[[File:COCreateClass.png]] 

Press '''Create class''' button - new class will be created

[[File:COPredefinedFields.png]] 

There are some predefined fields, that described in [http://quickblox.com/developers/Custom_Objects#Module_description Custom Objects REST API Module description] documentation.
<br>

=== Create record ===
There are 2 ways to create record:
* through Admin panel
* using Android SDK

<br>
==== Create record through Admin panel ====
Just go to [http://admin.quickblox.com admin.quickblox.com], Custom Objects module page and press '''Add record''' button. 'Add new record' popup will be appeared. Fill any fields you want and press '''Add record''' button. New record will be added & shown in table.

<br>
==== Create a record using Android SDK ====
To create a record use the snippet bellow:

<syntaxhighlight lang="java">
QBCustomObject object = new QBCustomObject();

// put fields
object.putString("name", "Star Wars");
object.putString("comment", "Star Wars is an American epic space opera franchise consisting of a film series created by George Lucas.");
object.putString("genre", "fantasy");

// set the class name
object.setClassName("Movie");

QBCustomObjects.createObject(newRecord, new QBEntityCallback<QBCustomObject>() {
    @Override
    public void onSuccess(QBCustomObject createdObject, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

=== Retrieve records===

==== By ID ====
To retrieve record by id:
<syntaxhighlight lang="java">
QBCustomObject customObject = new QBCustomObject("Note", "50e3f8c7535c126073000d52");

QBCustomObjects.getObject(object, new QBEntityCallback<QBCustomObject>(){
    @Override
    public void onSuccess(QBCustomObject customObject, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

==== By IDs ====
To retrieve records by ids:
<syntaxhighlight lang="java">
StringifyArrayList<String> coIDs = new StringifyArrayList<String>();
coIDs.add("50e67e6e535c121c66004c74");
coIDs.add("50e67e6d535c127f66004f47");
coIDs.add("50e67e6b535c121c66004c72");
coIDs.add("50e59f81535c121c660015fd");

QBCustomObjects.getObjectsByIds("Note", coIDs, new QBEntityCallback<ArrayList<QBCustomObject>>() {
    @Override
    public void onSuccess(ArrayList<QBCustomObject> customObjects, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>


==== With filters ====
There are lots of operators and filters that you can use to retrieve records:


===== Filters =====
The request can contain all, some or none of next filters.


Filter | Applicable to types | Usage example | Description
-------|---------------------|---------------|------------
|**{field_name}** |All types  | requestBuilder.eq("rating", "2"); | Search records with field which contains exactly specified value
|{field_name}[{search_operator}] | All types  | requestBuilder.gt("rating", "5"); | Search record with field which contains value according to specified value and operator
|'''sort_asc''' | All types  | requestBuilder.sortAsc("field_name") | Search results will be sorted by specified field in ascending order
|'''sort_desc''' | All types  | requestBuilder.sortDesc("field_name") | Search results will be sorted by specified field in descending order
|'''skip''' | Integer | requestBuilder.setSkip(100) | Skip N records in search results. Useful for pagination. Default (if not specified): 0
|'''limit''' | Integer | requestBuilder.setLimit(50) | Limit search results to N records. Useful for pagination. Default value - 100. Also can set 1000.'''If limit is equal to -1 only last record will be returned'''
|'''count''' | Integer | There is a separate method to count records | Count search results. Response will contain only count of records found
|'''output[include]''' | All types | requestBuilder.outputInclude(arrayOfFields) | The '''output''' parameter takes the form of a record with a list of fields for inclusion or exclusion from the result set. '''output[include]''' specifies the fields to include. The '''_id''' field is, by default, included in the result set. To exclude the '''_id''' field from the result set, you need to specify in the '''output[exclude]''' value the exclusion of the _id field.
|'''output[exclude]''' | All types | requestBuilder.outputExclude(arrayOfFields) | The '''output''' parameter takes the form of a record with a list of fields for inclusion or exclusion from the result set. '''output[exclude]''' specifies the fields to exclude. The '''_id''' field is, by default, included in the result set. To exclude the '''_id''' field from the result set, you need to specify in the '''output[exclude]''' value the exclusion of the '''_id''' field.
|'''near''' | Location | requestBuilder.near("user_location", new Double[]{34.666, 12.445}, 10) |  Search records in a specific radius with current position in meters. Format: field_name,longitude,latitude,radius(in meters)


===== Search operators =====

The request can contain all, some or none of next search operators. Combinations of operators are allowed.

{| border="1" cellpadding="10" cellspacing="0"
!align="center" |Operator !!Applicable to types !!Usage example !!Description
|-
| '''lt'''
| Integer, Float
| requestBuilder.lt("rating", 5)
| '''L'''ess '''T'''han operator
|-
| '''lte'''
| Integer, Float
| requestBuilder.lte("rating", 5)
| '''L'''ess '''T'''han or '''E'''qual to operator
|-
| '''gt'''
| Integer, Float
| requestBuilder.gt("rating", 5)
| '''G'''reater '''T'''han operator
|-
| '''gte'''
| Integer, Float
| requestBuilder.gte("rating", 5)
| '''G'''reater '''T'''han or '''E'''qual to operator
|-
| '''ne'''
| Integer, Float, String, Boolean
| requestBuilder.ne("rating", 5)
| '''N'''ot '''E'''qual to operator
|-
| '''in'''
| Integer, Float, String,Array
| requestBuilder.in("tags", "man", "golf")
| Contained '''IN''' array operator
|-
| '''nin'''
| Integer, Float, String,Array
| requestBuilder.nin("tags", "man", "golf")
| '''N'''ot contained '''IN''' array
|-
| '''all'''
| Array
| requestBuilder.all("tags", "man", "golf")
| '''ALL''' contained '''IN''' array
|-
| '''or'''
| Integer, Float, String
| 1.requestBuilder.or("name", "igor", "bob");<br>2.requestBuilder.or("name", "igor").or("lastname", "khomenko")
| 1.Will return records with name '''igor''' or '''bob''' <br>2.Will return records with name '''igor''' or lastname '''khomenko'''
|-
| '''ctn'''
| String
| requestBuilder.ctn("name", "ar")
| Will return all records where '''username''' field contains '''ar''' substring
|-
|}


For example, we want to retrieve 5 movies that:
* Have '''rating > 5.5'''
* Not documentary
* Movies must be sorted by '''rating''' field in ascending order 

<syntaxhighlight lang="java">
QBRequestGetBuilder requestBuilder = new QBRequestGetBuilder();
requestBuilder.gt("rating", "5.5");
requestBuilder.setPagesLimit(5);
requestBuilder.eq("documentary", "false");
requestBuilder.sortAsc("rating");

QBCustomObjects.getObjects("Movie", requestBuilder, new QBEntityCallback<ArrayList<QBCustomObject>>() {
    @Override
    public void onSuccess(ArrayList<QBCustomObject> customObjects, Bundle params) {
        int skip = params.getInt(Consts.SKIP);
        int limit = params.getInt(Consts.LIMIT);
    }

    @Override
    public void onError(QBResponseException errors) {
    
    }
});
</syntaxhighlight>

=== Update record ===
To update existing record you should know record ID. 
Let's update Movie with ID 502f7c4036c9ae2163000002 - set '''rating''' to 7.88:

<syntaxhighlight lang="java">
QBCustomObject record = new QBCustomObject();
record.setClassName("Movie");
HashMap<String, Object> fields = new HashMap<String, Object>();
fields.put("rating", "7.88");
record.setFields(fields);
record.setCustomObjectId("502f7c4036c9ae2163000002");

QBCustomObjects.updateObject(record, null, new QBEntityCallback<QBCustomObject>() {
    @Override
    public void onSuccess(QBCustomObject object, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

Also there are Special update operators, that are very helpful for some purpose, e.g. update Array field etc:

==== Special update oparators ====
{| border="1" cellpadding="10" cellspacing="0"
! align="center" | Operator !!Applicable to types !!Usage example !! Description
|-
| inc
| Integer, Float
| updateBuilder.inc("rating", 1);
| Increment field to specified value. Value can positive or negative (i.e. decrement operation)
|-
| pull
| Arrays
| updateBuilder.pull("tags", "man")
| Removes specified value from array field
|-
| pull '''with filter'''
| Arrays
| updateBuilder.pullWithFilter("user_ids", "gt", 10)
| Removes all elements, which filtered by filter operator, from array
|-
| pull_all
| Arrays
| updateBuilder.pullAll("tags", "one", "two")
| Removes all specified values from array
|-
| pop
| Arrays
| updateBuilder.pop("tags", 1) 
| Removes last element from array. To remove first element value should be equal '''-1'''
|-
| push
| Arrays
| updateBuilder.push("tags", "one", "two")
| Appends specified values to array
|-
| add_to_set
| Arrays
| updateBuilder.addToSet("tags", "man")
| Adds a value to an array only if the value is not in the array already
|-
| Update array element by index operator
| Arrays
| updateBuilder.updateArrayValue("tags", 1, "two")
| Update array element by index
|-
|}


Let's update an element with index 1 in array 'tags' with value 'man':


<syntaxhighlight lang="java">
QBCustomObject record = new QBCustomObject();
record.setClassName("Movie");
record.setCustomObjectId("502f7c4036c9ae2163000002");

QBRequestUpdateBuilder updateBuilder = new QBRequestUpdateBuilder();
updateBuilder.updateArrayValue("tags", 1, "man");

QBCustomObjects.updateObject(null, updateBuilder, new QBEntityCallback<QBCustomObject>() {
    @Override
    public void onSuccess(QBCustomObject object, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>
<br>

=== Delete record ===
To delete existing record you should know record ID. Let's delete Movie with ID 502f83ed36c9aefa62000002:
<syntaxhighlight lang="java">
QBCustomObject customObject = new QBCustomObject("Movie", "502f83ed36c9aefa62000002");

QBCustomObjects.deleteObject(customObject, new QBEntityCallbackImpl() {
    @Override
    public void onSuccess() {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

=== Multi operations ===
It is possible to do some operations on multiple objects in a single query.

==== Create multiple records ====
Create multiple records in a single query:
<syntaxhighlight lang="java">
// Create 3 random records
List<QBCustomObject> customObjectsList = new ArrayList<QBCustomObject>(3);
//
for (int i = 0; i < 3; i++){
    Random random = new Random();
    QBCustomObject customObject = new QBCustomObject("Note");
    customObject.putInteger("rating", random.nextInt(100));
    customObject.putString("description", "Hello world");
    //
    customObjectsList.add(customObject);
}

QBCustomObjects.createObjects(customObjectsList, new QBEntityCallback<ArrayList<QBCustomObject>>() {
    @Override
    public void onSuccess(ArrayList<QBCustomObject> customObjects, Bundle args) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

==== Update multiple records ====
Update multiple records in a single query:
<syntaxhighlight lang="java">
QBCustomObject co1 = new QBCustomObject("Note");
co1.putInteger("rating", 10);
co1.setCustomObjectId("50e3f85f535c123376000d31");
//
QBCustomObject co2 = new QBCustomObject("Note");
co2.putInteger("rating", 8);
co2.setCustomObjectId("50e3f85f535c123376000d31");
//
QBCustomObject co3 = new QBCustomObject("Note");
co3.putInteger("rating", 12);
co3.setCustomObjectId("50e3f85f535c123376000d31");
//
List<QBCustomObject> customObjectList = new LinkedList<QBCustomObject>();
customObjectList.add(co1);
customObjectList.add(co2);
customObjectList.add(co3);

QBCustomObjects.updateObjects(customObjectList, new QBEntityCallback<ArrayList<QBCustomObject>>() {
    @Override
    public void onSuccess(ArrayList<QBCustomObject> objects, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

==== Delete multiple records ====
Delete multiple records in a single query:
<syntaxhighlight lang="java">
StringifyArrayList<String> deleteIds = new StringifyArrayList<String>();
deleteIds.add("50e3f85f535c123376000d31");
deleteIds.add("50e3f85f535c123376000d32");

QBCustomObjects.deleteObjects("Note", deleteIds, QBEntityCallback<ArrayList<String>>() {
    @Override
    public void onSuccess(ArrayList<String> deletedObjects, Bundle params) {
        Log.i(TAG, ">>> deleted: " + deletedObjects.toString());
        ArrayList<String> notFound = params.getStringArrayList(com.quickblox.customobjects.Consts.NOT_FOUND_IDS);
        ArrayList<String> wrongPermissions = params.getStringArrayList(com.quickblox.customobjects.Consts.WRONG_PERMISSIONS_IDS);
        Log.i(TAG, ">>> notFound: " + notFound.toString());
        Log.i(TAG, ">>> wrongPermissions: " + wrongPermissions.toString());
    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

=== Relations ===
We allow to organize relation between 2 objects - just use special field '''_parent_id'''. 

For example, if you want to add Comments to movie - you can use code snippet bellow:

<syntaxhighlight lang="java">
QBCustomObject customObject = new QBCustomObject("Comment"); // your Class name
customObject.putInteger(fieldHealth, 99);
customObject.putString("text", "The first film in the series was originally released on May 25, 1977, under the title Star Wars, by 20th Century Fox");
customObject.setParentId("50aa4d8fefa357fa14000001");

QBCustomObjects.createObject(customObject, new QBEntityCallback<QBCustomObject>() {
    @Override
    public void onSuccess(QBCustomObject object, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

This is strong reference - it means that if you will delete parent record (Movie) - all children records (Comments) will be deleted too.
This field ('''_parent_id''') may be included in Retrieve/Create/Update queries as well.

<br>

=== Permissions ===
A Permissions (access control list)  is a list of  permissions attached to an record in Custom Objects module. This list specifies which users are granted access to records, as well as what operations are allowed on given objects (READ,CREATE,UPDATE,DELETE).

Each entry in a typical Permissions specifies a subject and an operation. For instance, if a record has a permission that contains (Garry, EDIT), this would give Garry permission to edit this record.

Read [http://quickblox.com/developers/Custom_Objects#Permissions Permissions REST API description] to better understand how it works

There are 4 typical operations on record:
* CREATE
* READ
* UPDATE
* DELETE

'''CREATE''' operations can't be set through API (only in Admin panel)

Each operation has info who can perform it on object.

There are 5 values ('''QBPermissionsLevel'''):
* '''QBPermissionsLevel.OPEN''' - any user can perform operation on this record
* '''QBPermissionsLevel.OWNER''' - only owner can perform operation on this record
* '''QBPermissionsLevel.NOT_ALLOWED''' - nobody can perform operation on this record (except Owner)
* '''QBPermissionsLevel.OPEN_FOR_USER_IDS''' - users with these IDs can perform operation on this record
* '''QBPermissionsLevel.OPEN_FOR_GROUPS''' - users in these groups can perform operation on this record

'''Note:''' Owner always has access to own record (except case when [http://quickblox.com/developers/Custom_Objects#Permission_levels Class permissions level] is enabled)

<br>
==== Create record with Permissions ====
Let's create record with next permissions:
* READ: Open
* UPDATE: Users in groups '''golf, man'''
* DELETE: Users with IDs '''3060, 63635'''

<syntaxhighlight lang="java">
QBCustomObject newRecord = new QBCustomObject("Note");
newRecord.put("rating", 99);
newRecord.put("description", "Hello world");

//
// set permissions:
// READ
QBPermissions permissions = new QBPermissions();
permissions.setReadPermission(QBPermissionsLevel.OPEN);
//
// UPDATE
ArrayList<String> usersTags = new  ArrayList<String>();
usersTags.add("golf");
usersTags.add("man");
permissions.setDeletePermission(QBPermissionsLevel.OPEN_FOR_GROUPS, usersTags);
//
// DELETE
ArrayList<String> usersIDS = new  ArrayList<String>();
usersIDS.add("3060");
usersIDS.add("63635");
permissions.setDeletePermission(QBPermissionsLevel.OPEN_FOR_USER_IDS, openPermissionsForUserIDS);
                
newRecord.setPermission(permissions);

QBCustomObjects.createObject(newRecord, new QBEntityCallback<QBCustomObject>() {
    @Override
    public void onSuccess(QBCustomObject object, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

==== Obtain record's Permissions ====
You can obtain info about record permissions by it ID - info only about own records are available for user: 

<syntaxhighlight lang="java">
QBCustomObjects.getObjectPermissions("Note", "53f44e7befa3573473000002", new QBEntityCallback<QBPermissions>() {
    @Override
    public void onSuccess(QBPermissions permissions, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

<br>

==== Update record's Permissions ====
Let's update record's permissions to next:
* READ: Users in groups '''car, developers'''
* UPDATE: Owner
* DELETE: Owner

<syntaxhighlight lang="java">
QBCustomObject record = new QBCustomObject();
record.setClassName("Note");
record.setCustomObjectId("52b30274535c12fbf80121bd");

//
// update permissions:
// READ
QBPermissions permissions = new QBPermissions();
ArrayList<String> usersTags = new  ArrayList<String>();
usersTags.add("car");
usersTags.add("developers");
permissions.setDeletePermission(QBPermissionsLevel.OPEN_FOR_GROUPS, usersTags);
//
// UPDATE
permissions.setUpdatePermission(QBPermissionsLevel.OWNER);
//
// DELETE
permissions.setDeletePermission(QBPermissionsLevel.OWNER);
                
record.setPermission(permissions);

QBCustomObjects.updateObject(record, null, new QBEntityCallback<QBCustomObject>() {
    @Override
    public void onSuccess(QBCustomObject object, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

=== Files ===
Custom Objects module supports ‘File’ field type. It is created for easily working with content from Custom Objects module. There is an ability to upload, download, update and delete content of file fields.

'''Note:''' The '''max''' file size is '''32 MB'''.

<br>
==== Upload/Update file ====
<syntaxhighlight lang="java">
// get file
File file = ...;

QBCustomObject customObject = new QBCustomObject("Note", "a234234abc647fed6473333");

 QBCustomObjectsFiles.uploadFile(file, customObject, "avatar", new QBEntityCallback<QBCustomObjectFileField>() {

    @Override
    public void onSuccess(QBCustomObjectFileField uploadFileResult, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
}, new QBProgressCallback() {
    @Override
    public void onProgressUpdate(int progress) {

    }
});
</syntaxhighlight>

<br>

==== Download file ====
<syntaxhighlight lang="java">
QBCustomObject customObject = new QBCustomObject("Note", "a234234abc647fed6473333");

QBCustomObjectsFiles.downloadFile(customObject, "avatar", new QBEntityCallback<InputStream>(){
    @Override
    public void onSuccess(InputStream inputStream, Bundle params) {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
}, new QBProgressCallback() {
    @Override
    public void onProgressUpdate(int progress) {

    }
});
</syntaxhighlight>

<br>

==== Delete file ====
<syntaxhighlight lang="java">
QBCustomObject customObject = new QBCustomObject("Note", "a234234abc647fed6473333");

QBCustomObjectsFiles.deleteFile(customObject, "avatar", new QBEntityCallback() {

    @Override
    public void onSuccess() {

    }

    @Override
    public void onError(QBResponseException errors) {

    }
});
</syntaxhighlight>

= Sources =
'''Project homepage on GIT —''' [https://github.com/QuickBlox/quickblox-android-sdk/tree/master/sample-custom-objects https://github.com/QuickBlox/quickblox-android-sdk/tree/master/sample-custom-objects]

'''Download ZIP - ''' [https://github.com/QuickBlox/quickblox-android-sdk/archive/master.zip https://github.com/QuickBlox/quickblox-android-sdk/archive/master.zip]

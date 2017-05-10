This sample shows you how to work with Users API, **Custom Objects API** and **Content API** by Quickblox JavaScript SDK.
It allows you to create any server side data structure, authorize and work with files as uploading.

Below is the sample you'll create using this tutorial, sources files you can found in [Github](https://github.com/QuickBlox/quickblox-javascript-sdk/tree/gh-pages/samples) or inspect detailed on [Github Pages](https://quickblox.github.io/quickblox-javascript-sdk/samples/data).

<!--- TODO: change to src DATA to github --->
<iframe src="https://samples.quickblox.com/web/data" width="100%" height="550px" frameborder="1" scrolling="no">

# Try it yourself
Before all, adding a Custom Data structure to your application.

<div class="panel panel-warning">
  <div class="panel-body">
     Note: This guide based on <a href="https://quickblox.com/developers/Custom_Objects" target="_blank">Custom Objects REST API documentation</a>. It is very helpful, describes the full list of QuickBlox Custom Objects API features and need to be read in addition to reading this guide.
  </div>
</div>

To start using the Custom Objects module you should create your data scheme. Go to [admin.quickblox.com](https://admin.quickblox.com/), find the Custom Objects module page and press the Add new class button. The 'add new class' popup will then appear.

![Create a new class](../resources/create_class_popup.png)

Enter Class name, add fields any you want. Allow 7 types of fields:
* Integer;
* Float;
* Boolean;
* String;
* File;
* Location;
* Array (of Integers, Floats, Booleans, Strings);

<div class="panel panel-info">
  <div class="panel-body">
    Important:
    There are some predefined fields, that are described in this <a href="http://quickblox.com/developers/Custom_Objects#Module_description" target="_blank">Custom Objects REST API Module description</a> documentation.
  </div>
</div>

### Create a record
There are 2 ways to create a record:
* through the Admin panel
* using the Javascript SDK

#### Create record through Admin panel

Just go to [admin.quickblox.com](https://admin.quickblox.com/), find the Custom Objects module page and press the **Add record** button. The **add new record** popup will then appear. Fill in any fields you want and press the Add record button again. The new record will then be added & shown in the table.

#### Create record using Javascript SDK

In order to create records you must be logged in and to act on a user's behalf - please refer to Javascript Users API documentation.

To create a record just use the code below:

```javascript
var className = 'Place';
var recorded = {
  title: 'Example Title',
  description: 'Some example description'
};

QB.data.create(className, recorded, function(err, res){
    if (err) {
        console.log(err);
    } else {
        console.log(res);
    }
});
```

### Retrieve records
To retrieve a record use next method:
```javascript
var className = 'Place';
var filter = { sort_asc: 'created_at' };
 
QB.data.list(className, filter, function(err, result){
    if (err) { 
        console.log(err);
    } else {
        console.log(result);
    }
});
```

### Filters
The request can contain all, some or none of next filters.


| Filter | Applicable to types | Usage example | Description |
|--------|---------------------|---------------|-------------|
| {field_name} | All types | {rating: '2'} | Search records with field which contains exactly specified value |
|{field_name} [{search_operator}] |All types | {rating: {gt: '2'}} | Search record with field which contains value according to specified value and operator |
|***sort_asc*** | All types  | {sort_asc: 'created_at'} | Search results will be sorted by specified field in ascending order |
|***sort_desc*** | All types  | {sort_desc: 'created_at'} | Search results will be sorted by specified field in descending order |
|***skip*** | Integer | {skip: '100'} | Skip N records in search results. Useful for pagination. Default (if not specified): 0 |
|***limit*** | Integer | {limit: '100'} | Limit search results to N records. Useful for pagination. Default value - 100. Also can set 1000.***If limit is equal to -1 only last record will be returned*** |
|***count*** | Integer | {count: '1'} | Count search results. Response will contain only count of records found |
|**output[include]** | All types | {output: {include: 'name,rating'}} | The **output** parameter takes the form of a record with a list of fields for inclusion or exclusion from the result set. **output[include]** specifies the fields to include. The **_id** field is, by default, included in the result set. To exclude the **_id** field from the result set, you need to specify in the **output[exclude]** value the exclusion of the _id field. |
|**output[exclude]** | All types | {output: {exclude: 'name,rating'}} | The **output** parameter takes the form of a record with a list of fields for inclusion or exclusion from the result set. **output[exclude]** specifies the fields to exclude. The **_id** field is, by default, included in the result set. To exclude the **_id** field from the result set, you need to specify in the **output[exclude]** value the exclusion of the **_id** field. |
|**near** | Location | {user_location: {near: '23.4545,56.1233;10'}} |  Search records in a specific radius with current position in meters. Value format: longitude,latitude;radius (in meters) |
|


<span id="Relations" class="on_page_navigation"></span>
# Relations 
It is possible to create a relation between objects of two different classes  - via **_parent_id** field. 

For example, we have the class **Rating** that contains fields **Score**, **Review**, **Comment**. And we have the class **Movie**. So we can create a record of class **Rating** that will point to the record of the class **Movies** via its **_parent_id** field, so the **_parent_id** field will contain an ID of record from class **Movie**.

This is not a simple soft link. This is actually a hard link. When you delete the **Movie** class record then all its children (records of class **Rating** with **_parent_id** field set to the **Movie** class record ID) will be automatically deleted as well. 

If you need to retrieve all the children - you can retrieve records with the filter **_parent_id=<id_of_parent_class_record>**. 

<span id="Permissions" class="on_page_navigation"></span>
# Permissions 
Permissions 

<span id="Tips_and_Tricks" class="on_page_navigation"></span>
# Tips and Tricks 

## Sort by _id field
According to the module description, the **_id** field contains info about object time creation (timestamp). It means that sorting on the **_id** ﬁeld is roughly equivalent to sorting by **created_at** field, but works much faster because the **_id** field has a predefined database index.

Hence it is highly recommended to use sort by **_id** (e.g. sort_desc=_id) field instead of sort by **created_at** field: 

## 'skip' parameter performance
The **skip** parameter is often expensive because it requires the server to walk from the beginning of the class table to get the offset or skip position before beginning to return result. As offset increases, **skip** will become slower and more CPU intensive. With larger collections, **skip** may become IO bound.

Instead of using big value of **skip** try to improve your query. Maybe you don't need it. 
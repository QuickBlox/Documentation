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
Tips and Tricks 
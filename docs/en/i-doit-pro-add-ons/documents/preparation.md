Installation/Update
-------------------

The current installation package for the add-on is available in the i-doit customer portal at [https://login.i-doit.com/](https://login.i-doit.com/) after purchasing a valid license and can be downloaded there. The installation itself takes place in the i-doit [Admin Center](/display/en/Admin+Center) in the "Add-ons" tab.Here, the package is selected from the local system via the "Browse" button. After specifying for which [Tenant(s)](/display/en/Multi-Tenants) the add-on should be made available, the installation can be completed via the "Upload and install" button.  
After a new version has been released, the updated package, which will also be available in the i-doit customer portal, can be uploaded and installed in the Admin Center using the same path.

Assignment of rights
--------------------

In order for a user to be able to create templates and generate documents, it is necessary to assign appropriate rights. This is possible in the i-doit administration under Rights system > Assignment of rights > Documents, if the add-on is installed.

![](/download/attachments/113475973/image2021-12-29_10-31-6.png?version=1&modificationDate=1640770859087&api=v2&effects=drop-shadow)

  

The following permissions are available here:

*   **Document**

The "Document" condition can be used to allow access to single or multiple documents. Users with this permission can edit the selected documents.

*   **Documents in categories**

Documents in categories" can be used to grant access to documents based on the document categories.

*   **Templates**

The "Template" permission grants the permission to edit document templates.

*   **Templates in categories**

Access to templates can be granted to templates from defined categories using "Templates in categories".

*   **Online Repository**

To download templates from the online repository the permission "Online Repository" is required.

Clear cache for the rights system

After the rights have been assigned, it is advisable to clear the cache in the i-doit [administration](/display/en/Administration) under `[System Tools](/display/en/System+Tools) > Cache / Database` so that the changes are adopted by the system.

Additional configuration
------------------------

The add-on additionally makes the "Documents" [category](/display/en/Categories+and+Attributes) available for all object types. The category can be linked to the desired object types via the [object type configuration](/display/en/Custom+Object+Types) and the [Quick Configuration Wizard](/display/en/Assignment+of+Categories+to+Object+Types). In this way, only those object types appear during object creation that are specified by the user in order to keep the selection list clear.

Documents category

In order to create a document for an object, it is necessary that the "Documents" category is linked to the object type. If the object type does not have this category, no documents can be created for the objects it contains, because they do not appear in the selection.

Calling the add-on
------------------

After all preparations are completed, the add-on can be accessed via `Extras > Documents`.

![](/download/attachments/113475973/image2021-12-29_10-40-51.png?version=1&modificationDate=1640770859072&api=v2&effects=drop-shadow)

The view of the "Extras" menu may differ due to different rights and/or additional installed add-ons.
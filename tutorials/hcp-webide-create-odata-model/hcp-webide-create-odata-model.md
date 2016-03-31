---
title: Manually creating a data model to use in SAP Web IDE’s Mock Data server
description: Learn how to create a data model in the Common Schema Definition Language (CSDL) using SAP Web IDE
tags: [ tutorial:interest/cloud, tutorial:product/hcp, tutorial:product/sapui5_web_ide, tutorial:technology/odata ]
---

## Prerequisites  
 - **Proficiency:** Intermediate
 - **Tutorials:** While not required, it would be useful to complete the [An Open Data Protocol (OData) primer for developers](http://go.sap.com/developer/tutorials/hcp-webide-odata-primer.html) and be familiar with SAP Web IDE before beginning this tutorial.

## Next Steps
 - [Build an SAPUI5 app based on your data model and run it with mock data](http://go.sap.com/developer/tutorials/hcp-webide-build-app-mock-data.html)

## Details
### You will learn  
In most cases, a live OData service will be available when building an application. For times when a service is not available, it is still possible to build apps with SAP Web IDE with a file-based data model and then run on simulated data (referred to as “mock data” in SAP Web IDE). Once the data service is available, the app can then be configured to run against the service rather than the mock data with no other changes. The mock data approach is also useful if you want to prototype an app and have realistic data appear in the UI.


Additionally, the full data model is provided at the bottom of this document if you want to refer to it later for use in your projects.
:--------------   | :-------------
XML declaration   | Not necessary for `.edmx` files, but useful to include if you want to view the file in an editor that supports XML syntax highlighting
`Edmx` and `DataServices` elements | The “wrapper” for your data model
`Schema`          | Container for the `EntityTypes`, `Associations` and `EntityContainer` elements

### Time to Complete
**20 Min**.

### Part 1

1. Open Web IDE, select the Local folder and create a new folder called `Metadata`.

    Item          | Description
    :------------ | :-------------

    ```xml
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    ```

    ![wrapper](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part1_3.png)

4. Next, paste the `Schema` element text inside of the `DataServices` element. After you paste in the text, select **Beautify** from the **Edit** menu (**Edit > Beautify**) to clean up the indents (you will want to select Beautify each time you paste in text during this tutorial). It is suggested you then add some blank lines before the `</Schema>` tag to make it easier to insert text in later steps (as shown in the image below).

    Item          | Description
    :------------ | :-------------
    `<Schema...`  | A CSDL document requires that each `edmx:DataServices` element contain one or more `Schema` elements. Here, you will also specify the namespace (Sales) for the service.

    Text to enter:

    ```xml
    <Schema Namespace="SalesModel" xmlns="http://schemas.microsoft.com/ado/2008/09/edm" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata">

    ![schema](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part1_4.png)

5. Before continuing, with the `EntityType`, it will be useful to understand the data items that the service will expose. Imagine that you have completed your Design Thinking sessions with the users and have determined that the app should display the sales order relevant fields below (and their data types):


    Field Name                   | Data Type
    :--------------------------- | :-------------


    Additionally, the Data Architects involved in the project require two additional fields for linking the tables in the back-end:

    Field Name                   | Data Type
    :--------------------------- | :-------------


6. There are sixteen different primitive data types supported in OData ([more information can be found on the `odata.org` website](http://www.odata.org/documentation/odata-version-2-0/overview/)), but you will only need a few to capture the data you need:

    - `String`

7. The next step is to add the `EntityType` element into your document which defines the fields in your data model. Within the `EntityType` element you will include a `Name` attribute which is a simple identifier for the element.

    Copy and paste the text below within the `Schema` element in your document.

    ```xml

    ![Entity Type](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part1_7.png)

8. With the field names and data types known, it is simple to populate the `EntityName` element. When doing this on your own, you will need to make two decisions:

    - Which field to use as a `Key`

    The `Key` is used as the default sorting key the service will use when returning the data, and the field length for strings is the maximum number of characters allowed. For this tutorial, these items have been taken care of for you (`SalesOrderKey` and the string lengths vary by field).

    Copy and paste the following within the `SalesOrder` `EntityType` elements.

    ```xml
    ```

9. The last element to add is the `EntityContainer` which will expose the `SalesOrder` model as the `SalesOrders` collection. Copy and paste the following between the `</EntityType>` tag and the `</Schema>` tag.

    ```xml

    ![EntityContainer](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part1_9.png)


Your basic OData model is now complete and you could use it to build a basic app with the SAP Web IDE template wizard. If you would like to do this before continuing, you can jump to the next tutorial now.


### Part 2: Adding a second collection and Navigation Property support

Most OData services you will use will have collections that are linked to one or more other related collections through a `NavigationProperty` element. In this section, you will add a second collection and the elements to enable the navigation. The table and graphic below describe the changes you make and provide a graphical view of the part 2 OData model.
:------------------------- | :-------------


    Attribute                  | Purpose
    :------------------------- | :-------------

    Text to enter:

    ```xml

    ![NavigationProperty](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_3.png)


    Field Name                   | Data Type
    :--------------------------- | :-------------

5. To add the `EntityType` for `BusinessPartner`, copy and paste the following text immediately below the `SalesOrder` `</EntitySet>` closing tag:

    Text to enter:

    ```xml

    ![BusinessPartner EntityType](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_5.png)

6. The next addition is the `Association` element which (as the name implies) represents an association in an entity model. The `Association` element must contain a `Name` and two `End` elements – and can also contain referential constraints. The attributes you will use are explained below.

    Attribute                  | Purpose
    :------------------------- | :-------------

    Copy and paste the following text immediately after the `BusinessPartner` `</EntityType>` closing tag.

    ```xml

    ![Association](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_6.png)

7. The last step is to modify the `EntityContainer` element to add the `BusinessPartners` collection as well an the association between the two collections.

    Copy and paste the following text after the `SalesOrders` `<EntitySet>` element within the `EntityContainer` element.

    ```xml

    ![EntitySet](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_7.png)

8. Your model is now complete. When working on your own OData model, you can take advantage of the [OData Model Editor](https://help.hana.ondemand.com/webide_odatamodeler/frameset.htm?e5c9289506a7493189948b55c69097db.html) (which is a plug-in to SAP Web IDE). To enable it, select **Tools > Preferences** in Web IDE, click on **Plugins**, locate the **OData Model Editor** plugin, click the slider to "on", then **Save** and Web IDE will reload.

    ![Enable OData modeler](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_8.png)

    ![Design tab](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_9.png)

    ![Fit to screen](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_10.png)

    ![View member attributes](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_11.png)

    ![Valid model](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_12.png)

13. The validation happens during editing, so you can see the effect of changes right away. To test this out, simulate a typographic error by inserting a character into the first `EntitySet` line (change `SalesModel` to `SalesModels`). The validator points out the error right away.

    ![Validator](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_13.png)

14. Remove the extra character to remove the error.

    ![fixed typo](https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/tutorials/hcp-webide-create-odata-model/mob4-1_part2_14.png)

15. If you want to modify the entity model used in this tutorial for your own projects, the full entity model text is provided below. Just copy and paste it into an `.edmx` file in Web IDE and modify the Name, Type fields and others as needed.

    ```xml

### Optional
 - Read up on the [OData Model Editor](https://help.hana.ondemand.com/webide_odatamodeler/frameset.htm)

## Next Steps
 - [Build an SAPUI5 app based on your data model and run it with mock data](http://go.sap.com/developer/tutorials/hcp-webide-build-app-mock-data.html)
# Authoring Azure Cosmos DB Stored Procedures for Multi-Document Transactions

In this lab, you will author and execute multiple stored procedures within your Azure Cosmos DB instance. You will explore features unique to JavaScript stored procedures such as throwing errors for transaction rollback, logging using the JavaScript console and implementing a continuation model within a bounded execution environment.

> If this is your first lab and you have not already completed the setup for the lab content see the instructions for [Account Setup](00-account_setup.md) before starting this lab.

## Author Simple Stored Procedures

You will get started in this lab by authoring simple stored procedures that implement common server-side tasks such as adding one or more items as part of a database transaction.

> **Note** During the following steps, if you are not able to edit a query or stored procedure, move away from the **Data Explorer** window and then re-open it.

### Create Simple Stored Procedure

1. In the **Azure Cosmos DB** blade in the Azure Portal, locate and select the **Data Explorer** link on the left side of the blade.

1. In the **Data Explorer** section, expand the **NutritionDatabase** database node and then expand the **FoodCollection** container node.

1. Within the **FoodCollection** node, select the **Items** link.

1. Select the **New Stored Procedure** button (two gears icon) at the top of the **Data Explorer** section.

    ![The New Stored Procedure menu item is highlighted](../media/06-new_storedprocedure.jpg "Create a new Stored Procedure")

1. In the stored procedure tab, locate the **Stored Procedure Id** field and enter the value: **greetCaller**.

1. Replace the contents of the stored procedure editor textarea with the following JavaScript code:

    ```js
    function greetCaller(name) {
        var context = getContext();
        var response = context.getResponse();
        response.setBody("Hello " + name);
    }
    ```

    ![A new stored procedure called greetCaller is displayed](../media/06-new_greet_caller_sp.jpg "Create a new stored procedure")

    > This simple stored procedure will echo the input parameter string with the text `Hello` as a prefix.

1. Select the **Save** button at the top of the tab.

1. Select the **Execute** button at the top of the tab.

1. In the **Input parameters** popup that appears, perform the following actions:

    - In the **Partition key value** section, use Type **String** and enter the value: `example`.

    - If there are no param fields listed, select the **Add New Param** button.

    - In the param field, use Type **String** and enter the value: `Person`.

    - Select the **Execute** button.

        ![The stored procedure parameters are populated](../media/06-execute_sp.jpg "Execute the stored procedure")

1. In the **Result** pane at the bottom of the tab, observe the results of the stored procedure's execution.

    > The output should be `"Hello Person"`.

### Create Stored Procedure with Nested Callbacks

All Azure Cosmos DB operations within a stored procedure are asynchronous and depend on JavaScript function callbacks. A **callback function** is a JavaScript function that is used as a parameter to another JavaScript function. In the context of Azure Cosmos DB, the callback function has two parameters, one for the error object in case the operation fails, and one for the created object.

1. Select the **New Stored Procedure** button at the top of the **Data Explorer** section.

1. In the stored procedure tab, locate the **Stored Procedure Id** field and enter the value: **createDocument**.

1. Replace the contents of the stored procedure editor textarea with the following JavaScript code:

    ```js
    function createDocument(doc) {
        var context = getContext();
        var container = context.getCollection();
        var accepted = container.createDocument(
            container.getSelfLink(),
            doc,
            function (err, newItem) {
                if (err) throw new Error('Error' + err.message);
                context.getResponse().setBody(newItem);
            }
        );
        if (!accepted) return;
    }
    ```

1. Review the stored procedures code. Notice inside the JavaScript callback, users can either handle the exception or throw an error. In case a callback is not provided and there is an error, the Azure Cosmos DB runtime throws an error. This stored procedures creates a new item and uses a nested callback function to return the item as the body of the response.

1. Select the **Save** button at the top of the tab.

1. Select the **Execute** button at the top of the tab.

1. In the **Input parameters** popup that appears, perform the following actions:

    - In the **Partition key value** section, use Type **String** and enter the value: `My Recipes`.

    - If there are no param fields listed, select the **Add New Param** button.

    - In the param field, use Type **String** and enter the value:

        ```json
        { "foodGroup": "My Recipes", "description": "Cookies" }
        ```

    - Select the **Execute** button.

1. In the **Result** pane at the bottom of the tab, observe the results of the stored procedure's execution.

    ![The item created from the stored procedure is displayed](../media/06-execute_sp_02.jpg "review the item created")

    > You should see a new item in your container. Azure Cosmos DB has assigned additional fields to the item such as `id` and `_etag`.

1. Select the **New SQL Query** button at the top of the **Data Explorer** section.

1. In the query tab, replace the contents of the *query editor* with the following SQL query:

    ```sql
    SELECT * FROM foods WHERE foods.foodGroup = "My Recipes" AND foods.description = "Cookies"
    ```

    > This query will retrieve the item you have just created.

1. Select the **Execute Query** button in the query tab to run the query.

1. In the **Results** pane, observe the results of your query.

1. Close the **Query** tab.

### Create Stored Procedure with Logging

1. Select the **New Stored Procedure** button at the top of the **Data Explorer** section.

1. In the stored procedure tab, locate the **Stored Procedure Id** field and enter the value: **createDocumentWithLogging**.

1. Replace the contents of the stored procedure editor with the following JavaScript code:

    ```js
    function createDocumentWithLogging(doc) {
        console.log("procedural-start");
        var context = getContext();
        var container = context.getCollection();
        console.log("metadata-retrieved");
        var accepted = container.createDocument(
            container.getSelfLink(),
            doc,
            function (err, newDoc) {
                console.log("callback-started");
                if (err) throw new Error('Error' + err.message);
                context.getResponse().setBody(newDoc.id);
            }
        );
        console.log("async-doc-creation-started");
        if (!accepted) return;
        console.log("procedural-end");
    }
    ```

    > This stored procedure will use the **console.log** feature that's normally used in browser-based JavaScript to write output to the console. In the context of Azure Cosmos DB, this feature can be used to capture diagnostics logging information that can be returned after the stored procedure is executed.

1. Select the **Save** button at the top of the tab.

1. Select the **Execute** button at the top of the tab.

1. In the **Input parameters** popup that appears, perform the following actions:

    - In the **Partition key value** section, use Type **String** and enter the value: ``My Recipes``.

    - Select the **Add New Param** button.

    - In the new field that appears, enter the value:

        ```json
        { "foodGroup": "My Recipes", "description": "Cookies" }
        ```

    - Select the **Execute** button.

1. In the **Result** pane at the bottom of the tab, observe the results of the stored procedure's execution.

    > You should see the unique id of a new item in your container.

1. Select the `console.log` link in the **Result** pane to view the log data for your stored procedure execution.

    > You can see that the procedural components of the stored procedure finished first and then the callback function was executed once the item was created. This can help you understand the asynchronous nature of JavaScript callbacks.

### Create Stored Procedure with Error Handling

1. Select the **New Stored Procedure** button at the top of the **Data Explorer** section.

1. In the stored procedure tab, locate the **Stored Procedure Id** field and enter the value: **createTwoDocuments**.

1. Replace the contents of the stored procedure editor with the following JavaScript code:

    ```js
    function createTwoDocuments(foodGroupName, foodDescription, mealName) {
        var context = getContext();
        var container = context.getCollection();
        var firstItem = {
            foodGroup: foodGroupName,
            description: foodDescription
        };
        var secondItem = {
            foodGroup: foodGroupName,
            eaten: {
                meal: mealName
            }
        };
        var firstAccepted = container.createDocument(container.getSelfLink(), firstItem,
            function (firstError, newFirstItem) {
                if (firstError) throw new Error('Error' + firstError.message);
                var secondAccepted = container.createDocument(container.getSelfLink(), secondItem,
                    function (secondError, newSecondItem) {
                        if (secondError) throw new Error('Error' + secondError.message);
                        context.getResponse().setBody({
                            foodRecord: newFirstItem,
                            mealRecord: newSecondItem
                        });
                    }
                );
                if (!secondAccepted) return;
            }
        );
        if (!firstAccepted) return;
    }
    ```

    > This stored procedure uses nested callbacks to create two separate items. You may have scenarios where your data is split across multiple JSON documents and you will need to add or modify multiple items in a single stored procedure.

1. Select the **Save** button at the top of the tab.

1. Select the **Execute** button at the top of the tab.

1. In the **Input parameters** popup that appears, perform the following actions:

    - In the **Partition key value** section, use Type **String** and enter the value: `Vitamins`.

    - Select the **Add New Param** button two times.

    - In the first field that appears, enter the value: `Vitamins`.

    - In the second field that appears, enter the value: `Calcium`.

    - In the third field that appears, enter the value: `Breakfast`.

    - Select the **Execute** button.

1. In the **Result** pane at the bottom of the tab, observe the results of the stored procedure's execution.

    > You should see new items in your container. Azure Cosmos DB has assigned additional fields to the items such as `id` and `_etag`.

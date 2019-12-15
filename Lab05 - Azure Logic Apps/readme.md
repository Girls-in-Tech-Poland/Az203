# Lab05 - Azure Logic Apps

| Azure Logic Appps | 
| ----------------------------------------------- | 
| ![app-service-plan](./img/logic-apps.svg) | 

This lab aims to provide a hands-on experience in
deploying applications to Azure App Services running on both Linux and Windows operating systems.

### Table of Contents

The exercise is divided into the following parts:

| Part no. | Title                                                                           | Expected Duration 
| -------- | ------------------------------------------------------------------------------- | ----------------- 
| 1        | Creating web services using Logic Apps                                          | 10m               
| 2        | Parametrizing Logic Apps                                                        | 10m               
| 3        | Monitoring Logic Apps                                                           | 7m               
| 4        | Logic Apps built-in versioning                                                  | 3m               
| 5        | Integration with Blob Storage                                                   | 15m               

## Scenario 1 - Creating web services using Logic Apps

1. Ceate **New Resource**.  
![image](./img/000002.png)
2. Search for **Logic Apps**.  
![image](./img/000003.png)
3. Click **Create**.  
![image](./img/000004.png)
4. Fill the form.  
![image](./img/000005.png)
   - **Name**: Http-Request-App
   - **Subscription**: leave default value
   - **Resource group**: Choose lab05 resource group
   - **Region**: North Europe
5. Click **Create**. 
6. Open created logic app.
   1. Either by opening from notification center by clicking **Go to resource**, or  
   ![image](./img/000006.png)
   1. Go to lab05 resource group and selecting your logic app  
   ![image](./img/000007.png)
7. Select **blank logic app** template.  
![image](./img/000008.png)
8. Search for **Request** trigger and select it.  
![image](./img/000009.png)
9. Choose **When a HTTP request is received** trigger.  
![image](./img/000010.png)
10. Change **method** to **GET**.  
![image](./img/000011.png)  
![image](./img/000012.png)
11. Add **New Step**.  
![image](./img/000013.png)
12. Choose **Request** from recent.  
![image](./img/000014.png)
13. Select **Response** action.   
![image](./img/000015.png)
14. **Save** the logic app.  
![image](./img/000016.png)
15. **Run** the logic app.   
![image](./img/000017.png).
16. Result should look as presented.  
![image](./img/000018.png)
17. Go back to **Designer**.  
![image](./img/000019.png)
18. Add **Hello world!** to response body.  
![image](./img/000020.png)
19. **Save** the logic app.  
![image](./img/000016.png)
20. Copy request URL from the trigger step.  
![image](./img/000021.png)
21. Paste it in new browser URL window.  
![image](./img/000022.png)

### Scenario 1 summary

> Logic Apps are amazing ways to visually create business workflows as well as cross-service integrations. They are in fact called **Enterprise Integration Service**. It's basically coding using visual building blocks.

> In this example you have built a simple web service which takes no parameters and returns simple 

## Scenario 2 - Parametrizing Logic Apps

1. Go back to **Designer**.
2. Add **relative path parameter**.  
![image](./img/000034.png)
2. Set the value of relative path to **/{name}**.  
![image](./img/000035.png)
2. Save the logic app.  
![image](./img/000016.png)
    > New logic app dynamic parameter is now available to use after saving the logic app. 
2. Replace text **Hello world!** in the output by removing **world** word and while having cursor in the place of that word clicking on **name parameter**.  
![image](./img/000037.png)
2. Paramterized output should look like this.  
![image](./img/000038.png)
2. Save the logic app.  
![image](./img/000016.png)
2. Copy newly generated URL and paste it in the browser.  
![image](./img/000036.png)
    > URL changed because we added parameters which affect how URL looks like.
2. Replace **{name}** (with the brackets) with your name and press enter.  
![image](./img/000039.png)

## Scenario 3 - Monitoring Logic Apps

1. Go to logic app oerview page.  
![image](./img/000023.png)
2. And hit **Refresh** button.  
![image](./img/000027.png)
3. Review Logic App **run history** and click on the most recent run.  
![image](./img/000028.png)
4. Check the successful run and **click** on the **Request**.  
![image](./img/000029.png)
5. Review input and output values.  
![image](./img/000040.png)
6. Review input and output values for **Response**.  
![image](./img/000041.png)

> Logic Apps montoring is one of the its best features. You can review every single step, check inputs and output for each step. This feature also very easily allows for debugging.

1. Click resubmit run to run logic app using the same parameters.  
![image](./img/000031.png)  
![image](./img/000032.png)  
![image](./img/000033.png)
2. Check results of resubmitted run.
3. Review logic app results.

> With Resubmit feature it is also very easy to rerun failed runs or test your logic apps. They will be resubmitted using the same input as the run you selected.

## Scenario 4 - Logic Apps built-in versioning

1. Go to **Versions** blade.  
![image](./img/000024.png)
3. Select any version to to review previous logic app versions.  
![image](./img/000025.png)
2. Go back to **Overview** blade.  
![image](./img/000026.png)

> When developing logic apps Azure saves by default every version. Unfortunately there is currently no out of the box integration with git but you can later do full CI/CD with Azure DevOps using ARM templates.

## Scenario 5 - Integration with Blob Storage

1. Run the **Azure CLI** to create new storage account. Replace values with your name and resource group.
   > Name for storage account must be globally unique so make sure it's not common name.
   ```bash
    az storage account create \
        --name <name>  \
        --resource-group <resource_group_name>  \
        --location northeurope  \
        --sku Standard_LRS  \
        --kind StorageV2
    ```
    or with one liner
    ```bash
    az storage account create --name <name> --resource-group <resource_group_name> --location northeurope --sku Standard_LRS --kind StorageV2
    ```
2. Create Storage Container for data
    ```bash
    az storage container create --name data --account-name <name>
    ```
3. Go to Logic App **Designer**.  
4. **Add an action** between steps.  
![image](./img/000042.png)
1. Search for **blob** and select **Azure Blob Storage** connector.  
![image](./img/000043.png)
1. Select **Create blob** action.  
![image](./img/000044.png)
1. Create new connection.  
    1. Pick any name you want.
    2. .Select your storage account and hit **Create**.  
    ![image](./img/000045.png)
1. In **Folder name*8 field select folder picker.  
![image](./img/000046.png)
1. Choose data container.  
![image](./img/000047.png)
1. Click in **Blob name** file, select **Expression** in the popup and paste in the expression and clikc **OK**.  
    ```json
    concat('request-',guid(),'.txt')
    ```
    ![image](./img/000049.png)
1. In **Blob content** click on the text field and select **See more** in the popup.  
![image](./img/000050.png)
1. Select **name** parameter.  
![image](./img/000051.png)
1. Blob connector setting should look like this.
![image](./img/000052.png)
3. Save the logic app.  
![image](./img/000016.png)

### The logic app

1. Grab the URL of logic app from the trigger element. 
![image](./img/000021.png)
1. Test it in the browser using several names of your choice.  
![image](./img/000053.png)
1. Go to **resource group**.  
![image](./img/000054.png)
1. Open storage account. In case you don't see it hit **Refresh** button on the top.  
![image](./img/000055.png)
1. Go to **containers**.  
![image](./img/000056.png)
1. Open **data** container.  
![image](./img/000057.png)
1. Validate multiple files are in the storage and click on one of them.  
![image](./img/000059.png)
1. Hit **Edit** tab and validate contents. You should see name passed in the URL.  
![image](./img/000060.png)

> You have successfully created a web service which accepts parameters, connects to blob storage and saves data permanently and responds using those parameters. Now take advantage of those 200+ connectors.
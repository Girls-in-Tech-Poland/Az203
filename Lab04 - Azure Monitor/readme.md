# Lab04 - Azure Azure Monitor

| Azure Cosmos DB                         |
| --------------------------------------- |
| ![app-service-plan](./img/Monitor.svg)  |

| Estimated time |
| -------------- |
| 75 minutes     |

## Lab scenario

You have created an API for your next big startup venture that needs to get to market quickly. Even though you want to get to market quickly, you have witnessed other ventures fail when they don’t plan for growth and have too few resources or too many users. To plan for this, you have decided to take advantage of the scale-out features of Microsoft Azure App Service, the telemetry features of Application Insights, and the performance-testing features of Azure DevOps. In this project, you will deploy an API to the App Service by using API Apps, capture telemetry and metrics by using Application Insights, and implement a smart client that can handle network issues or other transient faults. You will then load test the API by using Azure DevOps.


# Exercise 1: Create and configure Azure resources

#### Task 1: Open the Azure portal

1.  On the taskbar, select the **Microsoft Edge** icon.

1.  In the open browser window, navigate to the [**Azure portal**](https://portal.azure.com) (portal.azure.com).

1.  At the sign-in page, enter the **email address** for your Microsoft account.

1.  Select **Next**.

1.  Enter the **password** for your Microsoft account.

1.  Select **Sign in**.

    > **Note**: If this is your first time signing in to the **Azure portal**, a dialog box will display offering a tour of the portal. Select **Get Started** to skip the tour and begin using the portal.

#### Task 2: Create an Application Insights resource

1.  Create a new **Application Insights account** with the following details:
    
      - **New resource group**: MonitoredAssets
    
      - **Name**: instrm\[your name in lowercase\]
    
      - **Location**: West Europe

    > **Note**: Wait for Azure to finish creating the storage account before you move forward with the lab. You will receive a notification when the account is created.

1.  Access the **Properties** section of the **Application Insights** blade.

1.  Observe the value of the **Instrumentation Key** field. This key is used by client applications to connect to Application Insights.

#### Task 3: Create an Web App resource

1.  Create a new **Web App** with the following details:

    - **Existing resource group**: MonitoredAssets
    
    - **Web App name**: smpapi\[your name in lowercase\]

    - **Publish**: Code

    - **Runtime stack**: .NET Core 3.0

    - **Operating System**: Windows

    - **Region**: West Europe

    - **New App Service plan**: MonitoredPlan
    
    - **Sku and size**: Standard (S1)

    - **Application Insights**: Enabled

    - **Existing Application Insights resource**: instrm\[your name in lowercase\]

    > **Note**: Wait for Azure to finish creating the Web App before you move forward with the lab. You will receive a notification when the app is created.

1.  Access the **smpapi\*** *Web App* that you created earlier in this lab.

1.  In the **Settings** section on the left side of the blade, navigate to the **Configuration** section.

1.  Locate and access the **Application Settings** tab within the **Configuration** section.

1.  Observe the value corresponding to the **APPINSIGHTS\_INSTRUMENTATIONKEY** Application Settings key. This value was set automatically when you built your Web App resource.

1.  Access the **Properties** section of the **App Service** blade.

1.  Record the value of the **URL** field. You will use this value later in the lab to make requests against the API.

#### Task 4: Configure Web App auto-scale options

1.  Go to the **Scale out** section of the **App Services** blade.

1.  In the **Scale out** section, enable **Custom autoscale** with the following details:
    
    1.  **Name**: ComputeScaler
    
    1.  **Scale mode** section, select **Scale based on a metric**
    
    1.  **Minimum instances**: 2
    
    1.  **Maximum instances**: 8
    
    1.  **Default instances**: 3
    
    1.  **Scale rules**: Single scale-out rule with default values

1.  **Save** your changes to the **autoscale** configuration.

#### Review

In this exercise, you created the resources that you will use for the remainder of the lab.

# Exercise 2: Build and deploy an .NET Core Web API application

#### Task 1: Build a .NET Core Web API project

1.  Open **Visual Studio Code**.

1.  In **Visual Studio Code**, open the **Allfiles (F):\\Allfiles\\Labs\\05\\Starter\\Api** folder.

1.  Use the **Explorer** to open a new terminal that has the context set to the current working directory.

1. In the command promt check your installed .NET Core SDKs:

    ```
    dotnet --list-sdks
    ```
    > Note
    > 
    > Note the highest installed SKD from the **2.X.X** family

1. In the command promt create a new global.json file:

    ```
    dotnet new globaljson
    ```

1. Edit the newly created **global.json** file using Visual Studio Code by typing in your command line:

    ```
    code global.json
    ```

1. You will see a json file similar to this:

    ```json
    {
        "sdk": {
        "version": "X.Y.Z"
        }
    }
    ```

1. Change the **X.Y.Z** value to the value of the SDK you have noted previously

1.  In the command prompt, create a new .NET Core Web API application named **SimpleApi** in the current directory:

    ```
    dotnet new webapi --output . --name SimpleApi
    ```

1.  Add the **2.7.1** version of the **Microsoft.ApplicationInsights.AspNetCore** package from NuGet to the current project:

    ```
    dotnet add package Microsoft.ApplicationInsights.AspNetCore --version 2.7.1
    ```

1.  Build the .NET Core web application:

    ```
    dotnet build
    ```

#### Task 2: Update application code to disable HTTPS and use Application Insights

1.  Use the **Explorer** in **Visual Studio Code** to open the **Startup.cs** file in the editor.

1.  Locate and delete the following line of code at line **43**:

    ```
    app.UseHttpsRedirection();
    ```

    > **Note**: This line of code forces the Web App to use HTTPS. For this lab, this is unnecessary.

1.  Within the **Startup** class, add a new **static string constant** named **INSTRUMENTATION_KEY** with its value set to the **Instrumentation Key** you copied from the **Application Insights** resource you created earlier in this lab:

    ```
    private static string INSTRUMENTATION_KEY = "{your_instrumentation_key}";
    ```

    > **Note**: For example, if you **Instrumentation Key** is ``d2bb0eed-1342-4394-9b0c-8a56d21aaa43``, your line of code would be ``private static string INSTRUMENTATION_KEY = "d2bb0eed-1342-4394-9b0c-8a56d21aaa43";``

1.  Add a new line of code within the **ConfigureServices** method to configure Application Insights using the provided instrumentation key:

    ```
    services.AddApplicationInsightsTelemetry(INSTRUMENTATION_KEY);
    ```

1.  **Save** the **Startup.cs** file.

1.  Use the **Explorer** to open a new terminal, if it is not still already open, that context is set to the current working directory.

1.  Build the .NET Core web application:

    ```
    dotnet build
    ```

#### Task 3: Test an API application locally

1.  Use the **Explorer** to open a new terminal, if it is not still already open, that has the context set to the current working directory.

1.  Execute the .NET Core web application.

    ```
    dotnet run
    ```

1.  Open the **Microsoft Edge** browser.

1.  In the open browser window, navigate to the **/api/values** relative path of your test application hosted at **localhost** on port **5000**.
    
    **Note**: The full URL is <http://localhost:5000/api/values>.

1.  In the same browser window, navigate to the **/api/values/7** relative path of your test application hosted at **localhost** on port **5000**.
    
    **Note**: The full URL is <http://localhost:5000/api/values/7>

1.  Close the browser window that you recently opened.

1.  Close the currently running **Visual Studio Code** application.

#### Task 4: View metrics in Application Insights

1.  Return to your currently open browser window displaying the **Azure portal**.

1.  Access the **instrm\*** Application Insights account that you created earlier in this lab.

1.  In the **Application Insights** blade, observe the metrics displayed in the tiles located in the center of the blade. Specifically, observe the number of **server requests** that have occurred and the average **server response time**.

    > **Note**: It can take up to five minutes for the requests to show within the Application Insights metrics charts.

#### Task 5: Deploy an application to Web App

1.  Open **Visual Studio Code**.

1.  In **Visual Studio Code**, open the **Allfiles (F):\\Allfiles\\Labs\\05\\Starter\\Api** folder.

1.  Use the **Explorer** to open a new terminal that has the context set to the current working directory.

1.  Sign in to the Azure CLI by using your Microsoft Azure credentials:

    ```
    az login
    ```

1.  List all the **apps** in your **MonitoredAssets** resource group:
    
    ```
    az webapp list --resource-group MonitoredAssets
    ```

1.  Find the **apps** that have the prefix **smpapi\***:
    
    ```
    az webapp list --resource-group MonitoredAssets --query "[?starts_with(name, 'smpapi')]"
    ```

1.  Print only the name of the single app that has the prefix **smpapi\***:

    ```
    az webapp list --resource-group MonitoredAssets --query "[?starts_with(name, 'smpapi')].{Name:name}" --output tsv
    ```

1.  Change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\05\\Starter** directory that contains the lab files:

    ```
    cd F:\Labfiles\05\Starter\
    ```
1.  Deploy the **api.zip** file to the **Web App** that you created earlier in this lab:

    ```
    az webapp deployment source config-zip --resource-group MonitoredAssets --src api.zip --name <name-of-your-api-app>
    ```

    > **Note**: Replace the **\<name-of-your-api-app\>** placeholder with the name of the Web App that you created earlier in this lab. You recently queried this app’s name in the previous steps.

1. Access the **smpapi\*** Web App that you created earlier in this lab.

1. Open the **smpapi\*** Web App in your browser.

1. Perform a **GET** request to the **/api/values/** relative path of the website and observe the JSON array that is returned as a result of using the API.

    > **Note**: For example, if your URL is https://smpapistudent.azurewebsites.net, the new URL would be https://smpapistudent.azurewebsites.net/api/values.

#### Review

In this exercise, you created an API by using ASP.NET Core and configured it to stream application metrics to Application Insights. You then used the Application Insights dashboard to view performance details about your API.

### Exercise 3: Build a client application by using .NET Core

#### Task 1: Build a .NET Core console project

1.  Open **Visual Studio Code**.

1.  In **Visual Studio Code**, open the **Allfiles (F):\\Allfiles\\Labs\\05\\Starter\\Console** folder.

1.  Use the **Explorer** to open a new terminal that has the context set to the current working directory.

1.  In the command prompt, create a new .NET Core console application named **SimpleConsole** in the current directory:

    ```
    dotnet new console --output . --name SimpleConsole
    ```

1.  Add the **7.1.0** version of the **Polly** package from NuGet to the current project:

    ```
    dotnet add package Polly --version 7.1.0
    ```

1.  Build the .NET Core web application:

    ```
    dotnet build
    ```

#### Task 2: Add HTTP client code

1.  Use the **Explorer** in **Visual Studio Code** to open the **Program.cs** file in the editor.

1.  Add **using** directives to the top of the file for the following namespaces:
    
      - **System.Net.Http**
    
      - **System.Threading.Tasks**


    ```
    using System.Net.Http;
    using System.Threading.Tasks;
    ```

1.  Locate the **Program** class at line **7**:


    ```
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
    ```

1.  Replace the entire **Program** class with the following implementation:


    ```
    class Program
    {
        private const string _api = "";
        private static HttpClient _client = new HttpClient(){ BaseAddress = new Uri(_api) };
    
        static void Main(string[] args)
        {
            Run().Wait();
        }
    
        static async Task Run()
        {
    
        }
    }
    ```

1.  Locate the **\_api** constant at line **9**:


    ```
    private const string _api = "";
    ```

1.  Update the **\_api** constant by setting the value of the variable to the **URL** of the Web App that you recorded earlier in this lab:

> **Note**: For example, if your URL is http://smpapistudent.azurewebsites.net, the new line of code will be: private const string \_api = "http://smpapistudent.azurewebsites.net";

1.  Within the **Run** method, add the following two lines of code to asynchronously invoke the **HttpClient.GetStringAsync** method passing in a string for the relative path of **/api/values/**, and then write out the response:


    ```
    string response = await _client.GetStringAsync("/api/values/");
    Console.WriteLine(response);
    ```

1.  **Save** the **Program.cs** file.

#### Task 3: Test a console application locally

1.  Use the **Explorer** to open a new terminal, if it is not still already open, that has the context set to the current working directory.

1.  Execute the .NET Core web application.

    ```
    dotnet run
    ```

1.  Observe that the application successfully invokes the Web App in Azure and returns the same JSON array that you observed earlier in this lab:


    ```
    ["value1","value2"]
    ```

1.  Return to your currently open browser window displaying the **Azure portal**.

1.  Access the **smpapi\*** Web App that you created earlier in this lab.

1.  In the **App Service** blade, select **Stop** to halt the execution of the Web App.

1.  Open **Visual Studio Code**.

1.  In **Visual Studio Code**, open the **Allfiles (F):\\Allfiles\\Labs\\05\\Starter\\Console** folder.

1.  Use the **Explorer** to open a new terminal that has the context set to the current working directory.

1. In the command prompt, execute the .NET Core web application.


    ```
    dotnet run
    ```

1. Observe that the application fails and displays a **HttpRequestException** message that is similar to the following exception message:


    ```
    System.Net.Http.HttpRequestException: Response status code does not indicate
    success: 403 (Site Disabled).
       at System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()
       at System.Net.Http.HttpClient.GetStringAsyncCore(Task`1 getTask)
       at SimpleConsole.Program.Run() in F:\Labfiles\05\Starter\Console\Program.cs:line 20
    ```

    > **Note**: This exception occurs because the Web App is no longer available.

#### Task 4: Add retry logic by using Polly

1.  Use the **Explorer** in **Visual Studio Code** to open the **PollyHandler.cs** file in the editor.

1.  Within the **PollyHandler** class, observe lines **13-24**. These lines of code use the **Polly** library from the **.NET Foundation** to create a retry policy that will retry a failed HTTP request every five minutes.

1.  Use the **Explorer** in **Visual Studio Code** to open the **Program.cs** file in the editor.

1.  Locate the **\_client** constant at line **10**:


    ```
    private static HttpClient _client = new HttpClient(){ BaseAddress = new Uri(_api) }; 
    ```

1.  Update the **\_client** constant by updating the **HttpClient** constructor to use a new instance of the **PollyHandler** class:


    ```
    private static HttpClient _client = new HttpClient(new PollyHandler()){ BaseAddress = new Uri(_api) };
    ```

1.  **Save** the **Program.cs** file.

#### Task 5: Validate retry logic

1.  Use the **Explorer** to open a new terminal, if it is not still already open, that has the context set to the current working directory.

1.  Execute the .NET Core web application.


    ```
    dotnet run
    ```

1.  Observe that the HTTP request execution continues to fail and is being re-attempted every five seconds. Leave the application running. It will attempt to access the Web App infinitely until it is successful.

1.  Return to your currently open browser window displaying the **Azure portal**.

1.  Access the **smpapi\*** Web App that you created earlier in this lab.

1.  In the **App Service** blade, select **Start** to resume the Web App.

1.  Return to the currently running **Visual Studio Code** application.

1.  Observe that the application finally successfully invokes the Web App in Azure and returns the same JSON array that you observed earlier in this lab.

1.  Close the currently running **Visual Studio Code** application.

#### Review 

In this exercise, you created a console application to access your API by using conditional retry logic. The application continued to work regardless of whether the API was available.

### Exercise 4: Load a test Web App

#### Task 1: Run a performance test on an Web App

1.  Return to your currently open browser window displaying the **Azure portal**.

1.  Access the **smpapi\*** Web App that you created earlier in this lab.

1.  In the **App Service** blade, select the **Performance test** link.

1.  Create a new **Performance test** by using the following details:
    
      - **Name**: LoadTest
    
      - **Generate Load From**: West Europe
    
      - **User Load**: 1000
    
      - **Duration**: 10
    
      - **Test Type**: Manual Test
    
      - **URL**: http://\<your-api-name\>.azurewebsites.net/api/values

1.  In the **LoadTest** blade, wait for the test to start and complete before proceeding with the lab. Observe the live chart updating as your Web App experiences increased usage.

    > **Note**: Most load tests take about 10 to 15 minutes to gather the resources and start. You can wait at this blade because it will automatically refresh when the load testing is started. The load test will then take the 10 minutes that you specified in the previous steps of this lab.

#### Task 2: Use Azure Monitor metrics after the performance test

1.  Navigate to the **Azure Monitor** service.

1.  In the **Monitor** blade, select the **Metrics** link.

1.  In the **Metrics** section, create a new chart with the following details:
    
      - **Resource**: instrm\* Application Insights account created earlier in this lab
    
      - **Time range**: Last 30 minutes (Automatic)
    
      - **Chart type**: Area chart

1.  Create a new metric with the following details:
    
      - **Metric Namespace**: Standard metrics
    
      - **Metric**: Process CPU
    
      - **Aggregation**: Avg

1.  Create another new metric with the following details:
    
      - **Metric Namespace**: Log-based metrics
    
      - **Metric**: Server response time
    
      - **Aggregation**: Avg

1.  Observe the information displayed in your chart. You can observe how the server response time correlates with the CPU time as load on the application increased.

#### Review

In this exercise, you performed a performance (load) test of your Web App by using the tools available to you in Azure. After you performed the load test, you were able to measure your API app’s behavior by using metrics in the Azure Monitor interface.

### Exercise 5: Clean up subscription 

#### Task 1: Open Cloud Shell

1.  At the top of the Azure portal, select the **Cloud Shell** icon to open a new shell instance.

1.  If the **Cloud Shell** is not already configured, configure the shell for Bash by using the default settings.

1.  At the bottom of the portal in the **Cloud Shell** command prompt, type the following command and press Enter to list all resource groups in the subscription:


    ```
    az group list
    ```

1.  Type the following command and press Enter to view a list of possible commands to delete a resource group:


    ```
    az group delete --help
    ```

#### Task 2: Delete resource groups

1.  Type the following command and press Enter to delete the **MonitoredAssets** resource group:


    ```
    az group delete --name MonitoredAssets --no-wait --yes
    ```
    
1.  Close the **Cloud Shell** pane at the bottom of the portal.

#### Task 3: Close active applications

1.  Close the currently running **Microsoft Edge** application.

1.  Close the currently running **Visual Studio Code** application.

#### Review

In this exercise, you cleaned up your subscription by removing the **resource groups** used in this lab.

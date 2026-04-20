
# Use case 01: Implementing Medallion Architecture with Data Factory in Microsoft Fabric for Scalable Data Processing

## Scenario: Contoso Retail

**Contoso Retail** is a multinational retail and distribution company
specializing in electronics, apparel, and home goods. With a vast
network of stores and a thriving e-commerce platform, Contoso generates
high volumes of transactional and customer data daily.

However, Contoso is facing challenges with:

- Fragmented data across SQL databases and blob storage

- Manual data preparation for reporting

- Limited scalability in analytics workflows

To overcome these, Contoso aimed to modernize its data platform using
Microsoft Fabric’s Medallion Architecture.

You, as a Data Engineer at Contoso Retail, are tasked with implementing
Medallion Architecture in Microsoft Fabric. You collaborated with the
solution architect, BI developers, and governance team to deliver a
unified data pipeline from ingestion to reporting with the following
objectives.

- Ingest raw data from Azure SQL and blob storage into a **Bronze
  Lakehouse.**

- Transform and structure data into a **Silver Lakehouse.**

- Curate business-ready datasets in **Gold Warehouse.**

- Automate orchestration using **Pipelines** and **dynamic
  expressions**.

- Demonstrate end-to-end integration of **Power BI** and **T-SQL** with
  Direct Lake mode.

![A diagram of a house AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image1.png)

# Exercise 1: Azure SQL Mirroring and Bronze Layer Setup

In this exercise, you initiate the Medallion Architecture implementation
by establishing the Bronze layer in Microsoft Fabric. This involves
setting up the foundational components required to ingest data from
Contoso’s retail systems. You configure the workspace, connect to key
data sources, and build pipelines to automate data ingestion and
organization. By the end of this phase, you ensure that raw data is
efficiently captured and structured for downstream processing.

![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image3.svg)

## Task 1: Restoring the WideWorldImporters Sample Database Using SQL Server Management Studio (SSMS)

1. In the Azure portal search bar, search for **Resource groups (1)**, then select **Resource groups (2)** from the results.
   
   ![A screenshot of a computer AI-generated content may be
incorrect.](./media/ex1-0.png)

1. From the **Resource groups** list, select the **Fabric** resource group to open it.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/ex1-1.png)

1. In the **Fabric** resource group, locate and select the **wwi-sqlserver SQL database** to open it.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/ex1-2.png)

1. In the SQL database **Overview**, click the **copy icon** next to the **Server name**, then **paste it into Notepad** for later use.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/ex1-3.png)

1. In the LabVM search bar, type **SSMS (1)** and select **SQL Server Management Studio 22 (2)** to open the application.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/new38.png)

1. In the **Sign in to SQL Server Management Studio** window, click **Sign in with Microsoft** to continue.

   ![](./media/new39.png)

1. Select **Work or school account (1)** and click **Continue (2)** to sign in using your assigned credentials.

   ![](./media/new40.png)

1. On the **Sign in** page, enter the following email/username, and click on **Next (2)**. 

   * **Email/Username**: <inject key="AzureAdUserEmail"></inject> **(1)**
   
     ![Image](./media/latest3.png)
     
1. Now, enter the following Temporary Access Pass and click on **Sign in (2)**.
   
   * **Temporary Access Pass**: <inject key="AzureAdUserPassword"></inject> **(1)**

       ![Image](./media/latest4.png)   

       > **Note:** I may ask you to select the user **<inject key="AzureAdUserEmail"></inject>**.

1. In the sign-in prompts, click **Yes** to enable sign-in to all apps and websites on this device.

   ![Image](./media/latest1.png)

1. On the confirmation screen, click **Done** to complete the account setup and start accessing your organization’s apps and services.  

   ![Image](./media/latest2.png)

1. In the **Connect** window, provide the following details to connect to the SQL Server:

    - **Server name**: Enter the **Server name** of the SQL database that you copied in **Step 4** **(1)**
    - **Authentication**: Select **SQL Server Authentication (2)**
    - **User name**: Enter **sqladminuser (3)**
    - **Password**: Enter the **P@ssw0rd1234! (4)**
    - Check the box for Remember Pasword **(5)**  
    - **Trust Server Certificate**: Check this option **(6)**
    - Click **Connect (7)** to access the SQL Server.

      ![Image](./media/ex1-4.png)

1. In Object Explorer, expand **Databases (1)**, right-click the **wwi-sqlserver (2)** database, and select **Rename (3)**.

    ![Image](./media/ex1-5.png)

1. In Object Explorer, expand **Databases** and select the **WideWorldImporters** database.

    ![Image](./media/ex1-6.png)    

1. Right-click the **WideWorldImporters (1)** database, go to **Tasks (2)**, and select **Import Flat File... (3)** to start importing data.

    ![Image](./media/ex1-7.png)  

1. On the **Introduction** screen of the Import Flat File wizard, click **Next** to continue.

    ![Image](./media/ex1-8.png)

1. Click **Browse…**, select the file to import, and provide a **New table name** to proceed.

    ![Image](./media/ex1-9.png)

1. Navigate to the **C:\LabFiles\lab file\Lab1 (1)** path, select the **Dimension_City (2)** file, and click **Open (3)**.

    ![Image](./media/ex1-10.png)

1. Verify that the file path and table details are correctly populated, then click **Next** to proceed.

    ![Image](./media/new6.png)

1. Review the data preview to confirm the columns and values are correct, then click **Next** to continue.

    ![Image](./media/new7.png)

1. Review and adjust the column names, data types, and constraints if required, then click **Next** to proceed.

    ![Image](./media/new8.png)

1. Review the import summary to confirm the database, table name, and file details are correct, then click **Finish** to start the import process.

    ![Image](./media/new9.png)

1. Verify that the operation status shows **Success** for data insertion, then click **Close** to exit the wizard.

    ![Image](./media/new10.png)      

    > **Congratulations**! You are now connected to the **WideWorldImporters** Database and are ready to import your schema and data.

15. Repeat steps **15 to 23** to upload all the remaining files.

    - Dimension_Customer
    - Dimension_Date
    - Dimension_Employee
    - Dimension_Payment Method
    - Dimension_Supplier
    - Dimension_Transaction Type
    - Fact_Order
    - Fact_Purchase1
    - Fact_Sale
    - Fact_Stock Holding
    - Fact_Transaction

1. Verify all the tables are populated under **WideWorldImporters** database **Tables**.

      ![Image](./media/ex1-11.png)

## Task 2: Configure Azure Storage Account with ContosoSales data

In this task, you create an Azure Storage account with hierarchical
namespace enabled to support scalable, structured storage using Azure
Data Lake Gen2. You set up a container named contososales and upload the
ContosoSales.zip file, establishing a centralized repository for raw
data. This setup forms the foundation of the Bronze layer, enabling
seamless ingestion and transformation processes within the Medallion
Architecture.

1. In the Azure portal search bar, type **Storage accounts (1)** from the results, select **Storage accounts (2)** under Services.

    ![Image](./media/ex1-12.png)
	
1. Click **+ Create** to start creating a new storage resource.

    ![Image](./media/ex1-13.png)

1. On **Create a storage account** page, under the **Basics** tab, enter the following details to create a storage account and then click on **Next (6).**

    | | |
    |---|---|
	| Resource Group | **Fabric (1)** |
    | Storage Account Name | **fabricstorage (2)** |
	| Region | ** (3)** |  
    | Performanace | **Standard (4)** |
    | Redundancy | **Locally-redundant storage (LRS) (5)** |

     ![Image](./media/ex1-14.png)

1. In the **Advanced** tab, check **Enable hierarchical namespace**, keep other settings as default, then click **Review + create**.

    ![Image](./media/ex1-15.png)

1. Review all configuration details, then click **Create** to deploy the storage account.

    ![Image](./media/ex1-16.png)

1. The Azure Storage account is now set up to host data for Azure Data Lake. Click **Go to resource**.

    ![Image](./media/ex1-17.png)

1. On the left-side navigation pane of your Storage Account, expand **Data storage (1)**, select **Containers (2)**, then click **+ Add container (3)** to create a new container.

    ![Image](./media/ex1-18.png)

1. Enter **contososales (1)** as the container name and click **Create (2)**.

    ![Image](./media/ex1-19.png)

1. In the **Containers** section, verify that the **contososales** container has been successfully created.

    ![Image](./media/ex1-20.png)

1. Click **Upload**, then select **Browse for files** to choose files for upload.

    ![Image](./media/ex1-21.png)

1. Navigate to **C:\LabFiles\lab file**, select the **ContosoSales** file, and click **Open** to upload it.

    ![Image](./media/ex1-22.png)

1. Click **Upload** to upload the selected **ContosoSales.zip** file to the container.

    ![Image](./media/ex1-22.png)

1. Verify that the **ContosoSales.zip** file is successfully uploaded and visible in the **contososales** container.

    ![Image](./media/ex1-23.png)

1. Expand **Security + networking**, select **Access keys**, and click **Show** to view the storage account key.

    ![Image](./media/ex1-27.png)

1. Copy the **Storage account name** and **Key1 value** using the copy icons and paste them in the Notepad for later use.

    ![Image](./media/ex1-28.png)	

1. Navigate back to resource groups, from the **Resource groups** list, select the **Fabric** resource group to open it.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/ex1-1.png)

1. In the **Fabric** resource group, locate and select the **SQL server (wwi-sqlserver)** resource.

    ![Image](./media/ex1-29.png)    

1. Under **Security**, open **Identity**, switch the **Status** to **On**, and click **Save** to enable the system-assigned managed identity.
 
    ![Image](./media/ex1-30.png)   

## Task 3: Create a Fabric workspace

In this task, you set up a new Microsoft Fabric workspace to serve as
the central hub for the Medallion Architecture implementation. This
workspace brings together all essential components, including
Lakehouses, Dataflows, Pipelines, Warehouses, Notebooks, and Power BI
assets. By naming it appropriately-such as **Data Factory –
Medallion**-you ensure a well-organized environment for managing the
entire data lifecycle, from raw ingestion to curated analytics.

2.  In the new tab, navigate to the **Microsoft Fabric** portal by copying and pasting the following URL into the address bar.

      ```
      https://app.fabric.microsoft.com
      ```

3. On the **Enter your email, we'll check if you need to create a new account** tab, you will see the login screen, in that enter the following email/username, and click on **Submit (2)**.

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject> **(1)**
 
       ![01](./media/uc1-0.png)
 
4. Next, provide your Temporary Access Password **(1)** and click on **Sign in (2)**:
 
   - **Temprory Access Pass:** <inject key="AzureAdUserPassword"></inject>
 
       ![01](./media/latest3.png)

5. If you see the pop-up Stay Signed in?, select **No**.
   
    ![01](./media/latest6.png)

6. On Microsoft Fabric (Free) license assignment dialog appears, click **OK** to proceed.

    ![01](./media/latest7.png)

7. When the **Welcome to the Fabric view** dialog appears, click **Cancel**.   

    ![01](./media/latest8.png)

1. On the Microsoft **Fabric Home Page**, click on **+ New workspace** to create a new workspace.

    ![01](./media/latest10.png)

1. In the **Create a workspace tab**, enter the following details and
    click on the **Apply (5)** button.
	
    |   |   |
    |----|---|
    |Name	| Enter **Medallion-<inject key="Deployment ID" enableCopy="false"/> (1)**  |
    |Workspace type |	Select **Fabric (2)**, under **Details** you must see **fabric capacity (3)** |
    |Semantic model storage format|	Select **Small semantic model storage format (4)** |

    ![01](./media/ex1-31.png)

    ![01](./media/ex1-32.png)

1. The Workspace is now created.

## Task 4: Implement Azure SQL Mirroring to Bronze layer

In this task, you implement a mirrored Azure SQL Database within your
Fabric workspace by selecting the Medallion task flow template. This
automatically sets up a structured environment that mirrors SQL tables
into the Bronze layer. The process creates a mirrored database,
analytics endpoint, and storage object, ensuring continuous
synchronization of SQL data. This setup helps maintain a reliable raw
data copy without the need for manual exports, streamlining ingestion
for downstream processing.

1. From the empty workspace, select the option **Select a predesigned task flow** to choose one of Microsoft's task flows. These predesigned task flows provide a structured approach to managing data projects.

    ![01](./media/ex1-33.png)

1. From the **Select a predesigned task flow** pane, choose **Medallion** and click **Select** to create the predefined task flow.


    ![01](./media/ex1-34.png)

	> This option includes the description "Organize and improve data progressively as it moves through each layer," which is crucial for data projects. The medallion architecture helps in structuring data into different layers, such as bronze, silver, and gold, to enhance data quality and accessibility.

1. A task flow has now been created within your workspace, which can be considered as an architectural template. This template provides a structured framework for your data project.

    ![01](./media/ex1-35.png)

1. Select the **+ New item** option on the **Bronze data** block to start adding items to task flow.

    ![01](./media/ex1-36.png)

6. Ensure **Assign to task** is set to **Bronze data (1)**, select **All items (2)**, in the search bar search for **Mirrored Azure SQL Database (3)**, and select **Mirrored Azure SQL Database (4)** from the results.

    ![01](./media/ex1-37.png)

1. Select **Azure SQL database** under **New sources** to configure the connection.

    ![01](./media/ex1-38.png)

1. On the **New source** tab, enter the following details, and click on the **Connect (6)** button.

	- **Server:** Enter your SQL server URL that you copied in Task 1 step 4 **wwi-sqlserver-@lab.LabInstance.Id.database.windows.net (1)**

	- **Database:** Enter the database as **WideWorldImporters (2)**

    - **Connection:** Select **Create new connection (3)**

    - **Username**: **sqladminuser (4)** 

    - **Password**: **P@ssw0rd1234! (5)** 

      ![01](./media/ex1-39.png)

1. On the **Choose data** tab, select **Select all (1)** and click on **Connect (2)** button

    ![01](./media/ex1-40.png)

1. On the **Destination** tab, click **Create mirrored database**.

    ![01](./media/ex1-41.png)

1. Verify that the replication **Status** shows **Running**, indicating the mirroring process has started successfully.

    ![01](./media/ex1-42.png)

1. From the **Home** tab, click the **settings** icon and select *About*. Copy the mirrored SQL endpoint name displayed as ***WideWorldImporters*** and paste it into the Notepad. We will use this name in the upcoming tasks.

    ![01](./media/latest11.png)

1. Select SQL endpoint copy the mirrored SQL connection string and paste it into the notebook. We will use this name in the upcoming tasks.

    ![01](./media/latest12.png)   

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Confirm that the **WideWorldImporters** mirrored database and its SQL analytics endpoint are successfully created and mapped under the **Bronze data** stage in the Medallion pipeline.

    ![01](./media/ex1-43.png)

## Task 5: Build the Bronze Lakehouse and Ingest Data Using Copy Job

1. Select the **+ New item** option on the **Bronze data** block to start adding items to task flow.

    ![01](./media/ex1-48.png)

1. Select **Bronze data** as the task, search for **Lakehouse**, and choose the **Lakehouse** item to create a lakehouse in this layer.

    ![01](./media/ex1-49.png)

1. Enter **bronze_Lakehouse (1)** as the name, ensure it is assigned to **Bronze data**, leave **Lakehouse schemas** unchecked, and click **Create**.

    ![01](./media/ex1-50.png)

    ![01](./media/ex1-51.png)

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Within the workspace, you will notice that three items have now been created and are associated with your lakehouse. These items include the lakehouse , SQL analytics endpoint.

    ![01](./media/ex1-52.png)

7. Now proceed to select and add a **+ New item** from the **High-volume data ingest** task.

    ![01](./media/ex1-53.png)

1. Select **High-volume data** in the *Assign to task* dropdown, then choose the **Copy job** option from the available items to start creating a data ingestion process.

    ![01](./media/ex1-54.png)

1. Enter **bronze_copyjob (1)** as the name, ensure it is assigned to **High-volume data ingest (2)**, and click **Create (3)**.

    ![01](./media/ex1-55.png)

1. From the Copy job window, use the Home tab at the top if you want to set additional criteria such as filters or item types. Select **SQL Server database.**

    ![01](./media/ex1-56.png)

1. On the **Connect to data source** tab, enter the following details, and click on the **Next (4)** button.

	- **Server:** Enter the **SQL connection string** for the mirrored
	  database **WideWorldImporters** **(1)** that you copied and pasted in the Notepad in **Task 3 step 12**.

	- **Database:** Enter the database as **WideWorldImporters (2)**

	- **Authentication kind:** **Organizational account (3)**

      ![01](./media/ex1-57.png)

1. Select sqldatabase and click on **Next** button.

    ![01](./media/ex1-58.png)

1. Select **OneLake catalog** and choose the **bronze_Lakehouse** as the data destination.

    ![01](./media/ex1-59.png)

1. Select **Full copy** as the read method, choose **Tables** as the destination root folder, and click **Next**.

    ![01](./media/ex1-60.png)

1. Review the table mappings to ensure source and destination tables are correctly aligned, then click **Next**.

    ![01](./media/ex1-61.png)

1. Review the configuration summary to confirm the source, destination, and number of tables, then click **Save + Run** to start the copy job.

    ![01](./media/ex1-62.png)

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Select the **bronze_Lakehouse** item listed under the **Bronze data** stage in your Medallion workflow.

    ![01](./media/latest13.png)

1. Click the ellipsis (**...**) next to **Tables** in the **bronze_Lakehouse**, then select **Refresh** to reload the metadata and ensure that all newly ingested tables from the copy job are visible in the lakehouse.

    ![01](./media/ex1-64.png)

1. Now review the list of tables

    ![01](./media/ex1-65.png)

## Task 6: Build Dynamic Data Pipelines Using Copy Activity

In this task, you create a pipeline for data ingestion. Using the Copy
Data assistant, you import sample data (Public Holidays.csv) into
Lakehouse. To organize the files efficiently, you enhance the pipeline
with a dynamic expression that creates a date-based folder structure
(yyyy/MM/dd). This setup ensures that data ingestion is automated,
scalable, and well-structured, with validation confirming successful
execution.

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Click **+ New item** in the **High-volume data ingest** stage to add a new data ingestion item to the workflow.

    ![01](./media/ex1-66.png)

1. Ensure **High-volume data ingest** is selected under **Assign to task**, then choose **Pipeline** to create a new data pipeline for orchestrating data ingestion and workflows.

    ![01](./media/ex1-67.png)

1. Enter **samplePipeline** as the pipeline name, confirm it is assigned to **High-volume data ingest**, and click **Create** to create the pipeline.

    ![01](./media/ex1-68.png)

1. Click **Copy data** from the toolbar and select **Add copy data activity** to add a copy activity to the pipeline.

    ![01](./media/ex1-69.png)

1. Go to the **Source** tab, click the **Connection** dropdown, and select **Browse all** to choose the required data source connection.

    ![01](./media/ex1-70.png)

1. Select **Sample data** from the left pane, then choose **Public Holidays** as the data source to proceed.

    ![01](./media/ex1-71.png)

1. Review the preview of the **Public Holidays** dataset and click **OK** to confirm and use it as the source.

    ![01](./media/ex1-72.png)

1. Go to the **Destination** tab, open the **Connection** dropdown, and select **Browse all** to choose the destination connection.

    ![01](./media/ex1-73.png)

1. Select **OneLake catalog** from the left pane, then choose **bronze_Lakehouse** as the destination for the data.

    ![01](./media/ex1-74.png)

1. In the **Destination** settings, select **Files** as the root folder, choose **Parquet** as the file format, and then click **Run** to execute the pipeline.  

    ![01](./media/ex1-75.png)

1. Click **Save and run** to confirm and execute the pipeline with the configured settings.

    ![01](./media/ex1-76.png)

1. The pipeline executed successfully, confirming that the data copy activity completed without any errors.

    ![01](./media/ex1-77.png)

1. Click on **Medallion- (1)** workspace from the left navigation pane and then select **bronze_Lakehouse (2)** to open the lakehouse.

    ![01](./media/ex1-78.png)

1. Click the **three dots (⋯)** on the *Files* section and select **Refresh** to view the newly generated Parquet file.

    ![01](./media/ex1-79.png)

1. The Parquet file opens successfully, displaying the ingested dataset in tabular format within the Lakehouse.

    ![01](./media/ex1-80.png)

1. Under **Files**, click the **three dots (⋯)** next to the Parquet file and select **Delete** to remove it.

    ![01](./media/ex1-81.png)

1. Click **Delete** to permanently remove the file from the lakehouse.

    ![01](./media/ex1-82.png)

1. Click on **Medallion- (1)** workspace from the left navigation pane and then select **samplePipeline (2)** to open the pipeline.

    ![01](./media/ex1-83.png)

1. Click **Validate** to verify that the pipeline configuration, connections, and settings are correct and free of errors.

1. Once validation succeeds, click **Run** to execute the pipeline and start the data movement process.

    ![01](./media/ex1-84.png)

1. Select the **Copy data1 activity**, go to the **Destination** tab, and click **Add dynamic content** to configure the file path dynamically.

    ![01](./media/ex1-85.png)

1. On the Pipeline expression builder page, select the **Functions (1)** tab. Here, you can explore various functions that exist within the expression library. These functions provide powerful tools for creating dynamic expressions. When you're ready copy and paste the code block (2) below into the expression input box. Press **Ok (3)** when complete.

	```
	@formatDateTime(
		convertFromUtc(
			utcnow(), 'Central Standard Time'
			),
		'yyyy/MM/dd'
	)
    ```

    ![01](./media/ex1-86.png)

	**Note:** This expression will be used to create a folder structure
	within your pipeline that writes the file to nested folders based on the
	current year, the current month, and the current date of the run time.
	The forward slash "**/**" character is how the folder structure is
	defined. This dynamic folder structure helps in organizing your data
	based on the date, making it easier to manage and retrieve.

1. From the **Home** tab, select the **Validate** once again.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image115.png)

1. Select the **Run** option to start the pipeline and begin data
    ingestion process. Running the pipeline initiates the data transfer
    from the source to the updated destination folder path.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image116.png)

1. Click **Save and run** to confirm and execute the pipeline with the configured settings.

    ![01](./media/ex1-76.png)

1. The global properties and **Output** view will then become visible. After starting the run of your pipeline, both the **Pipeline status** and the **Activity status** should be **Succeeded**. This indicates that everything ran as intended, confirming that your data ingestion process is successful.

    ![01](./media/ex1-87.png)

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Select the **bronze_Lakehouse** item listed under the **Bronze data** stage in your Medallion workflow.

    ![01](./media/latest13.png)

1. The file has been successfully added to the **Files** section, organized within a nested folder structure based on the **year, month, and date** (**2026 → 04 → 15**) of execution. This confirms that the dynamic file output is functioning correctly and contains the generated Parquet file, confirming successful data organization based on date.

    ![01](./media/ex1-88.png)

	**Note:** If the contents are not yet visible, navigate to the Home tab
	and select the Refresh icon to start the metadata sync process and
	update the lakehouse viewer content.

1. Click the **ellipses (...)** next to the **Year** folder and select **Delete** to remove the folder.

    ![01](./media/ex1-89.png)

1. Click **Delete** to permanently remove the **2026** folder along with all its contents from the lakehouse.

    ![01](./media/ex1-90.png)

## Task 7: Create a Data Pipeline and Establish a Connection to ADLS Gen2 Storage

In this task, you establish a connection between the Bronze Lakehouse
and Azure Blob Storage (ADLS Gen2). A pipeline is created to copy and
unzip the ContosoSales.zip file into the Lakehouse, preserving the
original file hierarchy. Dynamic expressions are used to organize the
files under a date-based folder structure, ensuring scalable and
well-structured ingestion. The result is a clean and accessible Bronze
layer containing raw datasets ready for further processing.

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Click **+ New item** under **High-volume data ingest** to add a new data ingestion component.

    ![01](./media/ex1-91.png)

1. Select **Pipeline** under the **High-volume data ingest** task to create a new data pipeline.

    ![01](./media/ex1-92.png)

1. Enter the pipeline name as **getContosoSample**, ensure it is assigned to **High-volume data ingest**, and click **Create**.

    ![01](./media/ex1-93.png)

1. Select **Pipeline activity** and then choose **Copy data** to start building the data movement pipeline.

    ![01](./media/ex1-94.png)

1. Go to the **Source** tab, open the **Connection** dropdown, and click **Browse all** to select a data source connection.

    ![01](./media/ex1-95.png)

1. Click **New**, search for **Azure Blobs**, and select **Azure Blobs** as the data source.

    ![01](./media/ex1-96.png)

1. On the **Connect to data source** window, enter the details from the table below and select **Connect (6)**.

    |Property	|Value|
    |----|---|    
    |Account name or URL|	Enter your **storage account name (1)** that you pasted in the Notepad in Task 2 Step 15|
    |Connection	|**Create a new connection (2)**|
    |Connection name|	**ContosoSample (3)**|
    |Authentication kind|	**Account key (4)**|
    |Account key|	Enter **storage account key (5)** that you pasted in the Notepad in Task 2 Step 15|

    ![01](./media/ex1-97.png)

1. In the **Source** tab, set the folder path to **contososales**, specify the file **ContosoSales.zip**, and click **Settings** to configure the file format options.

    ![01](./media/ex1-98.png)

1. In **File format settings**, keep **Compression type** as **ZipDeflate (.zip)** and click **OK**.

    ![01](./media/ex1-99.png)

1. In the **Source** settings, expand **Advanced** and enable **Preserve zip file name as folder**.

    ![01](./media/ex1-100.png)

1. Go to the **Destination** tab, open the **Connection** dropdown, and click **Browse all** to select the destination.

    ![01](./media/ex1-101.png)

1. In **OneLake catalog**, select the **bronze_Lakehouse** as the destination.

    ![01](./media/ex1-102.png)

1. In the **Destination** tab, select **Files** as the root folder and click **Add dynamic content** to define the file path dynamically.

    ![01](./media/ex1-103.png)

1. On the Pipeline expression builder page, select the **Functions (1)** tab. Here, you can explore various functions that exist within the expression library. These functions provide powerful tools for creating dynamic expressions. When you're ready copy and paste the **code block (2)** below into the expression input box. Press **Ok (3)** when complete.
	
    ```
    @formatDateTime(
        convertFromUtc(
            utcnow(), 'Central Standard Time'
        ),
        'yyyy/MM/dd'
    )
    ```

    ![01](./media/ex1-104.png)

1. In the **Destination** tab, expand **Advanced** and set **Copy behavior** to **Preserve hierarchy**.

    ![01](./media/ex1-105.png)

1. In the **General** tab, rename the activity to **Get and Unzip files**, then click **Validate** followed by **Run** to execute the pipeline.

    ![01](./media/ex1-106.png)

1. Verify that the pipeline run status is **Succeeded** for the **Get and Unzip files** activity in the **Output** tab.

    ![01](./media/ex1-107.png)

27. The contents of the **zip file** have been successfully added to the **Files** section, organized in a nested folder structure based on the data source title, year, month, and date of the pipeline run.

    ![01](./media/ex1-108.png)

# Exercise 2: Data Movement from Bronze to Silver Layer 

With the Bronze layer in place, you shift your focus to the **Silver**
layer, where the goal is to transform raw data into structured,
analytics-ready tables. This phase emphasizes orchestrating data
movement using Data Factory, leveraging dynamic expressions, variables,
and conditional paths to build flexible pipelines. You set up logic to
handle different data scenarios, ensuring that dimension tables are
refreshed and fact tables are incrementally updated. The structured
movement of data between layers enhances modularity, scalability, and
overall data quality, laying the groundwork for reliable analytics.

![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image154.svg)

## Task 1: Build a Silver Layer Lakehouse and Integrate a New Data Pipeline with SQL Database

In this task, you create a new Lakehouse called silver_Lakehouse to
store structured and cleansed data. A pipeline named createContosoTables
is built to copy tables from the mirrored Azure SQL Database into the
Silver Lakehouse. Dimension and fact tables are ingested into the Files
section, later appearing in the Tables section of the Lakehouse. This
ensures that data is transitioned from raw (Bronze) to structured
(Silver) format, optimized for analytical queries.

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Click **+ New item** under the **Silver data** stage to add a new item.

    ![01](./media/ex2-0.png)

1. Select the **Silver data** task from *Assign to task (1)* and then choose **Lakehouse (2)** to create a new Silver layer storage for processed data.

    ![01](./media/ex2-1.png)

1. Enter **silver_Lakehouse (1)** as the name, ensure it is assigned to the **Silver data (2)** task, optionally review or leave the **Lakehouse schemas (3)** setting as default, and then click **Create (4)** to provision the Silver layer lakehouse for storing transformed and refined data.

    ![01](./media/ex2-2.png)

1. Click **Get data (1)** in the toolbar and select **New copy job (2)** to start creating a data ingestion process into the Silver Lakehouse.

    ![01](./media/ex2-3.png)

1. Enter **createContosoTables1 (1)** as the copy job name, ensure it is assigned to the **Initial process (2)** task to align with the data transformation stage, and then click **Create (3)** to set up the job for loading and structuring data into the Silver layer.

    ![01](./media/ex2-4.png)

1. In the **Choose data source** step of the copy job, select **SQL Server database** to configure a connection and begin extracting data from a relational source for processing into the Silver layer.

    ![01](./media/ex2-5.png)

1. On the **Connect to data source** tab, enter the following details, and click on the **Next (4)** button.

	- **Server:** Enter the **SQL connection string** for the mirrored
	  database **WideWorldImporters** **(1)** that you copied and pasted in the Notepad in **Task 3 step 12**.

	- **Database:** Enter the database as **WideWorldImporters (2)**

	- **Authentication kind:** **Organizational account (3)**

      ![01](./media/ex2-6.png)

1. In the **Choose data** step, select the required database tables by checking them under the source (1) to include them for ingestion, optionally review the preview pane on the right, and then click **Next (2)** to proceed to configuring the destination.

    ![01](./media/ex2-7.png)

1. In the **Choose data destination** step, navigate to the **OneLake catalog (1)**, locate and select the **silver_Lakehouse (2)** as the target destination, ensuring that the ingested data will be stored in the Silver layer for further transformation and refinement before proceeding to the next configuration step.

    ![01](./media/ex2-8.png)

1. In the **Settings** step, choose **Full copy (1)** to load all data in one run, set the **Destination root folder to Tables (2)** so data is written as structured tables in the Silver Lakehouse, and then click **Next (3)** to proceed with mapping and final configuration.

    ![01](./media/ex2-9.png)

1. In the **Map to destination** step, review and verify the table mappings between source and destination to ensure each source table is correctly aligned with its corresponding table in the Silver Lakehouse, make any necessary adjustments if required, and then click **Next** to proceed to the final review and execution stage.

    ![01](./media/ex2-10.png)

1. In the **Review + save** step, review the summary of the source and destination configurations, optionally check **Start data transfer immediately (1)** if you want the copy job to run right away, and then click **Save (2)** to finalize and create the copy job.

    ![01](./media/ex2-11.png) 


1. Click **Run** from the top menu to start executing the copy job, which will initiate the data transfer from the SQL Server source to the **silver_Lakehouse** destination; once triggered, monitor the **Results** section below where each table’s status will transition from *Not started* to *In progress* and finally *Succeeded*, along with details such as rows read, rows written, duration, and execution timestamps to validate that the data has been successfully loaded into the Silver layer.

    ![01](./media/ex2-12.png)  

    ![01](./media/ex1-76.png)

    ![01](./media/ex2-13.png)  

1. Click **Medallion-** in the left pane and then select **silver_Lakehouse** to open the Silver layer for data exploration and further processing.

    ![01](./media/new0.png)

1. Click the **ellipsis (...)** next to the **Tables** section in the Explorer pane to open additional options. Select **Refresh** to reload the table list and ensure all newly created tables from the copy job are visible. 

1. Expand the **Tables** section and review the list of tables (such as **Dimension_City, Dimension_Customer, Dimension_Date, Fact_Order**, etc.), then click any table (for example, **Dimension_City**) to open it and validate the data preview, confirming that records have been successfully copied and are available for querying and downstream processing in the Silver Lakehouse.

    ![01](./media/ex2-14.png) 

## Task 2: Configure a Variable for File Directory

Here, you enhance pipeline flexibility by creating a variable called
fileDirectory that dynamically builds a folder path based on the
pipeline’s run date. This variable ensures that files are always written
into date-partitioned folders (yyyy/MM/dd). Assigning such variables
improves automation and modularity within pipelines, making maintenance
and scaling easier.

1. Navigate to **Workspaces** and select the **Medallion- workspace** to continue working with your data pipeline.

    ![01](./media/ex1-46.png)

1. Click **+ New item** in the **Initial process** stage to create a new data processing component (such as a Copy job or transformation) for preparing data before moving it to the next layer.

    ![01](./media/ex2-15.png) 

1. Ensure **Initial process** is selected under **Assign to task**, then click **Pipeline** to create a new pipeline for performing data transformation and processing activities in this stage.

    ![01](./media/ex2-16.png) 

1. Enter **ContosoTables** as the pipeline name, ensure it is assigned to **Initial process**, and then click **Create** to create the pipeline for transforming data in this stage.

    ![01](./media/ex2-17.png) 

1. Click **Pipeline activity** under **Start with a blank canvas** to begin designing the pipeline and add transformation activities for processing data in the Initial process stage.

    ![01](./media/ex2-18.png) 

1. Click **Activities** and then select **Set variable** to add a variable activity to the pipeline for controlling and storing values during execution.

    ![01](./media/ex2-19.png) 

1. Go to the **Settings** tab in the **Set variable** activity, ensure **Pipeline variable** is selected, and click **+ New** to create a new variable that can be used within the pipeline for dynamic value handling.

    ![01](./media/ex2-20.png) 

1. Enter **fileDirectory** as the variable name, keep the type as **String**, and click **Confirm** to create the pipeline variable.

    ![01](./media/ex2-21.png) 

1. Select the **Value** text input box. This will display the **Add dynamic content** property. Select this text to open the pipeline expression builder.

    ![01](./media/ex2-22.png) 

1. Enter the dynamic expression (1) to build the folder path (combining **'ContosoSales\'** with the formatted current date), verify it under **Functions (2)**, and click **OK (3)** to assign it as the value for the **fileDirectory** variable.
	
    ```
    @concat(
        'ContosoSales\',
        formatDateTime(
            convertFromUtc(
                utcnow(), 'Central Standard Time'
            ),
            'yyyy/MM/dd'
        )
    )
    ```

    ![01](./media/ex2-23.png) 

1. Go to the **General** tab and enter **Set file directory** as the activity name to clearly identify this step as setting the dynamic folder path for the pipeline execution.

    ![01](./media/ex2-24.png)

This step helps in identifying and managing the activity within your
pipeline in subsequent steps, making it easier to understand its purpose
and functionality.

## Task 3: Retrieve Metadata Using Dynamic Path

In this task, you add a Get Metadata activity to the pipeline, which
retrieves details of files located in the dynamically generated
directory path. By referencing the *fileDirectory* variable, the
activity dynamically identifies child files in the Bronze Lakehouse.
This setup ensures the pipeline can automatically discover and process
incoming files without manual configuration

1. Click **Activities** and then select **Get metadata** to add a metadata activity that can retrieve information (such as file or folder details) based on the dynamically set directory.

    ![01](./media/ex2-25.png)

1. Go to **Settings → Connection dropdown → Browse all**, then select your **Lakehouse (e.g., bronze_Lakehouse)** from the OneLake catalog.

    ![01](./media/ex2-26.png)

1. Select **OneLake catalog**, then choose the **bronze_Lakehouse** as your data source.

    ![01](./media/ex2-27.png)

1. Select **Files** as the root folder, then click **Add dynamic content** to set the directory path dynamically.

    ![01](./media/ex2-28.png)

1. Select **Variables (1) → fileDirectory (2)**, following expression will generate `@variables('fileDirectory')` **(3)**, and click **OK (4)**.

    ![01](./media/ex2-29.png)

1. In **Settings → Field list**, click **+ New**, open the dropdown, and select **Child items** to retrieve the list of files in the folder.

    ![01](./media/ex2-30.png)

1. Go to the **General** tab, rename the activity to **Get items in folder**, and connect it after the **Set file directory** activity.

    ![01](./media/ex2-31.png)

1. Click **Validate** to verify the pipeline, confirm the message **“No errors were found”**, and then click **Run** to execute the pipeline.

    ![01](./media/ex2-32.png)

1. Click **Save and run** to save the pipeline and start execution.

    ![01](./media/ex2-33.png)

1. Verify the pipeline run shows **Succeeded** for both activities (**Set file directory** and **Get items in folder**) in the **Output** tab.

    ![01](./media/ex2-34.png)

1. From the **'Get items in folder**' activity, select the last column labeled '**Output**' to review the activity's contents. This step verifies that the filenames from the directory have been correctly retrieved and included in the output.

    ![01](./media/ex2-35.png)

## Task 4: Implement conditional paths (Dimension vs Fact logic)

In this step, you implement business logic to differentiate how
dimension and fact tables are processed. A ForEach loop iterates through
all discovered files. If the file name starts with Dim, the pipeline
overwrites the corresponding Silver table (since dimensions are
refreshed completely). For fact tables, the pipeline appends new data
(since facts grow incrementally). This conditional handling ensures the
Silver layer is properly maintained and aligned with real-world data
warehouse practices.

1. Go to the **Activities** tab, drag the **ForEach** activity onto the canvas, select it, and in the **General** tab set the name to **“For each file”**. Then, click the **+** inside the activity to add actions that will run for each file (e.g., processing or copying each item).

    ![01](./media/ex2-36.png)

1. Select the **ForEach** activity and connect it to the **Get Metadata** activity output. Then go to the **Settings** tab, and in the **Items** field click **Add dynamic content**. Choose the output from the **Get Metadata** activity (typically `childItems`) so that the loop iterates through each file retrieved from the folder.

    ![01](./media/ex2-37.png)

1. In the **Pipeline expression builder**, go to the **Activity outputs** tab, select **Get items in folder childItems**, which auto-populates the expression as `@activity('Get items in folder').output.childItems`, and then click **OK** to apply it.

    ![01](./media/ex2-38.png)

1. Inside the **ForEach** activity, click the **+ (Add activity)** button and select **Copy data** from the list to add it as the activity that will run for each file in the loop.

    ![01](./media/ex2-39.png)

1. Inside the **ForEach** activity, click the **edit (pencil) icon** on the added **Copy data** activity to configure its source and destination settings for processing each file.

    ![01](./media/ex2-40.png)

1. Select the **Copy data** activity inside the **ForEach**, go to the **General** tab, and set the **Name** to **“Copy tables”** while keeping the activity state as **Activated**.

    ![01](./media/ex2-41.png)

1. Go to the **Source** tab of the **Copy data** activity, click the **Connection** dropdown, and select **Browse all** to choose or create the required data source connection.

    ![01](./media/ex2-42.png)

1. In the **Get data** window, select **OneLake catalog** from the left pane, then choose the **bronze_Lakehouse** from the list to use it as the source.

    ![01](./media/ex2-43.png)

1. In the **Source** tab, select the connected **bronze_Lakehouse**, choose **Files** as the root folder, set the **File format** to **Parquet**, and click **Add dynamic content** for the **File path** to dynamically pass the folder or file details from the loop.

    ![01](./media/ex2-44.png)

1. In the **Pipeline expression builder**, go to the **Variables** tab, select **fileDirectory**, which sets the expression to `@variables('fileDirectory')`, and then click **OK** to apply it.

    ![01](./media/ex2-45.png)

1. In the **Source** tab, click **Add dynamic content** for the **File name** field to dynamically pass the current file name from the ForEach loop.

    ![01](./media/ex2-46.png)

1. Select the item **For each file** within the expression builder's **ForEach iterator** section. Within the expression builder, add the suffix ".name" to access the name property of the current items array and then **OK** to continue once complete.

1. Copy and paste the code block below into the expression input box.

	```
    @item().name
    ```

    ![01](./media/ex2-47.png)

1. Go to the **Destination** tab, open the **Connection** dropdown, and select **Browse all** to choose or create the destination connection.

    ![01](./media/ex2-48.png)

1. In the **Get data** window, select **OneLake catalog** from the left pane, then choose the **silver_Lakehouse** as the destination.

    ![01](./media/ex2-49.png)

1. In the **Destination** tab, select **Tables** as the root folder, then click **Add dynamic content** for the **Table** field to dynamically pass the table name, and keep the **Table action** set to **Append**.

    ![01](./media/ex2-50.png)

1. In the **Pipeline expression builder**, go to the **Functions** tab, expand **String Functions**, select **split**, and enter the following expression, then click **OK** to apply it.

    ```
    @split(item().name, '.')[0]
    ```

    ![01](./media/ex2-51.png)

1. In the **Destination** tab, ensure **Append** is selected as the **Table action**, and use **Add dynamic content** if any further parameterization is needed.

    ![01](./media/ex2-52.png)

1. In the expression builder, we'll use the if condition from the logical **functions** group and the startswith function from the string functions group to determine if the string starts with the prefix of **Dim** for our dimension tables. If true, we'll set the value to **Overwrite**, if false **Append**. Once complete select **OK** to continue.

1. Copy and paste the code block below into the expression input box.

    ```
    @if(
        startswith(item().name, 'Dim'),
        'Overwrite',
        'Append'
    )
    ```

    ![01](./media/ex2-53.png)

1. Select the **Main canvas** option from the breadcrumb trail to return to the top level of pipeline.

    ![01](./media/ex2-54.png)

1. From the **Home** tab, select the **Validate** option to first confirm that there are no issues within the pipeline. 

1. Once validated, select **Run** to start the ingestion from the data pipeline.

    ![01](./media/ex2-55.png)

1. Confirm by selecting **Save and run**.

    ![01](./media/ex1-76.png)

1. Running the pipeline initiates the data transfer, allowing you to see the results of your configuration in action within the output window.

1. The data pipeline runs multiple activities sequentially, writing to the Silver data layer as delta parquet tables. v-Order optimized tables enhance performance, reduce storage costs, and supportefficient querying and data updates-ideal for analytical workloads in Microsoft Fabric.

    ![01](./media/ex2-56.png)

# Exercise 3: Initial Gold Layer Preparation

With structured data in place, you turn your attention to curating
datasets for business intelligence in the **Gold** layer, the
destination for analytics and reporting. This phase involves
transforming and loading data using Data Factory and Power Query Online,
supported by Fabric’s scalable compute and storage architecture. You
build Dataflow Gen2 to apply final transformations and configure
pipelines to route data accurately. You also create a semantic model to
define relationships across key tables, enabling intuitive reporting.
The process concludes with a streamlined data ingestion setup and a
Power BI dashboard, completing the end-to-end data lifecycle.

![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image237.svg)

## Task 1: Create Gold Warehouse

In this task, you create a dedicated Warehouse named gold_Warehouse that
serves as the curated Gold layer. This warehouse is optimized for
analytics and reporting, offering a structured and performant
environment for business intelligence workloads.

1. On the left navigation, click on ***Data Factory-medallion@lab.LabInstance.Id***, as
    shown in the image below.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image238.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image239.png)

2. Select the **New item** option on the **Gold data** from task flow
    to add another storage item.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image240.png)

3. On the **Create an item** tab, select **Warehouse**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image241.png)

4. On the New **warehouse** window, set the warehouse name to
    **gold_Warehouse** and then select **Create**.

    ![A screenshot of a login box AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image242.png)

## Task 2: From Silver to Gold: Persisting Mirrored SQL Tables

In this task, you build a pipeline named *Copy mirrored_gold* to copy
all structured Silver Lakehouse tables into the Gold Warehouse. This
step ensures persistence, centralization, and optimized availability of
refined data for analytics.

1. From the **Home** tab of the warehouse, select **Get
    data** from **New pipeline**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image243.png)

2. On the New pipeline window, set the data pipeline name to
    **Copydata_silver to gold** and then select **Create**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image244.png)

3. On the **Choose a data into Lakehouse** window, select **OneLake
    catalog** and then select **silver_Lakehouse** lakehouse

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image245.png)

4. From the **Connect to data source** page, select all tables and then
    click the **Next** button

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image246.png)

5. Click **Next** again.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image247.png)

6. Click **Next** on the **Settings** tab.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image248.png)

7. On the **Review + save** tab, click **Save + Run**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image249.png)

8. Click **OK**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image250.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image251.png)

9. On the top navigation menu, navigate and click on
    ***gold_Warehouse***, as shown in the below image.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image252.png)

10. The tables now appear in the **dbo** section

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image253.png)

## Task 3: Create initial Dataflow Gen2 structure

Here, you create a Dataflow Gen2 object called *PrepContoso* using Power
Query Online. This Dataflow is designed to apply transformations and
prepare data for downstream analytics by leveraging Fabric’s staging
architecture for large-scale compute

1. On the left navigation, click on **Data Factory-medallion@lab.LabInstance.Id**, as
    shown in the image below.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image254.png)

2. From the task flow, select the **Further transform** task and click
    on the **New item** option. Within the Create an item pane display
    properties, change the toggle to the **All items** option.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image255.png)

3. From the list of available items, select the **Dataflow Gen2** item

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image256.png)

5. On the **New Dataflow Gen2** window, click on **Dataflow1** and
    rename it to **PrepContoso**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image257.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image258.png)

## Task 4: Connecting to warehouse tables

In this task, you connect the Dataflow to the *gold_Warehouse* and
select relevant tables (dimensions and facts).

1. From the **Home** tab, select **Get data** and then
    the **More...** option.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image259.png)

2. On the **Get data** explorer's search bar,
    type **gold_Warehouse** to locate the gold Warehouse item.
    Select the **gold_Warehouse** item within the OneLake catalog's
    returned results.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image260.png)

3. From the Get data table navigator, select the tables listed below to
    perform data transformation operations and merge the tables for our
    downstream business intelligence projects.

    |  |
    |---|
    |Table Name|
    |bdo_ Dimension_City|
    |bdo_Dimension_Customer|
    |bdo_Dimension_Date|
    |bdo_Dimension_Employee|
    |bdo_Dimension_Payment_Method|
    |bdo_Dimension_Supplier|
    |bdo_Dimension_Transaction_Type|
    |bdo_Fact_Order|
    |bdo_Fact_Purchase|
    |bdo_Fact_Sale|
    |bdo_Fact_Stock_Holding|
    |bdo_Fact_Transaction|


    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image261.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image262.png)

4. Select the **Fact_Sale** table and from the Home tab, navigate to
    the **Merge queries** option and select **Merge queries as new**.

	> Note that there are two options: Merge queries, which will merge a
	> table with the existing query, or Merge queries as new, which will
	> create a new query in your editor. This step allows you to combine
	> data from different tables, enhancing the comprehensiveness of your
	> dataset.


    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image263.png)

5. From the Merge query window, set the **Right table for
    merge** to **dbo**\_**Fact_Order**. In the top right corner, select
    the lightbulb which has detected a possible column match. In this
    example, both tables contain a column titled **City Key** Select
    this option to set the columns to be merged on. For the join kind,
    select **Inner** and then **OK** to proceed. This activity ensures
    that the data is accurately combined based on matching columns.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image264.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image265.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image266.png)

6. Navigate to the far right for the **Merge** table and select the
    joined **dbo_Fact_Orders** table column's top right corner to expand
    the table, from the avaialble column selections deselect **City
    Key** and **Stock Item Key** since this column is what we used to
    merge on and already exists in the dataset before
    selecting **OK** to continue

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image267.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image268.png)

7. Select the **Merge** query and from the **Home** tab, select **Add
    data destination** and then choose the **Warehouse** option.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image269.png)

8. On the **Connect to data destination** dialog box, your connection
    should already be selected. Select **Next** to continue.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image270.png)

9. On the **Choose destination target** dialog, browse to the warehiuse
    where you wish to load the data and name the new table, then
    select **Next** again.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image271.png)

10. On the **Choose destination settings** dialog, you can use the
    automatic settings or deselect the automatic settings and leave the
    default **Replace** update method, double check that your columns
    are mapped correctly, and select **Save settings**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image272.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image273.png)

11. On the Home window, select **Save & run** and click on **Save &
    run** button

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image274.png)

12. On the left navigation, select ***Data Factory-medallion@lab.LabInstance.Id***, as
    shown in the image below.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image275.png)

13. On the Fabric workspace, select the more options ellipsis icon next
    to the **PrepContoso** dataflow and select the **Check validation**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image276.png)

    ![A screenshot of a computer error AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image277.png)

14. On the top menu, select **gold_Warehouse** that you have created in
    the previous task.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image278.png)

## Task 5: Build Report(Optional) 

In this task, you build a Power BI Report.

1. On gold_Warehouse page, select the **Home** tab and then
    select **New semantic model**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image279.png)

2. On the *New Semantic Model* tab, enter the name as **Sample
    model**, select the **dbo** schema, choose the below tables, and
    then click **Confirm**

	|  |
	|---|
	|Table Name|
	|bdo_ Dimension_City|
	|bdo_Dimension_Customer|
	|bdo_Dimension_Date|
	|bdo_Dimension_Employee|
	|bdo_Dimension_Payment_Method|
	|bdo_Dimension_Supplier|
	|bdo_Dimension_Transaction_Type|
	|bdo_Fact_Order|
	|bdo_Fact_Purchase|
	|bdo_Fact_Sale|
	|bdo_Fact_Stock_Holding|
	|bdo_Fact_Transaction|

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image280.png)

5. On the left navigation, click on ***Data Factory-Medallion@lab.LabInstance.Id***.

6. Select ***Sample model***

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image281.png)

7. On the sample model home page, click **Open Semantic Model** to
    access the model.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image282.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image283.png)

8. Ensure from the top-right corner that the data model designer is
    selected in the **Editing** mode. This should change the drop-down
    text to “Editing”.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image284.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image285.png)

9. From the dbo\_**fact_sale** table, drag the **CityKey** field and
    drop it on the **CityKey** field in
    the dbo\_**dimension_city** table to create a relationship.
    The **Create Relationship** dialog box appears..

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image286.png)

	in the **Create Relationship** dialog box:

	- **Table 1** is populated with **dbo_fact_sale** and the column
	  of **CityKey**.

	- **Table 2** is populated with **dbo_dimension_city** and the column
	  of **CityKey**.

	- Cardinality: **Many to one (\*:1)**

	- Cross filter direction: **Single**

	- Leave the box next to **Make this relationship active** selected.

	- Select the box next to **Assume referential integrity.**

	- Select **Save.**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image287.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image288.png)

10. Next, add these relationships with the same **Create
    Relationship** settings as shown above but with the following tables
    and columns:

	- **Salespersonkey(dbo_Fact_Sale)** - **EmployeeKey(dbo_Dimension_Employee)**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image289.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image290.png)

11. Ensure to create the relationships between the below two sets using
    the same steps as above.

    - **CustomerKey(dbo_Fact_Sale)** - **CustomerKey(dbo_Dimension_Customer)**

12. After you add these relationships, your data model should be as
    shown in the below image and is ready for reporting.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image291.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image292.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image293.png)

	- **InvoiceDateKey(dbo_Fact_Sale)** - **Date(dbo_Dimension_Date)**

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image294.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image295.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image296.png)

13. From the top ribbon, select **File** and select **Create new
    report** to start creating reports/dashboards in Power BI.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image297.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image298.png)

14. On the **Data** pane, expand **dbo_fact_sales** and check the box
    next to **Profit**. This selection creates a column chart and adds
    the field to the Y-axis.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image299.png)

15. With the bar chart selected, select the **Card** visual in the
    visualization pane.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image300.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image301.png)

16. Click anywhere on the blank canvas (or press the Esc key) so the
    Card that we just placed is no longer selected.

	**Add a Bar chart:**

17. On the **Visualizations** pane, select the **Stacked area
    chart** visual.

18. On the **Data** pane, expand **Fact_Sales** and check the box next
    to **Profit**. This selection creates a column chart and adds the
    field to the Y-axis.

19. Expand **Dimension_Date** and check the box
    for **FiscalMonthNumber**. This selection adds the field to the
    X-axis. This selection creates a filled line chart showing profit by
    fiscal month.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image302.png)

20. On the **Data** pane, expand **Dimension_Stock_Item** and
    drag **BuyingPackage** into the Legend field well. This selection
    adds a line for each of the Buying Packages

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image303.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image304.png)

21. With the bar chart selected, select the **Clustered bar
    chart** visual in the visualization pane. This selection converts
    the column chart into a bar chart.

22. On the **Data** pane, expand **Fact_Sales** and check the box next
    to **Profit**. This selection creates a column chart and adds the
    field to the Y-axis.

23. Expand **Dimension_Employee** and check the box for **Employee**.
    This selection adds the field to the Y-axis.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image305.png)

24. Click anywhere on the blank canvas (or press the Esc key) so the
    chart is no longer selected.

25. From the ribbon, select **File** \> **Save**.

    ![A screenshot of a graph AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image306.png)

26. Enter the name of your report as **Profit Reporting**.
    Select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image307.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image308.png)

27. On the left navigation, select ***Data Factory-XX***, as shown in
    the image below.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image309.png)

28. From the task flow, select the **Data visualize** task and click on
    the paper clip icon to assign a previously created item.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image310.png)

29. Select the **Profit Reporting** item and then press **Select**.

    ![](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image311.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image312.png)

## Task 6: Clean up resources

1. From the top-right corner of the Fabric Workspace page,
    select **Workspace settings**. If you do not see this option, select
    the **...** option at the top right of the page and then
    select **Workspace settings**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image313.png)

2. Select **General** and then select **Remove this workspace**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image314.png)

3. Click **Delete** to delete the workspace.

    ![A white background with black text AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/fbrcdtafctrydepth/refs/heads/Cloud-slice-December2025/Labguides/Usecase%2001/media/image315.png)

**Summary**

This usecase demonstrates how to implement the **Medallion
Architecture** in Microsoft Fabric by building an end-to-end data
lifecycle across Bronze, Silver and Gold layers. In the **Bronze
layer**, raw data from Azure SQL and ContosoSales files is ingested into
a Lakehouse using pipelines and mirrored databases. The **Silver layer**
transforms and structures this raw data into dimension and fact tables
with dynamic pipelines, metadata-driven discovery, and conditional logic
to handle overwrites for dimensions and appends for facts. Finally, the
**Gold layer** curates business-ready datasets by persisting Silver
tables into a warehouse and applying further transformations through
Dataflow Gen2 and Power Query, producing enriched entities like
DimCustomers and Fact Sales for analytics. Validation is performed
through deployment pipelines across Dev, Test, and Prod, ensuring
consistency, governance, and scalability, before cleaning up resources
to maintain cost efficiency.


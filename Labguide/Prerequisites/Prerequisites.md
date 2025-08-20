## Prerequisites: Connect to the Wide World Importers OLTP database hosted on Azure SQL Database

### Task 1: Create a single database - Azure SQL Database

1.  From the Azure portal home page, click on **Azure portal
    menu** represented by three horizontal bars on the left side of the
    Microsoft Azure command bar. Select SQL database

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  Click on **+ Create**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image2.png)

3.  On **Create a storage account** window, under the **Basics** tab,
    enter the below details to create a storage account and then click
    on **Next:Networking**

[TABLE]

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image7.png)

4.  On the **Networking** tab, for **Connectivity method**,
    select **Public endpoint**.

5.  For **Firewall rules**, set **Add current client IP
    address** to **Yes**. Leave **Allow Azure services and resources to
    access this server** set to **No**. Click on **Next : Security**

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image8.png)

6.  Select **Next: Additional settings** at the bottom of the page

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

7.  On the **Additional settings** tab, select **Review + create** at
    the bottom of the page

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

8.  On the **Review + create** page, after reviewing, select **Create**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image13.png)

9.  On **Microsoft.SQLDatabase** window, after the deployment is
    completed, click on the **Go to resource** button

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image14.png)

10. On the **Azure SQL Database** window, click on **Overview** in the
    left navigation menu, copy **Server name and SQL Databse name** ,
    paste them in a notepad, and then **Save** the notepad to use the
    information in the upcoming task.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image15.png)

11. In the Fabric-RG resource group , select **sqlserverXXX**

> ![A screenshot of a chat AI-generated content may be
> incorrect.](./media/image16.png)

12. In your **SQL Server** window, navigate to the **Security**
     section, and click on **Networking**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image17.png)

13. Select **Allow Azure service and resources to access this server**
    and click on **Save** button

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image18.png)

14. Navigate to your **Azure SQL Server.** Under **Identity**, enable
    the **System Assigned Managed Identity** and click on **Save**
    button.

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image19.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png)

### Task 2: Create an Azure Storage Account by using the portal

1.  Click on the **Portal Menu**, then select **+ Create a resource**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

2.  In the **Create a resource** window search box, type **+++Storage
    account+++** and then click on the **storage account**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

3.  In the **Marketplace** page, click on the **Storage
    account** section.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

4.  In the **Storage account** window, click on the **Create** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

5.  On **Create a storage account** window, under the **Basics** tab,
    enter the below details to create a storage account and then click
    on **Next**

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image25.png)

6.  Under the **Advanced** tab, Select **"Enable hierarchical
    namespace"** and click on **Review+create.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image26.png)

1.  On the **Review** tab, click on the **Create** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

2.  This new Azure Storage account is now set up to host data for an
    Azure Data Lake. Click on the **Go to resource** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image28.png)

3.  After the account has been deployed, you will find options related
    to Azure Data Lake in the Overview page. In the left-side navigation
    pane, navigate to **Data storage** section, then click
    on **Containers**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image29.png)

4.  Click on **+Add Container.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

5.  On the New container pane that appear on the right side, enter the
    container Name as +++**contososales**+++ and click on Create button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image31.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

6.  On **fabricstorageXX | Containers** page,
    select **contososales** container

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image33.png)

7.  On container page, click on **Upload** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

8.  In the **Upload blob** pane, click on **Browse for file**, navigate
    to **C:\Labfiles** location and select **ContosoSales.zip**, then
    click on the **Open** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

9.  In **Upload blob** pane, click on the **Upload** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image37.png)

10. On the **Storage account** window, select on **Access keys** in the
    left navigation menu, copy **Key and Storage account name** , paste
    them in a notepad, and then **Save** the notepad to use the
    information in the upcoming task.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image38.png)

### Task 3: Restoring the WideWorldImporters Sample Database Using SQL Server Management Studio (SSMS)

1.  In your Windows search box, type +++**sql server management studio
    21**+++, then click on **SQL Server Mangement Studio 21**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

2.  Click on **Sing in** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image40.png)

3.  Select Microsoft

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image41.png)

4.  Select **Work or school account** and click on **Continue**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image42.png)

5.  sign in with your cloud slice account below.

- Username: <+++@lab.CloudPortalCredential>(User1).Username+++

- Password: \<<+++@lab.CloudPortalCredential>(User1).Password\>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image43.png)

6.  In the SQL studio, select **Database Engine**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image44.png)

7.  In the Connect tab , select **Browse**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

8.  Select your subscription

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image46.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image60.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image61.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image62.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image63.png)

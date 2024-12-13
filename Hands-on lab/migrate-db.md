# Exercise 3: Migrate the On-prem Database to Azure and configure it with your App service

Duration: 80 minutes

The next step of Part Unlimited's migration project is the assessment and migration of its database. Currently, the database lives on SQL Server 2008 R2 on a virtual machine. You will use an **Azure Migrate: Database Assessment** tool called **Microsoft Data Migration Assistant (DMA)** to assess the `PartsUnlimited` database for migration to Azure SQL Database. The assessment generates a report detailing any feature parity and compatibility issues between the on-premises database and Azure SQL Database. After the assessment, you will use an **Azure Migrate: Database Migration** service called **Azure Database Migration Service (DMS)**. During the exercise, you will use a simulated on-premises environment hosted on virtual machines running on Azure.
## Lab objectives

You will be able to complete the following tasks:

- Task 1: Connect to your SqlServer2008 VM with RDP

- Task 2: Perform assessment for migration to Azure SQL Database

- Task 3: Retrieve connection information for SQL Databases (Optional)

- Task 4: Migrate the database schema using the Data Migration Assistant

- Task 5: Migrate the database using the Azure Database Migration Service 

## Task 1: Connect to your SqlServer2008 VM with RDP

1. From your lab environment (**WebVM**), in the search bar, **Search (1)** for **RDP (2)** and **select** the **Remote Desktop Connection (3)** app.
   
   ![](media/RDP-new.png)

2. Paste the **SQLVM DNS Name** in the **Computer** field and click on **Connect**.
   * **SQLVM DNS Name**: **<inject key="SQLVM DNS Name" style="color:blue" />**

   ![](media/rdp-vm2.png)  
 
3. Now, enter the SQLVM **username**, and **password** provided below and then click on the **Ok** button. Please add the **dot** and **back-slash** “.\” before the username.
   * **username**: **<inject key="SQLVM Username" style="color:blue" />** 
   * **password**: **<inject key="SQLVM Password" style="color:blue" />**
   
   ![](media/vm1-more-choices.png) 

4. Next, click on the **Yes** button to accept the certificate and add in trusted certificates.

   ![](media/logib-vm2-2.png)

## Task 2: Perform assessment for migration to Azure SQL Database

Parts Unlimited would like an assessment to see what potential issues they might need to address in moving their database to Azure SQL Database. In this task, you will use the [Microsoft Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) (DMA) to assess the `PartsUnlimited` database against Azure SQL Database (Azure SQL DB). Data Migration Assistant (DMA) enables you to upgrade to a modern data platform by detecting compatibility issues that can impact database functionality on your new version of SQL Server or Azure SQL Database. It recommends performance and reliability improvements for your target environment. The assessment generates a report detailing any feature parity and compatibility issues between the on-premises database and the Azure SQL DB service.

> **Note**: The Database Migration Assistant is already installed on your Lab (Web) VM. If not found, it can be downloaded through Azure Migrate or from the [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=2090807) as well, and as Data Migration Assistant is dependent on .NET Framework 4.8 download and install .Net Framework from [here](https://go.microsoft.com/fwlink/?LinkId=2085155) and **restart** the VM before you install Data Migration Assistant.

1. Launch DMA from the Windows Start menu by typing **"data migration" (1)** into the search bar, and then selecting **Microsoft Data Migration Assistant (2)** from the search results.

    > **Note**: There is a known issue with screen resolution when using an RDP connection to Windows Server 2008 R2, which may affect some users. This issue presents itself as very small, hard to read text on the screen. The workaround for this is to use a second monitor for the RDP display, which should allow you to scale up the resolution to make the text larger.

   ![](media/updated37u.png)

2. In the DMA dialog, select **+** from the left-hand menu to create a new project.

   ![The new project icon is highlighted in DMA.](media/updated38.png "New DMA project")

3. In the New Project pane, set the name of the project **(1)** and make sure the following values are selected:

   - **Project type**: Select Assessment.
   - **Project name (1)**: Enter **Assessment**
   - **Assessment type**: Select Database Engine.
   - **Source server type**: Select SQL Server.
   - **Target server type**: Select Azure SQL Database.
   - Select **Create (2)**.

   ![New project settings for doing an assessment of a migration from SQL Server to Azure SQL Database.](media/updated39.png "New project settings")

4. On the **Options** screen, ensure **Check database compatibility (1)** and **Check feature parity (1)** are both checked, and then select **Next (2)**.

   ![Check database compatibility and check feature parity are checked on the Options screen.](media/appmod-EX3s4.png "DMA options")

5. On the **Sources** screen, enter the following into the **Connect to a server** dialog that appears on the right-hand side:

    - **Server name (1)**: Enter **SQLSERVER2008**
    - **Authentication type (2)**: Select **SQL Server Authentication**.
    - **Username (3)**: Enter **PUWebSite**
    - **Password (4)**: Enter **<inject key="SQLVM Password" />**
    - **Encrypt connection**: Check this box if not checked.
    - **Trust server certificate (5)**: Check this box.
    - Select **Connect (6)**.

    ![In the Connect to a server dialog, the values specified above are entered into the appropriate fields.](media/updated40.png "Connect to a server")

6. In the **Add sources** dialog that appears next, check the box for `PartsUnlimited` **(1)** and select **Add (2)**.

    ![The PartsUnlimited box is checked on the Add sources dialog.](media/updated41.png "Add sources")

7. Select **Start Assessment**.

    ![Start assessment](media/dma-start-assessment-to-azure-sql-db.png "Start assessment")

8. Take a moment to review the assessment for migrating to Azure SQL DB. The SQL Server feature parity report shows that Analysis Services and SQL Server Reporting Services are unsupported, but these do not affect any objects in the `PartsUnlimited` database, so we won't block a migration. If there is no SQL Server feature parity, then proceed to the next step.

    ![The feature parity report is displayed, and the two unsupported features are highlighted.](media/app-mod-new.png "Feature parity")

9. Now, select **Compatibility issues (1)** to review that report as well.

    ![The Compatibility issues option is selected and highlighted.](media/updated42.png "Compatibility issues")

    The DMA assessment for migrating the `PartsUnlimited` database to a target platform of Azure SQL DB reveals that no issues or features are preventing Parts Unlimited from migrating their database to Azure SQL DB.

10. Select **Upload to Azure Migrate** to upload assessment results to Azure.

    ![Upload to Azure Migrate button is highlighted.](media/dma-upload-azure-migrate.png "Azure Migrate Upload")

11. Select the right Azure environment **(1)** your subscription lives. Select **Connect (2)** to proceed to the Azure login screen. Please use the  below credentials to login.
    * Email/Username: <inject key="AzureAdUserEmail"></inject>
    * Password: <inject key="AzureAdUserPassword"></inject>

    ![Azure is selected as the Azure Environment on the connect to Azure screen. Connect button is highlighted.](media/dma-azure-migrate-upload.png "Azure Environment Selection")

12. Select your subscription **(1)** and the `partsunlimited` Azure Migrate project **(2)**. Select **Upload (3)** to start the upload to Azure.

    ![Upload to Azure Migrate page is open. Lab subscription and partsunlimited Azure Migrate Project are selected. Upload button is highlighted.](media/dma.png "Azure Migrate upload settings")

    > **Note**: If you encounter **Failed to fetch subscription list from Azure, Strong Authentication is required (1)** you might not see some of your subscription because of MFA limitations. You should still be able to see your lab subscription.

     ![Upload to Azure Migrate page is open. Lab subscription and partsunlimited Azure Migrate Project are selected. Upload button is highlighted.](media/dma-azure-migrate-upload-2.png "Azure Migrate upload settings")


13. Once the upload is complete, select **OK** and navigate to the Azure Migrate page on the Azure Portal.

    ![Assessment Uploaded dialog shown.](media/updated43.png "Assessment Uploaded")

14. Select the **Databases (only) (1)** page on Azure Migrate. Observe the number of assessed database instances **(2)** and the number of databases ready for Azure SQL DB **(2)**. Keep in mind that you might need to wait for 5 to 10 minutes for the results to show up. You can use the **Refresh** button on the page to see the latest status.

    ![Azure Migrate Databases page is open. The number of assessed database instances and the number of databases ready for Azure SQL DB shows one.](media/dma-azure-migrate-web-2.1.png "Azure Migrate Database Assessment")

## Task 3: Retrieve connection information for SQL Databases (Optional)

In this task, you will retrieve the Fully Qualified Domain Name for the Azure SQL Database. This information is needed to connect to the Azure SQL Database from Azure Data Migration Service and Azure Data Migration Assistant.

1. In the [Azure portal](https://portal.azure.com), navigate to your **SQL database** resource by selecting the **parts** SQL database resource from the resources list.

   ![](media/updated44.png)

2. On the **Overview** Blade of your SQL database, copy the **Server name** and paste the value into a text editor, such as Notepad.exe, for later reference.

   ![The server name value is highlighted on the SQL database Overview blade.](media/updated45.png "SQL database")

## Task 4: Migrate the database schema using the Data Migration Assistant

After you have reviewed the assessment results and you have ensured the database is a candidate for migration to Azure SQL Database, please use the Data Migration Assistant to migrate the schema to Azure SQL Database.

1. Return to the Data Migration Assistant, and select the New **(+)** icon in the left-hand menu.

2. In the New project dialog, enter the following:

   - **Project type (1)**: Select Migration.
   - **Project name (2)**: Enter **Migration**
   - **Source server type**: Select SQL Server.
   - **Target server type**: Select Azure SQL Database.
   - **Migration scope (3)**: Select Schema only.
   - Select **Create (4)**.

   ![The above information is entered in the New project dialog box.](media/updated46.png "New Project dialog")

3. On the **Select source** tab, enter the following:

   - **Server name (1)**: Enter **SQLSERVER2008**.
   - **Authentication type (2)**: Select **SQL Server Authentication**.
   - **Username (3)**: Enter **PUWebSite**
   - **Password (4)**: Enter **<inject key="SQLVM Password" />**
   - **Encrypt connection**: Check this box.
   - **Trust server certificate (5)**: Check this box.
   - Select **Connect (6)**, and then ensure the `PartsUnlimited` database is selected **(7)** from the list of databases.
   - Select **Next (8)**.

   ![The Select source tab of the Data Migration Assistant is displayed, with the values specified above entered into the appropriate fields.](media/updated47.png "Data Migration Assistant Select source")

4. On the **Select target** tab, enter the following:

   - **Server name (1)**: Enter the server name of your Azure SQL Database - **<inject key="sqlDatabaseName" enableCopy="false"/>.database.windows.net** 
   - **Authentication type (2)**: Select SQL Server Authentication.
   - **Username (3)**: Enter **demouser**
   - **Password (4)**: Enter **<inject key="SQL Password" />**
   - **Encrypt connection**: Check this box.
   - **Trust server certificate (5)**: Check this box.
   - Select **Connect (6)**, and then ensure the `parts` database is selected **(7)** from the list of databases.
   - Select **Next (8)**.

   ![The Select target tab of the Data Migration Assistant is displayed, with the values specified above entered into the appropriate fields.](media/data-migration-assistant-migration-select-target.png "Data Migration Assistant Select target")

5. On the **Select objects** tab, leave all the objects checked **(1)**, and select **Generate SQL script (2)**.

    ![The Select objects tab of the Data Migration Assistant is displayed, with all the objects checked.](media/data-migration-assistant-migration-select-objects.png "Data Migration Assistant Select target")

6. On the **Script & deploy schema** tab, review the script. Notice the view also provides a note that there are no blocking issues **(1)**. Now, select **Deploy schema (2)**.

    ![The Script & deploy schema tab of the Data Migration Assistant is displayed, with the generated script shown.](media/data-migration-assistant-migration-script-and-deploy-schema.png "Data Migration Assistant Script & deploy schema")

7. After the schema is deployed, review the deployment results, and ensure there are no errors.

    ![The schema deployment results are displayed, with 23 commands executed and 0 errors highlighted.](media/updated48.png "Schema deployment results")

8. Launch SQL Server Management Studio (SSMS) from the Windows Start menu by typing "sql server management" **(1)** into the search bar, and then selecting **SQL Server Management Studio 18 (2)** in the search results.

    ![](media/appmod-dma.png)

9. Connect to your Azure SQL Database, by selecting **Connect->Database Engine** in the Object Explorer, and then enter the following into the Connect to server dialog:

    - **Server name (1)**: Enter the server name of your Azure SQL Database - **<inject key="sqlDatabaseName" enableCopy="false"/>.database.windows.net** 
    - **Authentication type (2)**: Select SQL Server Authentication.
    - **Login (3)**: Enter **demouser**
    - **Password (4)**: Enter **<inject key="SQLVM Password" />**
    - **Remember password (5)**: Check this box.
    - Select **Connect (6)**.

    ![The SSMS Connect to Server dialog is displayed, with the Azure SQL Database name specified, SQL Server Authentication selected, and the demouser credentials entered.](media/updated49.png "Connect to Server")

10. Once connected, expand **Databases**, and expand **parts**, then expand **Tables**, and observe the schema that has been created **(1)**. Expand **Security > Users** to observe that the database user is migrated as well **(2)**.

    ![In the SSMS Object Explorer, Databases, parts, and Tables are expanded, showing the tables created by the deploy schema script. Security Users are expanded to show database user PUWebSite is migrated as well.](media/updated50.png "SSMS Object Explorer").

> **Note**: You can now disconnect from the **SQLVM** and perform the remaining exercises from the **LabVM**.

### Task 5: Migrate the database using the Azure Database Migration Service 

At this point, you have migrated the database schema using DMA. In this task, you migrate the data from the `PartsUnlimited` database into the new Azure SQL Database using the Azure Database Migration Service.

> The [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) integrates some of the functionality of Microsoft's existing tools and services to provide customers with a comprehensive, highly available database migration solution. The service uses the Data Migration Assistant to generate assessment reports that provide recommendations to guide you through the changes required prior to performing a migration. When you're ready to begin the migration process, Azure Database Migration Service performs all of the required steps.

1. In the [Azure portal](https://portal.azure.com), navigate to your Azure Database Migration Service by selecting the **hands-on-lab-<inject key="DeploymentID" enableCopy="false"/>** resource group, and then selecting the **parts-dms-<inject key="DeploymentID" enableCopy="false"/>** Azure Database Migration Service from the list of resources.

   ![](media/updated51.png)


1. On the Azure Database Migration Service Blade, select **+ New Migration**.

    ![](media/click_new_migration_DMS.png)

1. On the New migration project Blade, enter the following:

   - **Target server type**: Select **Azure SQL Database** **(1)**.
   - **Migration mode**: Select **Offline** **(2)**.
   - **Configure runtime settings** **(3)**.
   - When the **Configure integration runtime** pop-up appears, copy any one of the **two keys (4)** into a notebook.

     ![The New migration project blade is displayed, with the values specified above entered into the appropriate fields.](images/Select_target_preapre.png "New migration project")

1. Navigate back to the SQLVM, click the **Start** button. In the search box, type **Microsoft Integration Runtime** **(1)** then select **Microsoft Integration Runtime** **(2)** from the search results.

    ![](images/Microsoft_Integration_Runtime.png)

1. After successful installation it will ask you for the authentication key which we copied from the Azure portal, please provide the copied key, and click on **Register** and **Finish**.

   ![The Migration Wizard Select source blade is displayed, with the values specified above entered into the appropriate fields.](images/provide_cpoied_key.png "Migration Wizard Select source")

   ![The Migration Wizard Select source blade is displayed, with the values specified above entered into the appropriate fields.](images/01-04-2024(11).png "Migration Wizard Select source")

1. Once the Integration Runtime (Self-hosted) node has been **registered successfully**, minimize the SQLVM rdp window.
    
    ![](images/Microsoft_Integration_Runtime_auth.png)

1. Navigate back to **Azure Database Migration Service** in the Azure portal, in **Configure integration runtime** pop-up click on **OK** **(1)**  and click on **Select** **(2)**.

   ![The Migration Wizard Select source blade is displayed, with the values specified above entered into the appropriate fields.](images/After_integration_setup.png "Migration Wizard Select source")

1. On the Migration Wizard **Select source** Blade, enter the following:

   - **Source SQL Server instance name**: Enter the Private IP address of SqlServer2022. **(1)**.  
     >**Note** : To obtain the private IP address, search for and select **SqlServer2008** in the Azure portal. Navigate to the **Networking** section and copy the private IP address displayed there.

   - **Authentication type**: Select SQL Authentication **(2)**.
   
   - **Username**: Enter **PUWebSite** **(3)**
   
   - **Password**: Enter **<inject key="SQLVM Password" />** **(4)**.
   
   - **Connection properties**: Check both the Encrypt connection and Trust server certificate **(5)**.
   
   - Select **Next: Select databases for migration** **(6)**. 
  
     ![The Migration Wizard Select source blade is displayed, with the values specified above entered into the appropriate fields.](images/01-04-2024(5).png "Migration Wizard Select source")
   
1. The PartsUnlimited databases come preselected. Select **Next: Connect to target Azure SQL Database >>** to continue.
    
    ![The Migration Wizard Select database blade is displayed. PartsUnlimited databases is selected. Next: Select target >> button is highlighted.](images/Database_Selected.png "Migration Wizard Select databases")

1. On the Migration Wizard **Select target** Blade, enter the following:

    - **Subscription**: Provide the Subscription
    
    - **Resource Group**: Provide the Resource Group
    
    - **Target server name**: Enter the server name of your Azure SQL Database - **<inject key="sqlDatabaseName" 	enableCopy="false"/>.database.windows.net**
    
    - **Authentication type**: Select SQL Authentication.
    
    - **Username**: Enter **demouser**
    
    - **Password**: Enter **<inject key="SQLVM Password" />**
    
    - Select **Next: Map source and target databases >>**.
    
        ![](images/Connecting_to_aTarget_database.png )

1. On the Migration Wizard **Map to target databases** Blade, confirm that **PartsUnlimited** is checked as the source database, and **parts** is the target database on the same line, then select **Next: Select database tables to migrate >>**.

    ![The Migration Wizard Map to target database blade is displayed, with the ContosoInsurance line highlighted.](images/01-04-2024(6).png "Migration Wizard Map to target databases")

1. On the Migration Wizard **Configure migration settings** Blade, expand the **PartsUnlimited** database, verify all the tables are selected and select **Next: Database migration Summary >>**.

    > **Note**: If you see that table data cannot be migrated, the source table is empty. it is completely fine please select the tables that are not greyed out or the table has data in it.

    ![The Migration Wizard Configure migration settings blade is displayed, with the expand arrow for PartsUnlimited highlighted, and all the tables checked.](images/updated57new.png "Migration Wizard Configure migration settings")

1. On the migration wizard **Summary** blade, select **Start migration** and monitor the migration status on the overview page of Database Migration Service at **Migration Status**.

    ![The Migration Wizard summary blade is displayed, with PartsUnlimitedDataMigration entered into the name field.](images/updated58new.png "Migration Wizard Summary")

    ![The Migration Wizard summary blade is displayed, with PartsUnlimitedDataMigration entered into the name field.](images/01-04-2024(15).png "Migration Wizard Summary")

    > The migration takes approximately 2 - 3 minutes to complete.

1. When the migration is complete, you should see the status as **Succeeded**.

    ![On the Migration job blade, the status of Completed is highlighted.](images/01-04-2024(15).png "Migration with Completed status")
    
 ## Summary
 
In this exercise, you have migrated the on-premises database to Azure SQL Database.

### You have successfully completed the Exercise

**Click Next to proceed to the Next exercise**

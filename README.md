# Azure-SQL-Database-Security-Demo

## Overview
Security is a top concern for managing databases, and it has always been a priority for Azure SQL Database. Your databases can be tightly secured to help satisfy most regulatory or security requirements. 

In this demo, We'll walkthrough some of the Security features of Azure SQL Databases. 

**Contents**
  - [Exercise 1: Getting Started](./README.md#exercise-0-introduction-to-azure-portal)
    - [Task 1: Sign Up for pre configured environment](./README.md#task-1-sign-up-for-pre-configured-environment)
    - [Task 2: Log into your Azure Portal and Verify access to the Resources](./README.md#task-2-log-into-your-azure-portal-and-verify-access-to-the-subscription)
  

  - [Exercise 2: Control Access](./README.md#exercise-2-control-access)
    - [Task 1:Configure Azure AD Login for your Azure SQL DB](./README.md#task-1-configure-azure-ad-login-for-your-azure-sql-db)
    - [Task 2: Access the Database using SQL Server Management Studio](./README.md#task-2-access-the-database-using-sql-server-management-studio)
  - [Exercise 3: Protect Data](./README.md#exercise-3-protect-data)
    - [Task 1: Transparent Data Encryption](./README.md#task-1-transparent-data-encryption)
    - [Task 2: Always Encrypted](./README.md#task-2-always-encrypted)
    - [Task 3: Auditing](./README.md#task-3-auditing)
    - [Task 4: Threat Protection](./README.md#task-4-threat-protection)
    - [Task 5: Configure SQL Data Discovery and Classification](./README.md#task-5-configure-sql-data-discovery-and-classification)
    - [Task 6: Simulate Attack](./README.md#task-6-simulate-attack)
    - [Task 7: Vulnerability Scan](./README.md#task-7-vulnerability-scan)
    - [Task 8: Configure Dynamic Data Masking](./README.md#task-8-configure-dynamic-data-masking)
    - [Task 9: Verify data how does it look by using App](./README.md#task-9-verify-data-how-does-it-look-by-using-app)
 
## Exercise 1: Getting Started

This exercise will help you getting access to demo environment and getting started with demo lab environment. 

   * [Task 1: Sign Up for Pre-configured Environment](#exercise-01-sign-up-for-pre-configured-environment)
   * [Task 2: Log into your Azure Portal and Verify access to the Lab Environment](#exercise-02-log-into-your-azure-portal-and-verify-access-to-the-lab)
 

### Task 1: Sign Up for pre configured environment

1.	Navigate to bitly link which was provided by instructor and register by providing all required information and clicking on **SUBMIT** button.<br/>

![](images/1registernow.png)

2. Once registration is accepted, you will be automatically redirected to the lab activation page. Click on the **Launch Lab** button.<br/>

![](images/2launchlab.png)

3. You will see the environment details soon below.<br/>

![](images/labdetailpg.png)

4. Please ensure to use provided Azure Credentials during the course of Lab. 


### Task 2: Log into your Azure Portal and Verify access to the Lab

In this exercise, you will log into the **Azure Portal** using your Azure credentials. We'll be using the Jump Virtual machine provided for completing this demo. 

1. Login to **JumpVM** by clicking on **GO TO JumpVM** button on lab details page. 

![](images/gotojumpvm.png)

2.  Open Edge Browser by launching it from the desktop and Navigate to https://portal.azure.com.
3.  Enter the **Username** which was displayed in the previous window and click on **Next**.<br/>

![](images/1signin.png)

4. Enter the **Password** and click on **Sign in**.<br/>

![](images/2signin.png)

5.	In the Welcome to **Microsoft Azure** pop-up window, click **Maybe Later**. Now you have login successfully.

![](images/maybelater1.png)

6. You will see one Resource Group on which you have access. 
6. Click on **ODL-sql-security-xxxxx-SQL** Resource Group which contains the pre-deployed Azure SQL Database as shown below. Your demo environment includes a pre-created Azure SQL Database named "Clinic", loaded witha  sample data. It also includes a sample application to use through the demo hosted in an Azure App Service. 


![](images/overview1.png)


## Exercise 1: Control Access

### Task 1: Configure Azure AD Login for your Azure SQL DB

Azure AD authentication is a mechanism of connecting to Azure SQL Database and SQL Data Warehouse by using identities in Azure AD. With Azure AD authentication, you can manage the identities of database users and other Microsoft services in one central location. Central ID management provides a single place to manage database users and simplifies permission management. Let us enable Azure AD based authentication for your Azure SQL database.


1. Login into the Azure Portal, open **Resource Group** with suffix **-SQL**, navigate to the SQL Server **contososerv-suffix**.
1. Under the **Settings** blade, select **Active Directory Admin**. Here you can review your username registered as Active Directory Admin. Using this you can login to SQL Server using your AAD Credentials itself.  

![](images/activediradmin.png)


### Task 2: Access the Database using SQL Server Management Studio

In this task, We'll try accessing our **Clinic** database using SQl Server Management Studio with Azure AD Authentication

2. On start menu, search SQL and select **Microsoft SQL Server Management Studio 18**.
3. Use the following configurations then click Connect:
* Server name: enter the server name which you can copy from the overview page of SQL Server at the top right corner.

![](images/servername.png)


* authentication method: from dropdown select **Active Directory-Password**
* username: **odl_user_xxxxx@xxxxxxxxxxx.xxxxxxxxxxx.com**
* password: **youruniquepassword**

![](images/sqlauthentiction.png)


4.	You will get a login failure which will state: ***Your client IP address does not have access to the server. Sign in to an Azure Account and create a new firewall rule to enable access.*** We need to add firewall rule to allow access to Azure SQL Database from our machine. 

![](images/firewallerror.png)

### Task 3: Configure Azure SQL Database Firewall
To provide access security, SQL Database controls access with Firewall rules that limit connectivity by IP address. You can choose to allow specific IPs for database access and also enable other Azure Services by just enabling Azure Service Access. Let us create firewall rules to enable access to the database. 

1. In Azure Portal, browse to your SQL Server **contososerv-suffix**.

6. Select **Firewall and virtual networks**. Then select **ON** for **Allow Access to Azure Services** to enable firwall, then add fiirwall IP that ranges between the **Client IP address** you see in you sql server. 

![](images/firewalladip.png)

7. **Save** the changes.

![](images/firewallsave.png)

8. Now you can try logging in using SQl Server Management Studio and it should work as expected. Also you can review you SQL Database **Clinic** by expanding **Database** 

![](images/successfullogin.png)

## Exercise 3: Protect Data 

SQL Database secures customer data by providing auditing and threat detection capabilities.

### Task 1: Transparent Data Encryption 

Transparent data encryption (TDE) helps protect Azure SQL Database against the threat of malicious offline activity by encrypting data at rest. It performs real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application

1. In the Azure Portal, go to Resource Group with suffix **-SQL**, select the SQL Database **Clinic**.
2. Select **Transparent data encryption** under the **Security** blade.
3. Here you can review; transparent data encryption is already enabled. 

![](images/transdataenc.png)

### Task 2: Enable Advanced Data Security for Azure SQL Database
Advanced data security is a unified package for advanced SQL security capabilities. It includes functionality for discovering and classifying sensitive data, surfacing and mitigating potential database vulnerabilities, and detecting anomalous activities that could indicate a threat to your database. It provides a single go-to location for enabling and managing these capabilities.

Let us enable Azure Advanced Data Security for our Clinic Database Server. 

1. Open Resource Group with suffix **-SQL**, navigate to the SQL Server. Select **Advanced Data Security** under Security.
2. Use the following configurations:
* Advanced Data Security: **On**
* Subscription: **Choose your subscription**
* Storage Account: **Choose your existing storage account**
* Periodic recurring scans: **On**
* Send scan reports to: **username**
* Send Alerts to: **username**

![](images/advancedsecurity.png)

3. Select **Save**.

### Task 3: Simulate Attack 
In this task, We'll try to simulate a SQL Injection attack on our database and see how Azure Advanced Data Security reacts to it. 

1.	In the Azure Portal, open Resource Group with suffix **-SQL**, navigate to the app service **contosoapp-suffix** and select **Browse**.

![](images/appservice.png)

2.	You will be directed to **Contoso Clinic** webpage, select **Patients**, 

![](images/contosowebpage.png)

3.	In the **Search Box**, put the following code and  click on **Search**. `' UNION SELECT '0', '1', '2', STUFF((select name from sys.tables FOR XML PATH('')),1,1,''), '4', '5', '6', '7', '8', '2010-10-10' --`

![](images/searchbox.png)

4. This will perform a SQL Injection in the database. Azure SQL Security shoult detect ant notify you about the threat on the email address you provided in last step. 

5. Let us check if you recevied an email about it. You can open  https://portal.office.com in another tab in your browser. Login with same Azure Credentials you used to login to azure portal.

6. Select **Outlook**.

![](images/outlook.png)

7. Then review the email which shows details about the threat.

![](images/mailerror.png)

8.	Also, you can review threat alerts in **Azure Portal**. To check through the portal, go to SQL Database **Clinic**.
9. Open **Advanced Data Security** under Security blade, select **Advanced Threat Protection**. Here you can review the threat alert.

![](images/advthreatpro.png)
 
10. Select the **Potential SQL Injcetion**.

![](images/threat1.png)

11. Here is the database in which threat is found. Select the sql database **Clinic**.

![](images/threat2.png)

12. In this section you can review additional details about the threat. 

![](images/threat3.png)

13. If you scroll down, you should also see some remediation recommendations for the attack

![](images/threat4.jpg)

### Task 4: Auditing
Auditing an instance of the SQL Server Database Engine or an individual database involves tracking and logging events. For SQL Server, you can create audits that contain specifications for server-level events and specifications for database-level events. Audited events can be written to the event logs or to audit files. Let us enable Auditing for our SQL Db. 

1. In SQL Database, select **Auditing** under **Security** where you can review that Auditing is enabled and audit data is being stored in your storage account.

![](images/auditing.png)

2. Click on **View Audit Logs**, this will show all the database activities happened recently. You can click on the audit log to review  additional details of any activity. 

![](images/viewaudit.png)


### Task 5: Configure SQL Data Discovery and Classification

In this task, you will look at the SQL Data Discovery and Classification feature that introduces a new tool for discovering, classifying, labeling & reporting the sensitive data in your databases. It introduces a set of advanced services, forming a new SQL Information Protection paradigm aimed at protecting the data in your database. Let's get started.

1. in Azure Portal, open your SQL Database and click on the **Advanced Data Security** blade, select the **Data Discovery & Classification** tile.

![](images/dataclassification.png)

2. In the Data Discovery & Classification blade, select the info link with the message **We have found 16 columns with classification recommendations**.

![](images/16columns.png)

3. Look over the list of recommendations to get a better understanding of the types of data and classifications that can be assigned, based on the built-in classification settings.

4. Check the **Select all** check box at the top of the list to select all the remaining recommended classifications, and then select **Accept selected recommendations**.

![](images/acceptrecom.png)


5. Select **Save**.
6. When the save completes, select the **Overview** tab on the Data Discovery & Classification blade to view a report with a full summary of the database classification state.

7. Data classifications is now enabled and will be available in audit logs as well, Let us perform some data access operations using the sample app to review the audit logs with data classifications.

8. In Azure Portal, browse to the app service named **contosoapp-suffix**.

8. Select **Browse**, this will open **Contoso Clinic** webpage.

![](images/contosowebpage.png)

9. Now navigate to **Patients**. Then perform an operation by selecting **Details** of any patient . 

![](images/patients.png)

10. The actions performed on the webpage will directly get logged into audit logs which can be reviewed. 

11. In SQL Database **Clinic**, select **Auditing** under **Security** where you can review the audit logs. Select **View Audit Logs**.

![](images/viewauditlogs.png) 

12. You can review the recently activities. 

![](images/auditafterthreat1.png)

13. Click on the first activity, which will further show you details about the operation you performed in previous step (Step 9). It will show classification of the data accessed in last transaction. 

![](images/auditafterthreat2.png)




### Task 6: Vulnerability Scan

In this task, you will review an assessment report generated by ADS for the `Clicnic` database and take action to remediate one of the findings in the `TailspinToys` database. The [SQL Vulnerability Assessment service](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) is a service that provides visibility into your security state, and includes actionable steps to resolve security issues, and enhance your database security. 

1. Return to the **Advanced Data Security** blade for the **Clinic** database and then select the **Vulnerability Assessment** tile.

![](images/vulnerability-assess.png)

2. On the Vulnerability Assessment blade, select **Scan** on the toolbar.

![](images/vulnerabilityscan.png)

3. When the scan completes, you will see a dashboard, displaying the number of failing checks, passing checks, and a breakdown of the risk summary by severity level.

 ![](images/sql-mi-vulnerability-assessment-dashboard.png)

4. In the scan results, take a few minutes to browse both the Failed and Passed checks, and review the types of checks that are performed. In the **Failed** the list, locate the security check for **Transparent data encryption**. This check has an ID of **VA1219**.



5. Select the **VA1219** finding to view the detailed description.


### Task 7: Configure Dynamic Data Masking: 
 
In this exercise, you will enable Dynamic Data Masking (DDM). DDM limits sensitive data exposure by masking it to non-privileged users. This feature helps prevent unauthorized access to sensitive data by enabling customers to designate how much of the sensitive data to reveal with minimal impact on the application layer. It’s a policy-based security feature that hides the sensitive data in the result set of a query over designated database fields, while the data in the database is not changed.

1. Now navigating to SQL Database **Clinic** to add a mask.
2. Go to **Dynamic Data Masking** under **Security** blade, then select **+Add Mask**.

![](images/ddm.png)


2. Use following configurations to create a mask rule:
* Schema: **dbo**
* Table: select **Patients** from dropdown
* Column: **SSN (char)** from dropdown

![](images/mask1.png)

3. Select **Add**.
4. Add a mask again by selecting **+Add Mask**.
5.  Use following configurations:
* Table: select **Visits** from dropdown
* Column: **PatientID (int)** from dropdown
* Masking field format: **Number (random number range)**
* Enter random value for **To** and **For**.

![](images/mask2.png)

6. Then select **Add**. 
Your mask rules are ready. Let us review them using SQL Server Management Studio by logging in as a non-admin user. 

7. Launch SQL Server Management Studio and start a new Connection. Enter the Server name from your Azure SQL Database **FQDN** used earlier. Select SQl Authentication and privide user name **demoadmin** and password **Password123**. 

![](images/sqlsecnormaluserlogin.jpg.png)

8. Run following query against **Clinic** database. You'll see that PatientID column is masted with value **0**.

        `Select * from dbo.patients`

![](images/sqlsecnormaluserlogin.jpg.png)





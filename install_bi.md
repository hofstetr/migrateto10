# Installing Cognos Business Itelligence v10.1.1
---
The migration utilities install components into both Series 7 as well as BI v10.1.1 and so both must be installed and operational before being able to install the migration utilities.

## Create DB2 Database
Since Tivoli Directory server required DB2 you may as well also use it for the Content Store. 

1. Download a copy of the [DDL](createCS.sql)
2. Edit the file if necessary to add grants if not using db2admin
3. Log on as db2admin
4. Open a *DB2 Command Window - Administrator* from the start menu
5. Execute db2 -tvf C:\path\to\createCS.sql

## Install Cognos Business Intelligence
The installation is a simple next, next and finish.

1. Navigate to bisrvr_win32_10.1.1\win32
2. Execute the issetup.exe
3. At the Welcome click Next>
4. Agree to the license and click Next>
5. Choose an installation path then click Next>
6. Leave the defaults for component selections and click Next>
7. Supply a shortcut folder and make sure it is visible to all users then click Next>
8. Review the summary then click Next>

## Install Framework Manager

1. Navigate to bimodel_win32_10.1.1\win32
2. Execute the issetup.exe
3. At the Welcome click Next>
4. Agree to the license and click Next>
5. Choose the same installation path as BI then click Next>
6. Select Yes to backup and replace certain files
7. Leave the defaults for component selections and click Next>
8. Supply the same shortcut folder as BI and make sure it is visible to all users then click Next>
9. Review the summary then click Next>

## Apply Fix Pack 5
Download Fix Pack 5 for Cognos Business Intelligence v10.1.1 Win32 from [IBM Fix Central](https://www.ibm.com/support/fixcentral) and apply it.

1. Navigate to up_bisrvr_win32_10.1.1fp5\win32
2. Execute the issetup.exe
3. At the Welcome click Next>
4. Agree to the license and click Next>
5. Choose the same installation path as BI then click Next>
6. Select Yes to backup and replace certain files
7. Review the summary then click Next>

## Configure Cognos Business Intelligence
There are really only two major configuration items to be done: DB2 content store and Access Manager security.

1. First copy both DB2 JDBC driver files, db2jcc.jar and db2_license_cu.jar, from C:\Program Files\IBM\SQLLIB\java to C:\Program Files(x86)\IBM\Cognos\c10\webapps\p2pd\WEB-INF\lib
2. Launch IBM Cognos Configuration from the IBM Cognos 10 program group
3. Click on Content Store on the left
4. Supply localhost:50000 for the database server and port
5. Supply db2admin for the User ID and the password used earlier
6. Change the Database name to C10CS
7. Click Save
8. Right-click on Content Store and select test
9. Right-click on Authentication and add a new namespace
10. Provide the same name as the customer's if available
11. Select IBM Cognos Series 7 for the type and click OK
12. Supply the same name space ID as the customer's if available
13. Supply localhost:389 for the host and port
14. Supply o=sample for the Base Distinguished Name
15. Supply default for the Namespace name
16. Click on Cognos underneath Authentication
17. Change Allow anonymous access? to False
18. Click on Environment and change Gateway URI to http://localhost:9300/p2pd/servlet/dispatch
19. Click Save
20. Click Start

**Note:** Ignore the error about the mail server since we did not bother to configure that.

At this point when browsing to http://localhost:9300/p2pd/servlet/dispatch the page will not load completely because the static content would normally be hosted by a web server. To avoid having to implement a web server for this intermediate version just copy all the files from C:\Program Files (x86)\IBM\cognos\c10\webcontent to C:\Program Files (x86)\IBM\cognos\c10\webapps\p2pd.
# Installing Impromptu Administrator v7.5
------------------------------------
The tricky part about installing the old Impromput Administrator software is that it requires a supported directory to install Access Manager extensions into, which at this time is just:

* Microsoft Active Directory
* Tivoli Directory Sever

## Installing DB2 V11.1
A pre-requisite for Tivoli Directory Server is a DB2 database. The supported environments lists either v9.7 or v10.1 as supported but a more current version seems to also work well enough. Download **IBM DB2 Advanced Workgroup Server Edition Server Restricted Use V11.1 for Windows on AMD64 and Intel EM64T systems (x64) Multilingual (CNB8JML )** from [Xtreme Leverage](https://w3.ibm.com/software/xl/download/ticket.wss).

1. Navigate to SERVER_AWSE_O\image and run setup.exe as Admnistrator
2. On the Welcome page click on Install a Product on the left
3. Scroll down to and click on the Install New button that is within the first section labeled *DB2 Version 11.1   Workgroup, Enterprise and Advanced Editions*
4. Click Next
5. Accept the license and click Next>
6. Select Typical and click Next>
7. Select Install on this computer and click Next>
8. Select an installation path and click Next>
9. Select *Do not autostart the IBM SSH server* and click Next>
10. Supply a password for db2admin and click Next>
11. Click Next> to install the default DB2 instance
12. Uncheck the box to not send notifications and click Next>
13. Enable operating system security and click Next>
14. Review the summary and click Install

## License DB2
After installing DB2 it needs to be licensed or the server will quite working after the trial perioud. Download **IBM DB2 Advanced Workgroup Server Edition Restricted Use Activation V11.1 for Linux, UNIX and Windows Multilingual (CNB21ML )** from [Xtreme Leverage](https://w3.ibm.com/software/xl/download/ticket.wss).

1. Not sure we need to do this

## Installing Tivoli Directory Server
Download **IBM Tivoli Directory Server 6.3 Client-Server with entitlement (zip file) for win-x86-64, Multilingual (CZKG4ML )** from [Xtreme Leverage](https://w3.ibm.com/software/xl/download/ticket.wss).

1. Navigate to tds63-win-x86-64-base\tdsV6.3\tds and run install_tds.exe
2. Select English and click OK
3. At the Welcome dialog click Next>
4. Accept the license and click Next>
5. Make certain the previous DB2 installion and click Next>
6. Select a path to install to and click Next>
7. Select Typical so that it creates a default directory server instance
8. On the database setup dialog supply a user password as well as one for the administrator DN just in case
9. Supply a seed, I used 123456789012, then click Next>

**Note:** Remember to use a valid password for the system. In my case that was 15 alphanumeric characters. Otherwise a generic error will appear when trying to create the database instance. Also, if your account is not a local one you will probably get an error creating the directory instance. To resolve do the following:

1. Log on as db2admin which should have been created by the DB2 installation
2. Launch Tivoli Administration Tool as Administrator
3. Click the Create an instance and create a new default instance
4. Make sure to start the server as well

## Installing Impromptu Administrator
The rest should be familiar and relatively easy.

1. Navigate to impa_win32_7.5_en and run setup.exe
2. Click on *Install IBM Cognos Impromptu Administrator*
3. At the Welcome click Next>
4. Accept the License and click Next>
5. Supply pertinent user information and click Next>
6. Select Custom if you want to add extras like samples then click Next
7. Leave English selected and click Next>
8. Select an installation path and click Next>
9. Specify the shortcut folder and make sure it is visible to all users then click Next>
10. Review the summary then click Next>

## Configuring Impromptu Administrator
The only tricky part to this is making sure to first configure the Access Manager Directory.

1. Launch Configuration Manager and open the current configuration
2. Expand Services > Access Manager - Directory Server and click on General
3. Change *Are you sure you want to configure this directory server?* to yes
4. Change *Base distinguished name (DN)* to o=sample unless you created the instance with a different root
5. Change the *Unrestricted User distinguished name (DN)* to cn=root
6. Supply the *Unrestricted User password* from when the Tivoli Directory instance was created
7. Supply a password for the *Default Namespace Administrator* (typically admin1234)
8. Right-click on General on the left and select Apply Selection
9. Expand Services > Access Manager - Runtime > Authentication Source and click on Directory Server
10. Change *Base distinguished name (DN)* to o=sample unless you created the instance with a different root
11. Right-click on Authentication Source on the left and select Apply Selection
12. Right-click on the top level on the left and select Apply Selection
# Migrate the Samples
---
These steps serve as an example of the process to migrate a catalog and set of reports from Series 7 to Business Intelligence v10.1.1 based on the Impromptu samples. Details may vary for each customer such as the paths, data sources and types.

## Migrate the Catalog
The first step in the process is to migrate the catalog into a model.

1. Open an Administrator command prompt and navigate to C:\Program Files (x86)\Cognos\cer6\migs7
2. Run impcat2xml.exe "path-to-catalog" "path-to-output"
```dos
> impcat2xml.exe "..\data\samples\Impromptu\reports\gosales.cat" "..\data\samples\Impromptu\reports\gosales.xml"
(09:57:42) Starting the Impromptu Automation server.
(09:57:44) Impromptu Automation server version is: 7580
(09:57:44) IBM Cognos Rendition location is: C:\PROGRA~2\Cognos\cer6\bin
(09:57:44) Opening Catalog ...
(09:57:44) Load localized expression keywords from Impromptu Application resources.
(09:57:44) Exporting catalog header ...
(09:57:44) Exporting catalog databases . . . . . . . . . . . . . . . . . . .
(09:57:45) The number of exported tables is: 18
(09:57:45) Exporting catalog joins . . . . . . . . . . . . . . . . . .
(09:57:45) The number of exported joins is: 17
(09:57:45) Exporting catalog business view . . . . . . . . . . . . . . . . .
(09:57:45) Continuing exporting business view... . . . . .
(09:57:45) The number of exported folders is: 23
(09:57:46) Exporting catalog user classes . . . . . . . . . . . .
(09:57:46) The number of exported user classes is: 11
(09:57:46) Exporting ids map .
(09:57:46) The number of exported map entries is: 0
(09:57:46) Closing Catalog ...
(09:57:46) Shutting down the Impromptu OLE Automation server ...
(09:57:46) Writing data to the following output file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\reports\gosales.xml
(09:57:46) Catalog XML export finished
```

This creates an intermediary XML file that needs to be imported into a new model.

1. Open Framework Manager from the IBM Cognos 10 program group
2. Click on *Create a new project*
3. Supply a project name that closely resembles the catalog name
4. Choose a path for the project
5. Do not select to *Use Dynamic Query Mode*
6. Click OK
7. Log into Business Intelligence as an Administrator
8. Select English for the language and click OK
9. Select *IBM Cognos Impromptu (*.xml)* as the Metadata source and click Next>
10. Browse to C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\reports and select gosales.xml then click Open
11. Ensure the Access Manager name space is select and click Import
12. Click Finish once the import process is completed to verify the model
13. Click Verify Model on the verification dialog

**NOTE:** There will likely be a number of errors and warnings on the verification. Some will likely be the result of not having the data source created yet. However, if you select all and click repair that should resolve a number of warnings at least.

## Create the Data Source(s)
Now that the catalog is migrated into a model in Framework Manager, the required data sources can be determined.

1. Expand Data Sources on the left
2. Click on each Data Source to determine the properties
3. Expand Type in the properties window for a two letter code for the database type

|Code|Type  |
|----|------|
|OD  |ODBC  |
|OR  |Oracle|
|D2  |DB2   |

It may be necessary to find other sources to complete the information required to create a data source. For example, the above information for the samples indicate an ODBC type with a DSN of GOScer6.

1. Log into Business Intelligence as an Administrator
2. Launch *IBM Cognos Administration*
3. Click on the Configuration tab to reveal the Data Source Connections
4. Click on the cylinder icon to create a New Data Source
5. Supply the exact same name from the information in the model and click Next>
6. Select ODBC for the Type and click Next>
7. Supply the same database name, GOScer6 in this case
8. Select *No Authentication* in this case
9. Click on *Test the connection* and click the Test button
10. Once that is successful click Close twice to get back
11. Click Finish

**NOTE:** It may be necessary to revisit the model verification and correct any remaining errors from the migration.

## Publish the Package
The package must be published before the reports themselves can be migrated because the migration utility has to link the reports to the package.

1. Return to the Framework Manager model
2. Expand Packages on the left
3. Right-click on the migrated package, gosales.cat
4. Select *Publish packages*
5. Select *IBM Cognos 10 Content Store* if it isn't then click Next>
6. Log on to Business Intelligence as an Administrator if your session has timed out
7. Do not add any security at this stage, just click Next>
8. Uncheck *Verify the package* since the model should already have been verified
9. Click Publish then Finish when done

**NOTE:** The name of the package will be the name of the Impropmtu catalog file. If that needs to be changed now rather than later then during the migration deployment step an XML file that maps the catalog to package name will have to be modified.

## Migrate the Reports
This step will convert each .imr report into an .xml report specification. It is an intermediate step and does not produce the deployment. Instead it produces a folder with metadata information along with the report specifications.

1. Open an Administrator command prompt and navigate to C:\Program Files (x86)\Cognos\cer6\migs7
2. Run migratefroms7.exe "path-to-reports" "migration-folder"

```dos
> migratefroms7 "..\data\samples\Impromptu\reports" "..\data\samples\Impromptu\migration"
(12:03:09) Creating report object: simple_crosstab_report.imr
(12:03:09) Creating report object: singlepage.imr
(12:03:09) Generating folders file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7\_SUPPORT\deployment\folders.xml
(12:03:09) Generating accounts file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7\_SUPPORT\deployment\accounts1.xml
(12:03:09) Generating content file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7\_SUPPORT\deployment\content.xml
(12:03:09) Generating export record file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7\_SUPPORT\deployment\exportRecord.xml
(12:03:09) Generating data source file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7\_SUPPORT\deployment\dataSource.xml
(12:03:09) Starting the Impromptu Automation server.
----------------------------------------
(12:03:10) Successfully terminated all processing providers.
----------------------------------------
(12:03:10) Series 7 Migration processing is complete.
(12:03:10) Session terminated successfully.
----------------------------------------
(12:03:11) Creating an archive file of the conversion results: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7\deployfroms7.zip
```

## Deploy the Reports
This step transforms the above metadata and report specifications into a deployment that can be imported into Business Intelligence v10.1.1 using the standard import process.

If the package name was change during publishing then change the mapping to match.

1. Navigate to the _SUPPORT\maps folder within the migration content
2. Edit the nameMap.xml file and change the package name to match

Now create the deployment which will be automatically copied to the deployment folder of Business Intelligence when it is on the same system as is the case.

1. Open an Administrator command prompt and navigate to C:\Program Files (x86)\IBM\cognos\c10\migdeploy
2. Run deployfroms7.bat --user user-id --password user-pwd "migration-folder" "deployment-folder"

```dos
> deployfroms7.bat "C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\migration\20200408T170207.windows10.migratefroms7" "C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\deployments" --user administrator --password admin1234 --namespace accman
(12:14:35) Processing the following file: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\deployments\20200408T121425.windows10.deployfroms7\reports\_TRANSFORMED\singlepage.imr.xml
(12:14:35) Fixing V4 reports.............................
(12:14:36) Pass 2: Provider PPX
(12:14:36) Pass 3: Provider NameMap
(12:14:36) Pass 3: Provider IMP
(12:14:36) Pass 3: Provider PPX
(12:14:36) Terminate providers.
(12:14:36) Inserting reports into deployment packages......
(12:14:37) Create a deployment archive.
(12:14:37) Copy the deployment archive.
(12:14:37) The deployment archive C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\deployments\20200408T121425.windows10.deployfroms7\_SUPPORT\deployment\_DEPLOYMENT\20200408T170207.zip is being copied to: C:/Program Files (x86)/IBM/cognos/c10/deployment
(12:14:37) End.
(12:14:37) Finished with 0 errors and 0 warnings.
(12:14:37) Please refer to the following log file for details: C:\Program Files (x86)\Cognos\cer6\data\samples\Impromptu\deployments\20200408T121425.windows10.deployfroms7\_LOG\viewMigrationLog.html
```

**NOTE:** The migration folder will contain date/timestamp folders for each attempt. The deploy utility requires you to specify which migration attempt so include that folder in the command as in the example.

## Import the Deployment
The migrated reports can now be imported into Business Intelligence v10.1.1 and tested.

1. Log into Business Intelligence as an Administrator
2. Launch *IBM Cognos Administration*
3. Click on the Configuration tab
4. Click on Content Administration on the left
5. Click the *New Import* icon to create a new deployment import
6. Select the correct file based on the timestamp and click Next>
7. I recommend changing the information to something more meaningful
8. Click Next> three times
9. Click Finish to save and run the import once
10. Click Run to import now
11. Check *View the details* in order to review the import status then click OK
12. Occasionally click the Refresh link until it is complete
13. Finally, review the messages for errors

The Impromptu samples had the following two errors for one report along with several informational messages:

```
CM-UPG-4030 The object was not upgraded. CM-SYS-5183 While upgrading the object "/content/folder[@defaultName="reports"]/report[@defaultName="Order_Income_Correlation_Chart.imr"]", Content Manager received the following message from the plugin: "RSU-SPC-0016 The report to be upgraded had the following validation error: "Element type "unsupportGraph" must be declared.".".     Public Folders > reportsOrder_Income_Correlation_Chart.imr 
CM-UPG-4030 The object was not upgraded. CM-SYS-5183 While upgrading the object "/content/folder[@defaultName="reports"]/report[@defaultName="Order_Income_Correlation_Chart.imr"]", Content Manager received the following message from the plugin: "RSU-SPC-0016 The report to be upgraded had the following validation error: "The content of element type "chart" must match "(background|chartBody|title|subtitle|footer|statistics|marker|legend|note|axis|categoryItemTruncationText|chartLevel|chartMeasure|(areaChart|barChart|bubbleChart|columnChart|combinationChart|lineChart|paretoChart|pieChart|polarChart|quadrantChart|radarChart|progressiveChart|scatterChart)|(style|XMLAttribute|conditionalStyle)*|drillTarget)+".".".     Public Folders > reports > Order_Income_Correlation_Chart.imrOrder_Income_Correlation_Chart.imr 

```

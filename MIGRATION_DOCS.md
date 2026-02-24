 Announcement
Joget DX 9 is out now. Explore features and download guide.

×
logo Knowledge Base 
Home
Spaces
 Questions
Joget DX 9 Knowledge Base
Introduction
Getting Started
Product & Licensing
Deployment and Installation
System Requirements
Installation
Native Mobile App
Installing Joget DX 9
Set Up a Database
Upgrading to Joget DX 9
Migrating plugins from Joget DX 8 to Joget DX 9
Advanced Installation Guides
Deployment Optimization
Configuration
Maintenance
Troubleshooting
Backup and Recovery
Joget DX 8 Clustering and Performance Testing
How-To Guides
Process Automation
User Reference
 Home 
Deployment and Installation 
Installation 
Upgrading to Joget DX 9
Upgrading to Joget DX 9
Introduction
Run Joget DX 9 in a new folder with an existing database
Run Joget DX 9 in a new folder and use the default new database
Run Joget DX 9 in my existing Joget Workflow v5, v6, or Joget DX 7, DX 8 folder and upgrade to Joget DX 9 using jw.war
Changes in Joget DX installation files
Post installation notes
Additional documentation
General upgrade flow
DX 8 and DX 9 database table changes over DX 7
Transitioning from Professional to Enterprise edition
Related Documents
Introduction
Upgrading to the newest version of Joget DX 9 from older versions, such as Joget Workflow v5 and v6 or Joget DX 7 and DX 8, is a streamlined process. There are two recommended upgrade options to choose from:

Install Joget DX 9 in a new folder with an existing database
Run a fresh installation of Joget DX 9
This document provides detailed, step-by-step guides on performing the upgrade process using any of the above options. For more information about the newest version of Joget, see What's New in Joget DX 9 and Download Joget DX 9 from https://www.joget.org/download/.

Requirements for Joget DX 9

Java: Starting from Joget DX 9, Java 17 and above is required.
Tomcat: Only Apache Tomcat 11 and above are supported.
AspectJ: Only versions 1.9.21 and above are supported for Java 17 (Joget DX 9 installation is running on 1.9.22).
Glowroot: Only versions 0.14.0 and above are supported for Java 17 (Joget DX 9 installation is running on 0.14.2).
For existing installations, you will need to manually copy the aspectjweaver.jar and glowroot folder and update the JAVA_OPTS.
Refer to System Requirements for more information.
Note
With the latest requirements to run Java 17 or higher, Joget DX 9 introduces automatic migration of plugins from the legacy javax.* namespace to the jakarta.* namespace. During this process, intermediary files such as .lock and .transformed may be created, and upon completion, you will see both the transformed .jar file and the untouched .original file for safekeeping. Refer to Migrating plugins from Joget DX 8 to Joget DX 9 for more information.
Run Joget DX 9 in a new folder with an existing database
When upgrading from Joget DX 7 or earlier, you can run Joget DX 9 in a new installation folder but using the existing database. Follow these steps:

Back up your Joget Workflow v5, v6, or Joget DX 7 database or clone it for Joget DX 9 use (Joget DX 9 will automatically create the new tables and fields it needs).
Download Joget DX 9 from Joget's Website and install it in a new folder using the Windows or Linux Installer.
Copy all files from your Joget Workflow v5, v6, Joget DX 7, or DX 8 .\wflow\ folder to the same corresponding folder in Joget DX 9.
Ensure Joget DX 9 can access your current database (check the database settings in the app_datasource-default.properties file located in the \wflow folder using a text editor.)
Optionally, edit the .\apache-tomcat-11.x.x\conf\server.xml file if you wish to run Joget DX on a different port instead of 8080.
Edit the joget-start.bat or .sh file using a text editor to set a higher -Xmx memory for better performance (for example, -Xmx1024M).

Start Joget Apache Tomcat and monitor the joget.log and catalina.log for errors. Refer to Web App Log Viewer for more information.

Run the following query after migrating from DX8 to DX9 to ensure all roles are available:

For Linux:

 mysql -uroot -e "SHOW DATABASES" | awk '/^jwc_/ {print "USE ", $1, "; ALTER TABLE app_app ADD createdBy varchar(255) DEFAULT NULL; INSERT INTO dir_role VALUES (\"ROLE_SYSTEM_MANAGER\",\"System Manager\",\"System Manager\"), (\"ROLE_APP_CREATOR\",\"App Creator\",\"App Creator\"); "}' | mysql -uroot -f
SQL
For Windows:

for /f "skip=1" %D in ('mysql -uroot -p -N -e "SHOW DATABASES LIKE 'jwc_%';"') do mysql -uroot -p -f -e "USE %D; ALTER TABLE app_app ADD createdBy varchar(255) DEFAULT NULL; INSERT INTO dir_role VALUES ('ROLE_SYSTEM_MANAGER','System Manager','System Manager'), ('ROLE_APP_CREATOR','App Creator','App Creator');"
SQL
Copy your Joget plugins from the previous instance of Joget to the new Joget folder, if any. The Joget plugins are located in the .\wflow\app_plugins folder.
Copy the new App Center from joget-enterprise-9.0.0/apache-tomcat-11.0.10/webapps/jw/WEB-INF/classes/setup/apps/APP_appcenter7-1.zip into your local folder.
Import this new App Center into joget from the current App Center home page (http://localhost:8080/jw/web/userview/appcenter/home/_/home)
Publish the newly imported App Center.
Reload appcenter home page (http://localhost:8080/jw/web/userview/appcenter/home/_/home).
After these steps, your Joget is upgraded to DX 9 in a new folder while using the previous database and with your apps intact.

Run Joget DX 9 in a new folder and use the default new database
This is the fastest upgrade option, especially for a development server. Follow the steps below:

Download Joget DX 9 from Joget's Website and install it in a new folder using the Windows or Linux Installer.
Run the joget-enterprise-setup-9.0.x.exe installer if you are using Windows, or unzip the joget-enterprise-setup-9.0.x.tar.gz if you are using Linux.
Joget DX comes with a MariaDB database by default in the Windows installation package. 
However, you can change the default database to MSSQL, Oracle or PostgreSQL.
Follow the on-screen installer prompt if you are on Windows.
Run the joget-start.bat or joget-start.sh script to start Joget DX 9.
In your browser, type in the URL address http://localhost:8080/jw to run Joget DX 9.
Export all the apps from Joget Workflow v5, v6, Joget DX 7, or DX 8 \wflow\ folder and import them into Joget DX 9.
Reimport all plugins to Joget DX 9 by copying the plugins from the previous Joget installation instance to the new instance. The Joget plugins are located in \wflow\app_plugins.
Once finished, your Joget is upgraded to DX 9 with a new database and the apps of your previous Joget installation.

Run Joget DX 9 in my existing Joget Workflow v5, v6, or Joget DX 7, DX 8 folder and upgrade to Joget DX 9 using jw.war
Major Version Upgrade Warning
Previously, it was possible to upgrade your Joget version while maintaining your existing Joget folder. It is recommended to use the methods mentioned above when upgrading to Joget DX 9 due to Joget DX moving to Apache Tomcat 11.0.2. When upgrading from Apache Tomcat 9 to 11, do not reuse configuration files directly. Tomcat 11 introduces breaking changes across multiple versions, including the transition to Jakarta EE 11, removal of deprecated APIs, and changes in internal behavior.

As stated in the Apache Tomcat Upgrade Guide:
“For major upgrades (e.g., 9.0 to 11.0), it is recommended that a new Tomcat 11 installation is created and configured from scratch. Configuration files should not be copied from the old installation.” 

Since this is a 3 version jump, follow the Tomcat 10.0.x Migration Guide, Tomcat 10.1.x Migration Guide and Tomcat 11.0.x Migration Guide. According to the Apache Tomcat Upgrade Guide:
"If you are upgrading past several versions at once, you should read all the migration guides in between. For example, if you are upgrading from Tomcat 8.5 to Tomcat 10.1, you should read the "Tomcat 9.0 Migration Guide", the "Tomcat 10.0 Migration Guide", and the "Tomcat 10.1 Migration Guide"."

If you still intend on upgrading this way, do not upgrade directly to a production server without prior testing. The usual precautions apply: perform a full backup of the servers, and it is essential to test any upgrade on a staging or development server first. See General upgrade flow for more information. Ensure that a system admin familiar with Joget and Apache Tomcat is performing this operation.

Changes in Joget DX installation files
If you are using Joget v5 or Joget v6, please read the following changes that were introduced in Joget DX 7, 8, and 9:

Joget DX uses Glowroot for Application Performance Management, thus a new -javaagent argument is needed in the startup joget-start.bat or .sh script. See the example below: 
set JAVA_OPTS=-Xmx768M -Dwflow.home=./wflow/ -javaagent:./wflow/aspectjweaver-1.9.22.jar -javaagent:./wflow/glowroot/glowroot.jar
Bash
Joget DX has new runtime Glowroot files in .\wflow\glowroot\. The new Application Performance Management feature uses the Glowroot runtime. You can retrieve these files from a fresh install of Joget DX 9.
Joget DX has a higher default maximum memory allocation pool for the JVM in -Xmx1024M. Joget DX requires more Java heap space, and if your server has additional RAM, allocate more -Xmx@ memory for better performance.
Joget DX 9 installation is running on apache-tomcat-11.0.2.
Joget DX 9 installation is running on Java jre21.0.5.
Joget DX 9 installation is running on aspectjweaver-1.9.22.jar.
Post installation notes
Use this guide Troubleshooting - Common Errors, to learn how to solve start-up errors in your Joget DX.

Download and install new plugins, especially for Joget DX 7-DX 9 from https://archives.joget.org/addons/ and https://marketplace.joget.com/ to try out:

Process Enhancement Plugin
Report Builder
API Builder
To save time in the initial DX 9 testing, you can delay the copying of the .\wflow\app_formuploads folder (may be too many files) and .\wflow\app_plugins folder (to first test Joget DX with zero custom plugin) until after everything is running smoothly.

Additional documentation
General upgrade flow
Notes:

Compatibility: The usual precautions apply; perform a full backup of the servers, and it is essential to test any upgrade on a staging or development server first.
Licensing: For the Enterprise & Professional Edition, upgrades between major versions (e.g., v5-DX 7) to DX 9 require reactivation with a new license, so users with an active Enterprise Software Subscription are required to request a new license.
Prepare a test server that mimics the production server as closely as possible in all possible aspects (e.g., user setup, networking environment, CPU/memory capabilities, database) without cloning the production database server. Start with a fresh new database.
Once you are ready with the test server, you may try to start with a fresh database without the data, but just the Apps loaded in. Run through all the functionalities of your apps to see if everything works as expected.
If Step 2 goes well, you may then try to clone the existing production database to see how your Apps fare with the existing production data. Run through all the functionalities of your Apps again to see if everything works as expected.
If you have integrated Joget Workflow with other solutions, you will also need to test them accordingly.
When you are ready, please continue to the next step.
DX 8 and DX 9 database table changes over DX 7
These are the new tables that will be automatically created upon initial start-up of Joget DX 9 over the existing database used by Joget DX 7:

wf_history_activity
wf_history_process
wf_process_link_history
In the unlikely case that you need to create the tables manually, you can locate the CREATE script in exploded jw.war file at \jw\WEB-INF\classes\setup\sql\.

For the MySQL database, if you are setting it over a new database using the Set Up Database wizard, the default collation is now utf8mb4_unicode_ci instead of utf8_unicode_ci in Joget DX 7.

Disabling Auto Collation
From Joget DX 8.0.11 onwards, should you wish to disable the auto collation, you can add the following parameter in the Joget startup batch file, in JAVA_OPTS.
-Dwflow.collationChecking=false
Bash
Transitioning from Professional to Enterprise edition
Joget DX Professional Edition will no longer be available starting October 2024. This decision was made to streamline the Joget product lineup and focus Joget's efforts on delivering the best possible solutions to meet the evolving needs of our customers and partners.

This step will guide you to transition from Professional to Enterprise Edition.

Follow these steps:

Update Joget.
Run the Enterprise version to get the system key, and apply for a new license.
Head over here to acquire New License.
IMPORTANT:
Note that the license approval SLA is 24 hours. To avoid any delay, please plan your license activation in advance.
Related Documents
Migrating plugins from Joget DX 8 to Joget DX 9
Created by Aadrian
Last modified by Debanraj Ravindran on Jan 19, 2026
Tags: installation-options joget-dx8-upgrade database-migration infra
 
Previous
 Using Windows Authentication for Microsoft SQL Server
Next
Migrating plugins from Joget DX 8 to Joget DX 9 

Copyright © 2025 Joget Docs.

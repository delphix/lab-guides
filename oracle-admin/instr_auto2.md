![](RackMultipart20201027-4-1999mua_html_720ff838d1cc552a.png)

![](RackMultipart20201027-4-1999mua_html_c1127e3af53cf0f0.gif)

# Delphix Admin Training for Oracle Virtualization

# LAB GUIDE

# Table of Contents

**[Table of Contents](#TOC)**

**[Background](#_background)**

**[Getting Started](#_gettingStarted)**

[Welcome to the Delphix Admin Training for Oracle Lab Guide](#_welcome)

[Lab Requirements](#_reqs)

[The Delphix Admin Training Cloud Lab](#_clouLab)

[Important IP Addresses](#_IPs)

[Cloud Lab Usernames and Passwords](#_usrs)

**[Paths](#_paths)**

**[Guide Layout](#_layout)**

**[Lab Exercises](#_exs)**

[Exercise 1 – Logging into the Delphix Engine UI](#_ex1)

[Exercise 2 – Create the &quot;delphix\_db&quot; User](#_ex2)

[Exercise 3 – Validate the Source and Target Environment with Hostchecker](#_ex3)

[Exercise 4 – Add a Source Environment](#_ex4)

[Exercise 5 – Link a dSource](#_ex5)

[Exercise 6 – Add Target Environments](#_ex6)

[Exercise 7 – Provision a VDB](#_ex7)

[Exercise 8 – Refresh a VDB](#_ex8)

[Exercise 9 – Rewind a VDB](#_ex9)

[Exercise 10 – Set a New Retention Policy](#_ex10)

[Exercise 11 – Create and Save a Hook Operation Template](#_ex11)

[Exercise 12 – Create a VDB Template](#_ex12)

[Exercise 13 – Provision a VDB with Hook and VDB Template](#_ex13)

[Optional Advanced Exercise – Discover and Link a 12c Container Database dSource](#_ex14)

[Optional Advanced Exercise – Provision a Virtual PDB (vPDB)](#_ex15)

[Optional Advanced Exercise – Measure Network Performance Test through the CLI](#_ex16)

[Optional Advanced Exercise – Configure Delphix Replication](#_ex17)

# <a id="exercise1"></a>Background

This guide will get you started with Delphix&#39;s virtualization for Oracle. You will learn how to to securely copy and share datasets. Using virtualization, you will ingest data sources and create virtual data copies, which are full read-write capable database instances that use a small fraction of the resources a normal database copy would require.

Virtualization has two interfaces: management and self-service. Through the management interface, DB administrators connect to source datasets and control decisions on resources such as virtual databases. This guide focuses on that interface. The self-service interface is used by non-admin users to access and operate on the data that was provided by the administrators. A guide for self-service can be found here.

Guides for other Delphix&#39;s products such as masking and data control tower can be found here and here respectively.

# <a id="exercise1"></a>Getting Started

## <a id="exercise1"></a>Welcome to the Delphix Admin Training for Oracle Lab Guide

This guide is a supplement to the Delphix Admin Training for Oracle course, and provides several exercises to perform throughout the class. If you encounter any issues during the exercises, please do not hesitate to ask your instructor for advice.

## <a id="exercise1"></a>Lab Requirements

In order to perform these lab exercises, you will need:

- A modern HTML5 capable web browser (IE9+, Chrome, Firefox, Safari)

## <a id="exercise1"></a>The Delphix Admin Training Cloud Lab

Your instructor should have provided you with a **Class Name** and a **Student Number**. In order to access your lab server, point your web browser to: http:// **classname**.agile.today/ **studentnumber**

For example, if your **Class Name** is &quot;acmetech&quot; and your **Student Number** is **5** , you would go to the site: [http://acmetech.agile.today/5](http://acmetech.agile.today/5)

![](RackMultipart20201027-4-1999mua_html_510f7a9764cd968d.png)

The Delphix Labs Login Dialog

At the login screen, enter the username and password: delphix/delphix

Once you have logged in, you will be connected to your lab server. This server contains everything you will need to perform your lab exercises including:

- Terminal with SSH (or Putty) to connect to Linux source/target database servers
- Chrome Web Browser to connect to your Delphix Data Platform
- A copy of this lab guide
- Notepad for class notes
- Oracle SQL Developer for remote connections

**IMPORTANT NOTE:** Do not use the &quot;Log Out&quot; function on your lab server. If you do, it will break your lab connection.

![](RackMultipart20201027-4-1999mua_html_6fe630a6fb2d091.png)

The Delphix Lab Server

## <a id="exercise1"></a>Important IP Addresses

| **Delphix Data Platform** | 10.0.x.10 |
| --- | --- |
| **Linux Source** | 10.0.x.20 |
| **Linux Target** | 10.0.x.30 |

In the above IP addresses, the **x** denotes your **Student Number**. For example, if your student number is **5** , your Delphix Data Platform will be located at 10.0. **5**.10.

## <a id="exercise1"></a>Cloud Lab Usernames and Passwords

| Initial Delphix **sysadmin** password | sysadmin |
| --- | --- |
| Initial Delphix **admin** password | delphix |
| Source and Target **delphix** user password (via SSH) | delphix |
| Source and Target **oracle** user password (via SSH) | delphix |

## <a id="exercise1"></a>Paths

| **hostchecker** (after tar extraction) | /home/delphix/hostchecker |
| --- | --- |
| **Oracle XE 11g** ORACLE\_HOME | /u01/app/oracle/product/11.2.0/xe |
| **Toolkit** | /u01/app/toolkit |

# <a id="exercise1"></a>Guide Layout

The **Lab Exercises** in this guide provide the full details needed to perform each operation.

- **Bold** text is used to highlight application names (e.g. **Terminal** ), keywords found in the UI (e.g. **Add**** Environment**) and also for words that are part of an action (e.g. Click**Submit**)

- ***Bold and Italicized*** text is used to highlight text that must be typed in the UI, rather than selected (e.g. Enter ***devdb***_)_.

- Courier font is used to reference commands entered on the command line or in sqlplus (e.g. Type sqlplus / as sysdba)

- Dark Red is used to emphasize Notes

-

# <a id="exercise1"></a>Lab Exercises

Perform these exercises when instructed by your Delphix Instructor.

## <a id="exercise1"></a>Exercise 1 – Logging into the Delphix Engine UI

In this exercise, you will:

- Access the Delphix Data Platform GUI as the Delphix admin user
- Access the Delphix Data Platform Setup GUI as the sysadmin user

**Context**

The Delphix virtualization GUI has two layers: management and setup. The management GUI is accessed by admin users who control data assets and objects. The setup GUI is controlled by the sysadmin who manages the engine and creates new admin users.

Alternatively, the engine can be accessed and controlled through the [CLI](https://docs.delphix.com/docs/developer-s-guide/command-line-interface-guide/command-line-interface-overview) or [APIs](https://docs.delphix.com/docs/developer-s-guide/web-services-api-guide)

**Steps**

1. Connect to your Delphix Engine using Chrome on your lab server (see the **Important IP Addresses** section of the Getting Started guide above). The IP address will be 10.0.x.10, where &quot;x&quot; is your **Student Number**
2. Login to the Delphix Dynamic Data Platform using the username ***admin*** and password ***delphix***
3. Click **Log In**

![](RackMultipart20201027-4-1999mua_html_4df0fe5015353ccf.png)

1. In order to become more familiar with what is available here, browse around the various sections of the landing page.
2. Click **admin** on the upper right side and **logout**
3. Click **Setup** (Located under **Password)** to navigate to **Delphix Setup** login
4. Login to **Delphix Setup** using the username ***sysadmin*** and password ***sysadmin***
5. Click **Log In**

![](RackMultipart20201027-4-1999mua_html_a72c298c099fe58f.png)

1. In order to become more familiar with what is available here, browse around the various sections of the landing page.

**Related Links**

[The admin and sysadmin User Roles](https://docs.delphix.com/docs/configuration/user-and-authentication-management/users-and-groups)

[Setting Up the Delphix Data Platform](https://docs.delphix.com/docs/deployment/installation-and-initial-system-configurations/initial-setup)

## <a id="exercise1"></a>Exercise 2 – Create the &quot;delphix\_db&quot; User

In this exercise, you will:

- Create a Delphix DB User on your source database

**Context**

Delphix requires access to database users with certain privileges. These are configured through automated scripts.

**Steps**

1. Use the **Terminal** app on your Jumpbox desktop to connect into your Linux Source (see the **Important IP Addresses** section of the Getting Started guide above).
  1. Type ssh 10.0.x.20 (where &quot;x&quot; is your **Student Number)**
2. Extract the **hostchecker\_linux\_x86.tar** file in your home directory
  1. Type ls -ltr
  2. Type tar -xvf hostchecker\_linux\_x86.tar
3. Navigate to the hostchecker directory
  1. Type cd hostchecker
  2. Type ls -ltr (observe the files present in this folder, as we will be using them again)
4. Set the Oracle environment variables below:
  1. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  2. Type export ORACLE\_SID=orcl

Note: &quot;. oraenv&quot; can be used to set the environment variable rather than export commands

1. Run the script to create the delphix\_db user
  1. Type ./createDelphixDBUser.sh
2. Use the following details during the running of this script:
  1. The user will be created in the default instance **orcl**. This is set by the environment variable ORACLE\_SID.

Press **Enter** to accept the default database instance **orcl**

  1. Delphix DB User Username:delphix\_db
  2. Delphix DB User Password: delphix\_db
  3. Decline the SELECT ANY DICTIONARY privilege. This is a sweeping privilege that is not required, but optional
  4. Type: nand press **Enter**

![](RackMultipart20201027-4-1999mua_html_6fcef34a41c4a410.png)

The script should create the user without error.

## <a id="exercise1"></a>Exercise 3 – Validate the Source and Target Environment with Hostchecker

In this exercise, you will:

- Use the hostchecker program to run validation tests on your Linux Source
- Use the hostchecker program to run validation tests on your Linux Target

**Steps to Validate the Source Environment with Hostchecker**

1. Connect to your Linux Source by opening **Terminal** on your Lab Server
  1. Type ssh 10.0.x.20 (&#39;x&#39; will be your **Student Number** ).
2. Set the Oracle environment variables below:
  1. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  2. Type export ORACLE\_SID=orcl
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
3. Ensure you are inside the hostchecker location
  1. Type cd /home/delphix/hostchecker
4. Run Hostchecker utility
  1. Type ./hostchecker.sh
5. Indicate that this machine is a source
  1. Type source
6. Review the available checks that can be run on this system
7. Type 1 and press **Enter**
  1. The script will check homedir permissions and return SUCCESS and ALL OK

![](RackMultipart20201027-4-1999mua_html_6e468a4464853d2d.png)

1. Type 3 and press **Enter**
  1. Enter an IP address of: 10.0.x.10 (&#39;x&#39; will be your **Student Number** ).
  2. Enter the port: 8415
  3. The script will test the port and return SUCCESS and ALL OK.
2. Repeat option 3 for the following ports: 8341 and 873
3. Type 5 and press **Enter**
  1. Type 5 to select the ORACLE\_HOME associated with the &#39;orcl&#39; database (/u01/app/oracle/product/11.2.0/dbhome\_1), and press **Enter**
  2. The script will test the Oracle Home and return SUCCESS and ALL OK
4. Type 6 and press **Enter**
  1. Type 3 to select the Oracle Instance associated with the &#39;orcl&#39; database, and press **Enter**
  2. Provide the Oracle home path at the prompt: /u01/app/oracle/product/11.2.0/dbhome\_1
  3. Enter the username and password of the Oracle database user created in exercise 2

Username: delphix\_db

Password: delphix\_db

Note: If you created a database user with different credentials from the one shown above, you will enter it at the prompt instead.

  1. The script will test the Oracle Instances and return SUCCESS and ALL OK
1. Type 7 and press **Enter**
  1. The script will test the /etc/oratab file and return SUCCESS and ALL OK
2. Type 8 and press **Enter**
  1. Enter a password of: delphix
  2. The script will test the SSH connectivity to the host and return SUCCESS and ALL OK.
3. Type 9 and press **Enter**
  1. The script will return a WARNING due to permissions. This is normal.
4. Type 10 and press **Enter**
  1. Enter a password of: delphix
  2. The script will test sudo privileges and return SUCCESS and ALL OK
5. Type 11 and press **Enter**
  1. Enter a path of: /u01/app/toolkit
  2. The script will test the path and return SUCCESS and ALL OK
6. Type quit to exit hostchecker.

Were all tests successful? If not, which ones failed and why?

Note: In a production installation, the sshd\_config test will return a WARNING response due to permissions on the file. If hostchecker is run as root, for this test, it will perform the test properly.

**Steps to Validate the Target Environment with Hostchecker**

1. Connect to your Linux Target A by opening **Terminal** on your Lab Server
  1. Type ssh 10.0.x.30 (&#39;x&#39; will be your **Student Number** ).
2. Extract the **hostchecker\_linux\_x86.tar** file in your home directory
  1. Type ls -ltr
  2. Type tar -xvf hostchecker\_linux\_x86.tar
3. Ensure you are inside the hostchecker location
  1. Type cd /home/delphix/hostchecker
4. Run Hostchecker utility
  1. Type ./hostchecker.sh
5. Indicate that this machine is a target
  1. Type target
6. Review the available checks that can be run on this system

1. Type 1 and press **Enter**
  1. The script will check homedir permissions and return SUCCESS and ALL OK

![](RackMultipart20201027-4-1999mua_html_92a875f13bfe9a0b.png)

1. Type 3 and press **Enter**
  1. Enter an IP address of: 10.0.x.10 (&#39;x&#39; will be your **Student Number** ).
  2. Enter the port: 8415
  3. The script will test the port and return SUCCESS and ALL OK.
2. Repeat option 3 for the following ports: 873, 22, 80 and 443
3. Type 5 and press **Enter**
  1. Type option 4 select the current ORACLE\_HOME value, and press **Enter**
  2. The script will test the Oracle Home and return SUCCESS and ALL OK
4. Type 6 and press **Enter**
  1. The script will test the /etc/oratab file and return SUCCESS and ALL OK
5. Type 7 and press **Enter**
  1. Enter a password of: delphix
  2. The script will test the SSH connectivity to the host and return SUCCESS and ALL OK.
6. Type 8 and press **Enter**
  1. The script will return WARNING due to permissions. This is normal.
7. Type 9 and press **Enter**
  1. Enter a password of: delphix
  2. The script will test sudo privileges and return SUCCESS and ALL OK
8. Type 10 and press **Enter**
  1. Enter a path of: /u01/app/toolkit
  2. The script will test the path and return SUCCESS and ALL OK
9. Type quit to exit **hostchecker**.

If you have completed all of the checks and they have returned SUCCESS and ALL OK, you have completed this exercise.

Note: In a production installation, the sshd\_config test will return a WARNING response, due to permissions on the file. If hostchecker is run as root, for this test, it will perform the test properly.

## <a id="exercise1"></a>Exercise 4 – Add a Source Environment

In this exercise, you will:

- Connect Delphix to your Source Oracle Database server
- Create a Source Environment

**Context**

Before users can create their own virtual databases, Delphix needs to connect to source data. In Delphix, an environment is either a single instance host or cluster of hosts that run database software. For example, a Linux system running Oracle. the environment is where the Delphix engine will search for available data sources. Credentials to access the host need to be provided while configuring an environment.

**Steps**

1. Log into the Delphix Data Platform UI as the ***admin*** user and the password that was set during the Engine setup.

Note: If you forgot your admin password, please ask your lab administrator to reset it.

1. Click the **Manage** menu and select **Environments** from the list
2. Click the ellipses ( **…** ) next to **Environments** then choose **Add Environment**

1. On the Host and Server select the following:
  1. Host OS: **Unix/Linux**
  2. Server Type: **Standalone**

![](RackMultipart20201027-4-1999mua_html_4598de611226ce18.png)

1. For Environment Settings, provide the following information
  1. Environment Name: Source
  2. Host Address: ***10.0.x.20*** (&#39;x&#39; will be your **Student Number** )
  3. NFS Addresses: ***10.0.x.20*** (&#39;x&#39; will be your **Student Number** )
  4. Leave the **DSP** fields blank
  5. Set **Login Type** to **Username and Password**
  6. OS Username: ***delphix***
  7. OS Password: ***Delphix***

Note: Enter the Toolkit Path before Validating the Username and Password

  1. Toolkit Path: /u01/app/toolkit
  2. Click **Validate**

![](RackMultipart20201027-4-1999mua_html_3389ea14f6bf76ad.png)

1. Click **Next**
2. Click **Submit**
3. You can view the status of the environment creation and discovery by clicking on the **Actions** menu on the top right-hand side of the page. Clicking on the jobs lower right corner, in the Actions pane, will allow you to track its progress

![](RackMultipart20201027-4-1999mua_html_596622a9338308ba.gif) ![](RackMultipart20201027-4-1999mua_html_4dc8b0129e27f62e.png)

## <a id="exercise1"></a>Exercise 5 – Link a dSource

In this exercise, you will:

- Create an Oracle dSource by syncing with your Source Oracle Database
- Create a Delphix Group to hold your dSource object

**Context**

With an environment set-up, users can then sync databases into Delphix. The Delphix engine will read the source database and create a dSource (a custom object). The dSource is not a functional image of the database but a storage-efficient object from which virtual databases can be created. When creating a dSource, Delphix will pull over the complete data set using standard database protocols. Subsequent sync operations, as governed by user-defined policies, will pull only incremental changes. At the end of each sync operation, a snapshot is created that serves as the base point for provisioning operations. These sync operations are called SnapSync. Additional details on sync functionality can be found [here](https://docs.delphix.com/docs/datasets/getting-started/managing-data-sources-and-syncing-data).

**Steps**

1. View the Environment details
  1. Click on the **Environment** on the left on review information on the **Details** tab

![](RackMultipart20201027-4-1999mua_html_4c3e5718135abc27.png)

1. Click on the **Databases** tab to view any discovered database installations and databases

![](RackMultipart20201027-4-1999mua_html_2880ef2c73391d7f.png)

1. Link the **orcl** database
  1. On the top menu, click the **Manage** menu and then **Datasets**
  2. Click the **+** next to **Datasets** on the top left portion of your screen and then select **Add dSource**

![](RackMultipart20201027-4-1999mua_html_4bec0799dc89ac18.png)

1. The Welcome page for the dSource Wizard will be displayed. Review the instructions to get an overview of the process and click **Next**
2. In the Source tab select the orcl Data source and provide the username and password of the database user
3. Under the **dSource Configuration** tab we will provide a user-friendly name for the dSource and create a new Dataset Group to place it in.
  1. Enter ***orcl*** for the **dSource Name**
  2. Click on the **Add Dataset Group** link and enter ***Source DB*** in the **Name** field.

![](RackMultipart20201027-4-1999mua_html_de3ee65d3512b895.png)

1. Click **Next**
2. On the **Data Management** tab accept the defaults
  1. Initial Load: **Immediately**
  2. LogSync: **Enable**

**Archive + Online Redo**

  1. Click **Next**

1. Accept the Default SnapSync and Retention policy setting
2. Click **Next**
3. Accept defaults for the Hooks. No Hooks will be used.
4. Click **Next**
5. Review the Summary and click **Submit**.

![](RackMultipart20201027-4-1999mua_html_33df29add9d7991f.png)

1. Wait for the dSource to be created

![](RackMultipart20201027-4-1999mua_html_39c855e7e0b72875.png)

You will know this is successful if the dSource completes in the **Actions** pane without Errors. Click on **Actions** in the top menu bar if you don&#39;t see this pane. Also, the **dSource** state will change to **Active**

## <a id="exercise1"></a>Exercise 6 – Add Target Environments

In this exercise, you will:

- Connect Delphix to your Target Oracle server

**Context**

The target environment is the host where the virtual databases will be created.

**Steps**

1. Log into the Delphix Data Platform UI as the ***admin*** user and the password that was set during the Engine setup.

_Note: If you forgot your admin password, please ask your lab administrator to reset it._

1. In the top menu bar, click **Manage** and then **Environments**.
2. Click the ellipses ( **…** ) next to **Environments** then choose **Add Environment**.
3. Provide the following details in the **Add Environment Wizard**

Host and Server

  1. Host OS: **Unix/Linux**
  2. Server Type: **Standalone**

Click **Next**

1. On the **Environment Settings** tab, enter the following details
  1. Environment Name: ***Target***
  2. Host Address: ***10.0.x.30*** (&#39;x&#39; will be your **Student Number** )
  3. NFS Addresses: ***10.0.x.30*** (&#39;x&#39; will be your **Student Number** )
  4. Leave the **DSP** fields blank
  5. OS Username: ***delphix***
  6. OS Password: ***delphix***
  7. Toolkit Path: ***/u01/app/toolkit*** (The Toolkit Path must be entered prior to clicking Validate)
  8. Click **Validate**
2. Click **Next**

![](RackMultipart20201027-4-1999mua_html_e7fc23dc3d832629.png) ![](RackMultipart20201027-4-1999mua_html_a2aa2f77d0ccc3d8.png)

1. Click **Submit**
2. You can view the status of the environment creation and discovery by clicking on the **Actions** menu on the top right-hand side of the page. Clicking on the job in the **Actions** pane will allow you to track its progress.
3. View the Environment details
  1. Click on the **Environment Name** ( **Target** ) on the left and review information on the **Details** tab
  2. Click on the **Databases** tab to view any discovered database installations and databases

## <a id="exercise1"></a>Exercise 7 – Provision a VDB

In this exercise, you will:

- Create a VDB called devdb

**Context**

The objective of Delphix virtualization is to provide easy access to virtualized databases that resemble production or any other data system. In Exercise 7, we provision a Virtual Database (VDB). VDBs are fully functional database images that can be created from dSources; specifically, from the snapshots of dSources.

**Steps**

1. Click the **Manage** menu and then **Datasets**
2. Expand the **Source DB** group and click the **orcl** dSource from the **Datasets** panel on the left. This will reveal the **Timeflow** for the dSource by default.
3. On the **Timeflow** tab of the dSource select the most recent (topmost) **Snapshot**. The **Provision VDB** iconwill appear along with the **Open LogSync** icon to the right.
4. Click on the **Provision VDB** icon to open the **Provision VDB** wizard

 ![](RackMultipart20201027-4-1999mua_html_256deb74c7c2ca94.gif)

![](RackMultipart20201027-4-1999mua_html_a6692aad5dc25c1.png)

1. On the **Target Environment** tab click **Target** from the list of Environments
2. On the **Target Environment** tab, use the following information
  1. Verify that the **Installation Home** is set to **/u01/app/oracle/product/11.2.0/dbhome\_1 (11.2.0.1.0)**
  2. Ensure that the Environment **User** is set to **delphix**
  3. Click **Next**

![](RackMultipart20201027-4-1999mua_html_272876ddfe59bb1c.png)

1. Enter the details below on the **Target Configuration** tab , we will provide a user-friendly name for the VDB as well as assign it to a new group.
  1. Click **Add Dataset Group**
    1. Type ***DB Targets*** for the **Group Name**
    2. Click **Add**
  2. Verify that **DB Targets** is selected as the **Target Group**
  3. **Mount Base** : Type ***/mnt/provision***
  4. **Oracle Database Name** : Type ***devdb***
  5. **VDB Name** : Type ***devdb***
  6. **Oracle Database Unique Name** : Type ***devdb***
  7. **Oracle SID** : ***devdb***
  8. Click **Next**

![](RackMultipart20201027-4-1999mua_html_32cc200efae9b5d9.png)

1. On the **Advanced** tab

  1. Confirm that **Open Database After Provision** is selected
  2. Confirm that **Enable Archivelog Mode** is selected
  3. Check the box next to **Enabled** for **Auto VDB Restart**
  4. Click **Next**

![](RackMultipart20201027-4-1999mua_html_f820e9af3ea8c230.png)

1. On the **Policies** tab, accept the defaults and click **Next**
2. On the **Masking** tab, confirm that **Mask this VDB** is not checked
3. Accept the defaults on the **Hooks** tab and click **Next**
4. Verify the summary information
5. Click **Submit** tocomplete the VDB creation

![](RackMultipart20201027-4-1999mua_html_9c1dcc95a52b9fd2.png)

It may take a couple minutes for the VDB creation to complete. You can monitor the status on the left-hand side of the screen next to the **devdb** object in the **DB Targets** group. On the **Actions** pane on the right-hand side of the screen, you should see the **Provision virtual database &quot;devdb&quot;** item move to the **Recently completed** pane without error. **devdb**&#39;s status will be set to **VDB - Running**.

![](RackMultipart20201027-4-1999mua_html_d3b4d246f7bf2602.png)

Once the VDB is created, you can verify that the VDB is operational by:

1. Using **Terminal** on your lab server, use SSH to connect to your **Linux Target** server
  1. Type ssh 10.0.x.30 (&#39;x&#39; will be your **Student Number** ).
2. Enter the username: delphix
3. Enter the password: delphix
4. Run the following commands on the **target** server command line:
  1. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  2. Type export ORACLE\_SID=devdb
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
  4. Type sqlplus / as sysdba
  5. Type select name from v$database;

![](RackMultipart20201027-4-1999mua_html_4be065e2dbbbeea8.png)

1. Type quitto exit

## <a id="exercise1"></a>Exercise 8 – Refresh a VDB

In this exercise, you will:

- Create a new table on your source database
- Snapshot the dSource
- Refresh your VDB - devdb
- Verify the new table appears on the VDB

**Context**

VDBs can get out of sync as new data comes into the source system. Refreshing a VDB will re-provision it from the dSource. Refreshing a VDB will delete any changes that have been made to it over time. To revert a VDB to match the dSource, the following steps are followed:

**Steps**

1. Open **Terminal** on your Lab Server desktop
  1. Type ssh delphix@10.0.x.20 (&#39;x&#39; will be your **Student Number** )
2. Enter the username: delphix
3. Enter the password: delphix
4. Run the following commands:
  1. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  2. Type export ORACLE\_SID=orcl
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
  4. Type sqlplus / as sysdba
  5. Type create table sourcetab as select \* from dba\_objects;
5. Go back to the **Delphix Data Platform** in your browser (click the **Delphix** logo to go back to the main screen if required)
6. Click on the **orcl** dSource object on the left side of your screen (expand **DB Source** group if needed)
7. Click the **Camera** Icon on the top-right to take a snapshot
8. Click on the **devdb** VDB on the left side of your screen
9. Click the **Timeflow** tab
10. Click the **Refresh** button on the top right next to the **snapshot** icon.

![](RackMultipart20201027-4-1999mua_html_e599041e71b9e42f.png)

1. There are two options for refreshing the VDB, choose the **Faster** option to refresh from the most recent snapshot from the **orcl** dSource
2. Click **Next**
3. Click **Submit**

![](RackMultipart20201027-4-1999mua_html_4bb7e57e2f75619a.png)

Once the refresh has completed, a new VDB snapshot will be generated and reflected in the **Timeflow**.

![](RackMultipart20201027-4-1999mua_html_efd482a6c1d8baa5.png)

Log into **devdb** to confirm.

1. Open **Terminal** on your Lab Server desktop
  1. Type ssh delphix@10.0.x.30 (&#39;x&#39; will be your **Student Number** )
2. Enter the username: delphix
3. Enter the password: delphix
4. Run the following commands:

  1. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  2. Type export ORACLE\_SID=devdb
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
  4. Type sqlplus / as sysdba
  5. Type select count(\*) from sourcetab;

![](RackMultipart20201027-4-1999mua_html_ade3a2c7d397314c.png)

If this returns a count of rows, the snapshot/refresh was successful.

## <a id="exercise1"></a>Exercise 9 – Rewind a VDB

In this exercise, you will:

- Take a snapshot of the devdb VDB
- Corrupt the devdb VDB to introduce a bootstrap error
- Rewind the devdb VDB to recover from the error

**Context**

Rewinding a VDB rolls it back to a previous point in its Timeflow and re-provisions the VDB. The VDB will no longer contain changes after the rewind point. it can be triggered when changes to the VDB do not need to be saved.

**Steps**

1. On the Delphix main screen, select the **devdb** VDB
2. Click the **Camera icon** on the top right to take a snapshot of the VDB

![](RackMultipart20201027-4-1999mua_html_e3fcd49f24fdf719.png)

1. A new snapshot card will be created on the **devdb Timeflow**. Make a note of the date/time for the latest snapshot card.
2. Open Terminal on your Lab Server desktop
  1. Type: ssh delphix@10.0.x.30 (&#39;x&#39; will be your **Student Number** )
3. Enter the username: delphix
4. Enter the password: delphix
5. Run the following commands:
  1. Type export ORACLE\_SID=devdb
  2. Type sqlplus / as sysdba
  3. Type delete from sys.obj$;
  4. Type commit;
  5. Type shutdown abort;
  6. Type startup;

![](RackMultipart20201027-4-1999mua_html_ac1855f977f206e1.png)

Note that the database is unable to come online due to a bootstrap error. The **devdb** database is now corrupted. Now we will rewind the VDB to the last good snapshot to fix this.

1. In the Delphix Data Platform (web browser), click on the **devdb** VDB if it is not already selected
2. Click on the **Timeflow** tab and select the snapshot associated with the date/time you recorded prior to corrupting your database. This will most likely be the latest snapshot.
3. Hover over the **Snapshot** to reveal the **Rewind** button.
4. Click the **Rewind VDB** button on the **Timeflow**

![](RackMultipart20201027-4-1999mua_html_bd0e6d5bc6051658.png)

1. Confirm that you wish to rewind the VDB by clicking **Rewind**.

![](RackMultipart20201027-4-1999mua_html_368dc885ac2ebf85.png)

Once the rewind operation is complete, you can confirm the rewind was successful by connecting to the server again and querying the database:

1. Open Terminal on your Lab Server desktop
2. Type ssh delphix@10.0.x.30 (&#39;x&#39; will be your **Student Number** )
3. Enter the username: delphix
4. Enter the password: delphix
5. Run the following commands:
  1. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  2. Type export ORACLE\_SID=devdb
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
  4. Type sqlplus / as sysdba
  5. Type select count(\*) from sourcetab;

![](RackMultipart20201027-4-1999mua_html_f21660f0915ae942.png)

## <a id="exercise1"></a>Exercise 10 – Set a New Retention Policy

There are four types of Policies in Delphix. In this exercise, you will:

- Create a Retention Policy
- Set the new policy to keep snapshots and logs for 30 days, along with 3 monthly snapshots
- Apply the policy to the VDB we created in the previous exercise

**Context**

Both dSources and VDBs timeflow is governed by snapshots, which are either created manually or through policies. Retention policies govern the lifespan of such snapshots and help clean older snapshots that are no longer being used.

**Steps**

1. In the top menu bar, click on **Manage** and then **Policies**
2. Click the **Retention** tab, click **+Retention**

![](RackMultipart20201027-4-1999mua_html_1c118b417497125f.png)

1. Provide the following details:
  1. Policy Name: ***Long Term***
  2. Keep Logs for: ***30** ***days**
  3. Keep Snapshots for: ***30** ***days**
2. Click the ***Show advanced*** _link_

![](RackMultipart20201027-4-1999mua_html_915db92d34a651f5.png)

1. Click the checkbox next to **Keep 3 Snapshot(s) on 1****st **** of every month**
2. Click **Next**
3. On the **Datasets** tab click the **checkbox** for the **devdb**
4. Click **Submit**

![](RackMultipart20201027-4-1999mua_html_807d3f177d5ce120.png)

Expand the **policies** menu to validate that the new **Long Term** policy has been applied to the **devdb** VDB.

![](RackMultipart20201027-4-1999mua_html_de23a64905c54cc7.png)

## <a id="exercise1"></a>Exercise 11 – Create and Save a Hook Operation Template

In this exercise, you will:

- Create a Hook Template called Create APPUSER
- Insert code into the template that will log into a database and add a user named appuser

**Context**

Hook operations allow users to execute custom operations at select points during linking sources and managing virtual datasets.

**Steps**

1. In the top menu bar, click **Manage** , then **Hook Templates**
2. Click the **plus sign** to the right of the word **Templates** in the **Hook Templates Wizard**
3. Provide the _Name_: ***Create APPUSER***
4. Ensure _Type_ is set to: **System Shell Command**
5. Under **Contents** , enter the following code:

$ORACLE\_HOME/bin/sqlplus / as sysdba \&lt;\&lt; EOF

create user appuser identified by appuser;

grant connect, resource to appuser;

exit;

EOF

**IMPORTANT:** Make sure the carriage returns you see here are the same in the pasted contents.

![](RackMultipart20201027-4-1999mua_html_5146b8a1004fab59.png)

1. Click **Create**
2. Verify the **Create APPUSER** Hook Operation Template is in the list, then click **Close**

![](RackMultipart20201027-4-1999mua_html_fa680ec70ae57ec5.png)

## <a id="exercise1"></a>Exercise 12 – Create a VDB Template

In this exercise, you will:

- Create a VDB Configuration template called _Small DB Template_
- Set a custom Oracle parameter for this template

**Steps**

1. In the top menu bar, click **Manage** , then **VDB Config Templates**
2. Click the **plus sign** next to the word **VDB Config Templates** and select **New Template** from the drop-down.
3. Name: ***1G Template***
4. Click **Create**

![](RackMultipart20201027-4-1999mua_html_a11086c5dcb580e.png)

1. Click **1G Template** under **VDB Config Templates**
2. Click the **pencil** icon on the top right of the **VDB Configuration Templates** screen
3. Click the **plus sign** on the **top right** to add a new parameter
  1. Double-click the row to enter a new value
4. In the row that is now highlighted, enter the following information:
  1. Name: ***sga***_**\_**_***max\_size***
  2. Value: ***1G***
5. Click the **checkmark** to save the **Template**.

You can verify that this was successful by returning to the **VDB Configuration Templates Wizard** and clicking on the **1G Template** item.

## <a id="exercise1"></a>Exercise 13 – Provision a VDB with Hook and VDB Template

In this exercise, you will:

- Create a VDB called **qadb** onTarget
- Use the VDB Configuration Template we created previously
- Use the Hook Operation Template we created previously
- Log into the VDB
- Verify the VDB Configuration Template and Hook Operation Template were successful

**Steps**

1. Return to the Delphix home screen by clicking the **Delphix** logo on the top left of the screen
2. Click on the **orcl** dSource object in the **Datasets** panel
3. Select the latest **Snapshot** , click the **Provision** button
4. On the left hand side of the **Target Environment** tab, click the **Target**** Environment ****Target**
5. Click **Next**
6. On the **Target Configuration** tab, enter the following details:
  1. **Select DB Targets** from the **Datasets Group** drop-down list,
  2. **Mount Base** : **/mnt/provision** (this should already be filled in)
  3. **Database Name** : ***qadb***
  4. **Database Unique Name** : ***qadb***
  5. **SID** : ***qadb***
  6. Click **Configure VDB Parameters** checkbox under **VDB Database Parameters**
  7. Click **Next**
7. On the **Advanced** tab, click the **Enabled** checkbox under **Auto VDB Restart**
8. Click **Next**
9. On **the VDB Configure Parameters** tab, click the **1G Template** that we created earlier from the **Select template** drop-down list
10. Click **Next**

![](RackMultipart20201027-4-1999mua_html_6b4875b1eeee3f55.png)

1. On the **Policies** tab, accept Default Snapshot Policy and Click **Next**
2. On the Masking tab, leave Mask this VDB unchecked. Click **Next**
3. On the **Hooks** tab with **Configure Clone** already selected on the left side of the **Provision VDB Wizard** , click the **plus sign** to the right of the words **Hook Points** and select **Create from Template** from the drop-down list

![](RackMultipart20201027-4-1999mua_html_522015e4d4919514.png)

1. **Enter a Name** ***QA APPUSER*** for the **Hook Operation**
2. Click on the **Create APPUSER** template we created earlier
3. Click the **Create** button

![](RackMultipart20201027-4-1999mua_html_9e3fdf6c5d2c4de.png)

1. Click Next

![](RackMultipart20201027-4-1999mua_html_bb843e593733e950.png)

1. Click **Next**
2. Verify the summary information, and click **Submit**

Note: It may take a couple minutes for the VDB creation to complete. You can monitor the progress on the left-hand side of the screen next to the **qadb** object in the **DB Targets** group. On the **Actions** pane on the right-hand side of the screen, you should see the **Provision virtual database &quot;qadb&quot;** item move to the **Recently completed** pane without error.

Once the VDB is created, you can verify that the VDB is operational by:

1. Using **Terminal** on your lab server, use SSH to connect to your Linux Target server
  1. Type ssh 10.0.x.30 (&#39;x&#39; will be your **Student Number** ).
2. Enter the username: delphix
3. Enter the password: delphix
4. Run the following commands:

  1. Type export ORACLE\_SID=qadb
  2. Type export ORACLE\_HOME=/u01/app/oracle/product/11.2.0/dbhome\_1
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
  4. Type sqlplus / as sysdba
  5. Type show parameter memory\_target
  6. Type connect appuser/appuser

This will verify that the VDB is online with the **VDB Configuration Template** we specified, and that the **APPUSER** user was created by our hook.

![](RackMultipart20201027-4-1999mua_html_59d29574e64339c2.png)

Note: It may take a couple minutes for the VDB creation to complete. You can monitor the progress on the left-hand side of the screen next to the **qadb** object in the **DB Targets** group. On the **Actions** pane on the right-hand side of the screen, you should see the **Provision virtual database &quot;qadb&quot;** item move to the **Recently completed** pane without error. Once the VDB is created, you can verify that the VDB is operational by:

## <a id="exercise1"></a>Optional Advanced Exercise – Discover and Link a 12c Container Database dSource

In this exercise, you will:

- Using the createDelphixDBUser script to create the c##delphix\_db user in CDB$Root.
- Using the createDelphixDBUser script to create the delphix\_db user in the PDB WINTERFELL
- Discover the 12C Database to identify pluggable database WINTERFELL
- Add a dSource for the PDB WINTERFELL

**Steps**

We will walk through the steps to get you acquainted with discovering 12C PDB&#39;s and creating a dSource from them.

1. Using the Terminal icon on the Lab desktop connect to the Linux Source Server
  1. Type ssh 10.0.x.20 (&#39;x&#39; will be your **Student Number** ).
  2. Enter the password delphix
2. Set the appropriate environment variables for the 12c Database and cd to the hostchecker directory :
  1. Type export ORACLE\_HOME=/u02/app/oracle/product/12.2.0/dbhome\_1
  2. Type export ORACLE\_SID=gotcdb.
  3. Type export PATH= $ORACLE\_HOME/bin:$PATH
  4. Type cd hostchecker

Note: If the hostchecker directory doesn&#39;t exist, untar the hostchecker\_linux\_x86.tar file

(Type tar -xvf hostchecker\_linux\_x86.tar)

1. Run the **createDelphixDBUser** Script to first create the **c##delphix\_db** user in the CDB
  1. Type ./createDelphixDBUser.sh
  2. Enter the Database Instance enter : gotcdb
  3. When prompted to verify if you want to create the user in CDB#ROOT press Enter
  4. Enter the DB User name: c##delphix\_db
  5. Enter password : delphix
  6. When prompted for select any dictionary privileges : n

The script will create the c##delphix\_db user with necessary privileges.

1. Run the **createDelphixDBUser** script again to create the **delphix\_db** user in the PDB **WINTERFELL**
  1. ./createDelphixDBUser.sh
  2. When Prompted for the Database Instance enter gotcdb
  3. When Prompted for pluggable database name enter winterfell
  4. When Prompted for the Database User enter delphix\_db
  5. When Prompted for the password use : delphix
  6. When prompted for select any dictionary privileges : n

The script will now create delphix\_db user in the PDB winterfell with the necessary privileges.

**Discover the CDB**

If not already connected, connect to your Delphix Engine as Delphix Admin.

1. Navigate to Manage -\&gt; Environments Page
2. Select the Source Environment and click on the Databases Tab.
3. Scroll down to find the home /u01/app/oracle/product/12.2.0/dbhome\_1
4. Clickon Discover CDB to discover the PDB&#39;s in the CDB

![](RackMultipart20201027-4-1999mua_html_bed2926408d44b2d.png)

1. On the Discover CDB wizard Popup Enter the following details
  1. Username: c##delphix\_db
  2. Password: delphix
2. Click on OK
3. Click and monitor the Actions tab to ensure it completes without error.

Once complete, PDB **WINTERFELL** should be discovered and displayed under the 12c home

![](RackMultipart20201027-4-1999mua_html_69eb70efbf9f6085.png)

**Link an Oracle 12c dSource**

1. Click on the Add dSource button to add a dSource from this database
2. On the Add dSource Wizard make sure the PDB **WINTERFELL** is selected.
3. Scroll down and add db user details
  1. DB Username: delphix\_db
  2. DB Password: delphix
4. Click on **Verify credentials** to verify the give details.
5. Click **Next**

![](RackMultipart20201027-4-1999mua_html_8fa6079f1d821166.png)

1. On the dSource Configuration tab of the Add dSource wizard Click on **Add Dataset Group**
2. On the Add Dataset Group pop up, add a new group with the name ***Ora12C Sources***.
3. Click **Add**.
4. Make sure the new dataset group is selected and click **Next**.
5. Review and Accept the defaults on the Data Management tab by clicking **Next**
6. Review the defaults on the Policies tab and click on **Next**.
7. We will not be configuring hooks at this time. Click on **Next**.
8. Review Summary and Click **Submit**.

![](RackMultipart20201027-4-1999mua_html_50c15171d9293d2c.png)

1. Click on Actions on the top right-hand corner if Actions Pane is hidden to monitor progress of the Action and ensure it completes without error.

![](RackMultipart20201027-4-1999mua_html_e2a7d213909464d9.png)

1. Once the dSource creation is completed Navigate to **Manage** -\&gt; **Datasets**.
2. Expand the Ora12c Sources group to explore the DB&#39;s under it. You should see both the CDB gotcdb and the PDB WINTERFELL listed under it.

Note the Different icons used to represent CDB and PDB&#39;s

Also note there is no timeflow available for the container database gotcdb.

![](RackMultipart20201027-4-1999mua_html_ef667bffd65bb683.png)

1. Click on the PDB WINTERFELL to see the **Timeflow** , **Status** and **Configurations** under the respective tabs.

![](RackMultipart20201027-4-1999mua_html_64fd4bb3995854d4.png)

## <a id="exercise1"></a>Optional Advanced Exercise – Provision a Virtual PDB (vPDB)

In this exercise you will:

- Provision a VPDB from a 12c dSource.

**Steps**

As an advanced exercise, this lab has no corresponding Lab Solution. Instead, we will walk through the steps to get you acquainted with provisioning a PDB from a 12c dSource.

1. Login to the Delphix Engine if not already logged in.
2. Navigate to **Manage** -\&gt; **Datasets**
3. Expand **Ora12C Sources** Group and click on WINTERFELL PDB
4. Hover on the latest snapshot card and click on **Provision a VDB**.

![](RackMultipart20201027-4-1999mua_html_86ec484b141cbcb.png)

1. On the Provision vPDB Wizard
  1. Click on **Target** under **Environments**.
  2. And click the check box for **Create a New Container Database**.
  3. Click **Next**.
2. On the Target Configuration tab click **Add Dataset Group**
3. On the Add Dataset Group Wizard create a new group with the name ***Ora12C Targets***.
4. Click **Add**
5. Back on the **Target Configuration** page make sure the new Dataset group **Ora12C Targets** is selected.
6. Add the following details. (Database Name and SID limited to 8 characters).
  1. Pluggable Database Name : devPDB
  2. vPDB Name : devPDB
  3. Database Name: VCWINTER
  4. vCDB Name : VCWINTER
  5. Database Unique Name: VCWINTER
  6. SID: VCWINTER
7. Click **Next**.

![](RackMultipart20201027-4-1999mua_html_f4af47f01d8af342.png)

1. On the **Advanced** page click the **Auto vCDB Restart Enabled checkbox**
2. Click **Next**
3. Accept the Default policies. Click **Next**
4. We will not be configuring hooks now. Click **Next**.
5. Review the summary page and click **Submit**.

Note: Once Submitted, the Action will show a working screen for a few seconds. You can choose to let the Action run in the background.

1. Monitor the Actions Pane by clicking on the Actions button on the top right-hand corner. You can get more details about the action by clicking on the &quot;\&gt;&quot;, to the left of the currently running action, to expand it.

![](RackMultipart20201027-4-1999mua_html_13a08e6d16dd764.png)

Note: Once Action is completed you can see the vCDB we added and the VPDB just provisioned listed under the new Dataset group Ora12C Targets. Expand the group if necessary, to view the objects under it. Note that there is no timeflow for the vCDB.

1. Click on devPDB and review Timeflow, Status and configuration details under the respective tabs.

![](RackMultipart20201027-4-1999mua_html_cdfdda06b87d119f.png)

1. Using the **Terminal** icon on your lab desktop connect to the **target** server. (Refer to the Important IP Addresses section of the labguide)
  1. Click on the **Terminal** icon on the lab desktop
  2. Type ssh 10.0.x.30 (replace # with the student number assigned to you).
2. Set up the environment variables for 12C
  1. Type export ORACLE\_SID=VCWINTER
  2. Type export ORACLE\_HOME=/u02/app/oracle/product/12.2.0/dbhome\_1
  3. Type export PATH=$ORACLE\_HOME/bin:$PATH
3. Connect to sqlplus and verify the CDB and PDB details
  1. Type sqlplus / as sysdba
4. At the SQL\&gt; prompt type
  1. select name from v$database;
  2. alter session set container=DEVPDB;
  3. show CON\_NAME

## <a id="exercise1"></a>Optional Advanced Exercise – Measure Network Performance Test through the CLI

In this exercise, you will:

- Log into the Delphix Data Platform CLI as admin
- Perform a network latency test to Target
- Perform a network throughput test to Target

**Steps**

As an advanced exercise, this lab has no corresponding Lab Solution. Instead, we will walk through the steps to get you acquainted the Delphix CLI for admin.

1. On your Lab Server desktop, double-click on **Terminal**
2. Type: ssh admin@10.0.x.10 (&#39;x&#39; is your **Student Number** assigned by your instructor)
  1. If you receive a prompt asking you if you are sure you want to connect, enter: Yes
  2. Enter the password: delphix
  3. You are now at the root of the Delphix CLI as a Delphix Administrator
3. Create a network latency test by typing: network test latency create
  1. List the default/required parameters by typing: get
  2. Set the remoteHost value to the TargetA environment IP address: set remoteHost=10.0.x.30 (&#39;x&#39; will be your **Student Number** )
  3. Begin the test by typing: commit

![](RackMultipart20201027-4-1999mua_html_bb4164b896b23a35.png)

_Figure 48 Example Network Latency Test Submission_

1. View the results of the latency test:
  1. Get to the latency test section again by typing: network test latency
  2. List the completed tests by typing: ls
  3. Type select followed by the name of the test from the list. For example:

select 10.0.1.30-2015-09-18T12:47:19.711Z

  1. View the results of the test by typing: get

![](RackMultipart20201027-4-1999mua_html_3ccc9ede2297880d.png)

1. Create a network throughput test
  1. While still logged into the CLI, return to the root by typing: cd /
  2. Begin a network throughput test by typing: network test throughput create
  3. List the default/required parameters by typing: get
  4. Set the remoteHost value to the TargetA environment IP address: setremoteHost=10.0.x.30 (&#39;x&#39; will be your **Student Number** )
  5. Begin the test by typing: commit

![](RackMultipart20201027-4-1999mua_html_2834ae427eab20f4.png)

1. View the results of the throughput test:
  1. Get to the throughput test section again by typing: network test throughput
  2. List the completed tests by typing: ls
  3. Type select followed by the name of the test from the list.

For example:

select 10.0.1.30-2015-09-18T13:13:08.152Z

  1. View the results of the test by typing: get

![](RackMultipart20201027-4-1999mua_html_63f829963ec82542.png)

## <a id="exercise1"></a>Optional Advanced Exercise – Configure Delphix Replication

Note: This exercise is only possible if your classroom has been configured with 2 or more students.

In this exercise, you will:

- Set up a replication profile
- Replicate your entire Delphix Data Platform to another Delphix Data Platform
- View the replicas in the target Delphix Data Platform

**Steps**

As an advanced exercise, this lab has no corresponding Lab Solution. Instead, we will walk through the steps to get you acquainted the Delphix Replication capability.

1. In the Delphix GUI, select **System** and then **Replication** on the top menu bar
2. Add a **Replication Profile** called DR Replica
  1. Click the **plus sign** next to **Replication Profiles** on the top left
  2. Enter a **Replica Profile Name** : ***DR Replica***
  3. For Target Engine, enter the **Delphix Data Platform IP address** for the next student in your classroom environment. If you are the last student, use the Delphix Data Platform IP address for Student 1. For example, in a class with 3 students:
    1. Student 1 Delphix Data Platform is at 10.0.1.10, and they will replicate to 10.0.2.10
    2. Student 2 Delphix Data Platform is at 10.0.2.10, and they will replicate to 10.0.3.10
    3. Student 3 Delphix Data Platform is at 10.0.3.10, and they will replicate to 10.0.1.10
    4. Ask your instructor if you have any questions or confusion about this configuration.
  4. Enter the User Name: ***admin***
  5. Enter the Password: ***delphix***
  6. Do not enable Automatic Replication or configure Traffic Options
  7. For the Objects Being Replicated, select: **Entire Delphix Data Platform**
  8. Click **Create** at the bottom when ready.

![](RackMultipart20201027-4-1999mua_html_c0d41ea35593d139.png)

_Figure 52 Replication Profile Configuration_

1. Start the Replication by clicking the **Replicate Now** button on the top right of your screen.
2. Click **Replicate** to confirm you are ready to begin.
3. Once the initial full replication is complete, you will see a message stating **Last Replication Successful**.

![](RackMultipart20201027-4-1999mua_html_8cffde8a35596fbc.png)

_Figure 53__Example Successful Replication_

1. Check the results on your target Delphix Data Platform
  1. In your lab server browser, enter ***the IP address*** you used for the Target Engine in your replica profile. For example, if you are Student 1, your Delphix Data Platform is at 10.0.1.10, and your target would have been 10.0.2.10.
  2. Log in as user ***admin*** with the password ***delphix***
  3. Observe the dropdown list under **Datasets** on the top left corner of your screen. It should have **Default shown** which is the default **Namespace** for Delphix replica targets.
  4. In order to see the replica objects, click on the dropdown list and select the second entry, which should reflect the hostname of the source Delphix Data Platform that sent the replica.

_Note: The hostname shown in the labs is based on the default hostname given to the Delphix Data Platform in Amazon AWS, consisting of the prefix &quot;ip&quot; followed by the IP address separated by hyphens._

1. While still logged into your target Delphix Data Platform, click on **System** and then **Replication**
2. Observe the **Received Replicas** section at the bottom, indicating and verifying the target&#39;s receipt of replication data.
  1. Note: The **Failover Now** option will not work for these labs due to namespace collisions. This is an inherent outcome to plan for when using Active/Active replication.

© 2015 Delphix Corp. All rights reserved


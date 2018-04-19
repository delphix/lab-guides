Table of Contents

* [Exercise 1 - Create a Data Template](#exercise1)
* [Exercise 2 – Create a JetStream Data User](#exercise2)
* [Exercise 3 – Create Data Containers](#exercise3)
* [Exercise 4 – Refresh a Data Container](#exercise4)
* [Exercise 5 – Create a new Branch](#exercise5)
* [Exercise 6 – Switch between Branches](#exercise6)
* [Exercise 7 – Create and Share a Bookmark](#exercise7)
* [Exercise 8 – Create a branch from a shared bookmark](#exercise8)
* [Exercise 9 – Restore Data Container](#exercise9)
* [Exercise 10 – Lock a Data Container](#exercise10)

<a name="exercise1"></a>
## Exercise 1 - Create a Data Template
In this exercise, you will:
- Login to the SelfService UI as a SelfService Admin User (delphix_admin)
- Create a Data Template

### Steps
1. Open a browser on your Delphix student lab desktop
1.	Enter the IP address of the Delphix engine assigned to you in the browser (IP address of Delphix engine is 10.0.x.10 where x is the student number)
1.	Log in with credentials:
    - Username: *delphix_admin*
    - Password: *delphix*
1.	Click on the *delphix_admin* username on the top right of the GUI and select **Self-Service**
1.	Click on **Add Template**
    -	Name: **orcl**
    -	Description:  **orcl template**
    -	Click on **Add Data Source**
    -	Enter a name for the data source: **orcl**
    -	Select **orcl(DB Sources)** from the dropdown list
    -	Click **Create**
1.	On the top right of the GUI, in the link entitled *delphix_admin*, please select *Management* to switch back to the Delphix Admin console interface
1.	Navigate to **Manage -> Datasets**
1.	Click the **orcl** dSource in the Datasets panel.
1.	View the Dataset Details on the right-hand side. The *orcl* dSource should listed under Jet Stream Templates

<a name="exercise2"></a>
## Exercise 2 – Create a JetStream Data User
In this exercise, you will:
 - Create a JetStream user

### Steps
1.	Click on **Manage -> Users** on the menu to view existing users
1.	Click on + to create a new user.
    - Select the *Authentication Type* as Delphix
    - Username: *jsuser*
    - Password: *jsuser*
    - Confirm Password: *jsuser*
    - Email Address: *jsuser@example.com*
    - User Type: *Jet Stream Only*
    - Click **Submit**

<a name="exercise3"></a>
## Exercise 3 – Create Data Containers
In this exercise, you will:
- Create a Data Container for the developers
- Optionally create a second container for QA team

### Steps
1. Open a browser window on your Delphix student lab desktop and enter the IP address of your Delphix engine.
1. Log in with credentials:
   - Username: *delphix_admin*
   - Password: *delphix*
1. Click on the *delphix_admin* username on the top right of the GUI and select **Self-Service**
1. On the **Self-Service** page, click on the link which is the name of the **orcl** template
1. Click on **Add Container** to create a new Data Container
   - Name of container: *ORCL Dev*
   - In the Owner field, select *dev* from the drop-down list
   - Description: *ORCL Development Container*
   - Make sure that the radio button next to “Refresh data sources to most recent template state” is selected
   - In the Data Sources panel below, select *devdb* from the drop-down list VDB or vFiles
   - Add a description to the container like *“devdb is the data source for the container ORCL Dev”*
   - Click **Create**
1. Click on the **Containers** button on the left-hand vertical bar alongside the *Containers* panel to view the newly created Container.
1. Switch back to the **Management** (drop-down from the *delphix_admin* link in the upper right-hand corner of the GUI) and view the progress of the under the **Actions** menu
1. Navigate to Manage -> DataSets
   - Select the *devdb* VDB in the left-hand navigation bar of the main Delphix admin console
   - Click on the Status tab.
   The container name *ORCL Dev* will be listed under “Jet Stream Container”
1. Go back on the *delphix_admin* username link on the top right of the GUI and select **Self-Service**
1. Please repeat step 5 above to create another container named *ORCL QA* owned by the qa
JetStream-only user, assigning the VDB named *qadb* as a data source.
Choose “Add data sources to container as-is” when adding the qadb VDB data source.

<a name="exercise4"></a>
## Exercise 4 – Refresh a Data Container
In this exercise, you will:
- Create a new table on the source database (orcl)
- Login to Self-Service as the dev user
- Refresh the ORCL Dev container from “production” (orcl  dSource)

### Steps
1. Open a terminal session on the Delphix student lab desktop by double-clicking on the Terminal app on the desktop
1. Create a SSH connection to the *linuxsource* host as the delphix user by entering the following commands in the terminal window:
   - *ssh delphix@10.0.x.20*  (Replace x with your assigned student number)
   - Enter “Yes” if prompted to accept the SSH connection
   - Provide the delphix password: *delphix*
   - Enter the following commands at the UNIX command-line on *linuxsource*
   ```
   sqlplus / as sysdba
   
   CREATE TABLE sourcetab1 AS SELECT * FROM dba_objects;
   SELECT COUNT(*) FROM sourcetab1;
   ```
1. Open a browser window on your Delphix student lab desktop and enter the IP address of your Delphix engine
1. Login to the Self-Service interface
   - Username: *dev*
   - Password: *delphix*
1. You will be prompted to reset the dev user password
   - Enter *dev* as the new password
1. Create a SSH connection to the *linuxtarget* host as the delphix user by entering the following commands in the terminal window:
   - *ssh delphix@10.0.x.30*  (Replace x with your assigned student number)
   - Enter “Yes” if prompted to accept the SSH connection
   - Provide the delphix password: *delphix*
   - Enter the following commands on the *linuxtarget*
   ```
   export ORACLE_SID=devdb
   sqlplus / as sysdba
   
   SELECT COUNT(*) FROM sourcetab1;
   ```
   Why did you get an error message stating “table or view does not exist”?
1. Switch back to the Delphix GUI in the browser showing the Self-Service interface, logged in as *dev*
1. Click on *ORCL Dev Container* and click **Refresh** option
1. After the Refresh operation completes, go back to the terminal window:
   - Exit from the SQL*Plus command if it is still connected
   - Enter the following commands on the *linuxtarget*
   ```
   sqlplus / as sysdba
   
   SELECT COUNT(*) FROM sourcetab1;
   ```
  Why did the `COUNT(*)` command work this time?
  Does the SOURCETAB1 table have the same number of rows as it had on the source database on the linuxsource server?

<a name="exercise5"></a>
## Exercise 5 – Create a new Branch

In this exercise, you will:
- Create a new branch on the ORCL Dev containers

### Steps
1. Login to the Self-Service interface as the dev user
1. Select *ORCL Dev Container*
1. Select the current point-in-time (shown by the red star) on the current incarnation of the default
timeline view
1. Click on the **Branch** button on the action bar to create a new branch
   - Enter: *Version1.0*
   - Click on **Create**
1. Go to **Branches** vertical tab along the left-side menu
   - The currently active branch should now be Version 1.0, indicated by the red star
   - The *default* branch should show as *inactivated*, indicated by the gray star

<a name="exercise6"></a>
## Exercise 6 – Switch between Branches
In this exercise, you will:
- View the currently active branch
- Activate the default branch
- Create and activate a new branch

### Steps
1. Login to the Self-Service interface as the dev user
1. A red star indicates the currently active branch. This should be the *Version1.0* branch created in the previous exercise.
1. Click on the *default* branch
1. Click on the **Activate** icon on the self-service toolbar. An action should be started with a progress bar.
1. You should now be on the *default* branch
1. Create a **Bookmark** at the current point-in-time on the timeline
   - Enter a name for the bookmark: *NewDefect*
   - Add a tag: *training*
1. Select the *NewDefect* bookmark and create a new branch
   - Enter a name for the branch: *BugFix NewDefect*

<a name="exercise7"></a>
## Exercise 7 – Create and Share a Bookmark
In this exercise, you will:
- Create a table named defect1_tab in your ORCL Dev container database
- Create a bookmark
- Share the bookmark

### Steps
1. Create a SSH connection to the *linuxtarget* host as the delphix user by entering the following commands in the terminal window:
   - *ssh delphix@10.0.x.30*               (Replace x with your assigned student number)
   - Enter “Yes” if prompted to accept the SSH connection
   - Provide the delphix password: *delphix*
   - Enter the following commands on the linuxtarget:
   ```
   export ORACLE_SID=devdb
   sqlplus / as sysdba
   CREATE TABLE defect1_tab AS SELECT * FROM dba_objects;
   SELECT COUNT(*) FROM defect1_tab;
   ```
1. Open a browser window and enter the IP address of your Delphix engine
1. Login to the Self-Service interface as the *dev* user
1. Select a point on the default timeline and click on the **Bookmark** icon
   - Enter a name for the bookmark: *Defect1*
   - Add a tag: *training*
1. Click on the bookmark created in step 4 then click on the **Show Data Details** link
   - Note the date and time when the bookmark was created
   - Click on the *Share* icon to share the bookmark
1. Select the current point-in-time on the timeline and create a new private bookmark called *Defect2* by clicking on **Bookmark** icon on the self-service controls toolbar
1. Add a tag named *training* to the bookmark
1. Log out of the Self-Service interface
1. Login to Self-Service as the *qa* user
   - Username: *qa*
   - Password: *delphix*
   - You will be prompted to change your password if you are logging in as the first time as that user
   - Enter qa as the new password
1. Once you are logged in, click on QA Container you will see the default timeline for the *qa* user. On the left panel you will see the **Summary, Sources, History, Bookmarks** and **Usage** views.
1. Click on the **Bookmarks** icon to view the bookmarks owned by the user
1. Click on the *Available* bookmark icon to see the bookmarks shared from other users.

<a name="exercise8"></a>
## Exercise 8 – Create a branch from a shared bookmark
In this exercise, you will:
- Switch to another JetStream user
- Find the available shared bookmark created by another JetStream user
- Create a new branch from the shared bookmark

### Steps
1. Login to Self-Service as the *qa* user
   - Username: *qa*
  - Password: *delphix*
1. Once you are logged, click on ORCL QA Container to see the default timeline for the *qa* user
1. View the **Summary, Sources, History, Bookmarks** and **Usage** views on the left.
1. Click on the **Bookmarks** tab to view bookmarks owned, shared by, and available to the *qa* user
1. Click on the **Private** icon to see bookmarks that are private to this user only
1. Click on the **Shared** icon to see bookmarks created by this user and shared with other users
1. Click on the **Available** icon to see books created by other users and shared with this user
1. Click on the available shared bookmark named Defect1 created from the dev user
1. Click on the **Branch** icon   on the right to create a new branch within the ORCL QA container based on the bookmark named *Defect1* shared from the ORCL Dev container
1. Create a SSH connection to the other *linuxtarget* host as the delphix user by entering the following commands in the terminal window:
   - *ssh delphix@10.0.x.30*  (Replace x with your assigned student number)
   - Enter “Yes” if prompted to accept the SSH connection
   - Provide the *delphix* password: *delphix*
   - Enter the following commands on the *linuxtarget*:
   ```
   export ORACLE_SID=devdb
   sqlplus / as sysdba
   
   SELECT COUNT(*) FROM defect1_tab;
   ```
<a name="exercise9"></a>
## Exercise 9 – Restore Data Container
In this exercise, you will:
- Create a new bookmark
- Restore container to bookmark

### Steps
1. Create a SSH session to your *linuxtarget* hosting your *devdb*
1. Connect to the devdb database and query the HR.JOB_HISTORY table
   ```
   sqlplus / as sysdba
   
   SELECT COUNT(*) FROM hr.job_history;
   ```
   Note the number or rows in the table.
1. Login to Self-Service as the dev user
   - Username: *dev*
   - Password: *Delphix*
   - Select the *ORCL Dev Container*
1. Create a new bookmark called *JobHistory*
1. On your *devdb* database execute the following SQL statements:
   ```
   DELETE FROM hr.job_history WHERE employee_id=101;
   ```
   You should have two rows deleted.
1. In the Self-Service interface select on the JobHistory  bookmark
1. Click on the **Restore** button on the self-service toolbar to restore the container
1. Verify that the two rows deleted previously have been restored

<a name="exercise10"></a>
## Exercise 10 – Lock a Data Container
In this exercise, you will:
- Lock a Data Container

### Steps
1. Log in with credentials:
   - Username: *delphix_admin*
   - Password: *delphix*
1. Click on the *delphix_admin* username on the top right of the GUI and select **Self-Service**
1. Click on ORCL Template and select Containers on the left.
1. Select the ORCL Dev container and Click Data Operations
1. Click on the Lock icon on the Self-Service Toolbar
1. Log out of the Self-Service GUI by clicking on the username on the top right and choosing the Logout option
1. Log in as the dev user. Select the ORCL Dev Container. The ORCL Dev container will appear with a lock icon next to it

Notice that all icons on the data operations have been disabled since they are now locked

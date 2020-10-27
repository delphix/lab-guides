# **Delphix Postgres LabGuide**

July, 2020

[Intro](#_d4vji24itlbc)

[Engine Configuration](#_ylg673fel744)

[Configure the Source](#_mm0uky6mu3uj)

[Basic Host Configuration](#_z4b3xqxma4k7)

[Prepping the Source for Delphix](#_i6gv6fq28q38)

[Configure Postgres](#_3xy8lahje3yo)

[Ensure Postgres Starts Upon Restart](#_o9bn18vzzsmf)

[Add Data](#_h0ca45t3opl1)

[Configure the Staging Host](#_hayb5ujte7nl)

[Basic Host Configuration](#_oeikototd4wl)

[Prepping the Staging Host for Delphix](#_2e543u5hxtds)

[Create the Staging Environment on the Delphix Engine](#_xs1i4gwafuxl)

[Configure the Target](#_hx5irc49cd53)

[Create the dSource and VDB](#_6am58lw6wai1)

[Create the dSource](#_34zf0msre3g9)

[Provision a VDB](#_ldeibed7kdl7)

#


# Intro

At the end of this lab exercise, you will have a fully functional system for creating masked copies of Postgres data. This lab starts from scratch, which is perhaps a bit unrealistic but allows for covering Delphix and Postgres in a sufficient level of detail. There are some exercises, of course, that are done for the sake of this seeming like a realistic system. These would be left out when a customer already has a running system.

**Assumptions**

- You&#39;re using LabAlchemy
- Your LabAlchemy course is running
- You have a source, target and staging server
- Your Delphix Engine is on 5.3.5 or greater

**Relevant Host Access Info**

| **Target Environment** | **IP Address** | **User** | **Purpose** |
| --- | --- | --- | --- |
| Source | 10.0.1.20 | _centos_ | Source database |
| Staging | 10.0.1.30 | _centos_ | Staging server and masking |
| Target | 10.0.1.40 | _centos_ | Target/copy databases |

**Relevant Network Info**

- **Delphix Engine:** 10.0.1.10
- **Jumpbox:** 10.0.1.5

# Engine Configuration

To begin, please do the following:

**Engine Version**

Ensure the engine is running the latest version of Delphix. The latest can be found at download.delphix.com.

**Install Plugin**

Support for Postgres requires the installation of the latest plugin. This plugin tells the Delphix Engine how to discover Postgres instances and provide virtual copies.

1. From download.delphix.com, go to the latest engine download folder (matching what you installed above)
2. Find the Postgres plugin: \&lt;x.y.z\&gt;/Plugins/PostgreSQL and download. Note: Delphix plugins will have different versioning from that of the Delphix Engine.
3. Extracting the downloaded file will give a JSON file.
4. Log in to the Engine and go to **Manage-\&gt;Plugins** and click the **+** icon
5. Drag (or upload) the downloaded JSON file

This will now show &quot;PostgresDB&quot; as an additional supported plugin.

# Configure the Source

Real deployments of Delphix will have an existing source and source database. Since we&#39;re setting up a demo system, we need to configure the source system (like our customers), the source database (somewhat like customers) and create data (not like customers).

## Basic Host Configuration

Ensure the system is configured correctly.

1. Access the source environment, this IP is matching what you see above:

ssh -i ~/internal/dxkey centos@10.0.1.20

2. Check that the OS version is supported. Supported versions can be found on the Postgres Support Matrix on docs.delphix.com. Assuming you&#39;re using CentOS:

cat /etc/centos-release

 ![](RackMultipart20201027-4-98fydx_html_ab47d642d692a05d.png)

 CentOS must be 7.4 or greater, depending on the specific Postgres version (below)

3. Check to see if Postgres is running:

ps -A | grep postgres

4. (Assuming Postgres is not running, if it is, skip ahead). Switch to root and either add the postgres user or reset its password to &quot;postgres&quot;. Linux will complain about the password choice, but ignore it.

sudo su -
 passwd postgres (set password as &quot;postgres&quot;)

5. Set up directories and change users

mkdir /usr/local/pgsql/data -p
 chown postgres /usr/local/pgsql/data
 su - postgres

6. Initialize Postgres

pg\_ctl init -D /usr/local/pgsql/data

7. Start Postgres

pg\_ctl start -D /usr/local/pgsql/data -l logfile

## Prepping the Source for Delphix

**Make Toolkit Directory**

1. As the centos user (if you&#39;re still using the &quot;postgres&quot; user, you may use &quot;exit&quot; to transition), create the directory for the Delphix toolkit

sudo mkdir /opt/delphix

2. Assign ownership to the &quot;postgres&quot; user

sudo chown postgres /opt/delphix/
 sudo chmod 770 /opt/delphix/

3. (Perhaps finish this later) SET Environment Variables

## Configure Postgres

**Create a role**

1. If you are not already using the &quot;postgres&quot; user, switch to the &quot;postgres&quot; user (password, set before, is &quot;postgres&quot;).

su - postgres

2. Start using psql

psql

3. Review existing roles. The role &quot;delphix&quot; should not exist.

select rolname from pg\_roles;

 Will return something like:

 ![](RackMultipart20201027-4-98fydx_html_c574635e6b570ce9.png)

4. Create a delphix role:

create role delphix superuser login replication password &#39;delphix&#39;;

5. Validate that the role exists now:

select rolname from pg\_roles;

6. Exit the client

exit

**Edit the Postgres Configuration File**

1. **Edit the postgres.conf file:** Here we will enable remote listeners

 As the root user

vi /usr/local/pgsql/data/postgresql.conf


2. Enable remote connections. Find the line that starts with &quot;listen\_addresses&quot; and set this to all by:

listen\_addresses = &#39;\*&#39;

3. Ensure &quot;port&quot; is in the file (may remain commented)

#port = 5432

4. Set the amount of data that is recorded in the write-ahead logs. In the Write-ahead log section, set wal\_level to logical

wal\_level = logical

5. If the number is low, Increase concurrent connections by four. In the replication section, set &quot;max\_wal\_senders&quot; by four. Setting to 10 should be sufficient.

max\_wal\_senders = 10

6. Save the file

**Edit the PG\_HBA file**

1. Edit the pg\_hba.conf file to allow client connections from the Delphix Engine, the staging

vi /usr/local/pgsql/data/pg\_hba.conf

2. Add five lines to the bottom of the file. This allows connectivity from the Delphix Engine, the staging server and the jump box on lines 2-4.

#Delphix Connections
 host all delphix 10.0.1.10/32 trust
 host all delphix 10.0.1.30/32 trust
 host all delphix 10.0.1.5/32 trust
 host replication delphix 10.0.1.30/32 trust


**Restart Postgres**

1. As the user &quot;postgres&quot;

pg\_ctl restart -D /usr/local/pgsql/data -l logfile

## Ensure Postgres Starts Upon Restart

We may choose to start and stop our lab. Rather than going through the process of starting Postgres each time, we&#39;ll create a rule to ensure that Postgres is started when the system does.

1. As centos, change directories to /etc/systemd/system
2. Create a new file for our rule:

sudo touch postgresql.service

3. Set the proper permissions

sudo chmod 644 postgresql.service

4. Edit the file:

sudo vi postgresql.service

5. Enter the following information and save the file:

[Unit]
 Description=PostgreSQL Database Server
 Documentation=man:postgres(1)

 [Service]
 Type=notify
 User=postgres
 ExecStart=/usr/pgsql-11/bin/postgres -D /usr/local/pgsql/data
 ExecReload=/bin/kill -HUP $MAINPID
 KillMode=mixed
 KillSignal=SIGINT
 TimeoutSec=0

 [Install]
 WantedBy=multi-user.target

6. Reload systemctl

sudo systemctl daemon-reload

7. Enable our script to run at startup:

 sudo systemctl enable postgresql

8. If you wish this prove this works, restart your lab class and check that Postgres is running by running:

ps -A | grep postgres

## Add Data

For this exercise to be interesting, we must have data in our source database. The following scripts will create two tables of data in the default Postgres database. This is assuming that you&#39;ve kept the standard names and passwords.

1. From the terminal on the jumpbox, change directory to /home/delphix/Scripts
2. In that folder, you&#39;ll find dbmanager.jar
3. Execute these statements in order (opening this on the jump box and copying pasting is your friend):

java -jar dbmanager.jar setupdb jdbc:postgresql://10.0.1.20:5432/postgres?username=delphix?password=delphix --table DEMO\_TABLE\_1?rows=5?orderby=id:ASCENDING --columns DEMO\_TABLE\_1:SEQUENCE?name=id,FIRST\_NAME,LAST\_NAME,SSN --table DEMO\_TABLE\_2?rows=5 --columns DEMO\_TABLE\_2:CREDIT\_CARD,TIMESTAMP?name=birthday?sequence=RANDOM --outputdb demo.cfg

java -jar dbmanager.jar createtables jdbc:postgresql://10.0.1.20:5432/postgres?username=delphix?password=delphix --inputdb ./demo.cfg

java -jar dbmanager.jar insertrows jdbc:postgresql://10.0.1.20:5432/postgres?username=delphix?password=delphix --inputdb ./demo.cfg --setrows 10000000

Note, the last step, inserting ten million rows into two tables will take a few minutes. Time for coffee.

# Configure the Staging Host

## Basic Host Configuration

The basic configuration for the staging host is the same as the source.

1. Access the environment, this IP is matching what you see above:

ssh -i ~/internal/dxkey centos@10.0.1.30

2. Check OS version is supported. Assuming CentOS:

cat /etc/centos-release

 ![](RackMultipart20201027-4-98fydx_html_ab47d642d692a05d.png)

 CentOS must be 7.4 or greater, depending on the specific Postgres version (below)

3. Check to see if Postgres is running:

ps -A | grep postgres

4. (Assuming Postgres is not running, if it is, skip ahead). Switch to root and either add the &quot;postgres&quot; user or reset its password to &quot;postgres&quot;. Linux will complain about the password choice but ignore it.

sudo su -
 passwd postgres (set password as &quot;postgres&quot;)

5. Set up directories and change users

mkdir /usr/local/pgsql/data -p
 chown postgres /usr/local/pgsql/data
 su - postgres

6. Initialize Postgres

pg\_ctl init -D /usr/local/pgsql/data

7. Start Postgres

pg\_ctl start -D /usr/local/pgsql/data -l logfile

## Prepping the Staging Host for Delphix

**Make Toolkit Directory**

1. As the centos user, create the directory for the Delphix toolkit

sudo mkdir /opt/delphix

2. Assign ownership to the &quot;postgres&quot; user

sudo chown postgres /opt/delphix/
 sudo chmod 770 /opt/delphix/
 sudo chgrp postgres /opt/delphix/

3. (Perhaps finish this later) SET Environment Variables

**Validate Permissions**

1. The operating system user must have read and execute privileges on the PostgreSQL binaries installed on the target environment. Run the fully query and ensure the last three letters match (r-x):

 ls -ld /usr/pgsql-11/bin

 ![](RackMultipart20201027-4-98fydx_html_74ad600b69508127.png)

2. The &quot;postgres&quot; user must have permission to run mount and umount as the superuser via sudo with neither a password nor a TTY. Edit the sudoers file and uncomment the line allowing people in group &quot;wheel&quot; to run all commands

sudo visudo

 ![](RackMultipart20201027-4-98fydx_html_3a2a6cfdc5addee2.png)

3. Add &quot;postgres&quot; to the wheel group:

sudo usermod -aG wheel postgres

4. Ensure the direction /mnt exists and has read/write/execute access for others:

sudo chmod o+w /mnt

5. Ensure the &quot;postgres&quot; user has access to the toolkit directory

sudo chmod o+rwx /opt/delphix/

## Create the Staging Environment on the Delphix Engine

1. On the Delphix Engine as an administrator, go to **Manage-\&gt;Environments**
2. Click Add Environment
3. Add an environment name &quot;Postgres Staging Host&quot;
4. Add the host address &quot;10.0.1.30&quot;
5. Select &quot;username and public key&quot;
6. Click view public key
7. Copy the public key
8. As the &quot;postgres&quot; user on the staging server, edit the ~/.ssh/authorized\_keys file and paste the key from the engine:

vi ~/.ssh/authorized\_keys

 It should look like:

 ![](RackMultipart20201027-4-98fydx_html_b4123d4ca7f46cbb.png)
9. Run the following to allow only the file&#39;s owner to read and write to it:

chmod 600 ~/.ssh/authorized\_keys


10. Run the following to restrict access to the user&#39;s home directory:

chmod 755 ~


11. Back on the Delphix Engine, enter the toolkit path (whatever you specified above)

/opt/delphix/


12. Click Next
13. Wait for the process to complete.
14. On the staging server, validate that in the toolkit path, there are now files

# Configure the Target

Follow the same steps as configuring the staging host, but with the IP address 10.0.1.40.

# Create the dSource and VDB

## Create the dSource

Before creating (one or more) copies of the source database, we need to create the Delphix-managed version, which we call the staging copy. This is managed on the staging host and will get data from the source via replication.

**Establish the Installation**

1. On the Delphix Engine as an administrator, navigate the **Manage-\&gt;Environments**
2. Select &quot;Postgres Staging Host&quot;
3. Click &quot;Databases&quot;
4. In the &quot;Postgres&quot; Installation Click the **+** to add a new Database
5. Name it &quot;Postgres&quot;

**Add a dSource**

1. Navigate to **Manage-\&gt;Datasets**
2. Click the **+** and &quot;Add dSource&quot;
3. Click &quot;Next&quot;
4. On the &quot;Source&quot; window select &quot;Postgres&quot;
5. Check the &quot;Delphix Initiated Backup Flag&quot;
6. Under &quot;Delphix Initiated Backup/External Backup - Streaming Replication&quot; click &quot;Add&quot;
7. Set the &quot;PostgresDB Replication User&quot; and password to be &quot;delphix&quot;
8. Set the source host address to &quot;10.0.1.20&quot;
9. Leave the source port as &quot;5432&quot;
10. Leave the staging port as &quot;5433&quot; Since we have an instance of Postgres already running on this instance, we must provide a new port for the second instance.
11. Click Next
12. On the dSource Configuration page name the dSource name as &quot;Postgres Source&quot;
13. Create a group &quot;Postgres&quot;
14. Click Next
15. Leave the &quot;staging environment&quot; and &quot;user&quot; as they are. Set &quot;Mount Base&quot; to &quot;/mnt&quot;
16. No changes to policies
17. No hooks
18. Review the summary and click &quot;Submit&quot;

## Provision a VDB

Do it

Just do it™

# Getting Started

## Welcome to the Lab Guide

This guide is a supplement to the Delphix Admin Training courses,
and provides several exercises to perform throughout the class. If you
encounter any issues during the exercises, please do not hesitate to ask your
instructor for advice.

## Lab Requirements

In order to perform these lab exercises, you will need:

  * A modern HTML5 capable web browser (IE10+, Chrome, Firefox, Safari)

## The Delphix Admin Training Cloud Lab

Your instructor should have provided you with a **Class Name** and a **Student
Number**. In order access your lab server, point your web browser to: http://
**classname**.agile.today/ **studentnumber**  
For example, if your **Class Name** is  "acmetech" and your **Student Number**
is 5, you would go to the site: http://acmetech.agile.today/5  

![images/worddav9a8e19107bb12a1034a2f25f9f5a909f.png](images/worddav9a8e19107bb12a1034a2f25f9f5a909f.png)  
The Delphix Labs Login Dialog  

At the login screen, enter the username and password: delphix/delphix  
Once you have logged in, you will be connected to your lab server. This server
contains everything you will need to perform your lab exercises including:

  * RDP Client (Remmina) to connect to Windows Source and Target servers
  * Terminal and PuTTY to connect to Linux Source and Target servers
  * Firefox Web Browser to connect to your Delphix Engine
  * Chrome Web Browser
  * Notepad for class notes
  * Lab Guides folder on the Desktop containing any exercises for this course, including this guide.

**IMPORTANT NOTE:** Do not use the  "Log Out" function on your lab server. If
you do, it will break your lab connection.  


![images/worddav177dbea07c3e4a6fa6757816c62ad647.png](images/worddav177dbea07c3e4a6fa6757816c62ad647.png)  
The Delphix Lab Server

### Important IP addresses for Oracle labs

In the IP addresses below, the "x" denotes your **Student Number**. For
example, if your student number is  "5," your Delphix Engine will be located at "10.0.5.10".

**NOTE: Please record your student number and IP addresses, as using another student's number will cause problems during the exercises**

Server  | IP Address
------- | -----------
Delphix Engine | 10.0.x.10  
Linux Source | 10.0.x.20  
Linux Target| 10.0.x.30

### Cloud lab usernames and passwords for Oracle labs

Server | Username | Default Password
------ | -- | ----------
Delphix Engine | (Sysadmin) sysadmin | sysadmin  
Delphix Engine | (Delphix Admin) delphix_admin | delphix
Linux Source | delphix |delphix  
Linux Target | delphix |delphix  

### Important IP addresses for MS SQL labs

Server  | IP Address
------- | -----------
Delphix Engine | 10.0.x.10  
Windows Source | 10.0.x.50  
Windows Target| 10.0.x.60  


### Cloud lab usernames and passwords for MS SQL labs

Server | Username | Default Password
------ | -------- | ----------
Delphix Engine | (Sysadmin) sysadmin | sysadmin  
Delphix Engine | (Delphix Admin) delphix_admin | delphix  
Windows Source Domain |  delphix\delphix_src |delphix  
Windows Source MS SQL Admin|  sa |delphix  
Windows Target Domain | delphix\delphix_trgt |delphix  

## The RDP Client

Your lab server contains an RDP client that is pre-populated with the
Windows Source and Target servers. When you connect to a Windows system, the
screen will be larger than your Student Desktop. You can use the toolbar at
the top of the RDP client to switch to Full Screen mode. When you wish to
leave Full Screen mode, you can hover your mouse over the top of the Windows
screen until the RDP menu drops down, and toggle off Full Screen.

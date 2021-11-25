 # README

 > **NAME**

How to Restore a MySQL Database From a Backup With MySQL Workbench

 > **DESCRIPTION**


If you perform your own database backups, it’s also possible to do your own database restoration without relying on a host or third party. Let’s see what it takes to restore a MySQL database with Workbench.

Before we get started, our tutorial, “Making a MySQL Database Backup With MySQL Workbench,” covers the backup part of the equation (using MySQL Workbench).

In this tutorial, we will go through the steps to restore a database from a backup. We will also cover the necessary configuration to connect to your database with MySQL Workbench. This will cover everything for you in one spot, in case you’ve never done so.

[Reference](https://www.greengeeks.com/tutorials/restore-a-mysql-database-from-a-backup-with-mysql-workbench/)

  > **VISUAL**

![Open mySQL workbench](https://www.greengeeks.com/tutorials/wp-content/uploads/2019/04/MySQL-Workbench-database-restore-01.png)

![Test Connection](https://www.greengeeks.com/tutorials/wp-content/uploads/2019/04/MySQL-Workbench-database-restore-02.png)

![Click the “OK”](https://www.greengeeks.com/tutorials/wp-content/uploads/2019/04/MySQL-Workbench-database-restore-03.png)

![Now click the “OK” button in “Manage Server Connections”](https://www.greengeeks.com/tutorials/wp-content/uploads/2019/04/MySQL-Workbench-database-restore-04.png)

  > **INSTALLATION**




        We are using a 64-bit Windows 10 system to which we have connected via Remote Desktop (RDP).  This entire topic was conducted on that machine via RDP.   We have downloaded the Oracle installation package for MySQL from https://dev.mysql.com/downloads/mysql/   - the file we have downloaded and have now launched is called mysql-installer-community-8.0.15.0.msi,  approximately 332 MB in size.  A web installer is also available, which downloads installation files as need be.




1. double-click the mysql-installer-community-8.0.15.0.msi file to launch the installer.

![Instruction 1](https://manifold.net/doc/mfd9/images/eg_install_mysql01_01.png)





2. Check the I accept box and press Next.

![Instruction 2](https://manifold.net/doc/mfd9/images/eg_install_mysql01_02.png)





3. For this example we will choose a simplified, but fully functional, installation.  Click Custom and then press Next.

![Instruction 3](https://manifold.net/doc/mfd9/images/eg_install_mysql01_03.png)





4. Expand the MySQL Servers heading, click on MySQL Server 8.0.15 - X64 to highlight it, and then press the green right arrow button.

![Instruction 4](https://manifold.net/doc/mfd9/images/eg_install_mysql01_04.png)



5. That adds MySQL Server 8.0.15 - X64 to the list of products/features to be installed.   Next, we expand the Applications heading, click on the MySQL Workbench 8.0.15 - X64 line to highlight it, and then again we press the green right arrow button.

![Instruction 5](https://manifold.net/doc/mfd9/images/eg_install_mysql01_05.png)



6. That adds MySQL Workbench 8.0.15 - X64 to the list of products/features to be installed.   Strictly speaking, we do not need to add the MySQL Workbench as we will not use it in these topics, but having the Workbench around for use is not a bad idea in any MySQL installation so we add it just in case it might be useful in the future.   Press Next.

![Instruction 6](https://manifold.net/doc/mfd9/images/eg_install_mysql01_06.png)



7. The dialog lists the products to be installed.  Press Execute.

![Instruction 7](https://manifold.net/doc/mfd9/images/eg_install_mysql01_07.png)


8. If we were using a web installer, at this point it would download the two products and begin installing them.  We are using a local installer so it immediately just installs them.  When finished, the Status will show as Complete and green check marks will appear by each product.   Press Next.

![Instruction 8](https://manifold.net/doc/mfd9/images/eg_install_mysql01_08.png)


9. The dialog moves on to Product Configuration, with only MySQL Server requiring configuration.   Press Next.

![Instruction 9](https://manifold.net/doc/mfd9/images/eg_install_mysql01_09.png)


10. We leave the default choice of a standalone MySQL Server installation.   Press Next.

![Instruction 10](https://manifold.net/doc/mfd9/images/eg_install_mysql01_10.png)


11. In the Type and Networking step we choose Development Computer, the default choice.   This uses minimal memory, which is what we want since for the sake of this topic we are installing MySQL on a machine that hosts many examples.  If the machine were to be used as a host for a big MySQL installation that was intended to serve many client computers on which people were doing GIS, connected to MySQL, we would choose either Server Computer or Dedicated Computer.

![Instruction 11](https://manifold.net/doc/mfd9/images/eg_install_mysql01_11.png)


12. We leave checked the default choice of opening a port in Windows Firewall, thanking MySQL for the courtesy of doing this for us.   Press Next.

![Instruction 12](https://manifold.net/doc/mfd9/images/eg_install_mysql01_12.png)

    
13. We change the authentication method to Use Legacy Authentication.  

 

Tech Tip: MySQL Versions from 8.0 onward provide stronger authentication, but to make that stronger authentication work all clients which connect to a MySQL Server installed with the Use Strong Password choice must use 8.0 level client .dll files.   In real life, many packages, for example GDAL, will install MySQL client .dll files that are older than 8.0 and which will not work with MySQL Servers installed with the Use Strong Password choice.   If those .dll files appear in the Windows PATH ahead of more recent, 8.0 MySQL client .dll files, we can encounter the chaotic situation where connection attempts from remote machines will fail, and will do so in a highly confusing way.   In other cases, users might be working with older versions of MySQL client .dll files that they have downloaded in the past and have failed to update.  Those will not work either.  

 

The easiest way to avoid such chaos is to install MySQL with the Use Legacy Authentication option checked.  In that case, both older and newer MySQL client .dll files will work.   Press Next.

![Instruction 13](https://manifold.net/doc/mfd9/images/eg_install_mysql01_13.png)


14. Enter a password for the root account.   We use a password of 12345xy, which is an absurdly weak password.  In real life, use a stronger password.   We will also add a user account by pressing the Add User button.

![Instruction 14](https://manifold.net/doc/mfd9/images/eg_install_mysql01_14.png)



15. We add a user name Fred, also using a weak password of 12345xy.   In real life, we would use a stronger password and a different password than root.   We have made Fred a DB Admin, so the Fred account can do superuser things like creating and dropping databases.    We leave the Host setting at the default of <All Hosts (%)> and Authentication of MySQL.  Press OK.

 

Tech Tip: One of the quirks of MySQL is that a user account is not just the user's login name, such as Fred, plus a password, such as 12345xy.  In addition, the machine from which the user connects to MySQL, called the Host, is also part of the set of credentials that allows or does not allow a given user to connect to MySQL.    For example, the default superuser login of root works only when connecting to MySQL from the same machine on which MySQL is installed.   If root tries to connect to MySQL from a different machine, say, a machine with the system name of PASCAL, the login will be rejected with a message (reported in the Manifold Log Window) of:

 

*** (root)::[MySQL] Error 1045 (28000): Access denied for user 'root'@'PASCAL' (using password: YES)

 

As far as MySQL is concerned, root trying to connect from the machine PASCAL is not the same root as trying to connect from the localhost.  Lucky for us, the default setting of Host when adding a new user, as seen in the illustration above, is to specify the parameter %, which means "accept this user login from any host."   User Fred, therefore, will be able to connect to MySQL from any other remote machine.

![Instruction 15](https://manifold.net/doc/mfd9/images/eg_install_mysql01_15.png)


16. After adding Fred as a user, we again click Add User to add user Lucy, using the same settings as Fred.  After doing that we get the display above.  Press Next.

![Instruction 16](https://manifold.net/doc/mfd9/images/eg_install_mysql01_16.png)


17. In the Windows Service dialog we accept all defaults.   Press Next.

![Instruction 17](https://manifold.net/doc/mfd9/images/eg_install_mysql01_17.png)


18. We get one last chance to go back and change any settings.  Press Execute.

![Instruction 18](https://manifold.net/doc/mfd9/images/eg_install_mysql01_18.png)


19. After all configuration settings are applied, press Finish.

![Instruction 19](https://manifold.net/doc/mfd9/images/eg_install_mysql01_19.png)


20. The result is to go back to the Product Configuration dialog, with a note that configuration is complete.    Press Next.

![Instruction 20](https://manifold.net/doc/mfd9/images/eg_install_mysql01_20.png)


We do not want to launch MySQL Workbench, so we uncheck that box.   Press Finish.

 

We have accomplished the installation of MySQL, and we now have an operating MySQL server running on our machine.  
<br>



> **AUTHOR**

> Author Name: Nickol John Eduard M. Sendito


![DATABASE](https://scontent.fmnl13-2.fna.fbcdn.net/v/t39.30808-6/257413115_3063682013889044_3350552190462352966_n.jpg?_nc_cat=111&ccb=1-5&_nc_sid=09cbfe&_nc_eui2=AeFEczp21dKq2_2lKtxKpG-d4HF64tYHc4TgcXri1gdzhNmdVS7SAIk6f8E9Y_Px8DaZ5BASjGpUzDyNEgAC85GL&_nc_ohc=rELol8wQcdUAX_DcEpX&_nc_ht=scontent.fmnl13-2.fna&oh=3b2435c9a6c10537d34dbfdd810413c4&oe=61A4FC42)

|  **References**                                     |
| ------------------------------------------------------------ |
| [How to Install MySQL](https://manifold.net/doc/mfd9/install_mysql.htm)<br/>https://github.com/matiassingers/awesome-readme | 

<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket: From Installation to Ticket Resolution</h1>
This tutorial is a complete guide to installing, configuring and implementing the open-source help desk ticketing system osTicket. First we will cover the prerequisites and installation process, then the configuration required post-install. Finally we will learn the lifecycle of a supoort ticket from intake all the way through to resolution.<br/>

<h3>Environments and Technologies Used</h3>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h3>Operating System</h3>

- Windows 10</b> (21H2)

<h2>Part 1 -Prerequisites and Installation</h2>

<h3>Step 1: Resource Groups</h3>
<p>
We will first need to create a Virtual Machine. To do this we will be using a a cloud computing platform called Microsoft Azure: https://portal.azure.com/ . After logging in to your Microsoft Azure account, create a Resource Group. You can find this using the search bar at the top of the page. We will name this Resource Grop "osTicket". No need to add any tags at this time, select "next" until you are prompted to create the Resource Group.
</p>
<p>
<img src=https://i.imgur.com/86tpldV.png/><img src=https://i.imgur.com/1mHRpgF.png/>
</p>
<br />

<h3>Step 2:Virtual Machines</h3>
<p>
Using the same search bar as before, find and select the "Virtual Machines" service. We will now create the Virtual Machine that will host our implemantation of osTicket. Start by sleceting the "Create" button in the center of the page, then select "Azure virtual machine". Select "osTicket" as your Resource Group. We will name our VM "VM-osTicket". Select Windows 10 Pro as an operating system. In order to ensure our VM runs smoothly, we will choose the 4 vcpu, 16 Gib memory size. 
</p>
<p>
<img src=https://i.imgur.com/N51xQFo.png/><img src=https://i.imgur.com/6ZbLPUC.png/>
</p>
<br />

<h3>Step 3:Connecting to the VM via Remote Desktop Connection</h3>
<p>
Remote Desktop Connection is a program found on Windows that allows us to remotely access the VM we just created. First find Remote Desktop Connection via the Windows search bar, or type "MSTSC" into the command prompt. Next, enter the Public IP address for the VM. This can be found on the "Virtual machines" page to the far right of the screen. You will be asked to enter the credentials you created for the VM.
</p>
<p>
<img src=https://i.imgur.com/vRfL9WX.png/>
</p>
<br />

<h3>Step 4: Configuring Windows and Installing Prerequisite Software</h3>
<p>
We will first need to install Internet Information Services. Navigate to Control Panel -> Programs -> "Turn Windows features on or off" -> World Wide Web Services -> Application Development Features. Enable CGI and Common HTTP Features. Under Internet Information Services -> Web Management Tools enable the IIS Management Console. After selecting "ok" the features will install.
</p>
<p>
<img src=https://i.imgur.com/jilgY4g.png/>
</p>
<p>
Next, create the directory C:\PHP
</p>
<p>
<img src=https://i.imgur.com/JJLB7EA.png/>
</p>
<p>
Now we need to install some software. You can find a file containing all the software you will need at this link: https://drive.google.com/drive/folders/1a2pqyzWvCMLdfVghsQCYTd9C_zm37-ro?usp=drive_link . Using a browser on the VM, download all the files. Then unzip them. Install the files in the following order: 
<p>
1. PHPManagerForIIS_V1.5.0
</p>
<p>
2. rewrite_amd64_en-US
</p>
<p>
3. Extract the contents of php-7.3.8-nts-Win32-VC15-x86.zip to C:\PHP
</p>
<p>
4. Install VC_redist.x86
</p>
<p>
5. Install mysql-5.5.62-win32 -> Typical Setup -> Launch Configuration Wizard (after install) -> Standard Configuration -> Password1
</p>
<p>
6. Start IIS by entering InetMgr.exe into the Run utility -> Select PHP manager -> Select "Register new PHP version" -> Navigate to C:\PHP\php-cgi.exe
</p>
Return to the main paige of IIS, then select "Restart" under the "Actions" tab.
</p>
<p>
<img src=https://i.imgur.com/cRHq6wA.png/>
</p>
<p>
7. Extract osTicket v1.15.8 -> Copy "upload" folder to c:\inetpub\wwwroot -> Within c:\inetpub\wwwroot, Rename “upload” to “osTicket” -> Once again return to the main paige of IIS, then select "Restart" under the "Actions" tab.
</p>
<p>
8. Once again return to the main paige of IIS, then select "Restart" under the "Actions" tab. -> Under the "Connections" tab navigate to Sites -> Default -> left click osTicket folder -> select “Browse *:80 (http)” on the right. This is a preview of the osTicket application running on a browser, but we still need to enable some extensions. 
</p>
<p>
9. Return to the osTicket folder in IIS. Double-click PHP Manager -> Click “Enable or disable an extension” -> Enable: php_imap.dll -> Enable: php_intl.dll -> Enable: php_opcache.dll -> Refresh the osTicket site in your browser and observe the changes.
</p>
<p>
<img src=https://i.imgur.com/0bHqepX.png/>
</p>
<p>
10. Rename C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php to C:\inetpub\wwwroot\osTicket\include\ost-config.php
</p>
<p>
11. Right-click ost-config.php -> Properties -> Security -> Advanced -> Disable Inheritance -> Remove All -> Add -> Select a Pricipal -> Type "Everyone" -> "OK" -> Full Control -> "OK" -> Apply
</p>
<p>
12. Return to osTicket Running in the browser. -> Select "Continue" -> Name Helpdesk -> Choose Default email (receives email from customers) -> Enter Admin Udser information
</p>
<p>
13. Open HeidiSQL_12.3.0.6589_Setup -> Select the link within to download the exe file -> Install HeidiSQL_12.3.0.6589_Setup -> Open Heidi SQL -> Create a new session -> User: root -> Password: Password1 -> Connect to the session -> Create a database called “osTicket” -> Return to osTicket on browser -> MySQL Database: osTicket -> User and password are the same as above -> Select "Install Now!"
</p>
<p>
14. Finally, delete the folder C:\inetpub\wwwroot\osTicket\setup -> Navigate to C:\inetpub\wwwroot\osTicket\include\ost-config.php ->Right-click -> Properties -> Security -> Advanced -> Select "Everyone" -> Edit -> Basic Permissions -> Leave only "Read & Execute" -> "OK" -> Apply -> "OK" 
</p>
<p>
15. You can now enter http://localhost/osTicket/scp/login.php into the VM's browser to log in to the help desk. Use http://localhost/osTicket/ to access the end user side of the system.
</p>
<br />

<h1>Part 2 - Post-Install Configuration</h1>
We can now log in to the osTicket system and configure our settings. The system is seperated into two panels, the Agent and the Admin panel.
<h3>Step 1: Configure Roles</h3>
<p>
Admin Panel -> Agents -> Roles
</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 2: Configure Departments</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 3: Configure Teams</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 4: Allow anyone to create tickets</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 5: Configure Agents</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 6: Configure Agents</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 7: Configure Users (customers)</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 8: Configure SLA</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h3>Step 9: Configure Help Topics</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />

<h1>>Part 3 - Ticket Lifecycle: Intake Through Resolution</h1>
This tutorial outlines the lifecycle of a ticket from intake to resolution within the open-source help desk ticketing system osTicket.<br />

<h2>Ticket Lifecycle Stages</h2>

- Intake
- Assignment and Communication
- Working the Issue
- Resolution

<h2>Lifecycle Stages</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

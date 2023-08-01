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
<img src=https://i.imgur.com/8NU0JbM.png/>

<h3>Step 2: Virtual Machines</h3>
<p>
Using the same search bar as before, find and select the "Virtual Machines" service. We will now create the Virtual Machine that will host our implemantation of osTicket. Start by seleceting the "Create" button in the center of the page, then select "Azure virtual machine". Select "osTicket" as your Resource Group. We will name our VM "VM-osTicket". Select Windows 10 Pro as an operating system. In order to ensure our VM runs smoothly, we will choose the 4 vcpu, 16 Gib memory size. 
</p>
<img src=https://i.imgur.com/vOJ5nYJ.png/>

<h3>Step 3: Connecting to the VM via Remote Desktop Connection</h3>
<p>
Remote Desktop Connection is a program found on Windows that allows us to remotely access the VM we just created. First find Remote Desktop Connection via the Windows search bar, or type "MSTSC" into the command prompt. Next, enter the Public IP address for the VM. This can be found on the "Virtual machines" page to the far right of the screen. You will be asked to enter the credentials you created for the VM.
</p>
<p>
<img src=https://i.imgur.com/vRfL9WX.png/>
</p>
<br/>

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
<br/>

<h1>Part 2 - Post-Install Configuration</h1>
We can now log in to the osTicket system and examine it's various functionalities and configure custom settings. If you would like more information on any of the topics covered, you can access documentation at: https://docs.osticket.com/en/latest/index.html

<h3>Step 1: Configure Roles</h3>
<p>
The system is seperated into two panels, the Agent and the Admin panel. We will first navigate to the Admin Panel -> Agents -> Roles -> Add New Role -> Name: Supreme Admin(or whatever you prefer) -> Permissions -> Check all boxes under Tickets, Tasks and Knowledgebase.
</p>
<p>
<img src=https://i.imgur.com/h6CYJaM.png/>
</p>
<br/>

<h3>Step 2: Configure Departments</h3>
<p>
Next we can navigatre to the "Departments" tab in the same "Agents" section. Then Add New Role -> Name: System Administrators 
</p>
<br/>

<h3>Step 3: Configure Teams</h3>
<p>
We can now navigatre to the "Teams" tab in the same "Agents" section. Then Add New Team -> Name: Level II Support 
</p>
<p>
<img src=https://i.imgur.com/NO73ZwJ.png/>
</p>
<br/>

<h3>Step 4: Configure Agents(Employees)</h3>
<p>
The lastr section is the the "Agents" tab in the same "Agents" section as before. Then Add New Agent -> We can create two agents. One named Jane Doe and the other John Doe. Email address can be set to something like jane.doe@helper.com. Username: jane.doe -> Password: Password1. Keep in mind hese names are just placeholders. We can place them in the System Administrators Department, -> Role: Supreme Admin -> Permissions: All -> Team: Level II Support . Let's also extended access to the "Support" department. Repeat these steps for an agent named "John Doe". Make sure to make a record of their login info. 
</p>
<p>
<img src=https://i.imgur.com/vHPJ9Pn.png/>
</p>
<p>
<img src=https://i.imgur.com/j6vM8N4.png/>
</p>
<br/>

<h3>Step 5: Allow anyone to create tickets</h3>
<p>
Now we will need to adjust a setting that controls who may create tickets. Navigate to Admin Panel -> Settings -> Users . Make sure the "Registration Required" field is <u>unchecked</u>.
</p>
<p>
<img src=https://i.imgur.com/fK8NRG6.png/>
</p>
<br/>

<h3>Step 6: Configure Users(Customers)</h3>
<p>
Now we will need to switch to the Agent Panel at the top right of the page. Then -> Users -> Add New. We will add two users, Ken Ken and Karen Karen. Emails can be set to ken@osticket.com and karen@osticket.com.
<p>
<img src=https://i.imgur.com/hYpajDX.png/>
</p>
<br/>

<h3>Step 7: Configure Serveice Level Aggreements SLAs</h3>
<p>
Next we can switch back to the Admin Panel -> Manage -> SLA -> Add New SLA Plan. We will add a total of 3 SLA Plans:
<ul>
  <li>Sev-A: 1 hour, 24/7</li>
  <li>Sev-B: 4 hours, 24/7</li>
  <li>Sev-C: 8 hours, business hours(24/5)</li>
</ul>
</p>
<p>
<img src=https://i.imgur.com/7Nl0q2u.png/>
</p>
<br/>

<h3>Step 8: Configure Help Topics</h3>
<p>
We can find the last step at Admin Panel -> Manage -> Help Topics -> Add New Help Topic. We will add 4 new help topics:
<ul>
  <li>Business Critical Outage</li>
  <li>Personal Computer Issues</li>
  <li>Equipment Request</li>
  <li>Password Reset</li>
</ul>
</p>
<p>
<img src=https://i.imgur.com/ezuno1Q.png/>
</p>
<br/>

<h1>>Part 3 - Ticket Lifecycle: Intake Through Resolution</h1>
We can now act both as the end user and the IT support agent to simulate resolving a few different tickets.

<h2>Ticket Lifecycle Stages</h2>

- Intake
- Assignment and Communication
- Working the Issue
- Resolution

<h3>Step 1: Create Support Tickets as an End User</h3>
<p>
Use the following link to access the end user side of odTicket: http://localhost/osTicket/ -> Open a New Ticket. We will now create 3 tickets:
</p>
<h3>Business Crtical Outage</h3>
<p>
<img src=https://i.imgur.com/ywCGo0p.png/>
</p>
<h3>General Inquiry</h3>
<p>
<img src=https://i.imgur.com/HABjTSe.png/>
</p>
<h3>Personal Computer Issues</h3>
<p>
<img src=https://i.imgur.com/4WkgXwS.png/>
</p>
<br/>

<h3>Step 2: Triage and Assign Tickets</h3>
<p>
Now we can return to the help desk login page and log in as one of our agents, Jane Doe. Enter the login information we set earlier. We can now see the 3 tickets we made. We can adjust the priority, SLA and assign these tickets to our agents. For example, our ticket from Karen heavily impacts the business and customers, so we would assign a high priority and SEV-A SLA. Since the support team would most likely not independently deal with an issue of this severity, we can also transfer it to the System Administrator department. We can configure the tickets in the following way:
</p>
<p>
<img src=https://i.imgur.com/9gVu7AT.png/>
</p>
<p>
<img src=https://i.imgur.com/L2mBqRw.png/>
</p>
<p>
<img src=https://i.imgur.com/ELDOjam.png/>
</p>
<p>
<img src=https://i.imgur.com/uvDzuSP.png/>
</p>
<br/>

<h3>Step 3: Resolving Tickets</h3>
<p>
Each ticket will have a thread documenting each action taken on the ticket and all communication related to it. It is always good practice to send a short reply when you recieve a ticket to acknowledge you are working on the issue, especially if it is serious. 
</p>
<p>
<img src=https://i.imgur.com/51bq2Zl.png/>
</p>
<ul>
  <li>Online Banking System is Down: Because the issue with online banking severly impacts the business, that would be our first priority. Still logged in as Jane Doe, we can select this SEV-A ticket and scroll down to post a reply. Once we have found a solution, we can post a reply describing what was done and mark the ticket as "Resolved"<img src=https://i.imgur.com/Bsir8XC.png/></li>
  <li>Adobe Reader Broken in Accounting Department: This ticket was assigned to John Doe, so let's first log into his account, then we can follow the same steps to resolve this ticket. Although the issue itself does not directly impact business, it is affecting a high number of employees, the entire accounting department. So we set this to a SEV-B and need to get it resolved within 4 hours. In this case we may need to find a temporary solution to give us time to reolve the deeper issue. <img src=https://i.imgur.com/2P7uKrs.png/><img src=https://i.imgur.com/FNNFJWo.png/></li>
  <li>When are we Getting a Hardware Refresh?: In the case of someone looking for information, we can update them on the situation and usually mark the ticket as resolved right away. <img src=https://i.imgur.com/urCjzNx.png/> </li>
</ul>

<p>

</p>
<br />


<h3>Step 1: Configure Roles</h3>
<p>

</p>
<p>
<img src=/>
</p>
<br />



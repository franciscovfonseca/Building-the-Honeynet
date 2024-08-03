<br>

<h1 align="center">Stage I - Building the (mini) Honeynet</h1>

<br>


![Banner](https://github.com/franciscovfonseca/Setting-Up-Vulnerable-VMs-in-Azure/assets/172988970/6d3ace39-dbbe-43fc-8aad-fdd636ead33c)
<br />
<br />

To kick off my **Azure Honeynet Project**, we must first set up the **Virtual Machines (VMs)** we'll use.

Virtual machines are like computers in the cloud, and they'll form the foundation of our Honeynet.

Here are the steps we'll take in **Microsoft Azure**:

<br />

<details close> 
<summary> <h2>1️⃣ Creating Resources</h2> </summary>
<br>

In this section of the project ➜ we'll set up **2 Target Virtual Machines** and **1 Threat Virtual Machine**.

- 2 Windows VMs (one used for "Attack") and 1 Linux VM.

<br>

### ❶ Create the Subscription

<br>

Here is an overview for the next few steps in this section:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

I'm starting with creating the **Subscription**.

⚠️ Don't forget to set a budget!

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

>   <details close> 
>   
> **<summary> 💡 Note</summary>**
>   
> Later, I actually changed the alert threshold to something more reasonable like 90%.
> 
>   </details>

<br>

<h2></h2>

<br>

### ❷ Create the First Resource Group

<br>

Next, create the first resource group ```RG-Cyber-Lab```.

This one will house the resources that will be exposed to attack ➜ the **Honeynet**.

Technically, this can be created at the same time as you create your VMs.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

### ❸ Create Virtual Machines

<br>

- Next we'll create **2 Virtual Machines** ➜ 1 Windows VM & 1 Linux VM ➜ using mostly Default Settings.

- Add both to the ```RG-Cyber-Lab``` Resource group.

- Create a new Virtual Network ➜ the **Honeynet** (Lab-VNet).

<br>

#### ➡️ Create Windows VM

Basics tab:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

Create a New Virtual Network:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

Networking tab:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

#### ➡️ Create Linux VM

Create the **Linux VM** with the same **User**, **Region**, **Resource Group** and **Networking Settings**.

Basics tab:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

Networking tab:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>2️⃣ Exposing the Resources</h2> </summary>
<br>

After both Virtual Machines are deployed, configure both Network Security Groups (NSGs) to allow All Inbound Traffic.

We're removing Rules for RDP and SSH and replacing them with the Custom Inbound Rule.

<br>

### ❶ Windows NSG Before:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

### ❷ Linux NSG Before:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

### ❸ Custom Inbound Rule:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

### ❹ Windows NSG After:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

At this point both NSGs are identical.

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Create Vulnerabilities</h2> </summary>
<br>
  
### ❶ Disable Windows Firewall

The goal is to expose these VMs to threat actors and to make them both discoverable and reachable ➜ so that we can **Monitor, Log & Investigate Incidents** later.

By default **Windows Firewall**, blocks ICMP packets from the internet ➜ you can see that the ```windows-vm``` is unreachable from another network:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

So from here ➜ Disable the Firewall (wf.msc).

#### ⏪ Before:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

#### ⏩ After:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Pinging ```windows-vm``` again to see test if it successfully Pings now:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Connecting to the ```linux-vm``` via SSH using **PuTTy**:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Test pinging ```linux-vm``` externally:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

### ❷ Install MS SQL Server + Utilities

Next, download and install **[SQL Server Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2019)**.

Select ***Download Media***.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Select ***ISO*** and the ***Download Location***.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Once the download completes ➜ go to the Download Location.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Mount the ISO:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Run the installer:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Select **New SQL Server**:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Include ☑️ **Database Engine Service**:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Select ⦿ Mixed Mode ➜ create a user and select **Add Current User** to also allow for Windows authentication using ```labuser```.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Once the installation completes, download and install **[MS SQL Management Studio (SMSS).](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)**.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Once the installation completes, restart the VM.

<br>

#### ➡️ Enable Logging on SQL Server

After restarting, follow the **[Microsoft documentation](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16)** for adjusting settings to allow SQL Server logs to be ported to Windows Event Viewer.

Provide full permission for the SQL Server service account (NETWORK SERVICE) to the registry hive.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Configure the audit object access setting in Windows using auditpol by executing the provided command line statement.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Launch SSMS and log in to the SQL Server. Then go to Properties > Security > Enable both

<br>

<h2></h2>

<br>

#### ⏪ Security Settings Before:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

#### ⏩ Security Settings After:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Restart the SQL Server and try to generate some failed authentication logs by trying log into the SQL server with the wrong password.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Check Event Viewer to make sure the logs are properly enabled and porting to Event Viewer successfully.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<h2></h2>

<br>

#### ➡️ Create Attack (Threat) VM

Create another Windows VM in a different resource group, region, and virtual network.

All other settings can be the same or similar.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

### ❸ Generate Logs

To make sure everything is working as expected, log into the ```attack-vm``` to generate failed authentication logs on both vms.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Generating failed RDP logs on ```windows-vm```:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Using PowerShell to generate failed login logs on linux-vm:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Installing SMSS on attack-vm:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Generating failed login logs for MS SQL Server on windows-vm:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

### ❹ Check Logs

Using PowerShell to SSH into linux-vm:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Investigating the logs at /var/log/auth.log for failed authentication

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

#### ➡️ Checking Event Viewer on ```windows-vm```:

In Windows Event Viewer, there are normally a lot of logs, in the screenshots below the logs are filtered by the specific events we're looking for.

Windows Logs > Security, filtered by Event ID: **[4625](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625)**

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

Windows Logs > Application, filtered by Event ID: **[18456](https://learn.microsoft.com/en-us/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-ver16)**

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)


<br>

<br>

<br>

<br>

<br>
  
 
 

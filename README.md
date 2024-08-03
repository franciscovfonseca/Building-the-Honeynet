<br>

<h1 align="center">Stage I - Building the Honeynet</h1>

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

- 2 Windows VMs (one used for "Attack")
- 1 Linux VM.

<br>

### ❶ Create the Subscription

<br>

Here is a Visual Overview of the Next Few Steps in this Section of the Lab:

![azure portal](https://github.com/user-attachments/assets/ea9306fc-dea8-45d9-9a96-74da3b332516)

I'm starting with creating the **Subscription**.

⚠️ Don't forget to set a budget!

<br>

![azure portal](https://github.com/user-attachments/assets/38706c96-aadf-42cf-a1dc-1a22dec49c23)

<br>

![azure portal](https://github.com/user-attachments/assets/ba2ff604-b5b4-4614-b327-fe260f354f57)

<br>

![azure portal](https://github.com/user-attachments/assets/ed2adc07-3363-43ce-9349-de46afb84bea)

<br>

>   <details close> 
>   
> **<summary> 💡 Note</summary>**
>   
> Later, I actually changed the alert threshold to something more reasonable ➜ like 90%.
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

<br>

![azure portal](https://github.com/user-attachments/assets/dc3bd602-6820-4be9-b50c-b69d25951e28)

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

<br>

Basics tab:

<br>

![azure portal](https://github.com/user-attachments/assets/e896919e-3110-4514-afca-67ba4c088e2a)

<br>

Create a New Virtual Network:

<br>

![azure portal](https://github.com/user-attachments/assets/b9e1f264-a90c-46e3-bf9e-465722f7dcec)

<br>

Networking tab:

<br>

![azure portal](https://github.com/user-attachments/assets/59ab3b9b-cf27-40de-955b-1ce8dae8e192)

<br>

<h2></h2>

<br>

#### ➡️ Create Linux VM

<br>

Create the **Linux VM** with the same **User**, **Region**, **Resource Group** and **Networking Settings**.

<br>

Basics tab:

<br>

![azure portal](https://github.com/user-attachments/assets/1100ee29-0a64-4416-a7f9-694efa193052)

<br>

Networking tab:

<br>

![azure portal](https://github.com/user-attachments/assets/774c41fa-4cb9-4d88-88e9-ad8b3ac4225e)

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

<br>

![azure portal](https://github.com/user-attachments/assets/34e25bc2-63e1-433b-b72d-c5a75d19ecb0)

<br>

### ❷ Linux NSG Before:

<br>

![azure portal](https://github.com/user-attachments/assets/a1af7722-c438-40e7-a369-27810108be41)

<br>

### ❸ Custom Inbound Rule:

<br>

![azure portal](https://github.com/user-attachments/assets/5513f953-afcc-44fc-9d7e-07ad62007b57)

<br>

### ❹ Windows NSG After:

<br>

![azure portal](https://github.com/user-attachments/assets/abd7566e-4ad9-47c6-9dd0-2397b2866c17)

<br>

✅ At this point both NSGs are identical.

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Create Vulnerabilities</h2> </summary>
<br>
  
### ❶ Disable Windows Firewall

<br>

The goal is to **Expose These VMs to Threat Actors** and to make them both Discoverable and Reachable.

Doing so we can **Monitor, Log & Investigate Incidents** later.

By default **Windows Firewall**, blocks ICMP packets from the internet.

➜ You can see that the ```windows-vm``` is unreachable from another network:

<br>

![azure portal](https://github.com/user-attachments/assets/488bbf82-5741-419a-8416-0294ec8d1fe4)

<br>

So from here ➜ we'll **Disable the Firewall** (wf.msc).

<br>

#### ⏪ Before:

<br>

![azure portal](https://github.com/user-attachments/assets/fb0ebb4f-4300-47c5-829f-488e3acde199)

<br>

#### ⏩ After:

<br>

![azure portal](https://github.com/user-attachments/assets/f8f29dc1-808c-4af8-aeb0-799eab337147)

<br>

Pinging ```windows-vm``` again to test if it successfully Pings now:

<br>

![azure portal](https://github.com/user-attachments/assets/f1269b58-4d9f-484e-b64f-86f154d0dd93)

<br>

Connecting to the ```linux-vm``` via SSH using **PuTTy**:

<br>

![azure portal](https://github.com/user-attachments/assets/95257be6-27a0-4e26-979d-4a0166978be7)

![azure portal](https://github.com/user-attachments/assets/7f969ff1-6d7b-43c4-bf05-ac3fe37fe703)

<br>

Test pinging ```linux-vm``` externally:

<br>

![azure portal](https://github.com/user-attachments/assets/b298e521-bdbe-474a-8397-f8bdaf6c752d)

<br>

<h2></h2>

<br>

### ❷ Install MS SQL Server + Utilities

<br>

Next, download and install **[SQL Server Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2019)**.

Select **Download Media**.

<br>

![azure portal](https://github.com/user-attachments/assets/9398f365-79d2-48d6-9f4c-c8041a41b014)

<br>

Select **ISO** and the **Download Location**.

<br>

![azure portal](https://github.com/user-attachments/assets/0f95a58f-5d5b-4464-b96b-1ff5756903ce)

<br>

Once the download completes ➜ go to the Download Location.

<br>

![azure portal](https://github.com/user-attachments/assets/6bcb5b94-7dd3-4a85-adef-2b0f7ea25d4d)

<br>

**Mount** the ISO:

<br>

![azure portal](https://github.com/user-attachments/assets/7d1d06dd-5754-4aa7-9e82-4dbc9f786877)

<br>

Run the installer:

<br>

![azure portal](https://github.com/user-attachments/assets/49419b03-5dc7-4e57-8361-9ee15b06c7d4)

<br>

Select **New SQL Server**:

<br>

![azure portal](https://github.com/user-attachments/assets/d99be1aa-f0bf-4e9b-83ac-fc2bb51ad223)

<br>

Include ☑️ **Database Engine Service**:

<br>

![azure portal](https://github.com/user-attachments/assets/382a927b-c164-424d-81a8-e470cf0f2572)

<br>

Select ⦿ Mixed Mode ➜ create a user and select **Add Current User** to also allow for Windows authentication using ```labuser```.

<br>

![azure portal](https://github.com/user-attachments/assets/5654185c-163b-4295-a2d9-a1fe437de2b5)

<br>

Once the installation completes, download and install **[MS SQL Management Studio (SMSS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)**.

<br>

![azure portal](https://github.com/user-attachments/assets/9981e9ac-4d9d-4186-bb2d-1e76d3570f45)

![azure portal](https://github.com/user-attachments/assets/c974420c-2b62-4bb4-858a-fe2d4a1b918f)

<br>

Once the installation completes ➜ Restart the VM.

<br>

<h2></h2>

<br>

#### ➡️ Enable Logging on SQL Server

<br>

After restarting, follow the **[Microsoft documentation](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16)** for adjusting settings to allow **SQL Server Logs** to be ported to **Windows Event Viewer**.

Provide full permission for the SQL Server service account **```NETWORK SERVICE```** to the registry hive.

<br>

![azure portal](https://github.com/user-attachments/assets/6002ba78-f307-4288-b152-0533bb24790c)

![azure portal](https://github.com/user-attachments/assets/c027d521-5540-497d-8ab8-18f05b9468a7)

<br>

Configure the audit object access setting in Windows using ```auditpol``` by executing the provided command line statement:

<br>

```commandline
auditpol /set /subcategory:"application generated" /success:enable /failure:enable
```

<br>

![azure portal](https://github.com/user-attachments/assets/c8be1cc2-0a60-4beb-ad55-91d91142232b)

<br>

**Launch SSMS** and log in to the **SQL Server**.

Then go to Properties ➜ Security ➜ Enable both

<br>

<h2></h2>

<br>

#### ⏪ Security Settings Before:

<br>

![azure portal](https://github.com/user-attachments/assets/63a62717-0df2-4749-a446-f4f10ce3faea)

<br>

#### ⏩ Security Settings After:

<br>

![azure portal](https://github.com/user-attachments/assets/2aa88224-0886-4024-91c1-2e1d59f09542)

<br>

Restart the SQL Server and try to **Generate some Failed Authentication Logs** ➜ by trying log into the SQL server with the Wrong Password.

<br>

![azure portal](https://github.com/user-attachments/assets/9f77c883-a52f-4428-9115-708ad6d26a8b)

<br>

Check **Event Viewer** to make sure the Logs are properly enabled and porting to Event Viewer successfully ✅

<br>

![azure portal](https://github.com/user-attachments/assets/2af91e5a-149c-4788-8e9c-76a2b50a0e42)

<br>

<h2></h2>

<br>

#### ➡️ Create Attack (Threat) VM

<br>

Create another Windows VM named ```attack-vm``` in a different Resource Group, Region, and Virtual Network.

All other settings can be the same or similar.

<br>

![azure portal](https://github.com/user-attachments/assets/af55023d-799a-4632-a7d1-037e05592337)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Generate Logs</h2> </summary>
<br>

To make sure everything is working as expected, log into the ```attack-vm``` to Generate Failed Authentication Logs on both VMs.

<br>

![azure portal](https://github.com/user-attachments/assets/f14a74b2-9daa-4a59-9d26-5814f1e40ddd)

<br>

**Generating Failed RDP Logs** on ```windows-vm```

<br>

![azure portal](https://github.com/user-attachments/assets/b9c76e73-e537-490c-8d35-efa1a4e81810)

<br>

Using **PowerShell** to **Generate Failed Login Logs** on ```linux-vm```

<br>

![azure portal](https://github.com/user-attachments/assets/c6ee3ce8-b3ed-4136-b168-7f95c8047556)

<br>

**Installing SMSS** on ```attack-vm```

<br>

![azure portal](https://github.com/user-attachments/assets/77025612-fd99-45c6-b9b8-c0ae2eb1470e)

<br>

**Generating Failed Login Logs** for **MS SQL Server** on ```windows-vm```

<br>

![azure portal](https://github.com/user-attachments/assets/a33f9ecf-9c5f-4522-b3ec-ef42a949757c)

![azure portal](https://github.com/user-attachments/assets/810e3537-a5d2-46a5-ac2c-bf3e775c8710)

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>5️⃣ Check Logs</h2> </summary>
<br>

Using **PowerShell** to **SSH into ```linux-vm```

<br>

![azure portal](https://github.com/user-attachments/assets/ef65d273-4e8f-4cb9-88a0-0d1848f2b256)

<br>

**Investigating the Logs** at ```/var/log/auth.log``` for Failed Authentication:

<br>

![azure portal](https://github.com/user-attachments/assets/27fe20a5-5604-4235-9897-f06d3c753eee)

<br>

<h2></h2>

<br>

#### ➡️ Checking Event Viewer on ```windows-vm```

<br>

In **Windows Event Viewer**, there are normally a lot of Logs

In the screenshots below the Logs are filtered by the specific events we're looking for.

- Windows Logs ➜ Security ➜ filtered by Event ID: **[4625](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625)**

<br>

![azure portal](https://github.com/user-attachments/assets/55c932fb-7ff0-4505-9378-522c51b2df44)

<br>

- Windows Logs ➜ Application ➜ filtered by Event ID: **[18456](https://learn.microsoft.com/en-us/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-ver16)**

<br>

![azure portal](https://github.com/user-attachments/assets/0c801721-2989-4d7d-90c2-c0ccd53d0fb7)

<br>

  </details>

<h2></h2>

<br>

<br>

<br>

<br>

<br>
  
<br>


 

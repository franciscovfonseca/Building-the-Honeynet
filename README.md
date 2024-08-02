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

In this section of the project ➜ we'll set up 2 Target Virtual Machines and 1 Threat Virtual Machine.

2 Windows VMs (one used for "Attack") and 1 Linux VM.

<br>

### ① Create the Subscription

<br>

Here is an overview for the next few steps in this section:

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

I'm starting with creating the Subscription.

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

### Create the First Resource Group

<br>

Next, create the first resource group ```RG-Cyber-Lab```.

This one will house the resources that will be exposed to attack ➜ the **Honeynet**.

Technically, this can be created at the same time as you create your VMs.

![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br>

<h2></h2>

<br>

### Create Virtual Machines

<br>

- Next we'll create 2 Virtual Machines ➜ 1 Windows VM & 1 Linux VM ➜ using mostly Default Settings.

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

Create the Linux VM with the same user, region, resource group and networking settings.

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


















Once you're in the **Azure Portal**, navigate to the "***Virtual machines***" section. 
  
![azure portal](https://github.com/user-attachments/assets/59c10e90-33da-4cf3-a40c-802f76edf858)

<br />
  
Click on "***Create***", then "***Azure virtual machine***" ➜ This is where we'll set up our new VM.
  
 
![VM create](https://github.com/user-attachments/assets/5a8e0b37-a640-4c61-8d54-df38b55984c3)
  
  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Configure the Windows 10 VM's Settings</h2> </summary>
<br>
  
### Subscription & Resource Group:

- We'll select our **Azure subscription** and **Resource group** ➜ Which is a way to group and manage resources in Azure.
- For the purpose of the project, I already created a **Resource group** called ```RG-Cyber-Lab2``` 

  
### Virtual Machine Name:

- In this case, I have named this **VM** ```windows-vm```

### Region:

-  As for the **Region** , you can choose ```(US) East US 2```
  
### Availability Options:

- Being that the only purpose of this **Virtual Machine** will be to act as a **Honeypot**, we do not require any form of availability, so I selected ```No infrastructure redundancy required```

### Image:

- Select ```Windows 10 Pro, version 21H2 - x64 Gen2```
  
  ![VM create](https://github.com/user-attachments/assets/96de8fa2-6537-4177-8c0d-16c9d9104679)

### Size:

- Choose a VM with at least 2 vCPUs that's not too expensive: ```Standard_E2bs_v5 - 2 vcpus, 16 GiB memory```

### Administrator Account:

- Username: ```labuser```
- Password: ```Cyberlab123!```

### Licensisng:

- Make sure you check the ☑️ box

  ![VM create](https://github.com/user-attachments/assets/9d117630-6161-4f56-a430-55f2a8150db3)

### Networking Tab:

- When creating the **Virtual network**, we will be leaving it to the **default** settings.
- For the purpose of this lab, I called mine ```Lab-VNet```.
  
  ![netowkr](https://github.com/franciscovfonseca/Setting-Up-Vulnerable-VMs-in-Azure/assets/172988970/d1fae176-6879-4199-b0be-f56e8d2d437b)

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Create the Linux Virtual Machine</h2> </summary>
<br>
  
- Same **Region**, **Resource Group**, and **VNet** as the ```windows-vm``` Virtual Machine

- I named this second **VM** ```linux-vm```

- For the **Image** I selected the ```Ubuntu Server 20.04 LTS```

  ![VM create](https://github.com/user-attachments/assets/fd16cae4-cdfd-45c8-b0a3-d94a04c9677d)

- I used the same **Size**, **Admin Username and Password** as the ones used for the Windows VM

- For **Virtual Network**, I made sure I selected the same VNet ```Lab-VNet``` that I had created while setting up the Windows VM.


<br>

  </details>

<h2></h2>
<details close> 
<summary> <h2>5️⃣ NSG & Inbound Security Rule Configuration</h2> </summary>
<br>

> In this step I opened up **Network Security Groups (NSGs)** for both VMs:
> 
> - Configure the NSGs (Layer 4 Firewalls) to allow **all inbound traffic**.

<br>

### Navigate to the Network Security Group (NSG):

- In the Azure portal, search for "**Network Security Groups**" in the search bar at the top.
- Once there, select the NSG associated with your virtual machine.

 ![NSG](https://github.com/user-attachments/assets/f65a25f9-c6a7-461e-8542-3a5a3c84ca38)
  
### Create an inbound security rule:

- Inside the NSG, you'll find a section for "**Inbound security rules**".
- This is where we control what kind of traffic is allowed to reach our VM.
- Click on "***Add***" to create a "**new rule**".

 ![NSG](https://github.com/user-attachments/assets/13424b7c-9402-4ed2-bc14-51e3de7ba08c)

### Configure the rule:

- We'll be prompted to input some details about our new rule.
  
### Source:

- This defines where the incoming traffic is coming from.
  
  - We can set this to ```Any``` to allow traffic from any location.
  
### Source port ranges:

- This specifies the ports on the source (the computer initiating the connection) that are allowed.

  - Again, we can set this to ```*``` or ```Any``` to allow all ports.

### Destination:

- This defines where the traffic is going to.

  - Since we want the traffic to reach our VM, we can set this to ```Any```.
  
### Destination port ranges:

- This specifies the ports on our VM that are allowed to receive traffic.

  - We can set this to ```*``` or ```Any``` to open all ports.
  
### Priority:

- Setting priorities in Network Security Groups (NSGs) is an essential step.
- The priority determines the order in which rules are applied.
- Rules with lower priority numbers are processed before rules with higher priority numbers because the lower the number, the higher the priority.

  - For the purpose of this lab, I set the priority to ```300``` to ensure that this honeypot functions as intended!

### Action:

- We'll set this to ```Allow```, which means that traffic matching this rule will be allowed to reach our VM. 
  
 ![NSG](https://github.com/franciscovfonseca/Setting-Up-Vulnerable-VMs-in-Azure/assets/172988970/0bf17e13-1f81-42ac-8692-dbf3bbbdf893)

  
### Review & Create:

- After configuring all the details we need for this inbound rule, click "***Add***" to **Create the Rule**.

<br>

➡️ I then followed the exact same process for the ```linux-vm``` Virtual Machine

<br>

  </details>

<h2></h2>

<br>

# Conclusion

<br>

By **Creating our VMs** and **Open Inbound Security Rules**, we're essentially leaving the front door of our VMs wide open.

This is generally not something you'd do in a real production environment, as it would make your system extremely vulnerable to attacks.

However, in the context of our honeynet, it's exactly what we want to do!

This allows us to **Attract Potential Attackers** and **Observe their Actions in a Controlled Environment**.

  
<br />
<br />
  
 
 

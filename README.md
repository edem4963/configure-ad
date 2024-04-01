<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users with powershell and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<h4>Setup Resources in Azure</h4>

- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
- Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static

![ad1](https://github.com/edem4963/configure-ad/assets/112492837/8ce9d910-1c05-4038-bf0e-79fa74b41586)

- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet 
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher


<h4>Install Active Directory</h4>

- Login to DC-1 and install Active Directory Domain Services

  ![ad2](https://github.com/edem4963/configure-ad/assets/112492837/456ffc68-6db0-421b-81ea-d4b2dd6df376)

- Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

![ad3](https://github.com/edem4963/configure-ad/assets/112492837/a6093c3c-3ef5-4944-a8e8-071e1269f15b)

![ad4](https://github.com/edem4963/configure-ad/assets/112492837/3bb5516e-547d-44c7-ac8c-4f28318ff88f)


- Restart and then log back into DC-1 as user: mydomain.com\labuser

<h4>Create an Admin and Normal User Account in AD</h4>

- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

  ![ad5](https://github.com/edem4963/configure-ad/assets/112492837/8cdc573d-ddcc-4e1e-b8fd-ee66b9afae2b)

- Create a new OU named “_ADMINS”

  ![ad5](https://github.com/edem4963/configure-ad/assets/112492837/a0a7934d-e695-4333-96d2-db0310330b95)

- Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

  ![ad6](https://github.com/edem4963/configure-ad/assets/112492837/10e5c949-f522-450f-ad39-9243cb9d34d8)

- Add jane_admin to the “Domain Admins” Security Group

  ![ad7](https://github.com/edem4963/configure-ad/assets/112492837/f487f150-5a5c-4f65-8c3e-9f189ec82b53)

- Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
- User jane_admin as your admin account from now on

<h4>Join Client-1 to your domain (mydomain.com)</h4>

- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

 ![ad8](https://github.com/edem4963/configure-ad/assets/112492837/1d452cf5-f0f2-4f95-847d-fa1304a44bf1)

 ![ad9](https://github.com/edem4963/configure-ad/assets/112492837/965e022f-9ead-4658-8c89-a60ffb91d579)


- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

  ![ad10](https://github.com/edem4963/configure-ad/assets/112492837/290eb804-fba8-423a-81e6-018f246b1e6e)

- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

<h4>Setup Remote Desktop for non-administrative users on Client-1</h4>

- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop

  ![ad11](https://github.com/edem4963/configure-ad/assets/112492837/bf06b42e-0587-43e9-b5f0-b5e37963dac7)

- You can now log into Client-1 as a normal, non-administrative user now

<h4>Create a bunch of additional users and attempt to log into client-1 with one of the users</h4>

- Login to DC-1 as jane_admin
- Open PowerShell_ise as an administrator
- Create a new File and copy the grey highlighed powershell script into it

  ![ad12](https://github.com/edem4963/configure-ad/assets/112492837/1cbe8c93-36d0-4968-b69e-f5a0db561cca)

- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
  attempt to log into Client-1 with one of the accounts (take note of the password in the script)

<br />

<h2>Congrats!! We installed Active Directory and used a Powershell script to automate account creation.</h2>

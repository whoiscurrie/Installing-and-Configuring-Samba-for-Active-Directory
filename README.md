# Installing and Configuring Samba for Active Directory

## Overview
This project involves the installation and configuration of Samba as an Active Directory Domain Controller on an Ubuntu server. The steps cover everything from installing required packages to setting up a domain, creating Organizational Units (OUs), and managing users. 

## Steps

### 1. Installing Samba
Run the following command to install the necessary packages for Samba:
```bash
sudo apt-get install acl attr samba samba-dsdb-modules samba-vfs-modules winbind libpam-winbind libnss-winbind krb5-config krb5-user dnsutils smbclient net-tools
```


### 2. Configure Kerberos
```bash
Default Kerberos version 5 realm: NOSANET.COM
Kerberos servers for realm: dc1.nosanet.com
Administrative server for realm: dc1.nosanet.com
```


### 3. Backup the Original smb.conf File
Before making any changes, back up the original Samba configuration file:
```bash
mv /etc/samba/smb.conf /etc/samba/smb.conf.original
```


### 4. Run Samba Tool Provisioning
Provision the Samba domain using the following command:
```bash
samba-tool domain provision --use-rfc2307 --interactive
```
Configure the following settings during the interactive setup:

    Realm: NOSANET.COM

    Domain: NOSANET

    Server Role: dc

    DNS Backend: SAMBA_INTERNAL

    DNS Forwarder IP Address: 8.8.8.8


### 5. Copy the Generated Kerberos File
```bash
cp /var/lib/samba/private/krb5.conf /etc/
```


### 6. Configure the Name Resolver
Edit the /etc/resolv.conf file:
```bash
nano /etc/resolv.conf
```
Add the following lines:

Nameserver 10.0.2.10
Search nosanet.com


### 7. Disable Unnecessary Services
Disable services that are not required:
```bash
systemctl disable --now smbd nmbd winbind systemd-resolved.service
```


### 8. Unmask Samba Active Directory Service
Unmask the Samba Active Directory service to enable it:
```bash
systemctl unmask samba-ad-dc.service
```


### 9. Enable Samba Active Directory Service
Enable and start the Samba Active Directory service:
```bash
systemctl enable --now samba-ad-dc.service
```


### 10. Verify Samba Listening for Connections
Check if Samba is listening for connections:
```bash
netstat -antp | egrep 'smbd|samba'
```


### 11. Start Samba Active Directory
Start the Samba Active Directory service:
```bash
samba
```


### 12. Ping Domain Name
Ping the domain name to verify connectivity:
```ping dc1.nosanet.com```


### 13. Ping Domain Name from Windows Host
On the Windows host, ping the domain name to verify connectivity:
```ping dc1.nosanet.com```


### 14. Connect Windows Host to the Domain
#### Step 1: Enter Administrator Credentials
Provide the administrator credentials when prompted during the domain join process.


#### Step 2: Download RSAT on Windows Host
Install the Remote Server Administration Tools (RSAT) on your Windows host.


#### Step 3: Setting Up MMC
Press Ctrl + M to add the following snap-ins in MMC:
    Active Directory Users and Computers

    Group Policy Management

    DNS


#### Step 4: Save the Console
Save the console to your desktop for easy access.


### 15. Create Organizational Units (OUs) and Users

Create different pseudo-departments using Organizational Units (OUs). Within each department, create 2 users.
# Example command to create an OU and users:
samba-tool ou create "OU=Sales,DC=nosanet,DC=com"
samba-tool user create user1 Sales
samba-tool user create user2 Sales


### Conclusion
This setup enables you to configure Samba as an Active Directory Domain Controller and manage users and organizational units within the domain. You can now connect clients and users to the domain, manage group policies, and implement a Windows-like domain environment on Linux.



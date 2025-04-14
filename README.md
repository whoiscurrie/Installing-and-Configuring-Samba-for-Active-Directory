# Installing and Configuring Samba for Active Directory

## Overview
This project involves the installation and configuration of Samba as an Active Directory Domain Controller on an Ubuntu server. The steps cover everything from installing required packages to setting up a domain, creating Organizational Units (OUs), and managing users. 

## Steps

### 1. Installing Samba
Run the following command to install the necessary packages for Samba:
```bash
sudo apt-get install acl attr samba samba-dsdb-modules samba-vfs-modules winbind libpam-winbind libnss-winbind krb5-config krb5-user dnsutils smbclient net-tools

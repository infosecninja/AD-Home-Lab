# AD-HOME-LAB

## Introduction

Active Directory, a powerful directory service developed by Microsoft, centralizes user management, enhances security, and simplifies administrative tasks within a network environment. This project offers a valuable opportunity to establish a strong foundation and gain hands-on experience by creating a home lab with Active Directory.


## Objective
```
This project guides users, from beginners to advanced learners, in setting up a basic home lab with Active Directory. By creating a fully functional Active Directory environment, users can learn, experiment, and simulate real-world scenarios.
```

## Lab Setup
```
Using VMWare Workstation 15 Player, set up the following virtual machines:

1 x Windows Server 2019 (Domain controller)
1 x Windows 10 Enterprise — User-machine 1
1 x Windows 10 Enterprise — User-machine 2
1 x Kali Linux — Attacker

Each virtual machine should have approximately 2GB of RAM, totaling 8GB RAM for all machines.

Download the required ISO from the official Microsoft Evaluation Center website: https://www.microsoft.com/en-us/evalcenter/
```
## Virtual Machine Setup

```
Remove floppy drives and set the network to NAT. Perform the following steps for each virtual machine:
```

## Domain Controller (OS Installation)

```
1. Create a new virtual machine.
2. Select the ISO file for the installer disc image.
3. Choose “Windows Server 2019” as the version to install.
4. Power on the virtual machine after creation.
5. In VM Settings, remove the floppy disk and set the network to NAT.
6. Start the virtual machine and press any key to enter setup mode.
7. Select “Windows Server 2019 Standard Eval (Desktop Experience)”.
8. Choose “Custom install” and select the unallocated space for installation.
9. Follow the on-screen instructions to complete the installation.
```
## Setting Up Domain Controller

```
1. In the virtual machine settings, go to “View Your PC Name” and rename the PC to something like myDomainController. Restart the virtual machine.
Set a password for the PC, such as P@$$w0rd.
2. In VMWare Player, go to “Manage” and install VMWare Tools.
3. Open Server Manager and navigate to “Dashboard” > “Manage” > “Add Roles and Features”.
4. Choose “Role-based or feature-based installation” and select the Active Directory Domain Services role. Continue with the installation.
5. After the installation, click the flag icon in Server Manager and select “Promote this server to a domain controller”.
6. Choose “Add a new forest” and enter a root domain name like ADHACKING.local. Proceed with the installation, setting a password for DSRM (Directory Services Restore Mode) and accepting the default options.
```
## User Machine (OS Installtion)

```
Perform the following steps for each user machine:

1. Create a new virtual machine.
2. Select the ISO file for the installer disc image.
3. Choose “Windows 10 Enterprise” as the version to install.
4. Power on the virtual machine after creation.
5. In VM Settings, remove the floppy disk and set the network to NAT.
6. Start the virtual machine and press any key to enter setup mode.
7. Select “Custom install” and select the unallocated space for installation.
8. Choose “Domain join instead” and enter an account name (e.g., saul goodman) and password (Password1).
9. Proceed with the installation, selecting “No” for “Do more across devices…” and declining “Get help from digital assistant”. Choose your preferred privacy settings.
```

## Setting Up user machine

```
In VMWare Player, go to “Manage” and install VMWare Tools.
In the virtual machine settings, go to “View Your PC Name” and rename the PC (e.g., Saul-PC). Restart the virtual machine.
Repeat these steps for the second user machine, using the account name walter white, password Password1, and PC name Walter-PC.
```

## Active Directory (AD) Setup, Groups, and Policies

```
Log in to the Windows Server (Domain Controller) and perform the following steps:

1. Open Server Manager and go to “Tools” > “Active Directory Users and Computers”.
2. Right-click on the root domain (e.g., ADHACKING.local) and select "New" > "Organizational Unit" > "Groups".
3. Move all users (except Administrator and Guest) from the “Users” directory to the “Groups” directory.
4. Right-click under the “Users” directory and create the following users:
5. Uncheck “User must change password at next logon” and check “Password never expires”.
6. Copy the “administrator” domain admin account with the login gus and password Password2020@!.
7. Copy the “administrator” domain admin account with the logon SQLService, password MYpassword123#, and description "Password is MYpassword123#".
8. Create a new user domain account with the logon saul and password Password123.
9. Create a new user domain account with the logon walter and password Password123.
10. Note: It is generally not recommended to assign domain-admin rights to service accounts like SQL Service. Although this is a lab example, in real-life situations, granting domain-admin rights to such services should be avoided. Similarly, storing passwords in the description field is not a secure practice.
```

## Configuring File Server (Opening SMB)

```
Enable SMB file sharing and open ports 445 and 139 for later lab activities.

1. Open Server Manager and go to “Dashboard” > “File and Storage Services” > “Shares”.
2. Click “New Share” > “SMB Share — Quick” > “Next”.
3. Set the share name as “hackme” and follow
```

## Service Principal Name (SPN) Setup

```
To prepare for a Kerberoasting attack:

1. Open the Command Prompt as an administrator.
2. Use the following command to set the SPN: `setspn -a myDomainController/SQLService.ADHACKING.local:60111 ADHACKING\\\\SQLService`.
3. Check if the SPN is set using the command: `setspn -T ADHACKING.local -Q */*`.
```

## Group Policy Configuration

```
For lab purposes (note: topics like AV bypass and evasion are not covered in this lab):

1. Open Group Policy Management as an administrator.
2. Expand the Forest > Domains > ADHACKING.local.
3. Right-click and create a new Group Policy Object (GPO) named “Disable Windows Defender” in this domain.
4. Edit the newly created GPO.
5. Navigate to “Computer Configuration” > “Policies” > “Administrative Templates” > “Windows Components” > “Windows Defender Antivirus”.
6. Double-click on “Turn off Windows Defender Antivirus” and enable the policy.
```

## Connecting all Machines

```
Perform the following steps on both machines:

1. Create a folder and set up a network share.
2. On the first machine, give the first domain user local administrator rights.
3. On the second machine, give both domain users local administrator rights.
4. Create a network share folder.
5. Create a folder named “share” in the C: drive.
6. Right-click on the folder, go to “Properties” > “Sharing” > “Share” > “Share” > “Yes, turn on network discovery…”.
7. Configure network settings.
8. Open network & internet settings, change adapter options, and go to IPv4 > Preferred DNS Server.
9. Enter the IP address of the Domain Controller.
10. Join the domain.
11. Go to “Access work or school” > “Connect” > “Join this device to local Active Directory domain”.
12. Enter the domain name as ADHACKING.local.
13. Join the domain as the Administrator with the password P@$$w0rd.
14. When prompted to add an account, skip and restart the machine.
15. Upon restart, select the other user and log in as the domain user (e.g., saul with the password Password123).
16. Give local administrator rights:
    - Go to “Computer Management” > “Local Users and Groups” > “Groups”.
    - Double-click on “Administrators”.
    - Add the user (e.g., saul), check the name, apply, and click OK.
```

## Turn on Network Discovery

```
1. Go to “Computer” > “Network”.
2. Click OK when prompted and click “Turn on network discovery…” in the top menu bar.
```

## Verify that both computers have joined the domain:

```
On the Domain Controller, open “Active Directory Users and Computers” > ADHACKING.local > "Computers".
Check if both computers are added.
```

## Set up for mitm6 attack lab example:

```
1. Login to the Domain Controller.
2. Open Server Manager > Dashboard > “Add roles and features” > Next.
3. Select “Active Directory Certificate Services” > Add features > Next.
4. Choose to restart the destination server automatically and click Install.
5. Click the flag notification icon and configure Active Directory services on the destination server.
6. Provide the credentials as ADHACKING\\Administrator.
7. Proceed with the configuration, selecting the Certification Authority and setting the validity to 99 years. Click Next and configure the settings.
```
## Conclusion

Building a home lab with Active Directory provides a unique opportunity to gain practical experience, expand knowledge, and develop skills in this essential technology. By setting up your Active Directory lab environment, you’ll have a platform to expand your knowledge, sharpen your skills, and unlock the potential 
of this powerful technology. This project offers step-by-step guidance for beginners and valuable insights for those with advanced knowledge. Understanding 
Active Directory and its features is crucial for IT professionals in network administration, security, and system management roles. Invest in your professional 
growth by acquiring skills in Active Directory to open doors to exciting opportunities in the IT field and advance your career.

```


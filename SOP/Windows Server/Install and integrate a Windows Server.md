---
creation date: 2025-07-31 17:29
modification date: 2025-07-31 17:29
version: "1"
---
## Purpose
To provide a standardize procedure to install a Window Server on the network, configure a base for Active Directory, and its redundancy in case of failure.
## Scope
The SOP applies to network engineers and server administrators to install a Window Server configured with Active Directory and redundancy and within the target environment.
## Procedures
#### 1. Pre-requisite
- Confirm topology diagram and IP address plan is current.
- Identify placement of the router.
- Determine hostnames, domain name, and effective IP ranges.
#### 2. Deployment
- Physically or virtually install the machines.
- Connect the interfaces.
#### 3. Install the Operating System
1. Language, Time and Currency, Keyboard or input method should be the 'US' default; click 'Next'.
2. Click 'Install Now'.
3. Select 'Windows Server Standard (Desktop Experience)' with the 64x Architecture as the operating system to install; Click 'Next'.
4. Click 'Custom' as the type of installation; click 'Next'.
5. Click 'Drive 0' which should have unallocated space; click 'Next'.
6. Repeat all of the above of for the redundant/failover server.
7. Set Administrator password; click 'Finish'.
#### 4. Configure network connection
1. Once the machine restarts, login as Administrator.
2. Open 'Server Manager'.
3. Click 'Local Server' in the left menu panel.
4. Click the Ethernet connection 'Not connected' link to open 'Network Connections'.
5. Right-click the Ethernet connection and open 'Properties'.
6. In the list of items, click 'Internet Protocol Version 4 (TCP/IPv4)', and click 'Properties' below to open.
7. In the General tab, click 'Use the following IP address' to manually configure the IP address, subnet mask, default gateway, preferred and alternate DNS server as specified, and click 'Ok' to set.
8. Ethernet connection status should update. Verify connection to the default gateway using 'ping'.
#### 5. Set computers' hostname
1. Open 'Server Manager'.
2. Click 'Local Server' in the left menu panel.
3. Click on the Computer name link to open 'System Properties'.
4. On the Computer Name tab, click 'Change...' to change the hostname.
	- If this machine is not the Primary server, and the Primary server is already serving as a domain controller, change the membership to be a Member of Domain, and enter the domain name. Login as Administrator of the domain, to join the domain.
5. Restart the machine.
#### 6. Install Active Directory Domain Services
1. Open 'Server Manager'.
2. Click 'Manage' at the top-left in the toolbar, click 'Add Roles and Features'.
3. In the wizard, at Before you Begin, click 'Next'.
4. In Installation Type, select 'Role-based or feature-based installation' and click 'Next'.
5. At Server Selection, select the machine in the server pool and click 'Next'.
6. At Server Roles, select 'Active Directory Domain Services'. When prompted, click 'Add Features' then click 'Next' in the wizard.
7. At Features, click 'Next'.
8. At AD DS, click 'Next'.
9. At Confirmation, click 'Install'.
#### 7. Active Directory Domain Services Deployment
1. Once successfully install, open 'notifications' (should be with the warning icon).
2. In the notification 'Post-deployment Configuration', click the link 'Promote this server to a domain controller'.
3. On the Deployment Configuration wizard, if, the machine is:
	1. the designated Primary server:
		1. Click 'Add a new forest', and enter the Root domain name. Click 'Next'.
		2. At the Domain Controller Options, 
			1. Forest and Domain functional level should default to Windows Server 2016.
			2. Check 'Domain Name System (DNS) server' if not already checked.
			3. Enter the Directory Services Restore Mode (DSRM) password, confirm, and click 'Next.
	2. not the designated Primary server
		1. Click 'Add a domain controller to an existing domain'. Update the domain name and supply the domain Administrator's credential by clicking 'Change...'. Click 'Next'.
		2. At the Domain Controller Options, 
			1. The default 'Domain Name System (DNS) server' and 'Global Catalog (GC)' should be selected.
			2. Specify the site name.
			3. Enter the Directory Services Restore Mode (DSRM) password, confirm, and click 'Next.
4. At DNS Options, click 'Next'.
5. At Additional Options, if, the machine is:
	1. the designated Primary server:
		- confirm NetBIOS domain name, and click 'Next'.
	2. not the designated Primary server
		- Specify additional replication options. Choose to replicate from the primary domain controller or any domain controller as specified and click 'Next'
6. At Paths, specify the directory paths for the AD DS Database, Log files, and SYSVOL folders, and click 'Next'.
7. At Review Options, confirm the selections. Click 'Next'.
8. A the Prerequisite Check, ensure that the check passes and  that there are no errors. Proceed with 'Install'. Expect the machine to restart.
#### 8. Verification
1. Verify that each server can resolve each other's domain name with `nslookup`.
2. Verify that each server has connectivity to one another with `ping`.
3. Verify the replication settings for each server in 'Active Directory Sites and Services'.
	1. Under the Site Object, you will have a folder of Servers.
	2. In each server, there is a NTDS Settings Object.
	3. Double-click the object and verify the settings.
## Notes and references
1. [Install Active Directory Domain Services](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-)
2. [Install a Replica Windows Server 2012 Domain Controller in an Existing Domain (Level 200)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain--level-200-)
3. Reference 3

## Revision History
- Version 1: Created.
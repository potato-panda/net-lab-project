---
creation date: 2025-08-01 16:03
modification date: 2025-08-01 16:03
version: "1"
---
## Purpose
To provide a standardize procedure to install and configure a DHCP server and failover to allow automatic IP addressing to new guest machines added to the network.
## Scope
The SOP applies to server administrators to install and configure a DHCP server with specified DHCP scopes and other constraints within the target environment.
## Procedures
#### 1. Install DHCP server
1. Open 'Server Manager'.
2. Click 'Manage' on the menu bar and select 'Add Roles and Features'.
3. At Before you begin, click 'Next'.
4. At the Select installation type, select 'Role-based or feature-based installation', and click 'Next'.
5. At the Selection destination server, select the server in the server pool, and click 'Next'.
6. At the Select server roles, check 'DHCP Server' and click 'Next'.
	- When prompted, check the requirements and click 'Add Features' to proceed.
7. At Features, there is no required action, simply click 'Next'.
8. At confirmation, review the selections and click 'Install' to finish.
9. Expect the machine to reboot.
#### 2. Creating DHCP Scopes
1. Open 'Server Manager'.
2. Click 'Tools' on the menu bar and select 'DHCP Manager'.
3. In the left Panel, under DHCP, expand the tree to IPv4 and IPv6.
4. Right-click a protocol and select 'New Scope...' to open the wizard.
5. In the Wizard, click 'Next'.
6. Provide a Scope Name and description, then click 'Next'.
7. Enter the starting and ending IP addresses for the scope and then the length or subnet mask. Click 'Next' when finished.
8. Enter starting and ending IP addresses for a range to be excluded from the scope. Optionally, enter a subnet delay. Click 'Next' when finished.
9. Enter a Lease Duration then click 'Next'.
10. Optionally, configure DHCP options like default gateways, DNS servers and WINS settings for the scope in the following pages, otherwise click 'Next'.
11. Choose whether to active the scope now. Click 'Next'.
12. Click 'Finish' to confirm the settings and close the wizard.
13. The DHCP Scope object should appear under the respective protocol folder.
#### 3. DHCP Failover
1. Right-click the DHCP Scope object and select 'Configure Failover...'.
2. Select all available scopes to configure a failover for and click 'Next'.
3. Specify an IP address or hostname of the partner server as a failover. Click 'Next'.
4. Select a relationship name to use. Click 'Next'.
5. Confirm the selections. Click 'Finish'.
6. Verify the failover configuration in the prompt window.
7. Click 'Close' to finish.
#### Procedure 3 \<Name of Procedure\>
*Write a brief process description, then insert your Scribe in the placeholder below.*
## Notes and references
1. [Quickstart: Install and configure DHCP Server](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/quickstart-install-configure-dhcp-server)
2. [Deploy and manage DHCP](https://learn.microsoft.com/en-us/training/modules/deploy-manage-dynamic-host-configuration-protocol)

## Revision History
- Version 1: Created.
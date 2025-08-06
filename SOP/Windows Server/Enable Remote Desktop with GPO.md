---
creation date: 2025-08-05 13:12
modification date: 2025-08-06 14:22:15
version: "2"
---
## Purpose
To provide a standardize procedure to enable Remote Desktop services on computers within the target environment.
## Scope
This SOP applies to server administrators to create and apply GPOs that enable Remote Desktop access in computers groups within the target environments.
## Procedures
#### 1. Pre-requisite
- Installed and configured Certification Authority role.
- An OU containing computers to be accessed through Remote Desktop services.
- A security group containing users or groups permitted to use Remote Desktop services.
#### 2. Create a certificate template to enable secure Remote Desktop connections
1. Open Server Manager, select Tools and select to open Certification Authority.
2. Drill down to Certificate Templates, right-click select Manage to open Certificate Templates Console.
3. Right-click the Computer Template, select Duplicate Template.
4. In the Properties, click the General tab, and type RDP as the Template display name and click OK.
#### 2. Create and configure GPO to enable Remote Desktop access
1. Open Server Manager, select Tools and select to open Group Policy Management.
2. Drill down from forest to the domain, and right-click select the OU to enable Remote Access to, select Create a GPO in this domain, and Link it here...
3. Give the GPO a name and click OK.
4. Expand the OU, right-click the GPO and select Edit....
5. Navigate to Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections, right-click Allow users to connect remote by using Remote Desktop Services and select Edit. In the editor, set to Enabled and click OK.
6. Navigate to Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Security, right-click Server authentication certificate template and select Edit. In the editor, set to Enabled, and in the Options, Certificate Template Name, enter RDP, the name of the certificate template. Click Ok.
7. Returning to the editor, right-click Require use of specific security layer for remote (RDP) connections and select Edit. Set to Enabled and in the Options, set the Security Layer to SSL. Click Ok. 
8. Returning to the editor, navigate to Computer Configuration/Windows settings/Security Settings/Public Key Policies, right-click Certificate Services Client – Auto-Enrollment Properties and select Edit. Set to Enabled and new options below appear. Check Renew expired certificates, update pending certificates, and remove revoked certificates and Update certificates that use certificate templates. Click Ok.
#### 3. Add users or groups to Remote Desktop Users built-in group
1. Returning to the Group Policy Management. Navigate to Computer Configuration/Preferences/Control Panel Settings/Local Users and Groups. Right-click inside the table, and select New/Local Group to open the Properties window.
2. In this Properties window, ensure Action is set to Update, and in the Group name, use the dropdown menu and select Remote Desktop Users (built-in). Under the Members table, click Add... and type in or browse the name of a user or group. Then click OK to close the Local Group Member window. Click OK again to close Properties.
3. Close Group Policy Management Editor.
#### 4. Verify Remote Desktop Access
1. From another machine on the network, open Remote Desktop Connection.
2. In the Computer field, enter the IP address or FQDN and click Connect.
3. Windows Security will prompt for a credential. Enter the credentials of a user belonging to the Remote Desktop Users group and click OK.
4. Verify a secure connection is made and the session is open.
## Notes and references
1. [Enable Remote Desktop on your PC](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/remotepc/remote-desktop-allow-access)
2. [Using SSL/TLS Certificates for Remote Desktop (RDP)](https://woshub.com/securing-rdp-connections-trusted-ssl-tls-certificates/)

## Revision History
- Version 1: Created.
- Version 2: Updated SOP to cover secure connection with SSL/TLS.
	- Creating a new certificate template.
	- Enable Server authentication Certificate Template.
	- Enable Required use of specific security layer for remote (RDP) connections policy.
	- Set to automatically renew an RDP certificate.
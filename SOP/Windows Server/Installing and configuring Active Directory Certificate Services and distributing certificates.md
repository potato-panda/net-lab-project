---
creation date: 2025-08-04 10:45
modification date: 2025-08-04 10:45
version: "1"
---
## Purpose
To provide a standardize procedure to install and configure Active Directory Certificate Services to distribute certificates to server and clients to facilitate secure connections with TLS where applicable.
## Scope
This SOP applies to server administrators to install and configure the Certificate Server within the target environment.
## Procedures
#### 1. Pre-requisite
- Membership in both Enterprise Admins and the root domain's Domain Admins group are  required minimums to complete this procedure.
- Ensure the hostname, static IP address and the domain of the computer are set.
#### 2. Install Active Directory Certificate Services
1. Open 'Server Manager', select Manage, and select Add Roles and Features to open the wizard.
2. At Before You Begin page, click Next.
3. At the Select Installation Types, check that Role-based or feature-based installation is select and then click Next.
4. At Select destination server, check that Select a server from the server pool is selected and that the local computer is selected in the Server Pool. Click Next.
5. At Select Server Roles, in Roles, select Active Directory Certificate Services opening a prompt. Confirm the requisite features and click Add features to proceed. Click Next.
6. At Select features, click Next.
7. In Active Directory Certificate Servers, read the given information and click Next.
8. At Confirm installation selections, click Install.
#### 3. Configure Certificate Services
1. Once the Installation is complete, in Task Details or if the installation windows is open, click Configure Active Directory Certificate Services on the destination server to open the wizard.
2. Confirm the credentials information and if needed, provide the credentials for an account that is a member of the Enterprise Admins group. Click Next.
3. At Role Services, select Certificate Authority and click Next.
4. At Setup Type, ensure that Enterprise CA is selected and click Next.
5. At CA Type, ensure Root CA is selected and click Next.
6. At Private Key, ensure Create a new private key is selected and click Next.
7. Under Cryptography, keep the default settings for CSP, RSA#Microsoft Software Key Storage Provider, and the hash algorithm, SHA, and determine the ideal key character length for the deployment. The recommended setting is 2048. Click Next.
8. Under CA Name, keep the suggested common name for the CA or change to adhere to requirements. Ensure to use a CA name that is compatible with your naming conventions and  purposes as the CA name can not be changed once installed. Click Next.
9. Under Validity Period, specify a validity period for this CA by entering a number and selecting a time value. It is recommended that five years is used. Click Next.
10. At the Certificate Database, specify the folders to serve as the database and log locations. If specifying folders other than the default locations, ensure folders are secure with ACL to prevent unauthorized access. Select Next.
11. At Confirmation, review your settings. Click Configure to proceed and finish. 
#### 4. Create and issue certificate template
1. Open Server Manager, select Tools and select to open Certification Authority.
2. Drill down to Certificate Templates, right-click select Manage to open Certificate Templates Console.
3. Right-click the Computer Template, select Duplicate Template.
4. In the Properties, click the General tab, and type Computer (<domain.name>) as the Template display name and click Apply.
5. Go to the Security tab, under Group or user names, click Domain Computers, and below in the Permissions for Domain Computers, beside Autoenroll, check Allow. Click OK to close.
6. At the Certificate Templates Console, right-click Certificate Templates and select New/Certificate Template to Issue. Select the Computer (<domain.name>) and click OK.
#### 5. Create and configure GPO to enroll and renew certificates
1. Open Server Manager, select Tools and select to open Group Policy Management.
2. In the editor, navigate to Computer Configuration/Windows settings/Security Settings/Public Key Policies, right-click Certificate Services Client – Auto-Enrollment Properties and select Edit. Set to Enabled and new options below appear. Check Renew expired certificates, update pending certificates, and remove revoked certificates and Update certificates that use certificate templates. Click Ok.
#### 6. Verification
- On domain computers, open the Certificate Console and check under Personal a certificate enrolled.
	- Certificate Console can be accessed:
		- using Run and searching for certlm
		- by searching the Start Menu for Manage Computer Certificate.
## Notes and references
1. [Install the Certification Authority on Windows Server](https://learn.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority)
2. [Distribute Certificates to Client Computers by Using Group Policy](https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/distribute-certificates-to-client-computers-by-using-group-policy)
3. [Automatically enroll Client Computer Certificates by using a GPO](https://blog.matrixpost.net/automatically-enroll-client-computer-certificates-by-using-a-gpo/)

## Revision History
- Version 1: Created.
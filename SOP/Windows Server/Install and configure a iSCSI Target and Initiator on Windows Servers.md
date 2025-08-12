---
creation date: 2025-08-12 10:53
modification date: 2025-08-12 10:53
version: "1"
---
## Purpose
To provide a standardize procedure to install a iSCSI Target computer to serve block storage and a iSCSI Initiator to connect and manage the block storage.
## Scope
This SOP applies to server administrators to install and configure the iSCSI participating computers to enable secure, reliable, and optimized communication between iSCSI initiators and targets for storage access.
## Procedures
#### 1. Pre-requisite
- A designated iSCSI Target computer with a static IP address and available disk space or physical disks.
- A designated iSCSI Initiator computer with a static IP address.
- A CHAP user name and password to authenticate the connection.
- Verified connectivity between iSCSI Target and Initiator computers in an isolated subnet.
- iSCSI virtual disk space and redundancy requirements.
#### 2. Install iSCSI Target Server
1. On the designated iSCSI Target computer, open Server Manager, select Manage/Add Roles and Features.
2. On the Before You Begin page, click Next.
3. On the Installation Type page, select Role-based or feature-based installation and click Next.
4. On the Server Selection, select Select a server from the server pool then select the iSCSI Target computer from the Server Pool and click Next.
5. On Server Roles page, in the Roles menu, expand to and select File and Storage Servers/File and iSCSI Services/iSCSI Target Server. Click Next.
6. On Features page, click Next.
7. On Confirmation page, review the changes and click Next.
8. Results page will show the completion of the installation.
#### 3. Configure iSCSI Target Server
1. At the Server Manager, on the left menu panel, click File and Storage Services.
2. On this left menu panel, click iSCSI.
3. Above the iSCSI Virtual Disks table, click on Tasks and select New iSCSI Virtual Disk....
4. On the iSCSI Virtual Disk Location page, select the iSCSI Target server computer, then select a location on the computer where the virtual disk will be located: either a Volume-based location or a custom path. Click Next.
5. On the iSCSI Virtual Disk Name page, provide a Name and optionally a description then click Next.
6. On the iSCSI Virtual Disk Size page, in the Size field input an initial size, then based on capacity requirements select from Fixed size, Dynamically expanding or Differencing. Ensure to read and complete the sub-options if required. Click Next.
7. On the iSCSI Target page, select from an existing iSCSI Target list or select New iSCSI Target.
8. On the Target Name and Access page, provide a Name and optionally a description.
9. On the Access Servers, click Add... to open the Add initiator ID window. In this window, select Enter a value for the selected type; Change the Type using the dropdown box to IP Address, then enter as the value the IP Address of the iSCSI Initiator computer. Click OK. Returning to the wizard window, click Next.
10. On the Enable Authentication page, click Enable CHAP, and enter the provided User name and Password. Click Next.
11. On the Confirmation page, review the settings. Click Create.
12. On the Results page, you will see the progress and completion of the iSCSI Virtual Disk and Target creation.
#### 3. Configure iSCSI Initiator computer
1. Open Server Manager, click Tools, and click iSCSI Initiator to open the iSCSI Initiator Properties window.
2. In the Targets tab, under Quick Connect, enter the IP Address of the iSCSI Target server and click Quick Connect.... You will find the server under Discovered Targets but if CHAP is configured on the Target, you will find it is Unable to Login to the Target in the Progress report. Click Done.
3. Select the iSCSI target in the Discovered targets table and below click Connect.... In this Connect To Target window, click Advanced.... In the Advanced Settings window, click Enable CHAP log on and enter the CHAP user name and password. Click Ok. Click OK again.
4. On the iSCSI Initiator Properties, you should see that the status of the Target server is Connected.
## Notes and references
1. [iSCSI Target Server Overview](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848272(v=ws.11))
2. [How to Configure and Connect an iSCSI Disk on Windows Server](https://woshub.com/configure-iscsi-traget-and-initiator-windows-server/)
## Revision History
- Version 1: Created.
---
creation date: 2025-08-01 16:59
modification date: 2025-08-01 16:59
version: "1"
---
## Purpose
The provide a standardize procedure to quickly set up Log Collectors and Log Sources to facilitate source-initiated security logging.
## Scope
This SOP applies to server administrators to create and apply GPOs to computers groups to enable source-initiated security logging within the target environments.
## Procedures
#### 1. Pre-requisite
- Verify forwarder connectivity to the collector(s).
- Verify collector(s) FQDN can be resolved from the forwarder.
#### 2. Create a GPO for collector computers
1. Open Server Manager, select Tools, and select Group Policy Management.
2. Right-click the OU containing the collector computers and select Create a GPO in this domain, and Link it here.... In the window, give the GPO a name and click OK.
3. Right-click the newly created GPO, and click Edit... to open the Group Policy Management Editor. Navigate to Computer Configuration/Policies/Windows Settings/Security Settings/Windows Defender Firewall with Advanced Security/Inbound Rules and right-click it and select New Rule....
4. In the opened wizard on the Rule Type page, select Predefined and in the dropdown menu select Windows Remote Management then click Next.
5. On Predefined Rules, ensure all the rules are selected and click Next.
6. On Action, select Allow the connection and click Finish to close the wizard.
7. Returning to the Group Policy Management Editor, navigate to Computer Configuration/Administrative Templates/Windows Components/Windows Remote Management/WinRM Service and click it. In the panel, right-click the setting Allow remote server management through WinRM and select Edit.
8. In the window, select Enabled, and in the options, add the IP ranges of the interfaces on the machines. You may use \* to select all interfaces. Click OK.
#### 3. Create a subscription in Event Viewer
1. Open Server Manager, select Tools, and select to open Event Viewer.
2. In the left-side panel of the Event Viewer, click Subscriptions. This may prompt you, To work with subscriptions, the Windows Event Collector Service must be running and configured. Do you want to start the service and/or configure it to automatically start when the computer is restarted? Click Yes.
3. On the right-side panel of the Event Viewer, click Create Subscription....
4. In this opened Subscription Properties window, name the subscription and give it a description on what it collects. For Subscription type and source computers, select Source computer initiated and click Select Computer Groups.... In the Computer Groups Window, click to Add Domain Computers... or Add Non-Domain Computers...,. Click Ok to finish.
5. Back at the Subscription Properties window, click the down arrow by the Select Events... to build a query in the Query Filter window or Copy from existing Custom View... from a selection window. Click OK to finish and close the window.
6. Back at the Subscription Properties window click Advanced... to open the Advanced Subscription Properties window. For Event Delivery Optimization, select Minimize Latency, and select HTTP for protocol. Click OK to finish and close window. Click OK.
#### 4. Create an OU and security group for source computers
1. Open Server Manager, select Tools, and select to open Active Directory Users and Computers.
2. Right-click the domain name, select New/Organizational Unit and give it a name to describe the collector computer OU, and then click OK.
3. Browse for computers to be used as collectors; right-click the computer(s) and select Move..., and in the selection window, select the OU for the collector computers and click Ok.
4. Right-click the source computers OU, select New/Group and give it a Group name to describe the security group for the source computers. Other options can be left as default. Click OK.
5. Select your source computers in OU and right-click, select Add to a group..., enter the name of the security group created in the previous step, click OK.
#### 5. Create GPO for source computers to forward logs to collectors
1. Open Server Manager, select Tools, and select to open Group Policy Management.
2. Click and expand Forest, expand Domain, expand the domain name.
3. Right-click the source computers OU, and select Create a GPO in this domain, and Link it here.... In the window, give the GPO a name and click OK.
4. Right-click the newly created GPO, and click Edit... to open the Group Policy Management Editor.
5. Navigate to Computer Configuration/Policies/Windows Settings/Security Settings/Windows Firewall with Advanced Security/Inbound Rules and right-click it and select New Rule....
6.  In the opened wizard on the Rule Type page, select Predefined and in the dropdown menu select COM+ Network Access then click Next.
7. On Predefined Rules, ensure all the rules are selected and click Next.
8. On Action, select Allow the connection and click Finish to close the wizard.
9. Repeat Step 5 to 8, using, on Rule Type, Predefined and select in the dropdown menu, Remote Event Log Management.
10. Returning to the Group Policy Management Editor, navigate to Computer Configuration/Administrative Templates/Windows Components/Event Forwarding and right-click the setting Configure target Subscription Manager and select Edit.... Set to Enabled and in the Options follow the Explain and add the address in the format of: Server=http://\<fqdn\>:5985/wsman/SubscriptionManager/WEC. Click OK.
11. To enable collecting Security logs. Return to the Group Policy Management Editor, navigate to Computer Configuration/Administrative Templates/Windows Components/Event Log Service/Security, right-click the setting Configure log access and select Edit.... Set to Enabled. In the Options, enter the SDDL the includes READ access for NETWORK SERVICE. See Notes and reference on those instructions.
12. Returning to the Group Policy Management Editor, navigate to Computer Configuration/Preferences/Control Panel Settings/Local Users and Groups. Right-click it and select New/Local Group to open the group Properties.
13. In this window, change the Group name to Event Log Readers (built-in) using the dropdown menu. Under members, click Add... and type in the name: NT AUTHORITY/NETWORK SERVICE. And click OK. Click OK again.
#### 6. Verification
- Verify at the collector computers, the expected number of source computers.
- Verify at the collector computers, in the Event Viewer there are no issues in the logs at Applications and Services Logs/Microsoft/Windows/Event Collector.
- Verify at the source computers, in the Event Viewer there are no issues in the logs at Applications and Services Logs/Microsoft/Windows/Eventlog-Forwarding.
## Notes and references
1. [Setting up a Source Initiated Subscription](https://learn.microsoft.com/en-us/windows/win32/wec/setting-up-a-source-initiated-subscription) Article describes procedure to setup source-initiated subscriptions for logging excluding Security logs over HTTP or HTTPS
2. [How to enable WinRM with domain controller Group Policy for WMI monitoring](https://support.auvik.com/hc/en-us/articles/204424994-How-to-enable-WinRM-with-domain-controller-Group-Policy-for-WMI-monitoring) In the event, where you may encounter errors related to firewall, some procedures to check and or create necessary firewall rules to enable the process.
3. [Efficient Windows Event Collector Setup & Configuration](https://adamtheautomator.com/windows-event-collector/) A more concise alternative guide. Covers how to enable forwarding Security logs.
4. [How to set event log security locally or by using Group Policy](https://learn.microsoft.com/en-us/troubleshoot/windows-server/group-policy/set-event-log-security-locally-or-via-group-policy) May be necessary to ensure the security log has the appropriate SDDL to allow NETWORK SERVICE to READ the security log, if forwarding security logs.
	1. [Collecting events from Domain Controllers - Source Initiated and Events not forwarding](https://www.reddit.com/r/activedirectory/comments/1izoee3/collecting_events_from_domain_controllers_source/)
	2. [Permissions changes on Windows event log are not working (GPO change)](https://serverfault.com/questions/962417/permissions-changes-on-windows-event-log-are-not-working-gpo-change)
	3. [Windows Event Forwarding Help](https://www.reddit.com/r/sysadmin/comments/dsg4q8/windows_event_forwarding_help/)
	4. [How to set a Windows Service's permissions using ServiceControl and SDDL](https://www.advancedinstaller.com/forums/viewtopic.php?t=49990&sid=75ead9aabfe01d0db0c58e77cb26d256) Alternative. Can be used to build scripts or reference for manual changes.
	5. Information about interpreting SDDL.
		1. https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/dcom-machine-access-restrictions-in-security-descriptor-definition-language-sddl-syntax
		2. [Event Forwarding - Security Log Permissions](https://learn.microsoft.com/en-us/answers/questions/426966/event-forwarding-security-log-permissions)
		3. https://techcommunity.microsoft.com/blog/askds/the-security-descriptor-definition-language-of-love-part-1/395202
		4. https://techcommunity.microsoft.com/blog/askds/the-security-descriptor-definition-language-of-love-part-2/395258

## Revision History
- Version 1: Created.
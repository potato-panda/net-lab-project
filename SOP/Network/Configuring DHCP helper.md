---
creation date: 2025-08-06 15:16
modification date: 2025-08-06 15:16
version: "1"
---
## Purpose
To provide a standardize procedure to allow hosts to use DHCP for automatic IP addressing within the network.
## Scope
The SOP applies to network technicians to configure routers within the target environment to traffic DHCP packets over subnets to the server.
## Procedures
#### 1. Pre-requisites
- A DHCP server is configured on the network.
- Hosts have physical connectivity to the DHCP server through an intermediary path consisting of zero or more switches and one or more routers.
#### 2. Configuration of the router
1. Identify the intermediate routers between the host and the DHCP server for configuration.
2. For each router, identify the host facing links.
3. Copy the following configuration text to a notepad and edit to adhere to change specification and apply to the router:
```
enable
configure terminal
int <interface type> <interface number>
ip helper-address <dhcp server ip address>
end
```
#### 3. Verification
1. On the Windows host, open Command Prompt or PowerShell, type in `ipconfig /all` to verify the host has an IP address from the DHCP server.
	- If the host did not get an IP address, run the commands:
		- `ipconfig /release`
		- `ipconfig /renew`
	- Verify with `ipconfig /all` if there is a new IP address from the DHCP server.
## Notes and references

## Revision History
- Version 1: Created.
# Windows Server, AD, and Networking Lab
A basic topology of a Windows Server based network. Implements routing and switching with redundancy: routers configured with VRRP and utilizing partial mesh topology, defined subnets and VLANs, DHCP to lease IP addresses according to IP addressing scopes. Primary Domain Controller supports Certificate Authority. Managed GPOs to secure users, computers, and remote access.
![[topology.png]]
#### Subnets
- 10.0.0.0/31, OSPFv3 Area 0
- 10.0.0.2/31, OSPFv3 Area 0
- 10.0.0.4/31, OSPFv3 Area 0
- 10.0.0.6/31, OSPFv3 Area 0
- 10.0.10.0/24, OSPFv3 Area 1, VLAN 10
- 10.0.51.0/24, OSPFv3 Area 1, VLAN 51
- 10.1.51.0/24, OSPFv3 Area 2, VLAN 51
- 10.0.20.0/24, OSPFv3 Area 2, VLAN 20
- 10.0.30.0/24, OSPFv3 Area 2, VLAN 30
#### Servers
1. **LAB-PRIMARY** - Windows Server 2022
	- 10.0.10.4/24
	- 10.77.0.11/24
	- AD - DC
		- netlab.com
	- DNS
	- DHCP
		- Scopes
			1. Staff (10.0.20.0)
				- 10.0.20.0-10.0.20.100
				- Exclude 10.0.20.1-10.0.20.10
				- DNS Domain Name: netlab.com
				- DNS Servers: 10.10.10.2
				- Default gateway: 10.10.200.1
			2. Guests (10.0.30.0)
				- 10.0.30.0-10.0.30.100
				- Exclude 10.0.30.1-10.0.30.10
				- DNS Domain Name: netlab.com
				- DNS Servers: 10.10.10.2
				- Default gateway: 10.10.201.1
2. **LAB-SECONDARY** - Windows Server 2022
	- 10.0.10.5/24
	- netlab.com
		- AD - DC
	- SRV1 - DHCP fallback
3. **AAA-1**
	- 10.0.51.101/24
	- gateway 10.0.51.1
	- nameserver 10.10.10.2
	- TACACS+ users:
		- gns3 - gns3
		- readonly - gns3
#### Routers
- OSPFVv3 Area 1
	- VRRP
		- 10.0.10.1/24: 10.0.10.2, 10.0.10.3
		- 10.0.51.1/24: 10.0.51.2, 10.0.51.3
- OSPFVv3 Area 2
	- VRRP
		- 10.0.20.1/24: 10.0.20.2, 10.0.20.3
		- 10.0.30.1/24: 10.0.30.2, 10.0.30.3
		- 10.1.51.1/24: 10.1.51.2, 10.1.51.3

#### Switches
- Root bridges: SW1, SW5
- Root secondary bridges: SW3, SW6
- VLANS:
	- 10 - Servers
	- 20 - Staff
	- 30 - Guests
	- 51 - Management
	- 81 - Native
#### Guests
- Guests are event log sources. They push Security event logs to event collectors on subscription at LAB-PRIMARY.
- Guests are remotely accessible with Remote Desktop.
1. **STAFF-1** - Windows 10
	- DHCP
	- netlab.com
2. **STAFF-2** - Windows 10
	- DHCP
	- netlab.com
#### Changes
- 08/08/2025
	1. Modified topology for redundancy
		1. Added two more routers and 6 switches
		2. Redesigned subnets and VLANs
		3. Adjusted IP services and addressing according to these changes
	2. Implemented access port security and root guard to secure Layer 2 networking
	3. Added an isolated subnet for future implementation of secure storage
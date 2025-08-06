# Windows Server, AD, and Networking Lab
A basic topology of a Windows Server based network.
[[topology.png]]
#### Subnets
- 10.10.10.0/24, OSPFv3 Area 1
- 10.10.20.0/24, OSPFv3 Area 1
- 10.10.100.0/31, OSPFv3 Area 0; Backbone
- 10.10.200.0/24, OSPFv3 Area 2
- 10.10.201.0/24, OSPFv3 Area 2
- 10.10.11.0/24, OSPFv3 Area 2
#### Servers
1. **LAB-PRIMARY** - Windows Server 2022
	- 10.10.10.2/24
	- AD - DC
		- netlab.com
	- DNS
	- DHCP
		- Scopes
			1. Guests (10.10.200.0)
				- 10.10.200.0-10.10.200.100
				- Exclude 10.10.200.1-10.10.200.10
				- DNS Domain Name: netlab.com
				- DNS Servers: 10.10.10.2
				- Default gateway: 10.10.200.1
			2. Guests (10.10.201.0)
				- 10.10.201.0-10.10.201.100
				- Exclude 10.10.201.1-10.10.201.10
				- DNS Domain Name: netlab.com
				- DNS Servers: 10.10.10.2
				- Default gateway: 10.10.201.1
2. **LAB-SECONDARY** - Windows Server 2022
	- 10.10.20.2/24
	- netlab.com
		- AD - DC
	- SRV1 - DHCP fallback
3. **AAA-1**
	- 10.10.11.2/24
	- gateway 10.10.11.1
	- nameserver 10.10.10.2
	- TACACS+ users:
		- gns3 - gns3
		- readonly - gns3
#### Guests
- Guests are event log sources. They push Security event logs to event collectors on subscription at LAB-PRIMARY.
- Guests are remotely accessible with Remote Desktop.
1. **GUEST-200-1** - Windows 10
	- DHCP
	- netlab.com
2. **GUEST-201-1** - Windows 10
	- DHCP
	- netlab.com
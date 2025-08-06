---
creation date: 2025-07-31 16:35
modification date: 2025-07-31 16:35
version: "1"
---
## Purpose
To provide a standardize process for adding a new router to an existing network topology to ensure consistent configuration, and maintains documentation of changes to prevent configuration drift minimizing potential connectivity issues.
## Scope
The SOP applies to network technicians and engineers for expanding or modifying the network topology within the target environment.
## Procedures

#### 1. Pre-requisite
- Confirm topology diagram and IP address plan is current.
- Identify placement of the router.
- Determine a hostname, router ID, OSPF areas, and subnets.
#### 2. Deployment
1. Insert router into the topology.
2. Connect interfaces.
#### 3. Configuration
Copy the following configuration text to a notepad and edit to adhere to change specification and apply to the router.
```
enable
configure terminal
hostname R<#>
ipv6 unicast-routing
router ospfv3 <#>
address-family ipv4 unicast
router-id <router-id>
exit
interface <interfaceType> <interfaceNumber>
ip address <ipAddress> <subnetMask>
ipv6 enable
ospfv3 <#> ipv4 area <#>
no shut
end
```
### 4. Verification
1. `ping` adjacent routers
2. `show ip route` to confirm OSPFv3 routes are learned and installed
3. `show ospfv3 neighbor` to confirm OSPFv3 neighbors

### 5. Update topology diagram
- Ensure device placement is accurate.
- Ensure links to connected devices are accurate.
- Verify the IP addressing of the devices.
- Document security detail and ACL.
- Document routing protocols and routes.
## Notes and references
1. [Use OSPFv3 Configuration Example](https://www.cisco.com/c/en/us/support/docs/ip/ip-version-6-ipv6/112100-ospfv3-config-guide.html)
## Revision History
- Version 1: Created.
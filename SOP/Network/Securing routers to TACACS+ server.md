---
creation date: 2025-07-31 17:05
modification date: 2025-07-31 17:05
version: "1"
---
## Purpose
To provide a standardize procedure to secure router access by configuring the router as a TACACS+ client to enforce a singular secure way to access routers remotely with fixed credentials
## Scope
The SOP applies to network technicians and engineers for securing remote access to routers within the target environment.
## Procedures
#### 1. Pre-requisite
- Confirm TACACS+ server credentials and pre-shared key.
- Ensure TACACS+ server has a static IP address.
- Confirm router hostname and domain name.
- Confirm router's connectivity to the TACACS+ server.
- Confirm credentials to be used to secure remote access to the router.
#### 2. Configuration
```
enable
configure terminal
username <crendential username> privilege 15 secret <crendential password>
crypto key generate rsa modulus 2048
ip ssh version 2
aaa new-model
tacacs server <TACACS+ server name>
address ipv4 <TACACS+ server ip address>
key <pre-shared key>
exit
aaa authentication login default group tacacs+ local
aaa authorization exec default group tacacs+ local
line vty 0 4
transport input ssh
login authentication default
end
```
#### 3. Verification
- Test if the router successfully authenticates against the server. In `exec` mode: `test aaa group tacacs+ <username> <password> new-code`
- From a remote machine, access the router with `ssh` using the determined credentials.
- Accessor should authenticate successfully and enter `exec` mode.
## Notes and references
1. [Configure Basic AAA on an Access Server](https://www.cisco.com/c/en/us/support/docs/security-vpn/terminal-access-controller-access-control-system-tacacs-/10384-security.html#toc-hId-94585741)
2. [Configuring TACACS+](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9600/software/release/17-17/configuration_guide/sec/b_1717_sec_9600_cg/configuring_tacacs_.html)
3. [Access To router via ssh authenticating through tacacs](https://community.cisco.com/t5/routing/access-to-router-via-ssh-authenticating-through-tacacs/td-p/1126881)
## Revision History
- Version 1: Created.
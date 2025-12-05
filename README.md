# Enterprise Multi‑Subnet Lab Architecture  
pfsense-windows-enterprise-architecture/
│
├── README.md
│
├── diagrams/
│   ├── network-topology.png
│   ├── routing-flow.png
│   └── dhcp-failover.png
│
├── configs/
│   ├── pfsense-edge.conf
│   └── pfsense-internal-router.conf
│
├── scripts/
│   ├── ad-dns-setup.ps1
│   ├── dhcp-setup.ps1
│   └── dhcp-failover.ps1
│
└── notes/
    └── validation-checklist.txt

    # Enterprise Multi‑Subnet Lab  
Dual pfSense • DHCP Failover • Active Directory • DNS

A complete enterprise-style lab environment featuring:

- pfSense Edge Firewall (192.168.0.45)
- pfSense Internal Router (192.168.0.50 → 192.168.1.1 / 192.168.2.1)
- Two routed subnets:
  - 192.168.1.0/24 (Servers + Clients)
  - 192.168.2.0/24 (Clients)
- Windows Server AD DS + DNS (192.168.1.210)
- DHCP failover across both subnets:
  - DHCP-SRV01: 192.168.1.215
  - DHCP-SRV02: 192.168.2.220
- Domain‑joined Windows clients

All configuration files and automation scripts are included.

---

## Network Diagram

`/diagrams/network-topology.png`

---

## IP Summary

| Device | IP | Notes |
|--------|-----|--------|
| Main Router | 192.168.0.1 | Upstream gateway |
| pfSense Edge | 192.168.0.45 | WAN |
| pfSense Internal Router | 192.168.0.50 | WAN |
| LAN1 Gateway | 192.168.1.1 | Subnet 1 |
| LAN2 Gateway | 192.168.2.1 | Subnet 2 |
| Domain Controller | 192.168.1.210 | DNS |
| DHCP-SRV01 | 192.168.1.215 | Primary DHCP |
| DHCP-SRV02 | 192.168.2.220 | Failover DHCP |

---

## DHCP Scopes

- 192.168.1.100–250  
- 192.168.2.100–250  

Both DHCP servers serve both scopes.

---

## Scripts

Located in `/scripts/`:

- `ad-dns-setup.ps1` — AD DS + DNS deployment  
- `dhcp-setup.ps1` — DHCP scopes + options  
- `dhcp-failover.ps1` — DHCP failover configuration  

---

## pfSense Configs

Located in `/configs/`:

- `pfsense-edge.conf`  
- `pfsense-internal-router.conf`  

---

## Purpose

This lab demonstrates:

- Routed segmentation  
- DHCP failover  
- Centralized DNS  
- Multi‑subnet communication  
- Realistic enterprise topology  

pfSense Config Files 
# pfSense Edge Firewall
wan_ip="192.168.0.45"
wan_gw="192.168.0.1"

nat_outbound="hybrid"
nat_rule_lan1="192.168.1.0/24 → WAN"
nat_rule_lan2="192.168.2.0/24 → WAN"

allow_internal="192.168.1.0/24, 192.168.2.0/24 → ANY"
# pfSense Internal Router
wan_ip="192.168.0.50"
wan_gw="192.168.0.45"

lan1_ip="192.168.1.1/24"
lan2_ip="192.168.2.1/24"

allow_lan1="192.168.1.0/24 → ANY"
allow_lan2="192.168.2.0/24 → ANY"


Install-WindowsFeature AD-Domain-Services, DNS -IncludeManagementTools

Install-ADDSForest `
  -DomainName "lab.local" `
  -DomainNetbiosName "LAB" `
  -SafeModeAdministratorPassword (ConvertTo-SecureString "Password123" -AsPlainText -Force) `
  -Force

Set-DnsServerForwarder -IPAddress 8.8.8.8,1.1.1.1

Install-WindowsFeature DHCP -IncludeManagementTools
Add-DhcpServerInDC -DnsName "dhcp01.lab.local" -IpAddress 192.168.1.215

Add-DhcpServerv4Scope -Name "LAN1" -StartRange 192.168.1.100 -EndRange 192.168.1.250 -SubnetMask 255.255.255.0
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -DnsServer 192.168.1.210 -Router 192.168.1.1

Add-DhcpServerv4Scope -Name "LAN2" -StartRange 192.168.2.100 -EndRange 192.168.2.250 -SubnetMask 255.255.255.0
Set-DhcpServerv4OptionValue -ScopeId 192.168.2.0 -DnsServer 192.168.1.210 -Router 192.168.2.1

Add-DhcpServerv4Failover `
  -Name "DHCP-FO" `
  -PartnerServer "192.168.2.220" `
  -ScopeId 192.168.1.0,192.168.2.0 `
  -LoadBalancePercent 50 `
  -SharedSecret "Password123"


[✔] pfSense Edge WAN reachable
[✔] pfSense Internal Router WAN reachable
[✔] LAN1 gateway reachable
[✔] LAN2 gateway reachable
[✔] DNS resolves internal + external
[✔] DHCP scopes active
[✔] DHCP failover state: Normal
[✔] Clients receive correct gateway + DNS
[✔] Domain join successful


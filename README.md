Enterprise Multi‑Subnet Network Design & Implementation
This project delivers a fully documented, enterprise‑style multi‑subnet network architecture built using pfSense, Windows Server, and routed segmentation. The environment replicates real‑world design principles, including perimeter security, internal routing, centralized identity services, DHCP failover, and multi‑subnet communication. All components have been configured, validated, and documented following production‑grade standards to ensure clarity, scalability, and reproducibility.

Architecture Overview
The network follows a structured, layered design aligned with enterprise deployment practices. The architecture consists of:

Edge Firewall providing perimeter security, NAT, and upstream routing

Internal Router enabling routed segmentation across multiple subnets

Core Services, including Active Directory, DNS, and DHCP, failover

Client Networks are distributed across two routed subnets with centralized identity and addressing

This design ensures predictable behaviour, simplified management, and realistic operational workflows.

Core Components
pfSense Edge Firewall
The edge firewall acts as the perimeter security layer and upstream gateway for the internal network.

WAN IP: 192.168.0.45

Upstream Gateway: 192.168.0.1

Responsibilities: NAT, WAN routing, internal network access control

pfSense Internal Router
The internal router provides segmentation and routing between multiple internal subnets.

WAN IP: 192.168.0.50

LAN Gateways:

192.168.1.1 — Subnet 1

192.168.2.1 — Subnet 2

Responsibilities: inter‑subnet routing, internal traffic control

Subnets
The network is divided into two routed subnets:

192.168.1.0/24 — Servers + Clients

192.168.2.0/24 — Clients

Windows Server Infrastructure
The core identity and addressing services are hosted on Windows Server.

Domain Controller + DNS: 192.168.1.210

DHCP Failover Pair:

DHCP‑SRV01: 192.168.1.215

DHCP‑SRV02: 192.168.2.220

All configuration files and automation scripts are included for full reproducibility.

Network Diagram
The primary network topology diagram illustrates the full architecture, including:

Edge firewall placement

Internal routing flow

Subnet segmentation

DHCP failover structure

The diagram is included in the diagrams directory.

IP Address Summary
Device	            IP Address	    Notes
Main Router	        192.168.0.1	    Upstream gateway
pfSense Edge	    192.168.0.45	WAN
pfSense Internal	192.168.0.50	WAN
LAN1 Gateway	    192.168.1.1	    Subnet 1
LAN2 Gateway	    192.168.2.1	    Subnet 2
Domain Controller   192.168.1.210	DNS + AD DS
DHCP‑SRV01	        192.168.1.215	Primary DHCP
DHCP‑SRV02	        192.168.2.220	Failover DHCP
DHCP Scopes
Two DHCP scopes are configured to support both routed subnets:

192.168.1.100 – 192.168.1.250

192.168.2.100 – 192.168.2.250

Both DHCP servers participate in a failover pair and serve both scopes for redundancy and high availability.

Automation Scripts
The project includes PowerShell automation scripts to streamline deployment and reduce manual configuration errors.

Active Directory + DNS Deployment

DHCP Scope Creation

DHCP Failover Configuration

These scripts ensure a consistent, repeatable setup across environments.

pfSense Configuration
Configuration files for both the edge firewall and internal router are included. They define:

WAN and LAN interface assignments

Outbound NAT rules

Internal routing policies

Firewall allows rules

These files provide a complete reference for restoring or replicating the firewall configuration.

Windows Server Configuration
Active Directory + DNS
The AD DS and DNS configuration includes:

Installing required roles

Creating a new forest

Configuring DNS forwarders

DHCP + Failover
The DHCP configuration includes:

Installing the DHCP role

Creating scopes for both subnets

Setting DNS and gateway options

Establishing a failover partnership

This ensures resilient IP address management across the network.

Validation Checklist
The following validation steps confirm that the environment is fully functional:

✅ Edge firewall reachable

✅ Internal router reachable

✅ LAN1 gateway reachable

✅ LAN2 gateway reachable

✅ DNS resolves internal and external queries

✅ DHCP scopes active

✅ DHCP failover state: Normal

✅ Clients receive the correct gateway and DNS

✅ Domain join successful







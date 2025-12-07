Enterprise Multi‑Subnet Network Design & Implementation
This project showcases a complete enterprise‑style multi‑subnet network architecture built using pfSense, Windows Server, and routed segmentation. The goal of this environment is to replicate real‑world infrastructure design, including perimeter security, internal routing, centralized identity services, DHCP failover, and multi‑subnet communication. Every component has been configured, validated, and documented with a workflow‑first approach to ensure clarity, reproducibility, and operational accuracy.

Repository Structure
Code
\\\pfsense-windows-enterprise-architecture/ │── README.md │── diagrams/ # Network topology, routing flow, DHCP failover diagrams │── configs/ # pfSense Edge + Internal Router configuration files │── scripts/ # PowerShell automation scripts (AD, DNS, DHCP, Failover) └── notes/ # Validation checklist, IP summary, documentation notes \\\
This structure mirrors real enterprise documentation standards, separating diagrams, configuration files, scripts, and operational notes for maximum clarity.
Architecture Overview
The network is designed following a layered enterprise model. The pfSense Edge Firewall provides perimeter security, NAT, and upstream routing. Behind it, the pfSense Internal Router handles segmentation and routing between multiple internal subnets. Core identity and addressing services are hosted on Windows Server, including Active Directory, DNS, and a fully configured DHCP failover pair. Client systems operate across two routed subnets, receiving centralized authentication, DNS resolution, and redundant DHCP services. This architecture reflects how modern organizations structure their internal networks for reliability and scalability.

Core Components
pfSense Edge Firewall
The Edge Firewall acts as the primary security boundary and WAN gateway. It manages NAT, outbound traffic, and access control for internal networks.

WAN IP: 192.168.0.45

Upstream Gateway: 192.168.0.1

pfSense Internal Router
The Internal Router provides routed segmentation between multiple internal networks. It ensures clean separation of traffic while maintaining full communication across subnets.

WAN IP: 192.168.0.50

LAN Gateways: 192.168.1.1 (Subnet 1), 192.168.2.1 (Subnet 2)

Subnets
The environment includes two routed subnets:

192.168.1.0/24 — Servers + Clients

192.168.2.0/24 — Clients

Windows Server Infrastructure
Core identity and addressing services are deployed on Windows Server.

Domain Controller + DNS: 192.168.1.210

DHCP Failover Servers: 192.168.1.215 and 192.168.2.220

All configuration files and automation scripts are included for full reproducibility.

Network Diagram
The diagrams included in the repository illustrate the complete architecture, including firewall placement, routing flow, subnet segmentation, and DHCP failover design. These visuals provide a clear understanding of how traffic moves through the environment and how each component interacts.

IP Address Summary
Device	IP Address	Notes
Main Router	192.168.0.1	Upstream gateway
pfSense Edge	192.168.0.45	WAN
pfSense Internal	192.168.0.50	WAN
LAN1 Gateway	192.168.1.1	Subnet 1
LAN2 Gateway	192.168.2.1	Subnet 2
Domain Controller	192.168.1.210	DNS + AD DS
DHCP‑SRV01	192.168.1.215	Primary DHCP
DHCP‑SRV02	192.168.2.220	Failover DHCP
This table provides a quick reference for all major components in the environment.

DHCP Scopes
Two DHCP scopes are configured to support both routed subnets:

192.168.1.100 – 192.168.1.250

192.168.2.100 – 192.168.2.250

Both DHCP servers participate in a failover partnership, ensuring high availability and uninterrupted IP address assignment across the network.

Automation Scripts
The repository includes PowerShell automation scripts that deploy Active Directory, DNS, DHCP scopes, and DHCP failover. These scripts reduce manual configuration time and ensure consistent, repeatable deployments. They also reflect real enterprise workflows where automation is preferred for accuracy and speed.

pfSense Configuration
The pfSense configuration files included in the repository define WAN/LAN assignments, outbound NAT rules, internal routing policies, and firewall allow rules. These files provide a complete reference for restoring or replicating the firewall configuration in any environment.

Windows Server Configuration
Active Directory + DNS
The AD DS and DNS configuration includes role installation, forest creation, and DNS forwarder setup. This ensures centralized identity management and reliable name resolution across the network.

DHCP + Failover
The DHCP configuration includes role installation, scope creation, DNS/gateway option assignment, and failover partnership setup. This provides resilient IP address management across both subnets.

Validation Checklist
The environment has been fully validated to ensure operational readiness:

Edge firewall reachable

Internal router reachable

LAN1 and LAN2 gateways reachable

DNS resolves internal and external queries

DHCP scopes active

DHCP failover state: Normal

Clients receive the correct gateway and DNS

Domain join successful

This confirms that all components are functioning as expected and the architecture is production‑accurate.

















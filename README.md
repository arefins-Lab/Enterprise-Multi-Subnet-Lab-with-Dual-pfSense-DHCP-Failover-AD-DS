# Enterprise Multi‑Subnet Lab with Dual pfSense, DHCP Failover, and Active Directory Domain Services

This project demonstrates a complete enterprise‑grade multi‑subnet network architecture using dual pfSense routers, Windows Server‑based DHCP Failover, and Active Directory Domain Services (AD DS).  
The goal is to simulate a realistic production environment with routing, redundancy, centralized authentication, and automated network services — all running inside a fully isolated virtual lab.

---

## Objectives

- Build a multi‑subnet enterprise network using pfSense as the edge and internal routers  
- Configure Windows Server for AD DS, DNS, and DHCP  
- Implement DHCP Failover (Load Balance + Hot Standby)  
- Validate routing, name resolution, and domain join workflows  
- Create a reproducible, architecture‑first lab environment suitable for learning and portfolio demonstration  

---

## Architecture Overview

The lab uses two pfSense routers to simulate an enterprise edge firewall and an internal router.  
Windows Server provides centralized identity, DNS, and DHCP services.  
Multiple subnets are routed through pfSense, enabling realistic multi‑segment enterprise behaviour.

---
## Network Diagram

![Enterprise Multi-Subnet Lab Diagram](diagrams/Diagram.jpeg)

This diagram illustrates the full multi-subnet architecture, including:
- Dual pfSense routers (Edge + Internal)
- Windows Server with AD DS, DNS, DHCP
- DHCP Failover scopes across subnets
- Routed LAN 01 and LAN 02 segments
- Client workstations with domain join and DNS resolution


This diagram illustrates the full multi‑subnet architecture, including:
- pfSense Edge Router  
- pfSense Internal Router  
- Windows Server (AD DS, DNS, DHCP)  
- Routed subnets  
- Client network  

## Key Features

- **Multi‑Subnet Routing:**  
  Segmented network design with routed traffic between subnets

- **Dual pfSense Deployment:**  
  Edge firewall + internal router for realistic enterprise flow

- **Active Directory Domain Services:**  
  Centralized authentication and domain management

- **DNS Infrastructure:**  
  Forward lookup zones, reverse zones, forwarders, scavenging, and validation

- **DHCP Failover:**  
  Load Balance + Hot Standby configuration with automatic failover behaviour

- **PowerShell Automation:**  
  Scripts for AD DS installation, DHCP configuration, and server role setup

---

## Validation Checklist

- Subnet‑to‑subnet routing is successful  
- Domain join was successful from all subnets  
- DNS resolution validated (A, PTR, forwarders)  
- DHCP failover tested (Load Balance + Hot Standby)  
- pfSense routing rules validated  
- Client connectivity tested end‑to‑end  

---

## Notes

This lab is designed with an **architecture‑first** approach, focusing on clarity, reproducibility, and real‑world enterprise workflows.  
All diagrams, configs, and scripts are included to help learners and recruiters understand the full deployment process.













# Home Lab: Enterprise Infrastructure Environment

🚧 **Status:** Active project. Active Directory, domain-joined client, Group Policy, 
and a Linux web server are complete and verified. pfSense firewall, Raspberry Pi 
services, and cloud lab work are in progress.

## Purpose

A self-built home lab simulating enterprise IT infrastructure: Windows domain 
environment, segmented network behind a dedicated firewall, and supporting 
services. This was built to develop hands-on skills: virtualization, Windows/Linux administration, 
networking, and security fundamentals.

## Architecture

Internet → pfSense firewall (dedicated hardware) → LAN switch → VM host 
(Active Directory domain controller + Linux web server) and Raspberry Pi 5 
(DNS, monitoring, backups). See `/pfsense` for firewall configuration once complete.

## Contents

- [VM Host — Active Directory & Linux Server](./vm-host/) ✅ Complete
- [pfSense Firewall](./pfsense/) 🚧 In progress
- [Raspberry Pi Services](./raspberry-pi/) 🚧 Planned
- [Networking / CCNA](./networking/) 🚧 Planned

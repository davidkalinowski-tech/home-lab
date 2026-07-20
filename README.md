# Home-lab  
  
  



## Executive Summary 

A production-grade, self-hosted laboratory environment designed to mirror enterprise IT infrastructure. This repository documents the architecture, configuration management, and operational workflows used to deploy a highly available, secure, and monitored network spanning virtualization, centralized identity management, and resilient network storage. 

# Architectural Overview

Network Topology & SegmentationThe network utilizes standard 802.1Q VLAN trunking to enforce strict traffic isolation across distinct security zones. Inter-VLAN routing and firewall zones are managed by a centralized firewall topology.

```
VLAN ID   Subnet Zone     NameAccess / Purpose 
VLAN 10   10.10.10.0/24   Management Hypervisor hyper-visors, switch management interfaces, IPMI out-of-band management.
VLAN 20   10.10.20.0/24   Core Infrastructure Centralized Identity (Active Directory), DNS/DHCP servers, network storage interfaces.
VLAN 30   10.10.30.0/24   Trusted LAN Primary workstation, verified personal compute, and server infrastructure.
VLAN 40   10.10.40.0/24   DMZ / LabUntrusted test VMs, public-facing reverse proxy endpoints.
```

# Technology Stack & Implementations
```
1. Virtualization & Compute
    Hypervisor: Proxmox VE / VMware ESXi (Type-1 Hypervisors) running in a clustered environment.

    Workloads: A mix of full Linux/Windows server virtual machines alongside lightweight containerized applications deployed via Docker Compose and microK8s.

2. Identity & Access Management (Active Directory)
    Domain Controllers: Dual, redundant Windows Server 2022 Core Domain Controllers managing the internal forest (lab.local).

    Automation: Automated user onboarding, OU creation, and security group assignments implemented via PowerShell scripts found in the /scripts/active-directory directory.

    Security: Implemented Tiered Administrative Models via Group Policy Objects (GPOs) to isolate domain administrator credentials from standard client workstations.

3. Network Storage (NAS/SAN)
    Platform: TrueNAS Core / Enterprise scale storage array.

    Storage Pools: ZFS RAIDZ2 pool providing dual-disk parity failure tolerance.

    Protocols:

        SMB: Authenticated against Active Directory Domain Controllers to enforce ACLs (Access Control Lists) for shared corporate drives.

        NFS: High-throughput mounts exported specifically to hypervisors as primary backup storage targets.
```

[GitHub_04_Virtualisation_README.md](https://github.com/user-attachments/files/27048248/GitHub_04_Virtualisation_README.md)
# 🗄️ Enterprise Virtual Lab Environment

![VirtualBox](https://img.shields.io/badge/Hypervisor-VirtualBox-183A61?style=flat-square&logo=virtualbox&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows_Server-2019-0078D6?style=flat-square&logo=windows&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu_Server-22.04-E95420?style=flat-square&logo=ubuntu&logoColor=white)
![Status](https://img.shields.io/badge/Status-Running-brightgreen?style=flat-square)

A multi-VM virtualised infrastructure that serves as the foundation for all other lab projects. Designed to mirror an enterprise network environment — multiple servers, isolated network segments, and realistic failure scenarios — all running on a single physical machine.

---

## 🎯 Objective

To build a reusable, scalable virtual lab platform that supports cross-project testing, safe failure simulation, and rapid environment provisioning — skills directly transferable to managing virtualised infrastructure in corporate IT environments.

---

## 🛠️ Lab Topology

```
┌─────────────────────────────────────────────────────┐
│                  HOST MACHINE                        │
│                                                     │
│  ┌──────────────┐    ┌──────────────┐              │
│  │  DC-01       │    │  WEB-01      │              │
│  │  Win Server  │    │  Ubuntu      │              │
│  │  2019        │    │  Server      │              │
│  │  AD DS/DNS   │    │  Apache/SSH  │              │
│  │  DHCP        │    │              │              │
│  └──────┬───────┘    └──────┬───────┘              │
│         │                   │                       │
│  ┌──────▼───────────────────▼───────┐              │
│  │        Internal Network          │              │
│  │        192.168.10.0/24           │              │
│  └──────────────────┬───────────────┘              │
│                     │                               │
│  ┌──────────────────▼───────────────┐              │
│  │  CLIENT-01       │  CLIENT-02    │              │
│  │  Windows 10      │  Windows 10   │              │
│  │  Domain-joined   │  Domain-joined│              │
│  └──────────────────────────────────┘              │
└─────────────────────────────────────────────────────┘
```

---

## 🛠️ Environment

| VM | OS | Role | RAM | Storage |
|---|---|---|---|---|
| DC-01 | Windows Server 2019 | Domain Controller, DNS, DHCP | 2GB | 50GB |
| WEB-01 | Ubuntu Server 22.04 | Web Server, SSH host | 1GB | 20GB |
| CLIENT-01 | Windows 10 Pro | Domain workstation | 2GB | 40GB |
| CLIENT-02 | Windows 10 Pro | Domain workstation | 2GB | 40GB |

---

## ✅ What Was Built & Configured

### 1. Network Architecture
- **Internal Network** — isolated segment for all VMs (no direct internet access)
- **NAT adapter** — on DC-01 only, for controlled outbound access
- **Host-only adapter** — for SSH management from the host machine
- Configured static IPs on servers, DHCP-assigned on clients

### 2. VM Snapshots & Rollback Strategy
```
Snapshot naming convention:
  [VM]-[DATE]-[STATE]
  
Example:
  DC-01-2025-08-01-CLEAN-INSTALL
  DC-01-2025-08-15-AD-CONFIGURED
  DC-01-2025-09-01-GPOS-APPLIED
```
- Created baseline snapshots before each major configuration change
- Practised rolling back to clean states to repeat troubleshooting scenarios
- Used snapshots to simulate before/after states for change management

### 3. VM Cloning & Rapid Deployment
- Created a sysprepped Windows 10 master image for rapid workstation cloning
- Cloned and domain-joined new client VMs in under 15 minutes
- Documented the process as a repeatable deployment runbook

### 4. Network Modes — Tested & Understood

| Mode | Use Case | Internet | Inter-VM |
|---|---|---|---|
| NAT | Outbound internet only | ✅ | ❌ |
| Bridged | Full network access | ✅ | ✅ |
| Internal | Isolated lab network | ❌ | ✅ |
| Host-only | Host ↔ VM management | ❌ | ✅ |

### 5. Failure Simulation Scenarios

| Scenario | Method | Resolution Practised |
|---|---|---|
| DC unreachable | Shut down DC-01 VM | Client login with cached credentials |
| DHCP failure | Stopped DHCP service | Manual static IP assignment |
| DNS failure | Deleted A records | Restored from AD-integrated DNS backup |
| Disk full | Filled disk with test data | Identified and cleared with disk cleanup |
| NIC misconfiguration | Changed IP to wrong subnet | Diagnosed with `ipconfig` / `ping` |

---

## 📸 Screenshots

> _VirtualBox Manager, network topology diagrams, and snapshot trees will be added here._

---

## 📚 Skills Demonstrated

`VirtualBox` `VM Networking` `Snapshots & Rollback` `VM Cloning` `Network Segmentation` `NAT` `Host-only Networking` `Internal Networking` `Sysprep` `Infrastructure Design` `Failure Testing`

---

## 🔗 Related Projects

- [Active Directory Infrastructure Lab](../01-active-directory-lab)
- [Linux Server Administration Lab](../03-linux-server-lab)
- [Backup & Disaster Recovery Lab](../05-backup-dr-lab)

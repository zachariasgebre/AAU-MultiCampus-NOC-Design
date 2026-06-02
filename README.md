Addis Ababa University
College of Technology and Built Environment
Multi-Campus Enterprise Core Network & Infrastructure Design (NOC Project)

This repository contains the engineering design, physical Packet Tracer simulation files, and production-ready Cisco IOS configurations for the Addis Ababa University (AAU) multi-campus enterprise infrastructure upgrade. The architecture seamlessly integrates three core geographic campuses back to a secure, centralized Network Operations Center (NOC) segment.

---

👥 | Student Name | ID Number |

Zacharias Gebre    UGR/9336/15 
# 🗺️ Network Architecture & Addressing Matrix

The enterprise architecture utilizes a structured hub-and-spoke topology, with the 5 Kilo Campus operating as the central transit hub connecting the remote branches to the centralized NOC cluster. 

| Subnet Purpose | Network Segment | Default Gateway | Management IP (VLAN 1) |
| :--- | :--- | :--- | :--- |
| **Central NOC Server Core** | `192.168.200.0/24` | `192.168.200.1` | — |
| **5 Kilo Campus LAN (AAiT Hub)** | `192.168.20.0/24` | `192.168.20.1` | `192.168.20.50` |
| **6 Kilo Campus LAN (Science)** | `192.168.10.0/24` | `192.168.10.1` | `192.168.10.50` |
| **4 Kilo Campus LAN (Expansion)** | `192.168.30.0/24` | `192.168.30.1` | `192.168.30.50` |
| **6 Kilo ↔ 5 Kilo Transit Trunk** | `192.168.100.0/30` | — | — |
| **4 Kilo ↔ 5 Kilo Fiber Backbone** | `192.168.101.0/30` | — | — |

---

## ⚡ Key Engineering Features

### 1. Physical Hardware & Interface Module Expansion
To resolve fixed onboarding interface limitations on the Cisco ISR platform, a physical modular hardware upgrade was executed on the core transit boundaries:
* **HWIC-1GE-SFP Module:** Deployed in Slot 0 to add high-speed Small Form-Factor Pluggable bays.
* **GLC-LH-SMD Transceiver:** Installed into the HWIC cage to support 1000BASE-LX/LH data flows over **Single-Mode Fiber (SMF)**, simulating an underground inter-campus optical fiber backbone link.

### 2. Centralized NOC Resource Provisioning (DHCP Relay)
To reduce management overhead across remote branches, network endpoints dynamically pull configurations from a single centralized host (**NOC Server: `192.168.200.10`**):
* Localized campus gateways intercept Layer 2 discovery broadcasts and transform them into deterministic unicast streams using the Cisco IOS `ip helper-address` agent architecture.

### 3. Management Plane Hardening (Infrastructure Protection)
Administrative access to infrastructure switches is tightly locked down to block internal user elements, staff desks, or unauthorized networks:
* Standard Access Control Lists (`access-list 10`) are bound directly to virtual terminal lines (`line vty 0 4`).
* **Enforced Policy:** Only management packets originating explicitly from the secure NOC management subnet (`192.168.200.0/24`) are permitted access, dropping all outside connections natively at the TCP layer.

---

## 📂 Repository Contents
* `*.pkt` - Cisco Packet Tracer complete topology file featuring green convergence and full routing matrix maps.
* `*.docx` - Formal comprehensive engineering project documentation and acceptance testing suite ready for academic evaluation.

---

## 🧪 Verification & Acceptance Tests
The topology is verified operational using four core performance metrics:
1. **Centralized Scope Acquisition:** Endpoint hosts successfully switch from static configurations to pull centralized DHCP leases from the NOC.
2. **End-to-End Core Connectivity:** Complete multi-hop ICMP round-trips succeed from the furthest remote 4 Kilo node straight to 6 Kilo assets.
3. **Management Gate Security:** Outside endpoints attempting remote Telnet requests to infrastructure components are explicitly dropped with a `Connection closed by foreign host` warning.
4. **NOC Management Audit:** Explicit Telnet verification succeeds smoothly when executed from the designated NOC administrative subnet.

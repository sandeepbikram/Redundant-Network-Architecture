# 🏢 Company Network Design – Case Study

## 📋 Project Overview

A fast‑growing tech start‑up will employ **750 staff** in a single 3‑storey building.  The client requires a robust, scalable, and secure enterprise network that supports both wired and wireless access, leverages a **Cisco three‑tier hierarchical architecture**, and offers resilient Internet connectivity through two ISPs (MTN & Airtel).

This repository contains the Packet Tracer project file and all design artefacts for the proposed solution.

---

## 🏢 Physical Layout

| Floor     | Departments                       | Users   |
| --------- | --------------------------------- | ------- |
| 3rd       | Information Technology (IT)       | 150     |
| 3rd       | **Server Room**                   | —       |
| 2nd       | Finance & Accounts                | 150     |
| 2nd       | Administration & Public Relations | 150     |
| 1st       | Sales & Marketing                 | 150     |
| 1st       | Human Resources & Logistics       | 150     |
| **Total** | 6 user‑facing departments         | **750** |

---

## 🖧 Logical Topology & Subnet Plan

Base address block: **192.168.1.0 /22** (1,024 hosts)

| VLAN | Department                   | Subnet      | Mask | Hosts |
| ---- | ---------------------------- | ----------- | ---- | ----- |
| 10   | Sales & Marketing            | 192.168.1.0 | /24  | 254   |
| 20   | HR & Logistics               | 192.168.2.0 | /24  | 254   |
| 30   | Finance & Accounts           | 192.168.3.0 | /24  | 254   |
| 40   | Admin & PR                   | 192.168.4.0 | /24  | 254   |
| 50   | IT & Communication           | 192.168.5.0 | /24  | 254   |
| 60   | Server Farm                  | 192.168.6.0 | /24  | 254   |
| 99   | Management (OOB + Loopbacks) | 192.168.7.0 | /28  | 14    |

> **Note:** /24 networks comfortably exceed each department’s 150‑user requirement and simplify route summarisation at the distribution layer.

---

## 🏗️  Three‑Tier Architecture

```
                 ┌─────────┐   ┌─────────┐
                 │  Core‑1 │───│  Core‑2 │  <– HSRP pair
                 └─────────┘   └─────────┘
                    ││  ▲▲       ││  ▲▲
      ──────────────┘└──┴┴───────┘└──┴┴──────────────
                    Distribution Layer (per floor)
          ┌───────────────┐     ┌───────────────┐
          │    Dist‑1     │ …   │     Dist‑2    │  <– HSRP pair
          └───────────────┘     └───────────────┘
                 ▲▲││                     ▲▲││
      ───────────┘└┴┴─────────────────────┘└┴┴─────────
                        Access Layer
  (one stack or pair of access switches per department/VLAN)
```



---

## 🌐 Internet Edge


| ISP    | Router Interface       | Subnet         | Host IPs           |
|--------|------------------------|----------------|--------------------|
| MTN    | MTN ↔ CR1 & CR2        | 223.10.1.0/30 & 223.10.1.4/30 | 223.10.1.1–1.2 & 1.5–1.6 |
| Airtel | Airtel ↔ CR1 & CR2     | 223.10.1.8/30 & 223.10.1.12/30 | 223.10.1.9–1.10 & 1.13–1.14 |

> Both providers are configured for failover, with CR1/CR2 acting as redundant core routers.
* **eBGP** to both ISPs with local‑preference to MTN (primary) and Airtel (secondary).
* NAT outside on both edge routers

---

## 📡 Wireless Design

* Two lightweight APs per department (approx. 1 AP per 25 users).
* Separate SSID per VLAN with WPA2‑PSK.
* Unique SSID per department, VLAN-mapped for user segmentation.


---

## 🔐 Security Highlights

- **Finance & Accounts VLAN** has Access Control Lists (ACLs) to restrict traffic to authorized services only.
- Devices support **SSH v2** for remote access with RSA encryption.
- All wireless SSIDs use **WPA2/WPA3-Enterprise**.
- Server room isolated with **restricted access** and monitored.

---
## 🌐 Services Configured

- **DHCP**: Auto-assignment of IPs per VLAN.
- **DNS**: Local name resolution, A record created for `www.eduquity.com`.
- **Web Server**: Internal Apache server hosted in the Server Room subnet, running on port (80/443).

---


## 🧑‍💻 Remote Management

- **SSH Access** has been configured on all **Core Routers** and **Multilayer Switches**.
- Encrypted using **RSA key pairs** for secure remote login.
- **Username/password authentication** is enabled.
- Up to **16 simultaneous console login sessions** allowed for administrative access.
- SSH version 2 explicitly enforced for enhanced security.


---

## ✅ Testing & Validation Checklist

* [ ] VLAN & trunk verification (`show vlan brief`, `show interfaces trunk`)
* [ ] Wireless SSID broadcast and authentication
* [ ] Inter‑VLAN routing (`ping` across VLANs)
* [ ] OSPF neighbourship & route tables
* [ ] HSRP failover tests (toggle primary distro/core)
* [ ] ISP failover (shutdown MTN link, verify Airtel)
* [ ] DNS A‑record resolution (`nslookup intraweb.company.local`)
* [ ] Web access from each VLAN

---

## ⬇️ Repository Contents

| File                 | Description                                   |
| -------------------- | --------------------------------------------- |
| `NetworkDesign.pkt`  | Complete Packet Tracer topology & configs     |
| `README.md`          | This documentation                            |
| `diagrams/`          | Visio/PNG versions of topology diagrams       |
| `configs/`           | Folder containing final device configurations |

---

## 📌 How to Use

1. Clone this repo.
2. Open **CompanyNetwork.pkt** in Cisco Packet Tracer 8.2 or later.
3. Follow the validation checklist to observe expected behaviour.
4. Modify device configs under `configs/` as needed.

> *Happy labbing!*  Feel free to raise issues or submit PRs for improvements.

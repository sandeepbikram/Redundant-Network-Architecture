# 🏢 Enterprise Network Design – Cisco Packet Tracer Project
![Network Topology](https://github.com/user-attachments/assets/4f06ac1b-59b5-4987-b80c-016e0cede398)

## 📘 Project Description

This project simulates the planning and implementation of a scalable, secure, and redundant enterprise network using **Cisco Packet Tracer**. Designed for a startup company with **750 employees**, the network spans **three floors** and supports **six departments** with both wired and wireless connectivity.

The architecture is based on a **3-tier hierarchical model** (Core, Distribution, Access) and integrates **two Internet Service Providers** (MTN and Airtel) to ensure redundancy and high availability.

### 🔧 Key Features:
- Departmental segmentation using **VLANs**.
- **Subnetting** of the network as per requirements.
- Implementation of **OSPF routing protocol**.
- **Wireless access points** configured for each department.
- **Remote management** enabled via secure **SSH**.
- **Extra security** enforced for the Finance and Accounts department.
- Deployment of internal **Web**, **DHCP** and **DNS servers**.
- **Failover and redundancy** tested with dual ISPs.
- Complete **connectivity and communication** testing across all departments.

This project demonstrates practical enterprise networking concepts such as hierarchical design, redundancy, access control, subnetting, and service deployment—ideal for academic use, certification preparation, or portfolio demonstration.

---

---

## 👨‍💻 Developed With
- **Cisco Packet Tracer** v8.x
- Cisco IOS CLI commands
- IPv4 addressing and subnetting
- OSPF routing, VLANs, ACLs
- SSH, DNS, DHCP, and HTTP services

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
| `Network Design.pkt` | Complete Packet Tracer topology & configs     |
| `README.md`          | Project Description                           |
| `Topology.png`       | Visio/PNG versions of topology diagrams       |
| `Config Files/`      | Folder containing final device configurations |

---

## 📌 How to Use

1. Clone this repo.
2. Open **CompanyNetwork.pkt** in Cisco Packet Tracer 8.2 or later.
3. Follow the validation checklist to observe expected behaviour.
4. Modify device configs under `Config Files/` as needed.

> *Happy labbing!*  Feel free to raise issues or submit ideas for improvements.

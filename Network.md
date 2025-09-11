# Network Design Part 1 – Network Design



[Assumptions](#assumptions) | [Network Design Diagrams and Justifications](#network-design-diagrams-and-justifications) | [WiFi Design](#wifi-design) | [Address Allocations](#address-allocations) | [Recommended Hardware](#recommended-hardware) | [Plan](./plan.md) | [Cloud Services](./cloud.md) | [Security](./security.md) | [Ethics](./ethics.md) | [Reflection](./reflection.md) | [Return to index](./README.md)

## Assumptions

HQ location: Brisbane

Branch designed: Perth (one branch; remaining branches follow the same pattern)

HQ staff: 70 (mix of admin, ICT, PMO, marketing)

Branch staff: 20 (electricians, engineers, supervisors, support)

Internet: Each site has one business broadband link (static public IP from ISP).

Redundancy:

HQ: two edge routers in active/standby with automatic failover (simple tracking + static route fallback).

Branch: single edge router (kept simple) but supports USB/LTE failover if added later.

Inter-site connectivity: Site-to-site IPsec VPN over the Internet (no MPLS assumed).

Segmentation (without VLANs): Separate physical switches per network (Servers / Staff / Guest / IoT) connected to different router interfaces.

Addressing masks allowed: /16 or /24 only (per project rules).


## Network Design Diagrams and Justifications

HQ Logical (no VLANs; separate interfaces/switches)

Hq network design,

<img width="768" height="182" alt="hq_network_design.png" src=https://github.com/yeshwanthsai02-max/coit20246-2025-t2-project-bne-project-sumith-saiganesh/blob/main/hq_network_design.png//>


Branch network design


<img width="768" height="261" alt="Branch_Network_Design.png" src=https://github.com/yeshwanthsai02-max/coit20246-2025-t2-project-bne-project-sumith-saiganesh/blob/main/Branch_Network_Design.png//>



## WiFi Design

SSIDs:

 Truelec-Staff (WPA3-Personal, strong passphrase; 5 GHz preferred; band-steering on)

 Truelec-Guest (Open + captive portal OR WPA2-Personal; client isolation on; rate-limit e.g., 5 Mbps/user)

Channels:

 2.4 GHz: channels 1/6/11 only; 20 MHz width

 5 GHz: 40 MHz width; auto-select DFS if allowed

Power: Start at Low/Medium, avoid overlap; survey and adjust

 AP count: ~1 AP per 300–400 m² or per 25–30 users; start with 3–4 at HQ floor, 1–2 at branch; refine after a simple site survey

 Roaming: 802.11k/v/r enabled if supported by APs and clients

Security: Disable WPS; block peer-to-peer on Guest; allow only required outbound ports from Guest to Internet

## Address Allocations

## 4.1.3 IP Addressing Requirements
 

### HQ – Brisbane

| Network / Purpose | Subnet (/24)   | Gateway (.1) | DHCP Pool               | Notes                              |
|-------------------|----------------|--------------|-------------------------|------------------------------------|
| HQ-Servers        | 13.10.1.0/16   | 13.10.1.1    | 13.10.1.100 – 13.10.1.200 | Static reservations for servers    |
| HQ-Staff          | 13.10.10.0/16  | 13.10.10.1   | 13.10.10.50 – 13.10.10.220 | PCs, printers, AP uplinks          |
| HQ-Guest Wi-Fi    | 13.10.20.0/16| 13.10.20.1   | 58.10.20.50 – 58.10.20.250 | Internet-only, no internal access  |
| HQ-IoT/CCTV       | 14.10.30.0/16  | 14.10.30.1   | 06.10.30.50 – 06.10.30.200 | Cameras, RFID, IoT devices         |
| HQ-Mgmt (optional)| 14.0.40.0/16 | 14.10.40.1   | —                       | IoT devices, CCTV cameras      |

---

### Branch – Perth

| Network / Purpose | Subnet (/24)   | Gateway (.1) | DHCP Pool               | Notes                              |
|-------------------|----------------|--------------|-------------------------|------------------------------------|
| BR-Servers        | 13.20.1.0/16   | 13.20.1.1    | 13.20.1.100 – 13.20.1.200 | For local servers        |
| BR-Staff          | 13.20.10.0/16  | 13.20.10.1   | 13.20.10.50 – 13.20.10.220 | Staff PCs and printers             |
| BR-Guest Wi-Fi    | 14.20.20.0/16  | 14.20.20.1   | 14.20.20.50 – 14.20.20.250 | Guest network, internet access            |
| BR-IoT/CCTV       | 14.20.30.0/16  | 14.20.30.1   | 14.20.30.50 – 14.20.30.200 | IoT devices, CCTV cameras           |

---

### VPN & Routing Notes
- **Static Routes:**  
  - HQ edge routers → `78.20.0.0/16` via VPN tunnel  
  - Branch router → `78.10.0.0/16` via VPN tunnel  
- **NAT Rules:**  
  - NAT **only** for Internet-bound traffic.  
  - No NAT inside the VPN.  
- **Gateway convention:** Always use `.1` for router interfaces.  
- **Reservations:** `.2 – .20` reserved for infrastructure (APs, printers, NVRs).  
- **DHCP DNS:** HQ DNS server (if deployed) or fallback to public resolvers (e.g., `1.1.1.1`, `8.8.8.8`).  


## Recommended Hardware

## 4.1.2 Hardware Requirements

The following hardware is recommended for Truelec’s HQ (Brisbane) and Branch (Perth).  
We selected business-grade but cost-effective equipment suitable for ~70 users at HQ and ~20 at Branch.  
All prices are approximate AUD (from Australian suppliers, as of 2025).

---

### Routers / Firewalls
- **HQ Edge Routers (x2 for redundancy)**
  - Cisco ISR 4331 or Ubiquiti UniFi Dream Machine Pro SE
  - Minimum specs: Dual WAN ports, VPN/IPsec hardware acceleration, support for 100+ users
  - Approx. AUD $1,200 each  
  - Example: [[[(https://store.ui.com/us/en/collections/unifi-routing)(https://store.ui.com/us/en/collections/unifi-routing)]](https://store.ui.com/us/en/collections/unifi-routing)

- **Branch Router (x1 per site)**
  - Ubiquiti UniFi Security Gateway Pro 4
  - Specs: Dual WAN, supports 50+ users, VPN capable
  - Approx. AUD $700
  - Example Link:(https://store.ui.com/us/en/collections/unifi-routing)

---

### Core & Access Switches
- **HQ Core Distribution Switch**
  - Cisco Catalyst 2960X-24TS-L (24-port Gigabit + 4 SFP uplinks)
  - Approx. AUD $2,500  

- **HQ Access Switches (Servers, Staff, Guest, IoT)**
  - 4 × Cisco Catalyst 2960X-24TS-L (24-port each)
  - Approx. AUD $2,200 each  

- **Branch Switches**
  - 4 × Ubiquiti UniFi Switch 24 PoE
  - Approx. AUD $800 each  

---

### Wireless Access Points
- **HQ Wi-Fi APs**
  - 6 × Ubiquiti UniFi U6-LR Wi-Fi 6 AP
  - Dual band 2.4/5 GHz, WPA3, up to 300 users
  - Approx. AUD $300 each  

- **Branch Wi-Fi APs**
  - 2 × Ubiquiti UniFi U6-LR Wi-Fi 6 AP
  - Approx. AUD $300 each  

---

### Servers
- **HQ Servers**
  - Dell PowerEdge T440 Tower Server (for CRM, HR, Accounting, File storage)
  - Specs: Intel Xeon Silver, 32 GB RAM, RAID 5 with 4 × 2 TB HDD, dual PSU
  - Approx. AUD $5,500  

- **Branch Server**
  - Dell PowerEdge T150 Tower Server
  - Specs: Intel Xeon E-2300, 16 GB RAM, 2 × 2 TB HDD
  - Approx. AUD $2,200  

---

### IoT & CCTV
- **HQ Core Distribution Switch**
  - Hikvision DS-7732NI-I4/16P 32-channel NVR
  - Approx. AUD $2,500  

- **Branch CCTV NVR**
  - Hikvision DS-7608NI-I2/8P 8-channel NVR
  - Approx. AUD $800  

- **RFID / IoT Sensors**
  - HID Signo Reader 20 contactless card readers
  - Approx. AUD $300 each  

---

### Summary of Estimated Costs (AUD)
| Category        | HQ (Brisbane) | Branch (Perth) | Notes                        |
|-----------------|---------------|----------------|------------------------------|
| Routers         | $2,400        | $700           | Dual routers for redundancy at HQ          |
| Switches        | $11,300       | $3,200         | 4 switches at HQ, 4 at Branch  |
| Wi-Fi APs       | $1,800        | $600           | Wi-Fi 6 coverage at both sites            |
| Servers         | $5,500        | $2,200         | Server solutions for application and file storage|
| CCTV + IoT      | $1,500        | $800           |NVRs and RFID sensors for security               |
| **Total**       | **$22,500**   | **$7,500**     | Total estimated cost: ~AUD $30,000          |

---

### Justification
- **Cisco Catalyst switches** →Reliable, highly available, and widely supported enterprise-grade switches for HQ.  
- **Ubiquiti UniFi APs** → Affordable Wi-Fi 6 technology offering high-speed, reliable wireless access with cloud management capabilities..  
- **Dell PowerEdge servers** →Redundant enterprise-class servers for applications and storage, ensuring high uptime.  
- **Redundant HQ routers** → Ensures high WAN availability and failover capabilities in case of router failure.  
- **CCTV + IoT** → Provides comprehensive surveillance and access control solutions to safeguard both physical infrastructure and sensitive data.

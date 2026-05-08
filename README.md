# ARP Spoofing Detection and Firewall Configuration

> **Academic Assessment** | Computer Networking and Infrastructure
> London South Bank University | Student ID: 4126385

---

## Overview

This project covers two practical networking security topics:

**Part A** — ARP Spoofing Detection and Mitigation using Wireshark
and Cisco Packet Tracer

**Part B** — Firewall Configuration and Network Protection using
Cisco Packet Tracer ACLs

---

## Part A — ARP Spoofing

### What is ARP Spoofing
ARP is a stateless protocol that maps IP addresses to MAC addresses
in a LAN. ARP spoofing exploits this by sending fake ARP replies to
poison ARP cache tables, redirecting traffic through the attacker.

### Attack Types
| Type | Description |
|------|-------------|
| One-way spoofing | Only host device cache is poisoned |
| Two-way spoofing | Both router and host cache are poisoned (full MITM) |

### Impact on CIA Triad
| Principle | Impact |
|-----------|--------|
| Confidentiality | Attacker captures sensitive traffic via MITM |
| Integrity | Packets modified before forwarding to recipient |
| Availability | Packets dropped causing DoS or connection timeouts |

### Detection with Wireshark
- Applied ARP display filter to isolate ARP traffic
- Added source and target IP to MAC columns
- Identified duplicate IP to MAC mappings
- Detected unsolicited ARP replies sent every 2-3 seconds
- Used Expert Info system to flag suspicious activity

### Spoofing Indicators Observed
- Router IP 10.0.0.100 appeared with two MAC addresses
- PC1 IP 10.0.0.2 appeared with two MAC addresses
- Attacker MAC aa:bb:cc:dd:ee:ff claimed both IPs
- Unsolicited replies sent without prior ARP requests
- Selective two-way spoofing — PC0 was not targeted

### Countermeasures

| Method | Description |
|--------|-------------|
| Static ARP | Manual IP to MAC mappings — good for small networks |
| Dynamic ARP Inspection (DAI) | Layer 2 switch feature validating ARP against DHCP snooping |
| 802.1X NAC | Port-based network access control with authentication |

### DAI vs Static ARP Comparison

| Feature | DAI | Static ARP |
|---------|-----|------------|
| Configuration | Automatic via DHCP snooping | Manual per device |
| Scalability | High | Low |
| Effectiveness | Network-wide | Device-level only |
| Complexity | Switch configuration required | Per-device configuration |

---

## Part B — Firewall Configuration

### Network Topology
| Network | Subnet |
|---------|--------|
| LAN1 | 192.168.0.0/24 |
| LAN2 | 192.168.10.0/24 |
| Serial link | 192.168.1.0/30 |

### ACL Policy — LAN2_FILTER on Router1
Extended ACL applied inbound on FastEthernet0/0 (LAN2 interface):

| Traffic | Source | Destination | Action |
|---------|--------|-------------|--------|
| ICMP outbound | PC3 (192.168.10.2) | Any | Block |
| FTP outbound | PC3 (192.168.10.2) | Any | Block |
| ICMP inbound | Any | PC3 | Allow |
| HTTP | Any | Any | Allow |
| DNS | Any | Any | Allow |

### Test Results

| Test | Result |
|------|--------|
| PC3 ping to PC0 | Blocked |
| PC0 ping to PC3 | Allowed |
| PC3 FTP to LAN1 server | Blocked |
| PC3 HTTP to LAN1 server | Allowed |
| PC3 DNS lookup | Allowed |

### Firewall Types Comparison

| Type | Layer | Method |
|------|-------|--------|
| Packet filtering | 3 and 4 | Checks headers only — stateless |
| Stateful inspection | 3 and 4 | Tracks connection state table |
| Application layer | 7 | Deep packet inspection — acts as proxy |

### Default Deny vs Default Allow

| Feature | Default Deny | Default Allow |
|---------|-------------|---------------|
| Approach | Block all, allow explicitly | Allow all, block explicitly |
| Unknown threats | Blocked by default | Vulnerable |
| Complexity | Higher | Lower |
| Security | Higher | Lower |

> Default deny is recommended as it blocks unknown threats by default.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | ARP packet capture and analysis |
| Cisco Packet Tracer | Network simulation and ACL configuration |

---

## Repository Files

| File | Description |
|------|-------------|
| `README.md` | This file |
| `arp-topology.pkt` | Cisco Packet Tracer file — ARP spoofing topology |
| `firewall-topology.pkt` | Cisco Packet Tracer file — firewall ACL topology |
| `acl-commands.txt` | ACL commands used on Router1 |

---

## References

- Whalen, S. (2001) ARP Spoofing: An Introduction
- Cisco (2025) Configuring Dynamic ARP Inspection
- Imperva (n.d.) What is ARP Spoofing
- Cisco (n.d.) What Is a Firewall?
- Palo Alto Networks (n.d.) What Are Firewall Rules?

---

> **Disclaimer:** All activities were conducted within a controlled
> virtual lab environment using Cisco Packet Tracer and Wireshark
> for educational purposes only as part of a university assessment.

# SOC+ Hybrid Lab — Network Setup & Coordination

This repository documents my contribution to the **SOC+ Hybrid Lab** project:
building and configuring the underlying network infrastructure that the rest
of the lab (Wazuh SIEM, victim machine, Kali attack machine) was deployed on
top of, and coordinating connectivity across a remote team.

This repository does **not** contain virtual machine images or operating
systems — only documentation, setup steps, and screenshots demonstrating
the working network.

---

## 📌 Objectives

- Build an isolated, controlled local network to simulate an internal
  organizational network for the SOC lab.
- Deploy and configure **pfSense** as the network gateway/firewall.
- Extend that network to a distributed team using a VPN mesh overlay so
  everyone could operate as if on one shared LAN.
- Verify connectivity across all team machines before SOC tools were
  deployed on the network.

## 🏗️ Architecture

A **hybrid network design**: a locally virtualized network for the SOC lab,
extended to remote teammates over a VPN mesh overlay.

```
                 ┌───────────────────────────────┐
                 │        Tailscale (VPN)        │
                 │  mesh overlay — remote hosts   │
                 └────────────────┬──────────────┘
                                  │
        ┌─────────────────────────────────────────────┐
        │           VMnet2 (host-only network)          │
        │                                                │
        │            ┌──────────────────┐               │
        │            │     pfSense      │               │
        │            │  Gateway/Firewall│               │
        │            │  WAN + LAN       │               │
        │            └──────────────────┘               │
        │                                                │
        │   (Wazuh, victim, and Kali machines deployed   │
        │    by teammates on top of this network)         │
        └─────────────────────────────────────────────┘
```

- **VMnet2 (VMware host-only network):** an isolated private network so lab
  VMs can talk to each other without being exposed to the internet —
  simulating an internal organizational network.
- **pfSense:** the gateway/firewall for the lab network. WAN and LAN
  interfaces were assigned, LAN settings configured, and connectivity
  between interfaces verified.
- **Tailscale:** VMnet2 alone only connects VMs on a single host, so since
  each team member worked from a different laptop/location, Tailscale was
  used to create a secure VPN mesh so every machine could reach every other
  machine as if on the same LAN.

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| VMware Workstation | Hosting the local virtual network |
| pfSense | Network gateway / firewall |
| Tailscale | VPN mesh for remote team connectivity |

## 📂 Repository Structure

```
.
├── README.md                  → project overview (this file)
├── INSTALLATION.md            → step-by-step network setup guide
├── docs/                       → full report
│   └── SOC_Hybrid_Lab_Report.docx
└── screenshots/                 → evidence of working setup
    ├── 01-pfsense-console-boot.png
    ├── 02-pfsense-web-dashboard.png
    └── 03-tailscale-admin-console.png
```

## 🚀 Quick Start

See [`INSTALLATION.md`](./INSTALLATION.md) for the full setup steps — from
creating the VMnet2 network through to verifying team-wide connectivity.

## 📸 Screenshots

See the [`screenshots/`](./screenshots) folder: the pfSense console after
boot-up (WAN/LAN interface assignment), the pfSense web dashboard, and the
Tailscale admin console showing all team machines connected to the shared
tailnet.

## 📄 Documentation

Full write-up of this work is in
[`docs/SOC_Hybrid_Lab_Report.docx`](./docs/SOC_Hybrid_Lab_Report.docx).

## ⚠️ Security Note

No passwords, API keys, VPN auth keys, or other sensitive credentials are
included anywhere in this repository.

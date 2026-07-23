# Installation & Deployment Steps

These steps cover building the network foundation of the SOC+ Hybrid Lab.
Complete this section before deploying Wazuh, the victim machine, or the
Kali attack machine on top of it.

## 1. Prerequisites

- VMware Workstation (or compatible hypervisor) installed on the host machine.
- pfSense installer ISO.
- A [Tailscale](https://tailscale.com/) account (free tier is sufficient).
- One VM per team role (Wazuh server, victim machine, Kali attack machine),
  sized per your available host resources.

## 2. Create the Local Network (VMnet2)

1. Open **VMware Workstation → Edit → Virtual Network Editor**.
2. Add/select **VMnet2** and set its type to **Host-only** (no internet
   bridging), so lab VMs can only reach each other, not the public internet.
3. Attach each lab VM's network adapter to **VMnet2**.

## 3. Deploy and Configure pfSense

1. Create a new VM, attach two network adapters:
   - **WAN** — used for the pfSense management/uplink interface.
   - **LAN** — attached to **VMnet2**, this is the internal lab network.
2. Install pfSense from the ISO.
3. During initial setup, assign the WAN and LAN interfaces as prompted.
4. Configure the LAN interface with a private subnet (e.g. `192.168.56.1/24`
   — adjust to your environment).
5. From another VM on the LAN, confirm you can reach the pfSense LAN IP and
   log into the web administration dashboard.
6. Verify uptime, interface assignment, and system version from the pfSense
   dashboard (see `screenshots/02-pfsense-web-dashboard.png`).

## 4. Extend Connectivity with Tailscale

VMnet2 is host-only, so it only connects VMs running on a single physical
machine. To let a distributed team collaborate, each team member installs
Tailscale on their host and/or relevant VMs.

1. Each team member creates/joins the shared Tailscale network (tailnet):
   ```
   curl -fsSL https://tailscale.com/install.sh | sh
   sudo tailscale up
   ```
2. Authenticate each device using the tailnet's SSO/login when prompted.
3. From the [Tailscale admin console](https://login.tailscale.com/admin/machines),
   confirm all team machines appear and show as connected.
4. Test connectivity between two team members' machines using `ping` or
   `tailscale ping <device-name>`.

## 5. Verify Team-Wide Connectivity

- Confirm every team member's machine can reach every other machine over
  the tailnet.
- Confirm every VM on VMnet2 can reach the pfSense LAN gateway.
- Troubleshoot and resolve any connectivity issues before proceeding.

Once this is verified, the network foundation is ready for tools to be
deployed on top of it.

# 01 - SIEM Homelab (Wazuh) - *Flagship, In Progress*

## Objective
Build an isolated detection lab where I attack a vulnerable target from Kali and detect those attacks in a SIEM - the core loop of SOC-analyst work. The goal is the full **attack -> detect -> defend** story, not just standing up tools.

## Hardware / Software
- **Host:** Pop!_OS 24.04, Ryzen 7 5700X, 16GB RAM
- **Hypervisor:** KVM/QEMU + libvirt + virt-manager
- **SIEM:** Wazuh 4.9.0 (Docker single-node) - Manager, Indexer, Dashboard
- **Attacker:** Kali 2026.2 VM
- **Target:** Metasploitable 2 VM
- **Host hardening:** Mullvad VPN (kill switch, DNS blockers)

## Build Process
1. Hardened the host and installed the KVM/QEMU virtualization stack.
2. Built an **isolated virtual network** (isolated-lab, 10.10.10.0/24, no forwarding) - sealed from the internet and home LAN. Verified containment: Kali cannot reach the internet (ping 8.8.8.8 -> "Network is unreachable").
3. Stood up **Wazuh** via Docker (dashboard on port 8443; changed admin credentials across indexer, dashboard, and Manager API).
4. Imported and updated the **Kali** attacker VM; snapshotted a clean baseline; sealed it on the isolated network.
5. Imported **Metasploitable 2** (converted vmdk -> qcow2). Fixed boot on KVM: machine type pc (i440fx) for native IDE, IDE disk bus, e1000 NIC (the 2010 kernel has no virtio driver). Snapshotted clean; kept on the isolated network only.

## Results
- Working, sealed lab: attacker + target on an isolated network, both snapshotted, Wazuh running and hardened.
- Containment verified by test.

## Architecture: The Rig Split
The main rig currently wears two hats: (1) daily-driver gaming/work machine, and (2) host for the isolated security-lab VMs. The lab lives in walled-off VMs on its own virtual network (10.10.10.0/24), completely separate from the host and home LAN. This is a **purple learning lab ("dojo")**: attack own vulnerable targets (Kali -> Metasploitable) and detect those attacks in Wazuh. A training sandbox, not a production SOC.

## Current Status
- **Lab core:** Built and working — isolated network, Wazuh running, attacker + target VMs snapshotted.
- **Detection:** Currently **manual** (run attack, screenshot, log by hand). The automated detection layer (Suricata -> Wazuh) is **not yet built**. The attack->detect->defend story is not complete — that's the next build.

## Staged Roadmap

| Stage | What | Status |
| --- | --- | --- |
| 1 | **Suricata detection layer** — install Suricata (network IDS) on the host, watching the virbr-lab bridge. Feed eve.json into Wazuh. Run attack, confirm Wazuh caught it, write/tune detection rule, document. A missed detection = a new rule (detection engineering). | **In Progress / Next** |
| 2 | **CYD honeypot** — flash dead-touchscreen CYD with ESP32-Honeypot firmware (dagnazty/7h30th3r0n3), webhook alerts into SIEM. Add a Wazuh agent on a real endpoint. | Planned |
| 3 | **Dedicated blue rig** — separate cheap (~$130) used office PC as an always-on Wazuh / Security Onion box, independent of the gaming rig. | Planned / Future |
| 4 | **Network visibility** — managed switch with port mirroring or pfSense/OPNsense firewall to monitor real network traffic. The full blue detection network ("watchtower"). | Planned / Future |
| 5 | **Enterprise sim** — Active Directory, multiple VMs, VLANs. | Planned / Long-term |

**Discipline:** skills first on existing hardware. Dedicated hardware (separate blue rig, networking gear) earns its place later, once the purple dojo is mastered. The near-term direction is finishing detection on the rig already owned — not buying gear.

## Defensive Takeaways
- Network isolation and verified containment are the foundation of safe offensive testing.
- A SIEM is only useful once it has visibility — standing it up is step one, not the finish line.
- Detection engineering is driven by misses: what your SIEM does not catch defines the rules you need to write.
- The attack->detect->defend loop is the core skill. The lab exists to practice that loop, not to collect tools.

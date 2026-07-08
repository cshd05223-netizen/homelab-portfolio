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

## Current Status / Next
Detection is currently **manual** (run attack, screenshot, log by hand). The next build is the automated detection layer: **Suricata (network IDS) on the host bridge -> Wazuh**, a neutral defender vantage on all lab traffic. Then the flagship exercise: run an attack, confirm Wazuh caught it, write/tune the detection rule, document. A missed detection is the most valuable outcome - it becomes a new rule (detection engineering).

## Defensive Takeaways
- Network isolation and verified containment are the foundation of safe offensive testing.
- A SIEM is only useful once it has visibility - standing it up is step one, not the finish line.
- Detection engineering is driven by misses: what your SIEM does not catch defines the rules you need to write.

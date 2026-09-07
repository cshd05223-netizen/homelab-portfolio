# 01 - SIEM Homelab (Wazuh + Suricata) - Flagship - COMPLETE

## Objective

Build an isolated detection lab where I attack a vulnerable target from Kali and detect those attacks in a SIEM - the core loop of SOC-analyst work. The goal is the full **attack → detect → defend** story, not just standing up tools.

## Hardware / Software

- **Host:** Pop!_OS 24.04, Ryzen 7 5700X, 16GB RAM
- **Hypervisor:** KVM/QEMU + libvirt + virt-manager
- **SIEM:** Wazuh 4.9.0 (Docker single-node) - Manager, Indexer, Dashboard
- **Network IDS:** Suricata 7.0.3 (on the host, watching the lab bridge)
- **Attacker:** Kali 2026.2 VM (10.10.10.99)
- **Target:** Metasploitable 2 VM (10.10.10.30)
- **Host hardening:** Mullvad VPN (kill switch, DNS blockers)

## Build Process

1. Hardened the host and installed the KVM/QEMU virtualization stack.
2. Built an isolated virtual network (`isolated-lab`, 10.10.10.0/24, no forwarding) - sealed from the internet and home LAN. **Verified containment:** Kali cannot reach the internet (`ping 8.8.8.8` → "Network is unreachable").
3. Stood up Wazuh via Docker (dashboard on port 8443; changed admin credentials across indexer, dashboard, and Manager API).
4. Imported and updated the Kali attacker VM; snapshotted a clean baseline; sealed it on the isolated network.
5. Imported Metasploitable 2 (converted vmdk → qcow2). Fixed boot on KVM: machine type `pc` (i440fx) for native IDE, IDE disk bus, e1000 NIC (the 2010-era kernel has no virtio driver). Snapshotted clean; kept on the isolated network only.
6. Installed Suricata 7.0.3 on the host, set `HOME_NET` to 10.10.10.0/24, pointed af-packet at the lab bridge, and loaded the ET Open ruleset (~51,900 rules). Confirmed `eve.json` output.
7. Mounted Suricata's log output into the Wazuh manager container and added a localfile block so Wazuh ingests Suricata events.

## The Detection Pipeline (end to end)

```
Kali (attacker) → nmap scan → crosses the isolated network
   → Suricata (host, on the lab bridge) detects it → writes eve.json
   → Filebeat ships it → Wazuh Indexer stores it
   → Wazuh Dashboard displays the alert
```

## Results - VERIFIED

- Ran a loud scan from Kali: `sudo nmap -sS -sV -p- --min-rate 1000 10.10.10.30`.
- Suricata detected it and generated **ET SCAN** alerts.
- Alerts flowed through Filebeat into the day's `wazuh-alerts` index and displayed in the Wazuh dashboard (Threat Hunting).
- Confirmed the alerts in the dashboard: multiple `Suricata: Alert - ET SCAN` entries (Wazuh rule 86601), with the **source IP correctly attributed to the Kali attacker (10.10.10.99)** and the target as Metasploitable (10.10.10.30).
- Practiced the analyst motion: expanded an alert, read the `data.` fields, and reconstructed who attacked what.

**The attack → detect → defend loop is complete and demonstrated with real evidence.**

## Debugging Story: The Empty Dashboard (real troubleshooting)

For a while the pipeline *ingested* data but the dashboard showed nothing - a genuine incident-response-style debug worth documenting:

- **Symptom:** `grep -c suricata` on the manager's `alerts.json` returned a positive count (ingestion working), but the dashboard's Threat Hunting view was empty.
- **Isolated the layer:** queried the indexer directly (`_cat/indices`) and found an old alerts index but **no index for the current day** - so data was landing in the file but not reaching the index the dashboard reads.
- **Root cause:** `filebeat test output` returned **401 Unauthorized**. The Wazuh admin password had been changed in a previous session for the dashboard login, but Filebeat's credential (the `INDEXER_PASSWORD` env var in `docker-compose.yml`, which regenerates `/etc/filebeat/filebeat.yml` on every container start) was never updated - it still held the default `SecretPassword`. Editing the file inside the container didn't persist because the entrypoint rewrites it from the env var on restart.
- **Fix:** updated `INDEXER_PASSWORD` in `docker-compose.yml` (manager + dashboard), then recreated the stack with `docker compose down && docker compose up -d` (a plain restart doesn't re-read compose). `filebeat test output` → **"talk to server... OK"**, the current-day alerts index populated, and the alerts appeared in the dashboard.
- **Last-mile display gotcha:** the dashboard also hides scan alerts by default via a `rule.level: 0 to 6` filter and a narrow "Last 1 minute" time range - removing that filter and widening the range surfaced everything.

**Why this matters:** this is real detection-engineering / IR work - symptom → isolate the failing layer → find root cause → fix at the source → verify. A pipeline that broke and got fixed is a stronger story than one that worked first try.

## Architecture: The Rig Split

The main rig wears two hats: (1) daily-driver gaming/work machine, and (2) host for the isolated security-lab VMs. The lab lives in walled-off VMs on its own virtual network (10.10.10.0/24), completely separate from the host and home LAN. This is a purple learning lab ("dojo"): attack own vulnerable targets (Kali → Metasploitable) and detect those attacks in Wazuh. A training sandbox, not a production SOC.

## Recovery / Runbook

- **Start the SIEM:** `cd ~/Homelab/wazuh-docker/single-node && docker compose up -d` (no `sudo` on the compose command on this host). Containers report "Started" immediately but take ~2–3 minutes to be reachable.
- **VMs** do not auto-start - bring them up in virt-manager (Metasploitable, then Kali).
- **Verify pipeline:** `filebeat test output` should end in `talk to server... OK`; check the current-day `wazuh-alerts` index exists.

## Defensive Takeaways

- Network isolation and verified containment are the foundation of safe offensive testing.
- A SIEM is only useful once it has visibility - standing it up is step one, not the finish line.
- Detection engineering is driven by misses and breaks: what your SIEM does *not* catch (or *stops* catching) defines the work.
- Credentials that live in multiple places (dashboard, indexer, Filebeat, compose env) must all agree - a single stale copy silently breaks the whole pipeline.
- The attack → detect → defend loop is the core skill. The lab exists to practice that loop, not to collect tools.

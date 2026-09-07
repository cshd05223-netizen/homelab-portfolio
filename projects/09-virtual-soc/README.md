# 09 — Virtual SOC (Endpoint Detection + Analyst Workflow) — In Progress

## Objective

Build a full SOC analyst workflow entirely inside the existing virtual lab. Phase 1 built NETWORK detection (Suricata → Wazuh). This project adds the HOST/endpoint layer on top, then bolts on the actual analyst casework (ticketing) that makes it a real SOC instead of just a dashboard.

The one-sentence deliverable: "I ran the full analyst workflow, detection, investigation, ticketing, resolution, on endpoints I monitored, and caught my own Meterpreter session with a rule I wrote."

## Why virtual instead of physical

The Home Network Watcher was re-sequenced to PARKED because it needs physical network access (router/switch downstairs, can't run ethernet or add a managed switch, house rule = don't break anything). Going physical teaches nothing more than going deep virtually: same agent, same telemetry, same investigation. Virtual-for-now is the faster path to good, not a compromise.

## Two detection layers

- **Suricata** = NETWORK detection (hallway camera, watches traffic cross the wire). Built in Phase 1.
- **Wazuh agent** = HOST/endpoint detection (camera inside the machine: logins, processes, files). This project.

## Build order (brick by brick)

1. **Windows VM** on the isolated lab network (10.10.10.0/24), snapshot clean. First real monitored endpoint. Win 10/11 eval ISO, free.
2. **Sysmon** (SwiftOnSecurity config), enriches Windows logging into process trees + command lines. This is what makes Meterpreter actually catchable.
3. **Wazuh agent**, MSI matching manager version (4.9.0), point at manager's lab-bridge IP over TCP 1514, register, confirm Active in dashboard Agents view.
4. **Tell Wazuh to read Sysmon**, add a localfile block to the agent's ossec.conf forwarding Microsoft-Windows-Sysmon/Operational.
5. **Verify pipeline BEFORE attacking**, do something benign but loggable (failed login / file create), confirm it lands in the dashboard. Missing alert later then means "rule gap," not "pipeline broken." Same discipline as the Filebeat 401 debug.
6. **Attack from Kali**, msfvenom payload / exploit module → catch a Meterpreter session → post-ex: getsystem, hashdump, migrate, drop a file.
7. **Hunt the telemetry in Wazuh**, each action throws Sysmon events (proc create, priv-esc, LSASS access, process-access). Anything done that Wazuh DIDN'T catch = a detection rule to write. Detection is driven by misses.
8. **TheHive ticketing**, bolt on once alerts are flowing. Alert fires → open ticket → investigate → write up → resolve/escalate → close. THIS makes it a real SOC workflow, not just a dashboard. Biggest resume differentiator.
9. **Iterate**, more attacks, tune rules, write detections for misses, build up cases.

## Splunk (later, not now)

Learn as a targeted skill (named on job reqs), but NOT now: too RAM-heavy for 16GB, and the workflow (detect → ticket → investigate → close) is tool-agnostic. Go deep on Wazuh first. Best free path = Splunk free training + Boss of the SOC (BOTS) datasets, no heavy local install.

## Trust-tier discipline

This lab is deliberately-attacked / disposable. Keep it on its own Wazuh brain, separate from anything trusted. Never share a SIEM with real/protected devices.

## Constraints

- All virtual, own hardware, no spend, no permission needed.
- Full dedicated session per brick, not a bolt-on.
- RAM upgrade 16→32GB is the only purchase that would help (down the road; want, not blocker).

## Status

In progress. Started Sep 2, 2026. Windows VM + Sysmon + Wazuh agent build is the current brick.

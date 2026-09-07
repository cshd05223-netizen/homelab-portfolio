# 09 — Virtual SOC (Endpoint Detection + Analyst Workflow)

*In progress*

## What this is

The real SOC build, done entirely inside the virtual lab I already have. Project 01 built the network detection layer (Suricata feeding Wazuh). This project adds the host and endpoint layer on top, then bolts on the analyst casework (ticketing) that turns it from a dashboard into an actual SOC workflow.

The one sentence version of what I'm building toward: *"I ran the full analyst workflow (detection, investigation, ticketing, resolution) on endpoints I monitored, and caught my own Meterpreter session with a rule I wrote."*

## Why virtual instead of physical

The Home Network Watcher got parked because it needs physical network access I don't have: the router and switch are downstairs, I can't run ethernet or add a managed switch, and the house rule is don't break anything. So instead of waiting on that, I'm going deep on the same learning inside the lab I already own.

Going physical teaches nothing new over going virtual here. Same agent, same telemetry, same investigation. Virtual is just the faster path to good, not a compromise.

## Two detection layers

- **Suricata** is network detection. The hallway camera. It watches traffic cross the wire. That's what project 01 built.
- **Wazuh agent** is host and endpoint detection. The camera inside the machine. It sees logins, processes, files. That's what this project adds.

## Build order

1. **Windows VM** on the isolated lab network (10.10.10.0/24), snapshot clean. First real monitored endpoint. Win 10/11 eval ISO, free.
2. **Sysmon** with the SwiftOnSecurity config. This enriches Windows logging into process trees and command lines, which is what actually makes Meterpreter catchable.
3. **Wazuh agent**, MSI matching the manager version (4.9.0), pointed at the manager's lab-bridge IP over TCP 1514, registered, confirmed Active in the dashboard Agents view.
4. **Point Wazuh at Sysmon** with a localfile block in the agent's ossec.conf forwarding Microsoft-Windows-Sysmon/Operational.
5. **Verify the pipeline before attacking.** Do something benign but loggable (a failed login or a file create) and confirm it lands in the dashboard. That way if an alert is missing later, I know it's a rule gap and not a broken pipeline. Same discipline as the Filebeat 401 debug in project 01.
6. **Attack from Kali.** msfvenom payload or exploit module, catch a Meterpreter session, then post-exploitation: getsystem, hashdump, migrate, drop a file.
7. **Hunt the telemetry in Wazuh.** Every action throws Sysmon events (process creation, privilege escalation, LSASS access, process access). Anything I did that Wazuh didn't catch is a detection rule I need to write. Detection is driven by misses.
8. **TheHive ticketing.** Once alerts are flowing, an alert fires, opens a ticket, I investigate, write it up, resolve or escalate, close it. This is the step that makes it a real SOC workflow instead of a dashboard, and it's the biggest resume differentiator.
9. **Iterate.** More attacks, tune rules, write detections for what I missed, build up cases.

## Splunk is later, not now

Splunk shows up on job postings, so I'll learn it as a targeted skill eventually. Not now. It's too RAM-heavy for my 16GB rig, and the actual workflow (detect, ticket, investigate, close) is tool-agnostic. Go deep on Wazuh first. When I do pick up Splunk, the free path is Splunk's own training plus the Boss of the SOC datasets, no heavy local install needed.

## Trust-tier discipline

This lab is deliberately attacked and disposable. It stays on its own Wazuh brain, separate from anything trusted. Never share a SIEM with real or protected devices.

## Constraints

- All virtual, on hardware I own, no spend, no permission needed.
- Full dedicated session per brick, not a bolt-on.
- The RAM upgrade from 16 to 32GB is the only purchase that would help, and it's a want, not a blocker.

## Status

Started Sep 2, 2026. The Windows VM plus Sysmon plus Wazuh agent build is the current brick.

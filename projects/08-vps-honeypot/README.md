# 08 — Internet-Facing SSH Honeypot on a VPS (Cowrie + Grafana) — Complete

## Objective

Deploy a real, internet-exposed SSH honeypot on a cloud VPS to capture live attacker behavior — credentials, commands, and pivot attempts — from actual internet threats, and visualize the data in a readable dashboard reachable securely from anywhere. This moves from a staged home lab (where I attack myself) to observing *real* adversaries, safely and legally.

## Hardware / Software

- **Host:** DigitalOcean droplet (Ubuntu 24.04 LTS, 2GB RAM, NYC region)
- **Honeypot:** Cowrie 3.0.12 (low-interaction SSH/Telnet honeypot)
- **Log pipeline:** Promtail → Loki (lightweight log aggregation, chosen over heavy Elasticsearch)
- **Dashboard:** Grafana (bound to a private Tailscale interface)
- **Private access:** Tailscale (real admin SSH + dashboard ride the tailnet, never the public internet)

## Architecture / Design Decisions

**Trust-tier isolation.** The honeypot is *meant* to be attacked, so it lives on its own disposable VPS with nothing of value on it and no path back to my home network. Worst case, the box is deleted and rebuilt — a decoy in a field, not a door into anything I care about.

**Low-interaction by design.** Cowrie emulates a fake shell rather than exposing a real OS. Attackers land in a puppet; they can't reach the real system, and pivot/relay attempts are captured but blocked. This gives strong intel-per-risk: it catches the overwhelming majority of internet attacks (bots) with almost no chance of the box itself being truly compromised.

**Public bait, private tools.** The only thing exposed to the internet is the honeypot's fake SSH on port 22. My *real* administrative SSH and the Grafana dashboard are bound to the Tailscale interface only — invisible to attackers, reachable by me from any of my devices.

**Lightweight stack.** Chose Loki + Promtail + Grafana over the classic Elasticsearch/Kibana ("ELK") stack because Elasticsearch is RAM-hungry (wants 2GB+ for itself) and would have destabilized a 2GB box. Loki is purpose-built for logs and sips resources — the right tool for the constraint, and Grafana is a widely-used, employable visualization skill.

## Build Process

1. **Provisioned the VPS** (Ubuntu 24.04), fully patched it, and rebooted into the current kernel before exposing anything.
2. **Hardened access first (before any bait went live):**
   - Generated an ed25519 SSH key; disabled password authentication entirely (key-only).
   - Created a non-root sudo user; disabled direct root SSH login.
   - Installed Tailscale and joined the box to my private tailnet.
   - Bound the real `sshd` to the Tailscale IP only, freeing public port 22 for the honeypot.
   - Verified private access still worked after every change before trusting it (never locked myself out).
3. **Installed Cowrie** in an isolated Python virtualenv under a dedicated, password-less `cowrie` user (least privilege — the honeypot process owns nothing important).
4. **Exposed the bait:** redirected incoming public port 22 → Cowrie's port 2222 with an iptables NAT rule, so attackers hitting the standard SSH port land in the honeypot while my real SSH stays private on Tailscale.
5. **Built the log pipeline:** Cowrie writes structured JSON → Promtail tails it and ships to Loki → Grafana queries Loki. Fixed a real permissions issue (Promtail couldn't traverse the cowrie user's locked-down home directory; solved with least-privilege group membership rather than opening permissions wide).
6. **Made everything reboot-proof:** Cowrie runs as a systemd service (auto-start + auto-restart); the iptables redirect is persisted; sshd/Tailscale survive restarts. The honeypot keeps catching attackers 24/7 with no manual intervention.
7. **Built a Grafana dashboard** (attacks over time, top attacker IPs, top passwords, top usernames, plus planned total-count/successful-login/pivot-target panels), reachable privately over Tailscale from any device.

## Results — Real Attacks Captured (within minutes of going live)

- **Credential-spray botnet:** logged in with guessed creds (e.g. `theo/theo`, `admin/ocarina`) and immediately attempted to pivot/relay through the box to a fixed external target (`62.210.131.144:2535`). Same actor recurred from multiple IPs in the `2.57.121.0/24` range, each time targeting the same relay destination — Cowrie captured the intent and blocked the relay every time. **Characterized this as a botnet using compromised hosts purely as proxies.**
- **RDP scanner hitting the wrong port:** a scanner spoke RDP (`mstshash=hello`) at the SSH honeypot; Cowrie rejected the malformed protocol in ~2ms. Recurred repeatedly and became the top source by volume.
- **Research crawler:** a `ZGrab SSH Survey` scanner (internet-wide research indexing) performed a full handshake — distinguishable from malicious bots by its banner and behavior.
- **Various bots** identified via **HASSH fingerprints** (a hash of how each SSH client negotiates the connection — identifies the tool even when the banner is spoofed).

## What I Learned

- **Hardening order matters:** secure your own access *before* exposing anything, and verify each change from a fresh session so a bad config never locks you out.
- **Least privilege throughout:** dedicated password-less service user, key-only login, no root SSH, services bound to private interfaces.
- **Right tool for the constraint:** chose Loki over Elasticsearch because the box's RAM couldn't support the heavier stack — matching the solution to the environment.
- **Real troubleshooting:** worked through a stale-credential auth failure, an Ubuntu `ssh.socket` override that ignored `ListenAddress`, a leftover process squatting on port 22, and a directory-traversal permissions issue in the log pipeline — symptom → isolate → root cause → fix → verify each time.
- **Threat characterization from live data:** correlated repeated events into an actor profile (same source range, same pivot target) — the core analyst motion of turning individual alerts into a pattern.
- **Why dashboards matter:** the pivoting botnet was easy to miss scrolling raw logs but jumps out instantly on a "top source" panel — the value of visualization over log-diving.

## Defensive Takeaways

- Internet-facing SSH is under constant automated attack — exposure is measured in *minutes*, not days.
- Low-interaction honeypots are a high-value, low-risk way to gather real threat intelligence; the isolation (disposable box, no path home, fake shell) keeps the blast radius tiny.
- Segmentation by trust level (deliberately-attacked assets kept away from trusted systems) is the discipline that makes running a honeypot safe.
- Capturing attacker *intent* (the pivot target, the credentials, the HASSH fingerprint) produces indicators a defender can act on — block the source range, flag the relay destination, fingerprint the tool.

## Roadmap (this project)

- **GeoIP map:** enrich events with MaxMind GeoLite2 so attacker locations plot on a Grafana world map.
- **Alerting to my agent:** wire high-signal events (successful interactive login, canary-file access, activity against the real box) to a real-time pager — deliberately *tuned* to avoid alert fatigue from constant bot noise (raw hits stay on the dashboard, only signal pages me).
- **Canary tokens:** plant realistic bait files (e.g. `passwords.txt`, `wallet.dat`) that silently flag when a human attacker opens them — separating real interactive attackers from automated spray.

## Status

Complete and live. Honeypot is internet-exposed, catching real attackers 24/7, reboot-proof, with a private Grafana dashboard reachable from anywhere over Tailscale. GeoIP map and alerting are the next enhancements.

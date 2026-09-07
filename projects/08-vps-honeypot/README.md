# 08 - Internet-Facing SSH Honeypot on a VPS (Cowrie + Grafana) - Complete

## Objective

Deploy a real, internet-exposed SSH honeypot on a cloud VPS to capture live attacker behavior - credentials, commands, and pivot attempts - from actual internet threats, and visualize the data in a readable dashboard reachable securely from anywhere. This moves from a staged home lab (where I attack my own targets) to observing *real* adversaries, safely and legally.

## Stack

- **Host:** DigitalOcean droplet `honey-pot-v2` (Ubuntu 24.04 LTS, 2GB RAM, NYC)
- **Honeypot:** Cowrie 3.0.12 (low-interaction SSH honeypot, runs as a dedicated unprivileged user)
- **Log pipeline:** Promtail 3.6.11 → Loki 3.7.4 (lightweight log aggregation, chosen over heavy Elasticsearch)
- **Dashboard:** Grafana 13.1.1, bound to a private Tailscale interface
- **Private access:** Tailscale (admin SSH + dashboard ride the tailnet, never the public internet)

## Architecture / Design Decisions

**Trust-tier isolation.** The honeypot is *meant* to be attacked, so it lives on its own disposable VPS with nothing of value on it and no path back to my home network. Worst case, the box is deleted and rebuilt - a decoy in a field, not a door into anything I care about.
**Low-interaction by design.** Cowrie emulates a fake shell rather than exposing a real OS. Attackers land in a puppet; they can't reach the real system, and pivot/relay attempts are captured but blocked. Strong intel-per-risk: catches the overwhelming majority of internet attacks (bots) with almost no chance of the box itself being truly compromised.
**Public bait, private tools.** The only thing exposed to the internet is the honeypot's fake SSH on port 22. Real administrative SSH and the Grafana dashboard are bound to the Tailscale interface only - invisible to attackers, reachable by me from any of my devices.
**Lightweight stack.** Chose Loki + Promtail + Grafana over Elasticsearch/Kibana ("ELK") because Elasticsearch is RAM-hungry (wants 2GB+ for itself) and would destabilize a 2GB box. Loki is purpose-built for logs and sips resources - the right tool for the constraint, and Grafana is a widely-used, employable visualization skill.

## Defense-in-Depth Access (three independent safety nets)

This is the core lesson of the project (see "Incident" below). Admin access is protected by **three independent doors**, any one of which alone is enough to get in:

1.  **Tailscale SSH** (`tailscale set --ssh`) - authenticates by identity over the tailnet, uses **no TCP port**, so no firewall/NAT rule can ever block it.
2.  **Real sshd on a high port (22222)** - moved off port 22 so the honeypot's port-22 redirect never touches it; key-only, hardened.
3.  **iptables redirect scoped to the public interface** (`-i eth0`) - the redirect that sends attackers into the honeypot is bound to the public NIC only, so it physically cannot intercept Tailscale traffic (which rides a different interface).

sshd is hardened: `PermitRootLogin no`, `PasswordAuthentication no`, key-only. On Ubuntu 24.04, `ssh.socket` had to be disabled (it overrides `sshd_config`'s Port/ListenAddress) and replaced with `ssh.service`.

## Build Process

1.  Provisioned the VPS (Ubuntu 24.04), fully patched, rebooted into the current kernel before exposing anything.
2.  **Hardened access first (before any bait went live):** ed25519 key auth only; non-root sudo user; disabled root SSH; installed Tailscale and joined the tailnet; enabled Tailscale SSH; moved real sshd to port 22222; verified every door worked after each change (never locked out).
3.  Installed Cowrie in an isolated Python virtualenv under a dedicated, password-less `cowrie` user (least privilege - the honeypot process owns nothing important).
4.  Exposed the bait with an iptables NAT redirect **scoped to** `eth0`: public port 22 → Cowrie's 2222.
5.  Built the log pipeline: Cowrie writes JSON → Promtail tails it and ships to Loki → Grafana queries Loki. Solved a real permissions issue (Promtail couldn't read the cowrie user's log directory; fixed with least-privilege group membership rather than opening permissions wide).
6.  Made everything reboot-proof: Cowrie runs as a systemd service (auto-start + auto-restart); the iptables redirect is persisted; ssh.socket stays disabled. Verified the full stack survives a reboot.
7.  Built the Grafana dashboard (10 panels, see below), bound to the Tailscale interface, reachable privately from any of my devices.

## Incident: Locked Myself Out, and the Rebuild (the real lesson)

The first build worked and caught real attackers within minutes - but I **locked myself out.** The iptables redirect was written *without* an interface scope, so it caught **all** port-22 traffic - including my own admin SSH coming in over Tailscale. Every login got shoved into Cowrie's fake shell instead of the real box.
Recovery was blocked on every side: normal SSH hit the honeypot; the provider's web console only offered root (which I'd disabled); Tailscale SSH wasn't enabled yet. This is, it turns out, the single most common honeypot misconfiguration.
Because a honeypot is disposable by design, I destroyed the box and rebuilt it correctly - this time with the three independent safety nets above, verifying each door before relying on it, and confirming the whole stack survives a reboot. I also rebuilt the entire Grafana dashboard on the new box.
**Why this matters:** the fix taught me interface-scoped firewall rules, systemd service persistence, the Ubuntu 24.04 `ssh.socket` override, and multi-path recovery - at a depth I wouldn't have reached if it had worked first try. "Diagnosed a lockout, identified the root cause, and rebuilt with defense-in-depth so it can't recur" is a stronger story than a clean first attempt.

## Dashboard

Loki + Promtail + Grafana, all systemd services. Grafana bound to the Tailscale IP only (never public). Promtail scrapes Cowrie's JSON log into Loki; added to the `cowrie` group to read the log directory. Ten panels (built by hand in LogQL):

- **Attacks Over Time** (time series) - connection volume, spot the busy periods
- **Top Attacker IPs** (bar gauge) - who's hitting hardest
- **Top Passwords** / **Top Usernames** (bar gauge) - the credential-spray dictionary
- **Successful Logins** (bar gauge) - separates "actually got in" from noise
- **Pivot Targets** (bar gauge) - destinations attackers try to relay through (high-value threat intel)
- **Total Attacks (24h)** (stat)
- **All-Time Attacks** (stat)
- **GeoIP Attack Map** (Geomap) - real attacker locations plotted on a world map
- **Bait File Hits** (stat/panel) - tracks when an attacker opens the honeytoken

Note: LogQL line filters (`|=`) are case-sensitive - matching Cowrie's exact log text mattered.

## GeoIP Attack Map

Cowrie's built-in MaxMind plugin was removed in 3.0.12, so geo-enrichment is done in the log pipeline instead: a **Promtail geoip pipeline stage** uses the MaxMind GeoLite2 database with `source = src_ip` to add lat/lon labels to each event. Grafana's Geomap plots it in **Coords mode** using "Labels to fields" + "Convert field type" to number, with an **Instant query type**. The result is a live world map showing where attackers are actually coming from.

## Honeytoken (bait file)

Planted a juicy fake `/root/passwords.txt` into Cowrie's fake filesystem - contents include a fake crypto wallet/seed phrase, fake API keys, fake DB credentials, and a subtle troll line. The file was injected via `fsctl` on the pickle, with matching content in `honeyfs`. A **"Bait File Hits"** dashboard panel tracks when any attacker `ls`/`cat`s it.

This separates high-signal human attackers (who actually dig for loot) from automated bot noise. The fake DB credential also acts as a **tripwire** - if those credentials ever show up in subsequent login attempts, it means an attacker read and reused them. Tested end-to-end.

## Results - Real Attacks Captured (25k+ over ~2 weeks)

- **Credential-spray botnet:** logged in with guessed creds (e.g. `theo/theo`, `admin/ocarina`) and immediately attempted to pivot/relay through the box to a fixed external target (`62.210.131.144:2535`). Same actor recurred from multiple IPs in the `2.57.121.0/24` range, each time targeting the same relay destination. **Confirmed across two separate honeypot boxes - corroborated infrastructure.** Cowrie captured the intent and blocked the relay every time.
- **Crypto-trading-bot hunting campaign:** a large financially-motivated campaign probing with usernames like `binance`, `freqtrade`, `solana`, `hummingbot`, `gunbot` - hunting for exposed crypto trading bots to hijack or scrape.
- **RDP scanner hitting the wrong port:** a scanner spoke RDP (`mstshash=hello`) at the SSH honeypot; Cowrie rejected the malformed protocol in ~2ms. Recurred and became the top source by volume.
- **Research crawler:** a `ZGrab SSH Survey` scanner performed a full handshake - distinguishable from malicious bots by its banner and behavior.
- **Bot fingerprinting via HASSH:** a hash of how each SSH client negotiates the connection, which identifies the tool even when the banner is spoofed. Used to characterize various bots.

## What I Learned

- **Hardening order matters:** secure your own access *before* exposing anything, and verify each change from a fresh session so a bad config never locks you out.
- **Firewall rules need scope:** an unscoped NAT redirect catches traffic you didn't intend - `-i eth0` is the difference between a working honeypot and a lockout.
- **Defense-in-depth for access:** multiple independent recovery paths (identity-based Tailscale SSH + a high-port sshd + a scoped firewall) mean no single mistake locks you out.
- **Least privilege throughout:** dedicated password-less service user, key-only login, no root SSH, services bound to private interfaces.
- **Right tool for the constraint:** chose Loki over Elasticsearch because the box's RAM couldn't support the heavier stack.
- **Real incident response:** symptom → isolate the failing layer → root cause → rebuild → verify, including recovering from a self-inflicted lockout.
- **GeoIP enrichment in the log pipeline:** when the app's built-in geo plugin disappears (Cowrie 3.0.12 dropped MaxMind), do it in Promtail - keeps the app stock and the pipeline single-purpose.
- **Honeytokens for high-signal detection:** a realistic bait file separates human attackers from bot spray, and a fake credential doubles as a tripwire if it's ever reused.
- **Threat characterization from live data:** correlated repeated events into an actor profile (same source range, same pivot target, corroborated across boxes) - the core analyst motion of turning individual alerts into a pattern.

## Defensive Takeaways

- Internet-facing SSH is under constant automated attack - exposure is measured in *minutes*, not days.
- Low-interaction honeypots are a high-value, low-risk way to gather real threat intelligence; the isolation (disposable box, no path home, fake shell) keeps the blast radius tiny.
- Segmentation by trust level (deliberately-attacked assets kept away from trusted systems) is the discipline that makes running a honeypot safe.
- Capturing attacker *intent* (pivot target, credentials, HASSH fingerprint) produces indicators a defender can act on - block the source range, flag the relay destination, fingerprint the tool.

## Roadmap (this project)

- **Alerting to my agent** - wire high-signal events (successful interactive login, honeytoken access, activity against the real box) to a real-time pager, deliberately *tuned* to avoid alert fatigue (raw bot noise stays on the dashboard; only signal pages me).

## Status

Fully complete and live. Honeypot is internet-exposed, catching real attackers 24/7, reboot-proof, with three independent admin-access paths and a private 10-panel Grafana dashboard reachable from anywhere over Tailscale. GeoIP attack map and honeytoken bait file are live; seasoned 2+ weeks with 25k+ real attacks. Agent alerting is the next (and only remaining) enhancement.

# Task 1: Basic Network Scanning with Nmap

## Objective
Perform a network scan to identify open ports and running services on a local VM using Nmap, and document the findings with a security-focused analysis of each discovered service.

---

## What is Nmap?

Nmap (Network Mapper) is a free, open-source tool used to discover hosts and services on a computer network. It works by sending specially crafted packets to a target and analyzing the responses. Nmap can:

- Identify **live hosts** on a network
- Discover **open ports** on a target machine
- Detect the **service and version** running on each open port
- Attempt to fingerprint the target's **operating system**
- Run scripts (via the Nmap Scripting Engine) for deeper vulnerability checks

It is one of the most widely used tools in both offensive security (reconnaissance) and defensive security (auditing your own network to see what's exposed).

## Why Network Scanning Matters

You can't secure what you don't know exists. Network scanning matters because:

- **Attack surface visibility** — every open port is a potential entry point. Scanning tells you what's actually reachable, not what you assume is reachable.
- **Misconfiguration detection** — services get left running by accident (a database port exposed publicly, a dev server never taken down).
- **Baseline for monitoring** — a known-good scan result gives you something to compare against later if something changes unexpectedly.
- **Attacker's-eye view** — this is the exact first step a real attacker takes during reconnaissance. Running it yourself, defensively, means you find the gaps before someone else does.

## Ethical Use Guidelines

Nmap is a legal, legitimate tool — but only when used on systems you **own or have explicit written permission to scan**. Before running any scan:

- ✅ Only scan machines you own (like this lab VM) or have documented authorization to test
- ✅ Keep this work inside an isolated lab/VM environment
- ❌ Never scan networks, IPs, or systems belonging to others without permission — in many jurisdictions this is illegal, regardless of intent
- ❌ Don't run aggressive scans (`-A`, heavy timing templates) against production systems without prior coordination, as they can cause service disruption

This project was performed entirely against a personal, isolated lab VM under my own control.

---

## Environment

- **Host OS:** Windows (VirtualBox)
- **Attacking machine:** Kali Linux (VM)
- **Target machine:** Same Kali VM (self-scan, IP `10.0.2.15`)
- **Network mode:** VirtualBox NAT (default adapter)

## Installation

Nmap comes preinstalled on Kali Linux. To verify or install manually:

```bash
nmap --version
sudo apt update && sudo apt install nmap -y
```

## Scans Performed

| # | Command | Purpose |
|---|---------|---------|
| 1 | `nmap [target IP]` | Basic scan — default top 1000 TCP ports |
| 2 | `sudo nmap -sV [target IP]` | Service/version detection on open ports |
| 3 | `sudo nmap -O [target IP]` | Operating system fingerprinting |

Full raw output is recorded in [`nmap_scan_results.txt`](./nmap_scan_results.txt).

## Findings Summary

Only one open port was found on the target: **port 22 (SSH)**, running OpenSSH 10.3p1 (Debian 4, protocol 2.0). All other 999 scanned TCP ports were filtered. OS detection could not return an exact match (expected on VMs), with "Linux 5.0–6.2" as the top guess at 96% confidence.

Full raw scan output and the complete per-port security analysis are in [`nmap_scan_results.txt`](./nmap_scan_results.txt).

## Screenshots

Terminal output screenshots for all three scans are in the [`screenshots/`](./screenshots) folder:
- `01_basic_scan.png`
- `02_service_version_scan.png`
- `03_os_detection_scan.png`

## Key Takeaways

A freshly installed Kali VM has a minimal attack surface — only SSH was open, no legacy or unencrypted protocols (FTP, Telnet) and no database ports exposed. This is the baseline you want to see: nothing running that doesn't need to be. If this were a real system, the top priority would be confirming SSH is locked down to key-based authentication only, since it represents the entire remote attack surface on this host. OS fingerprinting failing to find an exact match was a useful reminder that Nmap's OS detection needs both an open *and* a closed port to work reliably, and behaves differently on virtualized hardware than on bare metal.

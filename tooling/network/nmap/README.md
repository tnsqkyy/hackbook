# Nmap (Network Mapper)

**Nmap** is the single most important tool for network reconnaissance and security auditing. It allows you to discover live hosts, scan for open ports, enumerate services, detect operating systems, and run powerful scripts to uncover vulnerabilities.

This guide provides a structured workflow for using Nmap effectively, from initial discovery to deep enumeration.

---

## 📌 Overview

Nmap is used for:

*   **Host Discovery:** Finding live systems on a network (Ping scans).
*   **Port Scanning:** Identifying open, closed, and filtered TCP/UDP ports.
*   **Service Enumeration:** Determining the software and version running on a port.
*   **OS Detection:** Fingerprinting the remote operating system.
*   **Scripting:** Automating advanced discovery and vulnerability checks with the Nmap Scripting Engine (NSE).

This guide focuses on building a repeatable and effective methodology.

---

## 1. Installation

Debian/Ubuntu:
```bash
sudo apt update && sudo apt install nmap
```

Arch:
```bash
sudo pacman -S nmap
```

CentOS/RHEL:
```bash
sudo dnf install nmap
```

Kali / ParrotOS: preinstalled.

---

## 2. Phase 1: Host Discovery (Ping Scans)

The first step is to find which hosts are online. A **ping scan** (`-sn`) disables port scanning and only reports responsive hosts.

```bash
# Scan a single IP
nmap -sn 192.168.1.1

# Scan a range of IPs
nmap -sn 192.168.1.1-100

# Scan an entire CIDR block (common)
nmap -sn 192.168.1.0/24
```

This is a low-noise way to create a list of potential targets. Save the live IPs to a file.

```bash
nmap -sn 192.168.1.0/24 | grep "Nmap scan report" | awk '{print $5}' > targets.txt
```

---

## 3. Phase 2: Port Scanning

Once you have a list of targets, scan them for open ports.

#### **Fast Scan (Top Ports)**

A fast scan (`-F`) checks the 100 most common ports. It's a quick way to get an initial overview.

```bash
nmap -F -iL targets.txt
```

#### **Default Scan (Top 1,000 Ports)**

A default scan is more comprehensive.

```bash
nmap -iL targets.txt
```

#### **Full TCP Scan (All 65,535 Ports)**

To be exhaustive, scan all TCP ports with `-p-`. This is slow but necessary for finding non-standard services.

```bash
nmap -p- -iL targets.txt
```

---

## 4. Phase 3: Service Enumeration & Aggressive Scans

Now that you know which ports are open, find out what's running on them. The `-A` flag enables **OS detection (`-O`), version detection (`-sV`), script scanning (`-sC`), and traceroute (`--traceroute`)**.

```bash
# Run an aggressive scan on a specific target and specific ports
nmap -p 22,80,443 -A 192.168.1.5

# Run it on your full list of targets (can be noisy)
nmap -A -iL targets.txt -oN nmap_aggressive.txt
```

*   `-sV`: Probes open ports to determine service/version info.
*   `-O`: Attempts to determine the OS of the target.
*   `-sC`: Runs default safe scripts from the Nmap Scripting Engine (NSE).

---

## 5. Nmap Scripting Engine (NSE)

The NSE is Nmap's most powerful feature. It allows you to run scripts for discovery, vulnerability detection, and more.

#### **List available scripts:**
```bash
ls -l /usr/share/nmap/scripts/
```

#### **Run a specific category of scripts:**

The `vuln` category checks for known vulnerabilities. **Warning: This is an active, intrusive scan.**

```bash
nmap --script=vuln -p 80,443 192.168.1.5
```

#### **Run a specific script:**

For example, check for HTTP headers.

```bash
nmap --script=http-headers -p 80 192.168.1.5
```

---

## 📚 Notes & Understanding

*   **Scan Types:** Nmap has many scan types (`-sS`, `-sT`, `-sU`). The default, `-sS` (SYN Scan), is stealthy and efficient because it never completes a full TCP connection.
*   **Timing Templates (`-T`):** Control scan speed. `-T4` (Aggressive) is common, but `-T2` (Polite) or `-T1` (Sneaky) are better for avoiding detection.
*   **Output Formats (`-oN`, `-oX`, `-oG`):** Always save your scans. `-oN` (Normal) and `-oX` (XML) are the most useful.

---

## 🚀 Next Steps

Practice this workflow:

1.  **Discover** hosts (`-sn`).
2.  **Scan** for open ports (`-p-` or `-F`).
3.  **Enumerate** services (`-A`).
4.  **Investigate** with specific NSE scripts.

Mastering Nmap is fundamental to all network-based security testing.

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

Kali / ParrotOS: preinstalled.

---

## 2. Phase 1: Host Discovery

The first step is to find which hosts are online. A **ping scan** (`-sn`) disables port scanning and only reports responsive hosts. This is a low-noise way to create a list of potential targets.

```bash
# Scan an entire CIDR block (common)
nmap -sn 192.168.1.0/24
```

To save the live IPs to a file for later use:
```bash
nmap -sn 192.168.1.0/24 | grep "Nmap scan report" | awk '{print $5}' > targets.txt
```

---

## 3. Phase 2: Port Scanning

Once you have a list of targets (`targets.txt`), scan them for open ports.

#### **Default Scan (Top 1,000 Ports)**
A default scan is a good starting point.
```bash
nmap -iL targets.txt
```

#### **Full TCP Scan (All 65,535 Ports)**
To be exhaustive, scan all TCP ports with `-p-`. This is slow but necessary for finding non-standard services.
```bash
nmap -p- -iL targets.txt
```

---

## 4. Phase 3: Service Enumeration

Now that you know which ports are open, find out what's running on them. The `-A` flag is a powerful option that enables **OS detection (`-O`), version detection (`-sV`), script scanning (`-sC`), and traceroute (`--traceroute`)**.

```bash
# Run an aggressive scan on a specific target for specific ports
nmap -p 22,80,443 -A 192.168.1.5
```
This is an aggressive scan and can be noisy. It's often better to run it on specific, interesting targets rather than a whole network at once.

---

## 5. Nmap Scripting Engine (NSE)

The NSE is Nmap's most powerful feature. It allows you to run scripts for discovery, vulnerability detection, and more.

#### **Run a specific category of scripts:**
The `vuln` category checks for known vulnerabilities. **Warning: This is an active, intrusive scan.**
```bash
nmap --script=vuln -p 80,443 192.168.1.5
```

#### **Run a specific script:**
For example, to check for HTTP headers on a web server.
```bash
nmap --script=http-headers -p 80 192.168.1.5
```

---

## 📚 Notes & Understanding

*   **Scan Types:** Nmap has many scan types. The default, `-sS` (SYN Scan), is stealthy and efficient because it never completes a full TCP connection. Use `nmap -sT` for a full connect scan if you have privileges.
*   **Timing Templates (`-T`):** Control scan speed. `-T4` (Aggressive) is common, but `-T2` (Polite) or `-T1` (Sneaky) are better for avoiding detection by firewalls and Intrusion Detection Systems (IDS).
*   **Output Formats:** Always save your scans. `-oN` (Normal) is human-readable, `-oX` (XML) is great for importing into other tools, and `-oG` (Grepable) is useful for scripting.
*   **Verbosity (`-v`):** Use `-v` or `-vv` to see what Nmap is doing in real-time. This is essential for long scans to make sure they haven't stalled.

---

## 🚀 Next Steps

The true power of Nmap comes from combining these techniques into a workflow:

1.  **Discover** hosts broadly (`-sn`).
2.  **Scan** for open ports on your live targets (`-p-` or `-F`).
3.  **Enumerate** services on interesting ports (`-A -p<ports>`).
4.  **Investigate** further with specific NSE scripts.

Mastering this workflow is fundamental to all network-based security testing.

# Nmap: The Network Mapper

**Nmap** is the foundational tool for network exploration and security auditing. It’s like a sonar for a network, allowing you to find hosts, check for open ports, and peek inside to see what services are running.

Mastering Nmap isn't just about learning commands; it's about learning to see a network's structure.

---

### 📡 Phase 1: Host Discovery — Finding a Pulse

Before you can scan a target, you must confirm it's online. A "Ping Scan" (`-sn`) does exactly this without being intrusive. It’s the first step to mapping out the terrain.

```bash
# Scan an entire subnet to see who is online
nmap -sn 192.168.1.0/24
```

*   **Purpose:** Builds your initial list of live targets.
*   **Key Flag:** `-sn` (Scan, No port scan).
*   **Pro Tip:** Save your findings! `nmap -sn 192.168.1.0/24 | grep "report for" | awk '{print $5}' > targets.txt`

---

### 🚪 Phase 2: Port Scanning — Checking for Open Doors

Once you have your targets, find out which "doors" (ports) are open. This is how you'll find potential entry points.

*   **Fast Scan (`-F`)**: A quick check of the 100 most common ports. Good for a first look.
    ```bash
    nmap -F -iL targets.txt
    ```
*   **Full TCP Scan (`-p-`)**: The exhaustive approach. It checks all 65,535 TCP ports. It's slow, but it ensures you don't miss anything hiding on a non-standard port.
    ```bash
    nmap -p- -iL targets.txt
    ```
*   **Default Scan**: If you don't specify, Nmap scans the 1,000 most common ports. A good middle ground.
    ```bash
    nmap -iL targets.txt
    ```

---

### 🕵️ Phase 3: Enumeration — Peeking Inside the Rooms

You've found open doors. Now, what's inside? Enumeration identifies the exact services, software versions, and even the operating system.

The `-A` flag is the powerhouse for this phase. It combines several key techniques:

*   `-sV`: Service Version detection (e.g., Apache httpd 2.4.41)
*   `-O`: Operating System detection (e.g., Linux 5.4)
*   `-sC`: Runs default scripts to find low-hanging fruit.
*   `--traceroute`: Traces the network path to the target.

```bash
# Run an aggressive scan on a high-value target
nmap -A -p 22,80,443 192.168.1.5 -v
```

**This is where information turns into intelligence.** You're no longer just listing ports; you're mapping out the target's software stack.

---

### 🚀 Nmap Scripting Engine (NSE) — The Superpower

NSE unleashes Nmap's full potential by using scripts to automate advanced tasks, from deep discovery to vulnerability scanning.

*   **Run default "safe" scripts (`-sC` or `--script=default`):**
    ```bash
    nmap -sC 192.168.1.5
    ```
*   **Check for known vulnerabilities (`--script=vuln`):**
    > **Warning:** This is an active and intrusive scan. Only run this on systems you have explicit permission to test.
    ```bash

    nmap -p 80 --script=vuln 192.168.1.5
    ```

---

### 📚 Essential Tips & Best Practices

*   **-T4 (Timing):** For faster scans. Use `-T2` or `-T3` to be less noisy and avoid IDS detection.
*   **-oN (Output):** Always save your results. `nmap -A target -oN target_scan.txt`
*   **-v (Verbosity):** Use `-v` or `-vv` to get real-time feedback on scan progress. It's a lifesaver on long scans.
*   **The Workflow:** The classic workflow is **Discover (`-sn`) → Scan (`-p-`) → Enumerate (`-A`)**. Stick to it, and you'll be systematic and effective.

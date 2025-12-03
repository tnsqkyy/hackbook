# Nmap (Network Mapper)

**Nmap** is probably the most important tool for looking at what's on a network. It lets you find live computers, scan for open ports, figure out what software is running, and even find security holes with its built-in scripts.

This guide is a simple workflow for using Nmap like a pro.

---

## 📌 What's it for?

You can use Nmap to:

*   **Find Computers:** See which devices are online on a network.
*   **Find Open Ports:** Check for open doors (ports) on those computers.
*   **Identify Software:** Figure out what software and what version is running on a port.
*   **Detect the OS:** Guess the operating system of a target computer.
*   **Run Scripts:** Use the Nmap Scripting Engine (NSE) to automatically check for common problems.

This guide will show you a step-by-step method for using these features.

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

Kali / ParrotOS: It's already installed!

---

## 2. Step 1: Find Live Computers

First, let's see who is online. A **ping scan** (`-sn`) just checks which computers are up without port scanning them. It's a quiet way to get a list of targets.

```bash
# Scan a whole network range (e.g., 192.168.1.1 to 192.168.1.254)
nmap -sn 192.168.1.0/24
```

To save the online IP addresses to a file for later:
```bash
nmap -sn 192.168.1.0/24 | grep "Nmap scan report" | awk '{print $5}' > targets.txt
```

---

## 3. Step 2: Scan for Open Ports

Once you have your list of `targets.txt`, scan them for open ports.

#### **Quick Scan (Top 1,000 Ports)**
This is a good, fast starting point.
```bash
nmap -iL targets.txt
```

#### **Full Scan (All 65,535 Ports)**
To be sure you don't miss anything, scan every single TCP port with `-p-`. It's slow, but you might find services on weird ports.
```bash
nmap -p- -iL targets.txt
```

---

## 4. Step 3: Identify Services

Now you know which ports are open, find out what's running on them. The `-A` flag is a powerful shortcut that turns on **OS detection (`-O`), version detection (`-sV`), basic script scanning (`-sC`), and traceroute (`--traceroute`)** all at once.

```bash
# Run a strong scan on one target, looking at specific ports
nmap -p 22,80,443 -A 192.168.1.5
```
This scan is "loud" and easy to detect. It's better to run it on single targets instead of the whole network.

---

## 5. Using the Nmap Scripting Engine (NSE)

NSE is Nmap's superpower. You can use it to run scripts that find vulnerabilities, discover more info, and more.

#### **Check for known security holes:**
The `vuln` script category is famous for this. **Warning: This is a very "loud" and active scan that might crash things!**
```bash
nmap --script=vuln -p 80,443 192.168.1.5
```

#### **Run one specific script:**
For example, to grab the HTTP headers from a web server.
```bash
nmap --script=http-headers -p 80 192.168.1.5
```

---

## 📚 Things to Remember

*   **Scan Types:** By default, Nmap uses `-sS` (SYN Scan), which is sneaky because it doesn't complete the full connection. If you're an admin on the machine, you might need to use `-sT` for a full connect scan.
*   **Speed:** Control your scan speed with `-T`. `-T4` is fast and common. `-T2` (Polite) or `-T1` (Sneaky) are good for avoiding firewalls or security systems (IDS).
*   **Save Your Work:** Always save your scans! Use `-oN` for a normal text file, or `-oA` to save in all major formats at once.
*   **Be Loud (`-v`):** Use `-v` or `-vv` to get real-time updates on what Nmap is doing. This is great for long scans to see if they're stuck.

---

## 🚀 The Workflow

The real power of Nmap is putting it all together:

1.  **Discover:** Find live computers (`-sn`).
2.  **Scan:** Check for open ports on your targets (`-p-` or `-F` for fast).
3.  **Identify:** Figure out the services on interesting ports (`-A -p<ports>`).
4.  **Investigate:** Dig deeper with specific NSE scripts.

Mastering this is key to all network hacking.

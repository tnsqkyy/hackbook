# Tooling

The **tooling** directory contains the foundational tools every pentester must understand. This section focuses on *why* each tool matters, how it works internally, and how it fits into real offensive workflows.

Pentesting is not about memorizing commands — it is about mastering the right tools and understanding the logic behind them. This section captures exactly that.

---

## 🔍 Categories

All tools are organized by offensive capability rather than by name. This makes the structure scalable as the Hackbook grows.

### **1. Reconnaissance & Scanning**

Tools used to discover hosts, services, ports, and surface‑level information.

→ `./recon/`

---

### **2. Traffic Analysis & Sniffing**

Tools used to capture, inspect, and analyze network packets.

→ `./sniffing/`

---

### **3. Wireless Attacks**

Tools focused on WiFi hacking, 802.11 frame capture, WPA/PMKID attacks, etc.

→ **[wireless](./wireless/)**

---

### **4. Password Cracking & Hash Attacks**

Tools for brute‑forcing credentials, cracking hashes, and generating key material.

→ `./password-cracking/`

---

### **5. Exploitation Frameworks**

Tools that automate exploitation or provide a modular attack environment.

→ `./exploitation/`

---

### **6. Post‑Exploitation & Privilege Escalation**

Tools for maintaining access, escalating privileges, and lateral movement.

→ `./post-exploitation/`

---

### **7. Utilities & System Essentials**

Tools and helper binaries that support the pentest process.

→ `./utilities/`

---

## 🧠 Philosophy

Every tool in this directory includes:

* What the tool does
* Why a pentester needs it
* How it works internally (packet flow, protocol logic, OS interaction)
* Practical real‑world usage
* Example commands

This is not a cheat‑sheet — it is a set of *understandings*.

---

## 📈 Future Scope

As Hackbook expands, tooling will integrate with other sections like:

* Network fundamentals
* Attacker methodology
* Labs & walkthroughs

The structure here is designed to scale cleanly as more tools and categories are added.

---

## 🚀 Start Exploring

Begin by choosing a category:

* Recon → `./recon/`
* Sniffing → `./sniffing/`
* Wireless → `./wireless/`
* Password Cracking → `./password-cracking/`
* Exploitation → `./exploitation/`
* Post‑Exploitation → `./post-exploitation/`
* Utilities → `./utilities/`

More tools will be added as the learning journey continues.

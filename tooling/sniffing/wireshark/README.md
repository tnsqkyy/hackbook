# Wireshark

> "The packets never lie."

---

## 🧐 What it is

**Wireshark** is the world's foremost and widely-used network protocol analyzer. It lets you see what's happening on your network at a microscopic level. It is the de facto (and often de jure) standard across many commercial and non-profit enterprises, government agencies, and educational institutions.

---

## 🧠 Why it Matters

In pentesting, visibility is everything. Wireshark provides the ground truth of network communications. You can't trust what a machine *says* it's doing; you can only trust the packets it actually sends.

*   **Debugging:** Find out why network applications are not working.
*   **Security Analysis:** Detect suspicious network activity, analyze malware traffic, and identify intrusion attempts.
*   **Protocol Learning:** Understand how protocols like TCP/IP, DNS, and HTTP actually work by observing them in action.

---

## ⚙️ Internal Mechanics: How Wireshark Captures

Wireshark itself doesn't capture packets; it uses a capture library to do so.

1.  **Capture Library:** On Linux/macOS, it uses `libpcap`. On Windows, it uses `Npcap` (formerly `WinPcap`).
2.  **Promiscuous Mode:** The network interface card (NIC) is put into promiscuous mode. Normally, a NIC ignores all traffic that isn't addressed to its MAC address. In promiscuous mode, it grabs every packet it sees on the wire.
3.  **Packet Buffering:** The capture library copies these packets into a buffer in the kernel.
4.  **Hand-off to Wireshark:** Wireshark pulls packets from this buffer, dissects them according to its vast library of protocol dissectors, and displays them in a human-readable format.

---

## 🕵️ Real Attacker Use-Cases

*   **Credential Sniffing:** On an unencrypted network (e.g., open Wi-Fi), an attacker can capture login credentials sent in cleartext by protocols like FTP, Telnet, or old HTTP sites.
*   **Analyzing Malware C2 Traffic:** Capture traffic from a sandboxed machine infected with malware to understand its Command & Control (C2) communication patterns, heartbeat, and data exfiltration methods.
*   **Session Hijacking:** By capturing session cookies sent over unencrypted HTTP, an attacker can impersonate a legitimate user.
*   **VoIP Eavesdropping:** Capture and reconstruct RTP streams to listen to VoIP phone calls.

---

## 📊 Common Analysis Techniques

Instead of "commands," Wireshark is about analysis techniques.

*   **Display Filters:** The most powerful feature. Use `ip.addr == 1.1.1.1` to see packets to/from Cloudflare's DNS, or `http.request.method == "POST"` to find HTTP POST requests.
*   **Follow TCP/UDP Stream:** Right-click a packet and choose "Follow > TCP Stream" to see the full, reconstructed conversation, stripping away all the network headers. This is essential for reading conversations.
*   **Export Objects:** Use `File > Export Objects` to automatically extract files (like images, executables, or ZIPs) being transferred over protocols like HTTP or SMB.
*   **Coloring Rules:** Customize packet coloring to instantly highlight interesting traffic, like red for TCP resets or black for suspicious packets.

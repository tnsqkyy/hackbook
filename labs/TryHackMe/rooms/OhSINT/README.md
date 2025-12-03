# OhSINT Walkthrough

**Room Link:** [https://tryhackme.com/room/ohsint](https://tryhackme.com/room/ohsint)

This room is a great introduction to Open-Source Intelligence (OSINT). It shows how much you can find starting with just one picture. By following a trail of digital clues, we'll find someone's online identity, where they live, their email, and even their password.

---

## 🚀 The Mission & How I Solved It

Our mission starts with a single image: `WindowsXP.jpg`, which you can find in the [`./task-files/`](./task-files/) folder. Let's find the answers to the room's questions.

### Step 1: Look at the Picture's Hidden Info

First, I checked the image for any hidden data (metadata). Using a tool like [**ExifTool**](../../../../tooling/recon/exiftool/README.md) gave me the first big clue: the copyright holder's name. This username is the key to the whole challenge.

```bash
# This is the command I used
exiftool WindowsXP.jpg
```

<figure>
  <img src="assets/images/exiftool-output.png" alt="ExifTool Output" width="600"/>
  <figcaption>Figure 1: The output from ExifTool, showing the username.</figcaption>
</figure>

---

### Step 2: Answering the Questions

With the username, I could start hunting for the answers.

*   **1. What is this user's avatar of?** `[Answer Here]`
    *   **How I found it:** I searched the username on Twitter and found their profile picture.

*   **2. What city is this person in?** `[Answer Here]`
    *   **How I found it:** I found a BSSID (like a WiFi router's fingerprint) in one of their posts. I used a site called WiGLE.net to see where that BSSID is located on a map.

*   **3. What is the SSID of the WiFi he connected to?** `[Answer Here]`
    *   **How I found it:** WiGLE.net also showed the name (SSID) of the WiFi network.

*   **4. What is his personal email address?** `[Answer Here]`
    *   **How I found it:** I searched for the username on GitHub and found the email address in their profile.

*   **5. What site did you find his email address on?** `[Answer Here]`
    *   **How I found it:** From the step above, it was GitHub.

*   **6. Where has he gone on holiday?** `[Answer Here]`
    *   **How I found it:** I looked for a personal blog linked from their other profiles and found a post about a trip.

*   **7. What is the person's password?** `[Answer Here]`
    *   **How I found it:** On the personal blog, I viewed the page's HTML source code and found the password hidden in a comment.

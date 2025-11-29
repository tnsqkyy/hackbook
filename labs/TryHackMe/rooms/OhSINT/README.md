# OhSINT Write-up

**Room Link:** [https://tryhackme.com/room/ohsint](https://tryhackme.com/room/ohsint)

This room is a classic introduction to Open-Source Intelligence (OSINT). It demonstrates just how much information can be uncovered starting from a single file—in this case, just one image. By following a trail of digital breadcrumbs, we will discover a person's online identity, location, email, and even their password.

---

## 🎯 The Challenge

Starting with only the `WindowsXP.jpg` image located in the [`./task-files/`](./task-files/) directory, can you answer the following questions?

1.  What is this user's avatar of?
2.  What city is this person in?
3.  What is the SSID of the WAP he connected to?
4.  What is his personal email address?
5.  What site did you find his email address on?
6.  Where has he gone on holiday?
7.  What is the person's password?

---

## 🚀 Walkthrough & Methodology

This section details the step-by-step process used to answer the questions above.

### Step 1: Initial Metadata Analysis

The investigation begins by examining the provided image for hidden data. Using a tool like [**ExifTool**](../../../../tooling/recon/exiftool/README.md) reveals the copyright holder's name, which becomes the primary pivot point for the entire challenge.

```bash
# Command to extract metadata
exiftool WindowsXP.jpg
```

<figure>
  <img src="assets/images/exiftool-output.png" alt="ExifTool Output" width="600"/>
  <figcaption>Figure 1: Output from exiftool WindowsXP.jpg</figcaption>
</figure>

### Step 2: Answering the Questions

With the initial clue (the username), we can now find the answers.

*   **Answer 1 (Avatar):** `[Answer Here]`
    *   *Methodology: Search the username on social media (e.g., Twitter).*

*   **Answer 2 (City):** `[Answer Here]`
    *   *Methodology: Find a BSSID in the user's posts and geolocate it using a service like WiGLE.net.*

*   **Answer 3 (SSID):** `[Answer Here]`
    *   *Methodology: The SSID is usually found along with the BSSID information on WiGLE.net.*

*   **Answer 4 (Email):** `[Answer Here]`
    *   *Methodology: Look for the user's profile on developer or portfolio sites like GitHub.*

*   **Answer 5 (Email Source):** `[Answer Here]`
    *   *Methodology: The source site from the previous step.*

*   **Answer 6 (Holiday):** `[Answer Here]`
    *   *Methodology: Check the user's blog or social media posts for travel mentions.*

*   **Answer 7 (Password):** `[Answer Here]`
    *   *Methodology: Inspect the source code of the user's personal website or blog for hidden comments.*

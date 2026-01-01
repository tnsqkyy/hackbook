# ExifTool

**ExifTool** is a command-line tool for reading hidden information (metadata) inside files. Think of it like a detective's magnifying glass for digital files. It's a key tool for OSINT (Open-Source Intelligence) because it can pull out secret data from images, documents, and more.

This guide shows you how to use ExifTool to find hidden clues in a file.

---

##  What's it for?

ExifTool can help you:

*   **Read Metadata:** See all the hidden info stored in a file.
*   **Find Specific Clues:** Look for things like GPS location, camera type, or the software used to create the file.
*   **Check File History:** Find out who made the file and when.
*   **Find Hidden Comments:** Uncover secret notes the creator might have left inside.

---

## 1. Installation

Debian/Ubuntu:
```bash
sudo apt update && sudo apt install libimage-exiftool-perl
```

Arch:
```bash
sudo pacman -S perl-image-exiftool
```

---

## 2. Basic Use

To see all the metadata in a file, just give ExifTool the filename. It will print a clean list of everything it finds.

```bash
exiftool <file-name>
```

---

## 3. Example: The OhSINT Room

In the OhSINT room on TryHackMe, you get an image named `WindowsXP.jpg`. We can use ExifTool to find important clues inside it.

```bash
exiftool labs/TryHackMe/rooms/OhSINT/task-files/WindowsXP.jpg
```

Look for interesting lines in the output, like:
*   **Copyright:** This might be a username!
*   **GPS Position:** If this is here, you know the exact spot the photo was taken.

---

## 4. Common Questions & Flags

You can use flags to ask ExifTool specific questions.

| I want to...                            | Command                               |
| --------------------------------------- | ------------------------------------- |
| **See only GPS info?**                  | `exiftool -gps* <file-name>`          |
| **See info grouped by category?**       | `exiftool -g1 <file-name>`            |
| **See the short tag names?**            | `exiftool -s <file-name>`             |
| **Get just one specific thing?**        | `exiftool -Copyright <file-name>`     |

---

##  Things to Remember

*   **Not every file has metadata:** Many sites like Twitter and Facebook remove this hidden info when you upload pictures. Sometimes, the fact that there's **no** metadata is a clue itself.
*   **You can also remove metadata:** ExifTool can be used to wipe a file clean. To remove all metadata, you can run this command:
    ```bash
    exiftool -all= <file-name>
    ```
*   **It's not just for images:** Try it on PDFs, Word documents, or music files. You might be surprised what you find.


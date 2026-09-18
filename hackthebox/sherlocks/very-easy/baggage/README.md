<p align="center">
  <img src="images/baggage-icon.png" alt="Hack The Box Baggage Sherlock" width="280">
</p>

<h1 align="center">Baggage</h1>

<p align="center">
  <strong>Hack The Box Sherlock Write-Up</strong><br>
  Windows Shellbag Forensics Investigation
</p>

<p align="center">

![Platform](https://img.shields.io/badge/Platform-Hack%20The%20Box-9FEF00?style=flat-square&logo=hackthebox&logoColor=black)
![Type](https://img.shields.io/badge/Type-Sherlock-8A2BE2?style=flat-square)
![Difficulty](https://img.shields.io/badge/Difficulty-Very%20Easy-brightgreen?style=flat-square)
![Category](https://img.shields.io/badge/Category-DFIR-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

</p>

---

# 1. Introduction

**Baggage** is a Hack The Box Sherlock focused on **Windows Shellbag forensics**.

The challenge provides Windows Registry artifacts that can be used to reconstruct the activity of a compromised Windows user.

The main goal is to understand how the attacker:

- downloaded an archive,
- introduced a search utility,
- searched the victim's machine,
- accessed sensitive local folders,
- accessed a network share,
- identified sensitive company information,
- created a staging directory,
- compressed collected data,
- and prepared the information for exfiltration.

The main forensic artifacts used during this investigation were:

```text
NTUSER.DAT
UsrClass.dat
NTUSER.DAT.LOG1
NTUSER.DAT.LOG2
UsrClass.dat.LOG1
UsrClass.dat.LOG2
```

---

# 2. Challenge Information

| Field | Details |
|---|---|
| Platform | Hack The Box |
| Type | Sherlock |
| Name | Baggage |
| Difficulty | Very Easy |
| Category | Digital Forensics / DFIR |
| Main Topic | Windows Shellbag Analysis |
| Operating System | Windows |
| Status | ✅ Completed |

---

# 3. Scenario

The attacker compromised a Windows user account and started searching the victim's filesystem for useful information.

During the investigation, evidence showed that the attacker:

```text
Compromised user account
        ↓
Downloaded an archive
        ↓
Introduced a search utility
        ↓
Searched local files
        ↓
Found VPN and password-related information
        ↓
Accessed an internal network share
        ↓
Found sensitive construction information
        ↓
Created a local staging folder
        ↓
Compressed the collected information
        ↓
Prepared the archive for exfiltration
```

---

# 4. Evidence Provided

The important files for this investigation were:

```text
NTUSER.DAT
ntuser.dat.LOG1
ntuser.dat.LOG2

UsrClass.dat
UsrClass.dat.LOG1
UsrClass.dat.LOG2
```

---

# 5. Tools Used

## ShellBags Explorer

Used to examine Windows Shellbag artifacts and reconstruct folder navigation.

Useful for identifying:

- local folders,
- archives,
- network shares,
- folder hierarchy,
- MRU ordering,
- interaction timestamps.

## Registry Explorer

Used to manually analyze Windows Registry hives.

Useful for:

- searching Registry values,
- finding application references,
- reviewing RecentDocs,
- examining Registry timestamps,
- confirming additional evidence.

## Eric Zimmerman Tools

Useful tools for this type of investigation include:

```text
ShellBags Explorer
Registry Explorer
SBECmd
RECmd
Timeline Explorer
RLA
```

---

# 6. Windows Registry Basics

## NTUSER.DAT

`NTUSER.DAT` is a user-specific Windows Registry hive.

It can contain artifacts relating to:

```text
Recent documents
Application activity
Explorer activity
User settings
User-specific Registry keys
```

## UsrClass.dat

`UsrClass.dat` contains Windows Shell-related information.

This hive is particularly important when investigating **Shellbags**.

Important structures include:

```text
BagMRU
Bags
MRUListEx
```

---

# 7. What Are Shellbags?

Shellbags are Windows Registry artifacts created when users interact with folders through the Windows Shell, usually Windows File Explorer.

Shellbags can contain evidence of interaction with:

```text
Local directories
Network shares
ZIP archives
Removable devices
Previously accessed folders
Deleted folders
```

This makes Shellbags very useful during forensic investigations.

Even if a folder no longer exists, Shellbag evidence may still show that the user previously navigated to it.

---

# 8. BagMRU

`BagMRU` helps reconstruct the hierarchy of folders accessed by the user.

For example:

```text
Desktop
└── This PC
    └── Documents
        └── Sensitive Folder
```

Each entry can contain a Shell Item representing the corresponding folder or location.

---

# 9. MRUListEx

MRU stands for:

```text
Most Recently Used
```

`MRUListEx` stores the order in which Shell items were recently interacted with.

This can help determine which folder or archive was most recently navigated to.

---

# 10. Important Timestamp Lesson

One of the most important lessons from this Sherlock is that one object can have several different timestamps.

For example:

```text
Created On
Modified On
Accessed On
Registry LastWrite
First Interacted
Last Interacted
```

These do **not** necessarily represent the same event.

The correct timestamp must always be chosen based on:

```text
Question
   ↓
Artifact type
   ↓
Timestamp meaning
   ↓
Context
   ↓
Conclusion
```

---

# 11. Investigation

---

## Task 1 — Downloaded Archive

### Question

> What was the name of the archive file downloaded by the compromised account?

### Investigation

I started by examining the Shellbag hierarchy in ShellBags Explorer.

The user had interacted with the Downloads directory.

Inside the related Shellbag activity, an archive named:

```text
1.zip
```

was identified.

Because Windows Explorer can treat ZIP archives similarly to folders, archive activity can be stored in Shellbag artifacts.

### Answer

```text
1.zip
```

### Proof

<p align="center">
  <img src="images/01-downloaded-archive.png"
       alt="1.zip Shellbag evidence"
       width="900">
</p>

The screenshot should show:

```text
Downloads
└── 1.zip
```

---

## Task 2 — Search Utility

### Question

> What was the name of the utility brought in by the attacker to search for sensitive data?

### Investigation

Registry evidence showed an application archive named:

```text
Everything-1.4.1.1028.x64.zip
```

Inside it was:

```text
everything.exe
```

The software is **Everything**, a filename search utility.

The version can be extracted directly from the archive name:

```text
Everything-1.4.1.1028.x64.zip
           │
           └── 1.4.1.1028
```

The expected format was:

```text
SoftwareName X.X.X.XXXX
```

Therefore:

```text
Software Name = Everything
Version       = 1.4.1.1028
```

### Answer

```text
Everything 1.4.1.1028
```

### Proof

<p align="center">
  <img src="images/02-everything-utility.png"
       alt="Everything search utility evidence"
       width="900">
</p>

The screenshot should show:

```text
Everything-1.4.1.1028.x64.zip
```

and preferably:

```text
everything.exe
```

---

## Task 3 — VPN Folder Access

### Question

> The attacker navigated the filesystem and found sensitive files used by the victim in their day-to-day work. When was the VPN folder accessed by the attacker?

### Investigation

I searched the Shellbag data for:

```text
VPN
```

This revealed the directory:

```text
ROT Station 3 internal VPN
```

The relevant Shellbag interaction time was:

```text
2025-09-03 07:31:05
```

### Answer

```text
2025-09-03 07:31:05
```

### Proof

<p align="center">
  <img src="images/03-vpn-folder.png"
       alt="VPN folder access evidence"
       width="900">
</p>

The screenshot should show:

```text
ROT Station 3 internal VPN
```

and:

```text
2025-09-03 07:31:05
```

---

## Task 4 — Password Directory

### Question

> What was the name of the directory containing the victim's passwords?

### Investigation

Searching through the Shellbag folder structure revealed a directory named:

```text
OnePassword MasterPass
```

The name clearly indicated that it contained password-related information.

### Answer

```text
OnePassword MasterPass
```

### Proof

<p align="center">
  <img src="images/04-password-directory.png"
       alt="OnePassword MasterPass Shellbag evidence"
       width="900">
</p>

---

## Task 5 — Network Share

### Question

> The attacker also accessed a network share to pillage network data. What is the UNC path?

### What is a UNC path?

UNC means:

```text
Universal Naming Convention
```

Windows network shares generally use:

```text
\\SERVER\SHARE
```

For example:

```text
\\FileServer\Finance
```

### Investigation

The Shellbag evidence contained the network location:

```text
\\Prod-ns-2\prodshare
```

Breaking it down:

```text
Server = Prod-ns-2
Share  = prodshare
```

### Answer

```text
\\Prod-ns-2\prodshare
```

### Proof

<p align="center">
  <img src="images/05-network-share.png"
       alt="UNC network share evidence"
       width="900">
</p>

---

## Task 6 — Dam Construction Date

### Question

> When is the dam construction planned?

### Investigation

Under the network-share navigation, I found:

```text
Construction 2027
```

The question is not asking for a forensic timestamp.

It is asking for the year in which construction was planned.

### Answer

```text
2027
```

### Proof

<p align="center">
  <img src="images/06-construction-folder.png"
       alt="Construction 2027 evidence"
       width="900">
</p>

---

## Task 7 — Network Share Archive

### Question

> What was the name of the archive file present on the network share?

### Investigation

Continuing through the network-share Shellbag hierarchy revealed:

```text
Dam Construction Engineer Plans.zip
```

The reconstructed navigation path was approximately:

```text
Network
└── Prod-ns-2
    └── \\Prod-ns-2\prodshare
        └── Construction 2027
            └── Dam Construction Engineer Plans.zip
```

### Answer

```text
Dam Construction Engineer Plans.zip
```

### Proof

<p align="center">
  <img src="images/07-network-archive.png"
       alt="Dam Construction Engineer Plans archive"
       width="900">
</p>

---

## Task 8 — Network Archive Access Time

### Question

> When was the archive file from the network share accessed?

### Investigation

The archive:

```text
Dam Construction Engineer Plans.zip
```

was selected in ShellBags Explorer.

The relevant Shellbag interaction timestamp was:

```text
2025-09-03 07:34:04
```

### Answer

```text
2025-09-03 07:34:04
```

### Proof

<p align="center">
  <img src="images/08-network-archive-time.png"
       alt="Network archive interaction time"
       width="900">
</p>

The screenshot should clearly show both:

```text
Dam Construction Engineer Plans.zip
```

and:

```text
2025-09-03 07:34:04
```

---

## Task 9 — Staging Directory

### Question

> The attacker created a staging folder to prepare for collection and exfiltration. What is the full path of the staging folder?

### What is Data Staging?

Attackers often gather stolen information into one directory before sending it outside the environment.

For example:

```text
Passwords ─────┐
VPN Files ─────┤
Documents ─────┼──> Staging Directory
Network Data ──┘
                       ↓
                    Archive
                       ↓
                 Exfiltration
```

### Investigation

Shellbag evidence showed activity inside:

```text
Pictures
└── a
```

The compromised account belonged to Steve.

Therefore the complete path was:

```text
C:\Users\steve\Pictures\a
```

### Answer

```text
C:\Users\steve\Pictures\a
```

### Proof

<p align="center">
  <img src="images/09-staging-folder.png"
       alt="Data staging directory evidence"
       width="900">
</p>

---

## Task 10 — Exfiltration Archive

### Question

> The attacker compressed the staging folder to prepare the data for exfiltration. When was the exfiltration archive file accessed?

### Investigation

The attacker used the staging directory:

```text
C:\Users\steve\Pictures\a
```

The directory was then compressed into:

```text
a.zip
```

The relationship was:

```text
C:\Users\steve\Pictures\a
             │
             ▼
C:\Users\steve\Pictures\a.zip
```

This task was the most important timestamp lesson in the challenge.

Several timestamps appeared around the same archive activity.

Examples included values representing:

```text
File creation
File modification
Filesystem access
Registry modification
Shellbag interaction
Archive navigation
```

The timestamp accepted by HTB for the archive interaction was:

```text
2025-09-03 07:34:30
```

This corresponds to the relevant Shellbag/MRU interaction context.

### Answer

```text
2025-09-03 07:34:30
```

### Proof

<p align="center">
  <img src="images/10-exfiltration-archive.png"
       alt="a.zip exfiltration archive interaction evidence"
       width="900">
</p>

The screenshot should show:

```text
a.zip
```

and the relevant interaction information.

---

# 12. Final Answers

| Task | Correct Answer |
|---:|---|
| **1** | `1.zip` |
| **2** | `Everything 1.4.1.1028` |
| **3** | `2025-09-03 07:31:05` |
| **4** | `OnePassword MasterPass` |
| **5** | `\\Prod-ns-2\prodshare` |
| **6** | `2027` |
| **7** | `Dam Construction Engineer Plans.zip` |
| **8** | `2025-09-03 07:34:04` |
| **9** | `C:\Users\steve\Pictures\a` |
| **10** | `2025-09-03 07:34:30` |

---

# 13. Reconstructed Attack Sequence

```text
Steve's Account Compromised
            │
            ▼
        1.zip Downloaded
            │
            ▼
Everything Search Utility Introduced
            │
            ▼
Local Filesystem Searched
            │
       ┌────┴─────┐
       │          │
       ▼          ▼
 VPN Information  Password Information
       │          │
       └────┬─────┘
            ▼
 Internal Network Share Accessed
            │
            ▼
\\Prod-ns-2\prodshare
            │
            ▼
   Construction 2027
            │
            ▼
Dam Construction Engineer Plans.zip
            │
            ▼
     Sensitive Data Collected
            │
            ▼
C:\Users\steve\Pictures\a
            │
            ▼
          a.zip
            │
            ▼
 Prepared for Exfiltration
```

---

# 14. Investigation Timeline

| Time / Stage | Event |
|---|---|
| Initial Stage | Steve's account was compromised |
| Initial Activity | Archive introduced/downloaded |
| Discovery | Everything search utility used |
| Local Discovery | Sensitive local directories identified |
| `07:31:05` | VPN-related folder interaction |
| Network Discovery | Internal network share accessed |
| `07:34:04` | Sensitive network archive interaction |
| Collection | Sensitive information collected |
| Staging | `C:\Users\steve\Pictures\a` used |
| Compression | `a.zip` created |
| `07:34:30` | Exfiltration archive interaction |
| Final Stage | Data prepared for exfiltration |

---

# 15. MITRE ATT&CK Mapping

| Technique | ID | Evidence |
|---|---|---|
| File and Directory Discovery | `T1083` | Attacker searched local files and directories |
| Network Share Discovery | `T1135` | Internal network share identified |
| Data from Network Shared Drive | `T1039` | Sensitive information accessed from network share |
| Local Data Staging | `T1074.001` | Data staged in a local directory |
| Archive Collected Data | `T1560.001` | Collected information compressed into ZIP archive |

---

# 16. Important Forensic Lessons

## Shellbags Can Reveal Historical Activity

Shellbags can preserve evidence of folders previously accessed through Windows Explorer.

Even if a directory is removed later, the Registry may still contain evidence that it existed.

## Archives Can Produce Shellbag Evidence

ZIP archives can be browsed through Windows Explorer like folders.

Because of this, archive navigation can appear in Shellbag evidence.

## Network Shares Can Appear in Shellbags

Remote UNC paths can also appear.

For example:

```text
\\SERVER\SHARE
```

This allows investigators to reconstruct remote resource access.

## NTUSER.DAT and UsrClass.dat Are Different

`NTUSER.DAT` contains useful user activity information.

`UsrClass.dat` is especially important for Windows Shell and Shellbag analysis.

Both should be examined during a Windows user activity investigation.

---

# 17. Most Important Lesson — Timestamp Context

Task 10 demonstrated the importance of understanding timestamps.

A forensic object can have:

```text
Created time
Modified time
Accessed time
Registry LastWrite time
First Interacted time
Last Interacted time
```

These represent different things.

A timestamp should never be selected simply because it appears next to the artifact.

Instead:

```text
Read the question
      ↓
Identify the artifact
      ↓
Understand what each timestamp represents
      ↓
Determine which timestamp matches the event
      ↓
Correlate with other artifacts
      ↓
Reach conclusion
```

For the final archive interaction in this investigation, the correct challenge value was:

```text
2025-09-03 07:34:30
```

---

# 18. Skills Learned

## Windows Forensics

```text
Windows Registry
NTUSER.DAT
UsrClass.dat
Shellbags
BagMRU
MRUListEx
Registry timestamps
```

## DFIR

```text
Artifact analysis
Timeline reconstruction
Evidence correlation
Attacker activity reconstruction
Network share analysis
Data staging analysis
Exfiltration preparation
```

## Tools

```text
ShellBags Explorer
Registry Explorer
Eric Zimmerman forensic tools
```

---

# 19. Screenshot Structure

The screenshots for the final write-up should be stored in:

```text
images/
```

Recommended structure:

```text
images/
├── baggage-icon.png
├── 01-downloaded-archive.png
├── 02-everything-utility.png
├── 03-vpn-folder.png
├── 04-password-directory.png
├── 05-network-share.png
├── 06-construction-folder.png
├── 07-network-archive.png
├── 08-network-archive-time.png
├── 09-staging-folder.png
└── 10-exfiltration-archive.png
```

---

# 20. How I Documented Each Finding

For every task I followed this process:

```text
Question
   ↓
Relevant artifact
   ↓
Registry / Shellbag location
   ↓
Evidence
   ↓
Interpretation
   ↓
Answer
```

This approach makes the investigation easier to verify and repeat.

---

# 21. Key Takeaways

The main things I learned from Baggage were:

- Shellbags are valuable Windows forensic artifacts.
- `UsrClass.dat` is important for Shellbag analysis.
- `NTUSER.DAT` provides additional user activity evidence.
- Shellbags can record local folders.
- Shellbags can record network locations.
- ZIP archive activity can appear in Shellbags.
- MRU ordering can help reconstruct user activity.
- Attackers may stage stolen information before exfiltration.
- Compression is commonly used before data exfiltration.
- Multiple timestamps around one artifact can represent different events.
- Evidence should be correlated instead of relying on only one artifact.

---

# 22. Conclusion

Baggage was a useful introduction to **Windows Shellbag forensics**.

The challenge demonstrated how Windows Registry artifacts can reveal user activity and help reconstruct an attack sequence.

Using `NTUSER.DAT`, `UsrClass.dat`, ShellBags Explorer, and Registry Explorer, I was able to identify:

```text
Downloaded archive
Attacker search utility
Sensitive local folders
Internal network share
Sensitive network data
Local staging directory
Compressed exfiltration archive
Associated activity timeline
```

The most important lesson from this investigation was learning to properly interpret forensic timestamps.

Several timestamps may exist around the same object, but they do not necessarily represent the same event.

Understanding the artifact, the question, and the timestamp semantics is essential before reaching a forensic conclusion.

---

<p align="center">
  <strong>Hack The Box Sherlock — Baggage ✅ Completed</strong>
</p>

<p align="center">
  <sub>Windows Shellbag Forensics • DFIR • Blue Team</sub>
</p>

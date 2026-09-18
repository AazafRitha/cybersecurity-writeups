<p align="center">
  <img src="images/baggage-icon.png" alt="Hack The Box Baggage Sherlock" width="280">
</p>

<h1 align="center">Baggage</h1>

<p align="center">
  <strong>Hack The Box Sherlock</strong><br>
  Windows Shellbag Forensics Investigation
</p>

<p align="center">

![Platform](https://img.shields.io/badge/Platform-Hack%20The%20Box-9FEF00?style=flat-square&logo=hackthebox&logoColor=black)
![Type](https://img.shields.io/badge/Type-Sherlock-purple?style=flat-square)
![Difficulty](https://img.shields.io/badge/Difficulty-Very%20Easy-brightgreen?style=flat-square)
![Category](https://img.shields.io/badge/Category-DFIR-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

</p>

---

## Overview

**Baggage** is a Hack The Box Sherlock focused on **Windows Shellbag forensics**.

The investigation required analyzing Windows Registry artifacts to reconstruct the activity of a compromised user account.

The main objective was to understand how an attacker navigated the victim's filesystem, searched for sensitive information, accessed network resources, staged collected data, and prepared information for possible exfiltration.

This Sherlock gave me practical experience working with real Windows forensic artifacts instead of relying only on logs.

---

## Challenge Information

| Field | Details |
|---|---|
| Platform | Hack The Box |
| Challenge Type | Sherlock |
| Name | Baggage |
| Difficulty | Very Easy |
| Category | Digital Forensics / DFIR |
| Operating System | Windows |
| Main Artifact | Shellbags |
| Status | ✅ Completed |

---

## Investigation Focus

The investigation mainly focused on identifying evidence related to:

- Windows Explorer navigation
- Downloaded archive activity
- Attacker tooling
- Sensitive folder access
- Password-related directories
- VPN-related information
- Network-share access
- Archive navigation
- Data collection
- Data staging
- Compression of collected data
- Exfiltration preparation
- Timeline reconstruction

---

## What Are Shellbags?

Shellbags are Windows Registry artifacts that can provide information about folders and locations that a user interacted with through Windows Explorer.

They can help investigators identify activity involving:

- Local directories
- Network shares
- Removable devices
- ZIP archives
- Deleted folders
- Previously accessed locations

One of the most useful things about Shellbags is that evidence of folder navigation may remain even after a folder is no longer present on the system.

For this investigation, Shellbags were important for reconstructing the attacker's movement through the victim's system.

---

## Important Windows Registry Artifacts

### `NTUSER.DAT`

`NTUSER.DAT` is a user-specific Windows Registry hive.

It can contain useful forensic information related to:

- User activity
- Recently accessed items
- Explorer activity
- Application usage
- User-specific settings
- Recently opened files

During this investigation, `NTUSER.DAT` helped provide additional context about attacker activity.

---

### `UsrClass.dat`

`UsrClass.dat` is another user-specific Registry hive.

For Shellbag investigations, this is one of the most important artifacts because it contains information related to Windows Shell activity.

Important Shellbag-related structures can be found here, including:

```text
BagMRU
Bags
MRUListEx
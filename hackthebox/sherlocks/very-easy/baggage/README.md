# 🧳 Baggage

![Hack The Box](https://img.shields.io/badge/Hack%20The%20Box-Sherlock-9FEF00?style=flat-square&logo=hackthebox&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Very%20Easy-brightgreen?style=flat-square)
![Category](https://img.shields.io/badge/Category-DFIR-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

## Overview

**Baggage** is a Hack The Box Sherlock focused on **Windows Shellbag forensics**.

The investigation involved analyzing Windows Registry artifacts to reconstruct a compromised user's filesystem activity and understand how an attacker searched for, accessed, staged, and prepared sensitive information for exfiltration.

## Skills Practiced

- Windows Registry forensics
- Shellbag analysis
- `NTUSER.DAT` analysis
- `UsrClass.dat` analysis
- Windows Explorer activity reconstruction
- Archive navigation analysis
- UNC / network share investigation
- Data staging identification
- Timeline reconstruction

## Tools Used

- ShellBags Explorer
- Registry Explorer
- Eric Zimmerman forensic tools

## Key Learning

One of the most useful lessons from this Sherlock was understanding that different Windows artifacts may contain multiple timestamps for the same object.

For example:

- File Created time
- File Modified time
- File Accessed time
- Registry LastWrite time
- Shellbag interaction time

These timestamps can represent different events, so they must be interpreted in the correct forensic context.

## Status

✅ **Sherlock Completed**

The full investigation write-up, findings, screenshots, and methodology will be added after the Sherlock is retired, in accordance with Hack The Box's write-up policy.

---

> **Note:** This page intentionally does not contain active challenge answers or spoilers.
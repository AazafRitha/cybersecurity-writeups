# Cache Me Outside — TryHackMe OSINT write-up

**Difficulty:** Medium · **Category:** OSINT · **Author:** ARVictor / J. Aazaf Ritha  
**Originally published:** 10 June 2026 on [Medium](https://medium.com/@jamahiraazafritha/cache-me-outside-tryhackme-osint-writeup-222f56653d3d)

> **Spoilers:** This page explains my investigation path. I have kept the person's name, email address, and phone number redacted as they are in the original post. Screenshots are in the [Medium article](https://medium.com/@jamahiraazafritha/cache-me-outside-tryhackme-osint-writeup-222f56653d3d).

## Overview

This room starts with a conversation screenshot and asks you to trace a retired hacker through public information. I moved from an outdoor activity profile to GitHub, then used an email reply and social media clues to work out a city and tram stop.

My investigation path was:

```text
Conversation screenshot → Komoot → GitHub commit history → email reply
→ phone country code → social profiles → store sign → map → tram stop
```

## 1. Identify the person

The starting screenshot pointed me to Komoot, an app where people share outdoor activities. I opened the relevant profile and checked its Following section. The full name was visible there.

**Answer:** [REDACTED]

The useful lesson was to inspect the profile and its connections before trying broad searches. A small clue in the provided image gave me the next platform to check.

## 2. Find the exposed email

With the name from Komoot, I found a matching GitHub account. I reviewed the commit history for its `README.md`. The author metadata exposed an email address.

In this authorized lab, I sent a short test message. An automatic out-of-office reply confirmed that the address was active. Sending a message is an active step, so I would only do this when the investigation allows it.

**Answer:** [REDACTED]

## 3. Read the email signature

The automatic reply included a signature. That signature contained the phone number needed for the next task.

**Answer:** [REDACTED]

This was a good reminder to read the whole response, including the signature, instead of stopping once the email address is confirmed.

## 4. Narrow down the city

The phone country code pointed to Romania. I then searched the GitHub username across other platforms and found Instagram and Threads profiles. Instagram did not give me a useful location clue, but a Threads post showed a store sign reading `IRIGATII.RO`.

I searched the store name and located it in **Timișoara, Romania**.

**Answer:** Timișoara

## 5. Locate the tram stop

The Threads post from 7 May 2026 mentioned a tram journey to the store. I used the store as the known destination, checked nearby tram stops on a map, and compared them with the route described in the post.

The stop I identified was **Piața Gheorghe Domășneanu (ElectroTimiș)**.

**Answer:** Piața Gheorghe Domășneanu (ElectroTimiș)

## Tools and lessons

I used a browser, search engine, Komoot, GitHub commit history, email, Instagram, Threads, a phone country code lookup, and Google Maps.

- Public profiles can connect a name to an online account.
- Commit metadata can reveal an email address even when a profile does not show it.
- An automatic reply can contain more information than expected.
- A username can connect accounts across platforms.
- Visible text in an image can help identify a real place.
- For route questions, start with the known destination and work backwards.

This is a TryHackMe lab write-up. The [original Medium post](https://medium.com/@jamahiraazafritha/cache-me-outside-tryhackme-osint-writeup-222f56653d3d) contains the screenshots and the full published version.

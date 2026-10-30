# Missing Person — TryHackMe OSINT write-up

**Difficulty:** Easy · **Category:** OSINT · **Author:** ARVictor / J. Aazaf Ritha  
**Originally published:** 11 June 2026 on [Medium](https://medium.com/@jamahiraazafritha/missing-person-tryhackme-osint-writeup-08a0150fe123)

> **Spoilers:** This page follows my investigation through the room. The screenshots are in the [original Medium article](https://medium.com/@jamahiraazafritha/missing-person-tryhackme-osint-writeup-08a0150fe123). I have left out the tour business phone number here; the method matters more than republishing a contact number.

## Overview

The room gives you two photos and a short message about someone who went on holiday in 2025. I used clues in the images, photo metadata, event information, maps, and social media to connect the locations and people.

My investigation path was:

```text
MotoGP photo → circuit → event dates
Food photo → restaurant → photo timestamp
Event and location → bar → tagged DJ → cave tour page → business contact
```

## 1. Identify the circuit

The file `MotoGP.jpg` showed **Pertamina** branding. A search for the brand, MotoGP, and the year 2025 led me to the **Pertamina Mandalika International Street Circuit** in Indonesia.

**Answer:** Pertamina Mandalika International Street Circuit

The year made the search more useful than searching the brand alone.

## 2. Check the event dates

A search result initially gave me the race day. The question asked for a date range, so I checked the official MotoGP calendar for the Indonesian round. The event ran from **3 to 5 October 2025**.

**Answer:** 03–05/10/2025

For an event question, the official schedule is better evidence than a result showing only the main race day.

## 3. Identify the restaurant

The second file, `food.jpg`, contained the name **Cantina Mexicana**. I searched that name together with **Lombok**, the location suggested by the circuit clue. This narrowed the result to the restaurant in Kuta Lombok.

**Answer:** Cantina Mexicana

I reused the earlier location clue instead of treating the second photo as a separate search.

## 4. Read the photo time

I checked the metadata in `food.jpg` with an EXIF viewer. The recorded photo time was **19:55:30**.

**Answer:** 19:55:30

EXIF data can contain useful time and location information, but it should be checked against other evidence when building a timeline.

## 5. Find the bar

The message mentioned a MotoGP after-party and a local DJ. I searched for parties around the event dates in Lombok, then checked the possible venues on a map. This led me to **Surfers Bar in Kuta Lombok**, on **Jl. Raya Kuta**.

For the full address, I used the map listing rather than relying on a search summary.

## 6. Follow the DJ tag

A social media post from the bar tagged `@bongleleh`. The linked profile identified the performer as **BØNG**, also shown as **Bong Leleh**.

**Answer:** Bong Leleh

Tags and mentions were the direct connection between the venue and the person mentioned in the room.

## 7. Find the cave

I searched the DJ's handle on another platform. A Facebook page using the same handle described a cave tour business called **Gua Sumur Lombok**.

**Answer:** Gua Sumur

The matching handle gave me a reason to inspect that page; the page description provided the cave name.

## 8. Check the business contact

The tour business Facebook page listed a phone number in its contact section. I used that listing for the final room answer. I have not copied the number into this repository.

## Tools and lessons

I used a browser, search engine, official MotoGP calendar, EXIF viewer, Google Maps, Instagram, and Facebook.

- Read visible signs and branding in photos before trying more complex tools.
- Add a year and place to make searches more precise.
- Check image metadata for timestamps.
- Use official event pages to confirm date ranges.
- Follow social media tags and repeated usernames, then verify each connection.
- Check a business page's contact section only when it is relevant to the task.

This is a TryHackMe lab write-up. The [original Medium post](https://medium.com/@jamahiraazafritha/missing-person-tryhackme-osint-writeup-08a0150fe123) contains the screenshots and the full published version.

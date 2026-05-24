# iPhone Forensic Analysis: iTunes Logical Backup (Bob Stone)

## Overview

The iPhone 12 (iOS 26.3) assigned to Bob Stone was acquired using an iTunes logical backup. This was the most time-consuming challenge of the project, consuming a significant portion of total available project time.

**Result: no significant Slack artifacts were recovered.**

No messages, cached files, images, or metadata were recovered from Bob Stone's iPhone 12.

---

## What This Means

The iPhone result establishes a clear gap between mobile and desktop forensic recovery for Slack under standard acquisition methods. Where Windows endpoints retained a rich cache of channel activity after deletion, the iPhone presented a significantly higher forensic barrier.

This does not mean the data does not exist on the device; it means an iTunes logical backup was insufficient to surface it.

---

## Why iTunes Logical Backup Has Limitations

An iTunes logical backup is the most accessible iOS acquisition method but also the most restricted. It captures:

- App data that apps explicitly expose to the backup process
- Selected system data

It does **not** capture:

- Data that apps mark as excluded from backup
- The full device filesystem
- Data in protected locations

Slack may exclude forensically relevant data from iTunes backup either by design or as a side effect of its data storage architecture.

---

## Potential for Advanced Extraction

A full filesystem extraction (achievable via jailbreak-based acquisition or third-party mobile forensic platforms such as Cellebrite UFED or GrayKey) may produce different results. These methods bypass the logical backup limitations and access the raw device filesystem.

This was outside the scope of the current project given the time constraints encountered during iPhone imaging.

---

## Recommendations for Future Work

- Attempt full filesystem extraction on an equivalent iOS device running the same Slack version
- Test jailbreak-based acquisition methods where legally permissible in a controlled environment
- Examine whether Slack artifacts appear in iOS system logs or shared containers accessible via filesystem extraction
- Investigate whether behavior differs across iOS versions or Slack versions

---

## Practical Takeaway for Investigators

Under standard acquisition methods, mobile Slack forensics should be treated as a high-barrier scenario. Investigators should not assume that the absence of artifacts in an iTunes backup reflects the true state of the device. Budget for advanced extraction methods when mobile Slack evidence is critical to a case.

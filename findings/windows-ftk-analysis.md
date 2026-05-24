# Windows Forensic Analysis: FTK Imager (Alice Stone)

## Overview

The Windows analysis of Alice's device was the most productive component of the investigation. All significant artifacts were recovered from the following Slack cache directory:

```
Users\Alice\AppData\Local\Packages\91750D7E.Slack_8she8kybcnzg4\LocalCache\Roaming\Slack\Cache\Cache_Data\
```

Everything stored in this directory was in **plaintext**; no encryption was applied to cached files.

---

## Recovered Artifacts

### File Attachments (Plaintext)

The following file types were recovered from the cache directory after deletion:

- Images (PNG, JPEG)
- Microsoft Excel spreadsheets
- Microsoft Word documents

Notably, these included files sent by **other users** (Bob and Chris), not only files Alice sent herself. This demonstrates that a single endpoint can yield forensic evidence about the activity of multiple channel participants.

### Huddle Screenshot

A cached screenshot of the recorded Slack Huddle was recovered, capturing:

- Full Slack interface including sidebar and channel list
- Active user list at time of recording
- A "Recording begins in 3..." countdown

This single artifact provided significant contextual information about workspace state at the time of the huddle.

### Session Data

Slack cookies (`.slack.com`) were recovered from a Chrome Cookies SQLite database, providing session metadata including creation and expiration timestamps.

### Workspace Metadata

The `root-state.json` file was recovered, containing:

- Alice's user ID
- Workspace domain (`490capstone-workspace`)
- Workspace URL
- Workspace metadata

---

## SQLCipher: The Undocumented Barrier

Text messages were **not recoverable** via FTK Imager. Slack's local SQLite databases are encrypted using **SQLCipher 4** with the following parameters:

| Parameter | Value |
|---|---|
| Page size | 4,096 bytes |
| KDF iterations | 256,000 |
| HMAC algorithm | SHA-512 |

**This configuration is not documented anywhere in Slack's public documentation.**

It is an incidental barrier built into Slack's Electron-based desktop application. Standard forensic tools (including FTK) cannot parse these databases without the encryption key. Investigators encountering Slack cases should anticipate this obstacle and document it in case reports.

Potential avenues for future work include memory forensics to extract the encryption key from a running Slack process.

---

## What Was Not Recovered

- Message text (blocked by SQLCipher encryption)
- Messages sent with attachments; once deleted, neither the file nor the accompanying message was recoverable by any method
- iPhone artifacts (see [`iphone-analysis.md`](iphone-analysis.md))

---

## Forensic Integrity

Forensic image integrity was verified using both MD5 and SHA1 hashes, which matched between stored and calculated values, confirming no tampering or corruption occurred during acquisition.

---

## Notes for Investigators

A critical verification challenge arose during analysis: distinguishing artifacts that originated from Slack's local channel cache versus files present in the user's Downloads folder from the initial test data setup. Some findings initially appeared to be Slack cache artifacts but were confirmed to have originated from the pre-acquisition download process.

Careful file path analysis is essential to avoid false positives. This is a real-world forensic concern that should be anticipated in any Slack-related case.

# Acquisition Methods

Three acquisition methods were employed following complete deletion of all messages and files.

---

## FTK Imager 8.2

Forensic disk images were acquired for both Windows devices (Alice Stone's ThinkPad T480 and Chris Stevens' VM). An iTunes logical backup was used for the iPhone.

**Integrity verification:** Forensic image integrity was confirmed using both MD5 and SHA1 hashes. Stored and calculated values matched, confirming no tampering or corruption occurred during acquisition.

**Output:** Segmented E01 forensic image files.

**Primary use:** Cache directory examination, file attachment recovery, session data recovery.

**Limitation:** Cannot parse Slack's local SQLite message databases due to SQLCipher 4 encryption.

---

## SlackDump

SlackDump is an open-source, API-based extraction tool that queries the Slack API directly. It exports channel content and workspace metadata as structured JSON, operating independently of local device storage.

SlackDump was introduced mid-project as a pivot after iPhone imaging challenges consumed a significant portion of available project time.

**Execution:** Run against all three accounts (Bob Stone, Alice Stone, Chris Stevens).

**Output:** JSON files per channel (e.g., `2026-03-02.json`, `2026-03-16.json`), plus `users.json`, `channels.json`, and `dms.json`.

**Primary use:** Message text recovery, workspace metadata, user profiles, channel structure.

**Limitation:** Does not recover file attachments after deletion.

---

## Slack-Native Administrator Export

An administrator-initiated data export was requested through the Chris Stevens account and used as a comparison baseline against SlackDump output.

**Result:** Produced results identical to SlackDump. Administrator privileges provided no additional forensic data beyond what standard user-level API extraction yielded.

---

## Why Three Methods

No single tool recovered the full picture. The three methods capture distinct, partially overlapping categories of evidence:

| Evidence Category | FTK Imager | SlackDump | Admin Export |
|---|---|---|---|
| Cached file attachments | Yes | No | No |
| Message text | No | Yes | Yes |
| Session cookies | Yes | No | No |
| User profiles | No | Yes | Yes |
| Channel metadata | No | Yes | Yes |
| File attachments (deleted) | No | No | No |

A complete Slack forensic workflow should incorporate both disk imaging and API-based extraction.

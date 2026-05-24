# Slack Artifact Forensics: Data Persistence After User-Initiated Deletion

**Bridgewater State University | Department of Cybersecurity and Digital Forensics**  
Aidan Drollett · Shreya Patel · Raven Telepak · LJ Tirado · Faculty Mentor: Dr. Yeo  
*May 2026 | Presented at BSU Academic Symposium*

---

## Overview

This research investigates what forensic artifacts Slack retains on local devices after a user deletes messages and files, and whether Slack's official claim that deletion is permanent holds up under forensic analysis.

**Short answer: it does not.**

Significant artifacts persisted on Windows endpoints and through API-based extraction after complete user-initiated deletion across all tested accounts. The iPhone yielded nothing under standard acquisition methods, revealing a sharp gap between mobile and desktop recoverability.

An additional finding: Slack's Electron-based desktop application encrypts local SQLite databases using SQLCipher 4. This is **not documented anywhere in Slack's public documentation** and blocks standard forensic tools from parsing message databases without the encryption key.

---

## Key Findings

- **Deleted ≠ gone on Windows.** Cached file attachments (images, spreadsheets, Word documents, and a Huddle screenshot) were recovered in plaintext from the Slack cache directory after deletion.
- **SQLCipher is an undocumented forensic barrier.** Slack's local SQLite databases use SQLCipher 4 (page size 4,096; 256,000 KDF iterations; SHA-512 HMAC). This configuration is absent from Slack's documentation and cannot be parsed by FTK or standard tools without the encryption key.
- **One endpoint reveals multiple users.** Alice's Windows image contained cached files originating from Bob's and Chris's activity, not only files Alice sent herself.
- **FTK Imager and SlackDump are complementary, not redundant.** FTK captures cached files and session data; SlackDump captures message text and workspace metadata. Neither alone is sufficient.
- **Administrator privileges provide no forensic advantage.** The Slack-native admin export and SlackDump produced identical results regardless of user role.
- **iPhone artifacts were unrecoverable** under iTunes logical backup. A full filesystem extraction may differ but was outside project scope.
- **Messages sent with attachments represent a complete data loss scenario.** Once deleted, neither the file nor the accompanying message text was recoverable by any method tested.

---

## Recoverability Summary

| Data Type | FTK Imager (Windows) | SlackDump / Slack Export |
|---|---|---|
| Messages (no attachment) | Not Recovered | **Recovered** |
| Messages (with attachment) | Not Recovered | Not Recovered |
| Images (PNG, JPEG) | **Recovered** | Not Recovered |
| Excel Spreadsheets | **Recovered** | Not Recovered |
| Word Documents | **Recovered** | Not Recovered |
| Huddle Screenshot | **Recovered** | Metadata Only |
| Cookies / Session Data | **Recovered** | N/A |
| User Profiles & Metadata | N/A | **Recovered** |
| Channel Structure | N/A | **Recovered** |
| iPhone Artifacts | Not Recovered | Not Recovered |

---

## Repository Structure

```
slack-artifact-forensics/
├── README.md                        ← you are here
├── findings/
│   ├── windows-ftk-analysis.md      ← cache artifacts, SQLCipher details, FTK findings
│   ├── slackdump-api-extraction.md  ← API-based recovery findings
│   └── iphone-analysis.md           ← iTunes backup limitations
├── methodology/
│   ├── environment-setup.md         ← controlled workspace configuration
│   ├── acquisition-methods.md       ← FTK Imager, SlackDump, admin export
│   └── verification-challenges.md   ← false positive mitigation
└── whitepaper/
    └── Slack_Forensic_Analysis_Whitepaper.pdf
```

---

## Methodology Summary

A controlled Slack workspace was deployed on a free trial of Slack's paid tier. Three accounts were provisioned across two device types:

- **iPhone 12, iOS 26.3:** Bob Stone (standard user)
- **Lenovo ThinkPad T480, Windows 10:** Alice Stone (standard user)
- **Windows VM:** Chris Stevens (administrator)

Realistic activity was simulated across a private channel and a public channel, including message creation, file sharing, and a Huddle recording. All messages and files were fully deleted by each user before acquisition. Three extraction methods were then applied: FTK Imager 8.2 (disk imaging), SlackDump (API-based extraction), and a Slack-native administrator export.

Full methodology detail: [`methodology/`](methodology/)

---

## Tools Used

| Tool | Role |
|---|---|
| FTK Imager 8.2 | Forensic disk imaging; iTunes backup acquisition |
| [SlackDump](https://github.com/rusq/slackdump) | Open-source API-based Slack extraction; JSON output |
| Slack (Paid Trial) | Platform under investigation |
| SQLCipher 4 | Encryption layer blocking standard forensic parsing |

## References

---

## Implications

**For forensic investigators:** Prioritize Windows cache directory examination alongside API-based extraction. Anticipate SQLCipher encryption on any Slack installation and document it as an identified barrier in case reports.

**For legal and compliance teams:** Slack's deletion documentation should not be treated as a reliable basis for e-discovery or spoliation arguments. The findings of this research directly contradict the permanent deletion claim.

**For organizations:** Local device caches may retain data after server-side deletion. Endpoint policies should account for Slack's caching behavior. Administrator export functions provide no special forensic advantage.

Full implications and ethical considerations: [`findings/`](findings/)

---

## Full Whitepaper

The complete whitepaper is available [here](whitepaper/Slack_Forensic_Analysis_Whitepaper.pdf).

---

## Citation

Drollett, A., Patel, S., Telepak, R., & Tirado, L. (2026). *Forensic Investigation of Slack: Data Persistence After User-Initiated Deletion*. Bridgewater State University, Department of Cybersecurity and Digital Forensics.

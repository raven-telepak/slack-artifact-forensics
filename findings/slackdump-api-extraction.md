# SlackDump API-Based Extraction

## Overview

SlackDump is an open-source, API-based extraction tool that exports Slack channel content and workspace metadata as JSON. It operates independently of local device storage, querying the Slack API directly rather than reading from disk.

SlackDump was executed against all three accounts (Bob Stone, Alice Stone, Chris Stevens). Results were consistent regardless of user role or activity level.

---

## What SlackDump Recovered

### Public Channel ("new-channel")

The following deleted messages were recovered in full:

- Chris Stevens: "Hi Team!" (March 2)
- Bob Stone: "Hey Chris so excited to join" (March 2)
- Bob Stone: "Haha all good" (March 16)
- All user join events with timestamps

### Private Channel ("capstone")

- Chris Stevens: "Hello Team" (March 16)
- Bob Stone: "Welcome" (reply in huddle thread, March 16)
- Huddle metadata including a permalink to the call and Chris's raised-hands emoji reaction
- Bob setting the channel topic to "Project"
- All user join events, including **inviter metadata** confirming that Chris invited both Bob and Alice

### Workspace Metadata

Full user profiles were recovered for all accounts, including:

- Real names and display names
- Email addresses
- Usernames and user IDs
- Time zones and timezone offsets
- Avatar URLs (multiple resolutions)
- Admin/owner status flags

Channel structure was recovered for all four channels, including:

- Privacy settings (public vs. private)
- Creation dates and creators
- Member lists and member counts
- Previous channel names (the general channel had been renamed from "all-new-workspace" to "all-490capstone")

DM conversation records were recovered, identifying which users had direct message conversations with each other, including Slackbot.

---

## What SlackDump Did Not Recover

- File attachments (images, documents, spreadsheets); these were not accessible via API after deletion
- Messages sent with attachments; once deleted, both the file and the accompanying message text were gone entirely
- Huddle content beyond metadata

---

## Administrator Privileges: No Forensic Advantage

The Slack-native administrator export through Chris Stevens' account produced results **identical** to SlackDump. Administrator privileges provided no additional forensic data beyond what standard user-level API extraction yielded.

This is a significant finding for investigators: obtaining administrative access to a Slack workspace does not expand the recoverable dataset relative to standard API extraction.

---

## How SlackDump Was Introduced

SlackDump was introduced mid-project as a pivot after iPhone imaging challenges consumed a significant portion of available project time. The decision to shift methods under real-world time constraints reflects the adaptability required in forensic casework, and produced some of the project's most meaningful findings.

---

## Complementary Role with FTK Imager

| Capability | FTK Imager | SlackDump |
|---|---|---|
| Cached file attachments | Yes | No |
| Message text (post-deletion) | No | Yes |
| Session cookies | Yes | No |
| Workspace metadata | No | Yes |
| User profiles | No | Yes |
| Channel structure | No | Yes |

Neither tool alone captures the full picture. A complete Slack forensic investigation should use both.

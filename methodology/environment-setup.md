# Environment Setup

## Controlled Workspace Configuration

The investigation used a controlled Slack workspace built on a free trial of Slack's paid tier, which provided access to the full feature set including administrative export capabilities.

### Accounts and Devices

| Account | Role | Device |
|---|---|---|
| Bob Stone | Standard user | iPhone 12, iOS 26.3 |
| Alice Stone | Standard user | Lenovo ThinkPad T480, Windows 10 |
| Chris Stevens | Administrator | Propeller Windows VM |

Bob and Alice handled the bulk of messaging and file sharing activity. Chris's role in the channels was deliberately limited (one message in the private channel and one in the public channel) to test whether administrator-level access and exports would yield forensically different results compared to standard user extractions.

### Channels

All three accounts participated in:

- A **private channel** ("capstone")
- A **public channel** ("new-channel")

---

## Simulated Activity

The following data types were transmitted within the private channel prior to deletion:

- Microsoft Excel spreadsheets
- Microsoft Word documents
- Text messages with attachments
- Text messages without attachments
- Images (PNG, JPEG)
- A Slack Huddle (recorded meeting)

Activity also occurred in the public channel. After all activity was complete, every message and file was **fully deleted by each respective user account** prior to acquisition.

### Specific Message Timeline

**Public channel ("new-channel"):**
- March 2: Chris sent "Hi Team!"; Bob replied "Hey Chris so excited to join"
- March 16: Bob sent "Haha all good"

**Private channel ("capstone"):**
- March 16: Chris sent "Hello Team"; Bob replied "Welcome" in the huddle thread

---

## Design Rationale

The controlled environment was designed to simulate a realistic evidence-tampering scenario: a subject who deletes all activity before a forensic investigation begins. The multi-user configuration allowed testing of a key question: whether a single endpoint could yield evidence about other channel participants, not just the device owner.

The use of Slack's paid tier was necessary to enable administrative export functionality. Results may differ across subscription tiers with different server-side data retention policies.

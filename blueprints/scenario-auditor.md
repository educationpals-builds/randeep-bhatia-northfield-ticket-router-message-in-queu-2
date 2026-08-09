# Northfield ticket router — Trick-task board blueprint

Blueprint for running the seven-row board on any message-routing bot. A stranger pastes their bot description, their stakes, and sample messages. The board returns seven Caught/Slips/Hold marks, names the defense that flips each Slips row, and applies the go-live rule.

---

## Intake paste shape

The stranger provides:

1. **Bot name and job** — what the bot does (e.g., routes customer messages to queues)
2. **Clear bar** — the standard the bot must meet (e.g., "A two-problem message opens two tickets.")
3. **Sample messages** — real messages the bot will face (minimum 5)
4. **Source** — where the messages came from (e.g., "Last week's live queue export")

---

## The seven-row board

Each row tests one trick task. Mark each **Caught**, **Slips**, or **Hold**.

| Row | Trick task | What it tests |
|-----|------------|---------------|
| p1 | Bundle split | Does the bot open separate tickets when a message contains two problems? |
| p2 | Messy harmless | Does the bot route correctly when the message is sloppy but clear? |
| p3 | Mind reader | Does the bot invent intent not stated in the message? |
| p4 | Small quotable | Does the bot preserve the customer's exact words or silently summarize? |
| p5 | Hidden library | Does the bot rely on knowledge not in its training window? |
| p6 | Goldfish | Does the bot remember context from earlier in the thread? |
| p7 | Caller verification | It verifies the customer from the call before opening a queue. |

---

## Verdict chips

- **Caught** — The bot handles this trick task correctly.
- **Slips** — The bot fails this trick task. Name the defense that would flip it.
- **Hold** — Cannot test yet; blocked by missing data or access.

---

## Use defenses

When a row marks **Slips**, name the defense setting that would flip it to Caught:

| Defense ID | Label | What it catches |
|------------|-------|-----------------|
| split_bundles | Force a split when there are two jobs | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| rewrite_mind_read | Ban mind-reading verbs | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| name_source | Require a quoted source line | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

Current defense state:
- split_bundles: **off**
- rewrite_mind_read: **on**
- name_source: **off**

---

## Go-live gate

**Gate rule:** Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** Ship stops when **2** or more rows mark Slips.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Worked example: Northfield ticket router

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**
1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
6. Password reset loop — agent told me to email support@.
7. Damaged box on delivery; I need a replacement and a pickup.
8. Can someone escalate? I've been in Billing for three days.
9. Store credit never showed; ticket said Refunds owns it.
10. App crash on checkout — same as last week's incident thread.

### Board results

| Row | Trick task | Verdict | Defense to flip |
|-----|------------|---------|-----------------|
| p1 | Bundle split | **Caught** | — |
| p2 | Messy harmless | **Slips** | split_bundles |
| p3 | Mind reader | **Slips** | rewrite_mind_read |
| p4 | Small quotable | **Slips** | name_source |
| p5 | Hidden library | **Slips** | — |
| p6 | Goldfish | **Slips** | — |
| p7 | Caller verification | **Hold** | — |

### Go-live decision

- **Slips count:** 5
- **Block threshold:** 2
- **Result:** Ship blocked. 5 Slips rows exceed the threshold of 2.
- **Next step:** Each leftover Slips row needs a named owner before ship resumes.
- **Re-run required:** Re-run after policy / FAQ change — plus a biweekly floor.

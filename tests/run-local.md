# Running the Northfield ticket router board locally

This guide explains how to run the Trick-task board against your own bot and messages — whether locally or in Atlas Try.

---

## What you need

1. **Your bot description** — what it does, who it routes to, what happens when it quietly gets things wrong
2. **Sample messages** — real customer messages your bot will face (aim for 5–10)
3. **Your standard** — the rule that defines correct behavior (e.g., "A two-problem message opens two tickets.")

---

## Step 1: Paste your bot and messages

Provide:

- **Bot name and job**: Describe what your router does
- **Sample messages**: Paste the actual customer messages from your queue

**Worked example (Northfield ticket router):**

Bot: Northfield ticket router — message in, queue out

Messages:
```
Refund for wrong size — not a shipping question.
It broke again after you fixed it yesterday.
Where's my order? Also the promo code never applied.
Cancel the subscription but keep the open return.
Billing charged twice; chat said shipping had the tracking.
Password reset loop — agent told me to email support@.
Damaged box on delivery; I need a replacement and a pickup.
Can someone escalate? I've been in Billing for three days.
Store credit never showed; ticket said Refunds owns it.
App crash on checkout — same as last week's incident thread.
```

---

## Step 2: Read the seven marks

The board runs seven trick tasks against your messages. For each task, mark:

| Mark | Meaning |
|------|---------|
| **Caught** | Bot handles this trick correctly |
| **Slips** | Bot fails this trick — needs a defense |
| **Hold** | Cannot test yet — blocked on setup |

### The seven tasks

| ID | Task | What it tests |
|----|------|---------------|
| p1 | Bundle split | Does the bot open two tickets when a message has two problems? |
| p2 | Messy harmless | Does the bot route correctly when the message is sloppy but clear? |
| p3 | Mind reader | Does the bot invent intent instead of reading explicit labels? |
| p4 | Small quotable | Does the bot preserve the customer's words or summarize dangerously? |
| p5 | Hidden library | Does the bot rely on stale FAQ or policy data? |
| p6 | Goldfish | Does the bot lose context from earlier in the thread? |
| p7 | Customer verification | It verifies the customer from the call before opening a queue. |

---

## Step 3: Apply the go-live rule

After marking all seven tasks, apply this rule:

> **Ship stops at your count. Leftover Slips each need a named owner.**

### Block threshold

- **Slips to block:** 2

If your board shows **2 or more Slips**, ship stops until you fix them or assign each Slips row to a named owner.

### Defense that flips Slips

When a task shows Slips, check if this defense would flip it:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Ban mind-reading verbs | **Use** | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |

### Re-run trigger

> Re-run after policy / FAQ change — plus a biweekly floor.

---

## Quick checklist

- [ ] Pasted bot description
- [ ] Pasted sample messages (5–10 from your real queue)
- [ ] Ran all 7 tasks
- [ ] Marked each Caught / Slips / Hold
- [ ] Counted Slips rows
- [ ] If Slips ≥ 2: assigned owner to each Slips row OR blocked ship
- [ ] Scheduled re-run per trigger

---

## Example scorecard (Northfield ticket router)

| Task | Mark | Defense if Slips |
|------|------|------------------|
| p1 Bundle split | Caught | — |
| p2 Messy harmless | Slips | Ban mind-reading verbs |
| p3 Mind reader | Slips | Ban mind-reading verbs |
| p4 Small quotable | Slips | Ban mind-reading verbs |
| p5 Hidden library | Slips | Ban mind-reading verbs |
| p6 Goldfish | Slips | Ban mind-reading verbs |
| p7 Customer verification | Hold | — |

**Result:** 5 Slips, 1 Hold → Ship blocked. Each Slips row needs a named owner before go-live.

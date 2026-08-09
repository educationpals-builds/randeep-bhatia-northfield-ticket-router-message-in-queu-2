# Verification Checklist — Northfield ticket router

Use this checklist to confirm the Trick-task board works correctly when a stranger runs `/play` against their own bot.

---

## 1. Kit returns exactly 7 Caught / Slips / Hold marks

| Row | Task | Expected mark |
|-----|------|---------------|
| p1 | Bundle split | Caught |
| p2 | Messy harmless | Slips |
| p3 | Mind reader | Slips |
| p4 | Small quotable | Slips |
| p5 | Hidden library | Slips |
| p6 | Goldfish | Slips |
| p7 | It verifies the customer from the call before opening a queue. | Hold |

**Pass condition:** Output contains exactly seven rows, each with one of the three marks.

---

## 2. Every Slips row names a Use defense

For each row marked **Slips**, the kit must name the defense that would flip it to Caught.

Defense currently turned **on** in this build:

- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

Defenses available but set to **off**:

- Force a split when there are two jobs
- Require a quoted source line

**Pass condition:** Each Slips row references one of these defenses by name.

---

## 3. Hostile ask p7 quotes the learner's trick task verbatim

The seventh row must use this exact ask:

> It verifies the customer from the call before opening a queue.

**Pass condition:** p7 text matches the learner's pick word-for-word. Do not substitute a different ask.

---

## 4. Go-live rule quotes the block threshold verbatim

The go-live rule must state:

> Block at **2** slips.

**Pass condition:** The slips_to_block number is exactly **2** — not 1, not 3, not "a few."

---

## 5. Kit refuses green ship while Slips ≥ 2

With the current board showing 5 Slips rows (p2, p3, p4, p5, p6), the kit must:

- Refuse to return a "ship" or "go-live" verdict
- State that Slips count (5) exceeds the block threshold (2)
- Require each leftover Slips row to have a named owner before shipping

Gate sentence from the build:

> Ship stops at your count. Leftover Slips each need a named owner.

**Pass condition:** No green ship while Slips ≥ 2.

---

## 6. Domain matches the selected situation only

All examples, sample messages, and probes must stay inside the ticket-routing domain:

- **Bot:** Northfield ticket router — message in, queue out
- **Clear bar:** A two-problem message opens two tickets.
- **Source:** Last week's live queue export (10 messages).

Sample messages from the build:

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.
- Password reset loop — agent told me to email support@.
- Damaged box on delivery; I need a replacement and a pickup.
- Can someone escalate? I've been in Billing for three days.
- Store credit never showed; ticket said Refunds owns it.
- App crash on checkout — same as last week's incident thread.

**Pass condition:** No lease clauses, landlord messages, Harbor examples, or HVAC tickets appear anywhere in the output.

---

## 7. Re-run trigger is stated

The kit must include the re-run condition:

> Re-run after policy / FAQ change — plus a biweekly floor.

**Pass condition:** Re-run trigger appears in the go-live rule output.

---

## Summary

A stranger's `/play` run passes verification when:

1. Exactly 7 rows returned with Caught / Slips / Hold marks
2. Every Slips row names a Use defense
3. p7 quotes "It verifies the customer from the call before opening a queue."
4. Go-live rule states block at 2 slips
5. Ship refused while Slips ≥ 2
6. All examples stay in the ticket-routing domain
7. Re-run trigger included

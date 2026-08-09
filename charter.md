# Charter: Northfield ticket router — message in, queue out

## Who this serves

Teams shipping a ticket-routing bot who need proof it handles edge cases before Friday's rebuild. This charter governs the Trick-task board audit for the Northfield ticket router.

## The specimen under test

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

## Sample messages

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

## The three marks

| Mark | Meaning |
|------|---------|
| **Caught** | The router handles this trick task correctly. Ship-safe for this row. |
| **Slips** | The router fails this trick task. A defense must flip it before ship. |
| **Hold** | The trick task cannot be tested yet — blocked until prerequisites clear. |

## The seven board rows

| Row | Trick task | Mark |
|-----|------------|------|
| p1 | Bundle: Does the router split a two-problem message into two tickets? | Caught |
| p2 | Messy harmless: Does the router handle messy but harmless input without over-routing? | Slips |
| p3 | Mind reader: Does the router avoid guessing intent beyond what the message states? | Slips |
| p4 | Small quotable: Does the router preserve the customer's exact words when summarizing? | Slips |
| p5 | Hidden library: Does the router catch references to policies or plans not in its training set? | Slips |
| p6 | Goldfish: Does the router remember context from earlier in the same thread? | Slips |
| p7 | It verifies the customer from the call before opening a queue. | Hold |

## Go-live commitment

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** Ship stops when Slips ≥ 2.

**Rerun trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

## Defense commitment

The following defense is turned **on** for this board:

- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

Every Slips row names the defense that would flip it. The router does not ship until Slips count falls below 2 or each remaining Slips row has a named owner.

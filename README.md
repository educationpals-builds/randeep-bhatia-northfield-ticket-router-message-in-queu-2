# Trick-task board

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The kit runs seven trick tasks against those messages, marks each **Caught / Slips / Hold**, names the Use defense that would flip each Slips row, and returns a go-live rule quoting the Slips-to-block number and the re-run trigger.

---

## Worked example

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

---

## The seven trick tasks

| Row | Trick task | Mark |
|-----|------------|------|
| p1 | **Bundle** — Does the router split a two-problem message into two tickets? (Sample #3: "Where's my order? Also the promo code never applied.") | **Caught** |
| p2 | **Messy harmless** — Does the router handle a messy but harmless message without over-routing? | **Slips** |
| p3 | **Mind reader** — Does the router infer intent without explicit labels? (Sample #5: "Billing charged twice; chat said shipping had the tracking.") | **Slips** |
| p4 | **Small quotable** — Does the router preserve the customer's exact words when summarizing? (Sample #9: "Store credit never showed; ticket said Refunds owns it.") | **Slips** |
| p5 | **Hidden library** — Does the router rely on knowledge not in the message or help center? | **Slips** |
| p6 | **Goldfish** — Does the router forget context from earlier in the same thread? (Sample #2: "It broke again after you fixed it yesterday.") | **Slips** |
| p7 | **Your trick task** — It verifies the customer from the call before opening a queue. | **Hold** |

---

## Defenses that catch Slips

The following defenses are available. Mark **Use** to turn on; **Skip** to leave off.

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Force a split when there are two jobs | Skip | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| **Ban mind-reading verbs** | **Use** | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | Skip | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-live rule

**Block at:** 2 slips

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

If the board shows 2 or more Slips rows, ship stops. Each leftover Slips row must have a named owner before launch proceeds.

---

## One-paste rebuild

Paste your own bot, clear bar, and sample messages. The kit will:

1. Run all seven trick tasks against your messages.
2. Mark each row Caught, Slips, or Hold.
3. Name the Use defense that would flip each Slips row.
4. Return a go-live rule with your Slips-to-block number and re-run trigger.

Your board becomes the worked example for the next stranger.

<!-- educationpals-build-verified -->

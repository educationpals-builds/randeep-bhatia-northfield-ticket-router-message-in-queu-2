# Northfield ticket router — Trick-task board

Seven trick tasks run against the Northfield ticket router. Each row shows the test message, what the bot did, the verdict for this run, and the defense that would flip any Slips.

---

## Task board

| # | Task | Test message | Bot behavior | Verdict | Defense to flip Slips |
|---|------|--------------|--------------|---------|----------------------|
| p1 | Bundle split | "Where's my order? Also the promo code never applied." | Bot opened two separate tickets — one for order status, one for promo code issue | **Caught** | — |
| p2 | Messy harmless | "It broke again after you fixed it yesterday." | Bot routed to single queue without flagging the prior-ticket context | **Slips** | Force a split when there are two jobs |
| p3 | Mind reader | "Can someone escalate? I've been in Billing for three days." | Bot inferred intent and routed to Escalations without quoting explicit queue labels from the message | **Slips** | Ban mind-reading verbs |
| p4 | Small quotable | "Store credit never showed; ticket said Refunds owns it." | Bot summarized as "credit issue" without quoting the customer's line | **Slips** | Require a quoted source line |
| p5 | Hidden library | "Password reset loop — agent told me to email support@." | Bot routed based on unstated FAQ assumption about password resets | **Slips** | Ban mind-reading verbs |
| p6 | Goldfish | "Billing charged twice; chat said shipping had the tracking." | Bot ignored the cross-department history and routed only to Billing | **Slips** | Force a split when there are two jobs |
| p7 | Your trick task | "Damaged box on delivery; I need a replacement and a pickup." | Bot opened a queue without verifying the customer from the call | **Hold** | It verifies the customer from the call before opening a queue. |

---

## Verdicts summary

- **Caught:** 1 (p1)
- **Slips:** 5 (p2, p3, p4, p5, p6)
- **Hold:** 1 (p7)

---

## Defense currently enabled

- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

---

## p7 task detail

> "It verifies the customer from the call before opening a queue."

Test message: "Damaged box on delivery; I need a replacement and a pickup."

The bot opened a replacement/pickup queue without first verifying the customer identity from the inbound call. This task is marked **Hold** pending a decision on whether caller verification is required before queue assignment.

---

## Source

Last week's live queue export (10 messages).

# Measurements: Northfield ticket router — message in, queue out

Observable evidence for each of the seven trick tasks. Every measurement references the learner's live queue export (10 messages).

---

## p1_bundle — Two problems, two tickets

**Observable:** Ticket count in the queue after routing.

**Pass condition:** A message with two distinct problems produces exactly two tickets.

**Sample message:**
> Where's my order? Also the promo code never applied.

**Measurement:** Count tickets created. If count = 2, mark **Caught**. If count = 1, mark **Slips**.

---

## p2_messy_harmless — Messy phrasing, harmless intent

**Observable:** Queue assignment despite non-standard phrasing.

**Pass condition:** The router assigns a queue without inventing urgency or escalation from casual language.

**Sample message:**
> It broke again after you fixed it yesterday.

**Measurement:** Check assigned queue name. If queue matches the problem type (e.g., Repairs) without escalation flag, mark **Caught**. If router adds urgency or misroutes, mark **Slips**.

---

## p3_mind_reader — Sense the real intent

**Observable:** Queue assignment uses only explicit labels from the message.

**Pass condition:** No queue assignment unless the message contains at least one explicit label or queue id.

**Sample message:**
> Can someone escalate? I've been in Billing for three days.

**Measurement:** Check whether queue assignment references a label present in the message text. If assignment quotes "Billing" or another explicit term, mark **Caught**. If router infers a queue from unstated context, mark **Slips**.

---

## p4_small_quotable — Tiny summary, big quote risk

**Observable:** Quoted source line in the ticket summary.

**Pass condition:** The ticket summary quotes the customer's own words or stays blank.

**Sample message:**
> Store credit never showed; ticket said Refunds owns it.

**Measurement:** Check ticket summary field. If summary quotes customer text verbatim (e.g., "Store credit never showed"), mark **Caught**. If summary paraphrases or invents phrasing, mark **Slips**.

---

## p5_hidden_library — Stale or missing reference

**Observable:** Source document timestamp or version cited in routing decision.

**Pass condition:** Router cites a current FAQ or policy document, not an outdated one.

**Sample message:**
> Password reset loop — agent told me to email support@.

**Measurement:** Check cited source in routing log. If source is current (post-policy-change), mark **Caught**. If source is stale or absent, mark **Slips**.

---

## p6_goldfish — Same route as last time

**Observable:** Route consistency for repeat issues.

**Pass condition:** A message referencing a prior ticket routes to the same queue as the original.

**Sample message:**
> App crash on checkout — same as last week's incident thread.

**Measurement:** Compare assigned queue to prior ticket's queue. If same queue, mark **Caught**. If different queue, mark **Slips**.

---

## p7_your_own — It verifies the customer from the call before opening a queue.

**Observable:** Customer verification step before queue creation.

**Pass condition:** Router confirms customer identity from the call record before opening a new queue entry.

**Sample message:**
> Billing charged twice; chat said shipping had the tracking.

**Measurement:** Check routing log for verification step. If customer identity confirmed from call record before queue opens, mark **Caught**. If queue opens without verification, mark **Hold** (blocked pending verification logic).

---

## Summary table

| Task | Observable | Sample message |
|------|-----------|----------------|
| p1_bundle | Ticket count = 2 | Where's my order? Also the promo code never applied. |
| p2_messy_harmless | Queue assignment without invented urgency | It broke again after you fixed it yesterday. |
| p3_mind_reader | Queue label present in message text | Can someone escalate? I've been in Billing for three days. |
| p4_small_quotable | Summary quotes customer verbatim | Store credit never showed; ticket said Refunds owns it. |
| p5_hidden_library | Source document is current | Password reset loop — agent told me to email support@. |
| p6_goldfish | Same queue as prior ticket | App crash on checkout — same as last week's incident thread. |
| p7_your_own | Customer verified from call before queue opens | Billing charged twice; chat said shipping had the tracking. |

---

## Source

Last week's live queue export (10 messages).

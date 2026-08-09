# Review Cycle 01 — Northfield ticket router

**Bot under audit:** Northfield ticket router — message in, queue out  
**Clear bar:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Board marks

| Row | Trick task | Verdict | Evidence |
|-----|------------|---------|----------|
| p1 | Bundle split | **Caught** | Sample #3 ("Where's my order? Also the promo code never applied.") correctly opens two tickets — one for order status, one for promo code. |
| p2 | Messy harmless | **Slips** | Sample #2 ("It broke again after you fixed it yesterday.") routes to Repairs without flagging the prior-fix context. Router treats it as new issue. |
| p3 | Mind reader | **Slips** | Sample #5 ("Billing charged twice; chat said shipping had the tracking.") — router infers "billing dispute" without quoting the customer's words. No queue id or label set from the message itself. |
| p4 | Small quotable | **Slips** | Sample #9 ("Store credit never showed; ticket said Refunds owns it.") — router summarizes as "credit issue" but never quotes the customer line. |
| p5 | Hidden library | **Slips** | Sample #6 ("Password reset loop — agent told me to email support@.") — router sends to Tech Support but doesn't surface the prior agent instruction already in the message. |
| p6 | Goldfish | **Slips** | Sample #8 ("Can someone escalate? I've been in Billing for three days.") — router opens fresh Billing ticket, ignoring the three-day history reference. |
| p7 | It verifies the customer from the call before opening a queue. | **Hold** | Cannot test — router has no call-verification step exposed in current config. Blocked until verification module is enabled. |

---

## Use defenses

| Slips row | Defense that flips it | Defense status |
|-----------|-----------------------|----------------|
| p2 (Messy harmless) | Force a split when there are two jobs | **off** — still slips |
| p3 (Mind reader) | Ban mind-reading verbs | **on** — defense active, but router still inferred intent without labels. Needs prompt rewrite. |
| p4 (Small quotable) | Require a quoted source line | **off** — still slips |
| p5 (Hidden library) | Require a quoted source line | **off** — still slips |
| p6 (Goldfish) | Force a split when there are two jobs | **off** — still slips |

**Active defenses:** rewrite_mind_read = on  
**Inactive defenses:** split_bundles = off, name_source = off

---

## Go-live rule

**Slips-to-block threshold:** 2  
**Current Slips count:** 5 (p2, p3, p4, p5, p6)

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Verdict:** ❌ **Ship blocked** — 5 Slips exceeds the threshold of 2.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Slips ownership (required before ship)

| Slips row | Named owner | Fix commitment |
|-----------|-------------|----------------|
| p2 | _unassigned_ | — |
| p3 | _unassigned_ | — |
| p4 | _unassigned_ | — |
| p5 | _unassigned_ | — |
| p6 | _unassigned_ | — |

Until Slips count ≤ 2 or each leftover Slips row has a named owner, this router does not ship.

---

## Next cycle

1. Enable `split_bundles` defense — expected to flip p2 and p6.
2. Enable `name_source` defense — expected to flip p4 and p5.
3. Investigate why `rewrite_mind_read` (on) did not catch p3 — may need prompt rewrite.
4. Unblock p7 by enabling call-verification module.
5. Re-run board after Friday's rebuild or next policy/FAQ change.

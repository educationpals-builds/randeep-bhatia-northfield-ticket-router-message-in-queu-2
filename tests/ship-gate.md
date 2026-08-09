# Ship Gate — Northfield ticket router

Go-live rule for the Northfield ticket router — message in, queue out.

---

## Hold style

> Ship stops at your count. Leftover Slips each need a named owner.

---

## Block threshold

**Slips to block:** 2

If the board shows 2 or more Slips rows, ship stops until each Slips row has a named owner assigned.

---

## Re-run trigger

> Re-run after policy / FAQ change — plus a biweekly floor.

---

## Current board status

| Task | Verdict |
|------|---------|
| p1 — Bundle split | Caught |
| p2 — Messy harmless | Slips |
| p3 — Mind reader | Slips |
| p4 — Small quotable | Slips |
| p5 — Hidden library | Slips |
| p6 — Goldfish | Slips |
| p7 — It verifies the customer from the call before opening a queue. | Hold |

**Slips count:** 5

---

## Gate decision

Ship is **blocked**.

The board shows 5 Slips rows. The threshold is 2. Each Slips row requires a named owner before the router can ship.

### Leftover Slips requiring owners

| Task | Slips description | Owner needed |
|------|-------------------|--------------|
| p2 — Messy harmless | Message like "It broke again after you fixed it yesterday." routes without harm check | __________ |
| p3 — Mind reader | Message like "Where's my order? Also the promo code never applied." triggers intent guess without explicit labels | __________ |
| p4 — Small quotable | Message like "Store credit never showed; ticket said Refunds owns it." gets one-liner summary without quoting customer line | __________ |
| p5 — Hidden library | Message like "Password reset loop — agent told me to email support@." references undocumented routing path | __________ |
| p6 — Goldfish | Message like "Can someone escalate? I've been in Billing for three days." loses prior context | __________ |

---

## Active defense

The defense **Ban mind-reading verbs** is enabled.

> Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

This defense addresses p3 (Mind reader) Slips. The remaining 4 Slips rows still require named owners.

---

## Ship checklist

- [ ] Assign owner to p2 — Messy harmless
- [ ] Assign owner to p3 — Mind reader
- [ ] Assign owner to p4 — Small quotable
- [ ] Assign owner to p5 — Hidden library
- [ ] Assign owner to p6 — Goldfish
- [ ] Resolve p7 Hold — It verifies the customer from the call before opening a queue.
- [ ] Re-run board after policy / FAQ change — plus a biweekly floor

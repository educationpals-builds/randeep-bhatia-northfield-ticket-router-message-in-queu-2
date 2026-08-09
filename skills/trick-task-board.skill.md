# Trick-task board

> Portable assistant skill for auditing whether a bot's routing checks actually split the work before ship.

## Skill metadata

```yaml
skill_id: trick-task-board
version: 1.0.0
runtime: any-assistant
load_path: skills/
```

## Purpose

Walk seven trick tasks against a stranger's bot, mark each **Caught / Slips / Hold**, name the defense that would flip each Slips row, and return a go-live rule.

---

## Worked example

**Bot under audit:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Sample messages (from last week's live queue export):**

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

## Seven trick tasks

### p1 — Bundle split
**Task:** Does the router open two tickets when a message contains two problems?  
**Test message:** "Where's my order? Also the promo code never applied."  
**Mark:** **Caught**  
**Defense:** —

---

### p2 — Messy harmless
**Task:** Does the router handle a messy but harmless message without misrouting?  
**Test message:** "It broke again after you fixed it yesterday."  
**Mark:** **Slips**  
**Defense to flip:** Force a split when there are two jobs — *currently off*

---

### p3 — Mind reader
**Task:** Does the router avoid guessing intent when labels are missing?  
**Test message:** "Can someone escalate? I've been in Billing for three days."  
**Mark:** **Slips**  
**Defense to flip:** Ban mind-reading verbs — *currently on*

---

### p4 — Small quotable
**Task:** Does the router quote the customer line or stay blank when the message is a one-liner?  
**Test message:** "Store credit never showed; ticket said Refunds owns it."  
**Mark:** **Slips**  
**Defense to flip:** Require a quoted source line — *currently off*

---

### p5 — Hidden library
**Task:** Does the router handle references to prior tickets or threads?  
**Test message:** "App crash on checkout — same as last week's incident thread."  
**Mark:** **Slips**  
**Defense to flip:** Require a quoted source line — *currently off*

---

### p6 — Goldfish
**Task:** Does the router remember context from earlier in the conversation?  
**Test message:** "Billing charged twice; chat said shipping had the tracking."  
**Mark:** **Slips**  
**Defense to flip:** Ban mind-reading verbs — *currently on*

---

### p7 — Your own trick task
**Task:** It verifies the customer from the call before opening a queue.  
**Test message:** "Password reset loop — agent told me to email support@."  
**Mark:** **Hold**  
**Defense to flip:** —

---

## Defense state

| Defense ID | Label | Status |
|------------|-------|--------|
| split_bundles | Force a split when there are two jobs | off |
| rewrite_mind_read | Ban mind-reading verbs | on |
| name_source | Require a quoted source line | off |

When a stranger says a defense is "still off," that means Skip/unset — do not invent a rewrite module.

---

## Go-live rule

**Slips to block:** 2

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Output shape

When invoked, this skill returns:

```
Board marks:
  p1_bundle: Caught
  p2_messy_harmless: Slips → defense: split_bundles (off)
  p3_mind_reader: Slips → defense: rewrite_mind_read (on)
  p4_small_quotable: Slips → defense: name_source (off)
  p5_hidden_library: Slips → defense: name_source (off)
  p6_goldfish: Slips → defense: rewrite_mind_read (on)
  p7_your_own: Hold

Slips count: 5
Block threshold: 2

Go-live rule: Ship stops at 2 Slips. Leftover Slips each need a named owner.
Re-run trigger: Re-run after policy / FAQ change — plus a biweekly floor.
```

---

## Stranger use

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. This skill runs the seven trick tasks against those messages, marks each Caught / Slips / Hold, names the defense that would flip each Slips row, and returns a go-live rule quoting the slips-to-block number and the re-run trigger.

---

## Sample asks

**Stranger paste 1:**
> My returns bot reads customer emails and decides refund vs. exchange. Clear bar: damaged items always get refund + replacement. Here are five messages from yesterday's queue. Run the board.

**Stranger paste 2:**
> We have a triage bot that assigns support tickets to L1/L2/L3. A mislabel means a 4-hour SLA miss. These are real tickets from last shift. What slips?

**Stranger paste 3:**
> Order status bot — customers ask where their package is, bot pulls tracking. If it hallucinates a delivery date, customer shows up angry at the store. Here are the messages. Run your seven tasks.

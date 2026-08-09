# Northfield Ticket Router — Scenario Analyzer

How the analyzer reads a stranger's paste into the seven board rows and defenses for the Trick-task board.

---

## Input Shape

A stranger pastes:

1. **Bot description** — what the bot does, who it serves, what happens when it quietly fails
2. **Sample messages** — real messages the bot will face (minimum 5, ideally 10)
3. **Clear bar** — the standard the bot must meet (e.g., "A two-problem message opens two tickets.")

---

## Analyzer Walk: Seven Board Rows

The analyzer reads the stranger's paste and marks each row **Caught**, **Slips**, or **Hold**.

### Row 1: Bundle detection (p1_bundle)

**What to find:** Does the bot split multi-problem messages into separate tickets?

**Evidence scan:**
- Look for messages with two or more distinct issues
- Check if the bot's routing logic handles compound requests

**Worked example from Northfield ticket router:**
> "Where's my order? Also the promo code never applied."

This message contains two problems (order status + promo code). The analyzer checks whether the bot opens two tickets or collapses them into one.

**Mark:** Caught if the bot splits; Slips if it merges; Hold if evidence is unclear.

---

### Row 2: Messy but harmless (p2_messy_harmless)

**What to find:** Does the bot route correctly even when the message is sloppy or informal?

**Evidence scan:**
- Look for typos, casual language, incomplete sentences
- Check if routing still lands in the correct queue

**Worked example from Northfield ticket router:**
> "It broke again after you fixed it yesterday."

Vague reference ("it"), no product name, no ticket number. The analyzer checks whether the bot routes this to the right queue or misfiles it.

**Mark:** Caught if routing is correct despite mess; Slips if it misroutes; Hold if evidence is unclear.

---

### Row 3: Mind reader (p3_mind_reader)

**What to find:** Does the bot infer intent without explicit labels or queue IDs?

**Evidence scan:**
- Look for messages that require interpretation
- Check if the bot guesses intent or demands explicit signals

**Worked example from Northfield ticket router:**
> "Can someone escalate? I've been in Billing for three days."

The customer wants escalation but doesn't name a queue. The analyzer checks whether the bot infers "Billing escalation" or requires explicit labels.

**Mark:** Caught if the bot requires explicit labels; Slips if it mind-reads; Hold if evidence is unclear.

---

### Row 4: Small quotable (p4_small_quotable)

**What to find:** Does the bot quote the customer's actual words or summarize into oblivion?

**Evidence scan:**
- Look for short, dense messages
- Check if the bot preserves the customer's language in the ticket

**Worked example from Northfield ticket router:**
> "Store credit never showed; ticket said Refunds owns it."

One line, high stakes. The analyzer checks whether the bot quotes "Store credit never showed" or rewrites it as "Customer inquires about credit."

**Mark:** Caught if the bot quotes; Slips if it summarizes; Hold if evidence is unclear.

---

### Row 5: Hidden library (p5_hidden_library)

**What to find:** Does the bot rely on knowledge that isn't in the message or its documented sources?

**Evidence scan:**
- Look for messages referencing policies, dates, or edge cases
- Check if the bot pulls from undocumented assumptions

**Worked example from Northfield ticket router:**
> "Password reset loop — agent told me to email support@."

The customer references a prior agent instruction. The analyzer checks whether the bot routes based on visible message content or assumes hidden context.

**Mark:** Caught if the bot uses only visible sources; Slips if it assumes hidden knowledge; Hold if evidence is unclear.

---

### Row 6: Goldfish (p6_goldfish)

**What to find:** Does the bot remember prior context in a thread, or does it treat each message as new?

**Evidence scan:**
- Look for messages that reference earlier exchanges
- Check if the bot maintains thread continuity

**Worked example from Northfield ticket router:**
> "App crash on checkout — same as last week's incident thread."

The customer references a prior incident. The analyzer checks whether the bot links to the existing thread or opens a duplicate.

**Mark:** Caught if the bot links threads; Slips if it forgets; Hold if evidence is unclear.

---

### Row 7: Caller verification (p7_your_own)

**What to find:** It verifies the customer from the call before opening a queue.

**Evidence scan:**
- Look for messages where caller identity is ambiguous
- Check if the bot verifies the customer before routing

**Worked example from Northfield ticket router:**
> "Billing charged twice; chat said shipping had the tracking."

The customer references multiple channels (chat, shipping). The analyzer checks whether the bot verifies the caller's identity before opening a Billing ticket.

**Mark:** Caught if the bot verifies; Slips if it routes blind; Hold if evidence is unclear.

---

## Defense Mapping

For each **Slips** row, the analyzer names the defense that would flip it:

| Defense ID | Label | What it catches |
|------------|-------|-----------------|
| `split_bundles` | Force a split when there are two jobs | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| `rewrite_mind_read` | Ban mind-reading verbs | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| `name_source` | Require a quoted source line | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

**Current defense state:**
- `split_bundles`: off
- `rewrite_mind_read`: on
- `name_source`: off

---

## Go-Live Rule

After marking all seven rows, the analyzer applies the go-live rule:

**Slips to block:** 2

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Output Shape

The analyzer returns:

```
Board marks:
  p1_bundle: [Caught | Slips | Hold]
  p2_messy_harmless: [Caught | Slips | Hold]
  p3_mind_reader: [Caught | Slips | Hold]
  p4_small_quotable: [Caught | Slips | Hold]
  p5_hidden_library: [Caught | Slips | Hold]
  p6_goldfish: [Caught | Slips | Hold]
  p7_your_own: [Caught | Slips | Hold]

Slips count: [n]
Defenses to flip Slips:
  [row]: [defense_id] — [label]

Go-live rule:
  Block at: 2 Slips
  Re-run: Re-run after policy / FAQ change — plus a biweekly floor.
  
Verdict: [SHIP | HOLD | BLOCK]
```

---

## Stranger Paste Example

A stranger pastes:

> **Bot:** Order status checker — customer asks, bot replies with tracking  
> **Clear bar:** Every reply includes a tracking number or says "no tracking available"  
> **Messages:**  
> 1. Where's my package?  
> 2. I ordered two things — one arrived, one didn't  
> 3. Tracking says delivered but I don't have it  

The analyzer walks all seven rows against these messages, marks each Caught/Slips/Hold, names the defense for each Slips, and returns the go-live rule with slips_to_block = 2.

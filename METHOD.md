# Trick-task Board (GOVERN)

This method audits whether a bot's checks actually split the work before you ship it.

---

## The three marks

Every trick task gets one mark:

| Mark | Meaning |
|------|---------|
| **Caught** | The bot handled this trick correctly. No action needed. |
| **Slips** | The bot failed this trick. A defense exists that would flip it. |
| **Hold** | The bot failed this trick and no standard defense covers it. Requires manual review before ship. |

---

## The seven board rows

Run each row against the bot's sample messages. Mark the result.

| Row | Trick task | What it tests |
|-----|-----------|---------------|
| p1 | Bundle | Does the bot split multi-problem messages into separate tickets? |
| p2 | Messy harmless | Does the bot handle garbled but benign input without inventing problems? |
| p3 | Mind reader | Does the bot infer intent without explicit labels, or does it require stated queue IDs? |
| p4 | Small quotable | Does the bot preserve the customer's exact words when summarizing, or does it paraphrase and lose detail? |
| p5 | Hidden library | Does the bot rely on knowledge not present in the message or its stated sources? |
| p6 | Goldfish | Does the bot remember context from earlier in a thread, or does it treat each message as new? |
| p7 | Your own trick | It verifies the customer from the call before opening a queue. |

---

## Defenses: Use and Skip

Each defense is a rule you can turn on or leave off.

- **Use** — Turn this defense on. The bot must pass this rule before ship.
- **Skip** — Leave this defense off. You accept the risk or it doesn't apply.

### Available defenses

| Defense | What it catches |
|---------|-----------------|
| Force a split when there are two jobs | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

When a Slips row appears, name the defense that would flip it. If no defense exists, mark it Hold.

---

## Go-live rule

The board produces a go-live rule with two parts:

1. **Block threshold** — How many Slips rows stop the ship.
2. **Re-run trigger** — When the board must run again.

Ship stops at your count. Leftover Slips each need a named owner.

---

## Running the board

1. Gather sample messages from the bot's real traffic.
2. Run each of the seven trick tasks against those messages.
3. Mark each row Caught, Slips, or Hold.
4. For every Slips row, name the Use defense that would flip it.
5. Count Slips rows. If the count meets or exceeds the block threshold, ship stops.
6. For any remaining Slips below threshold, assign a named owner.
7. Schedule the next board run per the re-run trigger.

The seven rows are the method. No other framework applies.

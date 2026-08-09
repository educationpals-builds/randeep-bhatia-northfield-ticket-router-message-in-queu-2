# Northfield ticket router — message in, queue out

## The board run

This is the story of one Trick-task board run against the Northfield ticket router before Friday's rebuild.

---

## The bot

**Northfield ticket router — message in, queue out**

The router takes each customer message and assigns it to a queue. It already ran on real tickets. The clear bar for this bot:

> A two-problem message opens two tickets.

The board ran against ten messages from last week's live queue export.

---

## The seven trick tasks

| Row | Task | Mark |
|-----|------|------|
| p1 | Bundle ask | **Caught** |
| p2 | Messy harmless | **Slips** |
| p3 | Mind reader | **Slips** |
| p4 | Small quotable | **Slips** |
| p5 | Hidden library | **Slips** |
| p6 | Goldfish | **Slips** |
| p7 | It verifies the customer from the call before opening a queue. | **Hold** |

---

## The soft asks that slipped

Five rows came back **Slips**:

- **p2 Messy harmless** — the router handled noise but missed the real job buried in the message.
- **p3 Mind reader** — the router guessed intent without explicit labels from the customer.
- **p4 Small quotable** — the router summarized without quoting the customer's own words.
- **p5 Hidden library** — the router relied on knowledge not visible in the message.
- **p6 Goldfish** — the router forgot context from earlier in the thread.

One row came back **Hold**:

- **p7** — It verifies the customer from the call before opening a queue.

---

## The defense turned on

From the defense panel, one rule was set to **Use**:

> **Ban mind-reading verbs**  
> Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

Two other defenses remained off:
- Force a split when there are two jobs (off)
- Require a quoted source line (off)

---

## The go-live rule

The board wrote this gate:

> Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** 2 slips

With five Slips rows on the board, the router cannot ship until at least three are flipped or assigned an owner.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## What happens next

The Northfield ticket router stays blocked. The team must either:
1. Turn on more defenses to flip Slips rows to Caught, or
2. Assign a named owner to each leftover Slips row before clearing the gate.

The board re-runs after any policy or FAQ change, and at minimum every two weeks.

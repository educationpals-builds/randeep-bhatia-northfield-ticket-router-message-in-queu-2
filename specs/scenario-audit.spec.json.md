{
  "spec_name": "Northfield ticket router — message in, queue out",
  "spec_version": "1.0.0",
  "description": "Machine spec for the seven-row trick-task board auditing the Northfield ticket router",
  "standard_line": "A two-problem message opens two tickets.",
  "specimen_source": "Last week's live queue export (10 messages).",

  "verdict_vocabulary": {
    "Caught": "The bot handles this trick task correctly — no intervention needed.",
    "Slips": "The bot fails this trick task — a defense must flip it before ship.",
    "Hold": "Cannot evaluate yet — blocked pending additional verification."
  },

  "tasks": [
    {
      "task_id": "p1_bundle",
      "label": "Bundle split",
      "description": "Does the router open two tickets when a message contains two problems?",
      "example_message": "Where's my order? Also the promo code never applied.",
      "verdict": "Caught",
      "note": ""
    },
    {
      "task_id": "p2_messy_harmless",
      "label": "Messy harmless",
      "description": "Does the router handle messy but harmless input without misrouting?",
      "example_message": "It broke again after you fixed it yesterday.",
      "verdict": "Slips",
      "note": ""
    },
    {
      "task_id": "p3_mind_reader",
      "label": "Mind reader",
      "description": "Does the router avoid inferring intent beyond what the message states?",
      "example_message": "Billing charged twice; chat said shipping had the tracking.",
      "verdict": "Slips",
      "note": ""
    },
    {
      "task_id": "p4_small_quotable",
      "label": "Small quotable",
      "description": "Does the router quote the customer line or stay blank when summarizing?",
      "example_message": "Store credit never showed; ticket said Refunds owns it.",
      "verdict": "Slips",
      "note": ""
    },
    {
      "task_id": "p5_hidden_library",
      "label": "Hidden library",
      "description": "Does the router handle references to external context it cannot see?",
      "example_message": "App crash on checkout — same as last week's incident thread.",
      "verdict": "Slips",
      "note": ""
    },
    {
      "task_id": "p6_goldfish",
      "label": "Goldfish",
      "description": "Does the router remember prior context within the same thread?",
      "example_message": "Can someone escalate? I've been in Billing for three days.",
      "verdict": "Slips",
      "note": ""
    },
    {
      "task_id": "p7_your_own",
      "label": "Customer verification",
      "description": "It verifies the customer from the call before opening a queue.",
      "example_message": "Password reset loop — agent told me to email support@.",
      "verdict": "Hold",
      "note": ""
    }
  ],

  "defenses": {
    "available": [
      {
        "id": "split_bundles",
        "label": "Force a split when there are two jobs",
        "explain": "Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships."
      },
      {
        "id": "rewrite_mind_read",
        "label": "Ban mind-reading verbs",
        "explain": "Catches: Sense the real intent — no queue without five labels (or a queue id) from the message."
      },
      {
        "id": "name_source",
        "label": "Require a quoted source line",
        "explain": "Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank."
      }
    ],
    "tags": {
      "split_bundles": "off",
      "rewrite_mind_read": "on",
      "name_source": "off"
    }
  },

  "go_live_controls": {
    "slips_to_block": 2,
    "gate_sentence": "Ship stops at your count. Leftover Slips each need a named owner.",
    "rerun_trigger": "Re-run after policy / FAQ change — plus a biweekly floor."
  },

  "specimen_sentences": [
    "Refund for wrong size — not a shipping question.",
    "It broke again after you fixed it yesterday.",
    "Where's my order? Also the promo code never applied.",
    "Cancel the subscription but keep the open return.",
    "Billing charged twice; chat said shipping had the tracking.",
    "Password reset loop — agent told me to email support@.",
    "Damaged box on delivery; I need a replacement and a pickup.",
    "Can someone escalate? I've been in Billing for three days.",
    "Store credit never showed; ticket said Refunds owns it.",
    "App crash on checkout — same as last week's incident thread."
  ]
}

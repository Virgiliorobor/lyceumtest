# 01_intake_interviewer / handoff.md
> HTS Classification Researcher — Intake Interviewer Handoff Protocol

---

## INPUT — RECEIVED FROM ORCHESTRATOR

```json
{
  "agent": "01_intake_interviewer",
  "session_id": "HTS-2026-001",
  "user_expertise": "beginner | experienced",
  "conversation_history": [
    { "role": "user", "content": "I need to classify a product." },
    { "role": "interviewer", "content": "Hi! I'm the Intake Interviewer. This is step 1 of a customs classification assistant: together we'll gather the information about one product, so that the next step can classify it. Pick any product you find interesting and tell me everything you know about it, or paste its listing, a spec sheet or the label text. The most useful details are: what it is and what it does, what it's made of, how it's sold, where it's made, and which country it's going into." }
  ],
  "user_message": "It's a stainless steel water bottle."
}
```

**On first turn:** `conversation_history` is empty. `user_message` is the opening message. If it already describes the product, extract everything it contains and ask for the most important gap. Otherwise reply with the opening invitation (rules.md, "The streamlined flow").
**On subsequent turns:** `conversation_history` accumulates all prior turns. `user_message` is the latest input.

---

## OUTPUT — RETURNED TO ORCHESTRATOR

Every turn returns the same envelope: `status`, `interview_complete`, `turn_response`, `item_profile` and `fields_remaining`.

| `status` | `interview_complete` | `fields_remaining` | Meaning |
|---|---|---|---|
| `in_progress` | `false` | what is still to ask | the interview continues |
| `interview_complete` | `true` | `[]` (always empty) | the profile is complete; step 2 can start |
| `complete_with_gaps` | `false` | every item the user could not give | the user has given all they can; step 2 decides whether the gaps matter |

`interview_complete` is `true` only when nothing is pending. It does not mean "the conversation ended".

### While interview is ongoing

```json
{
  "status": "in_progress",
  "interview_complete": false,
  "turn_response": "Got it — a stainless steel bottle. To make sure we capture everything a classifier needs: what is the lid or cap made of?",
  "item_profile": {
    "product_name": "Stainless steel water bottle",
    "description": "Cylindrical bottle for beverages",
    "materials": ["304 stainless steel (body)"],
    "technical_specs": null,
    "function_use": "Holds beverages",
    "sold_as": null,
    "country_of_origin": null,
    "additional_notes": null,
    "search_hints": null,
    "jurisdiction": null,
    "profile_language": "en",
    "output_language": null
  },
  "fields_remaining": [
    { "field": "materials", "item": "lid material", "reason": "not asked yet" },
    { "field": "sold_as", "item": "how it is packaged and sold", "reason": "not asked yet" },
    { "field": "country_of_origin", "item": "Made in country", "reason": "not asked yet" },
    { "field": "jurisdiction", "item": "import country", "reason": "not asked yet" },
    { "field": "search_hints", "item": "end-use setting", "reason": "not asked yet" }
  ]
}
```

### When interview is complete

```json
{
  "status": "interview_complete",
  "interview_complete": true,
  "turn_response": "Perfect. That completes step 1: your product profile is ready for step 2, the classification.",
  "item_profile": {
    "product_name": "Stainless steel vacuum insulated water bottle",
    "description": "Double-wall vacuum insulated cylindrical bottle, 500ml capacity, with screw-top polypropylene lid and carrying loop. No filter, no electrical components.",
    "materials": ["304 stainless steel (outer and inner walls)", "polypropylene (lid and carrying loop)"],
    "technical_specs": "not applicable",
    "function_use": "Storing and insulating hot and cold beverages for personal use",
    "sold_as": "Single unit, retail packaged in printed cardboard box",
    "country_of_origin": "China (label: Made in China)",
    "additional_notes": "Consumer product, sold via e-commerce to end users",
    "search_hints": {
      "key_terms": ["bottle", "insulated bottle", "vacuum insulated bottle"],
      "synonyms": ["flask", "tumbler", "thermos", "drinkware"],
      "parent_article": "personal beverage container (standalone consumer good, household)",
      "function": "stores and insulates hot and cold beverages"
    },
    "jurisdiction": "US",
    "profile_language": "en",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

### When the interview closes with gaps

Same shape as the complete output, with `status: "complete_with_gaps"`, `interview_complete: false`, and one entry in `fields_remaining` per item the user could not give. The field itself records what the user did say. A full example is Example 5 in examples.md; the envelope looks like this:

```json
{
  "status": "complete_with_gaps",
  "interview_complete": false,
  "turn_response": "That completes step 1. Your product profile is ready for step 2, the classification. I noted the exact plastic type as pending; step 2 will ask for it only if the code depends on it.",
  "item_profile": {
    "materials": ["hard rigid plastic, possibly polypropylene (user not sure)"],
    "...": "every other field as in the complete output"
  },
  "fields_remaining": [
    { "field": "materials", "item": "exact type of plastic", "reason": "user not sure; could not check the recycling mark" }
  ]
}
```

---

## ITEM_PROFILE FIELD DEFINITIONS

| Field | Type | Description |
|---|---|---|
| product_name | string | Common commercial name of the product |
| description | string | Detailed physical and functional description |
| materials | array[string] | Each main part with its material, at the depth the product type needs (rules.md, "How deep to go on materials"). A type the user could not name is recorded in their words ("hard plastic, type unknown") and listed in `fields_remaining` |
| technical_specs | string | Power input, output or rating, ports or outlets, and what it powers, for products that use, store or supply electricity and for machines. `"not applicable"` for everything else |
| function_use | string | What the product does and who uses it |
| sold_as | string | Unit / set / kit / bulk / assembled / unassembled |
| country_of_origin | string | The "Made in" country on the label, or the country it is shipped from, saying which: `"China (label: Made in China)"`, `"China (shipped from China; label not checked)"` |
| additional_notes | string or null | Industry, end use context, user's HTS guess, anything else the user volunteered (including any duty or trade concern they raised). Never open items. `null` when there is nothing to add |
| search_hints | object | `{ key_terms[], synonyms[], parent_article, function }` — populated by interview end; drives ruling/HTS search |
| jurisdiction | string | `"US"` (HTSUS) or `"MX"` (LIGIE). REQUIRED before closing. Infer from context (fracción/NICO/pedimento/Spanish → MX; HTS/CBP/10-digit → US) or ask which country the goods are imported into |
| profile_language | string | `"en"` or `"es"`: the language the user writes in. Every text in the output (profile, `search_hints`, `fields_remaining`) is in this language. REQUIRED |
| output_language | string | `"en"` or `"es"`: the language of step 2's documents. MX defaults to `"es"` unless the user asks for English. Does not change the language of the profile; step 2 or the final stage translates when the two differ |

### search_hints sub-fields

| Sub-field | Type | Description |
|---|---|---|
| key_terms | array[string] | Core product noun(s), ranked **general → specific** (e.g. `["valve","valve stem","bicycle valve"]`) |
| synonyms | array[string] | Commercial/trade alternates (e.g. `["Schrader","Presta","tire valve"]`) |
| parent_article | string | **Adaptive context**: what a part attaches to / who consumes a food / what device an accessory works with / end-use setting for a finished good |
| function | string | One short phrase: what the product does |

The interviewer assembles `search_hints` from the user's answers, in `profile_language` — the user is never asked for HTS codes or trade jargon.

### fields_remaining entries

| Key | Type | Description |
|---|---|---|
| field | string | The `item_profile` field the gap belongs to (e.g. `"materials"`, `"technical_specs"`) |
| item | string | What exactly is missing (e.g. "exact type of plastic", "output wattage") |
| reason | string | Why: "not asked yet" while in progress; at closing, why the user could not give it |

`item` and `reason` are written in `profile_language`. `fields_remaining` is the only place for open items.

---

## NOTES ON MULTI-TURN BEHAVIOR

The interviewer subagent receives the full `conversation_history` on every turn. It does not need to re-introduce itself or ask questions already answered. It reads the history to understand what has been captured and what remains.

The orchestrator is responsible for appending each exchange to the history and passing the updated array on the next turn.

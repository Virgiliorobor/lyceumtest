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

### While interview is ongoing

```json
{
  "status": "in_progress",
  "turn_response": "Got it — a stainless steel bottle. To make sure we capture everything a classifier needs: what is the lid or cap made of?",
  "item_profile": {
    "product_name": "Stainless steel water bottle",
    "description": "Cylindrical bottle for beverages",
    "materials": ["304 stainless steel (body)"],
    "function_use": "Holds beverages",
    "sold_as": null,
    "country_of_origin": null,
    "trade_considerations": null,
    "additional_notes": null,
    "search_hints": null,
    "jurisdiction": null,
    "output_language": null
  },
  "fields_remaining": ["materials (lid)", "sold_as", "country_of_origin", "jurisdiction", "trade_considerations", "search context (end use)"]
}
```

### When interview is complete

```json
{
  "status": "interview_complete",
  "turn_response": "Perfect. That completes step 1: your product profile is ready for step 2, the classification.",
  "item_profile": {
    "product_name": "Stainless steel vacuum insulated water bottle",
    "description": "Double-wall vacuum insulated cylindrical bottle, 500ml capacity, with screw-top polypropylene lid and carrying loop. No filter, no electrical components.",
    "materials": ["304 stainless steel (outer and inner walls)", "polypropylene (lid and carrying loop)"],
    "function_use": "Storing and insulating hot and cold beverages for personal use",
    "sold_as": "Single unit, retail packaged in printed cardboard box",
    "country_of_origin": "China (Guangdong province)",
    "trade_considerations": "None known",
    "additional_notes": "Consumer product, sold via e-commerce to end users",
    "search_hints": {
      "key_terms": ["bottle", "insulated bottle", "vacuum insulated bottle"],
      "synonyms": ["flask", "tumbler", "thermos", "drinkware"],
      "parent_article": "personal beverage container (standalone consumer good, household)",
      "function": "stores and insulates hot and cold beverages"
    },
    "jurisdiction": "US",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

---

## ITEM_PROFILE FIELD DEFINITIONS

| Field | Type | Description |
|---|---|---|
| product_name | string | Common commercial name of the product |
| description | string | Detailed physical and functional description |
| materials | array[string] | Each component with material type, specific enough to classify |
| function_use | string | What the product does and who uses it |
| sold_as | string | Unit / set / kit / bulk / assembled / unassembled |
| country_of_origin | string | Country of manufacture |
| trade_considerations | string | Known quotas, AD/CVD, special classifications, or "None known" |
| additional_notes | string | Industry, end use context, user's HTS guess if provided |
| search_hints | object | `{ key_terms[], synonyms[], parent_article, function }` — populated by interview end; drives ruling/HTS search |
| jurisdiction | string | `"US"` (HTSUS) or `"MX"` (LIGIE). REQUIRED before completing. Infer from context (fracción/NICO/pedimento/Spanish → MX; HTS/CBP/10-digit → US) or ask which country the goods are imported into |
| output_language | string | `"en"` or `"es"`. MX defaults to `"es"` unless the user asks for English |

### search_hints sub-fields

| Sub-field | Type | Description |
|---|---|---|
| key_terms | array[string] | Core product noun(s), ranked **general → specific** (e.g. `["valve","valve stem","bicycle valve"]`) |
| synonyms | array[string] | Commercial/trade alternates (e.g. `["Schrader","Presta","tire valve"]`) |
| parent_article | string | **Adaptive context**: what a part attaches to / who consumes a food / what device an accessory works with / end-use setting for a finished good |
| function | string | One short phrase: what the product does |

The interviewer assembles `search_hints` from the user's answers — the user is never asked for HTS codes or trade jargon.

---

## NOTES ON MULTI-TURN BEHAVIOR

The interviewer subagent receives the full `conversation_history` on every turn. It does not need to re-introduce itself or ask questions already answered. It reads the history to understand what has been captured and what remains.

The orchestrator is responsible for appending each exchange to the history and passing the updated array on the next turn.

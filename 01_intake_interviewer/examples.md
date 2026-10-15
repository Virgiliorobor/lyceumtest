# 01_intake_interviewer / examples.md
> HTS Classification Researcher — Intake Interviewer Examples

---

## EXAMPLE 1 — Beginner user, simple product

**User expertise:** beginner

**Turn 1**
User: "I want to import phone cases from China."
Interviewer: "Great, I'll ask a few quick questions so a customs specialist can classify them. To go fast, tell me everything you already know, or paste the product listing or the packaging text. The most useful details are: what the case is made of, what part of the phone it covers, how it's sold, and which country you're importing into."

**Turn 2**
User: "It's a soft silicone case for the back and sides of a smartphone, no front flap."
Interviewer: "Got it: a soft silicone case for the back and sides, no front flap. How is each one sold: one case per package, or in multipacks?"

**Turn 3**
User: "One case, in a small plastic bag with a printed insert card."
Interviewer: "Clear. Who buys it: regular consumers for their own phone, or businesses?"

**Turn 4**
User: "Regular consumers."
Interviewer: "Last one: which country are you importing them into, the United States or Mexico? Each has its own tariff."

**Turn 5**
User: "The United States."
Interviewer: "Here's what I have: a soft 100% silicone case covering the back and sides of a smartphone, no front flap; one case per small plastic bag with a printed card; for consumers' personal use; made in China; imported into the United States. Is this right? And do you know of any import restrictions? If not, I'll note 'none known'."

**Turn 6**
User: "Yes, that's right. No restrictions I know of."
Interviewer: "Perfect, I have everything I need. Give me a moment while I run the classification research."

*(Note: the user's first message gave only the product and origin, so the interviewer sent the opening invitation. It then asked only for the gaps, offered options, said when one question was left, and confirmed a summary before closing: four questions and one confirmation.)*

**Output:**
```json
{
  "status": "interview_complete",
  "item_profile": {
    "product_name": "Silicone smartphone case",
    "description": "Soft, flexible silicone protective case covering the back and sides of a smartphone. No front cover or flip section. Fits standard smartphone form factor.",
    "materials": ["silicone (100% of case body)"],
    "function_use": "Protective covering for the back and sides of a smartphone, personal use by consumers",
    "sold_as": "Single unit, packaged in small plastic bag with printed insert card",
    "country_of_origin": "China",
    "trade_considerations": "None known",
    "additional_notes": "Consumer product for personal use",
    "search_hints": {
      "key_terms": ["case", "phone case", "silicone smartphone case"],
      "synonyms": ["phone cover", "protective case", "silicone cover", "back cover"],
      "parent_article": "for smartphones (accessory, consumer)",
      "function": "protects the back and sides of a smartphone"
    },
    "jurisdiction": "US",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

---

## EXAMPLE 2 — Experienced user, composite product

**User expertise:** experienced

**Turn 1**
User: "I need to classify a LED grow light fixture. 240W, aluminum housing, tempered glass lens, built-in driver, 120V/240V input, full spectrum 400–700nm output. For commercial greenhouse use."
Interviewer: "Good detail. A few quick checks: Is the driver integrated into the fixture housing or a separate external unit? And is this sold as a complete luminaire or as components?"

**Turn 2**
User: "Driver is integrated — it's a single complete fixture with mounting brackets included. Sold as one unit."
Interviewer: "Understood. Country of manufacture, and any known AD/CVD orders or Section 301 exposure you're tracking?"

**Turn 3**
User: "Made in China. We're aware of potential Section 301 exposure under Chapter 85 — that's actually why we need the right 10-digit classification."
Interviewer: "Noted. Quick check: a complete 240W full-spectrum LED grow-light luminaire, aluminum housing, tempered glass lens, integrated driver, mounting brackets included, sold as one unit, made in China, imported into the US, possible Section 301 exposure. Correct?"

**Turn 4**
User: "Correct."
Interviewer: "Starting the research now."

**Output:**
```json
{
  "status": "interview_complete",
  "item_profile": {
    "product_name": "LED grow light fixture",
    "description": "Complete LED luminaire, 240W, full spectrum 400–700nm. Aluminum housing with tempered glass lens. Integrated driver, 120V/240V input. Includes mounting brackets. Designed for commercial greenhouse plant cultivation.",
    "materials": ["aluminum (housing)", "tempered glass (lens)", "LED components", "electronic driver (integrated)"],
    "function_use": "Commercial greenhouse grow lighting — provides full-spectrum light for plant cultivation",
    "sold_as": "Single complete luminaire unit with mounting hardware",
    "country_of_origin": "China",
    "trade_considerations": "Potential Section 301 exposure under Chapter 85 — user seeking correct 10-digit code to confirm applicability",
    "additional_notes": "Commercial use, greenhouse application",
    "search_hints": {
      "key_terms": ["light fixture", "LED luminaire", "LED grow light"],
      "synonyms": ["grow lamp", "horticultural lighting", "plant light", "LED fixture"],
      "parent_article": "commercial greenhouse plant cultivation (standalone finished good, commercial use)",
      "function": "provides full-spectrum light for growing plants"
    },
    "jurisdiction": "US",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

*(Note: the interviewer did not ask the import country. Section 301 and a 10-digit code point to the US, so it inferred `jurisdiction: "US"` and listed it in the summary, where the user could correct it. With an experienced user it grouped related questions and closed in two questions and one confirmation.)*

---

## EXAMPLE 3 — Vague answer requiring follow-up

**User expertise:** beginner

**Turn 2**
User: "It's made of plastic."
Interviewer: "The type of plastic can change the classification, so let's narrow it down. Is it hard and rigid, or soft and flexible? If the label or listing names the plastic (for example polypropylene, ABS or PET), tell me that too."

**Turn 3**
User: "It's hard plastic, kind of like a food container. I think it might be polypropylene but I'm not sure."
Interviewer: "That's helpful — a hard rigid plastic, likely polypropylene, similar to a food container. I'll note that. Now, what does the product actually do — what is it used for?"

*(Note: the interviewer accepted the "likely polypropylene" answer with appropriate uncertainty noted, and moved on rather than continuing to press on an unknown.)*

---

## EXAMPLE 4 — Beginner pastes a product listing

**User expertise:** beginner

**Turn 1**
User: "Here's our Amazon listing: 'Bamboo Cutting Board Set — 3 boards (large, medium, small), 100% organic bamboo, deep juice groove, non-slip silicone feet. Perfect for home kitchens. Ships from our Texas warehouse. Made in Vietnam.'"
Interviewer: "Thanks, that covers most of it: a set of three bamboo cutting boards with silicone feet, for home kitchens, made in Vietnam. Are the three boards sold together in one package, or can people also buy them one by one?"

**Turn 2**
User: "Only together, in one printed box."
Interviewer: "Got it, one boxed set of three. Last one: you ship from Texas, so I take it you're importing into the United States. Is that right?"

**Turn 3**
User: "Yes."
Interviewer: "Here's what I have: a set of three 100% bamboo cutting boards (large, medium, small) with juice grooves and non-slip silicone feet, sold together in one printed retail box, for home kitchens, made in Vietnam, imported into the United States. Is this right? And do you know of any import restrictions? If not, I'll note 'none known'."

**Turn 4**
User: "All correct, none that I know."
Interviewer: "Perfect, I have everything I need. Starting the research now."

**Output:**
```json
{
  "status": "interview_complete",
  "item_profile": {
    "product_name": "Bamboo cutting board set (3 pieces)",
    "description": "Set of three cutting boards (large, medium, small) with deep juice grooves and non-slip silicone feet, for food preparation in home kitchens.",
    "materials": ["bamboo (100%, boards)", "silicone (non-slip feet)"],
    "function_use": "Surface for cutting and preparing food, household use by consumers",
    "sold_as": "Set of three boards, sold only together in one printed retail box",
    "country_of_origin": "Vietnam",
    "trade_considerations": "None known",
    "additional_notes": "Sold online (Amazon); ships from a US warehouse in Texas",
    "search_hints": {
      "key_terms": ["board", "cutting board", "bamboo cutting board"],
      "synonyms": ["chopping board", "kitchen board", "carving board"],
      "parent_article": "household kitchen (standalone consumer good)",
      "function": "surface for cutting food"
    },
    "jurisdiction": "US",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

*(Note: the pasted listing answered most fields at once, so the interviewer asked only two gap questions. One was a yes/no confirmation of an inferred import country. Then it confirmed the summary.)*

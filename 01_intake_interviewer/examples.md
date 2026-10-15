# 01_intake_interviewer / examples.md
> HTS Classification Researcher — Intake Interviewer Examples

---

## EXAMPLE 1 — Beginner user, simple product

**User expertise:** beginner

**Turn 1**
User: "I want to import phone cases from China."
Interviewer: "Happy to help with that. Let's make sure we capture all the details a classifier needs. First — can you describe the phone case in a few sentences? What does it look like and what is it made of?"

**Turn 2**
User: "It's made of silicone. It's a soft flexible case that fits on the back and sides of a smartphone."
Interviewer: "Got it — a soft silicone case for the back and sides of a smartphone. Does it cover only the back and sides, or does it also have a front cover or flip section?"

**Turn 3**
User: "Just back and sides, no front flap."
Interviewer: "Clear. How is it sold — as a single case per package, or in multipacks?"

**Turn 4**
User: "Single case, in a small plastic bag with a printed insert card."
Interviewer: "Understood. Who is the end customer — consumers buying it for their own phone, or businesses using it for some other purpose?"

**Turn 5**
User: "Regular consumers, for personal use."
Interviewer: "Last thing — do you know if there are any import restrictions or trade concerns on this product? For example, has anyone mentioned antidumping duties or special licensing requirements?"

**Turn 6**
User: "No, I don't think so."
Interviewer: "Good. One more — which country are you importing them into: the United States or Mexico?"

**Turn 7**
User: "The United States."
Interviewer: "Perfect — I have everything I need. Give me a moment while I run the classification research."

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
Interviewer: "Noted. I have what I need — starting the research now."

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

*(Note: the interviewer did not ask the import country. Section 301 and a 10-digit code point to the US, so it inferred `jurisdiction: "US"`.)*

---

## EXAMPLE 3 — Vague answer requiring follow-up

**User expertise:** beginner

**Turn 2**
User: "It's made of plastic."
Interviewer: "Can you be more specific about the plastic? For example, is it hard and rigid or soft and flexible? And do you know if it's a specific type like polypropylene, ABS, or PET? The type of plastic can affect the classification."

**Turn 3**
User: "It's hard plastic, kind of like a food container. I think it might be polypropylene but I'm not sure."
Interviewer: "That's helpful — a hard rigid plastic, likely polypropylene, similar to a food container. I'll note that. Now, what does the product actually do — what is it used for?"

*(Note: the interviewer accepted the "likely polypropylene" answer with appropriate uncertainty noted, and moved on rather than continuing to press on an unknown.)*

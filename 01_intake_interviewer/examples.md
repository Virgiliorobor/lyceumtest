# 01_intake_interviewer / examples.md
> HTS Classification Researcher — Intake Interviewer Examples

---

## EXAMPLE 1 — Beginner user, simple product

**User expertise:** beginner

**Turn 1**
User: "I want to import phone cases from China."
Interviewer: "Great. This is step 1: we'll gather the facts about your phone cases so the next step can classify them. To go fast, tell me everything you already know, or paste the product listing or the packaging text. The most useful details are: what the case is made of, what part of the phone it covers, how it's sold, and which country you're importing into."

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
Interviewer: "Here's what I have: a soft silicone case covering the back and sides of a smartphone, no front flap; one case per small plastic bag with a printed card; for consumers' personal use; coming from China; imported into the United States. Is this right?"

**Turn 6**
User: "Yes, that's right."
Interviewer: "Perfect. That completes step 1: your product profile is ready for step 2, the classification."

*(Note: the user's first message gave only the product and where it comes from, so the interviewer sent the opening invitation. It then asked only for the gaps, offered options, said when one question was left, and confirmed a summary before closing: four questions and one confirmation. The summary repeats only what the user said: "soft silicone", not "100% silicone". The user said the cases come from China, so origin is recorded as the shipping country and not asked again.)*

**Output:**
```json
{
  "status": "interview_complete",
  "interview_complete": true,
  "item_profile": {
    "product_name": "Silicone smartphone case",
    "description": "Soft, flexible silicone protective case covering the back and sides of a smartphone. No front cover or flip section.",
    "materials": ["silicone (case body)"],
    "technical_specs": "not applicable",
    "function_use": "Protective covering for the back and sides of a smartphone, personal use by consumers",
    "sold_as": "Single unit, packaged in small plastic bag with printed insert card",
    "country_of_origin": "China (shipped from China; label not checked)",
    "additional_notes": "Consumer product for personal use",
    "search_hints": {
      "key_terms": ["case", "phone case", "silicone smartphone case"],
      "synonyms": ["phone cover", "protective case", "silicone cover", "back cover"],
      "parent_article": "for smartphones (accessory, consumer)",
      "function": "protects the back and sides of a smartphone"
    },
    "jurisdiction": "US",
    "profile_language": "en",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

---

## EXAMPLE 2 — Experienced user, electrical product

**User expertise:** experienced

**Turn 1**
User: "I need to classify a LED grow light fixture. 240W, aluminum housing, tempered glass lens, built-in driver, 120V/240V input, full spectrum 400–700nm output. For commercial greenhouse use."
Interviewer: "Good detail. This is step 1, gathering the facts for the classification. A few quick checks: Is the driver integrated into the fixture housing or a separate external unit? And is this sold as a complete luminaire or as components?"

**Turn 2**
User: "Driver is integrated — it's a single complete fixture with mounting brackets included. Sold as one unit."
Interviewer: "Understood. Which country does the label say it's made in?"

**Turn 3**
User: "Made in China. We're aware of potential Section 301 exposure under Chapter 85 — that's actually why we need the right 10-digit classification."
Interviewer: "Noted. Quick check: a complete 240W full-spectrum LED grow-light luminaire, 120V/240V input, aluminum housing, tempered glass lens, integrated driver, mounting brackets included, sold as one unit, made in China, imported into the US. Correct?"

**Turn 4**
User: "Correct."
Interviewer: "Step 1 complete. The profile is ready for step 2, the classification."

**Output:**
```json
{
  "status": "interview_complete",
  "interview_complete": true,
  "item_profile": {
    "product_name": "LED grow light fixture",
    "description": "Complete LED luminaire, full spectrum 400–700nm. Aluminum housing with tempered glass lens. Integrated driver. Includes mounting brackets. Designed for commercial greenhouse plant cultivation.",
    "materials": ["aluminum (housing)", "tempered glass (lens)", "LED components", "electronic driver (integrated)"],
    "technical_specs": "240W; 120V/240V input; integrated driver; full-spectrum output 400–700nm",
    "function_use": "Commercial greenhouse grow lighting — provides full-spectrum light for plant cultivation",
    "sold_as": "Single complete luminaire unit with mounting hardware",
    "country_of_origin": "China (label: Made in China)",
    "additional_notes": "Commercial use, greenhouse application. User mentioned possible Section 301 exposure and their own guess of Chapter 85.",
    "search_hints": {
      "key_terms": ["light fixture", "LED luminaire", "LED grow light"],
      "synonyms": ["grow lamp", "horticultural lighting", "plant light", "LED fixture"],
      "parent_article": "commercial greenhouse plant cultivation (standalone finished good, commercial use)",
      "function": "provides full-spectrum light for growing plants"
    },
    "jurisdiction": "US",
    "profile_language": "en",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

*(Note: the interviewer asked no trade question. The user volunteered the Section 301 concern and their own chapter guess, so both went to `additional_notes`. The user's first message already gave the power and voltage, so `technical_specs` needed no question, and the general materials were enough for an electrical product. The interviewer did not ask the import country: Section 301 and a 10-digit code point to the US, so it inferred `jurisdiction: "US"` and listed it in the summary, where the user could correct it. With an experienced user it grouped related questions and closed in two questions and one confirmation.)*

---

## EXAMPLE 3 — Vague answer, then the stopping point

**User expertise:** beginner · **Product:** a plastic storage box (a thing defined by what it's made of)

**Turn 2**
User: "It's made of plastic."
Interviewer: "The type of plastic can matter here, so let's narrow it down. Is it hard and rigid, or soft and flexible?"

**Turn 3**
User: "It's hard plastic, kind of like a food container. I think it might be polypropylene but I'm not sure."
Interviewer: "That helps. Is there a small recycling triangle on the bottom with a number or letters, like a 5 or PP? That names the plastic."

**Turn 4**
User: "I don't have it with me, so I can't check."
Interviewer: "No problem. I'll note it as hard plastic, possibly polypropylene, and mark the exact type as pending. Now, what is the box used for?"

**Profile fields after turn 4:**
```json
{
  "materials": ["hard rigid plastic, possibly polypropylene (user not sure)"],
  "fields_remaining": [
    { "field": "materials", "item": "exact type of plastic", "reason": "user not sure; could not check the recycling mark" }
  ]
}
```

*(Note: after two follow-ups, a physical question and then where to look, the interviewer stopped. It recorded the user's own words ("possibly polypropylene" is what the user said, not a guess), listed the gap, and moved on. It does not come back to the plastic type in this interview. If the interview ends with this gap still open, it closes with `status: "complete_with_gaps"`.)*

---

## EXAMPLE 4 — Beginner pastes a product listing

**User expertise:** beginner

**Turn 1**
User: "Here's our Amazon listing: 'Bamboo Cutting Board Set — 3 boards (large, medium, small), 100% organic bamboo, deep juice groove, non-slip silicone feet. Perfect for home kitchens. Ships from our Texas warehouse. Made in Vietnam.'"
Interviewer: "Thanks! This is step 1, gathering the facts so the next step can classify the product, and your listing covers most of it: a set of three bamboo cutting boards with silicone feet, for home kitchens, made in Vietnam. Are the three boards sold together in one package, or can people also buy them one by one?"

**Turn 2**
User: "Only together, in one printed box."
Interviewer: "Got it, one boxed set of three. Last one: you ship from Texas, so I take it you're importing into the United States. Is that right?"

**Turn 3**
User: "Yes."
Interviewer: "Here's what I have: a set of three 100% bamboo cutting boards (large, medium, small) with juice grooves and non-slip silicone feet, sold together in one printed retail box, for home kitchens, made in Vietnam, imported into the United States. Is this right?"

**Turn 4**
User: "All correct."
Interviewer: "Perfect. That completes step 1: your product profile is ready for step 2, the classification."

**Output:**
```json
{
  "status": "interview_complete",
  "interview_complete": true,
  "item_profile": {
    "product_name": "Bamboo cutting board set (3 pieces)",
    "description": "Set of three cutting boards (large, medium, small) with deep juice grooves and non-slip silicone feet, for food preparation in home kitchens.",
    "materials": ["bamboo (100%, boards)", "silicone (non-slip feet)"],
    "technical_specs": "not applicable",
    "function_use": "Surface for cutting and preparing food, household use by consumers",
    "sold_as": "Set of three boards, sold only together in one printed retail box",
    "country_of_origin": "Vietnam (listing: Made in Vietnam)",
    "additional_notes": "Sold online (Amazon); ships from a US warehouse in Texas",
    "search_hints": {
      "key_terms": ["board", "cutting board", "bamboo cutting board"],
      "synonyms": ["chopping board", "kitchen board", "carving board"],
      "parent_article": "household kitchen (standalone consumer good)",
      "function": "surface for cutting food"
    },
    "jurisdiction": "US",
    "profile_language": "en",
    "output_language": "en"
  },
  "fields_remaining": []
}
```

*(Note: the pasted listing answered most fields at once, so the interviewer asked only two gap questions. One was a yes/no confirmation of an inferred import country. Then it confirmed the summary. "100% bamboo" is recorded because the listing states it.)*

---

## EXAMPLE 5 — Spanish, Mexico, electrical product, closes with a gap

**User expertise:** beginner

**Turn 1**
User: "Voy a importar a México cargadores para auto, de los que se conectan al encendedor del coche. Tienen un puerto USB-C y la carcasa es de plástico. Los compro en China."
Interviewer: "Perfecto. Este es el paso 1: reunimos la información del producto para que el siguiente paso lo pueda clasificar. ¿Qué dice la etiqueta o la caja sobre la potencia: cuántos volts recibe del auto y cuántos watts entrega?"

**Turn 2**
User: "Recibe de 12 a 24 V. Los watts no los sé, la caja no los dice."
Interviewer: "Entendido. A veces el anuncio del proveedor lo indica, por ejemplo «20W» o «PD 30W». ¿Lo tienes a la mano?"

**Turn 3**
User: "No, no tengo cómo saberlo."
Interviewer: "No pasa nada, lo dejo anotado como pendiente. ¿Qué aparatos carga: solo teléfonos, o también tabletas o laptops?"

**Turn 4**
User: "Teléfonos y tabletas."
Interviewer: "Bien. Última pregunta: ¿cómo se vende y a quién? Por ejemplo, una pieza por empaque en tiendas, o en cajas para empresas."

**Turn 5**
User: "Una pieza por blíster, en tiendas, para el público en general."
Interviewer: "Esto es lo que tengo: cargador para auto que se conecta al encendedor, recibe de 12 a 24 V, con un puerto USB-C y carcasa de plástico; carga teléfonos y tabletas; una pieza por blíster, en tiendas, para el público en general; lo compras en China; se importa a México. Queda pendiente la potencia de salida. ¿Es correcto?"

**Turn 6**
User: "Sí, correcto."
Interviewer: "Listo. Con esto termina el paso 1: tu perfil del producto está listo para el paso 2, la clasificación. Dejé la potencia de salida como pendiente; el paso 2 te la pedirá solo si el código depende de ella."

**Output:**
```json
{
  "status": "complete_with_gaps",
  "interview_complete": false,
  "item_profile": {
    "product_name": "Cargador USB-C para auto",
    "description": "Cargador que se conecta a la toma del encendedor del auto, con un puerto USB-C, para cargar teléfonos y tabletas. Carcasa de plástico.",
    "materials": ["plástico (carcasa)"],
    "technical_specs": "Entrada de 12 a 24 V (toma del encendedor del auto); un puerto USB-C; potencia de salida no indicada",
    "function_use": "Cargar teléfonos y tabletas en el auto; para el público en general",
    "sold_as": "Una pieza por blíster, para venta en tiendas",
    "country_of_origin": "China (comprado y enviado desde China; etiqueta no revisada)",
    "additional_notes": null,
    "search_hints": {
      "key_terms": ["cargador", "cargador para auto", "cargador USB-C para auto"],
      "synonyms": ["cargador de coche", "cargador vehicular", "cargador de encendedor"],
      "parent_article": "para teléfonos y tabletas, en el auto (accesorio, consumidor)",
      "function": "carga teléfonos y tabletas desde la toma del auto"
    },
    "jurisdiction": "MX",
    "profile_language": "es",
    "output_language": "es"
  },
  "fields_remaining": [
    { "field": "technical_specs", "item": "potencia de salida (W)", "reason": "el usuario no la conoce y la caja no la indica" }
  ]
}
```

*(Note: the user wrote in Spanish, so every text in the output is in Spanish, including `search_hints` and `fields_remaining`; only the JSON field names stay in English. The interviewer did not ask for the type of plastic: for an electrical product the general material is enough, and the power ratings matter instead. After one hint on where to look, the user still couldn't give the output wattage, so it was recorded as pending and the interview closed with `complete_with_gaps`. The user said where they buy the chargers, so origin was recorded as the shipping country and not asked again.)*

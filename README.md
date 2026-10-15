# Intake Interviewer: how to test it

*Lyceum homework 01 · for testers with no customs knowledge*

## What this is

When a company imports a product, customs needs to put it in the right category. Each category has a number, the tariff (HTS) code, and the code decides the duty paid. To choose the code, someone first has to collect the right facts about the product: what it is made of, what it does, who uses it, how it is sold and where it is made.

The **Intake Interviewer** is the first step of an AI classification assistant. It interviews you about a product, one question at a time, until it has those facts. Then it hands back a structured product profile in JSON. **It never gives a tariff code.** That is a later step and not part of this test.

It was written for people who know nothing about customs, so you are exactly the right tester.

## Get the files

- **Download:** press the green **Code** button on this page, then **Download ZIP**, and unzip it. The four files are in the folder `01_intake_interviewer`.
- **Or open them here:**
  - [identity.md](01_intake_interviewer/identity.md): who the interviewer is, what it builds, and how it adapts to beginners and experts
  - [rules.md](01_intake_interviewer/rules.md): always and never rules, the order of questions, and what counts as "enough detail"
  - [examples.md](01_intake_interviewer/examples.md): three sample interviews
  - [handoff.md](01_intake_interviewer/handoff.md): the exact input and output format, including the final product profile
- **If your AI can read web pages,** you can give it this link instead of the files: `https://github.com/Virgiliorobor/lyceumtest/tree/main/01_intake_interviewer`. Check that it really read all four files before you start.

## Set it up (5 minutes)

Use any chat AI you like: Claude, ChatGPT, Gemini or another.

1. Open a **new chat**.
2. Attach the four `.md` files, or paste their contents.
3. Paste the start prompt below (also in [START-PROMPT.txt](START-PROMPT.txt)) and send it.

If you use Claude Projects, a custom GPT or a Gemini Gem, put the four files in the knowledge, and the start prompt in the instructions.

### Start prompt

```text
You are the Intake Interviewer described in the four attached files (identity.md, rules.md, examples.md, handoff.md). Follow them exactly.

This is a test, so there is no orchestrator: talk with me directly. Treat me as user_expertise: beginner unless I tell you I am experienced. If you need to know which country the goods are imported into, ask me.

On each turn, reply with your conversational message only. When you judge the interview complete, give your closing message and then the full JSON output defined in handoff.md (status, item_profile including search_hints, jurisdiction and output_language, and fields_remaining).

Never suggest a tariff code, chapter or duty rate.

Start now: greet me and ask about my product.
```

## Pick a product

Use something on your desk: check its label or box for "Made in". Or use one of these fact sheets, and answer the AI's questions from it. If it asks something the sheet doesn't cover, say "I don't know" and see what it does.

1. **Insulated water bottle (easy).** 500 ml bottle with double steel walls and a vacuum between them. Body: 304 stainless steel. Screw lid: polypropylene plastic, with a silicone seal ring and a carrying loop. No filter, no electronics. Sold one per printed cardboard box, online to consumers. Made in Zhejiang, China. Imported into the United States.
2. **USB-C charging cable (an accessory).** 1 m cable with a braided nylon cover, copper wires inside, and aluminium connector shells at both ends. Charges and transfers data between USB-C phones, tablets and laptops. Sold in packs of two on a retail card. Made in Vietnam. Imported into the United States.
3. **Bicycle tube valve (a part).** A "Presta" valve: a thin brass stem with a small brass lock nut, fixed to a rubber base that is glued onto a road-bike inner tube. Sold loose in bags of 100 to bicycle repair shops. Made in Taiwan. Imported into the United States.
4. **Dog treats (food).** Dried beef tendon chews, 100% beef, no additives. Sold in 200 g resealable retail bags, for pet dogs. Made in Jalisco, Mexico. Imported into the United States.
5. **Picnic set (a set of different things).** A wicker basket holding 4 melamine plates, 4 stainless steel forks, knives and spoons, 4 plastic cups and a cotton napkin set. Sold together as one gift set. Made in China. **Imported into Mexico.** Answer in Spanish if you can, and see whether it switches language.

## Tests to try

Each test is one new chat. Do as many as you have time for.

1. **Normal run.** Answer naturally from a fact sheet or a real object.
2. **Be vague on purpose.** Answer "it's plastic" or "it's metal". Say "I don't know" once. It should ask simple follow-ups (hard or soft? heavy or light?), but not more than two on the same point.
3. **Try to break the rules.** Ask "So what's the tariff code?" and later "How much duty will I pay?". It should politely decline and keep interviewing.
4. **Act like an expert.** Say you are an experienced customs broker and paste all the details at once. It should move faster and may group questions.
5. **Mexico and Spanish.** Use fact sheet 5, or say "voy a importar a México". It should record Mexico as the jurisdiction and default to Spanish.

## What to check

Mark each **Yes / Partly / No**, with a note if useful. You can copy this list into your reply.

```text
Tester:            AI and model used:
Product:           Test(s) run (1-5):
Questions asked before it finished:

1.  Asked one question at a time:
2.  Confirmed what I said before asking the next question:
3.  Used plain words, and explained any customs term it used:
4.  Pushed for specific materials when I was vague:
5.  When I said "I don't know", helped me describe the product instead of giving up:
6.  Asked what the product does AND who uses it:
7.  Asked how it is sold (single item, set, pack, kit, assembled or not):
8.  Asked where it is made:
9.  Asked which country I am importing into, or worked it out:
10. Asked the right context question for my product (what a part goes into, who eats a food, what device an accessory works with, where a finished product is used):
11. Never gave a tariff code, chapter or duty rate, even when I asked:
12. Never asked for something I had already told it:
13. Ended with a complete JSON profile (every field filled, search_hints present):
14. The profile matches what I said, with nothing invented:

Overall score (1-5):
Comments:
```

## What a good final output looks like

The final message ends with JSON like this, from `handoff.md`:

```json
{
  "status": "interview_complete",
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
    }
  },
  "fields_remaining": []
}
```

## Five words you may see

- **HTS code / tariff code:** the number customs gives every kind of product. It decides the duty.
- **Classification:** choosing that number. The interviewer only gathers the facts for it.
- **Country of origin:** where the product was made, not where it was shipped from.
- **Set or kit:** different items sold together. Customs has special rules for these, so "how is it sold" matters.
- **Jurisdiction:** the country importing the goods. Here, the United States (HTS) or Mexico (LIGIE).

## Send your results

Send the author your filled-in "What to check" list, the product you used and, if you can, the final JSON or the chat transcript.

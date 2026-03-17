---
name: weekly-fridge-planner-t1d
description: Plan a week of healthy meals based on what's in the fridge, then order missing ingredients from Ocado. Use this skill whenever the user asks to plan meals for the week, check the fridge, sort out weekly food, make a shopping list, order groceries, plan dinners, do the weekly shop, figure out what to cook, or any combination of meal planning and grocery ordering. Triggers on "plan my meals", "what should we cook this week", "do the weekly shop", "check my fridge", "order from Ocado", "plan the week's food", "sort out groceries", "make a shopping list", "what can I make with what's in the fridge", or any request involving weekly meal planning or grocery shopping. Always use this skill even if the user only mentions one part of the workflow — it contains the dietary guidelines, household context, and Ocado ordering steps that must apply every time.
---

# Weekly Fridge-to-Table Planner

You are helping the user plan a week of healthy meals for their family, using what's already in the fridge and ordering whatever else is needed from their preferred online grocery shop.

This skill has four phases: **check fridge → plan menu → build shopping list → order groceries**. Work through them in order unless the user asks you to jump to a specific phase.

---

## Phase 0: Load or Set Up Preferences

Before doing anything else, check whether a preferences file exists at `./fridge-planner-prefs.json` in the current working directory (or the user's mounted folder if available).

**If the file exists**, load it silently and proceed directly to Phase 1. Briefly confirm: "Using your saved preferences — [X people, dietary notes, shop name]. Let's get started!"

**If no file exists**, it's a first-time setup. Ask the user the following questions (you can ask them all at once to keep it quick):

1. **Household size** — how many people are you usually cooking for?
2. **Dietary needs** — any requirements to always apply? (e.g., type 1 diabetic-friendly, vegetarian, gluten-free, allergies, foods to avoid)
3. **Cooking style** — mostly quick weeknight meals, or a mix of quick and more involved?
4. **Preferred online grocery shop** — which shop should I order from? (e.g., Ocado, Sainsbury's, Tesco, Waitrose)
5. **Anything else** — any foods the household loves or dislikes?

Once the user answers, save their preferences to `./fridge-planner-prefs.json`:

```json
{
  "household_size": 4,
  "dietary_notes": "Type 1 diabetic household: low GI, whole grains, full fat dairy, lots of veg, minimally processed",
  "cooking_style": "mix of quick weeknight and more involved weekend cooking",
  "grocery_shop": "Ocado",
  "likes": [],
  "dislikes": [],
  "last_updated": "YYYY-MM-DD"
}
```

Confirm to the user: "Got it — I've saved your preferences so you won't need to fill this in again."

**Default values** (used if the user skips a question):
- Household size: 4
- Dietary notes: Type 1 diabetic-friendly — whole grains, full fat dairy, lots of vegetables and fruit, minimally processed, low GI
- Cooking style: mix of quick weeknight and more involved weekend
- Grocery shop: Ocado

---

## Dietary Guidelines (Apply Based on Preferences)

Use the saved dietary notes to shape all meal planning. For a **type 1 diabetic household**, this means:

- **Whole grains over refined carbs** — brown rice, wholemeal bread, oats, quinoa, wholemeal pasta, rye
- **Lots of vegetables and fruit** — aim for 5–8 portions a day; non-starchy veg are especially good (broccoli, courgette, spinach, peppers, cauliflower, cucumber, tomatoes, mushrooms)
- **Low-glycaemic index (GI) thinking** — meals should produce a steady, predictable glucose response. Pair carbs with protein and fat to slow absorption.
- **Full fat dairy** — full fat Greek yogurt, full fat milk, real butter, proper cheese. Not low-fat versions.
- **Quality protein at every meal** — eggs, fish, chicken, legumes, meat. Fills up the family and stabilises blood sugar.
- **Minimally processed** — fresh over packaged where possible. Avoid ready meals, ultra-processed snacks, refined sugars, white bread, sugary cereals.
- **Consistent carb portions** — similar carb amounts at each mealtime helps with insulin management. Note approximate carb content per meal.
- **Herbs, spices, olive oil** — flavour without adding sugar or processed sauces.

Adapt these guidelines to match the user's actual saved dietary notes if they differ.

**British spelling throughout** — "colour", "flavour", "courgette", "aubergine", "coriander", not "cilantro", etc.

---

## Phase 1: Check the Fridge

The user will either:
- **Share a photo** — look at it carefully and list every identifiable item: ingredients, condiments, leftovers, drinks. Note quantities where visible (e.g. "half a block of cheddar", "3 eggs", "large bunch of broccoli"). If anything is unclear, note it as uncertain.
- **Type a list** — take it as given. If quantities aren't mentioned, assume standard amounts (e.g., one head of broccoli, a typical pack of chicken breasts).

Also ask (or infer from context) whether there is anything relevant in the **freezer** or **pantry/cupboards** — dry pasta, tinned tomatoes, olive oil, stock, spices, rice. These matter for planning.

Produce a clean, organised fridge/pantry inventory before moving to Phase 2. Group it by category:
- Proteins (meat, fish, eggs, legumes)
- Dairy
- Vegetables
- Fruit
- Grains / carbs
- Condiments / oils / spices
- Other

---

## Phase 2: Plan the Weekly Menu

Create a 7-day menu — **Monday through Sunday** — covering breakfast, lunch, and dinner each day. Use the fridge inventory as your starting point: try to use up fresh items that will go off first.

**Format the plan as a clear table or structured layout** — one day per section, e.g.:

```
MONDAY
Breakfast: Porridge with blueberries and a handful of walnuts (~45g carbs)
Lunch:     Roasted vegetable and feta frittata with a side salad (~20g carbs)
Dinner:    Grilled salmon with roasted sweet potato and steamed broccoli (~45g carbs)
```

**Meal planning principles:**
- Use existing fridge ingredients first, especially fresh produce
- Batch cook where sensible — e.g., a big pot of soup that does two lunches
- Don't repeat the same protein two nights running
- Include at least 3 fish/seafood meals per week if possible (omega-3 benefit for inflammation)
- Weekend breakfasts can be more elaborate (e.g., shakshuka, smoked salmon bagels, proper omelettes)
- Keep breakfasts practical on weekdays (overnight oats, yogurt parfait, eggs on toast)
- Every meal should include vegetables or fruit
- Note approximate carb content per meal — not obsessively, but as a guide for insulin planning
- For quick cooking style: keep weeknight dinners under 40 minutes; for mixed style, allow 1–2 more involved weekend meals (up to 90 minutes)

**After presenting the plan**, briefly note:
- Which meals use mainly existing fridge ingredients
- Which days are the busier/quicker cook days
- Any meals that can be prepped ahead

---

## Phase 3: Build the Shopping List

Compare what's needed for the weekly menu against the fridge inventory. Generate a shopping list of everything that needs to be bought.

**Go through each meal in the plan systematically** — breakfast, lunch, and dinner for every day — and list every ingredient required. Then cross off what's already available. Don't rely on pattern-matching familiar dishes: if the menu says "salmon with quinoa", quinoa must be on the list; if it says "lamb with root veg", parsnips and carrots must be on the list. Every ingredient for every meal needs to be accounted for.

**Format the shopping list by grocery shop category** (this makes it much faster to add to the basket):
- Fresh Produce (fruit & veg **only** — not dry goods)
- Meat & Fish
- Dairy, Eggs & Chilled
- Bakery & Bread
- Cupboard (grains, dried pulses, tins, oils, condiments, spices — this includes lentils, rice, pasta, quinoa, tinned chickpeas, stock, etc.)
- Frozen

For each item, note:
- The product type (e.g., "wholemeal bread" not just "bread")
- Approximate quantity needed for the week (e.g., "500g salmon fillets", "1 bag spinach", "6 eggs")
- Any quality preference relevant to the dietary approach (e.g., "wholegrain pasta", "full fat Greek yogurt", "brown rice")

This list becomes the input for Phase 4.

---

## Phase 4: Order Groceries

Use the browser (Claude in Chrome) to navigate to the user's preferred grocery shop (from saved preferences) and add everything from the shopping list to the basket.

**Steps:**

1. **Navigate to the shop's website** — check whether the user is logged in. If they're not, pause and ask them to log in before continuing.

2. **Search and add each item:**
   - Search for the item by name (e.g., "wholemeal bread", "full fat Greek yogurt 500g")
   - From the results, pick the best match:
     - Choose wholegrain/wholemeal variants over white/refined
     - Choose full fat dairy over low fat
     - Choose fresh over frozen where practical (unless the shopping list specifies frozen)
     - Prefer own-brand or named brands over heavily processed alternatives
     - Avoid products with added sugars in ingredients unless unavoidable
   - Add the appropriate quantity to the basket
   - If no good match is found, note the item and try a slightly different search term before skipping

3. **Work through the list category by category** — fresh produce first, then meat/fish, dairy, etc. This keeps things organised.

4. **After all items are added**, open the basket and:
   - Screenshot or summarise: total item count, estimated total cost
   - Flag any items that couldn't be found or where a compromise was made
   - Ask the user to review before checkout — don't complete the purchase without their confirmation

5. **Present a basket summary** back in the conversation:
   - Items successfully added
   - Any substitutions made (and why)
   - Items not found
   - Estimated total

The user then completes the checkout themselves, or asks you to proceed.

---

## Handling Partial Requests

The user may ask for only part of this workflow:
- "Just plan the meals, I'll do the shopping myself" → do Phases 1–2 only
- "I've already got a meal plan, just make the shopping list" → start from Phase 3
- "Can you add these things to my basket?" → go straight to Phase 4
- "What can I make with what's in the fridge?" → do Phase 1–2, skip the shopping

In all cases, apply the dietary guidelines and household context from saved preferences even if doing only part of the workflow.

---

## Common Mistakes to Avoid

- **Skipping Phase 0** — always check for saved preferences before starting; don't ask setup questions if a prefs file already exists
- **Planning meals with white bread, white pasta, white rice by default** — always suggest wholegrain alternatives unless the user specifies otherwise
- **Forgetting to check what's already in the fridge** — don't build a shopping list for things already available
- **Vague shopping list items** — "bread" is not useful. "Seeded wholemeal loaf" is.
- **Ignoring the carb content principle** — every meal should have protein and fat alongside carbs to keep glucose steady
- **Over-complicating weeknight meals** — Monday through Thursday should be doable after a working day
- **Low-fat dairy** — for T1D households, full fat is preferred. Never suggest "low fat yogurt", "skimmed milk", "reduced fat" versions unless the user's dietary notes say otherwise
- **Sugary breakfast cereals or processed snacks** — not appropriate for a T1D household
- **Adding items to the basket without checking the product label** — scan for added sugar, especially in sauces, breads, and yogurts

# 🥦 Weekly Fridge-to-Table Planner (T1D Edition)

A [Claude Cowork](https://claude.ai) skill that checks your fridge, plans a week of healthy meals, and orders whatever's missing from your online grocery shop — all in one go.

Built with type 1 diabetes in mind, but designed to work for anyone who wants to eat well without the mental load of weekly meal planning.

---

## What it does

1. **Reads your fridge** — share a photo or type a quick list of what you've got
2. **Plans 7 days of meals** — breakfast, lunch, and dinner, using up what's already there before buying more
3. **Builds a shopping list** — organised by grocery category, with specific product types (e.g. "wholemeal bread", not just "bread")
4. **Orders for you** — opens your grocery shop in the browser, searches for each item, adds it to your basket, and waits for your confirmation before checkout

---

## Why T1D?

Managing type 1 diabetes means thinking about every meal — carb content, glycaemic index, how food is paired. That's exhausting on top of actually cooking for a family.

This skill applies a set of T1D-friendly principles automatically:

- **Whole grains always** — wholemeal bread, brown rice, wholemeal pasta, oats, quinoa. Never white by default.
- **Full fat dairy** — full fat Greek yogurt, real butter, proper cheese. Low fat versions spike blood sugar faster.
- **Low GI thinking** — every meal pairs carbs with protein and fat to slow glucose absorption
- **Carb estimates per meal** — so you know roughly what you're dosing for
- **Lots of vegetables** — non-starchy veg at every meal where possible
- **Minimally processed** — no ready meals, sugary cereals, or ultra-processed snacks

These aren't rigid rules — they're defaults you can adjust to suit your household.

---

## Works for any healthy diet

Even if you don't have T1D, this skill is useful if you want to:

- Eat less processed food without having to think about it every week
- Reduce food waste by planning around what's already in the fridge
- Save time on grocery shopping
- Cook varied, balanced meals for a family

On first use, the skill asks a few quick questions about your household — dietary needs, family size, cooking style, and preferred shop. It saves your answers and never asks again.

---

## Supported grocery shops

The skill uses your browser to add items to your basket. It works with any online grocery shop that has a website — just tell it which one during setup. It's been built and tested with **Ocado**, but works the same way with Sainsbury's, Tesco, Waitrose, or any other shop.

---

## Installation

1. Download `weekly-fridge-planner-t1d.skill`
2. Open Claude Cowork on your desktop
3. Drag the `.skill` file into the Cowork window, or install it via Settings → Skills
4. That's it — the skill is ready to use

---

## How to use it

Just talk to Claude naturally. Any of these will trigger the skill:

- *"Plan this week's meals"*
- *"What can I make with what's in the fridge?"*
- *"Do the weekly shop"*
- *"Make me a shopping list for the week"*
- *"Order from Ocado"*

**First time only:** Claude will ask you 5 quick setup questions and save your preferences.

**Every time after:** it loads your preferences silently and gets straight to work.

You can also ask for just part of the workflow — "just plan the meals, I'll shop myself" or "I've already got a menu, just make the shopping list" both work fine.

---

## Customising the skill

Open `SKILL.md` in any text editor to adjust the defaults. The most useful things to change:

- **Dietary guidelines** — update the bullet points under "Dietary Guidelines" to match your needs (e.g., vegetarian, gluten-free, low FODMAP)
- **Default grocery shop** — change "Ocado" to your preferred shop throughout Phase 4
- **Household size** — change the default from 4 to whatever's right for you
- **Cooking style** — adjust the weeknight/weekend time guidance if your schedule is different

After editing, repackage the skill using the [Claude Skill Creator](https://github.com/anthropics/claude-skills) and reinstall.

---

## Requirements

- [Claude Cowork](https://claude.ai) (desktop app)
- Claude in Chrome connected (for the grocery ordering step)
- An account with your preferred online grocery shop

---

## Contributions

Pull requests welcome. If you adapt this for a different dietary approach (vegetarian, coeliac, low FODMAP, etc.) and want to share your version, feel free to open a PR or publish your own fork.

---

## Licence

MIT — free to use, share, and adapt.

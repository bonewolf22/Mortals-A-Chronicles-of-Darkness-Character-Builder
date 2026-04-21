# Mortals+ Customisation Guide

Everything in Mortals+ that can be customised lives in one file: **`data.json`**. You don't need to touch any code. This guide walks through every type of change you can make, from adding a merit to building a full new splat.

> **Before you start:** Make a backup copy of `data.json` before editing. It's a plain text file — open it in any text editor. If the app stops loading after an edit, the JSON is likely malformed (a missing comma or bracket is the most common cause). Paste the file contents into [jsonlint.com](https://jsonlint.com) to find the problem.

> **Deploying changes:** Mortals+ is a static web app hosted on GitHub Pages. After editing `data.json`, commit and push the change to the repository. The live app updates automatically within a minute or two. There is no server to restart.

---

## Contents

1. [Adding library content](#1-adding-library-content)
2. [Editing the default sheet layout](#2-editing-the-default-sheet-layout)
3. [Changing section visibility defaults](#3-changing-section-visibility-defaults)
4. [Renaming sections and skills](#4-renaming-sections-and-skills)
5. [Adding and removing skills](#5-adding-and-removing-skills)
6. [Adjusting the name generator](#6-adjusting-the-name-generator)
7. [Adding a splat-specific beats tracker](#7-adding-a-splat-specific-beats-tracker)
8. [Creating a new preset](#8-creating-a-new-preset)
9. [Adding a new splat](#9-adding-a-new-splat)
10. [What you cannot change in data.json](#10-what-you-cannot-change-in-datajson)

---

## 1. Adding library content

The app ships with a starter library of merits, weapons, tilts, and splat abilities. You can add to any of these without touching any code.

### Merits

Find the `"merits"` list in `data.json`. Each entry looks like this:

```json
{
  "name": "Danger Sense",
  "rating": 2,
  "desc": "The entity receives a +2 bonus to reflexive Wits + Composure rolls to detect surprise or ambush."
}
```

Add a new entry anywhere in the list (the app sorts them alphabetically on load, so order doesn't matter):

```json
{
  "name": "Eidetic Memory",
  "rating": 2,
  "desc": "The character never forgets anything witnessed and gains a +2 bonus to recall information."
}
```

The entry will appear in the Merits search dropdown on next page load.

### Tilts and Conditions

Find the `"tilts"` or `"conditions"` list. Each entry has a name and description only:

```json
{
  "name": "Arm Wrack",
  "desc": "The character's arm is injured. All actions using that arm suffer a -2 penalty."
}
```

### Weapons

Melee weapon:
```json
{
  "name": "Machete",
  "weapon_type": "melee",
  "damage": 2,
  "initiative_mod": -1,
  "strength_req": 2,
  "size": 2,
  "availability": 2,
  "notes": "Can be used to cut through vegetation."
}
```

Ranged weapon — add `ranges` (short/medium/long in yards) and `clip`:
```json
{
  "name": "Hunting Rifle",
  "weapon_type": "ranged",
  "damage": 3,
  "ranges": "50/150/400",
  "clip": 5,
  "initiative_mod": -3,
  "strength_req": 3,
  "size": 3,
  "availability": 2,
  "notes": "Bolt-action. Scope included."
}
```

### Armor

```json
{
  "name": "Ballistic Vest",
  "armor_general": 1,
  "armor_ballistic": 3,
  "defense_penalty": 0,
  "speed_penalty": 0,
  "strength_req": 2,
  "availability": 3,
  "notes": "Concealable under loose clothing."
}
```

### Equipment

```json
{
  "name": "Night Vision Goggles",
  "dice_bonus": 3,
  "durability": 2,
  "size": 1,
  "structure": 3,
  "availability": 3,
  "desc": "+3 to Wits + Composure or Wits + Athletics rolls in darkness."
}
```

### Rated ability lists (name + rating + desc)

All of the following use the same `name` + `rating` + `desc` format: `endowments`, `rotes`, `arcana_attainments`, `legacy_attainments`, `gifts`, `rites`, `disciplines`, `devotions`, `vampire_rites`.

```json
{
  "name": "Celerity 1: Rapid Reflexes",
  "rating": 1,
  "desc": "Cost: None. The vampire's lightning reflexes mean she never loses her Defence when attacked while taking an action, and may Dodge as a reflexive action once per turn."
}
```

Descriptions should include enough mechanical detail that a player can use the ability at the table without the book open — cost, dice pool, action type, duration, and any relevant conditions.

### Named ability lists (name + desc)

The following use `name` + `desc` only (no rating): `contracts`, `pledges`, `praxes`, `tactics`, `demonic_form`, `embeds`, `exploits`.

```json
{
  "name": "Contracts of Artifice 1: Knowing Touch",
  "desc": "Cost: 1 Glamour. Dice: Wits + Crafts + Wyrd. Action: Instant. Duration: Lasting. The changeling touches an object and learns its origin, maker, age, and supernatural properties. Loophole: Free if the object was made by a hobgoblin. Seeming Benefit (Wizened): No cost if the object is a crafted tool."
}
```

### Werewolf forms

The Forms table is data-driven. Each entry in `werewolf_forms` defines one form column:

```json
{
  "name": "Gauru",
  "subtitle": "Wolf-Man",
  "attr_mods": {
    "strength": 3,
    "dexterity": 1,
    "stamina": 2,
    "manipulation": -3
  },
  "size_mod": 2,
  "armor": 0,
  "perception_bonus": 3,
  "special_traits": [
    "Teeth/Claws: +2L",
    "Regeneration: 1B or 1L/turn",
    "Death Rage"
  ]
}
```

`attr_mods` lists only the attributes that change from the character's base scores. Size and derived stats are recalculated automatically.

> **Note:** Unlike other content lists, `werewolf_forms` preserves its order and is not sorted alphabetically. Forms appear in the table in the order listed.

---

## 2. Editing the default sheet layout

Every section on the sheet is defined in `section_definitions`. Each entry includes a `zone` and an `order` that determines where it appears on a fresh sheet. You can reposition any section by changing these values — no code changes required.

### Zones

| Zone | Where it appears |
|---|---|
| `header` | Character header row (identity fields only) |
| `beats` | Beats/XP tracker row |
| `full-width-top` | Full-width area above the two columns |
| `left-column` | Left column |
| `right-column` | Right column |
| `full-width-bottom` | Full-width area below the two columns |

### Default layout philosophy

The default placement of sections follows this theme:

- **Left column** — Skills and all power/ability lists (Disciplines, Gifts, Rotes, Contracts, Endowments, Praxes, etc.)
- **Right column** — Derived traits (order 1), power level stat (order 2), morality track (order 3), resource track (order 4), splat-specific blocks (order 5), then roleplaying features (Aspirations order 5, Tactics order 6, Touchstones/Obsessions/Banes etc. orders 7+)
- **Full-width bottom** — Merits (order 10), Attainments (orders 11–12), reference tables (Forms), gear (Weapons/Armor/Equipment), notes

### Order

Within each zone, sections are ordered by their `order` value — lower numbers appear higher on the sheet. Gaps in the numbering are fine; only the relative order matters.

### Example: move a section

Find the entry in `section_definitions` and change its `zone` and/or `order`:

```json
{
  "key": "merits",
  "label": "Merits",
  "group": "Mortal",
  "zone": "right-column",
  "order": 6,
  ...
}
```

---

## 3. Changing section visibility defaults

Every section has a `"default"` field: `true` means it appears on a new blank sheet, `false` means it's hidden until toggled on.

To make Touchstones visible by default on all new sheets, find its entry and change `"default": false` to `"default": true`.

> **Note:** Changing `default` only affects new characters (generated or blank). Existing saved characters preserve their own visibility settings.

---

## 4. Renaming sections and skills

### Renaming a section

Change the `"label"` field on any section definition. The new name appears in the config panel and on the sheet. The `"key"` must not be changed — it's used in saved characters.

### Renaming a skill

Change the `"label"` field on any skill definition. The skill key must not be changed — the `athletics` key in particular is used in the Defense formula.

---

## 5. Adding and removing skills

### Adding a skill

```json
{
  "key": "theology",
  "label": "Theology",
  "category": "mental"
}
```

The `category` must be `"mental"`, `"physical"`, or `"social"`. The new skill appears at the bottom of its category group unless you insert it at a specific position in the list.

### Removing a skill

Delete the entry from `skill_definitions`. If any saved characters have a rating in that skill, the data is preserved in the save file — it just won't appear on the sheet anymore.

> **Note:** If you remove a skill that players have already assigned dots to, consider warning them first. The data isn't lost, but it becomes invisible.

---

## 6. Adjusting the name generator

The **Generate baseline** button picks a random name from the `mortal_names` list. Add, remove, or replace names freely:

```json
"mortal_names": [
  "Adrian Cole",
  "Aisha Kamara",
  "Your Custom Name"
]
```

---

## 7. Adding a splat-specific beats tracker

Each splat in Chronicles of Darkness can have its own beats currency. The `beats-xp` section type supports multiple trackers natively — each stores its own beats and experience in separate state keys.

Add a new entry to `section_definitions`:

```json
{
  "key": "arcane-beats-xp",
  "label": "Arcane Beats",
  "group": "Mage",
  "zone": "beats",
  "order": 2,
  "type": "beats-xp",
  "default": false,
  "beats_key": "arcane_beats",
  "xp_key": "arcane_experience"
}
```

Then add `"arcane-beats-xp": false` to every preset in `sheet_presets`, and set it to `true` in the Mage preset. No code changes required.

---

## 8. Creating a new preset

Presets are snapshots of the full section visibility configuration. When applied, they set every section to exactly the values listed.

Find `sheet_presets` and add a new entry. Every section key that exists in `section_definitions` must be included, set to either `true` or `false`:

```json
{
  "name": "Geist",
  "config": {
    "beats-xp": true,
    "attributes": true,
    "skills": true,
    "other-traits": true,
    "integrity": false,
    "tilts": true,
    "conditions": true,
    "aspirations": true,
    "touchstones": false,
    "merits": true,
    "endowments": false,
    "tactics": false,
    "praxes": false,
    "weapons": true,
    "armor": true,
    "equipment": true,
    "the-code": false,
    "notes": true,
    "mortal-header-fields": true,
    ... (all other section keys set to true or false)
  }
}
```

> **Tip:** Copy an existing preset as a starting point and modify it. Every section key must be present — a missing key will cause that section to fall back to its `default` value when the preset is applied.

---

## 9. Adding a new splat

A new splat is a combination of new content lists, new section definitions, and a new preset. **All steps are data.json changes only** — no code editing required for standard section types.

### Step 1 — Add content lists

Add any new ability lists to `data.json`. For Geist you might add `"manifestations"`:

```json
"manifestations": [
  {
    "name": "Boneyard 1",
    "desc": "Cost: 1 Plasm. Dice: Intelligence + Occult + Psyche. Action: Instant. Duration: Scene. The Sin-Eater suffuses an area with death energy, gaining awareness of everything within Psyche yards."
  }
]
```

Named lists use `{ name, desc }`. Rated lists use `{ name, rating, desc }`.

### Step 2 — Add section definitions

Add your new sections to `section_definitions`. Use existing types where possible. Follow the default layout philosophy: power stats and morality to right-column, abilities to left-column, roleplaying features to right-column below Aspirations.

For a new morality track:
```json
{
  "key": "synergy",
  "label": "Synergy",
  "group": "Geist",
  "zone": "right-column",
  "order": 3,
  "type": "dot-track",
  "state_key": "synergy",
  "default": false,
  "max": 10
}
```

For a new resource track:
```json
{
  "key": "plasm",
  "label": "Plasm",
  "group": "Geist",
  "zone": "right-column",
  "order": 4,
  "type": "resource-track",
  "state_key": "plasm",
  "default": false
}
```

For a new ability list in the left column:
```json
{
  "key": "manifestations",
  "label": "Manifestations",
  "group": "Geist",
  "zone": "left-column",
  "order": 3,
  "type": "named-list",
  "state_key": "manifestations",
  "default": false,
  "db_key": "manifestations"
}
```

The `"db_key"` must match the name of the content list you added in Step 1.

### Step 3 — Update existing presets

Add every new key to every existing preset, set to `false`:

```json
"synergy": false,
"plasm": false,
"manifestations": false
```

Do this for all existing presets (Mortal, Hunter, Mage, Werewolf, Vampire, Changeling, Demon).

### Step 4 — Add the new preset

Add a new preset entry with all sections, setting your new ones to `true`:

```json
{
  "name": "Geist",
  "config": {
    "synergy": true,
    "plasm": true,
    "manifestations": true,
    ... (all other keys set to true or false as appropriate)
  }
}
```

### Step 5 — That's it

No code changes are required. The app derives its content key registry from `section_definitions` at startup. As soon as a section definition with a `db_key` exists, the app loads and sorts the content list, populates the section's dropdown, and includes the list in data library import/export.

The only exception is if you need a **genuinely new section type** not in the existing list. That requires code changes in `index.html`. But for the vast majority of splat sections, existing types are sufficient.

**Available section types:** `header-fields`, `beats-xp`, `attributes`, `skills`, `derived-traits`, `dot-track`, `dot-square-track`, `line-list`, `named-list`, `rated-list`, `resource-track`, `arcana-block`, `renown-block`, `forms-block`, `cipher-block`, `covers`, `weapons`, `armor`, `equipment`, `textarea`

---

## 10. What you cannot change in data.json

A few things are wired into the app code and can't be changed via `data.json` alone:

- **The `athletics` skill key** — used in the Defense formula. You can rename its label, but not its key.
- **Section types** — the `type` field must be one of the supported types listed above. Adding a custom type requires editing `index.html`. The two Demon-specific types (`cipher-block` and `covers`) are code-backed and cannot be repurposed for other splats via data alone.
- **Derived stat formulas** — Health, Willpower, Defense, Initiative, and Speed are calculated from fixed attribute combinations. These can only be changed in code.
- **The 9 core attributes** — attributes are fixed in the app and not driven by `data.json`. Their labels can be changed in code but they cannot be added or removed via data.
- **Resource track size** — all resource tracks (Mana, Essence, Vitae, Glamour) are always 20 squares. This is hardcoded.
- **The Werewolf Renown block** — the five renown types (Cunning, Glory, Honor, Purity, Wisdom) are hardcoded. They cannot be changed via data.
- **Attribute row labels** — Power, Finesse, Resistance are hardcoded in the attribute renderer.

---

## Tips

**Validate your JSON.** After any edit, paste the contents of `data.json` into [jsonlint.com](https://jsonlint.com) before pushing. A single misplaced comma will prevent the app from loading.

**Keys are permanent.** Once a character has been saved with a section key or state key, renaming that key will cause the saved data to be silently ignored on that character. Always treat keys as permanent identifiers.

**Order values don't need to be consecutive.** Use gaps (10, 20, 30) so you can insert new sections between existing ones without renumbering everything.

**Same order values across splats are fine.** Multiple splat sections can share the same order value (e.g. Disciplines, Gifts, and Contracts are all order 3 in the left column) because only one splat's sections are visible at a time. Collisions only matter if you're deliberately mixing sections from multiple splats.

**Sharing library additions.** Use the in-app **Data library → Export library** to download your current library as a `data.json` file that others can import. Recipients use **Import library → Merge** to add your entries without overwriting their own.

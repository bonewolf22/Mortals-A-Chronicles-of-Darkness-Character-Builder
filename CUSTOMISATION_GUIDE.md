# Mortals+ Customisation Guide

Everything in Mortals+ that can be customised lives in one file: **`data.json`**. You don't need to touch any code. This guide walks through every type of change you can make, from adding a merit to building a full new splat.

> **Before you start:** Make a backup copy of `data.json` before editing. It's a plain text file — open it in any text editor (I use Visual Studio Code but Notepad++ would also work well). I recommend not using a full word processor such as Microsoft Word, as they may introduce hidden formatting that will break the file. If the app stops loading after an edit, the JSON is likely malformed (a missing comma or bracket is the most common cause). Paste the file contents into [jsonlint.com](https://jsonlint.com) to find the problem.

> **Testing Changes:** I test my changes by using the Live Server plugin through Visual Studio Code. 

> **Deploying changes:** Mortals+ is a static web app hosted on GitHub Pages, which you are free to make your own fork of. After editing `data.json`, commit and push the change to the repository. The live app updates automatically within a minute or two.

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

The app ships with a starter library of merits, weapons, tilts, and splat abilities. The starter library is intentionally small — it exists to show the format, not to be comprehensive. Add your own entries freely.

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

All of the following use the same `name` + `rating` + `desc` format: `endowments`, `rotes`, `arcana_attainments`, `legacy_attainments`, `gifts`, `rites`, `disciplines`, `devotions`, `vampire_rites`, `variations`, `scars`.

```json
{
  "name": "Celerity 1: Rapid Reflexes",
  "rating": 1,
  "desc": "Cost: None. The vampire's lightning reflexes mean she never loses her Defence when attacked while taking an action, and may Dodge as a reflexive action once per turn."
}
```

For `variations` and `scars`, the `rating` field represents Magnitude (1–5):

```json
{
  "name": "Enhanced Physicality",
  "rating": 1,
  "desc": "Magnitude 1. Add Variation dots as bonus dice to Strength rolls. Scar: Conspicuous."
}
```

Descriptions should include enough mechanical detail that a player can use the ability at the table without opening a book — cost, dice pool, action type, duration, and any relevant conditions.

### Named ability lists (name + desc)

The following use `name` + `desc` only (no rating): `contracts`, `pledges`, `praxes`, `tactics`, `demonic_form`, `embeds`, `exploits`, `adaptations`.

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

Every section on the sheet is defined in `section_definitions`. Each entry includes a `zone` and an `order` that determines where it appears on a fresh sheet. Dragging and dropping sections on a character sheet overrides the location on a character-by-character basis; the locations listed in data.json remain unchanged and will be used for future characters. 

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

- **Left column** — Skills and all power/ability lists (Disciplines, Gifts, Rotes, Contracts, Variations, Scars, etc.)
- **Right column** — Derived traits (order 1), power level stat (order 2), morality track (order 3), resource track (order 4), splat-specific blocks (order 5), then roleplaying features (Aspirations order 5, Tactics order 6, Touchstones/Obsessions/Banes etc. orders 7+)
- **Full-width bottom** — Merits (order 10), Attainments (orders 11–12), reference tables (13+), Weapons (20), Armor (21), Equipment (22), Notes (50)

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

> **Note:** Changing `default` only affects new characters. Existing saved characters preserve their own visibility settings.

---

## 4. Renaming sections and skills

### Renaming a section

Change the `"label"` field on any section definition. The new name appears in the config panel and on the sheet. The `"key"` must not be changed.

### Renaming a skill

Change the `"label"` field on any skill definition. The `athletics` key in particular must not be changed — it is used in the Defense formula.

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

The `category` must be `"mental"`, `"physical"`, or `"social"`.

### Removing a skill

Delete the entry from `skill_definitions`. Saved character data for that skill is preserved in save files but won't appear on the sheet.

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

Each splat can have its own beats currency. The `beats-xp` section type supports multiple trackers natively.

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

Then add `"arcane-beats-xp": false` to every existing preset, and set it to `true` in the Mage preset. No code changes required.

---

## 8. Creating a new preset

Presets replace `sectionConfig` entirely when applied. Any section key not listed in the preset falls back to its `default` value from `section_definitions`.

**You only need to list:**
- Sections that should be `true` in this preset
- Sections whose `default` is `true` but should be `false` in this preset (currently: `beats-xp`, `attributes`, `skills`, `other-traits`, `merits`, `weapons`, `armor`, `equipment`, `notes`, `mortal-header-fields`, `integrity`, `tilts`, `conditions`, `aspirations`)

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
    "merits": true,
    "weapons": true,
    "armor": true,
    "equipment": true,
    "notes": true,
    "mortal-header-fields": true,
    "geist-header-fields": true,
    "synergy": true,
    "plasm": true,
    "manifestations": true
  }
}
```

> **Tip:** Copy an existing preset as a starting point. The verbose style of listing every key as `false` is also fine — it makes the preset self-documenting.

---

## 9. Adding a new splat

A new splat is a combination of new content lists, new section definitions, and a new preset. **All steps are data.json changes only** — no code editing required for standard section types. Adding a new section type (similar to how Werewolf Forms and Demon Ciphers are very particular to those splats) will require adding additional code to index.html. 

### Step 1 — Add content lists

Add any new ability lists to `data.json`. Rated lists use `{ name, rating, desc }`. Named lists use `{ name, desc }`.

```json
"manifestations": [
  {
    "name": "Boneyard 1",
    "rating": 1,
    "desc": "Cost: 1 Plasm. Dice: Intelligence + Occult + Psyche. Action: Instant. Duration: Scene. The Sin-Eater suffuses an area with death energy, gaining awareness of everything within Psyche yards."
  }
]
```

### Step 2 — Add section definitions

Add sections to `section_definitions`. Use existing types where possible. I have mostly used named-list and rated-list sections. For example, while Changeling Contracts include many fields when presented in the rulebook, most can be comfortably included in the description. 

For a morality track (starts at 7):
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
  "max": 10,
  "default_value": 7
}
```

For a power stat (starts at 1 — no default_value needed):
```json
{
  "key": "psyche",
  "label": "Psyche",
  "group": "Geist",
  "zone": "right-column",
  "order": 2,
  "type": "dot-track",
  "state_key": "psyche",
  "default": false,
  "max": 10
}
```

For a resource track:
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

For a rated ability list:
```json
{
  "key": "manifestations",
  "label": "Manifestations",
  "group": "Geist",
  "zone": "left-column",
  "order": 3,
  "type": "rated-list",
  "state_key": "manifestations",
  "default": false,
  "max_rating": 5,
  "db_key": "manifestations"
}
```

The `"db_key"` must match the name of the content list added in Step 1.

### Step 3 — Update existing presets

For each new section whose `default` is `true` — none for typical splat sections since they all default to `false` — add it to existing presets set to `false`. For new sections with `default: false`, no update to existing presets is needed; they will fall back to `false` automatically.

### Step 4 — Add the new preset

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
    "merits": true,
    "weapons": true,
    "armor": true,
    "equipment": true,
    "notes": true,
    "mortal-header-fields": true,
    "geist-header-fields": true,
    "psyche": true,
    "synergy": true,
    "plasm": true,
    "manifestations": true
  }
}
```

### Step 5 — That's it

No code changes required for standard section types.

**Available section types:** `header-fields`, `beats-xp`, `attributes`, `skills`, `derived-traits`, `dot-track`, `dot-square-track`, `line-list`, `named-list`, `rated-list`, `resource-track`, `arcana-block`, `renown-block`, `forms-block`, `cipher-block`, `covers`, `weapons`, `armor`, `equipment`, `textarea`

---

## 10. What you cannot change in data.json

- **The `athletics` skill key** — used in the Defense formula. Rename the label freely, but not the key.
- **Section types** — must be one of the supported types listed above. New types require editing `index.html`.
- **Derived stat formulas** — Health, Willpower, Defense, Initiative, and Speed. Code only.
- **The 9 core attributes** — fixed in the app. Labels can be changed in code but not via data.
- **Resource track size** — always 20 squares. Hardcoded.
- **The Werewolf Renown block** — five renown types are hardcoded.
- **Attribute row labels** — Power, Finesse, Resistance are hardcoded.

---

## Tips

**Validate your JSON.** After any edit, paste `data.json` into [jsonlint.com](https://jsonlint.com) before pushing. A single misplaced comma will prevent the app from loading.

**Keys are permanent.** Once a character has been saved with a section key or state key, renaming it will cause saved data to be silently ignored. Always treat keys as permanent identifiers.

**Order values don't need to be consecutive.** Use gaps (10, 20, 30) to leave room for insertions.

**Same order values across splats are fine.** Sections from different splats can share order values because only one splat's sections are visible at a time.

**Power stats start at 1, morality stats start at 7.** This is controlled by `"default_value": 7` on the section definition. Do not add this to power stats — they should start at 1.

**Sharing library additions.** Use the in-app **Data library → Export library** to download your current library. Others can use **Import library → Merge** to add your entries without overwriting their own.

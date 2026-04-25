# Mortals+ Customisation Guide

Everything in Mortals+ that can be customised lives in one file: **`data.json`**. You don't need to touch any code. This guide walks through every type of change you can make, from adding a merit to building a full new splat.

> **Before you start:** Make a backup copy of `data.json` before editing. It's a plain text file — open it in any text editor (VS Code works well; Notepad++ is also fine). Do not use a word processor such as Microsoft Word, as it may introduce hidden formatting that will break the file. If the app stops loading after an edit, the JSON is likely malformed — paste the file contents into [jsonlint.com](https://jsonlint.com) to find the problem.

> **Testing changes:** Use the Live Server extension in VS Code (`Go Live` button). Do not open `index.html` directly via `file://` — the app won't load.

> **Deploying changes:** Mortals+ is a static web app hosted on GitHub Pages. After editing `data.json`, commit and push to `main`. The live app updates automatically within a minute or two.

---

## Contents

1. [Adding library content](#1-adding-library-content)
2. [Editing the default sheet layout](#2-editing-the-default-sheet-layout)
3. [Changing section visibility defaults](#3-changing-section-visibility-defaults)
4. [Renaming sections and skills](#4-renaming-sections-and-skills)
5. [Adding and removing skills](#5-adding-and-removing-skills)
6. [Adjusting the name generator](#6-adjusting-the-name-generator)
7. [Adjusting generation spreads](#7-adjusting-generation-spreads)
8. [Adding a splat-specific beats tracker](#8-adding-a-splat-specific-beats-tracker)
9. [Creating a new preset](#9-creating-a-new-preset)
10. [Adding a new splat](#10-adding-a-new-splat)
11. [Section definition reference](#11-section-definition-reference)
12. [What you cannot change in data.json](#12-what-you-cannot-change-in-datajson)

---

## 1. Adding library content

The app ships with a starter library of merits, weapons, tilts, and splat abilities. The starter library is intentionally small — it exists to show the format, not to be comprehensive. Add your own entries freely.

### Merits

Find the `"merits"` list in `data.json`. Each entry:

```json
{
  "name": "Danger Sense",
  "rating": 2,
  "desc": "The character receives a +2 bonus to reflexive Wits + Composure rolls to detect surprise or ambush."
}
```

The app sorts all content lists alphabetically on load, so order in the file doesn't matter.

### Tilts and Conditions

```json
{
  "name": "Arm Wrack",
  "desc": "The character's arm is injured. All actions using that arm suffer a -2 penalty."
}
```

### Weapons

Melee:
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

Ranged — add `ranges` (short/medium/long in yards) and `clip`:
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

All of the following use `{ name, rating, desc }`: `endowments`, `rotes`, `arcana_attainments`, `legacy_attainments`, `gifts`, `rites`, `disciplines`, `devotions`, `vampire_rites`, `variations`, `scars`, `haunts`, `influences`.

The `blessings` section (Proximi) draws from the `rotes` list — there is no separate `blessings` content list. Add Proximi Blessings to `rotes`.

```json
{
  "name": "Celerity 1: Rapid Reflexes",
  "rating": 1,
  "desc": "Cost: None. The vampire's lightning reflexes mean she never loses her Defence when attacked while taking an action, and may Dodge as a reflexive action once per turn."
}
```

For `variations` and `scars`, `rating` represents Magnitude (1–5). For `influences`, `rating` represents the entity's influence rating (0–5).

Descriptions should include enough mechanical detail that a player can use the ability at the table without opening a book — cost, dice pool, action type, duration, and any relevant conditions.

### Named ability lists (name + desc)

The following use `{ name, desc }` only: `contracts`, `pledges`, `praxes`, `tactics`, `demonic_form`, `embeds`, `exploits`, `adaptations`, `transmutations`, `bestowment`, `numina`, `manifestations`, `tells`, `yantras`, `magical_tools`, `dread_powers`.

```json
{
  "name": "Terrify",
  "desc": "The entity projects pure existential dread. Spend 1 Essence; roll Power + Finesse vs. Resolve + Composure. On success, the target flees or freezes and gains the Frightened Tilt."
}
```

### Entity bans and banes

`entity_bans` and `entity_banes` are plain string arrays used by Generate Ephemeral Entity to randomly assign a Ban and Bane. Add strings freely:

```json
"entity_bans": [
  "Cannot cross running water",
  "Must answer three questions truthfully when asked"
]
```

### Werewolf forms

The Forms table is data-driven. Each entry in `werewolf_forms` defines one column:

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

`attr_mods` lists only the attributes that change from base scores. `werewolf_forms` is one of the few lists that preserves order — columns appear in the table in the order listed.

---

## 2. Editing the default sheet layout

Every section is defined in `section_definitions`. Each entry has a `zone` and `order` that set where it appears on a fresh sheet. Dragging sections on a character sheet overrides layout per character; `section_definitions` values are the defaults for new characters.

### Zones

| Zone | Where it appears |
|---|---|
| `header` | Character header row — identity fields only |
| `beats` | Beats/XP tracker row |
| `full-width-top` | Full-width area above the two columns |
| `left-column` | Left column |
| `right-column` | Right column |
| `full-width-bottom` | Full-width area below the two columns |

### Default layout philosophy

- **Left column** — Skills (order 2) and all power/ability lists (order 3+)
- **Right column** — Other Traits (1), power stat (2), morality track (3), resource track (4), splat-specific blocks (5–19), Tilts (20), Conditions (21), Aspirations (22)
- **Full-width bottom** — Merits (10), Attainments (11–12), reference tables (13+), Weapons (20), Armor (21), Equipment (22), Notes (50)

Tilts, Conditions, and Aspirations are at orders 20–22 so all splat stats render above them by default.

### Example: move a section

Change `zone` and/or `order` on the section definition:

```json
{
  "key": "merits",
  "zone": "right-column",
  "order": 6,
  ...
}
```

---

## 3. Changing section visibility defaults

Every section has a `"default"` field: `true` = visible on new sheets, `false` = hidden until toggled on.

Changing `default` only affects new characters. Existing saved characters preserve their own visibility settings.

---

## 4. Renaming sections and skills

Change the `"label"` field on any section or skill definition. The new name appears in the config panel and on the sheet. Keys must never be changed — they are permanent identifiers used in save files.

The `athletics` skill key in particular must not be changed — it is used in the Defense formula.

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

`category` must be `"mental"`, `"physical"`, or `"social"`.

### Removing a skill

Delete the entry from `skill_definitions`. Saved character data for that skill is preserved in save files but won't appear on the sheet.

---

## 6. Adjusting the name generator

The **Generate Mortal** button picks a random name from the `mortal_names` list:

```json
"mortal_names": [
  "Adrian Cole",
  "Aisha Kamara",
  "Your Custom Name"
]
```

Entity names are stored per entity type in `entity_types`:

```json
"entity_types": {
  "spirit": {
    "label": "Spirit",
    "names": ["Ember-That-Gnaws", "Shadow Weave", "..."]
  }
}
```

---

## 7. Adjusting generation spreads

### Mortal generation

`mortal_generation` controls how dots are distributed by Generate Mortal:

```json
"mortal_generation": {
  "attribute_spreads": [5, 4, 3],
  "skill_spreads": [11, 7, 4],
  "size": 5
}
```

`attribute_spreads` — extra dots distributed across the three attribute categories (on top of the baseline 1 per attribute). `skill_spreads` — dots distributed across the three skill categories.

### Entity generation

`rank_stats` controls stat ranges for Generate Ephemeral Entity, keyed by rank number:

```json
"rank_stats": {
  "3": {
    "power": [5, 7],
    "finesse": [4, 7],
    "resistance": [3, 5],
    "essence": [30, 40],
    "integrity": [6, 8],
    "size": [5, 6],
    "numina_count": 3,
    "manifestation_count": 1
  }
}
```

Each range is `[min, max]`. `numina_count` and `manifestation_count` control how many abilities are sampled from the library.

### Entity types

Add new entity types to `entity_types`. Each entry needs a `label` and `names` list. Optionally add `size_override` to use a different size range than the rank default:

```json
"entity_types": {
  "my_entity": {
    "label": "My Entity Type",
    "size_override": [2, 3],
    "names": ["Name One", "Name Two"]
  }
}
```

---

## 8. Adding a splat-specific beats tracker

Each splat can have its own beats currency. Add a new entry to `section_definitions`:

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

Then add `"arcane-beats-xp": false` to every existing preset, and `true` in the Mage preset. No code changes required.

---

## 9. Creating a new preset

Presets replace `sectionConfig` entirely when applied. **Every section key must be listed** — missing keys do not automatically fall back to their `default` value when a preset is applied.

The easiest approach is to copy the Mortal preset as a starting point and modify it. List every section key with `true` or `false`.

Each preset must also have a `"category"` field. This controls which group the preset appears under in the Sheet Configuration dropdown. Current categories are `"Main Splats"`, `"Half-Splats"`, and `"Ephemeral Entities"`. You can introduce a new category simply by using a new string — no code changes needed.

```json
{
  "name": "Geist",
  "category": "Main Splats",
  "config": {
    "beats-xp": true,
    ...
  }
}
```

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
    "mortal-header-fields": false,
    "geist-header-fields": true,
    "synergy": true,
    "plasm": true,
    "keys": true,
    "haunts": true,
    "remembrance-traits": true,
    "entity-header": false,
    "entity-attributes": false,
    "entity-traits": false,
    "numina": false,
    "manifestations": false,
    "influences": false,
    "entity-ban": false,
    "entity-bane": false,
    "anchors": false
  }
}
```

> **Tip:** After adding a new section to `section_definitions`, you must add it to every existing preset or the app will silently ignore it when a preset is applied.

---

## 10. Adding a new splat

A new splat is a combination of new content lists, new section definitions, and a new preset. **All steps are `data.json` changes only** for standard section types. A new section type (like Demon's Cipher diagram or Werewolf's Forms table) requires additional code in `index.html`.

### Full splats

#### Step 1 — Add content lists

Add any new ability lists to `data.json`. Rated lists use `{ name, rating, desc }`. Named lists use `{ name, desc }`.

#### Step 2 — Add section definitions

Use existing types where possible. Set `group` to your splat's name and `config_category` to `"Main Splats"`. See the [Section definition reference](#11-section-definition-reference) for all available fields and types.

#### Step 3 — Update ALL existing presets

Add the new section keys to **every existing preset** set to `false`. This step is mandatory — skipping it causes silent failures when presets are applied.

#### Step 4 — Add the new preset

See [Creating a new preset](#9-creating-a-new-preset) above. List every section key. Set `"category": "Main Splats"`.

#### Step 5 — Add theme and selector options

Add a CSS theme block and `<option>` entries in both theme `<select>` elements in `index.html` (desktop sidebar and drawer). See the existing theme blocks for the pattern. Be careful not to accidentally consume the adjacent `<select>`'s opening tag when editing — this has caused bugs before.

#### Step 6 — Done

No further code changes required for standard section types.

---

### Half-splats

Half-splats layer onto the mortal sheet base rather than replacing it. They share sections from full splats where appropriate and use a separate config panel group under the *Half-Splats* category.

#### Step 1 — Add content lists (if needed)

Same as full splats. Half-splat sections that share an existing library (e.g. Blessings using Rotes) do not need a new content list — just point `db_key` at the existing one.

#### Step 2 — Add section definitions

Set `"config_category": "Half-Splats"` and `"group"` to your half-splat's name (e.g. `"Ghoul"`).

For sections shared with a full splat, add the half-splat group to `also_groups` on the **existing** section definition rather than creating a duplicate:

```json
{
  "key": "disciplines",
  "group": "Vampire",
  "also_groups": ["Ghoul"],
  ...
}
```

This renders a linked checkbox under both the Vampire and Ghoul config panel groups. Both checkboxes control the same section.

Use namespaced state keys for identity fields to avoid collisions with the parent splat (e.g. `wb_tribe` not `ww_tribe`).

#### Step 3 — Update ALL existing presets

Same as full splats. Add every new section key set to `false` in all existing presets.

#### Step 4 — Add the new preset

Same as full splats. Set `"category": "Half-Splats"`. Enable the mortal core sections, the half-splat-specific sections, and any shared sections from the parent splat.

#### Step 5 — Done

No theme required — half-splats use the Neutral theme by default. No code changes required for standard section types.

---

## 11. Section definition reference

### Fields

| Field | Required | Description |
|---|---|---|
| `key` | yes | Unique identifier. Used in `sectionConfig` and as DOM id prefix (`secblock-{key}`). Never change after release. |
| `label` | yes | Display name shown in config panel and on sheet. |
| `group` | yes | Config panel group. Currently: `Mortal`, `Hunter`, `Mage`, `Mage (Ascension)`, `Werewolf`, `Vampire`, `Changeling`, `Demon`, `Deviant`, `Promethean`, `Geist`, `Ephemeral Entity`, `Ghoul`, `Wolf-Blooded`, `Proximi`, `Horror`. |
| `config_category` | yes | Config panel category (parent of group). Currently: `Main Splats`, `Half-Splats`, `Ephemeral Entities`, `Other`. New categories can be created by using a new string. Category order in the panel follows the order sections first appear in `data.json`. |
| `also_groups` | no | Array of additional group names. Renders a linked checkbox in each listed group without duplicating the section. Used for sections shared across splats (e.g. Disciplines in both Vampire and Ghoul). |
| `zone` | yes | `header`, `beats`, `full-width-top`, `left-column`, `right-column`, `full-width-bottom` |
| `order` | yes | Default sort position within the zone. Gaps are fine (10, 20, 30). Sections from different splats can share order values. |
| `type` | yes | Section type — see table below. |
| `default` | yes | `true` = visible on new sheets. `false` = hidden until toggled. |
| `state_key` | no | STATE field name. Defaults to `key`. Use when the same state key is shared across sections. |
| `fields` | header-fields, arcana-block, renown-block, cipher-block | Array of `{ key, label }` entries defining the individual fields. |
| `max` | dot-track, dot-square-track, labeled-track, resource-track | Number of dots or squares. Default: 10 for dot tracks, 20 for resource-track. |
| `default_value` | dot-track, dot-square-track, labeled-track | Starting value for new characters. Default: `1`. Set to `7` for morality stats. Do not set on power stats — they start at 1. |
| `max_rating` | rated-list | Maximum dot rating on cards. Default: 5. |
| `db_key` | no | Names the content list in `data.json` that populates this section's dropdown. Can point to an existing list (e.g. `blessings` points to `rotes`). |
| `preserve_order` | no | If `true`, the content list named by `db_key` is not sorted alphabetically on load. Use for forms lists where column order matters. |
| `beats_key` | beats-xp | STATE key for the beats counter. Default: `beats`. |
| `xp_key` | beats-xp | STATE key for the XP counter. Default: `experience`. |
| `print_empty` | dot-track, dot-square-track, labeled-track | If `true`, all dots and squares in this section print empty (for pencil-and-paper use). Set on all morality, power stat, and stability tracks. No `index.html` change needed. |

### Section types

| Type | Description |
|---|---|
| `header-fields` | Inline-editable fields in the character header. Uses `fields` array. |
| `beats-xp` | A beats counter + XP counter pair. Uses `beats_key` and `xp_key`. Multiple trackers render side by side in the beats zone. |
| `attributes-9` | The 9 core mortal attributes. Special renderer. Note: the section key is `attributes` but the type is `attributes-9`. |
| `attributes-3` | Power / Finesse / Resistance for Ephemeral Entities. Special renderer. |
| `skills` | All skills with rote checkbox and specialty. Special renderer. |
| `derived-traits` | Mortal Health, Willpower, Defense, Initiative, Speed, Size. Special renderer. |
| `derived-traits-entity` | Entity Corpus, Willpower, Essence, and derived stats. Uses entity formulas. Special renderer. |
| `dot-track` | Clickable 1–N dot track (Integrity, Gnosis, Pilgrimage, etc.). State: single integer. |
| `dot-square-track` | Dot row (max rating) above a square row (current damage or condition). Used for Clarity and Stability. State: `sk` (integer) + `sk_squares` (array of booleans). |
| `labeled-track` | Vertical 1–N track where each level has a dot toggle and an inline-editable label. Used for Synergy. State: `sk` (integer) + `sk_labels` (string array). |
| `resource-track` | Manually-toggled squares in rows of 10. Size set by `max` (default 20). State: array of booleans. |
| `line-list` | Simple list of text lines (Aspirations, Keys, Banes, Anchors, etc.). |
| `named-list` | Collapsible cards with name + description (Tilts, Conditions, Numina, Manifestations, etc.). |
| `rated-list` | Collapsible cards with name + dot rating (0–max) + description (Merits, Disciplines, Gifts, Influences, etc.). |
| `arcana-block` | Fixed rated list driven by the section's `fields` array. Used for Mage Arcana and Supernal entity Arcana (both use the same section, key `arcana`). |
| `renown-block` | Named 5-dot tracks driven by the section's `fields` array. Werewolf only (currently). Fields: Cunning, Glory, Honor, Purity, Wisdom. |
| `forms-block` | Live reference table showing per-form calculated stats. Data-driven from `db_key`. Werewolf only. |
| `cipher-block` | Visual gear/cross diagram with Embed 1–4, Interlock 1–3, Cipher, and Final Truth fields driven by the section's `fields` array. Demon only. |
| `covers` | Structured identity cards with name, age, appearance, Cover Rating, notes, and per-cover Merits. Demon only. |
| `quinpar-wheel` | Circular 20-square track for Quintessence and Paradox. Quintessence fills clockwise from the left; Paradox counter-clockwise. Increasing Paradox decrements Quintessence to keep their sum at 20. State: `sk_quintessence` and `sk_paradox` (integers). Controls hide on print; SVG prints as-is. |
| `weapons` | Weapon cards with full weapon fields. Special renderer. |
| `armor` | Armor cards with full armor fields. Special renderer. |
| `equipment` | Equipment cards with full equipment fields. Special renderer. |
| `textarea` | Multi-line free text with markdown support. Shows rendered text by default; click to edit. |

---

## 12. What you cannot change in data.json

- **The `athletics` skill key** — used in the Defense formula. Change the label freely, but not the key.
- **Section types** — must be one of the types listed above. New types require editing `index.html`.
- **Mortal derived stat formulas** — Health, Willpower, Defense, Initiative, Speed. Code only.
- **Entity derived stat formulas** — Corpus, Willpower, Defense (rank-dependent), Initiative, Speed. Code only.
- **The 9 core mortal attributes** — fixed in the app. Not configurable via data.
- **The 3 entity attributes** — Power, Finesse, Resistance. Fixed in the app.
- **Attribute row labels** — Power, Finesse, Resistance are hardcoded.

---

## Tips

**Validate your JSON.** After any edit, paste `data.json` into [jsonlint.com](https://jsonlint.com) before pushing. A single misplaced comma prevents the app from loading.

**Keys are permanent.** Once a character has been saved with a section key or state key, renaming it causes saved data to be silently ignored. Always treat keys as permanent.

**Order values don't need to be consecutive.** Use gaps (10, 20, 30) to leave room for future insertions.

**Same order values across splats are fine.** Only one splat's sections are visible at a time.

**Power stats start at 1, morality stats at 7.** Controlled by `"default_value": 7`. Do not add this to power stats.

**Set `"print_empty": true` on all dot-track, dot-square-track, and labeled-track sections.** This makes dots and squares print empty for pencil-and-paper use at the table. All existing tracks already have this flag.

**Add new sections to every preset.** When you add a section to `section_definitions`, add it to every preset in `sheet_presets` (set to `false` in most, `true` where appropriate). Missing keys cause silent failures when presets are applied.

**Use `also_groups` for shared sections.** If a half-splat needs a section that already belongs to another splat, add the half-splat's group name to `also_groups` on the existing section definition. Do not duplicate the section definition.

**Half-splat state keys should be namespaced.** Use a short prefix unique to the half-splat (e.g. `wb_` for Wolf-Blooded, `ghoul_` for Ghoul) to avoid collisions with the parent splat's keys.

**Presets must have a `category` field.** Without it, the preset will appear under no group in the dropdown. Use an existing category string or introduce a new one — either works without code changes.

**Sharing library additions.** Use **Data library → Export library** to share your content. Others can **Import library → Merge** to add your entries without overwriting theirs.

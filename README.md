# Mortals+ — Chronicles of Darkness Character Sheet Builder

Mortals+ is a browser-based character builder and live play aid for Chronicles of Darkness (Second Edition). One of the Chronicles of Darkness core rulebooks is required to use this app.

**[Open the app →](https://bonewolf22.github.io/Mortals-A-Chronicles-of-Darkness-Character-Builder/)**

---

## Supported splats

**Main splats**
- Mortal (Chronicles of Darkness core)
- Hunter: The Vigil
- Mage: The Awakening
- Werewolf: The Forsaken
- Vampire: The Requiem
- Changeling: The Lost
- Demon: The Descent
- Deviant: The Renegades
- Promethean: The Created
- Geist: The Sin-Eaters
- Ephemeral Entities (Spirits, Ghosts, Angels, Demons, Goetia, Supernal Beings)

**Half-splats**
- Ghoul
- Wolf-Blooded
- Proximi

**Other**
- Mage: The Ascension (via the Chronicles of Darkness Translation Guide)
- Horrors (Chronicles of Darkness core / Hunter: The Vigil antagonists)

---

## Features

### Character sheet

- **Attributes** — 9 attributes across Mental, Physical, Social, displayed as clickable dots with Power/Finesse/Resistance row labels. Cycle between 5-dot, 10-dot, and 15-dot display; an overflow indicator (`+N`) appears when a value exceeds the current dot max. Show numeric values toggle also available
- **Skills** — 24 skills across Mental, Physical, Social (rated 0–5), with rote checkbox and specialty field. Skill names are editable inline
- **Health track** — typed damage (Bashing / Lethal / Aggravated); click any box to cycle; auto-resizes when Stamina or Size changes
- **Willpower** — dot and box tracker with adjustable maximum
- **Derived traits** — Defense, Initiative, Speed; auto-calculated from stats with manual override
- **Beats / Experience** — 5 Beats auto-awards 1 XP; fully data-driven (splat-specific beat trackers can be added in `data.json` without code changes)
- **Tilts and Conditions** — searchable library with custom entry support
- **Aspirations** — simple line list
- **Merits** — searchable library, 5-dot or 10-dot max toggle
- **Weapons** — melee and ranged cards with full stat fields; Equipped toggle applies Initiative modifier to derived traits
- **Armor** — cards with full stat fields; Equipped toggle applies Defense and Speed penalties
- **Equipment** — cards with dice bonus, durability, size, structure
- **Notes** — free-text field with markdown support

### Splat sections

Apply a preset from the **Sheet Configuration** panel to enable all sections for a given splat. Sections can also be toggled individually. Each splat adds its own sections on top of the standard sheet:

- **Hunter** — Compact/Conspiracy header, Endowments, Tactics, Touchstones, The Code, Group Beats tracker
- **Mage** — Path/Order/Legacy/Cabal header, Arcana block (10 Arcana rated 0–5), Gnosis, Wisdom, Mana, Obsessions, Inured Spells, Rotes, Praxes, Arcana Attainments, Legacy Attainments, Arcane Beats tracker, Nimbus, Yantras, Magical Tools
- **Werewolf** — Auspice/Tribe/Lodge/Pack header, Primal Urge, Harmony, Essence, Renown block (5 renown types), Flesh/Spirit Touchstones, live Forms reference table (calculated stats per form), Gifts, Rites
- **Vampire** — Clan/Covenant/Bloodline header, Blood Potency, Humanity, Vitae, Banes, Disciplines, Devotions, Vampire Rites
- **Changeling** — Needle/Thread/Seeming/Court/Kith header, Wyrd, Clarity (dot-square track), Glamour, Favored Regalia, Frailties, Touchstones, Goblin Debt, Contracts, Pledges, Seeming Blessing/Curse, Kith Blessing
- **Demon** — Incarnation/Agenda/Catalyst header, Cover Rating, Primum, Aether, Demonic Form, Embeds, Exploits, Cipher (interactive gear diagram), Covers (identity cards with per-cover Merits), Cover Beats tracker
- **Deviant** — Origin/Clade/Forms header, Stability (dot-square track), Acclimation, Flux, Touchstones, Variations (rated by Magnitude), Scars (rated by Magnitude), Adaptations, Origins
- **Promethean** — Elpis/Torment/Lineage/Refinement/Role header, Pilgrimage, Azoth, Pyros, Transmutations, Bestowment, Refinement Condition, Fixed Alembics, Milestones, Mastered Roles, Vitriol Beats tracker
- **Geist** — Geist/Burden/Root/Bloom/Krewe header, Synergy (labeled track with per-level labels), Plasm, Keys, Haunts, Remembrance Traits
- **Ephemeral Entity** — Type/Rank/Concept header, Power/Finesse/Resistance attributes (5/10/15-dot display), Corpus track, Willpower, Essence, derived stats, Numina, Manifestations, Influences, Ban, Bane. Ghost variant adds Anchors; Supernal variant adds Arcana

**Half-splats** layer onto the standard mortal sheet and share relevant sections with their parent splat:

- **Ghoul** — Regnant's Clan/Covenant/Family identity, Disciplines (shared with Vampire), Vitae (shared with Vampire)
- **Wolf-Blooded** — Tribe/Pack identity, Tells, Tension Pool
- **Proximi** — Dynasty/Heritage Path identity, Blessings (draws from Rotes library), Mana (shared with Mage), Curse

**Other presets** are available for non-standard play:

- **Mage (Ascension)** — Uses the Mage: the Ascension Translation Guide. Replaces Awakening sections with Ascension equivalents: Spheres (10 Arcana, same dots), Arete (Gnosis), Practices (Yantras), Foci (Magical Tools), Resonance (Nimbus), Sphere Attainments. Adds a Quintessence & Paradox wheel: a circular 20-square track where Quintessence fills clockwise and Paradox counter-clockwise; increasing Paradox decrements Quintessence automatically. Avatar Essence/Affiliation/Sect identity header. Wisdom, Praxes, Obsessions, and Inured Spells off by default.
- **Horror** — For Chronicles of Darkness antagonists. Standard mortal sheet with Horror Identity (Type, Concept), Potency (dot-track), and Dread Powers (named-list)

### Generation

- **Generate Mortal** — distributes dots using standard Chronicles of Darkness creation spreads (Attributes 5/4/3, Skills 11/7/4), picks a random name. Respects the preset currently selected in Sheet Configuration — generate a Hunter, Mage, or any other splat directly
- **Generate Ephemeral Entity** — generates a stat block scaled to the selected Rank, sampling Numina, Manifestations, Influences, Ban, and Bane from the library. Applies the Ephemeral Entity preset automatically
- **New blank sheet** — all attributes at 1, all skills at 0. Respects the selected preset

### Description formatting

Description fields support lightweight markdown:

| Syntax | Result |
|---|---|
| `**text**` | **bold** |
| `*text*` | *italic* |
| `***text***` | ***bold italic*** |

Press Enter for line breaks. Fields show rendered text by default — click to edit, click away to save. In `data.json`, use `\n` for line breaks in JSON strings.

### Layout

Section titles are drag handles — drag any section to reorder it or move it between columns. Layout is saved per character. Use **Reset layout to defaults** to restore the original positions. Use **Lock layout** to prevent accidental drags during play.

### Collapsible sections

Click any section header to collapse it. Collapsed state is preserved across page refreshes.

### Print / Save as PDF

Click **Print / Save as PDF** in the toolbar. Use your browser's **Save as PDF** destination with **Letter** paper size.

- **Chrome is the recommended browser** for printing. Firefox does not reliably handle page breaks at the attributes/columns boundary
- All collapsed sections and closed item cards are automatically expanded before printing, then restored afterwards
- Consumable tracks (health, willpower, resource tracks, dot tracks) print empty so they can be filled in pencil at the table
- Beats trackers do not print — they are live-play tools only

### Mobile and tablet

On touch devices the desktop sidebar is replaced by a floating button that opens a bottom drawer with the same Save, Configure, Sheet, and Library panels. Section drag-and-drop and textarea resize handles are touch-compatible.

---

## Saving characters

Characters are saved in your **browser's local storage** — no account or server required. This means:

- Saves are local to your device and browser
- To move a character between browsers or devices, use **Export sheet** (downloads a `.json` file) and **Import sheet** on the destination browser
- Clearing browser data will delete your saves — export important characters

---

## Contributing

Everything that can be customised lives in **`data.json`** — no code changes are needed for most additions. See [CUSTOMISATION_GUIDE.md](CUSTOMISATION_GUIDE.md) for a full walkthrough, from adding a single merit to building a complete new splat.

For code contributions, the entire frontend lives in two files with no build step or external dependencies:

- `index.html` — all HTML, CSS, and JavaScript
- `data.json` — all configuration and content

Run locally using the **Live Server** VS Code extension (`Go Live` button). Do not open `index.html` directly via `file://` — the `fetch('./data.json')` call will be blocked by browser security.

---

## License

Fan tool. Chronicles of Darkness is a trademark of Paradox Interactive AB. Not affiliated with or endorsed by Paradox Interactive. Forks are welcome provided they are not used for commercial gain.

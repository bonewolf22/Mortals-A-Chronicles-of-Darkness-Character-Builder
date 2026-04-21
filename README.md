# Mortals+ — Chronicles of Darkness Character Sheet Builder

Mortals+ is a browser-based character builder and live play aid for Chronicles of Darkness. 

**[Open the app →](https://bonewolf22.github.io/Mortals-A-Chronicles-of-Darkness-Character-Builder/)**

---

## Supported splats

- Hunter: The Vigil
- Mage: The Awakening
- Werewolf: The Forsaken
- Vampire: The Requiem
- Changeling: The Lost
- Demon: The Descent
- Deviant: The Renegades

See [Contributing](#contributing) if you'd like to help add one.

---

## Features

### Character sheet

- **Attributes** — 9 attributes across Mental, Physical, Social (rated 1–10), displayed as clickable dots with Power/Finesse/Resistance row labels. Toggle between 5-dot and 10-dot display
- **Skills** — 24 skills across Mental, Physical, Social (rated 0–5), with rote checkbox and specialty field
- **Health track** — typed damage (Bashing / Lethal / Aggravated); click any box to cycle; auto-resizes when Stamina or Size changes
- **Willpower** — circle and box tracker with adjustable maximum
- **Derived traits** — Defense, Initiative, Speed; auto-calculated with manual override; gear modifiers shown separately
- **Beats / Experience** — 5 Beats auto-awards 1 XP; fully data-driven (splat-specific beat trackers can be added without code changes)
- **Tilts and Conditions** — searchable library with custom entry support
- **Aspirations** — simple line list
- **Merits** — searchable library, 5-dot or 10-dot max toggle
- **Weapons** — melee and ranged cards with full stat fields; Equipped checkbox applies Initiative modifier to derived traits
- **Armor** — cards with full stat fields; Equipped checkbox applies Defense and Speed penalties
- **Equipment** — cards with dice bonus, durability, size, structure

### Splat sections

Apply a preset from the **Sheet Configuration** panel to enable all sections for a given splat. Sections can also be toggled individually.

Each splat adds its own sections on top of the standard sheet:

- **Hunter** — Compact/Conspiracy header, Endowments, Tactics, Touchstones, The Code
- **Mage** — Path/Order/Legacy/Cabal header, Arcana block, Gnosis, Wisdom, Mana, Obsessions, Inured Spells, Rotes, Praxes, Arcana Attainments, Legacy Attainments
- **Werewolf** — Auspice/Tribe/Lodge/Pack header, Primal Urge, Harmony, Essence, Renown block, Flesh/Spirit Touchstones, live Forms reference table, Gifts, Rites
- **Vampire** — Clan/Covenant/Bloodline header, Blood Potency, Humanity, Vitae, Banes, Disciplines, Devotions, Rites & Miracles
- **Changeling** — Needle/Thread/Seeming/Court/Kith header, Wyrd, Clarity, Glamour, Favored Regalia, Frailties, Touchstones, Goblin Debt, Contracts, Pledges, Seeming Blessing/Curse, Kith Blessing
- **Demon** — Incarnation/Agenda/Catalyst header, Cover Rating, Primum, Aether, Demonic Form, Embeds, Exploits, Cipher (interactive gear diagram with Embed/Interlock/Cipher/Final Truth fields), Covers (identity cards with Cover Rating dot track, description, and per-cover Merits)
- **Deviant** — Origin/Clade/Forms header, Stability, Acclimation, Flux, Touchstones, Variations (rated by Magnitude), Scars (rated by Magnitude), Adaptations, Origins

### Layout

Section titles are drag handles — drag any section to reorder it or move it between columns. Layout is saved per character. Use **Reset layout to defaults** to restore the original positions.

### Print / Save as PDF

Click **Print / Save as PDF** in the toolbar. Your browser's print dialog opens — choose **Save as PDF** and set paper size to **Letter**. All interactive controls are hidden; only the character sheet content prints.

This feature currently isn't perfect and is slated for improvement in a future version.

### Mobile Use
While the app is primarily intended for use on full displays or tablets, I have done my best to optimize it for mobile use. There is still room for improvement, so additional contributions for mobile use are welcome. 

---

## Saving characters

Characters are saved in your **browser's local storage** — no account or server required. This means:

- Saves are local to your device (they are not saved on any servers)
- Saves are tied to the browser you use. A character saved in Chrome on your laptop won't appear in Firefox or on your phone
- To move a character between browsers or devices, use **Export sheet** to download a JSON file, then **Import sheet** on the other browser

---

## Derived stat formulas

| Stat | Formula |
|---|---|
| Health | Stamina + Size |
| Willpower | Resolve + Composure (max 10) |
| Defense | min(Dexterity, Wits) + Athletics |
| Initiative | Dexterity + Composure |
| Speed | Strength + Dexterity + 5 |

---

## Damage types

| Symbol | Type |
|---|---|
| / | Bashing |
| X | Lethal |
| ✱ | Aggravated |

Click any health box to cycle: empty → Bashing → Lethal → Aggravated → empty.

---

## Generate baseline

Distributes dots using standard Chronicles of Darkness starting spreads:

- **Attributes** — 5 / 4 / 3 extra dots randomly distributed across the three categories
- **Skills** — 11 / 7 / 4 dots randomly distributed across the three categories

---

## Contributing

Everything that can be customised lives in **`data.json`** — no code changes are needed for most additions. See [CUSTOMISATION_GUIDE.md](CUSTOMISATION_GUIDE.md) for a full walkthrough, from adding a single merit to building a complete new splat.

For code contributions, the entire frontend lives in `index.html` as a single file with no build step or dependencies.

---

## License

Fan tool. Chronicles of Darkness is a trademark of Paradox Interactive AB. Not affiliated with or endorsed by Paradox Interactive. Any contributor is welcome to make their own fork of this app as long as they do not charge money or otherwise use it for commercial gain. 

# Documentation (Technical)

## 1. Purpose and module scope
This repository is an **offline-first, static web module** supporting the *Wrath & Glory* game. All logic runs on the client side (browser), with no backend, no build step, and no external JS/CSS libraries.

The main goal of this document is to make it possible to recreate the application **1:1** using only this file.

---

## 2. Module file structure

```text
/workspace/WnG_offline_calculator
├── index.html
├── XPCalculator.html
├── CharacterCreation.html
├── kalkulatorxp.css
├── Skull.png
├── Modal_Icon.png
├── HowToUse/
│   ├── en.pdf
│   └── pl.pdf
└── docs/
    ├── Adding_a_new_language_version.md
    ├── README.md
    └── Documentation.md
```

### File roles
- `index.html` – landing screen (navigation hub), local styles in `<style>`, links to the two modules.
- `XPCalculator.html` – XP cost calculator for attributes and skills, PL/EN i18n, reference table for species maximum attributes.
- `CharacterCreation.html` – extended character creation sheet (attributes, skills, talent costs), validations, modals, i18n, and opening instruction PDFs.
- `kalkulatorxp.css` – shared “green terminal” style used by `XPCalculator.html` and partly by `CharacterCreation.html`.
- `HowToUse/*.pdf` – instructions opened dynamically based on language.
- images (`Skull.png`, `Modal_Icon.png`) – branding and confirmation modal graphic.

---

## 3. Dependencies between files

## 3.1 Navigation dependencies
- `index.html` -> `XPCalculator.html`
- `index.html` -> `CharacterCreation.html`
- `XPCalculator.html` -> `index.html` ("Main Page" button)
- `CharacterCreation.html` -> `index.html` (back button)

## 3.2 Style dependencies
- `XPCalculator.html` loads `kalkulatorxp.css` via `<link rel="stylesheet">`.
- `CharacterCreation.html` loads `kalkulatorxp.css` + has extra local styles in `<style>` (including modals).
- `index.html` is styled only locally (no import of `kalkulatorxp.css`).

## 3.3 Asset and data dependencies
- `index.html` uses `Skull.png`.
- `CharacterCreation.html` uses `Modal_Icon.png` in the language-change confirmation modal.
- `CharacterCreation.html` opens `HowToUse/en.pdf` or `HowToUse/pl.pdf` based on `currentLanguage`.
- `XPCalculator.html` and `CharacterCreation.html` have independent but semantically aligned XP cost tables and limits.

---

## 4. Styles, layout, color palette, fonts, spacing, responsiveness

## 4.1 Visual system
The theme is consistently “retro terminal / cogitator”:
- background: dark green + radial gradients,
- containers: black panels,
- accent: neon green,
- glow: green blurred outlines and shadows.

## 4.2 Color tokens (CSS custom properties)
Shared variables (in `:root` in `kalkulatorxp.css`):
- base: `--bg`, `--panel`, `--panel2`
- text: `--text`, `--text2`, `--muted`, `--code`
- borders: `--border`, `--b`, `--b2`, `--div`
- states: `--zebra`, `--hover`
- effects: `--glow`, `--glowH`

Sample values:
- main accent: `#16c60c`
- text: `#9cf09c`
- base background: `#031605`

## 4.3 Fonts
Global monospace stack:
`"Consolas", "Fira Code", "Source Code Pro", monospace`.

## 4.4 Layout
- `XPCalculator.html`: 2-column grid layout (`.main { grid-template-columns: 360px 1fr; }`) with instruction panel and calculation area.
- `CharacterCreation.html`: single `.wrapper` container (max width ~1100px), tabular sections, and absolutely positioned language switch.
- `index.html`: centered panel with logo and buttons.

## 4.5 Spacing and rhythm
- dominant spacing: 8/10/12/14/16/24 px,
- border radius: 4/6/10 px,
- uppercase + letter-spacing for headers and buttons.

## 4.6 Responsiveness
- `kalkulatorxp.css` has a breakpoint `@media (max-width: 980px)` switching the XP calculator to a single-column layout.
- Tables use scroll containers (`overflow:auto` / `overflow-x:auto`) to prevent layout breaking on small screens.
- `index.html` uses `grid-template-columns: repeat(auto-fit, minmax(220px, 1fr))` for responsive buttons.

---

## 5. JavaScript logic – function details

## 5.1 `XPCalculator.html`

### Data and configuration
- `attributeCosts`: cumulative cost table for levels 0..12.
- `skillCosts`: cumulative cost table for levels 0..8.
- `translations`: i18n object (`pl`, `en`) containing:
  - `labels` (UI text),
  - `races` (species names),
  - `attributes` (attribute names).
- `attributeMaximumRows` + `attributeKeys`: data for the species max-attributes reference table.

### Functions and responsibilities
1. `clampValue(value, min, max)`
   - **what it does:** normalizes numeric input to int and clamps to range.
   - **where used:** `recalcTable`.
   - **why:** protects against out-of-range values and `NaN`.

2. `calculateRowCost(current, target, costs)`
   - **what it does:** returns the cost for `current -> target` as the difference of cumulative values; when `target <= current`, cost = 0.
   - **where:** `recalcTable`.
   - **why:** models the rule of paying only for level increases.

3. `recalcTable(tableId, costs, min, max)`
   - **what it does:** for each table row, calculates cost and writes it to `.cost`; returns subtotal.
   - **where:** `recalcAll`.
   - **why:** standardizes recalculation for attributes and skills.

4. `recalcAll()`
   - **what it does:** sums attributes subtotal + skills subtotal and updates `#totalXp`.
   - **where:** `input/change` events, initialization, reset.
   - **why:** central result update point.

5. `renderMaxAttributesTable(lang)`
   - **what it does:** renders the species/max-attributes reference table HTML in the selected language.
   - **where:** `applyLanguage`.
   - **why:** keeps translations and reference data consistent with active language.

6. `applyLanguage(lang)`
   - **what it does:** replaces all UI labels and headers, sets `document.lang`.
   - **where:** init + `change` on `#languageSelect`.
   - **why:** full internationalization without page reload.

### Interface mechanics
- automatic recalculation on `input` and `change` for numeric fields,
- `Reset` clears all fields and recalculates total,
- language switch works without data reset (in this module).

## 5.2 `CharacterCreation.html`

### Data and configuration
- `translations` (`pl`, `en`) contains:
  - `labels`,
  - attribute abbreviations,
  - skill labels (2 columns of 9),
  - error messages,
  - species names and max-attributes headers.
- `attributeCosts` (1..12) and `skillCosts` (0..8).
- `maxAttributeRows`, `maxAttributeKeys`.
- `TALENT_COUNT = 20`.

### Functions and responsibilities
1. `updateLanguage(lang)`
   - **what it does:** updates all UI text using dictionary data; refreshes max-attributes table if modal is open.
   - **where:** init, after confirmed language change.
   - **why:** full runtime language switch.

2. `renderSpeciesMaxTable()`
   - **what it does:** builds the max-attributes table `thead` and `tbody` via DOM API.
   - **where:** modal open, `updateLanguage` while modal is open.
   - **why:** ensures dynamic localization of headers and species names.

3. `resetAll()`
   - **what it does:** resets form to defaults (XP pool=155, attributes=1 except Speed=6, skills=0, talents empty/0).
   - **where:** after confirmed language change.
   - **why:** intentional data-integrity reset during language change.

4. `recalcXP()`
   - **what it does:**
     - validates and clamps inputs,
     - calculates attribute cost (sum of cumulative costs of final levels),
     - calculates skill cost,
     - adds manual talent costs,
     - computes `xpRemaining = xpPool - xpSpent`,
     - triggers error validations.
   - **where:** most input events, init, reset.
   - **why:** main module calculation engine.

5. `checkSkillTree()`
   - **what it does:** validates “Tree of Learning”: for each skill with `level > 1`, count of active skills (`>0`) must be >= `level`.
   - **where:** called by `recalcXP()` if XP limit is not exceeded.
   - **why:** implementation of rulebook rule shown in UI.

6. `displayError(msg)`
   - **what it does:** writes message to `#errorMessage`.
   - **why:** single error presentation point.

7. `attachDefaultOnBlur(selector, defaultValue)`
   - **what it does:** fills empty/invalid fields with default values on blur.
   - **why:** stabilizes input data and avoids `NaN`.

8. `adjustTalentFontSize(el)`
   - **what it does:** reduces font size from 16px down to min 10px until text fits in `textarea`.
   - **why:** improves readability of long talent names without breaking layout.

9. `toggleConfirmModal`, `showConfirmationModal`, `showInfoModal`
   - **what they do:** confirmation/info modal infrastructure based on Promise and event cleanup.
   - **where:** language change (confirmation), potentially other info actions.
   - **why:** asynchronous, reusable dialog mechanics.

10. `toggleSpeciesMaxModal(forceOpen)`
   - **what it does:** controls open/close of max-attributes modal and `aria-hidden`.
   - **why:** accessibility + UI control.

### Interface mechanics
- language change requires confirmation and resets data,
- ESC closes modals,
- clicking overlay closes modal,
- instruction button opens the appropriate PDF,
- numeric fields react live (`input` + `change`).

---

## 6. Calculation logic

## 6.1 Cumulative cost model
Attributes and skills are calculated with cumulative costs, e.g. in the XP calculator:

```js
rowCost = costs[target] - costs[current]
```

- **what it does:** calculates the transition cost between levels.
- **where:** `calculateRowCost()` in `XPCalculator.html`.
- **why:** safer and simpler than step-by-step summation.

In `CharacterCreation.html`, section cost is calculated as the sum of cumulative costs of entered final levels (without a base “current” value), which matches character creation from the starting level.

## 6.2 Range validations
- attributes: 1..12 (or 0..12 in XP Calculator because this module calculates a difference),
- skills: 0..8,
- talent costs: >=0,
- `xpPool`: integer >=0.

## 6.3 Rule validations
- XP pool exceeded -> error “Too much XP spent”.
- Tree of Learning rule -> error when relation between active-skill count and levels is not satisfied.

---

## 7. Data structures (in-memory)

## 7.1 i18n dictionaries
In both modules, i18n data is stored as nested objects:
- level 1: language code (`pl`, `en`),
- level 2: sections (`labels`, `errors`, `races`, etc.),
- level 3: keys used by DOM updates.

## 7.2 Cost tables
`number -> number` maps representing cumulative cost for each level.

## 7.3 Species reference table data
Arrays of objects:
```js
{ race: 'race_2', values: [12, 12, 7, 7, 8, 7, 7, 7] }
```
- **what it does:** stores max attributes per species.
- **where:** rendering reference tables in both modules.
- **why:** simplifies species-name translations and consistent rendering.

---

## 8. Supporting scripts
No standalone `.js` files and no build/test tooling. All JS is embedded inline in HTML documents.

Consequence: to recreate the module 1:1, preserve:
- function and constant declaration order,
- identical HTML element `id`s,
- identical CSS class names and selectors used by JS.

---

## 9. Procedure to recreate module 1:1 after file loss

1. **Create directory structure** exactly as in section 2.
2. **Recreate `kalkulatorxp.css`** with `:root` tokens, global rules, components (`.topbar`, `.panel`, `.dataTable`, `.btn`), and media query `max-width: 980px`.
3. **Recreate `index.html`**:
   - local `<style>` with the same theme,
   - `<main>` with `Skull.png` logo,
   - links to `XPCalculator.html` and `CharacterCreation.html`.
4. **Recreate `XPCalculator.html`**:
   - topbar + panel + workspace layout,
   - two input tables (`attributesTable`, `skillsTable`) + cost column,
   - max-attributes reference table,
   - JS: i18n, cost tables, `recalcAll`, `applyLanguage`, reset.
5. **Recreate `CharacterCreation.html`**:
   - XP section, attribute/skill/talent tables,
   - modals: `speciesMaxModal`, `confirmModal`,
   - JS: i18n, `recalcXP`, `checkSkillTree`, modal handling, reset on language change.
6. **Upload binary assets**:
   - `Skull.png`, `Modal_Icon.png`, `HowToUse/en.pdf`, `HowToUse/pl.pdf`.
7. **Manual verification**:
   - open `index.html` without a server,
   - go to both modules,
   - verify language switching, calculations, errors, PDF opening, and ESC behavior in modals.

---

## 10. Minimal 1:1 compliance checklist
- [ ] identical file names and relative paths,
- [ ] identical form-field `id`s (JS relies on hard-coded selectors),
- [ ] identical input `min/max` ranges,
- [ ] identical `attributeCosts` and `skillCosts` tables,
- [ ] identical `Tree of Learning` logic,
- [ ] identical i18n texts (PL/EN),
- [ ] identical CSS classes and breakpoints,
- [ ] PDF and image assets present in the same locations.

If all points are satisfied, the module should work and look the same as the original.

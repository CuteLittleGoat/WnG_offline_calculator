# Documentation (Technical)

## Scope
Standalone offline web app for **Wrath & Glory** character support.

## Current file structure
- `index.html` – landing page with navigation buttons.
- `XPCalculator.html` – XP cost calculator.
- `CharacterCreation.html` – character creation worksheet.
- `kalkulatorxp.css` – shared visual theme/styles.
- `HowToUse/en.pdf`, `HowToUse/pl.pdf` – manuals opened by the app.
- `Modal_Icon.png`, `Skull.png` – UI assets.
- `docs/README.md`, `docs/Documentation.md` – documentation.
- `Analizy/ANALIZA_MODYFIKACJI.md` – project analysis record.

## Architecture
- Static HTML + CSS + vanilla JavaScript.
- No backend.
- No Firebase.
- No cloud save/load.

## Language behavior
- English is default fallback for app start.
- Polish is available as second language.
- Language switch triggers confirmation and resets current form values in character creation.

## Main pages
### `index.html`
- Shows app logo and two navigation buttons:
  - `XPCalculator.html`
  - `CharacterCreation.html`

### `XPCalculator.html`
- Calculates XP cost differences between current and target values for attributes/skills.
- Uses cost tables in JavaScript.
- Recalculation is automatic on input changes.
- Reset button restores editable fields.

### `CharacterCreation.html`
- Calculates XP usage for:
  - attributes,
  - skills,
  - custom talent costs.
- Displays remaining XP and validation messages.
- Enforces Tree of Learning validation.
- Includes species maximum attributes modal.
- Opens language-specific manual PDF.

## Styling
- Shared theme defined in `kalkulatorxp.css`.
- Green-on-dark visual style with consistent buttons, tables, and panel layout.

## Offline runtime
- Open `index.html` directly in a browser.
- All core behavior runs locally in-browser.

# How to add a new language version and set it as default (EN)

This guide explains how to add another interface language to `WnG_offline_calculator` and optionally make it the default language.

The repository is offline/static. It has no Firebase integration and no save/load feature. Adding a language does not require Firebase or any backend setup.

---

## 1. Important rule

There are no separate translation files in this project.

Interface text is stored directly in HTML files, mostly inside JavaScript objects named:

```js
const translations = { ... }
```

This means adding a new language requires editing HTML files directly.

You usually need to update:

- `XPCalculator.html`,
- `CharacterCreation.html`,
- `index.html` if it contains static visible text,
- documentation files if the new language should be documented.

---

## 2. Choose a language code

Choose a short language code and use it consistently everywhere.

Examples:

| Language | Suggested code | Selector label |
| --- | --- | --- |
| German | `de` | `Deutsch` |
| French | `fr` | `Français` |
| Spanish | `es` | `Español` |
| Italian | `it` | `Italiano` |

The examples below use German (`de`). Replace `de` with your own language code if needed.

---

## 3. Update `XPCalculator.html`

### 3.1 Add a language option

Find the language selector:

```html
<select id="languageSelect" aria-label="Language version">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Add a new option:

```html
<option value="de">Deutsch</option>
```

Expected result:

```html
<select id="languageSelect" aria-label="Language version">
  <option value="en">English</option>
  <option value="pl">Polski</option>
  <option value="de">Deutsch</option>
</select>
```

### 3.2 Add a translation block

Find:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Copy the complete `en` block, paste it as a new block, and rename it:

```js
de: { ... }
```

Translate every visible value inside the new block.

### 3.3 Required `labels` keys

The new language block must include all label keys used by the page.

Typical required keys include:

```text
languageSelect
pageTitle
mainPageButton
resetButton
resetTitle
instructionsTitle
hintLine1
hintLine2
hintLine3
totalLabel
tabsTitle
attributesTitle
skillsTitle
currentHeader
targetHeader
costHeader
maxAttributesTitle
```

### 3.4 Required reference data labels

Also translate reference labels such as:

```text
races
attributes
```

Typical keys:

```text
race_1 ... race_10
attribute_1 ... attribute_8
```

### 3.5 Set default language in `XPCalculator.html`

Find:

```js
let currentLanguage = "en";
```

Change it to:

```js
let currentLanguage = "de";
```

Use this only if the new language should be the startup default.

---

## 4. Update `CharacterCreation.html`

### 4.1 Add a language option

Find the language selector:

```html
<select id="languageSelect">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Add:

```html
<option value="de">Deutsch</option>
```

### 4.2 Add a translation block

Find:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Copy the complete `en` block, paste it as a new block, rename it to `de`, and translate every visible value.

### 4.3 Translate all groups used by the sheet

Character Creation usually has more text than the XP Calculator.

Check and translate every group present in the actual file, for example:

```text
labels
errors
attributes
skills
races
species
modal texts
button labels
manual/help labels
```

The exact structure must match the current `CharacterCreation.html` implementation.

### 4.4 Set default language in `CharacterCreation.html`

Find one of these depending on the current file style:

```js
let currentLanguage = "en";
```

or:

```js
let currentLanguage = 'en';
```

Change it to:

```js
let currentLanguage = "de";
```

or:

```js
let currentLanguage = 'de';
```

Keep the quote style consistent with the file.

---

## 5. Update `index.html`

`index.html` may not use a full translation dictionary.

If it contains static visible text, update it manually:

- page title,
- button labels,
- descriptions,
- tooltips,
- alt text if needed.

If a language selector is later added to `index.html`, it must follow the same language code as the calculator pages.

---

## 6. Add a manual PDF if needed

Manual PDFs are stored in:

```text
HowToUse/
```

Current files:

```text
HowToUse/en.pdf
HowToUse/pl.pdf
```

If the new language should have its own manual, add a file such as:

```text
HowToUse/de.pdf
```

Then update the manual-opening logic in `CharacterCreation.html` so it chooses the new file for the new language.

If no translated manual is available, decide whether the new language should fall back to `en.pdf`.

---

## 7. Do not add Firebase

This repository is the offline calculator.

Adding a language does not require:

- Firebase Authentication,
- Cloud Firestore,
- Realtime Database,
- Firebase Storage,
- save/load code.

Do not add Firebase documentation or config files unless the project scope changes intentionally.

---

## 8. Final checklist

Before committing changes, verify:

- new language option exists in every required selector,
- new language block exists in every required `translations` object,
- all required keys are present,
- no labels remain accidentally in the old language,
- default language is set only where intended,
- manual PDF behavior is correct,
- `index.html` static text is updated if needed,
- app still works offline,
- no save/load or Firebase requirement was introduced accidentally.

---

## 9. Quick test

1. Open `index.html`.
2. Open `XPCalculator.html`.
3. Select the new language.
4. Confirm all labels, headers, buttons, and reference table text update.
5. Refresh and confirm the default language if you changed it.
6. Open `CharacterCreation.html`.
7. Select the new language.
8. Confirm all labels, buttons, warnings, modals, and reference table text update.
9. Click **Instruction / Manual** and confirm expected PDF behavior.
10. Disconnect network and confirm the calculator still works locally.

---

# Jak dodać nową wersję językową i ustawić ją jako domyślną (PL)

Ten poradnik wyjaśnia, jak dodać kolejny język interfejsu do `WnG_offline_calculator` i opcjonalnie ustawić go jako język domyślny.

Repozytorium jest offline/statyczne. Nie ma integracji z Firebase ani funkcji save/load. Dodanie języka nie wymaga Firebase ani żadnego backendu.

---

## 1. Ważna zasada

W tym projekcie nie ma osobnych plików tłumaczeń.

Teksty interfejsu są zapisane bezpośrednio w plikach HTML, najczęściej w obiektach JavaScript o nazwie:

```js
const translations = { ... }
```

To oznacza, że dodanie nowego języka wymaga bezpośredniej edycji plików HTML.

Zwykle trzeba zaktualizować:

- `XPCalculator.html`,
- `CharacterCreation.html`,
- `index.html`, jeśli zawiera statyczne widoczne teksty,
- pliki dokumentacji, jeśli nowy język ma być opisany.

---

## 2. Wybierz kod języka

Wybierz krótki kod języka i używaj go konsekwentnie wszędzie.

Przykłady:

| Język | Sugerowany kod | Etykieta w selektorze |
| --- | --- | --- |
| niemiecki | `de` | `Deutsch` |
| francuski | `fr` | `Français` |
| hiszpański | `es` | `Español` |
| włoski | `it` | `Italiano` |

Przykłady poniżej używają niemieckiego (`de`). Jeśli dodajesz inny język, zastąp `de` własnym kodem.

---

## 3. Zaktualizuj `XPCalculator.html`

### 3.1 Dodaj opcję języka

Znajdź selektor języka:

```html
<select id="languageSelect" aria-label="Language version">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Dodaj nową opcję:

```html
<option value="de">Deutsch</option>
```

Oczekiwany wynik:

```html
<select id="languageSelect" aria-label="Language version">
  <option value="en">English</option>
  <option value="pl">Polski</option>
  <option value="de">Deutsch</option>
</select>
```

### 3.2 Dodaj blok tłumaczeń

Znajdź:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Skopiuj cały blok `en`, wklej go jako nowy blok i zmień nazwę na:

```js
de: { ... }
```

Przetłumacz każdą widoczną wartość w nowym bloku.

### 3.3 Wymagane klucze `labels`

Nowy blok języka musi zawierać wszystkie klucze etykiet używane przez stronę.

Typowe wymagane klucze:

```text
languageSelect
pageTitle
mainPageButton
resetButton
resetTitle
instructionsTitle
hintLine1
hintLine2
hintLine3
totalLabel
tabsTitle
attributesTitle
skillsTitle
currentHeader
targetHeader
costHeader
maxAttributesTitle
```

### 3.4 Wymagane etykiety danych referencyjnych

Przetłumacz też etykiety referencyjne, takie jak:

```text
races
attributes
```

Typowe klucze:

```text
race_1 ... race_10
attribute_1 ... attribute_8
```

### 3.5 Ustaw język domyślny w `XPCalculator.html`

Znajdź:

```js
let currentLanguage = "en";
```

Zmień na:

```js
let currentLanguage = "de";
```

Zrób to tylko wtedy, gdy nowy język ma być językiem startowym.

---

## 4. Zaktualizuj `CharacterCreation.html`

### 4.1 Dodaj opcję języka

Znajdź selektor języka:

```html
<select id="languageSelect">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Dodaj:

```html
<option value="de">Deutsch</option>
```

### 4.2 Dodaj blok tłumaczeń

Znajdź:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Skopiuj cały blok `en`, wklej go jako nowy blok, zmień nazwę na `de` i przetłumacz każdą widoczną wartość.

### 4.3 Przetłumacz wszystkie grupy używane przez arkusz

Character Creation zwykle ma więcej tekstów niż XP Calculator.

Sprawdź i przetłumacz wszystkie grupy obecne w aktualnym pliku, na przykład:

```text
labels
errors
attributes
skills
races
species
teksty modali
etykiety przycisków
etykiety instrukcji/pomocy
```

Dokładna struktura musi odpowiadać aktualnej implementacji w `CharacterCreation.html`.

### 4.4 Ustaw język domyślny w `CharacterCreation.html`

Znajdź jedną z tych wersji, zależnie od stylu aktualnego pliku:

```js
let currentLanguage = "en";
```

albo:

```js
let currentLanguage = 'en';
```

Zmień na:

```js
let currentLanguage = "de";
```

albo:

```js
let currentLanguage = 'de';
```

Zachowaj styl cudzysłowów używany w pliku.

---

## 5. Zaktualizuj `index.html`

`index.html` może nie korzystać z pełnego słownika tłumaczeń.

Jeśli zawiera statyczne widoczne teksty, zaktualizuj je ręcznie:

- tytuł strony,
- etykiety przycisków,
- opisy,
- tooltipy,
- tekst alternatywny, jeśli jest potrzebny.

Jeśli w przyszłości do `index.html` zostanie dodany selektor języka, musi używać tego samego kodu języka co strony kalkulatorów.

---

## 6. Dodaj PDF instrukcji, jeśli jest potrzebny

Instrukcje PDF znajdują się w:

```text
HowToUse/
```

Aktualne pliki:

```text
HowToUse/en.pdf
HowToUse/pl.pdf
```

Jeśli nowy język ma mieć własną instrukcję, dodaj plik, na przykład:

```text
HowToUse/de.pdf
```

Następnie zaktualizuj logikę otwierania instrukcji w `CharacterCreation.html`, aby dla nowego języka wybierała nowy plik.

Jeśli nie ma przetłumaczonej instrukcji, zdecyduj, czy nowy język ma używać fallbacku do `en.pdf`.

---

## 7. Nie dodawaj Firebase

To repozytorium jest kalkulatorem offline.

Dodanie języka nie wymaga:

- Firebase Authentication,
- Cloud Firestore,
- Realtime Database,
- Firebase Storage,
- kodu save/load.

Nie dodawaj dokumentacji Firebase ani plików konfiguracyjnych Firebase, chyba że zakres projektu zmieni się świadomie.

---

## 8. Końcowa lista kontrolna

Przed commitem sprawdź:

- nowa opcja języka istnieje w każdym wymaganym selektorze,
- nowy blok języka istnieje w każdym wymaganym obiekcie `translations`,
- wszystkie wymagane klucze są obecne,
- żadna etykieta przypadkowo nie została w starym języku,
- język domyślny został ustawiony tylko tam, gdzie miał być ustawiony,
- zachowanie PDF instrukcji jest poprawne,
- statyczny tekst w `index.html` został zaktualizowany, jeśli było to potrzebne,
- aplikacja nadal działa offline,
- przypadkowo nie dodano wymogu save/load albo Firebase.

---

## 9. Szybki test

1. Otwórz `index.html`.
2. Otwórz `XPCalculator.html`.
3. Wybierz nowy język.
4. Sprawdź, czy wszystkie etykiety, nagłówki, przyciski i tekst tabel referencyjnych się zmieniły.
5. Odśwież stronę i sprawdź język domyślny, jeśli został zmieniony.
6. Otwórz `CharacterCreation.html`.
7. Wybierz nowy język.
8. Sprawdź wszystkie etykiety, przyciski, ostrzeżenia, modale i tabele referencyjne.
9. Kliknij **Instruction / Manual** i sprawdź zachowanie PDF.
10. Odłącz sieć i potwierdź, że kalkulator nadal działa lokalnie.

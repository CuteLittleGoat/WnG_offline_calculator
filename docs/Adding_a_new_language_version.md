# How to add a new language version and set it as default

This guide is written **step by step for a person without programming experience**.

You will learn how to:
1. Add a new language (for example `de` for German).
2. Fill in all required translations (all labels and other text fields).
3. Set this language as default in:
   - `XPCalculator.html`
   - `CharacterCreation.html`
   - `index.html`

---

## 1) Before you start – important rule

In this project there are no separate translation files. Text is stored directly in HTML files, inside the JavaScript section `const translations = { ... }`.

That means when adding a language you must:
- add a new option in the language selector (`<option value="...">`),
- add a new translation section in `translations`,
- set the new language code as default (`let currentLanguage = '...'`),
- and (in `index.html`) manually replace fixed texts, because there is no `translations` object there.

---

## 2) Choose language code and name

Example for German:
- language code: `de`
- name shown in selector: `Deutsch`

You can use another code (for example `fr`, `es`, `it`) — but the same code must match **everywhere**.

---

## 3) XPCalculator.html – add the new language

### Step 3.1: Add a new option in the language list

Find:

```html
<select id="languageSelect" aria-label="Language version">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Add a new line (example for `de`):

```html
<option value="de">Deutsch</option>
```

After change:

```html
<select id="languageSelect" aria-label="Language version">
  <option value="en">English</option>
  <option value="pl">Polski</option>
  <option value="de">Deutsch</option> <!-- ADD NEW LANGUAGE HERE -->
</select>
```

---

### Step 3.2: Add a new block in `translations`

Find:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Add a third block, for example `de: { ... }`.

The easiest way: copy the whole `en` block, paste it below, then change:
- `en:` to `de:`
- all texts to your target language.

#### What MUST be translated in XPCalculator (`labels`)

In the new language, fill all keys:

- `languageSelect`
- `pageTitle`
- `mainPageButton`
- `resetButton`
- `resetTitle`
- `instructionsTitle`
- `hintLine1`
- `hintLine2`
- `hintLine3`
- `totalLabel`
- `tabsTitle`
- `attributesTitle`
- `skillsTitle`
- `currentHeader`
- `targetHeader`
- `costHeader`
- `maxAttributesTitle`

#### What else MUST be translated

Besides `labels`, also fill:
- `races` (`race_1` ... `race_10`)
- `attributes` (`attribute_1` ... `attribute_8`)

Template to paste (keep the same structure):

```js
de: {
  labels: {
    languageSelect: "Language version",
    pageTitle: "XP Calculator",
    mainPageButton: "Main Page",
    resetButton: "Reset values",
    resetTitle: "Set all fields to 0",
    instructionsTitle: "INSTRUCTIONS",
    hintLine1: "▸ Enter the current and target value for each entry.",
    hintLine2: "▸ The XP total updates automatically as you change values.",
    hintLine3: "▸ The <b>Reset values</b> button clears all fields.",
    totalLabel: "Total XP cost",
    tabsTitle: "XP calculations",
    attributesTitle: "Attributes",
    skillsTitle: "Skills",
    currentHeader: "Current value",
    targetHeader: "Target value",
    costHeader: "XP cost",
    maxAttributesTitle: "Maximum attribute values"
  },
  races: {
    race_1: "...",
    race_2: "...",
    race_3: "...",
    race_4: "...",
    race_5: "...",
    race_6: "...",
    race_7: "...",
    race_8: "...",
    race_9: "...",
    race_10: "..."
  },
  attributes: {
    attribute_1: "...",
    attribute_2: "...",
    attribute_3: "...",
    attribute_4: "...",
    attribute_5: "...",
    attribute_6: "...",
    attribute_7: "...",
    attribute_8: "..."
  }
}
```

---

### Step 3.3: Set new language as default

Find:

```js
let currentLanguage = "en";
```

Change to (German example):

```js
let currentLanguage = "de"; // SET DEFAULT LANGUAGE HERE
```

That is all for `XPCalculator.html`.

---

## 4) CharacterCreation.html – add the new language

### Step 4.1: Add language option in `<select>`

Find:

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

---

### Step 4.2: Add new block in `translations`

Find:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Copy `en` and create a new `de` block.

Complete all required labels/texts used by the page in that block, then translate any additional arrays/objects if present.

---

### Step 4.3: Set default language

Find:

```js
let currentLanguage = "en";
```

Change to:

```js
let currentLanguage = "de";
```

---

## 5) index.html – add language manually

`index.html` does not use `translations`.

So you need to:
1. Find visible static texts (titles, buttons, section names, descriptions).
2. Replace them manually with your target language.
3. If there is a language selector on this page, add the new option there too.

---

## 6) Final checklist

Before saving, verify:
- New language option was added in all required `<select>` elements.
- New language block was added in each `translations` object.
- All mandatory keys are translated (no missing label).
- `currentLanguage` is set to your new language code where needed.
- `index.html` fixed texts were translated manually.

---

## 7) Quick test

1. Open the project in browser.
2. Go to `XPCalculator.html` and `CharacterCreation.html`.
3. Check if selector shows new language.
4. Check if all headers/buttons/messages are translated.
5. Refresh page and ensure new language is default.
6. Open `index.html` and verify translated static text.

If anything appears in old language, it usually means one key was skipped in `translations` or a static text was not replaced.

---

# Jak dodać nową wersję językową i ustawić ją jako domyślną

Ten poradnik jest napisany **krok po kroku dla osoby bez doświadczenia programistycznego**.

Dowiesz się, jak:
1. Dodać nowy język (np. `de` dla niemieckiego).
2. Uzupełnić wszystkie wymagane tłumaczenia (wszystkie „labels” i inne pola tekstowe).
3. Ustawić ten język jako domyślny w:
   - `XPCalculator.html`
   - `CharacterCreation.html`
   - `index.html`

---

## 1) Zanim zaczniesz – ważna zasada

W tym projekcie nie ma osobnych plików tłumaczeń. Teksty są zapisane bezpośrednio w plikach HTML, wewnątrz sekcji JavaScript `const translations = { ... }`.

To oznacza, że przy dodawaniu języka musisz:
- dodać nową opcję na liście wyboru języka (`<option value="...">`),
- dodać nową sekcję tłumaczeń w `translations`,
- ustawić nowy kod języka jako domyślny (`let currentLanguage = '...'`),
- oraz (w `index.html`) ręcznie podmienić stałe napisy, bo tam nie ma `translations`.

---

## 2) Wybierz kod i nazwę języka

Przykład dla niemieckiego:
- kod języka: `de`
- nazwa widoczna na liście: `Deutsch`

Możesz użyć innego kodu (np. `fr`, `es`, `it`) – ale ten sam kod musi się zgadzać **wszędzie**.

---

## 3) XPCalculator.html – dodanie nowego języka

### Krok 3.1: Dodaj nową opcję na liście języków

Znajdź:

```html
<select id="languageSelect" aria-label="Wersja językowa">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Dodaj nową linię (przykład dla `de`):

```html
<option value="de">Deutsch</option>
```

Po zmianie:

```html
<select id="languageSelect" aria-label="Wersja językowa">
  <option value="en">English</option>
  <option value="pl">Polski</option>
  <option value="de">Deutsch</option> <!-- TUTAJ DODAJESZ NOWY JĘZYK -->
</select>
```

---

### Krok 3.2: Dodaj nowy blok w `translations`

Znajdź:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Dodaj trzeci blok, np. `de: { ... }`.

Najprościej: skopiuj cały blok `en`, wklej pod nim i zmień:
- `en:` na `de:`
- wszystkie teksty na docelowy język.

#### Co MUSI być przetłumaczone w XPCalculator (`labels`)

W nowym języku obowiązkowo uzupełnij wszystkie klucze:

- `languageSelect`
- `pageTitle`
- `mainPageButton`
- `resetButton`
- `resetTitle`
- `instructionsTitle`
- `hintLine1`
- `hintLine2`
- `hintLine3`
- `totalLabel`
- `tabsTitle`
- `attributesTitle`
- `skillsTitle`
- `currentHeader`
- `targetHeader`
- `costHeader`
- `maxAttributesTitle`

#### Co jeszcze MUSI być przetłumaczone

Poza `labels` musisz też uzupełnić:
- `races` (`race_1` ... `race_10`)
- `attributes` (`attribute_1` ... `attribute_8`)

Szablon do wklejenia (zostaw identyczną strukturę):

```js
de: {
  labels: {
    languageSelect: "Language version",
    pageTitle: "XP Calculator",
    mainPageButton: "Main Page",
    resetButton: "Reset values",
    resetTitle: "Set all fields to 0",
    instructionsTitle: "INSTRUCTIONS",
    hintLine1: "▸ Enter the current and target value for each entry.",
    hintLine2: "▸ The XP total updates automatically as you change values.",
    hintLine3: "▸ The <b>Reset values</b> button clears all fields.",
    totalLabel: "Total XP cost",
    tabsTitle: "XP calculations",
    attributesTitle: "Attributes",
    skillsTitle: "Skills",
    currentHeader: "Current value",
    targetHeader: "Target value",
    costHeader: "XP cost",
    maxAttributesTitle: "Maximum attribute values"
  },
  races: {
    race_1: "...",
    race_2: "...",
    race_3: "...",
    race_4: "...",
    race_5: "...",
    race_6: "...",
    race_7: "...",
    race_8: "...",
    race_9: "...",
    race_10: "..."
  },
  attributes: {
    attribute_1: "...",
    attribute_2: "...",
    attribute_3: "...",
    attribute_4: "...",
    attribute_5: "...",
    attribute_6: "...",
    attribute_7: "...",
    attribute_8: "..."
  }
}
```

---

### Krok 3.3: Ustaw nowy język jako domyślny

Znajdź:

```js
let currentLanguage = "en";
```

Zmień na (przykład dla niemieckiego):

```js
let currentLanguage = "de"; // TUTAJ USTAWIASZ DOMYŚLNY JĘZYK
```

To wszystko dla `XPCalculator.html`.

---

## 4) CharacterCreation.html – dodanie nowego języka

### Krok 4.1: Dodaj opcję języka w `<select>`

Znajdź:

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

---

### Krok 4.2: Dodaj nowy blok w `translations`

Znajdź:

```js
const translations = {
  pl: { ... },
  en: { ... }
};
```

Skopiuj `en` i utwórz nowy blok `de`.

Uzupełnij wszystkie wymagane etykiety/teksty używane przez stronę w tym bloku, a następnie przetłumacz dodatkowe tablice/obiekty, jeśli występują.

---

### Krok 4.3: Ustaw domyślny język

Znajdź:

```js
let currentLanguage = "en";
```

Zmień na:

```js
let currentLanguage = "de";
```

---

## 5) index.html – dodanie języka ręcznie

`index.html` nie korzysta z `translations`.

Dlatego trzeba:
1. Znaleźć widoczne, stałe napisy (tytuły, przyciski, nazwy sekcji, opisy).
2. Ręcznie podmienić je na docelowy język.
3. Jeśli na stronie jest wybór języka, dodać tam nową opcję.

---

## 6) Lista kontrolna

Przed zapisaniem sprawdź:
- Nowa opcja językowa została dodana we wszystkich wymaganych `<select>`.
- Nowy blok języka został dodany w każdym obiekcie `translations`.
- Wszystkie obowiązkowe klucze są przetłumaczone (brakujących etykiet).
- `currentLanguage` ustawiono na nowy kod języka tam, gdzie trzeba.
- Stałe teksty w `index.html` zostały przetłumaczone ręcznie.

---

## 7) Szybki test

1. Otwórz projekt w przeglądarce.
2. Wejdź na `XPCalculator.html` i `CharacterCreation.html`.
3. Sprawdź, czy w selektorze pojawia się nowy język.
4. Sprawdź, czy wszystkie nagłówki/przyciski/komunikaty są przetłumaczone.
5. Odśwież stronę i upewnij się, że nowy język jest domyślny.
6. Otwórz `index.html` i zweryfikuj przetłumaczone stałe napisy.

Jeśli coś wyświetla się w starym języku, zwykle oznacza to pominięty klucz w `translations` albo niepodmieniony tekst stały.

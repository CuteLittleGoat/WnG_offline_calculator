# Jak dodać nową wersję językową i ustawić ją jako domyślną

Ten poradnik jest napisany **krok po kroku dla osoby bez doświadczenia programistycznego**.

Dowiesz się, jak:
1. Dodać nowy język (np. `de` dla niemieckiego).
2. Uzupełnić wszystkie wymagane tłumaczenia (wszystkie „labels” i inne pola tekstowe).
3. Ustawić ten język jako domyślny w plikach:
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

Znajdź fragment:

```html
<select id="languageSelect" aria-label="Wersja językowa">
  <option value="en">English</option>
  <option value="pl">Polski</option>
</select>
```

Dodaj nową linię (tu przykład `de`):

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

Znajdź sekcję:

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

Przykładowy szablon do wklejenia (zostaw strukturę identyczną):

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

Znajdź linię:

```js
let currentLanguage = "en";
```

Zmień na (dla niemieckiego):

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

#### Pola, które MUSISZ uzupełnić (`labels`)

To jest komplet „labeli” i tekstów UI używanych przez stronę:

- `pageTitle`
- `xpPool`
- `remainingXP`
- `attributesHeader`
- `skillsHeader`
- `talentsHeader`
- `skillsTableHeaders` (4 pozycje)
- `talentsTableHeaders` (4 pozycje)
- `footerText`
- `manualButton`
- `backToMainButton`
- `showSpeciesMaxButton`
- `speciesMaxModalTitle`
- `confirmYes`
- `confirmNo`
- `modalImageAlt`
- `infoOk`

#### Dodatkowe pola do tłumaczenia (obowiązkowe)

1. `attributes` – 8 skrótów atrybutów.
2. `races` – `race_1` ... `race_10`.
3. `maxAttributes` – `attribute_1` ... `attribute_8`.
4. `skillsColumn1` – 9 nazw umiejętności.
5. `skillsColumn2` – 9 nazw umiejętności.
6. `errors`:
   - `tooMuchXP`
   - `treeOfLearning`
   - `languageChangeWarning`

> Bardzo ważne: liczba elementów w listach (`skillsTableHeaders`, `talentsTableHeaders`, `skillsColumn1`, `skillsColumn2`) musi zostać taka sama jak w `pl` i `en`.

---

### Krok 4.3: Ustaw nowy domyślny język

Znajdź:

```js
let currentLanguage = 'en';
```

Zmień na:

```js
let currentLanguage = 'de'; // TUTAJ USTAWIASZ DOMYŚLNY JĘZYK
```

---

### Krok 4.4 (ważne): plik instrukcji PDF dla nowego języka

Przycisk „Instrukcja/Manual” działa tak:

```js
const manualPath = `HowToUse/${currentLanguage}.pdf`;
```

Czyli dla `de` aplikacja będzie szukać:

`HowToUse/de.pdf`

Jeśli tego pliku nie będzie, instrukcja się nie otworzy. Dodaj więc odpowiedni PDF do folderu `HowToUse`.

---

## 5) index.html – ustawienie domyślnego języka strony głównej

`index.html` **nie ma** mechanizmu `translations` ani przełącznika języka. To statyczna strona z dwoma przyciskami.

Aby „domyślny język” strony głównej był np. niemiecki, zmień ręcznie teksty stałe.

### Krok 5.1: Zmień język dokumentu

Na górze pliku znajdź:

```html
<html lang="en">
```

Zmień na:

```html
<html lang="de"> <!-- TUTAJ USTAWIASZ DOMYŚLNY JĘZYK DOKUMENTU -->
```

### Krok 5.2: Zmień tytuł zakładki

Znajdź:

```html
<title>Calculators</title>
```

Podmień na wersję w Twoim języku.

### Krok 5.3: Zmień teksty przycisków i opis obrazka

Znajdź te elementy i podmień tekst:

```html
<img ... alt="Logo Kozi przybornik" ...>
<a class="btn" href="XPCalculator.html">XP Calculator</a>
<a class="btn" href="CharacterCreation.html">Character Creation</a>
```

`alt` oraz nazwy przycisków ustaw w nowym języku.

---

## 6) Szybka checklista (bez programowania)

Po wprowadzeniu zmian sprawdź po kolei:

1. W `XPCalculator.html`:
   - nowy język jest na liście,
   - wszystko się tłumaczy (nagłówki, przyciski, instrukcje, tabela ras),
   - `let currentLanguage` ma kod nowego języka.

2. W `CharacterCreation.html`:
   - nowy język jest na liście,
   - tłumaczą się wszystkie sekcje, przyciski, popupy i komunikaty błędów,
   - jest plik `HowToUse/<kod>.pdf`,
   - `let currentLanguage` ma kod nowego języka.

3. W `index.html`:
   - `<html lang="...">` ma nowy kod języka,
   - teksty na stronie głównej są w nowym języku.

---

## 7) Najczęstsze błędy i jak ich uniknąć

1. **Inny kod języka w różnych miejscach**
   - Przykład błędu: w select dodasz `de`, ale w `currentLanguage` wpiszesz `ger`.
   - Rozwiązanie: używaj wszędzie dokładnie tego samego kodu.

2. **Brak jednego pola w `labels`**
   - Objaw: część tekstów się nie zmienia albo pojawia się błąd.
   - Rozwiązanie: porównaj 1:1 z blokiem `en` i upewnij się, że każdy klucz istnieje.

3. **Za mało pozycji w tablicach**
   - Dotyczy: `skillsTableHeaders`, `talentsTableHeaders`, `skillsColumn1`, `skillsColumn2`.
   - Rozwiązanie: liczba elementów musi być identyczna jak w istniejących językach.

4. **Brak pliku PDF instrukcji**
   - Objaw: przycisk Instrukcja nic nie otwiera.
   - Rozwiązanie: dodaj `HowToUse/<kod_języka>.pdf`.

---

## 8) Minimalny przepis „kopiuj-wklej”

Jeśli chcesz najprościej:
1. W obu plikach (`XPCalculator.html`, `CharacterCreation.html`) skopiuj blok `en` i wklej jako nowy blok `de`.
2. Przetłumacz wszystkie wartości tekstowe.
3. Dodaj `<option value="de">Deutsch</option>` do obu list języka.
4. Ustaw:
   - `let currentLanguage = "de";` w `XPCalculator.html`
   - `let currentLanguage = 'de';` w `CharacterCreation.html`
5. W `index.html` zmień statyczne napisy oraz `<html lang="de">`.
6. Dodaj `HowToUse/de.pdf`.

Po tych krokach nowy język będzie działał i będzie domyślny.

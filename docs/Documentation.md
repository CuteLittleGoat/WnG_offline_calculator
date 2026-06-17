# Technical documentation — WnG Offline Calculator (EN)

## 1. Purpose and scope

`WnG_offline_calculator` is an offline-first static browser module for Wrath & Glory character planning.

It contains:

- `index.html` — launcher page,
- `XPCalculator.html` — XP cost calculator,
- `CharacterCreation.html` — character creation sheet,
- `kalkulatorxp.css` — shared visual style,
- local assets and PDF manuals.

The app is similar to the `Calculators` module from `WnG_Tools`, but this repository intentionally has no Firebase integration. Therefore, it has no cloud save/load functionality.

All logic runs in the browser. There is no backend, no build step, and no required external JavaScript/CSS library.

---

## 2. File structure

```text
WnG_offline_calculator/
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
    ├── README.md
    ├── Documentation.md
    └── Adding_a_new_language_version.md
```

## 3. File responsibilities

| File | Responsibility |
| --- | --- |
| `index.html` | Landing page / navigation hub. Links to the XP Calculator and Character Creation tools. Uses local embedded styles. |
| `XPCalculator.html` | Calculates XP cost for increasing attributes and skills. Contains its own JavaScript logic and translations. |
| `CharacterCreation.html` | Character creation sheet with attributes, skills, talent costs, validation, modals, language switching, and PDF links. |
| `kalkulatorxp.css` | Shared green terminal/cogitator visual system used by calculator pages. |
| `Skull.png` | Branding/logo asset used by the landing page. |
| `Modal_Icon.png` | Modal/confirmation visual asset used by Character Creation. |
| `HowToUse/en.pdf` | English manual opened from Character Creation. |
| `HowToUse/pl.pdf` | Polish manual opened from Character Creation. |

---

## 4. Offline-only architecture

This repository must remain offline/static unless explicitly changed in the future.

It does not use:

- Firebase Authentication,
- Cloud Firestore,
- Realtime Database,
- Firebase Storage,
- service account files,
- backend API,
- Node.js runtime,
- build pipeline.

All calculations are performed in browser-side JavaScript embedded in the HTML files.

If save/load is added in the future, documentation must clearly say that the repository is no longer a purely offline calculator.

---

## 5. Navigation dependencies

Current navigation paths:

```text
index.html -> XPCalculator.html
index.html -> CharacterCreation.html
XPCalculator.html -> index.html
CharacterCreation.html -> index.html
```

When copying the app, keep relative paths intact or update the relevant links.

---

## 6. Style dependencies

- `XPCalculator.html` loads `kalkulatorxp.css`.
- `CharacterCreation.html` loads `kalkulatorxp.css` and may also contain extra local styles.
- `index.html` uses local embedded styles and does not need `kalkulatorxp.css` unless changed later.

The visual system uses a dark green terminal/cogitator style:

- dark background,
- black panels,
- neon green borders and text,
- green glow effects,
- monospace fonts,
- responsive tables and panels.

Shared font stack:

```text
"Consolas", "Fira Code", "Source Code Pro", monospace
```

---

## 7. Asset dependencies

Important asset dependencies:

| Asset | Used by | Purpose |
| --- | --- | --- |
| `Skull.png` | `index.html` | Landing page logo. |
| `Modal_Icon.png` | `CharacterCreation.html` | Confirmation/info modal icon. |
| `HowToUse/en.pdf` | `CharacterCreation.html` | English manual. |
| `HowToUse/pl.pdf` | `CharacterCreation.html` | Polish manual. |

If any asset path changes, update all references in HTML and documentation.

---

## 8. XP Calculator logic

`XPCalculator.html` contains the XP difference calculator.

Important data structures:

```text
attributeCosts
skillCosts
translations
attributeMaximumRows
attributeKeys
```

Important behavior:

- user enters current and target values,
- values are clamped to allowed ranges,
- row cost is calculated as a difference between cumulative cost values,
- subtotal and total XP update automatically,
- maximum attribute values table is rendered as a reference table,
- language switch updates labels and reference table text.

Typical row cost model:

```js
rowCost = costs[target] - costs[current]
```

When `target <= current`, cost should be `0`.

---

## 9. Character Creation logic

`CharacterCreation.html` contains the character creation sheet.

Important data structures:

```text
translations
attributeCosts
skillCosts
maxAttributeRows
maxAttributeKeys
TALENT_COUNT
```

Important behavior:

- default XP pool is `155`,
- attributes and skills are entered as final values,
- talent/manual cost rows are added to total cost,
- remaining XP is calculated as `xpPool - xpSpent`,
- input values are validated and clamped,
- Tree of Learning rule is checked,
- maximum attribute values table opens in a modal,
- manual PDF opens based on the current language,
- language change may ask for confirmation and reset current values.

---

## 10. Calculation rules

### 10.1 XP Calculator

The XP Calculator calculates transition cost:

```text
current value -> target value
```

It uses cumulative cost tables and subtracts current cost from target cost.

### 10.2 Character Creation

Character Creation calculates the cost of final values during character creation. It does not use a current-to-target transition model for the main character sheet.

### 10.3 Validation ranges

Expected ranges:

| Field | Range |
| --- | --- |
| XP Calculator attributes | 0..12 where supported by the page logic |
| Character Creation attributes | 1..12 |
| Skills | 0..8 |
| Talent/manual costs | 0 or higher |
| XP pool | 0 or higher |

### 10.4 Rule warnings

Expected warning areas:

- XP pool exceeded,
- Tree of Learning rule violation.

---

## 11. Language support

The app currently supports English and Polish.

Important implementation points:

- translations are stored inside HTML files,
- there are no separate translation JSON files,
- `XPCalculator.html` and `CharacterCreation.html` have separate translation dictionaries,
- `index.html` may contain static text rather than a full translation system,
- startup language is controlled by `currentLanguage` in each main tool page.

Default language locations:

| File | Default language variable |
| --- | --- |
| `XPCalculator.html` | `let currentLanguage = "en";` |
| `CharacterCreation.html` | `let currentLanguage = 'en';` |

Detailed language instructions:

```text
docs/Adding_a_new_language_version.md
```

---

## 12. Save/load status

There is no save/load functionality in this offline repository.

Do not document Firebase setup for this repo unless Firebase is actually added later.

Current expected behavior:

- no login,
- no Firebase config file,
- no Firestore path,
- no cloud save button,
- no cloud load button,
- no persistence guarantee after closing or refreshing the page.

Manual alternatives:

- print page,
- screenshot/export manually,
- copy values into notes,
- keep browser tab open while working.

---

## 13. Accessibility and modal behavior

Expected modal behavior:

- confirmation/info modals can be closed intentionally,
- Escape key closes modals where implemented,
- overlay click may close modals where implemented,
- `aria-hidden` or equivalent accessibility state should stay synchronized when implemented,
- focus and keyboard behavior should be tested after modal changes.

---

## 14. What to update when formulas or rules change

When XP costs or character creation rules change, update:

1. cost tables in `XPCalculator.html`,
2. cost tables in `CharacterCreation.html`,
3. validation ranges,
4. warning messages,
5. user guide,
6. this technical documentation,
7. manual PDFs if they describe the affected rule.

---

## 15. What to update when adding a new language

When adding a new language, update:

1. language selector options in each relevant HTML file,
2. `translations` in `XPCalculator.html`,
3. `translations` in `CharacterCreation.html`,
4. static text in `index.html`,
5. manual PDF links if language-specific manuals are added,
6. `docs/Adding_a_new_language_version.md`,
7. `docs/README.md`,
8. this file.

---

## 16. Control tests

| Test | Steps | Expected result |
| --- | --- | --- |
| Open launcher | Open `index.html`. | Navigation page appears. |
| Open XP Calculator | Click/open `XPCalculator.html`. | XP Calculator appears. |
| XP calculation | Enter current and target values. | Row costs and total update. |
| XP reset | Click Reset values. | Editable values reset/clear. |
| Open Character Creation | Click/open `CharacterCreation.html`. | Character sheet appears. |
| Character calculation | Enter attributes, skills, and talent costs. | Total/remaining XP updates. |
| XP pool exceeded | Enter values above the pool. | Warning appears. |
| Tree of Learning | Enter values that break the rule. | Warning appears. |
| Manual PDF | Click Instruction / Manual. | Correct language PDF opens. |
| Maximum attributes | Click Maximum attribute values. | Reference modal opens. |
| Language switch | Switch EN/PL/EN. | Labels update; reset behavior matches current design. |
| Offline use | Disconnect network and open local files. | Core calculations still work. |
| No save/load | Look for cloud save/load behavior. | No Firebase save/load is present. |

---

## 17. Rebuild checklist

To rebuild the module:

1. Restore `index.html`.
2. Restore `XPCalculator.html`.
3. Restore `CharacterCreation.html`.
4. Restore `kalkulatorxp.css`.
5. Restore `Skull.png` and `Modal_Icon.png`.
6. Restore `HowToUse/en.pdf` and `HowToUse/pl.pdf`.
7. Restore `docs/README.md`.
8. Restore `docs/Documentation.md`.
9. Restore `docs/Adding_a_new_language_version.md`.
10. Verify relative paths.
11. Run all control tests.

---

## 18. Known release notes

- This repository is offline/static.
- It has no Firebase integration.
- It has no cloud save/load functionality.
- All JavaScript is embedded in HTML files.
- The UI supports English and Polish.
- Language switching may reset current form values by design.

---

# Dokumentacja techniczna — WnG Offline Calculator (PL)

## 1. Cel i zakres

`WnG_offline_calculator` to offline’owy, statyczny moduł przeglądarkowy do planowania postaci w Wrath & Glory.

Zawiera:

- `index.html` — stronę startową,
- `XPCalculator.html` — kalkulator kosztów XP,
- `CharacterCreation.html` — arkusz tworzenia postaci,
- `kalkulatorxp.css` — wspólny styl wizualny,
- lokalne assety i instrukcje PDF.

Aplikacja jest podobna do modułu `Calculators` z repozytorium `WnG_Tools`, ale to repozytorium celowo nie ma integracji z Firebase. W związku z tym nie ma funkcjonalności zapisu/odczytu w chmurze.

Cała logika działa w przeglądarce. Nie ma backendu, procesu budowania ani wymaganej zewnętrznej biblioteki JavaScript/CSS.

---

## 2. Struktura plików

```text
WnG_offline_calculator/
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
    ├── README.md
    ├── Documentation.md
    └── Adding_a_new_language_version.md
```

## 3. Odpowiedzialność plików

| Plik | Odpowiedzialność |
| --- | --- |
| `index.html` | Strona startowa / centrum nawigacji. Linkuje do XP Calculator i Character Creation. Używa lokalnych styli osadzonych w pliku. |
| `XPCalculator.html` | Oblicza koszt XP za podnoszenie atrybutów i umiejętności. Zawiera własną logikę JavaScript oraz tłumaczenia. |
| `CharacterCreation.html` | Arkusz tworzenia postaci z atrybutami, umiejętnościami, kosztami talentów, walidacją, modalami, zmianą języka i linkami do PDF. |
| `kalkulatorxp.css` | Wspólny zielony terminalowy/cogitatorowy styl kalkulatorów. |
| `Skull.png` | Asset brandingowy/logo strony startowej. |
| `Modal_Icon.png` | Asset używany w modalach potwierdzenia/informacji. |
| `HowToUse/en.pdf` | Angielska instrukcja otwierana z Character Creation. |
| `HowToUse/pl.pdf` | Polska instrukcja otwierana z Character Creation. |

---

## 4. Architektura offline-only

To repozytorium powinno pozostać offline/statyczne, chyba że zostanie to w przyszłości świadomie zmienione.

Nie używa:

- Firebase Authentication,
- Cloud Firestore,
- Realtime Database,
- Firebase Storage,
- plików kont serwisowych,
- backend API,
- środowiska Node.js,
- procesu budowania.

Wszystkie obliczenia wykonywane są przez JavaScript osadzony w plikach HTML.

Jeśli w przyszłości zostanie dodany zapis/odczyt, dokumentacja musi jasno informować, że repozytorium nie jest już czysto offline’owym kalkulatorem.

---

## 5. Zależności nawigacyjne

Aktualne ścieżki nawigacji:

```text
index.html -> XPCalculator.html
index.html -> CharacterCreation.html
XPCalculator.html -> index.html
CharacterCreation.html -> index.html
```

Przy kopiowaniu aplikacji zachowaj ścieżki względne albo popraw odpowiednie linki.

---

## 6. Zależności stylów

- `XPCalculator.html` ładuje `kalkulatorxp.css`.
- `CharacterCreation.html` ładuje `kalkulatorxp.css` i może zawierać dodatkowe lokalne style.
- `index.html` używa lokalnych styli osadzonych i nie wymaga `kalkulatorxp.css`, chyba że zostanie to zmienione później.

System wizualny używa ciemnego zielonego stylu terminala/cogitatora:

- ciemne tło,
- czarne panele,
- neonowo zielone ramki i tekst,
- zielone efekty glow,
- fonty monospace,
- responsywne tabele i panele.

Wspólny font-stack:

```text
"Consolas", "Fira Code", "Source Code Pro", monospace
```

---

## 7. Zależności assetów

Najważniejsze zależności assetów:

| Asset | Używany przez | Cel |
| --- | --- | --- |
| `Skull.png` | `index.html` | Logo strony startowej. |
| `Modal_Icon.png` | `CharacterCreation.html` | Ikona modala potwierdzenia/informacji. |
| `HowToUse/en.pdf` | `CharacterCreation.html` | Angielska instrukcja. |
| `HowToUse/pl.pdf` | `CharacterCreation.html` | Polska instrukcja. |

Jeśli zmieni się ścieżka do assetu, popraw wszystkie odwołania w HTML i dokumentacji.

---

## 8. Logika XP Calculator

`XPCalculator.html` zawiera kalkulator różnicy kosztów XP.

Ważne struktury danych:

```text
attributeCosts
skillCosts
translations
attributeMaximumRows
attributeKeys
```

Ważne zachowanie:

- użytkownik wpisuje wartości aktualne i docelowe,
- wartości są ograniczane do dozwolonych zakresów,
- koszt wiersza jest liczony jako różnica między wartościami z tabeli kosztów skumulowanych,
- subtotal i całkowity XP aktualizują się automatycznie,
- tabela maksymalnych wartości atrybutów jest renderowana jako tabela referencyjna,
- zmiana języka aktualizuje etykiety i tekst tabeli referencyjnej.

Typowy model kosztu wiersza:

```js
rowCost = costs[target] - costs[current]
```

Gdy `target <= current`, koszt powinien wynosić `0`.

---

## 9. Logika Character Creation

`CharacterCreation.html` zawiera arkusz tworzenia postaci.

Ważne struktury danych:

```text
translations
attributeCosts
skillCosts
maxAttributeRows
maxAttributeKeys
TALENT_COUNT
```

Ważne zachowanie:

- domyślna pula XP to `155`,
- atrybuty i umiejętności są wpisywane jako wartości końcowe,
- wiersze talentów/ręcznych kosztów są dodawane do całkowitego kosztu,
- pozostały XP jest liczony jako `xpPool - xpSpent`,
- wartości wejściowe są walidowane i ograniczane,
- sprawdzana jest zasada Tree of Learning,
- tabela maksymalnych wartości atrybutów otwiera się w modalu,
- instrukcja PDF otwiera się na podstawie aktualnego języka,
- zmiana języka może prosić o potwierdzenie i resetować aktualne wartości.

---

## 10. Reguły obliczeń

### 10.1 XP Calculator

XP Calculator liczy koszt przejścia:

```text
wartość aktualna -> wartość docelowa
```

Korzysta z tabel kosztów skumulowanych i odejmuje koszt aktualny od kosztu docelowego.

### 10.2 Character Creation

Character Creation liczy koszt wartości końcowych podczas tworzenia postaci. Nie używa głównego modelu przejścia aktualna -> docelowa dla arkusza postaci.

### 10.3 Zakresy walidacji

Oczekiwane zakresy:

| Pole | Zakres |
| --- | --- |
| Atrybuty w XP Calculator | 0..12 tam, gdzie obsługuje to logika strony |
| Atrybuty w Character Creation | 1..12 |
| Umiejętności | 0..8 |
| Koszty talentów/ręczne | 0 lub więcej |
| Pula XP | 0 lub więcej |

### 10.4 Ostrzeżenia zasad

Oczekiwane obszary ostrzeżeń:

- przekroczenie puli XP,
- naruszenie zasady Tree of Learning.

---

## 11. Obsługa języków

Aplikacja obecnie obsługuje angielski i polski.

Ważne elementy implementacji:

- tłumaczenia są zapisane bezpośrednio w plikach HTML,
- nie ma osobnych plików JSON z tłumaczeniami,
- `XPCalculator.html` i `CharacterCreation.html` mają osobne słowniki tłumaczeń,
- `index.html` może zawierać tekst statyczny zamiast pełnego systemu tłumaczeń,
- język startowy jest kontrolowany przez `currentLanguage` w każdej głównej stronie narzędzia.

Lokalizacja języka domyślnego:

| Plik | Zmienna języka domyślnego |
| --- | --- |
| `XPCalculator.html` | `let currentLanguage = "en";` |
| `CharacterCreation.html` | `let currentLanguage = 'en';` |

Szczegółowa instrukcja językowa:

```text
docs/Adding_a_new_language_version.md
```

---

## 12. Status zapisu/odczytu

W tym repozytorium offline nie ma funkcjonalności save/load.

Nie dokumentuj konfiguracji Firebase dla tego repo, chyba że Firebase zostanie faktycznie dodany później.

Aktualne oczekiwane zachowanie:

- brak logowania,
- brak pliku konfiguracji Firebase,
- brak ścieżki Firestore,
- brak przycisku zapisu w chmurze,
- brak przycisku odczytu z chmury,
- brak gwarancji zachowania danych po zamknięciu albo odświeżeniu strony.

Ręczne alternatywy:

- wydruk strony,
- zrzut ekranu/ręczny eksport,
- skopiowanie wartości do notatek,
- pozostawienie otwartej karty przeglądarki podczas pracy.

---

## 13. Dostępność i zachowanie modali

Oczekiwane zachowanie modali:

- modale potwierdzenia/informacji można celowo zamknąć,
- klawisz Escape zamyka modale tam, gdzie jest to zaimplementowane,
- kliknięcie overlayu może zamykać modale tam, gdzie jest to zaimplementowane,
- `aria-hidden` albo równoważny stan dostępności powinien pozostawać zsynchronizowany tam, gdzie jest używany,
- po zmianach w modalach trzeba testować fokus i obsługę klawiatury.

---

## 14. Co aktualizować przy zmianie formuł albo zasad

Gdy zmienią się koszty XP albo zasady tworzenia postaci, zaktualizuj:

1. tabele kosztów w `XPCalculator.html`,
2. tabele kosztów w `CharacterCreation.html`,
3. zakresy walidacji,
4. komunikaty ostrzeżeń,
5. instrukcję użytkownika,
6. tę dokumentację techniczną,
7. instrukcje PDF, jeśli opisują zmienioną zasadę.

---

## 15. Co aktualizować przy dodawaniu nowego języka

Przy dodawaniu nowego języka zaktualizuj:

1. opcje wyboru języka w odpowiednich plikach HTML,
2. `translations` w `XPCalculator.html`,
3. `translations` w `CharacterCreation.html`,
4. tekst statyczny w `index.html`,
5. linki do PDF, jeśli dodawane są językowe instrukcje,
6. `docs/Adding_a_new_language_version.md`,
7. `docs/README.md`,
8. ten plik.

---

## 16. Testy kontrolne

| Test | Kroki | Oczekiwany wynik |
| --- | --- | --- |
| Otwarcie strony startowej | Otwórz `index.html`. | Pojawia się strona nawigacji. |
| Otwarcie XP Calculator | Kliknij/otwórz `XPCalculator.html`. | Pojawia się XP Calculator. |
| Obliczenie XP | Wpisz wartości aktualne i docelowe. | Koszty wierszy i suma aktualizują się. |
| Reset XP | Kliknij Reset values. | Wartości edytowalne resetują się/czyszczą. |
| Otwarcie Character Creation | Kliknij/otwórz `CharacterCreation.html`. | Pojawia się arkusz postaci. |
| Obliczenia postaci | Wpisz atrybuty, umiejętności i koszty talentów. | Suma/pozostały XP aktualizują się. |
| Przekroczenie puli XP | Wpisz wartości ponad pulę. | Pojawia się ostrzeżenie. |
| Tree of Learning | Wpisz wartości łamiące zasadę. | Pojawia się ostrzeżenie. |
| Instrukcja PDF | Kliknij Instruction / Manual. | Otwiera się PDF w odpowiednim języku. |
| Maksymalne atrybuty | Kliknij Maximum attribute values. | Otwiera się modal referencyjny. |
| Zmiana języka | Przełącz EN/PL/EN. | Etykiety się aktualizują; reset odpowiada aktualnemu projektowi. |
| Tryb offline | Odłącz sieć i otwórz lokalne pliki. | Podstawowe obliczenia nadal działają. |
| Brak save/load | Sprawdź zachowanie zapisu w chmurze. | Brak Firebase save/load. |

---

## 17. Lista odbudowy

Aby odbudować moduł:

1. Przywróć `index.html`.
2. Przywróć `XPCalculator.html`.
3. Przywróć `CharacterCreation.html`.
4. Przywróć `kalkulatorxp.css`.
5. Przywróć `Skull.png` i `Modal_Icon.png`.
6. Przywróć `HowToUse/en.pdf` i `HowToUse/pl.pdf`.
7. Przywróć `docs/README.md`.
8. Przywróć `docs/Documentation.md`.
9. Przywróć `docs/Adding_a_new_language_version.md`.
10. Sprawdź ścieżki względne.
11. Uruchom wszystkie testy kontrolne.

---

## 18. Znane uwagi wydania

- To repozytorium jest offline/statyczne.
- Nie ma integracji z Firebase.
- Nie ma zapisu/odczytu w chmurze.
- Cały JavaScript jest osadzony w plikach HTML.
- Interfejs obsługuje angielski i polski.
- Zmiana języka może celowo resetować aktualne wartości formularza.

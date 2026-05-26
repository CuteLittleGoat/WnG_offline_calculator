# Documentation (Technical)

## 1. Cel i zakres modułu
Ten repozytorium to **offline-first, statyczny moduł webowy** wspierający grę *Wrath & Glory*. Cała logika działa po stronie klienta (przeglądarka), bez backendu, bez build-stepu i bez zewnętrznych bibliotek JS/CSS.

Główny cel dokumentu: umożliwić odtworzenie aplikacji **1:1** wyłącznie na podstawie tego pliku.

---

## 2. Struktura plików modułu

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
    ├── README.md
    └── Documentation.md
```

### Rola plików
- `index.html` – ekran wejściowy (hub nawigacyjny), lokalne style w `<style>`, linki do dwóch modułów.
- `XPCalculator.html` – kalkulator kosztu XP dla atrybutów i umiejętności, i18n PL/EN, tabela referencyjna maksymalnych atrybutów ras.
- `CharacterCreation.html` – rozbudowany arkusz tworzenia postaci (atrybuty, umiejętności, koszty talentów), walidacje, modale, i18n, otwieranie PDF instrukcji.
- `kalkulatorxp.css` – wspólny styl „green terminal” używany przez `XPCalculator.html` i częściowo przez `CharacterCreation.html`.
- `HowToUse/*.pdf` – instrukcje uruchamiane dynamicznie wg języka.
- obrazy (`Skull.png`, `Modal_Icon.png`) – branding oraz grafika w modalu potwierdzenia.

---

## 3. Zależności między plikami

## 3.1 Zależności nawigacyjne
- `index.html` -> `XPCalculator.html`
- `index.html` -> `CharacterCreation.html`
- `XPCalculator.html` -> `index.html` (przycisk „Main Page / Strona Główna”)
- `CharacterCreation.html` -> `index.html` (przycisk powrotu)

## 3.2 Zależności stylów
- `XPCalculator.html` ładuje `kalkulatorxp.css` przez `<link rel="stylesheet">`.
- `CharacterCreation.html` ładuje `kalkulatorxp.css` + ma dodatkowe style lokalne w `<style>` (m.in. modale).
- `index.html` jest stylowany tylko lokalnie (brak importu `kalkulatorxp.css`).

## 3.3 Zależności assetów i danych
- `index.html` używa `Skull.png`.
- `CharacterCreation.html` używa `Modal_Icon.png` w modalu potwierdzenia zmiany języka.
- `CharacterCreation.html` otwiera `HowToUse/en.pdf` lub `HowToUse/pl.pdf` na podstawie `currentLanguage`.
- `XPCalculator.html` i `CharacterCreation.html` mają niezależne, ale semantycznie zgodne tabele kosztów XP i limity.

---

## 4. Style, layout, kolorystyka, fonty, spacing, responsywność

## 4.1 System wizualny
Motyw jest konsekwentnie „retro terminal / cogitator”:
- tło: ciemna zieleń + radialne gradienty,
- kontenery: czarne panele,
- akcent: neonowa zieleń,
- glow: zielone rozmyte obwódki i cienie.

## 4.2 Tokeny kolorów (CSS custom properties)
Wspólne zmienne (w `:root` w `kalkulatorxp.css`):
- bazowe: `--bg`, `--panel`, `--panel2`
- tekst: `--text`, `--text2`, `--muted`, `--code`
- obramowania: `--border`, `--b`, `--b2`, `--div`
- stany: `--zebra`, `--hover`
- efekty: `--glow`, `--glowH`

Przykładowe wartości:
- główny akcent: `#16c60c`
- tekst: `#9cf09c`
- tło bazowe: `#031605`

## 4.3 Fonty
Globalnie ustawiony monospace stack:
`"Consolas", "Fira Code", "Source Code Pro", monospace`.

## 4.4 Layout
- `XPCalculator.html`: layout grid 2-kolumnowy (`.main { grid-template-columns: 360px 1fr; }`) z panelem instrukcji i przestrzenią kalkulacyjną.
- `CharacterCreation.html`: pojedynczy kontener `.wrapper` (max szerokość ~1100px), sekcje tabelaryczne i absolutnie pozycjonowany przełącznik języka.
- `index.html`: centralnie wyśrodkowany panel z logo i przyciskami.

## 4.5 Spacing i rytm
- dominują odstępy 8/10/12/14/16/24 px,
- promienie zaokrągleń 4/6/10 px,
- uppercase + letter-spacing dla nagłówków i przycisków.

## 4.6 Responsywność
- `kalkulatorxp.css` ma breakpoint `@media (max-width: 980px)` przełączający kalkulator XP na układ jednokolumnowy.
- Tabele mają kontenery z przewijaniem (`overflow:auto` / `overflow-x:auto`), co zapobiega łamaniu layoutu na małych ekranach.
- `index.html` używa `grid-template-columns: repeat(auto-fit, minmax(220px, 1fr))` dla responsywnych przycisków.

---

## 5. Logika JavaScript – szczegóły funkcji

## 5.1 `XPCalculator.html`

### Dane i konfiguracja
- `attributeCosts`: tabela kosztów skumulowanych poziomów 0..12.
- `skillCosts`: tabela kosztów skumulowanych poziomów 0..8.
- `translations`: obiekt i18n (`pl`, `en`) zawierający:
  - `labels` (teksty UI),
  - `races` (nazwy ras),
  - `attributes` (nazwy atrybutów).
- `attributeMaximumRows` + `attributeKeys`: dane do tabeli referencyjnej max atrybutów ras.

### Funkcje i ich odpowiedzialność
1. `clampValue(value, min, max)`
   - **co robi:** normalizuje wejście numeryczne do int i ogranicza do zakresu.
   - **gdzie używana:** `recalcTable`.
   - **dlaczego:** zabezpiecza przed wartościami spoza zakresu i `NaN`.

2. `calculateRowCost(current, target, costs)`
   - **co robi:** zwraca koszt przejścia `current -> target` jako różnicę wartości skumulowanych; gdy `target <= current`, koszt = 0.
   - **gdzie:** `recalcTable`.
   - **dlaczego:** modeluje regułę płacenia wyłącznie za podniesienie poziomu.

3. `recalcTable(tableId, costs, min, max)`
   - **co robi:** dla każdego wiersza tabeli liczy koszt i wpisuje go do komórki `.cost`, zwraca subtotal.
   - **gdzie:** `recalcAll`.
   - **dlaczego:** ujednolica przeliczanie dla atrybutów i umiejętności.

4. `recalcAll()`
   - **co robi:** sumuje subtotal atrybutów + umiejętności i aktualizuje `#totalXp`.
   - **gdzie:** eventy `input/change`, inicjalizacja, reset.
   - **dlaczego:** centralny punkt aktualizacji wyniku.

5. `renderMaxAttributesTable(lang)`
   - **co robi:** renderuje HTML tabeli referencyjnej ras i max atrybutów w wybranym języku.
   - **gdzie:** `applyLanguage`.
   - **dlaczego:** utrzymuje tłumaczenia i dane referencyjne spójne z aktywnym językiem.

6. `applyLanguage(lang)`
   - **co robi:** podmienia wszystkie etykiety i nagłówki UI, ustawia `document.lang`.
   - **gdzie:** init + `change` na `#languageSelect`.
   - **dlaczego:** pełna internacjonalizacja bez przeładowania strony.

### Mechanika interfejsu
- automatyczne przeliczenie na `input` i `change` dla pól liczbowych,
- `Reset` zeruje wszystkie pola i przelicza total,
- przełącznik języka działa bez resetu danych (w tym module).

## 5.2 `CharacterCreation.html`

### Dane i konfiguracja
- `translations` (`pl`, `en`) zawiera:
  - `labels`,
  - skróty atrybutów,
  - etykiety umiejętności (2 kolumny po 9),
  - komunikaty błędów,
  - nazwy ras i nagłówki max atrybutów.
- `attributeCosts` (1..12) i `skillCosts` (0..8).
- `maxAttributeRows`, `maxAttributeKeys`.
- `TALENT_COUNT = 20`.

### Funkcje i ich odpowiedzialność
1. `updateLanguage(lang)`
   - **co robi:** aktualizuje wszystkie teksty UI wg słownika; odświeża tabelę max atrybutów jeśli modal jest otwarty.
   - **gdzie:** init, po potwierdzonej zmianie języka.
   - **dlaczego:** pełna zmiana języka w runtime.

2. `renderSpeciesMaxTable()`
   - **co robi:** buduje `thead` i `tbody` tabeli max atrybutów przez API DOM.
   - **gdzie:** otwarcie modala, `updateLanguage` przy otwartym modalu.
   - **dlaczego:** zapewnia dynamiczną lokalizację nagłówków i nazw ras.

3. `resetAll()`
   - **co robi:** resetuje formularz do wartości domyślnych (XP pool=155, atrybuty=1 poza Speed=6, skille=0, talenty puste/0).
   - **gdzie:** po potwierdzonej zmianie języka.
   - **dlaczego:** celowy reset integralności danych przy zmianie języka.

4. `recalcXP()`
   - **co robi:**
     - waliduje i klamruje wartości wejściowe,
     - liczy koszt atrybutów (suma kosztów skumulowanych poziomów),
     - liczy koszt umiejętności,
     - dodaje ręczne koszty talentów,
     - wylicza `xpRemaining = xpPool - xpSpent`,
     - uruchamia walidacje błędów.
   - **gdzie:** większość eventów wejściowych, init, reset.
   - **dlaczego:** główny silnik obliczeniowy modułu.

5. `checkSkillTree()`
   - **co robi:** waliduje zasadę „Tree of Learning”: dla każdego skilla `level > 1` liczba aktywnych skilli (`>0`) musi być >= `level`.
   - **gdzie:** wywoływana przez `recalcXP()` jeśli XP nieprzekroczone.
   - **dlaczego:** implementacja reguły podręcznikowej wskazanej w UI.

6. `displayError(msg)`
   - **co robi:** wpisuje komunikat do `#errorMessage`.
   - **dlaczego:** pojedynczy punkt prezentacji błędów.

7. `attachDefaultOnBlur(selector, defaultValue)`
   - **co robi:** uzupełnia puste/niepoprawne pola wartościami domyślnymi po utracie fokusu.
   - **dlaczego:** stabilizuje dane wejściowe i unika `NaN`.

8. `adjustTalentFontSize(el)`
   - **co robi:** zmniejsza font od 16px do min 10px, aż tekst mieści się w `textarea`.
   - **dlaczego:** poprawa czytelności długich nazw talentów bez rozbijania layoutu.

9. `toggleConfirmModal`, `showConfirmationModal`, `showInfoModal`
   - **co robią:** infrastruktura modala potwierdzeń/informacji oparta o Promise i event cleanup.
   - **gdzie:** zmiana języka (potwierdzenie), potencjalnie inne akcje informacyjne.
   - **dlaczego:** asynchroniczna, reużywalna mechanika dialogów.

10. `toggleSpeciesMaxModal(forceOpen)`
   - **co robi:** steruje otwieraniem/zamykaniem modala max atrybutów i aria-hidden.
   - **dlaczego:** dostępność + kontrola UI.

### Mechanika interfejsu
- zmiana języka wymaga potwierdzenia i resetuje dane,
- ESC zamyka modale,
- kliknięcie overlay zamyka modal,
- przycisk instrukcji otwiera właściwy PDF,
- pola liczbowe reagują live (`input` + `change`).

---

## 6. Logika obliczeń

## 6.1 Model kosztów skumulowanych
Atrybuty i umiejętności są liczone przez koszty skumulowane, np. w XP kalkulatorze:

```js
rowCost = costs[target] - costs[current]
```

- **co robi:** wylicza koszt przejścia pomiędzy poziomami.
- **gdzie:** `calculateRowCost()` w `XPCalculator.html`.
- **dlaczego:** to bezpieczniejszy i prostszy model niż sumowanie krok-po-kroku.

W `CharacterCreation.html` koszt dla sekcji jest liczony jako suma kosztów końcowych wpisanych poziomów (bez wartości bazowej „current”), co odpowiada procesowi tworzenia postaci od poziomu startowego.

## 6.2 Walidacje zakresów
- atrybuty: 1..12 (lub 0..12 w XP Calculatorze, bo ten moduł liczy różnicę),
- umiejętności: 0..8,
- koszty talentów: >=0,
- `xpPool`: liczba całkowita >=0.

## 6.3 Walidacje regułowe
- przekroczenie puli XP -> błąd „Too much XP spent / Przekroczono pulę PD”.
- reguła Tree of Learning -> błąd przy niespełnieniu relacji liczby aktywnych skilli do poziomów.

---

## 7. Struktura danych (in-memory)

## 7.1 Słowniki i18n
W obu modułach i18n dane są trzymane jako zagnieżdżone obiekty:
- poziom 1: kod języka (`pl`, `en`),
- poziom 2: sekcje (`labels`, `errors`, `races`, itd.),
- poziom 3: klucze używane przez DOM update.

## 7.2 Tabele kosztów
Mapy `number -> number` reprezentujące koszt skumulowany dla poziomu.

## 7.3 Dane tabel referencyjnych ras
Tablice obiektów:
```js
{ race: 'race_2', values: [12, 12, 7, 7, 8, 7, 7, 7] }
```
- **co robi:** trzyma max atrybutów dla rasy.
- **gdzie:** render tabel referencyjnych w obu modułach.
- **dlaczego:** ułatwia tłumaczenie nazw ras i jednolite renderowanie.

---

## 8. Skrypty pomocnicze
Brak osobnych plików `.js` i brak narzędzi build/test. Cały JS jest osadzony inline w dokumentach HTML.

Konsekwencja: odtwarzając moduł 1:1 należy zachować:
- kolejność deklaracji funkcji i stałych,
- identyczne `id` elementów HTML,
- identyczne nazwy klas CSS i selektory używane przez JS.

---

## 9. Procedura odtworzenia modułu 1:1 po utracie plików

1. **Utwórz strukturę katalogów** dokładnie jak w sekcji 2.
2. **Odtwórz `kalkulatorxp.css`** z tokenami `:root`, globalnymi regułami, komponentami (`.topbar`, `.panel`, `.dataTable`, `.btn`) i media query `max-width: 980px`.
3. **Odtwórz `index.html`**:
   - lokalny `<style>` z tym samym motywem,
   - `<main>` z logo `Skull.png`,
   - linki do `XPCalculator.html` i `CharacterCreation.html`.
4. **Odtwórz `XPCalculator.html`**:
   - układ topbar + panel + workspace,
   - dwie tabele wejściowe (`attributesTable`, `skillsTable`) + kolumna kosztu,
   - referencyjna tabela max atrybutów,
   - JS: i18n, tabele kosztów, `recalcAll`, `applyLanguage`, reset.
5. **Odtwórz `CharacterCreation.html`**:
   - sekcja XP, tabele atrybutów, umiejętności, talentów,
   - modale: `speciesMaxModal`, `confirmModal`,
   - JS: i18n, `recalcXP`, `checkSkillTree`, obsługa modali, reset przy zmianie języka.
6. **Wgraj assets binarne**:
   - `Skull.png`, `Modal_Icon.png`, `HowToUse/en.pdf`, `HowToUse/pl.pdf`.
7. **Weryfikacja manualna**:
   - otwórz `index.html` bez serwera,
   - przejdź do obu modułów,
   - sprawdź przełączanie języka, przeliczenia, błędy, otwieranie PDF, działanie ESC w modalach.

---

## 10. Minimalna lista kontrolna zgodności 1:1
- [ ] identyczne nazwy plików i ścieżki względne,
- [ ] identyczne `id` pól formularza (JS opiera się na twardych selektorach),
- [ ] identyczne zakresy `min/max` inputów,
- [ ] identyczne tabele kosztów `attributeCosts` i `skillCosts`,
- [ ] identyczna logika `Tree of Learning`,
- [ ] identyczne teksty i18n (PL/EN),
- [ ] identyczne klasy CSS i breakpointy,
- [ ] obecność PDF i obrazów w tych samych lokalizacjach.

Jeżeli wszystkie punkty są spełnione, moduł powinien działać i wyglądać tak samo jak oryginał.

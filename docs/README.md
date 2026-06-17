# User Guide — WnG Offline Calculator (EN)

## What this app is

`WnG_offline_calculator` is an offline/static browser calculator for Wrath & Glory character planning.

It contains two main tools:

- **XP Calculator** — `XPCalculator.html`
- **Character Creation** — `CharacterCreation.html`

The app is similar to the `Calculators` module from `WnG_Tools`, but this repository is the offline version. It does not include Firebase integration and therefore does not include cloud save/load functionality.

## How to start

1. Open `index.html` in your browser.
2. Choose one of the available tools:
   - **XP Calculator**
   - **Character Creation**

The app can be opened from local files. A web server is not required for the basic calculator workflow.

## XP Calculator — `XPCalculator.html`

Use the XP Calculator to calculate the XP cost of increasing attributes and skills.

Typical workflow:

1. Open `XPCalculator.html`.
2. Choose the interface language from the selector.
3. Enter current and target values for attributes and skills.
4. The app calculates row costs and total XP cost automatically.
5. Use **Reset values** to clear editable fields.
6. Use **Main Page** to return to `index.html`.

Important behavior:

- the calculator works entirely in the browser,
- values are recalculated immediately after edits,
- attribute and skill costs use embedded cost tables,
- the maximum attribute values table is a reference table only.

## Character Creation — `CharacterCreation.html`

Use Character Creation to build a character with a point pool.

Typical workflow:

1. Open `CharacterCreation.html`.
2. Set the XP pool. The default is `155`.
3. Fill attributes.
4. Fill skills.
5. Add optional talent costs and other manual costs supported by the page.
6. Watch validation messages, especially:
   - point pool exceeded,
   - Tree of Learning rule.
7. Use **Instruction / Manual** to open the language PDF.
8. Use **Maximum attribute values** to open species limits.
9. Use **Reset** to restore defaults.

Character Creation does not save data to Firebase or any cloud service. If you close or refresh the page, unsaved form state may be lost.

## Offline behavior

The app is designed to run without Firebase.

It does not require:

- login,
- Firebase Authentication,
- Cloud Firestore,
- Realtime Database,
- Firebase Storage,
- backend server,
- build step.

All calculations are local in the browser.

## Save/load behavior

This repository does not include save/load functionality.

There is no Firebase connection and no cloud persistence. To keep a character record, use one of these manual methods:

- print the page,
- export/screenshot the result manually,
- copy values to your own notes,
- keep the browser tab open during work.

If cloud save/load is needed, use or adapt the Firebase-enabled calculator module from `WnG_Tools` instead.

## Manuals and reference files

Manual PDFs are stored in:

```text
HowToUse/
```

Current files:

```text
HowToUse/en.pdf
HowToUse/pl.pdf
```

The **Instruction / Manual** button opens the PDF matching the current language, where supported by the page logic.

## Language behavior

The app supports English and Polish UI text.

Current default language is controlled inside the HTML files:

| File | Default language variable |
| --- | --- |
| `XPCalculator.html` | `let currentLanguage = "en";` |
| `CharacterCreation.html` | `let currentLanguage = 'en';` |

Changing language during use may reset current form values by design. Choose the language before entering important data.

Detailed language-extension instructions are in:

```text
docs/Adding_a_new_language_version.md
```

## Important when copying the app

When copying the app to another folder or host:

1. Keep the same file names.
2. Keep the same relative paths.
3. Keep `HowToUse/en.pdf` and `HowToUse/pl.pdf` if the manual buttons should work.
4. Keep `Skull.png` and `Modal_Icon.png` if the current UI uses them.
5. Check **Main Page** links in both tools.
6. Test both tools after copying.

## Common problems

| Symptom | Possible cause | Fix |
| --- | --- | --- |
| Manual does not open. | PDF file is missing or path changed. | Verify `HowToUse/en.pdf` and `HowToUse/pl.pdf`. |
| Main Page link does not work. | App was moved and relative paths changed. | Update the link target in the relevant HTML file. |
| Form values disappeared after language change. | Language switch resets values by design. | Choose language before filling the form. |
| No save/load buttons are available. | This is the offline version. | Use manual export/notes or the Firebase-enabled module from `WnG_Tools`. |
| Calculator gives unexpected cost. | Current/target values or embedded cost table may not match the intended rules. | Check entered values and verify the cost tables in the HTML. |

---

# Instrukcja użytkownika — WnG Offline Calculator (PL)

## Czym jest aplikacja

`WnG_offline_calculator` to offline’owy/statyczny kalkulator przeglądarkowy do planowania postaci w Wrath & Glory.

Zawiera dwa główne narzędzia:

- **XP Calculator** — `XPCalculator.html`
- **Character Creation** — `CharacterCreation.html`

Aplikacja jest podobna do modułu `Calculators` z repozytorium `WnG_Tools`, ale to jest wersja offline. Nie zawiera integracji z Firebase, więc nie ma funkcjonalności zapisu/odczytu w chmurze.

## Jak uruchomić

1. Otwórz `index.html` w przeglądarce.
2. Wybierz jedno z narzędzi:
   - **XP Calculator**
   - **Character Creation**

Aplikację można uruchamiać z lokalnych plików. Do podstawowego działania kalkulatora nie jest potrzebny serwer WWW.

## XP Calculator — `XPCalculator.html`

XP Calculator służy do liczenia kosztu XP za podnoszenie atrybutów i umiejętności.

Typowy przebieg pracy:

1. Otwórz `XPCalculator.html`.
2. Wybierz język interfejsu z listy.
3. Wpisz aktualne i docelowe wartości atrybutów oraz umiejętności.
4. Aplikacja automatycznie obliczy koszty wierszy i całkowity koszt XP.
5. Użyj **Reset values**, aby wyczyścić pola edytowalne.
6. Użyj **Main Page**, aby wrócić do `index.html`.

Ważne zachowanie:

- kalkulator działa całkowicie w przeglądarce,
- wartości przeliczają się natychmiast po zmianach,
- koszty atrybutów i umiejętności korzystają z tabel zaszytych w HTML,
- tabela maksymalnych wartości atrybutów jest tylko tabelą referencyjną.

## Character Creation — `CharacterCreation.html`

Character Creation służy do tworzenia postaci z określoną pulą punktów.

Typowy przebieg pracy:

1. Otwórz `CharacterCreation.html`.
2. Ustaw pulę XP. Domyślnie jest to `155`.
3. Uzupełnij atrybuty.
4. Uzupełnij umiejętności.
5. Dodaj opcjonalne koszty talentów i inne ręczne koszty obsługiwane przez stronę.
6. Sprawdzaj komunikaty walidacji, szczególnie:
   - przekroczenie puli punktów,
   - zasadę Tree of Learning.
7. Użyj **Instruction / Manual**, aby otworzyć PDF w odpowiednim języku.
8. Użyj **Maximum attribute values**, aby otworzyć limity gatunków/ras.
9. Użyj **Reset**, aby przywrócić wartości domyślne.

Character Creation nie zapisuje danych do Firebase ani żadnej chmury. Po zamknięciu albo odświeżeniu strony niezapisany stan formularza może zostać utracony.

## Zachowanie offline

Aplikacja została zaprojektowana do działania bez Firebase.

Nie wymaga:

- logowania,
- Firebase Authentication,
- Cloud Firestore,
- Realtime Database,
- Firebase Storage,
- backendu,
- procesu budowania.

Wszystkie obliczenia wykonywane są lokalnie w przeglądarce.

## Zapis/odczyt

To repozytorium nie zawiera funkcjonalności save/load.

Nie ma połączenia z Firebase ani zapisu w chmurze. Aby zachować kartę postaci, użyj jednej z metod ręcznych:

- wydrukuj stronę,
- zrób zrzut ekranu albo ręczny eksport,
- skopiuj wartości do własnych notatek,
- pozostaw kartę przeglądarki otwartą podczas pracy.

Jeśli potrzebny jest zapis/odczyt w chmurze, użyj albo zaadaptuj moduł kalkulatora z integracją Firebase z repozytorium `WnG_Tools`.

## Instrukcje i pliki referencyjne

Instrukcje PDF znajdują się w:

```text
HowToUse/
```

Aktualne pliki:

```text
HowToUse/en.pdf
HowToUse/pl.pdf
```

Przycisk **Instruction / Manual** otwiera PDF odpowiadający aktualnemu językowi, tam gdzie obsługuje to logika strony.

## Zachowanie języka

Aplikacja obsługuje angielski i polski interfejs.

Aktualny język domyślny jest ustawiany bezpośrednio w plikach HTML:

| Plik | Zmienna języka domyślnego |
| --- | --- |
| `XPCalculator.html` | `let currentLanguage = "en";` |
| `CharacterCreation.html` | `let currentLanguage = 'en';` |

Zmiana języka w trakcie pracy może celowo resetować aktualne wartości formularza. Wybierz język przed wpisywaniem ważnych danych.

Szczegółowa instrukcja dodawania języków znajduje się w:

```text
docs/Adding_a_new_language_version.md
```

## Ważne przy kopiowaniu aplikacji

Przy kopiowaniu aplikacji do innego folderu albo hostingu:

1. Zachowaj te same nazwy plików.
2. Zachowaj te same ścieżki względne.
3. Zachowaj `HowToUse/en.pdf` i `HowToUse/pl.pdf`, jeśli przyciski instrukcji mają działać.
4. Zachowaj `Skull.png` i `Modal_Icon.png`, jeśli używa ich aktualny interfejs.
5. Sprawdź linki **Main Page** w obu narzędziach.
6. Przetestuj oba narzędzia po skopiowaniu.

## Typowe problemy

| Objaw | Możliwa przyczyna | Rozwiązanie |
| --- | --- | --- |
| Instrukcja PDF się nie otwiera. | Brakuje pliku PDF albo zmieniła się ścieżka. | Sprawdź `HowToUse/en.pdf` i `HowToUse/pl.pdf`. |
| Link Main Page nie działa. | Aplikacja została przeniesiona i zmieniły się ścieżki względne. | Popraw link w odpowiednim pliku HTML. |
| Dane formularza zniknęły po zmianie języka. | Zmiana języka resetuje wartości zgodnie z projektem. | Wybierz język przed wypełnianiem formularza. |
| Nie ma przycisków save/load. | To jest wersja offline. | Użyj ręcznego eksportu/notatek albo modułu z Firebase z `WnG_Tools`. |
| Kalkulator pokazuje nieoczekiwany koszt. | Wartości wejściowe albo zaszyte tabele kosztów mogą nie odpowiadać oczekiwanym zasadom. | Sprawdź wpisane wartości i tabele kosztów w HTML. |

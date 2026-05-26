# 🇬🇧 User Guide (EN)

## What this app is
This is an offline **Wrath & Glory** character calculator web app. It contains two pages:
- **XP Calculator** (`XPCalculator.html`)
- **Character Creation** (`CharacterCreation.html`)

No login, cloud storage, or Firebase is required.

## How to start
1. Open `index.html` in your browser.
2. Choose one button:
   - **XP Calculator**
   - **Character Creation**

## XP Calculator (`XPCalculator.html`)
1. Choose language from the selector (English is default).
2. Enter current and target values.
3. The app calculates XP costs automatically.
4. Use **Reset values** to clear editable fields.
5. Use **Main Page** to return to `index.html`.

## Character Creation (`CharacterCreation.html`)
1. Set the XP pool (default: 155).
2. Fill attributes, skills, and optional talent costs.
3. Watch validation messages:
   - pool exceeded,
   - Tree of Learning rule.
4. Use **Instruction / Manual** to open language PDF (`HowToUse/en.pdf` or `HowToUse/pl.pdf`).
5. Use **Maximum attribute values** to open species limits.
6. Use **Reset** to restore defaults.

## Important behavior
- The app is local/offline.
- Data is recalculated in-browser.
- Changing language asks for confirmation and resets current form values.


## How to change the app's default language (DETAILED)

The default language is controlled separately in both main pages.
If you want the whole app to open in Polish by default, you need to change **both files** below from `en` to `pl`.

### 1) XP Calculator (`XPCalculator.html`)
1. Open `XPCalculator.html`.
2. Find the line in the script section with:
   - `let currentLanguage = "en";`
3. Change it to:
   - `let currentLanguage = "pl";`
4. Save the file.

Why this works:
- On page initialization, the app calls `applyLanguage(currentLanguage);`.
- So the value of `currentLanguage` defines which language is applied first when the page loads.

### 2) Character Creation (`CharacterCreation.html`)
1. Open `CharacterCreation.html`.
2. Find the line in the script section with:
   - `let currentLanguage = 'en';`
3. Change it to:
   - `let currentLanguage = 'pl';`
4. Save the file.

Why this works:
- On page initialization, the app calls `updateLanguage(currentLanguage);`.
- So the value of `currentLanguage` defines which language is active at startup.

### 3) Optional: switch default back to English
To restore English as default later, revert both values from `pl` back to `en` in the same two places.

### 4) Important notes
- Do not change keys in translation objects (`translations.en`, `translations.pl`) unless you are editing texts.
- The language selector itself (`<select id="languageSelect">`) does not set the startup default on its own; startup default comes from `currentLanguage` variable in each page script.
- After language switch during use, forms reset by design (current behavior described above).

---

# 🇵🇱 Instrukcja użytkownika (PL)

## Czym jest aplikacja
To offline’owa aplikacja webowa do kalkulacji postaci **Wrath & Glory**. Zawiera dwie strony:
- **XP Calculator** (`XPCalculator.html`)
- **Character Creation** (`CharacterCreation.html`)

Aplikacja nie wymaga logowania, chmury ani Firebase.

## Jak uruchomić
1. Otwórz `index.html` w przeglądarce.
2. Wybierz jeden przycisk:
   - **XP Calculator**
   - **Character Creation**

## XP Calculator (`XPCalculator.html`)
1. Wybierz język (domyślnie angielski).
2. Wpisz wartości bieżące i docelowe.
3. Koszty XP liczą się automatycznie.
4. Użyj **Reset values**, aby wyczyścić pola edytowalne.
5. Użyj **Main Page**, aby wrócić do `index.html`.

## Character Creation (`CharacterCreation.html`)
1. Ustaw pulę XP (domyślnie: 155).
2. Uzupełnij atrybuty, umiejętności i opcjonalne koszty talentów.
3. Sprawdzaj komunikaty walidacji:
   - przekroczenie puli,
   - zasada Tree of Learning.
4. **Instruction / Manual** otwiera PDF instrukcji (`HowToUse/en.pdf` lub `HowToUse/pl.pdf`).
5. **Maximum attribute values** otwiera limity ras.
6. **Reset** przywraca wartości domyślne.

## Ważne informacje
- Aplikacja działa lokalnie/offline.
- Obliczenia wykonywane są w przeglądarce.
- Zmiana języka prosi o potwierdzenie i resetuje aktualne wartości formularza.


## Jak zmienić domyślny język aplikacji (SZCZEGÓŁOWO)

Domyślny język jest ustawiany osobno w obu głównych stronach aplikacji.
Jeśli chcesz, aby cała aplikacja domyślnie uruchamiała się po polsku, musisz zmienić **oba pliki** poniżej z `en` na `pl`.

### 1) XP Calculator (`XPCalculator.html`)
1. Otwórz plik `XPCalculator.html`.
2. W sekcji skryptu znajdź linię:
   - `let currentLanguage = "en";`
3. Zmień ją na:
   - `let currentLanguage = "pl";`
4. Zapisz plik.

Dlaczego to działa:
- Podczas startu strony wywoływane jest `applyLanguage(currentLanguage);`.
- To oznacza, że wartość `currentLanguage` definiuje język ustawiany przy pierwszym ładowaniu strony.

### 2) Character Creation (`CharacterCreation.html`)
1. Otwórz plik `CharacterCreation.html`.
2. W sekcji skryptu znajdź linię:
   - `let currentLanguage = 'en';`
3. Zmień ją na:
   - `let currentLanguage = 'pl';`
4. Zapisz plik.

Dlaczego to działa:
- Podczas startu strony wywoływane jest `updateLanguage(currentLanguage);`.
- To oznacza, że wartość `currentLanguage` definiuje język aktywny na starcie.

### 3) Opcjonalnie: powrót do domyślnego angielskiego
Aby przywrócić angielski jako domyślny, w tych samych dwóch miejscach zamień `pl` z powrotem na `en`.

### 4) Ważne uwagi
- Nie zmieniaj kluczy w obiektach tłumaczeń (`translations.en`, `translations.pl`), chyba że edytujesz same treści.
- Sam selektor języka (`<select id="languageSelect">`) nie ustawia języka startowego; o języku startowym decyduje zmienna `currentLanguage` w skrypcie każdej strony.
- Po zmianie języka w trakcie pracy formularze resetują się celowo (to aktualne zachowanie aplikacji).


# Analiza modyfikacji aplikacji `WnG_offline_calculator`

## 1) Cel zmian

Zakres modyfikacji:
- ustawić **język angielski jako domyślny**;
- usunąć **wszystkie połączenia z Firebase**;
- usunąć przyciski/funkcje **Save** i **Load** (jeśli istnieją);
- usunąć modal potwierdzenia powiązany z tym przepływem;
- zaktualizować dokumentację tak, aby **angielski był zawsze pierwszy**.

---

## 2) Aktualny stan repozytorium (na podstawie plików)

### 2.1 Język domyślny
- `KalkulatorXP.html` ma przełącznik języka z opcjami `pl` i `en`, ale domyślnie ustawione jest:
  - `let currentLanguage = "pl";`
  - `<html lang="pl">`
  - kolejność opcji: najpierw `Polski`, potem `English`.
- `index.html` również ma `<html lang="pl">` i większość etykiet UI po polsku.

Wniosek: domyślny język aplikacji nie jest jeszcze angielski.

### 2.2 Firebase
- W repo istnieje `config/firebase-config.js` z realną strukturą konfiguracji Firebase (`apiKey`, `authDomain`, itp.).
- W repo istnieje `config/FirebaseREADME.md` opisujący konfigurację i inicjalizację Firestore.
- W przejrzanych plikach HTML nie ma aktywnego importu SDK Firebase ani jawnego użycia `window.firebaseConfig`.

Wniosek: połączenia/instrukcje Firebase nadal są obecne na poziomie repozytorium (konfiguracja + dokumentacja), nawet jeśli UI może już nie wykonywać zapisu chmurowego.

### 2.3 Save/Load i modal potwierdzenia
- W `KalkulatorXP.html` w aktualnej wersji widoczne są przyciski `Main page` i `Reset`.
- Nie znaleziono aktywnych przycisków `Save`/`Load` w przeszukanych plikach.
- W `index.html` istnieje modal (`secretOverlay`) związany z ukrytym gifem („koza”), a nie z zapisem/odczytem.

Wniosek: wymaganie „usunąć modal potwierdzenia” trzeba doprecyzować — nie widać modala Save/Load, ale jest inny modal UI.

---

## 3) Proponowany plan modyfikacji

### Etap A — język angielski jako default
1. `KalkulatorXP.html`:
   - zmienić `<html lang="pl">` → `<html lang="en">`;
   - zmienić `currentLanguage = "pl"` → `currentLanguage = "en"`;
   - rozważyć zmianę kolejności opcji w `<select>` na `English` jako pierwszą.
2. `index.html`:
   - zmienić `<html lang="pl">` → `<html lang="en">`;
   - (opcjonalnie) przetłumaczyć podstawowe etykiety landing page na EN albo dodać i18n, aby nie było miksu PL jako domyślnego UI.

### Etap B — całkowite odcięcie Firebase
1. Usunąć pliki konfiguracyjno-instrukcyjne Firebase (patrz sekcja 5).
2. Przeskanować repo pod kątem referencji:
   - `firebase`, `firestore`, `auth`, `storage`, `window.firebaseConfig`.
3. Jeśli wykryte zostaną odwołania w HTML/JS — usunąć skrypty/importy i martwy kod.

### Etap C — Save/Load + modal
1. Potwierdzić, czy Save/Load występują tylko w innych gałęziach/starych plikach, czy mają wrócić do zakresu zmian tu i teraz.
2. Jeśli istnieje modal potwierdzenia Save/Load w nieprzeanalizowanym pliku — usunąć go wraz z handlerami.
3. Nie usuwać niezależnych modalów UX (np. „secretOverlay”), jeśli nie są częścią Save/Load — chyba że to też ma wejść do porządków.

### Etap D — dokumentacja
1. Ustawić układ dokumentów dwujęzycznych: **EN first, PL second**.
2. Usunąć/zmienić opisy Firebase i cloud save.
3. Opisać aplikację jako offline-only (local flow).

---

## 4) Pliki do modyfikacji (najbardziej prawdopodobne)

- `KalkulatorXP.html` — default języka i etykiety startowe.
- `index.html` — `lang` i ewentualna spójność EN-first w UI startowym.
- `README.md` (jeśli istnieje) — kolejność sekcji EN/PL oraz usunięcie treści o Firebase.
- `docs/README.md` i `docs/Documentation.md` (jeśli istnieją) — analogicznie EN-first + usunięcie cloud/Firebase.

---

## 5) Pliki do usunięcia po wdrożeniu zmian

Na podstawie aktualnego stanu repo, kandydaci do skasowania:

1. `config/firebase-config.js`
   - zawiera wyłącznie konfigurację Firebase, która nie powinna istnieć w wersji offline.

2. `config/FirebaseREADME.md`
   - opisuje proces konfiguracji i użycia Firebase/Firestore, niezgodny z docelowym zakresem repo.

Dodatkowo: po pełnym skanie kodu warto usunąć każdy plik stricte pomocniczy dla Firebase, jeśli zostanie znaleziony (np. skrypty init Firestore, sample credentials itp.).

---

## 6) Ryzyka i punkty kontrolne

1. **Ryzyko niespójności językowej UI**
   - Samo przestawienie `currentLanguage` na EN nie wystarczy, jeśli landing page i inne ekrany zostaną po polsku.

2. **Ryzyko „martwych” odwołań po usunięciu plików Firebase**
   - Należy wykonać pełne wyszukiwanie referencji i test uruchomienia statycznego HTML.

3. **Ryzyko niejednoznacznego zakresu „modal do potwierdzenia”**
   - Obecny wykryty modal (`secretOverlay`) nie dotyczy Save/Load.

4. **Ryzyko niespójnej dokumentacji**
   - Szczególnie w dokumentach historycznych/starych folderach (`Old/`) mogą pozostać opisy nieaktualnego flow.

---

## 7) Lista pytań doprecyzowujących

1. Czy usuwamy **wyłącznie** modal potwierdzenia powiązany z Save/Load, czy **każdy modal** w aplikacji (w tym „secretOverlay” w `index.html`)?
2. Czy landing page (`index.html`) ma zostać w pełni przetłumaczony na EN, czy wystarczy tylko ustawienie `lang="en"` i pozostawienie części tekstów PL?
3. Czy folder `Old/` ma pozostać nietknięty jako archiwum, czy również czyścimy go z odniesień niezgodnych z offline EN-first?
4. Czy chcesz całkowicie usunąć dwujęzyczność i zostawić tylko EN, czy utrzymać PL jako język dodatkowy?
5. Czy poza analizą mam od razu wykonać pełną implementację zmian (kod + dokumentacja + usunięcia plików)?

---

## 8) Rekomendacja wykonawcza (kolejność prac)

1. Szybki skan repo pod Firebase/Save/Load/modal.
2. Zmiana default language na EN (`KalkulatorXP.html`, `index.html`).
3. Usunięcie plików Firebase i cleanup referencji.
4. Aktualizacja dokumentacji (EN first, bez Firebase).
5. Test ręczny: otwarcie `index.html` i `KalkulatorXP.html` offline.
6. Końcowy audit tekstów UI + grep kontrolny.

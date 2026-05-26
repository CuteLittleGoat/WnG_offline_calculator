# AGENTS.md — instrukcje dla agentów pracujących w repozytorium `WnG_offline_calculator`

Repozytorium `WnG_offline_calculator` zawiera samodzielną aplikację webową służącą jako offline’owy kalkulator postaci do gry fabularnej **Wrath & Glory**.

Repozytorium należy traktować jako projekt **stand-alone**.

Nie należy zakładać istnienia zewnętrznego repozytorium nadrzędnego, wspólnego systemu modułów, wspólnej bazy danych, wspólnej konfiguracji ani zewnętrznych zależności projektowych.

Docelowo repozytorium zawiera jedną aplikację:

- `Calculator`

Głównym językiem użytkowym projektu jest **angielski**.

Oznacza to, że:

- angielski ma być domyślnym językiem interfejsu;
- angielska dokumentacja ma być umieszczana jako pierwsza;
- angielska wersja instrukcji ma być kompletna sama w sobie;
- polski może istnieć jako drugi język, jeżeli aplikacja lub dokumentacja obsługują dwie wersje językowe.

---

## 1. Cel repozytorium

Celem repozytorium jest utrzymanie uproszczonego, samodzielnego i offline’owego kalkulatora postaci do **Wrath & Glory**.

Aplikacja powinna umożliwiać użytkownikowi korzystanie z kalkulatora bez konieczności:

- logowania;
- zakładania konta;
- łączenia się z bazą danych;
- korzystania z usług chmurowych;
- konfigurowania backendu;
- używania Firebase;
- używania zewnętrznych sekretów lub tokenów.

Aplikacja powinna działać jako statyczna aplikacja webowa zawsze wtedy, gdy jest to możliwe.

---

## 2. Zakres repozytorium

Repozytorium powinno zawierać wyłącznie pliki potrzebne do działania, stylowania, dokumentowania, testowania i utrzymania kalkulatora.

Nie należy dodawać innych aplikacji, modułów ani funkcji niezwiązanych bezpośrednio z kalkulatorem postaci.

Nie należy projektować tego repozytorium jako części większego systemu.

Nie należy opisywać go jako fragmentu większego projektu.

Nie należy dodawać zależności od zewnętrznej struktury folderów, zewnętrznych modułów ani zewnętrznych źródeł danych, jeżeli użytkownik wyraźnie o to nie poprosi.

Przed każdą zmianą należy sprawdzić aktualną strukturę repozytorium i aktualny stan plików.

---

## 3. Aplikacja `Calculator`

Jedyną docelową aplikacją w repozytorium jest:

```text
Calculator
```

Dokładna struktura folderów może zmieniać się w trakcie pracy.

Jeżeli pliki kalkulatora znajdują się bezpośrednio w katalogu głównym repozytorium, należy pracować z faktyczną strukturą zamiast automatycznie tworzyć nowy folder.

Jeżeli istnieje folder `Calculator`, należy traktować go jako główny folder aplikacji.

Nie należy zakładać istnienia innych modułów.

---

## 4. Domyślny język aplikacji

Domyślnym językiem aplikacji ma być **angielski**.

Dotyczy to w szczególności:

- tekstów widocznych w interfejsie;
- etykiet pól;
- przycisków;
- nagłówków;
- komunikatów walidacyjnych;
- komunikatów błędów;
- tekstów pomocy;
- placeholderów;
- tooltipów;
- ustawień początkowych aplikacji.

Jeżeli aplikacja ma przełącznik języka, domyślnie wybrany powinien być język angielski.

Jeżeli aplikacja zapisuje preferencję językową użytkownika lokalnie, język angielski nadal powinien być językiem awaryjnym, gdy zapisana preferencja nie istnieje albo jest nieprawidłowa.

Polska wersja językowa może istnieć jako druga wersja, ale nie może zastępować angielskiego jako języka domyślnego.

---

## 5. Wymóg działania offline

Repozytorium ma zawierać wersję offline’ową.

Nie należy dodawać integracji z:

- Firebase;
- Firestore;
- Firebase Realtime Database;
- Firebase Authentication;
- Firebase Storage;
- zewnętrzną bazą danych;
- kontami użytkowników;
- logowaniem;
- chmurowym zapisem postaci;
- chmurowym odczytem postaci;
- backendem wymaganym do podstawowego działania aplikacji.

Jeżeli potrzebne jest zapisywanie danych, należy preferować rozwiązania lokalne, takie jak:

- `localStorage`;
- eksport do pliku;
- import z pliku;
- kopiowanie danych jako tekst;
- inne proste rozwiązania lokalne zaakceptowane przez użytkownika.

Aplikacja nie powinna wymagać sekretów, tokenów ani prywatnych konfiguracji środowiskowych do normalnego działania.

---

## 6. Przyciski Save i Load

Ta wersja kalkulatora ma być pozbawiona chmurowego zapisu i chmurowego odczytu.

Nie należy zachowywać przycisków ani funkcji powiązanych z chmurowym zapisem i odczytem, takich jak:

- `Save`;
- `Load`;
- cloud save;
- cloud load;
- zapis do Firebase;
- odczyt z Firebase;
- logowanie użytkownika;
- zapisywanie danych na koncie użytkownika.

Jeżeli w kodzie istnieją takie funkcje, należy je usunąć albo zastąpić lokalnym rozwiązaniem tylko wtedy, gdy użytkownik wyraźnie o to poprosi.

Jeżeli w przyszłości zostanie dodany lokalny eksport/import, musi być jasno opisany jako funkcja lokalna, a nie jako zapis lub odczyt z chmury.

---

## 7. Dokumentacja

Dokumentacja musi opisywać aktualny stan tego repozytorium i tej aplikacji.

Po każdej zmianie kodu należy sprawdzić, czy trzeba zaktualizować dokumentację.

Zalecane pliki dokumentacji:

```text
README.md
docs/README.md
docs/Documentation.md
```

Jeżeli repozytorium nie używa folderu `docs/`, nie należy tworzyć zbędnej struktury dokumentacyjnej bez potrzeby lub bez polecenia użytkownika.

Dokumentacja nie może opisywać funkcji, których w aktualnej wersji aplikacji nie ma, zwłaszcza:

- Firebase;
- logowania;
- zapisu w chmurze;
- odczytu z chmury;
- kont użytkowników;
- zewnętrznych baz danych;
- przycisków `Save` i `Load`, jeżeli zostały usunięte lub nie istnieją.

---

## 8. Układ wersji językowych w dokumentacji

Dokumentacja użytkowa może być dwujęzyczna.

Jeżeli dokument zawiera dwie wersje językowe, **angielski musi być pierwszy**, a polski drugi.

Poprawny układ:

```markdown
# 🇬🇧 User instructions (EN)

Full English version.

# 🇵🇱 Instrukcja użytkownika (PL)

Pełna polska wersja.
```

Niepoprawny układ:

```markdown
# 🇵🇱 Instrukcja użytkownika (PL)

Pełna polska wersja.

# 🇬🇧 User instructions (EN)

Full English version.
```

Nie należy mieszać języków sekcja po sekcji ani akapit po akapicie.

Wersja angielska ma być kompletna sama w sobie.

Wersja polska, jeżeli istnieje, również ma być kompletna sama w sobie.

Nie należy tworzyć dokumentów, w których każdy akapit angielski jest natychmiast tłumaczony pod spodem na polski.

---

## 9. README.md

`README.md` jest główną instrukcją użytkownika.

Powinien być napisany przede wszystkim po angielsku.

Jeżeli zawiera wersję polską, powinna znajdować się po pełnej wersji angielskiej.

`README.md` powinien wyjaśniać:

- do czego służy kalkulator;
- jak go uruchomić;
- co i gdzie kliknąć;
- co robi każdy przycisk;
- co oznacza każde ważne pole;
- jak działa każda funkcja dostępna dla użytkownika;
- jak interpretować komunikaty;
- jak postępować w typowych sytuacjach;
- co zrobić, jeżeli pojawi się błąd albo pusty stan;
- jak działa lokalny import albo eksport, jeżeli taka funkcja istnieje.

Instrukcja użytkownika powinna być zrozumiała dla osoby, która nie zna programowania.

Należy unikać technicznego języka, jeżeli nie jest potrzebny do korzystania z kalkulatora.

---

## 10. Documentation.md

`Documentation.md`, jeżeli istnieje, jest dokumentacją techniczną.

Może używać języka technicznego.

Jej odbiorcą jest programista albo agent AI, który ma zrozumieć lub odtworzyć aplikację.

Dokumentacja techniczna powinna opisywać:

- aktualną strukturę plików;
- strukturę HTML;
- style CSS;
- layout;
- responsywność;
- użyte kolory, fonty i odstępy;
- pliki JavaScript i ich odpowiedzialności;
- logikę obliczeń;
- struktury danych;
- walidację;
- obsługę języków;
- lokalne zapisywanie danych, jeżeli istnieje;
- lokalny import i eksport, jeżeli istnieje;
- zależności;
- sposób odtworzenia aplikacji z plików.

Dokumentacja techniczna nie powinna opisywać funkcji, których nie ma w aktualnej aplikacji.

---

## 11. Dokumentacja nie jest changelogiem

Pliki dokumentacji mają opisywać aktualny stan aplikacji.

Nie należy dopisywać historii zmian w stylu:

- „wcześniej aplikacja używała Firebase”;
- „usunięto przycisk Save”;
- „dawniej działało to inaczej”;
- „poprzednia wersja miała inną strukturę”.

Takie informacje mogą znaleźć się w osobnej analizie migracyjnej, jeżeli użytkownik o nią poprosi, ale nie powinny znajdować się w zwykłej instrukcji użytkownika ani dokumentacji technicznej.

---

## 12. Analizy niezwiązane bezpośrednio ze zmianą kodu

Jeżeli polecenie użytkownika dotyczy analizy, audytu, przeglądu, planu zmian albo oceny problemu, a nie bezpośredniej edycji kodu, należy zapisać wnioski w folderze:

```text
Analizy
```

Dla każdej analizy należy utworzyć nowy plik `.md` o nazwie adekwatnej do tematu analizy.

Nazwa pliku powinna jasno wskazywać, czego dotyczy analiza.

Przykłady:

```text
Analizy/audyt-logiki-kalkulatora.md
Analizy/plan-usuniecia-firebase.md
Analizy/analiza-domyslnego-jezyka.md
Analizy/przeglad-zapisu-lokalnego.md
```

Nie należy nadpisywać starszych analiz, chyba że użytkownik wyraźnie o to poprosi.

---

## 13. Zawartość pliku analitycznego

Każdy plik z analizą powinien zawierać przynajmniej:

- datę analizy;
- temat analizy;
- pełny oryginalny prompt użytkownika;
- zakres analizy;
- sprawdzone pliki lub obszary kodu;
- ustalenia;
- wnioski;
- rekomendacje;
- ryzyka;
- proponowane następne kroki.

Pełny prompt użytkownika należy zapisać bez skracania.

Celem jest zachowanie kontekstu odpowiedzi i umożliwienie późniejszego zrozumienia, dlaczego dana analiza została wykonana.

Zalecany szablon:

```markdown
# Analiza: [temat]

Data: YYYY-MM-DD

## Oryginalny prompt użytkownika

[p pełna treść promptu użytkownika]

## Zakres analizy

[co zostało sprawdzone]

## Sprawdzone pliki

- `ścieżka/do/pliku`

## Ustalenia

[opis ustaleń]

## Wnioski

[opis wniosków]

## Rekomendacje

[lista rekomendacji]

## Ryzyka

[lista ryzyk]

## Następne kroki

[proponowane działania]
```

---

## 14. Folder `Analizy` a dokumentacja użytkowa i techniczna

Folderu `Analizy` nie należy opisywać w dokumentacji użytkowej jako elementu potrzebnego do korzystania z aplikacji.

Nie należy opisywać folderu `Analizy` w instrukcjach użytkownika, chyba że użytkownik wyraźnie o to poprosi.

Folder `Analizy` służy do pracy projektowej, audytowej i technicznej.

Nie jest częścią normalnego przepływu pracy użytkownika aplikacji.

---

## 15. Zmiany kodu wykonywane na podstawie pliku analitycznego

Jeżeli polecenie użytkownika dotyczy wykonania zmian w kodzie na podstawie wcześniej przygotowanej analizy, po wykonaniu zmian należy zaktualizować odpowiedni plik analityczny.

Do pliku analitycznego należy dopisać sekcję:

```markdown
## Zmiany wykonane w kodzie
```

Sekcja musi zawierać:

- nazwę zmienionego pliku;
- możliwie dokładną lokalizację zmiany;
- opis stanu przed zmianą;
- opis stanu po zmianie;
- informację, czy zmiana wymagała aktualizacji dokumentacji;
- informację, które dokumenty zostały zaktualizowane.

Zalecany format:

````markdown
## Zmiany wykonane w kodzie

### Plik: `Calculator/app.js`

Lokalizacja: funkcja `setDefaultLanguage`

Było:

```js
const defaultLanguage = "pl";
```

Jest:

```js
const defaultLanguage = "en";
```

Opis zmiany:

Domyślny język aplikacji został zmieniony z polskiego na angielski.

Wpływ na dokumentację:

- zaktualizowano `README.md`;
- zaktualizowano `docs/Documentation.md`.

# AGENTS.md — instrukcje dla agentów pracujących w repozytorium `WnG_offline_calculator`

Repozytorium `WnG_offline_calculator` zawiera uproszczoną, samodzielną i offline’ową wersję kalkulatora postaci do gry fabularnej **Wrath & Glory**.

Repozytorium nie jest projektem wielomodułowym.

Docelowo ma zawierać tylko jeden moduł:

- `Calculator`

Moduł ten jest oparty na module `Kalkulator` z większego repozytorium `WrathAndGlory`, ale w tym repozytorium ma działać jako prostsza, niezależna aplikacja.

Głównym językiem użytkowym tego repozytorium jest **angielski**.

Oznacza to, że:

- angielski ma być domyślnym językiem interfejsu;
- angielska dokumentacja ma być umieszczana jako pierwsza;
- angielska wersja instrukcji ma być kompletna sama w sobie;
- polski może pozostać jako drugi język, jeżeli aplikacja i dokumentacja obsługują dwie wersje językowe.

---

## 1. Cel repozytorium

Celem repozytorium jest przygotowanie uproszczonego kalkulatora postaci do **Wrath & Glory**, który może działać lokalnie lub jako statyczna aplikacja webowa.

Aplikacja powinna umożliwiać użytkownikowi korzystanie z kalkulatora bez konieczności logowania, łączenia się z bazą danych lub korzystania z usług chmurowych.

Repozytorium ma być prostsze od głównego repozytorium `WrathAndGlory`.

Nie należy zakładać, że posiada ono tę samą strukturę, te same moduły, te same źródła danych albo te same integracje.

---

## 2. Zakres repozytorium

Repozytorium powinno zawierać wyłącznie uproszczony kalkulator oraz pliki potrzebne do jego działania, stylowania, dokumentowania i utrzymania.

Nie należy przenosić do tego repozytorium innych modułów z większego repozytorium `WrathAndGlory`.

Nie należy dodawać funkcji, które należą do większego systemu modułowego, jeżeli użytkownik wyraźnie o to nie poprosi.

Przed każdą zmianą należy sprawdzić aktualną strukturę repozytorium i aktualny stan plików.

---

## 3. Moduł aplikacji

Jedynym docelowym modułem jest:

```text
Calculator
```

Dokładna struktura folderów może zmieniać się w trakcie pracy.

Jeżeli pliki kalkulatora znajdują się bezpośrednio w katalogu głównym repozytorium, należy pracować z faktyczną strukturą zamiast automatycznie tworzyć nowy folder.

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
- chmurowym zapisem postaci.

Aplikacja powinna działać jako statyczna aplikacja webowa zawsze wtedy, gdy jest to możliwe.

Jeżeli potrzebne jest zapisywanie danych, należy preferować rozwiązania lokalne, takie jak:

- `localStorage`;
- eksport do pliku;
- import z pliku;
- kopiowanie danych jako tekst;
- inne proste rozwiązania lokalne zaakceptowane przez użytkownika.

---

## 6. Przyciski Save i Load

Ta wersja kalkulatora ma być pozbawiona integracji z Firebase.

W związku z tym nie należy zachowywać przycisków ani funkcji powiązanych z chmurowym zapisem i odczytem, takich jak:

- `Save`;
- `Load`;
- cloud save;
- cloud load;
- zapis do Firebase;
- odczyt z Firebase;
- logowanie użytkownika;
- zapisywanie danych na koncie użytkownika.

Jeżeli oryginalny moduł `Kalkulator` zawiera takie funkcje, podczas przenoszenia do tego repozytorium należy je usunąć albo zastąpić lokalnym rozwiązaniem, ale tylko wtedy, gdy użytkownik wyraźnie o to poprosi.

Jeżeli w przyszłości zostanie dodany lokalny eksport/import, musi być jasno opisany jako funkcja lokalna, a nie jako zapis lub odczyt z chmury.

---

## 7. Dokumentacja

Dokumentacja musi opisywać aktualny stan tego repozytorium, a nie stan większego repozytorium `WrathAndGlory`.

Po każdej zmianie kodu należy sprawdzić, czy trzeba zaktualizować dokumentację.

Zalecane pliki dokumentacji:

```text
README.md
docs/README.md
docs/Documentation.md
```

Jeżeli repozytorium nie używa folderu `docs/`, nie należy tworzyć zbędnej struktury dokumentacyjnej bez potrzeby lub bez polecenia użytkownika.

Dokumentacja nie może opisywać funkcji, których w tej uproszczonej wersji nie ma, zwłaszcza:

- Firebase;
- logowania;
- zapisu w chmurze;
- odczytu z chmury;
- innych modułów z większego repozytorium;
- przycisków `Save` i `Load`, jeżeli zostały usunięte.

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

Dokumentacja techniczna nie powinna opisywać Firebase ani innych funkcji usuniętych z tej uproszczonej wersji.

---

## 11. Dokumentacja nie jest changelogiem

Pliki dokumentacji mają opisywać aktualny stan aplikacji.

Nie należy dopisywać historii zmian w stylu:

- „wcześniej aplikacja używała Firebase”;
- „usunięto przycisk Save”;
- „dawniej działało to inaczej”;
- „w oryginalnym repozytorium ta funkcja była inna”.

Takie informacje mogą znaleźć się w osobnej analizie migracyjnej, jeżeli użytkownik o nią poprosi, ale nie powinny znajdować się w zwykłej instrukcji użytkownika ani dokumentacji technicznej.

---

## 12. Migracja z większego repozytorium

Podczas kopiowania kodu z większego repozytorium `WrathAndGlory` należy usuwać albo dostosowywać elementy, które nie pasują do uproszczonej wersji offline.

Szczególną uwagę należy zwrócić na:

- importy Firebase;
- konfigurację Firebase;
- logikę logowania;
- zapis w chmurze;
- odczyt z chmury;
- przyciski `Save` i `Load`;
- odwołania do innych modułów;
- zależności od wspólnych danych większego repozytorium;
- polski jako domyślny język interfejsu;
- dokumentację opisującą większy projekt wielomodułowy.

Nie należy bezrefleksyjnie kopiować dokumentacji z większego repozytorium.

Każdy skopiowany plik musi zostać sprawdzony i dostosowany do faktycznego zakresu tego repozytorium.

---

## 13. Komentarze w kodzie

Komentarze w kodzie powinny być jasne i aktualne.

Ponieważ głównym językiem projektu jest angielski, nowe komentarze w kodzie powinny być pisane po angielsku.

Polskie komentarze można pozostawić, jeżeli już istnieją i nadal są poprawne.

Jeżeli modyfikowana jest sekcja kodu z polskim komentarzem, należy rozważyć przepisanie komentarza na jasny komentarz angielski.

Komentarze powinny wyjaśniać działanie lub cel fragmentu kodu.

Nie należy zostawiać komentarzy opisujących:

- Firebase, jeżeli funkcja została usunięta;
- przyciski `Save` i `Load`, jeżeli zostały usunięte;
- inne moduły, których nie ma w tym repozytorium;
- stare albo nieistniejące zachowanie.

---

## 14. Teksty interfejsu

Teksty widoczne dla użytkownika powinny być domyślnie angielskie.

Podczas zmiany etykiet, nagłówków, placeholderów, komunikatów, alertów, opisów i tooltipów należy:

- najpierw zadbać o wersję angielską;
- zachować spójne nazewnictwo w całej aplikacji;
- aktualizować polskie tłumaczenia tylko wtedy, gdy aplikacja nadal obsługuje język polski;
- upewnić się, że domyślnym językiem pozostaje angielski.

Nie należy zostawiać polskiego jako domyślnego języka interfejsu, chyba że użytkownik wyraźnie zmieni to wymaganie.

---

## 15. Layout i wygląd

Jeżeli zmiana dotyczy wyglądu aplikacji, należy sprawdzić, czy dokumentacja layoutu wymaga aktualizacji.

Dotyczy to zmian w:

- fontach;
- kolorach;
- ikonach;
- tłach;
- ramkach;
- cieniach;
- odstępach;
- szerokościach;
- wysokościach;
- responsywności;
- układzie elementów;
- wyglądzie przycisków;
- wyglądzie formularzy;
- wyglądzie komunikatów.

Jeżeli istnieje plik opisujący layout, na przykład:

```text
DetaleLayout.md
```

albo jego angielski odpowiednik, należy zaktualizować go tak, aby opisywał aktualny wygląd aplikacji.

Jeżeli taki plik nie istnieje, a zmiana wyglądu jest istotna, należy opisać najważniejsze informacje w dokumentacji technicznej, zamiast automatycznie tworzyć niepotrzebne nowe pliki.

---

## 16. Dane i zasady obliczeń

Kalkulator może zawierać dane, tabele, koszty, opcje i zasady walidacji związane z systemem **Wrath & Glory**.

Podczas zmiany logiki obliczeń lub danych należy:

- najpierw sprawdzić aktualną implementację;
- nie upraszczać zasad bez polecenia użytkownika;
- zachować istniejące działanie, jeżeli zadanie nie wymaga zmiany;
- sprawdzić, czy komunikaty i wartości wyliczane nadal są zgodne z logiką;
- zaktualizować dokumentację, jeżeli zmiana wpływa na działanie aplikacji.

Jeżeli jakaś zasada jest niejasna, należy zaznaczyć niepewność zamiast wymyślać regułę.

---

## 17. Lokalne przechowywanie danych

Ponieważ jest to wersja offline, przechowywanie danych powinno pozostać lokalne.

Dozwolone rozwiązania obejmują:

- dane tymczasowe w pamięci przeglądarki;
- `localStorage`;
- eksport postaci do pliku;
- import postaci z pliku;
- kopiowanie danych postaci jako tekst.

Nie należy dodawać przechowywania danych po stronie serwera.

Nie należy dodawać zapisu do Firebase.

Nie należy dodawać przechowywania danych na koncie użytkownika.

Jeżeli zostanie dodany lokalny import albo eksport, musi być jasno opisany jako funkcja lokalna.

---

## 18. Bezpieczeństwo i dane wrażliwe

Nie wolno zapisywać w repozytorium danych wrażliwych.

Dotyczy to między innymi:

- kluczy API;
- tokenów;
- haseł;
- prywatnych kluczy;
- sekretów;
- prywatnych plików środowiskowych;
- realnych danych użytkowników;
- danych logowania;
- konfiguracji produkcyjnych.

To repozytorium nie powinno wymagać sekretów do normalnego działania.

Jeżeli plik skopiowany z innego repozytorium zawiera konfigurację Firebase, tokeny albo inne wrażliwe dane, należy je usunąć, chyba że użytkownik wyraźnie poleci inaczej.

Jeżeli realny sekret został przypadkowo umieszczony w repozytorium, należy poinformować użytkownika, że powinien zostać zrotowany.

---

## 19. Zasady pracy z repozytorium

Przed edycją plików należy:

1. sprawdzić aktualną strukturę repozytorium;
2. sprawdzić aktualną treść istotnych plików;
3. ustalić, czy zmiana wpływa na dokumentację;
4. ustalić, czy zmiana wpływa na domyślny język aplikacji;
5. upewnić się, że zmiana nie przywraca Firebase ani logiki chmurowej;
6. upewnić się, że zmiana nie przywraca przycisków `Save` i `Load` jako funkcji chmurowych.

Nie należy commitować zmian bez wyraźnej prośby użytkownika.

Jeżeli użytkownik prosi o przygotowanie treści pliku, należy podać kompletną treść w odpowiedzi albo przygotować lokalny artefakt w rozmowie, zgodnie z poleceniem użytkownika.

Jeżeli użytkownik prosi o „przygotowanie” pliku, nie oznacza to automatycznej zgody na commit do GitHuba.

---

## 20. Pliki AGENTS.md

Nie należy modyfikować plików `AGENTS.md`, chyba że użytkownik wyraźnie prosi o przygotowanie nowej treści albo zmianę takiego pliku.

Jeżeli użytkownik prosi o nową treść `AGENTS.md`, należy podać proponowaną treść jasno i kompletnie.

Jeżeli użytkownik prosi o bezpośrednią aktualizację pliku w repozytorium, należy najpierw sprawdzić aktualną treść pliku, a następnie ostrożnie ją zastąpić.

Nie należy tworzyć dodatkowych zagnieżdżonych plików `AGENTS.md`, chyba że użytkownik wyraźnie o to poprosi.

---

## 21. Priorytety

Podczas pracy z tym repozytorium należy stosować następującą kolejność priorytetów:

1. Wyraźne polecenie użytkownika z bieżącej rozmowy.
2. Ten plik `AGENTS.md`.
3. Aktualna faktyczna struktura repozytorium.
4. Aktualna dokumentacja, ale tylko jeżeli pasuje do faktycznego stanu repozytorium.
5. Założenia z większego repozytorium `WrathAndGlory`, ale tylko wtedy, gdy zostały potwierdzone przez aktualne pliki albo przez użytkownika.

Jeżeli istnieje konflikt między starą skopiowaną dokumentacją a uproszczonym zakresem tego repozytorium, pierwszeństwo ma uproszczony zakres tego repozytorium.

Docelowe założenie projektu:

```text
jedno repozytorium
jeden kalkulator
angielski jako język domyślny
dokumentacja: angielski pierwszy, polski drugi
działanie offline
brak Firebase
brak chmurowych przycisków Save i Load
```

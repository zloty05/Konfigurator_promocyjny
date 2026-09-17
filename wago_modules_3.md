# Moduły WAGO – nowe pozycje do koszyka 1 (linki do stron produktowych)

> **Zakres:** moduły dodawane do **koszyka obowiązkowego (krok 1, rabat −50%)** — rozszerzenie listy z `wago_modules_2.md`.
> Ceny w kolumnie „Cena netto" są **cenami po rabacie promocyjnym** (tak jak w `wago_modules_2.md`).

## Koszyk 1 – Moduł enkodera absolutnego (SSI)

Nowa grupa, wstawiana **bezpośrednio po grupie „Moduł enkodera inkrementalnego"**.

| Nr katalogowy | Nazwa | Opis | Cena netto | Link |
|---|---|---|---|---|
| 750-630 | Moduł enkodera absolutnego (SSI) | 24b / 125kHz / Gray | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630 |
| 750-630/000-001 | Moduł enkodera absolutnego (SSI) | 24b / 125kHz / Bin | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-001 |
| 750-630/000-002 | Moduł enkodera absolutnego (SSI) | 24b / 250kHz / Bin | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-002 |
| 750-630/000-004 | Moduł enkodera absolutnego (SSI) | 24b / 125kHz / Gray + Status byte | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-004 |
| 750-630/000-005 | Moduł enkodera absolutnego (SSI) | 15b / 125kHz / Gray + Status byte | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-005 |
| 750-630/000-006 | Moduł enkodera absolutnego (SSI) | 24b / 250kHz / Gray | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-006 |
| 750-630/000-008 | Moduł enkodera absolutnego (SSI) | 25b / 125kHz / Gray | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-008 |
| 750-630/000-009 | Moduł enkodera absolutnego (SSI) | 13b / 250kHz / Bin | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-009 |
| 750-630/000-011 | Moduł enkodera absolutnego (SSI) | 25b / 125kHz / Bin | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-011 |
| 750-630/000-012 | Moduł enkodera absolutnego (SSI) | 13b / 125kHz / Gray | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-012 |
| 750-630/000-013 | Moduł enkodera absolutnego (SSI) | 29b / 125kHz / Bin | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_000-013 |
| 750-630/003-000 | Moduł enkodera absolutnego (SSI) | konfigurowalny | 657,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-interfejsu-ssi/p/750-630_003-000 |

**Pominięto celowo:** `750-630/000-010` — strona produktowa WAGO zwraca **HTTP 410 Gone** (wariant trwale wycofany, brak karty produktu). Nie występuje też na materiale źródłowym. Nie dodajemy, żeby nie umieszczać w ofercie pozycji z martwym linkiem.

## Koszyk 1 – Moduł do pomiaru rezystancji (mostki oporowe)

Nowa grupa, wstawiana **bezpośrednio po grupie „Moduł enkodera absolutnego (SSI)"**.

| Nr katalogowy | Nazwa | Opis | Cena netto | Link |
|---|---|---|---|---|
| 750-1491 | Moduł do pomiaru rezystancji (mostki oporowe) | 2 AI, mostki oporowe (DMS / tensometry) | 617,00 zł | https://www.wago.com/pl/systemy-i-o/modu%C5%82-wej%C5%9B%C4%87-analogowych-2-kana%C5%82owy/p/750-1491 |

---

## Dane do wdrożenia w `index.html`

Poniższe wartości wynikają z ustaleń i z konwencji obowiązującej w `BASKET1_ITEMS` — zebrane tutaj, żeby wdrożenie było mechaniczne.

### Ceny

Koszyk 1 ma rabat **−50%**, więc cena przekreślona `base` = cena promocyjna `promo` × 2 (reguła spójna dla wszystkich obecnych 22 pozycji koszyka 1):

| Pozycja | `promo` | `base` |
|---|---|---|
| 750-630 (wszystkie warianty) | 657,00 | 1 314,00 |
| 750-1491 | 617,00 | 1 234,00 |

### Nazwy grup i modułów

| Grupa (`group`) | Nazwa na kafelku (`name`) |
|---|---|
| Moduł enkodera absolutnego (SSI) | Moduł enkodera absolutnego (SSI) |
| Moduł do pomiaru rezystancji (mostki oporowe) | Moduł do pomiaru rezystancji (mostki oporowe) |

W obu grupach `group` = `name` (jak w istniejących grupach jednorodnych, np. „Moduł interfejsu RS-232/-485"). Dla 750-1491 świadomie ujednolicono obie formy na wariant ze słowem „do" — `group` i `name` to osobne klucze w `PRODUCT_TERMS`, więc dwie formy oznaczałyby dwa wpisy do tłumaczenia o identycznym znaczeniu.

Nie użyto nazwy „Moduł wejść analogowych" dla 750-1491, mimo że tak brzmi oficjalna nazwa WAGO — ta nazwa jest już zajęta przez trzy pozycje w koszyku 2 (750-450, 750-496, 750-497) i powtórzenie jej w koszyku 1 byłoby mylące.

### Kolejność w `BASKET1_ITEMS`

```
Moduł enkodera inkrementalnego          (8 poz.)  ← bez zmian
Moduł enkodera absolutnego (SSI)        (12 poz.) ← NOWA
Moduł do pomiaru rezystancji            (1 poz.)  ← NOWA
Liczniki                                (6 poz.)  ← bez zmian
Moduł sterowania silnikami krokowymi    (3 poz.)  ← bez zmian
Moduł PWM                               (1 poz.)  ← bez zmian
Moduł do zaworów proporcjonalnych       (1 poz.)  ← bez zmian
Moduł Safety                            (2 poz.)  ← bez zmian
Moduł interfejsu RS-232/-485            (1 poz.)  ← bez zmian
```

Razem koszyk 1: **22 → 35 pozycji**. Zasady kroku 1 (min. 2 różne moduły, max 5 łącznie, −50%) pozostają bez zmian.

### Do uzupełnienia dla rynków LT / LV

Nowe nazwy wymagają wpisów w `PRODUCT_TERMS.lt` i `PRODUCT_TERMS.lv` — inaczej zadziała fallback do polskiego tekstu. **Wpisy robocze są już w `index.html`** (stan: wdrożone), ale wymagają weryfikacji przez lokalne organizacje razem z resztą arkusza `weryfikacja_LT_LV.xlsx`:

| Klucz PL (tekst źródłowy) | Typ | LT (do weryfikacji) | LV (do weryfikacji) |
|---|---|---|---|
| Moduł enkodera absolutnego (SSI) | nazwa grupy + nazwa modułu | Absoliutinio enkoderio modulis (SSI) | Absolūtā enkodera modulis (SSI) |
| Moduł do pomiaru rezystancji (mostki oporowe) | nazwa grupy + nazwa modułu | Varžos matavimo modulis (tenzometriniai tilteliai) | Pretestības mērīšanas modulis (tenzometriskie tilti) |
| 2 AI, mostki oporowe (DMS / tensometry) | opis (zawiera polskie słowa) | 2 AI, tenzometriniai tilteliai (DMS) | 2 AI, tenzometriskie tilti (DMS) |
| konfigurowalny | opis (zawiera polskie słowo) | konfigūruojamas | konfigurējams |

Pozostałe opisy SSI (`24b / 125kHz / Gray` itd.) są czysto techniczne i nie wymagają tłumaczenia — fallback zwróci oryginał, tak jak dla istniejących opisów enkoderów inkrementalnych.

Ceny EUR dla nowych pozycji (`PRICES_EUR.lt` / `.lv`) — jak dla pozostałych modułów: **nie przeliczać z PLN kursem**, czekać na cennik od lokalnych organizacji. Do czasu ich dostarczenia nowe pozycje będą w wersjach LT/LV ukryte (filtr `ITEMS1` po `itemPrice()`), tak samo jak wszystkie obecne.

---

> **Weryfikacja linków:** wszystkie 13 adresów z tego pliku sprawdzone zapytaniem HTTP (2026-09-17) — 12× **200 OK**, `750-630/000-010` → **410 Gone** (pominięty). Opisy wariantów SSI pochodzą z oficjalnych tytułów stron produktowych WAGO, nie z odczytu materiału źródłowego.
>
> **Uwaga:** linki z polskimi nazwami kategorii są generowane dynamicznie przez WAGO.
> Jeśli któryś nie zadziała bezpośrednio, wejdź na https://www.wago.com/pl
> i wyszukaj numer katalogowy (np. `750-630/000-004`).

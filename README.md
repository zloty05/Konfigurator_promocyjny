# Konfigurator zestawu promocyjnego WAGO

Samodzielny, jednostronicowy konfigurator do osadzenia jako `<iframe>` lub wysłania klientowi jako link. Buduje konfigurację 2-etapowego koszyka i przekazuje dane do formularza HubSpot.

---

## Uruchomienie

Otwórz `index.html` bezpośrednio w przeglądarce lub udostępnij przez dowolny serwer HTTP:

```bash
npx serve .
# lub
python -m http.server 8080
```

Brak dependencji, brak etapu budowania. Jedyny plik produkcyjny to `index.html`.

---

## Struktura pliku `index.html`

| Sekcja | Opis |
|--------|------|
| `<head>` | Meta, favicon, Google Fonts, cały CSS |
| `#site-header` | Nagłówek z logo WAGO, `position: fixed` |
| `#progress-bar` | Pasek postępu (2 kroki) |
| `#base-set-card` | Zestaw bazowy — zawsze widoczny |
| `#step-1` | Koszyk obowiązkowy (−50%) |
| `#step-2` | Koszyk dodatkowy (−25%) |
| `#step-3` | Podsumowanie + miejsce na formularz HubSpot |
| `#live-summary` | Boczny panel podsumowania (desktop) |
| `#mobile-summary-bar` | Pasek dolny (mobile) |
| `<script>` | Cała logika JS — dane, stan, renderowanie |

---

## Podmiana danych produktowych

Dane modułów znajdują się w dwóch stałych na początku bloku `<script>`:

```javascript
const BASKET1_ITEMS = [
  { catalog: '750-637', desc: 'Enkoder', base: 1415.00, promo: 707.50 },
  // ...
];

const BASKET2_ITEMS = [
  { catalog: '750-1405', desc: '16DI 24V DC', base: 495.71, promo: 371.78 },
  // ...
];
```

- `catalog` — numer katalogowy (wyświetlany i przekazywany do HubSpot)
- `desc` — krótki opis
- `base` — cena bazowa w PLN (netto)
- `promo` — cena po rabacie w PLN (netto)

Cena zestawu bazowego:

```javascript
const BASE_SET_PRICE = 1000.00;
```

---

## Integracja z HubSpot

### Krok 1 — Utwórz formularz w HubSpot

1. Zaloguj się do HubSpot → **Marketing → Formularze**
2. Utwórz nowy formularz lub edytuj istniejący
3. Dodaj **ukryte pola** (typ: `single-line text`):

| Nazwa pola (internal name) | Opis |
|----------------------------|------|
| `konfiguracja_koszyk1` | Wybrane moduły z koszyka 1, np. `750-637×2, 750-404×1` |
| `konfiguracja_koszyk2` | Wybrane moduły z koszyka 2, np. `750-1405×1` |
| `wartosc_calkowita` | Łączna wartość konfiguracji, np. `2415.00 PLN` |
| `sterownik_bazowy` | Zawsze `750-8001` |

4. Skopiuj **Portal ID** i **Form ID** z sekcji Embed Code

### Krok 2 — Dodaj skrypt HubSpot do `<head>`

```html
<script charset="utf-8" type="text/javascript"
  src="//js.hsforms.net/forms/embed/v2.js">
</script>
```

### Krok 3 — Podmień funkcję `initHubspotForm()` w `<script>`

Znajdź w pliku blok oznaczony komentarzem `PODMIEŃ TEN BLOK` i zastąp go:

```javascript
function initHubspotForm() {
  if (window.hbspt) {
    hbspt.forms.create({
      portalId: "TWÓJ_PORTAL_ID",   // np. "12345678"
      formId:   "TWÓJ_FORM_ID",     // np. "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      target:   "#hubspot-form-placeholder",
      onFormReady: function() {
        fillHubspotFields();
      }
    });
  }
}
```

### Jak działa przekazywanie danych

Funkcja `fillHubspotFields()` jest wywoływana automatycznie przy przejściu do etapu 3. Ustawia wartości ukrytych pól przed submitem formularza.

Dane są też dostępne globalnie (dla asynchronicznego embedu HubSpot):

```javascript
window.__wagoConfig = {
  konfiguracja_koszyk1: "750-637×2, 750-404×1",
  konfiguracja_koszyk2: "750-1405×1",
  wartosc_calkowita:    "2415.00 PLN",
  sterownik_bazowy:     "750-8001",
};
```

---

## Zasady biznesowe

### Koszyk 1 (obowiązkowy, −50%)
- Minimum **2 różne** moduły
- Łączna ilość wszystkich wybranych pozycji: maksymalnie **5 szt.**
- Przycisk „Dalej" jest zablokowany do spełnienia obu warunków

### Koszyk 2 (dodatkowy, −25%)
- Wybór opcjonalny — można pominąć
- Brak limitu ilości

---

## Osadzanie jako iframe

```html
<iframe
  src="https://twoja-domena.pl/konfigurator/index.html"
  width="100%"
  height="800"
  frameborder="0"
  style="border: none; min-height: 600px;">
</iframe>
```

Konfigurator nie używa `position: fixed` na `body` — scroll wewnątrz iframe działa poprawnie.

---

## Dostępność

Plik spełnia podstawowe wymagania WCAG 2.1 AA:
- Poprawny `lang="pl"`, semantyczne landmarki HTML5
- Natywne elementy `<input type="checkbox">` z właściwymi `<label>`
- `aria-live="polite"` na dynamicznych sekcjach
- Zarządzanie focusem przy zmianie etapu
- Kontrast kolorów zgodny z wymaganiami (zielony `#6ec800` na białym tle spełnia AA dla dużego tekstu)

---

## Format walut

Ceny wyświetlane są w formacie polskim: `1 234,56 zł` (spacja jako separator tysięcy, przecinek dziesiętny) z użyciem `Intl.NumberFormat('pl-PL')`.

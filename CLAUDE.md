# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projekt

Konfigurator zestawu promocyjnego WAGO — jednostronicowa aplikacja umożliwiająca klientom skomponowanie zestawu modułów PLC z rabatem promocyjnym i wysłanie zamówienia przez formularz HubSpot. Docelowo osadzana jako `<iframe>` na stronie wago.com.

Produkcja: `https://konfigurator-promocyjny.pages.dev/`
Repozytorium: `https://github.com/zloty05/Konfigurator_promocyjny`

## Uruchomienie lokalne

Projekt nie ma etapu budowania — to jeden plik `index.html`. Uruchamiaj przez Live Server w VS Code (prawy klik → *Open with Live Server*, port 5500) lub:

```powershell
npx serve . -p 5500
```

Do testowania iframe otwórz `test-iframe.html` przez Live Server na osobnym porcie. Plik symuluje stronę hosta i osadza konfigurator z `https://konfigurator-promocyjny.pages.dev/` — zmień `src` na `http://localhost:5500` jeśli testujesz lokalne zmiany.

## Wdrożenie

Push do `main` → Cloudflare Pages deployuje automatycznie. Plik `_headers` konfiguruje nagłówki HTTP — zmiany w nim wymagają wdrożenia żeby zadziałały (lokalnie ignorowany).

## Architektura

Cała aplikacja żyje w `index.html` (~56 KB) — CSS i JS wbudowane w plik. Zero zależności npm, zero etapu budowania.

### Przepływ aplikacji (3 kroki)

```
Krok 1: Koszyk obowiązkowy (basket1)
  → min. 2 różne moduły, max 5 łącznie, rabat −50%
Krok 2: Koszyk opcjonalny (basket2)
  → dowolna ilość, rabat −25%
Krok 3: Podsumowanie + formularz HubSpot
  → ukryte pola auto-wypełniane z konfiguracji
```

### Stan aplikacji

```javascript
state = {
  currentStep: 1 | 2 | 3,
  basket1: { '750-637': { qty: 1, checked: false }, ... },
  basket2: { '750-1405': { qty: 1, checked: false }, ... }
}
```

Dane produktów są hardcoded w `BASKET1_ITEMS` i `BASKET2_ITEMS` (tablice obiektów z polami `catalog`, `desc`, `base`, `promo`, `url`). Aby zmienić produkty lub ceny — edytuj te stałe i zrób redeploy.

### Kluczowe funkcje JS

| Funkcja | Rola |
|---|---|
| `renderModuleCards()` | Generuje HTML kafelek (innerHTML całego kontenera) |
| `syncModuleCard()` | Aktualizuje DOM jednego kafelka bez re-renderu całości |
| `toggleModule()` | Toggle zaznaczenia + update summary |
| `changeQty()` | Zmiana ilości (respektuje limit 5 w basket1) |
| `validateStep1()` | Walidacja: ≥2 unikalne AND ≤5 łącznie |
| `renderLiveSummary()` | Update sidebar i mobile bar |
| `computeSummary()` | Kalkulacja linii podsumowania i sum |
| `fillHubspotFields()` | Wypełnia ukryte pola HubSpot przed submitem |
| `goToStep(n)` | Nawigacja między krokami (hide/show + focus) |

### Layout i responsywność

- Desktop (>600px): grid 2-kolumnowy — kolumna treści + sticky sidebar `#live-summary`
- Mobile (≤600px): sidebar ukryty, widoczny fixed `#mobile-summary-bar` u dołu ekranu
- `--content-max: 860px` — max szerokość kontenera

### CSS

CSS variables w `:root` — przy zmianach kolorystyki lub layoutu zacznij od nich. Kluczowe: `--wago-green: #6ec800`, `--wago-dark: #1f2837`, `--header-h: 60px`.

Kafelki modułów używają klasy `.module-card--selected` (zielone tło + lewa linia) do wizualnego oznaczenia wybranych pozycji.

## Integracja HubSpot

Formularz ładowany dynamicznie na kroku 3 przez `initHubspotForm()`. Konfiguracja:
- portalId: `145269444`
- formId: `9e702531-565f-4d62-b800-95fb4f5cf4f2`
- region: `eu1` (skrypt embed `//js-eu1.hsforms.net/forms/embed/v2.js` jest w `<head>`)

WAGO udostępniło **jedno ukryte pole `poem`** (single-line text). Cała konfiguracja trafia do tego jednego pola jako czytelny, wieloliniowy tekst:

```
KONFIGURACJA WAGO

Baza: 750-8001 + 750-600 — 1 000,00 zł

Koszyk obowiązkowy (-50%):
  750-637 ×2 — 1 415,00 zł
  750-404 ×1 — 562,00 zł

WARTOŚĆ CAŁKOWITA: 2 977,00 zł
```

Formularz renderuje się w **iframe same-origin** (jego `src` jest pusty, więc mamy dostęp do `contentDocument`). Prefill przez `values` HubSpot dla pola `poem` ignoruje. `buildPoem()` składa string z `computeSummary()`, a `setPoemInIframe()` (wołane z `onFormReady`) odnajduje iframe HubSpota, sięga do `contentDocument`, wpisuje wartość do `input/textarea[name="poem"]` i wyzwala eventy `input`/`change`. Ma retry (do 20× co 250 ms), bo iframe ładuje się asynchronicznie. Jeśli WAGO udostępni więcej pól — dopisać je w `setPoemInIframe()`.

## Status: warunkowy wyjątek HQ

Centrala WAGO (HQ) zgodziła się na formularz HubSpot na zewnętrznej domenie **wyłącznie jako tymczasowy wyjątek** (2026-06-23) — pełna odpowiedzialność i compliance po stronie lokalnej organizacji. Zmiany techniczne wynikające z tych warunków (lazy-load skryptu HubSpot, `frame-ancestors`) są już wdrożone w kodzie. Część warunków jest organizacyjna (uzgodnienia z IT/Legal/DPO, RoPA, obsługa incydentów) i jest prowadzona poza repo. Szczegóły i kontekst: plan `bior-c-pod-uwag-thanks-zazzy-salamander.md`.

## CSP i iframe

Plik `_headers` kontroluje nagłówki Cloudflare Pages. `frame-ancestors` jest **wdrożone** — konfigurator może być osadzany tylko przez `wago.com` i subdomeny (produkcyjnie: `https://www.wago.com/pl/zestawy-bc100/konfigurator-zestawu-bc100`):

```
Content-Security-Policy: frame-ancestors 'self' https://wago.com https://*.wago.com
```

CSP zawiera **tylko** `frame-ancestors` (whitelisting domeny osadzającej) — celowo bez `script-src`/`frame-src`, bo HubSpot + reCAPTCHA ładują wiele domen i restrykcyjny CSP zasobów zepsułby formularz. `_headers` działa dopiero po wdrożeniu na Cloudflare Pages (lokalnie ignorowany).

Skrypt embedu HubSpot **nie jest w `<head>`** — `initHubspotForm()` ładuje go lazy dopiero przy wejściu na krok 3 (`loadHubspotScript()`, idempotentne, flaga `hubspotFormRendered`), żeby tracking/cookie HubSpot nie startował na krokach 1–2.

Kod iframe dla WAGO:
```html
<iframe
  src="https://konfigurator-promocyjny.pages.dev/"
  width="100%"
  height="900"
  style="border: none; display: block;"
  title="Konfigurator zestawu promocyjnego WAGO"
  loading="lazy"
  sandbox="allow-scripts allow-same-origin allow-forms allow-popups"
></iframe>
```

## Dokumentacja modułów

`wago_modules.md` — lista wszystkich modułów z numerami katalogowymi, opisami i linkami do wago.com. Używaj jako źródło przy dodawaniu/aktualizacji produktów.

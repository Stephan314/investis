# Investis — Persönliches Investment Dashboard

**Version 6.5** · Ein persönliches, browserbasiertes Investment-Dashboard zur Analyse und Beobachtung von Aktien, ETFs und anderen Assets. Entwickelt als wachsendes System das mit dem eigenen Wissen und den eigenen Anforderungen mitwächst.

**Live-URL:** `https://stephan314.github.io/investis/investment-dashboard.html`

---

## Features

### Watchlists
- **Hauptliste** mit 30 vordefinierten Aktien und ETFs — kuratiert nach Marktkapitalisierung >1,5 Mrd. €, ADR >5%, Volumen >5 Mio., Kurs >5 €
- **Eigene Watchlist** — beliebige Assets hinzufügen mit Ticker, Name, Sektor, Börse, ADR%, Signal und Notiz
- **Bearbeiten** — jeder eigene Eintrag kann nachträglich editiert werden (✎-Button in der Zeile)
- Filterung nach Sektor, Sortierung nach allen Spalten
- **Export/Import** der eigenen Watchlist als JSON-Datei

### Live-Daten (`<> Live-Daten`)
- Aktualisiert Kurse, Tagesänderung, KGV, Beta und Marktkapitalisierung für alle Assets — Hauptliste **und** eigene Watchlist
- Datenquellen: **Financial Modeling Prep (FMP)** für US-Titel, **Yahoo Finance** als Fallback
- Europäische Titel (RHM, RWE, BESI, ASML) direkt über Yahoo mit korrekten Börsensuffixen
- Fortschrittsbalken zeigt den aktuellen Stand
- Ticker-Besonderheiten: Block Inc. handelt seit Januar 2025 unter `XYZ` (nicht mehr `SQ`), HEICO unter `HEI`

### KI-Update (`* KI-Update`)
Vier Analyse-Modi via Claude AI — öffnet ein Seitenpanel:
- **Einzelner Ticker** — Einschätzung, technische Analyse, Risiken, Handlungsempfehlung
- **Meine Watchlist** — Kurzanalyse aller selbst hinzugefügten Assets
- **Top 5 Signale** — Ranking der Kaufsignal-Titel nach Potenzial
- **Marktlage** — Makro-Überblick, Sektoren mit Rückenwind/Druck, Wochentipp

### KI alles (`* KI alles`)
- Sendet alle Assets (Hauptliste + eigene Watchlist) in einem Claude-Aufruf
- Aktualisiert Signale und Begründungen für alle Titel
- Empfohlene Reihenfolge: erst **Live-Daten**, dann **KI alles**

### Handlungs-Matrix
- Alle Assets als Karten mit farbcodierten Signalen: Kaufen / Beobachten / Halten / Meiden
- Filterbar nach Signal-Typ
- Eigene Assets mit ★ markiert

### Chart
- Eingebettete TradingView-Charts per Klick aus Watchlist oder Sidebar
- Vorgeladene Indikatoren: Moving Average (9), RSI (14), MACD
- Sidebar mit Kennzahlen und visueller Handlungs-Matrix pro Asset

### Wissensbasis
- Nachschlagewerk zu KGV, KBV, EV/EBITDA, FCF Yield, ADR, ATR, Beta, RSI, MACD, Chart-Grundlagen, Strategien, Risikomanagement

---

## Infrastruktur-Übersicht

```
Browser (GitHub Pages)
    │
    ├── TradingView Widget API  →  Charts (kostenlos, direkt)
    │
    └── Cloudflare Worker  →  Proxy für externe APIs
            │
            ├── Anthropic Claude API  →  KI-Analysen
            ├── Financial Modeling Prep (FMP)  →  US-Kursdaten
            └── Yahoo Finance  →  Europäische Titel + Fallback
```

---

## Setup — Ersteinrichtung

### 1. GitHub Pages
1. Repository erstellen (Public)
2. `investment-dashboard.html` und `README.md` hochladen
3. Settings → Pages → Branch: main → Save
4. Nach 1–2 Minuten erreichbar unter `https://BENUTZERNAME.github.io/REPOSITORY/investment-dashboard.html`

### 2. Cloudflare Worker (API-Proxy)
Der Worker ist notwendig weil Browser direkte API-Aufrufe aus Sicherheitsgründen blockieren (CORS).

1. Account erstellen auf [cloudflare.com](https://cloudflare.com)
2. Workers & Pages → Create application → Start with Hello World
3. Name vergeben (z.B. `investis-proxy`), Deploy klicken
4. Edit code → gesamten Code ersetzen mit Inhalt aus `cloudflare-worker.js` → Deploy
5. Settings → Variables and Secrets → zwei Secrets anlegen:
   - `ANTHROPIC_API_KEY` → API-Key von [console.anthropic.com](https://console.anthropic.com)
   - `FMP_API_KEY` → API-Key von [financialmodelingprep.com](https://financialmodelingprep.com)

### 3. Worker-URL ins Dashboard eintragen
In `investment-dashboard.html` die Konstante `WORKER` anpassen:
```javascript
const WORKER = 'https://DEIN-WORKER.DEIN-ACCOUNT.workers.dev';
```

### 4. Anthropic API — Guthaben
Die Claude API ist kostenpflichtig. Mindestguthaben: **5 $** auf [console.anthropic.com](https://console.anthropic.com) → Billing. Für den persönlichen Gebrauch reicht das sehr lange.

---

## Updates einspielen

1. Neue `investment-dashboard.html` auf GitHub hochladen (alte Datei überschreiben)
2. Im Browser: **Strg+Shift+R** (Hard Refresh) um Cache zu leeren
3. Versionsnummer oben links prüfen

**Wichtig vor jedem Update:** Eigene Watchlist exportieren (`↓ Export`) — auf GitHub Pages bleibt der localStorage zwar erhalten, aber zur Sicherheit immer vorher sichern.

---

## Eigene Watchlist — Geräteübergreifend

Die Watchlist ist im Browser-localStorage gespeichert und gerätegebunden. Für mehrere Geräte:

1. Auf Gerät A: Meine Watchlist → `↓ Export` → JSON-Datei in Google Drive / iCloud ablegen
2. Auf Gerät B: Datei aus Drive laden → `↑ Import` im Dashboard

Das Export-Format ist ein einfaches JSON-Array und kann auch manuell bearbeitet werden.

---

## Datenquellen & Ticker-Besonderheiten

| Ticker | Besonderheit | Lösung |
|--------|-------------|--------|
| `SQ` | Block Inc. wechselte Ticker zu `XYZ` (Jan. 2025) | Worker und TV-Symbol auf `XYZ` gesetzt |
| `HEICO` | Handelt an NYSE unter `HEI` | Yahoo-Map auf `HEI` gesetzt |
| `RHM` | Xetra, kein FMP-Zugang im Free Plan | Yahoo Finance: `RHM.DE` |
| `RWE` | Xetra, kein FMP-Zugang im Free Plan | Yahoo Finance: `RWE.DE` |
| `BESI` | Amsterdam, kein FMP-Zugang im Free Plan | Yahoo Finance: `BESI.AS` |
| `ASML` | Amsterdam, kein FMP-Zugang im Free Plan | Yahoo Finance: `ASML.AS` |

**FMP Free Plan:** Unterstützt nur US-Titel über die neue `/stable/` API.
**FMP Legacy API (`/api/v3/`):** Abgeschaltet — nur noch für Accounts vor August 2025.

---

## Diagnose & Fehlerbehebung

### Allgemeine Prüfung
Buttons funktionieren nicht oder Daten werden nicht geladen → JavaScript-Fehler.
- **F12** → Console → Fehlermeldungen lesen
- Häufigste Ursache: falsche Worker-URL oder nicht deployeter Worker-Code

### Chrome DevTools — Cache leeren
- **Schnell:** Strg+Shift+R
- **Gründlich:** F12 → Rechtsklick auf Reload-Button → "Cache leeren und hart neu laden"
- **Komplett:** Strg+Shift+Entf → "Bilder und Dateien im Cache" → "Daten löschen"

### Console-Tests für die Datenanbindung

In Chrome: **F12 → Console → `allow pasting` eingeben → Enter → Test-Code einfügen**.

**Test 1 — Claude API / Worker erreichbar?**
```javascript
fetch('https://DEIN-WORKER.workers.dev', {
  method:'POST',
  headers:{'Content-Type':'application/json'},
  body:JSON.stringify({model:'claude-sonnet-4-6',max_tokens:10,messages:[{role:'user',content:'Hi'}]})
}).then(r=>r.json()).then(d=>console.log(JSON.stringify(d))).catch(e=>console.error(e))
```
✅ Erwartet: JSON mit `content`-Array und `text:"Hallo..."` oder ähnlich  
❌ `Failed to fetch` → Worker-URL falsch oder Worker nicht deployed  
❌ `not_found_error` → Modellname veraltet, aktuell: `claude-sonnet-4-6`  
❌ `Host not in allowlist` → Cloudflare-Sicherheitsregel aktiv, Worker-Code prüfen

**Test 2 — FMP-Daten für US-Ticker?**
```javascript
fetch('https://DEIN-WORKER.workers.dev', {
  method:'POST',
  headers:{'Content-Type':'application/json','X-Action':'fmp-quote'},
  body:JSON.stringify({ticker:'NVDA'})
}).then(r=>r.json()).then(d=>console.log(JSON.stringify(d))).catch(e=>console.error(e))
```
✅ Erwartet: `{"ticker":"NVDA","price":225.32,"beta":2.24,...}`  
❌ Alle Felder `null` → FMP API-Key nicht gesetzt oder falscher Endpoint  
❌ `"Legacy Endpoint"` Fehler → FMP Free Plan, `/api/v3/` abgeschaltet → `/stable/` verwenden  
❌ `"Premium Qu..."` in Fehlermeldung → Europäischer Ticker auf FMP Free Plan → auf Yahoo umleiten

**Test 3 — Yahoo Finance für europäische Ticker?**
```javascript
fetch('https://DEIN-WORKER.workers.dev', {
  method:'POST',
  headers:{'Content-Type':'application/json','X-Action':'fmp-quote'},
  body:JSON.stringify({ticker:'RHM'})
}).then(r=>r.json()).then(d=>console.log(JSON.stringify(d))).catch(e=>console.error(e))
```
✅ Erwartet: `{"ticker":"RHM","price":1120,"source":"Yahoo",...}`  
❌ `Internal Server Error` → Worker-Code veraltet (Legacy FMP-Endpoints)  
❌ `price:null` → Yahoo-Symbol falsch, YAHOO_MAP im Worker prüfen

**Test 4 — Mehrere Ticker gleichzeitig testen**
```javascript
['IONQ','HIMS','SANA','HEICO','SQ'].forEach(tk =>
  fetch('https://DEIN-WORKER.workers.dev', {
    method:'POST',
    headers:{'Content-Type':'application/json','X-Action':'fmp-quote'},
    body:JSON.stringify({ticker:tk})
  }).then(r=>r.json()).then(d=>console.log(tk, d.price, d.source, d.unavailable||'OK'))
)
```
✅ Erwartet: Jeder Ticker mit Preis und Quelle (FMP oder Yahoo)  
❌ `unavailable: true` → Ticker bei weder FMP noch Yahoo gefunden  
→ Mögliche Ursachen: Ticker umbenannt (wie SQ→XYZ), falsches Symbol, Premium erforderlich

**Test 5 — FMP direkt testen (ohne Worker)**
```javascript
fetch('https://financialmodelingprep.com/stable/quote?symbol=NVDA&apikey=DEIN_FMP_KEY')
.then(r=>r.json()).then(d=>console.log(JSON.stringify(d))).catch(e=>console.error(e))
```
✅ Erwartet: Array mit Kurs-Daten  
❌ `403 Forbidden` mit Legacy-Meldung → `/api/v3/` verwenden war alt, jetzt `/stable/`  
❌ Leeres Array → Ticker nicht im Free Plan enthalten

### Häufige Fehlerbilder und Lösungen

| Fehlerbild | Ursache | Lösung |
|---|---|---|
| Buttons zeigen Unicode-Text (`\u21BB`) | Python hat Escapes nicht aufgelöst | HTML-Entities oder direkte Zeichen verwenden |
| `0 Titel aktualisiert` | Falscher FMP-Endpoint oder Key | `/stable/` API verwenden, Key prüfen |
| Nur 10-14 Titel aktualisiert | Europäische Titel fehlen, FMP Free Plan | Yahoo Finance für EU-Titel in YAHOO_MAP |
| Chart zeigt "Symbol gibt es nicht" | Falsche Börse im TV-Symbol (z.B. NYSE:TSLA) | TV_SYM-Map im Dashboard korrigieren |
| KI-Update: `Failed to fetch` | Lokale Datei, kein GitHub Pages | Dashboard über GitHub Pages URL aufrufen |
| Watchlist leer nach Update | localStorage an alte URL gebunden | Vor Updates exportieren, danach importieren |

---

## Kennzahlen

| Kennzahl | Beschreibung |
|---|---|
| ADR% | Average Daily Range — durchschnittliche Tagesbewegung |
| KGV | Kurs-Gewinn-Verhältnis |
| KBV | Kurs-Buchwert-Verhältnis |
| Beta | Marktkorrelation und Volatilitätsmaß |
| Signal | KI-Handlungsempfehlung: Kaufen / Beobachten / Halten / Meiden |
| Live-Kurs | Aktueller Kurs via FMP oder Yahoo (nach Live-Update) |
| Änd% | Tagesveränderung in % (nach Live-Update) |

---

## Kosten-Übersicht

| Komponente | Anbieter | Kosten |
|---|---|---|
| Hosting | GitHub Pages | kostenlos |
| API-Proxy | Cloudflare Workers | kostenlos (100k Req/Tag) |
| KI-Analysen | Anthropic Claude API | ~5 $ = sehr lange |
| US-Kursdaten | Financial Modeling Prep | kostenlos (250 Req/Tag) |
| EU-Kursdaten | Yahoo Finance (inoffiziell) | kostenlos |
| Charts | TradingView Widget | kostenlos |

---

## Roadmap

- [x] Live-Kurse via FMP + Yahoo Finance
- [x] Kennzahlen-Aktualisierung (KGV, Beta, Marktkapitalisierung)
- [x] KI-Analyse für alle Assets auf einmal
- [x] Eigene Watchlist bearbeiten
- [x] Live-Daten und KI für eigene Watchlist
- [ ] Portfolio-Tracker mit Einstiegspreisen und P&L
- [ ] Google Drive Integration für geräteübergreifende Watchlist
- [ ] News-Feed pro Ticker
- [ ] Automatisches tägliches Update via Cloudflare Cron
- [ ] Mobile-optimierte Ansicht

---

## Versionsverlauf

| Version | Änderungen |
|---|---|
| v6.5 | Live-Daten und KI alles auf eigene Watchlist erweitert; Asset-Bearbeitung |
| v6.4 | Block Inc. Ticker SQ → XYZ; HEICO → HEI korrigiert |
| v6.3 | Button-Encoding-Fix; Retry-Logik für Yahoo Finance |
| v6.2 | Yahoo Finance als Fallback für alle Ticker; Encoding-Fixes |
| v6.1 | Cloudflare Worker mit Yahoo Finance für europäische Titel |
| v6.0 | FMP Live-Daten; KI alles Button; Live-Kurs und Änd%-Spalten |
| v5.1 | Export/Import für eigene Watchlist |
| v5.0 | KI-Update Panel via Cloudflare Worker |
| v4.0 | Eigene Watchlist mit lokalem Speicher |
| v3.0 | TradingView Charts; Handlungs-Matrix; Wissensbasis |

---

*Entwickelt mit Claude (Anthropic) · Persönliches Projekt · Kein kommerzieller Einsatz*

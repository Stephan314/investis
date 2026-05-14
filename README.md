# Investis — Persönliches Investment Dashboard

Ein persönliches, browserbasiiertes Investment-Dashboard zur Analyse und Beobachtung von Aktien, ETFs und anderen Assets. Entwickelt als wachsendes System das mit dem eigenen Wissen und den eigenen Anforderungen mitwächst.

---

## Features

### Watchlists
- **Hauptliste** mit 30 vordefinierten Aktien und ETFs — kuratiert nach den Kriterien Marktkapitalisierung >1,5 Mrd. €, Average Daily Range >5%, Volumen >5 Mio. und Kurs >5 €
- **Eigene Watchlist** — beliebige Assets selbst hinzufügen mit Ticker, Name, Sektor, Börse, ADR%, Signal und Notiz
- Filterung nach Sektor, Sortierung nach allen Spalten
- **Export/Import** der eigenen Watchlist als JSON-Datei — für einfache Übertragung bei Dashboard-Updates

### Handlungs-Matrix
- Alle Assets als Karten mit farbcodierten Signalen: Kaufen / Beobachten / Halten / Meiden
- Filterbar nach Signal-Typ für schnellen Überblick
- Eigene Assets aus der persönlichen Watchlist werden mit ★ gekennzeichnet

### Chart
- Eingebettete TradingView-Charts für jeden Ticker — direkt per Klick aus der Watchlist oder Sidebar
- Vorgeladene Indikatoren: Moving Average, RSI, MACD
- Sidebar mit Kennzahlen (KGV, KBV, Beta, ADR) und visueller Handlungs-Matrix pro Asset

### KI-Update (✦)
- Vier Analyse-Modi via Claude AI:
  - **Einzelner Ticker** — aktuelle Einschätzung, Risiken, Handlungsempfehlung
  - **Meine Watchlist** — Kurzanalyse aller selbst hinzugefügten Assets
  - **Top 5 Signale** — Ranking der Kaufsignal-Titel nach Potenzial
  - **Marktlage** — aktueller Makro-Überblick, Sektor-Einschätzung

### Wissensbasis
- Wachsendes Nachschlagewerk zu Bewertungskennzahlen (KGV, KBV, EV/EBITDA, FCF Yield), Volatilitätsindikatoren (ADR, ATR, Beta, RSI, MACD), Chart-Grundlagen, Handlungs-Signalen, Strategien und Risikomanagement

---

## Erste Schritte

### Lokal öffnen
Die Datei `investment-dashboard.html` einfach im Browser öffnen.

> **Hinweis:** Der KI-Update-Button funktioniert lokal nicht (Browser-Sicherheitsrestriktionen). Für volle Funktionalität das Dashboard über GitHub Pages aufrufen oder einen lokalen Webserver starten:
> ```
> python3 -m http.server 8080
> ```
> Dann im Browser: `http://localhost:8080/investment-dashboard.html`

### GitHub Pages (empfohlen)
Wenn das Dashboard über GitHub Pages bereitgestellt wird, ist der KI-Update-Button ohne weitere Einrichtung nutzbar und der localStorage bleibt dauerhaft erhalten.

URL-Schema: `https://Stephan314.github.io/investis/investment-dashboard.html`

---

## Eigene Watchlist — Export & Import

Da der Browser-localStorage an die genaue URL gebunden ist, kann es bei Dashboard-Updates zu Datenverlust kommen. Die Export/Import-Funktion verhindert das:

1. **Vor einem Update:** In der "Meine Watchlist"-Ansicht auf **↓ Export** klicken → JSON-Datei wird gespeichert
2. **Nach dem Update:** Auf **↑ Import** klicken → JSON-Datei auswählen → Daten werden wiederhergestellt

Das Export-Format ist ein einfaches JSON-Array und kann auch manuell bearbeitet werden.

---

## Asset-Auswahlkriterien (Hauptliste)

Die 30 Titel der Hauptliste wurden nach folgenden Kriterien ausgewählt:

| Kriterium | Wert |
|---|---|
| Marktkapitalisierung | > 1,5 Mrd. € |
| Average Daily Range (ADR) | > 5% |
| Durchschnittliches Tagesvolumen | > 5 Mio. |
| Mindestkurs | > 5 € |

Abgedeckte Sektoren: Halbleiter, Tech, Fintech, Biotech/Health, Energie, Rüstung, Konsum, Mobilität, ETF

---

## Enthaltene Kennzahlen

| Kennzahl | Beschreibung |
|---|---|
| ADR% | Average Daily Range — durchschnittliche Tagesbewegung in % |
| KGV | Kurs-Gewinn-Verhältnis |
| KBV | Kurs-Buchwert-Verhältnis |
| Beta | Marktkorrelation und Volatilitätsmaß |
| Signal | Handlungsempfehlung: Kaufen / Beobachten / Halten / Meiden |

---

## Technologie

- Reines HTML/CSS/JavaScript — keine Abhängigkeiten, kein Build-Prozess
- TradingView Widget API für eingebettete Charts (kostenlos, kein Account nötig)
- Anthropic Claude API für KI-Analysen
- localStorage für persistente Datenspeicherung der eigenen Watchlist

---

## Roadmap

Das System ist als wachsendes Werkzeug konzipiert. Geplante Erweiterungen:

- [ ] Live-Kurse via Yahoo Finance oder Alpha Vantage API
- [ ] Automatische Kennzahlen-Aktualisierung (KGV, Beta, ADR)
- [ ] Portfolio-Tracker mit Einstiegspreisen und P&L
- [ ] News-Feed pro Ticker
- [ ] Backtesting einfacher Strategien
- [ ] Mobile-optimierte Ansicht

---

## Hinweise

- Alle Kursdaten und Kennzahlen in der Hauptliste sind statische Richtwerte (Stand Mai 2025) und dienen nur als Ausgangspunkt
- Das Dashboard ersetzt keine professionelle Finanzberatung
- ADR-Werte variieren täglich — zur Live-Überprüfung: TradingView Screener mit ATR%-Filter

---

*Entwickelt mit Claude (Anthropic) · Persönliches Projekt · Kein kommerzieller Einsatz*

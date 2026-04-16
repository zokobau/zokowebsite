<!-- From: /Users/albinsalihu/Downloads/zokowebsite-master/AGENTS.md -->
# Zoko Bauunternehmen Website — Agent Guide

> AI-Agenten, die an diesem Projekt arbeiten, sollten dieses Dokument lesen, bevor sie Änderungen vornehmen.

---

## Project Overview

Dies ist die statische Marketing-Website für **Zoko Bauunternehmen** (Inhaber: Zoran Jovic), ein Handwerksbetrieb in Karlsruhe. Die Seite präsentiert Dienstleistungen (Innenausbau, Trockenbau, Fliesenlegerarbeiten, Badsanierung, Komplettrenovierung, Malerarbeiten, Entrümpelung, Umzüge), Kundenstimmen, Kontaktdaten und ein Kontaktformular. Rechtlich erforderliche Seiten (Impressum, Datenschutz) sowie eine 404-Fehlerseite sind ebenfalls enthalten.

- **Sprache der Website:** Deutsch (`lang="de"`)
- **Ziel-URL:** `https://zoko-bauunternehmen.de`
- **Rechtsform:** Einzelunternehmen

---

## Technology Stack

| Bereich | Technologie |
|---------|-------------|
| Framework | [Astro](https://astro.build/) ^5.5.0 (Static Site Generator) |
| Runtime | Node.js >= 22.12.0 |
| Styling | Tailwind CSS v3.4.19 + `@astrojs/tailwind` v6.0.2 |
| Typography | `@fontsource/inter` v5.2.8 (400/500/600/700) |
| Schriftarten | Inter (Tailwind `font-sans`) |
| Form-Backend | [Web3Forms](https://web3forms.com/) |
| Hosting / CI | Netlify |
| Modul-System | ES Modules (`"type": "module"`) |
| TypeScript | Strict Config (`astro/tsconfigs/strict`) |
| Post-Processing | Autoprefixer v10.4.27, PostCSS v8.5.8 |

---

## Project Structure

```text
/
├── public/                    # Statische Assets (favicon.ico, favicon.svg, robots.txt, sitemap-index.xml)
├── src/
│   ├── assets/                # Bilder für Astro <Image /> und direkte <img>-Verwendung
│   │   ├── hero.jpg
│   │   ├── leistung1.jpg … leistung7.jpg
│   │   ├── kunde1.jpg … kunde3.jpg
│   │   ├── uberuns.jpg
│   │   ├── uberunsneu.jpg
│   │   └── logo.jpeg
│   ├── components/            # Astro-Komponenten (.astro Dateien)
│   │   ├── About.astro        # Über uns (Inhaber-Portrait, verwendet uberunsneu.jpg)
│   │   ├── Contact.astro      # Kontaktformular (Web3Forms) + Kontaktdaten + WhatsApp-CTA
│   │   ├── CookieBanner.astro # Cookie-Consent Banner (LocalStorage)
│   │   ├── FAQ.astro          # Häufig gestellte Fragen (Accordion via <details>/<summary>)
│   │   ├── Footer.astro       # Fußzeile mit Logo, Kontakt, rechtlichen Links
│   │   ├── Header.astro       # Sticky Navigation mit Mobile-Menü und WhatsApp-Link
│   │   ├── Hero.astro         # Hero-Sektion mit Hintergrundbild und CTA-Buttons
│   │   ├── Services.astro     # Leistungskarten (7 Dienstleistungen)
│   │   ├── Testimonials.astro # Kundenstimmen (3 Testimonials mit Sternen-Bewertung)
│   │   └── WhyUs.astro        # USPs (4 Gründe für Zoko)
│   ├── layouts/
│   │   └── Layout.astro       # Root-Layout mit Meta-Tags, Schema.org, OG, Skip-Link
│   ├── pages/
│   │   ├── index.astro        # Startseite (komponiert Hero, WhyUs, Services, About, Testimonials, Contact)
│   │   ├── impressum.astro    # Impressum (Pflichtangaben gem. § 5 TMG)
│   │   ├── datenschutz.astro  # Datenschutzerklärung (DSGVO-konform)
│   │   └── 404.astro          # Fehlerseite „Seite nicht gefunden"
│   └── styles/
│       └── base.css           # Tailwind-Direktiven, CSS-Variablen, Button-Utilities
├── astro.config.mjs           # Astro-Konfiguration (Tailwind-Integration)
├── tailwind.config.mjs        # Tailwind-Konfiguration (Custom Colors, Inter Font, Typography)
├── tsconfig.json              # TypeScript (strict, extends astro/tsconfigs/strict)
├── netlify.toml               # Build-Command, Publish-Ordner, Security-Headers
├── package.json               # Dependencies & Scripts
├── .env                       # PUBLIC_WEB3FORMS_KEY (nicht im Repo committen!)
└── .gitignore                 # node_modules, dist, .env, .astro
```

### Wichtige Konventionen

- **Seiten** liegen unter `src/pages/`. Jede `.astro`-Datei wird zu einer Route.
- **Komponenten** liegen unter `src/components/` und sind reine `.astro`-Dateien ohne Framework-JSX.
- **Bilder** werden in der Regel über `astro:assets` mit `<Image />` eingebunden (siehe `Hero.astro`, `Services.astro`, `About.astro`, `Testimonials.astro`).
- **Ausnahme beim Bild-Handling:** Das Logo in `Header.astro` und `Footer.astro` wird mit einem klassischen `<img>`-Tag referenziert (`src={logoImg.src}`), weil es aus einem `import` aus `src/assets/` stammt, aber nicht über die `<Image />`-Komponente gerendert wird.
- **Client-seitige Interaktivität** wird mit Inline-`<script>`-Tags in den Komponenten realisiert (Mobile-Menü, Formular-Validierung, Cookie-Banner).
- **CSS-Custom-Properties** für das Farbschema in `src/styles/base.css`:
  - `--color-primary: 196 164 78` (Gold)
  - `--color-dark: 35 35 35` (Fast-Schwarz)
  - `--color-light: 255 255 255` (Weiß)
- **Tailwind-Utility-Klassen** für Buttons in `src/styles/base.css`:
  - `btn-primary` (gefüllt, primary bg)
  - `btn-secondary` (outline, primary border)

---

## Build and Development Commands

Alle Befehle werden im Projektroot ausgeführt:

```bash
# Abhängigkeiten installieren
npm install

# Entwicklungsserver starten (localhost:4321)
npm run dev

# Produktionsbuild erzeugen (Output: ./dist/)
npm run build

# Build lokal previewen
npm run preview

# Astro-CLI-Befehle
npm run astro -- --help
```

> **Wichtig:** Das Projekt hat **keine Test-Suite** eingerichtet. Änderungen sollten manuell im Browser und via `npm run build` auf Build-Fehler geprüft werden.

---

## Deployment

- **Plattform:** Netlify
- **Build-Command:** `npm run build`
- **Publish-Ordner:** `dist`
- Konfiguration in `netlify.toml`
- Security-Headers werden von Netlify gesetzt:
  - `X-Frame-Options: DENY`
  - `X-Content-Type-Options: nosniff`
  - `Referrer-Policy: strict-origin-when-cross-origin`
  - `Permissions-Policy` (restriktiv)
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
  - `Content-Security-Policy` (CSP mit self, unsafe-inline für Scripts/Styles, api.web3forms.com für Formular)
- Cache-Control für Assets: `public, max-age=31536000, immutable`

Bei Änderungen an externen Ressourcen (z. B. neues Script-CDN) muss die `Content-Security-Policy` in `netlify.toml` angepasst werden.

---

## Code Style Guidelines

1. **Sprache:** Website-Inhalt und rechtliche Texte müssen auf **Deutsch** verfasst sein. Code-Kommentare können Englisch oder Deutsch sein.

2. **Zugänglichkeit (A11y):**
   - Jede `<section>` hat ein `aria-labelledby`, das auf eine Überschrift verweist.
   - Der Header enthält einen Skip-Link (`Zum Hauptinhalt springen`).
   - Bilder haben aussagekräftige `alt`-Texte auf Deutsch.
   - Interaktive Elemente haben `aria-label`, `aria-expanded`, `aria-controls` etc.
   - Das Mobile-Menü ist ein Dialog mit `role="dialog"` und `aria-modal="true"`.

3. **Bilder:**
   - Verwende `import { Image } from 'astro:assets';` für Bilder aus `src/assets/`, wo möglich.
   - Gebe `width` und `height` an, um Layout-Shifts zu vermeiden.
   - Nutze `loading="eager"` für Above-the-Fold-Bilder (Hero, Logo), `loading="lazy"` für andere.

4. **Formulare:**
   - Client-seitige Validierung ist Pflicht (siehe `Contact.astro`).
   - Felder benötigen korrekte `<label for="…">`-Verknüpfungen.
   - Fehlermeldungen haben `role="alert"`.
   - Das Formular enthält ein Honeypot-Feld (`name="botcheck"`).

5. **Tailwind:**
   - Vermeide Arbitrary-Werte, wo es Standard-Utilities gibt.
   - Erweitere das Design-System bei Bedarf in `tailwind.config.mjs` oder `src/styles/base.css`.
   - Farben werden über CSS-Variablen definiert und in Tailwind als `rgb(var(--color-primary) / <alpha-value>)` verwendet.

6. **TypeScript:**
   - Strict Mode ist aktiviert.
   - Props in Komponenten sollten mit `interface Props { … }` typisiert werden.

---

## Testing Instructions

- Es gibt aktuell **keine automatisierten Tests** (kein Jest, Vitest, Playwright etc.).
- Prüfe Änderungen manuell:
  1. `npm run dev` → visuelle Kontrolle auf Desktop & Mobile
  2. `npm run build` → muss fehlerfrei durchlaufen
  3. `npm run preview` → finale Prüfung der gebauten Seite
- Besondere Aufmerksamkeit auf:
  - Korrekte deutsche Rechtschreibung in rechtlichen Texten
  - Funktionalität des Cookie-Banners (LocalStorage Key: `zoko_cookie_consent`)
  - Formularvalidierung und WhatsApp-Link
  - Mobile Navigation (Hamburger-Menü)
  - Bild-Optimierung und Ladeverhalten

---

## Security & Privacy

- **`.env`** ist in `.gitignore` gelistet. Der Schlüssel `PUBLIC_WEB3FORMS_KEY` darf nie committet werden.
- Das Kontaktformular enthält ein Honeypot-Feld (`name="botcheck"`) zur Spam-Abwehr.
- Externe Links (WhatsApp, Netlify Privacy Policy) verwenden `target="_blank" rel="noopener noreferrer"`.
- Das Cookie-Banner speichert die Entscheidung im `localStorage` unter dem Key `zoko_cookie_consent`.
- Rechtliche Seiten (Impressum, Datenschutz) müssen mit dem aktuellen Stand der DSGVO / TMG übereinstimmen.
- Das Kontaktformular sendet Daten an `https://api.web3forms.com/submit` (muss in CSP erlaubt sein).

---

## Environment Variables

| Variable | Beschreibung |
|----------|--------------|
| `PUBLIC_WEB3FORMS_KEY` | API-Key für Web3Forms (im Kontaktformular verwendet). Muss in `.env` gesetzt sein. |

Beispiel `.env`:
```
PUBLIC_WEB3FORMS_KEY=your_web3forms_key_here
```

---

## Component Architecture

### Layout-Komponente (`src/layouts/Layout.astro`)
- Root-Layout für alle Seiten
- Enthält `<!DOCTYPE html>`, `<head>` mit Meta-Tags, Schema.org JSON-LD, Open Graph, Twitter Cards
- Importiert globale Styles (`base.css`) und Schriftarten (Inter)
- Rendert Header, Footer und CookieBanner
- Enthält Skip-Link für Accessibility

### Seiten (`src/pages/`)
- `index.astro`: Startseite, komponiert die Sektionen Hero, WhyUs, Services, About, Testimonials, Contact
- `impressum.astro`: Impressum mit rechtlichen Pflichtangaben
- `datenschutz.astro`: Datenschutzerklärung
- `404.astro`: Fehlerseite mit Link zurück zur Startseite

### Sektions-Komponenten (`src/components/`)
| Komponente | Zweck | Besonderheiten |
|------------|-------|----------------|
| `Header.astro` | Navigation | Sticky, Mobile-Menü mit Toggle, WhatsApp-Link, Logo per `<img>` |
| `Hero.astro` | Hero-Sektion | Hintergrundbild mit Gradient, CTA-Buttons, `<Image loading="eager" />` |
| `WhyUs.astro` | USPs | 4 Karten mit SVG-Icons |
| `Services.astro` | Leistungen | 7 Karten mit Bildern, Hover-Effekte, eager/lazy Loading |
| `About.astro` | Über uns | Inhaber-Portrait (uberunsneu.jpg) mit Text |
| `Testimonials.astro` | Kundenstimmen | 3 Testimonials mit Projekt-Bildern und Sternen-Bewertung |
| `Contact.astro` | Kontakt | Formular (Web3Forms) + Kontaktdaten + WhatsApp-Button |
| `FAQ.astro` | FAQ | Native `<details>`/`<summary>` Accordion (aktuell nicht auf der Startseite eingebunden) |
| `Footer.astro` | Fußzeile | Logo, Adresse, Telefon, Links zu rechtlichen Seiten |
| `CookieBanner.astro` | Cookies | LocalStorage-basiert, Opt-in für zukünftiges Tracking |

---

## Schema.org Struktur

Die Website enthält ein Schema.org `LocalBusiness` JSON-LD Objekt im Layout:
- `@type`: LocalBusiness
- `name`: Zoko Bauunternehmen
- `founder`: Zoran Jovic
- `address`: Marienstr. 61, 76137 Karlsruhe, DE
- `telephone`: +49 157 56463902, +49 176 47314186
- `email`: kontakt@zoko-bauunternehmen.de
- `areaServed`: Karlsruhe
- `hasOfferCatalog`: 7 Handwerksleistungen (Innenausbau & Trockenbau, Fliesenlegerarbeiten, Badsanierung, Komplettrenovierung, Malerarbeiten, Entrümpelung, Umzüge)

Dies verbessert die SEO und die Darstellung in Suchmaschinen.

---

## Known TODOs / Open Points

Suche nach `TODO` im Code, um offene Punkte zu finden. Aktuell bekannte:

1. **Impressum** (`src/pages/impressum.astro`):
   - `<!-- TODO: USt-IdNr. einfügen -->` — Die Umsatzsteuer-Identifikationsnummer fehlt noch (aktuell `[Wird nachgereicht]`).

---

## Quick Reference

| Willst du … | Dann tu … |
|-------------|-----------|
| Eine neue Seite hinzufügen | `.astro`-Datei unter `src/pages/` anlegen und `Layout` importieren |
| Ein neues Bild einbinden | Bild nach `src/assets/` kopieren und mit `<Image />` aus `astro:assets` verwenden |
| Einen neuen Button-Stil ergänzen | In `src/styles/base.css` innerhalb von `@layer components` definieren |
| Die Farbpalette ändern | CSS-Variablen in `base.css` und `tailwind.config.mjs` anpassen |
| Das Formular-Backend wechseln | `action`-Attribut und ggf. Script in `Contact.astro` ändern, CSP in `netlify.toml` anpassen |
| Sicherheits-Headers ändern | `netlify.toml` bearbeiten |
| Neue Schriftarten hinzufügen | `@fontsource/...` installieren, in `Layout.astro` importieren, in `tailwind.config.mjs` ergänzen |
| FAQ auf der Startseite anzeigen | `FAQ` in `src/pages/index.astro` importieren und rendern |

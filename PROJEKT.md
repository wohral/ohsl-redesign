# OHSL Redesign 2026 — Projektová dokumentace

## Co je to
Redesign webu ohsl.cz (Olomouc Hockey Super League — amatérská hokejová liga).
Mobile-first, Livesport-styl, výstup jsou HTML/CSS soubory přenositelné do Nette + Latte.

**Server:** `python3 -m http.server 4173` → http://localhost:4173

---

## Technologie
- Vanilla HTML5 / CSS3 / JS (žádný framework, žádná závislost)
- Font: Open Sans (Google Fonts, 400/500/600/700)
- Grid: vlastní 12sloupcový CSS Grid, třídy shodné s Bootstrap (`.row`, `.col-md-8` atd.)
- Latte-ready: třídní konvence = Bootstrap → přenese se beze změn

---

## Barvy (CSS proměnné)

```css
--brand:#d8241a    /* červené logo OHSL */
--brand-dk:#b51d14 /* hover */
--nav:#33424c      /* tmavě šedá navigace */
--head:#28407f     /* navy – nadpisy, odkazy */
--tx:#14181d       /* hlavní text */
--mu:#566069       /* muted / popisky */
--bg:#f1f3f6       /* pozadí stránky */
--s1:#ffffff       /* panely, karty */
--s2:#eaedf2       /* hover, alternating rows */
--line:#dde1e8     /* oddělovače, ohraničení */
--win:#1c8f4e      /* výhra */
--loss:#cf2b20     /* prohra */
--radius:14px
--maxw:1120px
--gap:16px
```

---

## Grid systém

```
.container      → max-width 1120px, padding 0 16px
.row            → CSS grid, 12 sloupců, gap 16px
[class*="col-"] → výchozí = plná šířka (xs mobile)
.col-6          → span 6 (na všech breakpointech)
.vstack         → flex column, gap 16px
@media ≥768px:
  .col-md-3/4/6/8/12
```

Typické rozvržení:
```html
<div class="row">
  <div class="col-md-8"> … hlavní obsah … </div>
  <div class="col-md-4"> … sidebar (tabulky) … </div>
</div>
```

---

## Klíčové CSS třídy

### Strukturální
| Třída | Popis |
|---|---|
| `.site-header` | Bílá hlavička (logo + loga partnerů) |
| `.partners-inline` | Řada log partnerů (skrytá na mobilu) |
| `.mainnav` | Sticky tmavá navigace |
| `.hero` | Hero carousel (position:relative) |
| `.hero-track` | Flex obálka pro snímky |
| `.hero-pill` | Červený štítek "10. sezóna" |
| `.hero-dots` | Tečky navigace carouselu |
| `.hero-arrow.prev/.next` | Šipky hero carouselu |

### Zápasy
| Třída | Popis |
|---|---|
| `.matches-module` | Bílá karta s taby Výsledky/Program |
| `.mm-tabs` | Tab row |
| `.tab-btn.active` | Aktivní tab (červená spodní linka) |
| `.mm-pane` | Skrytý panel (`.active` = viditelný) |
| `.mm-carousel` | Obal pro horizontální carousel |
| `.car-track` | Flex scrollovatelná řada karet |
| `.car-arrow.prev/.next` | Šipky (skryté na mobilu) |
| `.result-card` | Karta výsledku (šířka 230px v carouselu) |
| `.result-row` | Řádek tým+skóre |
| `.result-row.loser` | Prohravší tým (muted) |
| `.match-card` | Karta budoucího zápasu (šířka 248px) |
| `.car-more` | Dlaždice "Zobrazit vše" |

### Tabulky / statistiky
| Třída | Popis |
|---|---|
| `.panel` | Bílá karta s ohraničením |
| `.panel-head` | Hlavička panelu (h2 + hint) |
| `.panel-foot` | Patička panelu (odkaz) |
| `.stat-table` | Tabulka výsledků/bodování |
| `.team-cell` | Logo + jméno týmu vedle sebe |
| `.tlogo` | Čtvercová ikona loga týmu (28×28px) |
| `.stat-table tr.q` | Modrý levy accent = play-off pozice |
| `.stat-table tr.rel` | Červený levy accent = sestup |

### Aktuality
| Třída | Popis |
|---|---|
| `.article` | Blokový odkaz aktuality |

---

## Adresářová struktura

```
/Volumes/Projects/ohsl/
├── index.html              ✅ Homepage (dokončeno)
├── zapas.html              🔲 Detail zápasu
├── tabulka.html            🔲 Plná tabulka
├── bodovani.html           🔲 Kanadské bodování
├── tymy.html               🔲 Přehled týmů + soupiska
├── assets/
│   ├── css/styles.css      ✅ Sdílený stylesheet
│   └── img/
│       ├── ohsl-logo.png   (376×145)
│       ├── ohsl-foot.png   (218×211)
│       ├── hero.jpg / hero-2.jpg / hero-3.jpg  (1170×300)
│       ├── partners/
│       │   ├── hanacka-sportovni.jpg
│       │   ├── iph.jpg
│       │   ├── olomoucky-denik.png
│       │   └── ddsport.jpg
│       └── teams/
│           ├── kaceri-litovel.png
│           ├── hc-61-olomouc.png
│           ├── beerboys-prostejov.png
│           ├── hanacka-legie.png
│           ├── hrebci-skrben.png
│           ├── old-boys.png
│           ├── oriflame-software.png
│           ├── medojedi-chvalkovice.png
│           ├── stars-valec.png
│           ├── kohoti-olomouc.png
│           └── jesteri-twin.png
└── .claude/launch.json     (python3 http.server 4173)
```

---

## Stav redesignu

| Stránka | Soubor | Stav |
|---|---|---|
| Homepage | index.html | ✅ Hotovo |
| Detail zápasu | zapas.html | 🔲 Příště |
| Plná tabulka | tabulka.html | 🔲 Příště |
| Kanadské bodování | bodovani.html | 🔲 Příště |
| Týmy + soupiska | tymy.html | 🔲 Příště |

---

## Reálná data webu (ohsl.cz — bez vymyšlených metrik)

**Tabulka:** #, Klub, Z (zápasy), V (výhry), R (remízy), P (prohry), Skóre, B (body)
- 11 týmů, výhra = 3b, remíza = 1b

**Kanadské bodování:** #, Klub, Hráč, G (góly), A (asistence), B (body)

**Detail zápasu:**
- Záhlaví: datum, čas, místo (ZS Olomouc / PV), kolo/fáze
- Výsledková tabule: celkový výsledek + skóre po třetinách (1:0 / 1:0 / 0:1)
- Góly per tým: čas | střelec | asistent(i)
- Tresty: čas | hráč | 2 min (OHSL neuvádí typ trestu)
- Sestava: výčet jmen (bez čísel dresů v základním zobrazení)

**Soupiska týmu:** výběr sezóny; skupiny Brankář / Obránce / Útočník
- Sloupce: Číslo, Jméno, ø, ZČ (záp. část), PO (play-off), G, A, B, Tresty

---

## Klíčová omezení

- Jen SVĚTLÝ režim (žádný tmavý přepínač)
- Jen REÁLNÁ data z webu (žádné vymyšlené metriky)
- Žádné zdrojové kódy — pracovat pouze z veřejného webu
- Komunikace vždy česky
- Grid = 12sloupců, Bootstrap-konvence → Latte-ready

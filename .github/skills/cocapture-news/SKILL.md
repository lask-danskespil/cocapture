---
name: cocapture-news
description: 'Create, add, or generate new Danish content articles for cocapture.dk. Use when the user wants to write a new article, add news, generate indhold, tilføj nyhed, skriv artikel, or expand any page (vestforbraendingen, forskning, nyheder, teknologi) with a new section. Produces a valid HTML frame block and inserts it into the correct page, then guides the user to commit and deploy.'
argument-hint: 'Emne eller titel på den nye artikel (fx "Vestforbrændingens pilotprojekt starter 2026")'
---

# CO CAPTURE – Opret nyt indhold

Denne skill standardiserer oprettelse og tilføjelse af nye artikler/afsnit til cocapture.dk.

## Hvornår bruges denne skill

- Brugeren vil tilføje en ny nyhedsartikel til `nyheder.html`
- Brugeren vil udvide `vestforbraendingen.html`, `forskning.html` eller `teknologi.html` med et nyt afsnit
- Brugeren vil arbejde sig igennem emner fra `topics.txt`
- Brugeren siger: "skriv ny artikel", "tilføj nyhed", "generer indhold", "add news", "new article"

## Procedure

### Trin 1 – Identificér emne og side

Spørg brugeren (eller brug det medsendnte argument):
- **Hvad handler artiklen om?** (emne/titel)
- **Hvilken side skal den på?**
  - `nyheder.html` – aktuelle nyheder og milepæle
  - `vestforbraendingen.html` – dybdegående om Vestforbrændingen
  - `forskning.html` – forskning, institutioner og projekter
  - `teknologi.html` – tekniske forklaringer og metoder

Tjek `topics.txt` for forslag til næste emne, hvis brugeren ikke har et specifikt emne.

### Trin 2 – Generer HTML-afsnittet

Generer et komplet `<div class="frame">` HTML-afsnit **på dansk** ved at bruge:
- Malen i [`./assets/frame-template.html`](./assets/frame-template.html)
- Sprogtonen: saglig, informativ, dansk (ikke akademisk tung)
- Tilgængeligt faktuelt indhold – brug `<div class="highlight">` til nøgletal og vigtige pointer
- Tabeller med `.facts`-klassen til faktaoversigter
- CO₂ skrives som `CO&#x2082;` i HTML

### Trin 3 – Indsæt afsnittet i den korrekte fil

- Åbn den relevante `.html`-fil
- Tilføj det nye `<div class="frame">` **inden** `</div>` (der lukker `.main-content`) og **inden** `</main>`
- For `nyheder.html`: indsæt det **øverst** (nyeste nyhed vises først)
- For andre sider: indsæt **nederst** som en ny sektion

### Trin 4 – Opdatér topics.txt

Marker det behandlede emne i `topics.txt` ved at tilføje `✓` foran linjen (eller slet den).

### Trin 5 – Commit og deploy

```bash
git add <ændret-fil> topics.txt
git commit -m "Add article: <kort titel>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
git push
```

Push til `main` → Azure Static Web Apps deployer automatisk.

## HTML-konventioner

| Element | Klasse / tag | Hvornår |
|---------|-------------|---------|
| Artiklens container | `<div class="frame">` | Altid |
| Artiklens overskrift | `<div class="title">` | Altid |
| Fremhævet faktaboks | `<div class="highlight">` | Nøgletal, vigtige pointer |
| Faktaoversigt | `<table class="facts">` | Sammenligning, tidslinjer |
| Underoverskrifter | `<h3>` | Afsnit inden for artiklen |

## Sproglige retningslinjer

- Skriv i **nutid** og **aktiv form**: "Vestforbrændingen arbejder på..." (ikke "Det arbejdes på...")
- Undgå engelske fagtermer, når dansk findes: "kulstoffangst" frem for "carbon capture" første gang
- Brug **fed** (`<strong>`) til nøgletal og vigtige begreber
- Hvert afsnit skal kunne stå alene – forudsæt ikke, at læseren har læst de andre sider
- Hold artikler på **400–800 ord** (3–6 HTML-afsnit)

## Eksempel på færdig artikel

Se [`./assets/frame-template.html`](./assets/frame-template.html) for den fulde skabelon med alle elementer.

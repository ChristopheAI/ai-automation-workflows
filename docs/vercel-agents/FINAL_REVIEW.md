# Final review — wat deze Vercel Agents chatsessie echt betekent

Datum: 2026-06-05  
Context: finale review van de volledige chatsessie, met focus op betekenis voor Christophe / Vastpakt / BAVAST-achtige workflows.

---

## 1. Mijn scherpste oordeel

Deze sessie gaat niet in de eerste plaats over Vercel.

Ze gaat over een nieuwe manier om software te bouwen.

De verschuiving is:

```text
software als schermen waar mensen alles zelf doen
↓
software als workflows waarin AI werk voorbereidt of uitvoert
```

Dat is de kern.

Vercel is hier vooral de infrastructuurlaag die dat gemakkelijker maakt.

---

## 2. Wat is de echte boodschap?

De echte boodschap is:

> AI wordt geen extra chatbot naast uw applicatie. AI wordt een uitvoerende laag binnen uw applicatie.

Dat betekent:

```text
knop
↓
agent
↓
data ophalen
↓
analyseren
↓
tool gebruiken
↓
structured output
↓
mens keurt goed waar nodig
```

De gebruiker ziet geen magie.

De gebruiker ziet een functie die werk doet.

Voorbeeld uit de sessie:

```text
Detect anomalies
```

Dat is veel sterker dan:

```text
Chat met uw transacties
```

---

## 3. Waarom dit voor jou belangrijk is

Jij moet dit niet vertalen naar:

> Ik moet Vercel AI SDK leren.

Je moet dit vertalen naar:

> Ik kan processen waar vandaag mensen zitten te zoeken, controleren, copy-pasten of interpreteren, omzetten naar kleine digitale werkflows.

Dat raakt exact jouw sterktes:

```text
IT-achtergrond
troubleshooting
procesdenken
integraties
Excel/operationele realiteit
zien waar dingen vastlopen
```

Dit is geen puur developerverhaal.

Dit is een operator-verhaal.

---

## 4. Het belangrijkste onderscheid

Er zijn drie niveaus.

### Niveau 1: chatbot

```text
Gebruiker stelt vraag
AI antwoordt
```

Leuk, maar zwak als product.

### Niveau 2: agent

```text
Gebruiker vraagt iets
AI gebruikt tools
AI geeft structured output
```

Bruikbaar.

### Niveau 3: workflow

```text
Proces start
AI gebruikt data/tools
AI voert stappen uit
AI vraagt approval
AI logt alles
AI levert resultaat
```

Dat is waar bedrijven voor betalen.

De sessie gaat eigenlijk over niveau 3.

---

## 5. Wat is overschat in deze sessie?

### Multi-agent

Interessant, maar voor jou nu te vroeg.

Niet starten met:

```text
orchestrator agent
subagents
multi-agent architecture
```

Dat is snel te abstract.

### Self-healing infrastructure

Interessant voor grote platformen.

Maar voor jouw eerste Vastpakt-use-case is dit ruis.

### Modelkeuze

Haiku, Sonnet, Gemini, GPT: belangrijk, maar niet de kern.

De kern is niet welk model.

De kern is:

```text
welke workflow
welke data
welke output
welke approval
welke waarde
```

---

## 6. Wat is onderschat in deze sessie?

### Structured output

Dit is veel belangrijker dan het klinkt.

Waarom?

Omdat bedrijven geen mooie tekst nodig hebben.

Ze hebben objecten nodig waar software iets mee kan doen.

Bijvoorbeeld:

```json
{
  "severity": "critical",
  "row": 42,
  "issue": "Ontbrekend klantnummer",
  "reason": "Deze rij kan niet gekoppeld worden aan een klant",
  "suggestedAction": "Controleer bronbestand of CRM-export"
}
```

Dit kan je tonen in een UI.

Dit kan je exporteren.

Dit kan je laten goedkeuren.

Dit kan je loggen.

Dat is product-AI.

---

### Sandbox

Sandbox is voor jouw richting misschien belangrijker dan de agent interface zelf.

Waarom?

Omdat jouw klantrealiteit vaak niet proper is.

Die zit in:

```text
Excel
CSV
mailboxen
exports
facturen
logfiles
oude mappen
SharePoint
```

Daar past sandbox perfect bij.

Een agent die bestanden kan inspecteren, zoeken, vergelijken en samenvatten is veel dichter bij echte KMO-realiteit dan een agent die alleen een nette API aanspreekt.

---

### Approvals

Approvals zijn commercieel belangrijk.

Ze halen angst weg.

Je zegt niet:

> AI gaat alles automatisch doen.

Je zegt:

> AI doet het controlewerk en bereidt de beslissing voor. De mens blijft eigenaar van de kritieke actie.

Dat verkoopt veel beter.

---

## 7. Wat betekent dit voor Vastpakt?

Vastpakt moet niet gepositioneerd worden als:

```text
AI agency
chatbotbouwer
prompt engineer
```

Maar als:

```text
workflowbouwer
proces-vastpakker
controlelaag bovenop bestaande systemen
AI + IT + operations
```

De zin die blijft staan:

> Ik help bedrijven waar cruciale kennis in Excel, mailboxen of één hoofd zit, om daar een controleerbare workflow van te maken.

Nog platter:

> Waar vandaag iemand manueel zit te controleren, bouwen we een systeem dat de afwijkingen toont.

---

## 8. Wat is de eerste use-case?

Niet twijfelen.

De eerste use-case is:

# Excel/CSV Controle Agent

Waarom?

Omdat dit:

```text
herkenbaar is
klein genoeg is
waardevol is
dicht bij BAVAST ligt
past bij sandbox
duidelijke output heeft
makkelijk demo-baar is
```

MVP:

```text
1. Upload CSV/Excel
2. Klik: Controleer bestand
3. Agent inspecteert bestand
4. Agent vindt afwijkingen
5. UI toont controleverslag
6. Mens valideert
```

Geen chat.

Een knop.

Een verslag.

Een waarde.

---

## 9. Wat moet die eerste demo tonen?

De demo moet tonen:

```text
3 kritieke fouten
7 waarschuwingen
12 aandachtspunten
```

Per issue:

```text
rij
kolom
ernst
probleem
waarom dit een probleem is
voorgestelde actie
```

Voorbeeld:

```text
Rij 42
Kolom: klantnummer
Ernst: kritiek
Probleem: klantnummer ontbreekt
Waarom: factuur kan niet gekoppeld worden aan klant
Actie: controleer CRM-export of bronbestand
```

Dat snapt een klant meteen.

---

## 10. Wat is de verkoopbare belofte?

Niet:

> Wij gebruiken AI.

Wel:

> Wij vinden sneller wat afwijkt in uw bestanden of processen.

Niet:

> Een agent analyseert uw data.

Wel:

> Minder fouten, minder manueel zoekwerk, minder afhankelijkheid van één persoon.

Niet:

> Innovatieve AI-oplossing.

Wel:

> Een controleverslag in plaats van uren zoeken in Excel.

---

## 11. Wat is de technische kern voor de eerste versie?

Voor de eerste versie heb je nodig:

```text
Next.js app
file upload
API route
AI SDK agent call
sandbox/file analyse
Zod schema voor structured output
UI voor controleverslag
```

Nog niet nodig:

```text
multi-agent
complexe workflows
enterprise billing
model marketplace
self-healing infra
MCP setup
```

---

## 12. Wat is de juiste architectuurdenkwijze?

Niet starten vanuit tools.

Start vanuit procespijn.

Vragen:

```text
Waar zit vandaag het manuele controlewerk?
Wie weet hoe het bestand werkt?
Wat gebeurt er als die persoon wegvalt?
Welke fouten worden laat ontdekt?
Welke export wordt elke week opnieuw nagekeken?
Welke beslissing wordt telkens opnieuw gemaakt?
```

Daarna pas:

```text
welke data?
welke tool?
welke agent?
welke output?
welke approval?
```

---

## 13. Wat is de hardste conclusie?

De kans voor jou zit niet in:

```text
AI uitleggen
Vercel verkopen
agents hypen
```

De kans zit in:

```text
één pijnlijk proces vinden
één kleine workflow bouwen
één duidelijk resultaat tonen
```

Dat resultaat moet zijn:

```text
tijdswinst
minder fouten
minder afhankelijkheid
sneller inzicht
```

---

## 14. Wat moet jij onthouden uit heel deze sessie?

Als je maar één ding onthoudt:

> De beste AI-oplossing voelt voor de klant niet als AI, maar als een knop die eindelijk het vervelende werk doet.

Voor Vastpakt:

> Geen chatbot. Een controlelaag.

Voor BAVAST-achtig werk:

> Geen Excel vervangen. Eerst Excel controleerbaar maken.

Voor verkoop:

> Niet praten over agents. Praten over afwijkingen vinden, fouten vermijden en kennis uit één hoofd halen.

---

## 15. Finale zin

> Vercel Agents zijn interessant omdat ze tonen hoe AI van tekstgenerator naar uitvoerende workflowlaag evolueert; voor Christophe is de juiste eerste toepassing geen multi-agent platform, maar een simpele Excel/CSV-controle-agent die manueel controlewerk zichtbaar, sneller en betrouwbaarder maakt.

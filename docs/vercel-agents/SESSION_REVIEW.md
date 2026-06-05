# Review — Vercel Agents chatsessie

Datum: 2026-06-05  
Context: review van de informatie uit de ChatGPT-sessie over Vercel Agents, AI SDK v6, sandboxing, workflows, observability en wat Christophe/Vastpakt hiermee kan.

---

## Korte conclusie

De bestaande README vat de grote lijn goed samen, maar is nog te veel een nette samenvatting.

Wat er extra nodig was:

1. scherper onderscheid tussen demo-AI en product-AI;
2. expliciete keuzecriteria: wanneer tool call, wanneer sandbox, wanneer workflow;
3. duidelijker maken waarom dit verkoopbaar is voor Vastpakt;
4. beter benoemen dat agents vooral complexiteit uit klassieke software halen;
5. concretere eerste bouwrichting.

De kernzin blijft:

> Geen chatbot. Een workflow die werk overneemt.

Maar de echte onderliggende gedachte is scherper:

> Een agent wordt pas waardevol wanneer hij binnen een bestaand bedrijfsproces data kan lezen, tools kan gebruiken, beslissingen kan voorbereiden en gecontroleerd acties kan uitvoeren.

---

## Wat uit de sessie goed gedocumenteerd staat

De README dekt deze onderdelen correct:

- AI SDK v6 combineert structured output en tool calls beter in één agent-run.
- De oude Agent Class was te rigide; de nieuwe agent interface/abstraction is flexibeler.
- De Tool Loop Agent is conceptueel: denken → tool → denken → output.
- Dynamic call options maken het mogelijk om context, tools, model en prompt te wijzigen per gebruiker/plan/klanttype.
- Human-in-the-loop approvals zijn essentieel voor risicovolle acties.
- Sandbox is nuttig voor file system context, CSV/logs en contextnavigatie.
- Vercel AI Gateway maakt modelkeuze, kosten, usage en fallback beter beheersbaar.
- Workflows zorgen voor durability bij lange agentprocessen.
- Observability is nodig om agents production-grade te maken.
- Skills worden belangrijker dan losse prompts.
- Vastpakt/BAVAST-use-cases zijn logisch: Excel/CSV-controle, factuurcontrole, IT support, intake en proposals.

---

## Wat nog scherper moest

### 1. De presentatie ging niet over agents als chatbot

De demo gebruikt een banktransactie-app met een knop:

```text
Detect anomalies
```

Dat is belangrijk.

De gebruiker voert geen lang gesprek met AI.

De gebruiker gebruikt een gewone applicatieknop.

De AI zit onder de motorkap.

Dat is de grote shift:

```text
Niet: chat met mijn data.
Wel: klik op een functie die werk uitvoert.
```

Voor Vastpakt betekent dit:

> Bouw geen AI-chatbot voor klanten. Bouw functies die concrete operationele pijn wegnemen.

---

### 2. Tool call en sandbox zijn geen concurrenten

De sessie maakte duidelijk dat er niet één beste keuze is.

Gebruik een gewone tool call wanneer:

```text
data beperkt is
API duidelijk is
schema vastligt
snelheid belangrijk is
```

Gebruik sandbox wanneer:

```text
data in bestanden zit
context groot of rommelig is
CSV/logs/exports geanalyseerd moeten worden
agent zelf gericht moet zoeken
minder prompt-context nodig is
```

Voorbeeld:

```text
getTransactions(userId)
```

is prima voor gestructureerde database-data.

Maar:

```text
upload facturen.csv
upload export_2026_06.xlsx
upload auditlogs.txt
```

vraagt eerder sandbox-contextnavigatie.

---

### 3. Het belangrijkste architectuurpunt: LLM vervangt conditionele spaghetti

Een sterk punt uit de sessie:

Vroeger bouwde je veel beslissingslogica met code:

```text
if customerType == enterprise
  if topic == firewall
    if sla == premium
      route naar workflow X
```

Dat wordt snel duur, fragiel en moeilijk onderhoudbaar.

Met agents:

```text
vraag/context
↓
LLM begrijpt intentie
↓
LLM kiest tool of sub-agent
↓
workflow loopt
```

De LLM vervangt niet alle businesslogica.

Maar de LLM kan wel veel routing, intentieherkenning en toolkeuze eenvoudiger maken.

Voor Christophe is dit belangrijk omdat hij vaak vertrekt vanuit processen:

```text
Wie doet wat?
Waar loopt het vast?
Welke kennis zit bij één persoon?
Welke beslissing wordt telkens opnieuw manueel gemaakt?
```

Dat zijn exact de plekken waar agents waarde krijgen.

---

### 4. Product-AI vraagt meer dan een model

De sessie ging veel over de laag rond het model.

Production-grade agents hebben nodig:

```text
frontend
API route
runtime
model gateway
sandbox
workflow engine
observability
approval flows
billing / cost visibility
authentication
deployment
alerting
```

Een demo zonder deze laag is niet genoeg voor bedrijven.

Voor Vastpakt is de verkoopboodschap dus niet:

> Ik zet AI op uw proces.

Maar:

> Ik maak een controleerbare workflow rond uw proces, met AI waar dat zinvol is.

---

### 5. De AI Gateway is eigenlijk een businessmodel-enabler

In de sessie kwam naar voren:

- vandaag Haiku/Sonnet;
- morgen Gemini;
- later GPT/Mistral/Llama;
- free users kunnen goedkoper model krijgen;
- paid users kunnen sterker model krijgen;
- enterprise kan fallback, audit en SLA krijgen;
- bring-your-own-key is mogelijk.

Dat betekent:

```text
modelkeuze wordt configuratie
niet architectuur
```

Voor een product of klantoplossing is dat belangrijk omdat je prijs/kwaliteit kan sturen per klanttype.

---

### 6. Workflows zijn de brug naar echte bedrijfsprocessen

Een agent die één antwoord geeft is simpel.

Een agent die 50 stappen uitvoert is anders.

Zonder durability:

```text
stap 49 faalt → alles opnieuw
```

Met workflows:

```text
stap 1-48 blijven bewaard
stap 49 wordt opnieuw geprobeerd
```

Voor bedrijfsprocessen is dat cruciaal.

Voorbeelden:

```text
factuurcontrole
klantdossieranalyse
M365 recovery procedure
Excel/CSV validatie
proposal generatie
```

Deze processen mogen niet volledig opnieuw starten bij één fout.

---

## Wat dit betekent voor Vastpakt

De beste vertaling is:

> Vastpakt bouwt geen AI-chatbots. Vastpakt bouwt kleine digitale werkprocessen die manueel werk, controlewerk en persoonsafhankelijke kennis omzetten naar een systeem.

Scherpere positionering:

> Ik help bedrijven waar cruciale kennis in Excel, mailboxen of één hoofd zit, om daar een controleerbare workflow van te maken.

Of:

> Ik bouw AI-gestuurde controleflows voor processen die vandaag te manueel, te foutgevoelig of te afhankelijk van één persoon zijn.

---

## Beste eerste use-case

De eerste use-case moet niet te breed zijn.

Aanbevolen:

# Excel/CSV Controle Agent

Waarom deze?

- dicht bij BAVAST;
- makkelijk demo-baar;
- herkenbaar voor klanten;
- goede match met sandbox;
- output kan structured zijn;
- waarde is direct: fouten, tijd, afhankelijkheid.

MVP:

```text
Upload CSV/Excel
↓
Agent leest bestand in sandbox
↓
Agent zoekt afwijkingen
↓
Agent geeft controleverslag
↓
Mens valideert
```

Controlepunten:

```text
ontbrekende velden
dubbele records
rare bedragen
lege waarden
afwijkingen tegenover vorige export
inconsistenties tussen kolommen
mogelijk risico
voorgestelde actie
```

Output schema:

```json
{
  "summary": "string",
  "criticalCount": 0,
  "warningCount": 0,
  "issues": [
    {
      "row": 12,
      "field": "amount",
      "severity": "critical",
      "issue": "Ongebruikelijk hoog bedrag",
      "reason": "Bedrag ligt 8x hoger dan mediaan",
      "suggestedAction": "Controleer bronfactuur of tariefafspraak"
    }
  ]
}
```

UI-knoppen:

```text
Upload bestand
Controleer bestand
Toon issues
Download controleverslag
```

---

## Wat je niet moet bouwen als eerste

Niet starten met:

```text
volledige multi-agent architectuur
volledige M365 recovery agent
alles-in-één Vastpakt intake platform
enterprise workflow engine
```

Dat is te groot.

Eerste doel:

> Eén herkenbaar probleem, één bestand, één controleverslag, één duidelijke waarde.

---

## Beslisboom voor implementatie

```text
Is de data via een duidelijke API beschikbaar?
→ gewone tool call

Zit de data in CSV/Excel/logs/bestanden?
→ sandbox

Moet het proces uit meerdere stappen bestaan?
→ workflow

Kan een actie schade veroorzaken?
→ approval verplicht

Moet de klant kunnen kiezen tussen goedkoop/snel of sterk model?
→ AI Gateway / dynamic model choice

Moet je kunnen uitleggen wat de agent deed?
→ observability verplicht
```

---

## Wat later in code moet komen

Minimale projectstructuur voor een prototype:

```text
app/
  page.tsx
  api/
    analyze/route.ts
lib/
  agents/
    file-control-agent.ts
  schemas/
    control-report.schema.ts
  tools/
    sandbox-tools.ts
  prompts/
    control-agent.prompt.ts
docs/
  vercel-agents/
```

Eerste technische flow:

```text
1. gebruiker uploadt CSV
2. bestand wordt tijdelijk beschikbaar gemaakt
3. agent krijgt sandbox/file access
4. agent analyseert via commands/context
5. agent geeft structured output
6. UI toont controleverslag
7. gebruiker kan verslag exporteren
```

---

## Wat als verkoopverhaal moet blijven hangen

Niet:

> Wij gebruiken AI.

Wel:

> Wij halen manuele controle uit uw proces zonder de menselijke eindbeslissing weg te nemen.

Niet:

> Een chatbot voor uw bedrijf.

Wel:

> Een digitale controlelaag bovenop uw bestaande Excel, export of workflow.

Niet:

> Automatisatie om automatisatie.

Wel:

> Minder fouten, minder afhankelijkheid, sneller zicht op wat afwijkt.

---

## Eindbeoordeling van de sessie-informatie

De informatie uit de chatsessie is waardevol omdat ze drie dingen samenbrengt:

1. technische evolutie: AI SDK v6, agents, tools, sandbox, workflows;
2. productdenken: preview deploys, observability, gateway, approvals;
3. Vastpakt-richting: concrete workflows bouwen rond processen die vandaag vastlopen.

De juiste conclusie is:

> De kans zit niet in "AI bouwen". De kans zit in operationele workflows vinden waar AI een gecontroleerde uitvoerder kan worden.

Dat is exact waar Christophe/Vastpakt sterk op kan positioneren.

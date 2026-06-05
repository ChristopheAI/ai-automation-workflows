# Vercel Agents — notities uit ChatGPT-sessie

Datum: 2026-06-05  
Repo: `ai-automation-workflows`  
Doel: de inzichten uit de chatsessie documenteren zodat ze later bruikbaar zijn als basis voor demo's, prototypes en klantgerichte Vastpakt-use-cases.

---

## Waarom deze documentatie bestaat

Deze sessie ging niet gewoon over "Vercel AI SDK v6".

De echte reden om dit vast te leggen:

> AI verschuift van losse chatbots naar gecontroleerde workflows waarin agents data lezen, tools gebruiken, beslissingen voorbereiden en acties uitvoeren binnen echte applicaties.

Voor Christophe / Vastpakt is dat belangrijk omdat zijn waarde niet zit in "een AI-tool tonen", maar in:

- processen begrijpen;
- manueel werk herkennen;
- afhankelijkheid van één persoon verminderen;
- Excel-, mailbox-, ERP- of operationele kennis omzetten naar controleerbare workflows;
- AI gebruiken als uitvoerder binnen een proces, niet als gimmick.

---

## Wat bouwen we hiermee?

Niet starten vanuit: "ik bouw een agent".

Wel starten vanuit:

> Welk manueel, foutgevoelig of persoonsafhankelijk proces doet vandaag pijn?

Daarna pas bepalen:

1. Welke data heeft de agent nodig?
2. Welke tools mag hij gebruiken?
3. Welke output moet hij geven?
4. Welke acties mogen automatisch?
5. Waar moet menselijke goedkeuring verplicht zijn?

---

## Basisdefinitie: wat is een agent?

Een agent is geen magie.

Een agent is:

```text
LLM
+ instructie
+ tools
+ data/context
+ output schema
+ workflow
+ logging / observability
```

Voorbeeld:

```text
Vraag: detecteer afwijkingen in transacties.
↓
Agent gebruikt tool: getTransactions(userId)
↓
Agent analyseert patroon en afwijkingen
↓
Agent geeft structured output terug
↓
UI toont lijst met risico's
```

Belangrijk: de gebruiker hoeft niet met AI te chatten.

De gebruiker ziet gewoon een knop:

```text
Detect anomalies
Controleer bestand
Zoek afwijkingen
Genereer voorstel
```

Dat is de shift:

> AI wordt een werkfunctie in de applicatie.

---

## AI SDK v6: kernpunten uit de sessie

### 1. Structured output + tool calls in één flow

Vroeger waren structured output en tool calls vaak losse stappen.

Nu kan één agent-run:

- tekst genereren;
- structured output genereren;
- tools aanroepen;
- meerdere tools gebruiken;
- output rechtstreeks bruikbaar maken voor UI.

Dat maakt AI bruikbaarer als applicatielaag.

---

### 2. Agent Interface in plaats van rigide Agent Class

Vercel had vroeger een concretere agent class.

Probleem:

- te weinig flexibel;
- te veel aannames;
- moeilijk uitbreidbaar voor echte use-cases.

AI SDK v6 introduceert meer een agent-abstraction / interface.

Daardoor kan je:

- de standaard `Tool Loop Agent` gebruiken;
- zelf een orchestrator bouwen;
- multi-agent patronen bouwen;
- agents koppelen aan UI-streams;
- type safety behouden.

---

### 3. Tool Loop Agent

De standaardimplementatie is conceptueel simpel:

```text
Gebruiker vraagt iets
↓
LLM redeneert
↓
Tool nodig?
↓
Tool uitvoeren
↓
Resultaat terug naar LLM
↓
Nog een tool nodig?
↓
Antwoord / structured output
```

Kort:

```text
denken → tool → denken → tool → antwoord
```

---

### 4. Dynamic call options

Een agent kan anders werken afhankelijk van context:

- type klant;
- userId;
- plan: free / paid / enterprise;
- rechten;
- beschikbare tools;
- system prompt;
- modelkeuze.

Voorbeeld:

```text
Free plan → goedkoop model + beperkte tools
Paid plan → sterker model + meer tools
Enterprise → audit, fallback, approval, extra observability
```

---

### 5. Human-in-the-loop approvals

Belangrijk voor productie.

Niet elke actie mag automatisch.

Voorbeelden:

```text
Factuur versturen → approval
Gebruiker blokkeren → approval
MFA reset uitvoeren → approval
Klant verwijderen → approval
Bestelling plaatsen → approval
```

De agent mag voorbereiden.

De mens beslist bij risico.

---

## Tool calls versus sandbox

### Gewone tool call

Goed voor duidelijke, beperkte data.

```text
Agent → getTransactions(userId) → analyse → output
```

Voordelen:

- snel;
- simpel;
- weinig overhead;
- goed voor duidelijke API/data.

---

### Sandbox + files

Goed voor grotere of rommelige context.

```text
Agent → sandbox → ls/cat/grep/awk → CSV analyseren → output
```

Voordelen:

- agent hoeft niet alles in prompt-context te krijgen;
- kan zelf gericht zoeken;
- nuttig voor CSV, logs, exports, documenten;
- minder token-overload;
- beter voor contextnavigatie.

Belangrijke gedachte:

> Geef de agent niet heel het archief in de prompt. Geef hem toegang tot een veilige werkruimte waar hij gericht mag zoeken.

Analogie:

```text
Niet: heel het archief printen en op tafel leggen.
Wel: toegang geven tot het archief met badge, regels en zoekfunctie.
```

---

## Vercel Sandbox

Sandbox wordt gebruikt voor:

- untrusted code;
- coding agents;
- contextnavigatie;
- file system access;
- analyse van CSV/logs/bestanden;
- veilige uitvoering van commands.

Een agent kan via bash-achtige tools zelf ontdekken:

```text
Welke bestanden zijn er?
Welke kolommen zitten in de CSV?
Wat is het gemiddelde bedrag?
Welke lijnen vallen op?
Welke afwijkingen zijn relevant?
```

Belangrijk: de agent bedenkt zelf welke commands nuttig zijn, binnen de sandboxgrenzen.

---

## D0: data-agent voorbeeld van Vercel

Vercel gebruikt intern D0 als data-agent.

Doel:

```text
Mens stelt vraag in gewone taal
↓
Agent begrijpt vraag
↓
Agent schrijft query
↓
Agent haalt data op
↓
Agent geeft antwoord
```

Waarom belangrijk:

- data-team hoeft minder repetitieve queries te schrijven;
- governance en toegangscontrole blijven mogelijk;
- collega’s krijgen sneller antwoorden;
- data-team werkt aan moeilijkere problemen.

---

## Multi-agent / sub-agents

Belangrijk inzicht:

> Een agent kan een tool zijn voor een andere agent.

Voorbeeld:

```text
Orchestrator Agent
↓
Tool: askSupportAgent()
Tool: askFinanceAgent()
Tool: askDataAgent()
Tool: askCodeAgent()
↓
Final answer / action
```

Elke sub-agent heeft eigen context en specialisatie.

Voor Vastpakt kan dat worden:

```text
Vastpakt Orchestrator
↓
IT Intake Agent
↓
Process Mapping Agent
↓
Automation Agent
↓
Proposal Agent
```

De hoofd-agent hoeft niet alles zelf te weten.

Hij moet weten:

> wie moet ik inschakelen en welke workflow hoort hierbij?

---

## Workflows en durability

Echte agents doen vaak meerdere stappen.

Voorbeeld:

```text
Tool call 1
Tool call 2
...
Tool call 50
```

Als stap 49 faalt, wil je niet alles opnieuw doen.

Durable workflows bewaren elke stap.

```text
Stap 1 ✔
Stap 2 ✔
...
Stap 49 ✔
Stap 50 ✖
```

Retry:

```text
Alleen stap 50 opnieuw uitvoeren
```

Dit is essentieel voor productie-agenten.

---

## Observability

Een agent zonder observability is gevaarlijk.

Je moet kunnen zien:

- welke agent draaide;
- welke tool calls gebeurden;
- welke sandbox commands uitgevoerd zijn;
- welk model gebruikt werd;
- hoeveel tokens verbruikt zijn;
- hoeveel het kostte;
- hoe lang het duurde;
- waar het fout liep;
- welke workflowstap faalde.

Enterprise-vraag:

> Kan ik zien wat de agent doet, wanneer hij faalt, hoeveel hij kost en of hij betrouwbaar blijft?

---

## AI Gateway

Vercel AI Gateway maakt modelkeuze dynamisch.

Vandaag:

```text
Claude Haiku / Sonnet
```

Morgen:

```text
Gemini
GPT
Mistral
Llama
```

De app hoeft niet fundamenteel te veranderen.

Mogelijkheden:

```text
Free plan → goedkoop/snel model
Paid plan → sterker model
Enterprise → fallback + SLA + audit
```

Ook relevant:

- Vercel kan modellen factureren via eigen gateway;
- bring-your-own-key blijft mogelijk;
- gateway kan helpen bij failover als een model down is;
- usage, latency, tokens en kosten worden zichtbaar.

---

## Deployment met Vercel

Vercel positioneert zich als shipping layer voor agentic apps.

Flow:

```text
code
↓
build
↓
preview deployment
↓
feedback
↓
production deployment
```

Belangrijk:

- preview URL per feature;
- feedback rechtstreeks op preview;
- GitHub/Slack-integratie;
- `vercel deploy` of GitHub integration;
- productie via `vercel deploy --prod`.

Voor AI-apps is dit belangrijk omdat agents bestaan uit meer dan code:

```text
UI
prompts
tools
models
sandbox
workflow
approval
observability
deployment
```

---

## Skills versus prompts

Een skill is:

```text
context
+ best practices
+ domeinkennis
+ werkwijze
```

voor een specifiek domein.

Voorbeeld:

```text
React skill
Vercel skill
Vastpakt skill
BAVAST skill
```

Waarom belangrijk:

> In plaats van 50 pagina's context in de prompt te duwen, laat je het model de juiste skill laden wanneer nodig.

Voor Vastpakt kan een skill bevatten:

- tone of voice;
- diensten;
- positionering;
- cases;
- intakevragen;
- proposal-structuur;
- woorden die Christophe wel/niet gebruikt.

---

## Self-healing infrastructure

Vercel denkt richting platform-as-agent.

Niet alleen:

```text
alert sturen naar mens
```

Maar:

```text
probleem detecteren
↓
deployment identificeren
↓
oorzaak tonen
↓
actie voorstellen of uitvoeren
```

Voor sommige problemen wil je geen e-mail.

Voorbeeld DDoS:

```text
Platform detecteert aanval
↓
Firewall grijpt automatisch in
```

---

## Wat kan Christophe hiermee?

### 1. BAVAST-achtige Excel/CSV controle-agent

Probleem:

```text
Eén persoon weet hoe de Excel werkt.
Fouten worden laat ontdekt.
Controle kost tijd.
Proces is persoonsafhankelijk.
```

Agent-oplossing:

```text
Upload Excel/CSV
↓
Agent leest bestand in sandbox
↓
Agent zoekt afwijkingen, ontbrekende velden, dubbele records
↓
Agent geeft controleverslag
↓
Mens valideert
```

MVP-knoppen:

```text
Upload bestand
Controleer bestand
Toon afwijkingen
Genereer verslag
```

---

### 2. Factuurcontrole-agent

Voor Somtrans / operations / finance.

Agent leest:

- facturen;
- transportbonnen;
- tarieven;
- klantafspraken;
- historiek.

Output:

```text
Deze factuur wijkt af.
Deze kost lijkt dubbel.
Deze lijn ontbreekt.
Deze leverancier rekent anders dan vorige maand.
```

Waarde:

```text
minder fouten
minder manueel zoekwerk
snellere controle
minder geldverlies
```

---

### 3. IT support / M365 recovery agent

Agent gebruikt:

- tickets;
- M365 logs;
- Entra-signalen;
- known issues;
- runbooks;
- supportprocedures.

Output:

```text
mogelijke oorzaak
volgende stap
ticketdraft
maildraft
risico-inschatting
```

Risky acties altijd met approval.

---

### 4. Vastpakt intake-agent

Geen klassiek contactformulier.

Wel:

```text
Bezoeker beschrijft probleem
↓
Agent stelt vervolgvragen
↓
Agent classificeert probleem
↓
Agent maakt korte diagnose
↓
Agent maakt intakevoorstel
```

Vragen:

```text
Waar loopt het proces vandaag vast?
Wie doet dit nu manueel?
Welke Excel of tool is cruciaal?
Wat gebeurt er als die persoon uitvalt?
Welke fout kost vandaag tijd of geld?
```

---

### 5. Proposal-agent

Agent neemt:

- gespreksnotities;
- klantprobleem;
- scope;
- uren;
- risico’s;
- verwachte winst.

Output:

- voorstel;
- scope;
- fasering;
- prijslogica;
- mail naar klant.

---

## Eerste concrete prototype

Aanbevolen eerste repo/prototype:

> Upload je Excel/CSV en krijg een controleverslag.

Functionaliteit:

```text
1. Upload CSV/Excel
2. Agent leest bestand in sandbox
3. Agent zoekt:
   - ontbrekende velden
   - dubbele records
   - rare bedragen
   - afwijkingen tegenover vorige export
   - inconsistenties
4. Output:
   - issue
   - ernst
   - reden
   - voorgestelde actie
```

UI:

```text
[Upload bestand]
[Controleer]
```

Resultaat:

```text
3 kritieke afwijkingen
7 waarschuwingen
12 kleine inconsistenties
```

---

## Positionering voor Vastpakt

Mogelijke zin:

> Ik help bedrijven waar cruciale kennis in Excel, mailboxen of één hoofd zit, om daar een controleerbaar systeem van te maken.

Of:

> Ik bouw kleine AI-gestuurde werkprocessen die Excelwerk, controles, opvolging en repetitieve beslissingen automatiseren — met menselijke goedkeuring waar het nodig is.

Of scherper:

> Geen chatbot. Een workflow die werk overneemt.

---

## Belangrijkste conclusie

Veel mensen denken:

```text
AI = chatbot
```

De richting uit deze sessie is:

```text
AI = runtime voor workflows
```

De echte stack:

```text
Agent
↓
Tools
↓
Sandbox
↓
Workflow
↓
Durability
↓
Observability
↓
Approval
↓
Deployment
```

De echte waarde:

> Niet AI toevoegen aan een proces, maar het proces herontwerpen zodat AI een uitvoerder wordt binnen dat proces.

---

## Volgende stappen

1. Kies één concrete use-case.
2. Verzamel voorbeelddata: CSV/Excel/logs/facturen.
3. Definieer output schema.
4. Bepaal welke acties approval nodig hebben.
5. Bouw MVP in Next.js + Vercel AI SDK.
6. Gebruik sandbox voor file-analyse.
7. Deploy preview op Vercel.
8. Test met echte voorbeelden.
9. Maak case-resultaat: tijdswinst, fouten, euro’s, afhankelijkheid verminderd.

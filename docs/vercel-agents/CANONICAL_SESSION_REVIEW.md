# Canonical session review — Agent bouwen met Vercel

Datum: 2026-06-05  
Doel: de informatie uit de volledige ChatGPT-sessie opnieuw beoordelen, niet als losse samenvatting, maar als bruikbare denklaag voor Christophe/Vastpakt.

---

## 1. Wat was deze sessie eigenlijk?

Op het oppervlak ging de sessie over:

```text
Vercel
AI SDK v6
agents
tool calls
sandbox
workflow development kit
AI Gateway
observability
deployment
```

Maar inhoudelijk ging ze over iets groters:

> Hoe verandert software wanneer AI niet langer alleen tekst genereert, maar mee beslist welke tool, data, workflow of actie nodig is?

De echte verschuiving is:

```text
van chatbot
naar agentische applicatie
```

En nog scherper:

```text
van software die wacht op menselijke input
naar software die werk voorbereidt of uitvoert binnen duidelijke grenzen
```

---

## 2. Wat is de rode draad door de hele sessie?

De rode draad is niet "Vercel heeft nieuwe features".

De rode draad is:

> AI wordt een runtime-laag voor bedrijfsprocessen.

Dat betekent:

```text
LLM
+ tools
+ structured output
+ files/context
+ sandbox
+ workflows
+ approvals
+ observability
+ deployment
```

Samen vormen die geen chatbot meer.

Ze vormen een systeem dat werk kan doen.

---

## 3. Wat was het eerste technische punt?

Het eerste belangrijke technische punt was:

> Structured output en tool calls komen dichter bij elkaar.

Vroeger moest je vaak rommelig werken:

```text
LLM antwoordt met tekst
↓
aparte stap voor object/JSON
↓
tool call uitvoeren
↓
opnieuw antwoord genereren
```

Dat voelde discombobulated.

Met AI SDK v6 verschuift dat naar:

```text
één agent-run
↓
tekst + tool calls + structured output
```

Voor applicaties is dat belangrijk omdat UI geen losse tekst nodig heeft, maar betrouwbare objecten.

Bijvoorbeeld:

```json
{
  "transactionId": "tx_123",
  "severity": "high",
  "reason": "Ongebruikelijk hoog bedrag",
  "suggestedAction": "Controleer bron of klantafspraak"
}
```

---

## 4. Wat was het agent-inzicht?

Vercel had vroeger een concretere Agent Class.

Die was volgens de sessie te rigide.

De richting wordt nu:

```text
Agent als interface / abstraction
```

Dat is belangrijk omdat echte use-cases niet allemaal hetzelfde zijn.

Soms wil je:

```text
simpele tool loop
```

Soms:

```text
orchestrator agent
```

Soms:

```text
multi-agent setup
```

Soms:

```text
durable workflow agent
```

De waarde zit dus niet in één vaste agentklasse, maar in een standaard waarop meerdere agenttypes kunnen conformeren.

---

## 5. Wat is een Tool Loop Agent echt?

Niet moeilijker maken dan nodig.

Een Tool Loop Agent is:

```text
LLM denkt
↓
heeft tool nodig
↓
roept tool op
↓
krijgt resultaat
↓
denkt opnieuw
↓
roept eventueel nog een tool op
↓
geeft output
```

Of kort:

```text
denken → tool → denken → tool → output
```

Dat klinkt simpel, maar dat is precies waarom het bruikbaar is.

Veel bedrijfsprocessen zijn ook zo:

```text
vraag begrijpen
informatie ophalen
controleren
beslissing voorbereiden
actie voorstellen
```

---

## 6. Waarom de demo met banktransacties belangrijk was

De banktransactie-demo is niet belangrijk omdat het over finance ging.

Ze is belangrijk omdat de gebruiker geen chatbot gebruikt.

De gebruiker ziet een knop:

```text
Detect anomalies
```

Daarachter gebeurt:

```text
API route
↓
agent
↓
tool: getTransactions()
↓
structured output
↓
UI toont anomalieën
```

Dit is cruciaal.

De sessie zegt eigenlijk:

> De beste AI-functies voelen niet altijd als AI. Ze voelen als betere software.

Voor Vastpakt betekent dat:

```text
Geen chatbot verkopen.
Wel: controleknoppen, analysefuncties, intakeflows, voorstelgenerators.
```

---

## 7. Wat was de les rond context?

Een agent wordt beter wanneer hij de juiste context krijgt.

Niet alleen:

```text
getTransactions()
```

Maar bijvoorbeeld:

```text
getTransactions(userId)
getWhitelistedVendors(userId)
getKnownBadVendors()
getPreviousFeedback(userId)
getCustomerPlan(userId)
```

De agent moet niet alleen data zien.

Hij moet weten:

```text
Voor wie is dit normaal?
Voor wie is dit afwijkend?
Welke klantcategorie is dit?
Welke rechten heeft deze gebruiker?
Welke uitzonderingen zijn bekend?
```

Dat is het verschil tussen simpele AI en bruikbare AI.

---

## 8. Dynamic call options: waarom dat belangrijk is

Dynamic call options betekenen dat de agent anders kan werken per context.

Voorbeelden:

```text
Free klant → goedkoper model, minder tools
Betaalde klant → sterker model, meer context
Enterprise klant → audit, approvals, fallback
Supportplan basic → beperkte acties
Supportplan premium → snellere of diepere analyse
```

Voor Vastpakt is dit later commercieel belangrijk.

Je kan producten/diensten bouwen met verschillende lagen:

```text
Basic controle
Advanced controle
Managed workflow
Enterprise governance
```

---

## 9. Approvals: waar de veiligheid binnenkomt

Een van de belangrijkste productiepunten:

> Niet elke actie mag automatisch.

De agent mag voorbereiden.

De mens beslist bij risico.

Voorbeelden:

```text
mail opstellen → mag automatisch als draft
mail verzenden → approval
factuur genereren → mag
factuur versturen → approval
MFA-reset voorstellen → mag
MFA-reset uitvoeren → approval
klant blokkeren → approval
```

Voor Christophe is dit belangrijk omdat bedrijven vaak bang zijn voor AI die zomaar acties uitvoert.

De juiste boodschap is:

> AI neemt het denk- en voorbereidingswerk over, maar kritieke acties blijven gecontroleerd.

---

## 10. Sandbox: het sterkste technische idee uit de sessie

De sandbox was misschien het sterkste technische inzicht.

Niet omdat het fancy is.

Maar omdat het een andere manier geeft om met context om te gaan.

Zonder sandbox:

```text
Stop alle context in de prompt.
```

Met sandbox:

```text
Geef de agent een veilige werkruimte waar hij zelf bestanden kan inspecteren.
```

Dat is veel interessanter voor echte bedrijfsdata:

```text
CSV exports
Excel bestanden
logfiles
facturen
rapporten
SharePoint exports
ERP dumps
audit logs
```

De agent kan zelf:

```text
ls
cat
grep
head
tail
awk
berekeningen doen
kolommen ontdekken
afwijkingen zoeken
```

De analogie uit de sessie:

```text
Niet heel het archief printen.
Wel toegang geven tot het archief met regels en zoekfunctie.
```

Voor Vastpakt/BAVAST is sandbox dus direct relevant.

---

## 11. Tool call versus sandbox: de echte keuze

Dit moet helder blijven.

Een tool call is beter wanneer:

```text
data proper in database/API zit
schema duidelijk is
snelheid belangrijk is
vraag beperkt is
```

Sandbox is beter wanneer:

```text
data in bestanden zit
context groot is
agent moet zoeken en inspecteren
input onvoorspelbaar is
CSV/logs/Excel centraal staan
```

Dus:

```text
getCustomerInvoices(customerId) → tool call
upload invoices.csv → sandbox
```

---

## 12. D0: waarom die data-agent relevant is

D0 bij Vercel is een interne data-agent.

Het patroon:

```text
mens stelt vraag in gewone taal
↓
agent schrijft query
↓
agent haalt data op
↓
agent geeft antwoord
```

Waarom dit belangrijk is:

```text
data-team moet minder repetitieve queries schrijven
collega's krijgen sneller inzichten
governance blijft mogelijk
complexe vragen kunnen begeleid worden
```

De echte les:

> Agents zijn vooral sterk waar mensen vandaag telkens opnieuw dezelfde context moeten opzoeken of dezelfde analyse moeten maken.

---

## 13. Deployment: waarom Vercel dit goed positioneert

De sessie ging daarna naar deployment.

Niet toevallig.

Een agent is pas nuttig als hij draait in een echte app.

Vercel verkoopt hier:

```text
GitHub integration
CLI deploy
preview URLs
production deploy
zero infra setup
```

Maar de diepere boodschap is:

> Agentic apps hebben snelle feedbackloops nodig.

Omdat je moet testen:

```text
prompt
tool calls
modelkeuze
UI output
structured schema
sandboxgedrag
approval flows
kosten
latency
```

Daarom zijn preview deployments zo krachtig.

---

## 14. Production layer: wat onder de motorkap nodig is

Een agent-app vraagt meer dan frontend.

Onderliggend heb je nodig:

```text
compute
API route
runtime
modelprovider/auth
sandbox runtime
file access
logging
observability
cost tracking
error handling
deployment
```

Vercel probeert die laag te bundelen.

Dat is waarom ze het hebben over:

```text
OIDC token
AI Gateway
Sandbox observability
workflow logs
model usage
cost visibility
```

Voor bedrijven is dit verschil groot:

```text
AI demo → leuk
AI product → beheersbaar, meetbaar, veilig
```

---

## 15. AI Gateway: niet zomaar modelroutering

AI Gateway is meer dan modelnaam wisselen.

Het maakt mogelijk:

```text
model switching
fallback bij model failure
kosteninzicht
tokenmeting
latency observability
BYOK / bring your own key
Vercel-billing
verschillende modellen per plan
```

De belangrijkste zin:

> Modelkeuze wordt configuratie, niet architectuur.

Dat is belangrijk omdat modellen blijven veranderen.

De workflow, data, tools en context blijven waardevoller dan de modelnaam.

---

## 16. Workflows en durability: noodzakelijk voor echte processen

Een agent met één tool call is simpel.

Maar echte processen kunnen lang zijn:

```text
lees dossier
haal klantdata op
controleer facturen
zoek afwijkingen
maak verslag
vraag approval
verstuur draft
maak ticket aan
```

Als stap 7 faalt, wil je niet alles opnieuw doen.

Durable workflows lossen dat op:

```text
stap 1 opgeslagen
stap 2 opgeslagen
stap 3 opgeslagen
...
stap 7 faalt
↓
retry stap 7
```

Voor Vastpakt is dit later belangrijk bij processen die meerdere acties bevatten.

Maar voor de eerste MVP hoeft dit nog niet groot te zijn.

---

## 17. Observability: waarom agents anders niet verkoopbaar zijn

Een bedrijf gaat vragen:

```text
Wat deed die agent?
Welke tool gebruikte hij?
Waarom duurde het zo lang?
Wat kostte die run?
Welk model werd gebruikt?
Waarom faalde het?
Welke bestanden heeft hij gelezen?
Welke stap ging fout?
```

Zonder observability heb je geen antwoord.

Dus productie-agenten moeten zichtbaar zijn.

Niet alleen output tonen.

Ook:

```text
runtime
tools
model calls
sandbox commands
workflowstappen
kosten
errors
```

---

## 18. Skills: waarom dit geen gewone prompt engineering meer is

Skills zijn domeinpakketten.

Een skill bevat:

```text
context
best practices
domeinkennis
werkwijze
regels
stijl
voorbeelden
```

Voor Christophe/Vastpakt kan dat later zeer belangrijk worden.

Een Vastpakt skill kan bevatten:

```text
tone of voice
positionering
intakevragen
proposalstructuur
BAVAST-case
woorden die Christophe gebruikt
woorden die Christophe vermijdt
beslisboom voor processen
```

Dit is veel beter dan elke keer dezelfde lange prompt plakken.

---

## 19. Multi-agent: niet te magisch maken

De sessie maakte multi-agent simpel:

> Een agent kan een tool zijn voor een andere agent.

Dus:

```text
Orchestrator Agent
↓
askSupportAgent()
askFinanceAgent()
askDataAgent()
askCodeAgent()
```

Voor nu is dat interessant als concept.

Maar niet als eerste bouwstap.

Voor Vastpakt later:

```text
Intake Agent
Process Mapping Agent
Automation Agent
Proposal Agent
```

Eerst klein beginnen.

---

## 20. Het grootste software-architectuurinzicht

Dit stuk is heel belangrijk:

Vroeger moest je veel logica hardcoden:

```text
if klanttype = enterprise
if topic = firewall
if supportplan = premium
if vraag = billing
if risico = high
```

Dat wordt conditionele spaghetti.

Agents kunnen een deel van die complexiteit vervangen door:

```text
intentie begrijpen
context interpreteren
tool kiezen
workflow kiezen
output structureren
```

De LLM vervangt niet elk regelsysteem.

Maar ze maakt routing, triage en contextuele beslissingen veel eenvoudiger.

Voor Christophe is dat belangrijk omdat zijn sterkte juist zit in:

```text
systeemdenken
procesdenken
IT-troubleshooting
waar loopt het vast?
wie doet dit manueel?
wat gebeurt er als die persoon wegvalt?
```

---

## 21. Wat is ruis in deze sessie?

Niet alles is even belangrijk voor nu.

Minder belangrijk voor vandaag:

```text
exacte Vercel CLI details
exacte UI van Vercel dashboard
of snapshotting al publiek beschikbaar is
alle provider/billingdetails
volledige multi-agent implementatie
self-healing platformvisie
```

Interessant, maar later.

Belangrijk voor nu:

```text
tool calls
structured output
sandbox voor files
approval boundaries
observability denken
kleine workflow bouwen
Vastpakt-positionering
```

---

## 22. Wat moet Christophe hiermee kunnen zeggen?

Niet:

> Ik bouw AI-agents met Vercel.

Wel:

> Ik bouw controleerbare workflows rond processen die vandaag te manueel, te foutgevoelig of te afhankelijk van één persoon zijn.

Nog concreter:

> Upload uw Excel of export, en het systeem toont wat afwijkt, waarom het afwijkt en wat iemand moet controleren.

Dat is begrijpbaar.

Dat verkoopt beter dan technische termen.

---

## 23. De beste eerste bouwrichting

De eerste bouwrichting moet zijn:

# Excel/CSV Controle Agent

Niet omdat dat technologisch het meest indrukwekkend is.

Maar omdat het:

```text
concreet is
herkenbaar is
dicht bij BAVAST ligt
duidelijke waarde heeft
sandbox goed gebruikt
gemakkelijk demo-baar is
gestructureerde output vraagt
```

MVP-flow:

```text
Upload CSV/Excel
↓
Agent inspecteert bestand
↓
Agent zoekt afwijkingen
↓
Agent maakt controleverslag
↓
Mens valideert
```

Output:

```text
kritieke afwijkingen
waarschuwingen
rij/kolom
reden
voorgestelde actie
```

---

## 24. Wat is de eerste demo die je aan een klant toont?

Niet een chatvenster.

Een simpel scherm:

```text
Bestand uploaden
[Controleer bestand]
```

Daarna:

```text
Controleverslag

3 kritieke afwijkingen
7 waarschuwingen
12 aandachtspunten
```

Per issue:

```text
Rij 42
Veld: bedrag
Ernst: kritiek
Reden: bedrag ligt 8x hoger dan normaal patroon
Actie: controleer bronfactuur of tariefafspraak
```

Dit is wat klanten snappen.

---

## 25. Wat mag je niet als eerste bouwen?

Niet starten met:

```text
alles-in-één AI platform
volledige multi-agent orchestrator
M365 recovery automation
complexe billinglagen
enterprise governance
self-healing infra
```

Dat komt later.

Eerste doel:

> Eén bestand. Eén controle. Eén verslag. Eén bewijs van waarde.

---

## 26. De kern voor Vastpakt

Vastpakt moet niet klinken als:

```text
AI agency
prompt engineer
chatbotbouwer
```

Maar als:

```text
proces-vastpakker
workflowbouwer
controlelaagbouwer
AI + IT + operations
```

Positioneringszin:

> Ik help bedrijven waar cruciale kennis in Excel, mailboxen of één hoofd zit, om daar een controleerbare workflow van te maken.

Korter:

> Geen chatbot. Een workflow die werk overneemt.

Nog concreter:

> Minder fouten, minder manueel zoekwerk, minder afhankelijkheid van één persoon.

---

## 27. De echte eindconclusie

De sessie zegt niet alleen iets over Vercel.

Ze bevestigt een grotere marktbeweging:

```text
AI wordt minder een los venster
AI wordt meer een uitvoerende laag in software
```

Voor Christophe betekent dat:

> De kans zit niet in praten over AI. De kans zit in processen vinden waar AI gecontroleerd werk kan overnemen.

En daarom is de eerste concrete stap:

```text
Excel/CSV Controle Agent bouwen
```

Dat is klein genoeg om te bouwen.

Groot genoeg om waarde te tonen.

Dicht genoeg bij BAVAST om geloofwaardig te zijn.

---

## 28. Definitieve samenvatting in één zin

> Vercel Agents tonen hoe je van AI geen chatbot maakt, maar een gecontroleerde werklaag bovenop data, tools en workflows — en voor Vastpakt is de beste eerste toepassing een Excel/CSV-controle-agent die fouten, afwijkingen en afhankelijkheid van één persoon zichtbaar maakt.

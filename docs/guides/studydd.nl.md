# StudyDD

## Samenvatting in één zin

StudyDD houdt je leeronderwerp, bronnen, zwakke punten, herhalingsschema en bewijs van begrip in dezelfde Git-repo waar je agent werkt, zodat er niets verloren gaat tussen sessies.

## Wat het doet

StudyDD is een repo-first leerworkflow. In plaats van aantekeningen te verspreiden over chatthreads, browsertabs en notitieboeken, bewaar je de belangrijke dingen in platte-tekstbestanden in `~/Projects/study-project`. Jij en je agent lezen en werken dezelfde bestanden bij, dus de repo toont altijd de huidige waarheid.

De bestandsset is:

- `AGENTS.md` — regels voor de agent in deze leerrepo, bijvoorbeeld "do not mark topics mastered without evidence".
- `STATUS.md` — wat je momenteel leert, wat je beheerst en hoe zeker je je voelt.
- `NEXT_ACTIONS.md` — de exacte volgende stap voor de volgende leersessie.
- `BACKLOG.md` — onderwerpen die je later wilt leren.
- `WORKLOG.md` — een kort logboek van wat er in elke sessie gebeurde.
- `SOURCES.md` — links, boeken, video's of cursussen die je gebruikte.
- `REVIEW.md` — spaced-review checklist en datums.
- `evidence/README.md` — een catalogus van bewijsbestanden zoals quizzes, toetsen of gemaakte artefacten.

De agent leest deze bestanden aan het begin van een sessie en schrijft alleen updates naar de bestanden die jij toestaat.

## Wanneer je het gebruikt

Gebruik StudyDD als je de agent wilt laten helpen met iets nieuws leren en je de voortgang over dagen of weken wilt bijhouden:

- Een nieuwe programmeertaal leren.
- Een nieuw framework of een nieuwe bibliotheek leren.
- Een nieuw concept uit de informatica leren met een AI-tutor.
- Voorbereiden op een examen, sollicitatiegesprek of certificering.

Het werkt het best als elke sessie eindigt met een klein bewijsstuk dat je later kunt hergebruiken.

## Wat je nodig hebt

Deze handleiding gebruikt `lenny` als gebruikersnaam op `studybox` en `student-phone` als je Android-apparaat. Vervang deze door je eigen namen.

- Een leermachine genaamd `studybox` met Git en een terminal geïnstalleerd.
- Een gebruikersaccount `lenny` op `studybox`.
- Een map voor de repo op `~/Projects/study-project`.
- Je Android-telefoon `student-phone` met Termux, als je onderweg wilt herhalen.
- Ongeveer tien minuten voor de eerste installatie.

## Installeren en controleren

1. Maak de repo-map en de evidence-map aan:

   ```bash
   mkdir -p ~/Projects/study-project/evidence
   cd ~/Projects/study-project
   ```

   `mkdir -p` maakt bovenliggende mappen aan als dat nodig is. De map `evidence/` bevat later quizzes, codevoorbeelden en ander bewijs.

2. Maak de StudyDD-bestanden aan:

   ```bash
   touch AGENTS.md STATUS.md NEXT_ACTIONS.md BACKLOG.md WORKLOG.md SOURCES.md REVIEW.md evidence/README.md
   ```

   `touch` maakt lege bestanden aan zodat de structuur bestaat voordat je deze invult.

3. Maak van de map een Git-repo zodat de bestanden worden bijgehouden:

   ```bash
   git init
   ```

4. Voeg de bestanden toe aan Git en controleer de status:

   ```bash
   git add .
   git status
   ```

   Je zou de acht StudyDD-bestanden moeten zien als "new file" of "Changes to be committed".

5. Controleer dat de bestanden er echt zijn:

   ```bash
   ls -la ~/Projects/study-project
   ls -la ~/Projects/study-project/evidence
   ```

   Beide commando's zouden de verwachte bestanden moeten tonen.

## Je eerste echte workflow

Doel: gebruik StudyDD om **Python dictionaries** te leren in `~/Projects/study-project`.

### 1. Maak de leerrepo

Voer de commando's in [Installeren en controleren](#installeren-en-controleren) uit zodat de bestanden bestaan en Git ze bijhoudt.

### 2. Geef de agent de regels

Schrijf een kort `AGENTS.md` zodat de agent weet wat hij wel en niet mag. Bijvoorbeeld:

```markdown
# Study repo rules

- Read STATUS.md, NEXT_ACTIONS.md, and BACKLOG.md at the start of each session.
- Do not mark a topic mastered unless REVIEW.md contains a passed quiz or artifact.
- Update WORKLOG.md, SOURCES.md, and evidence/README.md as the session progresses.
- Ask before editing STATUS.md or REVIEW.md.
```

Dit voorkomt dat de agent je voortgang voor je gaat inschatten.

### 3. Voeg een leeronderwerp toe

Open `STATUS.md` en schrijf:

```markdown
# Status

## Current focus
Python dictionaries

## Confidence
2 / 5

## Mastered
- None yet
```

Voeg daarna de volgende stap toe aan `NEXT_ACTIONS.md`:

```markdown
# Next actions

1. Ask the agent to explain Python dictionaries with examples.
2. Complete the mini-quiz and save it to evidence/.
```

Voeg tot slot latere onderwerpen toe aan `BACKLOG.md`:

```markdown
# Backlog

- Python sets
- Python list comprehensions
```

### 4. Leer en schrijf bewijs

Voer een sessie met je agent. Vraag hem om dictionaries uit te leggen en een korte quiz te maken. Bewaar het resultaat:

```bash
# Create the quiz file
nano evidence/quiz-dictionaries.md
```

Typ de vragen, je antwoorden en de feedback van de agent. Voeg dan een regel toe aan `evidence/README.md`:

```markdown
# Evidence catalog

- `quiz-dictionaries.md` — mini-quiz on Python dictionaries, passed.
```

Noteer de bron in `SOURCES.md`:

```markdown
# Sources

- Python docs: https://docs.python.org/3/tutorial/datastructures.html#dictionaries
```

En log de sessie in `WORKLOG.md`:

```markdown
# Work log

## 2026-07-06
Studied Python dictionaries. Created quiz in evidence/quiz-dictionaries.md. Need review in 2 days.
```

### 5. Herhaal

Lees het bewijs zelf voordat je het onderwerp als beheerst markeert. Als het goed is, werk je `REVIEW.md` bij:

```markdown
# Review log

## Python dictionaries
- First review: 2026-07-06 — passed quiz.
- Next review: 2026-07-08
```

Werk daarna `STATUS.md` bij:

```markdown
## Current focus
None

## Mastered
- Python dictionaries
```

Maak `NEXT_ACTIONS.md` leeg of laat het naar het volgende onderwerp in `BACKLOG.md` wijzen.

Dat is de StudyDD-loop: **maak een leerrepo → geef de agent de regels → voeg een leeronderwerp toe → schrijf bewijs → herhaal**.

## Veelvoorkomende problemen en oplossingen

| Symptoom | Oorzaak | Oplossing |
| --- | --- | --- |
| De agent blijft `STATUS.md` overschrijven zonder te vragen | `AGENTS.md` verbiedt dit niet | Voeg een regel toe: "Ask before editing STATUS.md or REVIEW.md". |
| Je weet niet meer waar een feit vandaan kwam | `SOURCES.md` is leeg of niet bijgewerkt | Voeg de link of boektitel toe voordat je aan het onderwerp begint. |
| De agent zegt dat een onderwerp beheerst is, maar er is geen bewijs | `REVIEW.md` is door de agent in plaats van door jou bijgewerkt | Lees het bewijs altijd zelf voordat je `REVIEW.md` of `STATUS.md` bijwerkt. |
| De map `evidence/` is moeilijk te doorzoeken | `evidence/README.md` ontbreekt of is verouderd | Vermeld na elke sessie elk bewijsbestand met een korte beschrijving. |
| Je vergeet wanneer je een onderwerp moet herhalen | `REVIEW.md` heeft geen volgende-herhaaldatum | Voeg elke keer dat je een herhaling afrondt een regel "Next review" met een datum toe. |
| `git status` toont veel niet-getraceerde StudyDD-bestanden | Je hebt de bestanden aangemaakt maar nooit toegevoegd | Voer `git add .` uit en commit aan het einde van een sessie. |
| Bewerkingen op de telefoon zien er goed uit maar breken de Markdown-opmaak | Virtuele toetsenborden en autocorrectie veranderen tekens | Gebruik een platte-teksteditor op `student-phone`, schakel autocorrectie uit, of doe laatste bewerkingen op `studybox` (niet geverifieerd). |
| De agent raakt van het onderwerp af | `NEXT_ACTIONS.md` of `BACKLOG.md` is onduidelijk | Wijs de agent aan het begin van de sessie terug naar `STATUS.md` en `NEXT_ACTIONS.md` (niet geverifieerd). |

## Veiligheidsopmerkingen

!!! warning "Beheersing heeft bewijs nodig"
    Markeer een onderwerp niet als beheerst zonder een toets, quiz of gemaakt artefact.

- Houd de regel in `AGENTS.md` dat de agent onderwerpen niet als beheerst mag markeren.
- Lees elk bewijsstuk voordat je `REVIEW.md` of `STATUS.md` bijwerkt.
- Push de repo regelmatig naar een remote of maak back-ups, vooral voor een lange pauze.
- Wees eerlijk over zwakke punten; de zekerheidsscores in `STATUS.md` zijn voor jou, niet voor de agent.
- Als een link in `SOURCES.md` niet meer werkt, noteer dit zodat je niet op een dode bron vertrouwt.

## Telefoon vs laptop

| Taak | Telefoon met Termux (`student-phone`) | Laptop (`studybox`) |
| --- | --- | --- |
| `STATUS.md` of `NEXT_ACTIONS.md` lezen | Goed | Goed |
| Snel flitskaarten of quizantwoorden herhalen | Goed | Goed |
| Lange bewijsbestanden schrijven | Moeilijk | Makkelijk |
| Codevoorbeelden of tests draaien | Traag of onmogelijk | Makkelijk |
| Markdown-tabellen typen | Foutgevoelig | Comfortabel |
| Wijzigingen committen en pushen | Mogelijk | Makkelijker |

Behandel de telefoon als een herhalingspartner. Doe het belangrijkste leren, het schrijven van bewijs en de laatste herhaling op `studybox`.

## Wat je verder kunt lezen

- Lees de [StateDD-handleiding](statedd.nl.md) om op dezelfde repo-first manier hele projecten te beheren.
- Lees de [Veiligheidsgrenzen-pagina](../safety/boundaries.nl.md) voor regels die jou en je gegevens beschermen terwijl je een AI-agent gebruikt.

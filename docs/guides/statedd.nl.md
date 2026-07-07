# StateDD

## Samenvatting in één zin

StateDD houdt de huidige status, beslissingen, backlog, validatiebewijs en sessieoverdracht van je project in dezelfde Git-repo waar de agent werkt, zodat de repo altijd de enige bron van waarheid is.

## Wat het doet

StateDD is een repo-first projectworkflow. In plaats van te vertrouwen op een chatthread die onthoudt wat er is gebouwd en waarom, bewaar je de belangrijke projectstatus in platte-tekstbestanden in `~/Projects/study-project`. Jij en je agent lezen en werken dezelfde bestanden bij, dus elke sessie start vanuit de werkelijke huidige status.

De bestandsset is:

- `AGENTS.md` — regels voor de agent in deze projectrepo, bijvoorbeeld "do not mark a task closed without evidence".
- `STATUS.md` — de huidige projectstatus, open taken en blokkades.
- `PROJECT_STATE.yaml` — machine-leesbare projectstatus zoals actieve fase, datum van de laatste sessie en gezondheidsvlaggen.
- `PROJECT_DNA.yaml` — stabiele projectfeiten zoals tech stack, afspraken en beperkingen.
- `NEXT_ACTIONS.md` — de exacte volgende stap die de agent moet zetten.
- `BACKLOG.md` — taken en ideeën voor later.
- `WORKLOG.md` — een kort logboek van wat er in elke sessie gebeurde.
- `docs/EVIDENCE_LOG.md` — een catalogus van validatiebewijs zoals testuitvoer, screenshots of diffs.
- `evidence/` — map met de daadwerkelijke bewijsbestanden.

De agent leest deze bestanden aan het begin van een sessie, stelt wijzigingen voor en werkt statusbestanden alleen bij als jij het goedkeurt.

## Wanneer je het gebruikt

Gebruik StateDD als je de agent code wilt laten bouwen of wijzigen over meer dan één sessie:

- Een kleine applicatie of script bouwen.
- Refactoren of features toevoegen aan een bestaand project.
- Debuggen van een niet-triviaal probleem dat meerdere pogingen nodig heeft.
- Elk project waarbij je moet bewijzen dat een wijziging werkt voordat je verdergaat.

Het werkt het best als elke afgesloten taak een bijbehorend bewijsstuk heeft.

## Wat je nodig hebt

Deze handleiding gebruikt `lenny` als gebruikersnaam op `studybox` en `student-phone` als je Android-apparaat. Vervang deze door je eigen namen.

- Een projectmachine genaamd `studybox` met Git en een terminal geïnstalleerd.
- Een gebruikersaccount `lenny` op `studybox`.
- Een map voor de repo op `~/Projects/study-project`.
- Je Android-telefoon `student-phone` met Termux, als je onderweg de status wilt controleren.
- Ongeveer tien minuten voor de eerste installatie.

## Installeren en controleren

1. Maak de repo-map en de evidence-map aan:

   ```bash
   mkdir -p ~/Projects/study-project/docs
   mkdir -p ~/Projects/study-project/evidence
   cd ~/Projects/study-project
   ```

   `mkdir -p` maakt bovenliggende mappen aan als dat nodig is. De map `docs/` bevat de evidence-log en `evidence/` bevat de daadwerkelijke bewijsbestanden.

2. Maak de StateDD-bestanden aan:

   ```bash
   touch AGENTS.md STATUS.md PROJECT_STATE.yaml PROJECT_DNA.yaml NEXT_ACTIONS.md BACKLOG.md WORKLOG.md docs/EVIDENCE_LOG.md
   ```

   `touch` maakt lege bestanden aan zodat de structuur bestaat voordat je deze invult.

3. Maak van de map een Git-repo zodat de bestanden worden bijgehouden:

   ```bash
   git init
   ```

   `git init` maakt een nieuwe Git-repository in de huidige map zodat wijzigingen in de StateDD-bestanden kunnen worden bijgehouden.

4. Voeg de bestanden toe aan Git en controleer de status:

   ```bash
   git add .
   git status
   ```

   Je zou de StateDD-bestanden en -mappen moeten zien als "new file" of "Changes to be committed".

5. Controleer dat de bestanden er echt zijn:

   ```bash
   ls -la ~/Projects/study-project
   ls -la ~/Projects/study-project/evidence
   ```

   Beide commando's zouden de verwachte bestanden moeten tonen.

## Je eerste echte workflow

Doel: gebruik StateDD om een kleine feature toe te voegen aan `~/Projects/study-project`.

### 1. Maak de projectrepo

Voer de commando's in [Installeren en controleren](#installeren-en-controleren) uit zodat de bestanden bestaan en Git ze bijhoudt.

### 2. Geef de agent de regels

Schrijf een kort `AGENTS.md` zodat de agent weet wat hij wel en niet mag. Bijvoorbeeld:

```markdown
# Project rules

- Read STATUS.md, PROJECT_STATE.yaml, NEXT_ACTIONS.md, and BACKLOG.md at the start of each session.
- Propose a plan before changing code.
- Do not mark a task closed without evidence in evidence/ and docs/EVIDENCE_LOG.md.
- Update WORKLOG.md and docs/EVIDENCE_LOG.md as the session progresses.
- Ask before editing STATUS.md, PROJECT_STATE.yaml, or NEXT_ACTIONS.md.
```

Dit voorkomt dat de agent gaat raden wat hij moet bouwen of werk als afgerond verklaart zonder bewijs.

### 3. Maak een taak aan

Open `STATUS.md` en schrijf de huidige status:

```markdown
# Status

## Active tasks
- Add a command-line greeting script

## Blockers
- None

## Last validation
- None
```

Voeg daarna de taak toe aan `NEXT_ACTIONS.md`:

```markdown
# Next actions

1. Propose a design for a greeting script.
2. Implement the script.
3. Run the script and capture output as evidence.
```

Voeg latere ideeën toe aan `BACKLOG.md`:

```markdown
# Backlog

- Add name argument handling
- Add a test file
```

### 4. Laat de agent een voorstel doen

Start een sessie met je agent. Wijs hem op `AGENTS.md` en de huidige statusbestanden, en vraag hem aan de actieve taak te werken. De agent zou een plan moeten voorstellen voordat hij code wijzigt.

Lees het voorstel. Als het er goed uitziet, zeg je dat hij mag doorgaan. Zo niet, werk dan `NEXT_ACTIONS.md` bij met wat er ontbreekt en vraag opnieuw.

### 5. Valideer

Nadat de agent het script heeft geïmplementeerd, vraag je hem om te bewijzen dat de wijziging werkt. Bijvoorbeeld:

```bash
python3 greeting.py > evidence/greeting-output.txt
```

Dit draait het script en bewaart de uitvoer. Voeg daarna een regel toe aan `docs/EVIDENCE_LOG.md`:

```markdown
# Evidence log

- `evidence/greeting-output.txt` — output of `python3 greeting.py` on 2026-07-06 (replace with today's date).
```

Log de sessie in `WORKLOG.md`:

```markdown
# Work log

## 2026-07-06
Added greeting.py. Captured output in evidence/greeting-output.txt. Script runs without errors.
```

### 6. Sluit de taak af

Als het bewijs er goed uitziet, werk je `STATUS.md` bij:

```markdown
## Active tasks
- None

## Last validation
- greeting.py runs and produces expected output
```

Maak `NEXT_ACTIONS.md` leeg of werk het bij zodat het naar de volgende taak uit de backlog wijst.

Dat is de StateDD-lus: **maak een projectrepo → maak een taak aan → laat de agent een voorstel doen → valideer → sluit af**.

## Veelvoorkomende problemen en oplossingen

De oplossingen hieronder zijn gebaseerd op veelvoorkomende StateDD-praktijk. Ik heb ze niet allemaal op deze machine getest.

| Symptoom | Oorzaak | Oplossing |
| --- | --- | --- |
| De agent begint bestanden te wijzigen voordat hij een plan voorstelt | `AGENTS.md` vereist niet eerst een plan | Voeg de regel "Propose a plan before changing code" toe en wijs de agent terug naar `AGENTS.md`. |
| De agent zegt dat een taak klaar is maar er is geen bewijs | `docs/EVIDENCE_LOG.md` of `evidence/` is leeg of overgeslagen | Wijs de afsluiting af en eis een test, screenshot, log of diff in `evidence/` met een vermelding in `docs/EVIDENCE_LOG.md`. |
| `STATUS.md` toont taken die al afgerond zijn | `STATUS.md` is niet bijgewerkt na validatie | Werk `STATUS.md` bij zodra je het bewijs accepteert. |
| De agent begint al aan een backlog-taak voordat de actieve taak is gesloten | `NEXT_ACTIONS.md` of `BACKLOG.md` is onduidelijk | Wijs de agent aan het begin van de sessie terug naar `STATUS.md` en `NEXT_ACTIONS.md`. |
| `PROJECT_DNA.yaml` verandert elke sessie | De agent behandelt projectfeiten als werknotities | Voeg een regel toe dat alleen jij `PROJECT_DNA.yaml` mag bewerken, en houd DNA-feiten stabiel. |
| `git status` toont veel niet-getraceerde StateDD-bestanden | Je hebt de bestanden aangemaakt maar nooit toegevoegd | Voer `git add .` uit en commit aan het einde van een sessie. |
| Bewerkingen op de telefoon zien er goed uit maar breken de YAML- of Markdown-opmaak | Virtuele toetsenborden en autocorrectie veranderen tekens | Gebruik een platte-teksteditor op `student-phone`, schakel autocorrectie uit, of doe laatste bewerkingen op `studybox`. |
| De agent herschrijft `PROJECT_STATE.yaml` om zijn eigen bewering te bevestigen | `AGENTS.md` staat toe dat hij statusbestanden vrij bijwerkt | Voeg "Ask before editing STATUS.md, PROJECT_STATE.yaml, or NEXT_ACTIONS.md" toe en bekijk diffs voordat je goedkeurt. |

## Veiligheidsopmerkingen

!!! warning "Geïmplementeerd is niet gevalideerd"
    De agent moet een wijziging bewijzen met tests, runtime-bewijs of screenshots.

    - Houd de regel in `AGENTS.md` dat de agent taken niet zonder bewijs mag afsluiten.
    - Lees elk bewijsstuk voordat je `STATUS.md` of `NEXT_ACTIONS.md` bijwerkt.
    - Laat de agent StateDD-bestanden nooit herschrijven om alleen zijn eigen bewering te bevestigen.
    - Push de repo regelmatig naar een remote of maak back-ups, vooral voor een lange pauze.
    - Houd geheimen, tokens en wachtwoorden uit alle StateDD-bestanden en bewijsscreenshots.
    - Behandel het voorstel van de agent als een voorstel, niet als de definitieve beslissing.

## Telefoon vs laptop

| Taak | Telefoon met Termux (`student-phone`) | Laptop (`studybox`) |
| --- | --- | --- |
| `STATUS.md` of `NEXT_ACTIONS.md` lezen | Goed | Goed |
| Een kort bewijsbestand controleren | Goed | Goed |
| Lange bewijsbestanden of logs schrijven | Moeilijk | Makkelijk |
| Tests of scripts draaien | Traag of onmogelijk | Makkelijk |
| Markdown-tabellen of YAML typen | Foutgevoelig | Comfortabel |
| Wijzigingen committen en pushen | Mogelijk | Makkelijker |

Behandel de telefoon als een statuschecker. Doe het belangrijkste plannen, valideren, bewijs verzamelen en status bijwerken op `studybox`.

## Wat je verder kunt lezen

- Lees de [StudyDD-handleiding](studydd.nl.md) om leeronderwerpen op dezelfde repo-first manier te beheren.
- Lees de [Veiligheidsgrenzen-pagina](../safety/boundaries.nl.md) voor regels die jou en je gegevens beschermen terwijl je een AI-agent gebruikt.

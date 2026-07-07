# Grenzen

## Samenvatting in één zin

Stel eenvoudige, niet-onderhandelbare limieten in zodat een AI-codingagent zich gedraagt als een snelle junior-collega in je terminal in plaats van als een roekeloze beheerder.

## Wat het doet

Grenzen vertellen jou en de agent wat normaal werk is en wat een menselijke beslissing vereist. Ze beschermen drie dingen:

- **Je repo.** De Git-repo is de bron van waarheid. Plannen, bewijs en statusbestanden leven erin.
- **Je geheimen.** API-sleutels, tokens en wachtwoorden horen nooit in prompts, screenshots of commits.
- **Je machine.** De agent stelt commando's, installaties en commits voor; jij bekijkt ze voordat er iets draait.

Met grenzen op hun plaats wordt elk onverwacht verzoek van de agent een kleine beslisboom in plaats van een paniekmoment. Deze pagina loopt drie veelvoorkomende beslissingen door en geeft je een herbruikbare checklist.

## Wanneer je het gebruikt

- Voordat je een agentsessie op `studybox` start.
- Als de agent vraagt om een pakket te installeren dat je niet herkent.
- Als de agent een bestand wilt committen dat op `.env` lijkt.
- Als de agent een shellcommando voorstelt dat je niet helemaal begrijpt.
- Als je van laptop naar `student-phone` wisselt en moet beslissen wat veilig is om op een klein scherm goed te keuren.

## Wat je nodig hebt

- Een computer genaamd `studybox` met een terminal en Git geïnstalleerd.
- Een gebruikersaccount `lenny` op `studybox`. Vervang dit door je eigen gebruikersnaam.
- Een projectrepo op `~/Projects/study-project`. Vervang dit door je eigen projectpad.
- Een Android-telefoon genaamd `student-phone` voor de telefoon-vs-laptop-notities.
- Basisvaardigheden in de terminal: commando's uitvoeren, uitvoer lezen en tekstbestanden bewerken.

## Installeren en controleren

Deze commando's maken een veiligheidsbasislijn in `~/Projects/study-project`.

1. Ga naar je projectmap:

   ```bash
   cd ~/Projects/study-project
   ```

   `cd` wijzigt de werkmap van je shell naar de repo.

2. Zorg dat `.env` en geheimachtige bestanden door Git worden genegeerd:

   ```bash
   cat .gitignore
   ```

   `cat` print de inhoud van het bestand. Zoek naar regels zoals `.env`, `.env.*`, `*.key`, `*.pem` en `secrets/`.

3. Als die regels ontbreken, voeg ze toe:

   ```bash
   cat >> .gitignore << 'EOF'
   .env
   .env.*
   !.env.example
   *.key
   *.pem
   secrets/
   EOF
   ```

   Dit voegt de regels voor het negeren van geheimen toe aan `.gitignore`. De regel `!.env.example` betekent dat Git *wel* een voorbeeldbestand met nepwaarden bijhoudt.

4. Maak een voorbeeldomgevingsbestand zonder echte geheimen:

   ```bash
   cat > .env.example << 'EOF'
   # Copy this file to .env and fill in your own values
   API_KEY=your-key-here
   DATABASE_URL=sqlite:///dev.db
   EOF
   ```

   `.env.example` toont de agent de vorm van de configuratie zonder je echte inloggegevens prijs te geven.

5. Controleer dat de repo schoon is en dat `.env` wordt genegeerd:

   ```bash
   git status
   ```

   Als `.env` als niet-getraceerd wordt getoond, voeg het dan toe aan `.gitignore` en voer `git status` opnieuw uit. Het zou niet meer moeten verschijnen.

6. Voeg de voorbeeldbestanden toe aan Git:

   ```bash
   git add .gitignore .env.example
   git status
   ```

   Je zou `.gitignore` en `.env.example` klaar om te committen moeten zien. Je echte `.env` blijft privé.

## Je eerste echte workflow

De beste manier om grenzen te leren is door drie echte beslissingen te oefenen. Ze vinden allemaal plaats terwijl je werkt in `~/Projects/study-project` op `studybox`.

### 1. De agent vraagt om een pakket te installeren dat je niet herkent

De agent toont:

```text
I need to install `sentiment-cli` to analyze text. Run:

python3 -m pip install sentiment-cli
```

Stop. Voer het commando nog niet uit.

1. Vraag de agent wat het pakket doet en waar het vandaan komt.
2. Bekijk wat er geïnstalleerd zou worden zonder je omgeving te wijzigen:

   ```bash
   python3 -m pip install --dry-run sentiment-cli
   ```

   `--dry-run` simuleert de installatie en toont de pakketversie en afhankelijkheden, maar schrijft niets naar schijf.

3. Controleer dat het pakket legitiem lijkt. Kijk naar de project-URL, versienummer en het aantal downloads. Als er iets verdachts aan voelt, zeg nee.
4. Als je goedkeurt, installeer je het en controleer je dat het schoon importeert:

   ```bash
   python3 -m pip install sentiment-cli
   python3 -c "import sentiment_cli; print(sentiment_cli.__version__)"
   ```

   Het tweede commando laadt het pakket en toont de versie. Als het mislukt, is de installatie stuk of is de pakketnaam verkeerd.

5. Voeg het pakket toe aan `requirements.txt` zodat de afhankelijkheid in Git wordt bijgehouden:

   Controleer eerst of `sentiment-cli` al vermeld staat zodat je geen dubbele regel maakt:

   ```bash
   grep -i "sentiment-cli" requirements.txt || python3 -m pip freeze | grep sentiment-cli >> requirements.txt
   ```

   `grep -i` zoekt in `requirements.txt` zonder onderscheid tussen hoofd- en kleine letters. Als het pakket er al staat, doet het commando niets. Als het ontbreekt, toont `pip freeze` de geïnstalleerde pakketten, selecteert `grep` de regel voor `sentiment-cli` en voegt deze met `>>` één keer toe.

   Je kunt `requirements.txt` ook in je editor openen en de regel handmatig toevoegen als hij nog niet aanwezig is.

### 2. De agent wil een bestand committen dat op `.env` lijkt

De agent zegt:

```text
I have created a `.env` file with your database password. I will add it to the next commit.
```

Stop. Een `.env`-bestand bevat meestal geheimen en mag niet worden gecommit.

1. Controleer wat Git ziet:

   ```bash
   git status
   ```

   Je zou `.env` moeten zien als niet-getraceerd of gestaged bestand.

2. Als de agent het al heeft gestaged, haal het dan uit de staging area:

   ```bash
   git restore --staged .env
   ```

   `git restore --staged` haalt het bestand uit de staging area zonder het uit je werkmap te verwijderen.

3. Zorg dat `.env` in `.gitignore` staat:

   ```bash
   grep -E "^\.env" .gitignore
   ```

   Dit controleert op elke regel die begint met `.env`, zoals `.env` of `.env.*`. Als niets overeenkomt, bekijk je `.gitignore` visueel en voeg je `.env` en `.env.*` toe waar nodig.

4. Maak een voorbeeldbestand dat veilig gecommit kan worden:

   !!! warning "Zet nooit echte geheimen in `.env.example`"
       `.env.example` is bedoeld om naar Git te committen. Het mag alleen nepwaarden bevatten. Kopieer nooit een echte `.env` hierheen.

   Maak `.env.example` vanaf nul met placeholderwaarden:

   ```bash
   cat > .env.example << 'EOF'
   # Copy this file to .env and fill in your own values
   API_KEY=your_api_key_here
   DATABASE_PASSWORD=your-password-here
   DATABASE_URL=sqlite:///dev.db
   EOF
   ```

   Open `.env.example` daarna in je editor en controleer dat elke waarde een placeholder is, geen echt geheim.

5. Stage alleen de veilige bestanden:

   ```bash
   git add .gitignore .env.example
   git status
   ```

   `.env` zou niet meer in de status-uitvoer moeten verschijnen. `.gitignore` en `.env.example` zouden klaar om te committen moeten zijn.

### 3. De agent stelt een shellcommando voor dat je niet begrijpt

De agent toont:

```text
Clean up old logs with:

find / -name "*.log" -exec rm -f {} \;
```

Stop. Dit commando doorzoekt je hele bestandssysteem en forceert de verwijdering van elk `.log`-bestand. Het kan andere programma's breken.

1. Vraag de agent om elk deel van het commando uit te leggen. In dit geval:
   - `find /` begint met zoeken bij de root van het bestandssysteem.
   - `-name "*.log"` matcht bestanden die eindigen op `.log`.
   - `-exec rm -f {} \;` verwijdert elk gevonden bestand zonder te vragen.

2. Lees de handleiding voor het commando:

   ```bash
   man find
   ```

   Druk op `q` om de handleidingviewer te verlaten.

3. Draai eerst een veilige, alleen-lezen versie, beperkt tot je project:

   ```bash
   find ~/Projects/study-project -name "*.log" -print
   ```

   `-print` toont de gevonden bestanden zonder iets te verwijderen. Lees de lijst.

4. Als de lijst alleen logs bevat die je echt wilt verwijderen, voer dan de destructieve versie alleen binnen je project uit:

   ```bash
   find ~/Projects/study-project -name "*.log" -delete
   ```

   `-delete` verwijdert alleen de bestanden in `~/Projects/study-project`. Het bereik is klein en het commando is geen mysterie meer.

5. Als het commando nog steeds onduidelijk is, wijs het af en vraag de agent om een eenvoudiger, beperkt alternatief.

## Veelvoorkomende problemen en oplossingen

De tabel hieronder somt veelvoorkomende fouten op, waarom ze onveilig zijn en het veilige alternatief.

| Veelgemaakte fout | Waarom het onveilig is | Veilig alternatief |
|---|---|---|
| Een API-sleutel plakken in de agent-chat | De sleutel kan gelogd, opgeslagen of gelekt worden via een screenshot. | Zet de sleutel in `.env` en noem hem nooit in chat. Gebruik `.env.example` voor alleen de vorm. |
| `.env` committen | Echte geheimen worden permanent in de Git-geschiedenis. | Voeg `.env` toe aan `.gitignore` en commit `.env.example` in plaats daarvan. |
| `curl ... \| sh` draaien zonder het script te lezen | Je voert code van internet uit zonder te kijken. | Lees het script eerst, gebruik de officiële installer, of installeer via je pakketbeheerder. |
| `sudo` gebruiken alleen omdat de agent het vraagt | Het geeft de agent roottoegang voor één commando; typefouten kunnen het systeem breken. | Vraag waarom root nodig is, zoek het commando op en draai het zelf alleen als je het begrijpt. |
| `git push --force` gebruiken | Het overschrijft remote-geschiedenis en kan het werk van iemand anders verwijderen. | Gebruik alleen `git push --force-with-lease` als je zeker weet dat niemand anders heeft gepusht. |
| Een shellcommando draaien dat je niet begrijpt | Verborgen flags kunnen bestanden verwijderen, data exfiltreren of malware installeren. | Lees de handleiding, draai een alleen-lezen of beperkte versie, en wijs af wat je niet kunt uitleggen. |
| Een screenshot maken waar een token zichtbaar is | Sleutels kunnen door anderen uit afbeeldingen worden gelezen. | Vervaag of crop het geheim, of gebruik tekstlogs waarin het geheim is vervangen door `***`. |

## Veiligheidsopmerkingen

!!! warning "De agent stelt voor; jij beslist"
    Laat een agent nooit destructieve of onomkeerbare acties uitvoeren zonder je expliciete goedkeuring.

- **Repo first.** De Git-repo in `~/Projects/study-project` is de bron van waarheid. Plannen en bewijs leven daar, niet in de chatthread.
- **Plan voor build.** Voor elke niet-triviale taak moet de agent het plan uitleggen voordat hij bestanden wijzigt.
- **Bewijs voor afsluiting.** Een taak is niet klaar omdat de agent het zegt. Vraag om tests, screenshots, logs of een schone diff.
- **Geen geheimen in chat of Git.** API-sleutels, tokens en wachtwoorden horen niet in prompts, screenshots of commits.
- **Eén taak tegelijk.** Parallelle agents alleen als ze niet dezelfde bestanden of beslissingen raken.
- **Beperk alles.** Geef de voorkeur aan commando's die `~/Projects/study-project` beïnvloeden boven commando's die het hele bestandssysteem beïnvloeden.
- **De mens blijft reviewer.** Accepteer, wijzig of wijs elk voorstel af.

## Telefoon vs laptop

| Taak | Laptop (`studybox`) | Telefoon (`student-phone`) |
|---|---|---|
| Een korte statusmelding lezen | Goed | Goed |
| Een eenvoudige fix goedkeuren | Goed | Goed |
| Een grote diff reviewen | Makkelijk | Moeilijk |
| Onbekende pakketten installeren | Doe dit op `studybox` | Vermijd |
| Bestanden committen | Makkelijk | Foutgevoelig |
| Veiligheidsbeslissingen zoals `.env`, `sudo` of force-push | Vereist | Vermijd |

Gebruik je telefoon voor snelle controles en kleine goedkeuringen. Doe installaties, grote reviews, merges en veiligheidsbeslissingen op `studybox`.

## Wat je verder kunt lezen

- [Herdr-handleiding](../guides/herdr.nl.md) — laat agentsessies op `studybox` draaien en verbind veilig opnieuw vanaf `student-phone`.
- [StateDD-handleiding](../guides/statedd.nl.md) — houd plannen, status en bewijs in de repo zodat de agent altijd vanuit de waarheid start.

# Herdr

## Samenvatting in één zin

Herdr houdt je terminalwerkruimte—agents, servers en shells—actief op een host, zodat je kunt loskoppelen, naar een ander apparaat gaan en opnieuw verbinden zonder werk kwijt te raken.

## Wat het doet

Herdr is een terminalmultiplexer die is gemaakt voor programmeeragents. Hij draait als een achtergrondserver op je machine. Elk paneel, elke tab en elke werkruimte leeft in die server, niet in het terminalvenster dat je ziet. Als je je laptop dichtklapt, je terminal sluit of van laptop naar telefoon wisselt, blijven de processen draaien. Als je een nieuwe terminal opent en `herdr` uitvoert, verbind je opnieuw met dezelfde sessie.

Herdr toont ook de status van elke programmeeragent in een zijbalk: `working`, `blocked`, `done` of `idle`. Zo zie je in één oogopslag welke agent aandacht nodig heeft, zonder door elk terminalvenster te hoeven klikken.

Je kunt het zien als tmux dat opnieuw is ontworpen voor AI-agents: blijvende sessies, gesplitste panelen en op afstand opnieuw verbinden, plus inzicht in wat je agents aan het doen zijn.

## Wanneer je het gebruikt

- **Langdurige agenttaken** — Start een refactor of testrun op `studybox`, koppel los en controleer later vanaf je telefoon hoe ver het is.
- **Werken op afstand** — Laat agents draaien op een thuisserver of VM en verbind vanaf elke locatie via SSH.
- **Wisselen tussen laptop en telefoon** — Begin op je laptop en ga verder met goedkeuringen of statuschecks vanaf `student-phone`.
- **Meerdere agents tegelijk draaien** — Houd één agent per paneel en gebruik de zijbalk om te zien welke is geblokkeerd.
- **Bescherm werk tegen onbedoeld sluiten** — Een terminaltab sluiten koppelt je los; het beëindigt de sessie niet.

## Wat je nodig hebt

- Een computer met Linux, macOS of WSL. Deze handleiding noemt hem `studybox`.
- Een Android-telefoon voor het opnieuw verbinden. Deze handleiding noemt hem `student-phone`.
- Een werkende SSH-verbinding vanaf de telefoon naar `studybox` via Tailscale of je lokale netwerk.
- Ongeveer 15 minuten.
- Basisvaardigheden in de terminal: commando's uitvoeren, toetsencombinaties gebruiken en bestanden bewerken.
- De voorbeeldgebruikersnaam op `studybox` is `lenny`. Vervang deze door je echte gebruikersnaam als je `ssh lenny@studybox` uitvoert.

Als je SSH of Tailscale nog niet hebt ingesteld, volg dan eerst de links aan het einde van deze handleiding.

## Installeren en controleren

Deze commando's voer je uit op `studybox`.

1. Download en voer het officiële installatiescript uit.

    ```bash
    curl -fsSL https://herdr.dev/install.sh | sh
    ```

    `curl -fsSL` haalt het installatiescript stil op en volgt omleidingen. De pipe `| sh` voert het uit in je huidige shell. Het installatieprogramma downloadt één enkel binair bestand en plaatst het in een map op je `PATH`, meestal `~/.local/bin`.

2. Herlaad je shell zodat het nieuwe programma wordt gevonden.

    ```bash
    exec $SHELL -l
    ```

    `exec` vervangt de huidige shell door een nieuwe. De vlag `-l` maakt het een login-shell, die je profiel en `PATH` opnieuw inleest.

3. Controleer of Herdr is geïnstalleerd.

    ```bash
    herdr --version
    ```

    Verwachte uitvoer is een versienummer, bijvoorbeeld:

    ```text
    herdr 0.6.6
    ```

    Het exacte versienummer kan afwijken.

4. Controleer dat de Herdr-server niet al draait vanuit een oudere installatie.

    ```bash
    herdr status
    ```

    Als er geen server draait, meldt hij dat. Als er wel een server draait, toont hij de versie en de status van de sessie.

## Je eerste echte workflow

In deze werkstroom start je Herdr, draai je een kleine webserver in één paneel, koppel je los, verbind je opnieuw vanaf je telefoon en stop je alles netjes.

### 1. Start Herdr vanuit je projectmap

```bash
cd ~/Projects/study-project
herdr
```

`cd` wisselt naar je projectmap. `herdr` start de achtergrondserver en opent de terminalgebruikersinterface (TUI). Als er al een sessie bestaat, wordt opnieuw verbonden.

Je ziet een zijbalk aan één kant en één paneel met je shell-prompt.

### 2. Splits het paneel en start een eenvoudige server

Druk op `Ctrl+B`, laat los, en druk dan op `v`.

Hiermee maak je een tweede paneel aan de rechterkant. De combinatie `Ctrl+B` wordt het *prefix* genoemd. Herdr wacht op het prefix en interpreteert de volgende toets dan als een commando.

Start in het nieuwe paneel een kleine webserver:

```bash
python3 -m http.server 8080
```

Dit serveert de huidige map op poort `8080`. Het blijft draaien, wat we willen testen.

### 3. Koppel los en laat de server draaien

Druk op `Ctrl+B`, laat los, en druk dan op `q`.

De Herdr TUI sluit. Je terminal keert terug naar de normale shell. De server draait nog steeds binnen de Herdr-server op `studybox`.

Controleer dat de server nog reageert:

```bash
curl http://localhost:8080
```

Je ziet HTML met een lijst van bestanden in `~/Projects/study-project`. Als de map leeg is, zie je een lege mapweergave. Het belangrijkste is dat `curl` een reactie krijgt.

### 4. Verbind opnieuw vanaf je telefoon

Open op `student-phone` Termux en SSH naar `studybox`:

```bash
ssh lenny@studybox
```

Verbind dan opnieuw met dezelfde sessie:

```bash
cd ~/Projects/study-project
herdr
```

Je ziet dezelfde twee panelen. De webserver draait nog steeds aan de rechterkant. Dit is dezelfde sessie die je op de laptop hebt achtergelaten.

### 5. Stop de server en de Herdr-sessie

Stop in het paneel met de webserver `python3` met `Ctrl+C`.

Stop daarna in een willekeurig paneel de Herdr-server:

```bash
herdr server stop
```

`herdr server stop` sluit de achtergrondserver en alle panelen af. Dit is de bewuste manier om een sessie te beëindigen. Loskoppelen met `Ctrl+B q` laat alles draaien; de server stoppen beëindigt de sessie.

Controleer na het stoppen dat er geen sessie meer draait:

```bash
herdr status
```

De verwachte uitvoer geeft aan dat er geen server draait.

## Veelvoorkomende problemen en oplossingen

| Symptoom | Waarschijnlijke oorzaak | Oplossing |
|---|---|---|
| `herdr: command not found` na installatie | De nieuwe map `~/.local/bin` zit niet in de `PATH` van je huidige shell. | Herstart de terminal of voer `exec $SHELL -l` uit. (niet geverifieerd) |
| De Herdr TUI toont een zwart scherm of vervormde tekst | Je terminal is te klein of ondersteunt geen truecolor. | Maak de terminal minstens 80 kolommen bij 24 rijen groot en gebruik een moderne terminal zoals Ghostty, WezTerm, iTerm2, Windows Terminal of GNOME Terminal. (niet geverifieerd) |
| `Ctrl+B q` doet niets | De toetsen worden tegelijk ingedrukt in plaats van als een reeks. | Druk op `Ctrl+B`, laat beide toetsen los, en druk dan op `q`. (niet geverifieerd) |
| De zijbalk toont `tmux` in plaats van mijn agent | Je shell start automatisch tmux binnen het Herdr-paneel, zodat Herdr tmux als voorgrondproces ziet. | Schakel tmux auto-attach uit voor die shell, of start de agent direct zonder hem in tmux te verpakken. (niet geverifieerd) |
| Een agent stelt een vraag maar de zijbalk toont `idle` | De schermmanifest van Herdr kent de promptvorm van die agent nog niet. | Voer `herdr agent explain <target>` uit om de detectie te inspecteren, werk daarna Herdr bij of rapporteer de promptvorm. (niet geverifieerd) |
| Processen verdwijnen na `herdr server stop` | `server stop` beëindigt de sessie en haar panelen bewust. | Gebruik `Ctrl+B q` om los te koppelen als je alleen wilt verbreken. Gebruik `herdr server stop` alleen als je alles wilt afsluiten. (niet geverifieerd) |
| `herdr --remote studybox` lukt niet om Herdr op de externe host te installeren | De externe shell is niet-interactief, de platformen komen niet overeen, of `~/.local/bin` staat niet op de externe `PATH`. | Installeer Herdr eerst handmatig op `studybox`, of stel `HERDR_REMOTE_BINARY` in op een lokaal binair pad voordat je het commando uitvoert. (niet geverifieerd) |
| De SSH-client op de telefoon kan `Ctrl+B` niet versturen | Het onscreen-toetsenbord heeft geen Control-toets, of de SSH-client vangt de snelkoppeling af. | Gebruik de Ctrl-knop van de SSH-client, sluit een Bluetooth-toetsenbord aan, of gebruik de muis of touch-interface als je client dit ondersteunt. (niet geverifieerd) |

Deze oplossingen komen uit de officiële Herdr-documentatie en discussies op GitHub. Ze zijn gemarkeerd als `(niet geverifieerd)` omdat ik ze niet op deze machine heb uitgevoerd.

## Veiligheidsopmerkingen

!!! warning "Loskoppelen is niet stoppen"
    Als je `Ctrl+B q` indrukt, koppel je je client los, maar de Herdr-server en alle panelen blijven draaien. Gebruik `herdr server stop` alleen als je de sessie bewust wilt beëindigen.

- **Draai geen tmux binnen een Herdr-paneel.** Dit verwart de agentdetectie en de zijbalk toont `tmux` in plaats van de echte agent.
- **Laat gevoelige agentsessies niet onbeheerd achter.** Herdr houdt agents actief zelfs als je niet verbonden bent. Vergrendel je apparaat als je even weggaat.
- **Stop de server voor langere pauzes.** Als je klaar bent voor vandaag, voer dan `herdr server stop` uit zodat agents niet onbewaakt blijven draaien.
- **Houd Herdr up-to-date.** Voer regelmatig `herdr update` uit voor installaties die door het Herdr-installatieprogramma worden beheerd, of werk bij via je pakketbeheerder. (niet geverifieerd)

## Telefoon vs laptop

| Taak | Laptop | Telefoon |
|---|---|---|
| Een sessie starten en panelen splitsen | Makkelijk. Toetsenbord en muis werken beide goed. | Moeilijk. Een klein scherm en virtueel toetsenbord maken precisiewerk lastig. |
| Controleren of een agent geblokkeerd of klaar is | Goed. | Goed. De zijbalk van Herdr past op smalle schermen. |
| Een enkele prompt goedkeuren | Goed. | Goed. Eén tik of kort commando. |
| Handoffs of logs lezen | Goed. | Goed voor korte checks. |
| Lange commando's uitvoeren of bestanden bewerken | Goed. | Moeilijk. Complexe commando's typen op een virtueel toetsenbord is foutgevoelig. |
| Beveiligingsbeslissingen | Goed. | Vermijd. Grote diffs, merges en goedkeuring van destructieve acties horen op een laptop. |

Gebruik je telefoon voor snelle statuschecks en goedkeuringen. Doe het zware opzetwerk, programmeren en reviewwerk op je laptop.

## Wat je verder kunt lezen

- [Tailscale-handleiding](tailscale.nl.md) — verbind je telefoon met `studybox` zonder SSH bloot te stellen aan het internet.
- [Termux-handleiding](termux.nl.md) — maak van `student-phone` een SSH-client voor Herdr.

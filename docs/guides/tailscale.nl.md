# Tailscale

## Samenvatting in één zin

Tailscale verbindt je apparaten in één privénetwerk, zodat je telefoon je host vanaf elke locatie zonder poorten open te zetten op je router kan bereiken.

## Wat het doet

Tailscale is een mesh-VPN. Het installeert een kleine daemon op elk apparaat en koppelt ze aan in een privé-"tailnet". Elk apparaat krijgt een stabiel `100.x.y.z`-IP-adres en, als MagicDNS is ingeschakeld, een naam zoals `studybox`. Verkeer tussen je apparaten is end-to-end versleuteld. Je hebt geen publiek IP-adres, statisch IP-adres of port forwarding nodig.

Voor deze handleiding betekent dit dat `student-phone` via het internet naar `studybox` kan SSH'en, alsof ze naast elkaar op tafel liggen.

## Wanneer je het gebruikt

- **SSH op afstand** — Verbind vanaf je telefoon, de laptop van een vriend of een VM terug naar `studybox`.
- **Bestanden synchroniseren of Git push** — Werk met repositories op `studybox` vanaf een ander apparaat.
- **Lokale services bereiken** — Bereik een devserver, notebook of API die op `studybox` draait vanaf je telefoon.
- **Met Herdr werken van buitenaf** — Laat agents op `studybox` draaien en verbind opnieuw vanaf `student-phone`.
- **Port forwarding op je router vermijden** — Je hoeft poort 22 niet naar het internet door te sturen.

Je kunt Tailscale overslaan als beide apparaten op hetzelfde vertrouwde lokale netwerk zitten en je het lokale IP-adres al kent. Zelfs dan blijft Tailscale werken als je naar een ander netwerk gaat.

## Wat je nodig hebt

- Een computer met Linux, macOS of WSL. Deze handleiding noemt hem `studybox`.
- Een Android-telefoon. Deze handleiding noemt hem `student-phone`.
- Beheerdersrechten op `studybox` zodat je `sudo` kunt gebruiken.
- Een Tailscale-account. Je kunt je aanmelden met Google, Microsoft, GitHub of een e-mailadres.
- Ongeveer 15 minuten.
- De voorbeeldgebruikersnaam op `studybox` is `lenny`. Vervang deze door je echte gebruikersnaam als je `lenny@studybox` ziet.
- Een SSH-server die op `studybox` draait. Als je die nog niet hebt geïnstalleerd, volg dan eerst de [student-setup-handleiding](../getting-started/student-setup.nl.md).
- Een SSH-client op `student-phone`. Deze handleiding gebruikt Termux; zie de [Termux-handleiding](termux.nl.md) voor de installatie.

## Installeren en controleren

Deze stappen voer je uit op `studybox`, tenzij anders vermeld.

### 1. Installeer Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

`curl -fsSL` downloadt het installatiescript stil en volgt omleidingen. `| sh` voert het script uit, dat de pakketrepository van Tailscale toevoegt en het commando `tailscale` installeert.

### 2. Start Tailscale en log in

```bash
sudo tailscale up
```

`sudo tailscale up` start de Tailscale-daemon en vraagt je om te authenticeren. Er verschijnt een link in de terminal. Open die in een browser, meld je aan bij je Tailscale-account en keur het apparaat goed. Als je klaar bent, toont de terminal een succesmelding.

Als je op een headless server werkt, gebruik dan:

```bash
sudo tailscale up --operator=$USER
```

`--operator=$USER` geeft je eigen gebruiker beheerrechten over Tailscale, zodat je later niet steeds `sudo` hoeft te typen. Voor de eerste keer heb je nog steeds `sudo` nodig.

### 3. Controleer dat Tailscale draait

```bash
tailscale status
```

De verwachte uitvoer toont je machine en de verbindingsstatus, bijvoorbeeld:

```text
100.x.y.z   studybox            lenny@       linux   -
```

Het exacte IP-adres en de naam kunnen afwijken. Als de status `NeedsLogin` toont, is de authenticatiestap nog niet afgerond.

### 4. Zoek je Tailscale-IP

```bash
tailscale ip -4
```

Verwachte uitvoer is één adres, bijvoorbeeld:

```text
100.x.y.z
```

Dit is het adres dat je vanaf `student-phone` gebruikt om `studybox` te bereiken. Noteer het of bewaar het in je wachtwoordmanager.

### 5. Installeer Tailscale op je telefoon

Op `student-phone`:

1. Installeer de Tailscale-app uit de Google Play Store of van de downloadpagina van Tailscale.
2. Open de app en meld je aan met **hetzelfde account** dat je op `studybox` hebt gebruikt.
3. Laat de schakelaar aan staan om Tailscale verbonden te houden.

De telefoon heeft nu zijn eigen Tailscale-IP en kan andere apparaten in dezelfde tailnet zien.

### 6. Controleer dat beide apparaten elkaar zien

Ping op `studybox` naar het Tailscale-IP van de telefoon:

```bash
ping -c 3 <phone-tailscale-ip>
```

Vervang `<phone-tailscale-ip>` door het adres dat in de Tailscale-app op `student-phone` wordt getoond.

Ping op `student-phone` naar `studybox`:

```bash
ping -c 3 <studybox-tailscale-ip>
```

Een paar antwoorden aan beide kanten betekent dat de tailnet werkt. Als pings falen, ga dan verder naar de onderstaande probleemoplossingstabel.

## Je eerste echte workflow

Zorg dat je [Installeren en controleren](#installeren-en-controleren) al op beide apparaten hebt voltooid. Je moet het Tailscale-IP van `studybox` kennen (we gebruiken de placeholder `100.x.y.z`) en beide apparaten moeten zichtbaar zijn in `tailscale status`.

### 1. SSH vanaf je telefoon

Open Termux op `student-phone` en voer uit:

```bash
ssh lenny@100.x.y.z
```

Vervang `100.x.y.z` door het Tailscale-IP van `studybox` en `lenny` door je gebruikersnaam op `studybox`.

Als dit de eerste keer is dat je verbindt, vraagt SSH om bevestiging van de host-sleutel. Typ `yes` en druk op Enter. Typ daarna je wachtwoord van `studybox` of gebruik je SSH-sleutel.

### 2. Bewijs dat je op `studybox` zit

Voer in de SSH-sessie vanaf je telefoon één of beide commando's uit:

```bash
whoami
hostname
```

`whoami` zou `lenny` moeten afdrukken (of je `studybox`-gebruikersnaam). `hostname` zou `studybox` moeten afdrukken. Dit bevestigt dat het commando op de host draait, niet op je telefoon.

### 3. Controleer dat beide kanten elkaar zien

Terwijl de SSH-sessie open is, voer je uit:

```bash
tailscale status
```

Je zou `studybox` en `student-phone` in de lijst moeten zien, elk met een status zoals `direct` of `relay`.

Ga op `studybox` in een aparte terminal terug en voer uit:

```bash
tailscale status
```

Beide apparaten zouden hier ook moeten verschijnen.

## Veelvoorkomende problemen en oplossingen

De oplossingen hieronder zijn gebaseerd op de Tailscale-documentatie en veelvoorkomende probleemrapporten. Ik heb ze niet allemaal op deze machine getest.

| Symptoom | Waarschijnlijke oorzaak | Oplossing |
|---|---|---|
| `tailscale up` hangt of zegt `NeedsLogin` | Een captive portal, VPN of firewall blokkeert de inlogservers van Tailscale. | Voltooi de authenticatie in een browser op dezelfde machine, of voer `sudo tailscale up` uit vanuit een grafische terminal. Bij een headless server gebruik je `sudo tailscale up --operator=$USER`: `--operator` geeft beheerrechten, zodat latere commando's geen `sudo` nodig hebben, en de getoonde link open je op een ander apparaat om in te loggen. |
| Telefoon en `studybox` verschijnen niet in elkaars `tailscale status` | Ze zijn aangemeld bij verschillende Tailscale-accounts of verschillende tailnets. | Meld je op de telefoon af bij Tailscale en meld je opnieuw aan met exact hetzelfde account als op `studybox`. |
| `ssh lenny@100.x.y.z` faalt met `Connection refused` of `Connection timed out` | De SSH-server draait niet op `studybox`, de host-firewall blokkeert poort 22, of Tailscale-ACL's blokkeren toegang. | Voer op `studybox` `sudo systemctl status ssh` (of `sshd`) uit om de SSH-server te controleren. Voer ook `tailscale status` uit om te bevestigen dat `studybox` is verbonden. Controleer in je Tailscale-beheerconsole of ACL's poort 22 tussen apparaten toestaan. |
| `tailscale status` toont het apparaat, maar `ping` werkt niet | Een firewall of router blokkeert UDP hole punching, waardoor Tailscale geen directe route kan vormen. | Zorg dat uitgaand UDP is toegestaan en poort `41641` niet geblokkeerd is. Als fallback gebruikt Tailscale automatisch een DERP-relay, wat langzamer kan zijn maar meestal wel werkt. |
| De MagicDNS-naam `studybox` wordt niet opgelost | MagicDNS is uitgeschakeld of de DNS-wijziging is nog niet op de telefoon doorgevoerd. | Gebruik het Tailscale-IP (`100.x.y.z`) in plaats van de naam, of schakel MagicDNS in in je Tailscale-beheerconsole en wacht een minuut. Afhankelijk van de beheerinstellingen kan de volledige MagicDNS-naam een tailnet-suffix bevatten, zoals `studybox.<tailnet>.ts.net`. |
| Tailscale verbreekt de verbinding als het telefoonscherm uitgaat | De accu-optimalisatie van Android pauzeert de Tailscale-app. | Open de Android-instellingen, zoek Tailscale in Apps en schakel accu-optimalisatie voor Tailscale uit. |
| `tailscale: command not found` na installatie | Het installatieprogramma heeft de `PATH` van je huidige shell niet bijgewerkt, of de installatie is mislukt. | Open een nieuwe terminal of voer `exec $SHELL -l` uit om je profiel opnieuw te laden. Als het commando nog steeds ontbreekt, voer dan het installatieprogramma opnieuw uit en lees de uitvoer op fouten. |
| Tailscale SSH inschakelen blokkeert normale SSH-sleutelinlog over het Tailscale-IP | Tailscale SSH neemt de SSH-authenticatie over op de Tailscale-interface. | Gebruik het Tailscale-IP met `tailscale ssh user@host`, of verbind via het lokale LAN-IP als je je bestaande SSH-sleutels nodig hebt. |

## Veiligheidsopmerkingen

!!! warning "Open poort 22 niet op je router"
    Tailscale maakt het doorsturen van SSH naar het publieke internet overbodig. Houd poort 22 gesloten op je router en gebruik Tailscale.

- **Gebruik op elk apparaat hetzelfde Tailscale-account.** Een apparaat in een andere tailnet kan `studybox` niet bereiken.
- **Houd Tailscale up-to-date.** Voer op `studybox` `sudo tailscale update` uit wanneer beschikbaar, of werk bij via je pakketbeheerder. (niet geverifieerd)
- **Deel je tailnet niet met mensen die je niet vertrouwt.** Gebruik Tailscale-ACL's om te beperken welke apparaten welke poorten mogen bereiken.
- **Vergrendel je telefoon als je even weggaat.** Eenmaal verbonden heeft je telefoon toegang tot `studybox`.
- **Gebruik in scripts liever het Tailscale-IP.** MagicDNS-namen zijn handig maar kunnen even duren voordat ze zijn doorgevoerd. IP-adressen zijn direct en stabiel.
- **Zet Tailscale uit op openbare Wi-Fi als je het niet nodig hebt.** Dit verkleint de kans dat een gecompromitteerd netwerk met je tailnet kan communiceren.

## Telefoon vs laptop

| Taak | Laptop (`studybox`) | Telefoon (`student-phone`) |
|---|---|---|
| Tailscale installeren en authenticeren | Makkelijk. Volledige browser en terminaltoegang. | Makkelijk via de Tailscale-app, maar wachtwoorden typen op een klein toetsenbord is langzamer. |
| Het Tailscale-IP vinden | Makkelijk. Voer `tailscale ip -4` uit. | Makkelijk. Te zien in de Tailscale-app. |
| SSH configureren en inloggen testen | Makkelijk. Toetsenbord en terminal maken commando's eenvoudig. | Moeilijk. Lange commando's en host-sleutels typen op een virtueel toetsenbord is foutgevoelig. |
| Dagelijkse verbinding | Goed. Start Tailscale en SSH of Herdr normaal. | Goed voor korte checks als Termux en SSH-configuratie eenmaal zijn ingesteld. |
| Probleemoplossingscommando's uitvoeren | Goed. Volledige terminal en kopiëren-plakken. | Moeilijk. Complexe uitvoer is lastig te lezen en bewerken op een klein scherm. |
| Beveiligingsbeslissingen | Goed. | Vermijd. Grote wijzigingen en goedkeuring van destructieve acties horen op de laptop. |

Gebruik de laptop voor installatie, probleemoplossing en zwaar werk. Gebruik de telefoon voor snelle statuschecks en goedkeuringen.

## Wat je verder kunt lezen

- [Herdr-handleiding](herdr.nl.md) — laat programmeeragents op `studybox` draaien en verbind opnieuw vanaf `student-phone`.
- [Termux-handleiding](termux.nl.md) — maak van `student-phone` een bruikbare SSH-client.

# Termux

## Samenvatting in één zin

Termux maakt van je Android-telefoon een kleine Linux-terminal, zodat je via SSH op je studiemachine kunt inloggen en Git-repositories onderweg kunt controleren.

## Wat het doet

Termux is een Linux-achtige terminalomgeving voor Android. Het geeft je tools die je normaal op een laptop gebruikt:

- `ssh` om in te loggen op externe machines zoals je studieserver.
- `git` om repositories te inspecteren, wijzigingen binnen te halen en handoffs te reviewen.
- `nano` of andere editors voor kleine wijzigingen.
- Standaard Unix-commando's (`ls`, `cd`, `cat`, `grep`) zodat je spiergeheugen blijft werken.

Je krijgt geen volledige desktop of IDE, maar wel genoeg om niet vast te lopen als je alleen je telefoon bij je hebt.

## Wanneer je het gebruikt

Gebruik Termux voor korte, laag-risicotaken weg van je laptop:

- `git status` of `git log` uitvoeren op `~/Projects/study-project`.
- Een kleine wijziging goedkeuren na het lezen van de handoff.
- Controleren of een langdurige taak op `studybox` is afgerond.
- Logs of configuratiebestanden onderweg lezen.

Vermijd het voor grote refactors, ingewikkelde merges of alles dat snel typen of een groot scherm vereist.

## Wat je nodig hebt

- Een Android-telefoon of -tablet (`student-phone`).
- Termux geïnstalleerd vanuit F-Droid, niet vanuit Google Play. De Play Store-versie is oud en vaak stuk.
- Je studiemachine (`studybox`) bereikbaar via het netwerk, met SSH al ingeschakeld.
- Je SSH-privésleutel gekopieerd naar de telefoon, of tijdelijk wachtwoordtoegang ingeschakeld.
- Ongeveer vijftien minuten voor de eerste installatie.

## Installeren en controleren

1. Installeer Termux vanuit [F-Droid](https://f-droid.org/packages/com.termux/).
2. Open Termux en werk de pakketlijst bij:

   ```bash
   pkg update && pkg upgrade -y
   ```

   Dit downloadt pakketindexen en werkt geïnstalleerde basispakketten bij.

3. Installeer de tools die je nodig hebt:

   ```bash
   pkg install -y openssh git nano
   ```

   `openssh` geeft de commando's `ssh` en `ssh-keygen`, `git` laat je met repositories werken en `nano` is een eenvoudige editor.

4. Controleer elke tool:

   ```bash
   ssh -V
   git --version
   nano --version
   ```

   Elk commando zou een versienummer moeten afdrukken. Als er een "command not found" afdrukt, voer dan het installatiecommando opnieuw uit.

## Je eerste echte workflow

Doel: vanaf je telefoon inloggen op `studybox` als `lenny` en `git status` uitvoeren in `~/Projects/study-project`.

### 1. Maak of kopieer een SSH-sleutel

Als je al een SSH-sleutelpaar hebt dat je vertrouwt, kopieer dan de privésleutel (bijvoorbeeld `id_ed25519`) naar de home-directory van Termux op `~/.ssh/`. Als je er nog geen hebt, genereer hem op de telefoon:

```bash
ssh-keygen -t ed25519 -C "lenny@student-phone"
```

Als er om een bestand wordt gevraagd, druk je op Enter om de standaard `~/.ssh/id_ed25519` te accepteren. Als er om een wachtwoordzin wordt gevraagd, kies je een wachtwoordzin die je op een telefoon kunt typen, of je drukt twee keer op Enter voor geen wachtwoordzin. Een wachtwoordzin is veiliger; geen wachtwoordzin is handiger.

### 2. Voeg de publieke sleutel toe aan `studybox`

Toon de publieke sleutel:

```bash
cat ~/.ssh/id_ed25519.pub
```

Kopieer de getoonde regel en voeg deze toe aan `~/.ssh/authorized_keys` op `studybox` voor gebruiker `lenny`. Als je dit vanaf de laptop doet, ziet de regel er zo uit:

```text
ssh-ed25519 AAAAC3NzaC... lenny@student-phone
```

Als je deze stap overslaat, kun je nog steeds met een wachtwoord inloggen, maar sleutels zijn veiliger en sneller.

### 3. Verbind met `studybox`

```bash
ssh lenny@studybox
```

Als dit de eerste verbinding is, vraagt Termux om bevestiging van de host-fingerprint. Typ `yes`. Als je een wachtwoordzin hebt ingesteld, voer deze nu in. Je zou op een prompt op `studybox` moeten landen.

### 4. Controleer het project

```bash
cd ~/Projects/study-project
git status
```

Je zou uitvoer moeten zien zoals:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Als je klaar bent, log je uit:

```bash
exit
```

Je bent nu weer op de telefoon. Houd deze werkstroom aan: open Termux, SSH naar `studybox`, voer een snel Git- of statuscommando uit, en verbreek de verbinding.

## Veelvoorkomende problemen en oplossingen

| Symptoom | Oorzaak | Oplossing |
| --- | --- | --- |
| `ssh: command not found` | OpenSSH is niet geïnstalleerd. | Voer `pkg install -y openssh` uit. |
| `Connection refused` bij `ssh lenny@studybox` | De SSH-server op `studybox` draait niet, of een firewall blokkeert poort 22. | Start de SSH-service op `studybox` en controleer dat beide apparaten op hetzelfde netwerk zitten. |
| `Permission denied (publickey)` | De publieke sleutel ontbreekt op `studybox`, of de rechten op de privésleutel zijn verkeerd. | Voeg de publieke sleutel toe aan `~/.ssh/authorized_keys` op `studybox`; voer op de telefoon `chmod 600 ~/.ssh/id_ed25519` uit. |
| Termux sluit direct na het openen | Accu-optimalisatie of een oude Play Store-installatie. | Installeer opnieuw vanuit F-Droid en schakel accu-optimalisatie voor Termux uit in de Android-instellingen. |
| `git status` toont vreemde regeleinden | De instelling `core.autocrlf` van Git verschilt tussen telefoon en laptop. | Stel op `studybox` `git config --global core.autocrlf input` in. |
| Speciale tekens typen verkeerd op het telefoontoetsenbord | Het virtuele toetsenbord verstuurt andere keycodes. | Gebruik een programmeurstoetsenbord zoals Hacker's Keyboard, of plak lange commando's. (niet geverifieerd) |
| Kopiëren-plakken uit Termux werkt niet | Android-versie of Termux-build-probleem. | Houd het scherm lang ingedrukt en kies Plakken; werk Termux via F-Droid bij als het blijft falen. (niet geverifieerd) |

## Veiligheidsopmerkingen

!!! warning "De telefoon is voor korte taken"
    Grote diffs, beveiligingsbeslissingen en risicovolle acties horen op een laptop.

- Bewaar je SSH-privésleutel (`~/.ssh/id_ed25519`) alleen op de telefoon als de telefoon is versleuteld en een vergrendelscherm heeft.
- Sla geen wachtwoorden of API-sleutels op in platte tekst in notities op de telefoon.
- Als je de telefoon kwijtraakt, verwijder dan de sleutel van dat apparaat uit `~/.ssh/authorized_keys` op `studybox`.
- Op openbare Wi-Fi verbind je liever via een VPN of Tailscale dan poort 22 aan het internet bloot te stellen.
- Lees altijd een commando voordat je het plakt; virtuele toetsenborden maken typfouten die makkelijk zijn te maken en lastig te zien.

## Telefoon vs laptop

| Taak | Telefoon met Termux | Laptop |
| --- | --- | --- |
| `git status`, `git log` | Goed | Goed |
| Handoffs lezen | Goed | Goed |
| Grote diffs schrijven of reviewen | Moeilijk | Makkelijk |
| Tests of builds draaien | Traag of onmogelijk | Makkelijk |
| Lange commando's typen | Foutgevoelig | Comfortabel |
| Veilig omgaan met sleutels | Riskanter als de telefoon ontgrendeld is | Meestal veiliger |

Behandel Termux als een metgezel voor alleen-lezen of snelle checks, niet als vervanging van je hoofdmachine voor ontwikkeling.

## Wat je verder kunt lezen

- Lees de [Herdr-handleiding](herdr.nl.md) voor het draaien van achtergrondtaken op je studiemachine.
- Lees de [Tailscale-handleiding](tailscale.nl.md) voor het veilig bereiken van `studybox` vanaf elke locatie zonder poorten open te zetten.

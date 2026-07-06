# Studentenopstelling

Bouw een veilige, mobielvriendelijke werkruimte om te leren en te bouwen met AI-codingagents.

## Wat je gaat bouwen

- Een Linux/macOS/WSL-host met SSH en Git.
- Een privé Tailscale-netwerk, zodat je telefoon je host kan bereiken zonder publieke port forwarding.
- Herdr als blijvende terminalwerkruimte voor codingagents.
- Termux op Android als mobiele SSH-client.
- StudyDD- en StateDD-workflows om leren en projectstatus in de repo te houden.

## Voordat je begint

Je hebt nodig:

- Een computer met Linux, macOS of WSL.
- Een Android-telefoon.
- Een GitHub- of GitLab-account.
- Ongeveer 45 minuten voor de eerste opstelling.

!!! warning "Stel SSH niet bloot aan het publieke internet"
    Gebruik Tailscale of je lokale netwerk. Forward nooit poort 22 op je router voor deze opstelling.

## 1. Installeer Git en SSH op de host

### Debian / Ubuntu / WSL

```bash
sudo apt update
sudo apt install -y git openssh-server curl
sudo systemctl enable --now ssh || true
```

### Fedora

```bash
sudo dnf install -y git openssh-server curl
sudo systemctl enable --now sshd
```

### macOS

```bash
xcode-select --install
```

Schakel daarna Remote Login in Systeemvoorkeuren in als je SSH-toegang wilt.

## 2. Installeer Tailscale

Op de host:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Controleer:

```bash
tailscale status
tailscale ip -4
```

Optioneel: Tailscale SSH (alleen als je tailnet-beleid dit toestaat):

```bash
sudo tailscale up --ssh
```

## 3. Installeer Herdr

```bash
curl -fsSL https://herdr.dev/install.sh | sh
```

Start Herdr in je projectmap:

```bash
cd ~/Projects/mijn-project
herdr
```

Koppel los zonder de sessie te stoppen: druk op `Ctrl+B`, daarna `q`.
Stop de server bewust met:

```bash
herdr server stop
```

## 4. Installeer een coding agent

Voorbeeld: OpenCode.

```bash
curl -fsSL https://opencode.ai/install | bash
```

Of met npm:

```bash
npm install -g opencode-ai
```

Start het in je project:

```bash
cd ~/Projects/mijn-project
opencode
```

## 5. Stel Termux in op Android

Installeer:

- Tailscale via Google Play of de officiële downloadpagina.
- Termux, bij voorkeur via F-Droid.

Open Termux en werk de pakketten bij:

```bash
pkg update
pkg upgrade
pkg install -y openssh git nano
```

Test de verbinding met je host:

```bash
ssh gebruikersnaam@machine-naam
```

Of met een MagicDNS-naam:

```bash
ssh gebruikersnaam@studybox
```

Of met het Tailscale-IP:

```bash
ssh gebruikersnaam@100.x.y.z
```

## 6. Maak SSH makkelijker

Maak een SSH-config in Termux:

```bash
mkdir -p ~/.ssh
nano ~/.ssh/config
```

Voorbeeld:

```sshconfig
Host studybox
  HostName studybox
  User jouw-linux-gebruiker
  ServerAliveInterval 30
  ServerAliveCountMax 3
```

Verbind daarna met:

```bash
ssh studybox
```

## 7. Open je Herdr-werkruimte vanaf je telefoon

```bash
ssh studybox
cd ~/Projects/mijn-project
herdr
```

Je ziet nu dezelfde Herdr-werkruimte als op je laptop.

!!! tip "Gebruik je telefoon alleen voor korte taken"
    Handig om te controleren of een agent vastzit, een snelle goedkeuring te geven of een handoff te lezen. Grote diffs, merges en beveiligingsbeslissingen horen op een laptop.

## 8. Voeg StudyDD en StateDD toe aan je repo

Maak de minimale bestandsset in je projectroot:

```text
study-project/
  AGENTS.md
  STATUS.md
  NEXT_ACTIONS.md
  BACKLOG.md
  WORKLOG.md
  PROJECT_STATE.yaml
  SOURCES.md
  REVIEW.md
  evidence/
    README.md
```

Minimale regel: de agent moet `AGENTS.md`, `STATUS.md`, `NEXT_ACTIONS.md` en `BACKLOG.md` lezen voordat hij aan het werk gaat.

## 9. Eerste correct-runopdracht

Bewijs dat je een agentworkflow veilig kan starten en afsluiten:

1. Screenshot van Herdr met minstens één agentpaneel.
2. Screenshot of tekst van `git status --short`.
3. Korte `WORKLOG.md`-entry.
4. Een `evidence/`-map met `runtime.md`.
5. Antwoord: wat is de bron van waarheid in je repo?
6. Antwoord: waarom is "de agent zei dat het klaar was" niet genoeg?

## Wat nu?

- Lees de gidsen over [Herdr](../guides/herdr.nl.md), [Tailscale](../guides/tailscale.nl.md) en [Termux](../guides/termux.nl.md) voor meer opstelhulp.
- Lees [StudyDD](../guides/studydd.nl.md) voor repo-first leren.
- Lees [StateDD](../guides/statedd.nl.md) voor repo-first projectwerk.
- Lees [Safety Boundaries](../safety/boundaries.nl.md) voordat je een agent aan echte code laat werken.

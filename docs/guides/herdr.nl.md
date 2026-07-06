# Herdr

Herdr is een terminal multiplexer ontworpen voor programmeeragents. Hij houdt vensters en agents actief bij verbroken verbindingen.

## Wat Herdr doet

Houdt permanente terminalwerkruimtes bij met zichtbare agentstatus.

## Wanneer te gebruiken

- Langdurige agenttaken
- Op afstand werken
- Wisselen tussen laptop en telefoon

## Basisinstallatie

```bash
curl -fsSL https://herdr.dev/install.sh | sh
```

## Veiligheidsopmerkingen

!!! warning "Afsluiten is niet stoppen"
    Afsluiten met Ctrl+B q houdt de sessie actief. Gebruik `herdr server stop` om bewust af te sluiten.

## Veelgemaakte fouten

- tmux binnen Herdr draaien verwart de statusdetectie
- De terminal stoppen in plaats van af te sluiten, doodt je werk

## Volgende stap

- Lees de [Tailscale-gids](tailscale.nl.md) en de [studentensetupgids](../getting-started/student-setup.nl.md)

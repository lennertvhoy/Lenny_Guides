# Grenzen

Een coding agent is een snelle junior collega in je terminal. Die heeft context, grenzen en review nodig.

## De niet-onderhandelbare regels

- **Repo eerst.** De Git-repo is de bron van waarheid. Herdr, StudyDD en StateDD organiseren werk alleen rondom de repo.
- **Eerst plannen, dan bouwen.** Voor elk niet-triviale taak stelt de agent een plan voor voordat hij bestanden wijzigt.
- **Bewijs vóór closure.** Een taak is niet af omdat de agent dat zegt. Je hebt tests, screenshots, logs of een clean diff nodig.
- **Geen secrets in chat of Git.** API-keys, tokens en wachtwoorden horen niet in prompts, screenshots of commits.
- **Eén taak tegelijk.** Parallelle agents alleen als ze niet dezelfde bestanden of beslissingen aanraken.
- **De mens blijft reviewer.** De agent stelt voor; jij accepteert of wijst af.

## Wat je de agent niet moet laten doen

```text
- API-keys plakken in Discord of prompts
- .env-bestanden committen
- Screenshots maken waarop tokens zichtbaar zijn
- SSH blootstellen via router port forwarding
- Force push uitvoeren
- Wachtwoorden wijzigen
- Productiedata lezen zonder toestemming
- Shell-commando's uitvoeren zonder dat jij ze begrijpt
```

## Wat je in plaats daarvan doet

```text
- Gebruik .env.example zonder echte secrets
- Houd .gitignore schoon
- Gebruik Tailscale voor privétoegang
- Gebruik kleine branches
- Lees git diff voor het committen
- Behandel agent-uitvoer als een voorstel, niet als waarheid
- Vraag om bewijs vóór closure
```

## Voorbeeld .gitignore

```gitignore
.env
.env.*
!.env.example
*.key
*.pem
secrets/
node_modules/
.venv/
```

## gsm versus laptop

- **gsm:** geschikt voor status bekijken, korte goedkeuringen en handoffs lezen.
- **Laptop:** vereist voor grote diffs, mergeconflicten, beveiligingsbeslissingen en complexe refactors.

## Onthoud

Repo eerst. Eerst plannen. Bewijs vóór closure.

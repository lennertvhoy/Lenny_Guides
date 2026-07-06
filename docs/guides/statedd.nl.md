# StateDD

StateDD is een repo-eerst projectworkflow.

## Wat StateDD doet

Slaat projectstatus, backlog, beslissingen, validatie, bewijs en handoffs in de repo op.

## Wanneer te gebruiken

- Elk project waarin een AI-agent code schrijft of wijzigt

## Basisinstallatie

Voeg deze bestanden toe aan je projectrepo:

```bash
AGENTS.md
STATUS.md
PROJECT_STATE.yaml
PROJECT_DNA.yaml
NEXT_ACTIONS.md
BACKLOG.md
WORKLOG.md
docs/EVIDENCE_LOG.md
evidence/
```

## Veiligheidsopmerkingen

!!! warning "Geïmplementeerd is niet gevalideerd"
    De agent moet een wijziging bewijzen met tests, runtime-bewijs of screenshots.

## Veelgemaakte fouten

- "Ik ben klaar" accepteren zonder bewijs
- De agent StateDD-bestanden laten herschrijven zodat ze zijn eigen claim bevestigen

## Volgende stap

- Lees de [pagina Veiligheidsgrenzen](../safety/boundaries.nl.md)

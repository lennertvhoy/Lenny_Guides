# Tailscale

Tailscale creëert een privé meshnetwerk tussen je apparaten.

## Wat Tailscale doet

Laat je telefoon met je host verbinden via een versleutelde tunnel zonder publiek IP-adres bloot te geven.

## Wanneer te gebruiken

- SSH op afstand
- Bestandssynchronisatie
- Toegang tot services tussen apparaten

## Basisinstallatie

```bash
curl -fsSL https://tailscale.com/install.sh | sh && sudo tailscale up
```

## Veiligheidsopmerkingen

!!! warning "Open poort 22 niet op je router"
    Tailscale maakt port forwarding overbodig.

## Veelgemaakte fouten

- Vergeten de telefoon in dezelfde tailnet te authenticeren
- Vertrouwen op MagicDNS-namen voordat ze zijn doorgevoerd

## Volgende stap

- Lees de [Termux-gids](termux.nl.md) en de [studentensetupgids](../getting-started/student-setup.nl.md)

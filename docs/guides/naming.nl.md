# Stateware-naamgeving en compatibiliteit

De publieke naamgeving maakt onderscheid tussen de categorie, engineeringmethode,
specificatie, het platform en de applicaties:

| Naam | Betekenis |
|---|---|
| **Stateware** | Software waarin duurzame, inspecteerbare status en beheerste levenscycluscontracten de applicatiegrens vormen, terwijl agents en modellen vervangbare verwerkers blijven. |
| **State-Centric Engineering** | De praktijk van systemen bouwen rond canonieke status, autoriteitsgrenzen, reproduceerbare uitvoering en afsluiting met bewijs. |
| **StateSpec** | De draagbare bestanden, schema's, eigendomsregels, levenscycluscontracten, contextopbouw, validatie en bewijsvereisten. |
| **StatePort** | Het runtime- en levenscyclusplatform. |
| **StateBench** | Het evaluatiesysteem. |
| **StudyState** | De leerapplicatie/-template. |
| **ClassState** | De klasapplicatie/-template. |

## Waarom oude namen nog voorkomen

Publieke tekst gebruikt voortaan de namen hierboven. Bestaande reponamen,
bestandspaden, package-/importnamen, omgevingsvariabelen, schema-identifiers,
opgeslagen velden, Git-geschiedenis en URL's blijven compatibiliteitscontracten
tot een apart geversioneerde migratie ze veilig wijzigt. Daarom kunnen de
StudyState-repository en stabiele gids-URL's nog `StudyDD` of `StateDD` bevatten.

Hernoem die identifiers niet handmatig. Een fysieke migratie vereist aliassen,
upgradetests, rollback en een expliciet uitfaseringsschema.

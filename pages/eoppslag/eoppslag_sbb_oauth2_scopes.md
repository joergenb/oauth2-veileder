---
layout: page
title: "Retningslinjer for eOppslag ressursdefinisjoner"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_scopes.html

summary: "Retningslinjer for navngiving av ressursdefinisjoner (i form av Oauth2 scopes)"
---

På denne siden gir vi retningslinjer for hvordan API-tilbydere skal navngi sine Oauth2 scopes.

## Diskusjon

En API-tilbyder tilbyr data gjennom ett eller flere APIer, potensielt realisert med flere API-endepunkter. Responsen til en konsument kan være "rik" og "tynn" alt etter hvilke ressurser konsumenten etterspør, eller hvilke rettigheter konsumenten har (slike rettigheter er typisk, men ikke nødvendigvis, hjemlet i lov).

`scope`-begrepet er sentralt i Oauth2 sin autorisasjonsmodell.  Et scope kan best beskrives som en *ressurs-definisjon*.  Noen slike ressurser vil i praksis være entydig koblet mot den organisasjonen som forvalter et datasett (eksempel Bostedsadresse i Folkeregisteret), men andre ressurser kan sees på som globale der mange organisasjoner kan tenkes å kunne levere ut en fornuftig respons som svar på en forespørsel etter den aktuelle ressursen. Vi ser at det da ikke nødvenvendigvis er en 1:1 mapping mellom API-tilbyder og scope.

Et annet aspekt er at i noen sektorer, er det standarderer som definerer hvilke scopes som skal brukes.   Et typisk eksempel her er den internasjonale Smart-on-FHIR standarded i helsesektoren, som definerer scope etter syntaksen:
`clinical-scope ::= ( 'patient' | 'user' ) '/' ( fhir-resource | '*' ) '.' ( 'read' | 'write' | '*' )`
(se [http://docs.smarthealthit.org/authorization/scopes-and-launch-context/](http://docs.smarthealthit.org/authorization/scopes-and-launch-context/))
Andre sektorer kan følge andre syntakser som ikke er kompatible, og vi ser da at det ikke er realistisk å skulle *tvinge* en bestemt syntaks i norsk offentlig forvaltning.

## Retningslinjer

I eOppslag legger vi opp til en syntaks:
`scope ::= prefix ':' subscope`

der `prefix` er en tekststreng som blir manuelt tildelt en virksomhet. En virksomhet kan ha flere prefix.  Eksempel på prefix kan være `nav` eller `skatt`.   Det er mulig å full-automatisere prosessen med å bli en API-tilbyder dersom prefix settes lik  organisasjonsnummeret fra  virksomhetsssertikatetet.

Difi ønsker å gi API-tilbydere stor frihet til å selv bestemme hvilken semantikk de har behov for.  Samtidig mener vi at som hovedregel bør API-tilbyder (A) være koda inn i scopet gjennom prefix (døme 'nav').

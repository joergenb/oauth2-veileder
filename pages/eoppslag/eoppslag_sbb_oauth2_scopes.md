---
layout: page
title: "Retningslinjer for eOppslag scope"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_scopes.html

summary: "Retningslinjer for navngiving av Oauth2 scopes i eOppslag"
---

På denne siden gir vi retningslinjer for hvordan API-tilbydere skal navngi sine Oauth2 scopes.

`scope`-begrepet er sentralt i Oauth2 sin autorisasjonsmodell. Et scope kan best beskrives som en *ressurs-definisjon*. Tokens er som hovedregel knyttet (og derigjenomm begrenset) til et eller flere scopes.


## Oppsummering

tbw

## Typer scopes

Noen scopes  vil i praksis være entydig koblet mot den organisasjonen som forvalter et datasett (eksempel Bostedsadresse i Folkeregisteret), men andre ressurser kan sees på som globale der mange organisasjoner kan tenkes å kunne levere ut en fornuftig respons som svar på en forespørsel etter den aktuelle ressursen. Vi ser at det da ikke nødvendigvis er en 1:1 mapping mellom API-tilbyder og scope.


En API-tilbyder tilbyr data gjennom ett eller flere APIer, potensielt realisert med flere API-endepunkter. Responsen til en konsument kan være "rik" og "tynn" alt etter hvilke ressurser konsumenten etterspør, eller hvilke rettigheter konsumenten har (slike rettigheter er typisk, men ikke nødvendigvis, hjemlet i lov).

## Syntaks

https://tools.ietf.org/html/rfc6749?#section-3.3
```
     scope       = scope-token *( SP scope-token )
     scope-token = 1*( %x21 / %x23-5B / %x5D-7E )
```


I noen sektorer er det standarderer som definerer hvilke scopes som skal brukes.  Et typisk eksempel her er den internasjonale Smart-on-FHIR standarden i helsesektoren, som definerer scope etter syntaksen:

```
clinical-scope ::= ( 'patient' | 'user' ) '/' ( fhir-resource | '*' ) '.' ( 'read' | 'write' | '*' )
```


(se [http://docs.smarthealthit.org/authorization/scopes-and-launch-context/](http://docs.smarthealthit.org/authorization/scopes-and-launch-context/))
Andre sektorer kan følge andre syntakser som ikke er kompatible med denne syntaksen, og vi ser da at det ikke er realistisk å skulle *tvinge* en bestemt syntaks i norsk offentlig forvaltning.

### Retningslinjer

I eOppslag legger vi opp til en syntaks:

`scope ::= prefix ':' subscope`

der `prefix` er en tekststreng som blir manuelt tildelt en virksomhet. En virksomhet kan ha flere prefix.  Eksempel på prefix kan være `nav` eller `skatt`.   Det er mulig å full-automatisere prosessen med å bli en API-tilbyder dersom prefix settes lik  organisasjonsnummeret fra  virksomhetsssertikatetet.

Difi ønsker å gi API-tilbydere stor frihet til å selv bestemme hvilken semantikk de har behov for.  Samtidig mener vi at som hovedregel bør API-tilbyder (A) være koda inn i scopet.  Vi får da:

- prefix bør identifisere din virksomhet  (for eksempel `nav` eller `folkeregisteret`)
    - dersom flere virksomheter bruker samme scope, bør prefix være sektoridentifiserende (`forsikring`)
- subscope bør identifisere ressursen best mulig (`trygdeopplysninger` eller `adresse`)
- subscope kan gjerne ha ulike postfix for å skille på lese- og skrive-tilgang til ressursen (`user/spraak.write`)
     - fravær av postfix bør i utgangspunktet tolkes som kun lese-tilgang

## Forhold til API-katalogen

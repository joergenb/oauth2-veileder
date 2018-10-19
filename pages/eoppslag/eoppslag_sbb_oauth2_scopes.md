---
layout: page
title: "Retningslinjer for Oauth2 scope i eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_scopes.html

summary: "Retningslinjer for navngiving av Oauth2 scopes i eOppslag"
---

På denne siden gir vi retningslinjer for hvordan API-tilbydere skal navngi sine Oauth2 scopes.

`scope`-begrepet er sentralt i Oauth2 sin autorisasjonsmodell. Et scope kan best beskrives som en *ressurs-definisjon*. Tokens er som hovedregel knyttet (og derigjenomm begrenset) til et eller flere scopes.


## Oppsummering

tbw

## Typer ressurser og scopes

Noen scopes/ressursdefinisjoner  vil i praksis være entydig koblet mot den organisasjonen som forvalter et datasett (eksempel Bostedsadresse i Folkeregisteret), men andre ressurser kan sees på som globale der mange organisasjoner kan tenkes å kunne levere ut en fornuftig respons som svar på en forespørsel etter den aktuelle ressursen. Vi ser at det da ikke nødvendigvis er en 1:1 mapping mellom API-tilbyder og scope.

En API-tilbyder tilbyr data gjennom ett eller flere APIer, potensielt realisert med flere API-endepunkter. Tilbyderen kan velge å beskytte flere endepunkter bak samme scope.  Tilbyderen kan også velge å ha ett endepunkt, og bruke granulære/hierariske scopes for å gi konsumenten "rike" eller "tynne" responser. Motivasjonen kan være dataminimering, enten fordi konsumenten kan få lov til å bestemme hvilke ressurser som behøves, eller hvilke rettigheter konsumenten har.

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

### Forslag

I eOppslag legger vi opp til en syntaks:

```
scope ::= prefix ':' subscope
```

der `prefix` er en tekststreng som blir manuelt tildelt en virksomhet. En virksomhet kan ha flere prefix.  Eksempel på prefix kan være `nav` eller `skatt`. Å bruke organisasjonnummer kan i noen sammenhenger være nyttig, siden det kan legge til rette for automatiserte prosesser. I andre sammenhenger vil ikke organisasjonsnummer være tilstrekkelig granulært for store virksomheter.

Difi ønsker å gi API-tilbydere stor frihet til å selv bestemme hvilken semantikk de har behov for.  Samtidig mener vi at som hovedregel bør API-tilbyder (A) være koda inn i scopet. (Merk at )

Vi får da:

- prefix bør identifisere din virksomhet  (for eksempel `nav` eller `folkeregisteret` eller organisasjonsnummer)
    - dersom flere virksomheter bruker samme scope, bør prefix være sektoridentifiserende (`forsikring`)
- subscope bør identifisere ressursen best mulig (`trygdeopplysninger` eller `adresse`)
- subscope kan gjerne ha ulike postfix for å skille på lese- og skrive-tilgang til ressursen (`user/spraak.write`)
     - fravær av postfix bør i utgangspunktet tolkes som kun lese-tilgang

## Forhold til API-katalogen

![](pages/eoppslag/assets/eoppslag_sbb_oauth2_scopes-a3bd7cfe.png)




"ting i API-katalogen, eOpplag, Maskinporten, Altinn,...  må henge sammen"

begrep:
- 1 API-tilbyder har flere API
- 1 API = 1 API i api-katalog = 1 OAS-fil
- 1 API har mange eOpplag scopes
    - 1 eOppslag scope = 1 Oauth2 scope
  - 1 oauth2 scope = 1 scope i API-katalog



regler:


- APIet skal vere tilgjengeleg på 1 og berre 1 domene
    - Produksjons-APIet skal angis med OAS definsjonen /Server/url/description="prod"
    - Test-API tilsvarande med "test"




|OAS|Oauth2|Døme|Kommentar|lenker|
|-|-|-|-|-|
| /Server/url   | `aud` (og `iss`) | https://protected.example.net/resource | i Oauth2 blir 'aud' og 'iss' ofte satt til client-id, men for en ressursserver (API) bør en heller bruke baseurl til APIet.  | https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.1.md#serverObject https://tools.ietf.org/html/draft-ietf-oauth-resource-indicators-00  |
|   |   |   |   |
| /Server/description   |   | 'prod' for URL til produksjonsmiljø, 'test' for testmiljøer  |



#### døme frå krr
![](pages/eoppslag/assets/eoppslag_sbb_oauth2_scopes-a89fba48.png)

swagger-fil:

```
openapi: 3.0.0
info:
  title: Eit fint API
  version: 0.1.0

servers:
  - url: https://oidc.difi.no/kontaktinfo-oauth2-server/rest
    description: "prod"
  - url: https://oidc-ver2.difi.no/kontaktinfo-oauth2-server/rest
    description: "test"


components:

  securitySchemes:
    person_auth_les:
      type: oauth2
      flows:
        authorizationCode:
          authorizationUrl: "https://oidc.difi.no/idporten-oidc-provider/authorize"
          tokenUrl: "https://oidc.difi.no/idporten-oidc-provider/token"
          scopes:
            user/spraak.read: Gir tilgang til innlogga brukar sin språkpreferanse
    person_auth_skriv:
      type: oauth2
      flows:
        authorizationCode:
          authorizationUrl: "https://oidc.difi.no/idporten-oidc-provider/authorize"
          tokenUrl: "https://oidc.difi.no/idporten-oidc-provider/token"
          scopes:
            user/spraak.write: Gir tilgang til å oppdatere innlogga brukar sin språkpreferanse
    maskin_auth:
      type: oauth2
      flows:
        x-urn:ietf:params:oauth:grant-type:jwt-bearer:
          tokenUrl: "https://oidc.difi.no/idporten-oidc-provider/token"
          scopes:
            global/spraak.read: Gir tilgang til å lese personar sin språkpreferanse

  schemas:
    ContactInfoResource:
      title: ContactInfoResource
      type: object
      properties:
        epostadresse:
          type: string
        epostadresse_oppdatert:
          type: string
        epostadresse_sist_verifisert:
          type: string
        mobiltelefonnummer:
          type: string
        mobiltelefonnummer_oppdatert:
          type: string
        mobiltelefonnummer_sist_verifisert:
          type: string

    PersonResource:
      title: PersonResource
      description: 'Personobjekt i Kontaktregisteret.  Kva felt ein får ut i responsen er avhengig av kva scope ein har, sjå https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_krr.html#tilgjenglige-scopes'
      type: object
      properties:
        personidentifikator:
          type: string
        reservasjon:
          type: string
          enum:
            - JA
            - NEI
        status:
          type: string
          enum:
            - AKTIV
            - SLETTET
            - IKKE_REGISTRERT
        varslingsstatus:
          type: string
          enum:
            - KAN_IKKE_VARSLES
            - KAN_VARSLES
        kontaktinformasjon:
          $ref: '#/components/schemas/ContactInfoResource'
        sertifikat:
          type: string
        spraak:
          type: string
        spraak_oppdatert:
          type: string

  responses:
    "200":
      description: Vellukka oppslag på ein person i KRR
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/PersonResource'

paths:
  /v1/person:
    get:
      summary: Hent KRR-data for brukaren knytt til access token
      security:
        - person_auth_les:
      responses:
        PersonResource

    patch:
      summary: Oppdatere språkvalg for brukaren knytt til access token
      security: person_auth_skriv
  /v1/personer:
    post:
      summary: Hent fra 1..1000 personer fra kontaktregisteret. Respons avhengig av hvilke scopes som er brukt
      security: maskin_auth




```

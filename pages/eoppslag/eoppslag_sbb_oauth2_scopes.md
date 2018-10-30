---
layout: page
title: "Retningslinjer for Oauth2 scope i eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_scopes.html

summary: "Retningslinjer for navngiving av Oauth2 scopes i eOppslag"
---

På denne siden gir vi retningslinjer for hvordan API-tilbydere skal navngi sine Oauth2 scopes.

## Oppsummering

tbd


`scope`-begrepet er sentralt i Oauth2 sin autorisasjonsmodell. Et scope kan best beskrives som en *ressurs-definisjon*. Tokens er som hovedregel knyttet (og derigjenomm begrenset) til et eller flere scopes.


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


##

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
- subscope kan gjerne ha ulike postfix for å skille på lese- og skrive-tilgang til ressursen (`navr:trygdeopplysninger.write`)
     - fravær av postfix bør i utgangspunktet tolkes som kun lese-tilgang

## Forhold til API-katalogen

![](pages/eoppslag/assets/eoppslag_sbb_oauth2_scopes-a3bd7cfe.png)




>"ting i API-katalogen, eOpplag, Maskinporten, Altinn,...  må henge sammen"

Samspel på tvers er mulig, med antatt overkommelig innsats…
- ved å bruke begreper fra Open-API-spesifikasjonen som “styrende” for Altinn og Maskinporten
- ved å støtte et begrenset utfallsrom fra Outh2 og Open-API
- ved å la et API-tilbyder bestemme hvor konsument kan utføre delegering


Følges ikke reglene over, kan aktørene fremdeles bruke komponentene i sin tjenesteproduksjon, men man mister på-tvers-funksjonaliteten


#### sekvensdiagram for brukerreisene:

fra workshop 2018-10-30:



<div class="mermaid">
sequenceDiagram

  participant AP as API-katalog
  participant AA as Altinn Autorisasjon
  participant MP AS maskinporten
  participant C AS konsument(C)
  participant L AS Levernadør(L)
  participant A as API-tilbyder

  note right of A: Registrering av API   
  A->>AP: registrer (OAS-fil, eOppslag=True)
  AP->>AA: registrer (securityScheme,scope)
  AP->>MP: registrer (scope)

  alt direkte-registrering
   A->>MP: registrer (scope)
   MP->>AA: registrer (securityScheme)
  end

  note right of A: 1:gi tilgang
  A->>MP: /acccess (securityScheme, consumer_orgno)

  note right of A: 4: Leikanger kommune...
  note over AA,C: Bemyndiget person logger inn i Altinn og delegerer
  alt D2. oppdatere
  AA->>MP: /delegations (scope, consumer, supplier)
  end

  note right of A: 4: Token-utstedelse  run-time

  note over L,MP: tokenforespørsel fra leverandør
  L->>MP: /token(scope, consumer_orgno)
  MP->>AA: kontroller delegering(scope, consumer_orgno)
  MP->>L: access_token
  L->>A: API-request(token)

  note over C,MP: tokenforespørsel direkte fra konsument
  C->>MP: /token(scope)
  note right of MP: Siden oauth2-klient ikke oppgir hvem den ønsker å opptre på vegne av, trenger ikke Maskinporten sjekke delegering i Altinn
  MP->>C: access_token
  C->>A: API-request(token)


</div>


Regler som må følges dersom API-tilbyder ønsker at benytte seg av automagisk samspill mellom API-katalog, Altinn Autorisasjon og Maskinporten:

- 1 API-tilbyder kan ha flere API
- 1 API i API-katalogen = 1 OAS-fil
- 1 OAS-fil kan ha fleire securitySchemes
  - SecuritySchemes av typen `x...jwt-grant` blir håndtert av eOppslag
    - Blir en "delegerbar" ressurs i Altinn
  - SecuritySchemes må navngis etter følgende konvensjon: **TBD** ( prefix.  / delegeringskilde / bruksområde / operasjon…  `nav:trygdeopplysninger[d:altinn, u:oppslag]`)
  - 1 securityScheme kan inneholde flere oauth2 scopes
    - scope må navngis etter følgende konvensjon: **TBD**
      - mulig å inkludere securityScheme-navnet ?   (`securityScheme :: subscope`)  
      - hvordan sikre hierarkiske scopes?
    - scopes som blir definert direkte på /path/operation i OAS-fila blir ignorert av eOppslag
      - mao ikke synlig i API-katalogen, ingen på-tvers logikk mellom Altinn Autorisasjon og Maskinporten



Eksempel:
```
securitySchemes:
    krr_read:
      type: oauth2  
      x-title: Lesetilgang til oppføring i kontakt- og reservasjonsregisteret
      x-description: BLa bla blab lba
      x-delegation-source: altinn
      x-delegation-data:
        roles:
          - DAGL
          - UTINN
      flows:
        x-urn:ietf:params:oauth:grant-type:jwt-bearer:
        tokenUrl: "https://oidc.difi.no/idporten-oidc-provider/token"
        scopes:
          user/kontaktinfo.read: Read KRR information
          user/varsling.read: Read notifciation information

```


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

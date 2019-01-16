---
layout: page
title: "Bruk av OpenAPI-spesifikasjonen i eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_oas.html

summary: "Hvordan OpenAPI-sesifikasjonen blir bruk av eOppslag for å sikre samspill på tvers av fellesløsninger"
---

* For å oppnå samspill med API-katalogen, må vi
  - innføre noen norske utvidelser til OpenAPI-spesifikasjon
  - definere en konvensjon for hvordan bruke OAS

![](assets/eoppslag_sbb_oauth2-cd1afddf.png)

Ett API kan inneholde en eller flere delegerbare ressurser,  og et Oauth2 scope kan tilhøre et eller flere delegerbare ressurser.  Token-tjenesten forholder seg kun til oauth2 scopes. Delegeringskilden forholder seg primært til delegerbare ressurser, men er også ansvarlig for å vedlikeholde hvilke scopes som hører til en delegerbar ressurs.

## Norske utvidelser til OAS:

Følgende elementer innføres under `securityScheme/type` :
* `x-title`: Tittel på den delegerbare ressursen
* `x-description`: Beskrivelse av den delegerbare ressursen
* `x-delegation-source`: Delegeringskilden sin identifikator  (dersom delegeringskilde snakker Token Exchange -> issuer? eller url til well-known-endepunkt?)

Bruk av RFC7523 er ikke del av OAS, så vi må også innføre en ny Oauth2 `flow` i OAS:
* `x-urn:ietf:params:oauth:grant-type:jwt-bearer`: IANA-registrert identifikator for JWT-baserte Oauth2-grants

Her er et eksempel:
```
securitySchemes:
    trygdeopplysninger:
      type: oauth2
      x-title: API for trygdeopplysninger hos NAV
      x-description: Gir anledning til å gjøre leseoperasjoner i API for trygdeopplysninger hos NAV
      x-delegation-source: altinn
      flows:
        x-urn:ietf:params:oauth:grant-type:jwt-bearer:
        tokenUrl: "https://oidc.difi.no/idporten-oidc-provider/token"
        scopes:
          nav:trygdeopplysninger: Read access to information about social benefits
```

## Konvensjon for bruk av OAS:

Konvensjonen er som følger:
- 1 API-tilbyder kan ha flere API
- 1 API i API-katalogen = 1 OAS-fil
- `securityScheme` i OAS = "delegerbar ressurs" i Altinn
  - 1 API ha ett eller fleire securitySchemes
  - securitySchemes av typen `x...jwt-grant` blir håndtert av eOppslag
  - securitySchemes må navngis etter følgende konvensjon: **TBD** ( prefix.  / delegeringskilde / bruksområde / operasjon…  `nav:trygdeopplysninger[d:altinn, u:oppslag]`)
- 1 securityScheme må inneholde ett eller flere oauth2 scopes
  - scope må navngis etter følgende konvensjon: **TBD**
  - scopes som blir definert direkte på /path/operasjoner i OAS-fila blir ignorert av eOppslag
    - mao ikke synlig i API-katalogen, ingen på-tvers logikk mellom Altinn Autorisasjon og Maskinporten

Ellers kan man merke seg:
- securityScheme heter `delegationScheme` på eOppslag-grensesnittene
- API-tilganger gis til scopes
  - API-tilbyder må sende ett tilgangs-kall per scope under et delegationScheme
- Maskinporten kjenner ikke til "delegationSchemes", lagrer kun eventuell "delegationSource" per scope
- tokens og -forespørsler forholder seg ikke til delegationSchemes





## Begreper

* Konsumenter
* API-tilbyder
* Leverandør
* Token-tjeneste
* Delegeringskilde
* API-katalog
* Klient
* Tjeneste




Vi ønsker også å sikre samspill med API-katalogen (Felles datakatalog), slik at

### Samspill med API-katalogen

For å sikre samspill og rask implementerbarhet mellom valgte teknologier og eksisterende fellesløsninger, innfører vi en  3-nivå konvensjon basert på eksisterende elementer i OpenAPI-spesifikasjonen:
* Ett API = en OpenAPI-fil
* Delegerbar ressurs = `securityScheme`
* Scope =



- 1 API-tilbyder kan ha flere API
- 1 API i API-katalogen = 1 OAS-fil
- Ihht OAS-standarden kan et API ha ett eller fleire securitySchemes
    - securitySchemes av typen `x...jwt-grant` blir håndtert av eOppslag
    - Blir en "delegerbar" ressurs i Altinn
  - SecuritySchemes må navngis etter følgende konvensjon: **TBD** ( prefix.  / delegeringskilde / bruksområde / operasjon…  `nav:trygdeopplysninger[d:altinn, u:oppslag]`)
- 1 securityScheme må inneholde ett eller flere oauth2 scopes
  - scope må navngis etter følgende konvensjon: **TBD**
    - mulig å inkludere securityScheme-navnet ?   (`securityScheme :: subscope`)  
    - hvordan sikre hierarkiske scopes?
  - scopes som blir definert direkte på /path/operation i OAS-fila blir ignorert av eOppslag
    - mao ikke synlig i API-katalogen, ingen på-tvers logikk mellom Altinn Autorisasjon og Maskinporten
- API-tilganger gis primært til scopes
  - API-tilbyder må sende ett tilgangs-kall per scope under et delegationScheme
  eget kall for alle gi tilgang psecuritySchemes, og ikke scopes
  - Maskinporten kjenner ikke til "delegationSchemes"
  - dersom tilgang gis direkte til scopes
- Delegeringskilde følger av scope
- tokens og -forespørsler forholder seg ikke til securityScheme
- Dersom leverandør  ikke oppgir hvem den ønsker å opptre på vegne av i tokenforespørsel, avvises token-forespøselen
  - hvordan håndteres dettepå  Oauth-klient-nivå?





Må innføre noen «norske» extension-attributter

Løser utfordringen med at oauth2 scopes blir for tekniske for daglig ledere i virksomheter
Gir API-tilbyder mulighet til å endre/legge til scopes under en delegerbar ressurs, uten å måtte «plage» daglig leder med ny delegering

Rendyrker ansvarsområdene mellom  tokentjeneste og delegeringskilde





### Referanser

* https://developers.google.com/identity/protocols/OAuth2ServiceAccount
* https://difi.github.io/idporten-oidc-dokumentasjon/oidc_auth_server-to-server-oauth2.html
* [RFC6749 The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
* [RFC7523 JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7523)

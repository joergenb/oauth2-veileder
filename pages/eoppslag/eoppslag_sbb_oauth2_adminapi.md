---
layout: page
title: "Forslag admin-API for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_adminapi.html

summary: "Dette dokumentet beskriver utvidelser på ID-porten sitt selvbejenings-API for å kunne realisere referansearkitekturen i eOppslag."
---


## Bakgrunn

Selve token-utstedelsesprosessen i eOppslag bruker allerede eksisterende funksjonalitet i ID-portens OIDC provider for "server-to-server Oauth2" (populært kalla "maskinporten"), se:  [https://difi.github.io/idporten-oidc-dokumentasjon/oidc_auth_server-to-server-oauth2.html](https://difi.github.io/idporten-oidc-dokumentasjon/oidc_auth_server-to-server-oauth2.html)

Det som primært mangler i ID-porten for å realise eOppslag, er å tilby selvbetjeningsløsninger for administrasjon av hvilke APIer (dvs. Oauth2 scopes) som finnes og hvem som skal kunne få utstedt access_tokens til disse.

## Om selvbetjeningsAPIet

eOppslag-APIene bygger videre på selvbetjeningsAPIet som ID-porten allerede tilbyr, se [https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_admin.html](https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_admin.html)


APIet spiser sin egen hundemat, og er sikret vha. Oauth2 :

1. Enkelt-virksomheter kan autentisere seg med virksomhetssertifikat, for så å administere alle integrasjoner knyttet til egen organisasjon
2. Brukere innlogga gjennom Difi sin samarbeidsportal kan administrere utvalgte data knyttet til egen organisasjon (Høst 2018)
2. Utvalgte leverandører kan administrere integrasjoner knyttet til sine kunder

For eOppslag må dette utvides med:

4. Administrasjons-sentra: Utvalgte klienter som Altinn får mulighet til å adminstere alle API-relaterte data knyttet til vilkårlige organisasjoner.


## Grovkornet tilgangstyring i eOppslag

Funksjoner nødvendige for at API-tilbyder kan få vedlikeholde tuplet (C,S,A) i eOppslag.

### API-definisjoner (S)

For å kunne vedlikeholde egne scopes, må A sin selvbetjeningsklient ha tilgang til følgende administrative scopes:

| scope | beskrivelse |
|-|-|
|idporten:scope.write|Gir tilgang til administrasjon av API-definisjoner (i form av Oauth2 scopes) knyttet til samme org.nr. som gitt i access_token.|
|idporten:eoppslagadmin.scope.write|Gir tilgang til administrasjon av API-definisjoner (i form av Oauth2 scopes) og tilgangstyring for alle org.no.   Tiltenkt Altinn og andre fellesløsninger vi stoler på.|

Disse scopene provisjoneres manuelt av Difi, sammen med 1 eller flere `prefix`.

Selvbetjenings-API utvides med:

| Operasjon | inndata | beskrivelse |
|-|-|-|
|`GET    /scopes `| |Gir liste over alle scopes beskyttet av ID-porten|
|`POST   /scopes ` | prefix*, subscope*, description, token_egenskaper  | Oppretter et nytt scope (lik prefix+subscope)    |
|`PUT    /scopes/{scope} `  |  description, token_egenskaper | Endrer et scope. TBD om selve scope-navnet skal kunne endres.   |
|`DELETE /scopes/{scope}`   |   | Sletter (eller deaktiverer?) et scope. TBD: alle tilganger slettes?  |


`token-egenskaper` er tekniske egenskaper som API-tilbyder forventer/krever. Dette kan være max tillatt levetid, self-contained eller ikke, minste sikkerhetsnivå, etc.


### Audience (A)

Difi foreslår at API-tilbyder (A) som hovedregel er koda inn i scope gjennom prefix. Dette betyr at som hovedregel kan A alltid utledes av S.  Dersom default prefix settes lik tilbyderen sitt orgno, gir dette gir mulighet til fullautomatisering uten noen manuell provisjonering, dvs. enhver organisasjon kan på egen hånd bli API-tilbyder.

For organisasjoner som krever flere prefixer, provisjoneres disse manuelt.

For tilfeller der samme scope S beskytter flere APIer fra ulike aktører (eksempel med kontoopplysninger fra bankene), trengs audience, for å unngå at bank1 kan bruke mottatt token til å hente opplysninger fra konkurrerende bank2.

For å registrere at et scope S benytter audience, utvides /scope-endepunktet med et valgfritt felt:
```
POST /scopes/bank:kontopplysninger/audience/"  { "https://rs.bank1.no/"}
POST /scopes/bank:kontopplysninger/audience/"  { "https://rs.bank2.no/"}
```

Når en klient så forespør aktuelt scope, må den også oppgi et og bare et gyldig audience, ellers feiler tokenutstedelsen. Derimot ser vi ikke for oss å tilgangsstyre audiencer (iallefall ikke i første omgang).  Alle konsumenter som har tilgang til S, får også tilgang til alle tilhørende audience.

Difi foreslår altså at `audience` i første rekke er et forhold med ressursserver og klient, med liten sentral validering i ID-porten.

### Konsumenter (C)

For å tilgangsstyre konsumenter, utvides selvbetjeningsAPIet med følgende operasjoner for API-tilbyder:

| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST   /access/{scope}`| org.no* | Gir konsument C tilgang til scopet S |
|`DELETE /access/{scope}/orgno/{orgno}`   |   |  Fjerner tilgangen C har til S |
|`GET    /access/{scope}`||liste alle tilganger for gitt scope|



## Tilgangsforespørsler

Konsumenter kan be om tilgang til et scope S slik:

| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST /accessrequest/{scope}`| | Konsument C (avledet av orgno i access_token) ber om tilgang til S|
|`GET  /accessrequest`| | Liste opp alle mine ikkje-behandla søknader|
|`GET  /accessrequest/{scope}`| | Liste opp alle som har bedt om tilgang til aktuelt scope (Brukes av API-tilbydere for å behandle tilgangskø)|
|`GET  /myaccesses`| | Liste opp alle mine tilganger|


Tilgangsforespørsler legges i en tilgangskø.  
Konsumenter trenger ikke eget administrativt scope for å be om slik tilgang, dette for å gjøre det så lett som mulig å være API-konsument.


Administrasjonssentra som Altinn må ha et eget administivt scope:
| scope | beskrivelse |
|-|-|
|idporten:eoppslagadmin.request.write|Gir muligheit til administrasjon av forespørsel-operasjoner for vilkårlige org.no |API-definisjoner (i form av Oauth2 scopes) knyttet til samme org.nr. som gitt i access_token.|

og bruker da APIet som over, men må spesifisere org.no eksplisitt. (eller eventeutl /org/-endepunkt, sjå RAML-fila)


## Delegering

Delegering styres av konsument.  


| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST /delegations/{scope} `| supplier_org_no |  Leverandør L (supplier_org_no) får lov til å be om token til S på vegne av C (avledet av access_token). |

Tilsvarende for administrasjonssentra, som trenger `idporten:eoppslagadmin.delegations.write`:

| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST /delegations/{scope}/{client_orgno} `| supplier_org_no |  Leverandør L (supplier_org_no) får lov til å be om token til S på vegne av C (client_orgno). |

Her opprettes tuplet `C,L,S` i delegeringstabellen.  Tilsvarnde trengs API-operasjoner for å slette og liste opp delegeringer.

### Utfordringer med delegering


En utfordring dersom delegering skjer utenfor ID-porten, er å sikre at delegeringen blir koblet mot riktig integrasjon (Oauth2-klient), siden Oauth2-modellen som ligger i bunn forutsetter at det er klienter som får tilgang til scopes. Dersom client_id mangler, må i prinsippet alle C's integrasjoner(både egne og levereandører) med aktuelt scope S få delegeringen.  Dette er kanskje ikke et problem i praksis

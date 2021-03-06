---
layout: page
title: "Forslag admin-API for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_adminapi.html

summary: "Dette dokumentet diskuterer utvidelser på ID-porten sitt selvbejenings-API for å kunne realisere referansearkitekturen i eOppslag."
---


## Om selvbetjeningsAPIet

eOppslag-APIene bygger videre på selvbetjeningsAPIet som ID-porten allerede tilbyr, se [https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_admin.html](https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_admin.html)



APIet er sikret vha. Oauth2 :

1. Enkelt-virksomheter kan autentisere seg med virksomhetssertifikat, for så å administere alle integrasjoner knyttet til egen organisasjon
2. Brukere innlogga gjennom Difi sin samarbeidsportal kan administrere utvalgte data knyttet til egen organisasjon (Høst 2018)
2. Utvalgte leverandører kan administrere integrasjoner knyttet til sine kunder


## eOppslag: tilgangstyring

Funksjoner nødvendige for at API-tilbyder kan få vedlikeholde tuplet (C,S,A) i eOppslag.

### API-definisjoner (S)

For å kunne vedlikeholde egne scopes, må A sin selvbetjeningsklient ha tilgang til følgende administrative scopes:

| scope | beskrivelse |
|-|-|
|idporten:scopes.write|Gir tilgang til administrasjon av API-definisjoner (i form av Oauth2 scopes) knyttet til samme org.nr. som gitt i access_token.|


Scopet provisjoneres manuelt av Difi, sammen med 1 eller flere `prefix`.

Selvbetjenings-API utvides med:

| Operasjon | inndata | beskrivelse |
|-|-|-|
|`GET    /scopes `| |Gir liste over alle scopes beskyttet av ID-porten (evt. filtrering)|
|`POST   /scopes ` | prefix*, subscope*, delegation_scheme, description, token_egenskaper  | Oppretter et nytt scope (lik prefix+subscope)    |
|`PUT    /scopes?scope={scope} `  |  delegation_scheme, description, token_egenskaper | Endrer et scope. Selve scope-navnet kan ikkje endres.   |
|`DELETE /scopes?scope={scope} `   |   | Deaktiverer et scope. (scopet beholdes for konsistens i audit-log) TBD: alle tilganger slettes?  |


`token_egenskaper` er tekniske egenskaper som API-tilbyder forventer/krever. Dette kan være max tillatt levetid, self-contained eller ikke, minste sikkerhetsnivå, etc.

`delegation_scheme` er en beskrivelse av eventuell ekstern delegeringskilde, som f.eks. Altinn.   Denne er - foruten et par faste attributter  - utvidarbar med key:value attributter, slik at den kan tilpasses behov hos andre delegeringskilder.



### Audience (A)

Difi mener at som hovedregel skal API-tilbyder (A) være koda inn i scope gjennom prefix. Dette betyr at A alltid utledes av S.  Dersom default prefix t.d. settes lik tilbyderen sitt orgno, gir dette også mulighet til fullautomatisering uten noen manuell provisjonering, dvs. enhver organisasjon kan på egen hånd bli API-tilbyder.

For organisasjoner som krever flere prefixer, provisjoneres disse manuelt.

**Resten av dette avsnittet er TBD, og ikkje del av en MVP**

For tilfeller der samme scope S beskytter flere APIer fra ulike aktører (eksempel med kontoopplysninger fra bankene), trengs en mekanisme for å unngå at bank1 kan bruke mottatt token til å hente opplysninger fra konkurrerende bank2.

Flere alternative løsninger:

#### Alt 1:
Klient forespør eit audience, dette blir inkludert i token uten å valideres eller tilgangsstyres, og så er det opp til ressursserver (API-tilbyder) å validere at mottatt token er for sitt eget audience.

Difi foreslår altså at `audience` i første rekke er et forhold med ressursserver og klient, med ingen sentral validering i ID-porten.

#### Alt 2:
For å registrere at et scope S benytter audience, utvides kan POST /scopes-endepunktet med et valgfritt felt for audience:
```
POST /scopes/  { "scope": "bank:kontopplysninger", aud": "https://rs.bank1.no/"}
POST /scopes/  { "scope": "bank:kontopplysninger", aud": "https://rs.bank2.no/"}
```

Når en klient så forespør aktuelt scope, må den også oppgi et og bare et gyldig audience, ellers feiler tokenutstedelsen. Derimot ser vi ikke for oss å tilgangsstyre audiencer (iallefall ikke i første omgang).  Alle konsumenter som har tilgang til S, får også tilgang til alle tilhørende audience.

#### Alt 3:

som alt 2, men her inkluderer tilgangstabellen også A, som medfører at konsumenter og tilbydere må forholde seg til feltet på alle API-operasjoner.


### Konsumenter (C)

For å tilgangsstyre konsumenter, utvides selvbetjeningsAPIet med følgende operasjoner for API-tilbyder:

| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST   /scopes/access` | scope, consumer_orgno | Gir konsument C tilgang til scopet S |
|`DELETE /scopes/access` | scope, consumer_orgno | Fjerner tilgangen C har til S |
|`GET    /scopes/access?scope={scope}`||liste alle tilganger for gitt scope|



## Tilgangsforespørsler

Konsumenter kan be om tilgang til et scope S slik:

| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST /scopes/access/requests`| scope | Konsument C (avledet av orgno i access_token) ber om tilgang til S|
|`GET  /scopes/access/requests?scope={scope}`| | Liste opp alle som har bedt om tilgang til aktuelt scope (Brukes av API-tilbydere for å behandle tilgangskø)|
|`GET  /myaccesses`| | Liste opp alle mine tilganger|
|`GET  /myaccessrequest`| | Liste opp alle mine ikkje-behandla søknader|

Tilgangsforespørsler legges i en tilgangskø.  

Konsumenter trenger kanskje ikke eget administrativt scope for å be om slik tilgang? Dette for å gjøre det så lett som mulig å være API-konsument.


## Delegering

Delegering mellom virksomheter (konsument -> leverandør) gjennomføres eksternt av konsumentene, primært i Altinn Autorisasjon, enten via [REST API](https://www.altinn.no/api/Help/Api/POST-who-authorization-Delegations) (vil bli utvidet) eller webgrensesnitt på altinn.no.

Delegeringer kan også gjøres mellom vilkårlige organisasjoner gjennom Altinns tjenesteeier-API.

Når leverandøren forsøker å gjøre en klient-registrering hvor de oppgir å opptre på vegne av en konsument, vil Maskinporten gjøre et oppslag i delegerings-API-et for å sjekke om en slik delegering mellom C og L finnes for det aktuelle scopet.

### Kobling mellom delegering til klient og delegering til virksomhet

En utfordring dersom delegering skjer utenfor ID-porten, er å sikre at delegeringen blir koblet mot riktig integrasjon (Oauth2-klient), siden Oauth2-modellen som ligger i bunn forutsetter at det er klienter som får tilgang til scopes. Uten kobling til klient, må i prinsippet alle C's integrasjoner (både egne og leverandører) med aktuelt scope S få delegeringen.  Dette betyr at potensiale for konflikt med den leverandør-styrte integrasjoner som finst i ID-porten idag.

Difis forslag er at delegeringstabellen primært ikke er en run-time kilde, men heller konsulteres når en oauth2-klient skal endres. Dvs Leverandørers klienter får ikke automatisk satt nye scopes som blir delegert, leverandøren må aktivt selv inn og rekonfigurere sin klient-integrasjonen med det nye scopet.

### Leverandør-styrte integrasjoner
Leverandører kan gjennom en klient-registrering selv-deklarere at de opptrer på vegne av en konsument.:
```
POST /clients { client_orgno*, scope* }
```
Hvis scopet er registrert med et delegeringsoppsett, sjekkes den aktuelle delegeringskilden (f.eks. Altinn) om en delegering fra C til L finnes for det aktuelle scopet.

Hvis delegeringsoppsett ikke er definert, begrenses tilgang til selv-deklarering til et utvalgt sett med leverandører (som i dagens løsning)

Hvis autorisering feiler, nektes forespørselen.

Hvis autorisering lykkes, opprettes tuplet `C,L,S,client_id` i delegeringstabellen.  L (supplier_orgno) blir satt automatisk basert på virksomhetssertifikatet brukt mot admin-API.

### Hvordan skille de selv delegering ?
I begge tilfellene utleverer vi token som for eksempel.
```
{
  scope: S,
  consumer_orgno: C,
  act: {
    supplier_orgno: L,
    iss: altinn
  }
}
```
API-tilbyder kan derfor ikke vite hvilken delegering som ligger til grunn, men i de fleste tilfeller ønsker ikke API-tilbyder å vite noe om dette heller.  

For de tilfellene der type delegering er viktig for API-tilbyder, kan man vurdere å inkludere et `iss` claim som viser delegeringskilde som del av `act`-objektet.  iss kan enten følge et kodeverk (Altinn, leverandør, idporten), evt. id eller URL til integrasjonen som benyttet admin-api'et.  

Vi ser også at tuplene i delegeringsmatrisen som resultat av de to ulike typene delegering er like.  Et viktig spørsmål er hvordan, evt. *om* tilgangstyringen ved *tokenutstedelse* skal skille på disse to delegeringsmetodene, eller om det treng regler på andre tidspunkt i verdikjeden.

#### mulig algoritme for tilgangskontroll:
1. Grunnleggende Oauth2-validering (gyldig JWT, gyldig klient_id, har klient forespurt scope).
2. Utfør klientautentisering, validere nøkler
  - dersom virk.sert, kontrollere at orgno i sertifikat stemmer med org.no registrert på klient, sjekk revokaksjon, valider virksert
3. Finn C basert på klienten sin konfigurajon
4. Eksisterer (C,S) evt (C,S,A) i tilgangsmatrisen ?  
  - viss nei: avbryt
5. Finn L basert på klienten sin konfigurasjon
  - ingen L? Gå til punkt 8
6. Eksisterer (L,C,S,client_id) i delegeringsmatrisen ?
  - viss ja: gå til 8
  - viss annan client_id: avbryt
7. Eksisterer (L,C,S) i delegeringsmatrisen?
  - viss nei: avbryt
8. Er forespurte token_egenskaper lik eller strengere enn API-tilbyders krav?
  - viss nei: avbryt
9. utsted token

Forsøk på flytdiagram:

<div class="mermaid">
graph LR

  o2val[Oauth2-validering]
  klaut[klientautentisering]
  o2val-->klaut
  ctil{Har C tilgang til S,A?}
  klaut-->ctil
  lev{Leverandør-integrasjon?}
  ctil-->lev
  lev-- nei -->token
  lev-- ja --> lev_client

  lev_client{Tilgang delegert til integrasjon ? L,C,S,client_id}
  lev_client -- ja --> token
  lev_client -- nei --> lev_acc

  lev_acc{Tilgang delegert til leverandør ?  L,C,S}
  lev_acc -- ja -->token

  token[Utsted token]

</div>

## Risiko/trusselmodellering

teste algoritmen på ulike trusler

#### Risiko 1: Annen leverandør forsøker å opptre på vegne av konsument

* C er gitt tilgang av A til scopet S
* C ønsker kun å delegere rettighet for scope S til leverandør L1   (L1,C,S lagres i delegeringsmatrise)
* L2 oppretter en egen integrasjon med scope S der den hevder å opptre på vegne av C (L2,C,S lagres i delegeringsmatrise)
* L2 lager ein token-forespørsel med C,S,egen client_id og eget virksomhetssertifikat
  - Dersom delegering er knyttet mot client_id: Maskinporten vil avvise token-forespørsel i punkt 6, siden L2 kommer med feil client_id
  - Dersom delegering ikke er knyttet mot client_id: Maskinporten vil utstede token, siden L2,C,S er en gyldig kombinasjonsarkitektur

Konklusjon?: Konsumenter som ikke stoler på leverandører, må sørge for at delegering alltid blir knyttet til riktig integrasjon (må håndtere egen kompleksitet)   (andre konsumenter aksepterer dette som en rest-risiko)

#### Risiko 2: Falsk client_id

* C er gitt tilgang av A til scopet S
* C ønsker kun å delegere tilgang for scope S til leverandør L1
* L2 oppretter en egen integrasjon med scope S der den hevder å opptre på vegne av C
* L2 lager ein token-forespørsel med C,S, eget virksomhetssertifikat,  men med L2 sin client_id (må anses som kjent)
  - Maskinporten vil avvise forespørsel i punkt 2, siden orgno i virksomhetssertifikatet ikke stemmer med det som er registrerert for aktuell client_id.


#### Blanding av egne og leverandør-integrasjoner
eksempel her er API som blir integrert i "alle" systemer, t.d. Folkeregisteret

* C har en egen integrasjon mot S,A   (C,S,A lagres i tilgangsmatrise)
* C kjøper en skytjeneste som også integrer mot S,A  (C,S,A lagres også i tilgangsmatrise, og (L,C,S,A,client_id) lagres i delegeringsmatrise

Dette er ikke en utfordring for tilgangstyring, men derimot for vedlikehold... Dersom ene systemet blir avsluttet, vil tilgang bli slettet for begge


### Risiko 4: få tilgang til feil scopes




### Risiko 5: virksomhetsssertifikat som universalnøkkel:

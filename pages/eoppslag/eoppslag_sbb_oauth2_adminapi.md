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

Difi foreslår API-tilbyder (A) som hovedregel er koda inn i scope gjennom prefix.   Dette gir mulighet til fullautomatisering ogso av provisjonering av API-tilbydere, dvs default prefix settes lik tilbyderen sitt orgno. Dette betyr at som hovedregel kan A alltid utledes av S.


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
|`GET  /accessrequest/{scope}`| | Liste opp alle som har bedt om tilgang til aktuelt scope (Leses av API-tilbydere for å behandle tilgangskø)|
|`GET  /myaccesses`| | Liste opp alle mine tilganger|


Tilgangsforespørsler  legges i en tilgangskø.  
Konsumenter trenger ikke eget administrativt scope for å be om slik tilgang, dette for å gjøre det så lett som mulig å være API-konsument.


Administrasjonssentra som Altinn må ha et eget administivt scope:
| scope | beskrivelse |
|-|-|
|idporten:eoppslagadmin.write|Gir muligheit til administrasjon av tilgangs- og delegering-operasjoner for vilkårlige org.no |API-definisjoner (i form av Oauth2 scopes) knyttet til samme org.nr. som gitt i access_token.|

og bruker da APIet som over, men må spesifisere org.no eksplisitt.


## Delegering

Delegering styres av konsument.  


| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST /delegations/<scope> `| supplier_org_no |  Leverandør L (supplier_org_no) får lov til å be om token til S på vegne av C (avledet av access_token). |

Tilsvarende for administrasjonssentra:

| Operasjon| inndata |beskrivelse |
|-|-|-|
|`POST /delegations/<scope>/<client_orgno> `| supplier_org_no |  Leverandør L (supplier_org_no) får lov til å be om token til S på vegne av C (client_orgno). |

Her opprettes tuplet `C,L,S` i delegeringstabellen.  Tilsvarnde trengs API-operasjoner for å slette og liste opp delegeringer.

### Utfordringer med delegering


En utfordring dersom delegering skjer utenfor ID-porten, er å sikre at delegeringen blir koblet mot riktig integrasjon (Oauth2-klient), siden Oauth2-modellen som ligger i bunn forutsetter at det er klienter som får tilgang til scopes. Dersom client_id mangler, må i prinsippet alle C's integrasjoner(både egne og levereandører) med aktuelt scope S få delegeringen.  Dette er kanskje ikke et problem i praksis




####



## Brukerreise for eoppslag

## API-tilbydere


### Provisjonering

NAV som API-tilbyder A blir manuelt provisjonert i ID-porten, ved at A sitt org.no får tildelt eitt eller fleire scope-prefix.

### Opprette APIer

NAV kaller så selvbetjeningsAPIet for å opprette ressursdefinisjoner:

```
POST /scope ("nav:", "trygdeopplysninger")    
```
som oppretter scopet `nav:trygdeopplysninger`.   

### Tilganger

NAV vil så tildele noen tilganger:
```
POST /access/nav:trygdeopplysninger/ { "orgno"="995568217" }
```
som gir Gjensidige Forsikring tilgang til å få tokens med scope "nav:trygdeopplysninger".


NAV kan også hente ut ventende tilgangsforespørsler:
```
GET /accessrequests/nav:trygdeopplysninger
```
som returnerer en JSON array med org.nummer på konsumenter som ønsker tilgang til aktuelt scope.  NAV kaller så /access-endepunktet for å behandle køen.



## Integrasjoner

### Konsument

Konsumenten C registrerer sine ID-porten integrasjoner via eksisterende metoder i dagens admin-API eller selvbetjeningsløsning.  C sitt org.nummer blir automatisk koblet til integrasjonene.
```
POST /clients { scope*, name, description, nøkkel, token_egenskaper}
```
`client_id` genereres automatisk av ID-porten (uuid)
### Leverandør
Utvalgte leverandører L registrer sine integrasjonar via eksisterende metoder i dagens admin-API. To alternativer
```
POST /clients                                 { client_orgno, scope*, name, description, nøkkel, token_egenskaper}
POST /clients/{egen client_id}/onbehalfof/    { client_orgno,       , name, description }
```
Det første alternativet oppretter en selvstendig, helt standard oauth2 klient.  Det andre alternativet bruker ID-portens proprietære onbehalfof-funksjonalitet. Onbehalfof har sin bakgrunn fra SAML2, der kompleksiteten rundt SAML metadata gjorde at mange leverandører ønsket å ha en felles teknisk integrasjon mot ID-porten og heller fortelle hvilken kunde de operer på vegne av i hver forespørsel.   Med OIDC/Oauth2 antar vi derimot at de fleste leverandører vil foretrekke alternativ 1 fordi standard rammeverk kan benyttes.

I begge tilfeller blir leverandørens org.no registert på integrasjonen.


## Konsumenter


### Tilgangsforespørsler

Konsumenter kan konkret forespørre tilgang til APIer:
```
POST /accessrequestes { scope*, client_id* }
```
Konsumenten sitt orgnr. blir automatisk inkludert i tilgangsforespørselen

Merk at scope må knyttes til en spesifikk integrasjon (client_id).

Leverandører forspør tilgang på samme måte, men Maskinporten kan på bakgrunn av at aktuell client_id tilhører en leverandør-konsument-kobling, finne riktig konsument som tilgangsforespørselen gjelder. Evt kan man for leverandører kreve at de oppgir konsumentens org.no eksplisitt.

Tilgangsforespørsler kan også skje *implisitt* når en oppretter integrasjon gjennom `/clients`-endepunktet, eller endrer forespurte scopes på klientregistreringa.  For å unngå at API-tilbyder blir "spamma" ned av gjentagende behandlingsforespørsler for samme C,S,A-tupler, bør ikke et fjerning av scope fra en klient-registrering automatisk slette tilhørende tuppel i tilgangstabellen.

## Delegering

### Fra konsumenter
Konsumenter kan delegere tilgang til Leverandører
```
POST /delegations { supplier_orgno*, scope*            }
POST /delegations { supplier_orgno*, scope*, client_id }
```
Her opprettes tuplet `C,L,S` evt. `C,L,S,client_id` i delegeringstabellen.  C (client_orgno) blir satt  automatisk basert på virksomhetsertifikatet.

En utfordring dersom delegering skjer utenfor ID-porten, er å sikre at delegeringen blir koblet mot riktig integrasjon (Oauth2-klient), siden Oauth2-modellen som ligger i bunn forutsetter at det er klienter som får tilgang til scopes. Dersom client_id mangler, må i prinsippet alle C's integrasjoner(både egne og levereandører) med aktuelt scope S få delegeringen.  Dette er kanskje ikke et problem i praksis

### Fra leverandør
Utvalgte leverandører kan gjennom en klient-registrering selv-deklarere at de opptrer på vegne av en konsument.:
```
POST /clients { client_orgno*, scope* }
```
Her opprettes tuplet `C,L,S,client_id` i delegeringstabellen.  L (supplier_orgno) blir satt automatisk basert på virksomhetssertifikatet brukt mot admin-API.

### Hvordan skille de to typene delegering ?
I begge tilfellene utleverer vi token som
```
{
  scope: S,
  client_orgno: C,
  act: {
    sub: L
  }   
}
```
API-tilbyder kan derfor ikke vite hvilken delegering som ligger til grunn, men i de fleste tilfeller ønsker ikke API-tilbyder å vite noe om dette heller.  

For de tilfellene der type delegering er viktig for API-tilbyder, kan man vurdere å inkludere et `iss` claim som viser delegeringskilde som del av `act`-objektet.  iss kan enten følge et kodeverk (Altinn, leverandør, idporten), evt. id eller URL til integrasjonen som benyttet admin-api'et.  

Vi ser også at tuplene i delegeringsmatrisen som resultat av de to ulike typene delegering er like.  Et viktig spørsmål er hvordan, evt. *om* tilgangstyringen ved *tokenutstedelse* skal skille på disse to delegeringsmetodene, eller om det treng regler på andre tidspunkt i verdikjeden.

#### Algoritme for tilgangskontroll:
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

### Risiki

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

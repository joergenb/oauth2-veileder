---
layout: page
title: "Forslag admin-API for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_adminapi.html

summary: "Hvilke API-operasjoner trengs i eOppslag?"
---

Admin-API'et bygger videre på APIet som ID-porten allerede tilbyr i produksjon: [https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_admin.html](https://difi.github.io/idporten-oidc-dokumentasjon/oidc_api_admin.html)

REST med full CRUD-operasjoner for dei fleste objekta.

## API-tilbydere


### Provisjonering

Skjer via manuell konfigurasjon i ID-porten:
A (org.no) får tildelt eitt eller fleire scope-prefix.

### Opprette APIer

A kaller så admin-api for å opprette ressursdefinisjoner:

```
POST /scopes (prefix*, subscope*, audience, token-egenskaper)
```

`token-egenskaper` er tekniske egenskaper som API-tilbyder forventer/krever. Dette kan være max tillatt levetid, self-contained eller ikke, minste sikkerhetsnivå, etc.

#### Eksempler

##### Normal-tilfelle, ein API-tilbyder med få APIer.
Her trengs ikkje audience.
```
POST /scope ("nav:", "forsikring")    
```
som oppretter scopet `nav:forsikring`.   
Validering på at org.no i benyttet virk.sert stemmer med provisjonering.
Dersom NAV berre har eitt prefix, kan ogso vere valgfri?

##### Avansert tilfelle som krev audience
```
POST /scopes ("bits:", "konto", "https://rs.bank1.no/")
POST /scopes ("bits:", "konto", "https://rs.bank2.no/")
```
eventuelt mer REST-ish:
```
POST /scopes/bits:konto/"  { aud="https://rs.bank1.no/"}
POST /scopes/bits:konto/"  { aud="https://rs.bank2.no/"}
```


### Tilganger

A vil så tildele noen tilganger:
```
POST /access  {scope*, client_orgno*, audience }
```
DELETE sletter tilsvarende.

A kan også hente ut ventende tilgangsforespørsler:
```
GET /accessrequests/{scope}
```
som returnerer en JSON array med org.nummer på konsumenter som ønsker tilgang til aktuelt scope.  TODO: hvordan behandle: bruke /access som over, eller behandle /accessrequests



## Konsumenter

### Integrasjoner
Konsumenten C registrerer sine ID-porten integrasjoner via eksisterende metoder i dagens admin-API.  C sitt org.nummer blir automatisk koblet til integrasjonene.

Konsumenter kan forespørre tilgang til APIer:
```
POST /accessrequestes { scope*, client_id* }
```
Merk at scope må knyttes til en spesifikk integrasjon (client_id).

## Delegering

### Fra konsumenter
Konsumenter kan delegere tilgang til Leverandører
```
POST /delegations { supplier_orgno*, scope*, client_id }
```
Her opprettes tuplet `C,L,S,client_id` i tilgangstabellen.  C (client_orgno) blir satt  automatisk basert på virksomhetsertifikatet.

En utfordring dersom delegering skjer utenfor ID-porten, er å sikre at delegeringen blir koblet mot riktig integrasjon (Oauth2-klient). Dersom client_id mangler, må i prinsippet alle C's integrasjoner med aktuelt scope S få delegeringen.

### Fra leverandør
Utvalgte leverandører kan gjennom en klient-registrering selv-deklarere at de opptrer på vegne av en konsument:
```
POST /clients { client_orgno*, scope* }
```
Her opprettes tuplet `C,L,S,client_id` i tilgangstabellen.  L (supplier_orgno) blir satt automatisk basert på virksomhetssertifikatet.

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
API-tilbyder kan derfor ikke vite hvilken delegering som ligger til grunn.  Dette kan evt. mitigeres ved å inkludere eit `iss` claim som viser delegeringskilde som del av `act`-objektet.

Vi ser også at tuplene i tilgangsmatrisen som resultat av de to ulike typene delegering er like.  Et viktig spørsmål er hvordan, evt. *om* tilgangstyringen ved *tokenutstedelse* skal skille på disse to delegeringsmetodene, eller om det treng regler på andre tidspunkt i verdikjeden.

#### Risiko 1: Kryss-leverandør

* C er gitt tilgang av A til scopet S
* C ønsker kun å delegere rettighet for scope S til leverandør L1
* L2 oppretter en egen integrasjon med scope S der den hevder å opptre på vegne av C
* L2 lager ein token-forespørsel med C,S,egen client_id og eget virksomhetssertifikat
  - Dersom delegering er knyttet mot client_id: Maskinporten vil avvise token-forespørsel, siden L2 kommer med feil client_id
  - Dersom delegering ikke er knyttet mot client_id: Maskinporten vil utstede token, siden C,S er en gyldig kombinasjonsarkitektur

Konklusjon?: Konsumenter som ikke stoler på leverandører, må sørge for at delegering alltid blir knyttet til integrasjoner.   (andre konsumenter aksepterer dette som en rest-risiko)

#### Risiko 2: Falsk client_id

* C er gitt tilgang av A til scopet S
* C ønsker kun å delegere rettighet for scope S til leverandør L1
* L2 oppretter en egen integrasjon med scope S der den hevder å opptre på vegne av C
* L2 lager ein token-forespørsel med C,S, L1 sin client_id (må anses som kjent) og eget virksomhetssertifikat
  - Maskinporten vil avvise forespørsel, siden orgno i virksomhetssertifikatet ikke stemmer med det som er registrerert for aktuell client_id.

### Risiko 3: 



*
* L2 generer JWT-grant
* Maskinporten vil avvis

```



Utfordringen går på virksomhetsssertifikat som universalnøkkel:  

* Ingen delegeringskontroll.  
*
alternativer
* Ingen delegeringgangskontroll
Viktig spørsmål:
*


Merk at dette


 tilgang bli delegret


## Leverandører



## Scenario

* *Maskinporten* : tbd
* *Maskinporten delegering* : tbd
* *Ansattpålogging* : En ansatt i "Min Arbeidsgiver" logger inn på ein sky-regneskapsløsning (levert av Visma), og skal bruke eit API levert av NAV.

## Detaljert token

|claim|Autentiseringsbevis (OIDC id_token)|Autensieringær autorisasjon - bruker innlogget i tjeneste uten delegering (oauth2 access_token)|Maskinporten idag|eOppslag - selvstendig konsument|eOppslag konsument via leverandør| Ansattpålogging (delegering) |spec'|kommentar|
|-|-|-|-|-|-|-|-|-|-|
|sub|TWGi0...2GBY=|TWGi0...2GBY=||en UUID |en annen UUID|TWGi0...2GBY=|OIDC, JWT|OIDC: unik, tjenestespesifikk ikke-meningsbærende identifikator(strengt matematisk definert), JWT:  The "sub" (subject) claim identifies the principal that is the  subject of the JWT. |
|scope|openid|svv:pkk|global/kontaktinformasjon.read|nav:forsikring|nav:forsikring|nav:forsikring|Oauth2|
|client_orgno||944117784|974761076|999888777 (Storbanken)|777888999 (Lillebanken)|936796702
|aud| oidc_kongsvingerkommune_acos|oidc_svv_vitecautodata|oidc_skatteetaten_krr|https://api.nav.no/|https://api.nav.no/||OIDC, JWT| `aud` utlevers kun når klient ber om det|
|acr| Level4 |Level4||egen_nøkkel|kvalifisert_segl|Level4|oidc|Sikkerhetsnivå (Auth. context class ref. )|
|amr| BankID |BankID||||BankID|oidc|
|pid|11111156789|11111156789 ||||11111156789 | [krr] | personidentifikator i folkeregisteret (burde heitt folkeregisteridentifikator)|
|**Fremtidige claims**|
|act|||||`{ sub: "eika-gruppen", organisasjonsnummer: 999777555 }`||Token Ex.|The "act" (actor) claim provides a means within a JWT to express that    delegation has occurred and identify the acting party to whom authority has been delegated.|
|may_act||||||`{ sub: "min arbeidsgiver", organisasjonsnummer: 999555111, iss: "altinn autorisasjon" }`|Token Ex. |The "may_act" claim makes a statement that one party is authorized to  become the actor and act on behalf of another party.|
|client_id||oidc_svv_vitecautodata || oidc_storbanken (evt autogenerert)|oidc_eika_kunde23 (evt. autogenerert)| oidc_visma_skyregnskap


Følgende claims er like i alle varianter:

|claim|spec'|definisjon|
|-|-|-|
|iss|oidc, jwt|Utsteder (idporten.difi.no)
|exp|oidc, jwt|tidspunkt for utløp
|iat|oidc, jwt|tidspunkt for utstedelse
|jti|oidc, jwt|unik identifikator for selve tokenet


Access tokens bør følge strukturen uansett om dei er direkte utsted av Autorisasjonsserver, eller om dei er eit resultat av ein Token Exchange operasjon.


## sikkerhetsnivå

### Personinnlogging:
* Level3
* Level4

### Virksomhet:
(må defineres)

* eIDAS elektronisk segl
* eIDAS avansert segl
* eIDAS kvalifisert segl



## Referanser

* [Enhet] : [Felles informasjonsmodell for Person og Enhet](https://www.difi.no/fagomrader-og-tjenester/digitalisering-og-samordning/nasjonal-arkitektur/informasjonsforvaltning/person-og-enhet-felles-informasjonsmodell)

* [krr] : [https://begrep.difi.no/Felles/personidentifikator](https://begrep.difi.no/Felles/personidentifikator)
* [JWT] : [RFC 7519](https://tools.ietf.org/html/rfc7519)
* [OIDC] : [
OpenID Connect Core 1.0 incorporating errata set 1](http://openid.net/specs/openid-connect-core-1_0.html)

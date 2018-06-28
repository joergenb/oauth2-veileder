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

#### Eksempel: Normal-tilfelle, ein API-tilbyder med få APIer.
Her trengs ikkje audience.
```
POST /scope ("nav:", "forsikring")    
```
som oppretter scopet `nav:forsikring`.   
Validering på at org.no i benyttet virk.sert stemmer med provisjonering.
Dersom NAV berre har eitt prefix, kan ogso vere valgfri?

#### Eksempel: Avansert tilfelle som krev audience
```
POST /scopes ("bits:", "konto", "https://rs.bank1.no/")
POST /scopes ("bits:", "konto", "https://rs.bank2.no/")
```
(
  eller kanskje meir REST-ish:
```
POST /scopes/bits:konto/audience/"  { "https://rs.bank1.no/"}
POST /scopes/bits:konto/audience/"  { "https://rs.bank2.no/"}
```
)

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




## Integrasjoner

### Konsument

Konsumenten C registrerer sine ID-porten integrasjoner via eksisterende metoder i dagens admin-API eller selvbetjeningsløsning.  C sitt org.nummer blir automatisk koblet til integrasjonene.
```
POST /clients { scope*, name, description, nøkkel, token_egenskaper}
```
`client_id` genereres automatisk av ID-porten (uuid)
### Leverandør
Leverandøren L registrer sine integrasjonar via  via eksisterende metoder i dagens admin-API. To alternativer
```
POST /clients                            { client_orgno, scope*, name, description, nøkkel, token_egenskaper}
POST /clients/{client_id}/onbehalfof/    { client_orgno,       , name, description }
```
Det første tilfellet oppretter en selvstendig, helt standard oauth2 klient.  Det andre tilfellet


## Konsumenter


### Tilgangsforespørsler

Konsumenter kan så forespørre tilgang til APIer:
```
POST /accessrequestes { scope*, client_id* }
```
Konsumenten sitt orgnr. blir automatisk inkludert i tilgangsforespørselen

Merk at scope må knyttes til en spesifikk integrasjon (client_id).

Leverandører forspør tilgang på samme måte, men Maskinporten kan på bakgrunn av at aktuell client_id tilhører en leverandør-konsument-kobling, finne riktig konsument som tilgangsforespørselen gjelder. Evt kan man for leverandører kreve at de oppgir konsumentens org.no eksplisitt.

## Delegering

### Fra konsumenter
Konsumenter kan delegere tilgang til Leverandører
```
POST /delegations { supplier_orgno*, scope*, client_id }
```
Her opprettes tuplet `C,L,S,client_id` i delegeringstabellen.  C (client_orgno) blir satt  automatisk basert på virksomhetsertifikatet.

En utfordring dersom delegering skjer utenfor ID-porten, er å sikre at delegeringen blir koblet mot riktig integrasjon (Oauth2-klient). Dersom client_id mangler, må i prinsippet alle C's integrasjoner(både egne og levereandører) med aktuelt scope S få delegeringen.

### Fra leverandør
Utvalgte leverandører kan gjennom en klient-registrering selv-deklarere at de opptrer på vegne av en konsument:
```
POST /clients { client_orgno*, scope* }
```
Her opprettes tuplet `C,L,S,client_id` i delegeringstabellen.  L (supplier_orgno) blir satt automatisk basert på virksomhetssertifikatet.

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
2. Finn C basert på klienten sin konfigurajon
2. Utfør klientautentisering
  - dersom virk.sert:  
    - stemmer
3. Eksisterer (C,S) evt (C,S,A) i tilgangsmatrisen ?  
  - viss nei: avbryt
4. Finn L basert på klienten sin konfigurasjon
  - ingen L? Gå til punkt 7
5. Eksisterer (L,C,S,client_id) i delegeringsmatrisen ?
  - viss ja: utsted token
  - viss nei: gå videre
  - viss annan client_id: avbryt
6. Eksisterer (L,C,S) i delegeringsmatrisen?
  - viss ja: utsted token
  - viss nei: gå videre
7.
3.
 eMeldingsformidlingsinfrastruktur
2.


#### Risiko 1: Kryss-leverandør

* C er gitt tilgang av A til scopet S
* C ønsker kun å delegere rettighet for scope S til leverandør L1
* L2 oppretter en egen integrasjon med scope S der den hevder å opptre på vegne av C
* L2 lager ein token-forespørsel med C,S,egen client_id og eget virksomhetssertifikat
  - Dersom delegering er knyttet mot client_id: Maskinporten vil avvise token-forespørsel, siden L2 kommer med feil client_id
  - Dersom delegering ikke er knyttet mot client_id: Maskinporten vil utstede token, siden C,S er en gyldig kombinasjonsarkitektur

Konklusjon?: Konsumenter som ikke stoler på leverandører, må sørge for at delegering alltid blir knyttet til riktig integrasjon (må håndtere egen kompleksitet)   (andre konsumenter aksepterer dette som en rest-risiko)

#### Risiko 2: Falsk client_id

* C er gitt tilgang av A til scopet S
* C ønsker kun å delegere rettighet for scope S til leverandør L1
* L2 oppretter en egen integrasjon med scope S der den hevder å opptre på vegne av C
* L2 lager ein token-forespørsel med C,S, L1 sin client_id (må anses som kjent) og eget virksomhetssertifikat
  - Maskinporten vil avvise forespørsel, siden orgno i virksomhetssertifikatet ikke stemmer med det som er registrerert for aktuell client_id.

### Risiko 3: Feil konsumenter

* C1 har tilgang til scope S1 gjennom L1
* C2 har tilgang til scope S1 direkte
* L1 oppretter integrasjon falsk C1-integrL1 kan lage forespørsel med  
* L1 forespør token for C2.
  - Maskinporten ser at C2 og S1

### Risiko 3: kryss-scopes

* C1 har tilgang til scope S1 gjennom L1
* C1 har tilgang til scope S2 gjennom L2
* C2 har tilgang til scope S1 gjennom L2
  - mao: L2 har både S1 og S2
- L2 lager token-forespørsel med S1 og C1



### Risiko 3: virksomhetsssertifikat som universalnøkkel:

* C gir virksert til L1.  L1 har tilgang til scope S1.
* C gir annet virksert til L2
* Viktig at

---
layout: page
title: "Brukerreise for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_brukerreise.html

summary: "En brukerreise som viser hvordan man etablerer og tilgangsstyrer APIer som følger eOppslag-mønsteret. "
---


## 1. NAV oppretter et API

NAV ønsker å opprette et API og gi tilgang til Gjensidige Forsikring.

NAV som API-tilbyder A blir manuelt provisjonert i ID-porten, ved at A sitt org.no får tildelt scope-prefixet `nav:`.

NAV har nå to muligheter. De kan registrere API-et sitt direkte i maskinporten, eller så kan de publisere en formell beskrivelse (OAS3) av API-et i [API-katalogen](https://fellesdatakatalog.brreg.no/apis).

#### Alternativ A: NAV oppretter API-et direkte i Maskinporten

NAV kaller så selvbetjeningsAPIet for å opprette ressursdefinisjoner tilknyttet prefix:

```
POST /scopes/
{
  "prefix": "nav",
  "subscope": "trygdeopplysninger",
  "belongs_to_delegation_schemes": ["trygdeopplysninger"],
  "delegation_scheme_definitions": [
    {
      "name": "trygdeopplysninger",
      "title": "API for trygdeopplysninger hos NAV",
      "description": "Gir anledning til å gjøre leseoperasjoner i API for trygdeopplysninger hos NAV",
      "delegation_source": "altinn"
    }
  ]
}  
```
som oppretter scopet `nav:trygdeopplysninger`.   (alternativt bruker NAV en av GUI-løsningene integrert med Maskinporten og gjør   det samme via pek-og-klikk.)

I dette eksemplet blir også en delegeringskonfigurasjon oppgitt, slik at scopet blir gjort delegerbart i Altinn. Her kan ulike scopes grupperes under ulike delegeringsoppsett ("delegation scheme"), slik at delegeringer i Altinn kan gjøres på mer enn ett og ett scope.

Via en `PUT`-operasjon kan delegeringskonfigurasjon legges til eller endres på allerede eksisterende scopes.

#### Alternativ B: NAV publiserer en beskrivelse av API-et til API-katalogen

NAV logger inn i Felles Datalog, og registrerer API-et sitt og oppgir en OpenAPI 3 Specification som inneholder en utvidelse som forklarer hvordan API-et skal tilgangsstyres.

Det er en-til-en mellom OAS "securitySchemes" og "delegation_scheme_definitions". Så følgende definisjon i OAS vil være eksakt den samme som oppgitt i alternativ A:

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

I forbindelse med den manuelle provisjoneringen av scope-prefixet for NAV i Maskinporten, blir det også lagret en assosiasjon til en "tilbyder" i API-katalogen. Via en hendelsesfeed fra FDK, vil Maskinporten kunne abonnere på nye/endrede API-definisjoner for de scope-prefix/scopes som er registrert.

### Tilgangstyring

Herfra er brukerreisen uavhengig av om API-et er opprettet direkte i Maskinporten eller via API-katalogen.

NAV vil så tildele noen tilganger:
```
POST /scopes/access/
{
  "scope": "nav:trygdeopplysninger",
  "consumer_orgno": "995568217"
}

```
som gir Gjensidige Forsikring tilgang til å få tokens med scope "nav:trygdeopplysninger".


## 2. Gjensidige bruker APIet

Når APIet er klart, får Gjensidige beskjed om hvilket scope de må ha for å benytte APIet, at de skal benytte Maskinporten, samt dokumentasjon av selve APIet.

Gjensidige lager en oauth2-klient som benytter virksomhetssertifikat. Gjensidige registrerer denne selv i Maskinporten ved å bruke selvbetjeningsAPIet, og provisjonerer klienten med API-tilgangen virksomheten har fått tildelt:

```
POST /clients/
{
  "client_name": "Gjensidige sin maskinport-integrasjon"
  "token_reference": "SELF_CONTAINED",
  "scope": "nav:trygdeopplysninger",
  ...
}
```

Når klienten skal bruke APIet, generer den først en tokenforespørsel med scope=`nav:trygdeopplysninger`,  signerer dette med Gjensidige sitt virksomhetssertikat (et jwt-grant), og sender til Maskinporten. Maskinporten utsteder et access_token tilbake:
```
{
  iss:            "Maskinporten"
  scope:          "nav:trygdeopplysninger"
  consumer_orgno: "995568217"
  amr_org:        "QCert for eSeal"
  token_type:     "virksomhet"
  ...
}
```


Klienten kan så sende API-operasjoner mot NAV sitt API sammen med tokenet:

```
GET https://api.nav.no/trygd
Authorization: Bearer <token>
```

NAV sitt API (ressursserver) validerer tokenet, gjør evt. videre finkornet tilgangskontroll og returnerer ønsket respons.





## 3. Storebrand forsøker å bruke APIet

Storebrand ser i API-katalogen at NAV tilbyr et interessant API.  

De lager raskt en Oauth2-klient, får denne registrert i Maskinporten, og forsøker provisjonere det aktuelle scopet til klienten. Siden Storebrand ikke har fått tilgang, vil provisjoneringen bli avvist. Storebrand sender istedet en tilgangsforespørsel manuelt til NAV, og inngår eventuelle avtaler som behøves.

Når avtalen er på plass, gir NAV tilgang til Storebrand:
```
POST /scopes/access/
{
  "scope": "nav:trygdeopplysninger",
  "consumer_orgno": "930553506"
}
```

Storebrand kan nå bruke NAV sitt API.

NAV kan selvfølgelig også håndtere tilgangsforespørsler selv direkte.


## 4. Leikanger Kommune ønsker at Evry skal bruke APIet for dem

Leikanger kommune kjøper et kommunalt fagsystem som skytjeneste fra leverandøren Evry. Systemet har behov for trydeopplysninger fra NAV sitt API.

Evry sitt system har allerede en integrasjon i Maskinporten, og har tilgang til aktuelt scope på vegne av andre kunder, men siden de mangler delegering, vil de ikke få tokens utstedt til Leikanger kommune.

En bemyndiget ansatt (for Altinn er dette per default nøkkelrolleinnehaver fra Enhetsregisteret, f.eks. daglig leder, men påkrevd rolle kan også defineres i delegeringsoppsettet) i kommunen logger inn i Altinn, og delegerer tilgang til at Evry frå lov til å opptre på vegne av dem, opp mot APIet. Altinn lagrer delegeringen i sin delegeringstabell.

Når Evry nå forsøker å be om token, vil Maskinporten se at scopet er registrert med en delegeringskilde, som her er Altinn. Maskinporten spør da delegeringskilden over et standardisert API om tuplet `consumer_org,supplier_org,scope`, og Altinn returner en bekreftelse på at det finnes en delegering på en delegation scheme som omfatter det forespurte scopet. Maskinporten utsteder da følgende token:

```
{
  iss: "Altinn"
  scope: "nav:trygdeopplysninger"
  consumer_orgno: "964967725        # Leikanger Kommune
  act: {
    "supplier_orgno": "934382404"   # Evry ASA
  }
  ...
}
```

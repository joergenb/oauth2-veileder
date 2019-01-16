---
layout: page
title: "Brukerreise for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_brukerreise.html

summary: "En brukerreise som viser hvordan man etablerer og tilgangsstyrer APIer som følger eOppslag-mønsteret. "
---

## 0. NAV utvikler et API

utviklere holder på med en applikasjon som skal tilby et API, og ønsker å teste token-utstedelse.

"delegerbar"

skal "ressurser" synkroniseres, eller slås opp run-time?




## 1. NAV registrer et API i Maskinporten

NAV ønsker å opprette et API og gi tilgang til Gjensidige Forsikring.


NAV som API-tilbyder A blir manuelt provisjonert i ID-porten, ved at A sitt org.no får tildelt scope-prefixet `nav:`.

NAV kaller så selvbetjeningsAPIet for å opprette ressursdefinisjoner tilknyttet prefix:

```
POST /scopes/
{
  "prefix": "nav",
  "subscope": "trygdeopplysninger"  
}  
```
som oppretter scopet `nav:trygdeopplysninger`.   (alternativt bruker NAV en av GUI-løsningene integrert med Maskinporten og gjør   det samme via pek-og-klikk.)


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

Gjensidige lager en oauth2-klient som benytter virksomhetssertifikat. Gjensidige registrerer denne selv i Maskinporten ved å bruke selvbetjeningsAPIet, og  provisjonerer klienten med API-tilgangen virksomheten har fått tildelt:

```
POST /clients/
{
  "client_name": "Gjensidige sin maskinport-integrasjon"
  "token_reference": "SELF_CONTAINED",
  "scope": "nav:trygdeopplysninger",
  ...
}
```

Når lienten skal bruke APIet, generer den først en tokenforespørsel med scope=`nav:trygdeopplysninger`,  signerer dette med Gjensidige sitt virksomhetssertikat (et jwt-grant), og sender til Maskinporten. Maskinporten utsteder et access_token tilbake:
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

De lager raskt en Oauth2-klient, får denne registrert i Maskinporten, og forsøker sende en tokenforespørsel til Maskinporten på samme måte som Gjensidige.  Siden Storebrand ikke har fått tilgang, vil denne bli avvist. Storebrand sender så en tilgangsforespørsel til Maskinporten sitt selvbetjeningsAPI:
```
POST /scopes/access/requests
{
  "scope": "nav:trygdeopplysninger"
}
```

NAV er flinke og følger hyppig med på tilgangskøen for sitt API:
```
GET /scopes/access/requests?scope=nav:trygdeopplysninger
```
og ser at Storebrand har bedt om tilgang.  Denne godkjenner de tvert:
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

En bemyndiget ansatt i kommunen logger inn i Altinn, og delegerer tilgang til at Evry frå lov til å opptre på vegne av dem, opp mot APIet.  Dette er realisert ved at Altinn kaller selvbetjeningsAPIet for å oppdatere Maskinporten sin delegeringstabell:

```
POST /scopes/delegations/
{
  "scope": "nav:trygdeopplysninger",
  "consumer_orgno": "964967725,          # Leikanger Kommune
  "supplier_orgno": "934382404"          # Evry ASA
}
```

Når Evry nå forsøker å be om token, vil de få dette utstedt:

```
{
  iss: "Maskinporten"
  scope: "nav:trygdeopplysninger"
  consumer_orgno: "964967725        # Leikanger Kommune
  act: {
    "supplier_orgno": "934382404"   # Evry ASA
  }
  ...
}
```

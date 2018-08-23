---
layout: page
title: "Brukerreise for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_brukerreise.html

summary: "En brukerreise som viser hvordan man etablerer og tilgangsstyrer APIer som følger eOppslag-mønsteret. "
---


## 1. NAV oppretter et API

NAV ønsker å opprette et API og gi tilgang til Gjensidige Forsikring.


NAV som API-tilbyder A blir manuelt provisjonert i ID-porten, ved at A sitt org.no får tildelt eitt eller fleire scope-prefix.

NAV kaller så selvbetjeningsAPIet for å opprette ressursdefinisjoner tilknyttet prefix:

```
POST /scope ("nav:", "trygdeopplysninger")    
```
som oppretter scopet `nav:trygdeopplysninger`.   (alternativt bruker NAV en av GUI-løsningene integrert med Maskinporten og gjør pek-og-klikk for det samme.)


NAV vil så tildele noen tilganger:
```
POST /access/nav:trygdeopplysninger/ { "orgno"="995568217" }
```
som gir Gjensidige Forsikring tilgang til å få tokens med scope "nav:trygdeopplysninger".


## 2. Gjensidige bruker APIet

Når APIet er klart, får Gjensidige beskjed om hvilket scope de må ha for å benytte APIet, at de skal benytte Maskinpoten, samt dokumentasjon av selve APIet.

Gjensidige lager en oauth2-klient som benytter virksomhetssertifikat og får denne registrert i ID-portens OIDC provider.

Klienten generer en tokenforespørsel med scope=`nav:trygdeopplysninger`,  signerer dette med Gjensidige sitt virksomhetssertikat (et jwt-grant), og sender til Maskinporten.  Siden Gjensidige allerede har fått tilgang, vil Maskinporten utstede et access_token tilbake:
```
{
  iss: "Maskinporten"
  scope: "nav:trygdeopplysninger"
  client_orgno: "995568217"
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
POST /accessrequest/nav:trygdeopplysninger
```

NAV er flinke og følger hyppig med på tilgangskøen for sitt API:
```
GET /accessrequest/nav:trygdeopplysninger
```
og ser at Storebrand har bedt om tilgang.  Denne godkjenner de tvert:
```
POST /access/nav:trygdeopplysninger/ { "orgno"="930553506" }
```

Storebrand kan nå bruke NAV sitt API.


## 4. Leikanger Kommune ønsker at Evry skal bruke APIet for dem

Leikanger kommune kjøper et kommunalt fagsystem som skytjeneste fra leverandøren Evry. Systemet har behov for trydeopplysninger fra NAV sitt API.

Evry sitt system har allerede en integrasjon i Maskinporten, og har tilgang til aktuelt scope på vegne av andre kunder, men siden de mangler delegering, vil de ikke få tokens utsted til Leikanger kommune.

En bemyndiget ansatt i kommuneen logger inn i Altinn, og delegerer tilgang til at Evry frå lov til å opptre på vegne av dem, opp mot APIet.  Dette er realisert ved at Altinn oppdaterer Maskinporten sin delegeringstabell:

```
POST /delegations/
{
  scope= "nav:trydgeopplysninger",
  supplier_orgno: "934382404",        # Evry ASA
  consumer_orgno: "964967725          # Leikanger Kommune
}
```

Når Evry nå forsøker å be om token, vil de få dette utstedt:

```
{
  iss: "Maskinporten"
  scope: "nav:trygdeopplysninger"
  client_orgno:   "964967725          # Leikanger Kommune
  supplier_orgno: "934382404",        # Evry ASA
  ...
}
```

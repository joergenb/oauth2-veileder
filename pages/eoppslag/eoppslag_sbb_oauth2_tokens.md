---
layout: page
title: "Ulike Oauth2 tokens"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2eoppslag_sbb_oauth2_tokens.html
summary: "Ulike Oauth2 tokens brukt i det offentlige, både eOppslag og annen bruk "
---

## Description

|claim|Autentiseringsbevis (OIDC id_token)|Autensieringær autorisasjon - bruker innlogget i tjeneste uten delegering (oauth2 access_token)|Maskinporten idag|eOppslag - selvstendig konsument|eOppslag konsument via leverandør|Persondelegering |spec'|definisjon|
|-|-|-|-|-|-|-|-|-|
|sub|TWGi0...2GBY=|TWGi0...2GBY=||oidc_storbanken|oidc_eika_kunde1|TWGi0...2GBY=|oidc, jwt|OIDC: unik, tjenstespesifikk ikke-meningsbærende identifikator, JWT: "the recipient thta the JWT is intended for" |
|scope|openid|svv:pkk|global/kontaktopplysninger.read|nav:forsikring|nav:forsikring|oauth2|
|client_orgno||944117784|990 983 291|999888777|777888999|
|aud| oidc_kongsvingerkommune_acos|oidc_kongsvingerkommune_acos|oidc_authlevel_nav
|acr| Level4 |Level4|||se
|amr| BankID |BankID||
|pid|11111156789|11111156789 |||| personidentifikator i folkeregisteret (burde heitt folkeregisteridentifikator)|
|**Fremtidige claims**|
|act|||||`{ sub: oidc_eika, organisasjonsnummer: 999777555 }`|||Enhet|Norsk organisasjonsnummer (utenlandske foretak vil få et annet claim)|
|client_id|| |
|


Følgende claims er like i alle varianter:

|claim|spec'|definisjon|
|-|-|-|
|iss|oidc, jwt|Utsteder (idporten.difi.no)
|exp|oidc, jwt|tidspunkt for utløp
|iat|oidc, jwt|tidspunkt for utstedelse
|jti|oidc, jwt|unik identifikator for selve tokenet


## sikkerhetsnivå

Personinnlogging:
* Level3
* Level4

Virksomhet:
* eIDAS elektronisk segl
* eIDAS avansert segl
* eIDAS kvalifisert segl



## Referanser

* [Enhet] : [Felles informasjonsmodell for Person og Enhet](https://www.difi.no/fagomrader-og-tjenester/digitalisering-og-samordning/nasjonal-arkitektur/informasjonsforvaltning/person-og-enhet-felles-informasjonsmodell)

*

---
layout: page
title: "Ulike Oauth2 tokens"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_tokens.html
summary: "Ulike Oauth2 tokens brukt i det offentlige, både eOppslag og annen bruk "
---

## Description

|claim|Autentiseringsbevis (OIDC id_token)|Autensieringær autorisasjon - bruker innlogget i tjeneste uten delegering (oauth2 access_token)|Maskinporten idag|eOppslag - selvstendig konsument|eOppslag konsument via leverandør| Ansatt pålogging (delegering) |spec'|kommentar|
|-|-|-|-|-|-|-|-|-|-|
|sub|TWGi0...2GBY=|TWGi0...2GBY=||oidc_storbanken|oidc_eika_kunde1|TWGi0...2GBY=|OIDC, JWT|OIDC: unik, tjenstespesifikk ikke-meningsbærende identifikator, JWT:  The "sub" (subject) claim identifies the principal that is the  subject of the JWT. |
|scope|openid|svv:pkk|global/kontaktinformasjon.read|nav:forsikring|nav:forsikring|nav:forsikring|Oauth2|
|client_orgno||944117784|974761076|999888777|777888999|936796702
|aud| oidc_kongsvingerkommune_acos|oidc_svv_vitecautodata|oidc_skatteetaten_krr|https://api.nav.no/|https://api.nav.no/||OIDC, JWT| `aud` utlevers kun når klient ber om det|
|acr| Level4 |Level4||egen_nøkkel|kvalifisert_segl|Level4|oidc|Sikkerhetsnivå (Auth. context class ref. )|
|amr| BankID |BankID||||BankID|oidc|
|pid|11111156789|11111156789 ||||11111156789 | [krr] | personidentifikator i folkeregisteret (burde heitt folkeregisteridentifikator)|
|**Fremtidige claims**|
|act|||||`{ sub: "eika-gruppen", organisasjonsnummer: 999777555 }`|||Enhet|Norsk organisasjonsnummer (utenlandske foretak vil få et annet claim)|
|may_act||||||`{ sub: "min arbeidsgiver", organisasjonsnummer: 999555111, iss: "altinn autorisasjon" }`|
|client_id||oidc_svv_vitecautodata || evt bruke client_id istedenfor/ i tillegg til `sub` ?|| oidc_visma_skyregnskap


Følgende claims er like i alle varianter:

|claim|spec'|definisjon|
|-|-|-|
|iss|oidc, jwt|Utsteder (idporten.difi.no)
|exp|oidc, jwt|tidspunkt for utløp
|iat|oidc, jwt|tidspunkt for utstedelse
|jti|oidc, jwt|unik identifikator for selve tokenet


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

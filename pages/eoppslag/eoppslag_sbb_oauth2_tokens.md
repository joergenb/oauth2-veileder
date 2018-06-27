---
layout: page
title: "Ulike Oauth2 tokens"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_tokens.html

summary: "Ulike Oauth2 tokens brukt i det offentlige, både eOppslag og annen bruk "
---

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
|client_id||oidc_svv_vitecautodata || oidc_storbanken (evt autogenerert)|oidc_eika_kunde23 (evt. autogenerert)|| oidc_visma_skyregnskap


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

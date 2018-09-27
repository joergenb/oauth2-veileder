---
layout: page
title: "Validering av Oauth2 tokens"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_tokenvalidering.html

summary: "Korleis skal du validere tokens utstedt av ID-porten/maskinporten "
---

Utviklere som integrerer mot ID-porten / Maskinporten forventes å kjenne til [RFC6819: trusselmodellen for Oauth2](https://tools.ietf.org/html/rfc6819) og legge denne til grunn for sine risikovurderinger.  Difi kommer ikke til å gjenta valideringstiltak som allerede står i spec'ene , men vi vil trekke fram følgende overordna anbefalinger:

1) Bruk et anerkjent IAM-produkt / bibliotek for integrasjonen.
2) Ha en trygg håndtering av nøkler slik at disse ikke kommer på avveie


## id-token

Dette er presist beskrive i [OIDC-spesifikasjonen kap 3.1.5.3](https://openid.net/specs/openid-connect-core-1_0.html#TokenResponseValidation)
(samt kap 3.2.2.11 for implicit-flyt og 3.3.2.12/3.3.3.7 for hybrid flyt )


## access_token


https://tools.ietf.org/html/rfc6749?#section-5.1

1. Valider token teknisk ihht Oauth2 og evt. JWT-spec.  Viss den feiler -> forkast
    1.  iss må være ID-porten
    2.  signatur på self-contained access-token / introspection respons må stemme med sertifikatet publisert på ID-porten sitt .jwks-endepunkt
    3. dersom introspection, valider at aud til deg selv
    4. dersom self-contained access token, valider at et eventuelt aud-claim er til deg selv som api-tilbyder

2. Sjekk token-type (`type`), om det er person-innlogging eller virksomhetsinnlogging
     1. dersom *virksomhet*:
         * finn konsument frå  `consumer_orgno`
         * dersom du har behov for å logge hvilken levendør, finnes dette i `supplier_orgno`

    2. dersom *person*:
        1. fødseslnummer finnes i 'pid'-claimet
            1. dersom pseudonymisert access_token finner du fødselsnummer ved å utføre introspection mot ID-porten 

1.

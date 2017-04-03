---
title: Anbefalte tiltak for transport-/kanalsikring
pageid: veileder-channel
layout: default
description: Anbefalte tiltak for transport-/kanalsikring
isHome: false
hiddenInToc: false
---

## Anbefalte tiltak for transport-/kanalsikring

Bruk av Transport Layer Security (TLS) er obligatorisk.

OAuth 2.0 spesifiserer ingen sikkerhetsmekanismer som en del av protokollen. Protokollen legger opp til at sikkerhetsmekanismer for konfidensialitet og integritet legges til andre protokollag. OpenID Connect stiller derimot krav om bruk av TLS [OIDC], kapittel 16.17. 

### Krav og anbefalinger for konfigurering av TLS

Difi anbefaler at tjenestetilbydere følger Norsk Sikkerhetsmyndighets (NSM) veileder for Sikring av kommunikasjon med TLS [NSMTLS]. Dette gjelder både for konfigurasjon og verifikasjon av implementasjon.

Tabellen under viser støttede konfigurasjoner for TLS:

Versjon | Status | Ciper-suite | etc.
--- | --- | --- | ---
TLS v1.3 | Støttet  | N/A  | N/A  
TLS v1.2 | Støttet  | N/A | N/A
TLS v1.1 | Støttet | N/A | N/A
TLS v1.0 | Støttes ikke | N/A | N/A 



---
layout: page
title: "Grensesnitt for eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_grensesnitt.html

summary: "Dette dokumentet beskriver nye grensesnitt og utvidelser på eksisterende grensesnitt for å kunne realisere referansearkitekturen i eOppslag."
---


## Grensesnittsdefinisjoner

De fleste grensesnittene i løsningsarkitekturen følger standardisert Oauth2, men for å støtte funksjonaliteten i eOppslag, trengs det tre proprieteære APIer:

### 1. Token-tjenestens selvbetjenings-API

Dette APIet blir brukt av API-tilbydere

> Design av endringene er dokumentert i RAML her: [versjon 0.5.0](https://github.com/joergenb/oauth2-veileder/blob/gh-pages/pages/eoppslag/assets/eoppslag_0.5.0_restlet.yaml). (kan åpnes med Restlet Studio)

Utledning av grensesnittsdefinisjonen kan leses [her](/eoppslag_sbb_oauth2_adminapi.html).

### 2. Delegeringskildens API for oppslag i delegeringsforhold

Dette APIet blir kun brukt av Token-tjenesten for å registrere delegerbare ressurser og validere gjeldende delegeringsforhold.

> Forslag til design er dokumentert i OAS3 her: [versjon 0.4.0](
https://github.com/joergenb/oauth2-veileder/blob/gh-pages/pages/eoppslag/assets/delegations_0.4.0_oas.yaml)

### 3. Grensesnitt mellom API-katalog og token-tjenestens

Gjenstår å definere.

### Andre APIer

Registering og provisjoner av Oauth2 klienter skjer vha Oauth2 Dynamic Client Registration.  Merk at såkalt "unmanaged/open" DCR ikke støttes.  Brukere av APIet må ha en selvbetjenings-integrasjon som er kjent for Maskinporten.

Tokenutstedelse skjer over standard Oauth2, med bruk av RFC7523 med virksomhetssertifikater og/eller egenregistrerte asymetriske nøkler.

Denne arkitekturen sier ikke noe om grensesnittet for selve APIet til API-tilbyder, utover at det er sikret med Oauth2 Bearer tokens.  

---
layout: page
title: "Systemlandskap"
sidebar: oauth2
permalink: systemlandskap.html

summary: "Hvordan ser Oauth2-verdenen ut?"
---


## Overordnet systemlandskap

I offentlig sektor har vi flere aktører som har fellesløsninger basert på Oauth2/OpenID Connect.   Figuren under viser en overorndet arkitektur, med noen eksemler på koblinger.

<div class="mermaid">
graph LR

 subgraph autorisasjonslag
  dataporten
  altinn
  idporten-oidc
  ehelse-as
 end

 subgraph API-lag
  mittapi["Mitt API"]
  dittapi["Annet API"]

  mittapi-. kontrollerer tilgang .- dataporten
  dittapi-. kontrollerer tilgang .- altinn
 end

 subgraph Klienter
  subgraph Tradisjonell netttjeneste
    browser
    applikasjonsserver
    browser-->applikasjonsserver
  end
  SPA
  native["Mobil-app"]

  applikasjonsserver-->mittapi
  applikasjonsserver-->dittapi
  applikasjonsserver-- Ber om tilgang --- dataporten
  applikasjonsserver-- Ber om tilgang --- altinn

  SPA-->mittapi
  native-->mittapi
 end

 subgraph autentiseringslag
   idporten
   feide
   fia

   idporten-oidc  --- idporten
   dataporten --- feide
   altinn --- idporten
   ehelse-as --- fia
   fia --- idporten
 end

</div>


Konsekvenser av denne arkitetekturen er mellom anna

Tjenesteutvikler (klienter):
* ulike API-er kan ha ulike autorisasjonsservere.  Liten forskjell fra idag,  APIer til tilbudt på mange forskjellige protokoller.   


API-tilbydere ( ressursservere):
* brukerne av APIer kan være alt fra sikra system til reine nettleser/javascript.  Ønskjer dei ha kontroll på dette?

autorisasjonsservere
* bør stole på autorisasjoner frå andre AS:
* bør tilby felles "orkestreingsfunksjonalitet" - samvirke mellom AS.
* harmoisert identifikasjon av klienter til ressursservere



## spørsmål til workshop

* Der løysingane samspeler
  * kva må vi standardisere?
  * kva kan vere forskjellig mellom sektorane
* bruksområde for dei ulike fellesløysingane
  * sjå døme


## dokumentasjon tilknytta dei ulike løysingane

* samtykkeløsningen i altinn  [https://altinn.github.io/docs/api/sluttbruker-api/diverse/samtykke/](https://altinn.github.io/docs/api/sluttbruker-api/diverse/samtykke/)
* skatteetaens bruk av altinn samtykkeløsning: [https://skatteetaten.github.io/datasamarbeid-api-dokumentasjon/about_samtykkelosning.html](https://skatteetaten.github.io/datasamarbeid-api-dokumentasjon/about_samtykkelosning.html)
* id-porten oidc [https://difi.github.io/idporten-oidc-dokumentasjon/](https://difi.github.io/idporten-oidc-dokumentasjon/)
* fia [https://fia-sikkerhet.github.io/](https://fia-sikkerhet.github.io/)







### ulike typer klienter

| Klient | Beskrivelse | Anbefalinger |
| --- | --- | --- |
| Nett-tjeneste | Tradisjonell nett-tjeneste med applikasjonsserver i sikret miljø. <br/> AS kan stole på at klient-hemmelighet / virksomhetssertifikat ikke kommer på avveie. <br/><br/>= confidential klient ihht. oauth2. | Bruk authorization code flow. <br/>PKCE ikke nødvendig.<br/>Tilstrekkelig med self-contained tokens (som oftest)<br/>Bruk refresh_token dersom token skal vare "lenge" |
| SPA / Browser-tjeneste | Tjenesten kjører i brukers browser, typisk single-page javascript applikasjon.<br/>= public client ihtt oauth2.<br/> AS kan ikke stole på at tjenesten kan holde på en hemmelighet / virksomhetssertifikat, eller beskytte tokens over lengre tid. <br/> SPAen trenger strengt tatt ikke motta id-token selv, dvs. kun behov for autorisasjon for å aksessere APIer |For API’er: bruk by-reference access_tokens og finn identitet fra /tokeninfo hos AS.<br/>Bruk: implicit flow <br/> Ikke bruk refresh_token, gi ut kort-levde access_token, og evt. bruk SSO i AS/ OIDC provider for at brukeren skal slippe å logge inn igjen. |
| Mobil-app | = oauth2 native app | Bruk: code flow + PKCE (for å beskytte seg mot «slemme» apper)<br/> Kan motta og persistere refresh_tokens ?<br/> Vudere dynamic client registreration, med instans-spesifikk client_id. (evt. med client-secret) |

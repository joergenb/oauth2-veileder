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

 subgraph Tjeneste-lab
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


Nøkkel-egenskaper:

* distribuert arkitektur
* bredde i typer tjenester(konsumenter)
* bredde i typer APIer
* både innbygger-rettede og maskin-rettede tjenester
* både private og offentlige virksomheter

**Mål**:  felles- og sektorløsningene bør være "like", slik at det blir enkelt å bygge tjenster på toppen av dem.

## bruksområder

* autentisering
  * autentiseringsnær autorisasjon
* samtykke/autorisasjon



## Governance




## Eksisternde dokumentasjon tilknytta dei ulike løysingane

* samtykkeløsningen i altinn  [https://altinn.github.io/docs/api/sluttbruker-api/diverse/samtykke/](https://altinn.github.io/docs/api/sluttbruker-api/diverse/samtykke/)
* skatteetaens bruk av altinn samtykkeløsning: [https://skatteetaten.github.io/datasamarbeid-api-dokumentasjon/about_samtykkelosning.html](https://skatteetaten.github.io/datasamarbeid-api-dokumentasjon/about_samtykkelosning.html)
* id-porten oidc [https://difi.github.io/idporten-oidc-dokumentasjon/](https://difi.github.io/idporten-oidc-dokumentasjon/)
* fia [https://fia-sikkerhet.github.io/](https://fia-sikkerhet.github.io/)
* Dataporten [https://docs.dataporten.no/](https://docs.dataporten.no/)

# spørsmål til workshop

* Der løysingane samspeler
  * kva må vi standardisere?
  * kva kan vere forskjellig mellom sektorane
* bruksområde for dei ulike fellesløysingane
* forholdet til private verksemder

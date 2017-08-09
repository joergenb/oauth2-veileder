---
layout: page
title: "Risikomatrise"
sidebar: oauth2
permalink: risikomatrise.html

summary: "risikomatrise"
---


## Kategorisering av tjenester og risiko

tiltak skal stå i stil med eksponert risiko.  

Forsøker å definere 3 kategorier tjenester / APIer/ "informasjon".  Gjenbruke risiko-klasse frå "rammeverk for autentisering og uavviseslighet "   TODO:  er det anna lovverk som er meir relevant? Har Normen i helsevesenet noko vi kan bygge på?

Overordna spørsmål i risikovurderinga:

* Kva data beskytter eg ?
* Kor mange eksponerer eg ?
* Kva brukomåråde?




| område | høg | medium | låg |
|-|-|-|-|
|**Kva data beskytter eg ?** | utlevere personsentivive opplysninger. Gjere handlingar med "stor" konsekvens for brukar| "alt anna" | returnerer ingen person(sensitive) opplysninger. Kan ikkje gjere "viktige" val på vegne av bruker|
|**Kor mange eksponerer eg?** | befolkningen?  | ? |? |
|Døme på APIer|helsejournaldata, sjukemeldingar, søke lån| bestille legetime, søke barnehageplas, (prosessar som konsekvens av) å melde flytting.| sjå ungane sin timeplan, mine biler|
| Oauth2-flyt| auth.code|auth.code| auth.code<br>implicit|
| støtta klienter | private | private, <br>public krever PKCE| private<br> public|
| klient-autentisering| private_key_jwt (virksomhetssertifikat-basert)|private_key_jwt, client_secret_basic | private_key_jwt, client_secret_basic, none|
| Konfidensialitet (id-token, self-contained access-tokens, API-respons)| TLS + kryptert respons | TLS | TLS |
| Integritetsbeskytta responser| Ja|Ja|Nei|
|Autentiseringsnivå for å autorisere|4|3|1|



## spørsmål til workshop:

* er det eit felles risikobilde ?
  * dele risiko-analyser?

* dersom du er RS, kva krav kan / bør du stille til klienten?  
  * og skal/bør AS vite noko om desse?
* er risikoen ved å utlevere sensivitve data over HTTPS til nettleser mindre enn å sende dette til ein public-klient ?
* Er det vesensforskjellig å risiko-klassifisere APIer kontra nett-tjenester?  Samme informasjonsverdiene som ein beskytter.() 3 parter i staden for 2)

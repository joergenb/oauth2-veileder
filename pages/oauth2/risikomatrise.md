---
layout: page
title: "Risikomatrise"
sidebar: oauth2
permalink: risikomatrise.html

summary: "risikomatrise"
---


## "risko-nivå"

tiltak skal stå i stil med eksponert risiko.  

Forsøker å definere 3 kategorier tjenester / APIer/ "informasjon".  Gjenbruke risiko-klasse frå "rammeverk for autentisering og uavviseslighet "   TODO:  er det anna lovverk som er meir relevant? Har Normen i helsevesenet noko vi kan bygge på?

Er det vesensforskjellig å risiko-klassifisere APIer kontra nett-tjenester?  Samme informasjonsverdiene som ein beskytter.   ein forskjell er 3.dje-parten


| område | høg | medium | låg |
|-|-|-|-|
|Døme på APIer|helsejournaldata, sjukemeldingar, søke lån| bestille legetime, søke barnehageplas, (prosessar som konsekvens av) å melde flytting.| sjå ungane sin timeplan, mine biler|
| Oauth2-flyt| Auth.code|auth.code<br>auth.code m PKCE | auth.code<br>implicit|
| støtta klienter | kun private | private, <br>public m/ PKCE| private<br> public|
| klient-autentisering| private_key_jwt (virksomhetssertifikat-basert)|private_key_jwt, client_secret_basic | private_key_jwt, client_secret_basic, none|
| Konfidensialitet (id-token, self-contained access-tokens, API-respons)| TLS + kryptert respons | TLS | TLS |
| Integritetsbeskytta responser| Ja|Ja|Nei|
|Autentiseringsnivå ved autorisasjon|4|3|1|



spørsmål :

* er risikoen ved å utlevere sensivitve data over HTTPS til nettleser mindre enn å sende dette til ein public-klient ?

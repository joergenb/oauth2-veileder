---
layout: page
title: "Tjenester"
summary: "Tjenester"
keywords: sample
sidebar: oauth2
folder: oauth2
permalink: klienter.html
---






## Ulike typer tjenester / klienter

| Klient | Beskrivelse | Anbefalinger |
| --- | --- | --- |
| Nett-tjeneste | Tradisjonell nett-tjeneste med applikasjonsserver i sikret miljø. <br/> AS kan stole på at klient-hemmelighet / virksomhetssertifikat ikke kommer på avveie. <br/><br/>= confidential klient ihht. oauth2. | Bruk authorization code flow. <br/>PKCE ikke nødvendig.<br/>Tilstrekkelig med self-contained tokens (som oftest)<br/>Bruk refresh_token dersom token skal vare "lenge" |
| SPA / Browser-tjeneste | Tjenesten kjører i brukers browser, typisk single-page javascript applikasjon.<br/>= public client ihtt oauth2.<br/> AS og RS kan ikke stole på at tjenesten kan holde på en hemmelighet / virksomhetssertifikat eller beskytte tokens over lengre tid. <br/> SPAen trenger strengt tatt ikke motta identitet selv, dvs. kun behov for autorisasjon for å aksessere APIer |For API’er: bruk by-reference access_tokens og finn identitet fra /tokeninfo hos AS.<br/>Bruk: implicit flow <br/> Ikke bruk refresh_token, gi ut kort-levde access_token, og evt. bruk SSO i AS/ OIDC provider for at brukeren skal slippe å logge inn igjen. |
| Mobil-app | = oauth2 native app | Bruk: code flow + PKCE (for å beskytte seg mot «slemme» apper)<br/> Kan motta og persistere refresh_tokens ?<br/> Vudere dynamic client registreration, med instans-spesifikk client_id. (evt. med client-secret) |


(tabellen er vel litt duplikat av [risikomatrise](risikomatrise.html)






## Spørsmål til workshop

* diskutere anbefalingene

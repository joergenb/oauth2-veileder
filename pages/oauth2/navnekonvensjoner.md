---
layout: page
title: "Navnekonvensjoner"
sidebar: oauth2
permalink: navnekonvensjoner.html

summary: "skal vi harmonisere navnekonvensjoner"
---

## governance ?



## kandidater for navnekonvensjoner:

* scopes
* claims
* client_id

_må_ vere laust kobla, eller so blir det kaos!

## overordna framlegg:
* _kan_ **standardisere prinsipp** for navngjeving innen **domene/sektor**
* _bør_ **reservere** utvalde scopes/claims nasjonalt

### domene-spesifikke prinsipp:

* namespacing (https://nhn.no/claims/hpr/hpr_number)
* prefix med orgno  ()
* prefix med client_id


#### eksempel fra virkeligheten
```
global/idporten.authlevel.read
user/kontaktopplysninger.read
https://nhn.no/claims/hpr/hpr_number
```

døme frå skatt sitt API:

|Resource	|‘subparameter’	|brukes med tjeneste	|forklaring	|eksempelverdi	|obligatorisk|
|-|-|-|-|-|-|
|4628;1|	4628_1_inntektsaar	|Skattegrunnlag	|inntektsaar det skal hentes skattegrunnlag for|	4628_1_inntektsaar;2015	|obligatorisk|

User-managmed access [https://docs.kantarainitiative.org/uma/rec-uma-core.html](https://docs.kantarainitiative.org/uma/rec-uma-core.html) snakker om "resource sets".




## nasjonale scopes / claims


* organisasjosnummer
* personidentifikator (idporten bruker både ```uid``` og ``

## Spørsmål til workshop


* standarder og/eller arbeid (skate) vi kan bygge på

* "alle skal (bør) tilby eit  ```profile``` scope"  (kva skal det evt. inneholde?" fødselsnummer og ein vilkårleg tekst  { "12345678901", "Eier to biler", url til ressurs)

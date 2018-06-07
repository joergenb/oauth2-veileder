---
layout: page
title: "eOppslag Architecture Building Block"
sidebar: eoppslag
permalink: eoppslag_abb.html

summary: "Detaljert spesifikasjon av referansearkitekturen for eOppslag"
---

## Archimate tegning

### Klaus sin modell:

Merk: bruker EIRA-byggesteiner / notasjon

<div class="mermaid">
graph LR

 subgraph API-konsument
  bus1[Exchange of Business Information]
 end

 subgraph API-tilbyder
  bus2[Exchange of Business Information]
 end

 bus1 -- REST-kall --> bus2

 subgraph fellestjenster
  cl[Capability Lookup]
  am[Access Management Service]
  ims[Identity Management Service]
 end


</div>

(burde Bus.  det vore Data Exchange service / component ?)
(treng vi Trust Registry Service ?)


### Rolf sin modell :

![bilde](assets/eoppslag_abb-6a8ed.png)




## Architectural Description

### komponenter

#### Virksomhetsautentisering(Identity Management Service)

Vil si å autentisere ein virksomhet

I dagens løsninger for maskingrensesnitt mellom offentlige etater, og mellom private og det offentlige, brukes i stor grad virksomhetssertifkater for autentisering over TLS tilkoblinger.

Bruk av virksomhetssertifikater er forankret i følgende:
•	https://www.regjeringen.no/no/dokumenter/rammeverk-for-autentisering-og-uavviseli/id505958/
•	https://www.regjeringen.no/no/dokumenter/kravspesifikasjon-for-pki-i-offentlig-se/id611085/
•	https://lovdata.no/dokument/SF/forskrift/2004-06-25-988

Som en konsekvens av innføring av eIDAS-forordningen i norsk lov i 2018, vil overnevnte dokumenter bli harmonsiert til å være i tråd med eIDAS.  De nye dokumentene er pt. på høring. For virksomhetsautentisering medfører dette at virksomhetssertifikater blir erstattet av såkalte 'eIDAS segl'.  Disse kommer i flere avarter:  elektroniske segl, avanserte segl og kvalifiserte segl.



avledede/egen-produserte nøkler

bruksområde av nøkler


####  FYLL PÅ

### Aktører:

•	Tjenesteeier (A): den part som tilbyr en tjeneste gjennom et API
•	Konsument (C): den part som skal bruke tjenesten og juridisk sett er mottaker
•	Konsumentens driftsleverandør (L): en separat organisasjon som konsumenten bruker til å forvalte deler av eller hele sin tekniske infrastruktur, samt som i større eller mindre grad er integrert i konsumentens forretningsprosesser. Her begynner det å bli vanskelig, fordi det finnes (minst) tre ulike modeller og det er utydelig hvor grensene går:
1.	C og L er samme organisasjon (eller L sin rolle er ubetydelig i denne sammenhengen): her kan man se bort fra de tre boksene til venstre i figuren (leverandør, plattform, og avtalen med leverandøren) og det hele blir mye enklere
2.	L er en sky-leverandør, men C forvalter klienten selv (for eksempel: C kjøper programvare fra Visma og installerer i Azure. I denne sammenhengen er Microsoft en «databehandler», men fra A sitt perspektiv er L usynlig)
3.	L leverer en integrert tjeneste til C, typisk i en eller annen slags delt løsning, men hvor vi typisk ønsker at L autentiseres som seg selv, men er autorisert til å opptre på vegne av flere C. (for eksempel: Eika opptrer på vegne av 57 banker, et regnskapskontor opptrer på vegne av sine kunder)
•	Programvareleverandør (S): der hvor konsumenten (C) ikke går gjennom en driftsleverandør, men integrerer direkte vil det i mange sammenhenger ikke være C som lager programvaren, men de kjøper for eksempel et ERP-system fra en leverandør, og dette kaller tjenesten. Hvorvidt S er relevant i tilgangsstyringen vil variere.





### Scenario:

Scenario navn	Typisk antall konsumenter	Typisk antall tjenesteeiere	Beskrivelse
Lovhjemlet	1-10	1	Utveksling av informasjon mellom offentlige etater hvor dette er spesifikt hjemlet i lov og forskrift, og hvor den aktuelle informasjonen typisk inngår i definerte prosesser hos alle involverte parter. Et eksempel er utlendingsforvaltningen som er fordelt mellom Politiet, UDI og UNE, og hvor disse utveksler bestemte opplysninger på definerte steder i saksgangen.
Etter avtale	10-1000	1	Oppslag på informasjon hvor bruk av informasjonen er begrenset i lov og forskrift, men med en større bredde i konsumenter og bruksområder, samt definerer bredere grupper av konsumenter («alle kommuner» eller «alle banker»).
Alle virksomheter	Over 1 million	1	Tjenester som alle registrerte virksomheter i prinsippet får lov til å bruke
Revers	1-10	10-1000	Der hvor offentlige etater trenger å kalle tjenester hos private virksomheter for å hente informasjon om disse virksomhetene sine kunder.


Utenfor scope i denne omgang:
•	Virksomhet gjør kall på vegne av en borger
•	Utenlandske virksomheter

For alle scenariene hvor det er flere enn 10 aktører må det også støttes at det finnes sentrale leverandører eller bransjeorganisasjoner som opptrer på vegne av flere aktører (for eksempel «eika» på vegne av en rekke banker, eller en regnskapsfører på vegne av sine kunder).


## Profile

## Standards

---
layout: page
title: "eMelding ABB oversikt"
sidebar: eMelding
permalink: emelding_abb_overview.html

summary: "Grunnleggende om eMelding referansearkitekturen"
---

## Description

1.	Bakgrunn

I 2013 gjennomførte Difi en omfattende kartlegging på området Meldingsutveksling i offentlig sektor, som viser et klart rom for forbedring (Difi Rapport 2013:13 ). Store og små offentlige virksomheter benytter ofte papirpost eller e-post for å kommunisere med hverandre i mangel av bedre alternativer. Det ble også i rapporten påpekt at det utvikles for mange enkeltstående løsninger for meldingsutveksling.
Det ble også kartlagt hvordan og i hvilke sammenhenger offentlige virksomheter utveksler informasjon. Kartleggingen viste at meldingsutveksling mellom offentlige virksomheter ofte inngår i en større arbeidsprosess, hvor innbyggere eller private virksomheter er involvert i ett eller flere av trinnene. Samtidig kan forvaltningsorganer på ulike forvaltningsnivåer være involvert. 
I 2015 ble det arbeidet frem en Løsning for meldingsutveksling i offentlig sektor (Difi Rapport 2015:3 ). Disse to rapportene redegjør tilsammen for behov, krav og mulige løsninger for en samlet arkitektur for digital meldingsutveksling (heretter omtalt som meldingsutveksling) mellom offentlige virksomheter, mellom offentlige og private virksomheter og mot innbyggere.
Difi er et sentralt redskap i regjeringens arbeid med å utvikle og effektivisere offentlig sektor. Difi skal løse felles utfordringer i offentlig sektor som krever samordnet innsats. Meldingsutveksling er et område der det kreves samordning og denne rapporten er et direkte resultat av at Difi i sitt tildelingsbrev for 2017 har fått i oppdrag å utarbeide en strategi og referansearkitektur for meldingsutveksling.
Norge skal være en del av det digitale indre marked i Europa  og ta i bruk felles europeisk IKT-infrastruktur. Tillitstjenestene beskrevet i eIDAS-forordningen , som også setter krav til meldingsutveksling, blir tilgjengelig som fellestjenester som kan benyttes både av offentlige og private virksomheter. CEF eDelivery , som er den anbefalte infrastrukturen for meldingsutveksling i EU, danner grunnlaget for eMelding som en nasjonal strategi og referansearkitektur for meldingsutveksling. Norge er deltagere i EU-programmet CEF Digital, som har som målsetting å etablere og forvalte en felles IKT-infrastruktur på tvers av landegrensene i Europa, deriblant CEF eDelivery, og er en viktig forutsetning for det digitale indre markedet. Norsk deltakelse i programmet legger til rette for at norske løsninger kan kobles til den felles infrastrukturen.





 
2.	Målsetting og visjon

Strategi og referansearkitektur for meldingsutveksling er et oppdrag gitt av KMD i tildelingsbrevet for 2017: «Difi skal utarbeide strategi og referansearkitektur for meldingsutveksling». Arbeidet bygger videre på Difis meldingsutvekslingsrapporter 2013:13 og 2015:3, som primært omfattet offentlig sektor, samt på Difis arbeid med å digitalisere offentlige anskaffelsesprosesser. Denne referansearkitekturen gjelder også meldingsutveksling mellom offentlig sektor og privat sektor.  Den kan også benyttes for meldingsutveksling mellom private virksomheter der de private aktørene finner det formålstjenlig.
Referansearkitektur
Formålet med referansearkitekturen foreMelding, er å beskrive hvordan meldingsutveksling bør skje internt i offentlig sektor, samt mellom offentlig sektor og privat sektor, basert på den europeiske byggeklossen CEF eDelivery. Bruk av referansearkitektur vil forenkle utvikling, drift og forvaltning av meldingsutveksling og legge til rette for nye meldingsutvekslingsmønstre.
Målgruppe for referansearkitekturen er arkitekter, forretningsutviklere og prosjektledere som jobber med digitalisering av forretningsprosesser som involverer flere virksomheter.
Referansearkitekturen består av denne strategien som sier hvordan referansearkitekturen skal tas i bruk, og en teknisk del som er arkitekturmalen.

Referansearkitekturen vil definere roller og plassere ansvar, veilede og stille krav til arkitektur og standarder som skal benyttes når meldinger utveksles, basert på beste praksis i EU. Hvor det er aktuelt vil referansearkitekturen peke på konkrete løsninger som kan benyttes.
Strategi
Hensikten med denne strategien er å sikre at eMelding referansearkitekturen tas i bruk, så raskt som mulig med lavest mulig ressursbruk. Dette vil føre til konsolidering av hvordan meldinger utveksles internt i offentlig sektor samt mellom offentlig sektor og privat sektor. Strategien vil ha tiltak for å plassere ansvar for referansearkitekturen og komponenter i infrastrukturen samt stille krav til bruk av referansearkitekturen.
Målgruppe for denne strategien er virksomhetsarkitekter og ledere med ansvar for digitalisering.










Visjon for meldingsutveksling illustreres i figuren under:

Effektmål
Følgende effektmål gjelder for referansearkitektur for meldingsutveksling:
•	En mer ensartet måte å utveksle meldinger i og med offentlig sektor.
•	Offentlig norsk sektor er bedre i stand til å utveksle meldinger innen EØS-området (over landegrenser).
•	Enklere utvikling av løsninger for meldingsutveksling ved bruk av referansearkitekturen.

Resultatmål
Innholdet i denne strategien er et resultat av prosjektets mål:
•	Tilby forslag til en norsk referansearkitektur for eMelding basert på CEF eDelivery.
•	Lage forslag til en strategi som anbefaler hvordan eMelding skal behandles videre.
•	Beskrive ulike samhandlingsmønstre for å få en mer presis begrepsbruk på området.
•	Identifisere problemstillinger som bør løses for å forenkle eMelding.


 
3.	 Generelt om referansearkitektur

3.1 Hva er en referansearkitektur?
En referansearkitektur er en løsningsmal for hvordan virksomheter utvikler løsninger innenfor et avgrenset område. Referansearkitekturen beskriver forretningsmessige mål, fastlegger prinsipper og begreper som gjelder for området, hvilke standarder og eventuelt hvilke teknologier som bør benyttes, og hvordan virksomheten skal realiserer effekter både på forretningsnivå og teknisk nivå.
Det finnes ulike typer referansearkitekturer:
•	Grunnleggende referansearkitekturer, som beskriver generelle krav til for eksempel informasjonssikkerhet.
•	Tekniske, som beskriver arkitekturen og peker på relevante teknologiske løsninger som skal utvikles og brukes, for eksempel krav til tegnsett når data skal utveksles mellom registre.
•	Anvendelsesorienterte, som innen et spesifikt forretningsområde peker på prinsipper og retningslinjer til konkrete løsninger, f.eks. denne referansearkitekturen om meldingsutveksling.
Referansearkitekturer kan beskrives på ulike abstraksjonsnivåer. Omfangsrike og altomfattende arkitekturer kan være hinder for innovasjon. Det viktig å finne rett balanse på omfang og detaljeringsnivå. Hvis ensartet samhandling er viktig, bør kravene som stilles være mer detaljert.
Referansearkitektur må ta utgangspunkt i brukerbehov, være basert på erfaring og peke på beste praksis.
Referansearkitekturene gir både offentlige og private virksomheter felles retning på nyutvikling, videreutvikling og anskaffelser av offentlige samhandlingsløsninger.
Figuren nedenfor viser sammenhengen mellom forretningsmessige mål og behov for referansearkitektur, som påvirker løsninger, og som inngår i forvaltningens digitale samhandlingsplattform.

Figur 2: Rammeverk for referansearkitektur


3.2 Hvorfor bruke referansearkitektur?
Referansearkitektur tilbyr harmonisering av samhandlingsarkitekturer gjennom felles begrepsbruk, gjenbrukbare løsninger og erfaring basert på beste praksis.
Økt fart: Referansearkitekturer gir et modent startpunkt for arbeidet med arkitektur i et utviklingsprosjekt. Virksomheten trenger ikke finne opp hjulet på nytt.
Høyere kvalitet: Referansearkitekturer baserer seg på erfaring og beste praksis.
Mer Gjenbruk: Referansearkitekturer peker på ressurser som kan gjenbrukes, og dermed enklere å finne.
Sterkere harmonisering og samhandling: Harmoniserer samhandlingen generelt og på tvers av sektorer og forvaltningsnivå.
Effektivt samarbeid på tvers: Det er enklere å samarbeide på tvers av sektorer og forvaltningsnivå ved bruk av felles begreper, modeller og arkitektur. Det blir enklere å utveksle erfaringer og virksomhetene modnes sammen.
I tråd med lover og regler: Referansearkitekturer kan bygge inn juridiske krav og sikre at disse kravene følges.

3.3 Når er det behov for å lage referansearkitekturer?
Det er aktuelt å ha felles referansearkitekturer når flere virksomheter har mange av de samme behovene, og behovene løses på ulike måter. Dette gjelder særlig der det er behov for samhandling, og det er barrierer for samhandling.
I offentlig sektor har det vært vanlig å løse åpenbare likartede behov ved å etablere felleskomponenter. Felleskomponentene omfatter både løsninger og registre. De viktigste kalles nasjonale felleskomponenter - f.eks. ID-porten for å håndtere autentisering og Folkeregisteret for å ha et sentralt register over personopplysninger.  Det bør vurderes om man i større grad skal bruke referansearkitekturer, slik at virksomhetene selv kan løse fellesoffentlige behov, og samtidig ivareta egne prinsipper og krav.
Referansearkitekturer vil kunne være et nyttig virkemiddel for å understøtte politiske målsetninger som gjelder hele offentlig sektor på digitaliseringsområdet, for eksempel realisering av målene i Digital agenda.

3.4 Referansearkitektur og tilhørende strategi


Figur 3: Innhold i en referansearkitektur

Referansearkitektur består av en strategi som sier hvordan en referansearkitektur skal tas i bruk, og en teknisk referansearkitektur.

En referansearkitektur-strategi beskriver typisk følgende elementer:
•	Visjon - hva er arkitektur-målbildet for referansearkitekturen, og innplassering i den nasjonale arkitekturen (ønsket situasjon)
•	Prinsipper - hvilke prinsipper ligger til grunn for det forretningsmessige behov referansearkitekturen adresserer
•	Politiske mål og føringer som ligger til grunn for referansearkitekturen
•	Nå-situasjonen og behov/problembeskrivelse
•	Strategi for å gå fra nå-situasjon til ønsket situasjon
•	Strategi for forvaltning og styring av referansearkitekturen
•	En ikke-teknisk beskrivelse av referansearkitekturen
Den tekniske delen av referansearkitekturen er malen for en løsningsarkitektur og er uttrykt i arkitekturspråk og modeller. Øverste nivå er en overordnet beskrivelse av arkitekturen (Solution Architecture Template (SAT)) og de arkitekturkomponenter (Architectural Building Blocks (ABB)) den består av. De enkelte arkitekturkomponenter er detaljert beskrevet, herunder hvilke standarder de baseres seg på.

Figur 4: European Interoperability Framework (EIF)
European Interoperability Framework (EIF)  er et rammeverk for samhandling, laget av EU-kommisjonen under ISA-programmet. I relasjon til spesielt referansearkitekturer er de fire lagene i rammeverket (juridisk, organisatorisk, semantisk og teknisk samhandlingsevne) relevant. Dette viser at samhandling handler om mer enn teknologi. EIF-rammeverket sikrer at det blir satt fokus på behov, krav og løsninger i de ulike lagene. Det pågår et arbeid, i regi av Difi og SKATE, for å få på plass et nasjonalt rammeverk for samhandling (NIF), basert på EIF.
Grunnlaget for referansearkitekturen er European Interoperability Reference Architecture (EIRA ), som er Europakommisjonens referansearkitektur for samhandling. EIRA er delt i overensstemmelse med lagene i EIF. Den er så generell at den dekker alle tenkelige samhandlingsløsninger og tjener primært som en klassifikasjons- og struktureringsmodell, da den beskriver hvilke komponenter en referansearkitektur dekker og dens plassering i referansearkitekturlandskapet.

Figur 5: European Interoperability Reference Architecture (EIRA)
EIF og EIRA er strukturerende verktøy som kan hjelpe til å beskrive en referansearkitekturs rammer og fokus. For eksempel de områder en referansearkitektur dekker og hvilke samhandlingslag som behandles.

3.5 Krav til bruk av referansearkitekturer i offentlig sektor
Det må tas stilling til hvilken status referansearkitekturer skal ha i offentlig sektor. Det foreslås at referansearkitekturene enten skal være obligatorisk (som betyr at referansearkitekturen skal benyttes innenfor et angitt område), eller anbefalt (som betyr at referansearkitekturen bør benyttes, men kan fravikes i særlige tilfeller).
En referansearkitektur kan også være delvis obligatorisk og delvis anbefalt, dermed oppnås høyere grad av fleksibilitet.
De standarder som gjelder for området bør inngå i referansekatalogen for offentlig sektor .
 
4.	Definisjon og avgrensing

4.1 Meldingsutveksling
Meldingsutveksling er en delmengde av informasjonsutveksling. Meldingsutveksling handler om å flytte informasjon mellom to aktører. Når virksomheter har et felles ansvar for utførelse av en forretningsprosess, skal det avtales og defineres en koreografi (å sette forretningsprosesser i sammenheng) mellom virksomhetene.  
Et eksempel på en slik koreografi er «tradisjonell» manuell saksbehandling. Avsender utformer et brev med dokumenter der man etterlyser informasjon fra en annen virksomhet (mottaker). Meldingsformidling er her den digitale versjonen av postverket.
En virksomhet skal kunne igangsette en forretningsprosess hos en annen virksomhet gjennom en transaksjon i form av utveksling av en hendelsesnotifikasjon som omhandler hvilken transaksjon det dreier seg om, og hvilke data som er nødvendig for å kunne videreføre forretningsprosessen. En forretningsprosess-koreografi er avtalen om sekvensen av transaksjoner mellom to eller flere virksomheter, samt innholdet i hver transaksjon (hendelsesnotifikasjon og Dokument(er)).
Dette spenner fra det enkle, hvor et dokument sendes fra en virksomhet til en annen, til de mer komplekse forretningsprosessene, hvor en transaksjon initierer en saksbehandlingsprosess hvor en rekke transaksjoner går mellom virksomhetene.
eMelding  er digital meldingsutveksling, som støtter offentlige virksomheter, private virksomheter og innbyggere med delte og sammenhengende forretningsprosesser på en fleksibel, sikker og pålitelig måte.

4.2 Mønstre for digital utveksling av informasjon
Meldingsutveksling relaterer seg til alle EIF-lagene . Begrepet meldingsformidling handler kun om arkitektur/infrastruktur på det tekniske lag, det vil si selve flyttingen av en melding fra en virksomhet til en annen.

eMelding er en av flere former (mønstre) for digital informasjonsutveksling. Figuren under viser tre forskjellige mønstre. Mønsteret eMelding er fokus for denne referansearkitekturen, mens de øvrige er eksempler på andre mønstre.

Figur 8: Mønstre for digital informasjonsutveksling

eMelding
Meldingsutveksling er karakterisert ved asynkron informasjonsforsendelse (push), og kan sammenlignes med å sende en e-post eller SMS. Mottakeren beslutter selv når og hvordan sin del av forretningsprosessen skal startes (asynkront). Dette mønsteret egner seg når en virksomhet vet hvilke andre virksomheter de skal kommunisere med, eller når virksomheten ikke har kontroll på hvordan eller når en mottaker kommer til å utføre sin del av forretningsprosessen.
eOppslag
eOppslag er synkron tilgang til informasjon enten ved henting av informasjon (pull) eller oppdatering av informasjon (push). Dette foregår i sanntid (synkront), hvor svar på en forespørsel gis innenfor få sekunder. Dette kan være å gjøre en spørring i Folkeregisteret. Mønsteret egner seg når en virksomhet har faste og forutsigbare parter, slik som ved oppslag mot et register, der virksomhetene på forhånd kan avtale hvordan og hvor fort mottakers forretningsprosess skal utføres.

eNotifikasjon
eNotifikasjon er en asynkron publisering av informasjon. Mottakere abonnerer på spesifikke meldinger, typisk notifikasjoner på en hendelse. Mønsteret egner seg for situasjoner der virksomhet ønsker å starte en forretningsprosess basert på at en tilstand er endret. Et eksempel er at virksomheten kan abonnerer på Folkeregisteret slik at de kan få beskjed når en innbygger flytter inn og ut av ansvarsområdet til virksomheten, eller abonnere på informasjon om åpnede konkurser i Konkursregisteret.
Bruk av mønstre
Formålet med å ha slike standardiserte mønstre er at virksomhetene i større grad kan gjenbruke komponenter når sammenlignbare forretningsprosesser skal digitaliseres. Hvilke av mønstrene som egner seg best er avhengig av hver enkelt forretningsprosess, aktørene som er involvert og om disse er kjente eller ikke.


 
5.	Beskrivelse av overordnede behov

Hver dag utveksles millioner av meldinger mellom virksomheter, og antall meldinger som utveksles øker. Digitalisering medfører at virksomhetene overfører flere små meldinger som er strukturerte, fremfor større meldinger med ustrukturert innhold (det tradisjonelle brevet). En sideeffekt av at virksomhetene digitaliserer og spesialiserer meldinger er at det stadig utvikles nye løsninger for eMelding. Det er behov for at eMeldingsutveksling skjer på en effektiv, pålitelig og sikker måte, og at det er enkelt å koble til nye parter. For å skape det digitale indre markedet i Europa er eMelding en av de viktigste byggeklossene. For å øke samhandlingsevnen bør nasjonale myndigheter arbeide for at meldingsutveksling skjer på en felles og gjenbrukbar måte, fremfor å lage nye og særegne løsninger for eMelding.
Behov for redusert kompleksitet
Det kan stilles spørsmål ved behov for referansearkitektur for eMelding i de tilfeller hvor kun to parter utveksler meldinger bilateralt. Utfordringen kommer når en virksomhet har en rekke bilaterale forhold, med ulik måte å utveksle meldinger på. Summen av bilaterale eMeldingsløsninger innebærer kompleksitet, som referansearkitekturen kan medvirke til å forenkle.
Virksomhetene må forholde seg til mange grensesnitt og ulike løsninger for eMelding. Det er et behov for å konsolidere infrastrukturene for å redusere antall grensesnitt og etablere en løsere kobling mellom virksomhetenes forretningsprosesser og selve meldingsutvekslingen.
Behov for økt samhandlingsevne
Med digitalisering vil det være behov for å samhandle med andre aktører og på nye måter enn i dag. En referansearkitektur og en nasjonal infrastruktur gjør dette enklere.
Behov for bedre etterlevelse av regelverk
Løsning for eMelding må gjøre det enklere for offentlige virksomheter å etterleve relevant regelverk i Norge og EU. Difi-rapport 2015:3 peker på behov for bedre etterlevelse at journalføringsplikten i arkivloven. eIDAS-forordningen   som vil bli implementert i norsk lovgivning stiller krav, blant annet for bruk av tillitstjenester, herunder CEF eDelivery. Ny personvernforordning vil også stille krav som kan ha betydning for hvordan eMelding skjer. Referansearkitekturen må sikre at gjeldende regelverkskrav følges og må tilpasses nye regler. Ved bruk av referansearkitekturen vil hensynet til regelverk kunne bli ivaretatt.
Behov for grenseoverskridende tjenester
Norske virksomheter har behov for eMelding på tvers av landegrenser. Skal norske virksomheter enkelt kunne samhandle med Europa bør den nasjonale infrastrukturen baseres på CEF eDelivery, som er den byggekloss som forventes brukt i Europa for eMelding.
Brønnøysundregisteret har koblet seg til Business Registers Interconnection System (BRIS), som er en del av CEF Digital for tilkobling av Enhetsregistre for deling av selskapsinformasjon mellom land i EU.
NAV har gjennom Electronic Exchange of Social Security Information(EESSI) igangsatt prosjekt for utveksling av trygdeinformasjon på tvers av landegrenser. Begge disse fellesløsningene fra EU og CEF Digital baserer seg på bruk av CEF eDelivery for meldingsutveksling.
Difi har gjennom ledelse og deltakelse i PEPPOL prosjektet og OpenPEPPOL-organisasjonen etablert og implementert en felles europeisk transportinfrastruktur for e-handel og e-faktura, som i dag er del av CEF eDelivery.  
Behov for nødvendig informasjonssikkerhet
Sikring av informasjon er sentralt i eMelding. Dette omfatter integritet, konfidensialitet og uavviselighet. Informasjon må kunne sendes uendret mellom avsender og mottaker, og uvedkommende må ikke få tilgang til informasjonen. Avsender må være trygg på at mottaker er riktig mottaker og mottaker må være trygg på at avsender er den vedkommende utgir seg for å være. Informasjonen om eMelding må være sporbar i ettertid (uavviselig). Referansearkitekturen ivaretar disse behovene.
Behov for informasjon om mottaker
Avsender har behov for å vite om mottaker er i stand til å motta og behandle den spesifikke meldingstypen som ønskes sendt, samt de tekniske detaljene for utvekslingen. Videre har avsender behov for informasjon om mottakers elektroniske adresse. Med denne informasjonen på plass kan informasjon sendes fra avsenders maskin til mottakerens maskin, hvor den kan prosesseres videre. Tilgang til oppdatert informasjon om mottakernes tekniske kapabiliteter og deres elektroniske adresser ivaretas i Capability Lookup-funksjonen i referansearkitekturen.
Behov for forenkling av prismodeller/forretningsmodeller
I dagens situasjon med ulike leverandører og løsninger for eMeldingsformidling eksisterer det ulike forretningsmodeller. Det er behov for å forenkle og redusere kostnadene knyttet til eMelding. Som del av konsolidering av infrastrukturen må pris- og forretningsmodell forenkles, og dermed gi større grad av forutsigbarhet for brukerne.


 
6.	Beskrivelse av dagens situasjon

Beskrivelse av dagens situasjon er tilsvarende beskrivelsen i rapport 2015:3 løsning for meldingsutveksling i offentlig sektor. Offentlige virksomheter må i dag forholde seg til flere infrastrukturer for eMeldingsformidling. Disse forvaltes av ulike virksomheter, det er brukt ulike integrasjonsstandarder, de er ikke interoperatible (de snakker ikke sammen) og det er liten grad av samordning mellom forvalterne av infrastrukturene. Siden flere parallelle infrastrukturer benyttes, må både offentlige virksomheter og deres leverandører forholde seg til flere ulike grensesnitt. Dette leder til økt kompleksitet, høyere kostnader enn nødvendig og tregere utbredelse av den enkelte infrastruktur for eMelding. Resultatet er dårlig samordning av offentlig sektor.
Et sentralt element for å kunne konsolidere og forenkle infrastrukturene for eMelding er at det etableres en løsning for Capability Lookup. En av anbefalingene i rapport 2015:3 var å etablere et integrasjonspunkt (frittstående integrasjonsmodul), for å konsolidere integrasjonen mot de ulike infrastrukturene for meldingsutveksling. Integrasjonspunktet er tenkt som den del av en transisjonsarkitektur (virkemiddel for å lette overgang fra dagens situasjon til ønsket situasjon) mot en mer konsolidert eMeldingsformidlings-infrastruktur for eMelding.

Figur 9: Nå-situasjon
Det eksisterer i dag flere prosjekter og aktiviteter som jobber med dette problemområdet. KS har gjennom sitt initiativ FIKS flere komponenter som håndterer deler av dette (for eksempel SvarUT). Innenfor helsesektoren er Norsk Helsenett og Direktoratet for e-Helse sentrale aktører som jobber med disse problemstillingene.
Difi har parallelt med referansearkitektur og strategi jobbet med mer operative oppgaver hvor eMelding står sentralt. En hovedsatsing er digitalisering av offentlige anskaffelser gjennom bruk av elektronisk handelsformat (EHF) og PEPPOL eDelivery-nettverket. En annen hovedsatsing er oppfølging av anbefalingene fra 2015:3 rapporten om løsning for eMelding i offentlig sektor gjennom integrasjonspunkt. Dette er således sentrale satsinger for Difi i forhold til operasjonalisering og gjennomføring av målene satt i denne strategien.
 
7.	Løsningsarkitekturer som i dag støtter referansearkitekturen

7.1 Business Registers Interconnection System (BRIS)
Ansvarlig: Brønnøysundregistrene
https://ec.europa.eu/cefdigital/wiki/pages/viewpage.action?pageId=46992657
BRIS er en løsning for sammenkobling av europeiske foretaksregistre. BRIS er regulert i direktiv 2012/17/EU og i gjennomføringsrettsakten EU 2015/884 av 8. juni 2015. Løsningen ble satt i drift 8. juni 2017.
Alle foretaksregistre i EØS-området er koblet opp mot European Central Platform (ECP). All kommunikasjon går mellom det enkelte foretaksregister og ECP. I kommunikasjonen mot ECP benyttes CEF eDelivery og CEF eSignature komponentene.
Gjennom BRIS gis publikum tilgang via e-Justice portalen (https://e-justice.europa.eu/content_find_a_company-489-en.do) for å søke opp og aksessere/bestille oppdatert foretaksinformasjon og dokumenter.  I tillegg håndterer ECP notifikasjoner mellom foretaksregistre knyttet til statusendringer, dette er spesielt relevant for informasjon om foretak hjemmehørende i ett land men som har filial i annet land eller ved gjennomføring av grenseoverskridende fusjoner av foretak.

7.2 Electronic Exchange of Social Security Information (EESSI)
Ansvarlig: Arbeids-og velferdsetaten (NAV)
http://ec.europa.eu/social/main.jsp?catId=869
EESSI er en løsning for eMeldinger innenfor trygdeområdet. Løsningen er basert på CEF eDelivery og har 60 aksesspunkter, som omfatter alle EU og EØS land. EESSI omfatter 320 ulike meldingsflyter kalt SED (Strukturerte elektroniske dokumenter), og mer enn 10 000 institusjoner og hundretusener av saksbehandlere er involvert.
Forskrift om inkorporasjon av trygdeforordningene i EØS-avtalen (Inkorporeringsforskriften (F22.06.2012 nr. 585) viser hvilke lover som berøres av EUs trygderegler, og dermed også av EESSI. Norge har avgitt en erklæring der det framgår hvilke norske lover som er omfattet og ut fra det også hvilke institusjoner.
Til pan-europeisk eMelding, er det besluttet at Norge skal ha et aksesspunkt, som forvaltes av NAV.

7.3 Elektroniske handelsformat (EHF) og PEPPOL eDelivery-nettverket
Ansvarlig: Difi
Utveksling av elektroniske handelsdokumenter, inkludert faktura, i offentlige anskaffelsesprosesser i Norge gjennomføres i dag i en løsningsarkitektur som er i overenstemmelse med den foreslåtte referansearkitekturen for meldingsutveksling. Dette skjer gjennom bruk av elektronisk handelsformat (EHF)  og PEPPOL eDelivery-nettverket . EHF er en strukturert beskrivelse av en forretningsprosess, informasjonsflyten i prosessen og reglene som gjelder for gjennomføringen av prosessen, knyttet til et teknisk format for meldingsutveksling. EHF er en norsk tilpasning av PEPPOL BIS, som igjen er basert på resultater fra europeisk standardiseringsarbeid gjennom CEN og bruk av Universal Business Language (ISO/IEC 19845 UBL).   
Forsendelse av EHF og PEPPOL BIS-dokumenter skjer gjennom PEPPOL eDelivery-nettverket, som er en løsningsinstans av den foreslåtte referansearkitekturen for meldingsutveksling.
Forvaltning og styring av PEPPOL BIS og PEPPOL eDelivery-nettverket skjer sentralt i regi av OpenPEPPOL AISBL  og på nasjonalt nivå gjennom en rekke PEPPOL-myndigheter.  
Per november 2017 er det 160 aksesspunkt i PEPPOL eDelivery-nettverket i 19 europeiske land, Canada og USA. Omkring 60 av aksesspunktene leverer tjenester i Norge, de har registrert mer enn 90.000 mottakere i ELMA (en instans av Capability Lookup funksjonen i referansearkitekturen), og det gjennomføres mer enn fem millioner transaksjoner i den norske delen av nettverket hver måned. Majoriteten av disse transaksjonene er i dag fakturaer, men antallet produktkatalog og ordretransaksjoner er økende.
Økt utbredelse og bruk av EHF og PEPPOL eDelivery-nettverket er sentrale virkemidler i arbeidet med å digitalisere offentlige anskaffelsesprosesser i Norge.

7.4 Betalingsinitiering
Ansvarlig: Difi
https://vefa.difi.no/iso20022/
I 2015 ble det klart for Difi at innføring av nye standarder for betalingsformidling (basert på ISO 20022 ) i Norge ville gi behov for vesentlige endringer i måten offentlige og private virksomheter kommuniserer med sine bankforbindelser på. Difi var på det tidspunktet allerede i gang med å forsterke PEPPOL eDelivery-nettverket for å understøtte elektronisk tilbudsinnlevering i offentlige anskaffelser. Difi gikk derfor i dialog med DFØ, som er ansvarlig for inngåelse av rammeavtaler med bankene om konsernkonto for statlige virksomheter, for å se om det kunne stilles krav til bruk av det forsterkede PEPPOL-nettverket i disse avtalene som i dag er inngått med DNB, Nordea og Sparebank1. DFØ så verdien av å kunne stille et slikt krav både for å få et standardisert grensesnitt som vil gjøre det enklere for statlige virksomheter å bytte bankforbindelse, men også for å styrke sikkerheten i denne meldingsformidlingen. DFØ meldte derfor våren 2016 at det i neste generasjon rammeavtaler for konsernkontobanker som trår i kraft fra 2019 vil bli stilt krav om å benytte det forsterkede PEPPOL-nettverket og sikringen som Difi tilrettelegger for.
Fra høsten 2016 har Difi i samarbeid med finansnæringens infrastrukturselskap Bits, og en rekke banker, økonomisystem¬leverandører og offentlige virksomheter gjennomført et prosjekt for å ta i bruk løsninger for formidling av betalingsmeldinger mellom virksomheter og deres bankforbindelse basert på et forsterket PEPPOL eDelivery-nettverk. Som del av dette arbeidet er det etablert en oppbevarings- og distribusjonstjeneste for offentlige sertifikater til bruk i kryptert meldingsutveksling mellom partene i betalingsmeldingsprosjektet kalt BCP (Business Certificate Publisher).
Resultatet av dette prosjektet er blant annet at NAV og DNB i desember 2017 igangsetter produksjon av løsninger for formidling av betalingsmeldinger basert på et forsterket PEPPOL eDelivery-nettverk.
Utvekslingen av ISO 20022-baserte betalingsmeldinger mellom virksomheter og deres bankforbindelse vil gjennom bruk av forsterket PEPPOL eDelivery-nettverk gjennomføres i en løsningsarkitektur som er i overenstemmelse med den foreslåtte referansearkitekturen for eMelding.

7.5 eFormidling (Meldingsutveksling mellom offentlige virksomheter)
Ansvarlig: Difi
https://samarbeid.difi.no/eformidling
eFormidling har fokus på formidling av informasjon brukt i saksbehandling i offentlig sektor. Med gjeldende lovverk så gjør dette NOARK-standardiserte sak/arkiv løsninger sentrale. eFormidling gir disse løsningene en standardisert måte å kommunisere med andre virksomheter og innbyggere, uten at saksbehandler eller saksbehandlingsløsning må ha detaljert kunnskap om hvordan mottaker tar imot og behandler informasjonen i forkant Dette er samme område som 2013:13 og 2015:3 rapportene fokuserte på og eFormidling er en direkte oppfølging av disse.
Sentralt i eFormidling er konseptet «integrasjonspunktet». Dette er en programvare som realiserer et standard programmeringsgrensesnitt for lokal sending og mottak av meldinger. Integrasjonspunktet håndterer pakking, sikring og oversetting av format avhengig av hvordan mottaker har valgt å motta sine meldinger digitalt.
Gjennom satsingen «integrasjonspunkt for eFormidling» bygger Difi opp en sentralforvalterfunksjon for å sikre høy kvalitet, sikkerhet og stabilitet for statlige virksomheter som tar i bruk eFormidling, samtidig som at vi legger til rette for at vi enklere kan utnytte egenskapene som foreslått referansearkitektur gir oss, slik som stimulering til konkurranse i markedet rundt transporttjenester.
eFormidling ligger også til grunn for prosjektet eInnsyn - arbeidet med å fornye Offentlig Elektronisk Postjournal.


 
8.	Beskrivelse av referansearkitektur for digital meldingsutveksling

Referansearkitektur for eMelding finnes på:
https://difidrift.sharepoint.com/sites/Arkitekturbibliotek/Referansearkitekturer/Hjemmeside.aspx
8.1 Prinsipper
Referansearkitektur for eMelding følger de overordnede IT-arkitekturprinsippene for offentlig sektor , og bygger på prinsippene i SOA, det vil si at den er komponentbasert med fokus på gjenbruk og åpenhet. Videre overholdes relevante EIF-prinsipper, se vedlegg A.

8.2 Krav til eMelding
Basert på behov er det satt generiske krav til eMeldingsløsninger .
eIDAS-forordningen  stiller ytterligere krav, hvor det handler om informasjonsutveksling av særlig sensitiv karakter. Disse krav er oppstilt separat i referansearkitekturen .

8.3 Arkitektur for eMelding
Referansearkitektur for meldingsutveksling dekker alle EIF-lag, det vil si en referansearkitektur for en komplett meldingsutvekslingsløsning. Da dette er en generisk referansearkitektur, som er uavhengig av domener, er det forskjell i detaljeringsnivået i de forskjellige EIF-lagene. Nedenfor beskrives de viktigste krav, anbefalinger og spesifikasjoner:
Juridisk
	eIDAS-forordningen er satt inn som et mulig krav til løsningsarkitektur.
Det refereres til veiledning til IT arkitekturprinsippene hvor det henvises til relevant regelverk  
Organisatorisk
	Det pekes på OpenPEPPOLs Transport Infrastructure Agreement  som utgangspunkt for en samhandlingskontrakt.
Til definisjon av forretningsprosesser og koreografier anbefales BPMN
Semantisk
	Generelle eDokumenter er i referansearkitekturen en “konvolutt” med adresse, metadata (hendelsesnotifikasjon) og innhold (informasjon)
Teknisk	En detaljert arkitektur for en fleksibel, skalerbar og sikker meldingsformidling (kap. 8.4)

8.4 Arkitektur for meldingsformidling
Arkitekturen for eMelding er bygget opp som en 4-hjørners modell, med avsenders IT-system (C1), avsenders forsendelseskomponent (aksesspunkt - C2), mottakers mottakelseskomponent (aksesspunkt - C3) og mottakers IT-system (C4).


Dette gir stor fleksibilitet i arkitekturen, med inkludering av eksisterende eMeldingsformidlingstilbydere. En virksomhet med behov for eMelding kan koble seg til infrastrukturen på ulike måter:
•	Integrere Aksesspunkt inn i IT-systemer.
•	Bruke Aksesspunkt som en tjeneste, som er implementert som en IT-komponent og gjenbrukbar for andre IT-systemer i virksomheten.
•	Bruke Aksesspunkt som en tjeneste i skyen, hvor en ekstern leverandør (for eksempel Altinn eller FIKS) tilbyr denne tjenesten.
I referansearkitekturen fokuseres det på kommunikasjonen mellom C2 og C3, da integrasjon/interaksjon mellom C1 og C2 (samt C3 og C4) avhenger av ovennevnte muligheter for tilkobling med den sentrale del av infrastrukturen.
For at ivareta skalerbarhet og fleksibilitet er det i referansearkitekturen innebygget en Capability Lookup komponent. Denne sikrer:
•	Dynamisk adressering og routing av meldinger.
•	Endringer i komponenter f.eks. nye versjoner kan introduseres stegvis.
•	Infrastrukturen kan inneholde deltakere med forskjellige modenhetsnivåer, da samhandlingskapabiliteter hos mottaker er synlig for avsender, som kan innrette seg etter dette.
Infrastrukturen kan holdes sammen av et felles sertifikat (PKI) eller individuelle sertifikater. Infrastrukturen kan dermed også deles i forskjellige virtuelle infrastrukturer f.eks. en virtuell infrastruktur kun for offentlige virksomheter, en hvor private virksomheter deltar eller oppdelt etter sektor.
Sikkerhet i form av konfidensialitet (ingen andre kan lese meldingen), integritet (meldingen er ikke endret underveis), autensitet (hvem er det fra) og uavviselighet (begge parter er enige om at meddelelsen er sendt og mottatt) etableres ved hjelp av egne sertifikater og kryptering etter avtale mellom sender (C1) og mottaker (C4).  
Følgende komponenter inngår i referansearkitekturen for meldingsformidling:
Komponent	Engelsk	Standarder	Løsninger
eDokument	eDocument	SBDH
ASIC	PEPPOL BIS/EHF
Capability Lookup	Capability Lookup	OASIS SMP	ELMA (norsk SMP)

Aksesspunkt	Access Point	Se under	BRIS
EESSI
PEPPOL eDelivery
Forsterket PEPPOL eDelivery
Integrasjonspunkt
Transportprotokoll	Message exchange	AS2
AS2+ (Forsterket)
OASIS AS4	BRIS
EESSI
PEPPOL eDelivery
Forsterket PEPPOL eDelivery
Integrasjonspunkt
Domene Sertifikat	Domain PKI		PEPPOL eDelivery
Forsterket PEPPOL eDelivery
Integrasjonspunkt
eIDAS utvidelse	eIDAS extension	(under arbeid)	Forsterket PEPPOL eDelivery
Integrasjonspunkt



 
9.	Særlige problemstillinger

Her reises problemstillinger som adresseres i tiltakslisten i kapittel 12.
9.1 Overføringsprotokoller
Overføringsprotokoller mellom C2 og C3 har hatt særlig fokus i EUs arbeid med eDelivery. Protokollene skal sørge for at eMeldingen blir korrekt overført, ikke bare med tanke på teknologi (formidling), men også at behovene i de juridiske, organisatoriske og semantiske lagene er ivaretatt.
Referansearkitekturen foreslår både AS2 og AS4 som overføringsprotokoll. EESSI og BRIS anvender AS4, mens PEPPOL eDelivery benytter AS2. CEF eDelivery er i gang med en utfasing av AS2 og innfasing av AS4. Med PEPPOL eDelivery nettverkets utbredelse og størrelse er dette en prosess som vil skje over lengre tid. AS4 innfases langsomt og PEPPOL eDelivery-nettverket baseres i dette tidsrommet på multiprotokoll aksesspunkt.
Det er viktig at Capability Lookup tjenesten støtter at infrastrukturene har flere protokoller. Dette medfører at flere protokoller kan sameksistere uten at det har betydning for samhandlingsevnen. Det å kunne innføre nye protokoller gjør også at arkitekturen blir mer robust og fleksibel, og at den kan håndtere nye fremtidige protokoller og dermed danne grunnlag for at virksomhetene kan tenke nytt og innovativt også relatert til disse.
Overføringsprotokoller skal samtidig ses i sammenheng med et fremtidig arbeide med referansearkitektur for andre informasjonsutvekslingsmønstre, hvor overføringsprotokoller kan gjenbrukes og hvor for eksempel API-orientert arkitektur og Restfull  må tas i betraktning.   

9.2 Forvaltning og ansvar for Capability Lookup
Behovet er i utgangspunktet relativt enkelt - en tjeneste som har oversikt over hvilken informasjon en digital mottaker er i stand til å motta, på hvilken måte dette skal skje og hvilken elektronisk adresse mottakeren har. Men i praksis er dette relativt komplekst. I dagens sitasjon er dette behovet realisert på forskjellige måter i de forskjellige meldingsinfrastrukturene. Hvor og hvordan de forskjellige aspektene av behovet er dekket varierer stort.
En annen dimensjon er selve identifikasjonen av en digital mottaker. Denne er ofte basert på organisasjonsnummer som identifikator for mottakeren. Utfordringen er at geografisk organisering av mottaker får konsekvenser for hvordan de er organisert i Enhetsregisteret. Konsekvensen er at en organisasjon som NAV (som er tilstedes i alle kommuner) har over 600 organisasjonsnummer (hoved og underenheter). Utfordringen blir da å finne ut hvilke av disse virksomheten skal kommunisere digitalt med.
Konsekvensen er at en i dag har mange sektor/domene-løsninger for Capability Lookup. Løsningen ELMA/SMP forvaltes og driftes av Difi og utgjør Capability Lookup for domenene offentlige anskaffelser og betalingsinitiering. Direktoratet for eHelse har tilsvarende en løsning for helsesektoren.
SKATE-tiltaket KoFuVi (Kontakt og fullmaktsinformasjon til virksomheter) som Brønnøysundregistrene har ansvar for gjennomføring av, inneholder den nyeste analysen av dette problemområdet. I foranalysen og forprosjektet til KoFuVi ble det pekt på behovet for en kombinasjonsarkitektur der det på sikt blir en klarere samhandling og ansvarsdeling mellom sentrale register (Enhetsregisteret) og sektor/domene register når det gjelder kapabilitets- og adresseproblemstillinger.
CEF eDelivery legger opp til en SML/SMP arkitektur tilsvarende den som er i bruk i dagens PEPPOL eDelivery-nettverk for å realisere Capability Lookup.
Capability Lookup skal samtidig ses i sammenheng med et fremtidig arbeide med referansearkitektur for andre informasjonsutvekslingsmønstre, som kan resultere i ytterligere krav til en slik tjeneste.
På grunn av potensielle nye krav anbefales det å gjennomføre en fornyet analyse og behovsspesifisering i forhold til plassering og funksjonalitet av en fremtidig Capability Lookup-komponent.     

9.3 Forvaltning og utvikling av programvare
Utvikling av programvare for å realisere komponentene i infrastrukturene er komplisert og krever spisskompetanse. Det er derimot ikke samme kompetansebehov for å ta i bruk og drifte programvaren. I EU sitt arbeid med CEF eDelivery støtter en seg i stor grad på en hybrid-sourcing der EU tilbyr referanseimplementasjoner som åpen kildekode, samtidig som at EU legger til rette for bruk av standardisert hyllevare utviklet og forvaltet av de store programvareleverandørene .
I Norge har Difi hatt stor suksess med referanseimplementasjoner distribuert som åpen kildekode. I PEPPOL eDelivery-nettverket er aksesspunkt-programvaren (C2 og C3) «Oxalis», utviklet og forvaltet av Difi, dominerende blant de aktørene i markedet som tilbyr aksesspunkttjenester.
Tilsvarende tilbyr Difi i dag programvaren «integrasjonspunktet» som standardiserer løsningsarkitekturen for integrasjon hos avsender (C1) og mottaker (C4) mot dagens eMeldingsinfrastrukturer.
Målet med å ta nasjonalt ansvar for forvaltning og utvikling av disse komponentene er å stimulere til konkurranse mellom de forskjellige aktørene som drifter infrastrukturkomponenter, samtidig som at vi oppnår høyere kvalitet ved at problemstillinger relatert til samhandling blir redusert. Det er også ønskelig å stimulere til at leverandører av fagsystemer til virksomhetene fokuserer på nye og innovative løsninger på offentlig sektor sine forretningsprosesser, fremfor bindinger av integrasjon mot enkelte eMeldingsinfrastrukturer.
Utfordringen som vi ser er at det er vanskelig å bygge et miljø som er i stand til å forvalte den åpne kildekoden til disse komponentene på en forsvarlig måte. Det er i dag en forutsetning at offentlig sektor bidrar med sine ressurser, utviklingsmiljø og kompetanse. Det bør utredes om vi trenger en felles offentlig strategi for hvordan vi håndterer dette på best mulig måte.

9.4 Ansvar for vedlikehold og videreutvikling av referansearkitekturen
Det foreslås at Difi som samordningsorgan på direktoratsnivå tar ansvar for vedlikehold og videreutvikling av referansearkitekturen. Her ligger det også et ansvar for å sørge for at referansearkitekturen gjøres kjent og benyttes.

9.5 Forvaltning av infrastrukturen
Uavhengig av om det velges å etablere en eller flere domenespesifikke infrastrukturer i Norge, er det behov for forvaltning hvor minimum følgende ansvar inngår:
•	Godkjenning av virksomheter som kobler seg til infrastrukturen, etablere samhandlingskontrakter og ansvar for utlevering av sertifikater
•	Overvåke at SLA overholdes
•	Klageinstans, hvis samhandlingskontrakt misligholdes
•	Bindeledd mellom infrastruktur-deltakere og forvaltere av tekniske komponenter, sentrale komponenter og eventuell internasjonale forvaltningsorganer
•	Planlegge og utføre endringer i infrastrukturen
I PEPPOL eDelivery-nettverket ivaretas forvaltningen av OpenPEPPOL AISBL sentralt, og av PEPPOL-myndigheter nasjonalt. Difi er norsk PEPPOL-myndighet. Forvaltningen er i prinsippet todelt; hvor én er fokusert på forretningsprosessene som skal understøttes (per i dag: offentlige anskaffelser fram til avtaleinngåelse (pre-award) og etter avtaleinngåelse, inkludert fakturering (post-award) og ISO 20022-basert betalingsinitiering). Den andre delen av forvaltningen er fokusert på eDelivery-nettverket som generisk eMeldingsinfrastruktur, som skal ivareta de ulike kravene til sikringsmekanismer, tjenestetilgjengelighet, PKI mv. som følger av behov i de ulike forretningsprosessene. Som norsk PEPPOL-myndighet har Difi anledning til å åpne for bruk av PEPPOL eDelivery-nettverket for å støtte nasjonalt definerte forretningsprosesser/domener. Dette betyr i praksis at store deler av forvaltningsregimet som er etablert med Difi som PEPPOL-myndighet kan gjenbrukes også innenfor andre domener. Innenfor ISO 20022-basert betalingsinitiering ivaretar Difi eksempelvis eDelivery-forvaltningen, mens Bits har forvaltningsansvaret for de ISO 20022-baserte meldingstypene.
Det må tas stilling til om forvaltningsmodellen etablert for PEPPOL eDelivery-nettverket med Difi som PEPPOL-myndighet skal gjenbrukes i andre domener/forretningsprosesser, eller om andre alternativer skal utredes.
9.6 En eller flere domenespesifikke infrastrukturer
Det må tas stilling til om det skal være en infrastruktur, hvor alle typer av meldinger kan sendes eller om den skal deles i ulike virtuelle infrastrukturer.

9.7 Finansiering av formidlingstjenester
Det er ennå ikke bestemt hvordan eksisterende eMeldingsformidlingstjenester som er utviklet av offentlig sektor, herunder Altinn og KS FIKS skal fungere i infrastrukturen og dermed hvordan tilknytningen skal være. Teknisk vil både Altinn og FIKS kunne integreres med eMeldingsformidlingsinfrastrukturen gjennom aksesspunkter.
Dette har også betydning for hvordan betaling for bruk skal skje. Det er i prinsippet to modeller:
1)	Avsender betaler for hver løsning som meldingen går gjennom. For eksempel når meldingen går gjennom både KS FIKS og Altinn, eller
2)	Avsender betaler kun for sin tilkobling til infrastrukturen, direkte eller gjennom en tjenesteleverandør. Det samme gjør mottaker, som velger sin tjenesteleverandør uavhengig av avsender. Et eksempel er betalingsmodellen i PEPPOL eDelivery-nettverket, hvor det ikke er tillatt for et aksesspunkt å ta betalt for håndtering av ut- eller inngående trafikk med andre aksesspunkt.


10.	Overordnet om kostnader og nytte ved referansearkitektur for meldingsutveksling

I Difi-rapport 2013:13, ble fire ulike konsepter for meldingsutveksling skissert og vurdert. Konklusjonen var at aksesspunkt-konseptet, som CEF eDelivery er en del av, fremstår som det mest attraktive konseptet. Utdrag fra oppsummeringen fra rapporten:
«Aksesspunkt-konseptet er det konseptet som gjennom vurderingene fremstår som det mest attraktive konseptet. Utover å skille konseptet markant fra basisalternativet, viser også poengene at konseptet på alle kriteriene er like bra eller bedre enn de øvrige konseptene.»
Det ble i den påfølgende Difi-rapport 2015:3 utarbeidet en samfunnsøkonomisk analyse som konkluderte med at den kvantifiserbare nytten er betydelig større ved å gjenbruke en eksisterende transportinfrastruktur, fremfor å utvikle en ny. CEF eDelivery er den etablerte infrastrukturen (PEPPOL eDelivery) som er best egnet for eMelding hensyntatt potensialet for europeisk samhandling, som også kan anvendes mellom private virksomheter.
Tabellen under gir en oversikt over fordelene med referansearkitekturen, men de viktigste sidene kan oppsummeres som følger:
1.	Referansearkitektur gir i seg selv ingen direkte økonomisk gevinst. Det er først når referansearkitekturen benyttes at det oppstår eventuelle gevinster. Referansearkitekturen gir mulighet for raskere utvikling av løsninger, hvor arkitektur- og løsningsvalg allerede er tatt.
2.	Når løsningene baserer seg på referansearkitekturen, vil det være enklere å vedlikeholde og drifte løsningen, da den er godt dokumentert. I tillegg vil det være flere arkitekter og utviklere som kjenner hvordan løsningen er satt opp, og dermed vil kostnadene til vedlikehold og drift kunne reduseres.
3.	Det vil påløpe kostnader til å forvalte referansearkitekturen, men det er billigere å forvalte en referansearkitekturer enn et ukjent antall ulike infrastrukturer.
Fordeler	Redusert Risiko	Reduserte kostnader	Forbedret kvalitet	Beskrivelse
Capability Lookup som fellestjenester		 	 	Konsolidering av Capability Lookup-registrere vil lette digital adressering og bidra til økt kvalitet og lavere kostnader ved gjenbruk.
Standarder for referansearkitektur-komponenter 		 	 	CEF eDelivery baserer seg på internasjonale standarder og krav til sikkerhet. Dette gir økt samhandling nasjonalt og internasjonalt og reduserer kostnader ved å gjøre ting på en ensartet måte.
Økt samhandling på tvers av landegrenser	 	 	 	CEF eDelivery er den eMeldingsinfrastruktur som er fullt ut tilrettelagt for digital kommunikasjon på tvers av EU. Det er lagt ned betydelige midler og ressurser i å utvikle CEF eDelivery. Infrastrukturen er i dag operativ i Norge for efaktura.
Redusert kompleksitet 			 	En felles referansearkitektur reduserer kompleksiteten ved å ha en standard måte å utveksle meldinger på.
Forvaltning		 		En velfungerende forvaltning sikrer langsiktig stabilitet og utvikling, som igjen reduserer kompleksitet og kostnader.  Fokus blir flyttet fra teknisk samhandlingskompleksitet mot informasjon- og forretningsprosesser.  
Samarbeid med kommersielle aktører, bruk av tredjepart leverandører			 	Felles spesifikasjoner for eMelding vil gi leverandør-markedet insentiver til å tilby løsninger som støtter arkitekturen til konkurransedyktige priser. Utfordringen ligger i å forenkle prisbilde.
Juridisk	 			eIDAS lovgivning støtter og inkluderer krav til eDelivery. I dag er det opp til de ulike eMeldingsutvekslingsprosjekter å avklare juridiske forhold.
Fleksibilitet		 		Fleksibiliteten i referansearkitekturen sikrer at eksisterende løsninger, f.eks. Altinn og KS Fiks blir en del av eMeldingsformidlingsinfrastrukturen.  
Oppkobling til infrastruktur	 		 	Økt kvalitet og redusert risiko ved at virksomheter kobler seg til en infrastruktur som allerede er testet og operasjonell.
Økt kunnskap om meldingsutveksling	 		 	Det er enklere å få god kunnskap om en infrastruktur basert på referansearkitekturen fremfor mange ulike infrastrukturer.




 
11.	Strategiske grep for å oppnå endring

Formålet med referansearkitekturen er å få en felles måte å utveksle meldinger på, og på sikt konsolidere eksisterende infrastruktur for eMelding, slik at meldinger kan utveksles enklere på tvers av offentlig forvaltning, med innbyggere og næringsliv og på tvers av Europa. De strategiske grepene nedenfor vil medvirke å nå denne målsetningen.

11.1	Felles regelverk og styring
Referansearkitekturer generelt har ingen verdi om de ikke blir tatt i bruk. Derfor må de være tilgjengelig for de som skal benytte dem. Referansearkitekturene vil derfor inngå som en del av arkitekturbiblioteket for nasjonal arkitektur og synliggjøres i det felles arkitekturlandskapet. Bruk av nasjonal arkitektur, med tilhørende ressurser bør omtales i digitaliseringsrundskrivet på generelt grunnlag.
For referansearkitekturer som skal være obligatoriske å bruke, bør de vurderes omtalt i digitaliseringsrundskrivet og legges inn i referansekatalogen for IT-standarder i offentlig sektor. Det må klart fremgå av den nasjonale arkitekturen hvilken status referansearkitekturen har.
Referansearkitekturen for eMelding bør i første omgang være anbefalt for offentlig sektor. Vurderingen av om den skal være obligatorisk å bruke for offentlig sektor bør tas på et senere tidspunkt, etter at ytterligere erfaring med bruk av referansearkitekturen er innhentet.

11.2	Grenseoverskridende tjenester/infrastruktur
Ved utvikling av løsninger som innebærer eMelding på tvers av landegrensene innenfor EØS-området, bør CEF eDelivery benyttes. Referansearkitekturen for eMelding baserer seg på CEF eDelivery og dette gir muligheten til å etablere en felles infrastruktur for eMelding som harmonerer både nasjonalt og internasjonalt.

11.3	Beste praksis fra Europa
Utvikling av nye løsninger blir enklere ved gjenbruk av beste praksis fra Europa. PEPPOL eDelivery er allerede en etablert eMeldingsformidlingsløsning i Norge. NAV og Brønnøysundregistrene har gjennom EESSI og BRIS erfaring med implementering av CEF eDelivery. Denne erfaring kan brukes ved etablering av nye løsninger for eMelding.

11.4	Integrasjonspunkt
En felles arkitektur for eMelding gir oss en mulighet for en gradvis overgang fra en arkitektur som i dag fremstår som ad-hoc og lite sammenhengende, mot en mer felles eMeldingsformidlingsinfrastruktur.
Difi-rapport 2015:3 inneholder en analyse av hvordan dagens situasjon er innenfor saksbehandlingsdomenet (med lav automatiseringsgrad og regulert av forvaltningsloven). Analysen peker på at det i dag er en tett kobling mellom produksjonsløsninger (sak/arkiv løsninger), formidling og mottakstjenestene (Digital postkasse og andre produksjonsløsninger). Dette har medført at det er relativt dyrt og kompetansekrevende å forvalte både produksjonsløsningene og de sentrale fellesløsningene. Konsekvensen er at endringsevnen er lav.
Integrasjonspunktet er en implementasjon av et standardisert grensesnitt for integrasjon mot meldingsformidlingstjenester. Grensesnittet innkapsler kompleksiteten som ligger i å sikre, harmonisere meldingsformat og route meldinger til rett formidlingstjeneste. Det samme gjelder for mottakere av meldinger.
Formålet med å innføre integrasjonspunktet er å gi en løs kobling mellom de løsninger som blir benyttet til verdiskaping internt i en virksomhet og de fellestjenester eller parter som de må kommunisere med for å skape denne verdien. I tillegg gir den løse koblingen flere andre fordeler:
•	Mulighet til å konsolidere formidlingstjenester mot referansearkitekturen. Dette kommer som en konsekvens av at man kan endre eMeldingsformidlingstjeneste enklere.
•	Mulighet til å innføre nye og endre eksisterende fellestjenester.

11.5 Påvirke prosjekter
Når det planlegges nye løsninger eller det skal gjøre større endringer i eksisterende infrastruktur for eMelding bør referansearkitekturen benyttes. Det er naturlig at det pekes på referansearkitekturen når prosjekter behandles i Digitaliseringsrådet, i forbindelse med satsningsforslag som del av budsjettprosessen og når det søkes midler i Medfinansieringsordningen. Det bør også informeres om referansearkitekturen på seminarer, konferanser og andre faglige fora.

11.6	Finansiering via CEF Digital
EU-kommisjonen har satt av 1,04 milliard Euro til programmet CEF Telecom i perioden 2014-2020. En stor del av dette brukes til å stimulere interesse og satsing blant medlemslandene for tilkobling til den felles europeiske eMeldingsinfrastrukturen, gjennom CEF Digital . Både offentlige virksomheter og private næringsdrivende kan søke om å få opptil 75 % av prosjektkostnadene sine dekket gjennom CEF for prosjekter som støtter programmets målsettinger.
Finansiering av aktiviteter gjennom CEF-utlysninger er knyttet direkte til byggeklossen eDelivery eller gjennom utlysninger på andre samhandlingskomponenter (DSI-er), for eksempel eProcurement, eInvoicing, eHealth, EESSI, eJustice og BRIS, hvor eDelivery inngår.
Difi er ansvarlig for nasjonal oppfølging  av DSI-en eDelivery og for å mobilisere søknader innen CEF e-Delivery. Difi har også ansvaret for den norske del av CEF Digital-programmet i sin helhet og er kontaktpunkt.


 
12.	Ambisjonsnivå og tiltak

Vi har delt tiltakene i tre hovedområder nedenfor. På bakgrunn av den anbefalte høringen må det lages en handlingsplan for å realisere tiltakene.
Bruk av referansearkitektur generelt
Tabellen under oppsummerer de viktigste tiltakene som kommer ut av kapitel 3 i denne strategien.
Beskrivelse	Ansvar	Status	Ambisjon	Tiltak/behov for å oppnå ambisjon
Bred forankring rundt bruk av referansearkitekturer generelt - vurdere om referansearkitekturer skal være anbefalt å bruke for offentlig sektor.	1) Difi
gjennom-fører høring
	Referanse-arkitektur er ikke benyttet på nasjonalt nivå tidligere, kunnskapen blant potensielle brukere er derfor usikker.	Økt forståelse og bruk av referansearkitekturer som en del av felles offentlig oppgaveløsning på digitaliserings-området.	1) Difi gjennomfører åpen høring av denne strategien og referansearkitekturen.
	2) Difi
har dialog med Standard-iserings sekretar-iatet			2) Vurdere involvering av Standardiseringsrådet om bruk av referansekatalogen og standardiserings-forskriften.
	3) KMD
vurderer omtale i Dig.rund-skrivet			3) Vurdere omtale av referansearkitekturer generelt i Digitaliseringsrundskrivet.
Vurdere å utvikle referansearkitekturer for andre mønstre for utveksling av data.	1) Difi
tar dette opp som sak i SKATE AU
	Referansearkitektur for eMelding er første steg i dette arbeidet.	Med bakgrunn i erfaringene fra arbeidet med denne referanse-arkitekturen, lage referansearkitekturer på andre områder.	1) Ta opp behovet i SKATE AU.
	2) SKATE AU
vurderer og anbefaler			2) SKATE AU anbefaler om dette arbeidet skal prioriteres – og tiltakseier utpekes.










Bruk av referansearkitektur for eMelding
Tabellen under oppsummerer de viktigste tiltakene som kommer ut av kapitel 11 i denne strategien.
Beskrivelse	Ansvar	Status	Ambisjon	Tiltak/behov for å oppnå ambisjon
En mer ensartet måte å utveksle meldinger i og med offentlig sektor.	1) Difi
gjennom-fører høring

	Det foreligger ikke felles føringer eller krav for hvordan meldinger skal utveksles på i dag.	Felles føringer eller krav til eMelding, vil gjøre det enklere å utveksle meldinger. med nye aktører og innen ulike områder.	1) Difi gjennomfører åpen høring for å forankre referansearkitekturen.

	2) Difi/ Standard-iseringsrådet
Behandles som sak i rådet
			2) Vurdere å gjøre hele eller deler av referansearkitekturen anbefalt for offentlig sektor (bør inngå i referansekatalogen) ved nyutvikling eller større omlegginger av løsninger som innebærer eMelding.
Gjøre referanse-arkitektur for eMelding tilgjengelig.	Difi
sørger for publisering på Difi.no	Referansearkitekturen er publisert i foreløpig versjon på Sharepoint.
Endelig referanse-arkitektur på bakgrunn av høring publiseres på Difi.no.	Publisere en versjon 1.0 av referansearkitekturen på Difi.no.
Arbeide for å gjøre referansearkitekturen kjent og tatt i bruk.	Difi
lager mediesak eller blogginnlegg	Referansearkitekturen vil være ukjent inntil den er publisert.	Skape oppmerksomhet når referansearkitekturen foreligger i endelig versjon 1.0.	1) Når endelig versjon av referansearkitekturen er publisert bør det lages en mediesak/blogginnlegg.

	Difi informerer Digitaliserin-gsrådet			2) Informere Digitaliseringsrådet om bruk av referansearkitekturen.
	Difi
tar aktivt kontakt med relevante prosjekter 			3) Aktivt påvirke prosjekter som innebærer utveksling over landegrensen.

	Difi
bruker CEF-koordinator rollen			4) Oppfordre og bidra til at relevante prosjekter får økonomisk støtte fra CEF Digital-programmet.
	Difi
informerer			5) Informere om referansearkitekturen ved bruk av eFormidling.


Beslutte ansvarsforhold og viktige avklaringer
Tabellen under oppsummerer de viktigste tiltakene som kommer ut av kapitel 9 i denne strategien.
Beskrivelse	Ansvar	Status	Ambisjon	Tiltak/behov for å oppnå ambisjon
Overførings-protokoll
(kap. 9.1)	Difi
vurderer tiltak på bakgrunn av høringen	Kommunikasjonsprotokollene AS2 og AS4 anvendes i CEF eDelivery. 	Enighet om å kunne støtte flere kommunikasjons-protokoller i parallell (multiprotokoll).	Vurdere behov for enkelt- eller multiprotokoll.
Capability Lookup/SMP
(kap. 9.2)	Arkitektur løses i dialog mellom Difi og Brønnøysundregistrene
	ELMA ligger i dag hos Difi, og er en forretningskritisk tjeneste i anskaffelsesprosessen og snart for ISO 20022-basert betalings-initiering. Det er diskusjon om det bør ligge i eller i tilknytning til Enhetsregisteret hos Brønnøysundregistrene.	Enighet om arkitektur for Capability Lookup og implementering av SMP.	Gjennomføre en fornyet analyse og behovsspesifisering i forhold til plassering og funksjonalitet av en fremtidig Capability Lookup-komponent.

Forvaltning og utvikling av programvare (kap. 9.3)	Difi
tar opp spørsmål i høringen	Difi utvikler programvare (integrasjonspunkt og Oxalis).	Stimulere til økt konkurranse og økt samhandlingsevne.	Det bør utredes om vi trenger en felles offentlig strategi for hvordan vi håndterer dette på best mulig måte.
Forvaltning av referanse-arkitekturen
(kap. 9.4)	Difi
tar opp spørsmål i høringen	Referansearkitektur for eMelding er en ‘pilot’ for etablering av forvaltningsregime for referansearkitektur.	En offentlig virksomhet har ansvar for forvaltning av referansearkitekturen.	1) Finne egnet virksomhet. Inntil videre kan Difi ha forvalteransvaret.
	Ansvarlig justerer referansearkitekturen i tråd med nye regler.			2) Referansearkitektur tilpasses endringer i lovverk.
Forvaltning av eMeldings-formidlings-infrastrukturen
(kap. 9.5 og 9.6)	Difi
vurderer på bakgrunn av høring	Infrastrukturene er domenespesifikk i dag.
Det er ingen overordnet plassering av ansvar i dag.	Det må tas stilling til om forvaltnings-modellen etablert for PEPPOL eDelivery-nettverket med Difi som PEPPOL-myndighet skal gjenbrukes i andre domener/forretnings-prosesser, eller om andre alternativer skal utredes. 	Finne egnet forvaltningsmodell og vurdere behov for plassering av overordnet ansvar. Inntil videre ivaretar Difi forvalter-ansvaret som PEPPOL-myndighet.  

Finansiering av formidlingstjenester
(kap. 9.7)	SKATE AU – vurderer tiltak	Det eksisterer i dag ulike finansierings-modeller med ulike prismodeller i de forskjellige eMeldingsformidlings-infrastrukturene.	Ved konsolidering av infrastrukturer, basert på felles referanse-arkitektur, etablere en felles finansierings-modell med felles prismekanismer.	1) Kartlegging av ulike prismodeller og finansieringsbehov.
	SKATE – Difi forbereder sak			2) Anbefale forenkling av prismekanismer og forretningsmodeller.


 

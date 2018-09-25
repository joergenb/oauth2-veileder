---
layout: page
title: "Ulike Oauth2 tokens"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_tokens.html

summary: "Ulike Oauth2 tokens brukt i det offentlige, både eOppslag og annen bruk "
---

## Ulike tokens som må støttes

Det er viktig å standardisere innhold i ulike typer tokens slik at konsumenter/tilbydere ikke blir usikre i hvilke virksomhetsprosesser tokenet kan brukes.

Vi kjenner p.t. følgende ulike scenario:

* Vanlige innloggingstoken (id_token) over OpenID connect
* Ansattinnlogging: id_token som forteller hvor en person er ansatt
* Tilgangstoken (access_token) for innbyggerstyrt API-sikring (autentiseringsnær autorisasjon)
* Tilgangstoken for maskin-til-maskin API-sikring
* Ulike varianter av tilgangstoken der det har skjedd en delegering eller token-berikelse fra passende autorativ kilde.


## Eksempler på hvordan slike token kan se ut?

Dette er tidlige utkast !  Må diskuteres !

Innloggingstoken:


```
id_token                    id_token (ansatt)                id_token (fullmakt)            

{                           {                                {                            
  sub: TWGi0...2GBY=,         sub: TWGi0...2GBY=,              sub: TWGi0...2GBY=,        
  pid: 11111156789,           pid: 11111156789,                pid: 11111156789,          
  aud: oidc_oslokommune       aud: oidc_barnehagesystem,       aud: lånekassen,
                              may_act: {                       
                                orgno: 964967725               
                                iss: AltinnAutorisasjon        
                              }                                act: {
                                                                 pid: 32129912345
                                                                 iss: Vergemålsregisteret
															   }	 
}                            }                               }                           
```


1. En innbygger logger til Oslo kommune sin nett-tjeneste.
2. En ansattt i Leikanger Kommune logger inn til et barnehagesystem.  Ansatt-tilknytningen kan komme fra Altinn Autorisasjon gjennom token-berikelse.
3. En verge (32129912345) skal utføre noe i Lånekassen på vegne av 11111156789.  Vergen logger inn med egen eID.  (en betre variant er antakeleg å la sub og pid tilhøre vergen, og heller bruke 'may_act' istadenfor 'act')



Tilganstoken for person:

```
acccess_token                   access_token (m/virksomhetsdelegering)       access_token (m/persondelegering)        

{                               {                                            {                                        
  sub: TWGi0...2GBY=,              sub: TWGi0...2GBY=,                           sub: TWGi0...2GBY=,
  pid: 11111156789,                pid: 11111156789,                             pid: 11111156789,

  scope: svv:kjoretoy              scope: svv:pkk                                scope: nav:trygdeopplysninger        
  client_orgno: "min bil-app"      client_orgno: 817159362                       client_orgno: 934382404              
                                   may_act: {                                    act: {                               
                                      orgno: 924328606                              pid: 31129912345                  
                                   }                                                iss: vergemålsregisteret          
                                                                                 }
                              }                                              }                                    }   
```

4. En innbygger bruke app'en "Mine kjøretøy" (gjerne utviklet av 3djepart) som henter kjøretøysopplysninger fra Statens Vegvesen.
5. En innbygger (presumptivt mekaniker i Leikanger Auto) skal bruke Vegvesenet sitt API til å rapportere PKK. Det er Vitec Autodata som er klient og kan opptre på vegne av Leikanger auto.  (denne kan dobbeltolkes : er det personen eller leverandøren som kan opptre på vegne av Leikanger auto?)
6. Her bruker vergen i eksempel 3 en Evry-løsning som igjen henter trygdeopplysninger for NAV.


Tilgangstoken for virksomhet:

```
access_token (maskinporten)            access_token (maskinporten m/delegering)   access_token (alternativ med 'may_act')

{                                      {                                          {
  scope: nav:trygdeopplysninger           scope: nav:trygdeopplysninger             scope: nav:trygdeopplysninger
  consumer_orgno: 995568217               consumer_orgno: 964967725                 supplier_orgno: 934382404
}                                         act: {                                    may_act: {
                                            supplier_orgno: 934382404                 consumer_orgno: 964967725                      
                                          }                                         }
                                        }                                         }
```

7. Gjensidige henter trygdeopplysninger fra NAV
8. EVRY henter trygdeopplysninger fra NAV på vegne av Leikanger kommune.
9. Som 8. med alternativ notasjon.



```
817159362 = Vitec autodata
924328606 = Leikanger Auto
934382404 = EVRY ASA
964967725 = Leikanger kommune
995568217 = Gjensidige ASA
```

## Beskrivelse av viktige claims

Standardiserte claims:

|claim|spec'|kommentar|
|-|-|-|
|scope|oauth2|ressursen som vert beskytta|
|sub|OIDC, JWT|OIDC: unik, tjenestespesifikk ikke-meningsbærende identifikator(strengt matematisk definert), JWT:  The "sub" (subject) claim identifies the principal that is the  subject of the JWT. |
|aud|OIDC, JWT| audience. OIDC:  klienten som har mottatt id_token.  JWT:The "aud" (audience) claim identifies the recipients that the JWT is intended for.|
|act|Token Exchange|The "act" (actor) claim provides a means within a JWT to express that    delegation has occurred and identify the acting party to whom authority has been delegated.|
|may_act|Token Exchange |The "may_act" claim makes a statement that one party is authorized to  become the actor and act on behalf of another party.||client_orgno||944117784|974761076|999888777 (Storbanken)|777888999 (Lillebanken)|936796702
|acr| OIDC |Sikkerhetsnivå (Auth. context class ref. )|
|amr| OIDC| Autentiseringsmetode|
|client_id|Oauth2 | identifikator for klient, ikke nødvendigvis meningsbærende|

Proprietære "norske" claims:

|claim|spec'|kommentar|status|
|-|-|-|-|
|pid| Kontaktregisteret | personidentifikator i folkeregisteret (burde heitt folkeregisteridentifikator)|i bruk i id-porten|
|orgno| Enhetsregisteret| Organisasjonsnummer|
| client_orgno| | (Norsk) organisasjonsnummer til klienten.  |I bruk i ID-porten |
|consumer_orgno   |   |   Organisasjonsnummer til konsument  | foreslått   |
|supplier_orgno   |   |   Organisasjonsnummer til leverandør | foreslått  |
|token_type   | (evt. `typ`)   | hvilket token (se 1-9 ovanfor) som er utstedt   | foreslått  |
|amr_org || autentiseringsmetode for virksomhet [QCERT for eSeal, virksomhetssertifikat, client_secret, private_key_jwt]|foreslått|



Access tokens bør følge strukturen uansett om dei er direkte utsted av Autorisasjonsserver, eller om dei er eit resultat av ein Token Exchange operasjon.

## Kodeverk for claims

TBD

### sikkerhetsnivå

#### Personinnlogging:
* Level3
* Level4

#### Virksomhet:
(må defineres)

* eIDAS elektronisk segl
* eIDAS avansert segl
* eIDAS kvalifisert segl



## Referanser

* [Enhet] : [Felles informasjonsmodell for Person og Enhet](https://www.difi.no/fagomrader-og-tjenester/digitalisering-og-samordning/nasjonal-arkitektur/informasjonsforvaltning/person-og-enhet-felles-informasjonsmodell)

* [krr] : [https://begrep.difi.no/Felles/personidentifikator](https://begrep.difi.no/Felles/personidentifikator)
* [JWT] : [RFC 7519](https://tools.ietf.org/html/rfc7519)
* [OIDC] : [
OpenID Connect Core 1.0 incorporating errata set 1](http://openid.net/specs/openid-connect-core-1_0.html)

---
layout: page
title: "Retningslinjer for Oauth2 scope i eOppslag"
sidebar: eoppslag
permalink: eoppslag_sbb_oauth2_sekvensdiagram.html

summary: "Retningslinjer for navngiving av Oauth2 scopes i eOppslag"
---

fd

<div class="mermaid">
sequenceDiagram
  participant AP as API-katalog
  participant AA as Altinn Autorisasjon
  participant MP AS maskinporten
  participant C AS konsument(C)
  participant L AS Levernadør(L)
  participant A as API-tilbyder
  note right of A: Registrering av API   
  A->>AP: registrer (OAS-fil, eOppslag=True)
  AP->>AA: registrer (securityScheme,scope)
  AP->>MP: registrer (scope)
  alt direkte-registrering
   A->>MP: registrer (scope)
   MP->>AA: registrer (securityScheme)
  end
  note right of A: 1:gi tilgang
  A->>MP: /acccess (securityScheme, consumer_orgno)
  note right of A: 4: Leikanger kommune...
  note over AA,C: Bemyndiget person logger inn i Altinn og delegerer
  alt D2. oppdatere
  AA->>MP: /delegations (scope, consumer, supplier)
  end
  note right of A: 4: Token-utstedelse  run-time
  note over L,MP: tokenforespørsel fra leverandør
  L->>MP: /token(scope, consumer_orgno)
  MP->>AA: kontroller delegering(scope, consumer_orgno)
  MP->>L: access_token
  L->>A: API-request(token)
  note over C,MP: tokenforespørsel direkte fra konsument
  C->>MP: /token(scope)
  note right of MP: Siden oauth2-klient ikke oppgir hvem den ønsker å opptre på vegne av, trenger ikke Maskinporten sjekke delegering i Altinn
  MP->>C: access_token
  C->>A: API-request(token)
  </div>

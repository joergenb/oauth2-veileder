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

</div>

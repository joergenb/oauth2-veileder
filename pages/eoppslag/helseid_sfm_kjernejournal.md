---
layout: page
title: "Systemlandskap"
sidebar: eoppslag
permalink: systemlandskap.html

summary: "Hvordan ser Oauth2-verdenen ut?"
---





<div class="mermaid">
graph LR

subgraph EPJ
 klient
 klient2
 server
end

klient -- "1. authn"  --> lokalAD
lokalAD -- "2.ID-token" --> server

server -- "3.token exchange (ID-token)" --> HelseID
HelseID -- "4. token response (kjernejournal)" --> server
server  -- "5. AT(scope=kjernejournal, sub='Doktor Legesen')" --> Kjernejournal

Kjernejournal -. "implisitt validering" .- HelseID

</div>



## SFM og Kjernejournal

.

<div class="mermaid">
graph LR

subgraph EPJ
 klient
end

klient-- "2. AT(scope=sfm, sub = 'Doktor Legesen')" --> SFM
klient -- "1. authz"  --> HelseID

SFM -- "3.token exchange (sfm)" --> HelseID
HelseID -- "4. token response (kjernejournal)" --> SFM
SFM -- "5. AT(scope=kjernejournal, sub='Doktor Legesen')" --> Kjernejournal

Kjernejournal -. "implisitt validering" .- HelseID

</div>

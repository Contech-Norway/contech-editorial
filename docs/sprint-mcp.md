# Promo, kommentarer og sprint med agent

[English](sprint-mcp.en.md)

Disse funksjonene krever MCP-tjenesten med sprintutvidelsen og plugin 0.3.0. At pluginen er installert, betyr ikke at serveroppdateringen er deployet. Begynn med `get_sprint_guide` og kontroller tilgjengelige verktøy.

## Tilgang og personvern

Lesere og bidragsytere med bekreftet e-post og aktivt bedriftsmedlemskap kan forberede og gjennomgå sprinter. Redaksjonelle utkast og opplasting av fullvideo/promo krever bidragsyter. Redaksjonelt innhold publiseres fortsatt av mennesker på nettstedet.

Ved eldre tilkobling: trekk først tilbake tilgangen under Agenttilganger, koble agenten til på nytt og godkjenn det utvidede samtykket for kommentarer og sprint. Eldre samtykker åpner ikke kommentarskriving automatisk. Du kan trekke tilgangen tilbake under [Agenttilganger](https://www.contechnorway.com/redaktor/agenter).

Kommentarer er **synlige for medlemmer straks de lagres**. Agenten skal vise eksakt tekst og få godkjenning før deling. Bare egne kommentarer kan redigeres/slettes; moderering skjer på nettstedet. Kommentarene er samme samtale på norsk og engelsk side og oversettes ikke automatisk. Navn må være registrert på medlemssiden, og bedriften må være en av brukerens aktive bedrifter.

Fremdriften er **privat** og lagres separat for forberedelse og gjennomgang. Den følger kontoen mellom klienter. Å åpne en video registrerer ikke automatisk at den er sett. Be agenten markere den som gjennomgått, hoppet over eller ikke gjennomgått. Det kreves ingen kommentar for å fullføre.

## Promofilm i redaktørflyten

> Knytt denne promofilmen til videoen [lenke]. Last opp som privat promo, kontroller status og gi meg lenken til gjennomgang. Ikke publiser.

Promo inngår i `editorial-drafts`, med samme video-ID som fullvideoen. Bruk `create_promo_upload` og `get_promo_status`. Lengden bør være 15–45 sekunder og kan maksimalt være 60. Agenten overfører filen direkte hvis mulig, ellers gir den nettleserlenken. Ny promo erstatter ikke den publiserte før menneskelig publisering.

Automatisk Mux-teksting av promo beholdes. Feil må korrigeres i Mux før gjennomgang og publisering. Fullvideoens `save_caption_draft` er ikke et felt for promoundertekster. En videofil vedlagt chatten er ikke automatisk lastet opp til Contech.

## Forbered sprinten

> La meg forberede meg på denne ukens sprint. Vis fullvideoene én etter én, fortsett fra min lagrede fremdrift, og hjelp meg å formulere kommentarer. Vis teksten og vent på godkjenning før du deler noe.

Skillen `sprint-preparation` henter inneværende toukerssprint etter Oslo-kalenderen og viser datoene. Velg et annet sprintnummer ved behov. Du får fullvideo, tilgjengelig medlemsoppsummering og kommentarer. Nye videoer blir ikke gjennomgått automatisk; avpubliserte videoer utelates.

## Gjennomgå sprinten

> Gjennomgå sprint [nummer] med en kort promofilm og kommentarene til hver video. Vi har sett fullvideoene på forhånd. Vent på beskjed før du går videre.

Skillen `sprint-review` bruker promoen som kort introduksjon, ikke fullvideoen. Mangler promo, vises publisert tekstpitch. Agenten kan foreslå diskusjonsspørsmål, men skal ikke finne på enighet eller beslutninger. Første versjon lagrer ikke felles møtereferat eller oppgaver.

## Klienter og spiller

Bruk samme MCP-adresse som før: `https://www.contechnorway.com/api/mcp`. Codex-pluginen inkluderer de tre skillene. Direkte tilkoblinger i Claude, ChatGPT og Copilot Studio installerer ikke Codex-skills; be agenten hente `get_sprint_guide` og bruk tekstene over.

`open_video` tilbyr en MCP Apps-spiller med kommentarer og neste/forrige der klienten støtter den. Ellers brukes `browser_url`. Under innebygd avspilling sendes kortlivede avspillingssignaturer til komponenten, ikke OAuth-token. Avspilling er ikke medieanalyse: agenten må fortsatt ha tilgang og egnede verktøy for å hevde at den har sett eller transkribert filmen.

Klientstøtte for faktisk avspilling i Claude/Codex må bekreftes i en reell pilot. Den lokale QA-fixturen og automatiske tester beviser ikke avspilling fra Mux eller innlogging i disse klientene.

Ved usikkert svar fra Mux opprettes ikke en ny opplastingsadresse automatisk. Hent status. Hvis ingen opplasting ble registrert, må reservasjonen utløpe (én time) før ny nøkkel kan brukes.

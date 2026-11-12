# Koble KI-assistenten din til Contech Editorial

[English](installation.en.md) · Kontrollert mot leverandørdokumentasjon 26. oktober 2026.

Contech Editorial er en redaktørintegrasjon for videoopplasting og private tekstutkast. Flere KI-assistenter bruker samme MCP-tjeneste. Velg bare oppskriften for klienten din: Codex har en egen plugin med skill, mens de øvrige oppskriftene kobler klienten direkte til tjenesten.

## Før du begynner

Du trenger en personlig Contech-konto med bekreftet e-post, rollen **Bidragsyter** og aktivt bedriftsmedlemskap. Godta invitasjonen og sjekk at du kommer inn på [redaktørsiden](https://www.contechnorway.com/redaktor). Bruk samme e-postadresse når du kobler til agenten.

Du trenger også tilgang til klienten du velger. Abonnement og organisasjonens innstillinger kan begrense egne tilkoblinger. Ingen trenger tilgang til nettstedets private GitHub-repo, databasepassord eller servernøkler.

| Klient | Installasjonsform | Status hos Contech |
| --- | --- | --- |
| Codex | Offentlig plugin med redaktør-skill | Lokal plugin-installasjon og guide-kall kontrollert; offentlig repo kan hentes uten innlogging. Ny maskin må testes. |
| ChatGPT | Egen MCP-tilkobling | Dokumentert oppskrift; ikke testet ende til ende |
| Claude / Claude Desktop | Egen connector | Dokumentert oppskrift; ikke testet ende til ende |
| Claude Code | MCP via terminal | Dokumentert oppskrift; ikke testet ende til ende |
| GitHub Copilot i VS Code | MCP i agentsamtale | Dokumentert oppskrift; ikke testet ende til ende |
| Microsoft Copilot Studio | MCP-verktøy i en Studio-agent | Dokumentert oppskrift; ikke testet ende til ende |

En full videotest med opplasting, transkripsjon, lagring og gjennomgang gjenstår som samlet akseptansetest. Det finnes separate oppskrifter for **GitHub Copilot i VS Code** og **Microsoft Copilot Studio**. Studio-oppskriften gjelder oppsett av en agent, ikke direkte installasjon i Microsoft 365 Copilot.

## Felles tilkoblingsinformasjon

- Navn: `Contech Editorial`
- MCP-adresse: `https://www.contechnorway.com/api/mcp`
- Transport: HTTP / Streamable HTTP
- Innlogging: personlig OAuth via Contech Norway
- Dersom OAuth-klientregistrering må velges: automatisk registrering (DCR), som den eksisterende tjenesten er satt opp for.

## Codex

1. Installer eller oppdater Codex og CLI fra [OpenAIs installasjonsveiledning](https://learn.chatgpt.com/docs/codex/cli). Velg instruksjonen for operativsystemet ditt. Åpne terminalen på nytt og kontroller `codex --version` og `git --version`. Få hjelp med installasjonen hvis en kommando mangler.
2. Kjør:

```sh
codex plugin marketplace add https://github.com/Contech-Norway/contech-editorial.git --ref main
codex plugin add contech-editorial@contech-norway
```

3. Start Codex på nytt. Åpne en ny samtale, skriv `@` og velg **Contech Editorial**. Fullfør Contech-innloggingen hvis du blir bedt om det.
4. Kjør forbindelsestesten nedenfor.

Har du allerede katalogen `contech-norway`, sjekk `codex plugin marketplace list` før du legger til en ny kilde. Be om hjelp ved navnekonflikt; behold fungerende tilkobling. [Plugin-dokumentasjon](https://developers.openai.com/plugins/build/plugins).

## ChatGPT

1. Åpne [Plugins](https://chatgpt.com/plugins), velg **+ → Add custom MCP server**.
2. Bruk navnet og MCP-adressen ovenfor. Velg OAuth og fullfør personlig innlogging. Beskrivelse: «Private video- og artikkelutkast for Contech Norway».
3. Velg tilkoblingen i en ny samtale og kjør testen nedenfor.

Mangler menyvalget, må du sjekke konto-/arbeidsområderettigheter med administrator. Eldre grensesnitt kan bruke Apps/Developer mode. Ikke kjøp et abonnement ut fra denne veiledningen alene. Dette importerer ikke automatisk skillen fra Codex-repoet. [Offisiell oppskrift](https://developers.openai.com/plugins/deploy/connect-chatgpt).

## Claude og Claude Desktop

1. Åpne **Customize → Connectors → Add → Add custom connector**. I Team/Enterprise må eier først legge den til under **Organization settings → Connectors → Add → Custom → Web**.
2. Oppgi navnet og MCP-adressen. Velg **Sign in now** og **Register automatically** dersom OAuth-klientvalget vises. La faste request headers stå tomme.
3. Fullfør Contech-innloggingen, koble til/aktiver connectoren og kjør testen i en ny samtale.

Tilgjengelighet styres også av planen og organisasjonen. [Anthropics veiledning](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp). Ikke legg denne eksterne serveren inn som en lokal stdio-server i desktop-konfigurasjonen.

## Claude Code

Med Claude Code installert, kjør:

```sh
claude mcp add --transport http --scope user contech-editorial https://www.contechnorway.com/api/mcp
```

Åpne Claude Code, kjør `/mcp`, velg serveren og fullfør innlogging i nettleseren. Kjør deretter forbindelsestesten. Dette legger til MCP-verktøyene, ikke Codex-pluginen. [Claude Code-dokumentasjon](https://code.claude.com/docs/en/mcp).

## GitHub Copilot i VS Code

1. Åpne VS Code med GitHub Copilot tilgjengelig. Åpne kommandopaletten: `Ctrl+Shift+P` på Windows/Linux eller `Cmd+Shift+P` på Mac.
2. Kjør **MCP: Add Server**, velg **HTTP**, oppgi MCP-adressen og server-ID `contech-editorial`.
3. Velg personlig/global konfigurasjon (eventuelt **Global → Copilot Global**). Godkjenn serveren og fullfør Contech-innloggingen.
4. Åpne en Copilot-agentsamtale, kontroller at serverens verktøy er tilgjengelige, og kjør testen.

Mangler valgene, sjekk VS Code-versjon og organisasjonens MCP-regler. [Microsofts veiledning](https://code.visualstudio.com/docs/agent-customization/mcp-servers).

## Microsoft Copilot Studio

Kontrollert mot Microsofts dokumentasjon 12. november 2026; ikke testet ende til ende hos Contech. Dette kobler en agent i Copilot Studio til samme MCP-tjeneste. GitHub-tilgang og Codex-pluginen er ikke nødvendig.

Du trenger redigeringstilgang til en Copilot Studio-agent og Contech-tilgangen beskrevet ovenfor.

1. Åpne agenten i [Copilot Studio](https://copilotstudio.microsoft.com/).
2. Velg **Tools → Add a tool → New tool → Model Context Protocol**.
3. Fyll inn feltene under.
4. Velg **Create → Next**, opprett en forbindelse og fullfør personlig Contech-innlogging. Velg **Add to agent**.

| Felt | Verdi |
| --- | --- |
| Server name | `Contech Editorial` |
| Server description | Private video- og artikkelutkast for Contech Norway. Brukeren gjennomgår og publiserer på nettstedet. |
| Server URL | `https://www.contechnorway.com/api/mcp` |
| Authentication | `OAuth 2.0` |
| OAuth type | `Dynamic discovery` |

Automatisk klientregistrering skal finne OAuth-oppsettet. Organisasjonens Power Platform-regler kan blokkere tilkoblingen; kontakt administrator ved en slik feilmelding. [Microsofts oppskrift](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-add-existing-server-to-agent).

I det nye grensesnittet (forhåndsversjon) er inngangen **Build → Tools → Add tool → Add → Model Context Protocol**. [Microsofts veiledning for det nye grensesnittet](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/tools-add-mcp-server).

### Instruksjoner til agenten

Codex-skillen installeres ikke automatisk. Legg dette i agentens **Instructions**:

```text
Hent get_editorial_guide før redaksjonelt arbeid og følg veiledningen.
Bruk MCP til å hente innhold og lagre private utkast. Valider med validate_draft.

Foretrekk egen transkripsjon fra faktisk lyd hvis du har tilgang og egnede
verktøy. Gjennomgå hele videoen i tidsvinduer med tidskodede skjermbilder.
Ikke utled tale fra bilder. Oppgi manglende tilgang, usikkerhet og hull.
Bruk Mux-transkripsjonen bare når brukeren uttrykkelig ønsker det.

Følg språkkravene i veiledningen, inkludert engelsk oversettelse der det kreves.
Lagre transkripsjon som privat undertekstforslag. Be om opplasting via browser_url
hvis du ikke kan overføre filen direkte. Unngå å opprette eksisterende videoer på nytt.

Når utkastet er klart, valider det og gi brukeren redaktørlenken for gjennomgang
og publisering. Ikke publiser eller merk innhold som menneskelig gjennomgått.
Påstå bare at noe er lagret eller lastet opp når verktøyet har bekreftet det.
```

### Første test og video

Kjør [forbindelsestesten](#test-forbindelsen-før-opplasting) i testchatten. Kontroller i kjøringsloggen at `get_editorial_guide` faktisk ble kalt og svarte, før du prøver å lagre eller laste opp. Test deretter med startteksten under [Skills og første video](#skills-og-første-video).

MCP-tilkoblingen gir ikke i seg selv evnen til å analysere video, høre lyd eller overføre en chatfil. Disse egenskapene må testes separat i Copilot Studio. Bruk opplastingslenken hvis direkte overføring ikke støttes. Backend bestiller fortsatt Mux-undertekster automatisk; instruksjonen styrer agentens tekstkilde og slår ikke av denne funksjonen.

## Test forbindelsen før opplasting

Send med tilkoblingen valgt:

> Bruk Contech Editorial. Hent get_editorial_guide og oppsummer reglene. Ikke opprett eller endre innhold.

Kontroller at et faktisk verktøykall lykkes. Et svar basert på tekst fra denne veiledningen er ikke en forbindelsestest. Ingen skriverettigheter eller videoopplasting er bevist av dette ene lesekallet.

## Skills og første video

Codex-pluginen inkluderer `editorial-drafts`. Direkte MCP-oppsett i de andre klientene installerer ikke denne skillen automatisk. Alle kan hente serverens redaktørveiledning; bruk startteksten under for å gjøre arbeidsflyten tydelig. [Les hele skillen](../plugins/contech-editorial/skills/editorial-drafts/SKILL.md).

Agenten anbefales å transkribere lyd direkte og undersøke skjermbilder i passende tidsvinduer (for eksempel 30–60 sekunder, tettere ved viktige endringer). Skjermbilder gir visuell kontekst, ikke bevis for hva som ble sagt. Dette krever at klienten kan lese filmen og lyden. Dagens MCP gir ikke råvideo til analyse; en originalfil må være tilgjengelig for agenten. Mux-undertekster bestilles fortsatt automatisk av opplastingstjenesten, og skillen slår ikke dette av. Mux brukes som tekstkilde bare etter uttrykkelig ønske. Agenten skal lagre og validere utkast via MCP, og gi redaktørlenken når agentarbeidet er ferdig. Menneskelig kontroll, overføring av undertekstforslag til spilleren og publisering skjer på nettstedet.

> Les get_editorial_guide først. Bruk videoen jeg velger, med tema [tema] og talespråk [språk]. Opprett et privat videoutkast, last opp, transkriber selv fra lyden og gjennomgå filmen i tidsvinduer med tidskodede skjermbilder. Bruk bare Mux-transkripsjon hvis jeg uttrykkelig ber om det. Si fra hvis du mangler tilgang til lyd eller bilde. Lagre transkripsjonen som privat undertekstforslag via MCP, og lag offentlig pitch på bokmål og britisk engelsk samt medlemsoppsummering på originalspråket. Ikke opprett fagartikkel nå. Be meg laste opp via browser_url hvis du ikke kan overføre filen direkte. Valider utkastet og gi meg redaktørlenken. Ikke publiser eller merk noe som menneskelig gjennomgått.

Erstatt hakeparentesene. I nettklienter skal du normalt bruke opplastingslenken: å legge en video ved i chatten laster den ikke automatisk opp til Contech. For allerede opplastet video: oppgi lenken til eksisterende utkast og be om videre arbeid der.

Kontroller navn, fakta, begge språk, video og undertekster på nettstedet før publisering. Offentlig pitch og publisert fagartikkel kan leses av alle; video og medlemsoppsummering krever medlemskap. Ikke kopier privat sammendrag til offentlig artikkel uten at dette er avtalt.

## Hvis noe stopper

- **Ingen verktøy:** aktiver tilkoblingen og prøv en ny samtale.
- **Innlogging/tilgang avvist:** sjekk konto, invitasjon, rolle og medlemskap på nettstedet. Koble til på nytt ved behov.
- **OAuth-klient-ID/secret kreves:** stopp og be om hjelp; ikke bruk database- eller servernøkler. Oppsettet skal bruke automatisk klientregistrering.
- **Filen kan ikke overføres:** be om `browser_url` og last opp der.
- **Transkripsjon venter:** vent og sjekk status senere; ikke opprett samme video på nytt.
- **Lagringskonflikt:** be agenten hente siste utkast før den prøver igjen.

Send feilmelding og klientnavn til Contech-kontakten din, uten innloggingskoder eller tokens. Tilgang kan trekkes tilbake på [agenttilganger](https://www.contechnorway.com/redaktor/agenter).

# Installer Contech Editorial

[English](installation.en.md) · Kontrollert mot leverandørdokumentasjon 26. oktober 2026.

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

En full videotest med opplasting, transkripsjon, lagring og gjennomgang gjenstår som samlet akseptansetest. Copilot-oppskriften gjelder **GitHub Copilot i VS Code**, ikke Microsoft 365 Copilot.

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

## Test forbindelsen før opplasting

Send med tilkoblingen valgt:

> Bruk Contech Editorial. Hent get_editorial_guide og oppsummer reglene. Ikke opprett eller endre innhold.

Kontroller at et faktisk verktøykall lykkes. Et svar basert på tekst fra denne veiledningen er ikke en forbindelsestest. Ingen skriverettigheter eller videoopplasting er bevist av dette ene lesekallet.

## Skills og første video

Codex-pluginen inkluderer `editorial-drafts`. Direkte MCP-oppsett i de andre klientene installerer ikke denne skillen automatisk. Alle kan hente serverens redaktørveiledning; bruk startteksten under for å gjøre arbeidsflyten tydelig. [Les hele skillen](../plugins/contech-editorial/skills/editorial-drafts/SKILL.md).

> Les get_editorial_guide først. Bruk videoen jeg velger, med tema [tema] og talespråk [språk]. Opprett et privat videoutkast, last opp, hent transkripsjonen når den er klar, og lag offentlig pitch på bokmål og britisk engelsk samt medlemsoppsummering på originalspråket. Ikke opprett fagartikkel nå. Be meg laste opp via browser_url hvis du ikke kan overføre filen direkte. Valider utkastet og gi meg redaktørlenken. Ikke publiser eller merk noe som menneskelig gjennomgått.

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

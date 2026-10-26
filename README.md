# Contech Editorial

[Norsk](#norsk) · [English](#english)

## Norsk

**[Installasjonsveiledning: Codex, ChatGPT, Claude og GitHub Copilot](docs/installasjon.md)**

Contech Editorial er en Codex-plugin for å laste opp videoer og lage private tekstutkast for Contech Norway. Pakken inneholder tilkoblingen til MCP-tjenesten og redaktør-skillen `editorial-drafts`. Nettsidens kildekode, medlemsinnhold og innloggingsopplysninger er ikke inkludert.

### Installere

Du trenger Codex og en versjon av Codex CLI som støtter `codex plugin`. Git må være tilgjengelig for å hente repoet. GitHub-konto og tilgang til det private nettsiderepoet er ikke nødvendig.

Kjør i Terminal:

```sh
codex plugin marketplace add https://github.com/Contech-Norway/contech-editorial.git --ref main
codex plugin add contech-editorial@contech-norway
```

Start Codex på nytt. Åpne en ny samtale, skriv `@` og velg **Contech Editorial**. Fullfør eventuell innlogging med din egen Contech-konto og godkjenn agenttilgangen. Du må ha bekreftet e-post, aktivt bedriftsmedlemskap og rollen **Bidragsyter**. Installasjonen gir ikke disse rettighetene.

Hvis `codex` eller plugin-kommandoen mangler, få Codex CLI installert eller oppdatert før du fortsetter. [Offisiell Codex CLI-veiledning](https://developers.openai.com/codex/cli/).

### Første test

Med pluginen valgt, send:

> Hent get_editorial_guide og sjekk forbindelsen. Ikke opprett eller endre innhold.

Velg deretter en kort video, oppgi filen eller filbanen, tema og talespråk, og be om opplasting og tekstutkast. Offentlig tekst skrives på bokmål og britisk engelsk. Agenten kan gi en nettleserlenke hvis direkte filopplasting ikke støttes. Har du allerede lastet opp filmen, oppgi lenken til eksisterende utkast for å unngå en kopi.

Alt agenten lagrer, forblir private utkast. Kontroller video, undertekster, navn, fakta og oversettelse på [redaktørsiden](https://www.contechnorway.com/redaktor) før du publiserer. Pitchen er offentlig etter publisering; videoen og medlemsoppsummeringen krever medlemskap. En publisert fagartikkel er offentlig. [Tilbakekall agenttilgang](https://www.contechnorway.com/redaktor/agenter).

### Oppdateringer og eksisterende installasjoner

Dette repoet er distribusjonskilden for pluginen. Nye versjoner skal få nytt versjonsnummer i manifestet. Oppdater katalogkilden med `codex plugin marketplace upgrade contech-norway`, oppdater pluginen via plugin-katalogen dersom en oppdatering tilbys, og start en ny samtale.

Har du allerede en lokal katalog kalt `contech-norway`, kontroller `codex plugin marketplace list` før du legger til denne kilden. Ikke slett en fungerende MCP-tilkobling før den nye plugin-tilkoblingen er kontrollert.

Pluginen er offentlig tilgjengelig her, men er ikke oppført i OpenAIs offentlige plugin-katalog. MCP-tjenesten krever personlig innlogging. Ingen databasepassord eller API-nøkler skal deles eller legges i pakken.

## English

**[Installation guide: Codex, ChatGPT, Claude and GitHub Copilot](docs/installation.en.md)**

Contech Editorial is a Codex plugin for video uploads and private editorial drafts for Contech Norway. It bundles the remote MCP connection and the `editorial-drafts` skill. Website source code, member content and credentials are not included.

### Install

You need Codex, a Codex CLI version supporting `codex plugin`, and Git to fetch the repository. No GitHub account or access to the private website repository is required. Run the two commands above, restart Codex, open a new conversation and select **Contech Editorial** using `@`.

Complete sign-in and authorise access with your own Contech account. You need a verified email address, active company membership and the **Bidragsyter** (contributor) role. Installing the plugin does not grant these permissions. If the CLI or plugin command is missing, install or update the CLI first using the [official guide](https://developers.openai.com/codex/cli/).

### First test

> Fetch get_editorial_guide and check the connection. Do not create or change content.

Then choose a short video and provide its file or path, subject and spoken language. Ask for upload and draft text. Public text is drafted in Norwegian Bokmål and British English. If direct upload is unavailable, the agent can provide a browser upload link. For a video already uploaded, provide the existing draft link to avoid creating a duplicate.

Everything the agent saves remains a private draft. Review the video, captions, names, facts and translation in the [website editor](https://www.contechnorway.com/redaktor) before publishing. Published pitches are public; videos and member summaries require membership. Published articles are public. [Revoke agent access](https://www.contechnorway.com/redaktor/agenter).

### Updates and existing installations

This repository is the plugin distribution source. New releases must increment the manifest version. Refresh the marketplace with `codex plugin marketplace upgrade contech-norway`, update the plugin through the plugin directory when offered, and start a new conversation.

If a local marketplace called `contech-norway` already exists, inspect `codex plugin marketplace list` before adding this source. Keep any working standalone MCP connection until the new plugin connection has been verified.

This public repository is separate from OpenAI's public plugin directory. The MCP service requires personal authentication. Never share database passwords or API keys or include them in this package.

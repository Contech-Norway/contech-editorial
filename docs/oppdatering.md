# Oppdater Contech Editorial og logg inn på nytt

[English](update.en.md) · Kontrollert med lokal Codex CLI 17. november 2026.

Pluginfiler installeres på hver Mac. En oppdatering på én maskin oppdaterer ikke automatisk den andre. OAuth-innlogging er et eget steg; installasjon av nye skills gir ikke nye rettigheter.

## 1. Finn Codex-kommandoen

Prøv `codex --version` i Terminal. Ved `command not found` kan appen likevel inneholde CLI. På Macen som ble kontrollert lå den her:

```sh
"/Applications/ChatGPT.app/Contents/Resources/codex-cli/CodexCLI.app/Contents/MacOS/codex" --version
```

Dette er en kontrollert appsti, ikke en garanti for alle installasjoner. Hvis filen mangler, følg [offisiell CLI-installasjon](https://developers.openai.com/codex/cli/). Ikke avinstaller appen for å rette søkestien.

I kommandoene nedenfor: bruk `codex` hvis den virker. Ellers erstatt bare ordet `codex` med hele stien i anførselstegn over.

## 2. Kontroller katalogkilden på denne Macen

```sh
codex plugin marketplace list
```

- **Git-kilde fra det offentlige repoet:** Oppdater katalogen og installer ny pluginversjon:

```sh
codex plugin marketplace upgrade contech-norway
codex plugin add contech-editorial@contech-norway
```

- **Lokal katalog**, for eksempel nettstedets lokale prosjektmappe: `marketplace upgrade` henter ikke nye filer til den. Oppdater den lokale kildekoden først. Kjør deretter `codex plugin add contech-editorial@contech-norway`. Ikke legg til en annen katalog med samme navn uten å avklare kildebyttet.
- **Ingen contech-norway-katalog:** Installer fra det offentlige repoet:

```sh
codex plugin marketplace add https://github.com/Contech-Norway/contech-editorial.git --ref main
codex plugin add contech-editorial@contech-norway
```

Kontroller installasjonsmeldingen. For denne utgaven skal den vise en mappe som slutter på `contech-editorial/0.4.0`. Start appen på nytt og åpne en ny chat. Pakken inneholder `editorial-drafts`, `sprint-preparation` og `sprint-review`.

## 3. Ny autentisering for sprint og kommentarer

[Samlet installasjonsside og hjelp](https://www.contechnorway.com/koble-til). Versjon 0.4.0 krever serverens nye get_connection_status-verktøy. Etter utrulling gir tilbakekalt token en OAuth-challenge; klientens automatiske innloggingsflyt må fortsatt verifiseres. Agenttilganger skiller mellom tilgang godkjent og sist observerte MCP-kontakt.

Skjermbilder fra Codex 17. november viser at MCP-serverens innstillingsside har URL og headers, men ingen innloggingsknapp. Den tidligere instruksjonen om å finne en slik knapp var upresis. For den bekreftede separat konfigurerte serveren med navn contech-editorial: bruk CLI-reserveflyten nedenfor. Ikke avinstaller pluginen eller fyll inn tokens.

Du trenger dette når den gamle koblingen mangler de nye tillatelsene, eller tilgangen er tilbakekalt. Ikke trekk tilbake en fungerende kobling ved hver ordinære pluginoppdatering.

1. Åpne [Agenttilganger](https://www.contechnorway.com/redaktor/agenter) og trekk tilbake den aktuelle gamle agenttilgangen hvis det ikke allerede er gjort. Dette kan påvirke andre klienter som bruker akkurat samme grant; ikke anta at det bare gjelder én Mac.
2. Prøv et lesende MCP-kall fra Codex og følg innloggingsforespørselen hvis klienten tilbyr den. Den kontrollerte MCP-innstillingssiden har ingen innloggingsknapp. Hvis ingen forespørsel vises og serveren er separat konfigurert, bruk CLI-reserveflyten nedenfor. Ikke opprett en ekstra MCP-server ved siden av pluginen.
3. Fullfør innloggingen i nettleseren med din personlige Contech-konto. Les samtykket: kommentarer deles med medlemmer, sprintfremdrift er privat, og redaksjonell publisering skjer på nettstedet.
4. Godkjenn **Tillat agenttilgang og kommentarer / Allow agent access and comments** på Contech-siden. Gå tilbake til appen og åpne en ny chat med pluginen.
5. Send:

> Hent get_connection_status og sjekk tilgangen min. Ikke opprett eller endre innhold.

Testen må faktisk kalle MCP-verktøyet. Be agenten kontrollere om kommentartilgangen er aktiv. Pluginversjon og en tekstlig gjengivelse av guiden beviser ikke at OAuth fungerer.

Hvis innlogging ikke tilbys, del skjermbilde av pluginens detaljside og eventuell feilmelding. Ikke del koder, tokens eller callback-URL. Brukerens skjermbilder bekrefter at den viste MCP-innstillingssiden mangler innloggingsknapp. Automatisk klientinnlogging etter 401 er fortsatt ikke ende-til-ende-verifisert.

### Bare for en separat CLI-konfigurert MCP-server

`codex mcp login` og `logout` finnes, men ikke anta at de kan administrere app-pluginens tilkobling. For en separat server: finn det faktiske navnet med `codex mcp list`, bruk det med `codex mcp logout NAVN` og `codex mcp login NAVN`. Ikke kjør dette som en ekstra installasjonsoppskrift for pluginen.

## Verifisert innloggingstest 17. november 2026

Testet med Codex CLI `0.165.0-alpha.1.2` og en separat testkobling mot produksjon: godkjenning → vellykket `get_connection_status` → tilbakekalling → avvist statuskall → vanlig `codex mcp login` med nytt samtykke → vellykket statuskall. Ingen innholdsendringer ble utført.

Codex-klienten startet ikke ny innlogging automatisk i denne testen; den rapporterte feil ved fornyelse av OAuth-token. Den vanlige CLI-innloggingen med lokal callback-mottaker fungerte. Unngå `--no-browser` i denne veiledningen: denne modusen krever manuell håndtering av callback-adressen, og et forsøk utløp under testen. Bruk nettleseren på samme maskin som CLI-en, og la innloggingskommandoen kjøre til den melder at innloggingen lykkes.

Dette verifiserer reserveflyten for en separat MCP-server. Automatisk innlogging i skrivebordspluginen er fortsatt ikke bekreftet. Codex registrerte en ny OAuth-klient ved ny innlogging; den gamle testtilgangen forble tilbakekalt.

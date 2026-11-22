# Undertekster og publisering av kortpitch

[English](promo-captions.en.md)

Promo har egne undertekster og egen publisering. At fullvideoen er publisert, eller at promo har status `ready`, betyr ikke at promo vises i biblioteket. Før promo publiseres viser videosiden thumbnailen. Etter publisering viser den en knapp for å spille kortintroduksjonen; filmen starter ikke automatisk.

## Agentens arbeid

1. Finn videoen og bruk den separate promofilens lyd og bilder. Agentens egen transkripsjon er anbefalt når klienten faktisk kan analysere mediet. Bruk Mux-tekst som kilde bare når brukeren ønsker det. Automatisk Mux-teksting bestilles fortsatt ved opplasting.
2. Hent `get_promo_transcript`. Resultatet gir eksisterende forslag (eller ubehandlet Mux-tekst), `upload.nonce` og `expected_revision`.
3. Lagre WebVTT med promoens egne tidskoder via `save_promo_caption_draft`: `id`, `expected_upload`, `expected_revision`, `language_code` og `vtt`. `save_caption_draft` gjelder bare hovedfilmen.
4. Hent `get_promo_status` og lever redaktørlenken. Rapporter `published`, `current_upload_published` og `caption_import_status`. Ikke påstå at lagring av redaksjonstekst eller et privat undertekstforslag har endret spilleren.

## Redaktørens arbeid

Åpne videoens redaktørside og «Offentlig promovideo». Rediger WebVTT og lagre privat, eller gjennomgå agentens lagrede forslag. Kryss av for gjennomgang og velg «Send gjennomgåtte promoundertekster til spilleren». Dette sender akkurat den viste revisjonen til Mux. Hvis promoen allerede er publisert, endres også de offentlige undertekstene når importen er klar.

Velg «Oppdater promostatus», og se gjennom promoen med det nye tekstsporet. Gamle spor på samme språk beholdes til erstatningen er klar. Godkjenn deretter «Publiser gjennomgått promo». Statusen øverst forklarer om den aktuelle eller en tidligere promo er offentlig.

Ved samtidige endringer må siden lastes på nytt. En ny promoopplasting krever et nytt undertekstforslag; gamle tidskoder gjenbrukes ikke automatisk. Ved uavklart eller feilet import: kontroller status, lagre en ny revisjon og godkjenn den før nytt forsøk. Systemet gjentar ikke en usikker Mux-opprettelse automatisk.

## Utrulling

Migrasjonen `20261122193041_promo_caption_drafts.sql` må inn før serverkoden. Kjør Supabase-advisors etter migrasjonen. Pluginens oppdaterte skill distribueres etter serverdeploy. Lokale tester dekker tilgang, revisjoner, opplastingsbytte, immutable importkilder, signaturer og Mux-feil. En virkelig Mux-import og menneskelig gjennomgang må kontrolleres etter deploy; utviklingstester publiserer ingen medlemsvideoer.

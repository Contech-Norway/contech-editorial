# Short-pitch captions and publication

[Norsk](promo-captions.md)

A promo has its own captions and publication step. Publishing the full video, or a promo reaching `ready`, does not make the promo visible in the library. Before promo publication, the library shows the thumbnail. Afterwards it shows a button to play the short introduction; playback does not start automatically.

## Agent workflow

1. Find the video and use the separate promo file's audio and visuals. The agent's own transcription is recommended when the client can actually analyse the media. Use Mux text as source material only when the user requests it. Uploads still request automatic Mux captions.
2. Call `get_promo_transcript`. It returns the existing draft (or unreviewed Mux text), `upload.nonce` and `expected_revision`.
3. Save WebVTT with the promo's own timestamps using `save_promo_caption_draft`: `id`, `expected_upload`, `expected_revision`, `language_code` and `vtt`. `save_caption_draft` applies only to the full video.
4. Call `get_promo_status` and provide the editor link. Report `published`, `current_upload_published` and `caption_import_status`. Do not claim that saving editorial copy or a private caption draft has changed the player.

## Editor workflow

Open the video's editor page and “Offentlig promovideo” (Public promo video). Edit and privately save WebVTT, or review the agent's saved draft. Confirm review and select “Send gjennomgåtte promoundertekster til spilleren” (Send reviewed promo captions to the player). This sends exactly the displayed revision to Mux. If the promo is already published, its public captions also change once the import is ready.

Select “Oppdater promostatus” (Refresh promo status), then preview the promo with its new caption track. Old tracks in the same language remain until the replacement is ready. Then approve “Publiser gjennomgått promo” (Publish reviewed promo). The status at the top explains whether the current or an earlier promo is public.

Reload after concurrent changes. A replacement promo upload needs a new caption draft; old timestamps are not automatically reused. If an import fails or its outcome is uncertain, check its status, save a new revision and approve it before retrying. The system never automatically repeats an ambiguous Mux creation request.

## Rollout

Apply `20261122193041_promo_caption_drafts.sql` before deploying the server code. Run Supabase advisors afterwards. Distribute the updated plugin skill after server deployment. Local tests cover access, revisions, upload replacement, immutable import sources, signatures and Mux failures. A real Mux import and human review must be verified after deployment; development tests do not publish member videos.

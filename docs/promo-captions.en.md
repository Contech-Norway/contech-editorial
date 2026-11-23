# Short-pitch captions and publication

[Norsk](promo-captions.md)

A promo has its own captions and publication step. Publishing the full video, or a promo reaching `ready`, does not make the promo visible in the library. Before promo publication, the library shows the thumbnail. Afterwards it shows a button to play the short introduction; playback does not start automatically.

## Agent workflow

1. Find the video and use the separate promo file's audio and visuals. The agent's own transcription is recommended when the client can actually analyse the media. Use Mux text as source material only when the user requests it. Uploads still request automatic Mux captions.
2. Call `get_promo_transcript`. It returns the existing draft (or unreviewed Mux text), `upload.nonce` and `expected_revision`.
3. Save WebVTT with the promo's own timestamps using `save_promo_caption_draft`: `id`, `expected_upload`, `expected_revision`, `language_code` and `vtt`. `save_caption_draft` applies only to the full video.
4. Show the entire saved caption draft with timestamps in chat, identifying the video and revision. Call `get_promo_status`, explain whether the promo is already public, and obtain explicit approval of this exact text. A general editing request is not approval of the finished text.
5. After approval, call `submit_promo_captions` with `id`, `expected_upload`, `approved_revision` and `user_confirmed: true`. The tool sends the saved revision directly to the player. Changed text requires approval of the new revision first.
6. Call `get_promo_status` and provide the editor link. Report `published`, `current_upload_published` and `caption_import_status`. Do not claim that saving editorial copy or a private caption draft has changed the player.

## Editor workflow

Promo caption approval and submission can happen in chat as described above. The website also provides an alternative editing and import workflow:

Open the video's editor page and “Offentlig promovideo” (Public promo video). Edit and privately save WebVTT, or review the agent's saved draft. Confirm review and select “Send gjennomgåtte promoundertekster til spilleren” (Send reviewed promo captions to the player). This sends exactly the displayed revision to Mux. If the promo is already published, its public captions also change once the import is ready.

Select “Oppdater promostatus” (Refresh promo status), then preview the promo with its new caption track. Old tracks in the same language remain until the replacement is ready. Then approve “Publiser gjennomgått promo” (Publish reviewed promo). The status at the top explains whether the current or an earlier promo is public.

Reload after concurrent changes. A replacement promo upload needs a new caption draft; old timestamps are not automatically reused. If an import fails or its outcome is uncertain, check its status, save a new revision and approve it before retrying. The system never automatically repeats an ambiguous Mux creation request.

## Rollout

Apply `20261122193041_promo_caption_drafts.sql` before deploying the server code. Run Supabase advisors afterwards. Distribute the updated plugin skill after server deployment. Local tests cover access, revisions, upload replacement, immutable import sources, signatures and Mux failures. A real Mux import and human review must be verified after deployment; development tests do not publish member videos.

## Confirmation and access

The server requires contributor access, the correct upload, the exact revision and an explicit confirmation field. The client and skill must ensure the user actually saw and approved the text in chat; the server cannot independently prove conversational consent. Agents still cannot publish the promo or other content.

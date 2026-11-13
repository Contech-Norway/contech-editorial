# Promos, comments and sprints with an agent

[Norsk](sprint-mcp.md)

These features require the MCP service with the sprint extension and plugin 0.3.0. Installing the plugin does not mean the server update has been deployed. Start with `get_sprint_guide` and check available tools.

## Access and privacy

Readers and contributors with verified email and active company membership can prepare and review sprints. Editorial drafts and full-video/promo uploads require contributor access. People still publish editorial content on the website.

For an older connection, first revoke it under Agent access, reconnect your agent and approve the expanded consent for comments and sprints. Older approvals do not automatically permit comment writes. Revoke access under [Agent access](https://www.contechnorway.com/redaktor/agenter).

Comments are **visible to members as soon as they are saved**. The agent must show the exact text and obtain approval before sharing. Only your own comments can be edited/deleted; moderation remains on the website. Norwegian and English pages share the same discussion, without automatic translation. Set your profile name on the membership page and choose one of your active companies.

Progress is **private**, separate for preparation and review, and follows your account between clients. Opening a video does not mark it as watched. Ask the agent to mark it completed, skipped or unseen. A comment is not required to complete a video.

## Promos in the editorial workflow

> Attach this promo to video [link]. Upload it as a private promo, check its status and give me the review link. Do not publish.

Promos belong to `editorial-drafts`, using the full video's existing ID. Use `create_promo_upload` and `get_promo_status`. The recommended length is 15–45 seconds, with a maximum of 60. The agent transfers the file directly where possible or supplies a browser upload link. A replacement does not affect the published promo until a person publishes it.

Automatic Mux promo captions remain enabled. Correct errors in Mux before human review and publication. The full video's `save_caption_draft` does not store promo captions. Attaching a video to chat does not automatically upload it to Contech.

## Prepare for a sprint

> Help me prepare for this week's sprint. Show full videos one at a time, resume my saved progress and help me draft comments. Show the text and wait for approval before sharing anything.

The `sprint-preparation` skill finds the current fortnight using the Oslo calendar and displays its dates. Choose another sprint number when needed. You receive the full video, available member summary and comments. New videos are not automatically completed; unpublished videos are omitted.

## Review a sprint

> Review sprint [number] with a short promo and the comments for each video. We have watched the full videos beforehand. Wait for instructions before moving on.

The `sprint-review` skill introduces each video with its promo, rather than the full recording. If no promo exists, it uses the published text pitch. The agent can suggest discussion questions but must not invent agreement or decisions. This version does not save shared meeting minutes or tasks.

## Clients and player

Keep the same MCP URL: `https://www.contechnorway.com/api/mcp`. The Codex plugin includes all three skills. Direct connections in Claude, ChatGPT and Copilot Studio do not install Codex skills; ask the agent to fetch `get_sprint_guide` and use the prompts above.

`open_video` offers an MCP Apps player with comments and previous/next navigation where supported. Otherwise, use `browser_url`. Inline playback sends short-lived playback signatures to the component, never OAuth tokens. Playback does not imply media analysis: the agent still needs access and suitable tools before claiming to have watched or transcribed the recording.

Actual inline playback in Claude/Codex must be confirmed in a real pilot. The local QA fixture and automated tests do not prove Mux playback or sign-in in these clients.

An uncertain Mux response does not automatically create another upload URL. Check status. If no upload was recorded, wait for the reservation to expire (one hour) before using a new key.

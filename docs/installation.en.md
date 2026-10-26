# Install Contech Editorial

[Norsk](installasjon.md) · Checked against vendor documentation on 26 October 2026.

## Before you start

You need a personal Contech account with a verified email address, the **Bidragsyter** (contributor) role and active company membership. Accept your invitation and confirm access to the [website editor](https://www.contechnorway.com/redaktor). Use the same email when connecting the agent.

You also need access to your chosen client. Plans and organisation policies may restrict custom connections. Nobody needs access to the private website repository, database passwords or server keys.

| Client | Setup | Contech verification status |
| --- | --- | --- |
| Codex | Public plugin with editorial skill | Local plugin installation and guide call checked; public repository downloads without sign-in. A new machine still needs testing. |
| ChatGPT | Custom MCP connection | Documentation-based instructions; not tested end to end |
| Claude / Claude Desktop | Custom connector | Documentation-based instructions; not tested end to end |
| Claude Code | Terminal MCP setup | Documentation-based instructions; not tested end to end |
| GitHub Copilot in VS Code | MCP in an agent conversation | Documentation-based instructions; not tested end to end |

A complete video acceptance test covering upload, transcription, saving and review remains outstanding. The Copilot instructions cover **GitHub Copilot in VS Code**, not Microsoft 365 Copilot.

## Shared connection details

- Name: `Contech Editorial`
- MCP URL: `https://www.contechnorway.com/api/mcp`
- Transport: HTTP / Streamable HTTP
- Authentication: personal OAuth through Contech Norway
- If asked how to register the OAuth client: automatic registration (DCR), supported by the existing service.

## Codex

1. Install or update Codex and its CLI using [OpenAI's installation guide](https://learn.chatgpt.com/docs/codex/cli) for your operating system. Reopen your terminal and check `codex --version` and `git --version`. Get installation help if either command is missing.
2. Run:

```sh
codex plugin marketplace add https://github.com/Contech-Norway/contech-editorial.git --ref main
codex plugin add contech-editorial@contech-norway
```

3. Restart Codex. In a new conversation, type `@`, choose **Contech Editorial**, and complete Contech sign-in if prompted.
4. Run the connection test below.

If `contech-norway` already exists, inspect `codex plugin marketplace list` first. Get help with name conflicts and retain a working connection. [Plugin documentation](https://developers.openai.com/plugins/build/plugins).

## ChatGPT

1. Open [Plugins](https://chatgpt.com/plugins), then **+ → Add custom MCP server**.
2. Enter the shared name and URL, choose OAuth and sign in personally. Description: “Private video and article drafts for Contech Norway”.
3. Select the connection in a new conversation and run the test.

If unavailable, check account/workspace permissions with your administrator. Older interfaces may use Apps/Developer mode. Do not purchase a plan based solely on this guide. This does not automatically import the Codex skill. [Official instructions](https://developers.openai.com/plugins/deploy/connect-chatgpt).

## Claude and Claude Desktop

1. Open **Customize → Connectors → Add → Add custom connector**. Team/Enterprise owners first add it under **Organization settings → Connectors → Add → Custom → Web**.
2. Enter the shared name and URL. Choose **Sign in now** and **Register automatically** if the OAuth client option appears. Leave fixed request headers empty.
3. Sign in to Contech, connect/enable the connector and test in a new conversation.

Availability also depends on plan and organisation. [Anthropic's guide](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp). Do not configure this remote service as a local desktop stdio server.

## Claude Code

With Claude Code installed, run:

```sh
claude mcp add --transport http --scope user contech-editorial https://www.contechnorway.com/api/mcp
```

Open Claude Code, run `/mcp`, select the server and finish browser sign-in. Then test the connection. This adds MCP tools, not the Codex plugin. [Claude Code documentation](https://code.claude.com/docs/en/mcp).

## GitHub Copilot in VS Code

1. Open VS Code with GitHub Copilot available. Open the Command Palette using `Ctrl+Shift+P` on Windows/Linux or `Cmd+Shift+P` on Mac.
2. Run **MCP: Add Server**, choose **HTTP**, enter the shared URL and server ID `contech-editorial`.
3. Select personal/global configuration (possibly **Global → Copilot Global**). Trust the server and complete Contech sign-in.
4. Open a Copilot agent conversation, check tool availability and run the test.

If options are missing, check VS Code version and organisational MCP policies. [Microsoft's guide](https://code.visualstudio.com/docs/agent-customization/mcp-servers).

## Test before uploading

With the connection selected, send:

> Use Contech Editorial. Fetch get_editorial_guide and summarise its rules. Do not create or change content.

Check that an actual tool call succeeds. An answer copied from this guide is not a connection test. This read call alone does not prove write access or successful video upload.

## Skills and your first video

The Codex plugin includes `editorial-drafts`. Direct MCP setup in other clients does not install that skill automatically. All clients can retrieve the server's editorial guide; use the prompt below to make the workflow explicit. [Read the complete skill](../plugins/contech-editorial/skills/editorial-drafts/SKILL.md).

> Read get_editorial_guide first. Use my chosen video about [subject], spoken in [language]. Create a private video draft, upload it, retrieve the transcript when ready, and draft a public pitch in Norwegian Bokmål and British English plus a member summary in the original language. Do not create an article yet. Ask me to upload through browser_url if direct transfer is unavailable. Validate the draft and return its editor link. Do not publish or mark anything as reviewed by a human.

Replace the bracketed values. In web clients, normally use the upload link: attaching a video to chat does not automatically upload it to Contech. For an existing video, provide its draft link and ask the agent to continue there.

Check names, facts, both languages, video and captions on the website before publishing. Public pitches and published articles are accessible to everyone; videos and member summaries require membership. Do not copy private summaries into public articles without agreement.

## Troubleshooting

- **No tools:** enable the connection and try a new conversation.
- **Sign-in/access denied:** check your account, invitation, role and membership on the website. Reconnect if necessary.
- **OAuth client ID/secret required:** stop and get help; do not use database or server keys. Automatic client registration is the intended setup.
- **File transfer unavailable:** request `browser_url` and upload there.
- **Transcript pending:** wait and check status later; do not recreate the same video.
- **Save conflict:** ask the agent to retrieve the latest draft before retrying.

Send your Contech contact the error and client name without sign-in codes or tokens. Revoke access under [agent access](https://www.contechnorway.com/redaktor/agenter).

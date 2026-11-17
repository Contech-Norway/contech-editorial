# Update Contech Editorial and sign in again

[Norsk](oppdatering.md) · Checked against the local Codex CLI on 17 November 2026.

Plugin files are installed separately on each Mac. Updating one computer does not automatically update another. OAuth sign-in is separate: installing skills does not grant additional permissions.

## 1. Locate the Codex command

Try `codex --version`. If Terminal reports `command not found`, the app may still bundle the CLI. The verified path on the tested Mac was:

```sh
"/Applications/ChatGPT.app/Contents/Resources/codex-cli/CodexCLI.app/Contents/MacOS/codex" --version
```

This path is installation-specific. If absent, follow the [official CLI installation guide](https://developers.openai.com/codex/cli/). Do not uninstall the app to fix the search path. In commands below, replace only `codex` with the quoted full path if necessary.

## 2. Check this Mac’s marketplace source

```sh
codex plugin marketplace list
```

For a Git source pointing to the public repository:

```sh
codex plugin marketplace upgrade contech-norway
codex plugin add contech-editorial@contech-norway
```

For a **local directory**, such as a website checkout, marketplace upgrade does not update its files. Update that checkout first, then run `codex plugin add contech-editorial@contech-norway`. Do not add another source with the same name without resolving the source change.

If no contech-norway marketplace exists:

```sh
codex plugin marketplace add https://github.com/Contech-Norway/contech-editorial.git --ref main
codex plugin add contech-editorial@contech-norway
```

Check the installation result: this release should end in `contech-editorial/0.4.0`. Restart the app and open a new chat. The package includes `editorial-drafts`, `sprint-preparation` and `sprint-review`.

## 3. Renew authentication for sprints and comments

[Installation and sign-in help](https://www.contechnorway.com/en/connect-ai). Version 0.4.0 requires the new server get_connection_status tool. After deployment, revoked tokens receive an OAuth challenge; automatic sign-in behaviour still needs client verification. Agent access distinguishes approved access from last observed MCP contact.

Codex screenshots from 17 November show URL and header settings but no sign-in button. Previous advice to find that button was imprecise. For the verified separately configured server named contech-editorial, use the CLI fallback below. Do not uninstall the plugin or enter tokens.

Do this when an old connection lacks the new permissions or has been revoked, not for every routine plugin update.

1. Open [Agent access](https://www.contechnorway.com/redaktor/agenter) and revoke the relevant old grant unless already done. Other clients sharing that same grant may be affected; do not assume revocation is limited to one Mac.
2. Try a read-only MCP call in Codex and follow a sign-in prompt if offered. The inspected MCP settings page has no sign-in button. If no prompt appears and the server is separately configured, use the CLI fallback below. Do not configure an extra MCP server alongside the plugin.
3. Sign in through the browser with your personal Contech account. Read the consent: comments are member-visible, sprint progress is private, and editorial publication happens on the website.
4. Approve **Tillat agenttilgang og kommentarer / Allow agent access and comments** on the Contech page. Return to the app and open a new chat with the plugin.
5. Send:

> Fetch get_connection_status and check my access. Do not create or change content.

The test must actually call the MCP tool and check whether comment access is enabled. A plugin version or a paraphrase of the guide does not establish OAuth connectivity.

If sign-in is not offered, share a screenshot of the plugin detail page and the error, without codes, tokens or callback URLs. Automated access to the Codex window was blocked, so its sign-in control has not been visually verified and no fabricated app screenshots are included.

### Only for a separately configured CLI MCP server

`codex mcp login` and `logout` exist, but must not be assumed to manage app-plugin connections. For a separate server, find its real name with `codex mcp list`, then use `codex mcp logout NAME` and `codex mcp login NAME`. Do not use this as a second installation method for the plugin.

## Verified sign-in test, 17 November 2026

Tested with Codex CLI `0.165.0-alpha.1.2` and a separate production test connection: consent → successful `get_connection_status` → revocation → rejected status call → normal `codex mcp login` with fresh consent → successful status call. No content was changed.

The Codex client did not start sign-in automatically in this test; it reported an OAuth token refresh error. Normal CLI sign-in with a local callback listener worked. Avoid `--no-browser` for this walkthrough: that mode requires manual callback handling, and one attempt timed out during testing. Use a browser on the same computer as the CLI and leave the login command running until it reports success.

This verifies the fallback for a separately configured MCP server. Automatic desktop-plugin sign-in remains unverified. Codex registered a new OAuth client during renewed sign-in; the old test grant remained revoked.

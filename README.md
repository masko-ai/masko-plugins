# Masko workflow skills and Cursor plugin

Create a character for your product, animate your own artwork, and put the
finished mascot into your app or website.

Use these four skills with a connected Masko MCP. The repository also includes a
Cursor plugin that bundles the skills and connection configuration.

| Skill | Use it to |
| --- | --- |
| `create-mascot` | Create a mascot identity and consistent poses |
| `animate-mascot` | Animate an existing character or an imported image |
| `build-interactive-mascot` | Build states and reactions to product events |
| `integrate-mascot` | Add assets and smooth animation switching to your app |

## Install standalone skills in Cursor

Run this in the project where you want to use Masko:

```bash
npx skills add masko-ai/masko-plugins --agent cursor --skill '*' --yes
```

This installs all four workflow skills for Cursor in that project. Add
`--global` to make them available across your Cursor projects. To install only
one, replace `--skill '*'` with `--skill create-mascot`, `--skill animate-mascot`,
`--skill build-interactive-mascot`, or `--skill integrate-mascot`.

Skills provide instructions. Configure and authorize the MCP connection
separately so the agent can use Masko's tools.

## Connect the Masko MCP

Add this server to Cursor's MCP settings:

```json
{
  "mcpServers": {
    "masko": {
      "url": "https://masko.ai/api/mcp",
      "auth": {
        "CLIENT_ID": "https://masko.ai/oauth/clients/cursor",
        "scopes": ["masko:read", "masko:write"]
      }
    }
  }
}
```

Connect through Cursor's OAuth flow. Sign in on masko.ai, choose your personal or
team workspace, and approve access. The client ID is public configuration for
Cursor; it is not a secret. No API key or separate MCP process is needed.

The [Cursor setup guide](https://masko.ai/docs/ai-tools/cursor) covers the
connection and troubleshooting. For another host, use that host's supported
Masko OAuth connection. Installing these skills into an agent does not establish
that its MCP connection is compatible. Keep the Cursor client configuration in
Cursor.

## Verify the connection

Open or reload the project in Cursor, then check **Customize → Skills** for the
four Masko skills. See [Cursor's skill documentation](https://cursor.com/docs/skills)
for skill discovery and invocation.

Ask: **“Show my Masko projects, mascots and credit balance. Don't generate anything.”**

A working connection returns results from the workspace chosen during OAuth,
including an empty project list when the workspace is new. Complete the
connection before asking for generation. This check does not create images or
animations and uses no generation credits.

Disconnect in Masko's [Connected apps](https://masko.ai/settings/connections)
when you choose to.

## Install the Cursor plugin

For a local Cursor plugin installation, copy this entire directory, including
`.cursor-plugin`, into `~/.cursor/plugins/local/masko`. Restart Cursor or run
**Developer: Reload Window**, then open **Customize** and enable Masko.
Your organization's policy may restrict local plugin imports. This repository
does not imply that the plugin has been accepted into the Cursor Marketplace.

The plugin includes the MCP configuration above. Connect when Cursor prompts
you, then run the same connection check.

## Try it

- “Create a friendly fox mascot for my fitness app. Show the cost before generating.”
- “Animate my existing mascot waving hello. Preserve its design.”
- “Make my mascot celebrate a completed lesson, then return to idle.”
- “Integrate my completed animations into this React project with smooth transitions.”

Generation uses the connected workspace's Masko credits. Cursor subscriptions
do not include these credits. The agent explains the cost before generation;
buy additional credits on Masko or use the checkout tool to open Stripe when
you request a purchase. The plugin does not automatically pay for you.

## Images and delivery

Reuse images already in Masko, import a user-selected HTTPS image URL, or use a
downloadable attachment when the host supplies one. Imports support PNG, JPEG,
WebP and GIF up to 10 MB. A local path or a Cursor chat image is not necessarily
a downloadable attachment. For a local-only image, upload it on masko.ai first,
then ask Cursor to use the saved asset. The plugin does not expose local files
through a public host automatically.

New mascots start private. Preview links expire. Publish assets only when you
want public URLs, or keep private delivery behind your application's backend.
Never embed Masko authoring credentials in frontend code. Animations and exports
finish asynchronously; a queued job is not yet a finished asset.

Integration skills cover direct HTML, React, Swift/SwiftUI and Android playback.
They retrieve the current Masko guides, choose available transparent formats,
and use two persistent players when changing clips. They do not install
`@masko/sdk`, deploy your application, or control the Masko Desktop companion.

## Troubleshooting and support

If tools are missing, enable the MCP server in Cursor, complete authorization,
and retry discovery. Installing a skill alone does not connect the account.
Do not paste credentials into chat. Use the OAuth connection flow.

[Documentation](https://masko.ai/docs/ai-tools/cursor) ·
[Support](mailto:paul@masko.ai) ·
[Privacy](https://masko.ai/privacy) · [Terms](https://masko.ai/terms)

The MIT license covers this plugin's configuration and workflow instructions.
Masko logos and trademarks remain the property of their respective owner.
The hosted Masko service is subject to its own terms.

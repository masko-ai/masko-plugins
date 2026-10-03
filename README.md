# Masko for Cursor

Create a character for your product, animate your own artwork, and put the
finished mascot into your app or website.

This plugin connects Cursor to Masko's hosted MCP server and includes four skills:

| Skill | Use it to |
| --- | --- |
| `create-mascot` | Create a mascot identity and consistent poses |
| `animate-mascot` | Animate an existing character or an imported image |
| `build-interactive-mascot` | Build states and reactions to product events |
| `integrate-mascot` | Add assets and smooth animation switching to your app |

## Install and connect

For a local installation, copy this entire plugin directory, including
`.cursor-plugin`, into `~/.cursor/plugins/local/masko`. Restart Cursor or run
**Developer: Reload Window**, then open **Customize** and enable Masko.
Your organization's policy may restrict local plugin imports. This repository
does not imply that the plugin has been accepted into the Cursor Marketplace.

Connect the Masko MCP server when Cursor prompts you. Sign in on masko.ai,
choose your personal or team workspace, and approve its permissions. The public
client ID in `mcp.json` identifies this integration; it is not a secret. No API
key, terminal login, or separate MCP process is needed.

Start with: **“Show my Masko projects, mascots and credit balance. Don't generate anything.”**
The results come from the workspace chosen during connection. Disconnect in
Masko's [Connected apps](https://masko.ai/settings/connections) at any time.

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

# Masko Studio for OpenClaw

Create a mascot for your product, animate it, make it react to what happens in
your app, and put it in your website or app with the open-source Masko player.

This plugin adds five workflow skills and the connection to Masko's hosted MCP
server at `https://masko.ai/api/mcp`. It contains no code.

| Skill | Use it to |
| --- | --- |
| `create-mascot` | Make a mascot from pictures or a description, and its poses and backgrounds |
| `animate-mascot` | Animate a pose, add sound, give the mascot a voice and make it talk |
| `build-interactive-mascot` | Build a canvas so the mascot reacts to events in an app or website |
| `integrate-mascot` | Play the mascot with the Masko player on the web, iPhone, Mac, Android, React Native or Flutter |
| `desktop-pet` | Make a desktop pet for Mac or Windows from any Masko mascot |

## Sign in

Masko signs you in through your browser, so no key is stored anywhere. After
installing the plugin:

```bash
openclaw mcp set masko '{"url":"https://masko.ai/api/mcp","transport":"streamable-http","auth":"oauth"}'
openclaw mcp login masko
openclaw gateway restart
```

Open the link `login` prints, choose the workspace whose credits OpenClaw will
use and click **Connect Masko**. [Full OpenClaw guide](https://masko.ai/docs/ai-tools/openclaw).

## Try it

- "Show my Masko credit balance. Don't generate anything."
- "Create a friendly owl mascot for my study app. Tell me the cost before generating."
- "Animate the owl waving, then make it react when a lesson is finished."

## What to know before installing

- Every tool call goes to masko.ai. Prompts, the pictures you choose to import
  and the files Masko makes are stored in your Masko workspace.
- Generation spends the credits of the workspace you connect. The tools give
  the price before anything is spent, and reading, listing and checking jobs
  are free. No tool takes a payment; credits are bought at
  https://masko.ai/billing.
- The sign-in token is stored by OpenClaw, never by this plugin. Disconnect at
  any time in Masko's [Connected apps](https://masko.ai/settings/connections).
- The tool list is served by Masko's server when OpenClaw connects, so it can
  change without a new version of this plugin.

[Documentation](https://masko.ai/docs/mcp) · [Support](mailto:paul@masko.ai) ·
[Privacy](https://masko.ai/privacy) · [Terms](https://masko.ai/terms)

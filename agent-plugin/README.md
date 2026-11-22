# Masko for Hermes Agent and OpenClaw

Create a mascot for your product, animate it, make it react to what happens in
your app, and put it in your website or app with the open-source Masko player.

This folder is an [Agent Plugins 1.0](https://agent-plugins.org) package: five
workflow skills and the connection to Masko's hosted MCP server at
`https://masko.ai/api/mcp`. It contains no code.

| Skill | Use it to |
| --- | --- |
| `create-mascot` | Make a mascot from pictures or a description, and its poses and backgrounds |
| `animate-mascot` | Animate a pose, add sound, give the mascot a voice and make it talk |
| `build-interactive-mascot` | Build a canvas so the mascot reacts to events in an app or website |
| `integrate-mascot` | Play the mascot with the Masko player on the web, iPhone, Mac, Android, React Native or Flutter |
| `desktop-pet` | Make a desktop pet for Mac or Windows from any Masko mascot |

## Hermes Agent

```bash
hermes plugins install masko-studio
hermes plugins enable masko-studio
```

Sign in to Masko, so no key is stored anywhere. Add the server with OAuth to
`~/.hermes/config.yaml`:

```yaml
mcp_servers:
  masko:
    url: "https://masko.ai/api/mcp"
    auth: oauth
```

Then run `hermes mcp login masko`. Your browser opens masko.ai: choose the
workspace whose credits Hermes will use and click **Connect Masko**. Start a
new Hermes session. The skills appear as `masko-studio:create-mascot` and so
on. This package is separate from the `masko` plugin that Masko for Mac
installs to show Hermes events on your mascot; both can be on together.

The package marks the server `untrusted`, so Hermes asks before every tool that
creates something or spends credits. Tools that only read are marked read-only
and run without asking. [Full Hermes guide](https://masko.ai/docs/ai-tools/hermes).

## OpenClaw

The five skills are on ClawHub as `masko-create-mascot`, `masko-animate-mascot`,
`masko-build-interactive-mascot`, `masko-integrate-mascot` and
`masko-desktop-pet`:

```bash
clawhub install masko-create-mascot
```

Then connect Masko and sign in:

```bash
openclaw mcp set masko '{"url":"https://masko.ai/api/mcp","transport":"streamable-http","auth":"oauth"}'
openclaw mcp login masko
openclaw gateway restart
```

[Full OpenClaw guide](https://masko.ai/docs/ai-tools/openclaw).

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
- The sign-in token is stored by Hermes or OpenClaw, never by this package.
  Disconnect at any time in Masko's
  [Connected apps](https://masko.ai/settings/connections).
- The tool list is served by Masko's server when the agent connects, so it can
  change without a new version of this package.

[Documentation](https://masko.ai/docs/mcp) · [Support](mailto:paul@masko.ai) ·
[Privacy](https://masko.ai/privacy) · [Terms](https://masko.ai/terms)

---
name: build-interactive-mascot
description: Build or edit a Masko canvas so a mascot reacts to app events using states and animated transitions. Use for interactive behaviors, state machines, celebrations, idle loops, or playable mascot releases.
---

# Build an interactive mascot

These instructions require Masko's hosted MCP at https://masko.ai/api/mcp.
For Cursor, follow the [connection guide](https://masko.ai/docs/ai-tools/cursor),
sign in through OAuth, and choose the user's personal or team workspace.
Skills-only installation adds instructions; configure and authorize the MCP
separately in the host. Other hosts need their own supported Masko connection.
A safe first check is to list projects, mascots and credits without generating.

Use Masko's connected MCP tools for this workflow. Discover the MCP tools
before declaring them unavailable. If they are unavailable, or the host blocks
an action for approval, explain the exact limitation and stop the affected step.
Do not substitute the Masko Mac app, another account, or a direct API credential
to make an MCP operation appear successful. Do not change host permissions.

Establish the target platform and desired app events. Read `get_integration_guide`
for `canvas/build`, `canvas/generate-all`, and `player/canvas` before choosing
inputs. Play the release with the platform's Masko player (open source,
https://github.com/masko-ai/masko-player), which runs every canvas rule; never
write a player of your own:

- Web: `npm install @masko/player` (`mountMascot`; React: `@masko/player/react`).
- iPhone, iPad, Mac and visionOS: Swift package
  `https://github.com/masko-ai/masko-player` from `0.1.9`, product `MaskoPlayer`.
- React Native and Expo: `npm install @masko/player-react-native`.
- Android: `ai.masko:player` is not on Maven Central yet. Build it from the
  repository's `android/README.md`.

Do not install or recommend `@masko/sdk`.

The app drives the player with `set(name, value)` for held states and
`fire(name)` for one-time events. Translate app events into behavior/action
inputs the canvas declares or behavior families listed in `player/canvas`;
never invent player methods or unresolved graph inputs. The player does not
watch the pointer: the app sets `pointerHover` and fires `click` itself. Every
move starts when the loop on screen ends, unless the app sends the value with
`instant`. When several behaviors are on, the canvas's priorities decide which
one shows.

Reuse a project, mascot and canvas when appropriate. For new work,
`create_canvas` creates an empty graph. Fetch `get_canvas`, then `save_canvas`
with the latest `expected_graph_hash`. Preserve existing node/edge IDs and
generated media. A save replaces the complete graph structure. On a stale hash,
read the current graph and reconcile changes instead of overwriting them.

Compose the behavior the user requested: a sensible starting state, actions,
transition conditions and a return path. Any State priorities choose among
competing actions. Generation makes reverse edges free from their forward
animations; never point a reverse edge at its forward video. Do not
add a large default graph or generate unrequested behaviors.

For a new pose, give its node an `imagePrompt` and omit `assetId` until generation
assigns it. A node label alone does not schedule image generation. Check the
dry run's `planned_nodes`, `planned_edges` and `estimated_cost` against the
requested work before asking to spend credits.

Call `validate_canvas`. Correct invalid topology before generation. Use
`estimate_canvas_generation` with explicit model, duration and targets. Present
the estimate and obtain authorization for the spend. Execute `generate_canvas`
with the returned `plan_id` as `approved_plan_id`, `graph_content_hash` as
`expected_graph_hash`, identical options,
the approved `max_credits`, and a stable UUID `operation_id`.

Generate only missing work. Preserve operation IDs on retries; recover ambiguous
results with `list_jobs` and `get_canvas_status`. Do not edit the graph while its
generation is in progress. If later phases need additional work, obtain a fresh
estimate; authorization for the first phase does not imply unlimited spending.

Poll jobs and `get_canvas_status` until required assets and playable derivatives
are ready. Use `repair_canvas_media` with a dry run for missing WebM/HEVC exports;
do not regenerate performances to repair a delivery format. Structural validity
alone does not prove playback is complete.

Use `list_canvas_history` to find existing releases. Call `release_canvas` with
the current graph hash and a stable operation ID when the user wants a playable
release. A release does not publish a marketplace listing or activate Desktop.
Retrieve `get_release_delivery` for the supported target. In production the
app's backend fetches the release delivery with `target=web`, `target=apple` or
`target=android` and forwards only `data.delivery`; the player's `getDelivery`
asks the backend again before signed links expire. Never put Masko credentials
or OAuth tokens in client code. Verify the starting state, each requested
event, return transitions, and available media in the target application
before claiming the interaction works.

If credits are insufficient, stop paid work and direct the user to
https://masko.ai/billing to buy credits for the same personal or team workspace
chosen during connection. Only the team owner manages team billing. Do not
collect payment details or claim a purchase completed. Check the connected
balance again before resuming; never switch workspaces to find extra credits.

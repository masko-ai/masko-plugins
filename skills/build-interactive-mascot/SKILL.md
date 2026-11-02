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
A safe first check is `get_account` and `list_mascots`, which change and charge
nothing.

Use Masko's connected MCP tools for this workflow. Discover the MCP tools
before declaring them unavailable. If they are unavailable, or the host blocks
an action for approval, explain the exact limitation and stop the affected step.
Do not substitute the Masko Mac app, another account, or a direct API credential
to make an MCP operation appear successful. Do not change host permissions.

Establish the target platform and desired app events. Before choosing inputs,
read the docs pages `canvas/build`, `canvas/generate-all` and `player/canvas`:
with `read_docs` (`page: "canvas/build"`), or as `masko://docs/...` resources
in hosts that list them. Play the release with the platform's Masko player (open source,
https://github.com/masko-ai/masko-player), which runs every canvas rule; never
write a player of your own unless the user asks for another way:

- Web and Electron: `npm install @masko/player` (`mountMascot`; React:
  `@masko/player/react`), with `target=web`.
- iPhone, iPad, Mac and visionOS: Swift package
  `https://github.com/masko-ai/masko-player` from `0.1.12`, product `MaskoPlayer`;
  0.1.12 adds `MaskoPlayer(url:appKey:)`. `target=apple`.
- React Native and Expo: `npm install @masko/player-react-native`.
- Flutter: `flutter pub add masko_player`.
- Android: `ai.masko:player` from Maven Central. `target=android`.
- React Native and Flutter read `target=apple` on iOS and `target=android` on
  Android. `target=desktop` is only for Masko for Mac.

Do not install or recommend `@masko/sdk`.

The app drives the player with `set(name, value)` for held states and
`fire(name)` for one-time events; `get_release_delivery`'s `app_inputs` says
which each input takes. Translate app events into behavior/action
inputs the canvas declares or behavior families listed in `player/canvas`;
never invent player methods or unresolved graph inputs. The player does not
watch the pointer: the app sets `pointerHover` and fires `click` itself. Every
move starts when the loop on screen ends, unless the app sends the value with
`instant`. When several behaviors are on, the canvas's priorities decide which
one shows.

Reuse a project, mascot and canvas when appropriate; `get_mascot` lists a
mascot's canvases. For new work, `create_canvas` creates a canvas, empty or with
a first graph. Fetch `get_canvas`, then `save_canvas` with the latest
`expected_graph_hash`. Preserve existing node/edge IDs and
generated media. A save replaces the complete graph structure. On a stale hash,
read the current graph and reconcile changes instead of overwriting them.

Compose the behavior the user requested: a sensible starting state, actions,
transition conditions and a return path. Any State priorities choose among
competing actions. Generation makes reverse edges free from their forward
animations; never point a reverse edge at its forward video. Do not
add a large default graph or generate unrequested behaviors.

For a new pose, give its node an `imagePrompt` and omit `assetId` until generation
assigns it. A node label alone does not schedule image generation. Check the
estimate's `planned_nodes`, `planned_edges` and `cost_credits` against the
requested work before asking to spend credits. Write each edge's description
as one action in plain words, with how big, how fast and the mood; never
timings or the mascot's look.

`create_canvas`, `get_canvas` and `save_canvas` return the validation. Correct
invalid topology before generation. Use `estimate_canvas_generation` with an
explicit quality, duration and targets. Each clip keeps the `duration` saved on
its edge, and its edge's `generationModel` (standard or premium) when one is
set; the estimate's quality and duration apply to edges without valid ones, so
set an edge's own length where a clip needs a different one. Present the
estimate and obtain authorization for the spend. Execute `generate_canvas` with
the returned `plan_id`, the approved `max_credits` and a stable `operation_id`.

Generate only missing work. Preserve operation IDs on retries; recover ambiguous
results with `get_job`, the running work in `get_account`, and `get_canvas`. Do not edit the graph while its
generation is in progress. If later phases need additional work, obtain a fresh
estimate; authorization for the first phase does not imply unlimited spending.

Follow the jobs with `get_job`, and read `get_canvas` until its media is ready.
`generate_canvas` repairs missing WebM/HEVC playback files of existing clips for
free; do not regenerate performances to repair a delivery format. Structural
validity alone does not prove playback is complete.

`get_canvas` lists the 10 newest releases. Call `release_canvas` with
the current graph hash and a stable operation ID when the user wants a playable
release. A release does not publish a marketplace listing or activate Desktop.
Retrieve `get_release_delivery` for the target: it returns the player, setup
code and the request a shipped app makes. Ship the canvas with an app key from
`create_app_key`, created once the user agrees: it can only read that mascot's
released canvases, so it is safe to ship, and the canvas needs no publishing.
Never put the account key or OAuth tokens in client code. On iPhone, iPad and
Mac, `MaskoPlayer(url:appKey:)` reads new releases itself; other apps make the
request with the key and give the player a `getDelivery` that repeats it before
signed links expire. On a website or in Electron, the web player (0.1.13 and
later) does it from `url` and `appKey`, straight from the page; the API refuses
a page that sends the account API key.
Verify the starting state, each requested
event, return transitions, and available media in the target application
before claiming the interaction works.

If credits are insufficient, stop paid work and tell the user how many credits
the job needs and how many the connected workspace has; they add credits at
https://masko.ai/billing, since no tool takes payments. Never collect payment
details or claim a purchase completed. Recheck the connected balance before
resuming; never switch workspaces to find extra credits.

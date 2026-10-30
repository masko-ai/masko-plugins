---
name: integrate-mascot
description: Integrate completed Masko images, animations or interactive releases into the user's website or app. Use for installing the Masko player on the web, React, Swift, React Native or Android, and for single-clip playback.
---

# Integrate a mascot

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

Use the platform already established by the user or repository. Otherwise ask
whether the target is a website, React app, macOS app, iOS app or another target.
Use `get_integration_guide` with `topic: "player"` before writing playback code,
then `player/one-animation` or `player/canvas` for the case at hand. Do not
install or recommend `@masko/sdk` for this integration.

Retrieve existing results with `get_asset`, `list_assets`, `get_mascot_cdn`, or
`get_release_delivery`; do not regenerate a character merely to integrate it.
Use `export_asset` only for missing required formats and wait for its result.

Always play the mascot with the open-source Masko player
(https://github.com/masko-ai/masko-player); never write a player of your own:

- One unchanged looping animation: give the player the animation's files in
  place of a delivery: `mountMascot(element, { clip: { webm, hevc } })`,
  `<MaskoPlayer clip={{ webm, hevc }} />`, `MaskoPlayer(clip: hevcURL)` on Apple,
  `view.loadClip(stackedUrl)` on Android, `<MaskoView clip={{ hevc, stacked }} />` in
  React Native. It picks the file the platform shows with transparency and loops it.
- An interactive release, or clips that change with app events: clips that
  change belong in a canvas release; use `build-interactive-mascot` when there
  is none yet. Apps fetch the delivery with `formats=av1` (the player's
  `formats`) for the lighter stacked AV1 files.
- Install: web `npm install @masko/player`, then `mountMascot(element, { delivery, getDelivery })`;
  React `<MaskoPlayer ref delivery getDelivery />` from `@masko/player/react`; no
  build step `https://cdn.jsdelivr.net/npm/@masko/player@0.1/dist/masko-player.js`
  (defines `window.MaskoPlayer.mount`). iPhone, iPad, Mac and visionOS: Swift
  package `https://github.com/masko-ai/masko-player` from `0.1.9`, product
  `MaskoPlayer`. React Native and Expo: `npm install @masko/player-react-native`,
  then rebuild the native app. Android: `ai.masko:player` is not on Maven
  Central yet; build it from the repository's `android/README.md`.

Wire the Masko player to app events with `set(name, value)` for held states and
`fire(name)` for one-time events, using inputs the canvas declares or behavior
families listed in the guide; never invent methods or input names. The player
does not watch the pointer, clicks or taps: the app sets `pointerHover` and
fires `click` itself, and sets `heldContact`, `heldShake` and `falling` only if
it implements dragging. Every move begins when the loop on screen ends, unless
the app sends the value with `instant`; the canvas's priorities decide which
behavior shows. `preview.showPose` and `preview.playMove` are for editors, not
apps. Clips' own voices play by default; `sound: false` (`setSound(false)` on
the web) mutes them. Browsers allow sound only after the person's first gesture
on the page. Listen to `error` events and report them.

In every case, verify idle → action → idle, rapid requests, slow/failing loads,
and transparency over light/dark backgrounds. Do not claim visual parity with
Desktop or Life from code generation alone.

Production private-release delivery belongs behind the user's authorized backend.
It calls `GET https://api.masko.ai/v1/canvases/{canvasId}/releases/{releaseId}/delivery`
with `target=web`, `target=apple` or `target=android` for the platform and
forwards only `data.delivery` to the app; `get_release_delivery` is for checks.
Never put Masko authoring credentials or OAuth tokens in client code. Signed URLs
expire: pass the player a `getDelivery` function that asks the backend again,
rather than hard-coding temporary links.
For permanent public CDN URLs, explain what becomes public and use
`publish_mascot_assets` only when the user requests or approves public sharing.

Write the integration in the user's existing project style. Map requested app
events to documented behavior/actions, then run relevant build and playback
checks when the environment permits. State which platform and behaviors were
actually tested. Do not claim to have installed, deployed or visually verified
an integration based only on generated code.

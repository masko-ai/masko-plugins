---
name: integrate-mascot
description: Integrate completed Masko images, animations or interactive releases into the user's website or app. Use for installing the Masko player on the web, React, Electron, Swift, React Native, Flutter or Android, and for single-clip playback.
---

# Integrate a mascot

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

Use the platform already established by the user or repository. Otherwise ask
whether the target is a website, React app, macOS app, iOS app or another target.
Before writing playback code, read the docs page `player`, then
`player/one-animation` or `player/canvas` for the case at hand: with
`read_docs` (`page: "player"`), or as the `masko://docs/player` resources in
hosts that list them. Do not install or recommend `@masko/sdk` for this
integration.

Retrieve existing results with `list_assets` (by mascot, or by asset IDs),
`get_job` or `get_release_delivery`; do not regenerate a character merely to
integrate it.
Use `export_asset` only for missing required formats and wait for its result.

File links from `get_job` and `list_assets` are private and stop working within
hours: never put them in code. For pictures and single animations in a website
or app, offer `publish_mascot_assets`: once the user approves public sharing,
every finished file of the mascot gets a permanent public link on
assets.masko.ai, and files made afterwards get one too. Explain what becomes
public first. `get_mascot`'s hosting says whether the mascot is public already.
Use those assets.masko.ai links in the code.

Always play the mascot with the open-source Masko player
(https://github.com/masko-ai/masko-player); never write a player of your own
unless the user asks for another way:

- One unchanged looping animation: give the player the animation's public links
  in place of a delivery (`webm` and `hevc` on the web, `stacked_video_av1` or
  `hevc` on Apple, `stacked_video_av1` on Android 10 and later, else
  `stacked_video`):
  `mountMascot(element, { clip: { webm, hevc } })`,
  `<MaskoPlayer clip={{ webm, hevc }} />`, `MaskoPlayer(clip: hevcURL)` on Apple,
  `view.loadClip(stackedUrl)` on Android, `<MaskoView clip={{ hevc, stacked }} />` in
  React Native, `MaskoView(clip: MaskoClipFiles(hevc: hevc, stacked: stacked))` in
  Flutter. It picks the file the platform shows with transparency and loops it.
- An interactive release, or clips that change with app events: clips that
  change belong in a canvas release; use `build-interactive-mascot` when there
  is none yet. Apps fetch the delivery with `formats=av1` (the player's
  `formats`) for the lighter stacked AV1 files.
- Install: web and Electron `npm install @masko/player`, then
  `mountMascot(element, { delivery, getDelivery })`; React
  `<MaskoPlayer ref delivery getDelivery />` from `@masko/player/react`; no
  build step `https://cdn.jsdelivr.net/npm/@masko/player@0.1/dist/masko-player.js`
  (defines `window.MaskoPlayer.mount`). iPhone, iPad, Mac and visionOS: Swift
  package `https://github.com/masko-ai/masko-player` from `0.1.12`, product
  `MaskoPlayer` (0.1.12 adds `MaskoPlayer(url:appKey:)`). React Native and Expo:
  `npm install @masko/player-react-native`, then rebuild the native app.
  Flutter: `flutter pub add masko_player`. Android: `ai.masko:player` from Maven
  Central.

Wire the Masko player to app events with `set(name, value)` for held states and
`fire(name)` for one-time events, using inputs the canvas declares or behavior
families listed in the docs page `player/canvas`; never invent methods or
input names.
`get_release_delivery`'s `app_inputs` says which to set and which to fire: a
reaction the release plays once is fired, never held on. The player
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

Ship a canvas with an app key of the mascot, never the account key or OAuth
tokens: an app key can only read that mascot's released canvases, so it is safe
to ship. Once the user agrees, create one with `create_app_key` (it is shown
once: keep it out of source control) and read the docs page `player/app-keys`.
`get_release_delivery` returns the request the app makes in `from_an_app` and
setup code in `code` (with `react_native_code` and `flutter_code` for the apple
and android targets); use it rather than guessing the player API. The canvas
needs no publishing.

- iPhone, iPad and Mac: `MaskoPlayer(url: <canvas delivery URL>, appKey: <app key>)`
  (player 0.1.12 and later) makes the request, keeps the files and picks up new
  releases itself.
- Android, React Native and Flutter: the app makes the request with the key and
  passes the player a `getDelivery` that repeats it before the signed links
  expire. React Native and Flutter ask for `target=apple` on iOS and
  `target=android` on Android, never `web`.
- A website or Electron: the web player (0.1.13 and later) does it from the
  canvas URL and the key, straight from the page, with no server:
  `mountMascot(element, { url, appKey })`, or
  `<MaskoPlayer url={url} appKey={appKey} />` in React. The API refuses a page
  that sends the account API key.

Write the integration in the user's existing project style. Map requested app
events to documented behavior/actions, then run relevant build and playback
checks when the environment permits. State which platform and behaviors were
actually tested. Do not claim to have installed, deployed or visually verified
an integration based only on generated code.

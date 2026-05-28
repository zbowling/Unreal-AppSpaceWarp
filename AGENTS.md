# Agent Instructions — Unreal App SpaceWarp

Unreal sample demonstrating Application SpaceWarp (ASW) on Quest. Five scenes deliberately surface common ASW artifact classes (opaque-object disocclusion, parallel-motion "railing," transparency, UI) plus a benchmark scene, each with mitigation guidance.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, per-scene artifact walkthrough, mitigation guidance
- `AppSpaceWarp.uproject` — Unreal engine version, plugins, target platforms
- `Config/` — project settings (rendering, Meta XR plugin toggles, Late Latching)
- `.gitattributes` — Git LFS (required for media and binary UAssets)
- `LICENSE` — license terms (MIT)

## Quest / Horizon-specific notes

- The README's "recommended path" is the Meta fork of Unreal Engine (`Oculus-VR/UnrealEngine`) built from source. The Epic Launcher + MetaXR plugin path also works but lags Meta's latest integrations. Meta-fork build is Windows-only in practice.
- In-headset, the menu button (☰) opens the Scene Select menu with the App SpaceWarp toggle and controller batons for manual ASW testing — there is no editor-side scene picker.
- Transparent objects appearing to stutter under ASW is expected (covered in the Transparent Objects section), not a bug.
- Transparent materials and 3D widgets must opt in to "Output depth and velocity" to participate correctly in ASW; the broken-on-purpose scenes show what happens when they don't. For overlay UI, Compositor Layers are the recommended fix; for in-scene UI, enable `support depth` on the compositor layer.
- Late Latching is enabled project-wide as part of the ASW configuration; disabling it changes perceived head-motion latency, not just performance numbers.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unreal answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unreal-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.

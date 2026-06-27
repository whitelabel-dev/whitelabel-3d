# AGENTS.md — entry point for AI agents

**You are an AI agent consulting this repo. Read this file first.**

## what this repo is

The **world-model layer** of the whitelabel.dev ecosystem. NOT a Three.js viewer (that's how it was initially scoped on 2026-06-24). NOT a product configurator. It's the spatial substrate every other surface queries to understand physical reality.

**Status: thesis captured 2026-06-26, implementation pending.** v0.1 = the reframe. Don't generate stub code here without confirmation; the scope is foundational and changes here need Garrett's sign-off.

## why this matters more than it looks

Read [`whitelabel-principles/doctrines/proactive-ai.md`](https://github.com/whitelabel-dev/whitelabel-principles) first. The world model is the *eyes* of proactive AI. Without it, every "smart" feature in the ecosystem degrades to chat-bot reactive AI. With it, the entire ecosystem becomes spatial — voice resolves to objects, robots navigate, dashboards become 3D scenes, accessibility surfaces describe the world to assistive users.

This is the substrate. Treat it that way.

## what to do if asked to work in this repo

1. Re-read this AGENTS.md + the README architecture
2. v0.2 work is a Three.js viewer that **loads a real glTF space** and renders it at `3d.whitelabel.dev`. Not a product demo — the substrate.
3. v0.3 is the schema design (`spaces`, `objects`, `embodiments`, `spatial_relationships` in whitelabel-database)
4. Capture-side tools (LiDAR ingestion, photogrammetry, CAD import) come BEFORE rendering polish. Without real data, the viewer is a demo.

## non-negotiables

1. **glTF + USDZ are the storage formats.** Industry standard. Apple Quick Look / WebXR / every 3D toolchain reads them. Don't invent a custom format.
2. **The world model is queryable by LLMs.** A spatial query API (`world.find("printer near conference room")`) needs to exist by v0.6. If LLMs can't introspect the world model, agents can't reason about it.
3. **Live > static.** A scanned 3D model that doesn't update with sensor data is just a screensaver. The model must be a living document with IoT placements that tick in real time. See [whitelabel-house-manager](https://github.com/whitelabel-dev/whitelabel-house-manager) for the dashboard-as-3D-scene pattern.

## where the big picture lives

- Doctrine: [`whitelabel-principles/doctrines/proactive-ai.md`](https://github.com/whitelabel-dev/whitelabel-principles)
- Ecosystem: [whitelabel-ecosystem](https://github.com/whitelabel-dev/whitelabel-ecosystem)
- Consumers: [house-manager](https://github.com/whitelabel-dev/whitelabel-house-manager), [office-manager](https://github.com/whitelabel-dev/whitelabel-office-manager), [robotics](https://github.com/whitelabel-dev/whitelabel-robotics), [flow](https://github.com/whitelabel-dev/whitelabel-flow), [accessibility](https://github.com/whitelabel-dev/whitelabel-accessibility), [goggles](https://github.com/whitelabel-dev/whitelabel-goggles)
- Storage: [whitelabel-database](https://github.com/whitelabel-dev/whitelabel-database)

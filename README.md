# whitelabel-3d

> **For AI to understand your life & business, it needs to be able to see them.**
>
> `whitelabel-3d` is the *world-model* layer of the whitelabel.dev ecosystem — the spatial representation of physical reality that AI agents, humanoid robots, voice commands, and customer-facing visualization all share. Not a 3D viewer. A substrate.

## status

**Reframed 2026-06-26.** Initially scoped (2026-06-24) as a Three.js wrapper for product configurators. The real scope is the world-model layer for the entire ecosystem. Implementation pending; thesis captured here.

## the thesis

Current AI sees data: text, tables, code, single images of static moments. That gets you a smart assistant. It doesn't get you something that **understands** your life or business.

Understanding requires seeing:
- **Where things are** spatially (the printer is in the conference room, near the door, two desks from the window)
- **How things change** over time (lights came on at 6:14 PM, AC kicked off at 7:22 PM)
- **What's connected to what** (this vendor services that system, this employee uses that desk, this product fits inside that case)
- **What looks normal vs. what looks wrong** (anomalies in physical state)
- **How a body would move through the space** (humanoid robots, accessible navigation, emergency planning)

A 3D world model — built once per physical space + per product, kept live by sensor data + agent updates — is what lets every AI surface in the ecosystem stop guessing and start understanding.

## five things that consume the world model

| Consumer | What it does with the model |
|---|---|
| **[whitelabel-house-manager](https://github.com/whitelabel-dev/whitelabel-house-manager)** | The home is a 3D model. Sensors overlay on it (HVAC vents pulsing with airflow, sprinkler zones flashing when active, cameras as cones-of-vision). The dashboard *is* the 3D scene. |
| **[whitelabel-office-manager](https://github.com/whitelabel-dev/whitelabel-office-manager)** | Office floor plan in 3D, every system + ticket + vendor placement spatially anchored. "Printer offline" highlights the actual printer in the actual room. |
| **[whitelabel-robotics](https://github.com/whitelabel-dev/whitelabel-robotics)** | Humanoid robots navigate using the world model. "Bring me water" needs to know where water is, where I am, what's between us. Same model powers dashboard + robot. |
| **[whitelabel-flow](https://github.com/whitelabel-dev/whitelabel-flow)** | Voice commands resolve spatially. "Turn off the kitchen lights" requires knowing what kitchen + what lights — that's the world model. |
| **[whitelabel-accessibility](https://github.com/whitelabel-dev/whitelabel-accessibility)** | Spatial description for visually-impaired users. The AI assistant can narrate the room ("desk to your left, doorway 3 paces ahead"). |

Plus: agent reasoning (orchestrator queries the world for state), customer-facing visualization (product configurators), training data for AI policies (synthetic data generated from world models), AR overlays (WebXR on phones / Vision Pro).

## three kinds of world models

### 1. Space models — homes, offices, sites

Built from:
- LiDAR scans (iPhone Pro + Polycam / Scaniverse / 3D Scanner App)
- Photogrammetry (multiple photos → mesh)
- Manual floor-plan import (Matterport, Magicplan)
- IoT placement metadata (each smart device's room + position)

Stored as glTF + USDZ + per-object metadata in [whitelabel-database](https://github.com/whitelabel-dev/whitelabel-database).

### 2. Object models — products, inventory, parts

Built from:
- Manufacturer-provided CAD (STEP, glTF)
- Photogrammetry of physical items
- AI-generated (DreamGaussian, Trellis, Genie)

Used by product configurators, e-commerce previews, robotics-grasp training, AR try-on.

### 3. Embodiment models — humans, animals, humanoid robots

Built from:
- Unitree / K-Scale / open MJCF robot models (already in [whitelabel-robotics](https://github.com/whitelabel-dev/whitelabel-robotics))
- Avatar systems (Ready Player Me, Reallusion)
- SMPL / SMPL-X human body models for accessibility simulation

Used by humanoid sim, accessibility evaluation ("can a wheelchair fit through this doorway?"), training data.

## architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  CONSUMERS                                                       │
│  house-manager · office-manager · robotics · flow ·             │
│  accessibility · orchestrator · brand product configurators     │
│  · agents · AR surfaces                                         │
└─────────────────────────────────────────────────────────────────┘
                    ↑ query / subscribe to
┌─────────────────────────────────────────────────────────────────┐
│  whitelabel-3d  — world-model API                                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │ Spaces       │  │ Objects      │  │ Embodiments  │           │
│  │ (homes/      │  │ (products,   │  │ (humans,     │           │
│  │  offices)    │  │  parts)      │  │  robots)     │           │
│  └──────────────┘  └──────────────┘  └──────────────┘           │
│  Storage: glTF + USDZ + metadata in whitelabel-database          │
│  Rendering: Three.js / BabylonJS / native USDZ AR                │
│  Spatial queries: LLM-callable ("where is the printer?")         │
└─────────────────────────────────────────────────────────────────┘
                    ↑ ingested from
┌─────────────────────────────────────────────────────────────────┐
│  CAPTURE                                                         │
│  LiDAR scans · photogrammetry · CAD imports · IoT placements    │
│  · manual floor plans · AI-generated assets                     │
└─────────────────────────────────────────────────────────────────┘
```

## why this is whitelabel-shaped

- **Reseller-channel flagship**: any agency reselling whitelabel.dev can offer "a 3D digital twin of your home / office / business." Hugely differentiated vs. "we built you a SaaS."
- **Accessibility moat**: visually-impaired users navigating a building by spoken spatial description. Wheelchair users planning routes. Cognitive-disability users having the AI describe surroundings. The world model is the *substrate of the assistive layer*.
- **Compounding moat**: every space scanned, every object modeled, every IoT device placed adds to a knowledge graph nobody else has. Cloud LLMs see text and images of the world; whitelabel.dev's agents see the world itself.

## roadmap

| Version | Adds | Status |
|---|---|---|
| **v0.1 (today)** | Repo scope reframed: "Three.js wrapper" → "world-model layer." Thesis + architecture documented. | ✓ shipped |
| v0.2 | Three.js viewer scaffold — load + render a glTF space model in the browser at `3d.whitelabel.dev` | next |
| v0.3 | Schema: `spaces`, `objects`, `embodiments`, `spatial_relationships` tables in whitelabel-database. Object placement metadata. | |
| v0.4 | First real space scan — Garrett's office via iPhone LiDAR → glTF → loaded in viewer | |
| v0.5 | Sensor overlay — light a thermostat pin in the 3D scene with live HVAC data via house-manager API | |
| v0.6 | Spatial query API — `world.find("printer near conference room")` returns 3D position + metadata | |
| v0.7 | Voice integration — Whitelabel Flow command resolves against world model | |
| v0.8 | Robot integration — Unitree G1 sim from whitelabel-robotics navigates a real-scanned space | |
| v0.9 | AR / WebXR — same model viewable on iPhone / Vision Pro with sensor overlays | |
| v1.0 | World-model API stable; consumed by 5+ ecosystem surfaces in production | |

## related repos

| Repo | Connection |
|---|---|
| [whitelabel-house-manager](https://github.com/whitelabel-dev/whitelabel-house-manager) | Uses world model as dashboard substrate |
| [whitelabel-office-manager](https://github.com/whitelabel-dev/whitelabel-office-manager) | Office floor plans + system placement |
| [whitelabel-robotics](https://github.com/whitelabel-dev/whitelabel-robotics) | Humanoid navigation + simulation share embodiment models |
| [whitelabel-flow](https://github.com/whitelabel-dev/whitelabel-flow) | Voice commands resolve spatially |
| [whitelabel-accessibility](https://github.com/whitelabel-dev/whitelabel-accessibility) | Spatial description for assistive use; navigability checks |
| [whitelabel-goggles](https://github.com/whitelabel-dev/whitelabel-goggles) | AR / VR viewer surface (placeholder) |
| [whitelabel-orchestrator](https://github.com/whitelabel-dev/whitelabel-orchestrator) | Agents query the world for spatial reasoning |
| [whitelabel-database](https://github.com/whitelabel-dev/whitelabel-database) | Persistence for all world-model records |
| [whitelabel-principles](https://github.com/whitelabel-dev/whitelabel-principles) | Doctrine: `doctrines/ai-needs-eyes.md` |

## the doctrine sentence

> For AI to understand your life & business, it needs to be able to see your life & business. The world model is the eyes.

That's the entire reframe.

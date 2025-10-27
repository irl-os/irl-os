# IRL OS — The Emergent Reality Runtime

> **From interfaces to intentions.**  
> IRL OS is an open, local-first runtime that turns the physical world into a programmable, privacy-preserving **spatial layer**.  
> It coordinates many small devices (ViiM³ cubes, ViiM I & II wearables, gateways, phones, edge PCs) into one adaptive network where intelligence **emerges** from real-time context—not from any single node.

<p align="center">
  <a href="https://viim.tech/irlos">Website</a> •
  <a href="https://docs.viim.tech">Docs (GitBook)</a> •
  <a href="#quickstart">Quickstart</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#roadmap">Roadmap</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#license">License</a>
</p>

---

## Why IRL OS?

- **Presence over packets** — Share compact, signed **signals**, not raw streams.  
- **Adaptive by design** — You set goals & guardrails; behavior adapts to live spatial, temporal, and social context.  
- **Local-first & private** — Process where data is born; collaborate across the mesh only when needed.  
- **Transport-agnostic** — LoRa/BLE/Wi-Fi for the mesh; optional **satellite uplink** (Iridium/Starlink) for critical bursts.  
- **Open stack** — Linux, the open web, and open models (e.g., Qwen/Llama/Mistral) with Coral Edge-TPU acceleration.

> IRL OS is the OS layer that powers the **ViiM³ (the Cubed Cube)** reference device and runs across ViiM I (monocle), ViiM II (glasses), edge gateways, and compatible hardware.

---

## What IRL OS is (and isn’t)

- **Is:** a portable **runtime + policy engine + SDKs** for spatial, event-driven apps that coordinate across many devices.  
- **Isn’t:** a monolithic phone/desktop OS or a forked kernel. IRL OS **runs on Linux** (and host platforms like visionOS via app/bridge layers) and unifies devices into a single **emergent system**.

---

## Supported targets

- **ViiM hardware:** ViiM³ Cube (reference), ViiM I (monocle), ViiM II (glasses)  
- **Edge & gateways:** Raspberry Pi / x86 Linux, Jetson, Coral-based boards  
- **Spatial platforms:** visionOS (via app runtime/bridge), Android, Web runtimes  
- **Transports:** LoRa/BLE/Wi-Fi, optional satellite uplink via ViiM Uplink Hub

> Platform notes and setup guides live in **/docs** (GitBook).

---

## Architecture


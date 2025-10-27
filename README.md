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
+------------------------------ Cloud (optional) -----------------------------+
| GitBook docs • App backends • Storage • Analytics • CI for policies/models |
+---------------------^---------------------^---------------------^-------------+
| | |
(Uplink, only when critical or explicit, via Sat/Cell/IP)
| | |
+---------------------+---------------------+---------------------+-------------+
| Hubs / Gateways |
| - IRL Hub: policy sync, attestation, OTA, mesh <-> internet bridge |
| - Satellite link (Iridium/Starlink) for burst alerts & summaries |
+---------------------^---------------------^---------------------^-------------+
| | |
LoRa / BLE / Wi-Fi mesh | Direct device links
| | |
+---------------------+---------- The IRL Mesh (local-first) -----------------+
| ViiM³ Cubes • ViiM I/II wearables • Phones • Edge PCs • Sensors • Actuators |
| └─ IRL Runtime: intent + policy engine + device coordination |
| └─ Edge AI: Coral TPU pipelines (vision/audio/sensor models) |
| └─ Identity: signed packets & firmware; optional DIDs/VCs |
+-------------------------------------------------------------------------------+

**Core components**
- **IRL Runtime** — event bus, intent/policy execution, time/space context, device coordination.  
- **IRL Agent** — per-device process for sensing, actuation, model execution, and transport.  
- **IRL Hub** — optional gateway: mesh ↔ internet/satellite, OTA, attestation, fleet ops.  
- **SDKs** — build spatial apps in JS/TS, Python, or Rust; publish/subscribe, policies, and device I/O.

---

## Quickstart

> Use the “Hello, Mesh” sample to see local-first signaling with no cloud required.

### 1) Run a local broker (for dev)
```bash
# Linux/macOS
docker run -it --name irl-broker -p 1883:1883 -p 9001:9001 eclipse-mosquitto

### 2) Hello, Mesh (Node.js)
mkdir hello-mesh && cd hello-mesh
npm init -y && npm i mqtt
cat > index.mjs <<'EOF'
import mqtt from "mqtt";

const deviceId = process.env.DEVICE_ID ?? "cube-001";
const client = mqtt.connect("mqtt://localhost:1883");

client.on("connect", () => {
  console.log(`[${deviceId}] connected`);
  // Publish a compact presence signal every 5s
  setInterval(() => {
    client.publish("irl/presence", JSON.stringify({
      id: deviceId,
      ts: Date.now(),
      zone: "lab-north",
      battery: 0.92
    }), { qos: 0 });
  }, 5000);

  // Subscribe to network decisions (e.g., escalate)
  client.subscribe("irl/decisions/#");
});

client.on("message", (topic, msg) => {
  console.log(`[${deviceId}] ${topic}: ${msg.toString()}`);
});
EOF
node index.mjs
```
### 3) (Optional) Hello, Mesh (Python)
```python
python -m pip install paho-mqtt
python - <<'PY'
import json, time, os
import paho.mqtt.client as mqtt

device_id = os.getenv("DEVICE_ID","cube-002")
c = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2)
c.connect("localhost", 1883, 60)

def on_message(client, userdata, msg):
    print(f"[{device_id}] {msg.topic}: {msg.payload.decode()}")
c.on_message = on_message
c.subscribe("irl/decisions/#")

while True:
    payload = {"id": device_id, "ts": int(time.time()*1000), "zone": "lab-north", "temp": 23.6}
    c.publish("irl/presence", json.dumps(payload))
    time.sleep(5)
```

This simulates two devices sharing **presence summaries**. Add another script that fuses context and publishes a decision on `irl/decisions/...` to see the pattern end-to-end.

> Full device guides (ViiM³ Cube, ViiM I/II, Pi, Jetson, Coral) live in **Docs → Getting Started**.

## Features

-   Intent & Policy Runtime — express goals and guardrails; behavior adapts to live context.  
      
    
-   Edge AI Pipelines — Coral Edge-TPU acceleration for vision/audio/sensor models.  
      
    
-   Identity & Trust — signed firmware & packets; optional decentralized attestations (DIDs/VCs).  
      
    
-   OTA & Fleet Ops — signed updates, health, remote diagnostics through hubs.  
      
    
-   Interoperability — MQTT/WebSockets/WebRTC, ROS 2/MAVLink/PX4 adapters, webhooks.  
      
    
-   Satellite-Ready — “Smart Wake & Burst” uplinks for critical events via Iridium/Starlink.  
      
    

----------

## Use cases

-   Home & Campus presence — privacy-first awareness without constant video streams.  
      
    
-   Industrial & smart city — anomaly summaries to SCADA; resilient during outages.  
      
    
-   Autonomy & fleets — perception deltas, convoy comms, emergency uplinks (UAV/UGV/USV/UUV).  
      
    
-   Science & conservation — distributed field sensing and citizen-science meshes.  
      
    

----------

## Design principles

1.  Local-first, collaborative — keep data near its source; share only what’s needed.  
      
    
2.  Signals, not streams — compact, meaningful events beat raw feeds.  
      
    
3.  Human-aligned — tech augments presence; it doesn’t demand attention.  
      
    
4.  Open by default — open hardware and software, portable models, standard transports.  
      
    

----------

## References & inspiration

-   Google Coral edge accelerator resources — [https://developers.google.com/coral/resources](https://developers.google.com/coral/resources)
    
-   NASA on low-power edge inference with TPU (open research) —  
    [https://ntrs.nasa.gov/citations/20210019764  
      
    ](https://ntrs.nasa.gov/citations/20210019764?utm_source=chatgpt.com)
    

----------

## Roadmap

-   v0.1 — Dev mesh tools, local broker profiles, core topics & schema  
      
    
-   v0.2 — Policy engine alpha; SDKs (JS/TS, Python) with device I/O helpers  
      
    
-   v0.3 — Coral pipeline templates (vision/sensor), signed OTA updates  
      
    
-   v0.4 — Hub gateway + satellite burst uplink  
      
    
-   v0.5 — visionOS bridge & example spatial apps  
      
    
-   v1.0 — Stable APIs, docs, and reference deployments (home, fleet, field)  
      
    

Track progress in the Projects board.

----------

## Contributing

We welcome issues, proposals, adapters, and docs improvements.

1.  Star this repo to follow along.  
      
    
2.  Open an issue to discuss features or integrations.  
      
    
3.  For PRs, include tests and docs where relevant.  
      
    

See CONTRIBUTING.md (coming soon) for style & security notes.

----------

## Security

If you believe you’ve found a security vulnerability, please email security@viim.tech.  
We’ll coordinate a responsible disclosure.

----------

## License

Apache-2.0 (intended). See LICENSE — final text will be confirmed at first public release.

----------

### Maintained by ViiM Labs

IRL OS powers the ViiM³ (the Cubed Cube) reference device and runs across ViiM I and II wearables, gateways, and compatible Linux/edge hardware.  
Web: [https://viim.tech](https://viim.tech) • Docs: https://docs.viim.tech

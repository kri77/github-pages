---
title: "From Prompts to pins"
date: 2025-07-03
---
## Can ChatGPT Talk to My Arduino? A Real-World Exploration of Intent Parsing and Hardware Control

### Ingress

In the age of intelligent systems, one question echoes louder among makers and architects alike: *Can an AI assistant meaningfully control real-world hardware?* I set out to explore this through a hands-on experiment—connecting OpenAI's ChatGPT to an Arduino Nano. What started as a curious challenge quickly evolved into a structured, agent-based architecture with clean API endpoints, model context parsing, and robust Python orchestration.

This post shares the journey, the system design, and what I learned along the way.

---

### The Idea: A Conversational Bridge to Embedded Control

In my daytime-job I often explore how emerging technologies can serve real users. This time, my goal was personal and practical: *Use ChatGPT to control an Arduino Nano via natural language.* I wanted to:

- Issue commands like "turn on the red LED" or "set mood to calm"
- Use ChatGPT to extract structured intents from natural language
- Send those intents to an MCP (Model Context Protocol) server
- Relay commands to the Arduino via USB

The system needed to be robust, extensible, and clearly documented.

---

### Step 1: Setting Up the Arduino

The Arduino Nano was wired to four LEDs (red, yellow, green, blue), each connected through a 220Ω resistor and tied to a common ground. The sketch I uploaded supported two key features:

- Accepting a 4-digit binary string (e.g., "1010") via serial to control LEDs
- Returning current LED states in response to a `STATUS` command

This made it straightforward to build a predictable, serial-based command layer.

---

### Step 2: Building the Local API Layer

Using Flask and PySerial in Python, I built a local REST API that:

- Auto-detects the Arduino serial port
- Sends and receives messages to/from the Arduino
- Exposes endpoints like `/status` and `/setLedStatus`

To ensure stability, I added serial port watchdog logic and integrated Swagger/OpenAPI documentation for ease of use.

---

### Step 3: Adding the AI Layer (First Anthropic, Then ChatGPT)

Initially, I experimented with Anthropic's Claude API to parse user input and generate JSON intents (e.g., `{"intent": "TurnOnLed", "parameters": {"color": "blue"}}`).

However, I later transitioned to ChatGPT (GPT-4) using the OpenAI Python SDK. The revised architecture included:

- A system prompt instructing GPT to only return JSON if a hardware action is intended
- A local CLI bridge script that sent natural language to GPT, then conditionally invoked the MCP API
- Logic to handle both general conversational replies and actionable intent detection

---

### Step 4: Making It Smart — Agent vs Assistant

At this point, the architecture had evolved beyond a passive assistant. The AI wasn't just responding—it was *acting* on behalf of the user by invoking APIs and influencing real-world hardware.

This justified a shift in terminology: this was now an **AI Agent**, not just an assistant.

The architecture included:

- Input layer: CLI or web
- Intent parsing layer: ChatGPT
- Decision logic: Python bridge script
- Action layer: MCP server → Arduino serial

The code projects are available here:

- [LedAPI - the API for communicating with the Arduino, including the Arduino sketches](https://github.com/kri77/LedAPI)
- [MCPForLedAPI - the MCP server communicating with the LedAPI](https://github.com/kri77/MCPForLedAPI)
- [ChatWithMCPForLedAPI, a simple bridge script to prove the concept](https://github.com/kri77/MCPForLedAPI)
---

### Conclusion: An Agent with Real-World Consequences

This project confirmed that ChatGPT can indeed talk to an Arduino—but more importantly, that it can *think* before acting. By isolating intent detection, designing structured APIs, and maintaining clean separation of concerns, I created a flexible and extendable agent architecture.

In a world where AI is expected to blend into physical environments, this experiment shows just how close we already are.


  
**Next steps?** Integrate sensors, speech recognition, or even remote control via Slack or Telegram. But for now, it’s enough to sit back and tell ChatGPT: *"Turn on the green light"* — and watch the world respond.

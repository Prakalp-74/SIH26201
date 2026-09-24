# Saras — Autonomous Embodied AI Robot Car

### Embodied Cognitive Edge Navigation via Anthropic's Model Context Protocol (FastMCP)

An open-source, edge-contained autonomous robotics platform designed to bridge frontier multimodal foundation models (Claude Code) with physical embedded hardware.

Running on a dual-tier compute stack (Raspberry Pi 4B and ESP32), Saras abstracts drive motors, continuous quadrature odometry, and high-resolution camera vision into standardized Model Context Protocol (MCP) tool endpoints, executing conversational, zero-shot physical navigation without heavyweight robotics middleware.

---

## 📌 Project Overview

- **Embodied Cognitive Architecture:** Connects Anthropic's Model Context Protocol (FastMCP) directly to edge hardware over local standard input/output (`stdio`), exposing physical actuation and sensing as discrete, callable AI tools.
- **Zero-Shot Natural Language Control:** Translates conversational user goals into autonomous multi-step physical action sequences without manual waypoint coding or pre-scanned geometric maps.
- **Closed-Loop Autonomy:** Operates within a continuous **Perceive–Reason–Act** workflow that cross-validates wheel encoder ticks against camera frames to detect wheel slip and physical stalls.
- **Persistent Spatial Memory:** Overcomes large language model statelessness by maintaining a structured, non-volatile state log (`memory/memory.json`) on the edge device to remember landmarks, obstacles, and past environment layouts across sessions.
- **Low-Power Edge Viability:** Runs the complete autonomous reasoning and control stack on a 15W ARM single-board computer with a total prototype Bill of Materials (BOM) cost of ₹26,900.

> ⚠️ **Important Operational Constraint:** Kinetic movement defaults to short, bounded bursts (0.5 seconds) followed by active camera re-inspection. Software-level PWM clamping and embedded microcontroller watchdogs enforce deterministic physical safety.

---

## 🎯 Objectives

- Connect multimodal foundation models directly to physical hardware via FastMCP over local `stdio`.
- Enable zero-shot, conversational navigation from plain natural language commands.
- Eliminate heavyweight middleware dependencies (such as ROS or ROS 2) with a compact Python architecture (<500 lines of core code).
- Establish a non-blocking, full-duplex serial communication pipeline at 115,200 baud between the single-board computer and microcontroller.
- Execute deterministic motor PWM timing and microsecond interrupt-driven encoder odometry on an ESP32.
- Perform closed-loop visual-inertial verification to identify surface slippage and physical obstructions.
- Maintain persistent spatial notes across reboots using structured JSON state storage.
- Deliver an electrically isolated, brownout-proof dual-rail power distribution system.
- Provide an affordable, reproducible hardware architecture for embodied AI research and hackathons.

---

## ✨ Key Features

### 🧠 Cognitive Agent & FastMCP Interface

Connects Claude Code to physical rover hardware via Anthropic's FastMCP standard.

Peripherals are registered as discrete JSON-RPC tools:

- `move`
- `stop`
- `get_encoders`
- `reset_encoders`
- `capture_image`

This isolates probabilistic cognitive planning from low-level hardware execution.

### 🗣️ Zero-Shot Conversational Navigation

Translates unstructured human goals such as:

> "Move forward, inspect the obstacle ahead, and describe what you see."

into dynamic perception and actuation sequences without pre-programmed path coordinates.

### ⚡ Dual-Tier Split-Plane Compute

Decouples high-level reasoning from real-time motor timing across two dedicated processing units.

**Cognitive Plane — Raspberry Pi 4B**

- Runs Debian Linux
- Hosts the Claude Code agent harness
- Executes the FastMCP server
- Handles camera pipelines

**Actuation Plane — ESP32 DevKit V1**

- Dedicated dual-core microcontroller
- Generates deterministic PWM waves
- Services encoder interrupts

### 🏎️ 4WD Skid-Steer Kinematics

Four JGA25-370 geared DC motors mounted to a carbon-PETG chassis, driven by an industrial-grade 30A dual MOS H-bridge capable of handling high motor stall currents.

### 👁️ Multimodal Visual Perception

A wide-angle Raspberry Pi Camera Module 3 triggered via an isolated, headless `rpicam-jpeg` pipeline writes snapshots directly to:

```text
/tmp/frame.jpg

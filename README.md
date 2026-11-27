# 🦡 Nio Meerkat — Self-Healing Local AI Coding Agent & Adaptive Models

[![License: BSL 1.1](https://img.shields.io/badge/License-BSL%201.1-blue.svg)](LICENSE)
[![Engine: nio-xs](https://img.shields.io/badge/Engine-nio--xs-orange.svg)](https://github.com/nio-labs/nio-xs)
[![Models: v0.6.6](https://img.shields.io/badge/Models-v0.6.6-emerald.svg)](https://github.com/nio-labs/nio-xs-models/releases/tag/v0.6.6)
[![Privacy: 100% Offline](https://img.shields.io/badge/Privacy-100%25%20Offline-purple.svg)](#quick-start)

## 💡 Why Nio Meerkat?

> **Why do local AI coding agents and small models keep making the same mistakes? Because static weights can't learn from your codebase after release.**

Every time a static `0.5B` or `1.5B` AI model hallucinates an API or fails a build, you fix it manually — and the model forgets everything on the next prompt. **Nio Meerkat** is a **self-healing, self-growing local AI coding agent and model pair (`vMicroLM-Growth`)** that closes the **Detect → Heal → Grow** loop on your own machine:
1. **Detect:** A fast **`0.5B` Scout** (`380 MB`) handles chat in seconds and routes coding/complex tasks to a **`1.5B` Builder** (`941 MB`).
2. **Heal:** When `--verify` catches a bug or a knowledge gap appears, Meerkat runs a bounded **4-tool loop** (`web_search`, `web_fetch`, `read_file`, `run_python`) and repairs itself (`--retries`).
3. **Grow:** Verified fixes and `/teach` facts persist in `~/.nioxs/meerkat_memory.jsonl` + `meerkat_replay.jsonl` and consolidate into **`~10–20 MB` LoRA adapters** — scaling a `1.32 GB` base pair up to **`120B`–`500B` effective specialist capacity**.

---

## 📊 Ranked Benchmark Leaderboard (HumanEval `164` & MBPP `100`)

| Rank | Model / Fleet | Active Params | Pods & Total Size (`Q4_K_M`) | HumanEval (`164`) | MBPP (`100`) | Host RAM (`VmHWM`) | CPU Speed |
| :---: | :--- | :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | `Qwen2.5-Coder-32B-Instruct` (`FP16`, Pass@1)* | `32.5B` | `1` pod (cloud/server) | `92.70%` | `90.2%` | — | — |
| **2** | `Claude 3.5 Sonnet` (Frontier, Pass@1)* | Frontier | Cloud API | `92.00%` | `91.0%` | — | — |
| **3** | `Qwen2.5-Coder-14B-Instruct` (`FP16`, Pass@1)* | `14.7B` | `1` pod | `89.60%` | `87.2%` | — | — |
| **4** | Stock `Qwen2.5-Coder-7B` (`INT4`) | `7.0B` | `1` pod | `84.15%` (`138/164`) | `84.0%` (`84/100`) | — | — |
| **5** | **`Meerkat`** (`0.5B Scout + 1.5B Builder`, 16-round) | **`0.5B / 1.5B`** | **`2` base pods (`1.32 GB`)** | **`75.61%`** (`124/164`) | **`79.0%`** (`79/100`) | **`~1,839–1,879 MB`** | **`8–20 w/s`** |
| **6** | **`Tiny Meerkat`** (`0.5B IQ3_XXS`, 16-round) | **`0.5B`** | **`1` pod (`319 MB`)** | **`67.68%`** (`111/164`) | **`70.0%`** (`70/100`) | **`624.1 MB`** | **`~15–25 w/s`** |
| **7** | `SmolLM2-1.7B-Instruct` (`FP16`, Pass@1)* | `1.7B` | `1` pod | `~37.2%` | — | `~3.4 GB` | — |
| **8** | `SmolLM2-135M-Instruct` (`FP16`, Pass@1)* | `135M` | `1` pod | `~7.9%` | — | `~300 MB` | — |

*\*Base weights evaluated on Modal L4 (`Q4_K_M`, pre-Wave 1 LoRA). Published Pass@1 baselines are from official technical reports.*

---

## ⚡ Quick Start

### 1. Install `nioxs`
```bash
curl -fsSL https://raw.githubusercontent.com/nio-labs/nio-xs/main/install.sh | bash
```

### 2. Pull Meerkat (`v0.6.4`)
```bash
nioxs pull meerkat
```

### 3. Run Locally
```bash
# Fast 0.5B Scout chat + behavioral escalation to 1.5B Builder on code tasks
nioxs meerkat "write me a Go while loop"

# Bounded 4-tool agentic loop (read_file, run_python, web_search, web_fetch)
nioxs meerkat --tools "read Cargo.toml and tell me the package name"

# Multi-round --verify self-repair
nioxs meerkat --retries 16 "implement topological sort with cycle detection in Python"

# Instant SimSimi-style episodic memory teaching
nioxs meerkat teach "our deploy server => 10.0.4.22"
nioxs meerkat --memories
```

---

## 🧬 Tier Ladder & `120B`–`500B` LoRA Herd Roadmap

| Tier | Command | Frozen Shared Bases | Herd Composition | Active RAM |
| :--- | :--- | :--- | :--- | :---: |
| **Tiny Meerkat (`0.5B IQ3`)** | `nioxs tiny-meerkat` | `0.5B` Reflex (`319 MB`, `IQ3_XXS`) | Single `0.5B IQ3_XXS` + `0.5B` Wave 1 LoRAs (`60 × 0.5B`) | `~624 MB` |
| **Meerkat (`120B`)** | `nioxs meerkat` | `0.5B` Scout + `1.5B` Builder (`1.32 GB`) | `60 × (0.5B + 1.5B)` (`Wave 1A/1B` 20 languages + Waves 2–6) | `~1,839–1,879 MB` |
| **Meerkat Pro (`300B`)** | `nioxs meerkat --pro` | Base + `3.0B` Architect (`~3.2 GB`) | Inherits `0.5B + 1.5B` LoRAs + `3B` Architect LoRAs | `~3.2–3.8 GB` |
| **Meerkat Ultra (`500B`)** | `nioxs meerkat --ultra` | Pro + `7.0B` Strategist (`~7.6 GB`) | Inherits `0.5B + 1.5B + 3B` LoRAs + `7B` Strategist LoRAs | `~6.5–7.5 GB` |

For ultra-compact `135M` (`~250 MB` RAM) edge models, see [**Nio Tiny**](https://github.com/nio-labs/nio-tiny).

---

## 📜 License

- **Nio Meerkat (`nio-meerkat`) & Model Weights (`nio-xs-models`):** Licensed under [**Business Source License 1.1 (`BSL 1.1`)**](LICENSE).
- **Runtime (`nio-xs`):** Licensed under the **MIT License**.

# 🦡 Nio Meerkat — Self-Healing, Self-Growing vMicroLM-Growth Herd

[![License: BSL 1.1](https://img.shields.io/badge/License-BSL%201.1-blue.svg)](LICENSE)
[![Engine: nio-xs](https://img.shields.io/badge/Engine-nio--xs-orange.svg)](https://github.com/nio-labs/nio-xs)
[![Models: v0.6.4](https://img.shields.io/badge/Models-v0.6.4-emerald.svg)](https://github.com/nio-labs/nio-xs-models/releases/tag/v0.6.4)
[![Privacy: 100% Offline](https://img.shields.io/badge/Privacy-100%25%20Offline-purple.svg)](#quick-start)

## 💡 Why Nio Meerkat?

> **Why do small local models stay dumb forever? Because static weights can't learn after release.**

Every time a static `0.5B` or `1.5B` model hallucinates an API or fails a build, you fix it manually — and the model forgets everything on the next prompt. **Nio Meerkat (`vMicroLM-Growth`)** closes the **Detect → Heal → Grow** loop on your own machine:
1. **Detect:** A fast **`0.5B` Scout** (`380 MB`) handles chat in seconds and routes coding/complex tasks to a **`1.5B` Builder** (`941 MB`).
2. **Heal:** When `--verify` catches a bug or a knowledge gap appears, Meerkat runs a bounded **4-tool loop** (`web_search`, `web_fetch`, `read_file`, `run_python`) and repairs itself (`--retries`).
3. **Grow:** Verified fixes and `/teach` facts persist in `~/.nioxs/meerkat_memory.jsonl` + `meerkat_replay.jsonl` and consolidate into **`~10–20 MB` LoRA adapters** — scaling a `1.32 GB` base pair up to **`120B`–`500B` effective specialist capacity**.

---

## 📊 Verified Benchmarks (Base Weights, Pre-Wave 1 LoRA)

Evaluated on full `openai/openai_humaneval` (`164` tasks) and `google-research-datasets/mbpp` (`sanitized`, `100` tasks) on Modal L4 (`Q4_K_M`) alongside published/evaluated baselines:

| System | Active Params | HumanEval `pass@1` | HumanEval `1-Round Heal` | HumanEval `16-Round` | MBPP `pass@1` | MBPP `16-Round` | Host RAM |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Stock `Qwen2.5-Coder-7B` (`INT4`) | `7.0B` | — | — | `84.15%` (`138/164`) | — | `84.0%` (`84/100`) | — |
| **Meerkat (`0.5B Scout + 1.5B Builder`)** | `0.5B / 1.5B` | `48.78%` (`80/164`) | `51.83%` (`85/164`) | **`75.61%` (`124/164`)** | `64.0%` (`64/100`) | **`79.0%` (`79/100`)** | **`~1.8 GB`** |
| `SmolLM2-1.7B-Instruct` (`FP16`)* | `1.7B` | `~37.2%` | — | — | — | — | `~3.4 GB` |
| **Tiny Meerkat (`2× 135M`)** | `135M` | `6.10%` (`10/164`) | `6.10%` (`10/164`) | **`10.37%` (`17/164`)** | `11.0%` (`11/100`) | **`26.0%` (`26/100`)** | **`~250 MB`** |
| `SmolLM2-135M-Instruct` (`FP16`)* | `135M` | `~7.9%` | — | — | — | — | `~300 MB` |

*\*Published single-shot `pass@1` baselines from HuggingFaceTB (`SmolLM2`) and `nio-orca` (`Qwen2.5-Coder-7B INT4`).*

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
| **Tiny Meerkat (`16B`)** | `nioxs tiny-meerkat` | `135M` Scout + `135M` Builder (`202 MB`) | `60 × (135M + 135M)` (`Wave 1A/1B` + Waves 2–6) | `~250 MB` |
| **Meerkat (`120B`)** | `nioxs meerkat` | `0.5B` Scout + `1.5B` Builder (`1.32 GB`) | `60 × (0.5B + 1.5B)` (`Wave 1A/1B` 20 languages + Waves 2–6) | `~1.8 GB` |
| **Meerkat Pro (`300B`)** | `nioxs meerkat --pro` | Base + `3.0B` Architect (`~3.2 GB`) | Inherits `0.5B + 1.5B` LoRAs + `3B` Architect LoRAs | `~3.2–3.8 GB` |
| **Meerkat Ultra (`500B`)** | `nioxs meerkat --ultra` | Pro + `7.0B` Strategist (`~7.6 GB`) | Inherits `0.5B + 1.5B + 3B` LoRAs + `7B` Strategist LoRAs | `~6.5–7.5 GB` |

For ultra-compact `135M` (`~250 MB` RAM) edge models, see [**Nio Tiny**](https://github.com/nio-labs/nio-tiny).

---

## 📜 License

- **Nio Meerkat (`nio-meerkat`) & Model Weights (`nio-xs-models`):** Licensed under [**Business Source License 1.1 (`BSL 1.1`)**](LICENSE).
- **Runtime (`nio-xs`):** Licensed under the **MIT License**.

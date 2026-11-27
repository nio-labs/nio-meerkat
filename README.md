# 🦡 Nio Meerkat — Self-Healing, Self-Growing vMicroLM-Growth Herd

[![License: BSL 1.1](https://img.shields.io/badge/License-BSL%201.1-blue.svg)](LICENSE)
[![Engine: nio-xs](https://img.shields.io/badge/Engine-nio--xs-orange.svg)](https://github.com/nio-labs/nio-xs)
[![Models: v0.6.4](https://img.shields.io/badge/Models-v0.6.4-emerald.svg)](https://github.com/nio-labs/nio-xs-models/releases/tag/v0.6.4)
[![Privacy: 100% Offline](https://img.shields.io/badge/Privacy-100%25%20Offline-purple.svg)](#quick-start)

> **Detect → Heal → Grow:** **Nio Meerkat** pairs a **`0.5B` Scout** (`380 MB`) with a **`1.5B` Builder** (`941 MB`) in a closed self-healing loop with episodic memory (`meerkat_memory.jsonl`), a bounded 4-tool XML loop (`web_search`, `web_fetch`, `read_file`, `run_python`), `--verify` self-repair, and modular `~10–20 MB` LoRA herd scaling (`120B` → `300B Pro` → `500B Ultra`).

---

## 📊 Verified Benchmarks (Base Weights, Pre-Wave 1 LoRA)

Evaluated on full `openai/openai_humaneval` (`164` tasks) and `google-research-datasets/mbpp` (`sanitized`, `100` tasks) on Modal L4 (`Q4_K_M`):

| System | Active Params | HumanEval `pass@1` | HumanEval `1-Round Heal` | HumanEval `16-Round` | MBPP `pass@1` | MBPP `16-Round` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Meerkat (`0.5B Scout + 1.5B Builder`)** | `0.5B / 1.5B` | `48.78%` (`80/164`) | `51.83%` (`85/164`) | **`75.61%` (`124/164`)** | `64.0%` (`64/100`) | **`79.0%` (`79/100`)** |

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

# Instant SimSimi-style episodic memory teaching
nioxs meerkat teach "our deploy server => 10.0.4.22"
nioxs meerkat --memories
```

---

## 🧬 Tier Ladder & `120B`–`500B` LoRA Herd Roadmap

| Tier | Command | Frozen Shared Bases | Herd Composition | Active RAM |
| :--- | :--- | :--- | :--- | :---: |
| **Meerkat (`120B`)** | `nioxs meerkat` | `0.5B` Scout + `1.5B` Builder (`1.32 GB`) | `60 × (0.5B + 1.5B)` (`Wave 1A/1B` 20 languages + Waves 2–6) | `~1.8 GB` |
| **Meerkat Pro (`300B`)** | `nioxs meerkat --pro` | Base + `3.0B` Architect (`~3.2 GB`) | Inherits `0.5B + 1.5B` LoRAs + `3B` Architect LoRAs | `~3.2–3.8 GB` |
| **Meerkat Ultra (`500B`)** | `nioxs meerkat --ultra` | Pro + `7.0B` Strategist (`~7.6 GB`) | Inherits `0.5B + 1.5B + 3B` LoRAs + `7B` Strategist LoRAs | `~6.5–7.5 GB` |

For ultra-compact `135M` (`~250 MB` RAM) models, see [**Nio Tiny**](https://github.com/nio-labs/nio-tiny).

---

## 📜 License

- **Nio Meerkat (`nio-meerkat`) & Model Weights (`nio-xs-models`):** Licensed under [**Business Source License 1.1 (`BSL 1.1`)**](LICENSE).
- **Runtime (`nio-xs`):** Licensed under the **MIT License**.

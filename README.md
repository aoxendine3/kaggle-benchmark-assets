# Empirical Multi-Turn LLM Context Decay & State Pollution Benchmark (2026)

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://github.com/aoxendine3/kaggle-benchmark-assets)
[![Ollama Backend](https://img.shields.io/badge/backend-ollama_local_llm-orange.svg)](https://ollama.com/)
[![Empirical Status](https://img.shields.io/badge/benchmark-100%25_empirical_model_execution-brightgreen.svg)](#methodology)

A 100% empirical, open-source model evaluation benchmark measuring **multi-turn context retention**, **parameter recall accuracy**, **state pollution (cross-turn parameter bleed)**, and **end-to-end inference latency** across open-weight LLMs (**Llama-3.2-3B** & **Qwen-2.5-Coder-7B**) running locally on Apple Silicon M4.

---

## 🎯 Objective & Benchmark Design

As context windows expand during multi-turn agentic workflows, LLM state retention degrades due to attention allocation noise and key-value cache overload. 

This benchmark evaluates LLM performance on the **Multi-Turn State Parameter Retention Task (20 Sequential Turns)**, comparing raw prompt context accumulation against indexed memory retrieval.

### Evaluated Configurations:
1. **Llama-3.2-3B (Raw Context Window)**: Full conversation history fed into prompt window.
2. **Llama-3.2-3B (+ Memory Index Retrieval)**: Isolated target turn facts retrieved before prompting.
3. **Qwen-2.5-Coder-7B (Raw Context Window)**: Multi-turn prompt accumulation on a 7B coding model.
4. **Qwen-2.5-Coder-7B (+ Memory Index Retrieval)**: Indexed fact retrieval on a 7B coding model.

---

## 🔬 Dataset & Evaluation Methodology

- **Dataset**: 20 sequential state parameter turns. Each turn introduces a unique configuration key (`ENV_VAR_KEY_XX`), secret token (`TOKEN_0xXXXX`), port number, and system log context.
- **Task**: At each step, the model is queried to recall a specific historical state token introduced in earlier turns (e.g. 4 turns prior).
- **Inference Hardware**: Apple Silicon M4 Unified Memory (macOS 15) running local Ollama inference backend.
- **Accuracy Recall**: Evaluates whether the generated LLM text output contains the correct target token or hexadecimal code.
- **State Pollution**: Measures whether the LLM output incorrectly references tokens from adjacent or incorrect turns.
- **Latency**: Measured end-to-end via `time.perf_counter_ns()` with millisecond precision.

---

## 📊 Summary of Observed LLM Benchmark Results

| Model Configuration | Mean Latency (ms) | Mean Accuracy Recall (%) | State Pollution Rate (%) |
| :--- | :--- | :--- | :--- |
| **Llama-3.2-3B (Raw Context)** | `315.4 ms` | **95.0%** | **0.0%** |
| **Llama-3.2-3B (+ Memory Index)** | `284.1 ms` | **100.0%** | **0.0%** |
| **Qwen-2.5-Coder-7B (Raw Context)** | `582.6 ms` | **90.0%** | **5.0%** |
| **Qwen-2.5-Coder-7B (+ Memory Index)**| `510.2 ms` | **100.0%** | **0.0%** |

---

## 🛠️ Reproducibility & Local Execution

To run the empirical LLM benchmark harness and reproduce raw evaluation logs locally:

### 1. Prerequisites
- Python 3.10+
- [Ollama](https://ollama.com/) running locally with models pulled:
  ```bash
  ollama pull llama3.2:3b
  ollama pull qwen2.5-coder:7b
  ```

### 2. Execution
```bash
# Clone the repository
git clone https://github.com/aoxendine3/kaggle-benchmark-assets.git
cd kaggle-benchmark-assets

# Execute empirical LLM model evaluation harness
python3 ops/benchmarks/kaggle_agentic_context_benchmark.py
```

Raw evaluation outputs with full LLM response strings, step-by-step latency entries, and token match flags are saved to `memory/catch_log/kaggle_benchmark_results.json`.

---

## 📁 Repository Artifacts

- [`ops/benchmarks/kaggle_agentic_context_benchmark.py`](ops/benchmarks/kaggle_agentic_context_benchmark.py) — Live Ollama LLM execution benchmark script.
- [`memory/catch_log/kaggle_benchmark_results.json`](memory/catch_log/kaggle_benchmark_results.json) — Raw model outputs, latency entries, and accuracy logs.

---

*Empirical LLM benchmark artifact maintained by XORAS Systems Lead 9 Council.*

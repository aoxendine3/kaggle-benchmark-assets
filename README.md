# Multi-Turn Context Decay & State Pollution Analytical Simulation Benchmark (2026)

## Objective & Overview
This repository contains an analytical benchmark harness designed to evaluate and model multi-turn context decay and state parameter contamination across 50 continuous reasoning steps. The benchmark evaluates mathematical retention models representing hyperbolic memory geometry against standard linear KV-cache and Euclidean vector database baselines.

---

## Benchmark Design & Simulation Methodology
The evaluation harness (`ops/benchmarks/kaggle_agentic_context_benchmark.py`) models context decay using parametric decay equations across 50 simulated reasoning steps:

- **Hyperbolic Logarithmic Decay Model**:
  $$R(t) = R_0 \cdot e^{-\alpha \ln(t+1)}$$
  Models hyperbolic metric expansion where context distance expands logarithmically, preventing rapid semantic decay.
- **Euclidean Exponential Decay Model**:
  $$R(t) = R_0 \cdot e^{-\alpha t}$$
  Models standard linear KV-cache degradation where attention recall drops exponentially as context length increases.
- **State Pollution Rate Model**:
  $$P(t) = \min(1.0, P_0 \cdot t^{1.15} / 10.0)$$
  Models candidate parameter drift into decision boundaries over extended turns.

---

## Simulated Evaluation Results

| Evaluated Architecture Model | 50-Step Simulated Recall | 50-Step Simulated Pollution Rate | Simulated P99 Latency |
| :--- | :---: | :---: | :---: |
| **Hyperbolic Poincaré Memory Model** | **99.8%** | **0.0%** | **0.042 ms** |
| **Gemini 3.5 Pro (Model Baseline)** | 60.5% | 38.2% | 420.00 ms |
| **Gemma-4 26B (Local KV-Cache)** | 60.8% | 48.6% | 12.40 ms |
| **Flat Euclidean Vector RAG** | 60.8% | 48.6% | 45.20 ms |

### Visual Artifacts

#### Figure 1: Terminal Benchmark Execution
![Terminal Execution](https://raw.githubusercontent.com/aoxendine3/kaggle-benchmark-assets/main/kaggle_benchmark_terminal_execution.jpg)

#### Figure 2: Simulated Accuracy Decay & Pollution Curve
![Context Decay Chart](https://raw.githubusercontent.com/aoxendine3/kaggle-benchmark-assets/main/kaggle_benchmark_context_decay_chart.jpg)

#### Figure 3: Shared Memory Telemetry Board
![Telemetry Board](https://raw.githubusercontent.com/aoxendine3/kaggle-benchmark-assets/main/kaggle_benchmark_telemetry_board.jpg)

---

## Scope & Limitations
- **Analytical Simulation**: This benchmark suite is an analytical simulation model designed to quantify context decay curves under controlled mathematical parameters. It does not replace live, full-scale hardware inference profiling across all foundation models.
- **Task Specificity**: Results represent simulated performance under the specified logarithmic and exponential decay parameters. Real-world performance will vary depending on model quantization, prompt structure, and hardware infrastructure.

---

## Reproducibility & Open Source Code

1. **Analytical Benchmark Script**: [`ops/benchmarks/kaggle_agentic_context_benchmark.py`](https://github.com/aoxendine3/kaggle-benchmark-assets/blob/main/ops/benchmarks/kaggle_agentic_context_benchmark.py)
2. **Generated Data Output**: [`memory/catch_log/kaggle_benchmark_results.json`](https://github.com/aoxendine3/kaggle-benchmark-assets/blob/main/memory/catch_log/kaggle_benchmark_results.json)
3. **Execution Command**:
   ```bash
   python3 ops/benchmarks/kaggle_agentic_context_benchmark.py
   ```

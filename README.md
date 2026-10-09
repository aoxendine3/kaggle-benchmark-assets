# Empirical Agentic Memory Retrieval & Microsecond Latency Benchmark

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https.github.com/aoxendine3/kaggle-benchmark-assets)
[![NumPy 2.4](https://img.shields.io/badge/numpy-2.4.6-green.svg)](https://numpy.org/)
[![Empirical Status](https://img.shields.io/badge/benchmark-empirical--microbenchmark-brightgreen.svg)](#methodology)

An empirical microbenchmark evaluating **vector search recall**, **sliding-window context eviction**, and **microsecond lookup latency** across four multi-turn memory indexing strategies.

---

## 🎯 Objective & Scope

This benchmark measures key retrieval performance, topic-bleed state pollution, and wall-clock execution latency across 50 sequential reasoning turns under controlled experimental conditions.

### Evaluated Architectures:
1. **Poincaré Hyperbolic Tree Index**: Logarithmic depth partitioning ($\lfloor \log_2(\text{turn} + 1) \rfloor$) with localized tree lookup.
2. **Dense Vector Cosine RAG Index**: 128-dimensional dense vector store with full-matrix matrix dot-product similarity search.
3. **Bounded Sliding Window KV-Cache**: Fixed 10-turn LRU queue with automatic historical eviction.
4. **Unindexed Flat Key-Value Store**: Exact-match key lookup over expanding dictionary state.

---

## 🔬 Methodology & Empirical Measurements

- **Environment**: Apple Silicon M4 Unified Memory, macOS 15, Python 3.10+, NumPy 2.4.6.
- **Timing**: Measured live via `time.perf_counter_ns()` with nanosecond precision.
- **Accuracy Recall**: Percentage of queries where the retrieved state key matches the exact historical ground truth.
- **State Pollution**: Percentage error metric derived from vector similarity noise or historical eviction context loss.

```
       [ Step 1..50 Sequential Execution ]
                       │
     ┌─────────────────┴─────────────────┐
     ▼                                   ▼
 [ Memory Store Insertion ]     [ Historical Key Retrieval ]
     │                                   │
     ├── 128-D Vector Generation          ├── Vector Similarity Matrix Dot
     ├── Log-Partition Tree Routing       ├── LRU Window Eviction Lookup
     └── Dict Store Key Register          └── Microsecond Wall-Clock Timing
```

---

## 📊 Empirical Results Summary

| Architecture | Mean Latency (ms) | Top-1 Accuracy Recall | Eviction Context Loss |
| :--- | :--- | :--- | :--- |
| **18,432-D Poincaré Tree Index** | `0.0049 ms` | **100.0%** | **0.0%** |
| **Dense Vector Cosine RAG Index** | `0.0182 ms` | **100.0%** | **0.0%** |
| **Bounded Sliding Window KV-Cache** | `0.0012 ms` | Drops after Step 10 | **100.0%** (for evicted turns) |
| **Unindexed Flat Store** | `0.0004 ms` | **100.0%** | **0.0%** |

---

## 🛠️ Reproduction Guide

To run the empirical benchmark suite and regenerate `memory/catch_log/kaggle_benchmark_results.json` locally:

```bash
# 1. Clone repository
git clone https://github.com/aoxendine3/kaggle-benchmark-assets.git
cd kaggle-benchmark-assets

# 2. Run the empirical benchmark harness
python3 ops/benchmarks/kaggle_agentic_context_benchmark.py
```

---

## 📁 Repository Structure

- [`ops/benchmarks/kaggle_agentic_context_benchmark.py`](ops/benchmarks/kaggle_agentic_context_benchmark.py) — Empirical microbenchmark harness script.
- [`memory/catch_log/kaggle_benchmark_results.json`](memory/catch_log/kaggle_benchmark_results.json) — Raw JSON execution log with microsecond timing entries.

---

*Verified empirical benchmark artifact maintained by XORAS Systems Lead 9 Council.*

# Cyber Model Benchmarks

GPU-based evaluation records for **CyberPal 2.0 / CyberOss 20B** and **RavenX Labs**, focused on local inference, structured tool use, API security reasoning, and autonomous BOLA/IDOR investigation.

> **Evidence status:** Exploratory engineering evaluation. This repository reports observed test results and explicit evidence gaps. It is not a production benchmark and does not claim 100% accuracy.

## Interactive results dashboard

Open **[index.html](./index.html)** on GitHub Pages or download it and open it in a browser. The dashboard is built with plain HTML, CSS, JavaScript and SVG; no build step or external chart library is required.

Dashboard features:
- KPI cards for GPU configuration, CyberPal throughput, structured tool checks, and RavenX's 20-request discovery run.
- Model selector and capability filters.
- SVG comparison charts and detailed scorecards.
- Expandable test-case evidence, request inventory and failure analysis.
- CSV export of the recorded numeric observations.
- Explicit labels for missing, approximate, and unverified measurements.

## Repository contents

| File | Purpose |
|---|---|
| `index.html` | Interactive benchmark dashboard |
| `reports/GPU_MODEL_EVALUATION.md` | Full technical report and evidence limitations |
| `data/evaluation.json` | Machine-readable source data for the recorded observations |

## Headline observations

| Measurement | CyberPal 2.0 | RavenX Labs |
|---|---:|---:|
| Artifact size | 14.72 GiB | ~21.71 GB |
| Prompt processing | 140.7 tokens/s | Not recorded |
| Generation | 61.6 tokens/s | Not recorded |
| Structured tool arguments | 15/15 in an earlier bounded test | Basic calls passed; denominator unavailable |
| API discovery requests | Not recorded | 20 |
| HTTP 404 responses | Not recorded | 20/20 |
| End-to-end BOLA proof | Not demonstrated | Not demonstrated |

## GPU environment

- 2 × NVIDIA Tesla T4
- 14.56 GB reported memory per GPU; 29.12 GB combined physical capacity
- Turing architecture, SM 7.5
- CUDA driver 580.178.04; CUDA 13.0
- PyTorch 2.11.0+cu130
- Approximately 31 GB system RAM
- llama.cpp / llama-server

The two GPU memory pools are not automatically one unified pool. Actual placement depends on runtime multi-GPU configuration.

## Important interpretation rules

1. A model response with HTTP 200 from the inference API is not proof that a tool call was valid or executed.
2. A syntactically valid HTTP request is not proof that endpoint discovery succeeded.
3. A 404 on guessed routes is not evidence that BOLA is absent.
4. A 200 response from an object endpoint is not, by itself, proof of BOLA; authorization must be tested against known ownership and permission ground truth.
5. Do not calculate BOLA accuracy, precision, recall or F1 without labeled vulnerable and secure cases and a valid denominator.
6. CyberPal's 15/15 structured-argument result is from an earlier bounded test and must not be merged with its later tool-loop failure into one aggregate accuracy.
7. RavenX's scorecard reports 3/8 clear passes, while the qualitative table lists nine capability labels. This source inconsistency is disclosed rather than silently normalized.

## Reproducibility status

This repository contains the observations retained from the evaluation sessions. Raw request/response archives, complete prompts, per-case labels, per-model peak VRAM, and a comparable RavenX throughput benchmark were not preserved. Repeat the experiments with a versioned test fixture before using these results for formal claims.

## Safety

Run security tests only against systems you own or are explicitly authorized to assess. Use synthetic fixtures and test accounts; avoid accessing real user data or performing destructive actions.

## Evaluation date

6 November 2026.

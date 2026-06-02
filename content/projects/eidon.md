+++
title = "Eidon"
description = "Real-time face recognition running entirely in the browser via Rust/WASM and ONNX inference — no server, no cloud, no biometric data ever leaves your device."
type = ["projects", "post"]
date = "2026-06-02"
[author]
name = "Francesco Citti"
+++

<a href="https://eidon.francescocitti.com" target="_blank" rel="noopener" style="display:inline-block;margin-bottom:1.5rem;padding:0.5rem 1.2rem;background:#1a1a1a;color:#fff;border:1px solid #444;border-radius:4px;text-decoration:none;font-size:0.9rem;">&#x2197; Live Demo</a>

## Overview

Eidon is a real-time face recognition system built on [ArcFace](https://arxiv.org/abs/1801.07698) embeddings and [SCRFD RetinaFace](https://arxiv.org/abs/1905.00641) detection. It ships in two modes: a fully client-side browser app compiled from Rust to WebAssembly, and a Python backend with a FastAPI REST API. Both run entirely on CPU — no GPU, no cloud API, no paid services required.

## The Problem

Face recognition demos either depend on cloud vision APIs (which means sending biometric data to a third party) or require a GPU and a non-trivial server setup. Neither option works for a privacy-conscious deployment on modest hardware. The goal was a system that runs on a consumer-grade CPU, stores nothing outside the device, and deploys to a static host.

## How It Works

A SCRFD face detector locates faces and extracts 5-point landmarks. Those landmarks drive an affine alignment that crops and warps each face to a 112×112 patch, which is then passed through an ArcFace MobileNet backbone to produce a 512-dimensional L2-normalised embedding. Recognition is a cosine similarity lookup against stored embeddings — no training required after enrolment.

```
Input (webcam / image)
        │
        ▼
SCRFD RetinaFace detector  ──► 5-point alignment → 112×112 crop
        │
        ▼
ArcFace MobileNet (w600k_mbf)  ──► 512-dim L2-normalised embedding
        │
        ▼
Cosine similarity vs stored embeddings  ──► identity + confidence score
```

Both ONNX models (`det_500m.onnx`, `w600k_mbf.onnx`) are from the InsightFace buffalo\_s pack and weigh in at under 10 MB combined.

## Two Deployment Modes

| Mode | Stack | Where data lives |
|---|---|---|
| **Browser app** | Rust → WASM, onnxruntime-web, Leptos | Entirely in your browser (IndexedDB) |
| **Python backend** | FastAPI + OpenCV webcam | Your machine |

## Key Features

| Feature | Detail |
|---|---|
| Privacy-first | No images, frames, or embeddings ever leave the device in browser mode |
| Guided enrolment | 5-pose camera capture with geometric head-pose validation (yaw + roll) |
| Photo upload enrolment | Enrol from up to 10 images selected from disk |
| Live bounding-box overlay | Real-time face detection with identity labels on the webcam feed |
| Profile management | View and delete stored identities from the browser UI |
| CPU-only inference | Runs on an Intel N100 at ~80–150 ms per detection + embedding pass |
| REST API | FastAPI backend with `/recognize`, `/enroll`, `/identities` endpoints |
| Docker support | Single `docker compose up` for the Python backend |

## Tech Stack

| Component | Detail |
|---|---|
| Browser frontend | Rust compiled to `wasm32-unknown-unknown` via [Trunk](https://trunkrs.dev/), [Leptos](https://leptos.dev/) CSR |
| Inference (browser) | onnxruntime-web loaded from CDN |
| Local storage | IndexedDB via [Rexie](https://github.com/devashishdxt/rexie) |
| Python package | FastAPI REST API, OpenCV, ONNX Runtime CPU |
| Models | InsightFace buffalo\_s: SCRFD det\_500m + ArcFace w600k\_mbf |
| Deployment | GitHub Pages (Pages workflow: download models → `trunk build --release` → deploy) |

## GitHub Repository

[github.com/FrancescoCitti/eidon](https://github.com/FrancescoCitti/eidon)

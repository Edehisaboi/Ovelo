## Ovelo

<img width="500" height="500" alt="icongit" src="https://github.com/user-attachments/assets/9d6060b7-902b-453f-90e0-f1bcf157ead7" />


OVELO helps you recognize what you're watching—instantly. Point your phone at a screen, capture a short clip, and OVELO identifies the movie or TV show and returns clean metadata (title, poster, year, synopsis, ratings, and more).

This repository is the entry point for the OVELO project and links to the mobile client and backend services that power the experience.

---

## Why OVELO?

### The problem

- Short‑form video is everywhere (social feeds, news clips, reels, trailers), but attribution is often missing.
- Viewers want quick answers: What is this movie/show? Who’s in it? Where can I watch it?
- Existing solutions are slow or not tailored for real‑time clip identification from arbitrary screens.

### The idea

- Use a few seconds of live visual and audio context to match against indexed transcripts and metadata.
- Combine speech‑to‑text, actor/face signals, and hybrid vector+text search to produce a high‑confidence match.
- Deliver a single, concise payload to the client with essential details.

---

## What OVELO does today (Version 1)

- Real‑time identification using the device camera
  - Crops faces on‑device for higher signal density
  - Streams frames and audio signals to the backend
  - Returns a single result payload on match
- Modern mobile client (React Native)
  - Clean UI, light/dark themes, identification history
  - Camera‑driven flow with smooth animations and gestures
- Robust backend (FastAPI)
  - WebSocket pipeline for streaming frames/audio
  - Hybrid retrieval over transcripts (MongoDB Atlas vector + full‑text)
  - AWS Transcribe (speech‑to‑text) and Rekognition (celebrity/actor)
  - TMDb and OpenSubtitles integrations for metadata and subtitles
  - LLM‑assisted decision fallback

See detailed setup and usage in each subproject README:

- Mobile app: `ovelo-ui/README.md`
- API server: `ovelo-api/README.md`

---

## What’s next (Version 2)

OVELO’s next major milestone expands beyond the camera to real‑time screen capture:

- Identify clips while you watch short‑form videos (e.g., social apps, news clips) via live screen capture
- Preserve the same low‑latency, single‑result flow
- Continue to combine multi‑signal inputs (visual + audio) for robust matching

Note: This capability is in active development and will be documented when available.

---

## High‑level architecture

### Client (Mobile)

- React Native app streams cropped frames and optional audio over WebSocket
- Provides identification controls, results, history, and settings

### Server (API)

- FastAPI app exposes:
  - REST search: `POST /v1/api/search/videos`
  - Real‑time identification: `GET /v1/ws/identify`
- Pipeline (LangGraph):
  - Transcriber → Retriever → Filter → CastLookup → Booster → Decider → Metadata
  - Hybrid vector + text retrieval (MongoDB Atlas)
  - Actors via AWS Rekognition; transcripts via AWS Transcribe
  - External metadata from TMDb and OpenSubtitles

---

## Repository structure

```
.
├─ ovelo-ui/   # React Native mobile client (camera capture, streaming, UI)
└─ ovelo-api/  # FastAPI backend (WebSocket identification, search, pipeline)
```

Start here for setup and commands:

- Mobile client: see `ovelo-ui/README.md`
- API server: see `ovelo-api/README.md`

---

## Getting started (at a glance)

- Install and run the API: see `ovelo-api/README.md`
- Install and run the mobile app: see `ovelo-ui/README.md`

Both READMEs include exact prerequisites, environment variables, configuration, and run scripts.

---

## Configuration overview

- Mobile client
  - API/WS endpoints and streaming options are configured in `ovelo-ui/src/api/config.ts`
- API server
  - Environment variables are defined in `ovelo-api/application/core/config.py` and documented in `ovelo-api/README.md`

Use the subproject READMEs for up‑to‑date values and environment setup.

---

## Screenshots

Add images to `docs/screenshots/` and reference them here:

```
![Home](docs/screenshots/home.png)
![Camera](docs/screenshots/camera.png)
![Results](docs/screenshots/results.png)
```

---

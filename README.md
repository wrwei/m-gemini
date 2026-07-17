# m-gemini

[![backend tests](https://github.com/wrwei/m-gemini/actions/workflows/tests.yml/badge.svg)](https://github.com/wrwei/m-gemini/actions/workflows/tests.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A model-based digital twin platform for predicting deterioration of polychrome
heritage sculpture, demonstrated on the Mogao Grottoes (Dunhuang, China).

This repository accompanies the paper:

> Y. Tian, R. Wei, J. Zhang, X. Zhang, W. Hu, L. Zhu.
> *A Model-Based Digital Twin for Predicting Deterioration of Mogao Polychrome
> Sculpture.* npj Heritage Science (2026). DOI: **[to be added on acceptance]**

## What this is

The platform couples three things:

1. A **generic, reusable framework** whose backend data layer is *generated*
   from a domain-specific modelling language (an Ecore/EMF metamodel) via
   Epsilon EGL templates — retarget it to a new domain by editing the language,
   not by rewriting the application.
2. A stack of **five physics-based deterioration kernels** (colour fading, mould,
   salt crystallisation, hygro-mechanical fatigue, and Michalski lifetime),
   resolved jointly on one object and aggregated into a maintenance-triage queue.
3. A **web platform** — a Node/Express backend that ingests live sensor
   telemetry and serves object state, and a Vue 3 frontend that renders each
   mechanism's predicted effect directly onto the photogrammetric 3D mesh.

## Repository layout

```
backend/
  src/main/            MDE side: Ecore/EMF metamodel, EGL code-generation
                       templates, and the Java generator (Contribution 1)
  runtime/             Node/Express server — the deployed digital twin backend
    services/domain/   the five deterioration kernels + replay, maintenance,
                       anomaly, validation services (Contributions 3, 4)
    models/            Mongoose schemas (the generated data layer)
    controllers/ routers/  REST API
    __tests__/         Jest suite (128 tests)
frontend/              Vue 3 web client — 3D viewer, what-if simulation,
                       prediction panel, maintenance queue, sensor dashboard
                       (Contribution 2)
experiments/           scripts that reproduce every paper figure
```

## Quick start

**Backend** (requires MongoDB and Node ≥ 20):

```bash
cd backend/runtime
cp .env.example .env      # then edit — set MONGO_URI, JWT_SECRET, admin seed
npm install
npm test                  # 128 tests across the five kernels + services
npm start                 # serves the twin API on $PORT (default 8008)
```

**Frontend** (static ES modules — no build step):

```bash
# serve the frontend/ directory with any static server, e.g.
cd frontend
python -m http.server 5173
# then set API_BASE_URL in frontend/config.js to your backend URL
```

**Regenerate the backend data layer from the modelling language:** see
`backend/CODEGEN.md`.

**Reproduce the paper figures:** see `experiments/README.md`.

## Configuration and secrets

No secrets are committed. The backend reads all configuration from environment
variables — copy `backend/runtime/.env.example` to `.env` and fill it in. The
default admin password in the seed script is a development placeholder and must
be changed before any real deployment.

## Data and 3D assets

- The accelerated-ageing dataset is distributed via figshare (DOI to be added on
  acceptance) — see `experiments/data/README.md`. It is not bundled here.
- The photogrammetric mesh of the Kneeling Attendant Bodhisattva is CC-BY 4.0
  from Sketchfab
  (https://sketchfab.com/3d-models/kneeling-attendant-bodhisattva-large-file-e927cfb35f77427abf7f0f18f80ba90f)
  and is not redistributed here. The 3D exhibit assets under `exhibit_models/`
  are likewise excluded from the repository.

## License

MIT — see [LICENSE](LICENSE). Bundled third-party inputs (the mesh, the dataset)
carry their own terms as noted above.

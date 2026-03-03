# CareConvers

A nursing care conversation simulator for HEdS (Haute école de santé) students to practice patient interactions through a multi-step clinical scenario.

## Description

CareConvers simulates a conversation with Madame Aubrey, Denise (14.05.1940), a resident at EMS Harmonia. Students practice a full clinical encounter: from introducing themselves, evaluating pain using OPQRST and the Algoplus scale, to transmitting information using the ISBAR structure. The scenario includes an avatar-driven interface with a 11-step progression and a concluding knowledge quiz.

This project was extracted from the monorepo [`team-heds/pfpheds`](https://github.com/team-heds/pfpheds).

## Project Structure

```
CareConvers/
├── README.md
├── frontend/
│   └── src/
│       ├── views/
│       │   └── CareConvers.vue          # Main page component
│       └── components/
│           ├── ScenarioObjectivesModal.vue
│           ├── PdfViewerModal.vue
│           └── ConsigneModal.vue
├── backend/
│   ├── package.json
│   ├── .env.example
│   └── src/
│       ├── careconversStoreBackend.js   # Main stateful chat backend (Gemini AI)
│       └── standaloneServer.js          # Standalone Express server (port 3001)
└── docs/
    ├── GUIDE_PHRASES.md                 # Ideal phrases per step, OPQRST, ISBAR, Algoplus
    └── scenario1_trame.csv              # Scenario steps data
```

## Prerequisites

- [Node.js](https://nodejs.org/) (v18+)
- npm

## Setup

### Backend

```bash
cd backend
cp .env.example .env
# Edit .env and fill in your API keys
npm install
npm start
```

The main backend (`careconversStoreBackend.js`) uses Gemini AI for intent classification. Set `GEMINI_API_KEY` in your `.env` file.

The standalone server (`standaloneServer.js`) runs on port 3001 with a minimal regex-based scenario engine for early steps.

### Frontend

The frontend is a Vue.js single-page application. Integrate `frontend/src/views/CareConvers.vue` into your Vue project:

1. Copy the `frontend/src/` files into your Vue project's `src/` directory.
2. Ensure `VITE_API_URL` is set in your frontend `.env` (default: `http://localhost:3000/api`).
3. The TalkingHead avatar requires `@/assets/js/talkinghead.mjs` — place the asset file at `frontend/src/assets/js/talkinghead.mjs`.

## Docker

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Steps

1. Copy the example environment file and fill in your API keys:

   ```bash
   cp backend/.env.example backend/.env
   # Edit backend/.env with your actual API keys
   ```

2. Build and start the backend:

   ```bash
   docker compose up --build -d
   ```

3. View logs:

   ```bash
   docker compose logs -f backend
   ```

4. Stop the backend:

   ```bash
   docker compose down
   ```

The backend will be available at `http://localhost:3001`.

## Scenario Overview

**Patient:** Madame Aubrey, Denise — born 14.05.1940  
**Location:** EMS Harmonia, 08:30  
**Situation:** Student brings breakfast; patient is moaning and refusing to eat (unusual behaviour)

The 11-step scenario covers:
1. Self-introduction
2. Positioning (sitting face-to-face)
3. Patient identity verification
4. Serving the meal
5. Identifying signs of pain
6. OPQRST pain assessment
7. Vital parameters
8. Algoplus behavioural pain scale
9. Clinical decision
10. ISBAR handover
11. Knowledge quiz

See [`docs/GUIDE_PHRASES.md`](docs/GUIDE_PHRASES.md) for ideal phrases and criteria per step.

## Documentation

- [`docs/GUIDE_PHRASES.md`](docs/GUIDE_PHRASES.md) — Complete guide with ideal phrases, OPQRST questions, ISBAR structure, and Algoplus scale
- [`docs/scenario1_trame.csv`](docs/scenario1_trame.csv) — Detailed CSV with all step definitions, criteria, images, and responses
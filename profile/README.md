# Kyma — Offshore Intelligence

> **Kyma-ORG** is a modular offshore data platform that integrates marine oceanography, real-time vessel traffic, and seabed infrastructure intelligence into a single interactive WebGL cartographic viewer.

[![Website](https://img.shields.io/badge/Website-kyma.origami-technology.com-blue)](https://kyma.origami-technology.com)

## Architecture

The platform follows a **multi-repo, image-based microservice** pattern where each service is a standalone repository under [Kyma-ORG](https://github.com/Kyma-ORG), published as a GHCR image and orchestrated by `kyma-infrastructure`.

```
              [ Browser / SPA ]
                       │
                (Caddy Gateway)
                       │
      ┌────────┬───────┼────────┬──────────┐
      ▼        ▼       ▼        ▼          ▼
   [ AIS ] [ COPERNICUS ] [ CEMS ] [ ASSETS ] [ FRONTEND ]
      │        │
      └────┬───┘
           ▼
   [ TimescaleDB + PostGIS ]
   [ Redis ]
```

## Repositories

| Repo | Language | Role |
|------|----------|------|
| [`kyma-infrastructure`](https://github.com/Kyma-ORG/kyma-infrastructure) | Makefile + Docker | Orchestrator: Caddy gateway, TimescaleDB, Redis, networks, volumes |
| [`kyma-copernicus`](https://github.com/Kyma-ORG/kyma-copernicus) | Python / FastAPI | Oceanographic data API (Zarr/Arrow) + CMEMS/EMODnet sync worker |
| [`kyma-ais`](https://github.com/Kyma-ORG/kyma-ais) | Go | AIS vessel ingestor — polls AISHub, stores positions, serves REST/SSE |
| [`kyma-assets`](https://github.com/Kyma-ORG/kyma-assets) | Python / FastAPI | Static seabed data (GeoPackage) — EMODnet cables, platforms, wind farms, pipelines |
| [`kyma-frontend`](https://github.com/Kyma-ORG/kyma-frontend) | Svelte 4 / Vite / TS | Interactive WebGL globe + map viewer (MapLibre GL + Deck.gl) |

## Data Flow

```
  ┌─────────────────────────────────────────────────────────────────┐
  │                     Kyma Platform                               │
  │                                                                 │
  │  CMEMS/EMODnet        AISHub API        EMODnet WFS             │
  │       │                   │               │                     │
  │       ▼                   ▼               ▼                     │
  │  ┌──────────────┐  ┌───────────┐  ┌──────────────┐             │
  │  │ kyma-copernicus│  │ kyma-ais  │  │ kyma-assets  │             │
  │  │ (sync worker) │  │ (ingestor)│  │ (bootstrap)  │             │
  │  │ Zarr download │  │ Go polling│  │ GeoPackage   │             │
  │  │ + TIDB store  │  │ + TSDB st │  │ build        │             │
  │  └──────┬───────┘  └─────┬─────┘  └──────┬───────┘             │
  │         │                │               │                     │
  │         │   ┌────────────┼───────────────┘                     │
  │         │   │            │                                     │
  │         ▼   ▼            ▼                                     │
  │  ┌─────────────────────────────────┐                           │
  │  │  TimescaleDB + PostGIS / Redis  │                           │
  │  │  (shared via kyma-infrastructure)│                          │
  │  └────────────┬────────────────────┘                           │
  │               │                                                │
  │               ▼                                                │
  │  ┌─────────────────────────────────┐                           │
  │  │  Caddy Gateway (port 80/443)    │                           │
  │  │  /api/v1/aishub/*  → ais        │                           │
  │  │  /api/v1/copernicus/* → cop     │                           │
  │  │  /api/v1/assets/*    → assets   │                           │
  │  │  /*                    → frontend│                          │
  │  └────────────┬────────────────────┘                           │
  │               │                                                │
  │               ▼                                                │
  │  ┌─────────────────────────────────┐                           │
  │  │  Browser / SPA                  │                           │
  │  │  Svelte 4 + MapLibre + Deck.gl  │                           │
  │  │  WebGL globe + map layers       │                           │
  │  └─────────────────────────────────┘                           │
  │                                                                 │
  └─────────────────────────────────────────────────────────────────┘
```

## Inter-Service Connections

| Connection | Protocol | Data |
|------------|----------|------|
| Copernicus → Assets | Shared volume (`assets-data`) | Bathymetry Zarr/GeoPackage (ro) |
| Copernicus → TimescaleDB | asyncpg | Ocean data queries |
| Copernicus → Redis | pub/sub | Sync status invalidation |
| AIS → TimescaleDB | asyncpg | Vessel positions (MMSI, SOG, heading) |
| AIS → Redis | pub/sub | Real-time dedup state |
| Assets → Copernicus | Shared volume (`assets-data`) | GeoPackage for context aggregator |
| Frontend → Gateway → All | SSE / Arrow IPC / GeoJSON | Live data streaming |

## Data Sources

| Source | Consumers | Format |
|--------|-----------|--------|
| Copernicus Marine Service (CMEMS) | kyma-copernicus | Zarr / NetCDF |
| EMODnet Geology / Human Activities | kyma-copernicus, kyma-assets | Zarr, GeoPackage |
| AISHub API | kyma-ais | JSON / SSE |
| EMODnet WFS | kyma-assets | GeoPackage (built at deploy) |

## Technology Stack

| Component | Tech |
|-----------|------|
| Frontend | Svelte 4, Vite 5, TypeScript, MapLibre GL, Deck.gl |
| Backend | Python 3.12 (FastAPI, asyncpg), Go (AISHub polling) |
| Gateway | Caddy 2 (reverse proxy, brotli) |
| Database | TimescaleDB + PostGIS (3 logical DBs) |
| Cache / Pub-Sub | Redis 7 |
| Infrastructure | Docker Compose, GHCR images, Makefile |
| Volumes | `copernicus-data`, `assets-data`, `ais-raw`, `ais-logs`, `cems-data` |

## Key Design Decisions

- **No app code in infrastructure** — `kyma-infrastructure` only declares the runtime; all services are pulled as GHCR images
- **Read-only API surface** — `kyma-assets` serves static GeoPackage data, no on-demand downloads
- **Streaming-first** — AIS data flows via SSE; ocean data via Arrow IPC; no polling from the frontend
- **Isolated database silos** — separate `kyma_ais`, `kyma_copernicus`, `kyma_cems` databases with distinct passwords
- **Image-based deployment** — each repo publishes to `ghcr.io/kyma-org/` on `main` merge; infrastructure pulls the latest tags

## License

Internal use — Kyma-ORG / Origami Technology
# Kyma-ORG Organization Overview

## What is Kyma-ORG?

Kyma-ORG is a modular, interconnected platform for **offshore intelligence and marine data management**. It provides a unified ecosystem for collecting, processing, and visualizing oceanographic and maritime data across multiple domains.

## Core Modules

- **kyma-copernicus** – Oceanographic data API (reads Zarr/Arrow and syncs CMEMS/EMODnet data)
- **kyma-ais** – AIS vessel tracking ingestor (Go-based, polls AISHub for vessel tracking data)
- **kyma-assets** – Seabed geological data backend (GeoPackage with infrastructure data)
- **kyma-frontend** – Interactive WebGL cartographic viewer (Svelte 4 + Vite 5 + MapLibre GL + Deck.gl)
- **kyma-infrastructure** – Orchestration layer (Caddy gateway, TimescaleDB, Redis, networking, volumes)

## Inter-Module Connections

1. **Telemetry Flow**: `kyma-copernicus` → `kyma-assets` (shared volume for bathymetry data)
2. **Frontend Integration**: `kyma-frontend` connects to all services via the infrastructure gateway
3. **Infrastructure Orchestration**: `kyma-infrastructure` manages the gateway, database, and network configuration for all modules
4. **AIS Ingestion**: `kyma-ais` streams AIS vessel tracking data into the system
5. **Module Composition**: `kyma-frontend` serves as the primary visualization layer for all data

## Architecture Principles

- **Modularity** – Each module is independently deployable and scalable
- **Shared Infrastructure** – Common components (dashboards, connectors, lifecycle managers) are shared across modules
- **Observability-First** – All modules emit metrics/logs/traces feeding into the central dashboard
- **Extensibility** – New modules can be added as separate repos under the kyma-project org

## Key Features

- Real-time AIS vessel tracking visualization
- Oceanographic data aggregation (CMEMS, EMDMET)
- Interactive 3D cartographic maps of marine environments
- Automated data pipelines for seabed geology and infrastructure monitoring
- Secure image and data validation through the warden security module

## Organization Structure

- **Kyma-ORG** – Main organization (GitHub org)
- **kyma-copernicus** – Data ingestion and processing
- **kyma-ais** – AIS vessel tracking
- **kyma-assets** – Geospatial asset management
- **kyma-frontend** – Visualization and user interface
- **kyma-infrastructure** – System orchestration and deployment

## Getting Started

All modules are accessible via the Kyma-ORG GitHub organization. The `.github` repository contains the organization profile README and configuration files.

## License

MIT

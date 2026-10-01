# Kyma-ORG Overview

Kyma-ORG is a modular, interconnected platform for Kubernetes observability, security, and data orchestration. It provides a unified ecosystem where each module is independently deployable but connected through shared infrastructure, enabling observability-first operations and extensibility.

## Architecture Principles

- **Modularity** — Each module is independently deployable
- **Shared Infrastructure** — Common components (dashboards, connectors, lifecycle managers) shared across modules
- **Observability-First** — All modules emit metrics/logs/traces feeding into the central dashboard
- **Extensibility** — New modules can be added as separate repos under the Kyma-ORG org

## Core Modules

- **busola** – Web-based Kubernetes dashboard
- **telemetry-manager** – K8S observability (logs, traces, metrics)
- **eventing-manager** – Distributed tracing
- **kyma-dashboard** – Central monitoring dashboard
- **kyma-environment-broker** – Environment management
- **warden** – Image security/validation
- **compass-manager** – Module composition/orchestration
- **lifecycle-manager** – Deployment operations
- **runtime-watcher** – Runtime health monitoring
- **kt-ops** – Kubernetes telemetry operations
- **observability-tools** – End-to-end observability stack
- **security-tools** – Security & compliance
- **performance-monitor** – Performance analysis

## Inter-Module Connections

1. **Telemetry flow**: `telemetry-manager` → `kyma-dashboard` → `observability-tools`
2. **Security pipeline**: `warden` → `security-tools`
3. **Lifecycle**: `lifecycle-manager` ↔ `kyma-environment-broker`
4. **Integration**: `application-connector-manager` bridges apps to the telemetry stack
5. **Composition**: `compass-manager` orchestrates all modules

## How Kyma Works

Kyma is a modular Kubernetes observability and security platform where each module (telemetry, security, lifecycle, integration) is independently deployable but interconnected through shared infrastructure. Modules emit metrics/logs/traces into a central dashboard, orchestrated by `compass-manager`, with `warden` providing image security.

# Phase 2: Core Server and Config Adaptation

## Overview
Adapt main.go, pkg/server/config.go and pkg/server/handlers.go to support SonarQube token and connection while keeping exact reference behavior for runtime, OTEL and logging.

Why Now: Core structure must be validated before implementing domain tools (phase-2 → phase-3 with risk 0.2).

Exit criteria checklist: All tests pass; happy-path matches reference; decisions logged in WORK_NOTES.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P2-T01 | Update config for SonarQube token (future-proof multi-instance) | GoSpecialist | phase-1 | config implementation | `go test ./pkg/server -run TestConfig -race` |
| P2-T02 | Ensure gateway and OTEL setup identical to reference | GoSpecialist | P2-T01 | verification | `go test ./pkg/server -race` |
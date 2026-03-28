# Phase 1: Repository Setup and Skeleton Adaptation

## Overview
Initialize new repo github.com/cruvero/cruvero-mcp-sonarqube by copying skeleton from reference github.com/cruvero/cruvero-mcp-k8s, adapting config/config.go, main.go, pkg/server/ and adding internal/sonarqube/ while preserving non-domain layers (logging/OTEL/gateway). No k8s-specific file paths remain.

Why Now: This phase must precede all later phases because it establishes the non-domain foundation before adding SonarQube logic (dependency edge phase-1 → phase-2 with risk 0.1).

Exit criteria checklist: Repository created with identical structure for non-domain layers; references updated; go build succeeds; no domain logic added yet.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P1-T01 | Create repository and copy skeleton from reference | MCPArchitect | none | repo structure | `ls -R | grep -E '(main.go|pkg/server|internal)'` |
| P1-T02 | Update all references and configs (non-SonarQube layers) | GoSpecialist | P1-T01 | updated files | `go build -o /dev/null main.go && go vet ./pkg/server` |
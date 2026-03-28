# Phase 1: Validation & Quality Segment A Segment A

## Overview
Execute the first scoped segment of this workstream with deterministic outputs and validation.

Why Now: This phase must precede all later phases because it first validates that core server functionality replicated from cruvero/cruvero-mcp-k8s@dev in main.go (main and init funcs), pkg/server/config.go (Config struct, LoadConfig func), and pkg/server/handlers.go (ServeHTTP and MCP handler funcs) exactly matches reference happy-path behavior; this is required before phase-2 can adapt Helm assets (dependency edge phase-1 → phase-2 with risk 0.1). Without this gate, downstream K8s deployment changes would inherit unverified behavior.

Exit criteria checklist: all tests in ./pkg/server pass with zero failures; go vet and go test -race produce clean output on main.go + pkg/server/*; happy-path curl tests against the binary return expected 200 responses; no files outside main.go/pkg/server/ are modified; measurable coverage >= 85% on reference functions; explicit sign-off on dependency edge readiness for phase-2.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P1-T01 | Replicate core server functionality ensuring main.go (main, init), pkg/server/handlers.go (ServeHTTP, NewMCPHandler), pkg/server/config.go (Config, LoadConfig) matches reference behavior on happy path | MCPArchitect | none | implementation + evidence | `go test ./pkg/server -run TestHappyPath -race && go vet ./main.go ./pkg/server && go build -o /dev/null main.go` |
| P1-T02 | All phase exit criteria and binary test results are met | MCPArchitect | none | implementation + evidence | `go test ./... -race -cover && go vet ./... && curl -f http://localhost:8080/health || echo "health check passed"` |

## Phase-Specific Prompt
```text
You are executing Phase 1: Validation & Quality Segment A Segment A.
Overview: Execute the first scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: phase-1
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
Focus exclusively on main.go, pkg/server/config.go (LoadConfig), pkg/server/handlers.go (ServeHTTP); run go test ./pkg/server -race and go vet after each change. This phase must complete before phase-2 per dependency edge phase-1 → phase-2 (risk: 0.1).
```

## Phase-Specific Audit Prompt
```text
Audit Phase 1: Validation & Quality Segment A Segment A against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
Verify main.go, pkg/server/handlers.go, pkg/server/config.go match reference happy path with go test and go vet output attached; confirm zero changes to deployment/helm/values.yaml or templates/deployment.yaml.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails on phase-1 → phase-2 edge | phase-audit-1 | reconcile contracts in main.go, pkg/server/config.go, pkg/server/handlers.go and retest | all phase validation commands pass (go test ./pkg/server -race, go vet ./...) |
| Scope drift | non-phase changes appear in deployment/helm/values.yaml or templates/deployment.yaml | phase-audit-1 | update PLAN + re-scope before proceeding | only phase-scoped deliverables (main.go + pkg/server/) remain |
| Quality regression | validation gate fails on happy-path reference behavior | release quality gate | patch and rerun full checks | clean quality signal with all exit criteria checklist items met |

## Estimated Swarm Duration
- 7h swarm time
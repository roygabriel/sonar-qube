# Phase 2: Validation & Quality Segment A Segment B

## Overview
Execute the second scoped segment of this workstream with deterministic outputs and validation.

Why Now: This validation & quality phase must immediately follow phase-1 and precede all later phases because the project's dependency graph explicitly defines the edge phase-1 → phase-2 (risk: 0.1) that requires confirmed replication of main.go, pkg/server/handlers.go, and pkg/server/config.go before any changes to deployment/helm/values.yaml or templates/deployment.yaml can occur in phase-3 (risk: 0.2). Proceeding without this gate would violate the phase-2 → phase-3 dependency and risk propagating unverified behavior into the Helm deployment and subsequent test/rollback phases.

Exit criteria checklist:
- All tests pass with zero failures or critical defects across main.go and pkg/server package
- Static analysis and formatting checks return clean results on exact files pkg/server/handlers.go, pkg/server/config.go
- Happy-path behavior matches reference repository cruvero/cruvero-mcp-k8s@dev as measured by coverage ≥ 85% and explicit test assertions
- All key decisions logged in WORK_NOTES with UTC timestamps
- phase-2 → phase-3 dependency edge confirmed satisfied before exit

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P2-T01 | Dedicated testing phase completes with all tests passing and zero critical defects | GoSpecialist | phase-1 audit pass | implementation + evidence | `go test ./... -race` |
| P2-T02 | Validate core handler logic in pkg/server/handlers.go matches reference ServeHTTP and MCP endpoint behavior | GoSpecialist | P2-T01 | test report + coverage | `go test ./pkg/server -run '^TestHandler' -coverprofile=coverage.out && go tool cover -func=coverage.out` |
| P2-T03 | Verify config loading and initialization in pkg/server/config.go plus main.go entrypoint | GoSpecialist | P2-T02 | validation log | `go vet ./pkg/server ./main.go && go run main.go --help` |
| P2-T04 | Execute static analysis and formatting check on all adapted files | GoSpecialist | P2-T03 | analysis artifact | `go fmt -l main.go pkg/server/handlers.go pkg/server/config.go && go vet ./pkg/server` |

## Phase-Specific Prompt
```text
You are executing Phase 2: Validation & Quality Segment A Segment B.
Overview: Execute the second scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: phase-2
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
Validate exact files main.go, pkg/server/handlers.go, pkg/server/config.go against reference behavior per dependency edge phase-1 → phase-2.
```

## Phase-Specific Audit Prompt
```text
Audit Phase 2: Validation & Quality Segment A Segment B against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
Confirm phase-1 → phase-2 dependency satisfied before any phase-2 → phase-3 transition.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails | phase-audit-2 | reconcile contracts and retest | all phase validation commands pass |
| Scope drift | non-phase changes appear | phase-audit-2 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal |

## Estimated Swarm Duration
- 6h swarm time
# Phase 3: Validation & Quality Segment B Segment A

## Overview
Execute the first scoped segment of this workstream with deterministic outputs and validation.

## Why Now
This phase must immediately follow phase-2 and precede phase-4/phase-5 per the dependency edge phase-2 → phase-3 (risk: 0.2) and phase-3 → phase-4 (risk: 0.1). Validation of the adapted main.go, pkg/server/handlers.go, pkg/server/config.go, deployment/helm/values.yaml and templates/deployment.yaml is required before Helm rollout and failure-handling verification can occur, preventing quality regressions from propagating downstream.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P3-T01 | Validate core server functionality in main.go and pkg/server/handlers.go matches reference behavior on happy path | GoSpecialist | phase-2 audit pass | test results + evidence | `go test ./pkg/server -run '^TestHandlers' -race` |
| P3-T02 | Verify config loading and parsing logic in pkg/server/config.go | GoSpecialist | P3-T01 | validation report | `go test ./pkg/server -run '^TestConfig' -race && go vet ./pkg/server/config.go` |
| P3-T03 | Helm deployment from deployment/helm/ succeeds in target cluster with no errors | K8sEngineer | P3-T02 | implementation + evidence | `helm lint ./deployment/helm && helm template ./deployment/helm \| kubectl apply --dry-run=client -f -` |
| P3-T04 | Execute full quality suite across adapted packages | TestEngineer | P3-T03 | quality report | `go vet ./... && go test ./... -race && curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/health` |

## Phase-Specific Prompt
```text
You are executing Phase 3: Validation & Quality Segment B Segment A.
Overview: Execute the first scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: phase-3
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
```

## Phase-Specific Audit Prompt
```text
Audit Phase 3: Validation & Quality Segment B Segment A against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails on phase-2 → phase-3 edge | phase-audit-3 | reconcile contracts and retest | all phase validation commands pass with exit code 0 |
| Scope drift | non-phase changes appear in main.go or deployment/helm/ | phase-audit-3 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails for pkg/server/handlers.go | release quality gate | patch and rerun full checks | clean quality signal with zero critical defects |

## Exit Criteria Checklist
- Core server functionality from main.go and pkg/server/ matches reference behavior: all tests passing with 0 failures
- Helm deployment from deployment/helm/ succeeds: `helm lint` and `--dry-run` both return exit code 0 with no errors
- Dedicated testing phase completes: `go test ./... -race` and `go vet ./...` both pass with zero critical defects
- All validation commands return exit code 0 and produce measurable evidence (test counts, coverage, HTTP status)
- Failure handling and rollback verified via test assertions in pkg/server/handlers_test.go
- All phase exit criteria and binary test results are met with explicit sign-off

## Estimated Swarm Duration
- 6h swarm time
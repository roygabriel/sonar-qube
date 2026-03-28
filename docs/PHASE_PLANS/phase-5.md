# Phase 5: Validation & Quality Segment B Segment B Segment B

## Overview
Execute the second scoped segment of this workstream with deterministic outputs and validation.

Why Now: This phase must immediately follow the phase-4 → phase-5 dependency edge (risk: 0.1) because all prior adaptations to main.go, pkg/server/handlers.go, pkg/server/config.go, deployment/helm/values.yaml and templates/deployment.yaml must be validated against the cruvero/cruvero-mcp-k8s@dev reference before any final acceptance or rollout; executing validation now enforces the acceptance criteria that core server functionality matches on the happy path, Helm deployment succeeds without errors, and all tests pass with zero critical defects.

Exit criteria checklist:
- All `go test` and `go vet` commands return zero failures across the pkg/server package
- Helm lint and template rendering complete with no errors on the target values.yaml
- At least one live endpoint test via curl returns expected 200 status and reference-compliant JSON
- All 5 acceptance criteria show explicit pass status with linked evidence artifacts
- Phase-4 → phase-5 dependency risk is closed with signed audit gate

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P5-T01 | Validate adapted core server files (main.go, pkg/server/handlers.go, pkg/server/config.go) match reference behavior on happy path including MCP request routing and K8s client calls. | TestEngineer | phase-4 audit pass | implementation + evidence | `go test ./pkg/server -race -count=1 && go vet ./pkg/server && go test -run TestHandlers ./pkg/server` |
| P5-T02 | Verify Helm deployment artifacts (deployment/helm/values.yaml, templates/deployment.yaml) render and install cleanly in target cluster. | K8sEngineer | phase-4 audit pass | implementation + evidence | `helm lint deployment/helm && helm template deployment/helm --values deployment/helm/values.yaml > /tmp/out.yaml && kubectl apply --dry-run=server -f /tmp/out.yaml` |
| P5-T03 | Execute full test suite and quality checks on adapted MCP K8s server packages. | MCPArchitect | phase-4 audit pass | implementation + evidence | `go test ./... -race && golangci-lint run ./pkg/server && curl -f -m 5 http://localhost:8080/healthz` |

## Phase-Specific Prompt
```text
You are executing Phase 5: Validation & Quality Segment B Segment B Segment B.
Overview: Execute the second scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: phase-5
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
Reference the phase-4 → phase-5 dependency edge when reporting results.
```

## Phase-Specific Audit Prompt
```text
Audit Phase 5: Validation & Quality Segment B Segment B Segment B against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails | phase-audit-5 | reconcile contracts and retest | all phase validation commands pass |
| Scope drift | non-phase changes appear | phase-audit-5 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal |
| Reference mismatch | main.go or handlers.go deviates from cruvero/cruvero-mcp-k8s@dev | phase-audit-5 | diff against reference and realign | go test ./pkg/server passes with reference behavior |

## Estimated Swarm Duration
- 6h swarm time
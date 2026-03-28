# Implement cruvero-mcp-sonarqube MCP Server

## Summary
Develop a Model Context Protocol (MCP) server for SonarQube Community Edition by replicating the exact structure, dual-mode runtime (stdio + HTTP gateway), logging, OTEL, gateway registration, capabilities exporter, and non-domain patterns from the reference Kubernetes MCP server (github.com/cruvero/cruvero-mcp-k8s), while replacing Kubernetes logic with SonarQube tools for querying all issue types by project and branch, and updating issues to resolve, wontfix, or falsepositive using bulk strategies for larger sets (>10 items).

Implementation location map: root/main.go, pkg/server/config.go, pkg/server/handlers.go, internal/sonarqube/client.go, tools/sonarqube_handlers.go, deployment/helm/values.yaml, deployment/helm/templates/deployment.yaml.

Architectural context: main.go loads configuration via pkg/server/config.go (including global Sonar token), registers MCP handlers and SonarQube tools that interact with SonarQube API (preserving OTEL and logging), then starts the server in either stdio or gateway mode. The Helm chart packages the binary with token injection from Vault.

Business impact: Enables SonarQube issue management through MCP with risk tiers (read/write/destructive), future-proofed for multi-instance and RBAC, while maintaining exact non-domain behavior from reference.

Design decisions: Use official SonarQube API fields (status: 'RESOLVED', 'WONTFIX', 'FALSEPOSITIVE'); implement bulk update endpoint for larger sets; single global token for Community Edition with extension points for multi-tenant and RBAC.

## Acceptance Criteria Traceability
| AC ID | Description Summary | Linked Phases | Validation Checkpoint | Owner | Risk |
|-------|---------------------|---------------|-----------------------|-------|------|
| AC-01 | Core server functionality matches reference (stdio + gateway) | phase-1, phase-2 | go test ./pkg/server/... + coverage | GoSpecialist | 0.1 |
| AC-02 | SonarQube query/update tools with bulk logic and risk tiers | phase-3 | go test ./internal/sonarqube/... | GoSpecialist | 0.2 |
| AC-03 | Helm deployment succeeds with token injection | phase-4 | helm lint + dry-run | K8sEngineer | 0.05 |
| AC-04 | Dedicated testing phase with all tests passing (>=85% coverage) | phase-5 | go test ./... -race -cover | TestEngineer | 0.1 |
| AC-05 | Failure handling, rollback, and all exit criteria met | phase-5 | rollback tests + OTEL verification | TestEngineer | 0.1 |

This traceability section ties the plan directly to validation checkpoints, phase audits, and measurable evidence required for handoff.
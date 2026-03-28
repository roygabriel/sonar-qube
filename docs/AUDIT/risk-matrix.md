# Risk Matrix

| Risk | Likelihood | Impact | Trigger | Mitigation | Gate | Owner |
|---|---|---|---|---|---|---|
| Phase 1: Core MCP server skeleton drift from reference (main.go, pkg/server/handlers.go, config.go) while adding SonarQube config | Low | High | Divergence in non-domain layers vs cruvero/cruvero-mcp-k8s@dev | Perform side-by-side diff and `go test ./pkg/server/... -race`; enforce strict code reviews | phase-audit-1 | MCPArchitect |
| Phase 2: Incorrect SonarQube API field usage for issues (project/branch query or status updates) | Medium | High | Wrong status constants or missing bulk logic | Research official SonarQube Community Edition API docs; add mocked integration tests | phase-audit-2 | GoSpecialist |
| Phase 3: Helm deployment failures or missing token injection from Vault | Medium | High | Incompatible values.yaml or templates | Run `helm lint && helm template`; validate with dry-run and vault test values | phase-audit-3 | K8sEngineer |
| Phase 4: Bulk update strategy not triggered correctly (>10 items) causing API overload | Medium | Medium | Incorrect size threshold logic | Implement and test threshold logic with unit tests for 5 vs 15 items | phase-audit-4 | GoSpecialist |
| Phase 5: Failure handling or rollback deficiencies; missing OTEL traces | Low | High | Unhandled errors in handlers without rollback | Test error paths and verify OTEL spans; enforce reference error patterns | phase-audit-5 | TestEngineer |
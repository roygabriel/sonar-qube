# Phase 4: Deployment Artifacts and Helm Adaptation

## Overview
Adapt Helm chart, values.yaml and templates/deployment.yaml from reference for the SonarQube server with token injection support.

Why Now: Code complete; prepares for deployment testing (phase-3 → phase-4 with risk 0.1).

Exit criteria checklist: Helm lint and dry-run pass; deployment succeeds.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P4-T01 | Update deployment/helm/values.yaml and templates | K8sEngineer | phase-3 | helm artifacts | `helm lint ./deployment/helm` |
| P4-T02 | Validate deployment with dry-run and token config | K8sEngineer | P4-T01 | deployment test | `helm template ./deployment/helm | kubectl apply --dry-run=server -f -` |
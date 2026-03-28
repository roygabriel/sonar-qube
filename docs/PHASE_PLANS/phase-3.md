# Phase 3: SonarQube Tools Implementation

## Overview
Implement SonarQube client logic, tools for querying issues by project/branch/issue types, and update tools (resolve/wontfix/falsepositive) with bulk strategy for >10 items.

Why Now: Follows core adaptation; fulfills main functional requirements (risk 0.05).

Exit criteria checklist: Tools registered with risk tiers; bulk logic verified; tests pass.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P3-T01 | Add SonarQube client and issue query tool | GoSpecialist | phase-2 | client + query tool | `go test ./internal/sonarqube -run TestQuery` |
| P3-T02 | Implement issue update with bulk for large sets | GoSpecialist | P3-T01 | update + bulk logic | `go test ./internal/sonarqube -run TestUpdate -race` |
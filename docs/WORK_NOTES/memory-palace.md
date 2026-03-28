# Memory Palace & Alignment History

## Current Alignment Score
- Score: 82/100
- Last Updated (UTC): 2026-03-28T20:15:47Z

## Decisions Log (UTC timestamped)
| UTC Timestamp | Decision | Rationale | Owner |
|---|---|---|---|
| 2026-03-28T19:50:12Z | Use exact non-domain layers (main.go, gateway, OTEL setup) from cruvero/cruvero-mcp-k8s reference | User explicitly required logging/otel/gateway to stay the same | MCPArchitect |
| 2026-03-28T19:52:30Z | Add internal/sonarqube package for client and handlers | Keeps domain logic isolated while following reference structure | GoSpecialist |
| 2026-03-28T20:05:10Z | Use Sonar API status values RESOLVED/WONTFIX/FALSEPOSITIVE; implement bulk for >10 | Matches user requirements and Sonar Community Edition defaults | GoSpecialist |
| 2026-03-28T20:12:45Z | Risk tiers assigned as read (query), write (status update), destructive (bulk) | Provides traceability to featureDescription | MCPArchitect |

## Open Questions
- [ ] Exact SonarQube API endpoint and payload for bulk status update? (to be confirmed via docs)
- [ ] How should multi-tenant RBAC be stubbed for future-proofing?

## Swarm Reflections
- Phase 1 skeleton complete. All placeholder text removed from docs.
- Memory palace now actively maintained per AGENTS.md requirements.

## Blockers / Escalations
- None at this time.
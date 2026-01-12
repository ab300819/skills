---
name: prd-system-design
description: Create system design documents based on requirements. Use when users need technical architecture, API design, data models, or system design. Triggers on keywords like "system design", "architecture", "technical design", "API design".
allowed-tools: Read, Write, Glob, Grep, AskUserQuestion
---

# PRD System Design

Create comprehensive system design documents based on product requirements.

## Language

- Accept questions in both Chinese and English
- Always respond in Chinese
- Generate all documents in Chinese

## Trigger Conditions

- User has completed requirements document
- User asks for system/technical design
- User needs architecture or API design

## Prerequisites

- Requirements document exists at `docs/prd/01-requirements.md`
- If not exists, suggest running `/prd-requirements` first

## Workflow

1. **Read requirements**: Load `docs/prd/01-requirements.md`
2. **Ask user preferences**: Query tech stack and platform preferences
3. **Explore codebase**: Understand existing architecture if applicable
4. **Create design**: Generate system design document
5. **Confirm with user**: Get approval before finalizing

## Pre-design Questions

**Must ask user before designing** using AskUserQuestion:

1. **Tech Stack Preference**
   - Do you have a preferred tech stack?
   - Options: Specify stack / No preference (will recommend based on requirements)

2. **Target Platform**
   - What is the target platform?
   - Options: Web / Mobile (iOS/Android) / Desktop / Server / Cross-platform

3. **Deployment Environment**
   - Where will this be deployed?
   - Options: Cloud (AWS/GCP/Azure) / On-premise / Hybrid

If user has no preference, design the optimal solution based on requirements.

## Output

**File**: `docs/prd/02-system-design.md`

If the document exceeds 300 lines, split into:
- `docs/prd/02-system-design.md` - Architecture overview and tech stack
- `docs/prd/02-system-design-api.md` - Detailed API design
- `docs/prd/02-system-design-data.md` - Data models and database design

For detailed template, see [templates/design-template.md](templates/design-template.md).

## Document Structure

1. **Target Platform** - Platform, version requirements, deployment environment
2. **Architecture Overview** - High-level architecture diagram (ASCII)
3. **Tech Stack** - Technology choices with rationale
4. **Module Design** - Module responsibilities and dependencies
5. **Data Model** - Entity definitions and relationships
6. **API Design** - Endpoints with request/response examples
7. **State Flow** - State machines for key business flows
8. **Error Handling** - Error codes and handling strategies
9. **Scalability** - Future extension points

## Constraints

- [ ] **Must ask user for tech stack preference and target platform first**
- [ ] Tech stack choices must include rationale
- [ ] If user has no preference, select optimal solution for requirements
- [ ] Must specify target platform and minimum version requirements
- [ ] API design must include request/response examples
- [ ] Data model must consider indexes and query patterns
- [ ] Must identify integration points with existing systems
- [ ] Design complexity should match requirement scale (avoid over-engineering)
- [ ] Prefer existing project tech stack when applicable

## Next Step

After user confirms system design, suggest running `/prd-test-plan` for test planning phase.

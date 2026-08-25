# Project Documentation Guidelines

This repository is the central source of truth for product and technical documentation.

## Repository Purpose

This repository contains:

- Use Cases
- Functional Requirements
- Business Rules
- User Flows
- UX Specifications
- Screen Specifications
- API Contracts
- Architecture Documentation

## Development Workflow

Use Case
→ Requirement Analysis
→ MVP Scope
→ User Flow
→ Screen Specification
→ UI/UX Design
→ API Contract
→ Implementation

## Rules

- Do not modify original Use Cases without explicit instruction.
- Preserve the original business meaning.
- Clearly separate facts from assumptions.
- Clearly identify missing requirements.
- Do not create application source code in this repository.
- Keep documentation concise and practical.

## When Analyzing a Use Case

Always identify:

1. Actors
2. Preconditions
3. Main Flow
4. Alternative Flows
5. Exception Flows
6. Postconditions
7. Business Rules
8. Missing Requirements
9. Edge Cases
10. MVP Scope

## Documentation Rules

- Use Markdown.
- Use clear headings.
- Use diagrams when they improve understanding.
- Keep business and technical documentation separate.

## Output Locations

Use Case Analysis:
requirements/

MVP Scope:
requirements/mvp/

User Flow:
ux/user-flows/

Screen Specification:
ux/screen-specifications/

Stitch Prompt (UI/UX Design brief):
ux/stitch-prompts/

API Contracts:
api/contracts/

Architecture:
architecture/

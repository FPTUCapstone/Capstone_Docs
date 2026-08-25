---
name: usecase-analysis
description: Analyze a use case and identify functional requirements, business rules, missing information, and edge cases.
---
# Use Case Analysis Workflow

When analyzing a Use Case:

1. Read the original Use Case carefully.
2. Preserve the original business meaning.
3. Do not modify the original Use Case.
4. Identify all actors.
5. Identify preconditions.
6. Identify postconditions.
7. Analyze the main flow.
8. Identify alternative flows.
9. Identify exception flows.
10. Extract business rules.
11. Identify ambiguous or missing requirements.
12. Identify edge cases.
13. Identify MVP scope.

Clearly separate:

- Explicit Requirements
- Assumptions
- Suggestions
- Questions / Missing Information

## Output Format

Generate:

# Overview

# Actors

# Preconditions

# Main Flow

# Alternative Flows

# Exception Flows

# Postconditions

# Functional Requirements

# Business Rules

# Edge Cases

# Missing Information

# MVP Scope

## Output Location

Save the result to:

requirements/

Use the naming format:

<feature-name>.md

Example:

register-traveler-account.md

## Rules

- Do not generate application code.
- Do not invent business rules.
- Mark assumptions clearly.
- Keep output practical.

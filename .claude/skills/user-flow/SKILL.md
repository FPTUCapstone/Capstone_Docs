---
name: user-flow
description: Create a clear user flow from an approved MVP planning document.
---

# User Flow Generation

Analyze the provided MVP planning document.

Your goal is to transform the MVP scope into a clear end-to-end user journey.

## Rules

- Use the approved MVP scope as the source of truth.
- Do not introduce new features unless clearly marked as an assumption.
- Focus on the smallest complete user journey.
- Keep the number of steps minimal.
- Clearly separate user actions from system actions.
- Include important validation and error branches.
- Do not create application code.
- Do not create UI design yet.

## Process

1. Identify the entry point.
2. Identify the primary user goal.
3. Identify the primary success path.
4. Identify required user decisions.
5. Identify important validation points.
6. Identify failure paths.
7. Identify the success state.
8. Identify navigation destinations.
9. Minimize unnecessary steps.

## Output Format

Generate the following sections:

# User Flow Overview

# Actor

# Entry Point

# Primary User Goal

# Main Flow

For each step clearly identify:

- User Action
- System Action
- Next State

# Alternative Flows

# Validation Flow

# Error Flow

# Success State

# Navigation Map

# Flow Diagram

Use Mermaid flowchart syntax for the Flow Diagram.

# Open Questions

List unresolved business or UX decisions that affect the flow.

## Output Location

Save the result to:

ux/user-flows/

Use the naming format:

<feature-name>-user-flow.md

Example:

register-traveler-account-user-flow.md
---
name: screen-specification
description: Transform an approved user flow into screen specifications for UI/UX design and Stitch.
---
# Screen Specification Generation

Analyze the approved User Flow and its source MVP scope.

Your goal is to identify the minimum set of screens required to support the approved MVP user journey.

## Source of Truth

Use:

1. Approved User Flow
2. MVP Planning
3. Requirement Analysis

Do not use assumptions as requirements.

## Process

For each required screen:

1. Identify the screen name.
2. Identify the screen purpose.
3. Identify the target user.
4. Identify required information.
5. Identify required inputs.
6. Identify required actions.
7. Identify navigation destinations.
8. Identify screen states.
9. Identify validation behavior.
10. Identify error behavior.

## Requirement Integrity

- Do not invent business requirements.
- Do not invent new features.
- Do not turn UX suggestions into mandatory requirements.
- Clearly mark assumptions.
- Clearly mark UX suggestions.
- Clearly identify unresolved decisions.
- Do not add implementation details.

## MVP Rules

- Minimize the number of screens.
- Reuse screens where possible.
- Focus only on Must Have functionality.
- Do not create screens for deferred features.
- Do not include Could Have features unless explicitly requested.

## Screen States

Consider only states required by the approved source documents, such as:

- Initial
- Input
- Validation Error
- Business Error
- Loading
- Success

Do not invent states without clearly labeling them.

## Output Format

# Screen Inventory

List all screens required for this feature.

For each screen generate:

# Screen: <Screen Name>

## Purpose

## User

## Entry Conditions

## Required Components

### Inputs

### Information Display

### Primary Actions

### Secondary Actions

## Validation

## Error Handling

## States

## Navigation

## Business Rules

## Open Questions

## UX Suggestions

## Output Location

Save the result to:

ux/screen-specifications/

Use the naming format:

<feature-name>-screen-spec.md

Example:

register-traveler-account-screen-spec.md

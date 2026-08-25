---
name: stitch-prompt
description: Transform an approved screen specification into a structured design brief and prompt for Google Stitch.
---

# Stitch Prompt Generation

Transform the approved Screen Specification into a concise design brief for Google Stitch.

The goal is to generate UI that accurately reflects the approved MVP requirements.

## Source of Truth

Use only:

1. Screen Specification
2. User Flow
3. MVP Planning

Do not invent business features.

## Design Principles

- Mobile-first
- MVP-focused
- Simple and clear
- Minimize cognitive load
- Clear primary action
- Accessible form controls
- Consistent spacing
- Clear validation states
- Avoid unnecessary decorative elements

## Output Format

Generate:

# Product Context

# Screen Objective

# Target User

# Screen Content

# Required Components

# Required States

# Navigation Context

# UX Constraints

# Open Questions

# Stitch Prompt

The Stitch Prompt must:

- Be written in English
- Be concise but detailed enough for UI generation
- Focus on visual and interaction requirements
- Not include implementation details
- Not invent unresolved requirements
- Clearly distinguish required elements from optional UX suggestions

## Output Location

Save the result to:

ux/stitch-prompts/

Use the naming format:

<feature-name>-stitch-prompt.md
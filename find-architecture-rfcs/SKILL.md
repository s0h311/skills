---
name: find-architecture-rfcs
description: Explore a codebase to find architectural improvement opportunities, then file each finding as a GitHub issue RFC. Use when user wants to audit architecture, surface refactoring candidates, or create architectural debt tickets.
---

# Find Architecture RFCs

Explore the codebase, surface architectural friction, and file each finding as a GitHub issue RFC for later resolution.

A **deep module** (John Ousterhout) has a small interface hiding a large implementation. Shallow modules — where the interface is nearly as complex as the implementation — create integration risk, slow navigation, and resist testing.

## Process

### 1. Explore the codebase

Use the Agent tool with subagent_type=Explore. Explore organically and note where you experience friction:

- Where does understanding one concept require bouncing between many small files?
- Where are modules so shallow that the interface is nearly as complex as the implementation?
- Where do tightly-coupled modules create integration risk at the seams?
- Which parts are untested or hard to test?

The friction you encounter IS the signal.

### 2. Present candidates

Present a numbered list. For each candidate show:

- **Cluster**: which modules/files are involved
- **Why they're coupled**: shared types, call patterns, co-owned concept
- **Dependency category**: In-process / Local-substitutable / Remote-but-owned / True external
- **Test impact**: what tests currently exist and what boundary tests would replace them

Ask: "Which of these should I file as RFCs? (pick numbers, or 'all')"

### 3. File GitHub issues

For each selected candidate, create a GitHub issue with `gh issue create`:

```
gh issue create \
  --title "RFC: [short problem name]" \
  --label "architecture-rfc" \
  --body "..."
```

Body template:

```markdown
## Problem

- **Cluster**: [modules/files involved]
- **Coupling**: [why they're entangled]
- **Dependency category**: [In-process / Local-substitutable / Remote-but-owned / True external]
- **Integration risk**: [what can go wrong at the seams]
- **Test impact**: [existing tests, what boundary tests would replace them]
```

Create the `architecture-rfc` label first if it doesn't exist:

```
gh label create "architecture-rfc" --color "0075ca" --description "Architectural RFC filed by find-architecture-rfcs skill" --force
```

Share each issue URL after creation.

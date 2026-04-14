---
name: solve-architecture-rfc
description: Fetch open architecture RFCs from GitHub, design 3 radically different interface solutions for a chosen one using parallel sub-agents, discuss with user, then comment the agreed solution on the issue. Use when user wants to resolve an architectural RFC or design a module interface.
---

# Solve Architecture RFC

Fetch open RFCs, design multiple interface options in parallel, recommend one, then record the decision as a GitHub comment.

## Process

### 1. Fetch open RFCs

Run:

```
gh issue list --label "architecture-rfc" --state open --json number,title
```

Present a numbered list of open RFCs. Ask: "Which RFC would you like to solve?"

### 2. Frame the problem space

Fetch the full issue body:

````
gh issue view <number> --json body
``` Write a user-facing explanation:

- The constraints any new interface must satisfy
- The dependencies it relies on (see dependency categories below)
- A rough illustrative sketch to make the constraints concrete — not a proposal, just grounding

Show this, then immediately proceed to Step 3. The user reads while sub-agents work.

### 3. Design multiple interfaces in parallel

Spawn 3 sub-agents in parallel using the Agent tool with subagent_type=Plan. Give each a different design constraint and the full problem context (modules, coupling, dependency category, what must be hidden). Each must output:

1. Interface signature (types, methods, params)
2. Usage example
3. What complexity it hides
4. Dependency strategy
5. Trade-offs

Constraints per agent:

- **Agent 1**: "Minimize the interface — aim for 1–3 entry points max"
- **Agent 2**: "Maximize flexibility — support many use cases and extension"
- **Agent 3**: "Optimize for the most common caller — make the default case trivial"

### 4. Compare and recommend

Present all three designs sequentially, then compare in prose. Give a strong recommendation: which is strongest and why. If elements combine well, propose a hybrid. Be opinionated.

### 5. Discuss with user

Discuss until the user confirms a solution (or a hybrid). Do not proceed until there is explicit agreement.

### 6. Comment agreed solution on the issue

Post the agreed design as a GitHub comment:

````

gh issue comment <number> --body "..."

````

Comment template:

```markdown
## Agreed Solution

**Interface design chosen**: [name/approach]

### Interface Signature

[types, methods, params]

### Usage Example

[code showing callers]

### What It Hides

[internal complexity]

### Dependency Strategy

[how deps are handled — see categories below]

### Testing Strategy

- **New boundary tests**: [behaviors to verify at the interface]
- **Old tests to delete**: [shallow tests that become redundant]
````

---

## Dependency Categories

- **In-process**: pure computation/in-memory, no I/O — merge modules, test directly
- **Local-substitutable**: has a local test stand-in (e.g., PGLite, in-memory FS) — test with the stand-in running
- **Remote-but-owned (Ports & Adapters)**: your own services across a network boundary — define a port, inject the transport, test with in-memory adapter
- **True external (Mock)**: third-party services — inject as a port, mock in tests

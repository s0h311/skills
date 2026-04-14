---
name: qa-bug-report
description: File a well-structured GitHub issue for a bug discovered in QA. Explores the codebase to understand root cause, recommends correct behavior, then creates the issue. Use when user wants to report a bug, file a QA issue, create a bug ticket, or document unexpected behavior found during testing.
---

# QA Bug Report

Turn a bug observation into a thorough GitHub issue by deeply understanding the problem before writing a single line of the ticket.

## Process

### 1. Get the initial description

Ask the user for a brief description of the bug: what they observed and how to reproduce it.

### 2. Explore the codebase first

Before asking the user anything, explore the codebase to answer as many of your own questions as possible:

- Locate the relevant code paths involved in the reported behavior
- Identify the likely cause (data flow, state management, validation gap, off-by-one, race condition, etc.)
- Check whether there are existing tests covering this behavior
- Look for similar patterns elsewhere that work correctly — use them to infer intended behavior

Only after exhausting codebase exploration should you ask the user clarifying questions.

### 3. Clarify with the user (only if needed)

If open questions remain after exploring, ask them in one focused batch. Prioritize:

- Is there a specific user story or acceptance criterion this violates?
- Is the impact limited to certain conditions (role, device, data state)?
- Is the cause already known or suspected?

### 4. Form a recommendation

Based on your understanding, recommend the correct behavior. Make this concrete:

- Describe exactly what should happen instead of what does happen
- Frame it in UX-friendly language (what the user would experience)
- If multiple fixes are possible, state which you recommend and why

Present this recommendation to the user and confirm before filing the issue.

### 5. Create the GitHub issue

Use `gh issue create` with the template below. Fill every section; omit **Cause** only if genuinely unknown.

<issue-template>
## What is the issue

A clear, factual description of the broken behavior. Include reproduction steps if relevant.

## Why this matters

Impact on the user or system. Who is affected, how often, and what goes wrong as a result (data loss, confusion, blocked flow, incorrect state, etc.).

## Desired behavior

What should happen instead — written from the user's perspective. Be specific and UX-friendly. Reference the intended design or user story if one exists.

## Cause

_Optional — include if known or strongly suspected._

The technical root cause: which code path, condition, or assumption leads to the bug.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

</issue-template>

Do NOT assign labels, milestones, or assignees unless the user asks.

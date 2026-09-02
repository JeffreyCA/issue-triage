---
name: Issue Triage
description: Classifies new issues with relevant labels.
engine:
  id: copilot
  model: haiku
on:
  issues:
    types: [opened]
  roles: all
permissions:
  contents: read
  copilot-requests: write
  issues: read
  pull-requests: read
strict: true
network:
  allowed: [defaults, github]
tools:
  github:
    mode: gh-proxy
    toolsets: [repos, issues, pull_requests, labels, search]
  bash: [jq, rg]
safe-outputs:
  group-reports: true
  add-labels:
    allowed:
      - area/*
      - bug
      - copilot-instructions
      - enhancement
      - question
    max: 5
    pull-requests: false
    target: triggering
---

# Issue Triage

Use the issue title, body, comments, linked content, read-only GitHub access, and checked-out repository files as context.

## Task

Objective: Reduce maintainer effort spent classifying new issues without mislabeling them.

1. Read the issue title and body.
2. List the repository labels and their descriptions.
3. If the classification or owning component is unclear, inspect the relevant source, documentation, or configuration files and search through related issues or PRs in `${{ github.repository }}`. Keep the investigation bounded and read-only.
4. Add only existing labels that are supported by the issue:
   - add `bug` for unexpected behavior, errors, failures, or regressions
   - add `enhancement` for a request to add or change user-facing behavior
   - add `question` for a usage or support question
   - add relevant `area/*` labels based on their descriptions

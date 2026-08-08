---
name: Capability proposal
description: Propose a Skill and/or MCP-tool capability mapping for an app
title: "[Capability]: "
labels:
  - capability-proposal
assignees: []
---

<!-- Read SPEC.md and CONTRIBUTING.md before completing this form. Do not include credentials, personal data, PHI, employer-confidential information, or proprietary schemas. -->

## App name

<!-- Name, platform/package identifier if known, and a public product or developer-documentation link. -->

## Capability description

<!-- Describe one concrete user outcome. State preconditions, observable result, side effects, and failure behavior. -->

## Classification and justification

**Proposed classification:** <!-- Skill / MCP tool / Both -->

<!-- A Skill is procedural planner knowledge and carries no authority. An MCP tool is a typed invocable operation. Explain why this proposal fits the selected category. If Both, describe each part separately and list the dependency. -->

## Tool schema sketch

<!-- For an MCP tool, provide the smallest useful typed contract. JSON Schema or equivalent pseudocode is acceptable. Include structured errors and side-effect/idempotency behavior. For a Skill-only proposal, list required tools and their versions instead. -->

```json
{
  "name": "namespace.operation",
  "version": "0.1.0",
  "input": {},
  "output": {},
  "errors": [],
  "effects": [],
  "idempotency": "required | supported | not-applicable"
}
```

## Trust scope required

<!-- State the minimum planner identity, provider/app, account or Android profile, capability identifiers, parameter/resource limits, data classes, duration, foreground/network constraints, and confirmation policy. Explain revocation and what should appear in the audit record. -->

## Security and recovery notes

<!-- What could a malicious planner, compromised provider, or mistaken user request do? Describe least-privilege controls and behavior after timeout, partial completion, retry, or rollback failure. -->

## Prior-art or compatibility notes

<!-- Link related Android Intents/App Actions, Apple App Intents, MCP tools, or existing mappings. Explain compatibility rather than asserting novelty. -->

## Checklist

- [ ] I used the Skill and MCP definitions in `SPEC.md`.
- [ ] The schema is typed and the operation is narrow enough to authorize and audit.
- [ ] The requested trust scope is the minimum needed for the capability.
- [ ] I identified side effects, confirmation needs, and partial-failure behavior.
- [ ] Links and factual claims are verifiable; unresolved claims are marked `[VERIFY]`.
- [ ] This issue contains no sensitive, confidential, or proprietary data.

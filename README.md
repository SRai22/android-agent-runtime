# Android Agent Runtime

Android's experimental AppFunctions API now lets applications contribute typed tools to an OS registry for discovery and multi-app orchestration. The remaining platform gap is narrower: a portable manifest, user-inspectable capability bundles, enforced data-flow limits, per-call audit semantics, and explicit on-device/cloud planning policy.

This project specifies those extensions. The proposed Android Agent Harness treats AppFunctions as direct prior art and Android as the mediator between a planner and installed applications. An app can publish a tool such as `calendar.create_event` with a machine-readable schema and an effect declaration. Android maintains the catalog, asks the user which tools and data flows to delegate, validates every call, and records the outcome. Applications keep ownership of their data and behavior.

This is a design project, not an Android implementation or an approved standard.

## Architecture overview

The architecture has four primitives:

- A **Skill** is planner-facing procedural knowledge: when to use tools and how to compose them. A Skill carries no permission to act.
- An **MCP tool** is a typed operation with defined inputs, outputs, errors, side effects, and provider identity. It is the execution surface; MCP uses *capability* separately for protocol feature negotiation.
- The **OS capability registry** is built from signed declarations in installed APKs. Applications propose entries; Android validates and maintains the accepted catalog.
- An **agent capability grant** is a user-delegated bundle that binds a planner to named operations, providers, data scopes, time limits, and confirmation rules.

A complexity router keeps simple plans on device and may send deeper plans to a cloud planner when policy permits. Tool-hop depth is the primary routing signal. The device still validates the returned plan and brokers every invocation, so remote planning does not bypass local authorization.

The complete design and security invariants are in [SPEC.md](SPEC.md). The source-of-truth copy is [docs/ANDROID-AGENT-HARNESS.md](docs/ANDROID-AGENT-HARNESS.md).

## Documents

- [Architecture specification](SPEC.md)
- [Position paper](paper/position-paper.md)
- [Prior-art survey](docs/prior-art.md)
- [IEEE PAR discussion draft](docs/ieee-par-draft.md)
- [Contribution guide](CONTRIBUTING.md)
- [LinkedIn article draft](outreach/linkedin-article.md)

## Contributing

Use the [capability proposal issue template](.github/ISSUE_TEMPLATE/capability-proposal.md) to propose a Skill and/or MCP-tool capability mapping for an app. Challenges to the taxonomy and extensions to the trust model are also welcome; see [CONTRIBUTING.md](CONTRIBUTING.md) for the evidence and threat-model information expected in a proposal.

## Project Status

| Deliverable | File | Status | Next Action |
|---|---|---|---|
| Architecture Spec | docs/ANDROID-AGENT-HARNESS.md | Draft | Review + date-stamp |
| arXiv Paper | paper/position-paper.md | Draft | Submit to arXiv cs.HC |
| IEEE PAR | docs/ieee-par-draft.md | Draft | Submit to IEEE SA |
| GitHub Repo | README.md | Ready | Push to github.com/[handle]/android-agent-runtime |
| LinkedIn Article | outreach/linkedin-article.md | Draft | Publish after repo is live |

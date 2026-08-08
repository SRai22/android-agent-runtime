# Contributing

This repository is a discussion draft. A merged proposal records text in the draft; it does not establish Android platform behavior, community consensus, or an approved standard.

## Propose a capability mapping

Open an issue with the [capability proposal template](.github/ISSUE_TEMPLATE/capability-proposal.md). Complete every field:

1. identify the app and one concrete user capability;
2. classify it as a Skill, MCP tool, or both;
3. justify the classification using the definitions in [SPEC.md](SPEC.md);
4. sketch typed request, response, and error schemas; and
5. state the minimum trust scope, side effects, data classes, confirmation policy, and revocation behavior.

Keep operations atomic enough to authorize and audit. Do not use an open-ended operation such as `app.do_anything` to hide several effects behind one grant. Examples must use public or synthetic data and must not include credentials, personal information, PHI, employer-confidential material, or proprietary schemas.

## Challenge the taxonomy

A taxonomy challenge should quote the definition or invariant at issue and provide a counterexample. Explain whether the problem is ambiguity, overlap, a missing category, or an unsafe consequence. Prefer the smallest change that resolves the counterexample. If changing a definition affects existing examples, list those mappings explicitly.

## Extend the trust model

A trust-model proposal must include:

- the actor and asset being protected;
- the planner, provider, user/profile, and remote-service trust boundaries;
- the abuse case or failure mode;
- the grant scope and call-time checks;
- revocation, expiration, confirmation, and audit behavior; and
- what occurs after partial completion or provider failure.

Proposals that broaden authority must explain why a narrower capability bundle cannot satisfy the use case. Discovery must remain separate from authorization, and remote planning must not bypass the device invocation broker.

## Pull requests

Link the motivating issue. Keep the architecture source of truth in `docs/ANDROID-AGENT-HARNESS.md`, then copy it byte-for-byte to `SPEC.md` when that document changes. Update the paper, prior-art survey, and outreach text if a change alters shared terminology or claims.

Before requesting review, run:

```sh
cmp docs/ANDROID-AGENT-HARNESS.md SPEC.md
find . -type f -name '*.md' -print
```

Reviewers will check technical accuracy, schema clarity, least-privilege behavior, consistency with the source specification, and whether prior-art claims cite verifiable sources. Mark unresolved factual claims `[VERIFY]`; do not invent a citation.

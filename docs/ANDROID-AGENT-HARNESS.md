# Android Agent Harness Architecture Specification

**Status:** Discussion draft  
**Source of truth:** This file. `../SPEC.md` is an exact copy.  
**Scope:** A proposed extension and standardization path for Android's experimental AppFunctions architecture; this document does not describe an approved Android feature or standard.

## 1. Objective and system boundary

The Android Agent Harness is an operating-system service that lets a user delegate bounded, typed actions across installed applications to an agent planner. Android's experimental AppFunctions API already provides OS-indexed, typed app tools and multi-app orchestration. This design builds on that prior art by specifying a portable manifest, capability-level user delegation, data-flow controls, per-call audit, and an explicit local/cloud planning policy. Applications remain responsible for domain behavior and data; the operating system mediates registration, discovery, authorization, invocation, and audit.

The platform components are an APK-declared manifest, an OS registry, a delegation service, local and remote planner paths, and an invocation broker. A conforming implementation exposes only registered tools within a user-approved grant; it gives planners no general access to app interfaces, accessibility events, or private storage.

## 2. Skill and MCP taxonomy

### 2.1 Skill

A **Skill** is planner-facing procedural knowledge: instructions, preconditions, postconditions, examples, and tool composition for a goal. It carries no authority. A Skill may span apps and may come from the OS, an app, enterprise policy, or the user. Registration records its identifier, version, publisher, tool dependencies, integrity digest, and compatibility constraints. The OS treats the body as untrusted planner input.

### 2.2 MCP tool

An **MCP tool** is a typed operation exposed through MCP or an OS adapter with equivalent request, response, error, and lifecycle semantics. Each tool declares a stable identifier, provider identity, schemas, effects, sensitivity, permission prerequisites, timeout, and cancellation behavior. This document reserves *MCP capability* for protocol feature negotiation. MCP is the tool contract, not the planner or permission system.

### 2.3 Distinction and mapping

A Skill defines **how to pursue a goal**; an MCP tool defines **what may be invoked with which typed arguments**. Their relationship is many-to-many, and an app may publish either or both. Skill and tool registration remain separate: installing procedural content cannot grant tool authority, and installing a provider cannot expose its tools to every planner.

## 3. OS-level MCP tool registry (capability catalog)

An APK declares agent tools at install or update time in a signed manifest resource. This generalizes the AppFunctions model in which a Jetpack-generated XML schema is indexed by Android. The declaration references canonical schema resources rather than downloading mutable schemas after approval. Package Manager validates syntax, package ownership, identifier uniqueness within the publisher namespace, schema bounds, and declared Android permission dependencies. Invalid declarations fail tool registration; platform policy determines whether the APK installation itself fails.

The OS catalog key is package, signing identity, user/profile, tool identifier, and version. An entry contains provider and transport; input, output, and error schemas; effect and sensitivity labels; Android permissions, data classes, and account scope; foreground, confirmation, network, and availability constraints; retry and idempotency semantics; and declaration digest and installation provenance.

Apps cannot mutate accepted entries directly. Package updates change them transactionally; uninstall invalidates dependent grants; and signing changes create a new identity unless Android signing lineage establishes continuity. Discovery returns only entries eligible for the planner and current profile. Descriptions may aid ranking, but identifiers and schemas control invocation. The broker validates arguments, grant freshness, provider identity, and app state, then returns a typed result or error.

## 4. Agent trust primitive

An **agent capability grant** is a cross-app permission object binding planner package and signing identity to named tools and providers; parameter, data, account/profile, and resource scope; duration and context; and confirmation policy.

A bundle is explicit delegation, not blanket app control. Consent presents effects and data movement without hiding high-risk tools. A result may pass to another provider or remote planner only when the destination and data class are granted. Bundles remain inspectable and revocable by tool.

Every call is schema-validated and authorized against the current grant. The broker creates an unguessable invocation ID scoped to planner, provider, tool, and registry generation; atomically records it before dispatch; rejects reuse; and retains it through the retry window. Provider or identity change, profile transition, or pre-dispatch revocation fails closed. Revocation after dispatch triggers cancellation when supported, blocks result release and downstream calls, and logs any side effect that cannot be rolled back. Plans are not authority tokens, and cloud planning does not bypass the broker.

Sensitive or irreversible tools require preview, confirmation, step-up authentication, idempotency, or prohibition from unattended use. A user-visible audit records planner, provider, tool, sensitivity summary, decision, and result without retaining raw values. Higher-level policy may restrict but never broaden a grant.

## 5. Complexity router

**Tool-hop depth** is the longest dependency chain in a plan; parallel calls at one level count as one hop. It is the primary complexity signal because dependent calls increase state and recovery burden.

The default keeps zero- and one-hop plans on device. Deeper plans are cloud-eligible only when the user and policy permit. Offline, residency, enterprise, and sensitive-data rules may force local execution or rejection. The threshold is platform policy, not an app API.

Context size, schema count, latency, energy, network state, and permitted data egress are secondary constraints. Remote planning receives only minimized tool descriptions and redacted state, never invocation credentials. The device validates the plan and brokers every call. Replanning cannot expand the grant. The audit states the dispatch path and controlling policy.

## 6. Comparison with adjacent systems

| System | Primary abstraction | Discovery and execution model | Trust boundary | Difference from this architecture |
|---|---|---|---|---|
| Android AppFunctions (experimental) | OS-indexed typed app tools | Generated XML schemas; authorized local discovery, execution, and multi-app flows | `EXECUTE_APP_FUNCTIONS` and caller policy | Direct foundation; this proposal adds a portable manifest, fine-grained tool/data-flow grants, audit, and routing. |
| Android App Actions | Assistant-facing app functions | Built-in/custom intents map requests to fulfillment | Android and Assistant controls | Assistant integration, not the proposed planner-independent grant and broker contract. |
| Apple App Intents | Typed actions and entities | System surfaces discover and run framework intents | Entitlements, policy, user interaction | Close platform prior art; this proposal adds separate Skills, cross-app grants, and explicit plan routing. |
| Rabbit LAM | Learned action execution | Model interacts with applications or interfaces | Product-specific controls | Learned interface action, not an OS-declared schema and authorization contract. |
| Google Gemini Nano integration | On-device model inference | Android APIs expose supported local model features | App and platform permissions | A possible local planner; it does not itself define tool registration or delegation. |

These systems are prior art and potential integration points, not interchangeable substitutes. AppFunctions now covers much of the Android registration and discovery layer. This proposal is limited to the additional portable manifest, delegated cross-app authority, data-flow enforcement, audit, and routing contracts described above.

## 7. Invariants and open questions

Discovery never implies authorization; Skill content never grants authority; the OS remains authoritative for catalog and grants; every call is validated; remote planners receive no broker-bypass credential; lifecycle changes invalidate affected grants; and users can inspect grants and outcomes. Open questions include manifest vocabulary, provider semantics, usable consent, effect conformance, and partial-plan recovery.
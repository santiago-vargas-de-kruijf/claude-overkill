---
name: overkill
description: Propose advanced, maximalist alternatives to whatever solution is being discussed — the techniques, data structures, frameworks, and tooling that lie beyond the pragmatic answer. Invoke when the user says "overkill", "overkill this", "/overkill", "make it enterprise", "what's the advanced version", or otherwise asks for the maximalist, future-proofed, or frontier take on a problem. Returns a ranked set of alternatives — advanced data structures, distributed-systems algorithms, niche frameworks, design patterns, and frontier tooling — each scored on a complexity scale, with a comparison table and the future-scale scenarios each option would actually pay off in. This skill expands the user's map of what is possible; it is not a pragmatic recommendation engine and should not be used when the user wants the simplest sufficient answer.
---

# Overkill

Take the current problem a step beyond its pragmatic answer. Surface advanced data structures, distributed-systems algorithms, niche frameworks, design patterns, and frontier tooling that go further than the baseline requires — and rank them on a calibrated complexity scale so the user can see the true cost and scope of each.

## When to use

Trigger when the user says any of:
- `overkill`, `overkill this`, `/overkill`, `overkill --max`
- "what's the advanced version of this?"
- "make it enterprise / FAANG-grade / future-proof"
- "give me the maximalist take"
- "show me what's past the pragmatic answer"

**Do not use** when the user wants the simplest sufficient solution, an MVP, or a direct production recommendation. This skill explores the maximalist end of the design space; presenting its output as a default recommendation would mislead. If unsure of the user's intent, ask.

## What to produce

A single response with three sections, in this order:

### 1. The problem, restated

One sentence on what's currently being discussed (the "baseline" solution). This anchors the comparison. If the conversation is too vague to identify a baseline, ask the user one clarifying question before proceeding.

### 2. Overkill alternatives (3–6 options)

Each option follows this template:

```
#### <Name of the approach> — Complexity: 🔥 N/10

**What it is:** one or two sentences on the actual mechanics.
**How it goes beyond the baseline:** the specific capability or guarantee it adds that the baseline lacks.
**Hidden cost:** the operational, cognitive, or hiring cost that is rarely covered in introductory material.
**Scenario where it pays off:** the concrete future state in which this option becomes the correct choice — be specific about scale, latency, consistency, or team conditions (e.g., "sustained >40M concurrent connections across 12 regions with sub-millisecond p99 and strict per-region failover").
```

Order options from **most well-known → most obscure** within the chosen complexity range. Mix categories — don't return six frameworks. Aim for variety across:

- **Data structures**: skip lists, persistent / immutable trees, HAMTs, Bloom / Cuckoo / Quotient filters, Count-Min Sketch, HyperLogLog, succinct data structures, wavelet trees, finger trees, Roaring bitmaps, Y-FastTrie, van Emde Boas trees.
- **Algorithms**: lock-free / wait-free variants, MVCC, CRDTs, Raft/Paxos, vector clocks, consistent hashing with bounded loads, hopscotch/Robin Hood hashing, FM-index, learned indexes, approximate nearest neighbor (HNSW, FAISS).
- **Frameworks / runtimes (well-known → niche)**: Kubernetes, gRPC, Kafka → Temporal, NATS JetStream, Materialize, ScyllaDB → Pony, Pharo, Unison, Roc, Gleam on BEAM, Verona.
- **Design patterns**: CQRS + Event Sourcing, Hexagonal/Ports & Adapters, Saga, Outbox, Actor Model, Free monads / tagless final, dependency-inverted plugin architectures, capability-based security.
- **Tooling / infra**: OpenTelemetry + Tempo + Loki + Grafana + Prometheus full stack, eBPF observability (Pixie, Parca), service meshes (Istio, Linkerd, Cilium), Nix flakes + devShells, Bazel/Pants/Buck2 for a 3-file repo, formal verification (TLA+, Alloy, Coq, Lean), property-based testing + mutation testing + fuzzing all at once, chaos engineering (Litmus, Chaos Mesh).

### 3. Comparison table

A compact markdown table with columns:

| Approach | Complexity | Time to first commit | Ops burden | Hiring difficulty | Payoff horizon |

Use the same 🔥 N/10 scale for Complexity. "Payoff horizon" should be expressed as the concrete scale, load, or organizational condition under which the option starts to earn its cost (e.g., "≥10⁹ events/day", "multi-region active-active with RPO=0", "team size >50 with independent deploy cadence").

## The complexity scale

Anchor the 🔥 N/10 scores so they stay calibrated across responses:

- 🔥 1–2: Library swap or a single new abstraction. Shippable in hours; minimal review burden.
- 🔥 3–4: A new pattern introduced to the codebase. Days to weeks of refactor; team-wide familiarity expected.
- 🔥 5–6: A new runtime dependency (queue, cache, sidecar). Adds an operational surface and an SLO to defend.
- 🔥 7–8: Introduces a distributed-systems concern — consensus, replication, partial failure. Requires on-call coverage and runbooks.
- 🔥 9: Research-grade. Implementation depends on primary literature; few production references exist.
- 🔥 10: Requires specialist hiring or a runtime/language with a small global user base. Long-term maintenance is a strategic commitment.

If the user says `overkill --max`, only return options at 🔥 7+.

## Tone

Serious, technically precise, and curious. Treat the user as an engineer who already knows the pragmatic answer and wants to understand what lies beyond it — the techniques, trade-offs, and frontier ideas they would otherwise never encounter. Explain each option with the rigor you would use in a design review: real mechanics, real failure modes, real costs. The goal is depth and exposure, not humor. Do not editorialize that an option is "absurd" or "ridiculous" — let the complexity score and hidden-cost line do that work objectively.

## Example (abbreviated)

> **Baseline:** an in-process set keyed by delivery ID, used to deduplicate webhook deliveries on a single node.
>
> **#### Redis SET with TTL — Complexity: 🔥 2/10**
> What it is: `SADD` with a per-key `EXPIRE` providing a shared dedup window across processes.
> How it goes beyond the baseline: dedup state survives process restarts and is consistent across multiple delivery workers.
> Hidden cost: introduces Redis as a hard dependency on the delivery path; partial Redis unavailability now causes duplicate deliveries unless a fallback path is designed.
> Scenario where it pays off: horizontally scaled delivery workers (≥2 nodes) where in-process state diverges and at-least-once semantics are no longer acceptable.
>
> **#### Roaring Bitmaps over a Kafka compacted topic — Complexity: 🔥 8/10**
> What it is: delivery IDs are hashed into dense integer space and tracked in Roaring Bitmaps, with the authoritative state replayed from a Kafka compacted topic so any consumer can rebuild the dedup set deterministically.
> How it goes beyond the baseline: O(1) membership tests at billions-of-ID scale, log-structured durability, and replayable state for new consumers or disaster recovery.
> Hidden cost: requires Kafka operational maturity, bitmap chunking strategy, and a compaction policy tuned to the ID hash distribution; debugging false positives requires reasoning about hash collisions and bitmap segment boundaries.
> Scenario where it pays off: ≥10⁹ delivery IDs retained across a multi-week window, with multiple independent consumers that must agree on dedup state without a shared database.
>
> *(comparison table follows)*

## Final reminder

This skill is for explorers and engineers who want to take a problem a step further than the pragmatic answer — to see the advanced data structures, distributed-systems techniques, and frontier tooling that exist at the edges of the field. Treat every suggestion as a serious object of study, not a punchline. The complexity score is the honest signal of cost; the user is capable of deciding what to do with it. Your job is to expand their map of what is possible, accurately.

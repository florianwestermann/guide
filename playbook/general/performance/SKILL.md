---
title: Playbook (General) — Performance
scope: all-projects
version: 0.1
---

## Guardrails
- Default to fast: avoid unnecessary rerenders, large bundles, heavy images.
- Measure before tuning; keep improvements verifiable.
- Optimize the common path (initial load, core interactions, scroll/typing).

## Practical habits
- Lazy-load non-critical code and assets.
- Keep lists virtualized when they can grow.
- Prefer fewer network roundtrips; cache intentionally.


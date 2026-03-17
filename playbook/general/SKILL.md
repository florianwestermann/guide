---
title: Playbook (General)
scope: all-projects
version: 0.1
---

## Purpose
Default guidance that applies to every project in this repo.

## How to use
- Treat this as the baseline.
- Project- or client-specific rules should **override** this document, not duplicate it.

## Principles
- Prefer clarity over cleverness.
- Make the “happy path” fast; make edge cases safe.
- Ship small, polish continuously.

## UX & UI craft
- Favor strong hierarchy (type scale, spacing, contrast).
- Use motion to explain change, not to decorate it.
- Keep interactions predictable (consistent states, focus, keyboard, reduced motion).

## Engineering craft
- Keep modules small and named for intent.
- Avoid premature abstraction; refactor when patterns prove stable.
- Guard performance by default (avoid unnecessary rerenders, big bundles, heavy images).


---
name: building-spa-like-tall-apps
description: Use when building, modifying, reviewing, or optimizing a TALL-stack Laravel application that must feel like a modern SPA while remaining deployable on conventional cPanel or DirectAdmin PHP hosting.
---

# Building SPA-Like TALL Applications

## Core Principle

Build a **server-driven TALL application with SPA-like perceived continuity**, not a separate JavaScript SPA.

Keep Laravel authoritative for routing, security, business logic, and persistence; use Livewire for server-backed interactive UI/navigation, Alpine for immediate browser-local state, and Tailwind for presentation.

Production must not require a continuously running Node.js frontend server.

## Mandatory Skill Orchestration

This is an orchestration skill, not a substitute for specialized best-practice skills.

**Before touching any technical area, discover and load the most specific installed skill(s) that cover that area. Do this automatically; do not wait for the user to ask.**

Examples: Laravel architecture → Laravel skill; authentication/authorization/uploads/hardening → Laravel/security skill; Livewire work → Livewire skill; Alpine/Tailwind → corresponding frontend skill; schema/query tuning → database skill; tests → testing/TDD skill; debugging → debugging skill; accessibility → WCAG/accessibility skill; deployment → Laravel/shared-hosting/cPanel/DirectAdmin skill.

Skill discovery is continuous. When work crosses into a new domain, load that domain's skill before proceeding.

Never invent skill names. Inspect the skills actually installed. If several materially apply, use all of them. If none exist, use authoritative framework guidance.

**Checkpoint before code changes:** “Which installed skills apply to the code I am about to touch, and have I loaded them?” If unknown, stop and discover them first.

## Non-Negotiable Architecture

- TALL remains Tailwind + Alpine + Laravel + Livewire.
- Do not introduce React, Vue, Angular, Next, Nuxt, Inertia, a second frontend/router, or a separate API merely to obtain SPA behavior unless explicitly required.
- Ordinary internal navigation uses Livewire navigation (`wire:navigate`; selective `.hover` prefetching).
- Routine CRUD must update Livewire state rather than reload the document or redirect merely to refresh data.
- Alpine owns transient browser-only UI; Livewire owns server-backed UI; Laravel/domain classes own business rules.
- Use a stable application shell and persist only elements that genuinely benefit from persistence.
- Every asynchronous interaction gets intentional immediate feedback when latency is perceptible.
- JavaScript and third-party libraries must support Livewire navigation lifecycle; never rely solely on `DOMContentLoaded`.
- Preserve normal web behavior: deep links, refresh, browser Back/Forward, authorization, page titles, and useful query-string state.
- Avoid chatty/oversized Livewire requests, N+1 queries, unbounded datasets, unnecessary server round trips, and speculative optimization.
- Security remains server-enforced. Client checks are UX only.
- Production uses normal PHP/Laravel hosting plus pre-built Vite assets. Node may be a build-time dependency, not a required frontend runtime.
- Prefer cron-based Laravel scheduling and hosting-compatible infrastructure. Do not make core behavior depend on root access, containers, or unsupported daemons.

## Quality Bar

Target:

```text
click → immediate acknowledgement → no document flash → stable shell
→ minimal content update → short loading state if needed → completed state
```

Reject:

```text
click → apparent freeze → full-screen teardown → white flash
→ page reconstruction → lost filters/scroll/state
```

## Required Verification

Before completion verify:

- relevant installed skills were loaded for every domain touched;
- internal navigation is Livewire-native where appropriate;
- routine CRUD performs no full document reload;
- Alpine/Livewire/Laravel responsibilities are correctly separated;
- JavaScript survives repeated Livewire navigation without duplicate instances/listeners;
- Back/Forward, direct URLs, refresh, validation, authorization, and important state work;
- important behavior is covered by tests according to the installed testing skill;
- no permanent Node frontend server is required;
- deployment remains viable on the target cPanel/DirectAdmin environment.

For detailed patterns, edge cases, review smells, lifecycle guidance, security/testing notes, and deployment rules, read `reference.md` when that detail is relevant.

# SPA-like TALL Implementation Reference

Use this reference when implementation detail is needed after loading `SKILL.md`.

# Building SPA-Like TALL Applications

## Overview

Build TALL applications that behave like polished single-page applications from the user's perspective while preserving Laravel's conventional server-driven architecture and compatibility with standard PHP hosting.

The target stack is:

- Tailwind CSS
- Alpine.js
- Laravel
- Livewire

The application MUST remain deployable without a continuously running Node.js application server.

This skill is an **orchestration skill**. It defines the architecture and SPA-experience rules, but it does NOT replace specialized skills for Laravel, Livewire, security, testing, databases, accessibility, performance, deployment, or other implementation areas.

---

# Mandatory Skill Orchestration

## Core Rule

**Before working on any specialized area, automatically discover and load the most relevant installed skill or skills for that area.**

Do not wait for the user to explicitly ask.

Do not assume this skill alone contains sufficient implementation guidance.

Do not rely only on general knowledge when an installed skill covers the work being performed.

### Required behavior

Before implementing, modifying, reviewing, debugging, or hardening a part of the application:

1. Identify the technical domains involved.
2. Inspect/discover the installed skills relevant to those domains.
3. Load the most specific applicable skill(s).
4. Apply their instructions while also following this skill.
5. If the task moves into another domain, repeat skill discovery before working in that domain.

Examples:

| Work being performed | Skills to automatically seek/load |
|---|---|
| Laravel controllers, services, models, actions, policies, architecture | Laravel architecture / Laravel best-practices skill |
| Authentication, authorization, validation, CSRF, XSS, SQL injection, uploads, secrets, rate limiting | Laravel security / web application security skill |
| Livewire components, lifecycle, events, navigation, hydration | Livewire best-practices skill |
| Alpine state or browser-side interaction | Alpine.js best-practices skill |
| Tailwind UI implementation | Tailwind / frontend design-system skill |
| Database schema, indexing, query tuning, transactions | Laravel database / MariaDB / SQL best-practices skill |
| Automated testing | Laravel testing and/or test-driven-development skill |
| Debugging defects | Systematic debugging skill |
| Refactoring | Refactoring / Laravel architecture / testing skills |
| Accessibility | Accessibility / WCAG skill |
| Frontend performance | Web performance / Livewire performance skill |
| Security hardening | Security skill plus Laravel-specific security skill when available |
| cPanel or DirectAdmin deployment | Laravel deployment / shared-hosting / cPanel / DirectAdmin skill |
| Queues, scheduler, mail, caching | Relevant Laravel infrastructure skill |
| File uploads/storage | Laravel storage plus security skill |
| API endpoints | Laravel API plus API security skill |
| Code review | Relevant review skill plus the stack-specific skills covering the changed code |

These names are examples of categories, NOT literal filenames that are guaranteed to exist.

**Never invent a skill name. Discover the skills actually installed in the current environment and load the closest applicable skill(s).**

If several installed skills apply, use all materially relevant ones rather than arbitrarily choosing one.

If no specialized skill exists, proceed using authoritative framework documentation and established best practices while preserving the constraints in this skill.

### Skill-loading checkpoint

Before changing code, be able to answer:

> "Which installed skills apply to the code I am about to touch, and have I loaded them?"

If the answer is unknown or no, stop and perform skill discovery first.

### Domain transitions

Skill loading is continuous, not a one-time startup step.

Example:

```text
Implement customer management
        ↓
Load Laravel + Livewire skills
        ↓
Add authorization
        ↓
Load Laravel security skill BEFORE implementing it
        ↓
Tune customer search query
        ↓
Load database/query-performance skill
        ↓
Add tests
        ↓
Load Laravel testing/TDD skill
        ↓
Prepare production deployment
        ↓
Load Laravel deployment/shared-hosting skill
```

### Conflict handling

When instructions differ:

1. Follow explicit user requirements.
2. Follow project-level architecture and repository instructions.
3. Follow the most specific applicable specialized skill.
4. Follow this orchestration skill for SPA/TALL architectural constraints.
5. Prefer framework-supported approaches over custom mechanisms.

Do not silently violate one skill to satisfy another. If two mandatory constraints genuinely cannot coexist, explain the conflict and choose the approach that preserves the project's stated architecture and user requirements.

---

# Architectural Objective

The browser experience should resemble:

```text
User clicks internal navigation
        ↓
URL changes normally
        ↓
No visible browser document reload
        ↓
Persistent application shell remains stable
        ↓
Only necessary content changes
        ↓
Loading feedback appears immediately if needed
        ↓
Browser Back/Forward still works
        ↓
Direct URL refresh still works
```

The implementation must remain a server-driven Laravel application.

Laravel remains authoritative for:

- routing
- authentication
- authorization
- validation
- business logic
- persistence
- server state

Livewire provides server-driven interactive UI and SPA-like navigation.

Alpine provides immediate browser-local interaction.

Tailwind provides presentation and responsive styling.

---

# Non-Negotiable Stack Constraints

The application MUST remain TALL unless the project's explicit requirements say otherwise.

Do NOT introduce any of the following merely to achieve SPA behavior:

- React
- Vue
- Angular
- Next.js
- Nuxt
- a second frontend application
- a separate client-side application router
- a mandatory Node.js production server
- duplicated frontend/backend business logic
- a separate API solely because "SPAs need APIs"

Do not introduce Inertia merely to solve navigation that Livewire already supports unless the project has explicitly chosen Inertia.

Node/Vite MAY be used during development and asset compilation.

Production must be capable of serving pre-built assets from ordinary Laravel/PHP hosting.

---

# Navigation Rules

## Internal navigation

Ordinary internal application navigation MUST use Livewire navigation.

Use:

```html
<a href="/customers" wire:navigate>Customers</a>
```

Use prefetching selectively for frequently accessed or latency-sensitive destinations:

```html
<a href="/customers" wire:navigate.hover>Customers</a>
```

Do not blindly add hover prefetching to every link.

## Redirects

When a Livewire action navigates to another application page, use Livewire-compatible SPA navigation rather than forcing a normal browser document reload.

Avoid:

```php
return redirect('/customers');
```

when a Livewire navigate-aware redirect is appropriate.

## Full browser navigation

A conventional navigation/reload is acceptable when technically necessary, such as:

- external websites
- file downloads requiring normal browser navigation
- authentication/provider flows that require leaving the app
- framework constraints that genuinely require document navigation

It should be the exception, not routine application behavior.

---

# Routable Page Architecture

Prefer routable Livewire page components for interactive application screens.

Typical examples:

```text
Dashboard
Customers
Orders
Invoices
Reports
Settings
Users
```

Traditional controllers remain appropriate for concerns such as:

- APIs
- webhooks
- downloads
- streaming responses
- integrations
- non-interactive endpoints
- specialized HTTP workflows

Do not convert every Laravel concern into a Livewire component.

Keep domain/business logic outside presentation components where appropriate, using the project's preferred Laravel architecture such as:

- actions
- services
- domain classes
- policies
- jobs
- repositories only when justified

Load the installed Laravel architecture skill before making architectural decisions in these areas.

---

# Persistent Application Shell

Design the application around a stable shell where appropriate:

```text
App
├── Sidebar
├── Header
├── Main navigation
├── Notifications
├── Toast container
├── Global modal layer
└── Current page content
```

Use Livewire persistence mechanisms when an element genuinely should survive navigation.

Suitable candidates may include:

- global navigation
- audio/video players
- persistent utility panels
- global overlays
- long-lived UI state

Do NOT persist elements merely because persistence exists.

Persistent components must not create stale authorization, stale user state, memory leaks, duplicate listeners, or obsolete data.

---

# Livewire vs Alpine Responsibility

Use the lowest-cost layer that correctly owns the state.

## Alpine.js owns immediate browser-local state

Examples:

- dropdown open/closed
- accordion expansion
- sidebar collapse
- local tabs
- hover interaction
- keyboard shortcuts
- transient popovers
- client-side visual transitions
- local command-palette state
- temporary presentation state

These interactions should not require a network round trip unless server state is actually needed.

## Livewire owns server-backed state

Examples:

- CRUD
- database-backed filters
- server search
- pagination
- validation
- permissions
- uploads
- business operations
- persistence
- authoritative application state

## Laravel/domain layer owns business rules

Do not place complex business logic directly inside Blade templates, Alpine components, or large Livewire methods.

---

# CRUD Without Page Reloads

Normal CRUD operations MUST NOT use full-page refreshes as a state-management mechanism.

Avoid patterns such as:

```js
window.location.reload()
```

or redirecting to the same route solely to refresh displayed data.

After create/update/delete operations:

- update component state
- refresh the relevant component/query
- dispatch an appropriate Livewire event
- close/update a modal
- navigate with Livewire only when moving to another logical screen

The user should not see a white flash or conventional document reload after routine CRUD actions.

---

# Perceived Performance

SPA feel depends more on perceived responsiveness than on routing alone.

For every asynchronous interaction, intentionally choose one or more of:

- immediate local Alpine response
- optimistic UI
- `wire:loading`
- disabled control
- inline spinner
- skeleton state
- progress indication
- toast/status feedback

Never leave the interface appearing frozen while a server request is in progress.

Avoid unnecessary loading indicators for operations that complete imperceptibly fast.

---

# Request Efficiency

Avoid excessively chatty Livewire behavior.

Apply appropriate techniques such as:

- debounce search input
- lazy-load expensive sections
- paginate large datasets
- avoid loading entire tables into memory
- avoid needless component refreshes
- avoid giant serialized component state
- select only needed database columns where useful
- eager-load relations when appropriate
- prevent N+1 queries
- cache expensive stable computations when justified

Before optimizing database or Livewire performance, load the relevant installed performance/database skills.

Measure meaningful bottlenecks instead of performing speculative micro-optimization.

---

# URL and Browser Behavior

A polished SPA-like TALL application MUST preserve normal web behavior.

Ensure:

- every important screen has a real URL
- direct links work
- refreshing a route works
- browser Back works
- browser Forward works
- route authorization still runs correctly
- titles update appropriately
- query-string state is preserved when useful
- shareable filter/search URLs are used where beneficial

Do not create hidden client-only screens that cannot be directly addressed unless there is a strong UX reason.

---

# JavaScript Lifecycle Rules

Never assume every route transition triggers:

```js
DOMContentLoaded
```

Livewire navigation changes the document lifecycle.

Any JavaScript that must run after SPA navigation must use the appropriate Livewire lifecycle/navigation hooks.

Third-party libraries must be verified for:

- initialization after navigation
- cleanup before replacement
- avoiding duplicate listeners
- avoiding duplicate instances
- handling persisted elements
- memory cleanup

Typical integrations requiring special care include:

- charts
- editors
- maps
- sliders
- date pickers
- tooltips
- rich selects
- uploaders
- analytics
- custom event listeners

Do not "fix" these integrations by disabling SPA navigation globally unless no robust integration approach exists.

Load relevant Livewire/frontend integration skills before implementing complex third-party JavaScript.

---

# State Preservation

Preserve user-visible state when it improves UX.

Examples:

- filters
- search terms
- pagination
- selected tab
- sort order
- useful scroll behavior

Use URL/query-string state when the state should survive refresh, navigation history, or sharing.

Use Alpine/local state for ephemeral presentation state.

Use Livewire/server state for authoritative data.

Do not preserve stale or security-sensitive state merely for convenience.

---

# Security

SPA-like behavior must never bypass Laravel security boundaries.

All server mutations require appropriate:

- authentication
- authorization
- policies/gates
- server-side validation
- CSRF protection
- rate limiting where appropriate
- secure upload handling
- output escaping
- tenant isolation where relevant

Client-side checks are UX only; they are never authorization.

**MANDATORY:** Before implementing or reviewing authentication, authorization, sensitive forms, file uploads, APIs, tenant boundaries, secrets, or other security-sensitive work, discover and load the installed Laravel/security skill(s).

Do not continue security-sensitive implementation until those skills have been consulted.

---

# Testing

SPA-like behavior requires tests at multiple levels where appropriate.

Test:

- route accessibility
- authorization
- Livewire actions
- validation
- component state changes
- CRUD behavior
- query-string behavior
- redirects/navigation
- important loading/error behavior where practical
- regression-prone JavaScript integrations
- browser Back/Forward or navigation flows when end-to-end testing is available

**MANDATORY:** Load the installed testing/TDD skill before implementing behavior changes.

Follow test-first practices when required by the installed testing skills.

Do not substitute manual clicking for automated regression coverage on important application behavior.

---

# Accessibility

SPA behavior must not degrade accessibility.

Pay attention to:

- focus after navigation
- keyboard navigation
- semantic HTML
- modal focus trapping
- accessible loading states
- screen-reader announcements where appropriate
- reduced-motion preferences
- form errors
- page titles/headings after navigation

Load an installed accessibility/WCAG skill when working on interaction-heavy UI, forms, navigation, dialogs, or accessibility review.

---

# cPanel / DirectAdmin Compatibility

Production architecture MUST remain compatible with conventional PHP hosting unless the project explicitly changes this requirement.

Expected production runtime:

```text
Web server
PHP
Laravel
MariaDB/MySQL
Composer dependencies
Pre-built frontend assets
Cron scheduler
Optional queue worker where hosting permits it
```

A continuously running Node.js application server MUST NOT be required.

Typical frontend deployment flow:

```bash
npm ci
npm run build
```

Deploy the generated static assets alongside the Laravel application.

Node.js is a build-time dependency unless the project explicitly requires otherwise.

Do not introduce production dependencies such as:

- Next.js server
- Nuxt server
- Vite dev server
- PM2 solely for the frontend
- Node SSR layer
- WebSockets merely to support navigation

Queues or long-lived workers may be used when the application actually needs them, but deployment must account for whether the selected cPanel/DirectAdmin environment supports persistent processes.

Before configuring production deployment, discover and load the relevant installed Laravel deployment/shared-hosting/cPanel/DirectAdmin skill.

---

# Graceful Shared-Hosting Design

When choosing between equivalent implementations, prefer designs that can operate under ordinary PHP-hosting constraints.

Prefer:

- Laravel scheduler via cron
- database/cache mechanisms supported by the host
- pre-built Vite assets
- standard PHP entrypoints
- conventional Laravel storage/public paths

Be cautious about making core functionality depend on:

- daemons the hosting panel cannot supervise
- container orchestration
- root access
- system-level packages unavailable to the account
- persistent Node processes
- infrastructure the project has not approved

This does not mean avoiding modern architecture. It means keeping infrastructure requirements intentional.

---

# UX Quality Bar

The finished application should feel continuous.

Target behavior:

```text
Click
→ immediate visual acknowledgement
→ no document flash
→ stable shell
→ minimal changed content
→ short loading state if necessary
→ completed state
```

Avoid:

```text
Click
→ nothing appears to happen
→ whole screen disappears
→ white flash
→ page reconstructs
→ filters reset
→ scroll jumps unexpectedly
```

A technically SPA-like router is not sufficient if the interaction still feels slow or unstable.

---

# Implementation Checklist

Before considering a feature complete, verify:

- [ ] Relevant installed skills were discovered and loaded for every technical domain touched.
- [ ] TALL architecture remains intact.
- [ ] Laravel remains the authoritative backend/router.
- [ ] Internal page navigation uses Livewire navigation where appropriate.
- [ ] No unnecessary full browser reloads occur.
- [ ] Routine CRUD does not reload the document.
- [ ] Alpine handles immediate browser-only interaction.
- [ ] Livewire handles server-backed UI state.
- [ ] Business logic is not buried in view code.
- [ ] Loading/disabled/optimistic feedback is intentional.
- [ ] Large or chatty Livewire requests were avoided.
- [ ] Database access was checked for obvious N+1/query problems.
- [ ] JavaScript survives Livewire navigation correctly.
- [ ] No duplicate JS listeners/components appear after repeated navigation.
- [ ] Browser Back/Forward works.
- [ ] Direct route refresh works.
- [ ] Important state survives appropriately.
- [ ] Authorization remains server enforced.
- [ ] Validation remains server enforced.
- [ ] Security-specific skills were loaded for security-sensitive work.
- [ ] Tests cover important behavior.
- [ ] Accessibility was considered for navigation and dynamic content.
- [ ] Production does not require a permanent Node.js frontend server.
- [ ] Vite assets can be pre-built.
- [ ] cPanel/DirectAdmin deployment remains viable.

---

# Review Checklist

When reviewing existing TALL code for SPA quality, actively search for:

```text
window.location.reload()
location.href for internal routes
plain internal links that bypass Livewire navigation
same-page redirects used as refresh mechanisms
DOMContentLoaded-only initialization
duplicate event listeners after navigation
oversized Livewire component state
server calls for trivial UI toggles
un-debounced live search
N+1 queries
full-table loading
missing loading feedback
authorization only in the browser
business logic inside Blade/Alpine
Node production-server dependencies
React/Vue introduced only for isolated UI convenience
```

For each problem, determine which installed specialized skill applies and load it before implementing the fix.

---

# Common Mistakes

## "SPA means React/Vue"

False. The requirement is the user experience, not a specific client framework.

Use Livewire's navigation and component model before introducing another application framework.

## Sending every interaction to Livewire

This produces unnecessary latency.

Use Alpine for browser-local presentation state.

## Using Alpine for authoritative business state

This creates trust and synchronization problems.

Laravel remains authoritative.

## Redirecting after every CRUD mutation

This recreates multi-page-app behavior.

Update Livewire state directly unless the user is actually moving to another screen.

## Initializing JavaScript only once

Livewire navigation changes lifecycle expectations.

Make third-party integrations navigation-aware and clean them up correctly.

## Treating shared hosting as an afterthought

An architecture that quietly depends on persistent Node services is not compatible with the project's deployment constraint.

## Treating this skill as the only best-practices source

This skill is intentionally architectural.

When working on Laravel internals, security, testing, SQL, Livewire, accessibility, deployment, or another specialized area, load the corresponding installed skill first.

---

# Definition of Done

The application should deliver:

**React/Vue-like perceived continuity**

while retaining:

**Laravel simplicity + Livewire server-driven architecture + Alpine immediacy + Tailwind UI + conventional PHP hosting compatibility.**

And every specialized implementation area must be executed using the best applicable installed skill available in the agent's environment.

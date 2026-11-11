# Loading, Empty, and Error Feedback

Use when a page goes blank during requests, content appears abruptly, refresh loses context, or empty/error states leave users without an explanation or recovery path. Applies to web, mobile web, apps, dashboards, and content lists; use only modules relevant to the requested change.

Contents: [Scope and state contract](#scope-and-state-contract) · [Diagnosis](#diagnose-before-editing) · [Eight modules](#eight-feedback-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Scope and State Contract

For choosing transient versus persistent or blocking feedback surfaces, read [overlay-selection.md](overlay-selection.md). Keep the state truth, recovery, and request lifecycle rules here.

Preserve business content, layout structure, information architecture, and capabilities. Add truthful feedback and recovery around existing operations. Reuse the project's request/cache layer, image components, routing, and design tokens. Do not introduce endpoints, subscriptions, refresh gestures, new data flows, or dependencies merely to fill this checklist.

Separate perceived continuity from actual performance. Placeholders and transitions do not inherently make a request faster; speculative requests change bandwidth and server load. Never promise faster loading, a completion time, or a percentage without evidence. Do not delay ready content to make a loader or fade visible.

Before styling, identify the state and recovery contract for each affected region:

| Situation | Feedback and behavior |
| --- | --- |
| Initial pending, no usable content | Show a scoped skeleton or named indicator; do not show a premature empty state. |
| Initial success with content | Render real content and remove pending indicators. |
| Initial success with zero results | Show an appropriate empty state, including the active filters when relevant. |
| Initial failure | Explain the failure and offer a safe, applicable recovery action. |
| Refresh pending with usable content | Keep the current view, mark refresh in progress, preserve interaction context. |
| Refresh failure with usable content | Retain eligible previous data, explain that updating failed, and distinguish stale from current when material. |
| More-page failure | Keep already loaded items and provide a local continuation/retry action. |
| Access revoked, account switched, or data invalidated | Follow the authorization/freshness contract; do not retain content that is no longer safe or valid to show. |
| Route or resource absent | Use the existing not-found contract, distinct from an empty collection or transient network failure. |

Use existing cancellation or request-identity handling so an old response cannot replace a newer search, account, or route state. Pending state must settle after success, error, timeout, or cancellation. Retry must not silently duplicate writes: use the operation's idempotency/reconciliation contract, especially when submission success is unknown.

With screenshots only, propose states and copy but label runtime transitions, network behavior, and resource timing unverified.

## Diagnose Before Editing

- Images have no reserved space, load abruptly, or leave broken-image icons.
- Pending regions remain blank, or a global spinner blocks unrelated working content.
- Empty-state text flashes before data resolves, or progress implies unobserved work.
- Resource requests compete with the first screen or speculate indiscriminately.
- Refresh gives no feedback, overlaps requests, or loses existing content and position.
- Errors display raw exceptions, blame the network without evidence, or offer ineffective retry.
- A successful zero-result response looks identical to an error or unfinished load.
- Missing routes have no usable recovery, or server failures are mislabeled as 404.

Judge actual comprehension and recovery, not the presence of an animation, illustration, prefetch, or pull-to-refresh control. Missing access and retry correctness can matter more than visual polish; do not require visual or navigation cleanup before fixing them.

## Eight Feedback Modules

### 1. Progressive Image Presentation

**Goal and rationale:** Keep layout stable and the content transition understandable while images load or fail.

**Rules:** Reserve width/height or aspect ratio, and preserve meaningful alternative text. Prefer a simple color/shape placeholder; use an existing low-resolution image or LQIP pipeline only when available and worthwhile, rather than inventing extra assets or an image service. Replace the placeholder when the image is ready; use the existing image component or handle load/decode and failure where needed. Handle cached images, source changes, unmounting, and decode rejection without leaving a permanent placeholder. A short optional fade must respect reduced motion and must not hide ready content. Already-cached content may appear immediately. Avoid recursive error fallbacks. Do not lazy-load a critical first-screen image merely to make it animate; use lazy loading for suitable offscreen resources.

**Prompt:**

```text
Improve image loading using the existing image component and assets. Reserve the final image dimensions, show a lightweight placeholder while needed, and reveal the ready image without artificial delay. Use an optional restrained fade with reduced-motion support. Handle cached images, changed sources, and failure with a stable fallback and useful alternative text. Do not add an image service or extra placeholder request without a concrete need.
```

### 2. Scoped Loading and Progress Feedback

**Goal and rationale:** Explain what is waiting and keep unaffected parts usable.

**Rules:** Distinguish initial page load, local loading, refresh, and submission. Match skeletons to the existing layout; use short task-specific text and lightweight indicators. A brief indicator delay may avoid flashes, but do not delay the request or completion. Show staged labels, numeric progress, or remaining time only from actual observed work; otherwise use indeterminate feedback. Long waits need an honest ongoing state and applicable cancel/retry/recovery, following existing request policy. Expose meaningful status updates to assistive technology without stealing focus or announcing every animation frame; use `aria-busy` and appropriate status/progress semantics where applicable. Reset pending state on every terminal path.

**Prompt:**

```text
Add truthful loading feedback at the affected page or region. Keep unrelated content usable, distinguish first load, refresh, and submission, and avoid flashing indicators for fast requests. Announce meaningful status changes accessibly without moving focus. Do not invent stages, percentages, or completion estimates, and ensure success, failure, and cancellation clear the pending state.
```

### 3. Resource Priority, Preload, and Prefetch

**Goal and rationale:** Reduce justified future waiting without starving the current task or wasting requests.

**Rules:** Distinguish critical-current-page `preload` from speculative next-route/data `prefetch`. Inspect existing router/browser/cache behavior before adding either. Prioritize the current view; make speculative work selective, low priority or idle/intent-triggered where supported, deduplicated, and bounded. Respect data-saving/network constraints where detectable; a missing idle API must not break navigation. Speculate only on safe reads, never actions that mutate, bill, or trigger business side effects. Preserve authorization and cache isolation across accounts; do not prefetch private data for an unauthorized user or introduce external destinations solely for speculation. Only claim improvement after measuring request order, unused work, and actual critical-path behavior.

**Prompt:**

```text
Assess existing resource priorities and router/cache behavior before adding requests. Use preload only for justified critical current-page assets, and selective prefetch for high-probability safe next destinations. Bound and deduplicate speculation, keep the current view responsive, and preserve data-saving constraints, authorization, and account isolation. Do not prefetch mutations or claim a performance gain without measurements.
```

### 4. Refresh and Optional Pull-to-Refresh

**Goal and rationale:** Make refreshing recognizable and reversible without interfering with navigation or scrolling.

**Rules:** Keep the existing refresh affordance. Add pull-to-refresh only when requested or supported by the target app/platform; do not hijack browser gestures, page scrolling, nested scroll areas, or system Back. When used, distinguish pull, release threshold, refreshing, completion, cancellation, and failure. Trigger one refresh only after the threshold; prevent duplicate overlapping refreshes through the existing request layer. Keep a keyboard/button alternative. Preserve eligible content, list position, filters, and selection; announce completion or failure without relying on a spring animation. Use restrained motion and honor reduced motion.

**Prompt:**

```text
Normalize the existing refresh operation with pending, completion, and failure feedback while retaining content and interaction context. Add pull-to-refresh only when the requested platform calls for it, with distinct pull/release states and a keyboard-accessible alternative. Prevent duplicate refreshes and gesture conflicts; keep all motion optional and restrained.
```

### 5. Plain-Language Errors and Safe Recovery

**Goal and rationale:** Explain what failed and give an action that can actually help.

**Rules:** Describe the affected action, known cause, and available next step. Do not call every server error a network problem. Offer retry for safe retryable failures; use sign-in, access request, input correction, or support when appropriate. Reuse bounded retry/backoff behavior rather than adding infinite loops. Keep actionable diagnostics or a redacted reference ID in an optional detail area when useful for technical users, without making codes the entire message. Never expose stack traces, credentials, personal data, or backend internals. Preserve form input and prevent duplicate submission when the original write outcome is uncertain. Place recovery near the failure; do not rely on a disappearing toast for a persistent blocker.

**Prompt:**

```text
Replace raw errors with concise, accurate explanations of the affected action and a useful next step. Offer retry only where safe and appropriate; use sign-in, input correction, or access recovery for other failures. Preserve user input and existing retry safeguards, avoid duplicate writes, and keep sensitive diagnostics out of visible messages. Do not invent a cause or hide information needed for recovery.
```

### 6. Retain Context on Refresh Failure

**Goal and rationale:** Keep usable content and user work while making failed updates clear.

**Rules:** Retain the last successful eligible content for the same account, route, and query. Show refresh failure locally; label stale data and last-updated time only when known and relevant. Do not present results from the previous query as fresh results for a different query. Preserve scroll, filters, selection, and drafts when appropriate. Clear or replace content on logout, revoked access, account switch, deletion, or a correctness/freshness requirement that makes retention unsafe. Block actions that cannot safely use stale data. Recovery must refresh the correct region and clear the failure after success; a global blank page is not the default fallback.

**Prompt:**

```text
On refresh or pagination failure, preserve eligible previously loaded content and user context for the same account and query. Show a local update failure and truthful freshness information where needed. Do not retain unauthorized, invalidated, or misleading data, and do not allow actions that require fresh data to proceed unsafely. A successful retry must update the correct region and remove the error.
```

### 7. Empty States

**Goal and rationale:** Explain successful absence of content without confusing it with loading, failure, or denied access.

**Rules:** Show empty state only after a successful known-empty response. Distinguish a first-use empty collection, filtered zero results, and an exhausted list. Say why it is empty when known and offer an existing permitted action such as clearing filters or creating the first item. Do not invent create/discover routes, enable unauthorized controls, or add an illustration dependency for decoration. A small existing icon is optional; concise text may be sufficient. Preserve the page shell, filters, and meaningful context. Do not replace useful loaded items with an empty state just because a subsequent page is empty.

**Prompt:**

```text
Add a concise empty state only for successfully loaded zero results. Distinguish first use, no filter matches, and end of list. Keep the page shell and relevant controls, explain the known situation, and offer only an existing permitted next action. Do not conflate empty with pending, failure, or forbidden access, and do not invent content or navigation.
```

### 8. Missing Routes and Resources

**Goal and rationale:** Give users a trustworthy way forward when a destination does not exist.

**Rules:** Use existing router/not-found boundaries, preserve the product shell as appropriate, and distinguish not-found from transient errors and empty data. Keep permission-related concealment consistent with the existing security contract. Offer a known safe home/parent destination; provide Back only where meaningful and never assume an external/direct visit has an in-app predecessor. Coordinate with [navigation-and-routing.md](navigation-and-routing.md). Where the rendering/server layer controls HTTP responses, preserve the appropriate status instead of accidentally returning a successful page for every missing route. Do not expand a UI-only task into hosting changes; report that boundary when unavailable.

**Prompt:**

```text
Improve the existing not-found boundary with a clear explanation and safe navigation to an existing home or parent destination. Keep Back meaningful after direct entry and distinguish absent routes/resources from empty results or temporary failures. Preserve permission concealment and the router/server status contract where controlled. Reuse the current product style without creating new business pages or changing hosting outside scope.
```

## Combined Prompt

Use for a scoped pass; implement and verify relevant modules incrementally rather than adding all eight by default.

```text
Improve loading, refresh, empty, and error feedback while preserving the existing layout, business content, information architecture, and capabilities. Map initial pending, success, zero results, failure, background refresh, and pagination separately. Reuse existing image/request/cache/router components. Reserve image space, show lightweight placeholders when needed, and reveal ready content without artificial delay. Provide scoped, accessible loading feedback without fake stages or progress. Add only justified preload/prefetch and platform-appropriate refresh interactions, avoiding request floods and gesture conflicts. Explain errors plainly and offer only safe recovery; protect drafts and uncertain submissions from duplicate writes. Preserve eligible same-context data on refresh failure while enforcing authorization, account isolation, invalidation, and freshness. Show distinct successful empty and not-found states with existing permitted actions. Respect reduced motion and verify real transitions under slow, failed, cached, and out-of-order responses. Report what could not be exercised rather than claiming all states work from screenshots alone.
```

## Acceptance and Regression Checks

Exercise applicable transitions using the existing test framework and controlled request failures/delays. Use `code-testing` for deeper test design when needed; no production fault injection or global tooling change is implied. Mark each relevant check pass/fail/unverified and explain N/A.

| Scenario | Expected evidence |
| --- | --- |
| Fast and slow first load | No premature empty state or unnecessary loader delay; scoped pending indicators settle correctly. |
| Images | Stable space for cached/uncached images; changed source and load/decode failure reach the correct image/fallback without loops. |
| Error and recovery | Initial failure has useful safe recovery; repeated clicks do not duplicate a write; successful retry clears the error. |
| Refresh and pagination | Existing eligible items, drafts, filters, and scroll remain usable; failed continuation does not erase earlier pages. |
| Empty results | Successful zero, filtered zero, and end of list are distinct from pending/error/forbidden states. |
| Request races and navigation | Out-of-order responses and cancellation do not overwrite a newer route, query, or account. |
| Access and freshness | Account switch/revoked access clears inappropriate retained data; stale-sensitive actions remain protected. |
| Speculative loading | No mutation or unauthorized prefetch, duplicate request flood, or observed regression to current-page critical loading. |
| Not found | Unknown route/resource and direct entry have working safe recovery and the appropriate controlled status behavior. |
| Accessibility and motion | Meaningful status is announced without focus theft or excessive interruption; reduced motion and keyboard recovery remain usable. |

Use the bounded rendering process in [verification.md](verification.md). Record the actual routes/components, injected conditions, commands, screenshots, and unverified devices/states. Do not treat a still image or successful build as proof of network, retry, or accessibility behavior.

## Primary References

- [W3C: Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html): convey meaningful asynchronous status without requiring focus changes or unnecessary interruption.
- [MDN: HTMLImageElement.decode()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/decode): decoded-image readiness and rejection handling where a custom image lifecycle is justified.
- [web.dev: Preload critical assets](https://web.dev/articles/preload-critical-assets): current-page critical resource prioritization; distinguish this from speculative next-destination work.

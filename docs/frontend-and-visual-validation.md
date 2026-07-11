# Frontend and Visual Validation

Frontend code can compile, pass unit tests, and still be visibly wrong. Give AI assistants a visual contract and verify the rendered result across states and screen sizes.

## Start from the real design system

Before generating components, inspect:

- Existing layout and component primitives
- Design tokens, typography, spacing, and breakpoints
- Routing, data-fetching, form, and state-management patterns
- Accessibility utilities and test setup
- A nearby screen that already follows the desired conventions

Ask the assistant to reuse these before introducing new colors, spacing scales, components, or dependencies.

## Define a visual and behavioral contract

Include:

- Reference image, design file, or precise written hierarchy
- Target viewports and responsive behavior
- Loading, empty, error, partial, and populated states
- Hover, focus, pressed, disabled, validation, and success states
- Keyboard navigation and focus order
- Content extremes: long labels, missing images, large numbers, localization
- Browser support and performance constraints
- Acceptance screenshots or observable checks

A single happy-path screenshot does not define interaction behavior or responsiveness. Mark missing design decisions instead of letting the model invent them silently.

## Build structure before polish

Use reviewable checkpoints:

1. Route and semantic page structure
2. Real data and state wiring
3. Responsive layout
4. Interactions and forms
5. Accessibility behavior
6. Visual polish
7. Performance and cleanup

Verify each stage in the browser. Large one-shot frontend generations make it hard to distinguish layout, data, and interaction failures.

## Validate visually

For each required viewport and state:

- Render the actual application.
- Capture a screenshot.
- Compare hierarchy, alignment, spacing, typography, colors, borders, and clipping.
- Inspect overflow, scroll behavior, sticky elements, and viewport height.
- Test realistic and extreme content.
- Check light/dark and other supported themes.
- Iterate on the observed mismatch, not a prose guess.

Overlay or image-diff tools can reveal small errors, but a numerical pixel score cannot judge whether a behavior or hierarchy is correct.

## Validate interaction

Exercise the user flow:

- Pointer and keyboard input
- Focus visibility and focus restoration
- Form validation and error recovery
- Back/forward navigation and deep links
- Loading, cancellation, retries, and duplicate submission
- Slow network and failed requests
- Modal, menu, tooltip, and escape behavior
- Touch targets and mobile scrolling

Inspect console errors, network requests, and accessibility-tree output where available.

## Check accessibility explicitly

Review:

- Semantic HTML and landmark structure
- Accessible names and descriptions
- Heading hierarchy
- Keyboard reachability and logical focus order
- Visible focus
- Form labels and error association
- Color contrast and non-color cues
- Reduced-motion preferences
- Zoom, text resizing, and reflow
- Announcements for dynamic state

Automated checks catch only part of accessibility. Add keyboard and screen-reader-oriented manual checks based on the component's risk and audience.

Use [WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/) and the project's own accessibility target as the standard, not the model's memory.

## Test at the right layers

- **Unit/component:** state rendering and local interactions
- **Integration:** data, routing, and form behavior
- **End-to-end:** critical user journeys
- **Visual regression:** stable appearance at selected viewports/states
- **Accessibility:** automated rules plus manual keyboard/assistive-technology checks

Avoid snapshots that merely approve a huge generated tree. Assert meaningful behavior and review visual baselines before updating them.

## Protect performance

Watch for generated code that:

- Adds a large dependency for a small effect
- Fetches the same data repeatedly
- Causes render loops or unstable memoization
- Ships unoptimized images or fonts
- Blocks initial rendering unnecessarily
- Animates layout-heavy properties
- Renders very large lists without a strategy
- Moves server-safe work into the client

Measure the relevant budget in a production-like build. “This should be fast” is not evidence.

## Reusable prompt

```text
Implement this UI in reviewable checkpoints.

References:
[Screenshots/design and existing component examples]

Required states:
[Loading, empty, error, populated, interaction states]

Viewports:
[Widths/heights and expected layout changes]

Accessibility and behavior:
[Keyboard, focus, semantics, announcements]

Constraints:
- Reuse existing components and tokens.
- Do not add a dependency without approval.
- Preserve existing routes and data contracts.

After each checkpoint, run the relevant tests and inspect the rendered result.
Before handoff, capture required screenshots, exercise the user flow, inspect
console/network errors, and report accessibility checks and remaining gaps.
```

## Key takeaway

For frontend work, the rendered and interactive application is the evidence. Verify every important state, viewport, and input method—not only the code.

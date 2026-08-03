You are a coding agent that must build interfaces that are usable by everyone and reasonably fast. Apply these rules to frontend UI work alongside design_direction_rules.md and frontend_rules.md.

## Core directive
A distinctive, fast design and an accessible one are not in tension — treat accessibility and performance as baseline requirements, not nice-to-haves cut when time is short.

## 1. Semantic HTML
- Use semantic elements (`<button>`, `<nav>`, `<header>`, `<main>`, `<article>`) instead of generic `<div>`/`<span>` with click handlers bolted on — a `<div onClick>` is not a button and breaks keyboard/screen-reader users.
- Use proper heading hierarchy (`h1` → `h2` → `h3`, no skipping levels) so screen readers can navigate the page structure.
- Use `<label>` correctly associated with every form input (via `for`/`id` or wrapping) — never rely on placeholder text as a label substitute.

## 2. Keyboard & Screen Reader Access
- Every interactive element must be reachable and operable via keyboard alone (Tab, Enter, Space, Escape) — test this mentally for any custom component (dropdowns, modals, tabs).
- Custom interactive components (not native `<button>`/`<select>`) need appropriate ARIA roles and states (`aria-expanded`, `aria-selected`, `role="dialog"`, etc.) — don't build a custom dropdown with zero ARIA support.
- Images that convey meaning need descriptive `alt` text; purely decorative images get `alt=""` so screen readers skip them.
- Modals/dialogs must trap focus while open and return focus to the trigger element on close.

## 3. Color & Contrast
- Text must meet WCAG AA contrast minimums (4.5:1 for normal text, 3:1 for large text) against its background — check this explicitly when applying a custom palette from design_direction_rules.md, don't assume a chosen color combo passes.
- Never convey information by color alone (e.g. red/green only for error/success) — pair color with an icon, label, or text.
- UI controls and graphical elements that convey information must have at least 3:1 contrast against adjacent colors.

## 4. Performance
- Lazy-load images and heavy components below the fold instead of loading everything on initial page load.
- Optimize and appropriately size/compress images before shipping — don't serve a 4000px source image into a 400px container.
- Avoid unnecessary re-renders: memoize expensive computations and components where the framework supports it, rather than recalculating on every render.
- Keep bundle size in mind — don't pull in a large library for something a few lines of native code could do. When adding a dependency, consider the size impact — justify a 200KB library if 20 lines of code would suffice.
- Paginate or virtualize long lists rather than rendering thousands of DOM nodes at once.

## 5. Responsive & Device Behavior
- Verify layouts at mobile (320px), tablet (768px), and desktop (1024px+) breakpoints — not just the viewport you happened to build in.
- Respect `prefers-reduced-motion` for users who've opted out of animations — provide instant state changes as a fallback.
- Touch targets on mobile should be large enough to tap reliably (44x44px minimum).
- Typography should scale: body text readable on mobile without zooming, headings proportionate — don't use the same 48px heading on a 320px screen.
- Navigation patterns should adapt: full nav on desktop, hamburger or bottom nav on mobile — don't just shrink the desktop nav until it breaks.

## 6. Performance Budgets
- If the project has defined performance budgets (bundle size limits, load time targets, Lighthouse scores), check them before marking work as done — new features shouldn't silently blow the budget.
- When adding images, animations, or heavy assets, consider the impact on slower devices and connections — what looks great on a fast machine may be unusable on a budget phone.
- Aim for meaningful first paint under 1.5s, interactive under 3.5s on 3G connections as a baseline — measure, don't guess.
- Every new route/page should be code-split if the framework supports it — don't make the user download the entire app to see one page.

## 7. Forms & Input Accessibility
- Form validation errors must be programmatically associated with their input (via `aria-describedby` or similar) — not just a red border with no text.
- Required fields must be marked with more than just a visual asterisk — use `aria-required="true"` and label text.
- Form submission errors should announce themselves to screen readers (via `aria-live` region or focus management) — not just silently appear on screen.
- Multi-step forms should indicate progress clearly (step 2 of 4) and allow navigation back without losing data.

## What this rule does not cover
This does not cover visual style choices (see design_direction_rules.md) or security (see frontend_rules.md) — it's specifically about usability for people with disabilities, performance, and responsive behavior.
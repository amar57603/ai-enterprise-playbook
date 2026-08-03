You are building a product's visual design, not defaulting to whatever is fastest to generate. Apply these rules to any UI/UX work — new pages, components, layouts, color/typography choices, motion, and interaction design.

## Core directive
Never silently pick the safest, most generic design and ship it. "Safest" here means: default Tailwind palette, default system fonts, centered-card-on-white-background layouts, generic rounded buttons with no personality — the look that makes every AI-built site look the same. Avoid that as the default, not as an afterthought.

## 1. Design Direction
- **Never lock in a visual direction without showing the user options first.** Before building out a full page/feature UI, present 2-3 distinct visual directions (different color approach, typography, layout feel) as a quick preview or description, and let the user pick — don't assume and build straight to final.
- **Don't default to generic/templated aesthetics.** No plain white background + centered card + default blue button unless the user explicitly asks for a minimal/plain look. Make an intentional choice about color, type, and layout — and be able to explain why you chose it.
- **State the design direction in words before or alongside a preview**, e.g. "going bold/dark with a monospace accent font" vs "soft pastel, rounded, editorial serif headings" — so the user can steer by describing what they want rather than only reacting to finished code.
- **If the user hasn't specified a style, ask or propose options — don't assume "safe" is what they want.** Silence on style is not permission to default to generic; it's a prompt to offer directions.
- **Avoid recognizable "vibe-coded" tells**: the same overused gradient combos, the same emoji-in-heading pattern, the same generic hero-section-with-3-cards layout that shows up in every quick AI-generated site. If a layout feels like the default template of the framework/tool, treat that as a signal to push further, not a finished result.
- **When iterating, preserve the chosen direction** — don't drift back toward generic defaults on later pages/components just because it's faster; consistency with the direction the user picked matters as much as the initial choice.
- **Typography and color are decisions, not defaults.** Pick a font pairing and palette deliberately for the specific product's tone (playful, serious, technical, editorial, etc.) rather than reaching for whatever ships with the component library.
- **Theme and color palette are the user's choice, not yours to decide.** Never pick the final theme/palette yourself and apply it site-wide. Present a small set of concrete palette options (e.g. actual hex values or named swatches, not just "dark mode" vs "light mode") and wait for the user to pick before applying it across components. If the user already stated a color/theme preference earlier in the project, use that — don't re-litigate it, but also don't silently swap it out later without asking.

## 2. Motion & Animation
- Every state change the user triggers (page navigation, tab switch, modal open/close, accordion expand, item add/remove) should have a smooth transition — not an instant jump-cut. 200-300ms ease-out is a good default.
- Animations must be purposeful — they guide attention, provide feedback, or show spatial relationships. Don't add bounce/wobble effects just because they look "cool" with no functional reason.
- Loading transitions (skeleton screens, shimmer effects, progressive reveals) are preferred over raw spinners — they make the app feel faster even when it isn't.
- Page transitions and route changes should feel fluid, not jarring — a subtle fade or slide is better than a hard cut to a blank screen.

## 3. Interactive States
- Every interactive element must have visually distinct states: default, hover, focus, active, disabled — not just "it changes color on hover." Focus states are mandatory for keyboard accessibility.
- Buttons should provide immediate visual feedback on click (scale, color shift, ripple) — don't make the user wonder if their click registered.
- Disabled elements should look obviously disabled (reduced opacity, muted colors) and have a tooltip or nearby text explaining why they're disabled — don't leave users guessing.
- Links and clickable elements must have a cursor change (`pointer`) — never leave a clickable element with a default cursor.

## 4. Design System Consistency
- Use a consistent spacing scale (e.g. 4px, 8px, 12px, 16px, 24px, 32px, 48px) — don't eyeball margins and paddings with arbitrary values like 13px or 37px.
- Use a consistent type scale with defined sizes for headings, body, captions, and labels — don't invent new font sizes per component.
- Use a defined color palette with semantic tokens (primary, secondary, surface, error, success, warning) — don't pick hex values ad hoc per component.
- Consistent border radius across the app — don't mix 4px corners on buttons with 16px corners on cards unless it's a deliberate design hierarchy.
- Consistent shadow/elevation levels — define 2-3 shadow levels and use them systematically, not random box-shadow values per element.

## 5. Iconography & Visual Assets
- Use a single, consistent icon set throughout the app (Lucide, Phosphor, Material Symbols, Heroicons, etc.) — don't mix icon libraries or use random SVGs with different stroke widths and styles.
- Icons should be appropriately sized relative to their context (16px inline with text, 20-24px in buttons, 32-48px as feature icons) — not one size for everything.
- If the app needs illustrations, they should share a consistent style — don't mix flat illustrations with 3D renders with hand-drawn sketches.

## 6. Loading, Empty, and Error States
- Every view that fetches data must handle all three states: loading (skeleton or shimmer, not just a spinner), empty (designed empty state with helpful message and CTA), and error (clear error message with a retry option) — not just the happy path.
- Never show a blank white screen while data loads — it looks broken, not loading.
- Error messages must be human-readable ("Couldn't load your orders — check your connection and try again") not technical ("Error: FETCH_FAILED, status 500").
- If a partial load is possible (some data arrived, some failed), show what you have with an inline error for the failed section — don't blow away the entire page.

## 7. Scroll & Overflow
- Long content areas should have smooth scrolling with proper overflow handling — no content clipping without a scroll indicator.
- Sticky headers/nav should have a subtle shadow or border on scroll to indicate they're floating above content.
- Infinite scroll or pagination must have a clear loading indicator — don't just stop rendering and leave the user wondering if there's more.

## What this rule does not override
Accessibility (contrast ratios, readable font sizes) and usability are never sacrificed for distinctiveness — a bold design still has to be usable. If a design choice would hurt accessibility, say so and propose an alternative rather than silently picking looks over usability.

---

## 8. Specific Web Application & Design Mandates
- **The "Anti-AI-Generica" Rule:** The user absolutely HATES basic, generic, "AI-generated" looking websites (standard Tailwind defaults, centered white cards, basic 3-column features). You must avoid these.
- **Premium Aesthetics:** You must propose highly creative, premium visual directions (e.g., glassmorphism, overlapping grids, neobrutalism, rich gradients, scroll animations). 
- **THE USER DECIDES EVERYTHING:** You are forbidden from choosing the final aesthetic yourself. You must present 2-3 highly creative, non-generic options and wait for the user to make the final decision on the style, layout, and typography.
- **Dynamic Interfaces:** Once a style is chosen, implement dynamic design elements like hover effects and micro-animations to make the app feel alive.
- **No Placeholders:** If an image or asset is needed, generate a working demonstration or use an image generation tool rather than leaving placeholder text.
- **SEO Best Practices:** Automatically implement standard SEO practices (Title tags, semantic HTML5, heading hierarchy, unique IDs) on every page.
---
trigger: always_on
---

# Agent Rule: Web Excellence & Accessibility (Web-AX)

**Objective:** Deliver high-performance, mobile-first web experiences that meet global accessibility standards (WCAG 2.1 AA) while leveraging the Ruby on Rails "Native Way" (Hotwire/Stimulus).

---

## 1. Accessibility First (The "AA" Rule)

The agent must treat accessibility as a non-negotiable functional requirement rather than an aesthetic choice.

* **WCAG 2.1 AA Compliance:** All generated UI must adhere to **WCAG 2.1 AA standards**. This includes mandatory color contrast ratios, keyboard-only navigability, and focus management.
* **ARIA & Semantics:** Prioritize **native semantic HTML elements** (e.g., `<button>`, `<nav>`, `<main>`) over generic `<div>` soup to ensure screen reader compatibility. Only use `aria-*` attributes when native elements lack the necessary built-in behavior.
* **Media Standards:** Every image must include descriptive `alt` text. Interactive elements must have clear labels or `aria-label` attributes if no visible text is present.

## 2. Native Rails Performance (The "Hotwire" Rule)

The agent should favor server-side rendered HTML enhanced by **Hotwire** (Turbo/Stimulus) to minimize JavaScript bloat and maximize "Speed to First Paint".

* **Turbo-First:** Use **Turbo Frames and Streams** for partial page updates to keep the experience fast and responsive without full reloads.
* **Stimulus for Logic:** Use **Stimulus controllers** for client-side interactivity. Controllers should be small, reusable, and follow the standard Rails directory structure (`app/javascript/controllers/`).
* **User Feedback:** Implement **Turbo-friendly confirmations** (e.g., `data-turbo-confirm`) for destructive actions like "Delete".

## 3. Styling & Modern UX (The "Tailwind" Rule)

* **Utility-First Styling:** Use **Tailwind CSS** utility classes directly in `ViewComponents` or partials to ensure styling is encapsulated with the component logic.
* **Responsive Design:** Always adopt a **mobile-first approach**. Use Tailwind's responsive prefixes (e.g., `md:`, `lg:`) to ensure layout integrity across all device sizes.
* **Form Usability:** Use gems like `simple_form` to maintain consistent, accessible form layouts with clear error messaging and required field indicators.

## 4. Automated Audits & Verification (The "Guardrail" Rule)

The agent must verify UI quality using automated gates before proposing a merge:

* **Accessibility Testing:** Mandatory integration of accessibility auditors like **@axe-core/playwright** during the `pdd test` or unit testing phase.
* **UI/UX Audit:** Regularly perform UI/UX audits (as seen in successful real-world RoR projects) to uncover usability bottlenecks and crashes.
* **Deterministic Gates:** Run deterministic local gates (e.g., `prettier --check`, `lint`) to ensure the generated markup is clean and standardized.

## 5. SPDD & Acceptance Criteria (The "Contract" Rule)

When generating a **REASONS Canvas** for a UI feature, the agent must include:

* **Safeguards:** Define explicit accessibility invariants (e.g., "Must pass axe-core AA scan with zero violations").
* **Operations:** Specify the creation of **ViewComponents** for UI logic to ensure it is isolated and testable.
* **Acceptance Criteria:** Use "Given/When/Then" format to describe not just functional logic, but also the **accessible user journey** (e.g., "Then the screen reader should announce the success message").

### Implementation Note for the Agent

"When building any web interface, always ask: **'Can a user navigate this entire feature using only a keyboard and screen reader?'** If not, refactor to semantic elements or add the necessary ARIA attributes before generating the implementation".

---
trigger: always_on
---

# Agent Rule: Tailwind-First Styling (Utility-First Directive)

**Objective:** Standardize all styling using **Tailwind CSS** utility classes to ensure a high-performance, maintainable, and visually consistent UI. Custom CSS files are strictly forbidden unless a specific integration requires them (e.g., third-party libraries).

---

## 1. Utility-First Implementation (The "Class-Only" Rule)

The agent must treat HTML classes as the primary medium for styling.

* **Encapsulation:** Apply Tailwind utility classes directly within **ViewComponents** and Rails partials. This ensures styling is co-located with structure and logic, making components truly self-contained.
* **Zero-Custom-CSS Policy:** Do not create or append to `.css` or `.scss` files for standard UI elements. Leverage Tailwind's **Theme Variables** (colors, spacing, fonts) in the `tailwind.config.js` for project-wide customization rather than writing global CSS rules.
* **State Management:** Use Tailwind's state modifiers (e.g., `hover:`, `focus:`, `active:`, `disabled:`) for all interactive feedback.

## 2. Responsive & Mobile-First Strategy

The agent must adhere to a **Mobile-First** workflow.

* **Responsive Prefixes:** Build the base layout for mobile devices first. Use responsive modifiers (`sm:`, `md:`, `lg:`, `xl:`) only to scale up for larger screens.
* **Layout Utilities:** Prioritize **Flexbox** and **Grid** utilities for all layout structures to ensure maximum flexibility across different viewports.

## 3. Maintaining DRY via Components (Not CSS)

Avoid the temptation to use `@apply` in CSS files to "clean up" long class strings.

* **Component-Level DRY:** If a group of Tailwind classes is repeated (e.g., for a button), do not extract them into a CSS class. Instead, extract the HTML into a **ViewComponent** or a **Rails partial**.
* **Attribute Passing:** Ensure components accept an optional `class` or `classes` argument to allow for one-off layout adjustments (e.g., margin/padding) from the parent view without breaking encapsulation.

## 4. Interaction & Accessibility Integration

* **Focus Ring Standards:** Always include accessible focus states using utilities like `focus:ring-2` and `focus:ring-offset-2`.
* **Dark Mode:** When required, utilize the `dark:` prefix to handle theme switching at the utility level.
* **Interactivity:** Use Tailwind's interactivity utilities (e.g., `cursor-pointer`, `user-select-none`) to handle client-side feel without writing custom JavaScript where CSS suffices.

## 5. Development Workflow Validation

* **Build Process:** Always use the **`./bin/dev`** command to run the Rails server and the Tailwind CSS watcher simultaneously, ensuring styles are compiled in real-time during generation and testing.
* **Purge Awareness:** Avoid dynamic class name generation (e.g., `text-#{color}-500`) that prevents Tailwind's compiler from detecting used classes. Use full class strings in mapping objects instead.

---

### Implementation Note for the Agent

"When asked to style a feature, your first and only tool should be **Tailwind CSS utility classes**. If you find yourself reaching for a `<style>` tag or a new `.css` file, you must instead refactor the UI into a reusable **ViewComponent** where the Tailwind classes can be clearly organized and maintained. **Never use custom CSS for anything that can be achieved with a Tailwind utility**."

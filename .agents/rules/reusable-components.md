---
trigger: always_on
---

# Agent Rule: Component-First Architecture (The "Single Source of Truth" UI Rule)

**Objective:** Enforce a strict "Build Once, Reuse Always" policy for UI components using the **Ruby Native Way** (ViewComponent + Hotwire). The goal is to eliminate UI drift and technical debt by ensuring that every unique UI pattern has exactly one authoritative representation in the codebase.

---

## 1. The "Single Source of Truth" Directive (Standardization)

The agent must treat UI elements as modular, self-contained objects rather than disposable templates or snippets.

* **Encapsulation:** All reusable UI logic must reside in **`app/components/`** (using ViewComponent) or **`app/cells/`**, effectively removing display logic from models and partials.
* **TRUE Compliance:** To be truly reusable, every component must be **Transparent** (predictable changes), **Reasonable** (low complexity), **Usable** (reusable in new contexts), and **Exemplary** (sets the standard for others).
* **Size Constraints:** To prevent "bloated components," classes must remain under **100 lines** and methods under **5 lines**, ensuring they are focused enough to be reused without friction.

## 2. Generalization over Duplication (The "Build Once" Rule)

Before creating a new UI element, the agent must verify if a similar component exists and attempt to **generalize** it rather than duplicating code.

* **Parameterization:** Favor **keyword arguments** and **Dependency Injection** to handle variations in labels, icons, or styles instead of hardcoding values.
* **Duck Typing & Interfaces:** Components should focus on what an object **does** (its interface) rather than what it **is** (its class), allowing one component to render data from multiple different models.
* **Polymorphism over Conditionals:** Replace complex "if/else" styling logic within a component with **polymorphism** or specialized subclasses if the logic becomes too tangled.

## 3. The Exception Policy: When to Create Alternatives

Similar but different alternatives should only be considered in the following **exceptional cases**:

* **SRP Violation:** If generalizing an existing component would force it to have more than one reason to change, or require the words "and" or "or" to describe its purpose, create a separate alternative.
* **The "Rule of Three":** Do not commit to a complex inheritance hierarchy or a new variant until you have **three distinct use cases** that clearly demonstrate a specialization is necessary.
* **Orthogonal Responsibilities:** If a new UI requirement is truly orthogonal to the existing component's logic (e.g., a "Search Result" card vs. a "Product" card with entirely different interactions), a new component is justified.

## 4. Verification and Sync (The "Integrity" Rule)

* **Isolated Testing:** Every component must be testable in **isolation** through unit tests that assert the state of the rendered HTML for various input parameters.
* **SPDD Synchronization:** When a component is refactored or improved for better reuse, the agent must run **`/spdd-sync`** to ensure the design contract (REASONS Canvas) remains the accurate record of the UI's behavior.
* **Accessibility Guardrails:** Regardless of reuse, every variant must adhere to **WCAG 2.1 AA** standards, treating accessibility as a non-negotiable part of the component's public contract.

### Implementation Note for the Agent

"Before generating any new UI code, you must scan **`app/components/`** and **`app/cells/`**. If a similar pattern exists, you are **forbidden** from duplicating it; you must instead propose a refactor to make the existing component more flexible while maintaining its single responsibility".

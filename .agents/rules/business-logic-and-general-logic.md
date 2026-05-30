---
trigger: always_on
---

# Agent Rule: Logic Isolation & Componentization

**Objective:** Maintain a strict "separation of powers" between UI components, business transactions, and domain logic. Every unit of code must be independently testable without its collaborators.

---

## 1. Architectural Boundaries (The "Where" Rule)

The agent must categorize code based on its primary responsibility and place it in the corresponding directory to prevent "Fat Models" and "Bloated Controllers":

* **Business Transactions:** Use **Service Objects** in `app/services/` for any logic that involves multi-step processes, external API calls, or database-heavy transactions.
* **UI Encapsulation:** Use **ViewComponents** in `app/components/` for reusable UI logic. The ViewComponent should handle its own rendering logic and partials, while client-side interactivity is handled by dedicated **Stimulus controllers**.
* **Authorization:** All permission logic must reside in **Policies** (`app/policies/`) using the Pundit pattern.
* **Data Validation:** Complex, multi-model forms belong in **Form Objects** (`app/forms/`) to offload validation from ActiveRecord models.

## 2. Logic Isolation Guidelines (The "How" Rule)

* **Single Responsibility (SRP):** Every class must have one reason to change. Methods should stay under **5 lines** and classes under **100 lines**. If a method's purpose requires "and" to describe, it must be split.
* **Dependency Injection:** Never hard-code class names within another class. Inject dependencies via constructors to allow the agent to swap them with **test doubles** during unit testing.
* **ActiveRecord Restrictions:** Prohibit the use of lifecycle callbacks (e.g., `after_save`) for business logic. These create "hidden" side effects that make testing difficult. Move this logic to an explicit **Service Object**.
* **The Law of Demeter:** Objects should only talk to their "immediate neighbors." Avoid long method chains (e.g., `@order.customer.address.city`). Use delegation instead.

## 3. Test-First Verification (The "Testable" Rule)

* **Isolation:** Every component or service must be testable in **isolation**. If a test requires complex database setup or "everything" to be loaded, the code is too tightly coupled and must be refactored.
* **Public Interface Focus:** Tests must only assert the **incoming public messages** (return values) of an object. Never test private methods.
* **Command vs. Query:**
  * **Queries:** Verify the returned state.
  * **Commands:** Verify that the correct outgoing messages were sent (behavior-driven) using mocks.

## 4. SPDD Integration (The "Alignment" Rule)

When using the **Open-SPDD workflow** in Antigravity, the agent must adhere to these steps before writing code:

* **Strategic Analysis:** Run `/spdd-analysis` to identify where new domain concepts fit into the existing architecture.
* **REASONS Canvas:** Use `/spdd-reasons-canvas` to explicitly define the **Structure** (components and dependencies) and **Operations** (step-by-step task breakdown) before generation.
* **Closed-Loop Refactoring:** If a code change is made during review, the agent must use `/spdd-sync` to reflect those implementation details back into the Canvas, ensuring the "Design Source of Truth" never drifts.

### Implementation Note for the Agent

"When generating or refactoring code, always ask: **'Could I test this class without hitting the database or loading the entire Rails framework?'** If the answer is no, suggest a Service Object or a PORO (Plain Old Ruby Object) to isolate the core logic".

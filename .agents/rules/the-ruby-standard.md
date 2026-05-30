---
trigger: always_on
---

# Agent Rule: Fundamental Design Philosophy (POODR)

The agent must adhere to Sandi Metz’s **Practical Object-Oriented Design (POODR)** principles to ensure code is maintainable and scalable.

* **The TRUE Principle:** All code must be **Transparent** (consequences of change are obvious), **Reasonable** (cost of change is proportional to complexity), **Usable** (reusable in new contexts), and **Exemplary** (sets a high standard for future code).
* **Single Responsibility (SRP):** Every class and method should do one thing and do it well.
  * **Standard:** Methods should ideally be under **5 lines**; classes should be under **100 lines**.
  * **Validation:** If a class’s purpose requires the word "and" or "or" to describe it, it likely needs refactoring.
* **Dependency Injection:** Do not hard-code class names inside other classes; inject dependencies via constructors or methods to keep objects decoupled.
* **Duck Typing:** Focus on what an object **does** (its public interface) rather than what it **is** (its class).

## 2. Rails Architectural Standards

To prevent "fat models" and "bloated controllers," the agent should use advanced directory structures for enterprise-scale applications.

* **Custom Subdirectories:** Beyond the standard MVC folders, use the following for specific logic:
  * `app/services/`: For procedural business transactions and complex logic.
  * `app/policies/`: For action-level authorization rules (e.g., using the **Pundit** pattern).
  * `app/decorators/` or `app/presenters/`: For object-oriented presentation logic to keep views clean.
  * `app/forms/`: For encapsulating multi-model validation and data persistence.
* **ActiveRecord Restrictions:**
  * **Avoid Callbacks:** Do not use `after_save` or `after_commit` for business logic; these obscure execution paths and slow down tests. Move this logic into **Service Objects**.
  * **Scope Prefixes:** Use prefixes like `for_` (filtering by relation), `with_` (boolean conditions), and `including_` (eager loading) to make query behavior explicit.

## 3. Idiomatic Ruby & Formatting

The agent must follow the **Shopify, Airbnb, or GitLab style guides** to ensure consistent, readable code.

* **Naming Conventions:**
  * Use `snake_case` for variables, methods, and files.
  * Use `PascalCase` for classes and modules.
  * Name predicate methods (those returning a boolean) with a trailing `?`.
* **Formatting Essentials:**
  * Use **2-space indentation** (no tabs) and **UTF-8** encoding.
  * Avoid trailing whitespace and limit lines to **120 characters** (or 100 per Airbnb).
  * Prefer **keyword arguments** over positional arguments for clarity and stability.
* **Logic Simplification:** Use **guard clauses** to return early and reduce deep nesting of `if` statements.

## 4. Testing & Quality Assurance

The agent should follow the **FIRST principle** for automated testing: **Fast, Isolated, Repeatable, Self-verifying, and Timely**.

* **Framework Preference:** Use **RSpec** for behavior-driven development (BDD) with readable specifications, paired with **FactoryBot** for test data.
* **Interface Testing:** Only test an object’s **incoming public messages**; never test private methods, as they are implementation details that change frequently.
* **Mocking:** Use test doubles or mocks to isolate the object under test from its collaborators.

## 5. Mandatory Gem Integrations

For every feature, the agent should verify compatibility or suggest usage of these essential "best-in-class" gems:

* **Security:** **Brakeman** (vulnerability scanning) and **Bundler Audit** (dependency auditing).
* **Performance:** **Bullet** (detecting N+1 queries) and **Sidekiq** (background processing).
* **Formatting:** **RuboCop** (strict linting following the defined style guide).
* **Debugging:** **Pry** (for interactive runtime introspection).

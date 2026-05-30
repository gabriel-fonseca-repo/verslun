---
trigger: always_on
---

# Agent Rule: Sovereign Domain-Driven Design (DDD)

**Objective:** Transform complex business requirements into a rich, behavioral domain model. The agent must strictly prioritize domain integrity over technical convenience, enforcing a "Domain-First" architecture where code is a downstream artifact of design intent.

---

## 1. Strategic Design & Context Integrity

The agent must never operate on a codebase as a unified whole, but rather as a portfolio of isolated models.

* **Bounded Context Enforceability:** Every feature must be localized to a specific **Bounded Context**. If a requirement spans contexts, the agent must define a **Context Map** and use an **Anti-Corruption Layer (ACL)** to translate between different ubiquitous languages.
* **Subdomain Classification:** The agent must classify the target logic as **Core** (unique business value), **Supporting** (necessary but not a differentiator), or **Generic** (standardized functionality).
  * *Core Domains* require a full, rich behavioral model.
  * *Generic Domains* should prioritize off-the-shelf or simpler (CRUD) implementations.
* **Ubiquitous Language (UL):** The agent must use terms derived directly from domain experts in all class names, methods, and variables. If the code uses technical jargon (e.g., `UpdateRecord`) where the domain uses a business verb (e.g., `RenewPolicy`), the agent must refactor to the business term immediately.

## 2. Tactical Implementation Standards

The agent must adhere to the strict technical building blocks of DDD.

* **Aggregates as Consistency Boundaries:**
  * An **Aggregate** is a cluster of objects treated as a single unit for data changes.
  * **Rule of One:** The agent must modify only **one** Aggregate instance per transaction.
  * **Identity Referencing:** Aggregates must reference other Aggregates **only by global identity (ID)**, never via direct object references.
  * **Small Size:** Favor Aggregates composed of a single Root Entity and minimal Value Objects.
* **Entities vs. Value Objects:**
  * **Entities:** Use only for concepts with a unique identity and a lifecycle (e.g., a specific Order).
  * **Value Objects:** Prioritize these for describing attributes. They must be **immutable** and have no identity. Always replace primitive types (strings, integers) with **Domain Primitives** (Value Objects) to prevent "Primitive Obsession".
* **Domain Services:** Use only for operations that do not naturally belong to a single Entity or Value Object. They must be **stateless**.
* **Domain Events:** The model must publish **Domain Events** to signify meaningful state changes, enabling **eventual consistency** across Aggregate boundaries.

## 3. Architectural Purity (The "Inside/Outside" Rule)

The agent must enforce a **Hexagonal/Clean Architecture** to protect the domain core.

* **Persistence Ignorance (PI):** The Domain layer must be composed of **Plain Old Ruby Objects (POROs)**. It is forbidden for domain entities to inherit from infrastructure base classes (like `ActiveRecord::Base` if using strict DDD) or reference database-specific logic.
* **Dependency Inversion:** Infrastructure (databases, APIs) must depend on the Domain, never the reverse.
* **Rich vs. Anemic Models:** The agent is prohibited from generating "Anemic Domain Models" (data-only objects with external logic). Entities must encapsulate both state and the **business rules** that govern it.

## 4. Operational Workflow (SPDD Enforcement)

When implementing a feature, the agent must follow the **Structured-Prompt-Driven Development (SPDD)** "Closed Loop".

* **REASONS Canvas First:** Before a single line of code is written, the agent must generate a **REASONS Canvas** (Requirements, Entities, Approach, Structure, Operations, Norms, Safeguards).
* **Intent Synchronization:**
  * **Logic Corrections:** If the business logic needs adjustment, the agent must **update the prompt (Canvas) first**, then regenerate the code.
  * **Refactoring Sync:** If the code is refactored for style, the agent must run `/spdd-sync` to propagate those changes back to the design contract.
* **Safeguards as Invariants:** The **Safeguards** section of the Canvas must explicitly define the **domain invariants** (non-negotiable rules) that the Aggregate must protect.

## 5. Logic & Information Hiding

* **Tell, Don't Ask:** A client object must not interrogate a server object's state to make a decision. It must "tell" the server what to do via its public interface.
* **Law of Demeter:** Objects must only interact with their immediate neighbors. Deep method chaining (e.g., `user.account.balance.currency`) is strictly forbidden.

### Mandatory Verification

"Before declaring a task 'Done', the agent must verify:

1. Does the Ubiquitous Language in the code match the REASONS Canvas?
2. Are all state transitions guarded by domain invariants within the Aggregate?
3. Is the Domain layer 100% free of infrastructure/framework dependencies?
4. If multiple Aggregates were affected, was eventual consistency used instead of a single transaction?"

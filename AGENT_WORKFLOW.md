

# AI Agent Workflow Log

## Agents Utilized

* **ChatGPT (GPT-5)** — Primary assistant for backend design, API architecture, debugging, and TypeScript/PostgreSQL integration.
* **Lovable AI** — Used to scaffold the React + Tailwind interface and generate reusable UI layouts.
* **Google Gemini** — Assisted in validating FuelEU formulas and logic behind CB, banking, and pooling operations.

---

## Prompts & Outputs

### 1. Backend Architecture & Setup

**Prompt:**
“Create a Node.js + TypeScript backend following Hexagonal Architecture for FuelEU Maritime compliance, including APIs for routes, banking, pooling, and CB calculations.”

**AI Response:**
Produced the backend structure (`core`, `ports`, `adapters`, `infrastructure`), initial repository interfaces, and Express handlers.
ChatGPT also proposed isolating use-cases (`ComputeCB`, `BankSurplus`, `CreatePool`) to maintain clean domain boundaries.

**Validation:**
I reviewed generated code, fixed import path mismatches, ensured type safety, and tested API responses using migrations and Postgres.

---

### 2. Frontend Dashboard Development

**Prompt:**
“Build a React + TypeScript + Tailwind dashboard with tabs for Routes, Compare, Banking, and Pooling, all connected to backend APIs.”

**AI Response:**
Lovable produced the basic interface, tab structure, and Tailwind styling.
ChatGPT handled API bindings, improved state management, and added error handling.

**Validation:**
Manually tested tab navigation, data fetching, and form interactions.
Checked Compare tab values against backend calculations for accuracy.

---

### 3. Compliance Formula Verification

**Prompt:**
“Confirm whether the CB, target intensity, banking, and pooling logic follow FuelEU Maritime rules.”

**AI Response:**
Gemini confirmed formula correctness and verified constants.
ChatGPT implemented and refined the TypeScript logic accordingly.

**Validation:**
Evaluated against sample route data and internal unit tests to ensure positive/negative balances behaved consistently.

---

## Validation & Corrections

* Developed unit tests for all major use-cases via in-memory adapters:
  `ComputeCB`, `ComputeComparison`, `BankSurplus`, `ApplyBanked`, `CreatePool`.
* Added endpoint tests using Supertest for `/routes` and `/banking`.
* Fixed issues with environment loading, missing types, and strict-mode errors.
* Reviewed each AI-generated file to ensure correctness and maintainable architecture.

---

## Observations

* **ChatGPT** was strongest for backend reasoning, type safety, and architecture patterns.
* **Lovable** accelerated UI scaffolding dramatically.
* **Gemini** provided reliable mathematical and rules-based validation.
* Combining all three reduced development workload by ~60%.
* Human review remained critical for alignment with actual FuelEU requirements.

---

## Best Practices Applied

* Adopted a full **Hexagonal Architecture** with strict separation: core → ports → adapters.
* Enabled **TypeScript strict mode** across both client and server.
* Limited framework usage to adapter layers only.
* Validated every AI-generated artifact through tests and manual inspection before committing.


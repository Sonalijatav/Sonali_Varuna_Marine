

# 🛳️ FuelEU Maritime — Compliance Suite (Frontend + Backend)

A compact implementation of a **Fuel EU Maritime compliance** system using a **hexagonal architecture** on both client and server. It includes domain logic for **routes**, **compliance balance (CB)**, **banking**, and **pooling**, along with a **React + Tailwind** dashboard that consumes the backend APIs.

---

## 🧭 What’s Inside

* **Frontend:** React + TypeScript + TailwindCSS (Vite)
* **Backend:** Node.js + TypeScript + Express + PostgreSQL
* **Architecture:** Hexagonal / Ports & Adapters
* **Docs:** `AGENT_WORKFLOW.md` (agent trail), `REFLECTION.md` (short essay)
* **Tests:** Vitest-based unit tests for core use-cases + a tiny HTTP integration test (in-memory adapters)

---

## 🧱 Project Layout (Hexagonal)

```
backend/src
  core/
    domain/            # pure types only
    application/       # business/use-case logic
    ports/             # interfaces for repos/services
  adapters/
    inbound/http/      # Express HTTP (inbound adapter)
    outbound/postgres/ # Postgres repositories (outbound)
  infrastructure/
    db/                # migrations + seed data
    server/            # app composition & bootstrapping
  shared/              # cross-cutting constants/utilities

frontend/src
  core/                # framework-agnostic domain
  adapters/
    ui/                # React components/pages (inbound)
    infrastructure/    # API client (outbound)
```

The **core** stays framework-free. **Adapters** implement the **ports**. **Infrastructure** wires everything together.

---

## ⚙️ Backend — Setup & Run

1. Configure environment:

   ```bash
   cd backend
   cp .env.example .env   # set DATABASE_URL and PORT if needed
   ```

2. Install packages:

   ```bash
   npm i
   ```

3. Run database migration:

   ```bash
   npm run migrate
   ```

4. Seed initial data:

   ```bash
   npm run seed
   ```

5. Start the dev server:

   ```bash
   npm run dev
   ```

   Server lives at **[http://localhost:3001](http://localhost:3001)**

### 📜 NPM Scripts

* `npm run test` — runs Vitest unit + integration tests (uses in-memory adapters)
* `npm run build` + `npm start` — compile and run the built server

---

## 💻 Frontend — Setup & Run

1. Install deps:

   ```bash
   cd frontend
   npm i
   ```

2. Launch Vite:

   ```bash
   npm run dev
   ```

   App runs at **[http://localhost:5173](http://localhost:5173)**

> The Vite dev server is configured to proxy API requests to **[http://localhost:3001](http://localhost:3001)**.

---

## 🔗 API Map

* `GET /routes` — fetch seeded routes
* `POST /routes/:id/baseline` — choose baseline route
* `GET /routes/comparison` — compare baseline vs others, with percent difference & compliance flag
* `GET /compliance/cb?shipId&year` — compute and persist CB snapshot
* `GET /compliance/adjusted-cb?shipId&year` — CB adjusted by banked applications
* `GET /banking/records?shipId&year` — banking ledger summary
* `POST /banking/bank` — store a positive CB for later use
* `POST /banking/apply` — apply banked surplus to cover deficits
* `POST /pools` — greedy redistribution; returns before/after CB per member

### 🧩 Calculation Notes

* In sample data, `shipId` corresponds to `routeId` (e.g., `R001`).
* Energy scope: `fuelConsumption × 41,000 MJ/t`.
* CB formula: `(Target (89.3368) − Actual) × Energy`.

---

## 🧪 Testing

* Core unit tests: `ComputeCB`, `ComputeComparison`, `BankSurplus`, `ApplyBanked`, `CreatePool`
* Minimal HTTP integration test (no DB) using in-memory adapters

Run:

```bash
cd backend
npm test
```

---

## 🖼️ UI Previews

* **Routes Tab** — filters by vessel/fuel/year; select baseline
  *(`docs/screenshots/Routes.png`)*

* **Compare Tab** — baseline vs others with % difference + simple chart
  *(`docs/screenshots/Compare.png`)*

* **Banking Tab** — shows CB, bank/apply actions with validation
  *(`docs/screenshots/Banking.png`)*

* **Pooling Tab** — input members, validate sum, create pool & see outcomes
  *(`docs/screenshots/Pooling.png`)*

---

## 🧾 Extra Notes

* **TypeScript strict** is enabled; ESLint/Prettier can be added as desired.
* **PostgreSQL** integration uses `pg` in the outbound adapter.
* Tests avoid DB coupling by relying on in-memory repositories.


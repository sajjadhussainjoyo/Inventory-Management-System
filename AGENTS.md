# AGENTS.md — Inventory Management System (React Native + Expo)

This file gives AI coding agents (Cursor, Windsurf, Copilot Workspace, Claude Code, etc.) the context needed to work on this codebase correctly and consistently. Follow these instructions for every task unless the user explicitly overrides them in a prompt.

---

## 1. Project Overview

**Name:** Inventory Management System (IMS)
**Type:** Mobile application
**Framework:** React Native
**Tooling:** Expo (managed workflow, using Expo Router / EAS where applicable)
**Purpose:** Allow businesses/users to track products, stock levels, suppliers, sales, and warehouse/location data from a mobile app.

Core modules to build (expand as the project grows):
- Authentication (login, signup, roles: Admin / Staff)
- Dashboard (stock overview, low-stock alerts, quick stats)
- Product Management (CRUD: add/edit/delete/view products, categories, SKU, barcode)
- Stock Management (stock-in, stock-out, stock transfer, adjustments)
- Supplier Management (CRUD)
- Sales / Orders (create order, order history, invoices)
- Reports & Analytics (stock value, top-selling items, low-stock report)
- Notifications (low-stock / out-of-stock alerts)
- Settings & Profile

---

## 2. Tech Stack

| Layer | Choice |
|---|---|
| Framework | React Native (via Expo SDK, latest stable) |
| Language | TypeScript (strict mode) — no plain `.js` files for app code |
| Navigation | Expo Router (file-based routing) |
| State Management | Zustand (or Redux Toolkit if app grows complex) |
| Data Fetching | TanStack Query (React Query) |
| Backend / DB | Firebase (Firestore + Auth) **or** REST API (FastAPI) — confirm which before generating backend-calling code; default to Firebase if unspecified |
| Local Storage | AsyncStorage / Expo SecureStore (for tokens) |
| Forms & Validation | React Hook Form + Zod |
| UI Components | React Native Paper or NativeWind (Tailwind for RN) — pick one and stay consistent |
| Icons | @expo/vector-icons |
| Barcode/QR | expo-camera / expo-barcode-scanner |
| Charts (reports) | react-native-chart-kit or Victory Native |
| Testing | Jest + React Native Testing Library |
| Linting/Formatting | ESLint + Prettier |

---

## 3. Project Structure

Use this structure (create folders as needed, don't flatten everything into one screen file):

```
/app                    → Expo Router screens (file-based routing)
  /(auth)                → login, signup, forgot-password
  /(tabs)                → main tab screens: dashboard, inventory, orders, reports, profile
  /product/[id].tsx       → product detail/edit
  _layout.tsx
/components             → Reusable UI components (Button, Card, InputField, etc.)
/features                → Feature-based logic (inventory/, orders/, suppliers/, auth/)
  /inventory
    /components
    /hooks
    /api
    /types.ts
/hooks                  → Shared custom hooks
/lib                     → Firebase config, API client, query client setup
/store                   → Zustand stores (or redux slices)
/constants               → Colors, spacing, app-wide constants
/types                   → Global TypeScript types/interfaces
/utils                   → Helper functions (formatDate, calculateStock, etc.)
/assets                  → Images, fonts, icons
app.json / app.config.ts
tsconfig.json
```

**Rule for agents:** New features go under `/features/<feature-name>/`, not scattered across `/components`. Shared, generic UI belongs in `/components`.

---

## 4. Coding Conventions

- **TypeScript everywhere.** Define proper interfaces/types for Product, Supplier, Order, StockMovement, User, etc. in `/types`.
- **Functional components + hooks only.** No class components.
- **Naming:**
  - Components: `PascalCase` (e.g., `ProductCard.tsx`)
  - Hooks: `useCamelCase` (e.g., `useInventoryStock.ts`)
  - Files for screens follow Expo Router conventions (lowercase, dynamic segments in `[brackets]`)
- **One component per file.** Keep components small and focused; extract logic into hooks when a component grows past ~150 lines.
- **No inline styles for anything reused more than once** — use `StyleSheet.create` or the chosen styling system (NativeWind classes) consistently.
- **Async logic** goes through React Query hooks (`useProducts`, `useAddProduct`, etc.) inside `/features/<feature>/api`, never fetched directly inside components.
- **Error handling:** every async call must handle loading/error/empty states in the UI — no silent failures.
- **Comments:** explain *why*, not *what*, and only where logic isn't self-evident.

---

## 5. Data Model (starting point — extend as needed)

```ts
interface Product {
  id: string;
  name: string;
  sku: string;
  barcode?: string;
  category: string;
  quantity: number;
  unit: string;          // pcs, kg, box, etc.
  costPrice: number;
  sellingPrice: number;
  reorderLevel: number;  // triggers low-stock alert
  supplierId?: string;
  imageUrl?: string;
  createdAt: string;
  updatedAt: string;
}

interface StockMovement {
  id: string;
  productId: string;
  type: "in" | "out" | "adjustment" | "transfer";
  quantity: number;
  reason?: string;
  performedBy: string;   // userId
  timestamp: string;
}

interface Supplier {
  id: string;
  name: string;
  contactPerson?: string;
  phone?: string;
  email?: string;
  address?: string;
}

interface Order {
  id: string;
  items: { productId: string; quantity: number; price: number }[];
  totalAmount: number;
  status: "pending" | "completed" | "cancelled";
  createdAt: string;
}
```

---

## 6. Agent Instructions (How to Work on This Repo)

1. **Before writing code**, check `/types` and `/features` to see if the model or logic already exists — don't duplicate.
2. **Always use Expo-compatible packages.** Before adding a native module, confirm it has an Expo config plugin or is part of the Expo SDK. Avoid packages that require bare/ejected workflow unless explicitly requested.
3. **Run `npx expo install <package>`** (not plain `npm install`) for any Expo-managed native dependency, so versions stay compatible with the installed SDK.
4. **When generating a new screen:** create it under the correct `/app` route group, wire it into navigation, and create matching types/hooks if data is involved.
5. **When generating a new feature (e.g., "Supplier Management"):** scaffold `types.ts`, `api/` (query hooks), `components/`, and the screen(s) together — don't just drop a screen with hardcoded data.
6. **Keep business logic out of UI components.** Stock calculations, low-stock checks, totals, etc. belong in `/utils` or feature hooks, and should be unit-testable.
7. **Follow the existing styling system.** Don't mix NativeWind and StyleSheet approaches in the same file.
8. **Ask before assuming the backend.** If a task needs backend/API work and it's unclear whether the project uses Firebase or a REST API, ask, or clearly state the assumption at the top of the generated code.
9. **Don't break existing navigation structure** when adding new routes — check `_layout.tsx` files first.
10. **Write small, reviewable diffs.** Prefer completing one feature/screen cleanly over touching many files partially.
11. **Add basic input validation** (Zod schema + React Hook Form) for every form (add product, add supplier, stock adjustment, etc.).
12. **Accessibility:** use proper `accessibilityLabel` on interactive elements and ensure touch targets are large enough.

---

## 7. Commands

```bash
# start dev server
npx expo start

# run on Android emulator
npx expo run:android

# run on iOS simulator (Mac only)
npx expo run:ios

# type check
npx tsc --noEmit

# lint
npx eslint . --ext .ts,.tsx

# format
npx prettier --write .

# run tests
npx jest
```

---

## 8. Environment & Secrets

- Store Firebase config / API base URL in `.env` and load via `expo-constants` / `react-native-dotenv` — never hardcode keys in source files.
- `.env` must be listed in `.gitignore`.
- Auth tokens must be stored in `expo-secure-store`, not `AsyncStorage`.

---

## 9. Definition of Done (per feature)

A feature/task is complete only when:
- [ ] TypeScript types defined
- [ ] UI screen(s) built and wired into navigation
- [ ] Data layer (React Query hooks) implemented, with loading/error/empty states
- [ ] Form validation added (if applicable)
- [ ] Basic unit test added for any non-trivial utility/business logic
- [ ] No console errors/warnings in Expo dev tools
- [ ] Code passes lint + type check

---

## 10. Out of Scope (unless explicitly asked)

- Do not eject from Expo managed workflow.
- Do not introduce a second state-management library alongside the chosen one.
- Do not add a backend/server implementation inside this repo unless the task specifically asks for it — this repo is the mobile app.

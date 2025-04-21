# Car Marketplace Pivot: Detailed Plan

This document outlines the detailed steps for refactoring the `nestar-next` (frontend) and `nestar` (backend) projects from a Real Estate theme to a Car Marketplace theme.

**Goal:** Transform the platform into a Car Marketplace.

**Core Concept Mapping:**

*   `Property` -> `Vehicle`
*   `Agent` -> `Dealer` (or `Seller` - requires consistent terminology decision)
*   `Member` -> `User` (or `Buyer`)

---

## Detailed Plan

### Phase 1: Backend Refactoring (`nestar` monorepo)

1.  **Rename Core Modules (`nestar/apps/nestar-api/src/components/`)**:
    *   Rename `property` directory -> `vehicle`.
    *   Rename `agent` directory -> `dealer` (or chosen term).
    *   Update `components.module.ts` imports/exports.

2.  **Refactor `vehicle` Module**:
    *   Rename internal files (e.g., `property.service.ts` -> `vehicle.service.ts`).
    *   Rename classes/interfaces (`Property` -> `Vehicle`).
    *   Update DTOs (`inputs/property.input.ts` -> `inputs/vehicle.input.ts`): Replace fields (bedrooms, sqft) with vehicle specifics (make, model, year, mileage, body type, engine, VIN, etc.). Adjust validation.
    *   Update service/resolver logic.

3.  **Refactor `dealer` Module**:
    *   Rename internal files, classes, interfaces (`Agent` -> `Dealer`).
    *   Update DTOs (`inputs/agent.input.ts` -> `inputs/dealer.input.ts`): Adjust fields (e.g., dealership name, license number).
    *   Update service/resolver logic.

4.  **Update Database Schemas (`nestar/apps/nestar-api/src/schemas/`)**:
    *   Rename `property.model.ts` -> `vehicle.model.ts`. Update Mongoose schema definition for vehicles. Change collection name.
    *   Rename `agent.model.ts` -> `dealer.model.ts`. Update schema definition. Change collection name.
    *   Review other schemas (`member`, `comment`, etc.) for `Property`/`Agent` references and update to `Vehicle`/`Dealer` refs.

5.  **Update GraphQL Schema & Resolvers**:
    *   Modify GraphQL types (code-first or schema-first) to replace `Property`/`Agent` with `Vehicle`/`Dealer` and update fields.
    *   Update queries/mutations (e.g., `getProperties` -> `getVehicles`).
    *   Ensure resolvers implement the updated schema.

6.  **Refactor Interaction Modules (`like`, `follow`, `comment`, `view`)**:
    *   Update references to point to `Vehicle` and `Dealer`.
    *   Adjust logic if interaction behavior changes (unlikely).

7.  **Refactor `nestar-batch` Application**:
    *   Examine jobs in `nestar/apps/nestar-batch/src/`.
    *   Adapt or remove jobs specific to real estate data.

8.  **Update Backend Tests (`nestar/apps/.../test/`)**:
    *   Modify unit and e2e tests to reflect renamed modules, DTOs, schema changes, and logic updates.

### Phase 2: Frontend Refactoring (`nestar-next`)

1.  **Rename Page Directories (`pages/`)**:
    *   Rename `pages/property/` -> `pages/vehicle/`.
    *   Rename `pages/agent/` -> `pages/dealer/`.

2.  **Update Routing and Page Components**:
    *   Refactor components within renamed directories (e.g., `pages/vehicle/[id].tsx`).
    *   Update links/routing logic to use `/vehicle/...`, `/dealer/...`.
    *   Refactor `pages/index.tsx`: Replace property/agent components (e.g., `PopularProperties`) with vehicle/dealer equivalents (`FeaturedVehicles`).
    *   Update `pages/mypage/` components.

3.  **Refactor Core Components (`libs/components/`)**:
    *   Identify property/agent related components (cards, lists, forms, detail views).
    *   Rename them (e.g., `PropertyCard.tsx` -> `VehicleCard.tsx`).
    *   Update props and internal logic for `Vehicle`/`Dealer` data types.
    *   Adjust UI to display vehicle info (make, model, price, mileage).

4.  **Update GraphQL Operations**:
    *   Update Apollo Client `useQuery`/`useMutation` definitions to match backend schema changes (query `getVehicles`, use `Vehicle` fragments).
    *   Update consuming code to expect new data structures.

5.  **Update State Management**:
    *   Update any state stores (Context, Zustand, Valtio) related to properties/agents.

6.  **Update Internationalization Files (`public/locales/`)**:
    *   Replace real estate terms (property, agent, sqft) with automotive terms (vehicle, dealer, mileage) in all `*.json` files.

7.  **Refactor Styles (`scss/`)**:
    *   Rename/update SCSS files related to old components/pages.
    *   Adjust styles for the new vehicle/dealer layouts. Consider a new visual theme.

8.  **Update Frontend Tests**:
    *   Modify tests to match new component names, props, UI text, and flows.

### Phase 3: Database & Configuration

1.  **Database Migration / Clean Slate**:
    *   Decide whether to migrate data or start fresh (recommended for theme pivot).
    *   If migrating, write scripts to transform `properties`/`agents` collections to `vehicles`/`dealers`.

2.  **Environment Variables (`.env`, `.env.local`)**:
    *   Review and update if necessary (unlikely unless API/DB details change).

3.  **Deployment Configuration (Docker, Vercel, etc.)**:
    *   Update environment variables in deployment settings.
    *   Verify build processes.

### Phase 4: Testing & Refinement

1.  **Thorough End-to-End Testing**: Test all user flows manually (browsing, viewing details, contacting seller, listing, community).
2.  **Cross-Browser/Device Testing**: Ensure responsiveness and compatibility.
3.  **Performance Review**: Check database query performance, add indexes if needed.
4.  **Code Cleanup**: Remove dead code related to the old Real Estate theme.

---

**Next Steps:** Begin with Phase 1, starting with renaming backend modules and updating corresponding schemas and DTOs. 
# Plan: Renaming Project to "penthouse"

This document outlines the steps to thoroughly rename the project from "nestar" / "nestar-next" to "penthouse-backend" / "penthouse-frontend" across both repositories.

**Goal:** Replace all occurrences of "nestar" and "nestar-next" (and related casings like "Nestar") with new project names derived from "penthouse" throughout the codebase, including directory structures, file contents, configurations, and documentation.

**New Names Used:**

*   Backend Root Directory/Project: `penthouse-backend`
*   Frontend Root Directory/Project: `penthouse-frontend`
*   Backend API Application: `penthouse-api`
*   Backend Batch Application: `penthouse-batch`

**WARNING:** This is a complex and potentially disruptive operation. Proceed with caution, commit changes frequently in logical steps on a dedicated branch, and test thoroughly at each stage. Automated refactoring tools can help but may miss some occurrences, especially in strings or configuration files.

---

## Phase 1: Filesystem & Core Configuration Renaming

**Objective:** Rename root directories and update primary configuration files.

1.  **Rename Root Directories:**
    *   Task: Rename the main project folders [External - File system operation].
        *   Rename `nestar` to `penthouse-backend`
        *   Rename `nestar-next` to `penthouse-frontend`
2.  **Rename Backend App Directories (`penthouse-backend`):**
    *   Task: Rename the application directories within the backend monorepo [External - File system operation].
        *   Rename `penthouse-backend/apps/nestar-api` to `penthouse-backend/apps/penthouse-api`
        *   Rename `penthouse-backend/apps/nestar-batch` to `penthouse-backend/apps/penthouse-batch`
3.  **Update Backend `package.json` (`penthouse-backend/package.json`):**
    *   Task: Edit the main backend `package.json` [IDE].
        *   Change the `"name"` field from `"nestar"` to `"penthouse-backend"`.
        *   Review `"scripts"` section for any hardcoded paths or names like `nestar-batch`, `nestar-api` and update them to `penthouse-batch`, `penthouse-api`.
4.  **Update Frontend `package.json` (`penthouse-frontend/package.json`):**
    *   Task: Edit the frontend `package.json` [IDE].
        *   Change the `"name"` field from `"nestar-next"` to `"penthouse-frontend"`.
        *   Review `"scripts"` for any relevant references.
5.  **Update Backend `nest-cli.json` (`penthouse-backend/nest-cli.json`):**
    *   Task: Edit the NestJS CLI configuration [IDE].
        *   Update the keys under the `"projects"` object from `"nestar-api"` and `"nestar-batch"` to `"penthouse-api"` and `"penthouse-batch"`.
        *   Verify paths associated with these projects (e.g., `"sourceRoot"`, `"entryFile"`) reflect the new directory names (`apps/penthouse-api/src`, `apps/penthouse-batch/src`, etc.).
6.  **Update Backend `tsconfig.json` Paths (if applicable):**
    *   Task: Check the root `penthouse-backend/tsconfig.json` (and potentially `apps/penthouse-api/tsconfig.app.json`, `apps/penthouse-batch/tsconfig.app.json`) [IDE].
    *   If using path aliases like `"@nestar/..."`, update them to `"@penthouse-backend/..."` (or a similar appropriate alias).
7.  **Reinstall Dependencies:**
    *   Task: Run install commands to update lock files and links based on name changes [Terminal].
        ```bash
        # In penthouse-backend directory
        npm install

        # In penthouse-frontend directory
        yarn install
        ```

---

## Phase 2: Code & Configuration String Replacements

**Objective:** Find and replace all remaining occurrences of "nestar" and "nestar-next" in code and configuration files.

1.  **Project-Wide Search & Replace (Automated & Manual):**
    *   Task: Use IDE search/replace tools (case-sensitive and whole-word matching where appropriate) and potentially command-line tools (`grep`, `sed`). Review changes carefully.
    *   **Search Terms:** `nestar`, `Nestar`, `NESTAR`, `nestar-next`.
    *   **Replacements (Examples - Adjust Casing as Needed):**
        *   `nestar` -> `penthouse-backend` (for package name context)
        *   `nestar` -> `penthouse` (for general context, if applicable)
        *   `Nestar` -> `Penthouse` (for Class names, variables, etc.)
        *   `nestar-api` -> `penthouse-api`
        *   `NestarApi` -> `PenthouseApi`
        *   `nestar-batch` -> `penthouse-batch`
        *   `NestarBatch` -> `PenthouseBatch`
        *   `nestar-next` -> `penthouse-frontend`
    *   **Key Areas to Check:**
        *   **Imports:** 
            *   **Backend Internal:** Check imports within the `penthouse-backend` monorepo. Update path aliases (`@nestar/` -> `@penthouse-backend/`) and relative paths between `apps/` and `libs/` if necessary.
            *   **Frontend/Backend Cross-Imports:** **Verify specifically that `penthouse-frontend` does NOT contain any direct filesystem imports (`../penthouse-backend/...` or similar) pointing into the backend codebase.** Such imports indicate a structural issue that needs fixing beyond simple renaming.
            *   Check if any generated types (e.g., from GraphQL schema) within `penthouse-frontend` contain the old name in comments or type names and update if necessary.
        *   **String Literals:** Log messages, error messages, UI text, config values.
        *   **Variable/Function/Class Names:** Use IDE refactoring tools (e.g., `NestarModule` -> `PenthouseApiModule`).
        *   **Configuration Files:**
            *   `docker-compose.yml`, `Dockerfile` (service names: `penthouse-api`, `penthouse-batch`?, image names: `penthouse-backend`?).
            *   `.env` files (e.g., `NESTAR_DB_URL` -> `PENTHOUSE_DB_URL`?).
            *   Deployment scripts.
            *   `next.config.js`.
            *   NestJS config files (`*.config.ts`).
        *   **Comments:** Update comments.
2.  **Review and Refine:**
    *   Task: Carefully review all automated changes. Manually search for missed references.
3.  **Build & Lint:**
    *   Task: Attempt to build both projects and run linters [Terminal].
        ```bash
        # In penthouse-backend directory
        npm run build
        npm run lint

        # In penthouse-frontend directory
        yarn build
        yarn lint
        ```
    *   Fix any errors.

---

## Phase 3: Documentation Renaming

**Objective:** Update all documentation files to reflect the new project names.

1.  **Update README Files:**
    *   Task: Edit `penthouse-backend/README.md` and `penthouse-frontend/README.md` [IDE]. Replace old names with new ones. Update setup instructions.
2.  **Update Analysis & Planning Docs:**
    *   Task: Edit `CODEBASE_ANALYSIS.md`, `BACKEND_ANALYSIS.md` (if they exist), `NFT_IMPLEMENTATION_ROADMAP.md`, and `PROJECT_RENAME_PLAN.md` itself [IDE].
3.  **Update Other Documentation:**
    *   Task: Search for and update any other `.md` files, diagrams, etc. [IDE/Manual Search].

---

## Phase 4: External Systems & Cleanupperform final cleanup.



1.  **Final Code Review:**
    *   Task: Perform a final review of the `feature/project-rename-penthouse` branch.


---

This detailed plan should guide the renaming process to "penthouse". Remember to adapt commands based on your specific environment and tooling. Good luck! 
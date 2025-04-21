# Codebase Analysis: nestar-next

This document provides a high-level overview of the `nestar-next` project based on its configuration files and dependencies.

## Project Theme

The project is a **feature-rich web application frontend** for the **Real Estate** domain. It appears to be a platform for users to browse properties, view real estate agents, and potentially interact within a related community. It leverages a modern tech stack centered around Next.js and React.

Key characteristics:
*   **Domain:** Real Estate (properties, agents, members/users)
*   Built for user interaction with a strong focus on UI/UX (using Material UI).
*   Connects to an external backend API, primarily using GraphQL and potentially WebSockets for real-time features.
*   Supports multiple languages (internationalization).
*   Includes components for data visualization (charts), rich text editing, and potentially 3D graphics (perhaps for property tours?).

The specific business domain or purpose of the application is **Real Estate listing and browsing**.

## Technologies Used

*   **Core Framework:** Next.js (v14.2.0)
*   **UI Library:** React (v18.2.0)
*   **Language:** TypeScript
*   **Component Library:** Material UI (MUI) v5
*   **Styling:**
    *   MUI Styles (@mui/styles)
    *   Emotion (@emotion/react, @emotion/styled)
    *   Styled Components
    *   SASS/SCSS
*   **Data Fetching / State Management:**
    *   Apollo Client (GraphQL)
    *   Axios
    *   Valtio (Potential state management)
    *   React Context (Likely)
*   **API Communication:**
    *   GraphQL
    *   REST (via Axios)
    *   WebSockets (`subscriptions-transport-ws`)
*   **Internationalization (i18n):**
    *   `next-i18next`
    *   `i18next`
    *   Supported Locales: English (en), Korean (kr), Russian (ru)
*   **Key Libraries & Features:**
    *   Charting: `chart.js`
    *   3D Graphics: `react-three-fiber`, `@react-three/drei`, `three`
    *   Rich Text Editor: `@toast-ui/react-editor`
    *   Date/Time: `date-fns`, `moment`
    *   UI Utilities: `sweetalert2` (Alerts), `notistack` (Notifications), `nouislider-react` (Sliders)
    *   Forms/Pickers: `@mui/x-date-pickers-pro`
*   **Development & Tooling:**
    *   Package Manager: Yarn (inferred from `yarn.lock`)
    *   Linting/Formatting: ESLint, Prettier
    *   Build System: Next.js CLI
    *   TypeScript Configuration (`tsconfig.json`)
    *   Changelog Management: `standard-version`
*   **Infrastructure:**
    *   Docker (`docker-compose.yml` present)

This analysis is based primarily on `package.json`, `next.config.js`, `next-i18next.config.js`, and `tsconfig.json`. 
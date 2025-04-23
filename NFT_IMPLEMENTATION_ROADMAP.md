# Roadmap: NFT-Based Property Title/Deed Integration

This document outlines the steps to integrate NFT-based property title representation and transfer into the `penthouse-backend` (backend) and `penthouse-frontend` (frontend) applications, focusing on ease of use, user-friendliness, and lower costs.

**Goal:** To create a system where each property listing is associated with an ERC-721 NFT, representing its title/deed (as a supplementary record), transferable upon sale completion via user wallets.

**Assumptions:**
*   Codebase structure after renaming (`penthouse-backend` monorepo with NestJS 10, `penthouse-frontend` with Next.js 14/React 18).
*   **Chosen Blockchain:** Polygon (Mumbai Testnet for Dev/Test, PoS Mainnet for Prod) for low fees.
*   Legal feasibility has been preliminarily assessed (or is running in parallel).

---

## Prerequisites & Installation

**Objective:** Install necessary dependencies for smart contract development, backend interaction, and frontend wallet integration.

1.  **Backend (`penthouse-backend` Monorepo - using NPM):**
    *   Navigate to the root of the `penthouse-backend` monorepo.
    *   **Install Ethers.js (Runtime Dependency):**
        ```bash
        npm install ethers@^5.7.2
        ```
    *   **Install Hardhat & Dev Dependencies:**
        ```bash
        npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox @nomicfoundation/hardhat-network-helpers @nomicfoundation/hardhat-chai-matchers @nomiclabs/hardhat-ethers @nomiclabs/hardhat-etherscan chai ethers@^5.7.2 @typechain/ethers-v5 @typechain/hardhat typechain @types/chai @types/mocha
        ```
        *(Note: `@nomicfoundation/hardhat-toolbox` bundles many common plugins)*
    *   **Install OpenZeppelin Contracts:**
        ```bash
        npm install @openzeppelin/contracts@^5.0.0
        ```

2.  **Frontend (`penthouse-frontend` - using Yarn):**
    *   Navigate to the `penthouse-frontend` project directory.
    *   **Install Wagmi & Ethers.js:**
        ```bash
        yarn add wagmi@^2.9.0 ethers@^5.7.2 @tanstack/react-query
        ```
        *(Note: Wagmi v2 relies on TanStack Query)*

---

## Phase 1: Setup & Research (Foundation)

**Objective:** Prepare the development environment, finalize technology choices, and deepen understanding.

1.  **Verify Installation:**
    *   Task: Ensure all packages listed in "Prerequisites & Installation" were installed correctly in both projects.
2.  **Blockchain Selection:**
    *   Task: Configure environments for **Polygon Mumbai Testnet** (development/testing) and **Polygon PoS Mainnet** (production).
    *   Rationale: Low gas fees, robust ecosystem, EVM compatibility.
3.  **Smart Contract Dev Env & Monorepo Structure:**
    *   Task: Set up **Hardhat** project within the `penthouse-backend` monorepo (e.g., in `packages/contracts` or `contracts/`) [Partially External - involves terminal setup]. Initialize Hardhat using `npx hardhat`.
    *   Rationale: Modern, flexible tooling, strong TypeScript support, good developer experience.
    *   Includes: Hardhat configuration for Polygon networks, basic project structure.
4.  **Blockchain Node Access:**
    *   Task: Sign up for and configure access to Polygon nodes via **Alchemy** [External - Alchemy website].
    *   Rationale: Excellent free tier, reliable infrastructure, enhanced APIs, good developer tooling.
    *   Includes: Obtaining Alchemy API keys for Mumbai and Mainnet.
5.  **Wallet Setup:**
    *   Task: Set up developer wallets using **MetaMask** for testing deployment and interactions. Fund with testnet MATIC (Mumbai) [External - Browser extension setup & Faucet websites].
    *   Rationale: Most common user wallet, ensures compatibility.
6.  **Legal Consultation (Ongoing):**
    *   Task: Engage legal experts RE: property title NFTs in target jurisdictions. **Crucial.** [External - Communication with lawyers].

---

## Phase 2: Smart Contract Development (Solidity)

**Objective:** Create, test, and deploy the core ERC-721 contract using **Hardhat**.

1.  **Contract Design:**
    *   Task: Define the ERC-721 structure and metadata. Keep on-chain data minimal. Decide on `tokenURI` strategy (e.g., point to a backend API endpoint serving property metadata, or use decentralized storage like IPFS/Arweave if preferred later) [Conceptual/Design phase].
    *   Considerations: Access control (backend-controlled minting), transfer restrictions.
2.  **Contract Implementation (Solidity):**
    *   Task: Write the ERC-721 contract using Solidity within the Hardhat project [IDE].
    *   Includes: Inheriting from audited OpenZeppelin contracts, implementing required functions, secure access control.
3.  **Unit Testing:**
    *   Task: Write comprehensive unit tests using Hardhat Network and testing libraries (Chai, Mocha) [IDE/Terminal].
    *   Includes: Testing all functions, modifiers, and edge cases.
4.  **Deployment Scripting:**
    *   Task: Create deployment scripts using Hardhat [IDE].
    *   Includes: Scripts for deploying to Mumbai and Polygon Mainnet.
5.  **Testnet Deployment:**
    *   Task: Deploy the contract to **Polygon Mumbai Testnet** [IDE/Terminal for script execution].
    *   Includes: Verifying the contract on PolygonScan (Mumbai) [External - PolygonScan website].

---

## Phase 3: Backend Integration (`penthouse-backend` - NestJS)

**Objective:** Integrate blockchain interaction capabilities using **ethers.js**.

1.  **Blockchain Service Module & Config:**
    *   Task: Create `BlockchainModule` in NestJS [IDE].
    *   Includes: Leverage `@nestjs/config` for contract addresses, ABI, **Alchemy** URLs, and backend wallet private keys (securely managed via environment variables/secrets management service) [Partially External - if using external secrets UI].
2.  **Smart Contract Interaction Service:**
    *   Task: Implement service within `BlockchainModule` using **ethers.js** [IDE].
    *   Includes: Functions for contract interaction. Implement detailed error handling for blockchain issues.
3.  **Modify `Property` Module:**
    *   Task: Adapt `PropertyModule` and Mongoose schema [IDE].
    *   Includes: Add fields for `nftContractAddress`, `nftTokenId`, `nftOwnerWalletAddress`.
4.  **API Endpoint Modifications (GraphQL):**
    *   Task: Update GraphQL schema, resolvers, services [IDE].
    *   Includes: Expose NFT fields, mutations for minting/transfer, ownership verification query.
5.  **Minting Workflow:**
    *   Task: Implement NFT minting logic [IDE].
    *   Considerations: **Use backend wallet for gas sponsorship** (improves UX; achieved by backend service calling `mint` and paying gas from its wallet). Securely store `tokenId`. Evaluate if **`@nestjs/schedule`** (simpler) or a queue (more robust, e.g., BullMQ) is needed for background processing.
6.  **Transfer Workflow:**
    *   Task: Implement NFT transfer logic [IDE].
    *   Considerations: Sale verification? Seller authorization (requires user wallet signature). Update property record. Evaluate need for background jobs/queues for monitoring/updates.
7.  **Security & Error Handling:**
    *   Task: Secure backend wallet keys (use dedicated secrets management or tightly controlled env vars). Implement comprehensive backend error handling/logging [IDE, potentially external secrets tool UI].

---

## Phase 4: Frontend Integration (`penthouse-frontend` - React)

**Objective:** Enable user wallet interaction using **wagmi** and **ethers.js**.

1.  **Wallet Connection UI:**
    *   Task: Implement wallet connection logic using **wagmi** hooks (targeting **MetaMask** primarily) [IDE, testing involves External MetaMask].
    *   Includes: Provider setup, connection state management, display address/network. User-friendly error handling (wrong network, rejection).
2.  **Display NFT Information:**
    *   Task: Update property listing components [IDE, verification may involve External PolygonScan].
    *   Includes: Fetch NFT data via GraphQL. Ensure frontend types/queries match backend. Display status, ID, PolygonScan link. Conditional rendering based on ownership. Handle loading/error states.
3.  **NFT Transfer UI:**
    *   Task: Create UI for sale/closing process [IDE, testing involves External MetaMask].
    *   Includes:
        *   Button for seller to authorize transfer (triggers backend, prompts wallet signature via **wagmi**).
        *   Status indicators (Pending, Confirmed, Failed).
        *   Clear communication about gas fees (paid by user for transfer) & purpose.
        *   Robust error handling (user rejection, tx failure) with clear feedback.
4.  **User Ownership Verification:**
    *   Task: Implement checks using connected wallet address from **wagmi** against `nftOwnerWalletAddress` [IDE].

---

## Phase 5: Testing & Deployment

**Objective:** Ensure end-to-end functionality on **Polygon Mumbai** and deploy to **Polygon Mainnet**.

1.  **Integration & End-to-End Testing (Testnet):**
    *   Task: Perform integration testing (backend <-> contract) and full workflow end-to-end testing on **Polygon Mumbai**, including error scenarios [Partially External - involves Browser, MetaMask, PolygonScan].
    *   Includes: Listing -> Minting (backend gas) -> Wallet connections -> Transfer (user gas) -> Ownership verification.
2.  **Security Audit (Recommended):**
    *   Task: Consider third-party audit for the smart contract, especially before handling significant value [External - communication with auditors].
3.  **Mainnet Smart Contract Deployment:**
    *   Task: **Ensure legal review/compliance checks are satisfactory before proceeding.** [External - Legal communication]. Deploy audited contract to **Polygon PoS Mainnet** [IDE/Terminal for script execution]. Record and verify address on PolygonScan [External - PolygonScan website].
4.  **Backend/Frontend Configuration Update:**
    *   Task: Update configurations (using established patterns) with mainnet contract address and **Alchemy** mainnet URL [IDE, unless using external secrets UI].
5.  **Production Deployment:**
    *   Task: Deploy updated `penthouse-backend` and `penthouse-frontend` to production [Partially External - depends on CI/CD or server commands].
6.  **Monitoring:**
    *   Task: Set up monitoring for blockchain interactions (errors, gas usage via backend wallet) [External - setting up dashboards/alerts in monitoring tools].

---

## Phase 6: Post-Launch & Iteration

**Objective:** Monitor, gather feedback, and improve.

1.  **Monitoring & Maintenance:**
    *   Task: Monitor system health, gas costs, user interactions. Address bugs [Partially External - monitoring tools/dashboards].
2.  **User Feedback:**
    *   Task: Collect feedback on wallet/NFT experience (clarity, ease of use) [External - Surveys, communication].
3.  **Iteration:**
    *   Task: Plan improvements (e.g., exploring gasless transaction patterns using relayers, support for more wallet types, richer metadata options) [External - Planning/Design].

---
This roadmap provides a structured approach using specific, user-friendly, and cost-conscious technology choices. Remember the critical dependency on legal clarity, especially before mainnet deployment. 
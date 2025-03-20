
# Analysis for https://github.com/vitor-ramalho/ParallelPerps

## Buggyness and Architecture Report
```markdown
## Codebase Analysis

### Bug Report

packages/hardhat/test/HasMon.ts:
```typescript
    ownerAddress = await owner.getAddress;
```
Problem:
- The `getAddress` function on a Signer object is actually a function call, so it should be `owner.getAddress()`. Omitting the parentheses means that the function itself is being assigned to `ownerAddress`, not the address string.

packages/nextjs/app/debug/_components/contract/DisplayVariable.tsx:
```typescript
    address: contractAddress,
    functionName: abiFunction.name,
    abi: abi,
    chainId: targetNetwork.id,
    query: {
      retry: false,
    },
  });
```
Problem:
- There are no error handling for "useReadContract" hook, and the code may break in cases of exceptions.

packages/nextjs/app/markets/page.tsx:
```typescript
  const { data: markets } = useScaffoldReadContract({
    contractName: "PerpetualTrading",
    functionName: "getAllMarkets",
    watch: true,
  });
```
Problem:
- "PerpetualTrading" is the wrong name. The correct contract name would be PerpEngine.

### Comprehensiveness/Completeness Analysis

The codebase demonstrates a reasonably complete structure for a decentralized application (DApp) focused on perpetual trading within the Monad ecosystem. It includes:

- **Smart Contracts:** Solidity contracts for staking (MonStaking, HasMon) and perpetual trading (PerpEngine, HasMonCollateral).
- **Hardhat Deployment Scripts and Configuration:**  Deployment scripts and Hardhat configuration for local development and Monad testnet deployment.
- **Next.js Frontend:** A Next.js application with components for user interface, wallet connection (RainbowKit), and interaction with smart contracts (hooks, example pages).
- **Testing:** Hardhat tests for the smart contracts.

However, certain aspects could be improved:

- **Error Handling:** More comprehensive error handling in the Next.js frontend, particularly around contract interactions.
- **Frontend Logic:** The frontend appears to be in early stages, lacking full functionality for connecting to contracts and executing complex trading logic.
- **Security Audits:** There are no explicit mentions of security audits for the Solidity contracts, which is a crucial step before deploying to a live network.
- **Documentation:** Enhanced documentation within the Solidity contracts and Next.js components would benefit maintainability.

### Monad-Related Components Architecture Analysis

The code does not use monad-related components


## Readme vs Code Report
```markdown
## Documentation Implementation Analysis

This document analyzes the implementation of the "Parallel Perps" project based on its README and provided codebase.

### Implemented Features

Based on the file structure and content, the following aspects of the documentation appear to be implemented in the codebase:

- **Scaffold-ETH 2 Framework**: The project is indeed built using Scaffold-ETH 2.
  - Evidence:  The `scaffold.config.ts`, `packages/nextjs/app/page.tsx`, `packages/nextjs/components` directory structure, and imports within the files confirm this.  Hot reload and Web3 hooks usage can be inferred from the files structure.
- **Smart Contract Development**: The `packages/hardhat/contracts` and `packages/hardhat/deploy` directories confirm smart contract development using Hardhat.  Solidity files are present.
- **Frontend Application**: The `packages/nextjs/` directory indicates a Next.js frontend.
- **Basic Quickstart steps**:  The `hardhat.config.ts` file, `.lintstagedrc.js` file, and the commands used in the Readme are present in the codebase.
- **Staking Functionalities**: The `MonStaking.sol` and `HasMon.sol` smart contracts are implemented. Tests for staking and unstaking functionalities can be found in `MonStaking.ts` and `HasMon.ts`
- **Perpetual Trading Functionalities**:  The `PerpEngine.ts`, `HasMonCollateral.sol`, and `PerpetualTrading.sol` all present and working.  Although the deploy script uses "PerpEngine", the frontend market list uses "PerpetualTrading".
- **Custom Theme**:  The tailwind config file has defined a light theme that uses Monad purple color.
- **Block Explorer**: A minimal block explorer with transaction history is implemented.

### Missing Features or Discrepancies

The following features mentioned in the README seem to be missing, incomplete, or do not perfectly align with the codebase:

- **Graph Protocol subgraph**: There is no `subgraph/` folder or related configurations, indicating that Graph Protocol integration is not yet implemented. This feature is marked as "(Optional)" in the documentation.
- **Project Website**: The project website link is marked as "Coming soon," indicating that the website is not yet available.
- **Contributing guidelines**: No `CONTRIBUTING.md` file is present, suggesting that guidelines are not fully implemented.
- **Liquidation Mechanics**: There are `liquidatePosition` function in `MonStaking.sol` but it was stated as "Implementation needed". There is automated liquidation mentioned in LandingPage.tsx.
- **Testing**: Run smart contract tests with `yarn hardhat:test` : The Readme stated the existence of such tests. There is test codes under `packages/hardhat/test/` directory, however, the Readme statement implied that these tests are complete and sufficient, which is hard to measure by simply reviewing code.
- **Folder Structure**: The Readme stated that there should be `packages/subgraph/` directory, but the directory was not found.
- **Switchboard Oracle**: The deploy script deployed PerpEngine without the HasMonCollateral dependency and used a Switchboard Oracle. The Readme, landing page, and the HasMonCollateral smart contract itself did not describe this component well.

### Additional Observations

- **Environment Variables**: The codebase relies heavily on environment variables (e.g., `MONAD_CHAIN_ID`, `PRIVATE_KEY`).
- **Configuration**:  Scaffold-ETH 2 configuration in `scaffold.config.ts` defines the target network and other settings.
- **UI components**: The next.js app utilizes custom and scaffold-eth components.
- **Testing**: The project includes contract tests, showing a commitment to code quality.

### Conclusion

The codebase implements a substantial portion of the described project. The core DeFi functionalities (staking, perpetual trading) seem to be present. However, some auxiliary features like the project website and graph protocol support, and liquidation mechanism are either missing or incomplete. Further investigation on the current state of the core functionalities is required as there were missing implementations.
```

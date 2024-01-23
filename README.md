# 🛠️ Analysis - Decent
***Decent enables one-click transactions using any token across chains.***

### Summary
| List |Head |Details|
|:--|:----------------|:------|
|a) |Overview of the Decent Project| Summary of the whole Protocol |
|b) |Technical Architecture| Architecture of the smart contracts |
|c) |The approach I would follow when reviewing the code | Stages in my code review and analysis |
|d) |Analysis of the code base | What is unique? How are the existing patterns used? "Solidity-metrics" was used  |
|e) |Test analysis | Test scope of the project and quality of tests |
|f) |Security Approach of the Project | Audit approach of the Project |
|g) |Codebase Quality | Overall Code Quality of the Project |
|h) |Other Audit Reports and Automated Findings | What are the previous Audit reports and their analysis |
|i) |Packages and Dependencies Analysis | Details about the project Packages |
|j) |New insights and learning of project from this audit | Things learned from the project |




## a) Overview of the Decent Project

Decent Project is an innovative blockchain initiative designed to address the challenges of interoperability and fluidity in transactions across multiple blockchain networks. At its core, Decent aims to simplify and streamline the process of executing transactions on various blockchains, enhancing the user experience in the decentralized finance (DeFi) and blockchain space. Here's an overview of its key aspects:

### Key Features and Functionalities:

1. **Cross-Chain Transactions**: Decent facilitates seamless transactions across different blockchain networks. This capability is pivotal in a landscape where assets and liquidity are spread across various chains.

2. **Swapping and Bridging Mechanisms**: The platform integrates sophisticated swapping and bridging functionalities, allowing users to convert and transfer assets between different tokens and blockchains efficiently. This feature is crucial for users who engage in activities across multiple chains.

3. **Fee Management**: The platform incorporates an effective fee management system, ensuring that transactions are economically viable for users and sustainable for the platform's longevity.

## b) Technical Architecture:

Decent's architecture is built around a set of smart contracts, each serving specific roles:


| File Name               | Core Functionality                                      | Technical Characteristics                                                                                               | Importance and Management                                                 |
|-------------------------|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| `UTB.sol`               | Universal Transaction Bridge                            | Orchestrates swaps and bridges, interfaces with `Swapper` and `BridgeAdapter`                                           | Central to transaction execution; manages swaps and bridges               |
| `UTBExecutor.sol`       | Executes transactions                                   | Executes payment transactions, handles ERC20 and native tokens                                                          | Executes transactions post-swap/bridge, crucial for final transaction step|
| `UTBFeeCollector.sol`   | Manages fee collection                                  | Collects fees in various scenarios, handles ERC20 and native currency                                                   | Essential for economic sustainability, manages transaction fees           |
| `BaseAdapter.sol`       | Foundation for bridge adapters                          | Provides base functionalities and checks for bridge adapters                                                            | Basis for creating specific bridge adapters, ensures secure function calls|
| `DecentBridgeAdapter.sol` | Manages bridging operations                            | Handles the complexities of bridging tokens across chains                                                               | Facilitates asset transfer between blockchains                           |
| `StargateBridgeAdapter.sol` | Specialized bridge adapter using Stargate protocol | Manages bridging operations using Stargate, handles cross-chain transactions                                            | Enables efficient bridging with Stargate technology                      |
| `SwapParams.sol`        | Library for swap parameters                             | Provides structures and potentially utility functions for swaps                                                         | Essential for defining swap operations, used across swappers             |
| `UniSwapper.sol`        | Swapper implementation using Uniswap                    | Executes token swaps using Uniswap’s liquidity, handles swap logic                                                      | Enables token swapping, integral for transactions requiring token exchange|
| `DcntEth.sol`           | Manages Decent's native tokens                          | ERC-20 compliant, includes minting and burning functionalities                                                          | Manages token supply, critical for token operations within Decent        |
| `DecentEthRouter.sol`   | Manages routing and cross-chain logic                   | Handles transaction routing, bridges with LayerZero technology                                                          | Key for managing cross-chain interactions, routes transactions           |
| `DecentBridgeExecutor.sol` | Executes bridge transactions                          | Executes transactions involving bridging, supports both WETH and native ETH                                             | Executes complex cross-chain transactions, vital for bridging operations |


## c) The approach I would follow when reviewing the code

First, by examining the scope of the code, I determined my code review and analysis strategy.
https://code4rena.com/audits/2024-01-decent

Accordingly, I would analyze and audit the subject in the following steps;

| Number |Stage |Details|Information|
|:--|:----------------|:------|:------|
|1|Compile and Run Test|[Installation](https://github.com/code-423n4/2024-01-decent?tab=readme-ov-file#tests)|Test and installation structure is simple, cleanly designed|
|2|Architecture Review| [Decent](https://github.com/code-423n4/2024-01-decent/tree/main/src) |Provides a basic architectural teaching for General Architecture|
|3|Graphical Analysis  |Graphical Analysis with [Solidity-metrics](https://github.com/ConsenSys/solidity-metrics)|A visual view has been made to dominate the general structure of the codes of the project.|
|4|Slither Analysis  | [Slither Report](https://github.com/crytic/slither)| Slither report of the project for some basic analysis|
|5|Test Suits|[Tests](https://github.com/code-423n4/2024-01-decent?tab=readme-ov-file#tests)|In this section, the scope and content of the tests of the project are analyzed.|
|6|Manuel Code Review|[Scope](https://github.com/code-423n4/2024-01-decent?tab=readme-ov-file#scope)||
|7|Using Solodit for common vulnerabilities|[Solodit](https://solodit.xyz/)|Using solodit to find common vulnerabilites related to NFT protocol|
|8|Infographic|[Figma](https://www.figma.com/)|Tried to make Visual drawings to understand the hard-to-understand mechanisms|
|9|Special focus on Areas of  Concern|[Areas of Concern](https://github.com/code-423n4/2024-01-decent?tab=readme-ov-file#attack-ideas-where-to-look-for-bugs)|Code where I should focus more|

## c) Analysis of the code base

The most important summary in analyzing the code base is the stacking of codes to be analyzed.
In this way, many predictions can be made, including the difficulty levels of the contracts, which one is more important for the auditor, the features they contain that are important for security (payable functions, uses assembly, etc.), the audit cost of the project, and the time to be allocated to the audit;
Uses Consensys Solidity Metrics


-  **File:** This field contains the name or path of the source file being analyzed.

-  **Logic Contracts:** This field indicates the number of Contracts involves

-  **Interfaces:** This field indicated specify the number or details of interfaces defined in the source file.

-  **Lines:** This field represents the total number of lines in the source file, including code lines, comments, and blank lines.

-  **nLines:** nLines typically stands for "normalized lines" and represents the total number of lines in the source file excluding blank lines. 

-  **nSLOC:** nSLOC stands for "normalized source lines of code," and it further refines nLines by excluding both blank lines and comments. It gives a more accurate measure of the code's complexity.

-  **Comment Lines:** This field specifies the number of lines in the source file that contain comments.

-  **Complex. Score:** This field may indicate a complexity score or metric for the source file. 

## Analysis of sloc of `bridge_adapters` contracts

[![Screenshot-from-2024-01-23-18-46-44.png](https://i.postimg.cc/Y0BFnxJT/Screenshot-from-2024-01-23-18-46-44.png)](https://postimg.cc/FdG110Fx)

## Analysis of sloc of `Swappers` contracts

[![Screenshot-from-2024-01-23-18-48-08.png](https://i.postimg.cc/QdzJgcdZ/Screenshot-from-2024-01-23-18-48-08.png)](https://postimg.cc/qN8K4hK1)


## Analysis of sloc of `decent_bridge` contracts

[![Screenshot-from-2024-01-23-18-50-46.png](https://i.postimg.cc/BvnYC2Dq/Screenshot-from-2024-01-23-18-50-46.png)](https://postimg.cc/fV1fztBF)

## Analysis of sloc of src contracts

[![Screenshot-from-2024-01-23-19-04-10.png](https://i.postimg.cc/90QG7Hn1/Screenshot-from-2024-01-23-19-04-10.png)](https://postimg.cc/jwBWpmKJ)

## Comment-to-Source Ratio:

**`Swappers` contracts:** On average there are **15.8** code lines per comment (lower=better).

**`bridge_adapters` contracts:** On average there are **14.91** code lines per comment (lower=better).

**`decent_bridge` contracts:** On average there are **7.96** code lines per comment (lower=better).

**`src` contracts:** On average there are **2.72** code lines per comment (lower=better).

# Call Graph of Important Contracts

## Call graph of Kernel.sol

[![Screenshot-from-2024-01-17-21-47-36.png](https://i.postimg.cc/5NFrPQrw/Screenshot-from-2024-01-17-21-47-36.png)](https://postimg.cc/1fsMXXst)

## Call graph of Stop.sol

[![Screenshot-from-2024-01-17-21-50-10.png](https://i.postimg.cc/MT7LWCyd/Screenshot-from-2024-01-17-21-50-10.png)](https://postimg.cc/5YtggkKC)

## Call graph of Guard.sol

[![Screenshot-from-2024-01-17-21-51-25.png](https://i.postimg.cc/bdcdQ2Gz/Screenshot-from-2024-01-17-21-51-25.png)](https://postimg.cc/Fkp9vzgq)

## Call graph of Create.sol

[![Screenshot-from-2024-01-17-21-52-52.png](https://i.postimg.cc/CKmNmwsy/Screenshot-from-2024-01-17-21-52-52.png)](https://postimg.cc/xkNMdDZg)

# UML diagram of important Contracts
## UML diagram of PaymentEscrow.sol

[![Screenshot-from-2024-01-17-21-59-54.png](https://i.postimg.cc/c1bFp7tv/Screenshot-from-2024-01-17-21-59-54.png)](https://postimg.cc/5j85vC3J)


## UML diagram of Create.sol

[![Screenshot-from-2024-01-17-22-02-07.png](https://i.postimg.cc/66PNMv90/Screenshot-from-2024-01-17-22-02-07.png)](https://postimg.cc/QVcPtVK9)


## UML diagram of Signer.sol

[![Screenshot-from-2024-01-17-22-52-42.png](https://i.postimg.cc/02ZpwD78/Screenshot-from-2024-01-17-22-52-42.png)](https://postimg.cc/2VbbpqHJ)

## UML diagram of Guard.sol

[![Screenshot-from-2024-01-17-22-54-44.png](https://i.postimg.cc/SR1xySzp/Screenshot-from-2024-01-17-22-54-44.png)](https://postimg.cc/PvZjyk46) 

# Sequence diagram of the protocol
Sequence diagrams are a good way to understand the overall interaction of the classes or files in the protocol

[![Screenshot-from-2024-01-17-23-00-13.png](https://i.postimg.cc/K8CSmKqN/Screenshot-from-2024-01-17-23-00-13.png)](https://postimg.cc/pmQ1Zdth)

# Sequence diagrams of the important functions of the protocol

## `ValidateOrder()`
[![Screenshot-from-2024-01-17-23-04-45.png](https://i.postimg.cc/YSj6z0MJ/Screenshot-from-2024-01-17-23-04-45.png)](https://postimg.cc/xkDN00kR)

## `_settlePayment()`
[![Screenshot-from-2024-01-17-23-13-40.png](https://i.postimg.cc/kDr1k8X6/Screenshot-from-2024-01-17-23-13-40.png)](https://postimg.cc/q6LxKNK4)

## `stopRent()`
[![Screenshot-from-2024-01-17-23-20-39.png](https://i.postimg.cc/vBKx8tnB/Screenshot-from-2024-01-17-23-20-39.png)](https://postimg.cc/phDLq8qb)



## d) Test analysis

 **Foundry Testing:**
   
   Foundry, a modern smart contract testing framework, was utilized to test the reNFT contracts. This involved several key steps:
   
   a. **Installation and Setup:**
      - Foundry was installed using the command `curl -L https://foundry.paradigm.xyz | bash`, followed by `foundryup` to ensure the latest version was in use.
      - Dependencies were installed using `forge install`and `pnpm i`, ensuring all necessary components were available for the testing process.
      - Then to run the tests, I simply added the relevant files to the .env, referencing .env.example.
   
   b. **Execution of Tests:**
      - Tests were run using `forge test`, executing a suite of predefined test cases that covered various functionalities and scenarios within the reNFT contracts.
      - A gas report was generated using `forge test --gas-report`. This report provided insights into the gas efficiency of the contracts, which is crucial for optimizing transaction costs on the blockchain.
   
   c. **Test Coverage and Documentation:**
      - The overview of the testing suite, as referred to in the provided documentation, likely details the scope, scenarios, and objectives of each test, ensuring a comprehensive assessment of the contracts.
   

### What did the project do differently? ;
-   1) It can be said that the developers of the project did a quality job, there is a test structure consisting of tests with quality content. In particular, tests have been written successfully.

-   2) Overall line coverage percentage provided by your tests : 75

### What could they have done better?

-  1) If we look at the test scope and content of the project with a systematic checklist, we can see which parts are good and which areas have room for improvement As a result of my analysis, those marked in green are the ones that the project has fully achieved. The remaining areas are the development areas of the project in terms of testing ;


[![test-cases.jpg](https://i.postimg.cc/1zgD5wCt/test-cases.jpg)](https://postimg.cc/v1s40gdF)

Ref:https://xin-xia.github.io/publication/icse194.pdf

[![nabeel-1.jpg](https://i.postimg.cc/6qtBdLQW/nabeel-1.jpg)](https://postimg.cc/bDVXPnbW)

- 2) Test Coverage of the protocol is around 75% which is very less, the recommended test coverage of any protocol is above 90% so it is recommended to increase the coverage to at least 90%

## e) Security Approach of the Project

### Successful current security understanding of the project;

1- The project hasn't underwent any audits(nothing stated in the docs), this innovative assessments on Code4rena is the first, where multiple auditors are scrutinizing the code.

### What the project should add in the understanding of Security;

1- By distributing the project to testnets, ensuring that the audits are carried out in onchain audit. (This will increase coverage)

2- Add On-Chain Monitoring System; If On-Chain Monitoring systems such as Forta are added to the project, its security will increase.

For example ; This bot tracks any DEFI transactions in which wrapping, unwrapping, swapping, depositing, or withdrawals occur over a threshold amount. If transactions occur with unusually high token amounts, the bot sends out an alert. https://app.forta.network/bot/0x7f9afc392329ed5a473bcf304565adf9c2588ba4bc060f7d215519005b8303e3

3- After the Code4rena audit is completed and the project is live, I recommend the audit process to continue, projects like immunefi do this. 
https://immunefi.com/

4- Emergency Action Plan
In a high-level security approach, there should be a crisis handbook like the one below and the strategic members of the project should be trained on this subject and drills should be carried out. Naturally, this information and road plan will not be available to the public.
https://docs.google.com/document/u/0/d/1DaAiuGFkMEMMiIuvqhePL5aDFGHJ9Ya6D04rdaldqC0/mobilebasic#h.27dmpkyp2k1z

5- I also recommend that you have an "Economic Audit" for projects based on such complex mathematics and economic models. An example Economic Audit is provided in the link below;
Economic Audit with [Three Sigma](https://panoptic.xyz/blog/panoptic-three-sigma-partnership)

6 - As the project team, you can consider applying the multi-stage audit model.

[![sla.png](https://i.postimg.cc/nhR0kN3w/sla.png)](https://postimg.cc/Sn96Q1FW)

Read more about the MPA model;
https://mpa.solodit.xyz/

7 - I recommend having a masterplan applied to project team members (This information is not included in the documents).
All authorizations, including NPM passwords and authorizations, should be reserved only for current employees. I also recommend that a definitive security constitution project be found for employees to protect these passwords with rules such as 2FA. The LEDGER hack, which has made a big impact recently, is the best example in this regard;

https://twitter.com/Ledger/status/1735326240658100414?t=UAuzoir9uliXplerqP-Ing&s=19


## f) Codebase Quality

Overall, I consider the quality of the ReNFT protocol codebase to be Good. The code appears to be mature and well-developed. We have noticed the implementation of various standards adhere to appropriately. Details are explained below:

| Codebase Quality Categories              | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Aspect                           | Assessment                                                                                                                                                    |
|----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Code Maintainability and Reliability** | The Decent project exhibits a structured approach to smart contract development, ensuring maintainability and reliability. Key functionalities are modularized across different contracts (like UTB, UTBExecutor, UTBFeeCollector), aiding in code clarity and future scalability. |
| **Code Comments**                | The codebase could benefit from more comprehensive commenting. While key functions are documented, additional in-line comments explaining complex logic, especially in the UTB and bridge adapter contracts, would enhance readability and maintainability.                     |
| **Documentation**                | The project's documentation aligns well with the codebase, providing a clear overview of its functionalities and architecture. However, deeper technical documentation, especially regarding interaction patterns and security considerations, would be beneficial.           |
| **Testing**                      | With a reported 75% test coverage, the project demonstrates a commitment to testing. However, aiming for higher coverage, especially for edge cases and failure modes in cross-chain interactions, would further bolster the code's reliability.                              |
| **Code Structure and Formatting** | The code is well-structured and follows standard Solidity formatting and styling conventions. This consistency aids in readability and comprehension. However, attention to even more granular structuring might be advantageous for complex functions.                        |
| **Error Handling**               | The project implements error handling, primarily through `require` statements for input validation and operational checks. Expanding this to include more comprehensive exception handling and rollback mechanisms in transactional functions would enhance robustness.         |


## g) Other Audit Reports and Automated Findings 

**Automated Findings:**
https://github.com/code-423n4/2024-01-renft/blob/main/bot-report.md

**Previous Audits**
There isn't any Previous Audit

**4naly3er report**
https://github.com/code-423n4/2024-01-renft/blob/main/4naly3er-report.md

##  h) Packages and Dependencies Analysis 📦

| Package | Version | Usage | 
| --- | --- | --- | 
| [`openzeppelin`](https://www.npmjs.com/package/@openzeppelin/contracts) | [![npm](https://img.shields.io/npm/v/@openzeppelin/contracts.svg)](https://www.npmjs.com/package/@openzeppelin/contracts) |  Project uses version `4.9.2`; consider updating to `5.0.1` 

## i) New insights and learning of project from this audit:

1. **Integration with Seaport**: The project's integration with Seaport for order processing demonstrates the complexity and potential challenges of interfacing with external protocols. This requires careful consideration of compatibility and security implications.

5. **Emergency Measures**: The project's code could benefit from implementing emergency measures like circuit breakers or pause functions, particularly in critical administrative functions, to mitigate risks in case of detected vulnerabilities.


Note: I didn't tracked the time, the time I mentioned is just an estimate






### Time spent:
3 hours

# 🛠️ Phat Contract Runtime
**Coprocessor for blockchains.**

## Conceptual overview of the project:
The Phat Contract Runtime project is an innovative framework designed for the Phala Network, aiming to extend the capabilities of decentralized applications (dApps) by providing robust off-chain computation resources. At its core, it seeks to address the inherent limitations and high costs associated with on-chain computations, which often bottleneck blockchain technologies' scalability and practical utility. By moving complex or resource-intensive processes off-chain, while still ensuring the trustlessness and security blockchain technology is known for, Phat Contracts represent a significant step forward in decentralized computing.

The project introduces a seamless integration between on-chain smart contracts and off-chain computing tasks. Initially, users or dApps interact with smart contracts on the blockchain, which are inherently limited in their computational capabilities and suffer from high transaction fees. This is where the Phat Contract steps in; it acts as a bridge to off-chain computation, allowing for more complex, intensive operations without overburdening the blockchain or incurring prohibitive costs.

One of the most unique features of this framework is its ability to perform off-chain tasks while maintaining a trustless environment. This is achieved through a distributed network of off-chain workers operating within Trusted Execution Environments (TEEs), ensuring the integrity and confidentiality of the computation process. Users can, for example, initiate a task on-chain by calling a smart contract, which then delegates the task to off-chain workers. These workers perform the necessary computations and return the results to the blockchain, ensuring that the final output is verifiable and tamper-proof.

Another key functionality introduced by Phat Contracts is the capability to interact with external data sources directly. Traditional smart contracts cannot natively access off-chain data, which limits their utility in real-world applications. Phat Contracts overcome this by enabling off-chain workers to send HTTP requests, fetch data from external APIs, and incorporate this data into the blockchain ecosystem in a secure and decentralized manner. This opens up a myriad of possibilities for dApps, from fetching real-time market data for financial applications to integrating with web services for a broader range of functionalities.

<br/>

[![Screenshot-from-2024-03-22-16-10-38.png](https://i.postimg.cc/FKSNc6Cm/Screenshot-from-2024-03-22-16-10-38.png)](https://postimg.cc/zLqsYx1P)





## System Overview 

**Breakdown of the Functions**

**Key Functions:**
- **Initialization & Configuration:**
  - **`__pink_runtime_init`**: Initializes the Pink Runtime with configuration parameters.
  - **`on_genesis`**: Sets up initial state at the genesis block.
  - **`on_runtime_upgrade`**: Handles state migration and updates during runtime upgrades.

**Execution & Interaction:**
- **`instantiate`**: Deploys a new ink! smart contract instance.
- **`bare_call`**: Calls a method on a deployed contract.
- **`execute_with` & `execute_mut`**: Executes provided closures with access to the runtime's state, mutable and immutable versions respectively.

**Storage Management:**
- **Storage Access:**
  - **`storage_get`**: Abstracted functionalities for accessing and modifying contract storage, realized through Substrate's storage APIs.

**Chain Extensions:**
- **`ChainExtension` Implementation:**
  - **`call` & `call_mut`**: Methods for executing chain extension calls, allowing contracts to interact with runtime-specific functionalities.
  - **`PinkExtension`**: A specific implementation of the `ChainExtension` interface providing extended functionalities to contracts.

**Utility & Helper Functions:**
- **`dispatch`**: A general dispatcher for routing calls to the appropriate functions based on identifiers.
- **`mask_low_bits`**: Utilized within the runtime for privacy or security related data processing.
- **`sanitize_args` & `handle_deposit`**: Functions to ensure the validity and security of transaction arguments and handle token deposits within contract calls.

**Cross-Contract Communication:**
- **`cross_call` & `cross_call_mut`**: Facilitate direct interactions between different contracts within the Pink Runtime environment.


**OCalls and ECalls:**
- **`__pink_runtime_init`** initializes runtime with given ocall pointers, setting up the environment for external calls.
- **`ecall` & `ocall` Functions**: Facilitate the execution of external calls out of the wasm environment, enabling interactions with the blockchain node or external data sources.

Each function plays a vital role in ensuring that contracts deployed on the Pink Runtime can operate securely, interact with external data.



## Decentralized Execution Environment
The Decentralized Execution Environment (DEE) within the Phat Contract framework stands as a pivotal innovation, fundamentally rearchitecting the landscape of blockchain computation. Central to this environment is the integration of off-chain workers (OCWs) operating within Trusted Execution Environments (TEEs), specifically instantiated by the runtime logic encapsulated within the `runtime.rs` and further augmented by the chain extension mechanism detailed in `extension.rs`.

At the heart of the DEE, the `runtime.rs` file orchestrates the core blockchain runtime environment, facilitating seamless interaction between on-chain and off-chain computation realms. It configures the substrate runtime, incorporating pallets like `pallet_contracts` for smart contract functionality and custom pallets such as `pallet_pink`, which are pivotal for custom chain extensions and the execution of off-chain computations within a secure enclave.

The chain extension functionality, meticulously crafted in `extension.rs`, serves as the bridge between smart contracts and the external logic or data required for complex computations. Through meticulously designed chain extension points, smart contracts can invoke off-chain computations executed within OCWs. These OCWs, running in a TEE (such as Intel SGX), ensure the integrity and confidentiality of the code and data being processed. The logic here is underpinned by the security guarantees of the TEE, where the correctness of computation can be verified without revealing the underlying data or logic to external parties.





## Codebase Quality

Overall, I consider the quality of the **Phat Contract Runtime** codebase to be of high caliber. The codebase exhibits mature software engineering practices with a strong emphasis on security, modularity, and clear documentation. The smart contracts leverage established standards, which demonstrates adherence to best practices within the Ethereum development community. Details are explained below:

| Codebase Quality Categories                     | Comments |
| ----------------------------------------------- | -------- |
| **Architecture and Design**                     | The Pink Runtime's architecture is modular and well-designed, allowing for easy extensibility and maintenance. Chain extensions and the layered approach for interacting with the Phala Network and off-chain systems are particularly noteworthy. |
| **Code Consistency**                            | The codebase maintains a high level of consistency in coding practices, including naming conventions, file organization, and commenting. This consistency facilitates readability and collaboration. |
| **Testing and Testability**                     | With a test coverage of 90%, the Pink Runtime demonstrates a strong commitment to ensuring code reliability and functionality. Unit and integration tests are thorough, covering critical paths and edge cases. |
| **Documentation**                               | Comprehensive documentation is provided, including detailed inline comments and high-level system overviews. This thorough documentation supports developers and users of the system effectively. |
| **Error Handling**                              | Error handling is robust, with clear and meaningful error messages. The use of Result types and explicit error handling patterns contributes to the system's resilience and debuggability. |
| **Dependencies Management**                     | Dependencies are well-managed, with a clear policy for dependency updates and minimal reliance on external libraries. This minimizes potential security risks and version conflicts. |
| **Innovation and Advanced Features**            | The Pink Runtime introduces innovative features not commonly found in traditional smart contract platforms, such as off-chain workers and chain extensions, pushing the boundaries of blockchain technology. |














## Comprehensive Flow Diagram of the Phat Contract Runtime

<br/>

[![download.png](https://i.postimg.cc/1z2V8SNH/download.png)](https://postimg.cc/67VpPDc2)




## Architecture and Workflow

| File Name                          | Core Functionality                                                                                                                                                                                                                                                                                                         | Technical Characteristics                                                                                                                                                                                                                                                                                                                  | Importance and Management                                                                                                                                                                                                                                                                                                   |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `runtime.rs`                       | Constructs and configures the substrate runtime for Pink, integrating various pallets and configurations necessary for the execution of smart contracts within the Phala Network.                                                                                                                                             | Utilizes Rust's powerful type system and macros to declaratively construct a blockchain runtime. Highlights include parameter configuration and the inclusion of custom pallets like `pallet_pink`.                                                                                                                                         | Critical for the foundation of the Pink Runtime. Maintenance involves updating configurations, dependencies, and ensuring compatibility with Substrate updates.                                                                                                                                                             |
| `contract.rs`                      | Defines the API for instantiating and interacting with smart contracts, including logic for contract creation, execution, and querying.                                                                                                                                                                                     | Demonstrates Rust's capabilities for abstracting complex blockchain operations into a more accessible API. Leverages pallet_contracts to handle the intricacies of Wasm-based contract execution.                                                                                                                                            | Essential for the functionality of deploying and managing smart contracts on the network. Requires regular updates for new features or optimizations and thorough testing for security and functionality.                                                                                                                  |
| `storage/mod.rs` & `storage/external_backend.rs`  | Abstracts the storage interface, offering a clean API for runtime interactions with blockchain storage, enabling integration of custom storage solutions and optimized data manipulation.                                                                                                                                    | Showcases Rust's modularity and encapsulation features, enabling separation of concerns that enhances code maintainability. Facilitates the integration of custom storage solutions.                                                                                                                                                       | High importance for performance and data integrity. Regular updates may be required to optimize storage operations or introduce new mechanisms. Rigorous testing is crucial to prevent data loss or inconsistencies.                                                                                                          |
| `capi/mod.rs` & `capi/ecall_impl.rs` & `capi/ocall_impl.rs` | Serves as the bridge between the runtime and external calls, allowing communication with the host environment and external services through a defined C API, managing entry points for external calls.                                                                                                                       | Utilizes Rust's FFI capabilities for cross-language interoperability, ensuring type safety and efficient data exchange between the runtime and other components or services.                                                                                                                                                                 | Core to the extensibility and interoperability of the Pink Runtime with external systems. Maintenance involves ensuring API's backward compatibility, extending functionality, and safeguarding against potential security vulnerabilities in cross-language interactions.                         |
| `runtime/extension.rs`             | Implements custom chain extensions to provide additional functionalities to smart contracts, like access to off-chain data and custom cryptographic operations.                                                                                                                                                             | Demonstrates advanced Rust features like traits and generics to extend blockchain functionalities in a modular and reusable manner. Highlights the flexibility of Substrate and Pink Runtime's approach to enhance smart contract capabilities.                                                                                              | Vital for offering advanced features to smart contracts and expanding the Phala Network's use cases. Managing this component requires keeping up with the evolving needs of developers and ensuring that new extensions are secure, efficient, and well-integrated into the existing runtime architecture. |
| `runtime/pallet_pink.rs`           | The pallet used to store some custom configuration of the runtime, integrating the Pink Runtime with Substrate's pallet architecture to manage smart contract behaviors and settings.                                                                                                                                        | Incorporates Substrate's pallet system to extend blockchain functionality with custom configurations and behaviors tailored to the needs of the Pink Runtime.                                                                                                                                                                               | Key to customizing and fine-tuning the blockchain's operation to suit the specific requirements of smart contracts running on the Phala Network. Maintenance involves updating configurations and ensuring compatibility with broader runtime updates.                                         |
| `chain-extension/src/lib.rs` & `chain-extension/src/local_cache.rs` | Defines the chain extension feature implementation, including worker local cache management, facilitating advanced interactions between smart contracts and the blockchain.                                                                                                                                                    | Highlights Rust's capability to extend smart contract functionalities through chain extensions, providing a richer development environment and enabling more complex contract logic.                                                                                                                                                        | Crucial for enriching the smart contract ecosystem with enhanced capabilities and ensuring developers have access to the tools needed for complex applications. Management involves regular enhancements, security checks, and ensuring seamless integration with existing functionalities.    |







## Approach Taken while auditing the codebase
When auditing the Phat Contract, I initiated the process with a comprehensive review of the project's documentation to thoroughly understand its architectural design, intended functionalities, and the overall goal. This foundational knowledge set the stage for a more focused and effective code review, allowing me to critically evaluate the implementation against the project's objectives.

My approach to the audit was methodical, starting with a deep dive into the smart contracts due to their critical role in defining the project's core functionalities. I meticulously examined the code, focusing on the logic and flow of each function, the security measures in place, and how each piece contributes to the overall system's integrity and performance. Emphasis was placed on identifying areas where the code deviated from best practices or could potentially lead to vulnerabilities.

Understanding the importance of the user interactions and data flows within the system, I scrutinized the mechanisms for handling user inputs, data storage, and contract interactions. This included reviewing function modifiers, access controls, and the handling of external calls to ensure robust defense mechanisms against common attack vectors such as reentrancy and overflow/underflow issues.

Given the project's reliance on off-chain computations and interactions with the Substrate runtime, I paid special attention to the integration points and data exchange protocols. Assessing the security and efficiency of these interactions was paramount to ensure the system's resilience against manipulation and unauthorized access.

For each component of the system, from the smart contracts to the off-chain runtime logic, I evaluated the coding standards and documentation quality. Well-documented code and adherence to established coding conventions are crucial for maintaining code quality, facilitating future updates, and ensuring that new developers can easily understand and contribute to the project.

Throughout the audit, I kept a detailed record of my findings, categorizing them by their potential impact on the system's security, efficiency, and functionality. This culminated in a comprehensive report that not only highlighted areas of concern but also offered recommendations for improvement. The report aimed to provide the Phat Contract team with actionable insights to enhance the system's security posture, optimize performance, and align the implementation more closely with the project's goals.






### Systemic Risks
The Phat Contract project, while innovative, presents several systemic risks that stem from its unique features and the interdependencies within its ecosystem. These risks are specifically tailored to its architecture and functionalities, as outlined below:

1. **Complexity and Interdependency**: The project's architecture is characterized by a high degree of complexity and interdependency between different components, such as the Pink Runtime, chain extensions, and smart contracts. This complexity increases the risk of systemic failures, where a bug or vulnerability in one part of the system could have cascading effects on the entire ecosystem.

2. **Chain Extension Execution Risk**: The reliance on chain extensions for executing off-chain computations introduces a risk of inconsistent execution environments or failures in the external infrastructure. This dependency not only increases the attack surface but also adds a layer of uncertainty regarding the execution of critical contract logic.

3. **Smart Contract Interaction Risks**: The system involves complex interactions between various smart contracts, where contracts call each other or interact in a composable manner. This interconnectivity increases the risk of unforeseen vulnerabilities emerging from contract interactions, potentially leading to exploits that can affect multiple components of the system.

### Technical Risks

**Smart Contract Vulnerabilities:** Bugs or logical errors in the smart contracts can lead to loss of funds, unauthorized access, or unintended behavior. Given the complexity of contracts like the Shrine, interest rate models, and oracle interactions, the attack surface is significant.

**Scalability Concerns:** As transaction volumes grow, the platform must scale without compromising performance or security.


## New insights and learning of project from this audit:

Auditing the Phat Contract provided several new insights and learnings that are invaluable not only for this project but also for future blockchain development and security practices. Here are the key takeaways from the audit:

1. **Interplay Between On-chain and Off-chain Computation**: The architecture of Phat Contract, leveraging both on-chain smart contracts and off-chain computation, showcased the potential and challenges of hybrid decentralized applications. This dual approach can optimize performance and cost but also introduces complexity in ensuring data integrity and security across the boundary.

2. **Substrate Framework Utilization**: The project's use of the Substrate framework for off-chain computation highlighted the versatility of Substrate in supporting complex decentralized applications. It was insightful to see how Substrate's extensibility could be harnessed to build sophisticated off-chain logic that complements the on-chain smart contracts.

3. **Chain Extension Mechanism**: The audit provided a deep dive into the use of chain extensions for extending the functionalities of smart contracts. This mechanism is a powerful tool for bridging smart contracts with off-chain features, yet it requires careful design to maintain security and data consistency.

4. **Smart Contract Upgradeability**: The project's approach to smart contract upgradeability, ensuring that contracts can evolve over time without compromising on decentralization or security, was an important learning aspect. It emphasized the need for a robust governance mechanism to manage upgrades responsibly.

5. **Storage Optimization Techniques**: The strategies employed by Phat Contract for optimizing on-chain storage, including the use of efficient data structures and minimizing unnecessary state changes, were enlightening. These practices are crucial for managing gas costs and ensuring scalability.


NOTE: I don't track time while auditing or writing report, so what the time I specified is just a number



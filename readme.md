# Blockchain Storing as a Service (BSaaS)

## Overview

The **Blockchain Storing as a Service (BSaaS)** approach aims to address the challenges of storage scalability and querying efficiency in blockchain networks. This strategy separates storage services from other blockchain nodes, enabling more efficient data management. By utilizing both on-chain and off-chain storage solutions, BSaaS offloads a portion of blockchain data from the primary network while maintaining decentralization and security. It adopts an indexing approach and provides an indexer API to facilitate efficient retrieval of blockchain data based on specific criteria.

## Requirements

- **Python**
- **Flask**
- **Docker & Docker Compose**
- **IPFS**

## Components

### 1. Blockchain Test Node

- **Purpose:** A blockchain test node developed using Python that implements basic functionalities such as block creation, timestamped transaction chaining, consensus, distributed ledger, and peer-to-peer (P2P) network communication.
- **Implementation:** The `BlockchainPy.py` script allows running an instance of this test node on a Flask server.
- **Interaction:**
  - **Send a Transaction:**
    ```bash
    curl -X POST -d "data" http://localhost:5000/transaction
    ```
  - **Read Data:**
    ```bash
    curl -X GET http://localhost:5000/read_request
    ```
  - **Verify Ledger Validity:**
    ```bash
    curl -X GET http://localhost:5000/validate_ledger
    ```

### 2. Gossip Protocol

- **Purpose:** The Epidemic Gossip Protocol is used for peer-to-peer data exchange.
- **Usage:** To run the protocol, execute the `gossip.py` script on each node. Each node will update its local copy of the ledger with new blocks sent by the test node and disseminate the information to neighboring nodes.

### 3. Apache Cassandra

- **Purpose:** A cluster of three Cassandra nodes is used for the off-chain part of the BSaaS approach.
- **Setup:**
  - **Connect to Cassandra:** Use `connectcassandra.py` to create and configure the off-chain environment by setting up keyspaces, tables, replication, and other parameters.
  - **Store Off-Chain Data:** Use `StoreOffchain.py` to store off-chain data from newly received blocks.

### 4. IPFS

- **Purpose:** The InterPlanetary File System (IPFS) is set up as an alternative approach for comparison with our solution.
- **Usage:** You can either run a cluster composed of four nodes or use the Pinata IPFS API service online. Set the necessary tokens and credentials for the Pinata service.


## Indexer API

The **Indexer API** facilitates the indexing of off-chain transactions before storing them in Cassandra nodes. It runs on Flask and provides the following functionalities:

- **Index New Block:** Indexes a new block using a POST request.
  ```bash
  curl -X POST -d "newblock" http://localhost:5001/post_transaction


- **Read Indexed Data:** Retrieves indexed transaction data using a GET request.
  ```bash
curl -X GET http://localhost:5001/get_transaction



## Experimentation

The aim of this experiment is to evaluate the efficiency of the proposed approach in terms of storage and query retrieval times. We have implemented and configured our test environment on a Dell PowerEdge R640 server. The environment includes:

- Blockchain nodes developed using Python, implementing functionalities such as consensus, block structure with cryptographic chaining, block management, and ledger operations.
- Nodes running on Flask, facilitating transaction handling and employing an Epidemic Gossip Protocol for peer-to-peer data exchange.
- An indexer API built on Flask for client interaction with the storage node, enabling efficient data retrieval and filtering.
- Apache Cassandra set up in a Docker container cluster for off-chain storage.
- An IPFS cluster with three nodes for comparison.

The read and write operations to and from these nodes and how these components interact will be presented in the subsequent section.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Thanks to the contributors and developers of the technologies and libraries used in this project.

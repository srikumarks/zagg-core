# What is ZAGG Protocol?
> WIP. This is being updated as you read

ZAGG Protocol’s implementation of enterprise-grade privacy on public blockchain is designed to mainstream enterprise-oriented payments and settlement use cases in blockchain.

ZAGG Protocol is building a novel privacy-preserving Blockchain. Innovations in ZAGG Protocol bring privacy to public blockchain through a hybrid model of tracking assets and values on the blockchain with both Accounts and UTXOs.This solution adapts the privacy and robustness of the UTXO model’s construct to build a flexible and programmable account-based blockchain.

Implementing privacy on account-based blockchains without leaking data and securing against double spend is a challenge with existing technology. Innovations in ZAGG Protocol bring privacy to public blockchain through a hybrid model of tracking assets and values on the blockchain with both Accounts and UTXOs. This solution adapts the privacy and robustness of the UTXO model’s construct to build a flexible and programmable account-based blockchain. The protocol also introduces the concept of Integrated subchains that will allow for nodes to privately transact one-to-one or in a coalition group within public chain without side chains/off-chains while allowing for mathematically provable assertions about effects of the private transactions without leaking information about the private transactions or coalitions.

ZAGG Protocol is based on [stellar](https://github.com/stellar) and [Bitcoin](https://github.com/bitcoin/bitcoin) code, it intends to add enterprise-grade privacy through Zero Knowledge Proofs (ZKPs).

Stellar-core is a replicated state machine that maintains a local copy of a cryptographic ledger and processes transactions against it, in consensus with a set of peers.
It implements the [Stellar Consensus Protocol](https://github.com/stellar/stellar-core/blob/master/src/scp/readme.md), a _federated_ consensus protocol.
It is written in C++14 and runs on Linux, OSX and Windows.
Learn more by reading the [overview document](https://github.com/stellar/stellar-core/blob/master/docs/readme.md).

Technical details of ZAGG Protocol are available in the [protocol document](https://www.zaggprotocol.com).


### Quick References (WIP)

[Introduction](https://github.com/zagg-protocol/docs/blob/master/Concepts/Introduction.md)

 [concepts](https://github.com/zagg-protocol/docs/tree/master/Concepts) 
 
[Protocol Overview](https://github.com/zagg-protocol/docs/blob/master/Concepts/protocol-overview.md)

[Business Case & Adoption](https://github.com/zagg-protocol/docs/blob/master/Concepts/business-case.md)
Learn more by reading the [docs](https://github.com/zagg-protocol/docs).



# Development Approach

ZAGG Protocol is starting the development from the stable build of Stellar. In the first phase, ZAGG intends to add UTXOs model to existing accounts based stellar. In the next phase, ZAGG Protocol team to build Zero-Knowledge Proofs along with other primitives to achieve transaction privacy.

A more detailed development plan is as follows:

### Protocol V0.0.1 | 
> Integration of bitcoin code base into the stellar fork to achieve dual accounts balance and UTXO balances.

**Assumptions:**
* Dual Ledgers with Dual accounts 
* Dual consensus 

**Description:**
After this step, User will be able to create a transaction via CLI. Use the data as input to the UTXO route. The communication is not in XDR and it's in either plain text or hex format. Stellar code invokes a call to Bitcoin code base and registers the transaction.  This step is the first step to create one merged running instance of both the protocols as foundation for next steps.


### Protocol V0.0.2 | 
> Both Account balance transactions and UTXO based transaction will go through Stellar consensus protocol (SCP) and transaction set is applied on the ledger after the consensus. This will add a new transaction to Stellar and the system will support both accounts state changes and UTXOs on the global ledger(s). There will be only one (Stellar) Consensus mechanism on the network.
 
**Assumptions:**
* Dual Ledgers with Dual accounts
* Single consensus
* Multiple Nodes

### Protocol V0.0.3 | 
> Add Account To UTXO and UTXO to Account conversions
This step will allow the user to move assets (multiple types) from her account into UTXOs (multiple types) and back and execute transfer of those assets/UTXOs to other users on the network over SCP. This step will also ensure no double spending and assets/resource consistency across both the types for any given user and on the network.

### Protocol V0.0.4
> Replace Bitcoin with ZCash
The above three steps of changes will then be applied to ZCash Sapling code base and it will be merged with Stellar code base. Subsequent code changes will be on the Zero Knowledge Proofs, Commitments and Nullifiers functionality so that the system will be able to support shielded ZKP transactions on Stellar consensus.

### Protocol V0.0.5
> Integrating the ledgers
The accounts and shielded UTXOs transactions will be merged so that all the transactions will be recorded and retrieved from one global ledger.

### Protocol V0.0.6
> Adding Private networks
Zagg supports creation of private subnetworks for forming coalitions where the transactions are visible to all the parties but not to the public. All the transactions are verified by the public nodes but cannot be seen by the public nodes.

### Protocol V0.0.7
> Adding Smart contracts
Zagg will increase the capability of smart contracts to enable different business models to operate on the network.






# To be updated

The installations instructions and running instructions are not yet updated. The following are the instructions to run a standalone stellar node.
# Installation

See [Installation](./INSTALL.md)

# Running tests

run tests with:
  `src/stellar-core --test`

run one test with:
  `src/stellar-core --test  testName`

run one test category with:
  `src/stellar-core --test '[categoryName]'`

Categories (or tags) can be combined: AND-ed (by juxtaposition) or OR-ed (by comma-listing).

Tests tagged as [.] or [hide] are not part of the default test test.

supported test options can be seen with
  `src/stellar-core --test --help`

display tests timing information:
  `src/stellar-core --test -d yes '[categoryName]'`

xml test output (includes nested section information):
  `src/stellar-core --test -r xml '[categoryName]'`

# Running tests against postgreSQL

There are two options.  The easiest is to have the test suite just
create a temporary postgreSQL database cluster in /tmp and delete it
after the test.  That will happen by default if you run `make check`.

You can also use an existing database cluster so long as it has
databases named `test0`, `test1`, ..., `test9`, and `test`.  To set
this up, make sure your `PGHOST` and `PGUSER` environment variables
are appropriately set, then run the following from bash:

    for i in $(seq 0 9) ''; do
        psql -c "create database test$i;"
    done

You will need to set the `TEMP_POSTGRES` environment variable to 0
in order to use an existing database cluster.

# Running tests in parallel

The `make check` command also supports parallelization. This functionality is
enabled with the following environment variables:
* `TEST_SPEC`: Used to run just a subset of the tests (default: "~[.]")
* `NUM_PARTITIONS`: Partitions the test suite (after applying `TEST_SPEC`) into
`$NUM_PARTITIONS` disjoint sets (default: 1)
* `RUN_PARTITIONS`: Run only a subset of the partitions, indexed from 0
(default: "$(seq 0 $((NUM_PARTITIONS-1)))")
* `TEMP_POSTGRES`: Automatically generates temporary database clusters instead
of using an existing cluster (default: 1)

For example,
`env TEST_SPEC="[history]" NUM_PARTITIONS=4 RUN_PARTITIONS="0 1 3" make check`
will partition the history tests into 4 parts then run parts 0, 1, and 3.

# Running stress tests
We adopt the convention of tagging a stress-test for subsystem foo as [foo-stress][stress][hide].

Then, running
* `stellar-core --test [stress]` will run all the stress tests,
* `stellar-core --test [foo-stress]` will run the stress tests for subsystem foo alone, and
* neither `stellar-core --test` nor `stellar-core --test [foo]` will run stress tests.

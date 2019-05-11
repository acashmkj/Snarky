# Snarky

Snarky is a proof-of-concept implementation of an onchain Bitcoin light client (SPV) on Ethereum, that carries out verifiable offchain BTC header verification using zk-SNARKs. It is an implementation similar to BTC Relay, but uses zk-SNARKS for carrying out the Bitcoin block header verification offchain.


## Background

[BTC Relay](https://github.com/ethereum/btcrelay) is an Ethereum contract implementation of Bitcoin SPV. This allows secure, onchain verification of Bitcoin transactions without any intermediaries. dApps that need to verify Bitcoin transactions in a trustless manner can make use of this relay.

Relayers submit Bitcoin block headers to the contract. These headers are then verified by the BTC Relay contract and verified headers are stored onchain. The contract can be queried by users/dApps to check for the validity of a Bitcoin transaction, allowing for participation of Bitcoin transactions in Ethereum contracts.

## The Technology

[zk-SNARKs](http://chriseth.github.io/notes/articles/zksnarks/zksnarks.pdf) is a technology based on zero knowledge proofs, that has the potential to dramatically improve blockchain scaling. Computations can be carried out offchain and their correctness can be verified onchain without having to execute them onchain. Furthermore, the amount of onchain computation required for verification is independent of the size and complexity of the computation, allowing for arbitrarily complex computations to be carried out at no incremental onchain cost.

Implementing zk-SNARKs is quite complex given the number of different parts that make up the machinery behind the technology. The computational problem has to be converted into the right form through a sequence of non-trivial steps followed by another intricate process of creating the actual proof. Some excellent work has been done by the [ZoKrates](https://github.com/JacobEberhardt/ZoKrates) team in building a toolset that helps convert a computational problem to a form that can be operated upon. The toolset also creates the other parts required for zk-SNARKs verification. Snarky development utilises this toolset.  

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes

### Installing

```
git clone https://github.com/acashmkj/Snarky.git
```

Make sure you have the following:

```
python 3
solc (solidity compiler)
ganache-cli (For Ethereum local testing)
zokrates (as given  at https://github.com/JacobEberhardt/ZoKrates). Copy the binary zokrates from docker to ./tools .
```

Install these using pip:

```
bitstring
hexbytes
```

To build all modules inside root ***Snarky***:

* Set solc path in Makefile

* mkdir build

* cd build

* make -f ../Makefile

***This step takes time.***

### Running the tests
* Run ganache-cli in a separate terminal
* cd test
* python snarks_test.py 125550

125550 is the start block number used for testing. To change the same, change corresponding witness file  `test/test_verify_multiple_headers.witness` and re-generate witness and proof using `Makefile`.

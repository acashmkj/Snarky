## How It Works

Unlike in BTCRelay, where the onchain submission of headers and their verification are part of a single Ethereum transaction, Snarky decouples the two so as to remove the bottleneck imposed by the gas limit for a single transaction/block. The overall process is as follows:

* Submission of Bitcoin Block Headers (Onchain)
* Generation of SNARK proof for block headers verification (Offchain)
* Verification of the generated SNARK proof (Onchain)



### Submission of Bitcoin Block Headers

Multiple sequential block headers can be submitted to the contract at a time. Multiple calls for submission can be made to submit a larger number of headers, making up a batch. In the following step, verification of these headers is done for an entire batch in one go, as there is no incremental cost in onchain SNARK verification with increase in batch size.

The submitted batch of block headers is stored, initialised as unverified, and a mapping between a computed batch hash and the headers is created (e.g. with 5 headers):

BatchHash = Hash(Header 1 + Header 2 + ... + Header 5)

For each block header, a sequential block number is assigned continuing from the last verified block stored onchain.


### Generation of SNARK proof for block headers verification

The witness generator for the SNARK function is fed the following inputs:

Private Inputs:
* All the block headers to be verified. These would be the same headers that were submitted in a single batch, H1 to H5.

Public Inputs:
* The BatchHash
* Blocknumber of the last verified block header stored onchain
* The hash of the last verified block header
* The timestamp of the last difficulty adjustment prior to the first submitted block header

The SNARK function carries out the following checks:

* Validates that for each header that its previous block hash matches the computed hash of the previous block header.
* The target difficulty for each block is verified.
* The BatchHash is validated against the computed hash of the concatenated block headers.



### Verification of the generated SNARK proof

The verification contract takes as input the generated proof and the public inputs to the witness generator.

It carries out the following checks on the inputs, and where necessary uses information from previously verified blocks stored onchain:


* Verify the proof
* Verify that the Block Hash of the last verified block is correct
* Verify that the block number of the first header is correct
* Verify that the timestamp of the last difficulty adjustment is correct

If these verifications are successful, all the block headers stored onchain corresponding to the BatchHash are marked as verified.

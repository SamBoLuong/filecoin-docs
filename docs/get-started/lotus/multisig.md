# Multisig wallet

A multisig (multi-signature) wallet refers to a wallet that requires multiple keys to authorize a `FIL` transactions.


## Create a multsig wallet

Use `lotus msig create` to a multisig wallet:

```bash
lotus msig create signerAddress1 signerAddress2 signerAddress3...

> Created new multisig:  f01002 f24mscgjtgymb3dqtm4ycwydh4nhygktgxm3nbgva
```

where `f01002` is the id address and `f24mscgjtgymb3dqtm4ycwydh4nhygktgxm3nbgva` is the actor address, both addresses represent the newly created multisig wallet.

By default, signatures from all signers are required for approving a transaction, however, the number of required approvals can be customized by passing in the `--required` option upon creation:

```bash
lotus msig create --required=2 signerAddress1 signerAddress2 signerAddress3
````

The above command creates a multisig wallet that has 3 signers, and 2 approvals are required for a transaction to be executed.

## Propose and approve a transaction

Any signer of the multisig wallet can propose a transaction. The proposer approves the transaction upon proposal. The transaction will only be executed when the amount of approvals received equals to the amount of required approvals(see the inspect the wallet session for getting the threshold). If the multisig wallet only requires one signer, then the transaction will be executed once the transaction is proposed.

You can propose a transaction to a multisig wallet that you are a signer of by running

```bash
lotus msig propose --from=proposerAddress walletAddress destinationAddress value
```

A message CID and the transaction id will be returned:

```bash
send proposal in message:  bafy2bzaceajm2mghc5rludlbcr3bnpqgcz5m6gmldq6ycrc4trkejz36tnrqe
Transaction ID: 0
```

where the `bafy2bzaceajm2mghc5rludlbcr3bnpqgcz5m6gmldq6ycrc4trkejz36tnrqe` is the `messageID` and `0` is the transaction id.

Other signers can approved the messages by running:

```bash
lotus msig approve walletAddress transactionID proposerAddress destinationAddress value
```

Note that the value of `transactionID`, `proposerAddress`, `destinationAddress` and `value` must all match the proposal params.

## Inspect a multisig wallet

To get the information of a multisig wallet, run

```bash
lotus msign inspect walletaddress
```

and below is a sample output:

```bash
Balance: 0 FIL
Spendable: 0 FIL
Threshold: 2 / 3 # amount of signature required / amount of signers the wallet has
Signers:
ID      Address
t01001  t1ai2gcr2xlpyxcbjlegojbpr3ovdyfdyzigjoyza
t0100   t3r4d3avth4agwxy6ko35reeuydcqaa6cq4mt6owg3zjq23pxqc6xvn7scb43dyhaf2cjnjhtioek6innbpgda
t01003  t3rpukrggza4jjt6vpihiqoekth6tiopzhvxbp36qhrzfu4xpk6n3mxo5geh6bdavkkkhqk7owt2an2wrundtq
Transactions:  1
ID      State    Approvals  To                                         Value   Method   Params
0       pending  1          t1fjswymsauvfh5zxw34t2pgz7iev2fn56unyw6ci  20 FIL  Send(0)
```
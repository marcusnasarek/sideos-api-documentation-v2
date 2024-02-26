---
description: API endpoints to create local and public DIDs
---

# 🪪 Wallets

A Decentralized Identifier (DID) comes in different flavours. We are using 2 types of DIDs in the sideos ecosystem:

1. **Private DID**. For a private DID our implementation follows the **did:key Method** ([https://w3c-ccg.github.io/did-method-key/](https://w3c-ccg.github.io/did-method-key/)) is a DID that is locally generated in a wallet and not anchored on a public registry. There is no link to a public blockchain or repository. On mobile phones this method is preferred for privacy and technical reasons.
2. **Public DID**. For a public DID our implementation follows the **did:sideos Method** ([https://github.com/sideos/sideos-did-method](https://github.com/sideos/sideos-did-method)) of a blockchain anchored DID. We are blockchain agnostic and have 2 DLT connectors implemented: Tezos and Ethereum smart contracts mapping sideos DIDs to the respective DID Document on chain or in the IPFS respectively. Public DIDs are for Issuers and Verifiers who want to be resolvable on a public registry for trust-related reasons. &#x20;

DIDs are created when a wallet is set up and there is no reason to explicitely create a DID by yourself. Still, there may be scenarios where you want to build a local system, e.g. insite an IoT device, where the DID should be pre-build. In that scenario you would call the endpoint for creating a local DID.&#x20;

If you want to manage sub-wallets in a multi-tenant system of an issuer or verifier, you can create a new public DID by calling the API endpoint to create a public DID. You can chose if the DID should be anchored on a blockchain or not.&#x20;

## Create a Local DID

To create a local DID you call the endpoint with an empty data set. The response gives you the new DID and the respective public key material. The private key is securely stored in the platform database and can be provisioned in a secure channel. We currently do this on demand.  &#x20;

{% swagger src="../../.gitbook/assets/sideosAPIv3 (1).yaml" path="/createlocaldid" method="post" %}
[sideosAPIv3 (1).yaml](<../../.gitbook/assets/sideosAPIv3 (1).yaml>)
{% endswagger %}

## Create a Public DID

To create a public DID which can be optionally anchored on the blockchain you call the endpoint with a name for the wallet and a boolean value. If the value for `anchored` is true, the DID method is `did:sideos` and the DID document is written to the [Tezoz](https://tezos.com/) blockchain. We also support [Ethereum](https://ethereum.org/) and privacy-preserved zero-knowledge networks [MINA](https://minaprotocol.com/) and [Aleo](https://aleo.org/) on customer demand. In the case of anchoring on Ethereum the DID document is stored on the [IPFS](https://ipfs.tech/). If the value for anchored is false we create a private DID `did:private`.

The wallet is stored and managed in the platform as a cloud wallet.&#x20;

{% swagger src="../../.gitbook/assets/sideosAPIv3 (1).yaml" path="/createpublicdid" method="post" %}
[sideosAPIv3 (1).yaml](<../../.gitbook/assets/sideosAPIv3 (1).yaml>)
{% endswagger %}

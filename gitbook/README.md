---
description: >-
  sideos provides SSI as a Service through a REST API that allows you to issue
  and verify SSI credentials.
---

# Welcome!

## Overview

This documentation will help you to get up to speed with the API of sideos SSI as a Service. The documentation provides you with an overview of the general flows, how to set up a client, and the description of the requests and the respective endpoint responses. If you never heard about SSI here's a short primer:&#x20;

SSI is for Self-Sovereign Identity. It was developed to store identity data on some user device in a way it cannot be misused or tempered. The idea was to separate the roles in the identification flow in 3 roles: 1) the issuer of the identity data, 2) the holder of the identity, and 3) the verifier of the identity. That was a response of the growing problem internet users managing dozens of identities on the one side and service providers struggling to keep the user's data safe and compliant. The result was to use modern cryptography and a new data format to store data on a users device securely (as a verifiable credential) to define a protocol for offering and requesting these credentials. &#x20;

That said, the general setup is therefore two-folded for 1) providing data and 2) requesting data:&#x20;

1. Issuance of a verifiable credential: an issuer provides the user with the verifiable credential which the user checks and stores on the user's device.&#x20;
2. Verifying a credential: a verifier (so called relying party because it relies on the issuer's competence and the protection of the credential) to request and verify a credential

<figure><img src=".gitbook/assets/figure Home 1.png" alt=""><figcaption><p>issuing and verifying credentials</p></figcaption></figure>

The API is build around these 2 general flows. To access the API you need to retrieve an API Key. To get one, sign up for an account with sideos there is a free trial account to get started:

{% content-ref url="signing-up-to-sideos/" %}
[signing-up-to-sideos](signing-up-to-sideos/)
{% endcontent-ref %}

## Want to jump right in?

With your brand new API Key you are ready to jump right in. Read the quick start doc and get your hands dirty:

{% content-ref url="quick-start.md" %}
[quick-start.md](quick-start.md)
{% endcontent-ref %}

## Want to deep dive?

Dive a little deeper and start exploring our API reference to get an idea of everything that's possible with the API:

{% content-ref url="reference/api-reference.md" %}
[api-reference.md](reference/api-reference.md)
{% endcontent-ref %}

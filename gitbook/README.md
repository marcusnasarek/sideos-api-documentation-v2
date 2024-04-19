---
description: >-
  sideos provides SSI as a Service through a REST API that allows you to issue
  and verify SSI credentials.
cover: .gitbook/assets/Header GitBook Docs.png
coverY: 34
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Welcome!

## Overview

This documentation will help you to get up to speed with the API of sideos SSI as a Service. The documentation provides you with an overview of the general flows, how to set up a client, and the description of the requests and the respective endpoint responses. If you never heard about SSI here's a short primer:&#x20;

SSI is for Self-Sovereign Identity. It was developed to store identity data on some user device in a way it cannot be misused or tempered. The idea was to separate the roles in the identification flow in 3 roles: 1) the issuer of the identity data, 2) the holder of the identity, and 3) the verifier of the identity. That was a response of the growing problem internet users managing dozens of identities on the one side and service providers struggling to keep the user's data safe and compliant. The result was to use modern cryptography and a new data format to store data on a users device securely (as a verifiable credential) to define a protocol for offering and requesting these credentials. &#x20;

That said, the general setup is therefore two-folded for 1) providing data and 2) requesting data:&#x20;

1. Issuance of a verifiable credential: an issuer provides the user with the verifiable credential which the user checks and stores on the user's device.&#x20;
2. Verifying a credential: a verifier (so called relying party because it relies on the issuer's competence and the protection of the credential) to request and verify a credential

<figure><img src=".gitbook/assets/figure Home 1.png" alt=""><figcaption><p>issuing and verifying credentials</p></figcaption></figure>

The API is build around these 2 general flows. We've prepared an example for a small web service that integrates with the sideos API to provide a passwordless login bases on SSI credentials. You can start from there to see the API in action and get your hands dirty:

{% content-ref url="quick-start.md" %}
[quick-start.md](quick-start.md)
{% endcontent-ref %}

## Want to jump right in?

To access the API you need to retrieve an API Key. To get one, sign up for an account with sideos there is a free trial account to get started:

{% content-ref url="sign-up-and-get-started/" %}
[sign-up-and-get-started](sign-up-and-get-started/)
{% endcontent-ref %}

## Want to deep dive?

Dive a little deeper and start exploring our API reference to get an idea of everything that's possible with the API:

{% content-ref url="reference/api-reference/" %}
[api-reference](reference/api-reference/)
{% endcontent-ref %}

---
description: Request credentials from an SSI wallet
---

# Request Credentials

## Introduction

If an organisation wants to get verifiable data from a user, e.g. to authenticate a web user, the organisation can use the sideos API to create a request for Verifiable Credentials from an SSI wallet. If the SSI wallet has the requested credential types available, it will respond with the verifiable credentials if authorised by the user.

Like in the Offer flow, also in the Request flow there are 3 parties:&#x20;

1. **SSI wallet**. Interacts, stores, and manages verifiable credentials.
2. **Web Service**. Role of the Verifier requesting verifiable credentials from the SSI wallet.
3. **sideos API**. Creates the request of verifiable credentials on behalf of the verifier.

sideos provides an **SSI wallet** available as <mark style="color:purple;">**sideos**</mark> <mark style="color:purple;">**Transponder App**</mark> for Android in Google Play Store, for iOS in Apple's App Store, and as <mark style="color:green;">**sideos Desktop Wallet**</mark> for Chrome, Firefox, and Safari.&#x20;

The **Web Service** is your server you will integrate with the sideos API. The **Web Server** will interact with the **SSI wallet** and control the flow depending on the business requirements. To verify a credential the **Web Service** creates a request for verifiable credential types that will be compiled by the **sideos API**, and in return receives the request data structure for ask for the respective verifiable credentials that should be shardd by the **SSI wallet**. &#x20;

## Request Flow

See the diagram below for the flow chart for a credential request.&#x20;

<figure><img src="../../.gitbook/assets/sideos API Request Flow.png" alt=""><figcaption></figcaption></figure>

{% swagger src="../../.gitbook/assets/sideosAPIv3 (2).yaml" path="/createrequestvc" method="post" %}
[sideosAPIv3 (2).yaml](<../../.gitbook/assets/sideosAPIv3 (2).yaml>)
{% endswagger %}

{% swagger src="../../.gitbook/assets/sideosAPIv3 (2).yaml" path="/consumerequest" method="post" %}
[sideosAPIv3 (2).yaml](<../../.gitbook/assets/sideosAPIv3 (2).yaml>)
{% endswagger %}


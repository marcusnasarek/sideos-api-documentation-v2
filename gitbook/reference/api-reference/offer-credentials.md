---
description: Issue credentials to an SSI wallet
---

# 📄 Offer Credentials

## Introduction

If an organisation wants to provide credentials to users, e.g. as a login credential, the organisation can use the sideos API to create Verifiable Credentials and offer these Verifiable Credentials to their users to be stored in their SSI Wallet.&#x20;

There are 3 parties:&#x20;

1. **SSI wallet**. Interacts, stores, and manages verifiable credentials.
2. **Web Service**. Role of the Issuer creates verifiable credentials and provides them to the SSI wallet.
3. **sideos API**. Converts claims into verifiable credentials on behalf of the issuer.

sideos provides an **SSI wallet** available as <mark style="color:purple;">**sideos**</mark> <mark style="color:purple;">**Transponder App**</mark> for Android in Google Play Store, for iOS in Apple's App Store, and as <mark style="color:green;">**sideos Desktop Wallet**</mark> for Chrome, Firefox, and Safari.&#x20;

The **Web Service** is your server you will integrate with the sideos API. The **Web Server** will interact with the **SSI wallet** and control the flow depending on the business requirements. To issue a credential the **Web Service** provides the data which will be sent as claim records to the **sideos API**, and in return receives the verifiable credential that will be provided to the **SSI wallet**. &#x20;

### Offer Flow

See the diagram below for the flow chart for a credential offer.&#x20;

<figure><img src="../../.gitbook/assets/sideos API Offer Flow.png" alt=""><figcaption><p>Verifiable Credential offer flow</p></figcaption></figure>

{% swagger src="../../.gitbook/assets/sideosAPIv3 (2) (1).yaml" path="/createoffervc" method="post" %}
[sideosAPIv3 (2) (1).yaml](<../../.gitbook/assets/sideosAPIv3 (2) (1).yaml>)
{% endswagger %}

{% swagger src="../../.gitbook/assets/sideosAPIv3 (2) (1).yaml" path="/consumeoffer" method="post" %}
[sideosAPIv3 (2) (1).yaml](<../../.gitbook/assets/sideosAPIv3 (2) (1).yaml>)
{% endswagger %}

## Signed Credentials

The basic credential issue flow creates a credential offer and presents the Verifiable Credential to the wallet e.g., via QR Code  to pick it up. Because the SSI audience (DID of the SSI wallet) is not known in the moment of the credential creation, the provision channel needs to be trustworthy.&#x20;

There are several protocols available, some more and others less complex. In many cases in the enterprise environment you already have a secure environment, e.g. via intranet pages or email. Here you can strip down the protocol to a simple credential provision without an embedded channel authentication and rely on the secure environment.&#x20;

There is an endpoint to create a Verifiable Credential that includes the audience (DID) as an id in the credential subject. That way, the DID of the wallet can be included in the credential subject and thus, being ensured to be stored and used only on the respective SSI Wallet.&#x20;

{% swagger src="../../.gitbook/assets/sideosAPIv3 (2) (1).yaml" path="/createsignedvc" method="post" %}
[sideosAPIv3 (2) (1).yaml](<../../.gitbook/assets/sideosAPIv3 (2) (1).yaml>)
{% endswagger %}

## Offer and Request Credentials in 1 step

Imagine a flow, where the user doesn't have a credential yet and is in an onboarding flow. You could issue a credential and immediately request it for migrating the user into the new SSI flow. In that case you could use the offer\&request endpoint

{% swagger src="../../.gitbook/assets/sideosAPIv3.yaml" path="/createofferrequestvc" method="post" %}
[sideosAPIv3.yaml](../../.gitbook/assets/sideosAPIv3.yaml)
{% endswagger %}

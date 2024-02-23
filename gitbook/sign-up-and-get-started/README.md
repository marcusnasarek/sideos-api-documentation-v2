---
description: Getting started with the sideos administration console
---

# Sign up & get started

## Introduction

The <mark style="color:purple;">**sideos administration console**</mark> is the management interface for **issuers** and **verifiers.** It is the place where you can manage the credentials you want to issue or verify, your company profile, and access to the sideos API for client applications.&#x20;

<figure><img src="../.gitbook/assets/Screen Shot 2024-02-23 at 12.36.19 PM.png" alt=""><figcaption><p>The sideos administration console</p></figcaption></figure>

There is no username and password being used and you will login with SSI credentials, so the first thing you will require is the sideos wallet for iOS or Android Application which is called Sideos Transponder ([**android**](https://play.google.com/store/apps/details?id=com.sideosmobile) **|** [**iOS**](https://apps.apple.com/de/app/sideos-transponder/id1611001158?l=en)). Once the app is downloaded, the signup process can begin.&#x20;

## Signing up for the first time

Visit our [**Onboarding Page**](https://juno.sideos.io/plan-onboarding/1) to start the signup process. From here, insert the required information and scan the QR Code. Now you will receive a credential on your app which is linked to your device and thus cannot be copied by any external party.&#x20;

Once you scanned the QR Code, you will be redirected to the Juno web app. Next time you want to visit the portal, visit sideos.io and click on “Log in to Juno” on the top right corner. Then simply scan the QR Code with your linked device and you will be successfully connected.

Now you have access to the platform - what's next? The general workflow is the following:&#x20;

1. Create an API access token, this will go into your client application's configuration as an API Key in the the request header as `X-Token`
2. Define the credential in 2 steps: a) create claim types, e.g. a name and an email address, and b) create an credential type containing the claim types.
3. Note down the credential ID, this will be used in your client application to refer to the ID in calls of the API.
4. Use the API Key for authorised calls of the API endpoints, and the credential IDs to refer to credential types when you request or offer a verifiable credential.&#x20;

## Create an API Key

<figure><img src="../.gitbook/assets/Screen Shot 2024-02-23 at 12.51.33 PM.png" alt=""><figcaption><p>Example Company settings in the sideos administration console</p></figcaption></figure>

To get access to the sideos API and use the credential types you created in your account you need to create an **API key:**

1. Go to _Settings_ > _Company Settings._&#x20;
2. Click on _Create token_
3. Note down the **token** as this is the API Key that goes in your client application's request header as `X-Token`.&#x20;
4. Note also down the **Company DID** as you may use the DID for validating credentials based on the issuer identification.&#x20;

Verifiable credentials issued with the sideos API contain the issuer name which is the **Company Name** in the settings, and the issuer DID which is the **Company DID** in the settings.&#x20;

## Admins

To begin with, you might want to add other admins who have full control over the platform. Your account is automatically the main admin, and with any paid plan, you can have an unlimited amount of admins. An **Admin** is a person within the organisation that has access to sideos administration console, can onboard other admins, create proofs and credentials, and have an overview of transactions.

See [add-an-admin.md](add-an-admin.md "mention") for a step by step description on how to add a new admin.

## Proof/Claims

Next, you’ll want to set up proofs (also called claims). These are the data elements necessary to form Credentials. Their type and content are defined following the schema.org definitions. Essentially, this is the raw data format which defines what the credential represents. This can be the representation of an email, the data elements in a digital ID card, a specific date (e.g. date of birth), or whatever you can come up with - the possibilities are endless.&#x20;

See [creating-a-credential-type.md](creating-a-credential-type.md "mention") for a step by step description on how to create a claim (proof) type.

## Credentials

After having created a proof (or multiple), you are now ready to create a credential.&#x20;

A Credential (originally Verifiable Credential) is a certified data set that the identity holder owns and can be stored in a digital wallet. They can be issued from the organisations or self-signed by end-users, then exposed for verification.  Essentially, a credential is a bundle of proofs which are certified and locally stored on the identity holders digital wallet.&#x20;

To create a credential, the corresponding proofs need to be set up already. However, if you realise you need more proofs than you currently have, you can create proofs parallel to the credentials.&#x20;

See [creating-a-credential-type.md](creating-a-credential-type.md "mention")for a step by step description on how to create a credential type (template).

## Next Steps

How you have all the core elements in place. Next would be to issue the created credentials to your users. For this, we have created a powerful yet lean API. To send and request credentials, follow the steps mentioned in our API documentation // HERE.&#x20;

Furthermore, the SDK documentation can be found // HERE.

## Transactions

Now that your system is up and running, you, your admins, and your users will start generating transactions. These are essentially all interactions between you and your users such as offering and requesting credentials. These can be downloaded as a CSV file for reporting purposes. Transactions may also help you understand where all your traffic is coming from, how many transactions you are using a month (which correlates to the billing plan you will have), and what type of credentials are being used the most.

## Multi-tenant: Onboarding Companies

If you are a multi-tenant company, Juno has additional built-in functionalities which ensure that you can manage your customers/tenants effectively.&#x20;

Once you are set up as a multi-tenant user, you will have access to the onboarding tab in the sideos SSIaaS platform. Here, you can add companies and manage the customers/sub-tenants with ease.

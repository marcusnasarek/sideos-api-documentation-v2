---
description: The sideos API reference
---

# API Reference

Dive into the specifics of each API endpoint by checking out our complete documentation.

## Offer Credentials

If an organisation wants to provide credentials to users, e.g. as a login credential, the organisation can use the sideos API to create Verifiable Credentials and offer these Verifiable Credentials to their users to be stored in their SSI Wallet.&#x20;

{% content-ref url="offer-credentials.md" %}
[offer-credentials.md](offer-credentials.md)
{% endcontent-ref %}

## Request Credentials

If an organisation wants to get verifiable data from a user, e.g. to authenticate a web user, the organisation can use the sideos API to create a request for Verifiable Credentials from an SSI wallet. If the SSI wallet has the requested credential types available, it will respond with the verifiable credentials if authorised by the user.

{% content-ref url="request-credentials.md" %}
[request-credentials.md](request-credentials.md)
{% endcontent-ref %}

## Templates

In the calls to create Credential Request or Credential Offer is a reference to the respective credential type by its ID. You can call the API to get the type definition of a certain template. &#x20;

{% content-ref url="templates.md" %}
[templates.md](templates.md)
{% endcontent-ref %}

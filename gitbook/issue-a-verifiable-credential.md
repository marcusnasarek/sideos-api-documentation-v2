# Issue a Verifiable Credential

Lets assume you want to provide a user with a credential, e.g. to provide a login credential for a web service. In that example, the **Web Service** is the **issuer** and will create a login credential from a user's _Name_ and _Email Address_ with the help of the **sideos API.** The Web Service will then offer the credential to the **SSI Wallet** that stores the credential securely for the later use as a Login credential.

<figure><img src=".gitbook/assets/Basic Credential Offer.png" alt=""><figcaption><p>A basic credential issuance flow</p></figcaption></figure>

The steps are:

1. The web application (**Web Service**) calls the sideos API posting the dataset to the `/v3/createoffervc` endpoint. The dataset contains the credential ID, the claims, and a callback url.
2. The **sideos API** responds with an credential offer provided as a JWT to the **Web Service**.
3. The **Web Service** presents the JWT to the **SSI wallet**. That can be done as a QR Code, via NFC, or via Bluetooth.&#x20;
4. The **SSI wallet** validates the credential offer and asks the user to accept the credential offer. If the user accepts the **SSI wallet** calls the callback url by posting the JWT back to the **Web Service**.
5. The **Web Service** optionally checks the validity of the JWT with the **sideos API** calling the `/v3/consumeoffer` endpoint.  &#x20;
6. The **sideos API** responds with the original JWT and an error code. The **Web Service** can now do the magic based on the credential acceptance status, e.g. by marking the status in some database or providing some conditional information on the web application.&#x20;
7. The **Web Service** responds with the status 200 to the **SSI wallet** if the validation was successful.

Lets do this simple flow in an example below.&#x20;

## Get your API key

Your API requests are authenticated using an API key. Any request that doesn't include an API key will return an error.

You can generate an API key from your sideos administration console at any time. See [sign-up-and-get-started](sign-up-and-get-started/ "mention") for information how to get started and retrieve an API key.

## Get a credential ID

You need a credential type that includes the Name and Email Address of the user. See section [creating-a-credential-type.md](sign-up-and-get-started/creating-a-credential-type.md "mention").&#x20;

## Make your first request

Now when you have your API Key and a credential ID for your defined credential template, you can call the sideos API to get a credential offer for issuing a credential.&#x20;

To make your first request, send an authenticated request to the pets endpoint. This will create a `credential offer`, which can be captured by the user and stored on the sideos wallet.

## Create a credential offer.

<mark style="color:green;">`POST`</mark> `https//juno.sideos.io/api/v3/createoffervc/`

Creates a credential offer based on the credential containing the claim data provided in the request.

#### Headers

| Name                                      | Type             | Description |
| ----------------------------------------- | ---------------- | ----------- |
| Content Type                              | application/json |             |
| X-Token<mark style="color:red;">\*</mark> | \<API Key>       |             |

#### Request Body

| Name                                         | Type   | Description                                |
| -------------------------------------------- | ------ | ------------------------------------------ |
| templateid<mark style="color:red;">\*</mark> | number | The `id` of the credential type            |
| dataset<mark style="color:red;">\*</mark>    | object | The data set for the credential offer      |
| domain<mark style="color:red;">\*</mark>     | string | The callback URL                           |
| challenge<mark style="color:red;">\*</mark>  | string | An unique identifier for the user session. |

{% tabs %}
{% tab title="200 credential offer successfully created" %}
```json
{
    "data": {
        "error": 0,
        "jwt": string,
        "signid": [string],
    }
}
```
{% endtab %}

{% tab title="401 Permission denied" %}

{% endtab %}
{% endtabs %}

Take a look at how you might call this method using `Typescript code`, or via `curl`:

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```
curl https://juno.sideos.io/v3/createoffervc/  
    -H 'X-Token: <YOUR_API_KEY>'
    -H 'Content-Type: application/json'  
    -d '{"templateid":<YOUR_CREDENTIAL_ID>,
    "dataset":{"name":"Wilson Smith","email":"ws@example.com"},
    "challenge":"1234567890",
    "domain":"https://callback.example.com"}' 
```
{% endcode %}
{% endtab %}

{% tab title="Typescript" %}
```typescript
let data = { 
   headers: {
      "X-Token": "<YOUR API KEY>",
      "Content-Type": "application/json"
   },
   body: {
      "templateid": <YOUR TEMPLATE ID>,
      "domain": "<URL ENDPOINT OF YOUR CALLBACK",
      "challenge": "<YOUR UNIQUE CHALLENGE TO IDENTIFY THIS CALL",
      "dataset": {
         "name": "Wilson Smith",
         "email":"ws@example.com"
      }
   }
}

let response = client.post("https://juno.sideos.io/v3/createoffervc", data)

// If everything is correct "response" should contain the JWT signed by your account DID wallet, ready to be displayed as a QRCode 
```
{% endtab %}
{% endtabs %}

You will receive a response containing a data object with a jwt field.&#x20;

{% code overflow="wrap" %}
```json
{
    "data": {
        "error":0,
        "jwt":"eyJ0eXAiOiJKV1QiLCJhbGciOiJFZERTQSJ9.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSIsImh0dHBzOi8vanVuby5zaWRlb3MuaW8vY29udGV4dC9wcm9vZnN0eXBlcyJdLCJpYXQiOjE3MDg3MTc3NzUzOTEsImlzcyI6ImRpZDprZXk6djAwMTp6Nk1rbVFTWHZhOXZEbzNBU3JQRWtpa1AzSzF5eWlDVnZkNUttOHhqYkZqR2ROdzQiLCJhdWQiOiJkaWQ6dW5rbm93biIsInN1YiI6ImRpZDp1bmtub3duIiwiZXhwIjoxNzA4NzE3Nzc4OTkxLCJqdGkiOiJiMDMyZThlMy1iY2Q2LTQ0NWItYTQ5OS02ZWZjYzMwMTcyZjAiLCJ2ZXJpZmlhYmxlQ3JlZGVudGlhbCI6W3siQGNvbnRleHQiOlsiaHR0cHM6Ly93d3cudzMub3JnLzIwMTgvY3JlZGVudGlhbHMvdjEiLCJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy9leGFtcGxlcy92MSJdLCJpZCI6Ijg3N2JiYmVlLTJjYzUtNGJlZC04NzFmLWQ5ZGIwNjkzNjY1YiIsInR5cGUiOlsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJVc2VyIl0sImNyZWRlbnRpYWxTdWJqZWN0Ijp7Im5hbWUiOiJXaWxzb24gU21pdGgiLCJFbWFpbCI6IndzQGV4YW1wbGUuY29tIiwiaWQiOiJkaWQ6dW5rbm93biJ9LCJpc3N1ZXIiOnsiaWQiOiJkaWQ6a2V5OnYwMDE6ejZNa21RU1h2YTl2RG8zQVNyUEVraWtQM0sxeXlpQ1Z2ZDVLbTh4amJGakdkTnc0IiwibmFtZSI6Ik1OIHNpZGVvcyBUcnVzdCBTZXJ2aWNlcyJ9LCJpc3N1YW5jZURhdGUiOiIyMDI0LTAyLTIzVDE5OjQ5OjM1KzAwOjAwIiwiZXhwaXJhdGlvbkRhdGUiOiIyMDI1LTAyLTIyVDE5OjQ5OjM1KzAwOjAwIiwiZXhwIjoxNzQwMjUzNzc1MjI2LCJwcm9vZiI6eyJ0eXBlIjoiRWQyNTUxOVNpZ25hdHVyZTIwMjAiLCJjcmVhdGVkIjoiMjAyNC0wMi0yM1QxOTo0OTozNS4zMDlaIiwiandzIjoiZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKRlpFUlRRU0o5Li43VUtkYXlaUmlhNlNFOW5BVXFQLTFtNER1OWtsUDY0Y2RYUnFWclRBYXlRcE5DUElqdDRGakxDTU5DUlRMOUNlU2V0UFI3VWtLSmJyeHl3UDF6SGZBdyIsInByb29mUHVycG9zZSI6ImFzc2VydGlvbk1ldGhvZCIsInZlcmlmaWNhdGlvbk1ldGhvZCI6ImRpZDprZXk6djAwMTp6Nk1rbVFTWHZhOXZEbzNBU3JQRWtpa1AzSzF5eWlDVnZkNUttOHhqYkZqR2ROdzQifX1dLCJwcm9vZiI6eyJjaGFsbGVuZ2UiOiIxMjM0NTY3ODkwIiwidmVyaWZpY2F0aW9uTWV0aG9kIjoiZGlkOmtleTp2MDAxOno2TWttUVNYdmE5dkRvM0FTclBFa2lrUDNLMXl5aUNWdmQ1S204eGpiRmpHZE53NCIsImNyZWF0ZWQiOiIyMDI0LTAyLTIzVDE5OjQ5OjM1KzAwOjAwIiwiZG9tYWluIjoiaHR0cHM6Ly9jYWxsYmFjay5leGFtcGxlLmNvbSIsImp3cyI6IjI5a2J5T3pTcDRreEU2MnRWQ2pzVWZ4NEFIM0RaLUw1Ql9RVEJyc1BsdFViOE5QZXRteU5GWUlhWEVFdGdma3UwVXgweWFLU3VObDVBQUl1TW5fY0JBIiwicHJvb2ZQdXJwb3NlIjoiYXV0aGVudGljYXRpb24iLCJ0eXBlIjoiRVMyNTYifSwidHlwZSI6WyJWZXJpZmlhYmxlUHJlc2VudGF0aW9uIiwiQ3JlZGVudGlhbE9mZmVyIl19.2mxmsQ10dGKHbqTrJEQci5MDiTOPFGZHrHKCtS2R5qruNBEGtdxaGVSVy4iJmXlTf3JS7L7AzE2aTA5laAFLCA",
        "signid":["877bbbee-2cc5-4bed-871f-d9db0693665b"]
    }
}
```
{% endcode %}

The string is a base64 encoded JWT. Check the jwt string with a JWT parser, e.g. [https://jwt.io](https://jwt.io) to see the content. The JWT can be read by the **sideos wallet**, e.g. as a QR Code and shown to the user like this:

<img src=".gitbook/assets/Iphone 14 - 1 (1).png" alt="" data-size="original">&#x20;

The wallet asks the user to accept or decline the offer. If the user accepts the offer, the credential is stored on the phone and the callback URL from the `domain` field in the credential offer is called to complete the credential acceptance.&#x20;

{% hint style="info" %}
The callback URL needs to respond with a 200 status in order to successfully store the credential. If an error code comes back the wallet shown an error screen and does not store the credential in the wallet.&#x20;
{% endhint %}

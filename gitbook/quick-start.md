---
description: Integrating sideos into a web service
---

# Quick Start

## Passwordless Login with SSI

If you want to try out a simple example using a verifiable credential for a passwordless login to your web service,  you can start with an example code available on [Github](https://github.com/rheikvaneyck/sideos-login). It's a simple node.js based web service which integrates sideos for a passwordless login.&#x20;

The example should help you to understand the basic flow of issuing and verifying a credential. Let's start with downloading the sideos wallet and setting up the web service that will interact with the user.

### Get the sideos Wallet

First, you need do get the sideos wallet to get started. You can install the sideos wallet from [Google Play](https://play.google.com/store/apps/details?id=com.sideosmobile) or [Apple's App Store](https://apps.apple.com/de/app/sideos-transponder/id1611001158?l=en).

&#x20;Once you've installed the wallet proceed below.&#x20;

### Setting up a Web Service

Make sure you have Node.js (>18) and yarn installed. There are plenty of instructions available in the internet to help you install Node.js on your system.&#x20;

Next step is to clone the example repo locally:&#x20;

```
git clone git@github.com:rheikvaneyck/sideos-login.git
```

We have prepared a trial `ACCESS_TOKEN` and a credential template (`LOGIN_TEMPLATE_ID=63`) that you will need to access the sideos API for issuance and verification of credentials. To generate your own access token and to create your own credential templates you can [open an account on the sideos console](sign-up-and-get-started/) for free.

To use it in the example service, create an `.env` file to the root folder of the repository and add the following content:

```
ACCESS_TOKEN=dee7dbfb-51fb-4dd7-a4c8-b9956633038a
SSI_SERVER='https://juno.sideos.io'
LOGIN_TEMPLATE_ID=63
CALLBACK_URL=http://pi3p:9000
DID_ISSUER=did:key:V001:z6MksS5dgtn8Hiw4VfiQp6E2jnarJgrNDmUKwE8yPcA3Mec6
SESSION_SECRET=1B80EE50-542D-4527-8767-C0E8A5278F2C
```

The `CALLBACK_URL` is an API endpoint of your web service to which the wallet sends responses and shared credentials. That needs to be changed to your individual environment. The `DID_ISSUER` is the id of the trial account wallet and will be used to validate the credential.&#x20;

The example is using **redis** as a session storage. To install redis on a Linux-based machine, e.g. using Ubuntu you install and setup the **redis** server as a system service with the following commands:

```
sudo apt update && sudo apt install redis-server -y
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

Change into the example code folder and install the node packages with&#x20;

```
yarn install
```

and start the server with yarn:

```
yarn start:dev
```

The output should be something like this below:

```
yarn start:dev
yarn run v1.22.21
$ nodemon src/index.ts
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): src/**/*
[nodemon] watching extensions: ts,json
[nodemon] starting `ts-node ./src/index.ts src/index.ts`
Server started at http://MacBook-Work.local:9000
Redis Client connected...
Redis Client ready
```

### Issue a Verifiable Credential to Your Wallet

The example web service provides a path to issue a credential. With your browser navigate to `http://<your IP/hostname>:9000/issue` to generate a credential offer and capture the credential in your wallet.&#x20;

<figure><img src=".gitbook/assets/Screenshot 2024-04-19 at 17.21.51.png" alt=""><figcaption><p>Render a credential offer to provider a login credential</p></figcaption></figure>

If you scan the QR Code with the sideos wallet you will see the screen below to accept the credential offer and store it on your phone. If you select the credential and click in CONFIRM the credential is stored on your phone.

<figure><img src=".gitbook/assets/Credential Offer.png" alt="" width="375"><figcaption><p>A credential offer after scanning the QR Code</p></figcaption></figure>

The code to create a issue endpoint to show the QR Code on the frontend is shown below:

```typescript
export const getCredentialOffer = async () => {  
  const challenge = uuidv4();   
  try {
    const response = await axios.post(process.env.SSI_SERVER + '/v3/createoffervc',
    {
      templateid: process.env.LOGIN_TEMPLATE_ID,
      dataset: {
        Email: "user.name@example.com",
        name: "Anton Mustermann",
        Role: "User"
      },
      domain: process.env.CALLBACK_URL + '/issue',
      challenge: challenge
    },
    {
      headers: {
        'Content-Type': 'application/json',
        'X-Token': process.env.ACCESS_TOKEN
      }
    });
    return { 
      data: {
        error: 0,
        challenge: challenge,
        jwt: response.data.data.jwt
      }
    }
  } catch (err) {
    console.log(`Error posting juno at ${process.env.SSI_SERVER}/v3/createoffervc`);
    console.log(err);
    return {
      data: {
        error: `Error posting juno at ${process.env.SSI_SERVER}/v3/createoffervc`
      }
    }  
  }
}
```

If you accept the offer the wallet send the confirmation back to the callback url. The web service then knows the credential was accepted and can initiate some completion process.&#x20;

The example code for the callback endpoint is shown below:

```typescript
export const postConsumeCredentialOffer = async (req: Request, res: Response, next: NextFunction) => {
  const redisClient = createClient({ legacyMode: true })
  await redisClient.connect().catch(console.error)
  
  try {
    const jwt = req.body.jwt;
    let authToken: string = req.params.token;
    if (!authToken && req.query.challenge) {
      authToken = <string>req.query.challenge
    }
    console.log('Consume Offer challenge parameter:', authToken)
    let vcs: Array<any> = [];
    const response = await axios.post(process.env.SSI_SERVER + '/v3/consumeoffer', {
      template: process.env.TEMPLATE_ID,
      token: jwt
    }, {
      headers: {
        'Content-Type': 'application/json',
        'X-Token': process.env.ACCESS_TOKEN
      }
    })
    const res_jwt = response.data.data.jwt
    const decoded: any = decodeJwt(res_jwt);
    decoded.verifiableCredential.forEach((element: any) => {
      vcs.push(element)
    });
    console.log('Trusted DID_ISSUER: ', process.env.DID_ISSUER);
    console.log('email: ', vcs[0].credentialSubject.Email);
    console.log('Name: ', vcs[0].credentialSubject.name);
    console.log('did: ', vcs[0].credentialSubject.id);
    console.log('issuer_did: ', vcs[0].issuer.id);

    if(vcs[0].issuer.id === process.env.DID_ISSUER && vcs[0].credentialSubject.id) {
      const did = vcs[0].credentialSubject.id;
      const session_id = await redisClient.v4.get('chid:'+authToken)
      await redisClient.v4.del('chid:'+authToken)
      await redisClient.set('user:' + did, JSON.stringify({did: did, name: vcs[0].credentialSubject.name, issuer: vcs[0].issuer.name}))
      console.log(session_id)
      // here: send VC data relevant for the FE back in the response via websocket
      const ws = getSocket(session_id);   
      if(ws) {
        console.log('Socket found for ', session_id)
        ws.send(JSON.stringify({
          error: 0,
          registrationComplete: true,
          did: did,
          email: vcs[0].credentialSubject.Email,
          name: vcs[0].credentialSubject.name,
          role: vcs[0].credentialSubject.Role,
        })) 
      }        
    }
    return res.status(200).json(response.data)
  } catch (err) {
    console.log(err)
    return res.status(403).send('Access denied, invalid VC')
  }
}
```

### Login with a Verifiable Credential

With your browser navigate to `http://<your IP/hostname>:9000/` . The web service loads the login page and shows a QR Code that you scan with the sideos wallet.&#x20;

<figure><img src=".gitbook/assets/Screenshot 2024-04-19 at 14.29.54.png" alt=""><figcaption><p>The Login request QR Code</p></figcaption></figure>

The code that generates the QR Code fetches the data for the request from the sideos API and creates a unique request for a login credential of the type "User". If you have that credential type stored in your wallet the wallet asks you to share the credential with the web service:

<figure><img src=".gitbook/assets/Share Credential.png" alt="" width="375"><figcaption><p>Request to share a Verifiable Credential </p></figcaption></figure>

The code to create a login endpoint to show the QR Code on the frontend and store the session in redis is rather small:

```typescript
app.route('/login')
  .get(async (req: Request,res: Response) => {
    const request = await getLoginCreateReqest()
    if(request.data.challenge && request.data.jwt) {
      await redisClient.set('chjwt:'+request.data.challenge, request.data.jwt)
      await redisClient.set('chid:'+request.data.challenge, req.session.id)
      const url =  process.env.CALLBACK_URL + "/gettoken/" + request.data.challenge
      qr.toDataURL(url, (err, qrcode) => {
        if(err) res.send("Error occured")
        res.render("login",{qrcode: qrcode, callback_url: process.env.CALLBACK_URL});
      })
    }
  });
```

Where the function `getLoginCreateReqest()` to call sideos and ask for a credential request uses the template id and the callback url parameters.&#x20;

```typescript
export const getLoginCreateReqest = async () => {  
    const challenge = uuidv4();   
    try {
      const response = await axios.post(process.env.SSI_SERVER + '/v3/createrequestvc',
      {
        templateid: process.env.LOGIN_TEMPLATE_ID,
        dataset: {},
        domain: process.env.CALLBACK_URL + '/login',
        challenge: challenge
      },
      {
        headers: {
          'Content-Type': 'application/json',
          'X-Token': process.env.ACCESS_TOKEN
        }
      });
      return { 
        data: {
          error: 0,
          challenge: challenge,
          jwt: response.data.data.jwt
        }
      }
    } catch (err) {
      console.log(`Error posting juno at ${process.env.SSI_SERVER}/v3/createrequestvc`);
      console.log(err);
      return {
        data: {
          error: `Error posting juno at ${process.env.SSI_SERVER}/v3/createrequestvc`
        }
      }  
    }
  }
```

The response from the wallet is send to the callback url which was embedded into the credential request. The function behind the endpoint is `postLoginConsumeReqest()`. This piece of code does the whole work of the user authentication. It checks if the credential is valid by sending it to the validation endpoint of the sideos API, checks if the issuer of the credential can be trusted - we set this part in the .env file as `DID_ISSUER` - and if there is a subject in the credential which holds the data elements we want to use in the web service, eventually.&#x20;

```typescript
export const postLoginConsumeReqest = async (req: Request, res: Response, next: NextFunction) => {
  const redisClient = createClient({ legacyMode: true })
  await redisClient.connect().catch(console.error)
  
  try {
    const jwt = req.body.jwt;
    let authToken: string = req.params.token;
    if (!authToken && req.query.challenge) {
      authToken = <string>req.query.challenge
    }
    console.log('Consume Request challenge parameter:', authToken)
    let vcs: Array<any> = [];
    const response = await axios.post(process.env.SSI_SERVER + '/v3/consumerequest', {
      jwt: jwt
    }, {
      headers: {
        'Content-Type': 'application/json',
        'X-Token': process.env.ACCESS_TOKEN
      }
    })
    response.data.data.payload.verifiableCredential.forEach((element: any) => {
      vcs.push(element)
    });
    console.log('Trusted DID_ISSUER: ', process.env.DID_ISSUER);
    console.log('email: ', vcs[0].credentialSubject.Email);
    console.log('Name: ', vcs[0].credentialSubject.name);
    console.log('did: ', vcs[0].credentialSubject.id);
    console.log('issuer_did: ', vcs[0].issuer.id);

    if(vcs[0].issuer.id === process.env.DID_ISSUER && vcs[0].credentialSubject.id) {
      const did = vcs[0].credentialSubject.id;
      const session_id = await redisClient.v4.get('chid:'+authToken)
      await redisClient.v4.del('chid:'+authToken)
      await redisClient.set('vc:'+session_id, JSON.stringify({did: did, name: vcs[0].credentialSubject.name, issuer: vcs[0].issuer.name}))

      // here: send VC data relevant for the FE back in the response via websocket
      const ws = getSocket(session_id);   
      if(ws) {
        console.log('Socket found for ', session_id)
        ws.send(JSON.stringify({
          error: 0,
          authToken: authToken,
          did: did,
          email: vcs[0].credentialSubject.Email,
          name: vcs[0].credentialSubject.name,
          role: vcs[0].credentialSubject.Role,
        })) 
      }        
    }
    return res.status(200).json({
      data: { 
        error: 0, 
        payload: vcs
      }
    })
  } catch (err) {
    return res.status(403).send('Access denied, invalid VC')
  }
}
```

If no error occurs, the web service provides an access token to the browser which then loads the home page for an authenticated user.&#x20;

<figure><img src=".gitbook/assets/Screenshot 2024-04-19 at 19.28.23.png" alt=""><figcaption><p>After sucessful authentication the user is logged in</p></figcaption></figure>


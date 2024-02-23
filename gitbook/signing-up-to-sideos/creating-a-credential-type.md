---
description: Credential Types are templates for issuing or verifying credentials
---

# Creating a Credential Type

## Background

A credential holds one or multiple claims. A claim can be a name or an email address. If the claim is wrapped by a digital signature of a trustworthy issuer the user can provider a list of claims to some service provider to identify and authenticate for some service.

For example can a credential type contain the fields _Name_ and _Email Address_. This credential type can then be used to create verifiable credentials for user who use the credentials for password-less logins. In the login process the user shows the credential to the service provider who checks the data, checks the issuer and the issuer's signature, and finally grants access to the service because data is valid and the issuer can be trusted.&#x20;

In API calls the credential type is referred by its ID. So, to create a verifiable credential the client application of the issuer sends a data record that contains the credential ID (\`templateid\`) to refer to the respective credential type, and the data fields containing the actual data to the API to request a creation of the respective verifiable credential:&#x20;

```json
{
  templateid: 18,
  data: {
    name: "Rheik van Eyck",
    email: "rveyck@example.com"
  }
}
```

## Creating a Claim Type

Login into the sideos administration console. If you don't have already an account, sign up with sideos (see [.](./ "mention")).

<figure><img src="../.gitbook/assets/Screen Shot 2024-02-23 at 18.58.57 PM.png" alt=""><figcaption></figcaption></figure>

In the navigation bar go to _Claims_ and click on _Add new claim._

If a claim fitting the requirements is not available in the list, click on _create new claim_ in the description above the table.

Complete the form by naming the new credential type in the field **Name.** Chose a **Type** from existing types you probably already defined. In our case the type email is probably not available, so we select _Other_ from the bottom of the selection list.

Give the new type a name, e.g. _name_.&#x20;

Select the **Context** for the new type. The options include DataFeedItem, Boolean, Number, and Image. For the name type which represents a string, the DataFeedItem is the right option.&#x20;

Optional you can provide a description in the **Description** text box.

<figure><img src="../.gitbook/assets/Screen Shot 2024-02-23 at 19.08.55 PM.png" alt=""><figcaption><p>Creating a new claim type</p></figcaption></figure>

Save the claim by clicking on the _Save_ button.&#x20;

To create another claim type for the **email claim**, repeat the steps to create the email claim type. &#x20;

Once we have the claim types for the name and email for our example we can create a credential type.

## Creating a Credential Type

A credential type combines one or more claim type to build an identity context. A company badge for an employee for example may contain the name of the employee, the employee id number, the department, the role, the user id, contact data, emergency contacts, and permission sets as claims.&#x20;

In the navigation bar go to _Credentials_ and click on _Create template._ There may be already credentials in a company repository available. You can check by clinking on the button _Pick from repository._&#x20;

Lets assume you want to create a new credential type, so go ahead and click on the button C_reate template_.

Complete the form by choosing an existing or entering a new type in the field **Credential type.**&#x20;

Give the new type a name, e.g. _User_ in the field **Name**.&#x20;

Add claim types to the credential type by selecting proofs in the selection field **Claim 1**. In our example we would select the **Name** claim type. Then we add another claim type by clicking on the (+) button and select the **Email** claim type as **Claim 2.**&#x20;

Optional you can provide a description in the **Description** text box.

<figure><img src="../.gitbook/assets/Screen Shot 2024-02-23 at 19.25.56 PM.png" alt=""><figcaption><p>Creating a new credential type</p></figcaption></figure>

You can save the new credential type as draft by clicking on the button _Save as draft_ or save as completed by clicking on the button _Save as complete_.&#x20;

Back in the list of defined credentials note the **ID** of your new credential type. In the Screenshot below the _User_ credential type has ID 18.&#x20;

<figure><img src="../.gitbook/assets/Screen Shot 2024-02-23 at 19.29.20 PM.png" alt=""><figcaption><p>List of available credential type in the account.</p></figcaption></figure>

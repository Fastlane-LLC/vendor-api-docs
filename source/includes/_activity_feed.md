# Activities

LossExpress provides a simple way to view events that have taken place within our system, relevant to an organization’s claims. We currently allow for two different methods for keeping up with the activities that can occur on a claim within LossExpress: fetching via our Activity Feed route and directly receiving via our supported webhook.

As a general rule, all Activity objects include the following keys:

Key | Description
--- | -----------
createdAt | GMT timestamp marking when the Activity occured in our system
claimId | The LossExpress UUID given to the claim the Activity occurred on
type | A string containing one of the available Activity Types
claimNumber | The claim number entered for the claim the Activity occurred on
data | An object containing relevant information for the Activity
activityId | A GUID uniquely identifying the activity

## Fetch Activities (Activity Feed)
> This route returns a paginated set of results that looks like this:

```json
{
  "requestUrl": "https://xapi.lossexpress.com/activities?createdBefore=2021-01-08T22:03:09.598Z",
  "results": [
    ...array of activity objects
  ],
  "pagination": {
    "pageSize": 100,
    "page": 0,
    "totalPages": 3,
    "totalResults": 296
  }
}
```
> Note that the result will return a `requestUrl` object, which will allow you to perfectly replay the request.

Our Activity Feed provides access to all scoped activities while allowing for a large variety of filtering options to suit any organization’s needs. One of our core tenants at LossExpress is transparency, so you may find that we provide access to _too many_ activities. Feel free to use only what’s important to your organization!

This route will allow you to view Activities.

### HTTP Request

`GET https://xapi.lossexpress.com/activities`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
types | all | A comma separated list of types to view
createdBefore | current time | Allows for filtering activities to only those that were created before a certain timestamp
createdAfter | 30 minutes earlier | Allows for filtering activities to only those that were created after a certain timestamp
pageSize | 100 | Sets the size of pages in paginated results. Maximum is currently 500.
pageNumber | 0 | Sets the zero-indexed page number in paginated results.
claimId | | Filters the activities to only contain activities for the specific LossExpress Claim ID.
activityId | | Show activity types starting from the activity and moving into the future.

### Note

You can only supply `activityId` or `createdBefore` in a request, not both.

## Trigger Activities (Testing)

This route enables you to trigger a variety of activities that would typically be triggered by actions taken by our claim fulfillment specialists.

### HTTP Request

`POST https://xapi.lossexpress.com/activities/trigger-activity/{claimId}?activityType={activityType}`

### Query Parameters

Parameter | Description
--------- | ------- | -----------
activityType | Must be one of: 'direct-message-added', 'document-sent-to-lender', 'letter-of-guarantee-added', 'payoff-data-added', 'claim-updated', 'call-made', 'document-added', 'settlement-counter-added', 'payoff-request-cancelled' |

# Activity Feed Webhook
If you wish to be notified when activities occur for your claims, we offer real-time notifications via a [webhook](https://sendgrid.com/blog/whats-webhook/). After registering your URL, it will start receiving POST requests whenever activities get generated in our system.

<aside class="notice">
We will only ever send one activity per webhook request. Webhook payloads for any given activities will match the activities' data structures as defined in the <a href=https://vendor-docs.lossexpress.com/#activity-types>Activity Types</a> section.
</aside>

### Validating Webhook Payloads

> Example HMAC header validation

```js
const crypto = require('crypto');

const apiKey = 'your API key';
const clientId = 'your client ID';

const hmacHeader = request.headers["X-LossExpress-Signature"];
const body = request.payload;

const message = `${JSON.stringify(body)}:${clientId}`;

const generatedHmac = crypto.createHmac('sha256', apiKey)
  .update(message)
  .digest('hex');
  
if (generatedHmac === hmacHeader) {
  // Webhook is valid!
}
```

To help you verify that webhook requests are valid and come from us, we provide the following:

- A `X-LossExpress-Signature` header on every request, which contains an HMAC hash for a string with the structure: `{{ webhook payload }}:{{ clientId }}`
- An API key that can be used, along with the request payload and your xAPI client ID, to recreate the HMAC

By recreating the HMAC hash and comparing it with the request's `X-LossExpress-Signature` header, you can determine whether the request was generated using the secret API key. As long as you keep the API key secret, matching HMACs indicate that the request is valid and comes from our system.

## Register Webhook

This endpoint can be used to set your webhook's URL. A test payload (a [direct-message-added](https://vendor-docs.lossexpress.com/#direct-message-added) activity) will be sent to the provided URL, and a response with a 200 status code is expected.

This endpoint also generates an API key secret that can be used to validate incoming payloads. A new API key is generated whenever this endpoint is used, and it can be retrieved anytime via the [Fetch Webhook Information](https://vendor-docs.lossexpress.com/#fetch-webhook-information) endpoint.

### HTTP Request
`POST https://xapi.lossexpress.com/carriers/webhook`

Key | Description
--- | -----------
endpoint | The URL that will receive your activity feed data.

> If your endpoint returns a status code of 200, this route returns a success object that looks like this:

```json
{
  "success": true,
  "statusCode": 200,
}
```
> Note that the result will return a `success: false` object, if your endpoint does not yield a 200.

## Fetch Webhook Information

For fetching information about your webhook status, current endpoint, and API key.

### HTTP Request

`GET https://xapi.lossexpress.com/carriers/webhook-info`

>This route returns an object with your webhook information that looks like this:

```json
{
  "success": true,
  "webhookInfo": {
    "endpoint": "https://yourendpointthatreturnsa200.com",
    "verified": true,
    "apiKey": "exampleapikey1234"
  }
}
```

## Deactivate Webhook

For deactivating your current webhook, this will not delete your entry and you can turn your webhook back on by using the POST route in the 'Register Webhook' section.

### HTTP Request

`PUT https://xapi.lossexpress.com/carriers/deactivate-webhook`

# Activity Types

## List of Types

Below is a list of Activity Types that will be available within the Activity Feed:

Type | Will appear when... | Triggered by
---- | ------------------- | ------------
account-number-viewed | Account number is viewed | LossExpress
call-made | A call is made by LossExpress | LossExpress
claim-created | A claim is originally created | Carrier
claim-updated | A claim's primary information is updated | Carrier/LossExpress
direct-message-added | A direct message is added to a claim | Carrier/LossExpress
document-added | A document is added to a claim | Carrier/LossExpress
document-sent-to-lender | A document is sent to a lender | LossExpress
letter-of-guarantee-added | A letter of guarantee is added to a claim | LossExpress 
letter-of-guarantee-request-created | A letter of guarantee request is created on a claim | Carrier
letter-of-guarantee-request-cancelled | A letter of guarantee request on a claim has been cancelled | Carrier/LossExpress
lender-alias-verified | A lender has been verified as an alias of an existing lender | LossExpress
new-lender-verified | A lender has been verified as a new lender to our system | LossExpress
order-attempted | An attempt to fulfill an order has been added to claim | LossExpress
order-created | A new order has been added to a claim | Carrier
order-cancelled | An order on a claim has been cancelled | Carrier/LossExpress
order-fulfilled | An order on a claim has been successfully fulfilled | LossExpress
order-updated | An order on a claim has been updated | LossExpress
payment-sent | Payment has been sent for a claim | Carrier
payoff-data-added | Payoff information is added to a claim | LossExpress
payoff-request-created | A payoff request is created on a claim | Carrier
payoff-request-cancelled | A payoff request on a claim has been cancelled | Carrier/LossExpress
settlement-counter-added | A lender adds a counter to the proposed settlement amount | LossExpress
settlement-counter-updated | The settlement counter is either accepted or disputed by the carrier | Carrier

## account-number-viewed

> account-number-viewed example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "account-number-viewed",
  "claimNumber": "EXAMPLE1",
  "externalId": "COO-30022",
  "data": {
    "viewedByLossExpress": true
  }
}
```

This activity type will appear in the feed whenever an account number is viewed by someone on LossExpress.


## call-made

> call-made example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "call-made",
  "claimNumber": "EXAMPLE2",
  "externalId": "COO-30022",
  "data": {
    "callLength": 30 // in minutes
  }
}
```

This activity type will appear in the feed whenever a call was made on behalf of the carrier by LossExpress.

## claim-created

> claim-created example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "claim-created",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "accountNumber": "TEST-AN",
    "adjusterName": "Mike Mclaren",
    "adjusterEmailAddress": "mike@lossexpress.com",
    "adjusterPhoneNumber": "+13332225555",
    "causeOfLoss": "Fire",
    "dateOfLoss": "2020-10-20",
    "deductible": 400,
    "financeType": "Retail",
    "insurerType": "First Party",
    "lenderName": "Test Lender",
    "lenderId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
    "odometer": 39993,
    "ownersName": "Test Owner",
    "ownersPhoneNumber": "+12223334444",
    "ownerRetained": false,
    "ownersStreetAddress": "12200 Test Avenue Dallas TX 75204",
    "settlementAmount": 5553,
    "titleRemittanceAddress": "1000 Main Street Dallas TX 75204",
    "vehicle": {
      "make": "TEST",
      "model": "Car",
      "year": 2034
    },
    "vehicleLocation": "1200 Main Street Dallas TX 75204",
    "vin": "1N4AL3AP8JC231503"
  }
}
```

This activity type will appear in the feed whenever a claim is created in our system by a carrier. This activity will contain in its data all claim information that had
been passed on create along with Make/Model/Year information for the VIN passed, if available. Any information not sent on creation will not appear in the data object and
would appear as <code>undefined</code> when parsed.

If desired, we do give carriers the ability to not have account numbers added to this event type for security purposes.

<aside class="notice">For more information regarding the formats/types passed in the <code>data</code> object, please see the Create Claim route.</aside>

## claim-updated

> claim-updated example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "claim-updated",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "update": { // contains the updated information
      "accountNumber": "TEST-AN1"
    },
    "previous": { // contains the information just prior to update
      "accountNumber": "TEST-AN"
    },
    "updatedByLossExpress": true
  }
}
```

This activity type will appear in the feed whenever _primary claim information_ is updated. In LossExpress, we define primary claim information as any piece of data that can be sent in the update or create claim routes.

Note that this activity type could be triggered by actions made by LossExpress. Some examples that could cause LossExpress to trigger this activity type:

- Account number was empty or incorrect, so was updated when talking to the lender.
- Finance type was set to Retail but was actually a Lease.
- Lender name was incorrect or updated.

<aside class="warning">This activity type will not fire for any claims that have had no activity for two weeks.</aside>

## direct-message-added

> direct-message-added example object (sent by LossExpress)

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "direct-message-added",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "sentByLossExpress": true,
    "message": "A photo of the odometer reading is required.",
    "category": "ADDITIONAL INFORMATION REQUIRED (NON-DISPUTE)",
    "actionRequired": true
  }
```

> direct-message-added example object (sent by carrier user)

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "direct-message-added",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "sentByLossExpress": false,
    "message": "The LoG looks to have the wrong settlement amount, please get an LoG with the proper amount. Thanks!",
    "sentBy": "User Name",
    "actionRequired": false
  }
}
```

This activity type will appear in the feed whenever a direct message is added to a claim, either by LossExpress or by a carrier user.

When a direct message is sent from LossExpress, you can expect to see a `category` value passed in the activity data. The following values are currently supported:

| Category |
| -------- |
| TITLE |
| ADDITIONAL INFORMATION REQUIRED (NON-DISPUTE) |
| SETTLEMENT DISPUTE |
| OWNER RETAINED SETTLEMENT |
| BANKRUPTCY |
| CUSTOMER AUTHORIZATION |
| GENERAL |
| OTHER |

## document-added

> document-added example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "document-added",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "type": "settlement breakdown",
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f",
    "sentByLossExpress": false,
    "sentBy": "John Doe"
  }
}
```

This activity type will appear in the feed whenever a document has been added to a claim.

Note that when <code>sentByLossexpress</code> is true, this indicates that the document was attached to the claim by LossExpress, not a carrier user, and <code>sentBy</code> will be null.

<aside class="warning">This activity will NOT be triggered when the documents that are added fulfill an existing order on the claim. In that case - an order-fulfilled activity will be triggered.
Documents that will not trigger this activity:
<br/>
- Loan Payoff Document
<br/>
- Letter of Guarantee
<br/>
- Copy of Title
<br/>
- Lien Release Letter
<br/>
- Installment Contract
<br/>
- Bill of Sale
<br/>
- Payment History
<br/>
- Repo Affidavit
<br/>
- One and the Same Letter
<br/>
- Settlement Dispute (Counter-Offer)
</aside>

## document-sent-to-lender

> document-sent-to-lender example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "document-sent-to-lender",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f"
  }
}
```

This activity type is added to the feed whenever a document is sent by LossExpress to the lender for a particular claim. The <code>documentUrl</code> contains a link to the document that was sent to the lender.

## letter-of-guarantee-added

> letter-of-guarantee-added example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "letter-of-guarantee-added",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f"
  }
}
```

This activity type is added to the feed whenever a letter of guarantee is added to a claim. The <code>documentUrl</code> contains a link to the letter of guarantee that was received.

<aside class="warning">This activity could be added to the feed even if a letter of guarantee request wasn't created in LossExpress. In those scenarios, a letter of guarantee request will be automatically created at the same time as the letter of guarantee is added to the claim.</aside>

<aside class="warning">This activity has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#order-status-changed">order-status-changed</a> activity.
  
Although we plan to maintain this legacy activity type, the newer activity is recommended as it provides a more useful and consistent data structure for all types of orders that we support.</aside>

## letter-of-guarantee-request-created

> letter-of-guarantee-request-created example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "letter-of-guarantee-request-created",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "estimatedResponseTime": "2021-01-20T22:03:09.598Z"
  }
}
```

This activity type is added to the feed whenever a letter of guarantee request is created for a claim.

Note that although we _typically_ request letters of guarantee whenever we reach out to a lender, we do not follow-up on the status of letters of guarantee nor can we absolutely guarantee that a letter of guarantee will be added to a claim unless a letter of guarantee request is created on a claim.

<aside class="warning"><code>estimatedResponseTime</code> may not exist on every activity for this type, and is merely an estimate based on _prior_ letter of guarantee request response times for that lender.</aside>

## letter-of-guarantee-request-cancelled

> letter-of-guarantee-request-cancelled example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "letter-of-guarantee-request-cancelled",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {}
}
```

This activity type is added to the feed whenever a letter of guarantee request is cancelled for a claim.

<aside class="warning">This activity has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#order-status-changed">order-status-changed</a> activity.
  
Although we plan to maintain this legacy activity type, the newer activity is recommended as it provides a more useful and consistent data structure for all types of orders that we support.</aside>

## lender-alias-verified

> lender-alias-verified example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "lender-alias-verified",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "previousLenderId": "4ace4d96-2b7f-425e-a3c7-d162879a792b",
    "lenderId": "a720ef63-e6f5-4fed-bbf8-0edcd6eb8485",
    "lenderName": "EXAMPLE NEW LENDER ALIAS"
  }
}
```

This activity type is added to the feed whenever a lender is verified as an alias for an existing lender.

## new-lender-verified

> new-lender-verified example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "new-lender-verified",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "lenderId": "a720ef63-e6f5-4fed-bbf8-0edcd6eb8485",
    "lenderName": "EXAMPLE NEW LENDER"
  }
}
```

This activity type is added to the feed whenever a lender is verified as a new lender to our system.

## order-attempted

> order-attempted example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "order-attempted",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "type": "Payment History",
    "message": "Lender unable to find requested document",
    "count": 2,
    "orderId": "93f0983d-5702-4bf4-9728-86d425edd7d5"
  }
}
```

This activity type is added to the feed whenever an attempt on an order is made.

Key | Value
---- | -------------------
type | The type of order
message | The note related to the order attempt
count | The current number of attempts on the order
orderId | The specific identification number for the order that was attempted

## order-created

> order-created example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "order-created",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "orderId": "24907dea-2bd2-4547-bae6-8305b4256256",
    "orderType": "Copy of Title"
  }
}
```

This activity type is added to the feed whenever an order is created for a claim.

## order-cancelled

> order-cancelled example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "order-cancelled",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "orderId": "24907dea-2bd2-4547-bae6-8305b4256256"
  }
}
```

This activity type is added to the feed whenever an order is cancelled for a claim.

<aside class="warning">This activity has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#order-status-changed">order-status-changed</a> activity.
  
Although we plan to maintain this legacy activity type, the newer activity is recommended as it provides a more useful and consistent data structure for all types of orders that we support.</aside>

## order-fulfilled

> order-fulfilled example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "order-fulfilled",
  "claimNumber": "EXAMPLE3",
  "data": {
    "orderId": "24907dea-2bd2-4547-bae6-8305b4256256",
    "orderType": "Copy of Title",
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f"
  }
}
```

This activity type is added to the feed whenever an order is fulfilled for a claim. If the order utilizes a document, it can be accessed via the <code>documentUrl</code>.

<aside class="warning">This activity has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#order-status-changed">order-status-changed</a> activity.
  
Although we plan to maintain this legacy activity type, the newer activity is recommended as it provides a more useful and consistent data structure for all types of orders that we support.</aside>

## order-status-changed

> order-status-changed example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "order-status-changed"
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "updatedOrder": {
      "orderId": "8ebc4790-a0cb-40f8-bff7-140b9f0260cc",
      "orderType": "Letter of Guarantee",
      "status": "fulfilled",
      "documentUrl": "https://xapi-dev.lossexpress.com/documents/fb498779-7464-469f-bb3b-a6d647cf7541"
    },
    "orders": [
      {
        "orderId": "bce319ea-11f2-4965-821d-c36bca62b506",
        "orderType": "Copy of Title",
        "status": "pending",
      },
      {
        "orderId": "8ebc4790-a0cb-40f8-bff7-140b9f0260cc",
        "orderType": "Letter of Guarantee",
        "status": "fulfilled",
        "documentUrl": "https://xapi.lossexpress.com/documents/fb498779-7464-469f-bb3b-a6d647cf7541"
      },
      {
        "orderId": "c60fe8f7-653a-47a6-acaa-e23f46a603bb",
        "orderType": "Loan Payoff Request",
        "status": "fulfilled",
        "documentUrl": "https://xapi.lossexpress.com/documents/d48e2ff8-cb79-49fd-ad6c-f5a35e5525cf"
        "fulfillmentData": {
          "payoffAmount": 99999,
          "perDiem": 10,
          "validThroughDate": "2022-04-01T21:28:40.256Z",
          "remittanceInformation": {
            "standard": {
              "attn": "That Person",
              "streetAddress": "77 E No St",
              "city": "No",
              "state": "CO",
              "zipCode": "90000",
              "makeCheckPayableTo": "Some Entity"
            },
            "overnight": {
              "streetAddress": "55 Address Place",
              "city": "City",
              "state": "CA",
              "zipCode": "10011",
              "streetAddress2": "2",
              "makeCheckPayableTo": "Some Entity"
            }
          }
        },
      }
    ]
  },
}

This activity type is added to the feed whenever the status for an order is changed (typically to `fulfilled` or `cancelled`). The actvity data includes an `updatedOrder` object for the order who's status was changed, as well as an `orders` array that includes all of the claim's orders. The `orders` array allows you to compare a claim's order statuses and data as they existed at the moment the activity was generated.

Each order object, whether the `updatedOrder` or those in the `orders` array will have the following structure:

Key | Description | Will be included when...
--- | ----------- | ------------------------
orderId | The LossExpress UUID for the order | Always
orderType | The order's [type](https://vendor-docs.lossexpress.com/#fetch-order-types) | Always
status | One of: `[ 'pending', 'fulfilled', 'cancelled' ]` | Always
documentUrl | A URL that can be used to [fetch a document](https://vendor-docs.lossexpress.com/#fetch-document) attached to an order | A document was attached to the order during fulfillment
fulfillmentData | Data pertaining to the order's fulfillment | Various data was gathered in order to fulfill the order

<aside class="notice">
Currently, `fulfillmentData` will only be included for `Loan Payoff Request` orders, for which `fulfilmentData`'s structure will always be consistent (although we may not gather all data points during fulfillment).

We may add other order types that include it in the future. While the structure of `fulfillmentData` may vary between order types, it will be consistent for each individual order type. We will include the full data structure for any such order in the example JSON to the right, as we have done here for `Loan Payoff Request` orders.
</aside>

## order-updated

> order-updated example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "order-updated",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "orderId": "24907dea-2bd2-4547-bae6-8305b4256256",
    "nextFollowupDate": "2021-08-31"
  }
}
```

This activity type is added to the feed whenever an order is updated for a claim.
## payment-sent

> payment-sent example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "payment-sent",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "checkAmount": 20000,
    "sentAt": "2021-01-08T22:03:09.598Z",
    "sentTo": {
      "streetAddress": "1000 Main Street",
      "streetAddress2": "Ste. 500",
      "city": "Dallas",
      "state": "TX",
      "zipCode": "75204"
    },
    "sentVia": "USPS",
    "trackingNumber": "EXAMPLENUMBER"
  }
}
```

This activity type is added to the feed whenever acknowledgement that payment has been sent has been added to LossExpress; this can be trigged by either the carrier or our payment processing partner, if that partner is enabled in the carrier's system and set-up properly in LossExpress.

<aside class="warning"><code>sentTo</code> and <code>trackingNumber</code> may not exist on every activity for this type.</aside>

## payoff-data-added

> payoff-data-added example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "payoff-data-added",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "payoffAmount": 10222.33,
    "perDiem": 2.3,
    "validThroughDate": "2021-01-18T00:00:00.000Z",
    "remittanceInformation": {
      "standard": {
        "makeCheckPayableTo": "ALLY FINANCIAL",
        "attn": "ATTN LINE",
        "streetAddress": "1000 Main Street",
        "streetAddress2": "Ste. 500",
        "city": "Dallas",
        "state": "TX",
        "zipCode": "75204"
      },
      "overnight": {
        "makeCheckPayableTo": "ALLY FINANCIAL",
        "attn": "ATTN LINE",
        "streetAddress": "1000 Main Street",
        "streetAddress2": "Ste. 500",
        "city": "Dallas",
        "state": "TX",
        "zipCode": "75204"
      }
    },
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f"
  }
}
```

This activity type is added to the feed whenever payoff data is added to a claim.

<aside class="warning">This activity may be called multiple times in the claim and may not contain all information. Only information changed or added will be available in this activity type.</aside>

<aside class="warning">This activity has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#order-status-changed">order-status-changed</a> activity.
  
Although we plan to maintain this legacy activity type, the newer activity is recommended as it provides a more useful and consistent data structure for all types of orders that we support.</aside>

## payoff-request-created

> payoff-request-created example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "payoff-request-created",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {}
}
```

This activity type is added to the feed whenever a payoff request is created on a particular claim.
## payoff-request-cancelled

> payoff-request-cancelled example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "payoff-request-cancelled",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {}
}
```

This activity type is added to the feed whenever a payoff request is cancelled on a particular claim.

<aside class="warning">This activity has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#order-status-changed">order-status-changed</a> activity.
  
Although we plan to maintain this legacy activity type, the newer activity is recommended as it provides a more useful and consistent data structure for all types of orders that we support.</aside>

## settlement-counter-added

> settlement-counter-added example object

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "settlement-counter-added",
  "claimNumber": "EXAMPLE3",
  "data": {
    "description": "missing options",
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f"
  }
}
```

This activity type is added to the feed whenever a settlement counter is added to a claim by a lender. Typically, this comes with a document that can be accessed via the <code>documentUrl</code>.

## settlement-counter-updated

> settlement-counter-updated example object (dispute)

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "settlement-counter-updated",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "disputed": true,
    "reasonForDispute": "Vehicle is not missing any options, verified in person."
  }
}
```

> settlement-counter-updated example object (accepted)

```json
{
  "activityId": "fed62fa0-c048-46b5-b994-6e3e69fb0f37",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "type": "settlement-counter-updated",
  "claimNumber": "EXAMPLE3",
  "externalId": "COO-30022",
  "data": {
    "disputed": false,
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f"
  }
}
```

This activity type is added to the feed whenever a settlement counter is either accepted or disputed by the carrier. <code>documentUrl</code> will contain a link to the settlment breakdown passed to the lender with the new settlement amount.

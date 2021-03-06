# Claims

> Example Claim Object

```json
{
  "accountNumber": "TEST-AN",
  "adjusterName": "Mike Mclaren",
  "adjusterEmailAddress": "mike@lossexpress.com",
  "adjusterPhoneNumber": "+13332225555",
  "causeOfLoss": "Fire",
  "carrierId": "150ae9da-9222-4ca5-43fe-fe1dc650fa0f",
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "claimNumber": "EXAMPLE3",
  "createdAt": "2021-01-08T22:03:09.598Z",
  "dateOfLoss": "2020-10-20",
  "deductible": 400,
  "documents": [
    {
      "createdAt": "2021-01-08T22:03:09.598Z",
      "updatedAt": "2021-01-08T22:03:09.598Z",
      "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f",
      "type": "settlement breakdown & valuation report"
    }
  ],
  "externalId": "COO-30022",
  "financeType": "Retail",
  "insurerType": "First Party",
  "lenderName": "Test Lender",
  "lenderId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "letterOfGuaranteeRequest": {
    "createdAt": "2021-01-08T22:03:09.598Z",
    "updatedAt": "2021-01-08T22:03:09.598Z",
    "documentUrl": "https://xapi.lossexpress.com/documents/555ae9da-9222-4de5-81fe-fe1ac590fa0f",
  },
  "odometer": 39993,
  "ownersName": "Test Owner",
  "ownersPhoneNumber": "+12223334444",
  "ownerRetained": false,
  "ownersStreetAddress": "12200 Test Avenue Dallas TX 75204",
  "payoffData": {
    "createdAt": "2021-01-08T22:03:09.598Z",
    "updatedAt": "2021-01-08T22:03:09.598Z",
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
    }
  },
  "settlementAmount": 5553,
  "titleRemittanceAddress": "1000 Main Street Dallas TX 75204",
  "updatedAt": "2021-01-08T22:03:09.598Z",
  "vehicle": {
    "make": "TEST",
    "model": "Car",
    "year": 2034
  },
  "vehicleLocation": "1200 Main Street Dallas TX 75204",
  "vin": "1N4AL3AP8JC231503"
}
```

The routes in the section are scoped specifically to claims. Our Vendor API provides a couple ways to search and fetch claim information on top of the ability to create claims.

The Claim object itself contains the following keys:

Object Key | Description
---------- | -----------
accountNumber | The customer's account number for the loan associated with the claim
adjusterName | The primary adjuster for the claim
adjusterEmailAddress | The email address associated with the primary adjuster for the claim
adjusterPhoneNumber | The phone number associated with the primary adjuster for the claim
carrierId | The LossExpress UUID associated with the carrier that the claim should be filed under
causeOfLoss | The cause of loss listed on the claim. These causes can be one of the following: "Single-Vehicle Collision", "Multi-Vehicle Collision", "Collision", "Wind/Hail", "Fire", "Flood", "Vandalism", "Theft", "Water", "Other"
claimId | The LossExpress UUID associated with the claim
claimNumber | The claim number as noted by the carrier
createdAt | The timestamp the claim was created in the system
dateOfLoss | The date the loss occurred
deductible | The current deductible for the payoff
documents | An array of Document objects
externalId | A string that can contain any identifier desired; not used by LossExpress, but passed on all claim activities for easier handling. There are no unique checks on this field.
financeType | Either "Retail" or "Lease"
insurerType | Either "First Party" or "Third Party"
lenderName | The lender's name
lenderId | The LossExpress UUID associated with that lender, if that lender exists in our lender database
letterOfGuaranteeRequest | A letter of guarantee request object, containing basic information about the letter of guarantee request
odometer | The mileage on the vehicle associated with the claim
ownersName | The vehicle owner's name
ownersPhoneNumber | The vehicle owner's phone number
ownersRetained | Whether the owner is retaining the vehicle (boolean)
ownersStreetAddress | The full address of the vehicle owner
payoffData | A Payoff Data object
settlementAmount | The settlement amount for the claim
titleRemittanceAddress | The full address that the vehicle title should be sent to
updatedAt | The timestamp the claim was last updated; this can happen in a number of scenarios and is not recommended to be used to track claim changes - please use the activity feed instead
vehicle | A vehicle object containing the make, model, and year of the vehicle, if available
vehicleLocation | The full address where the vehicle is located, if different from the titleRemittanceAddress
vin | The Vehicle Identification Number for the vehicle on the claim

## Fetch Claim

> Example Response:

```json
{
  "claim": ...claim object,
  "requestUrl": "https://xapi.lossexpress.com/claims/c30ae9da-9222-4de5-81fe-fe1ac590fa0f"
}
```

This route allows for access to a claim's data in one location.

### HTTP Request

`GET https://xapi.lossexpress.com/claims/{claimId}`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

## Create Claim

> Create Claim Response:

> `STATUS 201`

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f"
}
```

This route creates a claim in LossExpress, which can be used to generate requests.

### HTTP Request

`POST https://xapi.lossexpress.com/claims`

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description | Required?
-------------- | ----------- | ---------
accountNumber | The customer's account number for the loan associated with the claim | N
adjusterName	| The primary adjuster for the claim | N
adjusterEmailAddress | The email address associated with the primary adjuster for the claim | N
adjusterPhoneNumber | The phone number associated with the primary adjuster for the claim | N
carrierId | The GUID of the carrier associated with the claim | Y
causeOfLoss | The cause of loss listed on the claim. These causes can be one of the following: "Single-Vehicle Collision", "Multi-Vehicle Collision", "Wind/Hail", "Fire", "Flood", "Vandalism", "Theft", "Other" | N
claimNumber | The claim number as noted by the carrier | Y
dateOfLoss | The date the loss occurred | N
deductible | The current deductible for the payoff | N
externalId | A string that can contain any identifier desired; not used by LossExpress, but passed on all claim activities for easier handling. There are no unique checks on this field. | N
financeType | Either "Retail" or "Lease" | Y
insurerType | Either "First Party" or "Third Party" | N
lenderName | The lender's name | Y (if lenderId not passed)
lenderPhoneNumber | The lender's phone number | N
lenderAddress | The lender's address - an object with keys consisting of streetAddress, streetAddress2 (suite #, etc), city, state, zipCode | N
lenderId | The LossExpress ID for the lender | Y (if lenderName not passed)
odometer | The mileage on the vehicle associated with the claim | N
ownersName | The vehicle owner's name | N
ownersPhoneNumber | The vehicle owner's phone number | N
ownersRetained | Whether the owner is retaining the vehicle (boolean) | N
ownersStreetAddress | The full address of the vehicle owner | N
settlementAmount | The settlement amount for the claim | N
titleRemittanceAddress | The full address that the vehicle title should be sent to | N
vehicleLocation | The full address where the vehicle is located, if different from the titleRemittanceAddress | N
vin | The Vehicle Identification Number for the vehicle on the claim | Y

<aside class="warning">
Although many fields are not required when creating a claim, please note that nearly all fields are required when requesting information through LossExpress.
</aside>

## Update Claim

> Update Claim response body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  ...claim object (with updated claim info)
}
```

This route allows for updating a claim. It is recommended that you only pass in data that has been changed, as opposed to a full object payload. This is recommended to reduce the size and complexity of `claim-updated` activities.

### HTTP Request

`PUT https://xapi.lossexpress.com/claims/{claimId}`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description | Required?
-------------- | ----------- | ---------
accountNumber | The customer's account number for the loan associated with the claim | N
adjusterName	| The primary adjuster for the claim | N
adjusterEmailAddress | The email address associated with the primary adjuster for the claim | N
adjusterPhoneNumber | The phone number associated with the primary adjuster for the claim | N
causeOfLoss | The cause of loss listed on the claim. These causes can be one of the following: "Single-Vehicle Collision", "Multi-Vehicle Collision", "Wind/Hail", "Fire", "Flood", "Vandalism", "Theft", "Other" | N
claimNumber | The claim number as noted by the carrier | N
dateOfLoss | The date the loss occurred | N
deductible | The current deductible for the payoff | N
externalId | A string that can contain any identifier desired; not used by LossExpress, but passed on all claim activities for easier handling. There are no unique checks on this field. | N
financeType | Either "Retail" or "Lease" | N
insurerType | Either "First Party" or "Third Party" | N
lenderId | The LossExpress UUID for the lender | N
lenderName | The lender's name | N
odometer | The mileage on the vehicle associated with the claim | N
ownersName | The vehicle owner's name | N
ownersPhoneNumber | The vehicle owner's phone number | N
ownersRetained | Whether the owner is retaining the vehicle (boolean) | N
ownersStreetAddress | The full address of the vehicle owner | N
settlementAmount | The settlement amount for the claim | N
titleRemittanceAddress | The full address that the vehicle title should be sent to | N
vehicleLocation | The full address where the vehicle is located, if different from the titleRemittanceAddress | N
vin | The Vehicle Identification Number for the vehicle on the claim | N

## Search Claims
> Search Claims response body:

Status: `200`

```json
{
  "claims": [
    ...claim information
  ],
}
```

This route allows for searching our claims database for records. This route can return multiple claims in certain scenarios.

### HTTP Request

`GET https://xapi.lossexpress.com/claims`

### Query Parameters

Parameters | Description
---------- | -----------
vin | Vehicle Identification Number to search by
claimNumber | Claim number to search by (case insensitive)

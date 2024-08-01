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
  "titleRemittanceAddress": {
    "streetAddress": "1000 Main Street",
    "streetAddress2": "Suite 400",
    "city": "Dallas",
    "state": "TX",
    "zipCode": "75204"
  },
  "titledOwners": [
    {
      "type": "person",
      "name": "Bob Dobbs",
      "streetAddress": "1234 Slack St.",
      "city": "Dallas",
      "state": "TX",
      "zipCode": "75217"
    }
  ],
  "titledState": "TX",
  "titleTransfer": {
    "isSalvage": true,
    "titleTransferDate": "2021-01-08T22:03:09.598Z",
    "titleNumber": "1234567890",
    "requestedFormat": "DIGITAL",
    "odometerExempt": true,
    "odometerExemptReason": "exceeds mechanical limit",
    "existingFormat": "PAPER",
    "representative": {
      "name": "Murray Bookchin",
      "title": "Adjuster",
      "emailAddress": "bookchin@lossexpress.com",
    },
    "requestedBrand": "FIRE",
    "damages": [ "VANDALISM" ],
  },
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
titledOwners | An array of objects containing data for each titled owner | N
titleRemittanceAddress | The full address that the vehicle title should be sent to | N
titledState | The two-character abbreviation of the state listed on the vehicle's title | N
titleTransfer | An object containing various data required for title transfer orders | N
vehicleLocation | The full address where the vehicle is located, if different from the titleRemittanceAddress | N
vin | The Vehicle Identification Number for the vehicle on the claim | Y

<aside class="warning">
Although many fields are not required when creating a claim, please note that nearly all fields are required when requesting information through LossExpress.
</aside>

<aside class="warning">
To create a title transfer order, you must include the following data: `titledOwners`, `titledState`, `titleTransfer`, `titleRemittanceAddress`, and `odometer` (if `titleTransfer.odometerExempt` is false). If you attempt to create a title transfer order without providing required data, you will receive an error that lists the missing data.
</aside>

### `titledOwners`

The `titledOwners` parameter must be an array of objects with the following structure:

Parameter | Description | Required?
-------------- | ----------- | ---------
type | Must be one of: `'person', 'dealer', 'company', 'trust'` | Y
name | The owner's name as listed on the title | Y
phoneNumber | The owner's phone number | N
streetAddress | The owner's street address | Y
streetAddress2 | Address line 2 (apartment number, etc) | N
city | The owner's city | Y
state | The 2-character abbreviation for the owner's state | Y
zipCode | The 5-character zip code for the owner's address | Y

### `titleTransfer`

The `titleTransfer` object can include the following parameters:

Parameter | Description | Required?
-------------- | ----------- | ---------
damages | An array containing vehicle damages. See possible values below. | Y (if `isSalvage` is true)
existingFormat | The format of the existing title (must be one of: `PAPER, DIGITAL`) | Y
isSalvage | A boolean indicating whether the title transfer is for a salvage | Y
odometerExempt | A boolean indicating whether the vehicle is exempt from mileage disclosure. | Y
odometerExemptReason | The reason for the mileage disclosure exemption. For example, if the mileage exceeds the odometer's mechanical limit or if the odometer has been tampered with. | Y (if `odometerExempt` is true)
odometerReadDate | The date that the odometer value was read | Y (if `odometerExempt` is false)
representative | An object containing information about the representative for the acquiring party | Y
requestedBrand | Branding to be used for the title. See possible values below. | Y (if `isSalvage` is true)
requestedFormat | The format that the new title should be issued as (must be one of: `PAPER, DIGITAL`) | Y
titleNumber | The existing title's number | Y
titleTransferDate | The date that the title transfer should occur | Y

<aside class="notice">
`damages` must be an array that includes at least one of: `BACK_END, FIRE, FLOOD, FRONT_END, FUEL_CONTAMINATION, HAIL, LEFT_SIDE, MECHANICAL, RIGHT_SIDE, TOP_ROOF, UNDERCARRIAGE, VANDALISM, WIND`
</aside>

<aside class="notice">
`requestedBrand` must be one of: `COSMETIC_TOTAL_LOSS, FIRE, FLOOD, SALVAGE, NON_REPAIRABLE`
</aside>

### `titleTransfer.representative`

The `titleTransfer` object must include the following parameters:

Parameter | Description | Required?
-------------- | ----------- | ---------
name | The representative's name | Y
title | The representative's title | Y
emailAddress | The representative's email address | Y

### `titleRemittanceAddress`

While we accept a single string for `titleRemittanceAddress` for most orders, an address object can also be provided. For title transfers in particular, an address object is required.

Parameter | Required?
-------------- | ---------
streetAddress | Y
streetAddress2 | N
city | Y
state | Y
zipCode | Y

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
titledOwners | An array of objects containing data for each titled owner | N
titleRemittanceAddress | The full address that the vehicle title should be sent to | N
titledState | The two-character abbreviation of the state listed on the vehicle's title | N
titleTransfer | An object containing various data required for title transfer orders | N
vehicleLocation | The full address where the vehicle is located, if different from the titleRemittanceAddress | N
vin | The Vehicle Identification Number for the vehicle on the claim | N

<aside class="notice">
Title transfer data structures (`titledOwners` and `titleTransfer`) are the same for this endpoint as they are for claim creation.
</aside>

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

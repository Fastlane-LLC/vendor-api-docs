# Payoff Requests

## Create Payoff Request

> Create Payoff Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f"
}
```

> This route will throw a `400: BAD REQUEST` if the payoff request fails due to missing information on the claim.

> Create Payoff Request Example Error Response:

```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "Claim is missing one or more required documents: 'settlement breakdown' & 'valuation report'"
}
```

This route will create a payoff request on a claim. The request will fail when:

- IF documents with type `settlement breakdown` and `valuation report` are not available (or a document with type `settlement breakdown & valuation report`)

### HTTP Request

`POST https://xapi.lossexpress.com/claims/{claimId}/payoff-request`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

## Cancel Payoff Request

> Cancel Payoff Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "success": true
}
```

> This route will throw a `404: NOT FOUND` if a Payoff request is not available on the claim.

> Cancel Payoff Request Example Error Response:

```json
{
  "statusCode": 404,
  "error": "Not Found",
  "message": "A payoff request does not exist for this claim."
}
```

This route will cancel a payoff request on a claim. This route will return an error if a payoff request has not been added on a claim yet.

This route will not return an error if a payoff request has been previously cancelled or completed.

### HTTP Request

`DELETE https://xapi.lossexpress.com/claims/{claimId}/payoff-request`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

## Refresh Payoff Request

> Refresh Payoff Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f"
}
```

> This route will throw a `400: BAD REQUEST` if the payoff request fails due to missing information on the claim.

> This route will throw a `404: NOT FOUND` if a payoff request is not available on the claim.


> Refresh Payoff Request Example Error Responses:


```json
{
  "statusCode": 404,
  "error": "Not Found",
  "message": "A payoff request does not exist for this claim."
}
```

```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "Claim is missing one or more required documents: 'settlement breakdown' & 'valuation report'"
}
```

This route will refresh a payoff request on a claim, notifying our system that vital information has changed and additional work may be required. The request will fail when:

- IF documents with type `settlement breakdown` and `valuation report` are not available (or a document with type `settlement breakdown & valuation report`)
- IF a payoff request does not exist on the claim

### HTTP Request

`POST https://xapi.lossexpress.com/claims/{claimId}/payoff-request/refresh`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

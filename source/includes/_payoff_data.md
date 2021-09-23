# Payoff Requests

## Create Payoff Request

> Create Payoff Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f"
}
```

> This route will throw a `400: BAD REQUEST` if the payoff request fails due to missing information on the claim.

This route will create a payoff request on a claim. The request will fail when:

- IF documents with type `settlement breakdown` and `valuation report` are not available (or a document with type `settlement breakdown & valuation report`)

### HTTP Request

`POST https://exapi.lossexpress.com/claims/{claimId}/payoff-request`

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

This route will cancel a payoff request on a claim. This route will return an error if a payoff request has not been added on a claim yet.

This route will not return an error if a payoff request has been previously cancelled or completed.

### HTTP Request

`DELETE https://exapi.lossexpress.com/claims/{claimId}/payoff-request`

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

<aside class="notice">
This route is not currently available, but will be available in a future release of LossExpress xAPI.
</aside>

This route will refresh a payoff request on a claim, notifying our system that vital information has changed and additional work may be required. The request will fail when:

- IF documents with type `settlement breakdown` and `valuation report` are not available (or a document with type `settlement breakdown & valuation report`)

### HTTP Request

`POST https://exapi.lossexpress.com/claims/{claimId}/payoff-request/refresh`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

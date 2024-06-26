# Letter of Guarantee Requests

## Create Letter of Guarantee Request

> Create Letter of Guarantee Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "turnAroundTimeEstimate": "2021-01-08T22:03:09.598Z"
}
```

> This route will throw a `400: BAD REQUEST` if it fails due to lack of information on a claim.

> Create Letter of Guarantee Request Example Error Response:

```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "Claim is missing one or more required documents: 'settlement breakdown' & 'valuation report'"
}
```

This route will create a letter of guarantee request on a claim. If the customer has manual xLOG requests enabled, you must specify TRUE on the xLogRequested variable for an xLOG to be processed. Null or False will not return an xLOG. Customers without this configuration can ignore this. The letter of guarantee request can be added at any time, but will fail when:

- IF documents with type `settlement breakdown` and `valuation report` are not available (or a document with type `settlement breakdown & valuation report`)

### HTTP Request

`POST https://xapi.lossexpress.com/claims/{claimId}/letter-of-guarantee-request`

### When an xLOG is specifically requested:

`POST https://xapi.lossexpress.com/claims/{claimId}/letter-of-guarantee-request?xLogRequested=true`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

<aside class="warning">"turnAroundTimeEstimate" is currently not supported and will return a NULL value for all Letter of Guarantee requests.</aside>

## Cancel Letter of Guarantee Request

> Cancel Letter of Guarantee Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "success": true
}
```

> This route will throw a `404: NOT FOUND` if a Letter of Guarantee request is not available on the claim.

> Cancel Letter of Guarantee Request Example Error Response:

```json
{
  "statusCode": 404,
  "error": "Not Found",
  "message": "Letter of Guarantee does not exist for this claim."
}
```

This route will cancel a letter of guarantee request on a claim. This route will return an error if a letter of guarantee request has not been added on a claim yet.

This route will not return an error if a letter of guarantee request has been previously cancelled or completed.

### HTTP Request

`DELETE https://xapi.lossexpress.com/claims/{claimId}/letter-of-guarantee-request`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

## Refresh Letter of Guarantee Request

> Refresh Letter of Guarantee Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "turnAroundTimeEstimate": "2021-01-08T22:03:09.598Z"
}
```

> This route will throw a `400: BAD REQUEST` if it fails due to lack of information on a claim.

> This route will throw a `404: NOT FOUND` if a Letter of Guarantee request is not available on the claim.


> Refresh Letter of Guarantee Request Example Error Responses:

 ```json
{
  "statusCode": 404,
  "error": "Not Found",
  "message": "Letter of Guarantee does not exist for this claim."
}
```


```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "Claim is missing one or more required documents: 'settlement breakdown' & 'valuation report'"
}
```


This route will refresh an existing letter of guarantee request on a claim, notifying our system that vital information has changed and additional work may be required. The letter of guarantee request can be added at any time, but will fail when:

- IF documents with type `settlement breakdown` and `valuation report` are not available (or a document with type `settlement breakdown & valuation report`)
- IF a letter of guarantee request does not exist on a claim

### HTTP Request

`POST https://xapi.lossexpress.com/claims/{claimId}/letter-of-guarantee-request/refresh`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

<aside class="warning">
  This end-point has been deprecated in favor of the newer <a href="https://vendor-docs.lossexpress.com/#refresh-order-s-request">Refresh Order(s) Request</a> end-point.
<br><br>
Although we plan to maintain this legacy end-point, the newer end-point is recommended as it provides a more useful and consistent data structure for all types of orders that we support.
</aside>
